# Cybersecurity Glossary

> A living glossary containing cybersecurity terms learned throughout my SOC Analyst roadmap.

---

# A

## Access Control

The mechanism that enforces [authorization](#authorization) decisions by allowing or denying access to resources.

> [!TIP]
> **Think of it as:** The security guard that carries out the decision.

**SOC relevance:** Access control logs — who accessed what, when, and whether it was denied — are a core data source for spotting unauthorized access attempts and privilege misuse.

**See also:** [Identity and Access Management (IAM)](notes/security-fundamentals/identity-and-access-management.md)

---

## Application Layer

The topmost layer of the network stack, where application-level protocols such as HTTP, HTTPS, DNS, and DHCP operate.

> [!TIP]
> **Think of it as:** Where the actual conversation content lives — the letter itself, not the envelope or the delivery truck.

**SOC relevance:** HTTP and HTTPS traffic at this layer represent real web application activity, making it a key source of evidence during investigations of web-based attacks.

**See also:** [Networking Fundamentals](notes/networking/networking-fundamentals.md)

---

## APT (Advanced Persistent Threat)

A sophisticated attacker — often a nation-state or well-funded group — that gains long-term unauthorized access to a network while actively avoiding detection.

> [!TIP]
> **Think of it as:** A spy who moves in and stays hidden for months, not a thief who smashes a window and runs.

**Examples**

- Nation-state espionage campaigns
- Long-term supply-chain compromises
- Multi-stage intrusions that persist for months or years

**SOC relevance:** Analysts look for the subtle, long-running indicators of APT activity — unusual login times, slow data exfiltration, persistence mechanisms — rather than the loud alerts a smash-and-grab attack would trigger.

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

**SOC relevance:** Mapping the attack surface helps analysts understand what's normal to see targeted versus what indicates a new or unexpected entry point.

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

**SOC relevance:** Failed and anomalous authentication attempts — unusual times, locations, or volumes — are among the most common SOC alert triggers.

**See also:** [Authentication & Authorization](notes/security-fundamentals/authentication-and-authorization.md)

---

## Authorization

The process of determining what an authenticated identity is allowed to access or perform.

> [!NOTE]
> **Question answered:** What are you allowed to do?

**SOC relevance:** Reviewing authorization logs helps analysts spot privilege escalation or access to resources beyond what an account should normally reach.

**See also:** [Authentication & Authorization](notes/security-fundamentals/authentication-and-authorization.md)

---

# B

## Backdoor

A hidden method of bypassing normal authentication to maintain unauthorized access to a system.

> [!TIP]
> **Think of it as:** A spare key hidden under the mat that only the attacker knows about.

> [!NOTE]
> **Often follows:** An initial [Exploit](#exploit) or [Trojan](#trojan) infection, giving the attacker a way back in later.

**SOC relevance:** Backdoors are often what persistence-hunting during incident response is looking for — unexplained services, scheduled tasks, or listening ports left behind after an initial compromise.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Bot

A compromised device that is remotely controlled by an attacker, usually without the owner's knowledge.

**Examples**

- Infected personal computers
- Compromised IoT cameras and routers

**SOC relevance:** Identifying bot behavior — beaconing to a C2 server, unusual outbound traffic patterns — is a common detection goal in network monitoring.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Botnet

A collection of [Bots](#bot) controlled by a single attacker, typically through a command-and-control (C2) server.

**Used for**

- [DDoS attacks](#ddos-distributed-denial-of-service)
- Spam campaigns
- [Cryptojacking](#cryptojacking)

**SOC relevance:** Botnet C2 traffic patterns — regular beacon intervals, known bad IPs or domains — are a frequent target of threat-intel-driven detection rules.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Brute Force Attack

An attack that tries every possible password against a single account until the correct one is found.

> [!NOTE]
> **Compare to Password Spraying:** Brute Force hits *one account* with *many passwords* (risking lockout); [Password Spraying](#password-spraying) hits *many accounts* with *one password* (avoiding lockout).

**SOC relevance:** A spike in failed logins against a single account is a classic brute force indicator that SIEM correlation rules are built to catch.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

# C

## CIA Triad

The three fundamental principles of information security.

- **Confidentiality** – Prevent unauthorized disclosure of information.
- **Integrity** – Ensure information is accurate and cannot be altered without authorization.
- **Availability** – Ensure systems and data remain accessible when needed.

**SOC relevance:** Every incident response decision maps back to protecting one or more of these three properties — it's the lens analysts use to judge impact.

**See also:** [CIA Triad and Basic Security Concepts](notes/security-fundamentals/cia-triad-and-basic-security-concepts.md)

---

## CIDR (Classless Inter-Domain Routing)

A classless IP addressing method that uses a prefix length, such as `/24`, to define the network boundary of an IP address.

> [!TIP]
> **Think of it as:** The `/24` tells you how much of the IP address belongs to the network and how much is available for hosts.

**SOC relevance:** Understanding CIDR notation lets analysts quickly tell whether two IPs belong to the same network segment when scoping an incident.

**See also:** [Subnetting & CIDR](notes/networking/subnetting-and-cidr.md)

---

## Credential Stuffing

An attack that uses usernames and passwords leaked from previous data breaches to gain unauthorized access.

> [!NOTE]
> **Works because:** Many users reuse passwords across multiple websites.

**SOC relevance:** Login attempts using known-breached credential pairs are detectable by correlating failed and successful logins against threat-intel breach data.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## Cryptocurrency

Digital currency secured using cryptography.

> [!NOTE]
> **Why it matters here:** It's the usual payment demanded by [Ransomware](#ransomware) and the resource stolen by [Cryptojacking](#cryptojacking), since it's hard to trace.

**SOC relevance:** Wallet addresses and blockchain transactions tied to ransom payments or cryptojacking proceeds can sometimes be tracked as part of an investigation.

**See also:** [Cryptocurrency & Cryptojacking](notes/security-fundamentals/cryptocurrency-and-cryptojacking.md)

---

## Cryptojacking

Unauthorized use of someone else's computing resources to mine cryptocurrency.

> [!TIP]
> **Think of it as:** Someone secretly plugging their generator into your outlet and running up your electric bill.

**SOC relevance:** Sudden, sustained CPU or GPU usage spikes on endpoints with no legitimate workload explanation are a common cryptojacking indicator.

**See also:** [Cryptocurrency & Cryptojacking](notes/security-fundamentals/cryptocurrency-and-cryptojacking.md)

---

## CVE (Common Vulnerabilities and Exposures)

A unique identifier assigned to a publicly disclosed vulnerability.

> [!TIP]
> **Think of it as:** A vulnerability's official case number — e.g., CVE-2021-44228 (Log4Shell).

**SOC relevance:** Matching detected software versions against known CVEs helps analysts prioritize which alerts represent an actively exploitable weakness.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## CVSS (Common Vulnerability Scoring System)

A standardized system for rating vulnerability severity on a scale of 0–10.

> [!NOTE]
> **Used with CVE to:** Decide which vulnerabilities to patch first.

**SOC relevance:** CVSS scores help analysts and patch teams triage a backlog of vulnerabilities when there isn't time to fix everything at once.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## Cyberattack

An attempt to compromise a system, network, or data.

**SOC relevance:** Recognizing the early stages of a cyberattack — reconnaissance, initial access — before it escalates is the core job of a SOC.

---

## Cybercrime

Illegal activities performed using computers or networks.

**SOC relevance:** Understanding the criminal motive behind an attack — financial fraud vs. extortion vs. data theft — can shape how an incident is investigated and reported.

---

## Cybersecurity

The practice of protecting digital assets from cyber threats.

**SOC relevance:** The SOC exists to operationalize this practice — turning the general goal of "protecting digital assets" into daily detection and response work.

**See also:** [What Cybersecurity Actually Is](notes/security-fundamentals/what-cybersecurity-actually-is.md), [Cybersecurity Fundamentals](notes/security-fundamentals/cybersecurity-fundamentals.md)

---

# D

## Data Breach

Unauthorized access, disclosure, modification, or destruction of protected information.

> [!NOTE]
> **Often the final stage of:** The [Identity Attack Flow](#identity-attack-flow) (see Revision Notes below).

**SOC relevance:** Confirming whether an incident actually resulted in a breach — versus an attempted-but-blocked intrusion — determines legal and regulatory reporting obligations.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## DDoS (Distributed Denial of Service)

A DoS attack launched from many compromised systems simultaneously, usually a [Botnet](#botnet).

**SOC relevance:** DDoS traffic shows up as sudden abnormal volume from many source IPs, distinguishing it from a single-source DoS attack.

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

## Default Gateway

The device a host sends traffic to when the destination is outside the host's local subnet.

```text
Windows VM
192.168.27.128/24
        |
        | Remote destination
        v
Default Gateway
192.168.27.2
```

The gateway provides the next hop for traffic that cannot be delivered directly on the local network.

**SOC relevance:** Understanding the default gateway helps an analyst understand how traffic leaves a host's local network and where routing should occur.

**See also:** [Routing & Network Segmentation](notes/networking/routing-and-network-segmentation.md)

---

## DHCP (Dynamic Host Configuration Protocol)

A protocol that automatically provides network configuration to devices, such as an IP address, subnet information, default gateway, and DNS server.

> [!TIP]
> **Think of it as:** The network's receptionist that gives a device the information it needs to join the network.

**SOC relevance:** Unexpected or rogue DHCP servers on a network can be used to redirect victims to malicious gateways or DNS servers, making DHCP logs worth reviewing during network anomalies.

**See also:** [DHCP & DNS](notes/networking/dhcp-and-dns.md)

---

## Dictionary Attack

A password attack that uses a list of common words and passwords rather than every possible combination.

> [!TIP]
> **Think of it as:** A focused subtype of [Brute Force](#brute-force-attack) — trying the likely words first instead of every combination.

**SOC relevance:** Dictionary-attack traffic often shows the same signature as brute force — many failed logins in a short window — so it's usually caught by the same detection rules.

---

## DNS (Domain Name System)

A system that translates domain names into network information, such as IP addresses, allowing devices to locate services using human-readable names.

> [!TIP]
> **Think of it as:** The Internet's phonebook: you provide a name and DNS helps find its address.

**SOC relevance:** DNS query logs are one of the richest sources of evidence for identifying malicious domains, DGA activity, and command-and-control beaconing.

**See also:** [DHCP & DNS](notes/networking/dhcp-and-dns.md)

---

## DNS Resolution

The process of obtaining DNS information for a domain name, commonly including the IP address or addresses associated with that domain.

```text
example.com
     ↓
DNS resolution
     ↓
IPv4 / IPv6 addresses
```

**SOC relevance:** DNS resolution provides useful evidence about domains an endpoint is attempting to contact and can help identify suspicious domains, DGA activity, and DNS tunneling.

**See also:** [DHCP & DNS](notes/networking/dhcp-and-dns.md)

---

## DNS Resolver

A system or service responsible for resolving domain names into IP addresses by performing DNS queries.

**SOC relevance:** Knowing which resolver a host uses helps analysts trace where a DNS query actually went before concluding it reached — or bypassed — approved DNS infrastructure.

**See also:** [DHCP & DNS](notes/networking/dhcp-and-dns.md)

---

## DNS Stub Resolver

A local DNS interface that applications on a host can query, which forwards requests to an upstream DNS resolver.

> [!NOTE]
> **Example:** On Ubuntu, `127.0.0.53` is the local stub resolver address provided by `systemd-resolved`.

**SOC relevance:** Recognizing stub resolver traffic — such as queries to `127.0.0.53` — prevents an analyst from mistaking normal local DNS forwarding for suspicious loopback activity.

**See also:** [DHCP & DNS](notes/networking/dhcp-and-dns.md)

---

## DNS Upstream Server

A DNS server that a local resolver forwards DNS queries to when it cannot answer them from local information or cache.

> [!NOTE]
> **Example:** `resolvectl status` on an Ubuntu host might show upstream DNS servers configured as `192.168.1.1` and `fe80::1`.

**SOC relevance:** Comparing the configured upstream DNS server against what's actually being queried can reveal DNS hijacking or a misconfigured/rogue resolver.

**See also:** [DHCP & DNS](notes/networking/dhcp-and-dns.md)

---

## DoS (Denial of Service)

An attack intended to make a service unavailable.

**SOC relevance:** A DoS is typically identified by a sudden resource exhaustion or availability drop traced back to a single source, unlike DDoS's many sources.

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

# E

## Echo Request / Echo Reply

ICMP messages commonly used by the `ping` utility to test whether a destination responds to network-level diagnostic traffic.

```text
Client
  │
  │ ICMP Echo Request
  ▼
Server
  │
  │ ICMP Echo Reply
  ▼
Client
```

**SOC relevance:** ICMP Echo traffic can provide evidence that a host responded to network-level probes, but a successful ping does not prove that a particular TCP or UDP service is reachable.

**See also:** [ICMP (Internet Control Message Protocol)](#icmp-internet-control-message-protocol)

---

## Ephemeral Port

A temporary source port, typically assigned by the operating system, to the client side of an outgoing network connection.

```text
192.168.1.25:53142
        │
        │ TCP
        ↓
192.168.1.20:443
```

Here, `53142` is the ephemeral source port, while `443` is the destination service port.

> [!TIP]
> **Think of it as:** A temporary return address used by a client for a particular conversation.

**SOC relevance:** Recognizing which side of a connection used an ephemeral port helps analysts tell client traffic from server traffic when reading raw connection logs.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

## Exploit

A technique, script, or piece of code that takes advantage of a [Vulnerability](#vulnerability) to perform unauthorized actions.

> [!NOTE]
> **Related:** A [Zero-Day](#zero-day) exploit targets a vulnerability with no patch available yet.

**SOC relevance:** Detecting exploit attempts often relies on signature- or behavior-based rules tied to a specific CVE or technique.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

# F

## FQDN (Fully Qualified Domain Name)

A complete DNS name that identifies a domain within the DNS hierarchy.

> [!NOTE]
> **Note:** A trailing dot, such as `google.com.`, represents the DNS root and indicates an absolute DNS name.

**SOC relevance:** Working with FQDNs precisely, rather than partial hostnames, avoids ambiguity when correlating DNS logs across different tools.

**See also:** [DHCP & DNS](notes/networking/dhcp-and-dns.md)

---

# H

## Hacktivist

An attacker motivated by political or ideological beliefs rather than money.

**SOC relevance:** Attributing an attack to a hacktivist group, versus a financially motivated crime, can change how an incident is prioritized and who else needs to be notified.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## HTTP (Hypertext Transfer Protocol)

An application-layer protocol used for communication between web clients and servers. HTTP defines how requests and responses are structured and exchanged.

**SOC relevance:** Unencrypted HTTP traffic can provide visibility into requests, URLs, headers, parameters, and other web activity.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

## HTTPS (Hypertext Transfer Protocol Secure)

HTTP communication protected by TLS. HTTPS provides confidentiality, integrity protection, and server authentication for the communication channel.

> [!IMPORTANT]
> **Important:** HTTPS does not mean that a website is trustworthy — encryption protects the channel, not the intent of what's on the other end.

**SOC relevance:** Attackers can also use HTTPS for C2 and data transfer. Encrypted content may be hidden, but network metadata, DNS activity, TLS information, and endpoint telemetry can still provide investigation evidence.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

# I

## IAM (Identity and Access Management)

A framework used to manage digital identities and control access to organizational resources.

**SOC relevance:** IAM logs — role assignments, permission changes — are a key place to spot privilege creep or unauthorized access grants.

**See also:** [Identity and Access Management (IAM)](notes/security-fundamentals/identity-and-access-management.md)

---

## ICMP (Internet Control Message Protocol)

A network-layer protocol used for diagnostic, error-reporting, and control messages between network devices.

> [!TIP]
> **Think of it as:** A way for network devices to report conditions or ask diagnostic questions, such as whether a host responds to an Echo Request.

> [!NOTE]
> **Common use:** The `ping` command commonly uses ICMP Echo Request and Echo Reply messages to test reachability.

**SOC relevance:** ICMP activity can help analysts investigate host discovery, network troubleshooting, scanning, and unusual ICMP-based communication.

**See also:** [Networking Fundamentals](notes/networking/networking-fundamentals.md), [Protocol Comparison](notes/networking/protocol-comparison.md)

---

## Identity

A digital representation of a user, device, application, or service that can authenticate to a system.

**Examples**

- User account
- Service account
- Application
- Server
- Virtual machine

**SOC relevance:** Every alert eventually traces back to an identity — the analyst's job is often to determine whether that identity's behavior was legitimate.

---

## Identity Theft

Unauthorized use of another person's identity.

**SOC relevance:** Confirming identity theft usually requires correlating credential misuse across multiple systems rather than relying on a single alert.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## Insider Threat

A threat originating from someone with legitimate organizational access.

> [!TIP]
> **Think of it as:** The danger isn't someone breaking in — they already have a key.

**SOC relevance:** Insider threats are harder to detect with perimeter tools since the activity comes from legitimate credentials — behavioral baselining (UEBA) is often needed instead.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## Integrity

Ensuring information remains accurate and unaltered.

> [!NOTE]
> **Note:** One of the three pillars of the [CIA Triad](#cia-triad) — see that entry for the full picture alongside Confidentiality and Availability.

**SOC relevance:** File integrity monitoring and hash comparisons are the practical tools SOCs use to detect unauthorized changes to critical files.

**See also:** [CIA Triad and Basic Security Concepts](notes/security-fundamentals/cia-triad-and-basic-security-concepts.md)

---

## IoT (Internet of Things)

Internet-connected physical devices such as smart cameras, TVs, and appliances.

> [!NOTE]
> **Watch out:** Weak default passwords on IoT devices make them a favorite target for recruiting [Bots](#bot) into a [Botnet](#botnet).

**SOC relevance:** IoT devices often lack proper logging, making network-level monitoring — rather than the device itself — the main way to detect a compromised one.

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

# K

## Keylogger

Spyware that records every keystroke entered by a user.

**SOC relevance:** Endpoint detection tools look for keylogger-like behavior, such as low-level keyboard hooking or unusual process access to input APIs.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# L

## Least Privilege (PoLP)

A security principle stating that users should receive only the minimum permissions required to perform their job.

**Benefits**

- Reduces attack surface
- Limits damage after compromise
- Prevents privilege abuse

**SOC relevance:** Auditing accounts against least privilege helps analysts and admins spot over-permissioned accounts before they become a bigger risk if compromised.

---

## Local Network

The network segment that a host considers directly reachable based on its IP address and subnet mask.

```text
192.168.27.128/24
192.168.27.1/24
```

Both addresses belong to `192.168.27.0/24`, so they are on the same local subnet.

**SOC relevance:** Determining whether a destination is local or remote helps analysts understand expected network paths and identify unusual communication patterns.

**See also:** [Routing & Network Segmentation](notes/networking/routing-and-network-segmentation.md)

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

**SOC relevance:** Malware detection ranges from signature matching known families to behavioral detection of what the malware actually does once running.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Meltdown

A CPU vulnerability allowing unauthorized access to protected memory.

**SOC relevance:** Meltdown-class vulnerabilities are typically addressed through patching and monitoring rather than direct detection, since exploitation leaves little network trace.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## MFA (Multi-Factor Authentication)

An authentication method requiring two or more authentication factors.

**Authentication factors include:**

- Something you know
- Something you have
- Something you are

**SOC relevance:** MFA fatigue attacks — repeated push notifications until a user accepts one — are a modern SOC concern worth building detections for.

**See also:** [Authentication & Authorization](notes/security-fundamentals/authentication-and-authorization.md)

---

## MitMo (Man-in-the-Mobile)

A mobile-focused attack that intercepts authentication codes or banking communications.

**SOC relevance:** Investigating MitMo usually involves mobile carrier and SMS logs alongside standard authentication logs.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## MITM / On-Path Attack

An attack where the attacker secretly intercepts communication between two parties.

> [!TIP]
> **Think of it as:** Someone tapping the phone line between you and your bank.

**SOC relevance:** Detecting MITM activity often relies on spotting unexpected certificate changes, ARP anomalies, or unusual routing.

**See also:** [Network Security](notes/security-fundamentals/network-security.md)

---

# N

## NAT (Network Address Translation)

A technique that translates IP addresses between different network addressing contexts, commonly allowing multiple private devices to share a public IP address when accessing the Internet.

```text
Windows VM
192.168.27.128
        |
        v
VMware NAT
        |
        v
Ubuntu Host
        |
        v
Home Router
        |
        v
Internet
```

**SOC relevance:** NAT is important when interpreting network telemetry because the address observed at different points in a network may differ. An analyst should understand the network architecture before assuming that an observed IP represents a directly reachable physical host.

**See also:** [IP Addressing](notes/networking/ip-addressing.md)

---

## NAT Gateway

The network component that performs or participates in Network Address Translation between a private network and another network.

```text
Windows VM
192.168.27.128
        |
        v
NAT Gateway
192.168.27.2
```

The Windows VM uses `192.168.27.2` as its default gateway for traffic destined outside its local subnet.

**SOC relevance:** Understanding the NAT gateway helps analysts reconstruct network paths and understand how traffic from virtual machines reaches external networks.

**See also:** [IP Addressing](notes/networking/ip-addressing.md)

---

## Network Segmentation

The practice of dividing a network into separate logical or physical zones to control communication and limit the potential impact of a compromise.

**SOC relevance:** Segmentation can restrict lateral movement and separate sensitive systems from general user networks, making it a key control analysts check when scoping how far an intrusion could spread.

**See also:** [Routing & Network Segmentation](notes/networking/routing-and-network-segmentation.md), [Network Security](notes/security-fundamentals/network-security.md)

---

## Network Troubleshooting

A systematic process of identifying where communication is failing by testing different layers and components, such as local configuration, IP connectivity, DNS resolution, TCP/service connectivity, TLS, and the application layer.

**SOC relevance:** The same layer-by-layer approach used to troubleshoot connectivity issues is useful for isolating where in the network path a security event actually occurred.

---

# P

## Packet

A unit of data carried across a network. At the network layer, an IP packet contains information such as source and destination IP addresses along with the data being transported.

> [!TIP]
> **Think of it as:** A packaged piece of information being sent across a network.

**SOC relevance:** Packet-level information can help analysts understand communication between endpoints, identify suspicious traffic, and investigate network-based attacks.

**See also:** [Networking Fundamentals](notes/networking/networking-fundamentals.md)

---

## Packet Loss

The percentage of transmitted packets for which no response is received. Packet loss can indicate connectivity problems, congestion, filtering, or temporary network conditions.

**SOC relevance:** Unexplained packet loss during an investigation can indicate active filtering or a security control, like an IPS, dropping malicious traffic.

---

## Password Spraying

An attack that attempts one commonly used password against many different user accounts.

> [!NOTE]
> **Unlike Brute Force:** It spreads attempts across many accounts instead of hammering one, which helps avoid account lockouts.

**SOC relevance:** Password spraying shows up in logs as many accounts each getting one or two failed logins around the same time — a pattern that's easy to miss if only watching for per-account lockouts.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## Phishing

A social engineering attack that uses fraudulent messages to trick users into revealing sensitive information — such as usernames, passwords, or MFA codes — or to deliver malware.

**SOC relevance:** Phishing reports and email gateway logs are usually the first signal a SOC gets before a broader compromise is discovered.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Port

A logical number used to identify a specific service or application endpoint on a network host. Ports range from `0` to `65535`.

> [!TIP]
> **Think of it as:** An IP address tells you which building you're communicating with, while a port tells you which door or service you're trying to reach.

**Common examples**

- `22` → SSH
- `53` → DNS
- `80` → HTTP
- `443` → HTTPS
- `445` → SMB
- `3389` → RDP

> [!IMPORTANT]
> **Important:** A port number does not guarantee which protocol or application is actually running there — it indicates only the expected service endpoint.

**SOC relevance:** Destination ports help analysts identify what type of service a host is attempting to communicate with and provide context during investigations involving scanning or suspicious connections.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

## Port Scan

A technique that probes multiple ports on a host or multiple hosts to determine which network services may be accessible.

> [!TIP]
> **Think of it as:** Checking many doors on a building to find out which ones are open.

**SOC relevance:** Port scanning can indicate reconnaissance. However, vulnerability scanners, asset discovery tools, and authorized security assessments can generate similar traffic, so the activity must be investigated in context.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md), [Network Security](notes/security-fundamentals/network-security.md)

---

## Pretexting

Creating a fabricated scenario to convince a victim to reveal information.

**SOC relevance:** Pretexting attacks are hard to catch technically — awareness training and verification procedures are usually the actual control, with the SOC relying on user reports.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Private IP Address

An IP address reserved for use within private networks and not directly routable across the public Internet.

**Private IPv4 ranges**

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

**SOC relevance:** Recognizing private IP ranges helps analysts quickly tell internal traffic from traffic that has actually left the network.

**See also:** [IP Addressing](notes/networking/ip-addressing.md)

---

## Protocol

A defined set of rules that determines how systems communicate and exchange data over a network. Examples include TCP, UDP, HTTP, HTTPS, DNS, SSH, and SMB.

> [!TIP]
> **Think of it as:** The language and rules that two systems agree to use when communicating.

**SOC relevance:** Knowing which protocol is expected on a given port helps analysts spot protocol mismatches — like non-HTTP traffic on port 80 — a common sign of evasion.

**See also:** [Protocol Comparison](notes/networking/protocol-comparison.md), [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

# Q

## QUIC (Quick UDP Internet Connections)

A modern transport protocol built on UDP that provides features such as reliable delivery, encryption integration, and multiplexed streams. QUIC is used by HTTP/3.

> [!NOTE]
> **Note:** Detailed QUIC and HTTP/3 behavior is still an emerging area to revisit as it becomes more relevant.

**SOC relevance:** QUIC is important because modern web traffic increasingly runs over UDP rather than the traditional TCP transport model, which can affect how network monitoring tools classify traffic.

---

## Quid Pro Quo

Offering something in exchange for sensitive information.

**SOC relevance:** Like other social engineering tactics, quid pro quo attempts are usually caught through user reporting rather than technical detection alone.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# R

## Ransomware

Malware that encrypts a victim's files and demands payment — usually in [Cryptocurrency](#cryptocurrency) — for the decryption key.

**SOC relevance:** Mass file modification/encryption events and sudden shadow-copy deletion are classic ransomware indicators SOC playbooks watch for.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## RDP (Remote Desktop Protocol)

A protocol commonly used to provide graphical remote access to Windows systems. RDP commonly uses TCP port `3389`.

**SOC relevance:** Unexpected Internet-originated RDP activity can warrant investigation, since RDP is a common entry point for ransomware operators.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

## Remote Network

A network that is outside the host's local subnet.

For example:

```text
Windows VM:  192.168.27.128/24
Destination: 192.168.1.1
```

The destination belongs to `192.168.1.0/24`, while the Windows VM belongs to `192.168.27.0/24` — so the destination is remote from the Windows VM's perspective. Traffic to a remote network is sent toward the default gateway.

**SOC relevance:** Determining whether communication is local or remote helps an analyst understand expected routing behaviour and network paths.

**See also:** [Routing & Network Segmentation](notes/networking/routing-and-network-segmentation.md)

---

## Risk

The likelihood that a threat will exploit a vulnerability, combined with the potential impact if it occurs.

**SOC relevance:** Risk framing — likelihood × impact — is how analysts and management decide which findings need urgent action versus a ticket for later.

**See also:** [Risk Management](notes/security-fundamentals/risk-management.md)

---

## Rootkit

Malware designed to hide itself or other malicious software on a system.

> [!NOTE]
> **Often paired with:** A [Backdoor](#backdoor), so the attacker can keep returning undetected.

**SOC relevance:** Rootkits are designed to evade standard OS-level tools, so detection often relies on offline forensic analysis or specialized anti-rootkit scanners.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Routing

The process of determining where network traffic should be forwarded to reach its destination.

> [!TIP]
> **Think of it as:** Choosing the road a packet needs to take to reach another network.

**SOC relevance:** Understanding routing paths helps analysts figure out where in a network an attacker's traffic would have passed through, and where visibility gaps might exist.

**See also:** [Routing & Network Segmentation](notes/networking/routing-and-network-segmentation.md)

---

## RTT (Round-Trip Time)

The amount of time required for a packet to travel from the source to the destination and for the response to return.

**SOC relevance:** Unusual RTT patterns can sometimes hint at traffic being routed through an unexpected path, such as a proxy or VPN.

---

# S

## SEO Poisoning

Manipulating search engine results to direct victims to malicious websites.

**SOC relevance:** SEO poisoning campaigns are usually flagged through web filtering and reputation feeds rather than internal log analysis alone.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Session Hijacking

An attack where an attacker steals an authenticated session token or cookie to impersonate a legitimate user.

**SOC relevance:** Session hijacking can sometimes be spotted by a session token suddenly being used from a new, geographically implausible location.

**See also:** [Identity Attacks](notes/security-fundamentals/identity-attacks.md)

---

## SIEM

**Security Information and Event Management**

A platform that collects, analyzes, correlates, and monitors security logs from multiple sources to detect suspicious activity.

**SOC relevance:** The SIEM is the SOC analyst's primary workspace — where logs from across the environment are correlated into the alerts an analyst actually triages.

---

## SMB (Server Message Block)

A network protocol commonly used in Windows environments for file and printer sharing and other network resource access. SMB commonly uses TCP port `445`.

**SOC relevance:** SMB is important in SOC investigations because attackers may abuse it for lateral movement across a compromised network.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

## Smishing

Phishing conducted through SMS messages.

**SOC relevance:** Smishing incidents typically reach the SOC through user reports rather than network telemetry, since the delivery channel is outside corporate infrastructure.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## SNI (Server Name Indication)

A field in the TLS handshake that allows a client to indicate the hostname it is attempting to connect to.

```text
Client → Server

TLS SNI:
example.com
```

**SOC relevance:** SNI can provide visibility into the hostname associated with encrypted TLS traffic even when the application data itself cannot be read.

**See also:** [Protocol Comparison](notes/networking/protocol-comparison.md), [Network Security](notes/security-fundamentals/network-security.md)

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

**SOC relevance:** Since social engineering targets people rather than systems, the SOC's role is often limited to detecting the aftermath — a compromised account, a delivered payload — rather than the manipulation itself.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Spectre

A CPU vulnerability exploiting speculative execution to leak sensitive information.

**SOC relevance:** Like Meltdown, Spectre-class issues are mitigated through patching and microcode updates rather than caught by traditional network or log-based detection.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

## Spyware

Malware that secretly collects user information.

**SOC relevance:** Spyware detection often relies on endpoint monitoring for unusual data collection behavior or unexpected outbound connections.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Stuxnet

A sophisticated worm that targeted Iranian industrial control systems — widely considered one of the first cyberweapons used in the real world.

**SOC relevance:** Stuxnet is a useful case study for understanding how a threat can bridge the IT/OT gap — a consideration for SOCs covering industrial environments.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## Subnet

A smaller logical network created by dividing an IP network into separate address ranges — it defines which IP addresses are considered part of the same local network.

```text
192.168.27.0/24
```

In the SOC lab, `192.168.1.0/24` is the physical home LAN, while `192.168.27.0/24` is the VMware VMnet8 network.

> [!NOTE]
> **Why it matters:** Subnets create network boundaries that help organizations organize infrastructure and control communication between network zones.

**SOC relevance:** Subnets help analysts determine whether traffic is local or requires routing, and provide context when investigating network connections.

**See also:** [Subnetting & CIDR](notes/networking/subnetting-and-cidr.md)

---

# T

## TCP (Transmission Control Protocol)

A connection-oriented transport protocol that provides reliable and ordered delivery of data. TCP can retransmit lost data and ensures that data is delivered to the application in the correct order.

> [!NOTE]
> **Establishing a connection:** TCP commonly uses a three-way handshake — see [TCP Three-Way Handshake](#tcp-three-way-handshake) for the full diagram.

**SOC relevance:** TCP connection information helps analysts investigate scanning, suspicious connections, SYN floods, and unusual network behavior.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md), [Protocol Comparison](notes/networking/protocol-comparison.md)

---

## TCP Connection

A communication session established between TCP endpoints after successful TCP connection establishment.

> [!IMPORTANT]
> **SOC interpretation:** A successful TCP handshake demonstrates that TCP connection establishment succeeded for the specified endpoints and port. It does not by itself prove that meaningful application data was exchanged or that the service is legitimate.

**SOC relevance:** Analysts should treat a completed handshake as evidence of reachability, not proof of a benign or successful application-level interaction.

**See also:** [TCP Three-Way Handshake](#tcp-three-way-handshake), [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

## TCP Three-Way Handshake

The process used by TCP to establish a connection using three messages: SYN, SYN-ACK, and ACK.

```text
Client          Server

  SYN --------->

      <--------- SYN-ACK

  ACK --------->
```

**SOC relevance:** Recognizing a normal handshake pattern makes it easier to spot abnormal ones — like SYN floods, where the handshake never completes.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md)

---

## Threat

Anything capable of exploiting a vulnerability and causing harm to a system, network, or organization.

**Examples**

- Hacker
- [Malware](#malware)
- [Insider Threat](#insider-threat)
- Natural Disaster

**SOC relevance:** Classifying something as a threat, versus a vulnerability or just a risk, shapes whether the SOC's response is detection-focused or hardening-focused.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## Threat Actor

An individual or group capable of carrying out cyberattacks.

**SOC relevance:** Attributing activity to a specific threat actor or group, when possible, helps predict their likely next moves based on known tactics.

**See also:** [Threat Actors & Cyber Warfare](notes/security-fundamentals/threat-actors-and-cyber-warfare.md)

---

## TLS (Transport Layer Security)

A cryptographic protocol used to protect network communications by providing confidentiality, integrity protection, and authentication.

> [!NOTE]
> **Note:** In HTTPS, TLS protects HTTP communication while it travels across the network.

**SOC relevance:** TLS metadata, certificates, connection patterns, and endpoint telemetry can help investigate encrypted communications even when the payload itself is unreadable.

**See also:** [Protocol Comparison](notes/networking/protocol-comparison.md)

---

## TLS Handshake

The negotiation process used by TLS to establish the parameters needed to protect communication between a client and server, including negotiating cryptographic parameters and the server presenting a certificate for authentication.

**SOC relevance:** TLS handshake metadata, certificates, SNI, TLS versions, and connection patterns can provide useful evidence when investigating encrypted network traffic.

**See also:** [Protocol Comparison](notes/networking/protocol-comparison.md)

---

## Trojan

Malware disguised as legitimate software, tricking the user into installing it themselves.

**SOC relevance:** Trojans often rely on the user being convinced to run them, so email and download-source analysis are as important as endpoint detection.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# U

## UDP (User Datagram Protocol)

A connectionless transport protocol that provides lower-overhead communication without TCP's built-in mechanisms for reliable and ordered delivery. It's commonly used for DNS, real-time applications, VoIP, and online gaming.

**SOC relevance:** UDP traffic can be relevant when investigating DNS activity, DDoS amplification, unusual traffic volumes, and unexpected services.

**See also:** [Ports & Protocols](notes/networking/ports-and-protocols.md), [Protocol Comparison](notes/networking/protocol-comparison.md)

---

# V

## Virtual Network

A software-defined network created within or by a system such as a hypervisor.

```text
Ubuntu Host
      |
      v
VMware VMnet8
192.168.27.0/24
      |
      v
Windows VM
```

A virtual network can provide its own addressing, interfaces, routing, DHCP, and NAT services.

**SOC relevance:** SOC analysts may encounter virtual machines and cloud or virtualized infrastructure in enterprise environments. Understanding virtual networking helps explain where an endpoint exists and how its traffic reaches other networks.

**See also:** [Routing & Network Segmentation](notes/networking/routing-and-network-segmentation.md)

---

## Virus

Malware that attaches itself to another file and requires user execution to spread.

> [!NOTE]
> **Compare to Worm:** A Virus needs a human to run the infected file; a [Worm](#worm) spreads on its own.

**SOC relevance:** Virus infections typically require user execution, so email attachments and download logs are common starting points for tracing patient zero.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Vishing

Phishing conducted through voice calls.

**SOC relevance:** Vishing incidents are usually surfaced through employee reports rather than technical logs, since the attack happens over a phone call.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## VMnet8

VMware Workstation's virtual network interface commonly used for NAT networking — a virtual networking component, not a physical switch or physical network.

```text
VMnet8:               192.168.27.0/24
Host-side interface:  192.168.27.1
NAT Gateway:          192.168.27.2
Windows VM:           192.168.27.128
```

**SOC relevance:** Understanding VMnet8 helps distinguish the virtual network from the physical LAN when interpreting network activity generated by the Windows VM.

**See also:** [Routing & Network Segmentation](notes/networking/routing-and-network-segmentation.md)

---

## Vulnerability

A weakness in a system, application, process, or configuration that can be exploited by a threat.

**Examples**

- Unpatched software
- Weak password
- Misconfigured firewall

**SOC relevance:** Vulnerability data from scanners or CVE feeds is often correlated with exploit-attempt alerts to judge how serious an alert really is.

**See also:** [Vulnerabilities & Patch Management](notes/security-fundamentals/vulnerabilities-and-patch-management.md)

---

# W

## Whaling

A phishing attack that specifically targets executives.

**SOC relevance:** Whaling attempts targeting executives often warrant faster escalation given the potential access and authority of the target.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

## Worm

Self-replicating malware that spreads automatically without requiring user action.

> [!NOTE]
> **Compare to Virus:** A Worm doesn't need a human to run it — it spreads on its own across a network, unlike a [Virus](#virus).

**SOC relevance:** Because worms self-propagate, a SOC spotting one instance should immediately check for lateral spread across the network.

**See also:** [Malware & Social Engineering](notes/security-fundamentals/malware-and-social-engineering.md)

---

# Z

## Zero Trust

A security model based on the principle:

> Zero Trust principle
> Never Trust. Always Verify.

Every user, device, and access request must be verified before access is granted.

**SOC relevance:** Zero Trust principles shape how SOCs design detection — every access request is worth verifying and logging, not just ones crossing a network perimeter.

**See also:** [Defense in Depth](notes/security-fundamentals/defense-in-depth.md)

---

## Zero-Day

A vulnerability with no available security patch at the time it is discovered or exploited.

**SOC relevance:** Zero-days are especially hard to detect via signatures, so SOCs rely more on behavioral anomaly detection to catch them before a patch exists.

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

---

## Protocol Layers Mental Model

```text
Application
    ↓
HTTP / HTTPS / DNS / DHCP

Transport
    ↓
TCP / UDP

Network
    ↓
IP
```

**See also:** [Networking Fundamentals](notes/networking/networking-fundamentals.md), [Protocol Comparison](notes/networking/protocol-comparison.md)

---

## Critical Distinctions (Protocols)

```text
TCP ≠ encryption
UDP ≠ insecure

HTTP ≠ TCP
HTTPS ≠ TCP
DNS ≠ UDP
```

Ports identify services or application endpoints. They do not inherently belong to TCP or UDP.
