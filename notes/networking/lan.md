---
type: concept-note
status: reviewed
tags:
  - networking
  - lan
  - ethernet
  - switching
  - soc
---

# LAN

> **Purpose**
>
> A LAN (Local Area Network) connects devices within a limited geographical area. This note explains how LANs are structured, how devices communicate within them, and why LAN knowledge matters during SOC investigations.

---

# What Is a LAN?

A **LAN (Local Area Network)** is a network that connects devices within a relatively limited area.

Common examples include:

- Home networks
- Office networks
- School networks
- Networks within a building

A simple LAN might look like:

```text
PC 1 ──┐
PC 2 ──┤
PC 3 ──┼── Switch ── Router ── Internet
PC 4 ──┤
Printer ┘
```

The devices can communicate with each other through the networking infrastructure.

---

# Devices Commonly Found on a LAN

A LAN can contain many different types of devices.

| Device | Purpose |
|---|---|
| Workstation | User's computer |
| Server | Provides services or resources |
| Printer | Provides printing services |
| Switch | Connects devices within the LAN |
| Router | Connects the LAN to other networks |
| Access Point | Provides wireless connectivity |
| IoT Device | Provides specialized network-connected functionality |

A home router often combines several of these functions into one physical device.

---

# LAN Addressing

Devices on a LAN normally use **IP addresses** for network-layer communication.

For example:

```text
PC 1
192.168.1.25

PC 2
192.168.1.50
```

The devices may also have unique MAC addresses:

```text
PC 1
IP:  192.168.1.25
MAC: AA:AA:AA:AA:AA:AA

PC 2
IP:  192.168.1.50
MAC: BB:BB:BB:BB:BB:BB
```

The IP address provides logical network addressing, while the MAC address is used for local Layer 2 communication.

Related note:

- [IP Addressing](ip-addressing.md)

---

# Switching Within a LAN

A **switch** is commonly used to connect devices within a LAN.

For example:

```text
PC 1
  │
  ↓
Switch
  ├── PC 2
  ├── PC 3
  └── Server
```

A switch learns which MAC addresses are associated with its ports.

When a frame arrives, the switch can use its MAC address table to determine where the frame should be forwarded.

Conceptually:

```text
MAC Address              Port

AA:AA:AA:AA:AA:AA   →    1
BB:BB:BB:BB:BB:BB   →    2
CC:CC:CC:CC:CC:CC   →    3
```

This allows traffic to be forwarded toward the appropriate device instead of simply sending every frame to every port.

Related notes:

- [Networking Fundamentals](networking-fundamentals.md)
- [Packets & Frames](packets-and-frames.md)

---

# Communication Within a LAN

Consider:

```text
192.168.1.25
      ↓
192.168.1.50
```

If both devices are on the same local network, the source device can communicate with the destination through the local network.

A simplified process is:

```text
Source Host
    ↓
Determine destination is local
    ↓
Resolve destination MAC if necessary
    ↓
Create Ethernet frame
    ↓
Switch forwards frame
    ↓
Destination Host
```

ARP can be involved when a host needs to determine the MAC address associated with an IPv4 address.

Related note:

- [DHCP & DNS](dhcp-and-dns.md)

---

# LAN vs Internet

A LAN is a local network.

The Internet is a global collection of interconnected networks.

For example:

```text
Local LAN
192.168.1.0/24
       │
       ↓
    Router
       │
       ↓
   Internet
       │
       ↓
Remote Network
```

The router provides the connection between the local network and other networks.

This distinction is important because communication inside a LAN and communication across multiple networks involve different forwarding decisions.

---

# Broadcast Communication

Some LAN communication uses **broadcasts**.

An IPv4 broadcast is intended for multiple devices on the local network.

A common example is:

```text
ARP Request
```

A host can ask:

> Who has this IP address?

Other devices on the local network receive the broadcast and the device owning that IP can respond.

Conceptually:

```text
             ┌── PC 1
             │
             ├── PC 2
Broadcast ───┼── PC 3
             │
             └── PC 4
```

Broadcast traffic is therefore an important part of understanding local network behavior.

---

# LAN Topology

