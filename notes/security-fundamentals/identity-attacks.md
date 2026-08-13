---
type: concept-note
status: reviewed
tags:
  - identity
  - attacks
  - authentication
---

# Identity Attacks

> **Purpose**
>
> This note documents common attacks targeting digital identities, authentication systems, credentials, and authenticated sessions. It also covers the indicators and investigation approaches relevant to a SOC analyst.
>
> Identity attacks are important because compromised credentials or sessions can provide attackers with legitimate-looking access to organizational resources.

---

# Digital Identity

For the full definition, entity types, and identity-related attributes, see:

[Identity and Access Management (IAM)](identity-and-access-management.md)

Attackers target identities because compromising one can provide access to systems and resources without necessarily exploiting a software vulnerability.

---

# Why Identity Attacks Matter

Traditional attacks often focused heavily on exploiting systems and network infrastructure.

Modern organizations rely extensively on:

- Cloud computing
- Remote work
- SaaS applications
- Mobile devices
- Remote access
- Identity-based access controls

Users can therefore access organizational resources from many locations and devices.

If an attacker compromises a legitimate identity or authenticated session, malicious activity can appear similar to normal user activity.

For SOC analysts, this makes identity telemetry an important source of security evidence.

---

# Authentication and Authorization

Identity attacks often target authentication mechanisms or abuse the permissions granted after authentication.

### Authentication

**Authentication** verifies that an identity is who or what it claims to be.

### Authorization

**Authorization** determines what an authenticated identity is permitted to access or do.

See:

[Authentication & Authorization](authentication-and-authorization.md)

for detailed coverage of authentication factors, MFA, authorization, access control, and least privilege.

---

# Multi-Factor Authentication (MFA)

MFA significantly reduces the risk of password-only compromise, but it does not eliminate identity attacks. Attackers still abuse it through MFA fatigue (push bombing), Adversary-in-the-Middle (AiTM) phishing, session cookie theft, SIM swapping, and plain social engineering.

For the authentication factor categories, the MFA fatigue sequence, and the full list of bypass techniques, see:

[Authentication & Authorization](authentication-and-authorization.md)

---

# Password Attacks

## Brute Force

A **brute-force attack** attempts many possible passwords against a target account or authentication system until a valid credential is found.

The number of attempts can be reduced by controls such as:

- Account lockout
- Rate limiting
- MFA
- Strong password policies
- Authentication monitoring

---

## Dictionary Attack

A **dictionary attack** uses a predefined list of likely or commonly used passwords instead of trying every possible combination.

Examples may include:

- `password123`
- `welcome123`
- `admin123`

Dictionary attacks are more efficient than trying every possible password but depend on users choosing predictable passwords.

---

## Password Spraying

A **password spraying attack** attempts a small number of commonly used passwords against many different accounts.

Example:

```text
Common password
      ↓
User 1
User 2
User 3
User 4
...
User 500
```

The attacker may use this approach to reduce the likelihood of triggering account lockout policies associated with repeated failures against one account.

### SOC Indicators

Possible indicators include:

- Similar failed-login patterns across many accounts
- Authentication attempts from the same source against multiple users
- Low numbers of attempts per individual account
- Authentication attempts outside expected patterns

---

## Credential Stuffing

**Credential stuffing** uses usernames and passwords obtained from previous breaches or other credential leaks against another service.

It relies heavily on password reuse.

Example:

```text
Credentials leaked from Service A
        ↓
Same credentials tested against Service B
        ↓
Successful authentication
```

Credential stuffing differs from brute force because the attacker is using **known or previously exposed credentials** rather than guessing passwords.

---

# Phishing

**Phishing** is a social engineering attack designed to trick a victim into revealing information, performing an action, or interacting with a malicious resource.

Identity-focused phishing may attempt to steal:

- Usernames
- Passwords
- MFA codes
- Authentication tokens
- Session information

Common techniques include:

- Fake login pages
- Fake password-reset messages
- Spoofed Microsoft or cloud-service emails
- Malicious links
- Malicious QR codes
- Fake cloud-storage notifications

