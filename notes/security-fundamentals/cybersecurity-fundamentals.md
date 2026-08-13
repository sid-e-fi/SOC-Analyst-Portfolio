---
type: concept-note
status: reviewed
tags:
  - fundamentals
  - identity
  - data-protection
---

# Cybersecurity Fundamentals

> **Purpose**
>
> This note serves as a high-level foundation for understanding cybersecurity, the assets organizations protect, the information they handle, common security concerns, and the role of cybersecurity in protecting business operations.
>
> Detailed concepts such as the CIA Triad, identity attacks, vulnerabilities, and threat actors are maintained in their dedicated notes and linked where relevant. For the people/process/technology model, the security lifecycle, and the SOC's operational role, see [What Cybersecurity Actually Is](what-cybersecurity-actually-is.md).

---

# What Is Cybersecurity?

Cybersecurity is the practice of protecting digital assets from unauthorized access, misuse, modification, disruption, and destruction.

Digital assets include:

- Computers
- Servers
- Mobile devices
- Networks
- Applications
- Cloud services
- Data
- Digital identities

Cybersecurity is not about making systems impossible to attack. Instead, it focuses on **reducing risk** through prevention, detection, response, and recovery.

See:

[What Cybersecurity Actually Is](what-cybersecurity-actually-is.md)

---

# Why Cybersecurity Matters

Modern organizations rely heavily on digital systems to conduct business. A successful cyberattack can affect not only technology but also the organization's ability to operate.

Potential consequences include:

- Financial loss
- Operational disruption
- Reputational damage
- Legal and regulatory penalties
- Loss of customer trust
- Theft of sensitive information

Cybersecurity therefore protects both **technology and business operations**.

---

# Core Security Objectives

The **CIA Triad** provides three fundamental security objectives:

- **Confidentiality**
- **Integrity**
- **Availability**

These objectives help analysts understand what security property may have been affected during an incident.

For detailed definitions, examples, and security controls, see:

[CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)

---

# Personal Data

**Personal data** is information that can identify or relate to an individual.

Examples include:

- Name
- Phone number
- Email address
- Government identification
- Financial information
- Medical records

Organizations must protect personal information from unauthorized access, disclosure, modification, or misuse.

---

# Online Identity

An **online identity** is the digital representation of an individual across online services and systems.

It can be associated with information such as:

- Usernames
- Email addresses
- Browsing history
- Search history
- Cookies
- IP addresses
- Device information
- Social media activity

Individual pieces of information may appear insignificant, but combining multiple pieces can reveal significant information about a person.

Identity is therefore an important security asset.

See:

[Identity and Access Management (IAM)](identity-and-access-management.md)

[Identity Attacks](identity-attacks.md)

---

# Organizational Data

Organizations store different types of information depending on their business requirements.

Examples include:

- Customer information
- Financial records
- Employee information
- Source code
- Research data
- Intellectual property
- Business records

The sensitivity, value, and business importance of data influence the controls required to protect it.

---

# Data Classification

Organizations can classify information according to its sensitivity and required level of protection.

| Classification | Description | Example |
|---|---|---|
| Public | Intended for public access | Company website |
| Internal | Intended for authorized employees or internal use | Internal documentation |
| Confidential | Sensitive information requiring restricted access | Customer database |
| Restricted | Highly sensitive information requiring strong protection | Encryption keys, administrator credentials |

Exact classification names and handling requirements vary between organizations.

The purpose of classification is to ensure that sensitive information receives appropriate protection.

---

# Data Breaches

A **data breach** occurs when protected information is accessed, disclosed, modified, or destroyed without authorization.

Common causes can include:

- Phishing
- Malware
- Insider threats
- Weak passwords
- Cloud misconfiguration
- Unpatched systems

A data breach can affect:

- Confidentiality
- Integrity
- Availability

depending on what occurred.

---

# Identity Theft

**Identity theft** occurs when someone illegally uses another person's identity or identity-related information.

Examples include:

- Account takeover
- Banking fraud
- Fraudulent applications
- Email compromise
- Unauthorized use of personal information

