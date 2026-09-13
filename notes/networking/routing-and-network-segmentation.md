---
type: concept-note
status: reviewed
tags:
  - networking
  - routing
  - network-segmentation
  - network-security
  - soc
---

# Routing & Network Segmentation

> **Purpose**
>
> This note explains how traffic moves between networks, the role of default gateways and routing tables, and how organizations use network segmentation to control communication between different network zones.
>
> For IP addressing and CIDR, see [IP Addressing](ip-addressing.md) and [Subnetting & CIDR](subnetting-and-cidr.md).

---

# What Is Routing?

**Routing** is the process of determining how network traffic should travel from a source network to a destination network.

A host first determines whether the destination is reachable through its local network.

If the destination is on another network, the host uses an appropriate route, commonly through its **default gateway**.

A simplified model is:

```text
Source Host
    ↓
Is destination local?
    ↓
 YES ─────────────→ Local communication
    │
    NO
    ↓
Routing decision
    ↓
Default Gateway
    ↓
Router / Firewall
    ↓
Destination Network
```

---

# Default Gateway

The **default gateway** is the next-hop device a host uses when traffic does not match a more specific route.

In a typical home network, the default gateway is usually the local router.

Example:

```text
Laptop
192.168.1.25/24
       │
       ↓
Default Gateway
192.168.1.1
       │
       ↓
Router
       │
       ↓
Internet
```

The default gateway does not necessarily have to be a traditional router. Depending on the network architecture, it could also be a firewall or Layer 3 switching device.

---

# Local vs Remote Destinations

Consider a host configured as:

```text
IP:
192.168.1.25

Subnet:
192.168.1.0/24

Gateway:
192.168.1.1
```

If it communicates with:

```text
192.168.1.50
```

the destination is within the same subnet.

The host can communicate locally without sending the traffic to its default gateway.

However, if it communicates with:

```text
192.168.2.50
```

the destination belongs to a different network.

The host therefore needs a route toward that network, commonly through:

```text
192.168.1.1
```

---

# Routing Tables

Operating systems and network devices maintain **routing tables** that contain information about where traffic should be sent.

A routing table can contain routes for:

- Directly connected networks
- Specific remote networks
- A default route
- Different network interfaces
- Different next-hop gateways

A simplified example:

```text
Destination          Next Hop
192.168.1.0/24       Local
192.168.2.0/24       192.168.1.1
0.0.0.0/0            192.168.1.1
```

The default route:

```text
0.0.0.0/0
```

can be thought of as:

> "If no more specific route matches the destination, send the traffic here."

The exact routing process can become significantly more complex in enterprise networks, but this model is sufficient for understanding basic network traffic as a SOC Analyst L1.

---

# Routing vs Firewalling

Routing and security policy are related but are **not the same thing**.

### Routing

Determines:

> **Where can the packet be sent?**

### Firewall / ACL

Determines:

> **Is this traffic allowed?**

For example:

```text
Employee Network
192.168.10.0/24
        │
        ↓
     Firewall
        │
        ↓
Server Network
192.168.20.0/24
```

A route may exist between the networks, but the firewall can still block the communication.

Therefore:

> **A route existing does not mean the traffic is authorized.**

---

# Network Segmentation

**Network segmentation** divides an organization's infrastructure into separate logical or physical network zones.

For example:

```text
192.168.10.0/24
Employee Network

192.168.20.0/24
Server Network

192.168.30.0/24
Security Network
```

Different network zones can have different:

- Security policies
- Access requirements
- Monitoring
- Trust levels
- Types of systems

---

# Why Organizations Segment Networks

Segmentation can help organizations:

### Reduce attack surface

Systems do not necessarily need unrestricted connectivity to every other system.

### Limit lateral movement

If an attacker compromises one workstation, segmentation can restrict access to other network zones.

### Control access

Organizations can define which networks are allowed to communicate.

### Reduce blast radius

A compromise in one network segment may be contained rather than immediately exposing the entire environment.

### Improve monitoring

Traffic crossing important network boundaries can provide useful security telemetry.

---

# Segmentation Does Not Automatically Provide Security

A common misconception is:

> "Different subnets cannot communicate."

That is false.

Different subnets can communicate through routing.

For example:

```text
Employee Network
192.168.10.0/24
        │
        ↓
Router / Firewall
        │
        ↓
Server Network
192.168.20.0/24
```