See:

[Malware & Social Engineering](malware-and-social-engineering.md)

for broader coverage of phishing and social engineering.

---

# Social Engineering Against Identity

Identity attacks do not always require a technical vulnerability.

Attackers may manipulate users through:

- Phishing
- Smishing
- Vishing
- Spear phishing
- Pretexting
- MFA fatigue

The objective may be to:

- Obtain credentials
- Obtain authentication codes
- Convince a user to approve access
- Obtain sensitive information
- Bypass security procedures

---

# Session Hijacking

## Definition

**Session hijacking** occurs when an attacker obtains an authenticated session token or cookie and uses it to access a service as the victim.

The attacker may not need to know the victim's password because authentication has already occurred.

Simplified sequence:

```text
User authenticates
        ↓
Authenticated session created
        ↓
Session token / cookie stolen
        ↓
Attacker reuses session
        ↓
Access as victim
```

Session hijacking can sometimes bypass MFA because the attacker is reusing an already authenticated session rather than performing a new authentication.

---

# Account Takeover (ATO)

## Definition

**Account Takeover (ATO)** occurs when an attacker gains control of another user's account.

Common causes include:

- Stolen credentials
- Password reuse
- Phishing
- Credential stuffing
- Malware
- Session hijacking
- MFA abuse

Potential consequences include:

- Unauthorized access
- Data theft
- Fraud
- Further compromise
- Abuse of legitimate permissions
- Access to additional systems

An account takeover can therefore be both an outcome of an identity attack and the starting point for additional malicious activity.

---

# Identity Theft

**Identity theft** occurs when an attacker steals and uses another person's identity or identity-related information without authorization.

Possible objectives include:

- Account takeover
- Financial fraud
- Impersonation
- Unauthorized access to corporate systems

Identity theft and account takeover are related but not identical concepts.

---

# Cloud Identity Attacks

Cloud environments rely heavily on identity-based access.

Compromising a cloud identity may provide access to resources such as:

- Email
- Cloud storage
- Virtual machines
- Databases
- Administrative portals
- SaaS applications

Because cloud resources can be accessed remotely, identity monitoring is particularly important.

Potential indicators include:

- Suspicious cloud sign-ins
- Unusual locations
- New device registrations
- Unexpected privilege changes
- Unusual resource access
- Suspicious authentication patterns

---

# Zero Trust

Identity attacks succeed partly because traditional security assumed an identity or device was safe simply for being inside the network or having authenticated once. Zero Trust removes that assumption and continuously re-evaluates access instead.

For the full definition, the signals Zero Trust considers, and its relationship to IAM, see:

[Identity and Access Management (IAM)](identity-and-access-management.md)

---

# Common Signs of Identity Compromise

Potential indicators include:

- Multiple failed login attempts
- Successful login after repeated failures
- Logins from unusual locations
- Impossible travel
- Logins from unfamiliar countries
- MFA requests the user did not initiate
- Repeated MFA failures
- Unexpected password-reset activity
- New device registrations
- Suspicious cloud sign-ins
- Disabled-account login attempts
- Unexpected privilege changes
- Suspicious mailbox forwarding rules
- Unusual access to sensitive resources

These indicators do **not automatically prove compromise**.

An analyst must investigate the context and available evidence.

---

# Detection Opportunities for SOC Analysts

SOC analysts may monitor identity-related events such as:

- Authentication failures
- Successful authentication
- Password changes
- Password resets
- MFA events
- Account creation
- Account disablement
- Group membership changes
- Privilege changes
- Device registration
- Cloud sign-ins
- Resource access

Useful evidence can include:

- Username
- Source IP
- Destination system
- Timestamp
- Authentication method
- Device information
- Geolocation
- MFA activity
- User activity
- Related endpoint telemetry

---

# SOC Investigation Workflow

A simplified identity-attack investigation can follow this process:

