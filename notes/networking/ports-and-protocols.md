---
type: concept-note
status: reviewed
tags:
  - networking
  - ports
  - protocols
  - tcp
  - udp
  - network-services
  - soc
---

# Ports & Protocols

> **Purpose**
>
> This note explains how network traffic is directed toward services using ports and how transport and application protocols work together. It focuses on the level required to interpret common SOC network telemetry without making assumptions beyond the available evidence, and covers common ports and protocols in more depth — including port ranges, well-known services, and their relevance to SOC investigations.

---

# What Is a Port?

A **port** is a numbered transport-layer endpoint used to distinguish different network services or communication endpoints on a host.

An IP address helps identify the host:

```text
192.168.20.50
```

A port identifies the particular endpoint being targeted:

```text
192.168.20.50:443
```

Therefore:

```text
IP address
    ↓
Which host?

Port
    ↓
Which service endpoint?
```

A single host can have many services communicating through different ports.

For example:

```text
Server
192.168.20.50

22    → SSH
53    → DNS
80    → HTTP
443   → HTTPS
445   → SMB
3389  → RDP
```

These port numbers are **commonly associated** with those services.

A port number alone does not guarantee which application protocol is actually being used.

---

# Source and Destination Ports

A network connection can contain both a source and destination port.

For example:

```text
192.168.10.25:53142
        ↓
192.168.20.50:443
```

This can be interpreted as:

```text
Source:
192.168.10.25
Source Port:
53142

Destination:
192.168.20.50
Destination Port:
443
```

The source port is often an **ephemeral port** selected temporarily by the client.

The destination port is commonly the port associated with the service the client is attempting to reach.

---

# Ephemeral Ports

An **ephemeral port** is a temporary port typically used by a client for an outgoing connection. The operating system assigns it automatically when the client initiates a connection.

For example:

```text
Client
192.168.10.25:53142
        ↓
Server
192.168.20.50:443
```

Here:

```text
53142 → likely ephemeral client port
443   → destination service port
```

The exact range used for ephemeral ports depends on the operating system and configuration.

Different clients can use completely different ephemeral ports to reach the same server and port:

```text
Client A
192.168.1.15:53142
        ↓
      :443

Client B
192.168.1.20:51873
        ↓
      :443
```

Both clients can communicate with the same server and destination port at the same time. The combination of addressing and ports allows network devices to keep different communication flows separate.

For SOC analysis, the important concept is:

> **Client-side source ports are often temporary, while destination ports commonly identify the service being targeted.**

---

# Ports Are Not Services

A common shortcut is:

```text
443 = HTTPS
445 = SMB
22 = SSH
```

These associations are useful, but they should not be treated as absolute.

A service can be configured to listen on a non-standard port.

For example:

```text
HTTPS
 ↓
443
```

is conventional, but HTTPS could also be configured on another port.

Therefore:

> **A port number provides an indication of the expected service, not definitive proof of the application protocol.**

This distinction is important when interpreting SIEM and network telemetry.

---

# TCP

**TCP**, or **Transmission Control Protocol**, is a transport-layer protocol designed to provide reliable, ordered communication between endpoints.

TCP provides mechanisms including:

- Connection establishment
- Acknowledgments
- Retransmission
- Sequencing
- Flow control

A simplified TCP connection establishment process is:

```text
Client                         Server

  | -------- SYN ------------> |
  |                            |
  | <------ SYN-ACK ---------- |
  |                            |
  | -------- ACK ------------> |
  |                            |
       Connection established
```

This is commonly called the **TCP three-way handshake**.

TCP is commonly used where reliable communication is important. Examples include:

```text
HTTP
HTTPS
SSH
RDP
SMB
SMTP
```

Seeing TCP traffic does not automatically prove that a connection was successfully established. The available telemetry must show enough information to support that conclusion.

---

# UDP

**UDP**, or **User Datagram Protocol**, is a transport-layer protocol designed with lower overhead than TCP.

UDP is connectionless: it does not establish a connection in the same manner as TCP and does not provide TCP's built-in mechanisms for reliable, ordered delivery and retransmission.

