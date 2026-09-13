---
type: lab-note
status: active
day: 10
topic: TryHackMe Networking Labs
tags:
  - networking
  - tryhackme
  - soc
  - tcp
  - icmp
  - ports
  - packets
---

# Day 10 - TryHackMe Networking Labs

# Purpose

Apply the networking concepts covered on Days 8 and 9 through practical exercises in TryHackMe.

The focus was to observe basic network communication rather than only memorise definitions. The work covered network fundamentals, packets and frames, ICMP ping, ports, TCP communication, and basic network path behaviour.

From a SOC perspective, the objective was to become more comfortable interpreting network activity in terms of endpoints, protocols, ports, and communication behaviour.

# Environment

| Item | Details |
|---|---|
| Platform | TryHackMe |
| Path | Pre Security |
| Module | Network Fundamentals |
| Path Progress at completion | 55% |
| Terminal | TryHackMe browser-embedded terminal |
| Personal VM used | No |
| AttackBox used | No |

The exercises were performed directly inside the TryHackMe room using its embedded practical environments and terminal.

# Module Completed

The **Network Fundamentals** module was completed with all six items checked:

1. What Is Networking?
2. Intro to LAN
3. OSI Model
4. Packets & Frames
5. Extending Your Network
6. Topic Transition Recap

The Topic Transition Recap was completed with **48 points** earned.

![Network Fundamentals completed](day-10-thm-network-fundamentals-complete.png)

# Practical Exercises

## 1. ICMP Ping

### Objective

Use `ping` to test connectivity to a network endpoint and observe ICMP communication.

### Command Used

```bash
ping -c 4 8.8.8.8
```

### Result

The command successfully sent four ICMP requests and received four replies.

Observed result:

- 4 packets transmitted
- 4 packets received
- 0% packet loss
- RTT average approximately 9.428 ms

The exercise also returned:

```text
THM{I_PINGED_THE_SERVER}
```

### Observation

`ping` uses ICMP rather than TCP or UDP. The successful replies demonstrated that the endpoint was reachable and responding to ICMP echo requests at the time of the test.

![ICMP ping practical](day-10-thm-networking-icmp-ping.png)

### SOC Relevance

An analyst may encounter ICMP traffic during network investigations. A successful ping can indicate connectivity or reachability, but ICMP activity alone does not establish malicious behaviour.

Additional context would be required, such as the source endpoint, destination, frequency, surrounding events, and whether the activity is expected.

## 2. Port Connection Using Netcat

### Objective

Use a TCP connection to interact with a specific destination port and reinforce the distinction between an IP address and a port.

### Command Used

```bash
nc 8.8.8.8 1234
```

### Result

The connection succeeded and returned:

```text
THM{YOU_CONNECTED_TO_A_PORT}
```

The room also showed examples of common protocol and port associations:

| Protocol | Port |
|---|---:|
| HTTP | 80 |
| HTTPS | 443 |
| SMB | 445 |
| RDP | 3389 |

### Observation

The IP address identifies the network endpoint, while the port identifies the service endpoint being contacted on that host.

The practical exercise demonstrated that a connection can be directed to a specific port rather than simply to an IP address.

![Packets and Frames netcat practical](day-10-thm-packets-and-frames-netcat-port-connection.png)

### SOC Relevance

Ports are frequently visible in network telemetry and security alerts.

For example:

```text
Source IP -> Destination IP:Destination Port
```

An analyst can use the destination port as investigation context, but the port number alone is not enough to determine whether activity is malicious.

A service may also operate on a non-standard port, so analysts should not assume that a port number always proves which application is running.

## 3. TCP Network Simulator

### Objective

Observe how a TCP packet travels through a simple network and understand the TCP three-way handshake in a visual simulator.

### Network Topology

```text
computer1
    |
  switch1
    |
  router
    |
  switch2
    |
computer3
```

`computer2` was also connected to `switch1`.

### Practical Action

A TCP packet was sent from:

```text
computer1 -> computer3
```

### Observed Handshake

The Network Log showed the TCP three-way handshake completing successfully:

```text
SYN
SYN/ACK
ACK
```

The exercise recorded **5 HANDSHAKE entries** in the Network Log.

The practical flag was:

```text
THM{YOU'VE_GOT_DATA}
```

![TCP network simulator](day-10-thm-extending-network-network-simulator.png)

### SOC Relevance

TCP connection establishment is important when interpreting network telemetry.

A successful TCP connection normally involves:

1. Client sends SYN
2. Server responds with SYN/ACK
3. Client sends ACK

