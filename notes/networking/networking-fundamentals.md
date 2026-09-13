---
type: concept-note
status: reviewed
tags:
  - networking
  - network-fundamentals
  - cybersecurity
  - lan
  - topology
  - soc
---

# Networking Fundamentals

> **Purpose**
>
> This note explains the fundamental concepts behind computer networks and how network communication is organized — including network types, topologies, and networking devices, alongside addressing, segmentation, and SOC-relevant analysis. It provides the foundation for understanding IP addressing, subnetting, routing, DNS, DHCP, ports, protocols, packet analysis, and network security from a SOC analyst perspective.

---

# What Is a Network?

A **network** is an interconnection of two or more devices that can communicate and exchange data.

Networks can use:

- Wired connections
- Wireless connections
- Or a combination of both

Devices on a network can include:

- Computers
- Servers
- Smartphones / phones
- Network appliances
- Printers
- IoT devices
- Virtual machines
- Routers
- Switches
- Access points

For example:

```text
Laptop
   │
   ↓
Switch
   │
   ├── Server
   ├── Printer
   └── Another Laptop
```

Communication across a network depends on addressing, protocols, and networking infrastructure. The exact architecture depends on the purpose and scale of the network.

---

# Internet vs Network

A network can exist entirely within a local environment, such as a home, school, or company.

The **Internet** is a global system of interconnected networks.

A useful mental model is:

```text
Device
   ↓
Local Network
   ↓
Router / Gateway
   ↓
Other Networks
   ↓
Internet
```

The Internet is therefore not a single network. It is an interconnected collection of many independent networks.

---

# How Devices Are Identified

Network communication requires devices and interfaces to be identifiable.

Two important identifiers are:

### IP Address

An **IP address** is a logical network address used to identify a network interface and facilitate communication between hosts.

Example:

```text
192.168.1.25
```

IP addressing, private/public addressing, and NAT are covered in:

[IP Addressing](ip-addressing.md)

### MAC Address

A **MAC address** identifies a network interface at the link layer.

A MAC address is commonly associated with the device's network hardware, although modern operating systems can use randomized or spoofed MAC addresses.

MAC addresses are primarily relevant to local network communication.

---

# Network Communication

When a device communicates with another host, several pieces of information help determine how the traffic should be handled.

A simplified representation is:

```text
Source IP
    ↓
Destination IP
    ↓
Port
    ↓
Transport Protocol
    ↓
Application Protocol
```

For example:

```text
192.168.10.25:53142
        ↓
192.168.20.50:443
        ↓
TCP
```

This tells an analyst which hosts are involved, which endpoints are being used, and which transport protocol is involved.

However, individual network fields do not automatically reveal the full application activity or whether the traffic is malicious.

---

# Local vs Remote Communication

A device first determines whether a destination is on its local network.

Conceptually:

```text
Destination on local subnet
        ↓
Local communication

Destination on another network
        ↓
Routing required
        ↓
Default Gateway
```

The **default gateway** is normally the next-hop device used when traffic needs to leave the local network and there is no more specific route.

Routing and network boundaries are covered in:

[Routing & Network Segmentation](routing-and-network-segmentation.md)

---

# Network Segmentation

Organizations often divide their networks into separate logical networks.

For example:

```text
192.168.10.0/24 → Employee Network
192.168.20.0/24 → Server Network
192.168.30.0/24 → Security Network
```

Segmentation can help organizations:

- Reduce unnecessary communication paths
- Limit lateral movement
- Control access between network zones
- Reduce the potential impact of a compromised host

However, simply placing systems into different subnets does not automatically prevent communication between them.

Controls such as:

- Firewalls
- ACLs
- Routing policies
- VLAN configurations
- Security groups

may enforce restrictions between network segments.

Subnet boundaries and CIDR notation are covered in:

[Subnetting & CIDR](subnetting-and-cidr.md)

---

# Network Services and Protocols

Different network services use different protocols and transport mechanisms.

Examples include:

| Service / Protocol | Common Association |
|---|---|
| DNS | Name resolution |
| DHCP | Network configuration |
| HTTP | Web communication |
| HTTPS | HTTP protected by TLS |
| SSH | Secure remote administration |
| SMB | Windows file and network sharing |

Transport protocols such as **TCP** and **UDP** provide different mechanisms for carrying application traffic.

Ports identify transport-layer endpoints and are commonly associated with particular services.

These concepts are covered in:

[Ports & Protocols](ports-and-protocols.md)

---

# Network Configuration

A device commonly requires several pieces of network configuration before it can communicate effectively:

```text
IP Address
Subnet / Prefix
Default Gateway
DNS Server
```

These values may be configured manually or provided dynamically through **DHCP**.

DHCP and DNS are covered in:

[DHCP & DNS](dhcp-and-dns.md)

---

# Networking and SOC Operations

Networking knowledge is fundamental to SOC analysis because many security alerts contain network information.

A SOC analyst may encounter telemetry containing:

- Source IP
- Source port
- Destination IP
- Destination port
- Protocol
- DNS queries
- Connection timestamps
- Firewall decisions
- Network segmentation information

For example:

```text
Source:
192.168.10.25:49152

Destination:
192.168.20.50:445

Protocol:
TCP
```

An analyst can determine that a host in the employee network is communicating with a host in the server network using TCP and targeting destination port 445.

However, the analyst should not automatically conclude that:

- The activity is malicious
- The TCP connection was successfully established
- SMB was definitely used
- Data was transferred
- The source host was compromised

Additional evidence is required.

---

# Evidence vs Inference

Network telemetry often provides incomplete information.

A SOC analyst should distinguish between:

**What the evidence proves**

and

**What the analyst suspects or infers.**

For example:

```text
Port 443
```

is commonly associated with HTTPS, but the port number alone does not prove that HTTPS was used.

Similarly:

```text
TCP traffic to port 445
```

does not by itself prove that an SMB session was successfully established or that the activity was malicious.

This principle is important throughout SOC investigations:

> **State what the available evidence supports without claiming more than the evidence proves.**

---

# Network Investigation Context

A network event becomes more useful when correlated with additional telemetry.

For example:

```text
Network Event
      ↓
Source IP
      ↓
DHCP Records
      ↓
Device Attribution
      ↓
User / Host
      ↓
DNS Activity
      ↓
Process / Endpoint Telemetry
      ↓
Authentication Events
      ↓
Investigation
```

The same network connection can have very different meanings depending on:

- The identity of the source host
- The destination asset
- The user's role
- The expected network architecture
- The time of activity
- The process generating the traffic
- Whether the communication is normally allowed
- What happened before and after the event

---

# SOC Perspective

Networking allows a SOC analyst to answer important questions such as:

> **Who communicated with whom?**

> **Across which network boundary?**

> **Using which transport and service endpoint?**

> **Was the communication expected?**

> **What additional evidence is needed to determine what happened?**

Network telemetry rarely provides the complete answer by itself.

It becomes significantly more useful when correlated with endpoint, identity, DNS, DHCP, firewall, and authentication telemetry.

---

# Network Types

Networks can be categorized based on their size and scope.

## LAN

**LAN** stands for **Local Area Network**.

A LAN connects devices within a relatively limited geographical area such as:

- Home
- Office
- School
- Building

For example:

```text
Office LAN

PC ──┐
PC ──┤
PC ──┼── Switch ── Router
PC ──┤
Printer ─┘
```

LANs are commonly privately managed by an organization or individual.

---

## WAN

**WAN** stands for **Wide Area Network**.

A WAN connects networks across larger geographical areas.

For example:

```text
Office A
   │
   ↓
Internet / WAN
   │
   ↓
Office B
```

The Internet can be considered the largest example of an interconnected collection of networks.

---

# Network Topologies

A **network topology** describes how devices are arranged and connected within a network. Common topologies include bus, star, ring, and mesh, and the choice affects reliability, scalability, troubleshooting, and potential points of failure.

Modern Ethernet LANs commonly use a **star topology** centered on a switch — it isolates individual connections better than a shared bus and is easier to manage and troubleshoot, though the central device becomes an important dependency.

For the full comparison of bus, star, ring, and mesh — including advantages, disadvantages, failure scenarios, and scalability — see:

[Network Topologies](network-topologies.md)

---

# Networking Devices

A **hub** repeats incoming traffic to all connected ports, while a **switch** learns MAC addresses and forwards Ethernet frames only toward the appropriate port — which is why switches are the standard choice in modern LANs. A **router** connects different networks and forwards traffic using IP addressing. A **wireless access point** bridges wireless clients into the network; in many home environments a single device combines router, switch, and access point functions.

For the full explanation of each device — including repeaters, hub limitations, switch/router comparisons, and how these devices generate SOC-relevant telemetry — see:

[Extending Your Network](extending-your-network.md)

---

# Network Devices at a Glance

| Device | Primary role |
|---|---|
| Hub | Forwards traffic to connected ports |
| Switch | Connects devices within a LAN and forwards frames based on MAC addresses |
| Router | Connects networks and forwards IP packets |
| Access Point | Provides wireless network connectivity |

The exact capabilities of modern networking equipment can overlap.

For example, a home router commonly contains a switch and wireless access point in the same physical device.

---

# Network Communication Across Networks

A device communicating across a network generally requires:

```text
Source
   ↓
Addressing
   ↓
Networking Devices
   ↓
Protocols
   ↓
Destination
```

For example:

```text
192.168.1.25
      ↓
Router
      ↓
Internet
      ↓
8.8.8.8
```

The IP addresses identify the source and destination at the network layer.

Ports can then identify services or applications associated with the communication.

For example:

```text
192.168.1.25:51532
        ↓
8.8.8.8:443
```

The IP addresses identify the communicating hosts, while the ports identify the endpoints of the transport-layer communication.

---

# Network Communication and the SOC

A SOC analyst frequently encounters network information in alerts and logs.

