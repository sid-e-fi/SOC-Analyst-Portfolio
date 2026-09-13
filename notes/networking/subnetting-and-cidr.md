---
type: concept-note
status: reviewed
tags:
  - networking
  - subnetting
  - cidr
  - network-segmentation
  - soc
---

# Subnetting & CIDR

> **Purpose**
>
> This note explains how IP networks are divided into smaller logical networks using subnetting and CIDR notation. It focuses on the level of subnetting required for SOC Analyst L1 work, including network boundaries, network and host portions, `/24` notation, and the relationship between subnetting and network segmentation.
>
> Advanced subnet calculations are outside the current scope and can be added later if required.

---

# What Is a Subnet?

A **subnet**, or subnetted network, is a smaller logical network created from a larger IP network.

Subnetting allows organizations to divide their network into separate logical sections.

For example:

```text
192.168.10.0/24
```

could represent an employee network, while:

```text
192.168.20.0/24
```

could represent a server network.

These are different IP networks even though both use private IPv4 addressing.

---

# Why Subnet Networks?

Subnetting helps organizations organize and control their networks.

Common reasons include:

- Network organization
- Reducing unnecessary communication
- Limiting lateral movement
- Separating departments or security zones
- Applying different network policies
- Controlling access between network segments
- Reducing the potential blast radius of a compromise

For example:

```text
Employee Network
192.168.10.0/24
        │
        │ controlled access
        ↓
Server Network
192.168.20.0/24
        │
        │ restricted
        ↓
Security Network
192.168.30.0/24
```

Subnetting provides the network boundaries, while controls such as firewalls and ACLs can enforce restrictions between those networks.

---

# CIDR Notation

Modern IPv4 networks commonly use **Classless Inter-Domain Routing (CIDR)** notation.

Example:

```text
192.168.10.0/24
```

The `/24` is the **prefix length**.

IPv4 addresses contain:

```text
32 bits
```

Therefore:

```text
32 total bits
- 24 network bits
----------------
8 host bits
```

The prefix determines how much of the address belongs to the network portion.

---

# Network and Host Portions

For:

```text
192.168.10.0/24
```

the first 24 bits represent the network portion and the remaining 8 bits represent the host portion.

Conceptually:

```text
192.168.10 | xxx
-----------   ---
 Network      Host
```

Therefore, addresses such as:

```text
192.168.10.10
192.168.10.25
192.168.10.50
192.168.10.200
```

belong to the same `/24` network.

A different network could be:

```text
192.168.20.0/24
```

---

# `/24` Network

A `/24` leaves 8 bits for the host portion.

This gives:

```text
2^8 = 256
```

total addresses.

In a typical IPv4 `/24` network, the first address identifies the network and the final address is used as the broadcast address.

For example:

```text
192.168.10.0      Network address
192.168.10.1      Host
192.168.10.2      Host
...
192.168.10.254    Host
192.168.10.255    Broadcast
```

This results in **254 commonly usable host addresses**.

---

# Network Address

The **network address** identifies the subnet itself rather than an individual host.

For:

```text
192.168.10.0/24
```

the network address is:

```text
192.168.10.0
```

It represents the network:

```text
192.168.10.0/24
```

rather than a normal host within it.

---

# Broadcast Address

In traditional IPv4 subnetting, the **broadcast address** is the final address in the subnet.

For:

```text
192.168.10.0/24
```

the broadcast address is:

```text
192.168.10.255
```

It can be used for communication intended for all hosts on the local IPv4 subnet in broadcast scenarios.

---

# CIDR Controls Network Size

The CIDR prefix determines how much address space belongs to the network.

For example:

| Prefix | Total addresses |
| ------ | --------------- |
| `/24` | 256 |
| `/25` | 128 |
| `/26` | 64 |
| `/27` | 32 |

As the prefix length increases, more bits are used for the network portion and fewer remain for hosts.

Therefore:

```text
/24
 ↓
larger network

/25
 ↓
smaller network

/26
 ↓
even smaller network
```

The important concept for SOC L1 is understanding that **the prefix length determines the network boundary and therefore the size of the subnet**.

---

# Subnet Boundaries Matter

Do not determine whether two IP addresses belong to the same network simply by looking at their first few decimal numbers.

The CIDR prefix matters.

For example:

```text
192.168.10.0/24
```

means:

```text
192.168.10.1
192.168.10.50
192.168.10.200
```

are within the same `/24` subnet.

But a network can be divided further:

```text
192.168.10.0/25
192.168.10.128/25
```

Now the original address space is divided into two smaller networks.

Therefore, two addresses beginning with:

```text
192.168.10
```

are not necessarily in the same subnet.

The CIDR prefix must be considered.

---

# Classful Addressing vs CIDR

Historically, IPv4 used a **classful addressing system**.

The traditional classes were:

| Class | First octet | Default prefix |
|---|---|---|
| A | `1-126` | `/8` |
| B | `128-191` | `/16` |
| C | `192-223` | `/24` |

For example:

```text
192.168.1.1
```

historically falls within the Class C range.

However, modern networking uses **CIDR** rather than relying on the old class system to determine network boundaries.

For example:

```text
192.168.1.0/24
192.168.1.0/25
192.168.1.0/26
```

The prefix explicitly defines the network boundary.

Therefore:

> **Class A, B, and C are useful historical concepts, but CIDR is the modern addressing model.**

---

# Subnets and Routing

Subnetting and routing are closely related.

A host determines whether a destination is within its local subnet.

Conceptually:

```text
Destination
     ↓
Same subnet?
   /     \
 YES      NO
 ↓         ↓
Local     Routing
communication
           ↓
     Default Gateway
```

For example:

```text
Host:
192.168.10.25/24
```

communicating with:

```text
192.168.10.50
```

is local because both addresses belong to:

```text
192.168.10.0/24
```

But:

```text
192.168.10.25
```

communicating with:

```text
192.168.20.50
```

requires routing because the destination belongs to a different subnet.

Routing concepts are covered in:

[Routing & Network Segmentation](routing-and-network-segmentation.md)

---

# Subnets in SOC Investigations

Subnet information provides useful context when investigating network traffic.

Suppose a company documents:

```text
192.168.10.0/24 → Employee Network
192.168.20.0/24 → Server Network
```

The SIEM reports:

```text
Source:
192.168.10.25

Destination:
192.168.20.50

Protocol:
TCP

Destination Port:
445
```

From the subnet information, the analyst can establish:

> A host in the employee network is communicating with a host in the server network.

This indicates communication across network boundaries.

However, subnet information alone does not prove:

- Malicious activity
- Unauthorized access
- Successful communication
- Compromise
- Data transfer

Additional evidence is required.

---

# Network Segmentation and Security

Subnetting can support security architecture by creating logical boundaries between systems.

For example:

```text
Employee Network
       │
       ↓
Firewall / ACL
       │
       ↓
Server Network
```

A compromised employee workstation may therefore have restricted access to sensitive server networks.

This can reduce the potential for unrestricted lateral movement.

However:

> **A subnet is not itself a security control.**

Different subnets can still communicate if routing and security policies permit it.

Actual restrictions may be enforced through:

- Firewalls
- ACLs
- VLAN configuration
- Routing policies
- Security groups

---

# SOC Perspective

Subnet information helps an analyst understand **where a host sits within the network architecture**.

When investigating an IP address, the analyst may want to know:

- Which subnet does it belong to?
- Which department or security zone owns that subnet?
- Is the destination in the same subnet?
- Is traffic crossing a network boundary?
- Is that communication expected?
- What controls exist between the networks?

This context can significantly change the interpretation of an alert.

For example:

```text
Employee PC
      ↓
Employee subnet
      ↓
Server subnet
      ↓
Database server
```

may deserve more scrutiny than communication between two hosts within the same workstation subnet, depending on the organization's expected traffic patterns.

---

# Interview Notes

### What is subnetting?

Subnetting is the process of dividing a larger IP network into smaller logical networks.

### What does `/24` mean?

`/24` means that 24 of the 32 IPv4 bits are used for the network portion, leaving 8 bits for hosts.

### What is CIDR?

CIDR is a classless IP addressing method that uses a prefix length such as `/24` to explicitly define the network boundary.

### Are Class A, B, and C still used to determine modern subnet boundaries?

No. The classful system is legacy. Modern networks use CIDR and explicit prefix lengths.

### Why does subnetting matter to a SOC analyst?

Subnetting provides network context. It allows analysts to determine which logical network an IP belongs to and whether traffic is crossing network boundaries or security zones.

---

# Key Takeaways

- **A subnet is a smaller logical network.**
- **CIDR notation defines the network boundary.**
- **IPv4 addresses contain 32 bits.**
- **`/24` means 24 network bits and 8 host bits.**
- **A typical `/24` contains 256 total addresses and 254 commonly usable host addresses.**
- **The network and broadcast addresses are not normally assigned to individual hosts.**
- **Larger CIDR prefixes create smaller networks.**
- **Class A, B, and C addressing is legacy; modern networking uses CIDR.**
- **Subnetting supports network organization and segmentation.**
- **Subnet boundaries help SOC analysts understand network context and traffic paths.**
- **A subnet alone does not prevent communication. Security controls must enforce restrictions between networks.**

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [IP Addressing](ip-addressing.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Network Security](../security-fundamentals/network-security.md)
- [Ports & Protocols](ports-and-protocols.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Extending Your Network](extending-your-network.md)
- [Home Lab Network Diagram](home-lab-network-diagram.md)
- [LAN](lan.md)
- [Network Topologies](network-topologies.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
