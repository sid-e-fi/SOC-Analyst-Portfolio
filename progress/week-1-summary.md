---
type: weekly-summary
status: reviewed
tags:
  - week-1
  - fundamentals
  - soc
  - cisco
  - iam
---

# Week 1 Summary: Security Basics + Lab Setup

> [!NOTE]
> **Purpose:** This note consolidates Week 1 (Days 1 to 6) of the SOC Analyst L1 roadmap: lab environment setup, core security fundamentals, malware and social engineering, identity and access management, and the full Cisco Introduction to Cybersecurity course. Raw daily logs remain in `11 Daily Logs/Week 1` for process history; this note is the reviewed, portfolio-facing record of what was actually learned and built.

**Dates covered:** 25 to 31 July 2026
**Time invested:** Roughly 2 hours theory plus documentation per day, 6 days

---

## What I Built

- SOC-L1 workspace with organized folders for ISOs, VMs, notes, reports, resources, screenshots, projects, and backups
- Obsidian vault structured for networking, Linux, Windows, SIEM, MITRE ATT&CK, incident reports, cheat sheets, and daily logs
- VMware Workstation Pro installed on an Ubuntu host
- Windows 11 VM (**WIN-LAB**) with VMware Tools, updated, renamed, on a local administrator account (kept separate from personal Microsoft account by design)
- First snapshot (**Fresh Install**) as a clean recovery baseline
- VM memory tuned down from a planned 8 GB to 4 GB after the host became sluggish, a small but real troubleshooting call

See [VMware Setup](../labs/week-1-lab-setup/vmware-setup.md) and [Windows 11 Lab](../labs/week-1-lab-setup/windows-11-lab.md) for the full setup notes.

---

## What I Learned

### Security Fundamentals
CIA triad (confidentiality, integrity, availability), asset, threat, vulnerability, exploit, risk, and impact. The main early confusion was risk versus vulnerability and exploit versus malware, resolved by treating them as stages rather than synonyms: a vulnerability is a weakness, an exploit is the method that takes advantage of it, and risk is the likelihood and impact of that happening.

See [CIA Triad and Basic Security Concepts](../notes/security-fundamentals/cia-triad-and-basic-security-concepts.md).

### Malware and Social Engineering
Malware families (virus, worm, trojan, ransomware, spyware, adware) and the difference between a payload and a delivery method. Social engineering types: phishing, spear phishing, whaling, smishing, vishing. Also covered endpoint concepts (antivirus, EDR, telemetry) and how logs, alerts, and incidents relate: logs are records, alerts are what a tool generates after analyzing logs, and an incident is a confirmed, validated event.

Worked a mini incident response scenario (PowerShell execution to encoded command to suspicious IP connection to credential dumping to new admin account) and practiced the reasoning: validate, contain, isolate, weigh business impact, then investigate.

See [Malware & Social Engineering](../notes/security-fundamentals/malware-and-social-engineering.md).

### Identity and Access Management
Digital identity, authentication versus authorization, access control, least privilege, MFA and its limits, and four access control models (DAC, MAC, RBAC, ABAC). Also covered identity attacks: password spraying, brute force, credential stuffing, phishing, and session hijacking, plus an introduction to Zero Trust ("never trust, always verify").

The real sticking point was authorization versus access control. Authorization decides what an identity is allowed to do; access control is the mechanism that enforces that decision.

See [Identity and Access Management (IAM)](../notes/security-fundamentals/identity-and-access-management.md), [Identity Attacks](../notes/security-fundamentals/identity-attacks.md), and [Authentication & Authorization](../notes/security-fundamentals/authentication-and-authorization.md).

### Cisco Introduction to Cybersecurity (Modules 1 to 5)
Completed the full course and earned the Cisco badge and certificate (Credly ID: `92efe065-193b-4477-a189-975a44d854ed`).

- **Module 1:** personal, organizational, and online identity data, data breaches, identity theft, threat actors, insider versus external threats, cyberwarfare
- **Module 2:** DoS/DDoS, botnets, on-path (MITM) attacks, SEO poisoning, Wi-Fi and password attacks, APTs, hardware and software vulnerabilities, CVE/CVSS, patch management, cryptocurrency and cryptojacking
- **Module 3:** device and wireless security, encryption, backups, secure deletion, 2FA, OAuth, browser and email privacy
- **Module 4:** firewalls, port scanning, IDS versus IPS, NetFlow, behavior-based detection, defense in depth, penetration testing, CSIRT, security playbooks
- **Module 5:** legal and ethical issues in cybersecurity, professional certifications, career pathways