For example:

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

This immediately gives the analyst several pieces of information:

```text
Who?
   ↓
Source IP

Where?
   ↓
Destination IP

Which service?
   ↓
Destination Port

How?
   ↓
Transport Protocol
```

However, these fields alone do not explain whether the activity is malicious.

The analyst needs additional context such as:

- Which host generated the traffic?
- Which user was involved?
- Which process created the connection?
- Was the destination expected?
- Was the connection successful?
- What happened before the connection?
- What happened afterward?

Networking knowledge therefore forms the foundation for interpreting network telemetry.

---

# Network Topology and Security

Network design also affects security.

A poorly designed flat network may allow systems to communicate with many other systems unnecessarily.

For example:

```text
All Devices
     │
     └── Same Network
```

A more controlled design can separate systems into logical network segments:

```text
Users
  │
  ├── Network Segment
  │
Servers
  │
  ├── Network Segment
  │
Security Infrastructure
```

Segmentation can reduce unnecessary communication and limit the potential impact of a compromised host.

This connects directly with:

- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)

---

# Practical Networking Perspective

During the TryHackMe networking exercises, network communication can be observed rather than treated as an abstract concept.

For example, a **ping** uses ICMP:

```text
Host A
   │
   │ ICMP Echo Request
   ↓
Host B
   │
   │ ICMP Echo Reply
   ↓
Host A
```

A TCP connection involves a handshake:

```text
Client
   │
   │ SYN
   ↓
Server
   │
   │ SYN/ACK
   ↓
Client
   │
   │ ACK
   ↓
Connection Established
```

This demonstrates that communication between two IP addresses involves protocols and packet exchanges rather than a simple direct transfer of data.

---

# SOC Perspective: Correlating Network Evidence

A SOC analyst should think about networks in terms of **evidence and communication paths**.

A basic investigation may start with:

```text
Alert
  ↓
Source IP
  ↓
Destination IP
  ↓
Port
  ↓
Protocol
  ↓
Network Session
  ↓
Host / User / Process
```

The analyst then correlates network evidence with other telemetry.

For example:

```text
Network Alert
      ↓
192.168.1.25
      ↓
DHCP
      ↓
Workstation
      ↓
DNS Query
      ↓
example.com
      ↓
TCP Connection
      ↓
Endpoint Process
```

This is how networking concepts begin to connect with actual SOC investigations.

---

# Interview Notes

### What is a network?

A network is an interconnection of devices that allows them to communicate and exchange data.

### What is the Internet?

The Internet is a global system of interconnected networks rather than a single network.

### Why is networking important to a SOC analyst?

SOC analysts frequently investigate alerts containing IP addresses, ports, protocols, DNS activity, and network connections. Understanding how these components work allows analysts to interpret alerts and correlate network activity with other security telemetry.

### Why is network segmentation important?

Segmentation separates systems into logical network zones and can reduce unnecessary communication, limit lateral movement, and provide boundaries where security controls can be enforced.

---

# Key Takeaways

- **A network connects devices so they can communicate and exchange data.**
- **The Internet is a collection of interconnected networks.**
- **LANs cover relatively small areas such as homes and offices.**
- **WANs connect networks across larger geographical areas.**
- **A topology describes how network devices are arranged and connected.**
- **Bus topology relies on a shared communication medium.**
- **Star topology connects endpoints through a central device.**
- **Star topology is generally easier to manage, troubleshoot, and scale than bus topology.**
- **IP addresses provide logical network addressing.**
- **MAC addresses identify network interfaces at the link layer.**
- **A switch primarily connects devices within a LAN and forwards frames based on MAC addresses.**
- **A router connects different networks and forwards IP packets.**
- **An access point provides wireless connectivity to a network.**
- **Devices determine whether destinations are local or require routing.**
- **Network segmentation can limit communication and lateral movement.**
- **Network architecture and segmentation can affect the security impact of a compromised system.**
- **Ports identify transport-layer endpoints.**
- **IP addresses identify hosts at the network layer, while ports identify transport-layer endpoints.**
- **TCP and UDP provide different transport mechanisms.**
- **DHCP can provide network configuration to devices.**
- **DNS provides name-resolution and other DNS services.**
- **Network telemetry must be interpreted carefully and correlated with additional evidence.**
- **Network information is important evidence during SOC investigations.**
- **A SOC analyst should distinguish evidence from inference.**

---

# Related Notes

- [IP Addressing](ip-addressing.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Ports & Protocols](ports-and-protocols.md)
- [Protocol Comparison](protocol-comparison.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
- [LAN](lan.md)
- [Network Topologies](network-topologies.md)
- [Extending Your Network](extending-your-network.md)
- [Network Commands & Troubleshooting](network-commands-and-troubleshooting.md)
- [Home Lab Network Diagram](home-lab-network-diagram.md)
- [Network Security](../security-fundamentals/network-security.md)
- [Cybersecurity Fundamentals](../security-fundamentals/cybersecurity-fundamentals.md)
