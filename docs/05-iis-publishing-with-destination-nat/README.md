# IIS Publishing with Destination NAT

This chapter documents internal IIS publishing through a FortiGate Virtual IP and an inbound firewall policy. It separates local web-server validation from external access through destination NAT.

---

## Purpose

Publish an internal IIS service through destination NAT while separating local service validation from external firewall-path validation.

## Technical Context

The internal IIS portal is published through a FortiGate Virtual IP. In this design, the VIP performs **destination NAT**: traffic sent to the external address is translated and forwarded to the private address of the IIS server. The Virtual IP defines the address translation, while a separate WAN-to-LAN firewall policy decides whether the inbound service is allowed.

The server is tested locally before the firewall objects are created. This separates application troubleshooting from firewall troubleshooting: if IIS does not respond on the server itself, changing the VIP or policy cannot make the website work externally.

> A FortiGate VIP is not automatically a load balancer. This exercise maps an external address to one internal server through destination NAT. FortiGate virtual-server load balancing is a separate configuration used when traffic must be distributed across multiple backend servers.

**Implemented controls:**

- Installed and locally validated the IIS web service.
- Created a Virtual IP mapping from the WAN address to AtlasAD.
- Created an inbound firewall policy referencing the VIP.
- Validated the IIS portal through the FortiGate external address.
- Documented service minimization and production role-separation requirements.

Production recommendation: IIS and Active Directory are placed on the same server only because this is an isolated lab. A public-facing web service should normally be separated from a domain controller and protected with minimal services, logging, TLS hardening, IPS, and reverse-proxy or WAF controls where appropriate.

## Key Technical Terms

| Term | Meaning in this chapter |
|------|-------------------------|
| Virtual IP | A FortiGate object that maps an external address to an internal server address. |
| Destination NAT | Translation that changes the destination of inbound traffic so it reaches a private server. |
| Loopback test | A local test to confirm the web service works before testing the firewall path. |
| Inbound policy | The firewall rule that allows traffic from WAN toward the internal published service. |

## Steps Covered

| Step | Description |
|------|-------------|
| Install IIS on the Atlas server | The Web Server (IIS) role is installed on AtlasAD to provide an internal website for the publishing exercise. |
| Understand and use the loopback address | The default IIS page is opened through 127.0.0.1 on the Atlas server. |
| Create the Virtual IP object | The Virtual IP object maps the FortiGate WAN address to the private Atlas server address. |
| Permit inbound web traffic | A WAN to LAN policy uses the website VIP as its destination and allows the configured Web Access service group. |
| Validate the published portal externally | The IIS page is opened from outside the server by using the FortiGate external address. |

---

## Detailed Walkthrough

### Step 01 - Install IIS on the Atlas server

The Web Server (IIS) role is installed on AtlasAD to provide an internal website for the publishing exercise. Installing the role creates the web-service components that will listen for HTTP requests before FortiGate redirects external traffic to the server.

> Combining IIS and Active Directory is acceptable for an isolated lab with limited resources, but the roles should be separated in production. A public-facing web-service vulnerability should not place domain-controller credentials, directory data, and authentication services on the same host at risk.

![IIS role selection](../../images/06-virtual-ip-and-iis-portal-publishing/01.png)

<p><sub><strong>Screenshot 042 - IIS Role Installation:</strong> The Web Server role is selected on the internal Atlas Windows server.</sub></p>


---

### Step 02 - Understand and use the loopback address

The default IIS page is opened through `127.0.0.1` on the Atlas server. The IPv4 loopback address sends the request back to the same host without using the physical LAN, so a successful page proves that the IIS service is installed, listening, and able to return content locally.

> Loopback testing isolates the application from the network path. It is a necessary first check, but it does not validate the server's LAN address, the FortiGate destination translation, the inbound policy, or external reachability.

![Local IIS validation](../../images/06-virtual-ip-and-iis-portal-publishing/02.png)

<p><sub><strong>Screenshot 043 - Local IIS Validation:</strong> The IIS default page responds on the server's loopback address.</sub></p>


---

### Step 03 - Create the Virtual IP object

The Virtual IP object maps the FortiGate WAN address to the private Atlas server address. When inbound traffic targets the external address, FortiGate can replace the packet's destination with the mapped internal address and forward it toward the LAN server.

> A VIP defines how the destination address should be translated; it does not independently permit traffic. FortiGate still requires a matching firewall policy so the administrator can control source, interface, schedule, and service.