See [Cisco Introduction to Cybersecurity - Course Notes](../notes/security-fundamentals/cisco-introduction-to-cybersecurity-course-notes.md), [Threat Actors & Cyber Warfare](../notes/security-fundamentals/threat-actors-and-cyber-warfare.md), [Vulnerabilities & Patch Management](../notes/security-fundamentals/vulnerabilities-and-patch-management.md), [Cryptocurrency & Cryptojacking](../notes/security-fundamentals/cryptocurrency-and-cryptojacking.md), [Defense in Depth](../notes/security-fundamentals/defense-in-depth.md), and [Network Security](../notes/security-fundamentals/network-security.md).

---

## Key Realizations

The biggest shift this week was connecting isolated terms into attack chains and workflows instead of memorizing definitions in a vacuum:

- Phishing to trojan to payload
- Logs to SIEM to alert to incident
- Vulnerability to exploit to compromise
- Endpoint to telemetry to EDR investigation

And a mindset point that came up repeatedly by Day 6: a SOC analyst does not label activity malicious on sight. A port scan, an unfamiliar login, or a large data transfer is a signal that needs context, evidence, and authorization checks before a disposition is made. Technical capability is not the same thing as authorization.

---

## Challenges Worked Through

| Confused | Resolved as |
|---|---|
| Risk vs vulnerability | Vulnerability is the weakness; risk is likelihood times impact |
| Virus vs worm | Virus needs user interaction; worm spreads on its own after initial infection |
| Trojan as a payload | Trojan is a delivery method through deception, not the payload itself |
| Logs vs alerts | Logs are raw records; alerts are generated after analysis |
| Authorization vs access control | Authorization decides what is allowed; access control enforces it |
| Firewall vs IDS vs IPS vs NetFlow | Firewalls enforce traffic policy, IDS detects, IPS can block, NetFlow gives flow metadata rather than packet contents |

---

## Credentials Earned

- **Cisco Introduction to Cybersecurity** certificate and Credly badge (ID: `92efe065-193b-4477-a189-975a44d854ed`), added to LinkedIn Licenses & Certifications

---

## Documentation Produced This Week

- [What Cybersecurity Actually Is](../notes/security-fundamentals/what-cybersecurity-actually-is.md)
- [CIA Triad and Basic Security Concepts](../notes/security-fundamentals/cia-triad-and-basic-security-concepts.md)
- [Malware & Social Engineering](../notes/security-fundamentals/malware-and-social-engineering.md)
- [Identity and Access Management (IAM)](../notes/security-fundamentals/identity-and-access-management.md)
- [Identity Attacks](../notes/security-fundamentals/identity-attacks.md)
- [Authentication & Authorization](../notes/security-fundamentals/authentication-and-authorization.md)
- [Cisco Introduction to Cybersecurity - Course Notes](../notes/security-fundamentals/cisco-introduction-to-cybersecurity-course-notes.md)
- [Threat Actors & Cyber Warfare](../notes/security-fundamentals/threat-actors-and-cyber-warfare.md)
- [Vulnerabilities & Patch Management](../notes/security-fundamentals/vulnerabilities-and-patch-management.md)
- [Cryptocurrency & Cryptojacking](../notes/security-fundamentals/cryptocurrency-and-cryptojacking.md)
- [Defense in Depth](../notes/security-fundamentals/defense-in-depth.md)
- [Network Security](../notes/security-fundamentals/network-security.md)
- [Cybersecurity Glossary](../glossary.md)
- [VMware Setup](../labs/week-1-lab-setup/vmware-setup.md)
- [Windows 11 Lab](../labs/week-1-lab-setup/windows-11-lab.md)

---

## Readiness for Week 2

Able to explain the CIA triad, vulnerability versus exploit, authentication versus authorization, and MFA without notes. The main open area is technical precision around overlapping security controls (firewall, IDS, IPS, WAF), which Week 2's networking module and later hands-on labs should sharpen. Lab environment, GitHub, and LinkedIn are set up and ready. Moving into Week 2: networking fundamentals (IPs, DNS, ports, protocols).
