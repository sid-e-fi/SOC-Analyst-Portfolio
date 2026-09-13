---
type: concept-note
status: reviewed
tags:
  - networking
  - network-devices
  - switching
  - routing
  - wireless
  - soc
---

# Extending Your Network

> **Purpose**
>
> Networks need different devices to connect hosts, extend physical connectivity, connect separate networks, and provide wireless access. Understanding the role of these devices helps a SOC analyst understand network architecture and interpret network traffic.

---

# Why Networks Need to Be Extended

A small network may initially contain only a few devices:

```text
PC ── PC
```

As more devices are added, the network needs infrastructure that can connect them efficiently.

For example:

```text
PC
 │
PC ── Switch ── Server
 │
PC
```

When networks become larger or need to communicate with other networks, additional devices such as routers and access points are used.

---

# Repeater

A **repeater** receives a signal and regenerates it so that it can travel farther.

```text
Device
   │
   │ Weakening signal
   ↓
Repeater
   │
   │ Regenerated signal
   ↓
Destination
```

A repeater does not make routing decisions or understand the meaning of the traffic.

Its purpose is primarily to extend the physical transmission distance.

A repeater operates at the **Physical layer (Layer 1)**.

---

# Hub

A **hub** is a basic networking device that connects multiple devices.

When a hub receives a signal on one port, it repeats the signal to the other connected ports.

```text
          PC
           │
           ↓
PC ────── Hub ────── PC
           │
         Server
```

The hub does not make forwarding decisions based on MAC addresses.

As a result, traffic is shared across connected devices.

## Limitations

- Unnecessary traffic is sent to multiple devices
- Shared communication medium
- More contention as traffic increases
- Poorer performance and efficiency than modern switches
- Difficult to isolate individual communication

Hubs are largely obsolete in modern Ethernet networks.

---

# Switch

A **switch** connects devices within a LAN and forwards Ethernet frames.

```text
PC 1 ──┐
PC 2 ──┤
PC 3 ──┼── Switch
Server ─┘
```

A switch learns which MAC addresses are associated with which ports.

For example:

```text
MAC Address              Port

AA:AA:AA:AA:AA:AA   →     1
BB:BB:BB:BB:BB:BB   →     2
CC:CC:CC:CC:CC:CC   →     3
```

When a frame arrives, the switch can use the destination MAC address to determine where to forward it.

This is one of the major improvements over a hub.

Related notes:

- [LAN](lan.md)
- [Packets & Frames](packets-and-frames.md)
- [OSI Model](osi-model.md)

---

# Router

A **router** connects different networks.

For example:

```text
LAN A
192.168.1.0/24
      │
      ↓
   Router
      │
      ↓
LAN B
192.168.2.0/24
```

Routers use IP addressing to make forwarding decisions.

A router can therefore connect:

```text
Local Network
      ↓
   Router
      ↓
Other Network
```

The router determines where traffic should be sent based on its routing information.

Related notes:

- [IP Addressing](ip-addressing.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)

---

# Wireless Access Point

A **wireless access point (AP)** allows wireless devices to connect to a network.

```text
Laptop ))) 
          \
Phone ))) ── Access Point ── Switch
          /
Tablet )))
```

The access point provides wireless connectivity while connecting wireless clients to the network infrastructure.

In an enterprise environment, multiple access points may be deployed to provide wireless coverage across a building.

---

# Hub vs Switch

The difference is important.

### Hub

```text
PC 1
 │
 ↓
Hub
 ├──→ PC 2
 ├──→ PC 3
 └──→ PC 4
```

A hub repeats traffic to its other ports.

### Switch

```text
PC 1
 │
 ↓
Switch
 │
 └──→ PC 3
```

A switch uses MAC address information to make forwarding decisions.

Therefore:

> **A hub mainly repeats traffic, while a switch intelligently forwards Ethernet frames.**

---

# Switch vs Router

These devices perform different primary functions.

| Device | Primary function | Addressing |
|---|---|---|
| Switch | Connects devices within a LAN | MAC addresses |
| Router | Connects different networks | IP addresses |

Simplified:

```text
Same Network

PC ── Switch ── PC
```

versus:

```text
Different Networks

LAN A ── Switch ── Router ── Switch ── LAN B
```

A real network can contain many switches and routers working together.

---

# Extending a LAN

Suppose one switch does not have enough ports.

A second switch can be connected:

```text
PC 1 ──┐
PC 2 ──┤
        │
     Switch 1
        │
        │
     Switch 2
      /   \
    PC 3  PC 4
```

This allows more devices to join the network.

The switches must be configured appropriately so that traffic can move between them.

