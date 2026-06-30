# Full SSL/TLS Inspection

This chapter covers outbound SSL/TLS inspection planning and policy attachment. It explains why encrypted traffic visibility matters and what the available evidence confirms.

---

## Purpose

Attach an SSL/TLS inspection profile to outbound traffic so later security profiles can inspect encrypted sessions when certificate trust and policy design allow it.

## Technical Context

Encrypted traffic protects confidentiality between a client and a remote service, but it can also hide downloads, web requests, or application behavior from ordinary firewall inspection. Full SSL/TLS inspection allows FortiGate to act as an inspection intermediary: it decrypts the permitted session, applies the selected security profiles, and re-encrypts the connection toward its destination.

This chapter creates a dedicated inspection profile and attaches it to outbound traffic. The configuration prepares the firewall to inspect encrypted content that would otherwise be visible only as a TLS connection between two addresses.

> Deep inspection changes the certificate trust path seen by the client. The FortiGate inspection CA must be trusted by managed endpoints, and production deployment must account for privacy, certificate-pinned applications, legal requirements, and explicit bypass categories.

**Implemented controls:**

- Created `Office_To_Internet_Inspection` in Full SSL Inspection mode.
- Selected the FortiGate laboratory inspection CA.
- Applied the profile to the outbound firewall policy.
- Documented the additional trust and validation requirements for production.

Production recommendation: full inspection requires a trusted inspection CA distributed to managed clients, privacy review, application exceptions, and staged compatibility testing. The lab shows the profile and policy attachment, not a full enterprise certificate rollout.

## Key Technical Terms

| Term | Meaning in this chapter |
|------|-------------------------|
| SSL/TLS inspection | Firewall inspection of encrypted sessions so security profiles can evaluate traffic contents. |
| Inspection CA | The certificate authority FortiGate uses to re-sign inspected sessions for managed clients. |
| Full inspection | Deeper TLS inspection that requires client trust in the FortiGate inspection certificate. |
| Policy attachment | The step that makes an inspection profile apply to real matching traffic. |

## Steps Covered

| Step | Description |
|------|-------------|
| Create the inspection profile | The Office To Internet Inspection profile is configured for full inspection using the FortiGate laboratory CA. |
| Apply inspection to outbound traffic | The profile is attached to the firewall policy carrying traffic from the office network toward the internet. |

---

## Detailed Walkthrough

### Step 01 - Create the inspection profile

The `Office_To_Internet_Inspection` profile is configured for full inspection using the FortiGate laboratory CA. The profile determines which encrypted protocols are inspected and which CA FortiGate uses when generating certificates for the client-facing side of inspected TLS sessions.

> The selected certificate is sufficient for demonstrating profile configuration in this isolated lab. In a managed environment, clients must trust the inspection CA or they will receive certificate warnings, and sensitive or incompatible applications may require carefully reviewed exemptions.

![Full SSL inspection profile](../../images/08-ssl-tls-inspection/01.png)

<p><sub><strong>Screenshot 056 - Full SSL/TLS Inspection Profile:</strong> The outbound inspection profile is configured for deep inspection with the FortiGate CA certificate.</sub></p>


---

### Step 02 - Apply inspection to outbound traffic

The profile is attached to the firewall policy carrying traffic from the office network toward the internet. This placement ensures that encrypted sessions matching the outbound rule are handed to the full-inspection profile before the later Web Filter, Antivirus, IPS, or Application Control decisions are made.

> Creating a security profile does nothing until a matching firewall policy references it. The screenshot confirms policy attachment, but a separate client trust deployment and decrypted-session validation were not recorded, so the repository does not claim a complete production rollout.

![SSL inspection applied](../../images/08-ssl-tls-inspection/02.png)

<p><sub><strong>Screenshot 057 - SSL/TLS Inspection Policy:</strong> The full-inspection profile is selected on the outbound firewall rule.</sub></p>


---

## Validation

Validation confirms that the full-inspection profile exists and is attached to the outbound policy. A complete production validation would additionally test certificate trust, browser behavior, inspected sessions, and security-profile detections over HTTPS.

## Chapter Summary

This chapter completes the SSL/TLS inspection setup shown in the lab. The evidence confirms profile creation and policy attachment, while full production use would still require certificate trust deployment and compatibility testing.

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
