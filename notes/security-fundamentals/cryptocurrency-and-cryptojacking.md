---
type: concept-note
status: reviewed
tags:
  - cryptocurrency
  - cryptojacking
  - malware
---

# Cryptocurrency & Cryptojacking

> **Purpose**
>
> This note introduces the fundamentals of cryptocurrency, blockchain, and cryptocurrency mining, then explains how attackers abuse computing resources through cryptojacking. It also covers the indicators and investigation steps relevant to a SOC analyst.

---

# Cryptocurrency

## Definition

**Cryptocurrency** is a form of digital currency that uses cryptographic techniques to secure transactions, verify ownership, and help prevent double spending.

Unlike traditional currencies, many cryptocurrencies operate on decentralized networks without a central authority such as a bank.

Cryptocurrency itself is a legitimate technology. Its use does not inherently indicate malicious activity.

---

# Common Cryptocurrencies

Examples include:

- Bitcoin (BTC)
- Ethereum (ETH)
- Monero (XMR)
- Litecoin (LTC)

Different cryptocurrencies use different technologies and consensus mechanisms.

---

# Blockchain

## Definition

A **blockchain** is a distributed digital ledger that records transactions across multiple computers or nodes.

Instead of relying on one central database, participating systems maintain synchronized copies of the ledger.

### Characteristics

- Distributed
- Decentralized
- Transparent
- Tamper-resistant

Transparency is typical but not universal — public ledgers like Bitcoin record transactions openly, while privacy-focused cryptocurrencies (see Monero below) are specifically designed to obscure transaction details.

Blockchain technology is not inherently malicious and has legitimate uses beyond cryptocurrency.

---

# Cryptocurrency Mining

## Definition

**Cryptocurrency mining** is the process of using computing resources to perform the work required by certain cryptocurrency networks and, depending on the cryptocurrency, receive rewards.

Mining can consume significant:

- CPU resources
- GPU resources
- Memory
- Electricity

Mining is legitimate when performed with the owner's knowledge and authorization.

---

# Why Cybercriminals Use Cryptocurrency

Cybercriminals may use cryptocurrency because it can provide:

- Fast international transactions
- Reduced reliance on traditional banking systems
- Transactions that may be difficult to reverse
- Varying levels of financial privacy depending on the cryptocurrency

Cryptocurrency itself is not inherently criminal. The malicious activity comes from how it is obtained or used.

Examples of criminal use include:

- Ransom payments
- Cryptocurrency theft
- Fraud
- Unauthorized mining

---

# Cryptojacking

## Definition

**Cryptojacking** is the unauthorized use of another person's or organization's computing resources to mine cryptocurrency.

Instead of purchasing and operating their own computing infrastructure, attackers secretly use compromised systems to perform mining.

The victim may bear the costs of:

- Electricity
- Hardware usage
- Cloud resources
- Reduced system performance

while the attacker receives the mining rewards.

---

# How Cryptojacking Works

A typical cryptojacking attack can follow this sequence:

```text
Initial Compromise
       ↓
Cryptomining Software Deployed
       ↓
Persistence Established
       ↓
Connection to Mining Infrastructure
       ↓
Computing Resources Consumed
       ↓
Attacker Receives Mining Rewards
```

The attacker may attempt to keep the miner running quietly for as long as possible.

---

# Common Delivery Methods

Attackers may deploy cryptominers through:

- Trojan malware
- Exploitation of software vulnerabilities
- Weak or compromised credentials
- Compromised cloud servers
- Malicious browser extensions
- Supply-chain compromises

The initial access method determines how the cryptominer entered the environment and should therefore be investigated rather than assuming the miner itself was the initial compromise.

---

# Symptoms of Cryptojacking

Common indicators include:

- Constantly high CPU utilization
- High GPU utilization
- Increased memory consumption
- High system temperatures
- Continuous fan activity
- Reduced system performance
- Increased electricity consumption
- Rapid battery drain on laptops
- Unknown or suspicious mining processes

### Important

**High CPU usage alone does not confirm cryptojacking.**

Legitimate applications, system updates, rendering workloads, development tools, and other processes can also consume significant resources.

SOC analysts need additional evidence before determining that mining activity is malicious.

---

# Enterprise Targets

Cryptojacking can affect both personal and enterprise systems.

Common targets include:

- Cloud virtual machines
- Kubernetes clusters
- Containers
- Web servers
- Database servers
- Employee workstations

Cloud environments can be particularly attractive because compromised computing resources can generate significant unauthorized usage and financial costs.

---

# Mining Pools

A **mining pool** is a group of miners that combines computing resources to increase the likelihood of earning cryptocurrency rewards.

Cryptojacking malware may communicate with mining infrastructure or public mining pools to submit mining work.

Connections to mining infrastructure can therefore provide useful network evidence during an investigation.

---

# SOC Perspective

When investigating suspected cryptojacking, analysts should examine multiple sources of evidence rather than relying on resource usage alone.

