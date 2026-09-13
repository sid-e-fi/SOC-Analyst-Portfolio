---
type: concept-note
status: reviewed
tags:
  - networking
  - ip-addressing
  - ipv4
  - nat
  - soc
---

# IP Addressing

> **Purpose**
>
> This note explains how devices and network interfaces are logically addressed using IP addresses, the difference between private and public IPv4 addresses, and how IP addressing relates to NAT and SOC investigations.
>
> For subnet boundaries and CIDR notation, see [Subnetting & CIDR](subnetting-and-cidr.md). For physical network interface addressing, see the MAC address section below.

---

# What Is an IP Address?

An **IP address** is a logical network address used to identify a network interface and enable communication between hosts across IP networks.

An IPv4 address consists of **32 bits** and is normally written as four decimal octets.

Example:

```text
192.168.1.25
```

The address identifies the interface from a network communication perspective. It should not be treated as a permanent identity of the physical device.

A device can have:

- Multiple network interfaces
- Multiple IP addresses
- Different IP addresses at different times

---

# IPv4 Address Structure

An IPv4 address contains four octets:

```text
192.168.1.25
 │    │  │ │
 └────┴──┴─┴── Four octets
```

Each octet can have a value from:

```text
0 to 255
```

IPv4 addresses contain 32 bits in total.

The division between the **network portion** and **host portion** depends on the subnet configuration.

For example:

```text
192.168.1.25/24
```

The `/24` defines the network boundary.

Detailed subnetting and CIDR concepts are covered in:

[Subnetting & CIDR](subnetting-and-cidr.md)

---

# Private vs Public IP Addresses

IPv4 addresses can be used in different addressing scopes.

## Private IP Addresses

Private addresses are intended for use inside private networks and are not directly routable across the public Internet.

The main private IPv4 ranges are:

| Range | CIDR |
| ---------------------------------- | ---------------- |
| `10.0.0.0` to `10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0` to `172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0` to `192.168.255.255` | `192.168.0.0/16` |

Example:

```text
192.168.1.25
```

is a private IPv4 address.

Private addressing is commonly used for:

- Home networks
- Enterprise networks
- Internal servers
- Virtual machines
- Internal network infrastructure

---

## Public IP Addresses

A public IP address is globally routable within the public Internet and can be used to identify an Internet-facing network endpoint.

For example:

```text
203.x.x.x
```

may represent a public Internet address.

A public IP does not necessarily identify a single device. It may represent a router, firewall, server, cloud service, or another network endpoint.

---

# Private IP Does Not Mean "Invisible"

A private IP address is not directly routable across the public Internet, but that does not make the device invisible.

For example:

```text
Laptop
192.168.1.25
      ↓
Router / NAT
      ↓
Public IP
      ↓
Internet
```

External systems will generally see the public address used by the network's NAT or other Internet-facing infrastructure rather than the laptop's private address.

This distinction is important when interpreting network logs.

---

# NAT

**Network Address Translation (NAT)** translates IP addresses between different addressing contexts.

A common home-network example is:

```text
Laptop
192.168.1.25
      ↓
Router / NAT
      ↓
Public IP
203.x.x.x
      ↓
Internet
```

Multiple internal devices can therefore appear to external Internet services as traffic originating from the same public IP.

For example:

```text
Laptop   → 192.168.1.25 ┐
Phone    → 192.168.1.26 ├→ NAT → 203.x.x.x → Internet
TV       → 192.168.1.27 ┘
```

This means an external server seeing:

```text
Source IP: 203.x.x.x
```

cannot necessarily identify which internal device generated the traffic.

Additional evidence may be required, such as:

- NAT logs
- DHCP records
- Firewall logs
- Endpoint telemetry
- Timestamps

---

# IP Address vs MAC Address

IP addresses and MAC addresses serve different purposes.

| IP Address | MAC Address |
|---|---|
| Logical network address | Link-layer interface identifier |
| Used for IP routing | Used for local network communication |
| Can change | Often associated with network hardware |
| Can be assigned dynamically | Can be randomized or spoofed |
| Relevant across routed networks | Primarily relevant within the local network |

A MAC address is commonly associated with a network interface's hardware identity, but it should not be treated as permanently trustworthy because operating systems can randomize or spoof MAC addresses.

A useful mental model is:

```text
MAC
 ↓
Local network interface identity

IP
 ↓
Logical network addressing
```

---

# Multiple Network Interfaces

A device can have multiple network interfaces.