UDP is commonly used where low overhead or fast communication is useful.

Examples include:

- DNS
- DHCP
- Real-time voice/video
- Online gaming
- Certain telemetry applications

UDP traffic should not automatically be considered less secure or more suspicious than TCP traffic.

Both TCP and UDP can carry legitimate or malicious traffic.

---

# TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Connection establishment | No TCP-style handshake |
| Reliable delivery mechanisms | No built-in delivery guarantee |
| Ordered data | No built-in ordering guarantee |
| Retransmission mechanisms | No built-in retransmission |
| More overhead | Lower overhead |

The choice between TCP and UDP depends on the requirements of the application.

---

# Application Protocols

TCP and UDP are **transport protocols**.

Protocols such as:

- HTTP
- HTTPS
- DNS
- SSH
- SMB

operate at higher levels and define how specific applications or services communicate.

A simplified model is:

```text
Application
    ↓
HTTP / HTTPS / DNS / SSH / SMB
    ↓
Transport
    ↓
TCP / UDP
    ↓
IP
```

For example:

```text
HTTPS
  ↓
TCP
  ↓
IP
```

or:

```text
DNS
  ↓
UDP
  ↓
IP
```

The exact protocol stack depends on the application and configuration.

---

# Common Ports and Protocols

These are common associations worth recognizing:

| Port | Protocol | Common Service | Typical Use | SOC Relevance |
|---|---|---|---|---|
| 22 | TCP | SSH | Secure remote administration | Remote administration |
| 25 | TCP | SMTP | Mail server communication | Email |
| 53 | UDP / TCP | DNS | Domain name resolution | Name resolution |
| 80 | TCP | HTTP | Web traffic | Web traffic |
| 110 | TCP | POP3 | Email retrieval | Email |
| 143 | TCP | IMAP | Email access and mailbox synchronization | Email |
| 443 | TCP | HTTPS | HTTP over TLS | Encrypted web traffic |
| 445 | TCP | SMB | Windows file and network sharing | Windows file/network sharing |
| 3389 | TCP | RDP | Windows remote desktop | Windows remote desktop |

> **Important**
>
> These are common conventions, not guarantees. A SOC analyst should avoid concluding that an application protocol was definitely used, or that activity is malicious, based on the port number alone.

---

# Reading Network Telemetry

Consider:

```text
Source:
192.168.10.25:52341

Destination:
192.168.20.50:443

Protocol:
TCP
```

The analyst can identify:

```text
Source host:
192.168.10.25

Source port:
52341

Destination host:
192.168.20.50

Destination port:
443

Transport:
TCP
```

Port `52341` is likely an ephemeral client-side port.

Port `443` is commonly associated with HTTPS.

However, the available fields do not automatically prove:

- HTTPS was used
- The TCP connection succeeded
- Data was exchanged successfully
- The activity was malicious

Additional evidence is required.

---

# Evidence vs Inference

A SOC analyst should distinguish between what network telemetry **shows** and what it **suggests**.

For example:

```text
Destination Port:
443
```

Supports:

> Traffic targeted port 443.

It does not automatically prove:

> HTTPS was definitely used.

Similarly:

```text
Protocol:
TCP
```

supports:

> TCP traffic was observed.

It does not automatically prove:

> A successful TCP session was established.

Likewise:

```text
Destination Port:
445
```

supports:

> Traffic targeted port 445, which is commonly associated with SMB.

It does not automatically prove:

> SMB was successfully used to transfer files.

---

# SOC Investigation Example

Suppose a SIEM alert contains:

```text
Source:
192.168.10.25:49152

Destination:
192.168.20.50:445

Protocol:
TCP
```

An analyst can state:

> A host at `192.168.10.25` generated TCP traffic targeting `192.168.20.50` on destination port 445.

Because port 445 is commonly associated with SMB, SMB may be relevant to the investigation.

However, the analyst should not immediately state:

> "The employee PC attacked the server using SMB."

That conclusion contains several assumptions that the available evidence does not establish.

The analyst should investigate:

- Was a TCP connection established?
- Was SMB actually negotiated?
- What process generated the traffic?
- What user was involved?
- Is this communication expected?
- Was the traffic allowed by the firewall?
- Did data transfer occur?
- Are there related authentication events?
- Are other hosts showing similar behavior?

---

# SOC Perspective

Ports and protocols allow analysts to move from:

> **Which hosts communicated?**

to:

> **Which service endpoint was targeted?**

and:

> **Which transport mechanism was involved?**

For example:

```text
Source IP
    ↓
Destination IP
    ↓
Destination Port
    ↓
TCP / UDP
    ↓
Possible Application Protocol
    ↓
Additional Evidence
```

This layered interpretation prevents analysts from making conclusions that are stronger than the available telemetry.

---

# Ports and Protocols

Network communication can be understood by looking at several pieces of information together:

```text
Source IP
    ↓
Source Port
    ↓
Protocol
    ↓
Destination IP
    ↓
Destination Port
```

For example:

```text
192.168.1.15:53142
        ↓
       TCP
        ↓
192.168.1.10:445
```

This tells us that `192.168.1.15` is communicating with `192.168.1.10` using TCP, with the destination service being accessed through port `445`.

The **IP address identifies the host**, while the **port identifies the service endpoint on that host**.

A single host can have many services listening on different ports.

---

# Port Numbers

A port is a logical endpoint used by network applications and services.

Port numbers range from:

```text
0 - 65535
```

They are commonly divided into:

| Range | Name | General purpose |
|---|---|---|
| 0 - 1023 | Well-known ports | Common system and network services |
| 1024 - 49151 | Registered ports | Applications and services |
| 49152 - 65535 | Dynamic / ephemeral ports | Temporary client-side connections |

The exact ephemeral port range can vary by operating system.

For example:

```text
Client:
192.168.1.15:53142

Server:
192.168.1.10:445
```

Here:

```text
53142 → ephemeral source port
445   → destination service port
```

The client does not need to use the same source port as another device.

---

# Port 22: SSH

**SSH** stands for **Secure Shell**.

It provides encrypted remote access to systems and is commonly used for remote administration of Linux and Unix systems.

Common related uses include:

```text
Remote login
Remote command execution
SCP file transfer
SFTP file transfer
SSH tunneling
```

### SOC Relevance

SSH exposed to the Internet can become a target for:

- Brute-force authentication attempts
- Password spraying
- Unauthorized remote access
- SSH tunneling
- Pivoting

Useful evidence includes:

```text
Source IP
Authentication attempts
Successful logins
Failed logins
Username
Time of access
Process / command activity
```

A large number of failed authentication attempts followed by a successful login would be particularly interesting to investigate.

---

# Port 53: DNS

**DNS** stands for **Domain Name System**.

DNS allows systems to resolve names into network information.

For example:

```text
example.com
     ↓
    DNS
     ↓
IP address
```

DNS commonly uses:

```text
UDP/53
```

TCP/53 can also be used in situations such as certain DNS operations and responses.

### SOC Relevance

DNS is valuable telemetry because it can reveal which domains a host is attempting to resolve.

Attackers can also abuse DNS for:

```text
Command and control
DNS tunneling
Data exfiltration
Malware infrastructure discovery
```

Useful investigation points include:

- Requested domain
- Query frequency
- Domain reputation
- Unusual subdomains
- Unusually long queries
- Host generating the queries
- Process generating the queries
- Whether subsequent connections were made to the resolved address

A DNS query to a suspicious domain is **not by itself proof of compromise**.

---

# Port 80: HTTP

**HTTP** stands for **Hypertext Transfer Protocol**.

It is commonly used for web communication.

Traditional HTTP traffic is not protected by TLS.

```text
Client
   │
   │ HTTP
   │ TCP/80
   ↓
Web Server
```

### SOC Relevance

HTTP traffic can be inspected more easily than encrypted HTTPS traffic because the application data is not protected by TLS.

Port 80 can therefore appear in:

- Normal web browsing
- Software updates
- Malware downloads
- Command and control
- Redirects to HTTPS

