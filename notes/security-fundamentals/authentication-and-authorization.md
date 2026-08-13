---
type: concept-note
status: reviewed
tags:
  - identity
  - authentication
  - authorization
  - iam
  - mfa
---

# Authentication & Authorization

> **Purpose**
>
> This note explains how systems verify identities and determine what authenticated identities are allowed to access or do. Authentication and authorization are foundational concepts in Identity and Access Management (IAM) and are central to detecting and investigating identity-based attacks.

---

# Authentication

## Definition

**Authentication** is the process of verifying that a user, device, application, or other identity is who or what it claims to be.

Authentication answers:

> **Who are you?**

Authentication occurs before authorization decisions are made.

---

## Authentication Factors

Authentication methods are commonly grouped into three factor categories.

### Something You Know

Information the identity knows.

Examples:

- Password
- PIN

### Something You Have

Something the identity possesses.

Examples:

- Mobile phone
- Security key
- Smart card

### Something You Are

A physical characteristic of the identity.

Examples:

- Fingerprint
- Face recognition
- Iris scan

---

# Multi-Factor Authentication (MFA)

## Definition

**Multi-Factor Authentication (MFA)** requires authentication using two or more different authentication factor categories.

Example:

```text
Password
    +
Authenticator App
    ↓
Authentication
```

MFA significantly reduces the risk of account compromise because a stolen password alone may not be sufficient to authenticate.

However, MFA is **not a complete security solution**.

Attackers may attempt to bypass or abuse MFA through techniques such as:

- MFA fatigue
- Social engineering
- Session cookie theft
- Adversary-in-the-Middle (AiTM) phishing
- SIM swapping

---

## MFA Fatigue

MFA fatigue, also called push bombing, abuses repeated MFA approval requests to pressure a user into approving an authentication attempt they did not initiate.

Typical sequence:

```text
Password compromised
        ↓
Attacker attempts login
        ↓
MFA challenge triggered
        ↓
Repeated MFA notifications
        ↓
User eventually approves
        ↓
Attacker gains authenticated access
```

The MFA control itself was not necessarily technically defeated. Instead, the attacker abused the authentication process through social engineering.

---

# Authorization

## Definition

**Authorization** determines what an authenticated identity is permitted to access or do.

Authorization answers:

> **What are you allowed to access or do?**

Examples include permission to:

- Read files
- Modify configurations
- Restart servers
- View payroll data
- Access SIEM dashboards
- Access administrative settings

Authorization occurs after the identity has been authenticated.

---

# Authentication vs Authorization

| Authentication               | Authorization                         |
| ----------------------------- | -------------------------------------- |
| Verifies identity              | Determines permissions                 |
| Answers "Who are you?"         | Answers "What are you allowed to do?"  |
| Happens before authorization   | Depends on the authenticated identity  |
| Example: password + MFA        | Example: permission to access payroll  |

### Example

An employee signs into Microsoft 365 using a password and MFA.

```text
Employee
    ↓
Password + MFA
    ↓
Authentication
    ↓
Identity verified
    ↓
Authorization
    ↓
Determine permitted resources
    ↓
Outlook / SharePoint / Teams / Admin settings
```

A user can successfully authenticate but still be denied access because they do not have authorization for the requested resource.

---

# Access Control

**Access Control** is the mechanism used to enforce authorization decisions.

A simplified workflow is:

```text
Identity
    ↓
Authentication
    ↓
Authorization
    ↓
Access Control
    ↓
Allow or Deny
```

The distinction is important:

> **Authorization determines what should be allowed. Access control enforces that decision.**

---

# Principle of Least Privilege

## Definition

The **Principle of Least Privilege (PoLP)** states that users, applications, and systems should receive only the permissions necessary to perform their required tasks.

### Example

A marketing intern should not have Domain Administrator privileges.

A SOC analyst may need permission to:

- View SIEM data
- Investigate alerts
- Access security logs

but may not require permission to modify production infrastructure.

---

## Why Least Privilege Matters

Least privilege helps:

- Reduce attack surface
- Limit the blast radius of compromised accounts
- Reduce privilege abuse
- Restrict unauthorized actions
- Simplify auditing

If an account is compromised, excessive permissions can allow an attacker to cause significantly more damage.

---

# Relationship with IAM

Authentication and authorization are core components of **Identity and Access Management (IAM)**.

IAM focuses on managing identities and controlling their access to organizational resources.

A useful simplified model is:

```text
Identity
    ↓
Authentication
"Who are you?"
    ↓
Authorization
"What can you access?"
    ↓
Access Control
"Allow or deny"
```

For a broader explanation of identity lifecycle, roles, groups, and access-control models, see:

[Identity and Access Management (IAM)](identity-and-access-management.md)

---

# SOC Relevance

Authentication and authorization generate important security telemetry that SOC analysts may investigate.

Examples include:

- Failed authentication attempts
- Successful logins after repeated failures
- Password spraying indicators
- MFA failures
- Suspicious MFA requests
- Privilege escalation
- New user creation
- Group membership changes
- Suspicious cloud logins
- Disabled account logins
- Unusual authentication locations
- New device registrations

An analyst should not automatically assume that an authentication alert represents a compromise.

The analyst must examine the available evidence and determine whether the activity is:

- Expected
- Benign
- Suspicious
- Potentially malicious
- Part of a confirmed security incident

---

# Authentication & Authorization in Identity Attacks

Attackers frequently target authentication systems and identities because compromised credentials or authenticated sessions can provide legitimate access to organizational resources.

Common identity attacks include:

- Brute force
- Password spraying
- Credential stuffing
- Phishing
- Session hijacking
- MFA fatigue

See:

[Identity Attacks](identity-attacks.md)

for detailed coverage of identity-based attack techniques and their detection opportunities.

---

# Interview Notes

### What is authentication?

Authentication verifies that an identity is who or what it claims to be.

### What is authorization?

Authorization determines what an authenticated identity is permitted to access or do.

### What is the difference between authentication and authorization?

Authentication verifies identity; authorization determines permissions.

### Why is MFA useful?

MFA requires multiple authentication factors, so compromising a password alone may not be sufficient to gain access.

### Does MFA prevent account compromise?

No. MFA significantly reduces risk but can be abused or bypassed through techniques such as MFA fatigue, social engineering, session theft, and AiTM phishing.

### What is least privilege?

Least privilege means granting only the permissions necessary for an identity, application, or system to perform its required tasks.

---

# Key Takeaways

- **Authentication verifies identity.**
- **Authorization determines permissions.**
- **Access control enforces authorization decisions.**
- **MFA adds additional authentication factors and reduces the risk of password-only compromise.**
- **MFA is not invulnerable.**
- **Least privilege limits what a compromised identity can do.**
- **Authentication and authorization are core components of IAM.**
- **Authentication activity is important security telemetry for SOC analysts.**

---

# Related Notes

- [Identity and Access Management (IAM)](identity-and-access-management.md)
- [Identity Attacks](identity-attacks.md)
- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
