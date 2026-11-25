# Active Directory Security Lab

## Project Overview

This project demonstrates a hands-on Active Directory security lab built with Windows Server 2022 and Kali Linux in VirtualBox.

The objective was to configure a Windows domain environment, perform security reconnaissance, test authentication mechanisms, implement security policies, and analyze Windows Security Event Logs.

## Lab Architecture

| Component | Configuration |
|---|---|
| Domain Controller | Windows Server 2022 |
| Attacker / Testing Machine | Kali Linux |
| Hypervisor | Oracle VirtualBox |
| Domain | `techsolutions.test` |
| Domain Controller IP | `192.168.56.10` |
| Kali Linux IP | `192.168.56.30` |
| Network | Host-only + NAT on Kali |

## Project Objectives

- Deploy and configure Active Directory Domain Services.
- Create Organizational Units, users, and security groups.
- Perform network reconnaissance using Nmap.
- Enumerate Active Directory using LDAP.
- Enumerate SMB shares and inspect SYSVOL.
- Configure domain password and account lockout policies.
- Configure Windows security auditing.
- Generate and investigate security events.
- Validate Kerberos authentication from Kali Linux.

## Tools Used

- Windows Server 2022
- Kali Linux
- Oracle VirtualBox
- Nmap
- LDAP utilities
- SMBClient
- Kerberos client utilities
- PowerShell
- Windows Event Viewer

## Security Testing and Results

### 1. Network Reconnaissance

Performed an Nmap service scan against the domain controller.

Identified services included:

- DNS (53)
- Kerberos (88)
- LDAP (389)
- SMB (445)
- Global Catalog LDAP (3268)

### 2. Active Directory Enumeration

Used authenticated LDAP queries to enumerate domain user accounts.

Inspected SMB shares and accessed SYSVOL to review domain Group Policy files.

### 3. Password and Account Lockout Policies

Configured and verified:

- Minimum password length: 14 characters.
- Password complexity: Enabled.
- Password history: 24 passwords.
- Account lockout threshold: 5 failed attempts.
- Lockout duration: 30 minutes.
- Reversible password encryption: Disabled.

### 4. Windows Security Event Analysis

Analyzed the following Windows Security Event IDs:

| Event ID | Description |
|---|---|
| 4625 | Failed logon |
| 4688 | Process creation |
| 4728 | Member added to security group |
| 4729 | Member removed from security group |
| 4740 | Account locked out |
| 5136 | Directory service object modified |

### 5. Kerberos Authentication

Successfully authenticated from Kali Linux using a domain account and obtained a Kerberos Ticket Granting Ticket (TGT).

### 6. Security Auditing

Enabled auditing for:

- Process Creation
- Directory Service Changes
- Security Group Management
- Group Membership
- Account Lockout

## Key Findings

- Authenticated LDAP enumeration exposed domain account names.
- Authenticated SMB enumeration revealed available domain shares.
- SYSVOL exposed domain Group Policy configuration files to an authenticated domain user.
- Account lockout controls