For example, a laptop could have:

```text
Wi-Fi
192.168.1.20

Ethernet
192.168.1.21
```

Both addresses can belong to the same physical laptop.

The operating system determines which interface and route to use based on its routing configuration.

Therefore, an IP address should not automatically be interpreted as the identity of an entire physical device without additional context.

---

# IP Address Attribution

SOC analysts frequently need to answer:

> **Which device was using this IP address at the time of the event?**

An IP address alone may not be sufficient.

For example:

```text
Alert
 ↓
192.168.1.25
 ↓
DHCP records
 ↓
Device / interface
 ↓
User or hostname
```

The answer may also require:

- Timestamp
- DHCP lease information
- MAC address
- Hostname
- Authentication logs
- Endpoint telemetry
- NAT records

This is particularly important when investigating private IP addresses in networks where addresses are dynamically assigned.

---

# IP Addresses in SOC Alerts

A network alert may contain information such as:

```text
Source IP:
192.168.10.25

Destination IP:
192.168.20.50

Destination Port:
445

Protocol:
TCP
```

The IP addresses tell the analyst which network endpoints are involved.

However, the addresses alone do not establish:

- Which user generated the traffic
- Which process generated it
- Whether the connection succeeded
- Whether the activity was malicious
- What application protocol was actually used

Additional telemetry is required.

---

# Evidence vs Inference

An IP address provides useful evidence, but an analyst should avoid assigning meaning that the evidence does not support.

For example:

```text
Source IP:
192.168.10.25
```

does not automatically prove:

> "Employee John generated this traffic."

You would need organizational context and additional telemetry to make that attribution.

Similarly:

```text
Source IP:
203.x.x.x
```

does not automatically identify a specific internal device if NAT is involved.

The correct approach is:

> **Use IP addresses as evidence and correlate them with other data before making attribution decisions.**

---

# SOC Relevance

IP addressing is foundational to network-based security monitoring.

SOC analysts commonly use IP addresses to:

- Identify communication endpoints
- Investigate internal network traffic
- Identify Internet-facing destinations
- Correlate firewall and network events
- Investigate suspicious connections
- Attribute activity to hosts
- Trace network communication across security controls

IP addresses become significantly more useful when combined with:

```text
IP
 ↓
Timestamp
 ↓
Port
 ↓
Protocol
 ↓
DNS
 ↓
DHCP
 ↓
Endpoint telemetry
 ↓
User / process
```

This allows the analyst to move from:

> "An IP address generated traffic."

toward:

> "This host, associated with this user and process, communicated with this destination at this time."

---

# Interview Notes

### What is an IP address?

An IP address is a logical network address used to identify a network interface and enable communication across IP networks.

### What is the difference between private and public IP addresses?

Private IP addresses are intended for internal networks and are not directly routable across the public Internet. Public IP addresses are globally routable Internet addresses.

### Why can multiple devices appear to have the same public IP?

NAT can translate multiple private internal addresses to a shared public address when communicating with external networks.

### Is an IP address a permanent identity for a device?

No. IP addresses can change, can be dynamically assigned, and a single device can have multiple interfaces and addresses.

### Why is IP attribution important in a SOC?

An alert containing an IP address does not necessarily identify the exact device or user responsible. Analysts often need DHCP, NAT, endpoint, authentication, and timestamp information to establish attribution.

---

# Key Takeaways

- **IP addresses provide logical network addressing.**
- **IPv4 addresses contain 32 bits and are represented using four octets.**
- **Private IP addresses are used within private networks.**
- **Public IP addresses are globally routable Internet addresses.**
- **Private addresses are not directly routable across the public Internet.**
- **NAT can translate multiple private addresses to a shared public address.**
- **An IP address is not a permanent identity of a physical device.**
- **A device can have multiple interfaces and IP addresses.**
- **MAC and IP addresses operate at different networking layers and serve different purposes.**
- **SOC analysts should correlate IP addresses with timestamps and additional telemetry before attributing activity to a specific device or user.**

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [DHCP & DNS](dhcp-and-dns.md)
- [Ports & Protocols](ports-and-protocols.md)
- [Network Security](../security-fundamentals/network-security.md)
- [Extending Your Network](extending-your-network.md)
- [Home Lab Network Diagram](home-lab-network-diagram.md)
- [LAN](lan.md)
- [Network Commands & Troubleshooting](network-commands-and-troubleshooting.md)
- [Network Topologies](network-topologies.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