![FortiGate Virtual IP](../../images/06-virtual-ip-and-iis-portal-publishing/03.png)

<p><sub><strong>Screenshot 044 - Destination NAT Object:</strong> The Virtual IP maps external traffic to the internal IIS server.</sub></p>


---

### Step 04 - Permit inbound web traffic

A WAN-to-LAN policy uses the website VIP as its destination and allows the configured Web Access service group. This rule provides the missing authorization layer between the address translation and the internal IIS server, allowing inbound traffic to traverse `Port1` and reach the LAN.

> The laboratory service group includes HTTP, HTTPS, and DNS, but DNS is not required merely to publish IIS. A production rule should permit only the protocols actually hosted by the server, normally HTTP and/or HTTPS, and should restrict source addresses where practical.

![Inbound IIS policy](../../images/06-virtual-ip-and-iis-portal-publishing/04.png)

<p><sub><strong>Screenshot 045 - Inbound Web Policy:</strong> The WAN-to-LAN rule references the website VIP and permits the configured web-access services.</sub></p>


---

### Step 05 - Validate the published portal externally

The IIS page is opened from outside the server by using the FortiGate external address. The successful response confirms the complete publishing path: the request reaches the WAN interface, matches the inbound policy, is translated by the VIP, arrives at IIS, and returns through the firewall to the client.

> External validation is what proves the firewall publishing configuration. The earlier loopback result verified IIS locally; this result adds the network interfaces, destination NAT, policy evaluation, routing, and return path.

![External IIS portal](../../images/06-virtual-ip-and-iis-portal-publishing/05.png)

<p><sub><strong>Screenshot 046 - External IIS Access:</strong> The internal IIS portal is reachable through the FortiGate WAN address and destination NAT configuration.</sub></p>


---

## Validation

Validation is layered: IIS responds locally through loopback first, then the same portal responds through the FortiGate WAN address. This separates application readiness from the destination NAT and inbound firewall-policy path.

## Chapter Summary

This chapter completes the internal-service publishing workflow. The evidence shows IIS working locally first, then reachable through the FortiGate external address after the Virtual IP and WAN-to-LAN policy are applied.

---

## Project Chapters

| Chapter | Description |
|---------|-------------|
| [Project Overview](../../README.md) | Main project overview, network topology, scope, and outcomes |
| [Administrator Access and Role-Based Control](../01-administrator-access-and-rbac/README.md) | Named administrator accounts, custom admin profiles, and least-privilege validation |
| [FortiGate Password Policy Hardening](../02-password-policy-hardening/README.md) | Local password complexity controls for administrators and IPsec pre-shared keys |
| [LDAP Integration and SSL VPN Tunnel Mode](../03-ldap-and-ssl-vpn-tunnel-mode/README.md) | Active Directory LDAP integration, SSL VPN Tunnel Mode, FortiClient access, and restricted RDP validation |
| [SSL VPN Web Mode](../04-ssl-vpn-web-mode/README.md) | Browser-based SSL VPN access with LDAP group mapping and an RDP bookmark |
| [IIS Publishing with Destination NAT](../05-iis-publishing-with-destination-nat/README.md) | Internal IIS publishing through a FortiGate Virtual IP and inbound firewall policy |
| [Site-to-Site IPsec VPN](../06-site-to-site-ipsec-vpn/README.md) | FortiGate-to-FortiGate IPsec connectivity with directional service restrictions |
| [Full SSL/TLS Inspection](../07-full-ssl-tls-inspection/README.md) | Full SSL/TLS inspection profile creation and outbound policy attachment |
| [Web Filtering](../08-web-filtering/README.md) | Static URL filtering, category authentication, client testing, and FortiGate web-filter logs |
| [DNS Filtering](../09-dns-filtering/README.md) | DNS filter profile configuration, controlled domain blocking, and DNS-filter log review |
| [Antivirus Inspection](../10-antivirus-inspection/README.md) | Flow-based antivirus profile deployment and safe test-sample validation |
| [Intrusion Prevention](../11-intrusion-prevention/README.md) | IPS sensor deployment, controlled test traffic, and dropped-event validation |
| [Application Control and Quarantine](../12-application-control-and-quarantine/README.md) | Application signature blocking, TeamViewer validation, and a temporary quarantine workflow |
| [Final Summary](../13-final-summary/README.md) | Validation results, recommendations, limitations, and portfolio outcomes |
