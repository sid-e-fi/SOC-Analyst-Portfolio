---
type: lab-note
status: active
day: 11
topic: Home Lab Network Diagram
tags:
  - networking
  - home-lab
  - vmware
  - virtualization
  - nat
  - subnetting
  - gateways
  - soc
---

# Home Lab Network Diagram

> **Purpose**
>
> Document the network architecture of my SOC home lab and show how the physical network, Ubuntu host, VMware virtual networking, and Windows 11 VM are connected. The goal is to understand how traffic moves between network segments and how gateways and subnets determine where traffic is sent.

---

# Architecture

My current lab uses **Ubuntu as the physical host operating system** and **Windows 11 as the virtual machine**.

There is no separate Ubuntu VM because Ubuntu is already running as the host operating system.

The simplified architecture is:

```text
Internet
    │
    ↓
Home Router
192.168.1.1
    │
    ↓
Ubuntu Host
192.168.1.24
    │
    ↓
VMware VMnet8
192.168.27.1
    │
    ↓
VMware NAT Gateway
192.168.27.2
    │
    ↓
Windows 11 VM
192.168.27.128
```

The important distinction is that **VMware VMnet8 is a virtual networking layer created by VMware**, not a physical switch or physical network segment.

---

# Physical Network

The physical network consists of the home router and the Ubuntu host.

```text
Internet
    │
    ↓
Home Router
192.168.1.1
    │
    ↓
Ubuntu Host
192.168.1.24
```

### Home Router

| Property | Value |
|---|---|
| Role | Home network gateway |
| IP Address | `192.168.1.1` |
| Network | `192.168.1.0/24` |

The router provides the Ubuntu host with access to the external network.

### Ubuntu Host

| Property | Value |
|---|---|
| Role | Physical host OS |
| OS | Ubuntu |
| LAN IP | `192.168.1.24` |
| Network | `192.168.1.0/24` |

Ubuntu is the physical machine running VMware Workstation.

It participates directly in the physical home network while also providing the host-side interface for VMware's virtual networking.

---

# Virtual Network

VMware creates a separate virtual network for the Windows VM.

The lab uses **VMnet8**, which is VMware's NAT network.

```text
Ubuntu Host
     │
     │
     ↓
VMnet8
192.168.27.0/24
     │
     ↓
NAT Gateway
192.168.27.2
     │
     ↓
Windows 11 VM
192.168.27.128
```

This creates a separate IP subnet from the physical home network.

| Network | Purpose |
|---|---|
| `192.168.1.0/24` | Physical home LAN |
| `192.168.27.0/24` | VMware VMnet8 virtual NAT network |

The two networks are therefore different subnets.

---

# Windows 11 VM

The Windows VM is connected to VMnet8.

Observed configuration:

| Property | Value |
|---|---|
| Hostname | `WIN-LAB` |
| IPv4 Address | `192.168.27.128` |
| Subnet Mask | `255.255.255.0` |
| Network | `192.168.27.0/24` |
| Default Gateway | `192.168.27.2` |
| DHCP Server | `192.168.27.254` |
| DNS Server | `192.168.27.2` |
| Network Mode | VMware NAT |

The Windows VM therefore belongs to the `192.168.27.0/24` subnet rather than the physical `192.168.1.0/24` subnet.

---

# Why the Gateway Matters

The Windows VM has:

```text
IP:
192.168.27.128

Subnet:
192.168.27.0/24

Gateway:
192.168.27.2
```

The subnet determines whether a destination is considered local or remote.

For example:

```text
Windows VM
192.168.27.128
      │
      │ Destination:
      │ 192.168.27.1
      ↓
Same subnet
```

`192.168.27.1` belongs to the same `/24` subnet, so Windows can communicate with it directly without sending the traffic to its default gateway.

However:

```text
Windows VM
192.168.27.128
      │
      │ Destination:
      │ 192.168.1.1
      ↓
Different subnet
      │
      ↓
Default Gateway
192.168.27.2
```

