---
type: concept-note
status: reviewed
tags:
  - networking
  - dhcp
  - dns
  - network-services
  - soc
---

# DHCP & DNS

> **Purpose**
>
> DHCP and DNS are two fundamental network services that allow devices to obtain network configuration and resolve names into network information. Understanding both is important for troubleshooting and for SOC investigations involving host attribution and DNS activity.

---

# DHCP

**DHCP** stands for **Dynamic Host Configuration Protocol**.

It allows network clients to obtain network configuration dynamically instead of requiring every device to be configured manually.

A DHCP server can provide information such as:

```text
IP Address
Subnet Mask / Prefix
Default Gateway
DNS Server
Lease Information
```

For example:

```text
IP:
192.168.1.25

Subnet:
192.168.1.0/24

Gateway:
192.168.1.1

DNS:
192.168.1.1
```

DHCP therefore provides the configuration a device needs to participate in the network.

### Typical Ports

DHCP communicates over UDP:

```text
UDP 67 → DHCP server
UDP 68 → DHCP client
```

---

# DHCP Lease

DHCP commonly assigns addresses using **leases**.

A lease gives a client permission to use an IP address for a specified period.

This allows addresses to be reused when devices leave the network rather than permanently assigning every address to a device.

Conceptually:

```text
DHCP Server
     ↓
Assign IP
     ↓
Lease active
     ↓
Lease expires / renews
     ↓
Address can be reused
```

The DHCP server maintains information about these assignments.

This makes DHCP records useful for **IP-to-device attribution** during security investigations.

---

# DORA

The basic DHCP address allocation process is commonly represented as **DORA**:

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
Acknowledgment
```

### Discover

The client searches for available DHCP servers.

```text
Client
   ↓
"Is there a DHCP server?"
```

### Offer

A DHCP server offers configuration to the client.

```text
DHCP Server
   ↓
"You can use 192.168.1.25"
```

### Request

The client requests the offered configuration.

```text
Client
   ↓
"I want this configuration."
```

### Acknowledgment

The DHCP server confirms the assignment.

```text
DHCP Server
   ↓
"Configuration confirmed."
```

After this process, the client can configure its network interface using the supplied information.

---

# DHCP Reservations and Static Addresses

Not every device necessarily receives a completely unpredictable address.

A network can also use:

### Static IP

An administrator manually configures the device with an IP address.

### DHCP Reservation

The DHCP server is configured to consistently provide a particular address to a particular device.

Therefore:

> **DHCP does not mean that every device always receives a different IP address.**

The important concept is that DHCP provides network configuration dynamically through a centralized service.

---

# DHCP in SOC Investigations

Suppose a SOC alert contains:

```text
Source IP:
192.168.1.25

Time:
14:32:11
```

An analyst may need to determine:

> **Which device was using `192.168.1.25` at 14:32:11?**

DHCP records may help:

```text
SIEM Alert
    ↓
192.168.1.25
    ↓
Timestamp
    ↓
DHCP Lease
    ↓
Device / MAC / Host
```

This can help establish which host was associated with an IP address at a particular point in time.

DHCP data can therefore become an important source of evidence for **host attribution**.

---

# DNS

**DNS** stands for **Domain Name System**.

DNS allows systems to use names instead of requiring users and applications to know IP addresses directly.

For example:

```text
example.com
     ↓
DNS
     ↓
IP address
```

Humans generally prefer names such as:

```text
example.com
```

while network communication ultimately requires addressing information such as:

```text
93.x.x.x
```

DNS provides the system used to resolve these names.

---

# DNS Resolver

A client commonly sends DNS queries to a **DNS resolver**.

For example:

```text
Laptop
    │
    │ "What is example.com?"
    ↓
DNS Resolver
    │
    │ obtains the answer
    ↓
Laptop
    │
    │ uses returned IP information
    ↓
Destination
```

The resolver may obtain the required information from DNS infrastructure or from its cache.

A home router may act as the DNS resolver or forward DNS queries to another resolver.

---

# DNS Records

DNS does more than simply map names to IPv4 addresses.

Some common record types include:

| Record | Common purpose |
|---|---|
| A | Domain to IPv4 address |
| AAAA | Domain to IPv6 address |
| CNAME | Alias for another domain |
| MX | Mail server information |
| TXT | Text and verification information |
| NS | Name server information |

The most important distinction is:

```text
A
 ↓
IPv4

AAAA
 ↓
IPv6
```

---

# DNS Caching

DNS responses can be cached so that repeated queries do not always require a new lookup.

Conceptually:

```text
First query
    ↓
DNS Resolver
    ↓
Answer
    ↓
Cache
    ↓
Future query
    ↓
Cached answer
```

DNS records have a **TTL**, or time-to-live, which determines how long a response can generally remain cached.

Caching reduces DNS traffic and can improve response time.

---

# DNS and Network Communication

When a user enters:

```text
https://example.com
```

a simplified sequence is:

```text
Client
   ↓
DNS Query
   ↓
DNS Resolver
   ↓
DNS Response
   ↓
Destination IP
   ↓
Network Connection
```

DNS therefore helps bridge the gap between a human-readable domain name and the network destination that the client needs to communicate with.

---

# DNS as SOC Evidence

DNS is particularly valuable to SOC analysts because DNS activity can provide visibility into which domains hosts are attempting to resolve.

For example:

```text
Host:
WS-023

DNS Query:
updates-example.com

