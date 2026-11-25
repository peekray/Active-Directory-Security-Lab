# Active Directory — Security Event Analysis

## 1. Failed Logon Attempts — Event ID 4625

Command:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4625
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Purpose:

Identify failed Windows authentication attempts.

Laboratory findings:

- Failed authentication attempts were recorded.
- Some events showed status `0xC000006D` and substatus `0xC000006A`, indicating an incorrect password.
- Other events showed `0xC000015B`, indicating that the requested logon type was not granted.

These are different failure conditions and should not be interpreted as the same issue.

## 2. Process Creation — Event ID 4688

Command:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4688
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Purpose:

Monitor process creation activity.

Laboratory findings:

- Process creation events were recorded.
- Examples included `powershell.exe` and `conhost.exe`.
- The command-line field was not populated in the observed events.

## 3. Security Group Changes — Event IDs 4728 and 4729

Event 4728 — Member added:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4728
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Event 4729 — Member removed:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4729
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Laboratory findings:

- Carlos Rivera (`crivera`) was temporarily added to `Security-Analysts`.
- Event 4728 recorded the addition.
- Event 4729 recorded the removal.

These events demonstrate monitoring of security group membership changes.

## 4. Account Lockout — Event ID 4740

Command:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4740
} -MaxEvents 5 |
Format-List TimeCreated, Id, Message
```

Purpose:

Identify accounts locked out by Active Directory.

Laboratory findings:

- The dedicated account `lockouttest` reached the configured lockout threshold.
- Event 4740 recorded the account lockout.
- The account was subsequently unlocked and verified.

## 5. Directory Service Changes — Event ID 5136

Command:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 5136
} -MaxEvents 10 |
Format-List TimeCreated, Id, Message
```

Purpose:

Monitor modifications to Active Directory objects.

Laboratory findings:

- The description attribute of user `atorres` was modified.
- Event 5136 recorded the previous and new values.
- The event demonstrated directory change auditing.

## 6. Security Findings

| Finding | Observation |
|---|---|
| LDAP enumeration | Authenticated user enumeration succeeded |
| SMB enumeration | Default domain shares were visible |
| SYSVOL access | Domain Group Policy files were readable |
| Password policy | Minimum length configured to 14 characters |
| Account lockout | Threshold configured to 5 failed attempts |
| Kerberos | Valid TGT successfully obtained |
| Security auditing | Multiple security event categories enabled |

## 7. Recommendations

- Review Active Directory group membership regularly.
- Monitor failed logon attempts and account lockouts.
- Maintain strong password and lockout policies.
- Monitor privileged account activity.
- Review Group Policy configurations periodically.
- Centralize and correlate security events where possible.

## Conclusion

This laboratory demonstrated practical Active Directory security monitoring, authentication analysis, network reconnaissance, account management, and Windows event investigation.

All activities were conducted in an isolated educational environment.
