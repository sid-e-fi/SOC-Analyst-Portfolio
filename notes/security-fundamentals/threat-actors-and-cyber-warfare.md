---
type: concept-note
status: reviewed
tags:
  - threat-actors
  - cyber-warfare
  - attribution
---

# Threat Actors & Cyber Warfare

> **Purpose**
>
> This note explains who performs cyber attacks, why they do it, what resources and capabilities they may have, and how those differences affect attacker behavior and risk.
>
> For a SOC analyst, understanding threat actors provides context for investigations, but attribution must be based on evidence rather than assumptions.

---

# Core Concepts

## Cyberattack

A **cyberattack** is a deliberate attempt to compromise the confidentiality, integrity, or availability of a system, network, account, or data.

This note uses "cyberattack" interchangeably with "attack" as defined in [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md).

Examples include:

- Phishing
- Malware
- DDoS
- Password attacks
- Exploitation of vulnerabilities
- SQL injection

A cyberattack may succeed or fail.

---

## Cybercrime

**Cybercrime** refers to illegal activities carried out using computers, networks, or the internet.

Examples include:

- Ransomware
- Identity theft
- Banking fraud
- Credit card theft
- Cryptocurrency theft
- Unauthorized access

Financial gain is a common motivation for cybercriminals.

### Cyberattack vs Cybercrime

These terms are related but not identical.

> **Cyberattack** describes an attempt to compromise a system, network, account, or data.

> **Cybercrime** describes illegal activity involving computers, networks, or the internet.

Therefore, not every cyberattack is necessarily classified as traditional cybercrime. For example, government-sponsored cyber operations may be conducted for intelligence or strategic purposes rather than financial gain.

---

# Threat Actor

A **threat actor** is an individual, group, or organization capable of causing harm to an information system or its assets.

Threat actors can differ significantly in:

- Motivation
- Skill
- Resources
- Access
- Persistence
- Objectives

These characteristics influence how an attacker operates and what they are likely to target.

---

# Threat Actor Motivation

Common motivations include:

- Financial gain
- Espionage
- Intelligence gathering
- Political objectives
- Ideological beliefs
- Military advantage
- Sabotage
- Revenge
- Curiosity
- Reputation

Motivation provides useful context, but it does not prove attribution.

For example:

> A ransomware attack may suggest financial motivation, but the analyst should still rely on evidence to determine who actually conducted the attack.

---

# Threat Actor Types

## Cybercriminals

### Primary Motivation

**Financial gain.**

### Common Activities

- Ransomware
- Banking malware
- Credential theft
- Financial fraud
- Phishing
- Cryptocurrency theft
- Account takeover

Cybercriminals are among the threat actors most frequently encountered by enterprise security teams.

See:

[Malware & Social Engineering](malware-and-social-engineering.md)

[Identity Attacks](identity-attacks.md)

---

## Nation-State Actors

**Nation-state actors** are government-sponsored or government-directed groups conducting cyber operations in support of national interests.

### Common Objectives

- Espionage
- Intelligence gathering
- Military advantage
- Strategic information collection
- Political objectives
- Critical infrastructure disruption

Nation-state operations can involve significant resources, specialized capabilities, and long-term campaigns.

### Important

Not every sophisticated attack should automatically be attributed to a nation-state.

Attribution requires evidence.

---

## Hacktivists

**Hacktivists** are individuals or groups motivated primarily by political, social, or ideological beliefs.

Common activities include:

- Website defacement
- Information leaks
- DDoS
- Public disclosure of stolen information

Financial gain is generally not their primary objective.

---

## Insider Threats

An **insider threat** originates from someone who has legitimate access to an organization's systems, facilities, or information.

Potential insiders include:

- Employees
- Contractors
- Administrators
- Third-party vendors

Insider threats can be either intentional or accidental.

### Malicious Insider

A malicious insider intentionally abuses legitimate access.

Example:

```text
Employee
   ↓
Legitimate Access
   ↓
Unauthorized Data Collection
   ↓
Data Theft
```

### Accidental Insider

An employee unintentionally causes a security problem.

Examples:

- Sending sensitive data to the wrong recipient
- Misconfiguring a cloud resource
- Clicking a malicious link
- Accidentally exposing credentials

### Why Insiders Are Difficult

Insiders may already possess:

- Valid credentials
- Authorized access
- Knowledge of internal systems
- Knowledge of business processes

This can make malicious insider activity difficult to distinguish from legitimate activity.

---

## Script Kiddies

**Script kiddies** are individuals with limited technical knowledge who rely heavily on publicly available tools, scripts, or exploit code.

They may have limited understanding of the underlying techniques.

However:

> **Low sophistication does not mean low risk.**

Poorly secured systems can still be significantly affected by attacks performed with publicly available tools.

---

# Hacker Hats

Hacker-hat classifications primarily describe **authorization and intent**, not technical skill.

---

## White Hat

A **white hat** is an authorized security professional who performs security testing or research with permission.

Common activities include:

- Penetration testing
- Vulnerability assessment
- Security research
- Red-team exercises

The defining characteristic is **authorization**.

---

## Black Hat

A **black hat** is an unauthorized attacker who compromises systems for malicious or unauthorized purposes.

Possible objectives include:

- Financial gain
- Data theft
- Espionage
- Sabotage
- Unauthorized access

---

## Gray Hat

A **gray hat** accesses systems without authorization but may not intend to cause harm.

For example, someone might discover and exploit a vulnerability without permission and later report it to the organization.

Even when the intent is helpful, unauthorized access remains unauthorized and may be unethical or illegal.

---

## Red Hat

**Red hat** is an informal and inconsistently defined term.

It is sometimes used to describe vigilante hackers who attempt to attack or disrupt other malicious hackers.

Because the term does not have a universally accepted professional definition, it is less useful than classifications such as white hat, black hat, or gray hat.

---

# Security Teams

Threat actors should not be confused with defensive security teams.

## Red Team

A **Red Team** consists of authorized security professionals who simulate attacks to test an organization's defenses.

Goals include:

- Identify weaknesses
- Test security controls
- Evaluate detection
- Test response capabilities

Red Team activity is authorized.

---

## Blue Team

A **Blue Team** is responsible for defending systems and detecting and responding to threats.

Typical activities include:

- Monitoring
- Detection
- Investigation
- Threat hunting
- Incident response
- Security improvement

A **SOC analyst is part of the defensive security function and commonly operates within the Blue Team**.

---

## Purple Team

A **Purple Team** represents collaboration between offensive and defensive security functions.

The objective is to use Red Team findings to improve Blue Team:

- Detection
- Monitoring
- Investigation
- Response

Purple Teaming is therefore less about being a separate "team" and more about improving collaboration and defensive capability.

---

# Internal vs External Threats

## Internal Threat

An internal threat originates from someone with legitimate access to the organization's environment.

Advantages an insider may have include:

- Existing credentials
- Existing permissions
- Knowledge of internal systems
- Familiarity with security procedures

---

## External Threat

An external threat originates outside the organization.

Examples include:

- Cybercriminals
- Nation-state groups
- Hacktivists
- External attackers using compromised infrastructure

External attackers generally need to obtain initial access before reaching internal resources.

---

# Threat Actor Capability

Threat actors can differ in capability.

A useful conceptual model is:

```text
Low Capability
      ↓
Publicly Available Tools
      ↓
Moderate Capability
      ↓
Custom Tools / Specialized Skills
      ↓
High Capability
      ↓
Significant Resources + Advanced Techniques
```

Capability does not automatically determine impact.

A low-skilled attacker using an automated exploit against a poorly secured system can sometimes cause more damage than a highly capable attacker who never gains access.

---

# Threat Actor Objectives

The same technique can be used for different objectives.

For example:

```text
Phishing
   ↓
Credential Theft
   ├── Financial Fraud
   ├── Account Takeover
   ├── Espionage
   └── Initial Access for Further Attack
```

Therefore, SOC analysts should investigate both:

> **How was the attack performed?**

and:

> **What was the attacker trying to achieve?**

---

# Cyber Warfare

## Definition