Seeing these events helps an analyst understand whether a TCP connection attempt progressed into an established connection.

This is useful when investigating connection attempts, scanning behaviour, blocked connections, and unusual outbound or inbound communication.

# Topic Transition Recap

The Network Fundamentals Topic Transition Recap was completed successfully.

```text
Total points: 48
```

![Topic Transition Recap](day-10-thm-networking-transition-recap.png)

# Key Technical Observations

## IP Address

An IP address identifies a network endpoint involved in communication.

Example:

```text
8.8.8.8
```

## Port

A port identifies a service endpoint on a host.

Example:

```text
8.8.8.8:1234
```

The IP and port together provide more specific information about the destination of a connection.

## Protocol

The protocol describes the communication mechanism being used.

Examples encountered during the labs:

- ICMP
- TCP
- HTTP
- HTTPS
- SMB
- RDP

## TCP

TCP is connection-oriented and uses a handshake to establish communication.

The simulator demonstrated:

```text
SYN -> SYN/ACK -> ACK
```

## ICMP

ICMP was observed through the `ping` exercise.

The successful ping demonstrated reachability and response from the destination.

## Packets

Network communication is divided into smaller units for transmission across a network.

The TryHackMe network simulator made the movement of a TCP packet through network devices visible.

# SOC Perspective

The most important lesson from the practical work is that a network event should not be judged in isolation.

A SOC analyst may see something similar to:

```text
Internal Host
    |
    | TCP
    v
External IP:443
```

That tells the analyst useful facts:

- which endpoint communicated
- which destination was contacted
- which transport protocol was involved
- which destination port was used

It does **not** automatically tell the analyst:

- whether the destination is malicious
- whether the user intentionally initiated the connection
- whether malware caused the connection
- whether data was exfiltrated
- whether the activity violates organisational policy

The correct investigation mindset is:

```text
Event
  ↓
Context
  ↓
Additional Evidence
  ↓
Decision
```

rather than:

```text
Event
  ↓
Looks suspicious
  ↓
Malicious
```

# Evidence Collected

| Evidence | Result |
|---|---|
| Network Fundamentals module | Completed |
| What Is Networking? | Completed |
| Intro to LAN | Completed |
| OSI Model | Completed |
| Packets & Frames | Completed |
| Extending Your Network | Completed |
| Topic Transition Recap | Completed |
| ICMP ping | Successful |
| Ping packet loss | 0% |
| Netcat connection | Successful |
| TCP simulator | Successful |
| TCP handshake | SYN, SYN/ACK, ACK observed |
| Recap points | 48 |

# Screenshots

The screenshots were renamed specifically for Day 10 documentation.

## Screenshot 1

`day-10-thm-extending-network-network-simulator.png`

Shows the Extending Your Network practical network simulator, including the network topology, TCP packet transmission, completed handshake, and successful task results.

## Screenshot 2

`day-10-thm-network-fundamentals-complete.png`

Shows the completed Network Fundamentals module and its six completed items.

## Screenshot 3

`day-10-thm-networking-icmp-ping.png`

Shows the ICMP ping practical using `ping -c 4 8.8.8.8`, including successful replies and 0% packet loss.

## Screenshot 4

`day-10-thm-networking-transition-recap.png`

Shows successful completion of the Topic Transition Recap with 48 points.

## Screenshot 5

`day-10-thm-packets-and-frames-netcat-port-connection.png`

Shows the netcat practical connecting to `8.8.8.8` on port `1234` and receiving the expected flag.

# Technical Problems

No technical problems were encountered during the practical work.

# Lessons Learned

1. An IP address identifies the network endpoint, while a port identifies a service endpoint on that host.
2. ICMP is used by `ping` to test reachability and obtain response timing.
3. TCP uses a three-way handshake consisting of SYN, SYN/ACK, and ACK.
4. Network communication can be observed as traffic moving through switches and routers.
5. Protocols and ports provide useful context during network investigations.
6. A network connection by itself is not sufficient evidence to classify activity as malicious.
7. SOC analysis requires context and additional evidence before making a decision.

# Future Usage

The concepts from this lab will be used later when working with:

- network logs
- SIEM alerts
- firewall events
- endpoint network connections
- DNS telemetry
- HTTP/HTTPS traffic
- incident investigations

# Completion Status

**Day 10 networking lab work: COMPLETED**

The required TryHackMe networking material was completed, practical observations were recorded, and five meaningful screenshots were captured and renamed for documentation.
