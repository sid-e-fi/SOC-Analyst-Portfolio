---
type: weekly-summary
status: reviewed
tags:
  - week-2
  - networking
  - subnetting
  - protocols
  - packet-tracer
  - soc
---

# Week 2 Summary: Networking Fundamentals

> [!NOTE]
> **Purpose:** This note consolidates Week 2 (Days 8 to 13) of the SOC Analyst L1 roadmap: core networking fundamentals, transport and application layer protocols, network architecture and topologies, a hands-on mapping of the actual home lab network, common SOC-relevant ports, and layered command-line troubleshooting on both Windows and Ubuntu. Raw daily logs remain in `11 Daily Logs/Week 2` for process history; this note is the reviewed, portfolio-facing record of what was actually learned and built. Day 7 and Day 14 are expected to be the Week 1 and Week 2 review/publishing days, mirroring that pattern from Week 1, and fall outside this note's six core learning days.

**Dates covered:** Approximately 19 to 26 August 2026 (estimated from cadence; only Day 8 carries an explicit date, 19 August 2026, in its frontmatter)
**Time invested:** Roughly 2 hours of theory, hands-on labs, and documentation per day, across 6 days

---

## What I Built

- Six permanent networking knowledge notes created under `02 Networking/` on Day 8: [Networking Fundamentals](../notes/networking/networking-fundamentals.md), [IP Addressing](../notes/networking/ip-addressing.md), [Subnetting & CIDR](../notes/networking/subnetting-and-cidr.md), [Routing & Network Segmentation](../notes/networking/routing-and-network-segmentation.md), [DHCP & DNS](../notes/networking/dhcp-and-dns.md), [Ports & Protocols](../notes/networking/ports-and-protocols.md), cross-linked with Wikilinks instead of duplicated content
- A [Protocol Comparison](../notes/networking/protocol-comparison.md) note (Day 9) placing TCP, UDP, HTTP, HTTPS, DNS, and DHCP side by side
- Knowledge notes drawn from the required TryHackMe Pre Security modules (Day 10): [LAN](../notes/networking/lan.md), [Network Topologies](../notes/networking/network-topologies.md), [OSI Model](../notes/networking/osi-model.md), [Packets & Frames](../notes/networking/packets-and-frames.md), [Extending Your Network](../notes/networking/extending-your-network.md)
- A [Home Lab Network Diagram](../notes/networking/home-lab-network-diagram.md) built in Cisco Packet Tracer (Day 11), mapping the actual lab: home router to Ubuntu host to VMware VMnet8 NAT network to Windows 11 VM, with the physical and virtual layers visually separated. Flagged portfolio-ready in the Day 11 log since it documents a real environment rather than a generic textbook topology
- A [Network Commands & Troubleshooting](../notes/networking/network-commands-and-troubleshooting.md) note, plus an updated [Network Observation Lab](../labs/week-2-networking/network-observation-lab.md) note, from Day 13's command-line work, capturing what each diagnostic command actually proves

See [Day 10 - TryHackMe Networking Labs](../labs/week-2-networking/day-10-tryhackme-networking-lab.md), [Lab Network Architecture](../labs/week-2-networking/lab-network-architecture.md), and [Network Observation Lab](../labs/week-2-networking/network-observation-lab.md) for the full lab documentation.

---

## Labs and Practical Work

Week 2 carried noticeably more hands-on work than Week 1's mostly conceptual pace.

**Day 9:** Ran `nslookup example.com` and `curl -I https://example.com` / `curl -I http://example.com`, observing a live DNS resolution and an HTTP 200 OK response, then tied the output back to the request lifecycle (DNS to IP to transport to TLS to HTTP).

**Day 10:** Completed the required TryHackMe Pre Security networking modules (What Is Networking?, Intro to LAN, OSI Model, Packets & Frames, Extending Your Network) plus the Topic Transition Recap, scoring 48 points. Hands-on exercises covered ICMP ping, the TCP three-way handshake, and port connectivity. Five screenshots were retained as completion evidence.

**Day 11:** Ran `ipconfig /all` on the Windows 11 VM and pinged four destinations from it: the VMware host-side interface (192.168.27.1), the VMware NAT gateway (192.168.27.2), the physical home router (192.168.1.1), and an external IP (8.8.8.8). All four succeeded, which is what let the Packet Tracer diagram be grounded in real, observed addressing rather than assumptions.

**Day 13:** A full layered troubleshooting pass on both operating systems. Windows: `ipconfig`, `ping` to the gateway, `8.8.8.8`, and `google.com`, `nslookup google.com` and `nslookup google.com.`, `curl https://example.com`. Ubuntu: `ifconfig`, `ping 8.8.8.8`, `resolvectl status`, `curl https://example.com`. Along the way, diagnosed a transient Ubuntu packet loss result (100 percent loss on the first run, 20 percent on the second, 0 percent on a third) without prematurely calling it a real outage, and worked out why Windows returned `google.com.localdomain` until a trailing dot forced the fully qualified domain name.

