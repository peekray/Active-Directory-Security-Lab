# Kerberos Authentication — Command Reference

## 1. Kerberos Client Installation

Install the Kerberos client utilities on Kali Linux:

```bash
sudo apt update
sudo apt install krb5-user -y
```

The Kerberos client was configured to use the laboratory realm:

`TECHSOLUTIONS.TEST`

Domain Controller:

`192.168.56.10`

## 2. Request a Ticket Granting Ticket (TGT)

Authenticate the laboratory user with the Kerberos Key Distribution Center:

```bash
kinit atorres@TECHSOLUTIONS.TEST
```

The command prompts for the user's laboratory password.

## 3. Verify Kerberos Tickets

Display the current Kerberos ticket cache:

```bash
klist
```

Expected principal:

`atorres@TECHSOLUTIONS.TEST`

Expected ticket:

`krbtgt/TECHSOLUTIONS.TEST@TECHSOLUTIONS.TEST`

## 4. Account Lockout Test

A dedicated laboratory account was used to validate Active Directory account lockout behavior.

The command used to request authentication was:

```bash
kinit lockouttest@TECHSOLUTIONS.TEST
```

Incorrect authentication attempts were intentionally performed against this test account until the configured lockout threshold was reached.

**Important:** The test account was subsequently unlocked and verified as enabled and not locked.

## 5. Security Analysis

The Kerberos test demonstrated:

- Successful authentication with a valid domain account.
- Successful issuance of a Ticket Granting Ticket (TGT).
- Verification of the ticket using `klist`.
- Integration between Kali Linux and the Windows Server 2022 Active Directory domain.
- Account lockout enforcement using a dedicated test account.

## Laboratory Information

- Realm: `TECHSOLUTIONS.TEST`
- Domain: `techsolutions.test`
- Domain Controller: `192.168.56.10`
- Test user: `atorres`
- Test account: `lockouttest`

All tests were performed in an isolated educational laboratory.
