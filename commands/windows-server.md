# Windows Server 2022 — Commands Reference

## Active Directory Information

Display domain information:

```powershell
Get-ADDomain
```

Display forest information:

```powershell
Get-ADForest
```

Display domain controllers:

```powershell
Get-ADDomainController -Filter *
```

## Organizational Units, Users and Groups

List organizational units:

```powershell
Get-ADOrganizationalUnit -Filter *
```

List domain users:

```powershell
Get-ADUser -Filter * -Properties Enabled |
Select-Object Name, SamAccountName, Enabled
```

List domain groups:

```powershell
Get-ADGroup -Filter *
```

Display members of IT-Support:

```powershell
Get-ADGroupMember -Identity "IT-Support" |
Select-Object Name, SamAccountName
```

Display members of Security-Analysts:

```powershell
Get-ADGroupMember -Identity "Security-Analysts" |
Select-Object Name, SamAccountName
```

Add Carlos Rivera to Security-Analysts:

```powershell
Add-ADGroupMember -Identity "Security-Analysts" -Members "crivera"
```

Remove Carlos Rivera from Security-Analysts:

```powershell
Remove-ADGroupMember -Identity "Security-Analysts" -Members "crivera" -Confirm:$false
```

## Password and Account Lockout Policy

Display the default domain password policy:

```powershell
Get-ADDefaultDomainPasswordPolicy
```

Configure password length and account lockout settings:

```powershell
Set-ADDefaultDomainPasswordPolicy `
-Identity "techsolutions.test" `
-MinPasswordLength 14 `
-LockoutThreshold 5 `
-LockoutDuration (New-TimeSpan -Minutes 30) `
-LockoutObservationWindow (New-TimeSpan -Minutes 30)
```

Verify a user's account status:

```powershell
Get-ADUser lockouttest -Properties Enabled,LockedOut,BadPwdCount |
Select-Object Name,Enabled,LockedOut,BadPwdCount
```

Unlock the laboratory test account:

```powershell
Unlock-ADAccount -Identity "lockouttest"
```

## Windows Security Auditing

Enable process creation auditing:

```powershell
auditpol /set /subcategory:"Process Creation" /success:enable /failure:enable
```

Enable directory service changes auditing:

```powershell
auditpol /set /subcategory:"Directory Service Changes" /success:enable /failure:enable
```

Enable security group management auditing:

```powershell
auditpol /set /subcategory:"Security Group Management" /success:enable /failure:enable
```

Enable group membership auditing:

```powershell
auditpol /set /subcategory:"Group Membership" /success:enable /failure:enable
```

Enable account lockout auditing:

```powershell
auditpol /set /subcategory:"Account Lockout" /success:enable /failure:enable
```

Verify audit configuration:

```powershell
auditpol /get /category:*
```

## Windows Event Log Investigation

Query failed logon events (4625):

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4625
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Query process creation events (4688):

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4688
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Query security group membership additions (4728):

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4728
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Query security group membership removals (4729):

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4729
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Query account lockout events (4740):

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4740
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Query directory service changes (5136):

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 5136
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

## Active Directory Attribute Modification

Modify the description of the laboratory user:

```powershell
Set-ADUser -Identity "atorres" -Description "SOC Audit Test - Change 2"
```

This change was used to generate and investigate Event ID 5136.

## Group Policy Investigation

Generate a computer policy report:

```powershell
gpresult /scope computer /r
```

Export the Resultant Set of Policy report:

```powershell
Get-GPResultantSetOfPolicy -ReportType Html -Path C:\AD-RSoP.html
```

Export local security policy settings:

```powershell
secedit /export /cfg C:\audit-policy.inf
```

## Project Notes

- Domain: `techsolutions.test`
- Domain Controller IP: `192.168.56.10`
- NetBIOS domain: `TECHSOLUTIONS`
- Password minimum length: 14 characters
- Account lockout threshold: 5 failed attempts
- Lockout duration: 30 minutes
- Password history: 24 passwords

All commands were used in an isolated educational Active Directory laboratory.

**Security note:** No real user passwords or credentials are included in this documentation.