---

## What I Learned

### Core Networking Concepts (Day 8)
Built the foundational model: what a network actually is, IP addressing (private versus public, NAT, IP versus MAC, why an IP is not a permanent device identity), subnetting and CIDR (network versus host portion, why classful A/B/C addressing is now mostly historical), routing and gateways (local versus remote destinations, segmentation, lateral movement), DHCP (the DORA process and its value for attribution), DNS (record types, caching and TTL, and its value plus limits as SOC telemetry), and ports (ephemeral ports, and why a port number alone never proves the application protocol in use). The day's takeaway chained into one mental model: DHCP to IP configuration to subnet to routing/gateway to DNS to destination IP to port to transport protocol to application protocol to network activity to SOC investigation.

See [Networking Fundamentals](../notes/networking/networking-fundamentals.md), [IP Addressing](../notes/networking/ip-addressing.md), [Subnetting & CIDR](../notes/networking/subnetting-and-cidr.md), [Routing & Network Segmentation](../notes/networking/routing-and-network-segmentation.md), [DHCP & DNS](../notes/networking/dhcp-and-dns.md), [Ports & Protocols](../notes/networking/ports-and-protocols.md).

### Transport and Application Layer (Day 9)
Replaced the oversimplified beginner model of TCP equals secure, UDP equals fast with one based on actual transport characteristics: reliability, ordering, retransmission, connection state, and overhead. Learned HTTP as an application layer protocol and HTTPS as HTTP wrapped in TLS, and that HTTPS alone says nothing about whether a site is trustworthy. Walked the simplified lifecycle of opening a website: DNS resolution, destination IP, transport connection, TLS establishment, HTTP request, HTTP response, browser rendering.

See [Protocol Comparison](../notes/networking/protocol-comparison.md).

### Network Architecture and Topologies (Day 10)
Covered network topologies (bus, star, ring, mesh), with a specific correction to the assumption that star is simply better than bus: star improves fault isolation and manageability, but the central switch remains a shared, finite dependency of its own. Studied the OSI model's seven layers and mapped them to concrete objects (Layer 4 to TCP/UDP/ports, Layer 3 to IP/routing, Layer 2 to Ethernet/MAC, Layer 1 to physical transmission), the encapsulation chain from application data down to bits, common networking devices (repeater, hub, switch, router, access point), and the distinction between a VLAN (logical Layer 2 segmentation) and a subnet (a Layer 3 address range).

See [LAN](../notes/networking/lan.md), [Network Topologies](../notes/networking/network-topologies.md), [OSI Model](../notes/networking/osi-model.md), [Packets & Frames](../notes/networking/packets-and-frames.md), [Extending Your Network](../notes/networking/extending-your-network.md).

### Mapping the Real Lab Network (Day 11)
Took the theory from Days 8 to 10 and applied it to the actual SOC lab: home router (192.168.1.1) to Ubuntu host (192.168.1.24) to the VMware VMnet8 NAT network (192.168.27.0/24) to the Windows 11 VM (192.168.27.128). The key realization was that a single physical machine participates in multiple networks at once (the Ubuntu host sits on the physical LAN and also provides the host-side interface for the virtual NAT network), and that only cross-subnet traffic needs to go through the default gateway. That distinction, whether an address is local or needs routing, is what made subnetting click after Day 8's more abstract treatment of it.

See [Home Lab Network Diagram](../notes/networking/home-lab-network-diagram.md) and [Lab Network Architecture](../labs/week-2-networking/lab-network-architecture.md).

### Ports and Protocols as SOC Evidence (Day 12)
Went deep on nine SOC-relevant ports (22, 53, 80, 443, 3389, 445, 25, 110, 143) and what each is commonly, but not definitively, associated with. Reinforced the week's central theme in its clearest form yet: a port or protocol is context, not a verdict. Traffic direction changes the investigative read entirely (inbound RDP from the internet to an internal host is a very different signal than routine internal RDP use), and even a normal-looking DNS query on port 53 can warrant follow-up on the requested domain, frequency, and source process.

See [Ports & Protocols](../notes/networking/ports-and-protocols.md).

### Command-Line Troubleshooting (Day 13)
Learned to treat troubleshooting as a layered process: local configuration, then IP connectivity, then DNS resolution, then TCP/service connectivity, then TLS, then the application itself, with a different command mapped to each layer. `ipconfig`/`ifconfig` answer how the system is configured. `ping` proves reachability, not that a service works. `nslookup` proves DNS resolution, not that the resolved service is reachable. `resolvectl status` exposed Ubuntu's actual resolver architecture in a way `nslookup` alone could not. `curl` was the only command that actually proved an application layer response.

