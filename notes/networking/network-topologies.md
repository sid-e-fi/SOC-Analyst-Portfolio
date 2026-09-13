---
type: concept-note
status: reviewed
tags:
  - networking
  - topology
  - lan
  - network-design
  - soc
---

# Network Topologies

> **Purpose**
>
> A network topology describes how devices and connections are arranged within a network. Topology affects scalability, reliability, troubleshooting, performance, and potential points of failure.

---

# What Is Network Topology?

A **network topology** is the arrangement of devices and connections within a network.

It can describe:

- How devices are physically connected
- How devices communicate
- Where potential bottlenecks exist
- Where failures can affect the network

Common topologies include:

```text
Bus
Star
Ring
Mesh
```

---

# Bus Topology

In a **bus topology**, devices share a common communication medium called the backbone.

```text
PC ── PC ── PC ── PC
       │
   Shared Backbone
```

All connected devices use the same main communication path.

## Advantages

- Simple design
- Less cabling than some other topologies
- Historically useful for small networks

## Disadvantages

- Shared medium can become a bottleneck
- More devices increase contention
- Heavy traffic can reduce performance
- A failure affecting the backbone can affect the network
- Troubleshooting can be difficult
- Poor scalability compared with modern switched networks

### Adding Devices

Adding another device is not necessarily the main problem.

The bigger issue is that the new device also has to share the same communication medium.

```text
Few Devices

PC ── PC ── PC


More Devices

PC ── PC ── PC ── PC ── PC ── PC
        Shared Backbone
```

As traffic and the number of devices increase, the shared backbone has more work to handle.

---

# Star Topology

In a **star topology**, devices connect to a central networking device.

Modern Ethernet LANs commonly use switches as the central device.

```text
           PC
            │
            │
PC ───── Switch ───── PC
            │
            │
          Server
```

Each endpoint has its own connection to the central device.

## Advantages

- Easy to add and remove devices
- Easier troubleshooting
- Failure of one endpoint connection normally affects only that endpoint
- Better scalability than a shared bus
- Centralized management

## Disadvantages

- Central device is an important dependency
- Failure of the central switch can affect connected devices
- The central device can become a bottleneck if overloaded
- Requires more cabling than a traditional bus

---

# Why Is Star Better Than Bus?

Star topology is not dramatically better because it removes every possible bottleneck.

The important improvement is **how the network handles individual connections**.

Compare:

```text
Bus

PC ── PC ── PC ── PC
      Shared Medium
```

with:

```text
Star

      PC
       │
PC ─ Switch ─ PC
       │
     Server
```

In the bus design, devices compete for access to the same shared medium.

In the star design, each device has its own connection to the central switch.

This makes the network:

- Easier to manage
- Easier to troubleshoot
- Easier to expand
- More resilient to individual cable/device failures

The central switch does introduce a dependency, but the benefits of dedicated connections and switching generally outweigh that limitation in modern LANs.

---

# Hub-Based Star vs Switch-Based Star

A star topology can use different types of central devices.

### Hub

A hub repeats incoming traffic to connected ports.

```text
             PC
              │
PC ───────── Hub ───────── PC
              │
            Server
```

Traffic is shared more broadly between connected devices.

### Switch

A switch learns MAC addresses and can forward frames toward the appropriate port.

```text
             PC
              │
PC ─────── Switch ─────── PC
              │
            Server
```

This makes a switched star topology much more efficient than a traditional hub-based network.

Therefore:

> **The topology alone does not determine network performance. The equipment and communication method used within that topology also matter.**

---

# Mesh Topology

In a **mesh topology**, devices have multiple connections to other devices.

A full mesh can be represented as:

```text
        A
       / \
      /   \
     B─────C
      \   /
       \ /
        D
```

The multiple paths provide redundancy.

## Advantages

- High redundancy
- Multiple possible paths
- Better resilience against individual connection failures

## Disadvantages

- Expensive
- More complex
- Requires more connections
- Difficult to scale as the number of devices increases

Mesh topology is useful where redundancy and availability are more important than simplicity.

---

# Ring Topology