```text
Identity Alert
      ↓
Identify User / Account
      ↓
Review Authentication History
      ↓
Check Source IP and Location
      ↓
Check Device Information
      ↓
Review MFA Activity
      ↓
Check Password / Account Changes
      ↓
Review Privilege and Group Changes
      ↓
Review Resource Access
      ↓
Correlate Endpoint / Network Evidence
      ↓
Determine Whether Activity Is Expected
      ↓
Assess Impact
      ↓
Contain / Escalate if Necessary
```

The analyst should avoid making a decision based on one indicator alone.

For example:

> **Login from a foreign country ≠ automatic compromise**

It becomes more significant when combined with other evidence, such as:

- Impossible travel
- New device
- MFA anomaly
- Suspicious resource access
- Endpoint compromise
- Known malicious source

---

# Prevention

Organizations can reduce identity-related risk through controls such as:

- Strong and unique passwords
- Password managers
- Multi-Factor Authentication
- Least privilege
- Conditional access
- Security awareness training
- Access reviews
- Account lifecycle management
- Privileged access controls
- Identity monitoring
- Login anomaly detection

No single control completely prevents identity attacks.

---

# Identity Attack Comparison

| Attack | Main Technique | Typical Target |
|---|---|---|
| Brute Force | Many password guesses against a target | Account |
| Dictionary Attack | Common-password list | Account |
| Password Spraying | Few common passwords across many accounts | Multiple accounts |
| Credential Stuffing | Previously leaked credentials | Multiple services |
| Phishing | Social engineering to steal credentials or tokens | User |
| MFA Fatigue | Repeated MFA prompts | User |
| Session Hijacking | Theft and reuse of authenticated session | Active session |
| Account Takeover | Gaining control of an account | User account |

---

# Interview Notes

### Why are identities important to attackers?

Compromised identities can provide legitimate-looking access to systems and resources without requiring the attacker to exploit a software vulnerability.

### What is the difference between brute force and password spraying?

Brute force typically attempts many passwords against one account, while password spraying attempts a small number of common passwords across many accounts.

### What is credential stuffing?

Credential stuffing uses previously leaked username and password combinations against other services.

### Why is password reuse dangerous?

A password compromised in one service can potentially be reused to access another service if the same credentials are used there.

### Can MFA be bypassed?

MFA can be abused or bypassed through techniques such as MFA fatigue, AiTM phishing, session theft, SIM swapping, and social engineering.

### What is session hijacking?

Session hijacking involves obtaining an authenticated session token or cookie and using it to access a service as the victim.

### What is account takeover?

Account takeover occurs when an attacker gains control of another user's account.

### What should a SOC analyst investigate after detecting suspicious login activity?

The analyst should examine the user, source IP, timestamp, location, device, authentication method, MFA activity, account changes, resource access, and related endpoint or network evidence before deciding whether the activity is suspicious or malicious.

---

# Key Takeaways

- **Identity is a major attack surface in modern organizations.**
- Authentication verifies identity; authorization determines permissions.
- **Brute force** attacks many passwords against a target.
- **Dictionary attacks** use lists of common passwords.
- **Password spraying** uses a small number of common passwords across many accounts.
- **Credential stuffing** uses previously leaked credentials.
- **Phishing** uses social engineering to steal credentials or authentication material.
- **MFA fatigue** abuses repeated authentication prompts.
- **Session hijacking** abuses stolen authenticated sessions.
- **Account takeover** occurs when an attacker gains control of an account.
- MFA significantly reduces risk but does not eliminate identity attacks.
- Identity alerts require context and correlation with additional evidence.
- SOC analysts should investigate authentication, MFA, device, network, privilege, and resource-access telemetry.
- A single indicator does not automatically prove identity compromise.

---

# Related Notes

- [Authentication & Authorization](authentication-and-authorization.md)
- [Identity and Access Management (IAM)](identity-and-access-management.md)
- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
- [Malware & Social Engineering](malware-and-social-engineering.md)
- [Threat Actors & Cyber Warfare](threat-actors-and-cyber-warfare.md)
- [Risk Management](risk-management.md)
- [Incident Response](incident-response.md)
- [Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)
