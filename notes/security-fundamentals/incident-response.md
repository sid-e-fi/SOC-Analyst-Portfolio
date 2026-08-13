---
type: concept-note
status: reviewed
tags:
  - incident-response
  - soc
---

# Incident Response

> **Purpose**
>
> Incident Response is the structured process an organization follows to prepare for, detect, analyze, contain, eradicate, and recover from cybersecurity incidents.
>
> The objective is to limit damage, restore normal operations, preserve useful evidence, and learn from the incident so that future incidents can be handled more effectively.

---

# What Is an Incident?

A **security incident** is an event that has, or could have, a negative impact on the confidentiality, integrity, or availability of information systems or data.

Examples include:

- Confirmed malware infection
- Account compromise
- Unauthorized access
- Data theft
- Ransomware
- Malicious privilege changes
- Significant phishing compromise
- Unauthorized system modification

Not every security alert is an incident.

An alert is an indication that something potentially suspicious occurred. An incident requires investigation and sufficient evidence to determine that the activity represents a security issue requiring response.

---

# Incident Response Lifecycle

The incident response lifecycle consists of:

1. **Prepare**
2. **Detect and Analyze**
3. **Contain**
4. **Eradicate**
5. **Recover**
6. **Lessons Learned**

```text
Prepare
   ↓
Detect / Analyze
   ↓
Contain
   ↓
Eradicate
   ↓
Recover
   ↓
Lessons Learned
   ↓
Improved Preparation
```

The lifecycle is continuous. Lessons from previous incidents should improve future preparation and response.

---

# 1. Preparation

Preparation occurs before an incident happens.

The goal is to ensure that people, processes, and technology are ready to respond.

Preparation can include:

- Incident response plans
- Security policies
- Incident response procedures
- Contact lists
- Escalation procedures
- Security monitoring
- Log collection
- Endpoint telemetry
- Backups
- Security tools
- Response playbooks
- Staff training

For a SOC, preparation also means ensuring that analysts know:

- Which alerts require investigation
- Which systems are critical
- Who owns affected systems
- When to escalate
- What evidence should be collected
- Which containment actions are authorized

For the full definitions of a **CSIRT** (Computer Security Incident Response Team) and **security playbooks**, including the standard playbook examples (phishing, malware, brute force, suspicious authentication, ransomware), see:

[Cisco Introduction to Cybersecurity - Course Notes](cisco-introduction-to-cybersecurity-course-notes.md)

---

# 2. Detection and Analysis

This phase begins when suspicious activity is detected.

Possible sources include:

- SIEM alerts
- EDR alerts
- IDS / IPS alerts
- Firewall logs
- Authentication logs
- User reports
- Endpoint telemetry
- Network activity
- Threat intelligence

The first objective is to understand what happened.

A useful SOC triage framework is:

```text
What happened?
      ↓
Where and when?
      ↓
Is it expected?
      ↓
What evidence supports the conclusion?
      ↓
How serious is it?
      ↓
What should happen next?
```

---

## What Happened?

Describe the event in one neutral sentence.

Example:

> Multiple failed authentication attempts were observed against a user account from the same external source IP.

Avoid immediately assuming that the event is malicious.

---

## Where and When?

Identify relevant context such as:

- Host
- User
- Source IP
- Destination
- Log source
- First observed time
- Last observed time
- Event count

---

## Is It Expected?

Determine whether the activity has a legitimate explanation.

Check for:

- Authorized users
- Normal processes
- Maintenance activity
- Security scanners
- Scheduled tasks
- Known administrative activity
- Expected application behavior

A suspicious-looking event is not automatically malicious.

---

## What Evidence Supports the Conclusion?

Useful evidence may include:

- Related logs
- Authentication history
- Process parent and child relationships
- Network connections
- Endpoint telemetry
- Email headers
- File activity
- User activity
- Security alerts

Evidence should support the analyst's conclusion rather than simply confirm an initial assumption.

---

## How Serious Is It?

