---
type: concept-note
status: reviewed
tags:
  - network-security
  - firewalls
  - ids-ips
---

# Network Security

> **Purpose**
>
> This note explains the foundational network security controls and concepts relevant to protecting, monitoring, and investigating network activity. It focuses on what the controls do, what evidence they generate, and how a SOC analyst should interpret network activity in context.

---

# What Is Network Security?

**Network security** is the practice of protecting network infrastructure, communications, connected systems, and the services they provide from unauthorized access, misuse, disruption, and attack.

Network security uses multiple controls because no single control provides complete protection.

Common controls and technologies include:

- Firewalls
- Intrusion Detection Systems (IDS)
- Intrusion Prevention Systems (IPS)
- Network segmentation
- Reverse proxies
- VPNs
- Network monitoring
- Endpoint security
- SIEM

---

# Firewall

## Definition

A **firewall** controls network traffic according to configured security policies.

A firewall can allow or block traffic based on characteristics such as:

- Source IP address
- Destination IP address
- Source port
- Destination port
- Protocol
- Connection state
- Application or other inspected attributes, depending on the firewall

A basic firewall may allow HTTPS traffic because TCP port 443 is permitted without understanding whether the application-layer request itself contains a malicious payload.

Modern firewalls can provide more advanced inspection capabilities.

---

## Firewall Example

```text
Internet
   ↓
Firewall
   ↓
Internal Network
   ↓
Servers / Endpoints
```

The firewall acts as a policy enforcement point between network zones.

A firewall rule might conceptually look like:

```text
Source: Any
Destination: Web Server
Protocol: TCP
Destination Port: 443
Action: Allow
```

The exact rules depend on the organization's security requirements.

---

# IDS vs IPS

## IDS

**Intrusion Detection System (IDS)** detects suspicious activity and generates alerts.

An IDS is primarily a **detective control**.

```text
Network Traffic
      ↓
     IDS
      ↓
Suspicious Activity
      ↓
    Alert
```

The traffic may continue unless another control takes action.

---

## IPS

**Intrusion Prevention System (IPS)** can detect suspicious activity and automatically block or prevent certain activity.

An IPS therefore provides both detection and prevention capabilities.

```text
Network Traffic
      ↓
     IPS
      ↓
Suspicious Activity
      ↓
    Block
```

---

## IDS vs IPS

| IDS | IPS |
|---|---|
| Detects suspicious activity | Detects and can block suspicious activity |
| Generates alerts | Generates alerts and can enforce prevention |
| Primarily detective | Detective + preventive |
| Usually does not directly block traffic | Can block or prevent traffic |

Neither system is automatically better than the other. Their usefulness depends on the organization's architecture, policies, tuning, and operational requirements.

---

# Port Scanning

## Definition

**Port scanning** is a reconnaissance technique used to identify accessible ports and exposed services on a host.

A port scan can help an attacker determine what services may be available for further investigation or exploitation.

Example:

```text
Attacker
   ↓
Target Host
   ↓
Port 22   → SSH
Port 80   → HTTP
Port 443  → HTTPS
Port 3389 → RDP
```

A port scan does **not automatically mean that a successful attack occurred**.

Port scanning can also be performed legitimately by:

- Security teams
- Vulnerability scanners
- System administrators
- Penetration testers
- Network management systems

---

# SOC Investigation of Port Scanning

When investigating a port-scanning alert, examine:

- Source IP
- Internal vs external origin
- Destination host
- Destination ports
- Number of ports targeted
- Number of hosts targeted
- Scan timing
- Authorized security scanners
- Penetration-testing activity
- Maintenance activity
- Subsequent connections
- Subsequent exploitation attempts

The important question is not simply:

> "Was a port scan detected?"

Instead:

> "Is this scanning activity expected, and did anything suspicious happen afterward?"

---

# NetFlow

## Definition

**NetFlow** provides network-flow metadata rather than the complete contents of network communications.

Typical information can include:

- Source IP
- Destination IP
- Source port
- Destination port
- Protocol
- Timestamp
- Duration
- Packets transferred
- Bytes transferred

