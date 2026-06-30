# FortiGate Password Policy Hardening

This chapter documents local FortiGate password-policy hardening for administrator credentials and IPsec pre-shared keys. It separates the lab enforcement baseline from the stronger credential controls expected in a production firewall deployment.

---

## Purpose

Enforce a baseline credential policy for local FortiGate secrets and document where this lab control stops compared with production authentication hardening.

## Technical Context

Password controls reduce the chance that locally managed FortiGate credentials or IPsec pre-shared keys can be guessed through dictionary, brute-force, or weak-password attacks. The configured policy requires at least eight characters and a mixture of uppercase letters, lowercase letters, numbers, and special symbols.

The selected scope is `Both`. On FortiGate, this applies to locally defined administrator passwords and IPsec VPN pre-shared keys; it does not replace the Active Directory password policy for LDAP users. Eight characters are suitable for demonstrating the control in a lab, while production environments should prefer longer passwords or passphrases and multi-factor authentication.

> Password policy scope is important because FortiGate cannot impose this local setting on credentials stored in Active Directory. LDAP users remain governed by the domain's account policy, while this FortiGate control protects the local administrative and IPsec secrets managed on the firewall.

**Implemented controls:**

- Enabled the FortiGate password policy.
- Applied the policy to administrator passwords and IPsec pre-shared keys.
- Required multiple character classes to reduce predictable password choices.
- Documented the stronger password and MFA expectations for production use.

## Key Technical Terms

| Term | Meaning in this chapter |
|------|-------------------------|
| Password policy | A rule set that defines length and complexity requirements for accepted local secrets. |
| Scope `Both` | The FortiGate setting that applies the local policy to administrator passwords and IPsec pre-shared keys. |
| Pre-shared key | A shared secret used by VPN peers to authenticate each other during tunnel negotiation. |
| MFA | Multi-Factor Authentication, a production control that adds a second proof beyond a password. |

## Steps Covered

| Step | Description |
|------|-------------|
| Configure the password policy | The password policy page is opened under the FortiGate system settings and the scope is set to Both . |

---

## Detailed Walkthrough

### Step 01 - Configure the password policy

The password-policy page is opened under the FortiGate system settings and the scope is set to `Both`. The minimum length and character requirements are then configured so newly accepted local administrator passwords and IPsec pre-shared keys must satisfy the defined baseline.

> Complexity requirements make simple guessing attacks more expensive, but they do not compensate for reused credentials or a compromised administrator session. Production protection should combine long unique secrets with MFA, restricted management access, monitoring, and timely credential rotation.

![FortiGate password policy](../../images/03-password-policy/01.png)

<p><sub><strong>Screenshot 014 - Password Complexity Policy:</strong> FortiGate enforces the configured length and character requirements for both administrator passwords and IPsec pre-shared keys.</sub></p>


---

## Validation

Validation is configuration-based: the FortiGate policy screen shows the enabled scope, length, and character-class requirements. A production validation would also test rejected weak passwords and confirm MFA or centralized admin authentication where used.

## Chapter Summary

This chapter defines the local credential baseline used by the FortiGate lab. The configuration evidence confirms the password-policy scope and complexity requirements while leaving production MFA and centralized identity controls as hardening recommendations.

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
