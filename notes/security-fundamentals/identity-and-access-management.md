---
type: concept-note
status: reviewed
tags:
  - identity
  - iam
  - access-control
---

# Identity and Access Management (IAM)

> **Purpose**
>
> This note explains how organizations manage digital identities and control access to resources at scale. It covers digital identity, the identity lifecycle, roles, groups, access control models, and Zero Trust.
>
> For the mechanics of authentication, authorization, access control, and least privilege themselves, see [Authentication & Authorization](authentication-and-authorization.md). IAM is a foundational security domain because compromised identities can provide attackers with legitimate access to organizational resources.

---

# What Is IAM?

**Identity and Access Management (IAM)** is the process of managing digital identities and controlling their access to organizational resources.

A useful mental model is:

> **The right identity gets the right access to the right resource at the right time.**

IAM helps organizations:

- Identify users and systems
- Authenticate identities
- Authorize access
- Assign permissions
- Manage roles and groups
- Enforce least privilege
- Monitor identity activity
- Remove or modify access when it is no longer required

---

# Why IAM Matters

Modern organizations rely heavily on:

- Cloud services
- Remote work
- SaaS applications
- Mobile devices
- APIs
- Service accounts

Users and systems may access resources from many locations and devices.

As a result, protecting identity and access is a major part of enterprise security.

An attacker who compromises a legitimate identity may be able to access resources without exploiting a software vulnerability.

This makes identity-related activity important for SOC monitoring and investigation.

---

# Digital Identity

## Definition

A **digital identity** represents an entity that can interact with or authenticate to a digital system.

Examples include:

- Human users
- Service accounts
- Applications
- Servers
- Virtual machines
- APIs

An identity may be associated with information such as:

- Username
- Credentials
- Roles
- Permissions
- Group memberships
- Attributes
- Authentication methods

---

# Identity Lifecycle

IAM is not only about logging in.

A simplified identity lifecycle is:

```text
Identity Created
      ↓
Access Assigned
      ↓
Authentication
      ↓
Authorization
      ↓
Access Used
      ↓
Access Reviewed
      ↓
Access Modified or Revoked
      ↓
Identity Disabled / Removed
```

Access should change when a user's role, responsibilities, or employment status changes.

For example, an employee moving from one department to another may require different permissions.

An employee leaving an organization should have unnecessary access revoked or the account disabled according to organizational procedures.

---

# Authentication, Authorization & Access Control

Authentication and authorization are the mechanisms this lifecycle depends on: authentication verifies **who** an identity is, and authorization determines **what** it is allowed to do. Access control then enforces that authorization decision, and least privilege governs how much access any identity should hold in the first place.

These mechanics — authentication factors, MFA and MFA fatigue, the authentication-vs-authorization distinction, access control enforcement, and the Principle of Least Privilege — are covered in full in:

[Authentication & Authorization](authentication-and-authorization.md)

---

# Roles

A **role** is a collection of permissions associated with a particular job function or responsibility.

### Example

```text
SOC Analyst
    ↓
SOC Analyst Role
    ↓
Required Permissions
```

A SOC Analyst role might provide permissions to:

- View SIEM dashboards
- Investigate alerts
- Access relevant security logs

while restricting unnecessary administrative permissions.

Roles help organizations manage access consistently.

---

# Groups

A **group** is a collection of identities managed together for administrative or access-control purposes.

Instead of assigning permissions individually to every user, administrators can assign users to groups and apply appropriate permissions to those groups.

Example:

```text
SOC Team
    ↓
SOC Analyst Group
    ↓
Required Access
```

Groups can simplify:

- Permission management
- Access reviews
- Onboarding
- Offboarding
- Role changes

---

# Roles vs Groups

Roles and groups are related but are not identical concepts.

### Role

Represents a function or set of responsibilities.

Example:

> SOC Analyst

### Group

Represents a collection of identities.

Example:

> SOC Analysts

Organizations may use groups to assign permissions associated with particular roles.

The exact implementation depends on the identity platform and access-control architecture.

---

# Access Control Models

Different organizations can use different models to determine how access is granted.

---

## DAC - Discretionary Access Control

In **Discretionary Access Control (DAC)**, the owner of a resource can control who is allowed to access it.

Example:

A user sharing a Google Drive file with specific people.

The resource owner has discretion over access.

---

## MAC - Mandatory Access Control

In **Mandatory Access Control (MAC)**, access decisions are enforced by centrally defined security policies.

Users cannot freely change access permissions.

MAC is commonly associated with environments requiring strict information-control policies, including certain military and government systems.

---

## RBAC - Role-Based Access Control

In **Role-Based Access Control (RBAC)**, access is assigned according to job roles.

Example:

```text
User
 ↓
SOC Analyst Role
 ↓
SIEM Access
Security Log Access
Investigation Permissions
```

RBAC can simplify enterprise access management because permissions can be associated with roles rather than individually assigned to every user.

---

## ABAC - Attribute-Based Access Control

In **Attribute-Based Access Control (ABAC)**, access decisions are based on attributes and conditions.

Possible attributes include:

- User role
- Device
- Location
- Time
- MFA status
- Risk level

Example:

```text
User
 +
Managed Device
 +
Corporate Network
 +
MFA Completed
 +
Low Risk
      ↓
Access Granted
```

ABAC can provide more context-aware access decisions than simple role-based access.

---

# IAM and Zero Trust

IAM is an important component of **Zero Trust** security models.

Zero Trust does not automatically trust an identity simply because it is inside a network.

Access decisions can consider signals such as:

- Identity
- Device
- Location
- Authentication status
- MFA status
- Risk
- Resource sensitivity

The broader principle is:

> **Access should be continuously evaluated according to identity, context, and policy rather than granted solely because a user or device is considered trusted.**

---

# IAM Security Controls

Organizations can strengthen IAM through controls such as:

- Strong authentication
- MFA
- Least privilege
- Role-based access
- Conditional access
- Access reviews
- Password policies
- Privileged access management
- Account lifecycle management
- Identity monitoring

No single IAM control provides complete protection.

---

# SOC Relevance

IAM generates security telemetry related to the **identity lifecycle** itself — separate from the authentication-specific telemetry (failed logins, password spraying, MFA failures) covered in [Authentication & Authorization](authentication-and-authorization.md).

Common events include:

- Password resets
- New user creation
- Account disablement
- Group membership changes
- Privilege changes
- New device registrations
- Suspicious cloud logins
- Logins from unusual locations
- Impossible travel indicators
- Suspicious mailbox forwarding rules

A single event does not automatically prove compromise.

The analyst should establish context and determine whether the activity is:

- Expected
- Benign
- Suspicious
- Potentially malicious
- Part of a confirmed security incident

---

# IAM Investigation Example

Suppose a user account generates several failed login attempts followed by a successful login from an unusual location.

A SOC analyst might investigate:

```text
Failed Login Attempts
        ↓
Source IP
        ↓
User Account
        ↓
Successful Login
        ↓
Location / Device
        ↓
MFA Activity
        ↓
Subsequent Account Activity
        ↓
Privilege / Resource Access
```

The analyst should then determine whether the activity is consistent with legitimate user behavior or indicates possible account compromise.

Relevant evidence may include:

- Authentication logs
- Source IP addresses
- Device information
- Geolocation
- MFA events
- Password reset activity
- Group membership changes
- Resource access
- Endpoint telemetry

---

# Identity Attacks

IAM knowledge is essential for understanding identity-based attacks.

Common attacks include:

- Brute force
- Dictionary attacks
- Password spraying
- Credential stuffing
- Phishing
- MFA fatigue
- Session hijacking
- Account takeover

These attacks are covered in detail in:

[Identity Attacks](identity-attacks.md)

---

# Interview Notes

### What is IAM?

IAM is the process of managing digital identities and controlling their access to organizational resources.

### Why is IAM important?

Compromised identities can provide attackers with legitimate access to organizational resources, so controlling and monitoring identity access is a major security requirement.

### What is RBAC?

RBAC grants access according to predefined job roles.

### What is ABAC?

ABAC makes access decisions using attributes and contextual conditions such as user role, device, location, time, MFA status, or risk level.

### Why is IAM relevant to a SOC?

Account changes, privilege changes, and identity-lifecycle activity generate security telemetry that SOC analysts can investigate for signs of identity compromise. Authentication-specific telemetry is covered in [Authentication & Authorization](authentication-and-authorization.md).

---

# Key Takeaways

- **IAM manages digital identities and their access to resources.**
- Authentication, authorization, access control, and least privilege are explained in [Authentication & Authorization](authentication-and-authorization.md).
- **Roles represent functions or responsibilities.**
- **Groups organize identities for easier administration and access management.**
- **DAC, MAC, RBAC, and ABAC are different approaches to access control.**
- **IAM is an important component of Zero Trust security.**
- **Identity lifecycle management includes creating, assigning, reviewing, modifying, and revoking access.**
- **IAM generates valuable telemetry for SOC investigations.**
- **Identity-related alerts require context and evidence before they can be classified as malicious.**

---

# Related Notes

- [Authentication & Authorization](authentication-and-authorization.md)
- [Identity Attacks](identity-attacks.md)
- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
- [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)
- [Risk Management](risk-management.md)
- [Defense in Depth](defense-in-depth.md)