Port 80 itself is not malicious.

The analyst needs to examine the destination, URL, process, user, timing, and surrounding activity.

---

# Port 443: HTTPS

HTTPS is **HTTP transported over TLS**.

It provides encryption and authentication for web communication.

```text
Client
   │
   │ HTTPS
   │ TCP/443
   ↓
Web Server
```

### SOC Relevance

HTTPS is extremely common in legitimate Internet traffic, which also makes it useful for attackers attempting to blend malicious communication into normal traffic.

Because application data is encrypted, SOC analysts may rely more heavily on available metadata and supporting telemetry, such as:

- Destination IP
- Domain
- Certificate information
- Connection timing
- Traffic volume
- Connection frequency
- Endpoint process
- DNS activity
- Network reputation

Encrypted traffic is therefore **not invisible**, but payload inspection may be limited depending on the organization's monitoring architecture.

---

# Port 3389: RDP

**RDP** stands for **Remote Desktop Protocol**.

It provides graphical remote access to Windows systems.

```text
RDP Client
    │
    │ TCP/3389
    ↓
Windows Host
```

### SOC Relevance

Internet-exposed RDP is a common target for:

- Brute-force attacks
- Password spraying
- Credential attacks
- Unauthorized remote access
- Ransomware operations

Useful evidence includes Windows authentication events, source IPs, account names, login times, and whether authentication succeeded.

For example:

```text
Repeated failed logons
        ↓
Successful RDP login
        ↓
Suspicious account
        ↓
Unusual source IP
```

This chain would warrant significantly more investigation than a single failed login.

---

# Port 445: SMB

**SMB** stands for **Server Message Block**.

It is heavily used in Windows environments for:

- File sharing
- Printer sharing
- Network resource access
- Other Windows network operations

```text
Windows Host A
      │
      │ TCP/445
      ↓
Windows Host B
```

### SOC Relevance

SMB is particularly important for detecting potential **lateral movement** inside Windows environments.

Attackers may abuse SMB to move between systems or access remote resources.

Historical malware such as **WannaCry** also demonstrated the security impact of vulnerabilities involving SMB.

However:

> **SMB traffic is not inherently suspicious.**

A workstation communicating with an authorized file server over TCP/445 may be completely normal.

More suspicious situations can include:

```text
Workstation
    ↓
Unexpected Server
    ↓
TCP/445
```

or:

```text
Compromised Host
    ↓
Host 1
Host 2
Host 3
Host 4
    ↓
TCP/445
```

Unexpected SMB communication across network segments can therefore be worth investigating.

---

# Port 25: SMTP

**SMTP** stands for **Simple Mail Transfer Protocol**.

Port 25 is primarily associated with mail server communication.

```text
Mail Server A
      │
      │ TCP/25
      ↓
Mail Server B
```

### SOC Relevance

SMTP can be abused for:

- Spam distribution
- Phishing campaigns
- Unauthorized mail relay
- Email-based data exfiltration

An ordinary workstation unexpectedly generating large volumes of outbound SMTP traffic could indicate a compromised system or misconfiguration.

The analyst should determine:

- Which host generated the traffic
- Destination mail server
- Volume of traffic
- Whether the host is authorized to send mail
- Whether the activity is expected

---

# Port 110: POP3

**POP3** stands for **Post Office Protocol version 3**.

It is used to retrieve email from a mail server.

POP3 is traditionally associated with downloading messages to a client.

```text
Email Client
     │
     │ TCP/110
     ↓
Mail Server
```

Encrypted alternatives commonly use **POP3S on port 995**.

### SOC Relevance

Unencrypted POP3 can expose credentials and email contents to interception.

If port 110 is observed in an environment where encrypted mail protocols are expected, the analyst should investigate whether the configuration is authorized and appropriately secured.

Port 110 alone does not prove that sensitive information is being transmitted.

---

# Port 143: IMAP

**IMAP** stands for **Internet Message Access Protocol**.

It allows clients to access and manage mail while keeping messages and mailbox state on the server.