Large networks commonly use multiple interconnected switches.

---

# Extending Wireless Networks

Large physical areas may require multiple access points.

```text
             Network
                │
        ┌───────┴───────┐
        ↓               ↓
       AP 1            AP 2
      /   \            /   \
   Laptop Phone      PC    Tablet
```

Multiple access points can provide wireless coverage across larger areas.

Enterprise wireless networks also use authentication, encryption, and centralized management to control access.

---

# Connecting Networks

When two different IP networks need to communicate, routing is required.

For example:

```text
Network A
10.0.1.0/24
      │
      ↓
    Router
      │
      ↓
Network B
10.0.2.0/24
```

The router provides the path between the networks.

This is fundamentally different from simply adding another endpoint to the same LAN.

---

# Home Network Example

A typical home network may use a single device that combines several functions.

```text
             Internet
                 │
                 ↓
        ┌─────────────────┐
        │ Home Router     │
        │                 │
        │ Router          │
        │ Switch          │
        │ Access Point    │
        └─────────────────┘
          │      │      )))
          │      │        )))
         PC    Console   Phone
```

One physical device can therefore perform multiple networking roles.

This is why the terms "router" and "Wi-Fi router" are often used interchangeably in home environments even though the device contains several separate networking functions.

---

# Enterprise Network Example

Enterprise networks are usually more complex.

A simplified design might look like:

```text
                    Internet
                       │
                       ↓
                    Router
                       │
                    Firewall
                       │
                 Core Network
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
         Switch 1             Switch 2
          /    \                /   \
        PCs   APs             PCs   Servers
```

The actual architecture can be considerably more complex, but the basic principle remains:

```text
Endpoints
    ↓
Switching
    ↓
Routing
    ↓
Security Controls
    ↓
Other Networks
```

---

# Security Considerations

Adding networking devices increases functionality but also creates additional infrastructure that must be secured.

Potential risks include:

- Unauthorized devices
- Rogue access points
- Misconfigured switches
- Weak wireless security
- Incorrect routing rules
- Poor segmentation
- Exposed management interfaces
- Unnecessary network connectivity

A network device itself can become a security concern if it is compromised or incorrectly configured.

---

# Network Visibility

Networking infrastructure can also generate valuable security telemetry.

For example:

```text
Switch
   ↓
MAC / Port information

Router
   ↓
IP / Routing information

Firewall
   ↓
Connection / Policy information

Access Point
   ↓
Wireless client information
```

A SOC analyst can correlate these sources during an investigation.

For example:

```text
Unknown Device
      ↓
Switch Port
      ↓
MAC Address
      ↓
DHCP Lease
      ↓
IP Address
      ↓
User / Host
```

This can help determine what device is actually responsible for suspicious activity.

---

# SOC Investigation Example

Suppose a SOC alert shows:

```text
Source:
192.168.1.25

Destination:
192.168.1.100

Port:
445

Protocol:
TCP
```

The analyst should not stop at the IP addresses.

They may investigate:

```text
192.168.1.25
      ↓
Which device?
      ↓
Which switch port?
      ↓
Which MAC address?
      ↓
Which DHCP lease?
      ↓
Which user?
      ↓
Which process generated the connection?
```

The network infrastructure provides additional context that can help identify the actual endpoint.

---

# SOC Perspective

Networking devices are not just infrastructure.

They can become **sources of security evidence**.

Think of the relationship as:

```text
Endpoint
   ↓
Network Device
   ↓
Traffic
   ↓
Logs / Telemetry
   ↓
SIEM
   ↓
SOC Alert
   ↓
Investigation
```

Understanding what each device does helps an analyst understand where network evidence came from and what that evidence actually means.

---

# Key Takeaways

- A **repeater** regenerates signals to extend physical transmission distance.
- A **hub** repeats traffic to connected ports.
- A **switch** connects LAN devices and forwards frames using MAC addresses.
- A **router** connects different networks and forwards traffic using IP addressing.
- An **access point** provides wireless connectivity to a network.
- Multiple switches can be interconnected to support larger LANs.
- Multiple access points can provide wireless coverage across larger areas.
- Routers are required when traffic needs to move between different IP networks.
- Modern home networking equipment commonly combines router, switch, and access point functions.
- Networking devices can provide valuable telemetry during security investigations.
- A SOC analyst can correlate MAC, IP, DHCP, switch, routing, and endpoint information to identify devices and investigate suspicious traffic.

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [LAN](lan.md)
- [Network Topologies](network-topologies.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
- [IP Addressing](ip-addressing.md)
- [Ports & Protocols](ports-and-protocols.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md) 