Severity can depend on factors such as:

- Number of affected systems
- Number of affected users
- Privilege level
- Asset value
- Data sensitivity
- Business impact
- Evidence of active malicious behavior
- Potential for lateral movement

---

## What Should Happen Next?

Possible outcomes include:

- Close as benign
- Continue monitoring
- Contact the system or account owner
- Investigate further
- Isolate an affected endpoint
- Escalate
- Create or update an incident

The correct action depends on the evidence, severity, and organizational procedures.

---

# Alert vs Incident

An **alert** is a notification generated because a security tool detected an event, anomaly, or pattern of interest.

An **incident** is a security event that has been sufficiently investigated and determined to require incident response.

```text
Security Event
      ↓
Detection
      ↓
Alert
      ↓
Investigation
      ↓
Evidence + Context
      ↓
Incident?
   ↙       ↘
 No         Yes
 ↓           ↓
Close /      Response
Monitor      Lifecycle
```

A SOC analyst should not automatically treat every alert as a confirmed incident.

---

# 3. Containment

**Containment** limits the spread or impact of an active security incident.

The objective is to prevent the attacker or malicious activity from causing additional damage while investigation and eradication continue.

Possible containment actions include:

- Isolating an endpoint
- Disabling a compromised account
- Blocking malicious network communication
- Blocking malicious domains or IP addresses
- Restricting access to affected systems
- Segmenting affected systems

Containment should consider business impact.

For example, immediately shutting down a critical production system may stop malicious activity but could also create significant operational disruption.

Containment decisions should therefore follow organizational procedures and appropriate authorization.

---

# Short-Term vs Long-Term Containment

### Short-Term Containment

Immediate actions intended to stop or limit active malicious activity.

Examples:

- Isolate an endpoint
- Disable a compromised account
- Block a malicious IP
- Disconnect an affected system from the network

### Long-Term Containment

Actions that maintain protection while the organization continues investigating and preparing for eradication.

Examples:

- Additional network restrictions
- Increased monitoring
- Temporary access restrictions
- Moving affected services into controlled environments

---

# 4. Eradication

**Eradication** removes the cause of the incident and eliminates malicious artifacts or unauthorized access.

Possible actions include:

- Removing malware
- Removing persistence mechanisms
- Resetting compromised credentials
- Removing unauthorized accounts
- Patching exploited vulnerabilities
- Removing malicious files
- Correcting security misconfigurations
- Reimaging compromised systems when appropriate

Eradication should address the **root cause**, not merely the visible symptom.

For example:

```text
Malware discovered
      ↓
Remove malware
      ↓
Identify how it entered
      ↓
Identify persistence
      ↓
Remove persistence
      ↓
Fix initial access weakness
```

Removing a malicious executable without addressing the initial compromise may allow the attacker to return.

---

# 5. Recovery

**Recovery** restores affected systems and services to normal operation.

Recovery can include:

- Restoring systems from known-good backups
- Rebuilding compromised systems
- Restoring services
- Resetting credentials
- Validating system security
- Monitoring restored systems
- Confirming normal operations

Recovery should not simply mean turning the affected system back on.

The organization should verify that:

- The threat has been removed
- Required security controls are functioning
- Systems are operating normally
- No suspicious activity remains
- Monitoring is in place

---

# 6. Lessons Learned

After the incident, the organization reviews what happened and identifies improvements.

Questions include:

- What happened?
- How did the incident begin?
- What allowed it to occur?
- How was it detected?
- What worked well?
- What failed?
- Was important evidence available?
- Was containment effective?
- Was escalation timely?
- What should change?

Possible improvements include:

- New detection rules
- Better logging
- Improved access controls
- Additional security awareness
- Updated playbooks
- Better network segmentation
- Improved backup procedures
- Patching
- Configuration changes

The purpose is not simply to document the incident.

The purpose is to make the organization better prepared for the next one.

---

# Incident Timeline

A timeline helps analysts reconstruct the sequence of events.