This makes IMAP useful when the same mailbox is accessed from multiple devices.

```text
Laptop ──────┐
             │
Phone ───────┼──→ Mail Server
             │
Desktop ─────┘
```

IMAP commonly uses:

```text
TCP/143
```

Encrypted IMAP commonly uses:

```text
TCP/993
```

### SOC Relevance

Unencrypted IMAP can expose credentials and email contents to interception.

SOC analysts may also investigate unusual authentication patterns such as:

- Repeated failed logins
- Successful logins from unexpected locations
- Unusual login times
- Multiple unusual source IPs
- Sudden mailbox access from unfamiliar infrastructure

As with other ports, **143 itself does not prove malicious activity**.

---

# How a SOC Analyst Reads a Network Alert

Consider:

```text
Source:
192.168.1.15:53142

Destination:
192.168.1.10:445

Protocol:
TCP

Action:
ALLOW
```

An analyst can infer:

```text
Source host:
192.168.1.15

Source port:
53142

Destination host:
192.168.1.10

Destination port:
445

Transport:
TCP

Common service:
SMB
```

However, the analyst **cannot conclude from this entry alone**:

- That SMB activity is malicious
- That the hosts are definitely in the same subnet
- That authentication succeeded
- That files were transferred
- Which user initiated the activity
- Which process generated it
- Why the connection occurred

Additional telemetry is required.

---

# SOC Investigation: Port Does Not Equal Maliciousness

A common beginner mistake is treating a port number as a verdict.

For example:

```text
445 = SMB
```

does **not** mean:

```text
445 = Attack
```

Similarly:

```text
3389 = RDP
```

does not mean:

```text
3389 = Malicious
```

Instead, the analyst should ask:

```text
What service is involved?
        ↓
Who initiated the communication?
        ↓
Who received it?
        ↓
Is this expected?
        ↓
Who was the user?
        ↓
Which process generated it?
        ↓
When did it happen?
        ↓
How often did it happen?
        ↓
What happened before and after?
```

The same port can represent either legitimate or malicious activity depending on context.

---

# Direction Matters

The direction of communication changes the investigation.

For example:

```text
Internal Host
    │
    │ TCP/3389
    ↓
Internet
```

This means the internal host initiated an outbound connection to an Internet destination.

Possible explanation:

```text
Administrator
    ↓
Legitimate remote server
```

But another possibility could be:

```text
Compromised Host
    ↓
Unauthorized RDP connection
```

Compare that with:

```text
Internet
    │
    │ TCP/3389
    ↓
Internal Windows Host
```

Now the internal host is receiving an inbound RDP connection attempt.

This may deserve greater scrutiny because the organization may not permit Internet-originated RDP access.

Direction is therefore an important part of network alert interpretation.

---

# Common Investigation Questions

When investigating a port-related alert, ask:

### Host

- What is the source host?
- What is the destination host?
- What type of systems are they?
- Are they expected to communicate?

### Network

- What protocol is being used?
- What source and destination ports are involved?
- Is the traffic inbound or outbound?
- Is the destination internal or external?

### Identity

- Which user was involved?
- Was authentication successful?
- Is the account authorized to perform the activity?

### Endpoint

- Which process initiated the connection?
- What command or executable was involved?
- Did anything suspicious execute before or after the connection?

### Timing

- When did it occur?
- Is the timing normal for this host?
- Is the activity recurring?

### Threat Intelligence

- Is the destination known?
- Is the IP or domain associated with known malicious infrastructure?
- Are other hosts communicating with the same destination?

---

# SOC Investigation Example: Inbound RDP Alert

Consider:

```text
Internet IP
    │
    │ TCP/3389
    ↓
192.168.1.50
```

The first conclusion should **not** be:

> "The machine is compromised."

Instead:

```text
Internet source
      ↓
RDP connection attempt
      ↓
Internal Windows host
```

The analyst investigates:

```text
Who owns the source IP?
        ↓
What is 192.168.1.50?
        ↓
Is inbound RDP permitted?
        ↓
Which account was targeted?
        ↓
Did authentication succeed?
        ↓
What happened after authentication?
```

