---
type: concept-note
status: reviewed
tags:
  - fundamentals
  - cia-triad
  - risk
  - security-controls
---

# CIA Triad and Basic Security Concepts

> **Purpose**
>
> This note explains the CIA Triad and the foundational security concepts used to understand what organizations are protecting, what can threaten those assets, where weaknesses exist, and how security controls reduce risk.
>
> These concepts form part of the foundation for security monitoring, incident investigation, risk assessment, and SOC analysis.

---

# CIA Triad

The **CIA Triad** is a foundational model used to describe three primary security objectives:

- **Confidentiality**
- **Integrity**
- **Availability**

When investigating a security event, analysts can use the CIA Triad to understand which security property may have been affected.

---

## Confidentiality

### Definition

**Confidentiality** ensures that information is accessible only to authorized users, systems, or entities.

### Goal

Prevent unauthorized disclosure of information.

### Examples

- Customer data is accessed by an unauthorized person.
- Employee credentials are exposed.
- Confidential company documents are leaked.

### Security Controls

Common controls that support confidentiality include:

- Access control
- Least privilege
- Multi-Factor Authentication (MFA)
- Encryption
- Data classification

---

## Integrity

### Definition

**Integrity** ensures that information remains accurate, complete, and protected from unauthorized modification.

### Goal

Prevent unauthorized or improper changes to information.

### Examples

- Payroll records are modified without authorization.
- Database values are altered.
- A website is defaced.
- Malware modifies system files.

### Security Controls

Common controls that support integrity include:

- Hashing
- Digital signatures
- File Integrity Monitoring
- Access control
- Version control

---

## Availability

### Definition

**Availability** ensures that systems, services, and information remain accessible to authorized users when they are needed.

### Goal

Prevent or reduce service interruptions.

### Examples

- Ransomware makes business files unavailable.
- A Distributed Denial-of-Service (DDoS) attack disrupts a service.
- A critical server fails.
- A network outage prevents users from accessing an application.

### Security Controls

Common controls that support availability include:

- Backups
- Disaster recovery
- Redundant infrastructure
- Load balancing
- Uninterruptible Power Supply (UPS)
- Business continuity planning
- DDoS protection

---

# CIA Triad in Security Incidents

A single security incident can affect more than one element of the CIA Triad.

### Example: Ransomware

```text
Ransomware
    ↓
Files encrypted
    ↓
Integrity affected
    +
Availability affected
```

If the attacker also steals the organization's data before encryption:

```text
Data theft
    ↓
Confidentiality affected

File encryption
    ↓
Integrity / Availability affected
```

The CIA Triad therefore helps analysts describe **what security property was affected**, rather than simply labeling something as "a cyberattack."

---

# Core Security Concepts

## Asset

An **asset** is something valuable that an organization needs to protect.

Examples include:

- Data
- User accounts
- Servers
- Workstations
- Applications
- Networks
- Cloud resources
- Intellectual property
- Business services

The importance of an asset influences how much risk the organization may be willing to accept.

---

# Threat

A **threat** is anything capable of causing harm to an asset, system, network, or organization.

Examples include:

- Cybercriminals
- Malware
- Insider threats
- Nation-state actors
- Accidental human actions
- Natural or environmental events

A threat represents a potential source of harm. It does not mean that an attack has already occurred.

---

# Vulnerability

A **vulnerability** is a weakness in hardware, software, configuration, or a process that could be exploited or abused.

A vulnerability does **not** automatically mean that an attack has occurred.

For the full breakdown of vulnerability types, CVE, CVSS, and zero-days, see:

[Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)

---

# Exploit

An **exploit** is a technique, tool, or piece of code used to take advantage of a vulnerability.

For the full breakdown of exploits and the vulnerability–exploit–attack chain, see:

[Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)

---

# Attack

An **attack** is an attempt to compromise a system, account, network, or data.

Examples include:

- Phishing
- Password spraying
- Malware execution
- Exploitation of a software vulnerability
- DDoS

An attack may **succeed or fail**.

---

# Risk

## Definition

**Risk** represents the potential for loss, damage, disruption, or other adverse impact resulting from a threat exploiting or abusing a vulnerability.

Risk is influenced by factors such as likelihood, potential impact, asset value, exposure, threat capability, and vulnerability severity, and should never be treated as a precise calculation.

---

# Vulnerability vs Risk

A vulnerability and risk are not the same thing.

> **Vulnerability = a weakness**  
> **Risk = the potential consequence of that weakness in a particular context**

A vulnerability does not automatically mean that risk is high.

For the full risk formula, the worked isolated-system-vs-internet-facing example, and the complete risk-scoring model, see:

[Risk Management](risk-management.md)

---

# Security Relationship Model

The following model is useful for understanding how the concepts relate:

```text
                Asset
                  │
        ┌─────────┴─────────┐
        │                   │
      Threat           Vulnerability
        │                   │
        └─────────┬─────────┘
                  ↓
                Risk
                  │
          Likelihood × Impact
                  │
          Potential Consequences
```

If exploitation actually occurs:

```text
Threat Actor
     ↓
Attack / Exploit
     ↓
Compromise
     ↓
Potential Impact
     ↓
Security Incident
```

The important distinction is:

> **Risk exists before successful exploitation.**

An organization does not need to wait for an attack to succeed before deciding that a vulnerability represents a significant risk.

---

# Worked Example

Applying the model above to a concrete case:

```text
Asset:
Employee Laptop

Threat:
Cybercriminal

Vulnerability:
Weak Password

Exploit:
Password Spraying

Risk:
Unauthorized access to company resources

Impact:
Sensitive company files are stolen.
```

Each stage maps directly onto the Security Relationship Model: the asset and threat combine with a vulnerability to create risk, and if exploitation actually occurs, that risk becomes a realized impact.

---

# Common Beginner Mistakes

### ❌ "A threat is the same as a vulnerability."

### ✔ Correct

A threat takes advantage of a vulnerability. They are not interchangeable — a threat is a potential source of harm, while a vulnerability is the weakness that makes harm possible.

---

### ❌ "An exploit is the same as malware."

### ✔ Correct

An exploit is the technique or code used to gain access by abusing a vulnerability. Malware is often the payload executed afterward, not the exploit itself.

---

### ❌ "Risk is the same as vulnerability."

### ✔ Correct