DNS Response:
203.0.113.50
```

Later, network telemetry may show:

```text
Source:
WS-023

Destination:
203.0.113.50

Port:
443
```

The analyst can correlate the events:

```text
Host
 ↓
DNS Query
 ↓
Domain
 ↓
Resolved IP
 ↓
Subsequent Network Connection
```

This creates a useful investigative relationship between the domain and subsequent network activity.

---

# DNS Does Not Prove Compromise

A suspicious-looking DNS query does **not** automatically prove that a host is compromised.

For example:

```text
WS-023
   ↓
suspicious-domain.example
```

could represent:

- Legitimate software activity
- A website intentionally accessed by the user
- A legitimate service that appears unfamiliar
- A misclassified or newly registered domain
- Malicious activity

Additional evidence is required.

A SOC analyst should investigate:

- What is the domain used for?
- Who operates it?
- Is it expected in the organization?
- Which host generated the query?
- Which user was involved?
- Which process generated the query?
- Did the host connect to the resolved IP?
- Are other hosts making the same query?
- What happened before and after the DNS event?

---

# DNS Visibility

Traditional DNS can commonly be observed through network telemetry.

However, DNS is not always transmitted in an easily inspectable form.

Modern environments can use encrypted DNS mechanisms such as:

- DNS over HTTPS (DoH)
- DNS over TLS (DoT)

Therefore:

> **DNS remains valuable telemetry, but visibility depends on how DNS is transported and where monitoring is performed.**

A SOC analyst should not assume that every DNS query will be visible in plaintext.

---

# DHCP vs DNS

These services solve different problems.

| DHCP | DNS |
|---|---|
| Provides network configuration | Resolves names |
| Assigns/leases IP addresses | Provides DNS records |
| Provides gateway information | Helps locate services |
| Can help attribute IPs to devices | Can reveal host/domain activity |
| Used when configuring network access | Used during name resolution |

A useful mental model is:

```text
DHCP
 ↓
"How should this device configure its network?"

DNS
 ↓
"What network information corresponds to this name?"
```

---

# SOC Investigation Example

Consider:

```text
14:32:11

Host:
WS-023

DNS Query:
updates-example.com

DNS Response:
203.0.113.50
```

Five minutes later:

```text
14:37:14

Host:
WS-023

Destination:
203.0.113.50

Destination Port:
443

Protocol:
TCP
```

The analyst can establish a relationship between:

```text
WS-023
 ↓
DNS query
 ↓
updates-example.com
 ↓
203.0.113.50
 ↓
Later TCP traffic
```

However, the evidence alone does not prove:

- HTTPS was definitely used
- The connection was successful
- The domain was malicious
- WS-023 was compromised

Those conclusions require additional telemetry and investigation.

---

# SOC Perspective

DHCP and DNS can answer different investigative questions.

### DHCP helps answer:

> **Which device was using this IP at this time?**

### DNS helps answer:

> **Which domains was this host attempting to resolve?**

Together they can become useful for attribution and investigation:

```text
Network Alert
     ↓
Source IP
     ↓
DHCP
     ↓
Host
     ↓
DNS Activity
     ↓
Domain
     ↓
Resolved IP
     ↓
Network Connection
     ↓
Endpoint / Process Evidence
```

This is an example of how SOC analysts build conclusions by correlating multiple sources of telemetry rather than relying on a single event.

---

# Interview Notes

### What is DHCP?

DHCP is a protocol that dynamically provides network configuration such as IP addresses, subnet information, gateways, and DNS servers to clients.

### What is DORA?

DORA stands for Discover, Offer, Request, and Acknowledgment, representing the basic DHCP address allocation process.

### What is DNS?

DNS is the Domain Name System, which provides name-resolution and other DNS information used by networked systems.

### Why is DHCP useful to a SOC analyst?

DHCP records can help determine which device was using an IP address at a particular time.

### Why is DNS useful to a SOC analyst?

DNS telemetry can reveal which domains hosts are attempting to resolve and can be correlated with subsequent network connections.

### Does a suspicious DNS query prove malware?

No. It is evidence that requires additional context and correlation.

---

# Key Takeaways

- **DHCP provides network configuration to clients.**
- **DORA represents Discover, Offer, Request, and Acknowledgment.**
- **DHCP uses leases to manage address assignments.**
- **DHCP records can help attribute an IP address to a device at a specific time.**
- **DNS resolves names and provides other DNS information.**
- **A and AAAA records commonly provide IPv4 and IPv6 address information respectively.**
- **DNS responses can be cached according to their TTL.**
- **DNS activity can be valuable SOC telemetry.**
- **A DNS query does not prove malicious activity.**
- **DNS visibility depends on how DNS traffic is transported and where it is monitored.**
- **DHCP and DNS provide different services but can be correlated during investigations.**

---

# Related Notes

- [Networking Fundamentals](networking-fundamentals.md)
- [IP Addressing](ip-addressing.md)
- [Subnetting & CIDR](subnetting-and-cidr.md)
- [Routing & Network Segmentation](routing-and-network-segmentation.md)
- [Ports & Protocols](ports-and-protocols.md)
- [Network Security](../security-fundamentals/network-security.md)
- [Incident Response](../security-fundamentals/incident-response.md)
- [Extending Your Network](extending-your-network.md)
- [LAN](lan.md)
- [Network Commands & Troubleshooting](network-commands-and-troubleshooting.md)
- [OSI Model](osi-model.md)
- [Packets & Frames](packets-and-frames.md)
