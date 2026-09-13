---
type: concept-note
status: reviewed
tags:
  - networking
  - packets
  - frames
  - osi
  - ethernet
  - soc
---

# Packets & Frames

> **Purpose**
>
> This note explains how data is encapsulated as it moves through a network, with a focus on the difference between packets and frames. Understanding this distinction is important for interpreting network traffic and investigating network-based security events.

---

# Packet vs Frame

The terms **packet** and **frame** refer to data at different layers of the networking stack.

A simplified view is:

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

Therefore:

- **Segment** → Transport layer
- **Packet** → Network layer
- **Frame** → Data Link layer
- **Bits** → Physical layer

The terms describe the same overall communication at different stages of encapsulation.

---

# What Is a Packet?

A **packet** is a unit of data associated with the **Network layer (Layer 3)**.

An IP packet contains information such as:

```text
Source IP
Destination IP
Payload
Other IP header information
```

For example:

```text
Source IP:
192.168.1.25

Destination IP:
8.8.8.8
```

The IP packet allows routers to determine where network-layer traffic needs to go.

---

# What Is a Frame?

A **frame** is a unit of data associated with the **Data Link layer (Layer 2)**.

An Ethernet frame contains information such as:

```text
Source MAC
Destination MAC
Payload
Other Ethernet information
```

Conceptually:

```text
Source MAC
     ↓
Destination MAC
     ↓
IP Packet
```

A switch primarily uses the MAC addresses in Ethernet frames to make forwarding decisions within a LAN.

---

# Packet vs Frame: Comparison Table

|  | Packet | Frame |
|---|---|---|
| OSI layer | Layer 3 | Layer 2 |
| Common protocol | IP | Ethernet |
| Addressing | IP addresses | MAC addresses |
| Primarily handled by | Routers | Switches |
| Data unit | Packet | Frame |

The packet is concerned primarily with **communication between networks**, while the frame is concerned primarily with **local network delivery**.

---

# Encapsulation

When a host sends data, information is added as the data moves down the networking stack.

For example, when an application sends data using TCP over IP and Ethernet:

```text
Application Data
       ↓
TCP Header + Application Data
       ↓
IP Header + TCP Segment
       ↓
Ethernet Header + IP Packet
```

The resulting Ethernet frame is then transmitted across the physical network.

This process is called **encapsulation**.

---

# Decapsulation

When the receiving host receives the frame, the process happens in reverse.

```text
Ethernet Frame
       ↓
IP Packet
       ↓
TCP Segment
       ↓
Application Data
```

This is called **decapsulation**.

Each layer processes and removes the information relevant to that layer before passing the remaining data upward.

---

# A Practical Example

Suppose:

```text
192.168.1.25
```

wants to communicate with:

```text
192.168.1.50
```

on the same LAN.

The application generates data.

```text
Application Data
       ↓
TCP Segment
       ↓
IP Packet
       ↓
Ethernet Frame
```

The Ethernet frame contains local MAC addressing.

```text
Source MAC
     ↓
Destination MAC
     ↓
IP Packet
```

The switch examines the frame and uses the destination MAC address to determine where to forward it.

---

# What Happens Across a Router?

Now consider communication between different networks:

```text
192.168.1.25
      ↓
   Switch
      ↓
   Router
      ↓
   Internet
      ↓
8.8.8.8
```

The host sends the IP packet inside an Ethernet frame.

When the router receives the frame:

```text
Ethernet Frame
      ↓
Router processes Layer 2 information
      ↓
IP Packet
      ↓
Routing decision
      ↓
New Ethernet Frame
      ↓
Next network
```

The important concept is that **the Layer 2 frame can change as the packet travels through different networks**.

The packet is routed toward its destination, while each local network uses its own Layer 2 framing.

---

# TCP Handshake Example

During the TryHackMe practical exercise, a TCP connection can be observed through packets exchanged between two hosts.

The basic TCP three-way handshake is:

```text
Client                    Server
  │                         │
  │ -------- SYN --------> │
  │                         │
  │ <----- SYN/ACK ------- │
  │                         │
  │ -------- ACK --------> │
  │                         │
  │   Connection Ready     │
```

### SYN

The client requests to establish a TCP connection.

### SYN/ACK

The server acknowledges the request and responds with its own synchronization information.

### ACK

The client acknowledges the server's response.

The TCP connection can then proceed.

This connects directly to:

- [Ports & Protocols](ports-and-protocols.md)
- [Protocol Comparison](protocol-comparison.md)
- [OSI Model](osi-model.md)

---

# Packets and Frames in Network Analysis

Network analysis tools can show information from multiple layers.

A simplified observation might look like:

```text
Ethernet
    ↓
Source MAC
Destination MAC

IP
    ↓
Source IP
Destination IP

TCP
    ↓
Source Port
Destination Port
Flags

Application
    ↓
Application protocol / data
```

This allows an analyst to investigate communication across several layers.

---

# SOC Perspective

Packets and frames are important because network security investigations often begin with network traffic.

For example, an alert may identify:

```text
Source IP:
192.168.1.25

Destination IP:
8.8.8.8

Protocol:
TCP

Destination Port:
443
```

The analyst can then investigate the underlying communication.

Questions include:

```text
Who communicated?
        ↓
Source / destination IP

Which local network?
        ↓
MAC / VLAN / subnet information

Which transport?
        ↓
TCP / UDP

Which service?
        ↓
Destination port

What actually happened?
        ↓
Packet / session contents and additional logs
```

A packet capture can provide much more evidence than a basic firewall log.

---

# Important Distinction

Do not treat **packet** and **frame** as interchangeable terms.

A useful mental model is:

```text
Frame
┌─────────────────────────────┐
│ Ethernet Header             │
│                             │
│   Packet                    │
│   ┌───────────────────────┐ │
│   │ IP Header             │ │
│   │                       │ │
│   │ TCP Segment           │ │
│   │ ┌───────────────────┐ │ │
│   │ │ TCP Header        │ │ │
│   │ │ Application Data  │ │ │
│   │ └───────────────────┘ │ │
│   └───────────────────────┘ │
└─────────────────────────────┘
```

The exact structure depends on the protocols being used, but the general idea is that higher-layer data is encapsulated inside lower-layer protocol units.

---

# Key Takeaways

- A **packet** is associated with Layer 3 and commonly contains an IP packet.
- A **frame** is associated with Layer 2 and commonly contains an Ethernet frame.
- Packets use **IP addresses** for network-layer addressing.
- Frames use **MAC addresses** for local Layer 2 delivery.
- Switches primarily forward frames.
- Routers primarily make forwarding decisions using IP packets.
- Data is encapsulated as it moves down the networking stack.
- Data is decapsulated as it moves back up the stack.
- A packet can travel through multiple networks while the Layer 2 frame can change at each hop.
- TCP communication includes a three-way handshake: **SYN → SYN/ACK → ACK**.
- Packet captures can provide detailed evidence during network investigations.

---

# Related Notes

- [OSI Model](osi-model.md)
- [Networking Fundamentals](networking-fundamentals.md)
- [IP Addressing](ip-addressing.md)
- [Ports & Protocols](ports-and-protocols.md)
- [Protocol Comparison](protocol-comparison.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [DHCP & DNS](dhcp-and-dns.md) 
- [Extending Your Network](extending-your-network.md)
- [LAN](lan.md)
- [Network Topologies](network-topologies.md)