**Cyber Warfare** involves cyber operations conducted by or on behalf of governments to achieve strategic objectives.

These operations may support:

- National security
- Intelligence gathering
- Military objectives
- Political objectives
- Strategic disruption

Cyber Warfare can target both digital systems and physical processes controlled by digital technology.

---

# Common Cyber Warfare Targets

Potential targets include:

- Power grids
- Water treatment facilities
- Telecommunications
- Banking infrastructure
- Transportation systems
- Government systems
- Military systems
- Industrial control systems

Attacks against critical infrastructure can create consequences beyond traditional data theft.

---

# Cyber Warfare vs Cybercrime

| Cybercrime | Cyber Warfare |
|---|---|
| Usually financially motivated | Usually strategically motivated |
| Often targets individuals and businesses | May target governments, military systems, or critical infrastructure |
| Ransomware and fraud are common examples | Espionage and strategic disruption are common objectives |
| Criminal organizations are common actors | Government-sponsored actors are commonly associated |
| Primary objective is often profit | Primary objective is often national or strategic interest |

These categories can overlap in practice.

For example, criminal infrastructure or tools may be used in campaigns with strategic objectives.

---

# Stuxnet

**Stuxnet** was a sophisticated computer worm discovered in 2010.

It targeted industrial control systems associated with Iran's nuclear enrichment program.

### Why Stuxnet Was Significant

- It targeted industrial systems rather than only conventional IT systems.
- It demonstrated that malware could affect physical industrial equipment.
- It reportedly caused physical damage to uranium centrifuges.
- It demonstrated how cyber operations can produce physical-world consequences.
- It became a major historical example in discussions of cyber warfare.

---

# Advanced Persistent Threat (APT)

## Definition

An **Advanced Persistent Threat (APT)** describes a sophisticated and persistent intrusion in which an attacker gains unauthorized access and attempts to maintain that access over an extended period while avoiding detection.

### Characteristics

- Long-term persistence
- Stealth
- Multiple attack techniques
- High-value targets
- Continued access
- Deliberate attacker operations

APTs are often associated with nation-state or highly capable threat actors, but **APT should describe observed characteristics of an intrusion, not be used as an automatic attribution label**.

---

# Persistence and Threat Actors

A persistent attacker may attempt to maintain access through mechanisms such as:

- Malicious accounts
- Scheduled tasks
- Services
- Registry modifications
- Backdoors
- Stolen session tokens
- Other persistence mechanisms

SOC analysts should investigate persistence because removing the initial malware or closing the initial access path may not be sufficient.

See:

[Malware & Social Engineering](malware-and-social-engineering.md)

[Incident Response](incident-response.md)

---

# Command and Control

**Command and Control (C2)** refers to the communication mechanisms attackers use to control compromised systems or send instructions to them.

A compromised endpoint may communicate with attacker-controlled infrastructure to:

- Receive commands
- Download additional malware
- Send collected information
- Maintain attacker access

Example:

```text
Compromised Endpoint
        ↓
Outbound C2 Connection
        ↓
Attacker Infrastructure
        ↓
Commands / Payloads
        ↓
Compromised Endpoint
```

C2 activity can be an important detection opportunity for a SOC.

---

# Threat Actor Behavior and SOC Investigations

Understanding the likely threat actor can provide useful context, but analysts should begin with evidence.

A SOC analyst may ask:

### Why was this organization targeted?

Consider:

- Industry
- Asset value
- Data sensitivity
- Political relevance
- Financial opportunity

### What is the likely objective?

Possible objectives include:

- Credential theft
- Financial gain
- Data theft
- Espionage
- Disruption
- Persistence
- Initial access for a later attack

### Which systems are most at risk?

Consider:

- Internet-facing systems
- Privileged accounts
- Critical infrastructure
- Sensitive databases
- High-value endpoints

### How capable does the activity appear?

Consider:

- Tools used
- Exploitation techniques
- Persistence
- Operational discipline
- Ability to evade detection

### What is likely to happen next?

Look for evidence of:

- Credential theft
- Privilege escalation
- Discovery
- Lateral movement
- Data collection
- Exfiltration
- Persistence

---