If the investigation finds:

```text
Repeated failed logins
        ↓
Successful login
        ↓
Unusual external IP
        ↓
Privileged account
        ↓
Suspicious process execution
```

the severity increases substantially.

This demonstrates why SOC investigations rely on **correlated evidence rather than a single port number**.

---

# Interview Notes

### What is a port?

A port is a numbered, logical transport-layer endpoint used by network applications and services to distinguish communication endpoints and commonly associated services on a host's network stack.

### What is the difference between an IP address and a port?

An IP address identifies the network host, while a port identifies a service or application endpoint on that host.

### What is an ephemeral port?

An ephemeral port is a temporary port, typically assigned by the operating system to a client for an outgoing connection.

### What is the difference between TCP and UDP?

TCP is connection-oriented and provides reliable, ordered delivery mechanisms. UDP is connectionless and provides lower overhead without TCP's built-in reliability and ordering mechanisms.

### Is port 445 malicious?

No. Port 445 is commonly used by SMB and can be completely legitimate. Its significance depends on the communicating hosts and context.

### Why is RDP important to a SOC analyst?

RDP is commonly used for legitimate Windows administration but is also frequently targeted for unauthorized remote access, brute-force attacks, and ransomware activity.

### Why can DNS be useful during an investigation?

DNS telemetry can reveal which domains a host attempted to resolve and can be correlated with subsequent network connections.

### What is TCP?

TCP is a connection-oriented transport protocol that provides mechanisms for reliable and ordered communication.

### What is UDP?

UDP is a connectionless transport protocol with lower overhead and without TCP's built-in reliability and ordering mechanisms.

### Does port 443 prove HTTPS?

No. Port 443 is commonly associated with HTTPS, but the port number alone does not prove the application protocol.

### Does TCP traffic prove a successful connection?

No. The available telemetry must contain sufficient evidence, such as the TCP handshake or session information, to establish that a connection succeeded.

---

# Key Takeaways

- **Ports identify transport-layer endpoints.**
- **IP addresses identify hosts (network endpoints); ports identify service endpoints.**
- **Ephemeral ports are temporary client-side ports, often used as the source port for outgoing connections.**
- **TCP provides connection establishment and reliable, ordered delivery mechanisms.**
- **UDP provides lower-overhead connectionless communication.**
- **Application protocols operate above the transport layer.**
- **Port numbers are conventions, not proof of application protocols.**
- **TCP traffic does not automatically prove a successful connection.**
- **A SOC analyst should distinguish observed network evidence from inference.**
- **Common ports are useful for investigation, but they should not be treated as definitive protocol identification.**
- **TCP and UDP are transport-layer protocols commonly used with ports.**
- **Port numbers do not automatically indicate malicious activity.**
- **TCP/22 is commonly associated with SSH.**
- **UDP/TCP 53 is commonly associated with DNS.**
- **TCP/80 is commonly associated with HTTP.**
- **TCP/443 is commonly associated with HTTPS.**
- **TCP/3389 is commonly associated with RDP.**
- **TCP/445 is commonly associated with SMB.**
- **TCP/25 is commonly associated with SMTP.**
- **TCP/110 is commonly associated with POP3.**
- **TCP/143 is commonly associated with IMAP.**
- **Traffic direction is important during investigations.**
- **The same port can represent legitimate or malicious activity.**
- **SOC analysts need additional evidence such as users, processes, authentication events, timing, destination reputation, and surrounding network activity.**

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [IP Addressing](ip-addressing.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Network Security](../security-fundamentals/network-security.md)
- [Incident Response](../security-fundamentals/incident-response.md)
- [Packets & Frames](packets-and-frames.md)
- [Protocol Comparison](protocol-comparison.md)
- [Home Lab Network Diagram](home-lab-network-diagram.md)
- [Extending Your Network](extending-your-network.md)
- [LAN](lan.md)
- [Network Commands & Troubleshooting](network-commands-and-troubleshooting.md)
- [Network Topologies](network-topologies.md)
- [OSI Model](osi-model.md)
