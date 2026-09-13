---
type: concept-note
status: reviewed
tags:
  - networking
  - protocols
  - comparison
  - soc
---

# Protocol Comparison

# Overview

This note covers the core networking protocols introduced during Week 2 and their relevance to SOC analysis.

The key concept is **layering**:

```text
Application Layer
│
├── HTTP
├── HTTPS
├── DNS
└── DHCP
        ↓
Transport Layer
│
├── TCP
└── UDP
        ↓
Network Layer
│
└── IP
```

---

# TCP

**Full name:** Transmission Control Protocol  
**Layer:** Transport, Layer 4

## Purpose

TCP provides reliable, connection-oriented and ordered delivery of data between endpoints.

## Key characteristics

- Connection-oriented
    
- Reliable delivery
    
- Ordered delivery
    
- Retransmission of lost data
    
- More transport overhead than UDP
    

## Typical uses

- SSH
    
- File transfers
    
- Traditional HTTP/HTTPS transport
    
- Other applications requiring reliable communication
    

## Port

TCP itself does **not** have a default port.

Ports belong to services and applications that use TCP.

## SOC relevance

TCP telemetry can help analysts investigate:

- Port scanning
    
- SYN floods
    
- Suspicious outbound connections
    
- Unusual connection patterns
    
- Potential data transfer or exfiltration
    

## Important misconception

**TCP is not inherently secure or encrypted.**

For example:

```text
HTTP → TCP
```

uses TCP without providing encryption.

---

# UDP

**Full name:** User Datagram Protocol  
**Layer:** Transport, Layer 4

## Purpose

UDP provides lightweight, connectionless transport without built-in guarantees for delivery or ordering.

## Key characteristics

- Connectionless
    
- No built-in delivery guarantee
    
- No built-in ordering
    
- No built-in retransmission
    
- Lower transport overhead than TCP
    

## Typical uses

- DNS
    
- DHCP
    
- Real-time voice communication
    
- Some latency-sensitive applications
    

## Port

UDP itself does **not** have a default port.

Ports belong to services and applications that use UDP.

## SOC relevance

UDP telemetry can help analysts investigate:

- DDoS amplification
    
- Unusual traffic volumes
    
- Unexpected UDP services
    
- Suspicious network behavior
    

## Important misconception

**UDP is not inherently insecure.**

It simply does not provide the reliability mechanisms built into TCP.

---

# HTTP

**Full name:** Hypertext Transfer Protocol  
**Layer:** Application, Layer 7  
**Typical port:** 80  
**Transport:** Traditionally TCP

## Purpose

HTTP defines communication between web clients and web servers.

A simplified exchange:

```text
Client
   ↓
HTTP Request
   ↓
Web Server
   ↓
HTTP Response
   ↓
Client
```

Example:

```text
GET / HTTP/1.1
Host: example.com
```

## Security

HTTP does not provide TLS protection by itself.

Data sent using ordinary HTTP can therefore be exposed to network observers depending on the network position and other security controls.

## SOC relevance

Visible HTTP traffic can potentially provide information such as:

- HTTP methods
    
- URLs
    
- Headers
    
- Parameters
    
- User-Agent
    
- Request and response content
    

This can assist with investigating suspicious web activity and potential C2 communication.

---

# HTTPS

**Full name:** HTTP Secure  
**Layer:** Application, Layer 7  
**Typical port:** 443  
**Transport:** Traditionally TCP

## Purpose

HTTPS is HTTP protected by TLS.

Simplified architecture:

```text
HTTP
 ↓
TLS
 ↓
TCP
 ↓
IP
```

## TLS provides

- Confidentiality
    
- Integrity protection
    
- Server authentication through certificates
    

## Important misconception

HTTPS does **not** mean:

> The website is safe.

A malicious or phishing website can use HTTPS.

HTTPS protects the communication channel. It does not establish that the destination itself is trustworthy.

## SOC relevance

Even when application content is encrypted, analysts may still have access to information such as:

- Source IP
    
- Destination IP
    
- Destination port
    
- Domain information
    
- DNS history
    
- TLS information
    
- Certificate information
    
- Connection timing
    
- Traffic volume
    
- User
    
- Endpoint process
    

Deeper inspection of encrypted application content may require appropriate TLS inspection or decryption capabilities.

---

# DNS

**Full name:** Domain Name System  
**Layer:** Application, Layer 7  
**Typical port:** 53  
**Transport:** UDP commonly, TCP also

## Purpose

DNS resolves domain names into IP addresses and provides other types of DNS records.

Example:

```text
example.com
     ↓
    DNS
     ↓
104.x.x.x
```

A DNS response may contain:

- IPv4 addresses
    
- IPv6 addresses
    
- Other DNS record types
    

## SOC relevance

DNS activity is valuable during investigations because it can reveal which domains an endpoint is attempting to contact.

Analysts may investigate:

- Suspicious domains
    
- Domain Generation Algorithms
    
- DNS tunneling
    
- Unusual query patterns
    
- Malicious domain communication
    
- DNS activity followed by suspicious network connections
    

## Important misconception

DNS is **commonly** associated with UDP, but DNS can also use TCP.

Therefore:

```text
DNS ≠ UDP only
```

---

# DHCP

**Full name:** Dynamic Host Configuration Protocol  
**Layer:** Application, Layer 7  
**Transport:** UDP

## Typical ports

```text
UDP 67 → DHCP server
UDP 68 → DHCP client
```

## Purpose

DHCP dynamically provides network configuration to clients.

It can provide:

- IP address
    
- Subnet mask
    
- Default gateway
    
- DNS server
    
- Lease information
    

## SOC relevance

DHCP information can help analysts:

- Identify unexpected devices
    
- Detect rogue DHCP servers
    
- Correlate IP addresses with MAC addresses
    
- Reconstruct network activity during investigations
    

---

# SOC Mental Model

A protocol or port provides **context, not a verdict**.

For example:

```text
Source:      10.0.0.50
Destination: 142.x.x.x
Protocol:    TCP
Port:        443
```

A reasonable initial inference is:

> TCP traffic is being sent to destination port 443, which is commonly associated with HTTPS.

But this does **not** prove:

- The connection is benign
    
- The destination is trustworthy
    
- The traffic is definitely HTTPS
    
- The user intentionally initiated it
    
- Data was successfully transferred
    

Further evidence is required.

Useful additional telemetry may include:

```text
Domain
DNS history
Process
User
Command line
Connection state
TLS information
Endpoint events
Related network connections
```

---

# Key Rules to Remember

```text
TCP ≠ encryption
UDP ≠ insecure

HTTP ≠ TCP
HTTPS ≠ TCP
DNS ≠ UDP

TCP/UDP = transport protocols
HTTP/HTTPS/DNS/DHCP = application protocols

Ports identify service/application endpoints.
```

# Website Communication Model

A simplified traditional HTTPS flow:

```text
User enters URL
       ↓
DNS resolution
       ↓
Destination IP
       ↓
TCP connection
       ↓
TLS establishment
       ↓
HTTP request
       ↓
HTTP response
       ↓
Browser processes resources
       ↓
Webpage rendered
```

> **SOC takeaway:** Understand what each layer tells you before deciding what the activity means.

---

# Related Notes

- [Network Commands & Troubleshooting](network-commands-and-troubleshooting.md)
- [Networking Fundamentals](networking-fundamentals.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
- [Ports & Protocols](ports-and-protocols.md)