# Attribution

## What Is Attribution?

**Attribution** is the process of determining who is responsible for a cyberattack or intrusion.

Attribution is difficult because attackers can:

- Use compromised infrastructure
- Use public tools
- Reuse malware
- Spoof identities
- Route traffic through other systems
- Copy techniques used by other groups

Therefore:

> **A technique, IP address, malware family, or domain alone is rarely enough to confidently attribute an attack to a specific threat actor.**

---

# Evidence Before Attribution

SOC analysts should distinguish:

### Observed Evidence

> "The endpoint executed PowerShell and connected to an external IP."

from:

### Attribution Claim

> "This was definitely performed by Threat Actor X."

The first statement can be supported directly by telemetry.

The second requires significantly more evidence.

This principle is especially important because attackers may intentionally imitate the tools or techniques of other groups.

---

# Threat Intelligence Context

Threat intelligence can provide additional context about:

- Known threat actors
- Known infrastructure
- Malware families
- Attack techniques
- Campaigns
- Indicators of compromise

However, intelligence should be treated as **contextual evidence**, not unquestionable proof.

A known malicious IP may be useful evidence, but the analyst should still establish what the affected host actually did.

This becomes particularly important during alert triage.

---

# SOC Investigation Example

Suppose a workstation connects to infrastructure associated with a known threat actor.

A weak investigation would conclude:

> "The machine is compromised by that threat actor."

A stronger investigation would examine:

```text
Known / Suspicious Destination
        ↓
Identify Source Host
        ↓
Identify User
        ↓
Review DNS Activity
        ↓
Review Network Connections
        ↓
Inspect Endpoint Processes
        ↓
Check File Activity
        ↓
Check Persistence
        ↓
Review Authentication Activity
        ↓
Determine What Actually Happened
```

The threat intelligence indicator provides a reason to investigate.

It does not replace the investigation.

---

# Threat Actors and Risk Management

Threat actor characteristics can influence risk assessment.

Consider:

```text
Threat Actor
     +
Motivation
     +
Capability
     +
Target
     +
Exposure
     +
Potential Impact
     ↓
Risk Context
```

For example, an internet-facing critical infrastructure system targeted by a capable actor with a clear strategic objective may require a much higher priority than an isolated test system receiving generic automated scans.

See:

[Risk Management](risk-management.md)

---

# Threat Actors and Incident Response

Threat actor information can help incident responders anticipate possible attacker behavior.

For example:

```text
Initial Access
      ↓
Credential Theft
      ↓
Privilege Escalation
      ↓
Lateral Movement
      ↓
Persistence
      ↓
Data Collection
      ↓
Exfiltration / Impact
```

However, analysts should not assume that every attacker will follow the same sequence.

The investigation should remain evidence-driven.

See:

[Incident Response](incident-response.md)

---

# Threat Actors and Malware

Malware can provide clues about attacker activity, but malware identification alone does not establish attribution.

For example:

```text
Malware Sample
      ↓
Family Identified
      ↓
Known Techniques
      ↓
Possible Threat Actor Associations
      ↓
Additional Evidence Required
      ↓
Attribution Assessment
```

Attackers can reuse malware, modify malware, or deliberately use tools associated with other groups.

See:

[Malware & Social Engineering](malware-and-social-engineering.md)

---

# Common Beginner Mistakes

### ❌ "A threat actor is always a hacker."

### ✔ Correct

A threat actor can be an individual, group, organization, or other entity capable of causing harm.

---

### ❌ "Nation-state attacks are always the most dangerous."

### ✔ Correct

Risk depends on the target, exposure, asset value, attacker capability, likelihood, and potential impact.

---

### ❌ "White hats are more skilled than black hats."

### ✔ Correct

White hat and black hat primarily describe authorization and intent, not skill level.

---

### ❌ "Every insider threat is malicious."

### ✔ Correct

Insider threats can be malicious or accidental.

---

### ❌ "APT means nation-state."

### ✔ Correct

APT describes a sophisticated, persistent intrusion. Nation-state groups are commonly associated with APT activity, but the term itself should not automatically be treated as attribution.

---