`192.168.1.1` belongs to a different subnet.

Windows therefore treats it as a remote destination and sends the traffic toward its default gateway.

---

# Connectivity Observations

The following tests were performed from the Windows VM.

## 1. Ping VMware Host Interface

```text
ping 192.168.27.1
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This demonstrates successful communication between the Windows VM and the host-side VMnet8 interface.

---

## 2. Ping VMware NAT Gateway

```text
ping 192.168.27.2
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This demonstrates that the Windows VM can reach its configured default gateway.

---

## 3. Ping Physical Home Router

```text
ping 192.168.1.1
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

The destination is on a different subnet from the Windows VM.

The traffic therefore needs to be routed through the VMware NAT layer before reaching the physical network.

---

## 4. Ping External IP

```text
ping 8.8.8.8
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This demonstrates that the Windows VM can reach an external IP through the virtual NAT network and the physical network.

A successful ping does not prove that every type of network traffic is allowed. It only demonstrates successful ICMP communication at the time of the test.

---

# Traffic Paths

## Windows VM to Same Subnet

Example:

```text
192.168.27.128
       │
       ↓
192.168.27.1
```

Both addresses belong to:

```text
192.168.27.0/24
```

The destination is therefore considered local to the Windows VM.

The default gateway is not required for the initial routing decision.

---

## Windows VM to Physical Router

```text
Windows VM
192.168.27.128
       │
       ↓
VMware NAT Gateway
192.168.27.2
       │
       ↓
Ubuntu Host
       │
       ↓
Home Router
192.168.1.1
```

The important point is that the Windows VM and physical router are on different IP subnets.

The Windows VM therefore sends traffic toward its default gateway.

---

## Windows VM to Internet

```text
Windows VM
192.168.27.128
       │
       ↓
VMware NAT Gateway
192.168.27.2
       │
       ↓
Ubuntu Host
       │
       ↓
Home Router
192.168.1.1
       │
       ↓
Internet
```

VMware NAT allows the private address space used by the VM to communicate through the host's physical network connection.

---

# Packet Tracer Representation

Cisco Packet Tracer was used to create a logical representation of the lab.

The Packet Tracer diagram does **not** reproduce the physical VMware implementation internally.

Instead, VMware components were represented using network devices so that the logical relationships could be visualized.

The diagram represents:

```text
Internet
    │
    ↓
Home Router
    │
    ↓
Ubuntu Host
    │
    ↓
VMnet8
    │
    ↓
VMware NAT Gateway
    │
    ↓
Windows 11 VM
```

The grey region in the diagram represents the **virtual networking layer created by VMware Workstation inside the Ubuntu host**.

The switch and router symbols inside this region are logical representations used for visualization. They should not be interpreted as physical Cisco hardware existing inside the Ubuntu machine.

---

# Network Segmentation

The lab contains two important IP networks:

| Network | Example Host | Purpose |
|---|---|---|
| `192.168.1.0/24` | Ubuntu `192.168.1.24` | Physical LAN |
| `192.168.27.0/24` | Windows `192.168.27.128` | VMware NAT network |

This separation is important because it demonstrates basic network segmentation.

The Windows VM does not simply become another host on the physical `192.168.1.0/24` network.

Instead, VMware places it behind a virtual NAT network.

---

# SOC Relevance

Understanding this topology matters when investigating network telemetry.

Suppose a SOC alert reports:

```text
Source IP:
192.168.27.128

Destination IP:
8.8.8.8

Destination Port:
443
```

The analyst should understand that `192.168.27.128` is the Windows VM in this lab.

The analyst should also understand that the traffic does not directly travel from the VM to the physical router as if both devices were on the same subnet.

The VM is on the VMware NAT subnet and uses:

```text
192.168.27.2
```

as its default gateway.

This network context helps prevent incorrect conclusions when interpreting network logs.

---

# SOC Investigation Example

Consider a network event:

```text
Source:
192.168.27.128

Destination:
8.8.8.8

Port:
443
```

From the lab topology, we can establish:

```text
192.168.27.128
        ↓
Windows 11 VM
        ↓
VMware NAT
        ↓
Ubuntu Host
        ↓
Home Router
        ↓
External Network
```

However, the network event alone does not tell us:

- Which process generated the connection
- Which user initiated it
- Whether the connection succeeded
- What application protocol was actually used
- Why the connection occurred
- Whether the activity was malicious

Additional telemetry would be required.

This is the same principle used in SOC investigations:

> **Network telemetry provides evidence, not the complete story.**

---

# Security Considerations

The VM uses a private RFC 1918 address space.

The address:

```text
192.168.27.128
```

is not globally routable on the public Internet.

The address itself is therefore not sensitive in the same way as a public IP address, credential, API key, or authentication token.

However, the complete network topology can still reveal information about the lab environment.

For public documentation such as GitHub, private addresses can be replaced with documentation ranges or placeholders.

For example:

```text
Windows VM
10.10.20.10

VMware NAT Gateway
10.10.20.1
```

The private addresses used in this local Obsidian documentation describe my actual lab environment.

---

# Limitations

This diagram is a logical representation of the lab rather than a packet-level representation of every VMware component.

In particular:

- VMnet8 is a VMware virtual network.
- The NAT gateway is provided by VMware.
- The Packet Tracer switch and router are visual representations.
- The Ubuntu host is both part of the physical network and the system hosting VMware.
- The diagram does not represent every internal VMware process or virtual interface.
- Packet Tracer does not actually connect to or control my real VMware network.

The purpose of the diagram is therefore to understand and communicate the network architecture, not to reproduce the VMware implementation exactly.

---

# Lessons Learned

1. A host operating system can participate in the physical network while also providing virtual networking for VMs.
2. A VM connected through VMware NAT can reside on a different subnet from the physical host.
3. The subnet determines whether a destination is considered local or remote.
4. The default gateway is used when the destination is outside the local subnet.
5. VMnet8 is a virtual NAT network created by VMware Workstation.
6. NAT allows the VM to communicate through the host's physical network connection.
7. Packet Tracer can represent the logical architecture even when the real environment contains virtual components that Packet Tracer cannot reproduce directly.
8. Network telemetry must be interpreted using topology and routing context.
9. An IP address and port alone do not provide enough evidence to determine the process, intent, or maliciousness of a connection.

---

# Evidence

The following evidence was collected during Day 11:

- Windows `ipconfig /all` output
- Successful ping to VMware host interface
- Successful ping to VMware NAT gateway
- Successful ping to physical home router
- Successful ping to external IP `8.8.8.8`
- Cisco Packet Tracer network diagram

The screenshots for this evidence are embedded in [Lab Network Architecture](../../labs/week-2-networking/lab-network-architecture.md), which documents the same Day 11 lab from the hands-on evidence side. This note stays the conceptual/architecture reference; that lab doc is the one with the actual command output and diagram images.

---

# Portfolio Status

**YES**

The network diagram is suitable for the SOC portfolio because it demonstrates that I understand the architecture of my lab rather than simply installing a VM.

The final public version should:

- Use a clean diagram
- Explain the physical and virtual layers
- Show the network relationships
- Avoid unnecessary personal or environment-specific information
- Use sanitized IP addresses where appropriate

The diagram can later become part of the `networking/` portfolio section.

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [IP Addressing](ip-addressing.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Network Topologies](network-topologies.md)
- [VMware Setup](../../labs/week-1-lab-setup/vmware-setup.md)
- [Windows 11 Lab](../../labs/week-1-lab-setup/windows-11-lab.md)
- [Day 10 - TryHackMe Networking Labs](../../labs/week-2-networking/day-10-tryhackme-networking-lab.md) 
- [Network Commands & Troubleshooting](network-commands-and-troubleshooting.md)
- [Ports & Protocols](ports-and-protocols.md)
