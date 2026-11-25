# Kali Linux — Reconnaissance and Enumeration Commands

## 1. Network Configuration

Display network interfaces and IP addresses:

```bash
ip -br addr
```

Display routing table:

```bash
ip route
```

Display NetworkManager device status:

```bash
sudo nmcli device status
```

Display active network connections:

```bash
nmcli connection show --active
```

## 2. Network Connectivity

Test connectivity to the Domain Controller:

```bash
ping -c 4 192.168.56.10
```

Test external IP connectivity:

```bash
ping -c 4 8.8.8.8
```

Test DNS and Internet connectivity:

```bash
ping -c 4 google.com
```

## 3. DNS Enumeration

Resolve the Active Directory domain:

```bash
nslookup techsolutions.test
```

Resolve the Domain Controller hostname:

```bash
nslookup WIN-2DTLFQUT6UO.techsolutions.test
```

## 4. Nmap Network Reconnaissance

Scan selected Active Directory ports and identify services:

```bash
sudo nmap -n -Pn -sV -p 53,88,135,139,389,445,464,636,3268 192.168.56.10
```

Services investigated:

- 53 — DNS
- 88 — Kerberos
- 135 — Microsoft RPC
- 139 — NetBIOS
- 389 — LDAP
- 445 — SMB
- 464 — Kerberos password change
- 636 — LDAPS (port detected; TLS functionality not independently verified)
- 3268 — Global Catalog LDAP

## 5. LDAP Enumeration

Install LDAP and DNS utilities:

```bash
sudo apt update
sudo apt install ldap-utils dnsutils -y
```

Perform authenticated LDAP enumeration:

```bash
ldapsearch -x -H ldap://192.168.56.10:389 -D "atorres@techsolutions.test" -W -b "DC=techsolutions,DC=test" "(objectClass=user)" sAMAccountName
```

The command queried Active Directory user objects and returned account names.

## 6. SMB Enumeration

List SMB shares using the laboratory account:

```bash
smbclient -L //192.168.56.10 -U 'TECHSOLUTIONS/atorres'
```

Connect to SYSVOL using SMB3:

```bash
smbclient //192.168.56.10/SYSVOL -U 'TECHSOLUTIONS/atorres' -m SMB3
```

SYSVOL was inspected to review domain Group Policy files.

## 7. Kerberos Client Preparation

Install the Kerberos client:

```bash
sudo apt update
sudo apt install krb5-user -y
```

Request a Kerberos Ticket Granting Ticket (TGT):

```bash
kinit atorres@TECHSOLUTIONS.TEST
```

Display Kerberos tickets:

```bash
klist
```

## 8. Laboratory Network Architecture

| Machine | Interface | IP address |
|---|---|---|
| Windows Server 2022 | Host-only | 192.168.56.10 |
| Kali Linux | Host-only | 192.168.56.30 |
| Kali Linux | NAT | 10.0.3.15 |

The Host-only network provided isolated communication between the laboratory machines. The NAT interface provided Kali with Internet access.

## Project Notes

All reconnaissance and enumeration activities were performed against the user's own isolated VirtualBox laboratory.

No external systems were targeted. Authentication credentials are intentionally excluded from this documentation.
