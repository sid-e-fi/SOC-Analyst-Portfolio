---
type: concept-note
status: reviewed
tags:
  - networking
  - osi
  - network-model
  - protocols
  - soc
---

# OSI Model

> **Purpose**
>
> The OSI (Open Systems Interconnection) model is a conceptual framework used to understand how network communication is divided into different layers.
>
> For a SOC analyst, the OSI model provides a structured way to understand where protocols, addressing, networking devices, and security controls operate.

---

# What Is the OSI Model?

The **OSI model** divides network communication into **seven layers**.

Each layer has a specific role in moving data between communicating systems.

The seven layers are:

```text
7  Application
6  Presentation
5  Session
4  Transport
3  Network
2  Data Link
1  Physical
```

A useful way to remember the order is:

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

The model helps break a complicated networking process into smaller and easier-to-understand components.

---

# The Seven Layers

## Layer 7: Application

The **Application layer** is closest to the applications and network services used by users.

Examples include protocols such as:

```text
HTTP
HTTPS
DNS
SMTP
FTP
SSH
```

For example:

```text
Web Browser
     ↓
HTTP / HTTPS
     ↓
Application Layer
```

The Application layer does not mean the actual application itself.

It refers to the network protocols and services that applications use to communicate.

---

## Layer 6: Presentation

The **Presentation layer** deals with how data is represented.

Its responsibilities can include:

- Data formatting
- Encoding
- Encryption
- Compression

Conceptually:

```text
Application Data
      ↓
Formatting / Encoding / Encryption
      ↓
Data representation
```

In modern networking, responsibilities associated with this layer are often implemented as part of application protocols or libraries rather than as a distinct layer.

---

## Layer 5: Session

The **Session layer** manages communication sessions between systems.

It can be thought of as helping establish, maintain, and terminate communication sessions.

Conceptually:

```text
Session Start
     ↓
Communication
     ↓
Session Management
     ↓
Session End
```

In modern networking, session-related functions are often handled by application protocols and operating systems rather than appearing as a clearly separate layer.

---

## Layer 4: Transport

The **Transport layer** provides communication between applications running on different hosts.

Common transport protocols include:

```text
TCP
UDP
```

TCP provides features such as:

- Reliable delivery
- Connection establishment
- Sequencing
- Retransmission
- Flow control

UDP provides a simpler connectionless transport mechanism without TCP's reliability features.

Ports are also associated with the Transport layer.

For example:

```text
192.168.1.25:51532
        ↓
8.8.8.8:443
```

The IP addresses identify the hosts, while the ports identify transport-layer endpoints.

This connects directly with:

- [Ports & Protocols](ports-and-protocols.md)
- [Protocol Comparison](protocol-comparison.md)

---

## Layer 3: Network

The **Network layer** is responsible for logical addressing and routing between networks.

The most important example is:

```text
IP
```

For example:

```text
192.168.1.25
      ↓
Router
      ↓
8.8.8.8
```

Routers operate primarily at Layer 3 because they make forwarding decisions using network-layer addressing.

The Network layer is therefore important when understanding:

- IP addresses
- Routing
- Subnets
- Network segmentation

This connects directly with:

- [IP Addressing](ip-addressing.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)

---

## Layer 2: Data Link

The **Data Link layer** handles communication between devices on the same local network.

Ethernet is a major example.

Layer 2 uses **MAC addresses** for local network communication.

For example:

```text
Device A
MAC: AA:AA:AA:AA:AA:AA
       ↓
    Switch
       ↓
Device B
MAC: BB:BB:BB:BB:BB:BB
```

A switch primarily operates at Layer 2.

Data at this layer is commonly called a **frame**.

This connects directly with:

- [Networking Fundamentals](networking-fundamentals.md)
- [Packets & Frames](packets-and-frames.md)

---

## Layer 1: Physical

The **Physical layer** deals with the actual transmission of bits across a physical or electrical medium.

Examples include:

```text
Ethernet Cable
Fiber Optic Cable
Radio Signals
Electrical Signals
Connectors
Physical Network Interfaces
```

At this layer, data is transmitted as physical signals representing bits.

Conceptually:

```text
Bits
 ↓
Electrical / Optical / Radio Signals
 ↓
Physical Medium
```

A damaged network cable is therefore primarily a Layer 1 problem.

---

# OSI Model at a Glance

| Layer | Name | Examples / Responsibilities |
|---|---|---|
| 7 | Application | HTTP, HTTPS, DNS, SSH |
| 6 | Presentation | Encoding, encryption, compression |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP, ports |
| 3 | Network | IP, routing, logical addressing |
| 2 | Data Link | Ethernet, MAC addresses, frames |
| 1 | Physical | Cables, signals, physical transmission |

---

# Data Units

As data moves through the networking stack, different terms are commonly used to describe it.

A simplified model is:

```text
Application
     ↓
   Data
     ↓
Transport
     ↓
   Segment
     ↓
Network
     ↓
   Packet
     ↓
Data Link
     ↓
   Frame
     ↓
Physical
     ↓
   Bits
```

For TCP:

```text
Application Data
      ↓
TCP Segment
      ↓
IP Packet
      ↓
Ethernet Frame
      ↓
Bits
```

These terms are important when working with packet captures and network logs.

---

# Encapsulation

When data is sent across a network, each layer adds information required by that layer.

This process is called **encapsulation**.

Conceptually:

```text
Application Data
       ↓
TCP Header + Data
       ↓
IP Header + TCP Segment
       ↓
Ethernet Header + IP Packet
       ↓
Bits
```

