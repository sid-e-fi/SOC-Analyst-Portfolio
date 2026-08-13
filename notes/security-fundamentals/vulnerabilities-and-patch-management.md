---
type: concept-note
status: reviewed
tags:
  - vulnerabilities
  - patch-management
---

# Vulnerabilities & Patch Management

> **Purpose**
>
> This note explains vulnerabilities, exploits, common vulnerability categories, vulnerability identification and scoring, zero-day vulnerabilities, and the patch management process used to reduce security risk.
>
> It also explains how vulnerability management connects to SOC operations, threat detection, and incident investigation.

---

# Core Definitions

## Threat

A **threat** is anything capable of causing harm to a system, network, asset, or organization — see [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md) for the full definition.

---

## Vulnerability

A **vulnerability** is a weakness in hardware, software, configuration, or a process that could be exploited or abused.

Examples include:

- Weak passwords
- Unpatched software
- Misconfigured systems
- Software bugs
- Authentication weaknesses
- Authorization weaknesses
- Poor input validation

A vulnerability is a **weakness**, not an attack.

---

## Exploit

An **exploit** is a technique, tool, or piece of code used to take advantage of a vulnerability.

Example:

```text
Vulnerability
      ↓
Exploit
      ↓
Unauthorized Action
```

The distinction is:

> **Vulnerability = weakness**  
> **Exploit = method used to abuse the weakness**

---

## Attack

An **attack** is an attempt to compromise a system, account, network, or data, and may succeed or fail — see [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md) for the full definition.

The relationship between these terms:

```text
Threat Actor
      ↓
Vulnerability
      ↓
Exploit
      ↓
Attack
      ↓
Potential Impact
```

---

# Attack Chain

A simplified attack relationship is:

```text
Threat
   ↓
Vulnerability
   ↓
Exploit
   ↓
Attack
   ↓
Impact
```

### Example

```text
Cybercriminal
      ↓
Weak Password
      ↓
Password Spraying
      ↓
Unauthorized Login
      ↓
Data Theft
```

This model helps analysts understand how a weakness can become a security incident.

---

# Hardware Vulnerabilities

## Definition

**Hardware vulnerabilities** are weaknesses that exist within physical hardware components or their associated firmware.

Examples include:

- CPUs
- Firmware
- BIOS / UEFI
- TPM (Trusted Platform Module)

Hardware vulnerabilities can be particularly difficult to remediate because the affected hardware may already be deployed across an environment.

---

# Spectre

**Spectre** is a class of hardware vulnerabilities involving speculative execution in modern processors.

### Potential Impact

Spectre can potentially allow attackers to infer or access sensitive information from memory through speculative-execution techniques.

### Why It Matters

Spectre demonstrated that performance optimizations in modern processors can introduce security risks at the hardware level.

It affected processors from multiple manufacturers.

---

# Meltdown

**Meltdown** is a hardware vulnerability involving speculative execution that can allow unauthorized access to protected kernel memory.

### Potential Impact

Potentially exposed information can include:

- Passwords
- Encryption keys
- Other sensitive data

### Spectre vs Meltdown

Both involve speculative execution, but they exploit different behaviors.

| Spectre | Meltdown |
|---|---|
| Manipulates speculative execution to leak information | Can bypass memory isolation |
| Affects multiple processor architectures | Primarily affected certain processor architectures |
| More difficult to fully eliminate | Addressed through hardware, firmware, and operating-system mitigations |

---

# Software Vulnerabilities

## Definition

**Software vulnerabilities** are weaknesses caused by programming errors, design flaws, insecure configurations, or other problems within software.

Examples include vulnerabilities in:

- Windows
- Linux
- Web browsers
- Microsoft Office
- Adobe applications
- Web applications
- Network services

Common causes include:

- Coding mistakes
- Poor input validation
- Memory corruption
- Authentication flaws
- Authorization flaws

---

# Common Categories of Software Vulnerabilities