Example:

```text
10:02  Suspicious email received
10:08  User clicks malicious link
10:10  Credentials submitted
10:15  Suspicious login detected
10:18  SOC alert generated
10:25  Analyst begins investigation
10:40  Account disabled
10:55  Endpoint isolated
11:30  Malicious activity confirmed
12:15  Persistence removed
13:00  Credentials reset
14:00  System restored
15:00  Monitoring increased
```

A timeline helps establish:

- Initial access
- Attacker activity
- Detection time
- Response actions
- Containment time
- Recovery time

It can also reveal gaps between important events.

---

# Evidence During Incident Response

Evidence allows analysts to establish what happened rather than relying on assumptions.

Potential evidence includes:

- Authentication logs
- Endpoint logs
- Network logs
- Firewall logs
- IDS / IPS alerts
- EDR telemetry
- Process activity
- File activity
- Email headers
- DNS activity
- User activity
- Security alerts

Important information can include:

- Timestamp
- Host
- User
- IP address
- Process
- Event type
- Source
- Destination
- Related events

Evidence should be documented clearly so another analyst can understand how the conclusion was reached.

---

# Incident Severity

Incident severity helps determine how urgently an incident should be handled.

Severity can be influenced by:

- Scope
- Business impact
- Asset criticality
- Data sensitivity
- Privilege level
- Active attacker behavior
- Potential for further compromise

A simple conceptual model is:

```text
Low
↓
Limited scope / limited impact

Medium
↓
Meaningful security impact requiring coordinated response

High
↓
Significant compromise, sensitive assets, or major business impact

Critical
↓
Severe or widespread impact requiring immediate response
```

Organizations should define their own severity criteria.

Severity should be based on evidence and business context rather than the alert name alone.

---

# SOC Analyst Role in Incident Response

A Level 1 SOC analyst commonly contributes to the early stages of incident response.

Typical responsibilities can include:

- Monitoring alerts
- Performing initial triage
- Gathering evidence
- Determining whether activity is expected
- Assessing initial severity
- Documenting findings
- Escalating confirmed or high-risk activity
- Following established playbooks
- Supporting containment actions when authorized

A useful mental model is:

```text
Alert
  ↓
Triage
  ↓
Context
  ↓
Evidence
  ↓
Disposition
  ↓
Escalate / Contain / Close
```

The L1 analyst does not necessarily perform every stage of incident response independently.

Complex incidents may be escalated to:

- Senior SOC analysts
- Incident responders
- Digital forensics teams
- Threat hunters
- Security engineers
- System administrators
- Management

---

# Incident Response and the SOC

The SOC is often the point where suspicious activity is first detected and investigated.

Example:

```text
Security Control
      ↓
Alert
      ↓
SOC Analyst
      ↓
Triage
      ↓
Evidence Collection
      ↓
Severity Assessment
      ↓
Escalation
      ↓
Incident Response
```

The SOC and incident response process therefore work together.

The SOC focuses heavily on monitoring, detection, triage, and escalation, while incident response coordinates the broader response to confirmed incidents.

---

# Common Incident Types

Incident response can apply to many scenarios, including:

- Account compromise
- Ransomware
- Malware infection
- Phishing compromise
- Brute-force attacks
- Unauthorized privilege changes
- Data exfiltration
- Suspicious endpoint activity
- Unauthorized access
- Network intrusion

The response process remains structured, but the technical investigation and containment actions vary by incident type.

---

# Incident Response Documentation

A useful incident report should allow another analyst to understand:

- What happened
- When it happened
- What was affected
- What evidence was found
- How severe it was
- What actions were taken
- What remains unresolved
- What should happen next

A practical structure is:

```text
Incident Summary
↓
Timeline
↓
Affected Assets
↓
Evidence
↓
Analysis
↓
Severity
↓
Impact
↓
Containment
↓
Eradication
↓
Recovery
↓
Lessons Learned
↓
Next Actions
```

---