A flow record describes communication between endpoints without necessarily containing the full contents of the communication.

---

## SOC Uses of NetFlow

NetFlow can help analysts identify:

- Unusual communication patterns
- Possible command-and-control activity
- Potential data exfiltration
- Unexpected external connections
- High-volume communication
- Long-lived connections
- Communication between unusual internal systems

Example:

```text
Endpoint
   ↓
Unusual External Destination
   ↓
Persistent Connection
   ↓
Large Outbound Data Transfer
   ↓
SOC Investigation
```

NetFlow alone does not prove malicious activity.

It provides evidence that should be correlated with other telemetry.

---

# Reverse Proxy

## Definition

A **reverse proxy** sits in front of backend servers and receives client requests on their behalf.

A simplified architecture is:

```text
Client
   ↓
Reverse Proxy
   ↓
Backend Web Server
```

The reverse proxy can:

- Hide backend servers from direct client access
- Distribute requests
- Perform load balancing
- Offload certain processing tasks
- Provide an additional security layer

Reverse proxies can therefore be part of both application architecture and security architecture.

---

# Network Segmentation

## Definition

**Network segmentation** divides a network into separate logical or physical network zones.

The purpose is to control communication between different parts of the environment and limit unnecessary access.

Example:

```text
Internet
    ↓
Firewall
    ↓
DMZ
    ↓
Internal Network
    ├── User Network
    ├── Server Network
    └── Management Network
```

Higher-risk systems can also be placed on isolated networks.

For example:

```text
IoT Devices
     ↓
Isolated Network
     ↓
Restricted Communication
```

If an IoT device is compromised, segmentation can help reduce the attacker's ability to move laterally into other parts of the environment.

---

# Why Segmentation Matters to a SOC

Network segmentation can limit:

- Lateral movement
- Unauthorized access
- Exposure between systems
- Blast radius of a compromise

It also provides useful investigation context.

For example, communication between two systems in different security zones may be more significant than the same communication between systems that are expected to communicate.

---

# Network Security and Defense in Depth

Network security controls are part of a broader **Defense in Depth** strategy.

A simplified architecture might look like:

```text
Internet
    ↓
Firewall
    ↓
IDS / IPS
    ↓
Reverse Proxy
    ↓
Network Segmentation
    ↓
Endpoint Security
    ↓
SIEM
    ↓
SOC Investigation
    ↓
Incident Response
```

Not every organization will use every layer.

The important principle is that multiple controls can provide different opportunities to prevent, detect, contain, and investigate malicious activity.

See:

[Defense in Depth](defense-in-depth.md)

---

# Network Security and the CIA Triad

Network security controls can support all three objectives of the CIA Triad.

### Confidentiality

Network controls can restrict unauthorized access to sensitive systems and communications.

Examples:

- Firewalls
- Network segmentation
- Access controls
- VPNs

### Integrity

Network monitoring and access controls can help identify or prevent unauthorized changes and malicious activity.

### Availability

Network controls can help maintain service availability by controlling unwanted traffic and supporting resilient network architecture.

Examples:

- Firewalls
- IPS
- Redundancy
- Traffic filtering
- DDoS protection

See:

[CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)

---

# Network Security Evidence

A SOC analyst may encounter network evidence from:

- Firewall logs
- IDS alerts
- IPS events
- NetFlow
- DNS logs
- Proxy logs
- VPN logs
- Authentication logs
- Endpoint telemetry
- SIEM correlation

Useful fields can include:

- Timestamp
- Source IP
- Destination IP
- Source port
- Destination port
- Protocol
- Action
- Hostname
- Username
- Bytes
- Packets
- Rule or signature
- Alert name

---

# SOC Investigation Mindset

Network alerts should not be investigated in isolation when additional evidence is available.

For example:

```text
Port Scan
    ↓
Identify Source
    ↓
Identify Target
    ↓
Determine Whether Scan Is Authorized
    ↓
Review Other Network Activity
    ↓
Check Endpoint Activity
    ↓
Check Authentication Activity
    ↓
Look for Exploitation
    ↓
Assess Impact
```