The receiving system performs the reverse process, known as **decapsulation**.

```text
Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Application Data
```

This layered structure allows different parts of the networking stack to perform their specific functions.

---

# Example: Opening a Website

Suppose a user visits:

```text
https://example.com
```

A simplified view using the OSI model is:

```text
Application
    ↓
HTTPS

Transport
    ↓
TCP
    ↓
Port 443

Network
    ↓
IP
    ↓
Destination IP

Data Link
    ↓
Ethernet
    ↓
MAC addresses

Physical
    ↓
Electrical / Optical / Radio signals
```

This connects the OSI model with the website communication process we discussed previously.

---

# OSI Model and Networking Devices

Different networking devices primarily operate at different layers.

| Device | Primary OSI layer |
|---|---|
| Hub | Layer 1 |
| Switch | Layer 2 |
| Router | Layer 3 |
| Firewall | Depends on implementation, commonly Layers 3 to 7 |

Modern security devices can inspect traffic at multiple layers, so these associations should not be treated as absolute boundaries.

---

# OSI Model in SOC Investigations

The OSI model gives a SOC analyst a useful framework for understanding network evidence.

Suppose an alert contains:

```text
Source IP:
192.168.1.25

Destination IP:
8.8.8.8

Destination Port:
443

Protocol:
TCP
```

We can map this information conceptually:

```text
Layer 4
TCP
Port 443
      ↓
Layer 3
Source IP
Destination IP
      ↓
Layer 2
MAC addresses / Ethernet
      ↓
Layer 1
Physical transmission
```

If a connection fails, the analyst can ask where the problem occurs.

For example:

```text
Physical
   ↓
Is the link working?

Data Link
   ↓
Can devices communicate locally?

Network
   ↓
Can traffic reach the destination network?

Transport
   ↓
Is the required port reachable?

Application
   ↓
Is the service responding correctly?
```

This gives the analyst a structured troubleshooting approach.

---

# OSI Model and Security

The OSI model can also help classify security controls and attacks.

Examples:

```text
Layer 1
Physical access / cable disruption

Layer 2
MAC-related attacks
ARP-related attacks

Layer 3
IP-based filtering
Routing attacks

Layer 4
Port scanning
TCP-based attacks

Layer 7
Web attacks
DNS abuse
Application-layer attacks
```

Real attacks can involve multiple layers, so the OSI model should be used as a framework rather than assuming every attack belongs to exactly one layer.

---

# SOC Perspective

A SOC analyst should not memorize the OSI model simply because it appears in certification questions.

Its real value is providing a mental framework for interpreting network evidence.

For example:

```text
Alert
  ↓
What traffic is involved?
  ↓
Layer 3
IP addresses
  ↓
Layer 4
Ports and transport protocol
  ↓
Layer 7
Application / service
  ↓
Additional telemetry
  ↓
Host / User / Process
```

A packet capture can provide even more detail:

```text
Ethernet Frame
      ↓
IP Packet
      ↓
TCP Segment
      ↓
Application Protocol
      ↓
Application Data
```

Understanding these layers makes packet analysis and network investigation much easier.

---

# Important Limitation

The OSI model is a **conceptual model**.

Real-world networking does not always fit perfectly into seven isolated layers.

For example:

- Modern protocols can span multiple OSI layers.
- Some Layer 5 and Layer 6 functions are handled by application software.
- Modern firewalls can inspect traffic at several layers.
- TCP/IP networking is commonly described using a different model.

Therefore:

> **Use the OSI model as a framework for understanding networking, not as a literal description of how every modern protocol is implemented.**

---

# Interview Notes

### What is the OSI model?

The OSI model is a seven-layer conceptual framework used to understand how network communication is structured.

### What are the seven OSI layers?

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

### Which layer does TCP operate at?

**Layer 4, Transport.**

### Which layer does IP operate at?

**Layer 3, Network.**

### Which layer does Ethernet operate at?

**Layer 2, Data Link.**

### Which layer uses MAC addresses?

**Layer 2, Data Link.**

### Which layer uses IP addresses?

**Layer 3, Network.**

### Which layer uses ports?

**Layer 4, Transport.**

### What is encapsulation?

Encapsulation is the process of adding protocol information as data moves down the networking stack.

### Why is the OSI model useful to a SOC analyst?

It provides a structured way to understand network traffic, classify protocols and evidence, troubleshoot communication problems, and reason about where security controls or attacks may operate.

---

# Key Takeaways

- **The OSI model contains seven layers.**
- **Layer 7 is Application.**
- **Layer 4 is Transport and includes TCP, UDP, and ports.**
- **Layer 3 is Network and includes IP addressing and routing.**
- **Layer 2 is Data Link and includes Ethernet and MAC addresses.**
- **Layer 1 is Physical and deals with signals and transmission media.**
- **Data is encapsulated as it moves down the networking stack.**
- **The receiving system decapsulates the data.**
- **Packets, segments, frames, and bits refer to data at different stages of the stack.**
- **The OSI model is conceptual and does not perfectly represent every modern protocol implementation.**
- **For a SOC analyst, the OSI model is useful for interpreting network evidence and troubleshooting communication.**

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [IP Addressing](ip-addressing.md)
- [Ports & Protocols](ports-and-protocols.md)
- [Protocol Comparison](protocol-comparison.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Packets & Frames](packets-and-frames.md) 
- [Extending Your Network](extending-your-network.md)
- [LAN](lan.md)
- [Network Topologies](network-topologies.md)
