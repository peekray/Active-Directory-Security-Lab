# Active Directory Security Lab
### Hands-on Cybersecurity | SOC Analysis | Junior Penetration Testing

![Platform](https://img.shields.io/badge/Platform-Windows%20Server%202022-blue)
![Linux](https://img.shields.io/badge/Attacker-Kali%20Linux-purple)
![Virtualization](https://img.shields.io/badge/Virtualization-VirtualBox-orange)
![Focus](https://img.shields.io/badge/Focus-Active%20Directory%20Security-red)

## Project Overview

This project consists of building and analyzing an isolated Active Directory environment to practice Windows domain administration, network reconnaissance, authenticated enumeration, security auditing, and authentication monitoring.

The objective was to understand how an Active Directory environment can be assessed from both a system administration and cybersecurity perspective.

## Lab Architecture

| Component | Configuration |
|---|---|
| Domain Controller | Windows Server 2022 |
| Domain | `techsolutions.test` |
| NetBIOS | `TECHSOLUTIONS` |
| Domain Controller IP | `192.168.56.10` |
| Attacker Machine | Kali Linux |
| Kali Host-only IP | `192.168.56.30` |
| Kali NAT IP | `10.0.3.15` |
| Virtualization | Oracle VirtualBox |

### Network Design

- Host-only networking provided isolated communication between Kali and Windows Server.
- NAT networking provided Internet access to Kali.
- Both machines were configured in a controlled laboratory environment.

## Tools and Technologies

- Windows Server 2022
- Active Directory Domain Services
- DNS
- Group Policy
- PowerShell
- Kali Linux
- Nmap
- LDAP utilities
- SMBClient
- Kerberos
- Windows Event Viewer
- Windows Security Auditing
- VirtualBox

## Project Activities

### 1. Active Directory Administration

- Installed and configured Active Directory Domain Services.
- Created organizational units.
- Created and managed domain users.
- Created security groups and configured memberships.
- Reviewed domain and forest configuration.

### 2. Network Reconnaissance

Performed service discovery against the laboratory Domain Controller using Nmap.

Identified services included:

- DNS (53)
- Kerberos (88)
- LDAP (389)
- SMB (445)
- Global Catalog (3268)

### 3. LDAP Enumeration

Performed authenticated LDAP queries to identify Active Directory user objects and account names.

### 4. SMB Enumeration and SYSVOL Analysis

- Enumerated available SMB shares.
- Accessed SYSVOL using an authorized domain account.
- Reviewed domain Group Policy files.
- Examined security settings stored in `GptTmpl.inf`.

### 5. Password Policy and Account Lockout

Configured and verified:

- Minimum password length: 14 characters.
- Password complexity: Enabled.
- Password history: 24 passwords.
- Account lockout threshold: 5 failed attempts.
- Lockout duration: 30 minutes.

Used a dedicated laboratory account to validate account lockout behavior.

### 6. Windows Security Event Monitoring

Enabled auditing and investigated the following Windows Security Event IDs:

| Event ID | Description |
|---|---|
| 4625 | Failed logon |
| 4688 | Process creation |
| 4728 | Security group member added |
| 4729 | Security group member removed |
| 4740 | Account locked out |
| 5136 | Directory service object modified |

### 7. Kerberos Authentication

- Configured the Kerberos client in Kali Linux.
- Authenticated using a domain account.
- Obtained a Ticket Granting Ticket (TGT).
- Verified the ticket using `klist`.

## Security Findings

| Area | Observation |
|---|---|
| LDAP | Authenticated enumeration exposed domain account names |
| SMB | Standard domain shares were accessible to the authorized user |
| SYSVOL | Group Policy files were readable |
| Password policy | Stronger password length and lockout settings were configured |
| Account monitoring | Lockout activity was recorded in Event ID 4740 |
| Group monitoring | Membership changes were recorded in Events 4728 and 4729 |
| Directory auditing | Attribute modifications were recorded in Event ID 5136 |
| Kerberos | Successful TGT issuance was verified |

These observations represent the behavior of this laboratory configuration. They are not, by themselves, proof of exploitable vulnerabilities.

## Evidence

### Lab Architecture

- [Virtual machines](evidence/architecture/01-virtual-machines.png)
- [Windows Server network](evidence/architecture/02-windows-server-network.png)
- [Kali Host-only network](evidence/architecture/03-kali-host-only-network.png)
- [Kali NAT network](evidence/architecture/04-kali-nat-network.png)
- [Kali to Domain Controller connectivity](evidence/architecture/05-kali-to-domain-controller-ping.png)
- [Kali IP configuration](evidence/architecture/06-kali-ip-configuration.png)

### Network Reconnaissance and Enumeration

- [Nmap scan](evidence/nmap/08-nmap-domain-controller.png)
- [LDAP enumeration](evidence/ldap/09-ldap-enumeration.png)
- [SMB enumeration](evidence/smb/10-smb-enumeration.png)

### Windows Security Events

- [Account lockout — Event 4740](evidence/windows-events/19-event-4740-account-lockout.png)
- [Failed logon — Event 4625](evidence/windows-events/20-event-4625-failed-logon.png)
- [Process creation — Event 4688](evidence/windows-events/21-event-4688-process-creation.png)
- [Group member added — Event 4728](evidence/windows-events/22-event-4728-group-member-added.png)
- [Group member removed — Event 4729](evidence/windows-events/23-event-4729-group-member-removed.png)
- [Directory modification — Event 5136](evidence/windows-events/24-event-5136-directory-change.png)

### Kerberos and Security Policies

- [Kerberos TGT verification](evidence/kerberos/25-kerberos-authentication-klist.png)
- [Final password policy](evidence/security-policies/26-final-password-policy.png)
- [Final audit policy](evidence/security-policies/27-final-audit-policy.png)
- [Account lockout reset](evidence/security-policies/28-account-lockout-reset.png)

## Command Documentation

Detailed command references are available in the `commands/` directory:

- [Windows Server commands](commands/windows-server.md)
- [Kali Linux commands](commands/kali-linux.md)
- [Kerberos commands](commands/kerberos.md)
- [Security event analysis](commands/security-analysis.md)

## Skills Demonstrated

- Active Directory administration
- Windows Server security configuration
- Network reconnaissance
- LDAP enumeration
- SMB enumeration
- DNS troubleshooting
- Kerberos authentication
- PowerShell scripting and administration
- Windows Security Event Log analysis
- Account lockout investigation
- Security auditing
- Technical documentation

## Recommendations

- Review security group memberships regularly.
- Monitor failed authentication attempts.
- Maintain strong password policies.
- Audit changes to Active Directory objects.
- Monitor process creation and suspicious activity.
- Review Group Policy configurations.
- Centralize security logs for correlation and incident investigation.

## Disclaimer

This project was developed strictly for educational purposes in an isolated VirtualBox laboratory.

All reconnaissance, enumeration, authentication, and security testing were performed against systems owned and controlled by the author.

No unauthorized systems were accessed.

---

**Project:** Active Directory Security Lab  
**Environment:** Windows Server 2022 + Kali Linux  
**Focus:** Active Directory Security, SOC Monitoring, and Penetration Testing Fundamentals