### ❌ "The IP belongs to a known threat actor, so the attack is confirmed."

### ✔ Correct

Threat intelligence provides context. The analyst still needs evidence showing what happened on the affected system.

---

### ❌ "A sophisticated attack must have been performed by a sophisticated threat actor."

### ✔ Correct

Attackers can use publicly available tools or compromised infrastructure to make an operation appear more sophisticated than the underlying actor actually is.

---

# Interview Notes

### What is a threat actor?

An individual, group, or organization capable of causing harm to an information system or its assets.

### What motivates cybercriminals?

Primarily financial gain, although individual criminal operations can have additional objectives.

### What is a nation-state actor?

A government-sponsored or government-directed group conducting cyber operations in support of national interests.

### What is a hacktivist?

An individual or group motivated primarily by political, social, or ideological beliefs.

### What is an insider threat?

A threat originating from someone with legitimate access to an organization's systems, information, or facilities. It can be malicious or accidental.

### What is a script kiddie?

An individual with limited technical knowledge who relies heavily on publicly available tools, scripts, or exploit code.

### What do white hat, black hat, and gray hat mean?

They primarily describe authorization and intent. White hats are authorized, black hats are unauthorized malicious attackers, and gray hats access systems without authorization but may not intend harm.

### What is the difference between Red Team and Blue Team?

Red Teams simulate attacks to test defenses. Blue Teams defend systems through monitoring, detection, investigation, and response.

### What is Purple Teaming?

Collaboration between offensive and defensive security functions to use attack findings to improve detection and defense.

### What is cyber warfare?

Cyber operations conducted by or on behalf of governments to achieve strategic objectives.

### What is an APT?

An Advanced Persistent Threat is a sophisticated and persistent intrusion in which an attacker maintains unauthorized access over an extended period while attempting to remain undetected.

### What is attribution?

Attribution is the process of determining who is responsible for a cyberattack or intrusion.

### Why is attribution difficult?

Attackers can use compromised infrastructure, public tools, copied techniques, spoofed identities, and other methods to hide or misdirect investigators.

### Can an IP address prove who attacked an organization?

No. An IP address is evidence about network activity, not automatically proof of the attacker's identity.

### Why should SOC analysts care about threat actors?

Threat actor context can help analysts understand likely objectives, prioritize investigations, assess risk, and anticipate possible follow-on activity.

---

# Key Takeaways

- **Threat actors differ in motivation, capability, resources, access, and objectives.**
- Common threat actors include cybercriminals, nation-state actors, hacktivists, insiders, and script kiddies.
- Cybercriminals commonly pursue financial gain.
- Nation-state actors commonly pursue strategic, intelligence, military, or political objectives.
- Hacktivists are primarily motivated by political, social, or ideological goals.
- Insider threats can be malicious or accidental.
- Script kiddies may have limited technical knowledge but can still cause significant damage.
- White hat, black hat, and gray hat primarily describe authorization and intent, not skill.
- Red Team performs authorized offensive testing.
- Blue Team performs defensive monitoring, detection, investigation, and response.
- Purple Teaming improves collaboration between offensive and defensive functions.
- Cyber Warfare involves government-linked cyber operations conducted for strategic objectives.
- Stuxnet demonstrated that cyber operations can produce physical-world consequences.
- **APT describes a sophisticated and persistent intrusion, not automatic attribution to a nation-state.**
- C2 infrastructure allows attackers to communicate with compromised systems.
- Threat intelligence provides useful context but should not replace direct investigation.
- **Attribution requires evidence.**
- A single IP address, malware family, technique, or domain is not sufficient proof of attribution.
- SOC analysts should use threat actor context to improve investigation and risk assessment while remaining evidence-driven.

---

# Related Notes

- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
- [Malware & Social Engineering](malware-and-social-engineering.md)
- [Identity Attacks](identity-attacks.md)
- [Identity and Access Management (IAM)](identity-and-access-management.md)
- [Network Security](network-security.md)
- [Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)
- [Risk Management](risk-management.md)
- [Defense in Depth](defense-in-depth.md)
- [Incident Response](incident-response.md)