# SOC Triage Reference

| Question | What to determine |
|---|---|
| What happened? | Describe the event neutrally |
| Where and when? | Host, user, IP, log source, first/last time, count |
| Is it expected? | Change context, normal behavior, authorized user/process |
| How serious is it? | Scope, privilege, data/asset value, active malicious signs |
| What evidence is needed? | Related logs, process tree, login history, network connections, email headers |
| What is the next action? | Close, monitor, contact owner, isolate, escalate, or create incident |

---

# Incident Response and the CIA Triad

Incident response aims to limit damage to confidentiality, integrity, and availability. A ransomware incident is the clearest example: encryption affects availability and integrity, and any data theft that happened before encryption affects confidentiality too.

For the full ransomware-as-CIA-Triad-example breakdown, see:

[CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)

---

# Incident Response and Risk

Incident response reduces the potential impact of security incidents.

```text
Threat
   ↓
Attack
   ↓
Security Incident
   ↓
Detection
   ↓
Containment
   ↓
Eradication
   ↓
Recovery
   ↓
Reduced Impact
```

Incident response does not eliminate the underlying risk.

Lessons learned should feed back into security controls, monitoring, risk management, and preparation.

See:

[Risk Management](risk-management.md)

---

# Interview Notes

### What is incident response?

Incident response is the structured process used to prepare for, detect, analyze, contain, eradicate, and recover from cybersecurity incidents.

### What are the incident response phases?

Prepare, detect and analyze, contain, eradicate, recover, and lessons learned.

### What is the purpose of containment?

To limit the spread and impact of an active security incident while investigation and eradication continue.

### What is eradication?

Removing the cause of the incident, including malware, persistence, compromised credentials, vulnerabilities, or other mechanisms that allowed the attacker to remain active.

### What is recovery?

Restoring affected systems and services to normal operation after confirming that the threat has been removed and appropriate security controls are functioning.

### Why are lessons learned important?

They identify what worked, what failed, and what should be improved so that future incidents can be detected and handled more effectively.

### Is every alert an incident?

No. An alert indicates potentially suspicious activity. Investigation is required to determine whether it represents a security incident.

### What does an L1 SOC analyst do during incident response?

An L1 analyst commonly performs initial triage, gathers evidence, assesses severity, documents findings, follows playbooks, and escalates confirmed or high-risk activity according to organizational procedures.

### What evidence should an analyst collect?

Relevant logs, authentication events, endpoint telemetry, process activity, network connections, email headers, timestamps, affected hosts, users, and other evidence that helps establish what happened.

---

# Key Takeaways

- **Incident response is a structured process, not an improvised reaction.**
- The lifecycle is **Prepare → Detect/Analyze → Contain → Eradicate → Recover → Lessons Learned**.
- An alert is not automatically an incident.
- Investigation should be based on evidence and context.
- **Containment limits ongoing damage.**
- **Eradication removes the cause and malicious mechanisms.**
- **Recovery restores normal operations.**
- **Lessons learned improve future security.**
- Incident timelines help reconstruct what happened and when.
- Severity should consider scope, business impact, asset criticality, data sensitivity, privilege, and active malicious behavior.
- L1 SOC analysts commonly focus on monitoring, triage, evidence gathering, documentation, and escalation.
- Incident response should protect confidentiality, integrity, and availability.
- The goal is not only to resolve the current incident, but to improve the organization's ability to prevent, detect, and respond to future incidents.

---

# Related Notes

- [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)
- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
- [Authentication & Authorization](authentication-and-authorization.md)
- [Identity and Access Management (IAM)](identity-and-access-management.md)
- [Identity Attacks](identity-attacks.md)
- [Defense in Depth](defense-in-depth.md)
- [Network Security](network-security.md)
- [Risk Management](risk-management.md)
- [Malware & Social Engineering](malware-and-social-engineering.md)
- [Threat Actors & Cyber Warfare](threat-actors-and-cyber-warfare.md)
- [Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)