Whether the communication is permitted depends on the network's security controls.

Possible controls include:

- Firewalls
- Access Control Lists
- VLAN configuration
- Routing policies
- Security groups

Therefore:

> **Subnetting creates network boundaries. Security controls enforce access across those boundaries.**

---

# Network Segmentation and Lateral Movement

Segmentation is particularly important when considering **lateral movement**.

Suppose an attacker compromises:

```text
Employee PC
192.168.10.25
```

Without effective segmentation, the attacker may have many potential paths to:

```text
File Servers
Database Servers
Application Servers
Domain Controllers
Security Systems
```

With appropriate segmentation and access controls:

```text
Compromised PC
      │
      ↓
Employee Network
      │
      ↓
Firewall / ACL
      │
      ├── Allowed → Required services
      │
      └── Blocked → Sensitive systems
```

The objective is not necessarily to make every network completely isolated.

The objective is to **allow necessary communication while restricting unnecessary access**.

---

# SOC Investigation Example

Suppose the organization documents:

```text
192.168.10.0/24 → Employee Network
192.168.20.0/24 → Server Network
```

The SIEM generates:

```text
Source:
192.168.10.25

Destination:
192.168.20.50

Destination Port:
445

Protocol:
TCP
```

The analyst can establish:

> A host in the employee network is communicating with a host in the server network over TCP toward destination port 445.

This is **cross-network communication**.

However, that does not automatically mean the event is malicious.

The analyst should investigate:

- What is `192.168.10.25`?
- What is `192.168.20.50`?
- Is this communication normally allowed?
- Is the source host expected to access the destination?
- Did the firewall permit or block the connection?
- Was a TCP session actually established?
- Which process generated the traffic?
- What user was logged in?
- Did other suspicious activity occur?

---

# Routing Information as SOC Evidence

Routing information can help analysts understand how traffic should move through the environment.

For example:

```text
Employee PC
     ↓
Employee Gateway
     ↓
Firewall
     ↓
Server Network
```

If an alert shows traffic between these networks, the analyst can determine which network boundary the traffic crossed and which security controls may have handled it.

This can lead to additional evidence from:

- Firewall logs
- Router logs
- Network monitoring systems
- IDS/IPS alerts
- Endpoint telemetry
- Authentication logs

---

# SOC Perspective

When investigating network activity, a SOC analyst should ask:

> **Where is the source located?**

> **Where is the destination located?**

> **Are they on the same subnet?**

> **If not, what route connects them?**

> **What security controls exist between them?**

> **Is the communication expected?**

> **Was it allowed or blocked?**

> **What additional telemetry can confirm what happened?**

This provides network context before making a security determination.

---

# Interview Notes

### What is routing?

Routing is the process of determining where network traffic should be forwarded to reach its destination.

### What is a default gateway?

The default gateway is the next-hop device used when traffic does not match a more specific route.

### What is the difference between routing and firewalling?

Routing determines where traffic can be forwarded, while firewall policies determine whether specific traffic is permitted.

### Why is network segmentation important?

Segmentation separates systems into logical or physical network zones, helping organizations control communication, limit lateral movement, and reduce the potential blast radius of a compromise.

### Does putting systems in different subnets automatically block communication?

No. Different subnets can communicate through routing. Security controls such as firewalls and ACLs must enforce restrictions.

---

# Key Takeaways

- **Routing determines how traffic reaches a destination network.**
- **A default gateway is commonly used for traffic leaving the local network.**
- **Routing tables contain information used to make forwarding decisions.**
- **A route existing does not mean traffic is authorized.**
- **Firewalls and ACLs enforce access policies between networks.**
- **Network segmentation divides infrastructure into logical or physical zones.**
- **Segmentation can reduce lateral movement and limit blast radius.**
- **Different subnets can communicate if routing and security policies permit it.**
- **SOC analysts use subnet and routing context to understand where traffic is moving and which security controls may be involved.**

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [IP Addressing](ip-addressing.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Ports & Protocols](ports-and-protocols.md)
- [Network Security](../security-fundamentals/network-security.md)
- [Incident Response](../security-fundamentals/incident-response.md)
- [Extending Your Network](extending-your-network.md)
- [Home Lab Network Diagram](home-lab-network-diagram.md)
- [LAN](lan.md)
- [Network Topologies](network-topologies.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