## Authentication Vulnerabilities

Authentication vulnerabilities can allow attackers to bypass or weaken identity verification.

Example:

```text
Legitimate Authentication
        ↓
Authentication Weakness
        ↓
Unauthorized Access
```

---

## Authorization Vulnerabilities

Authorization vulnerabilities can allow authenticated users to access resources or perform actions they should not be permitted to access or perform.

Example:

```text
Authenticated User
        ↓
Authorization Flaw
        ↓
Unauthorized Resource Access
```

---

## Input Validation Vulnerabilities

Input validation vulnerabilities occur when an application fails to properly validate or sanitize user-supplied input.

Examples include:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection

---

## Memory Vulnerabilities

Memory vulnerabilities are caused by unsafe memory handling.

Examples include:

- Buffer overflow
- Memory corruption

These vulnerabilities can potentially lead to:

- Application crashes
- Data exposure
- Unauthorized code execution

---

## Race Conditions

A **race condition** occurs when the behavior of a system depends on the timing or ordering of multiple operations accessing shared resources.

Poorly handled race conditions can create security vulnerabilities.

---

# Common Vulnerabilities and Exposures (CVE)

## Definition

**Common Vulnerabilities and Exposures (CVE)** is a standardized system for assigning unique identifiers to publicly disclosed security vulnerabilities.

Example:

```text
CVE-2025-12345
```

A CVE identifier allows security teams, vendors, researchers, and other organizations to refer to a specific vulnerability consistently.

### Benefits

- Standardized communication
- Vulnerability tracking
- Vendor coordination
- Security research
- Remediation tracking

---

# Common Vulnerability Scoring System (CVSS)

## Definition

**Common Vulnerability Scoring System (CVSS)** provides a standardized severity score for vulnerabilities.

Scores range from:

| Score | Severity |
|---|---|
| 0.1 – 3.9 | Low |
| 4.0 – 6.9 | Medium |
| 7.0 – 8.9 | High |
| 9.0 – 10.0 | Critical |

CVSS helps organizations prioritize vulnerability remediation.

### Important

A CVSS score is not the same thing as an organization's actual risk.

Risk also depends on factors such as:

- Asset value
- Exposure
- Business importance
- Likelihood of exploitation
- Existing security controls

See:

[Risk Management](risk-management.md)

---

# Zero-Day Vulnerability

## Definition

A **zero-day vulnerability** is a vulnerability for which no official security patch is available when it is discovered or exploited.

### Why Zero-Days Are Dangerous

- No official patch may be available
- Attackers may actively exploit the vulnerability
- Organizations have limited time to prepare
- Traditional patch-based defenses may not be sufficient

---

# Zero-Day Exploit

A **zero-day exploit** is the technique, tool, or malicious code used to exploit a zero-day vulnerability.

The distinction is:

> **Zero-day vulnerability = the weakness**  
> **Zero-day exploit = the method used to abuse it**

---

# Compensating Controls

When a vulnerability cannot immediately be patched, organizations may implement **compensating controls** to reduce the associated risk.

Examples include:

- Firewall rules
- Network segmentation
- Disabling vulnerable services
- IPS protections
- Increased EDR monitoring
- Restricting access to affected systems

Compensating controls reduce exposure but do not necessarily remove the underlying vulnerability.

---

# Software Updates

Software vendors release updates for various reasons, including:

- Fixing vulnerabilities
- Improving stability
- Adding features
- Improving performance

Security updates should be evaluated and deployed according to the organization's patch management process.

---

# Patch Management

## Definition

**Patch Management** is the structured process of identifying, assessing, testing, deploying, and verifying software updates.

A simplified workflow is:

```text
Identify Vulnerability
        ↓
Assess Risk
        ↓
Test Patch
        ↓
Approve
        ↓
Deploy
        ↓
Verify Installation
        ↓
Monitor Systems
```

Patch management is not simply installing every available update immediately.

Organizations must consider:

- Security risk
- Business impact
- Compatibility
- Downtime
- Testing requirements
- System criticality

---

# Why Organizations Do Not Patch Everything Immediately

Immediate patching is not always practical.

Updates may:

- Break critical applications
- Interrupt business operations
- Cause downtime
- Create compatibility problems
- Require testing before deployment

Organizations therefore balance:

```text
Security Risk
      +
Business Requirements
      +
Operational Risk
      ↓
Patch Decision
```

The goal is to reduce security risk without creating unacceptable operational disruption.

---

# Patch Tuesday

**Patch Tuesday** refers to Microsoft's regular monthly security update release, typically occurring on the **second Tuesday of each month**.

Organizations often use this predictable release schedule to plan:

- Patch testing
- Deployment
- Vulnerability assessment
- Monitoring

SOC teams may also increase awareness of newly disclosed vulnerabilities and monitor for exploitation attempts against systems that remain unpatched.

---

# Vulnerability Prioritization

Organizations should not treat every vulnerability as equally urgent.

A useful prioritization process considers:

- CVSS severity
- Whether exploitation is known
- Internet exposure
- Asset criticality
- Business impact
- Availability of a patch
- Availability of compensating controls
- Existing security controls

Example:

```text
Critical Vulnerability
        +
Internet-Facing Server
        +
Known Exploitation
        +
Sensitive Data
        ↓
Very High Remediation Priority
```

A lower-scoring vulnerability on an isolated, non-critical system may require less immediate attention.

---

# Vulnerability Management vs Patch Management

These terms are related but not identical.

### Vulnerability Management

The broader process of:

- Identifying vulnerabilities
- Assessing vulnerabilities
- Prioritizing vulnerabilities
- Tracking remediation
- Monitoring exposure

### Patch Management

The process of:

- Testing updates
- Approving updates
- Deploying updates
- Verifying updates

Patch management is therefore one component of the broader vulnerability management process.

---

# SOC Perspective

SOC analysts typically do not install patches themselves.

Instead, they may:

- Monitor vulnerability reports
- Validate affected systems
- Identify potentially exposed assets
- Prioritize critical findings
- Coordinate with infrastructure teams
- Monitor for exploitation attempts
- Investigate exploitation alerts
- Verify remediation evidence where required

---

# Vulnerability Exploitation Detection

SOC analysts may encounter evidence that an attacker is attempting to exploit a known vulnerability.

Useful telemetry can include:

- IDS / IPS alerts
- Firewall logs
- EDR telemetry
- Web server logs
- Application logs
- Network traffic
- Process creation
- Authentication activity
- SIEM correlation

A simplified investigation could look like:

```text
Known Vulnerability
        ↓
Exploit Attempt Detected
        ↓
Identify Target
        ↓
Check Whether Target Is Vulnerable
        ↓
Review Endpoint / Application Evidence
        ↓
Determine Exploitation Success
        ↓
Assess Impact
        ↓
Contain / Escalate if Required
```

An exploit attempt does not automatically mean exploitation succeeded.

The analyst should look for evidence of the actual outcome.

---

# Vulnerability Management and Incident Response

A vulnerability becomes especially important when it is actively being exploited.

Example:

```text
Unpatched Software
       ↓
Known Vulnerability
       ↓
Exploit Attempt
       ↓
Successful Exploitation
       ↓
Malicious Code Execution
       ↓
Security Incident
```

At this point, vulnerability management and incident response overlap.

The vulnerability must be remediated, while the incident must be investigated and contained.

See:

[Incident Response](incident-response.md)

---

# Vulnerability Management and Risk

A vulnerability creates potential risk, but the level of risk depends on context.

Consider:

```text
Vulnerability
      +
Exposure
      +
Likelihood
      +
Asset Value
      +
Potential Impact
      ↓
Risk
```

This is why vulnerability management is not simply a race to achieve a zero-vulnerability environment.

Organizations prioritize vulnerabilities according to actual risk.