A vulnerability is a weakness. Risk is the possibility and potential consequence of that weakness being exploited — see [Vulnerability vs Risk](#vulnerability-vs-risk) above.

---

# Risk Treatment

Organizations can respond to identified risks by reducing/mitigating, transferring, avoiding, or accepting them.

For the full explanation of each approach and worked examples, see:

[Risk Management](risk-management.md)

---

# Security Controls

Security controls are measures used to reduce security risk.

Controls can serve different purposes.

### Preventive Controls

Designed to prevent unwanted activity.

Examples:

- Firewalls
- MFA
- Access controls
- Network segmentation
- IPS (also detective — see below)

### Detective Controls

Designed to identify suspicious or unauthorized activity.

Examples:

- IDS
- IPS (also preventive — can block as well as detect)
- SIEM detections
- EDR detections
- File Integrity Monitoring

### Corrective / Recovery Controls

Designed to restore operations or reduce the impact after an event.

Examples:

- Backups
- Disaster recovery
- Incident response
- System restoration

No single security control provides complete protection.

---

# Defense in Depth

**Defense in Depth** is the use of multiple layers of security controls so that failure of one control does not automatically result in complete compromise.

The important principle is:

> **Do not depend on a single security control to provide complete protection.**

Defense in Depth can reduce likelihood of successful compromise, impact, detection time, and recovery time.

For the full layered architecture and worked examples, see:

[Defense in Depth](defense-in-depth.md)

---

# Security Mindset

Effective cybersecurity is not about eliminating every possible threat.

Organizations have limited resources and must prioritize security decisions based on:

- Business value
- Asset importance
- Likelihood
- Potential impact
- Exposure
- Legal and regulatory requirements
- Operational requirements

The goal is to reduce risk to an acceptable level while allowing the organization to continue operating.

---

# SOC Perspective

These concepts appear constantly in SOC work.

When an alert is generated, an analyst may need to determine:

### What asset is involved?

```text
User account
Server
Endpoint
Application
Cloud resource
Network device
```

### What threat or suspicious activity is involved?

```text
Malware
Credential attack
Phishing
Unauthorized access
Suspicious network activity
```

### Is there a vulnerability or weakness involved?

```text
Unpatched software
Weak credentials
Misconfiguration
Excessive permissions
```

### What evidence exists?

```text
Logs
Authentication events
Endpoint telemetry
Network traffic
Process activity
Security alerts
```

### What could be the impact?

```text
Confidentiality
Integrity
Availability
Business operations
Customer data
Financial loss
```

This allows analysts to move beyond:

> "An alert fired."

and toward:

> "What happened, what is affected, how serious could it be, and what should happen next?"

---

# Interview Notes

### What are the three components of the CIA Triad?

**Confidentiality, Integrity, and Availability.**

### What is confidentiality?

Protecting information from unauthorized access or disclosure.

### What is integrity?

Ensuring information remains accurate, complete, and protected from unauthorized modification.

### What is availability?

Ensuring systems, services, and information remain accessible when authorized users need them.

### What is a threat?

Anything capable of causing harm to an asset, system, network, or organization.

### What is a vulnerability?

A weakness that could be exploited or abused.

### What is an exploit?

A technique, tool, or code used to take advantage of a vulnerability.

### What is risk?

The potential for loss or adverse impact based on factors such as likelihood and impact.

### Does a vulnerability automatically mean high risk?

No. Risk depends on context, including exposure, likelihood, asset value, and potential impact.

### What is Defense in Depth?

Using multiple layers of security controls so that failure of one control does not automatically result in complete compromise.

---

# Key Takeaways

- **Confidentiality** protects information from unauthorized disclosure.
- **Integrity** protects information from unauthorized modification.
- **Availability** keeps systems and information accessible when needed.
- An **asset** is something valuable that needs protection.
- A **threat** is capable of causing harm.
- A **vulnerability** is a weakness.
- An **exploit** is a method used to abuse a vulnerability.
- An **attack** is an attempt to compromise a system, account, network, or data.
- **Risk exists before successful exploitation.**
- Risk depends on factors such as likelihood, impact, asset value, and exposure.
- A vulnerability does **not** automatically mean that risk is high.
- Organizations can **mitigate, transfer, avoid, or accept** risk.
- **Defense in Depth** reduces dependence on any single security control.
- SOC analysts use these concepts to understand alerts, assess potential impact, and prioritize investigations.

---

# Personal Notes

- I understand the concepts well but sometimes mix up the terminology.
- I should focus on using the correct security terms consistently.
- Think of the chain: Asset → Threat → Vulnerability → Exploit → Risk → Impact

---

# Related Notes

- [Authentication & Authorization](authentication-and-authorization.md)
- [Identity and Access Management (IAM)](identity-and-access-management.md)
- [Identity Attacks](identity-attacks.md)
- [Defense in Depth](defense-in-depth.md)
- [Risk Management](risk-management.md)
- [Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)
- [Threat Actors & Cyber Warfare](threat-actors-and-cyber-warfare.md)
- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
- [Incident Response](incident-response.md)
- [Network Security](network-security.md)
- [Malware & Social Engineering](malware-and-social-engineering.md)
- [What Cybersecurity Actually Is](what-cybersecurity-actually-is.md)
