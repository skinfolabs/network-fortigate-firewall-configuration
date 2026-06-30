# FortiGate Firewall Security, VPN, and Threat-Prevention Lab

![Firewall](https://img.shields.io/badge/Firewall-FortiGate-EE3124)
![VPN](https://img.shields.io/badge/VPN-SSL%20VPN%20%7C%20IPsec-2176FF)
![Security](https://img.shields.io/badge/Security-Web%20%7C%20DNS%20%7C%20AV%20%7C%20IPS-2E8B57)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

> Project by Samuel Kim. All rights reserved. See [LICENSE](LICENSE).

## Project Overview

This project documents a FortiGate firewall lab focused on secure access, identity-aware policy design, VPN connectivity, internal service publishing, and layered threat prevention. FortiGate is used as the central enforcement point between the WAN, the internal LAN, remote VPN users, and protected internal services.

The lab demonstrates how several FortiGate capabilities work together in an enterprise-style workflow: administrator role-based access control, password hardening, LDAP integration, SSL VPN Tunnel Mode, SSL VPN Web Mode, Virtual IP publishing, site-to-site IPsec VPN, SSL/TLS inspection, web filtering, DNS filtering, antivirus inspection, intrusion prevention, application control, and quarantine.

From a cybersecurity perspective, the project focuses on least privilege, controlled remote access, service-restricted firewall policy, encrypted traffic inspection, content filtering, malware-prevention testing, botnet/C2 protection, and validation through client behavior and FortiGate logs.

## Network Topology

FortiGate is positioned between the external WAN and the protected internal environment. `Port1` represents the WAN-facing interface, while `Port2` represents the LAN-facing interface. The internal side includes the AtlasAD domain server and a Windows client used for RDP, SSL VPN validation, web access testing, and security-profile enforcement checks.

| Component | Purpose |
|-----------|---------|
| FortiGate VM | Firewall, VPN gateway, destination NAT device, and security-inspection point |
| Port 1 | WAN-facing interface connected to the external network path |
| Port 2 | LAN-facing interface connected to the internal lab environment |
| AtlasAD | Active Directory and LDAP server; also hosts IIS for the publishing exercise |
| Windows client | Internal workstation used for RDP, browsing, VPN, and validation tests |
| FortiClient VPN | Remote-access client used to validate SSL VPN Tunnel Mode |
| Envario | Virtual lab platform hosting the systems used in the exercise |

![FortiGate logical topology](images/01-fortigate-overview-and-network-topology/01.png)

<p><sub><strong>Screenshot 002 - FortiGate Logical Topology:</strong> The firewall is positioned between the internal LAN and the external internet path, establishing the trust boundary used by the later policies.</sub></p>

![FortiGate lab network overview](images/01-fortigate-overview-and-network-topology/02.png)

<p><sub><strong>Screenshot 003 - Lab Network Overview:</strong> The FortiGate VM, WAN path, internal server, workstation, and lab network segments are shown together.</sub></p>

### Site Addressing Used by the IPsec Exercise

| Site | Networks / endpoint |
|------|---------------------|
| New York | `10.90.1.0/24`, `10.90.11.0/24` |
| Tel Aviv | `10.74.1.0/24`, `10.74.11.0/24` |
| Tel Aviv WAN | `13.82.92.101` |

The addresses belong to the isolated lab evidence and make the VPN design reproducible. They should not be reused as production addressing documentation.

## Objectives

- Define the FortiGate placement, interfaces, trusted networks, and external traffic path.
- Implement role-based administrative access with named accounts and restricted profiles.
- Apply a FortiGate password policy to local administrators and IPsec pre-shared keys.
- Integrate Active Directory identities through LDAP for group-based remote access.
- Configure and validate SSL VPN Tunnel Mode and browser-based Web Mode access.
- Publish an internal IIS service through destination NAT and a controlled inbound policy.
- Build a site-to-site IPsec VPN with directional service restrictions.
- Inspect encrypted and application traffic with FortiGate security profiles.
- Validate allowed and blocked behavior through client results, VPN status, and FortiGate logs.

## Project Chapters

| Chapter | Description |
|---------|-------------|
| [Administrator Access and Role-Based Control](docs/01-administrator-access-and-rbac/README.md) | Named administrator accounts, custom admin profiles, and least-privilege validation |
| [FortiGate Password Policy Hardening](docs/02-password-policy-hardening/README.md) | Local password complexity controls for administrators and IPsec pre-shared keys |
| [LDAP Integration and SSL VPN Tunnel Mode](docs/03-ldap-and-ssl-vpn-tunnel-mode/README.md) | Active Directory LDAP integration, SSL VPN Tunnel Mode, FortiClient access, and restricted RDP validation |
| [SSL VPN Web Mode](docs/04-ssl-vpn-web-mode/README.md) | Browser-based SSL VPN access with LDAP group mapping and an RDP bookmark |
| [IIS Publishing with Destination NAT](docs/05-iis-publishing-with-destination-nat/README.md) | Internal IIS publishing through a FortiGate Virtual IP and inbound firewall policy |
| [Site-to-Site IPsec VPN](docs/06-site-to-site-ipsec-vpn/README.md) | FortiGate-to-FortiGate IPsec connectivity with directional service restrictions |
| [Full SSL/TLS Inspection](docs/07-full-ssl-tls-inspection/README.md) | Full SSL/TLS inspection profile creation and outbound policy attachment |
| [Web Filtering](docs/08-web-filtering/README.md) | Static URL filtering, category authentication, client testing, and FortiGate web-filter logs |
| [DNS Filtering](docs/09-dns-filtering/README.md) | DNS filter profile configuration, controlled domain blocking, and DNS-filter log review |
| [Antivirus Inspection](docs/10-antivirus-inspection/README.md) | Flow-based antivirus profile deployment and safe test-sample validation |
| [Intrusion Prevention](docs/11-intrusion-prevention/README.md) | IPS sensor deployment, controlled test traffic, and dropped-event validation |
| [Application Control and Quarantine](docs/12-application-control-and-quarantine/README.md) | Application signature blocking, TeamViewer validation, and a temporary quarantine workflow |
| [Final Summary](docs/13-final-summary/README.md) | Validation results, production recommendations, limitations, and portfolio outcomes |

## Lab Environment Summary

| Component | Role |
|-----------|------|
| FortiGate VM | Firewall, router, VPN endpoint, destination NAT device, and inspection engine |
| Active Directory / LDAP | Central identity source for VPN users and groups |
| IIS on AtlasAD | Internal web service used for destination NAT publishing validation |
| Windows workstation | RDP destination and client-side validation machine |
| FortiClient | SSL VPN Tunnel Mode client |
| Remote FortiGate peer | IPsec VPN endpoint for the site-to-site exercise |

## Tools and Technologies

- FortiGate / FortiOS web administration
- Active Directory Domain Services and LDAP authentication
- SSL VPN Tunnel Mode, Web Mode, and FortiClient
- Remote Desktop Protocol (RDP)
- Internet Information Services (IIS)
- Destination NAT through a FortiGate Virtual IP
- Site-to-site IPsec VPN
- SSL/TLS inspection
- Web Filter, DNS Filter, Antivirus, IPS, and Application Control

## Key Outcomes

- Administrative access was separated into named accounts and restricted profiles.
- LDAP-backed user groups were used for SSL VPN Tunnel Mode and Web Mode access.
- SSL VPN Tunnel Mode was validated with FortiClient and an internal RDP session.
- SSL VPN Web Mode was validated through a browser portal and RDP bookmark.
- An internal IIS portal was published through FortiGate destination NAT.
- A site-to-site IPsec VPN was configured and tested with directional service limits.
- Web, DNS, antivirus, IPS, and application controls were configured and validated with client-side behavior and FortiGate logs.

## Skills Demonstrated

- FortiGate firewall administration and directional policy design
- Role-based administrator access and least-privilege validation
- Active Directory LDAP integration for identity-aware policy
- SSL VPN Tunnel Mode, Web Mode, FortiClient, and RDP access
- Destination NAT and internal-service publishing
- Site-to-site IPsec VPN configuration and traffic restriction
- SSL/TLS inspection planning and policy attachment
- Web Filter, DNS Filter, Antivirus, IPS, and Application Control deployment
- FortiGate log review and evidence-based validation