See:

[CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)

[Risk Management](risk-management.md)

---

# SOC Investigation Questions

When a vulnerability-related alert occurs, ask:

### What vulnerability is involved?

Identify:

- CVE
- Affected software
- Affected version
- Vulnerability type

### Is the system actually vulnerable?

Check:

- Installed version
- Patch status
- Configuration
- Compensating controls

### Is the system exposed?

Determine:

- Internet-facing or internal
- Accessible ports
- Network location
- Security controls

### Is exploitation being attempted?

Look for:

- IDS / IPS alerts
- Suspicious requests
- Exploit signatures
- Abnormal network activity
- Related endpoint activity

### Was exploitation successful?

Look for:

- Unexpected process execution
- File creation
- Authentication changes
- Persistence
- Privilege escalation
- Command-and-Control activity

### What is the impact?

Determine:

- Affected systems
- Affected users
- Data sensitivity
- Business importance
- Potential lateral movement

---

# Interview Notes

### What is a vulnerability?

A weakness in hardware, software, configuration, or a process that could be exploited or abused.

### What is an exploit?

A technique, tool, or piece of code used to take advantage of a vulnerability.

### What is the difference between a vulnerability and an attack?

A vulnerability is the weakness. An attack is an attempt to compromise a system, account, network, or data.

### What is CVE?

CVE is a standardized system for assigning unique identifiers to publicly disclosed security vulnerabilities.

### What is CVSS?

CVSS provides standardized severity scores for vulnerabilities and helps organizations prioritize remediation.

### Does a high CVSS score automatically mean high organizational risk?

No. Organizational risk also depends on factors such as asset value, exposure, likelihood of exploitation, business impact, and existing controls.

### What is a zero-day vulnerability?

A vulnerability for which no official security patch is available when it is discovered or exploited.

### What is a zero-day exploit?

The technique, tool, or malicious code used to exploit a zero-day vulnerability.

### What is patch management?

The structured process of testing, approving, deploying, and verifying software updates.

### Why don't organizations immediately install every security update?

Updates can introduce compatibility problems, downtime, or operational disruption. Organizations therefore balance security risk against business and operational requirements.

### What does a SOC analyst do with vulnerability information?

A SOC analyst may validate affected assets, monitor for exploitation, investigate exploitation attempts, assess potential impact, coordinate with infrastructure teams, and verify remediation evidence.

---

# Key Takeaways

- **A vulnerability is a weakness.**
- **An exploit is a method used to abuse a vulnerability.**
- **An attack is an attempt to compromise a system, account, network, or data.**
- Vulnerabilities can exist in hardware, software, configurations, and processes.
- Authentication, authorization, input validation, memory handling, and race conditions are examples of areas where software vulnerabilities can occur.
- **CVE** provides standardized vulnerability identifiers.
- **CVSS** provides standardized severity scoring.
- A CVSS score helps prioritize vulnerabilities but does not represent complete organizational risk.
- A **zero-day vulnerability** has no official security patch available when discovered or exploited.
- A **zero-day exploit** is the method used to exploit that vulnerability.
- **Compensating controls** can reduce exposure when immediate patching is not possible.
- **Patch management** is one part of broader vulnerability management.
- Organizations balance security risk against business continuity and operational requirements.
- SOC analysts monitor for exploitation, investigate affected systems, and coordinate remediation rather than normally installing patches themselves.
- An exploit attempt does not automatically prove successful exploitation.
- Vulnerability management and incident response overlap when vulnerabilities are actively exploited.

---

# Related Notes

- [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)
- [Risk Management](risk-management.md)
- [Malware & Social Engineering](malware-and-social-engineering.md)
- [Identity Attacks](identity-attacks.md)
- [Threat Actors & Cyber Warfare](threat-actors-and-cyber-warfare.md)
- [Network Security](network-security.md)
- [Defense in Depth](defense-in-depth.md)
- [Incident Response](incident-response.md)