See [Network Commands & Troubleshooting](../notes/networking/network-commands-and-troubleshooting.md).

---

## Key Realizations

The single idea that showed up in some form on every day of Week 2: a network event, port, protocol, or diagnostic result is evidence, not a verdict. Day 8 phrased it as "do not claim more than the available evidence proves," Day 9 as "a port or protocol provides context, not a verdict," Day 12 almost word for word as "a network event is evidence, not a verdict," and Day 13's whole lab was built around collecting evidence layer by layer before concluding anything.

A secondary pattern worth naming: several oversimplified beginner mental models got refined rather than thrown out this week, TCP equals secure/UDP equals fast on Day 9, star topology is simply better on Day 10, and a known service IP must run other services too, also on Day 10. In each case the fix was the same move: break the claim into its actual separate parts (transport reliability versus encryption, management versus bottleneck, address versus port versus service) instead of trusting the shortcut.

Day 11 also marked the first time this roadmap's concepts got grounded in the person's own real environment rather than a textbook example, which is likely why it is the first Week 2 day flagged as portfolio-ready outright.

---

## Challenges Worked Through

| Confused | Resolved as |
|---|---|
| Subnetting math: IP plus CIDR plus network/host portion | Reframed around the practical question of whether the destination is inside the same subnet or needs to be routed |
| Classful (A/B/C) versus classless addressing | Classful addressing is now mostly historical; modern networking defines boundaries explicitly with CIDR prefixes |
| TCP equals secure, UDP equals fast | Replaced with a model built on reliability, ordering, retransmission, and overhead; TLS, not the transport protocol, provides encryption |
| Ports having one typical transport protocol | Ports identify service endpoints; TCP and UDP themselves have no single default port |
| Star topology is simply better than bus | Star improves fault isolation and management, but the central switch is still a shared, finite dependency |
| A known service IP (8.8.8.8) implies other services on other ports | IP address, port, and service are three separate things; the address alone doesn't say what's listening where |
| One failed ping equals a real network outage | Repeated the test; transient packet loss has to be verified before being called a persistent failure |
| Windows `nslookup` returning `google.com.localdomain` | Caused by the local DNS suffix; a trailing dot forces the fully qualified domain name and resolves correctly |

---

## Credentials and Milestones

No external certificate this week. Completed the required TryHackMe Pre Security networking module set (What Is Networking?, Intro to LAN, OSI Model, Packets & Frames, Extending Your Network) and the Topic Transition Recap, scoring 48 points.

---

## Documentation Produced This Week

- [Networking Fundamentals](../notes/networking/networking-fundamentals.md)
- [IP Addressing](../notes/networking/ip-addressing.md)
- [Subnetting & CIDR](../notes/networking/subnetting-and-cidr.md)
- [Routing & Network Segmentation](../notes/networking/routing-and-network-segmentation.md)
- [DHCP & DNS](../notes/networking/dhcp-and-dns.md)
- [Ports & Protocols](../notes/networking/ports-and-protocols.md)
- [Protocol Comparison](../notes/networking/protocol-comparison.md)
- [LAN](../notes/networking/lan.md)
- [Network Topologies](../notes/networking/network-topologies.md)
- [OSI Model](../notes/networking/osi-model.md)
- [Packets & Frames](../notes/networking/packets-and-frames.md)
- [Extending Your Network](../notes/networking/extending-your-network.md)
- [Network Commands & Troubleshooting](../notes/networking/network-commands-and-troubleshooting.md)
- [Home Lab Network Diagram](../notes/networking/home-lab-network-diagram.md)
- [Day 10 - TryHackMe Networking Labs](../labs/week-2-networking/day-10-tryhackme-networking-lab.md)
- [Lab Network Architecture](../labs/week-2-networking/lab-network-architecture.md)
- [Network Observation Lab](../labs/week-2-networking/network-observation-lab.md)

---

## Readiness for Week 3

Able to explain, without notes, the difference between local and remote traffic, why a port or protocol is context rather than proof, and what each core diagnostic command (ping, nslookup, curl, resolvectl status) actually establishes versus merely suggests. The [Home Lab Network Diagram](../notes/networking/home-lab-network-diagram.md) is portfolio-ready as is. The main open area, repeated across Days 11 through 13, is retention rather than understanding: subnetting, gateways, and the layered troubleshooting model are solid while actively in use but will need periodic revision. Day 12's plan already points past Week 2, toward listening ports, active connections, and service identification, and eventually correlating network telemetry with endpoint and authentication evidence once Windows/Linux logs and SIEM tooling enter the roadmap.