In a **ring topology**, each device connects to two neighboring devices, forming a loop.

```text
PC ── PC
│      │
PC ── PC
```

Traffic can travel around the ring according to the technology being used.

## Advantages

- Predictable communication structure
- Can provide organized traffic flow

## Disadvantages

- A failure can affect communication depending on the implementation
- Adding or removing devices can be more complicated
- Less common in modern general-purpose LANs

---

# Topology Comparison

| Topology | Main characteristic | Main advantage | Main limitation |
|---|---|---|---|
| Bus | Shared backbone | Simple | Shared medium and bottleneck |
| Star | Central device | Easy management and troubleshooting | Central device dependency |
| Mesh | Multiple paths | High redundancy | Cost and complexity |
| Ring | Circular connection | Structured traffic flow | Failure and expansion concerns |

---

# Failure Scenarios

Understanding topology helps predict how failures affect a network.

### Bus

```text
PC ── PC ── X ── PC ── PC
```

A failure involving the shared backbone can affect multiple devices.

### Star

```text
PC ──┐
PC ──┤
PC ──┼── X
PC ──┤
PC ──┘
```

If one endpoint cable fails:

```text
PC ── X
```

the other devices can normally continue communicating.

However, if the central switch fails:

```text
PC ──┐
PC ──┤
PC ── X
PC ──┤
PC ──┘
```

multiple connected devices can lose connectivity.

### Mesh

```text
A ───── B
 \     / 
  \   /
   \ /
    C
```

If one path fails, another path may still be available.

---

# Scalability

Topology affects how easily a network can grow.

### Bus

Adding devices is physically possible, but additional devices increase competition for the shared medium.

### Star

Adding a device generally means connecting it to an available switch port.

```text
Existing:

PC ── Switch ── PC
        │
      Server


New device:

PC ── Switch ── PC
        │
      Server
        │
       PC
```

The central switch does have finite port capacity and processing capacity, so a star network is not infinitely scalable.

Large networks therefore use multiple switches and hierarchical designs.

---

# Network Topology and Security

Topology also affects security and network visibility.

For example, a switched network can provide more control over individual connections than a traditional shared-medium network.

Network segmentation can further separate groups of systems:

```text
Users
  │
Switch
  │
User Network


Servers
  │
Switch
  │
Server Network
```

This can reduce unnecessary communication between systems.

Related notes:

- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)

---

# SOC Perspective

A SOC analyst does not normally investigate an incident by asking only:

> "What topology does this network use?"

Instead, topology provides context for interpreting network evidence.

For example:

```text
Alert
  ↓
Source Host
  ↓
Local Network
  ↓
Switch / Segment
  ↓
Destination Host
  ↓
Traffic
```

Understanding the network structure helps answer questions such as:

- Is the source and destination on the same network?
- Is the traffic expected between these systems?
- Is the host communicating across a network boundary?
- Could segmentation have prevented the communication?
- Is a suspicious host contacting many systems?

Topology therefore contributes to understanding the **communication paths and trust boundaries** within an environment.

---

# Key Takeaways

- A network topology describes how devices and connections are arranged.
- **Bus** uses a shared communication medium.
- **Star** connects devices through a central device.
- **Mesh** provides multiple communication paths.
- **Ring** connects devices in a circular structure.
- Bus networks can struggle as more devices and traffic share the same medium.
- Star networks are easier to manage and troubleshoot because individual devices have separate connections to the central device.
- A star network still has a potential central bottleneck or single point of failure.
- A switch makes a star topology more efficient than a traditional hub-based network.
- Mesh provides redundancy at the cost of complexity and infrastructure.
- Topology affects reliability, scalability, troubleshooting, and security.
- SOC analysts can use network topology as context when investigating communication and lateral movement.

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [LAN](lan.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
- [IP Addressing](ip-addressing.md)
- [Ports & Protocols](ports-and-protocols.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Subnetting & CIDR](subnetting-and-cidr.md) 
- [Extending Your Network](extending-your-network.md)
- [Home Lab Network Diagram](home-lab-network-diagram.md)
