# Cybersecurity Glossary

> A living glossary containing cybersecurity terms learned throughout my SOC Analyst roadmap.

---

# A

## Access Control

The mechanism that enforces [authorization](#authorization) decisions by allowing or denying access to resources.

> [!TIP]
> **Think of it as:** The security guard that carries out the decision.

**See also:** [Identity and Access Management (IAM)](notes/security-fundamentals/identity-and-access-management.md)

---

## APT (Advanced Persistent Threat)

A sophisticated attacker — often a nation-state or well-funded group — that gains long-term unauthorized access to a network while actively avoiding detection.

> [!TIP]
> **Think of it as:** A spy who moves in and stays hidden for months, not a thief who smashes a window and runs.

**Examples**

- Nation-state espionage campaigns
- Long-term supply-chain compromises
- Multi-stage intrusions that persist for months or years

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## Attack Surface

All possible entry points an attacker can target to gain access to a system or network.

> [!TIP]
> **Think of it as:** Every door, window, and vent into a building — the more there are, the more there is to guard.

**Examples**

- Open network ports
- Public-facing web applications
- Exposed APIs
- Employee email accounts
- IoT devices

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

## Authentication

The process of verifying that an identity is who it claims to be.

> [!NOTE]
> **Question answered:** Who are you?

**Examples**

- Password
- PIN
- Fingerprint
- Face Recognition
- Security Key
- One-Time Password (OTP)

**See also:** [Authentication & Authorization](notes/security-fundamentals/authentication-and-authorization.md)

---

## Authorization

The process of determining what an authenticated identity is allowed to access or perform.

> [!NOTE]
> **Question answered:** What are you allowed to do?

**See also:** [Authentication & Authorization](notes/security-fundamentals/authentication-and-authorization.md)

---

# B

## Backdoor

A hidden method of bypassing normal authentication to maintain unauthorized access to a system.

> [!TIP]
> **Think of it as:** A spare key hidden under the mat that only the attacker knows about.

> [!NOTE]
> **Often follows:** An initial [Exploit](#exploit) or [Trojan](#trojan) infection, giving the attacker a way back in later.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Bot

A compromised device that is remotely controlled by an attacker, usually without the owner's knowledge.

**Examples**

- Infected personal computers
- Compromised IoT cameras and routers

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Botnet

A collection of [Bots](#bot) controlled by a single attacker, typically through a command-and-control (C2) server.

**Used for**

- [DDoS attacks](#ddos-distributed-denial-of-service)
- Spam campaigns
- [Cryptojacking](#cryptojacking)

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Brute Force Attack

An attack that tries every possible password against a single account until the correct one is found.

> [!NOTE]
> **Compare to Password Spraying:** Brute Force hits *one account* with *many passwords* (risking lockout); [Password Spraying](#password-spraying) hits *many accounts* with *one password* (avoiding lockout).

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

# C

## CIA Triad

The three fundamental principles of information security.

- **Confidentiality** – Prevent unauthorized disclosure of information.
- **Integrity** – Ensure information is accurate and cannot be altered without authorization.
- **Availability** – Ensure systems and data remain accessible when needed.

**See also:** [CIA Triad and Basic Security Concepts](notes/security-fundamentals/cia-triad-and-basic-security-concepts.md)

---

## Credential Stuffing

An attack that uses usernames and passwords leaked from previous data breaches to gain unauthorized access.

> [!NOTE]
> **Works because:** Many users reuse passwords across multiple websites.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## Cryptocurrency

Digital currency secured using cryptography.

> [!NOTE]
> **Why it matters here:** It's the usual payment demanded by [Ransomware](#ransomware) and the resource stolen by [Cryptojacking](#cryptojacking), since it's hard to trace.

**See also:** [Cryptocurrency & Cryptojacking](notes/security-fundamentals/cryptocurrency-and-cryptojacking.md)

---

## Cryptojacking

Unauthorized use of someone else's computing resources to mine cryptocurrency.

> [!TIP]
> **Think of it as:** Someone secretly plugging their generator into your outlet and running up your electric bill.

**See also:** [Cryptocurrency & Cryptojacking](notes/security-fundamentals/cryptocurrency-and-cryptojacking.md)

---

## CVE (Common Vulnerabilities and Exposures)

A unique identifier assigned to a publicly disclosed vulnerability.

> [!TIP]
> **Think of it as:** A vulnerability's official case number — e.g., CVE-2021-44228 (Log4Shell).

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## CVSS (Common Vulnerability Scoring System)

A standardized system for rating vulnerability severity on a scale of 0–10.

> [!NOTE]
> **Used with CVE to:** Decide which vulnerabilities to patch first.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## Cyberattack

An attempt to compromise a system, network, or data.

---

## Cybercrime

Illegal activities performed using computers or networks.

---

## Cybersecurity

The practice of protecting digital assets from cyber threats.

**See also:** [What Cybersecurity Actually Is](notes/security-fundamentals/what-cybersecurity-actually-is.md), [Cybersecurity Fundamentals](notes/security-fundamentals/cybersecurity-fundamentals.md)

---

# D

## Data Breach

Unauthorized access, disclosure, modification, or destruction of protected information.

> [!NOTE]
> **Often the final stage of:** The [Identity Attack Flow](#identity-attack-flow) (see Revision Notes below).

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## Dictionary Attack

A password attack that uses a list of common words and passwords rather than every possible combination.

> [!TIP]
> **Think of it as:** A focused subtype of [Brute Force](#brute-force-attack) — trying the likely words first instead of every combination.

---

## DDoS (Distributed Denial of Service)

A DoS attack launched from many compromised systems simultaneously, usually a [Botnet](#botnet).

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

## DoS (Denial of Service)

An attack intended to make a service unavailable.

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

# E

## Exploit

A technique, script, or piece of code that takes advantage of a [Vulnerability](#vulnerability) to perform unauthorized actions.

> [!NOTE]
> **Related:** A [Zero-Day](#zero-day) exploit targets a vulnerability with no patch available yet.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

# H

## Hacktivist

An attacker motivated by political or ideological beliefs rather than money.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

# I

## IAM (Identity and Access Management)

A framework used to manage digital identities and control access to organizational resources.

**See also:** [Identity and Access Management (IAM)](notes/security-fundamentals/identity-and-access-management.md)

---

## Identity

A digital representation of a user, device, application, or service that can authenticate to a system.

**Examples**

- User account
- Service account
- Application
- Server
- Virtual machine

---

## Identity Theft

Unauthorized use of another person's identity.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## Insider Threat

A threat originating from someone with legitimate organizational access.

> [!TIP]
> **Think of it as:** The danger isn't someone breaking in — they already have a key.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## Integrity

Ensuring information remains accurate and unaltered.

> [!NOTE]
> **Note:** One of the three pillars of the [CIA Triad](#cia-triad) — see that entry for the full picture alongside Confidentiality and Availability.

**See also:** [CIA Triad and Basic Security Concepts](notes/security-fundamentals/cia-triad-and-basic-security-concepts.md)

---

## IoT (Internet of Things)

Internet-connected physical devices such as smart cameras, TVs, and appliances.

> [!NOTE]
> **Watch out:** Weak default passwords on IoT devices make them a favorite target for recruiting [Bots](#bot) into a [Botnet](#botnet).

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

# K

## Keylogger

Spyware that records every keystroke entered by a user.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# L

## Least Privilege (PoLP)

A security principle stating that users should receive only the minimum permissions required to perform their job.

**Benefits**

- Reduces attack surface
- Limits damage after compromise
- Prevents privilege abuse

---

# M

## Malware

Software designed to harm, exploit, or gain unauthorized access to systems. An umbrella term covering several specific types below.

**Examples**

- [Virus](#virus)
- [Worm](#worm)
- [Trojan](#trojan)
- [Ransomware](#ransomware)
- [Spyware](#spyware)
- [Rootkit](#rootkit)

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Meltdown

A CPU vulnerability allowing unauthorized access to protected memory.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## MFA (Multi-Factor Authentication)

An authentication method requiring two or more authentication factors.

**Authentication factors include:**

- Something you know
- Something you have
- Something you are

**See also:** [Authentication & Authorization](notes/security-fundamentals/authentication-and-authorization.md)

---

## MitMo (Man-in-the-Mobile)

A mobile-focused attack that intercepts authentication codes or banking communications.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## MITM / On-Path Attack

An attack where the attacker secretly intercepts communication between two parties.

> [!TIP]
> **Think of it as:** Someone tapping the phone line between you and your bank.

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

# P

## Password Spraying

An attack that attempts one commonly used password against many different user accounts.

> [!NOTE]
> **Unlike Brute Force:** It spreads attempts across many accounts instead of hammering one, which helps avoid account lockouts.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## Phishing

A social engineering attack that uses fraudulent messages to trick users into revealing sensitive information — such as usernames, passwords, or MFA codes — or to deliver malware.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Pretexting

Creating a fabricated scenario to convince a victim to reveal information.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# Q

## Quid Pro Quo

Offering something in exchange for sensitive information.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# R

## Ransomware

Malware that encrypts a victim's files and demands payment — usually in [Cryptocurrency](#cryptocurrency) — for the decryption key.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Risk

The likelihood that a threat will exploit a vulnerability, combined with the potential impact if it occurs.

**See also:** [Risk Management](notes/security-fundamentals/risk-management.md)

---

## Rootkit

Malware designed to hide itself or other malicious software on a system.

> [!NOTE]
> **Often paired with:** A [Backdoor](#backdoor), so the attacker can keep returning undetected.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# S

## SEO Poisoning

Manipulating search engine results to direct victims to malicious websites.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Session Hijacking

An attack where an attacker steals an authenticated session token or cookie to impersonate a legitimate user.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## SIEM

**Security Information and Event Management**

A platform that collects, analyzes, correlates, and monitors security logs from multiple sources to detect suspicious activity.

---

## Smishing

Phishing conducted through SMS messages.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Social Engineering

Manipulating people into revealing information or performing insecure actions, rather than attacking systems directly.

**Umbrella term covering:**

- [Phishing](#phishing)
- [Vishing](#vishing)
- [Smishing](#smishing)
- [Whaling](#whaling)
- [Pretexting](#pretexting)
- [Quid Pro Quo](#quid-pro-quo)
- [SEO Poisoning](#seo-poisoning)

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Spectre

A CPU vulnerability exploiting speculative execution to leak sensitive information.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## Spyware

Malware that secretly collects user information.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Stuxnet

A sophisticated worm that targeted Iranian industrial control systems — widely considered one of the first cyberweapons used in the real world.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

# T

## Threat

Anything capable of exploiting a vulnerability and causing harm to a system, network, or organization.

**Examples**

- Hacker
- [Malware](#malware)
- [Insider Threat](#insider-threat)
- Natural Disaster

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## Threat Actor

An individual or group capable of carrying out cyberattacks.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## Trojan

Malware disguised as legitimate software, tricking the user into installing it themselves.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# V

## Virus

Malware that attaches itself to another file and requires user execution to spread.

> [!NOTE]
> **Compare to Worm:** A Virus needs a human to run the infected file; a [Worm](#worm) spreads on its own.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Vishing

Phishing conducted through voice calls.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Vulnerability

A weakness in a system, application, process, or configuration that can be exploited by a threat.

**Examples**

- Unpatched software
- Weak password
- Misconfigured firewall

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

# W

## Whaling

A phishing attack that specifically targets executives.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Worm

Self-replicating malware that spreads automatically without requiring user action.

> [!NOTE]
> **Compare to Virus:** A Worm doesn't need a human to run it — it spreads on its own across a network, unlike a [Virus](#virus).

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# Z

## Zero Trust

A security model based on the principle:

> [!IMPORTANT]
> **Zero Trust principle:** Never Trust. Always Verify.

Every user, device, and access request must be verified before access is granted.

**See also:** [Defense in Depth](notes/security-fundamentals/defense-in-depth.md)

---

## Zero-Day

A vulnerability with no available security patch at the time it is discovered or exploited.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

# Revision Notes

## Authentication vs Authorization

[Authentication](#authentication) = **Who are you?**

[Authorization](#authorization) = **What can you do?**

[Access Control](#access-control) = **Enforces the authorization decision.**

---

## Virus vs Worm vs Trojan (quick compare)

| Type | Spreads how | Needs user action? |
|---|---|---|
| [Virus](#virus) | Attaches to a file | Yes — user must run it |
| [Worm](#worm) | Self-replicates across a network | No |
| [Trojan](#trojan) | Disguised as legitimate software | Yes — user installs it |

---

## Identity Attack Flow

[Identity](#identity)

↓

Credential Theft

↓

[Authentication](#authentication)

↓

[Authorization](#authorization)

↓

Access Granted

↓

Potential [Data Breach](#data-breach)

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md), [Incident Response](notes/security-fundamentals/incident-response.md)