LAN devices can be physically or logically arranged using different topologies.

Common examples include:

```text
Bus
Star
Ring
Mesh
```

Modern Ethernet LANs commonly use a **star topology** centered around switches.

```text
           PC
            │
            │
PC ───── Switch ───── PC
            │
            │
          Server
```

This makes individual connections easier to manage and troubleshoot than a shared bus arrangement.

Related note:

- [Network Topologies](network-topologies.md)

---

# LAN and Network Segmentation

A large LAN does not necessarily need to be one completely flat network.

Networks can be divided into logical segments.

For example:

```text
Users
  │
  └── User Network

Servers
  │
  └── Server Network

Security Devices
  │
  └── Security Network
```

Segmentation can control which systems are able to communicate with each other.

This can reduce unnecessary access and limit the potential impact of a compromised host.

Related notes:

- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)

---

# LAN Security Considerations

Because many devices share the same local infrastructure, LAN security is important.

Potential security concerns include:

- Unauthorized devices
- ARP-based attacks
- Rogue access points
- Network scanning
- Lateral movement
- Misconfigured segmentation
- Excessive network access

A compromised workstation may attempt to communicate with other systems on the LAN.

For example:

```text
Compromised Workstation
        ↓
Internal Scanning
        ↓
Other Hosts
        ↓
Potential Lateral Movement
```

This is why internal network traffic can be valuable evidence during a SOC investigation.

---

# LAN in SOC Investigations

A SOC analyst may encounter LAN-related information in:

- Firewall logs
- Network monitoring systems
- IDS/IPS alerts
- DHCP logs
- DNS logs
- Switch logs
- Endpoint telemetry
- Packet captures

For example:

```text
Source:
192.168.1.25

Destination:
192.168.1.50

Protocol:
TCP

Destination Port:
445
```

The analyst should not immediately assume that the traffic is malicious.

Instead, investigate:

```text
Who owns 192.168.1.25?
        ↓
Which device is it?
        ↓
Which user is logged in?
        ↓
What process generated the traffic?
        ↓
Is 192.168.1.50 expected?
        ↓
Is port 445 expected?
        ↓
What happened before and after?
```

This is the difference between **observing network traffic** and actually investigating it.

---

# Example: Compromised Host

Suppose a workstation is compromised.

```text
Workstation
192.168.1.25
       │
       ├──→ Server 1
       ├──→ Server 2
       ├──→ Workstation 3
       └──→ Unknown Host
```

An analyst may notice unexpected connections to several internal systems.

The important questions become:

- Is this normal behavior for the workstation?
- Which ports are being accessed?
- Are multiple hosts being scanned?
- Is the same user responsible for the activity?
- Which process generated the connections?
- Did the activity begin after a suspicious event?

Network context helps establish whether the behavior represents normal administration, an application function, or possible lateral movement.

---

# SOC Perspective

A LAN is not simply a collection of computers connected to a switch.

For a SOC analyst, it represents a **local communication environment** where valuable evidence exists.

Think of a LAN as:

```text
Hosts
  ↓
Local Communication
  ↓
Switching
  ↓
Protocols
  ↓
Network Traffic
  ↓
Security Telemetry
  ↓
Investigation
```

Understanding how normal LAN communication works makes abnormal communication easier to identify.

---

# Key Takeaways

- A **LAN** connects devices within a limited geographical area.
- Switches are commonly used to connect devices within a LAN.
- Devices use IP addresses for logical network addressing and MAC addresses for local Layer 2 communication.
- ARP can be used to associate IPv4 addresses with MAC addresses on a local network.
- Modern Ethernet LANs commonly use a star topology.
- LANs can be divided into logical segments to control communication.
- Broadcast traffic is an important part of local network communication.
- Internal traffic can provide valuable evidence during SOC investigations.
- Unexpected internal communication can be relevant when investigating scanning or lateral movement.
- Understanding normal LAN behavior is essential for identifying abnormal network activity.

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
- [IP Addressing](ip-addressing.md)
- [Ports & Protocols](ports-and-protocols.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md) 
- [Extending Your Network](extending-your-network.md)
- [Network Topologies](network-topologies.md)