A network event becomes more meaningful when correlated with other evidence.

For example:

```text
Port Scan
   +
Successful Authentication
   +
Suspicious Process
   +
Outbound Connection
```

is more concerning than a single isolated port-scan event.

---

# Common Investigation Questions

When investigating network activity, ask:

### What happened?

Describe the network event in one neutral sentence.

### Where did it originate?

Determine:

- Source IP
- Source host
- Internal or external origin
- User, if available

### What was targeted?

Determine:

- Destination IP
- Destination host
- Destination port
- Application or service

### When did it happen?

Determine:

- First observed time
- Last observed time
- Frequency
- Duration

### Was it expected?

Check:

- Authorized scanners
- Maintenance
- Normal application behavior
- Known infrastructure
- Expected administrative activity

### What happened afterward?

Look for:

- Successful connections
- Authentication
- Exploitation
- Process execution
- Data transfer
- Persistence

### What is the impact?

Consider:

- Affected hosts
- Affected users
- Sensitive systems
- Data exposure
- Service disruption
- Lateral movement

---

# Important Distinctions

## Firewall vs IDS

A firewall primarily enforces network traffic policy.

An IDS primarily detects suspicious activity and generates alerts.

## IDS vs IPS

An IDS detects suspicious activity.

An IPS can detect and automatically block or prevent certain activity.

## Network Traffic vs Network Metadata

Network traffic may include the actual communication contents.

NetFlow provides metadata about the communication rather than the complete contents.

## Scanning vs Exploitation

Scanning identifies potentially accessible services.

Exploitation attempts to take advantage of a vulnerability.

A scan does not prove that exploitation occurred.

---

# Interview Notes

### What does a firewall do?

A firewall controls network traffic according to configured security policies and can allow or block traffic based on characteristics such as addresses, ports, protocols, connection state, and other inspected attributes.

### What is the difference between IDS and IPS?

An IDS detects suspicious activity and generates alerts. An IPS can detect suspicious activity and automatically block or prevent certain activity.

### What is port scanning?

Port scanning is a reconnaissance technique used to identify accessible ports and exposed services on a host.

### Does port scanning mean a system was compromised?

No. Port scanning is reconnaissance. It does not prove that exploitation or compromise occurred.

### What is NetFlow?

NetFlow provides metadata about network communications, such as source and destination IPs, ports, protocol, duration, packets, and bytes.

### What is a reverse proxy?

A reverse proxy sits in front of backend servers and receives client requests on their behalf. It can hide backend servers, distribute requests, perform load balancing, and provide an additional security layer.

### Why is network segmentation important?

Segmentation separates network zones and restricts communication between them, helping reduce unauthorized access and limit lateral movement after a compromise.

### What should a SOC analyst consider when investigating a port scan?

The analyst should examine the source, target, ports, timing, whether the scanner is authorized, and whether the scan was followed by suspicious connections, exploitation, authentication, or other malicious activity.

---

# Key Takeaways

- **Network security protects network infrastructure, communications, connected systems, and services.**
- **Firewalls enforce network traffic policies.**
- **IDS detects suspicious activity and generates alerts.**
- **IPS can detect and block certain suspicious activity.**
- **Port scanning is reconnaissance, not proof of compromise.**
- **NetFlow provides network-flow metadata that can help identify unusual communication patterns.**
- **Reverse proxies sit in front of backend servers and can provide architectural and security benefits.**
- **Network segmentation limits unnecessary communication and can reduce lateral movement.**
- Network alerts should be investigated using context and correlated evidence.
- SOC analysts should examine what happened, where and when it happened, whether it was expected, what happened afterward, and what the potential impact is.
- Network security is one layer of a broader Defense in Depth strategy.

---

# Related Notes

- [Defense in Depth](defense-in-depth.md)
- [CIA Triad and Basic Security Concepts](cia-triad-and-basic-security-concepts.md)
- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
- [Risk Management](risk-management.md)
- [Identity and Access Management (IAM)](identity-and-access-management.md)
- [Identity Attacks](identity-attacks.md)
- [Incident Response](incident-response.md)