Identity theft is particularly dangerous because attackers may use legitimate-looking identities or compromised accounts to conduct further activity.

See:

[Identity Attacks](identity-attacks.md)

---

# Internet of Things (IoT)

The **Internet of Things (IoT)** refers to physical devices connected to networks or the internet that can collect, process, or exchange data.

Examples include:

- Smart TVs
- Security cameras
- Smart speakers
- Smart watches
- Smart home devices

Common security issues include:

- Default or weak passwords
- Poor security configurations
- Limited security updates
- Outdated firmware
- Exposed network services

IoT devices can increase an organization's **attack surface** because each connected device can introduce additional software, hardware, credentials, and network connections that require protection.

---

# Why Attackers Attack

Attackers can have different motivations.

Common motivations include:

- Financial gain
- Espionage
- Political or ideological goals
- Personal revenge
- Curiosity
- Reputation or fame

Understanding motivation can help analysts develop context around an attack and assess potential objectives.

However, motivation alone is not enough to attribute an attack to a specific threat actor. Analysts should rely on evidence.

See:

[Threat Actors & Cyber Warfare](threat-actors-and-cyber-warfare.md)

---

# Organizational Security Mindset

Organizations protect information and systems according to factors such as:

- Business value
- Operational importance
- Legal requirements
- Customer trust
- Risk
- Security requirements

Cybersecurity decisions are therefore not based on eliminating every possible threat.

Instead, organizations prioritize resources to **reduce risk to an acceptable level while maintaining business operations**.

---

# Cybersecurity and Business Risk

Cybersecurity decisions must account for business context.

A security issue affecting a critical production server may require a different priority from the same technical issue affecting an isolated test system.

Important factors include:

- Asset value
- Exposure
- Likelihood
- Potential impact
- Business importance
- Available security controls

See:

[Risk Management](risk-management.md)

[Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)

---

# Interview Notes

### What is cybersecurity?

Cybersecurity is the practice of protecting digital assets from unauthorized access, misuse, modification, disruption, and destruction.

### Why is cybersecurity important to a business?

A cyberattack can cause financial loss, operational disruption, reputational damage, legal consequences, loss of customer trust, and theft of sensitive information.

### What is the CIA Triad?

Confidentiality, Integrity, and Availability. These represent three fundamental security objectives.

### What is an attack surface?

An attack surface is the collection of systems, devices, applications, accounts, services, and other points through which an organization could potentially be attacked.

### Why is identity important in cybersecurity?

Compromised identities can provide attackers with legitimate-looking access to systems and resources, making identity protection and monitoring important parts of modern security.

### What does a SOC do?

A SOC monitors security activity, investigates alerts, collects evidence, assesses potential impact, responds to threats, and escalates incidents when necessary. See [What Cybersecurity Actually Is](what-cybersecurity-actually-is.md) for the full SOC analyst mental model.

---

# Key Takeaways

- Cybersecurity protects digital assets, people, systems, and business operations.
- Cybersecurity focuses on **reducing risk**, not achieving absolute security.
- The CIA Triad provides the fundamental objectives of confidentiality, integrity, and availability.
- Personal data and digital identities are important security assets.
- Organizations classify data according to sensitivity and business requirements.
- Data breaches can result from technical weaknesses, human actions, or both.
- IoT devices can increase an organization's attack surface.
- Attackers may have financial, political, ideological, personal, or other motivations.
- Security decisions should be based on both technical evidence and business context.
- For the people/process/technology model, the security lifecycle, and the SOC's operational role, see [What Cybersecurity Actually Is](what-cybersecurity-actually-is.md).

---

# Related Notes

- [What Cybersecurity Actually Is](what-cybersecurity-actually-is.md)
- [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)
- [Authentication & Authorization](authentication-and-authorization.md)
- [Identity and Access Management (IAM)](identity-and-access-management.md)
- [Identity Attacks](identity-attacks.md)
- [Risk Management](risk-management.md)
- [Threat Actors & Cyber Warfare](threat-actors-and-cyber-warfare.md)
- [Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)
- [Defense in Depth](defense-in-depth.md)
- [Incident Response](incident-response.md)
- [Network Security](network-security.md)