## System Activity

Check for:

- CPU utilization
- GPU utilization
- Memory consumption
- Disk activity
- System temperature
- Performance degradation

---

## Process Analysis

Investigate:

- Unknown processes
- Suspicious process names
- Parent-child process relationships
- Command-line arguments
- Executable locations
- Digital signatures
- Process creation events

A suspicious process should be investigated in context rather than judged solely by its filename.

---

## Network Activity

Examine:

- Connections to suspected mining infrastructure
- Outbound network traffic
- DNS requests
- Destination IP addresses
- Destination domains
- Unusual persistent connections

Network evidence can help determine whether a suspicious process is communicating with external mining infrastructure.

---

## Persistence

Check for mechanisms that could allow the cryptominer to survive reboots.

Examples include:

### Windows

- Scheduled Tasks
- Windows Services
- Registry Run Keys
- Startup folders

### Linux

- Cron jobs
- Systemd services
- Startup scripts

---

## User Activity

Determine:

- Which user launched the process
- Whether the software was authorized
- Recent software installations
- Recent privilege changes
- Whether the affected account was compromised

This can help distinguish legitimate mining activity from unauthorized cryptojacking.

---

# Investigation Workflow

A simplified investigation workflow is:

```text
High Resource Usage Alert
        ↓
Identify Suspicious Process
        ↓
Determine Whether Process Is Authorized
        ↓
Inspect Process Details
        ↓
Inspect Network Connections
        ↓
Check for Persistence
        ↓
Determine Initial Infection Vector
        ↓
Assess Scope and Impact
        ↓
Contain Affected System
        ↓
Remove Unauthorized Miner
        ↓
Address Initial Access Method
        ↓
Monitor for Reinfection
```

The investigation should not stop after removing the miner.

If the initial compromise is not identified and addressed, the attacker may simply reinstall the cryptominer.

---

# Evidence to Collect

During an investigation, useful evidence may include:

- Hostname
- Username
- Process name
- Process ID
- Parent process
- Command line
- Executable path
- File hash
- Process creation time
- Network connections
- DNS requests
- Destination IPs and domains
- Persistence mechanisms
- Recent software installations
- Authentication activity
- Relevant security alerts

This evidence helps establish **what happened, when it happened, how it happened, and what systems may be affected**.

---

# Impact

Cryptojacking can cause:

- Increased CPU/GPU usage
- Reduced system performance
- Increased electricity consumption
- Increased cloud costs
- Hardware wear
- Reduced availability of computing resources
- Additional security risk if the miner was installed through a broader compromise

The mining activity may therefore be only one visible symptom of a larger security incident.

---

# Interview Notes

### What is cryptocurrency?

A digital currency that uses cryptographic techniques to secure transactions and verify ownership.

### What is blockchain?

A distributed digital ledger maintained across multiple systems that records transactions in a tamper-resistant manner.

### What is cryptocurrency mining?

The process of using computing resources to perform the work required by certain cryptocurrency networks and potentially receive rewards.

### What is cryptojacking?

The unauthorized use of another person's or organization's computing resources to mine cryptocurrency.

### Why do attackers use cryptojacking?

It allows attackers to use someone else's computing resources and shift costs such as electricity or cloud usage to the victim while receiving the mining rewards.

### What are common indicators of cryptojacking?

Examples include persistent high CPU/GPU usage, unknown mining processes, performance degradation, unusual network connections, and connections to suspected mining infrastructure.

### Does high CPU usage prove cryptojacking?

No. High resource usage has many legitimate causes. Additional process, network, persistence, and authorization evidence is required.

### How would you investigate suspected cryptojacking?

Identify the resource-consuming process, determine whether it is authorized, inspect process and command-line details, examine network connections and DNS activity, check persistence mechanisms, determine the initial infection vector, assess scope, contain the affected system, remove the miner, and monitor for reinfection.

---

# Key Takeaways

- Cryptocurrency is a legitimate technology and is not inherently malicious.
- Blockchain is a distributed digital ledger used for recording transactions.
- Cryptocurrency mining can consume significant computing resources.
- **Cryptojacking is the unauthorized use of computing resources for cryptocurrency mining.**
- Cryptojacking can target workstations, servers, cloud environments, containers, and Kubernetes clusters.
- High CPU or GPU usage alone does not prove cryptojacking.
- SOC analysts should correlate system, process, network, persistence, and user activity.
- Removing the miner is not enough if the initial compromise remains unresolved.
- Behavioral and contextual evidence is more useful than relying on a process name alone.

---

# Related Notes

- [Cybersecurity Fundamentals](cybersecurity-fundamentals.md)
- [Malware & Social Engineering](malware-and-social-engineering.md)
- [Threat Actors & Cyber Warfare](threat-actors-and-cyber-warfare.md)
- [Vulnerabilities & Patch Management](vulnerabilities-and-patch-management.md)
- [Incident Response](incident-response.md)
