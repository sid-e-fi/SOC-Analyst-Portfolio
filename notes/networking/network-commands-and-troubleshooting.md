---
type: concept-note
status: reviewed
tags:
  - networking
  - network-troubleshooting
  - commands
  - soc
---

# Network Commands & Troubleshooting

> **Purpose**
>
> This note documents basic commands used to inspect network configuration, test connectivity, investigate DNS resolution, and verify application-layer connectivity. These commands are useful for basic troubleshooting and SOC investigations.

---

# Command Overview

| Command | Platform | Primary purpose |
|---|---|---|
| `ipconfig` | Windows | View IP network configuration |
| `ifconfig` | Linux | View network interface configuration |
| `ping` | Windows/Linux | Test ICMP connectivity |
| `nslookup` | Windows/Linux | Test DNS resolution |
| `resolvectl status` | Linux | Inspect DNS resolver configuration |
| `curl` | Windows/Linux | Test application-layer HTTP/HTTPS connectivity |

---

# `ipconfig`

## Purpose

`ipconfig` displays network configuration information on Windows.

Common information includes:

- IPv4 address
- Subnet mask
- Default gateway
- DNS-related configuration

### Example

```cmd
ipconfig
```

### SOC relevance

Before investigating connectivity problems, an analyst can use `ipconfig` to establish how the Windows endpoint is configured.

The analyst can determine:

```text
IP address
Subnet
Default gateway
DNS configuration
```

This provides context for subsequent connectivity tests.

---

# `ifconfig`

## Purpose

`ifconfig` displays network interface configuration on Linux.

It can show information such as:

- Network interfaces
    
- IPv4 addresses
    
- Netmasks
    
- MAC addresses
    
- Interface state
    
- Packet statistics
    

### Example

```bash
ifconfig
```

### Lab observation

The Ubuntu host used:

```text
wlp0s20f3
```

as its Wi-Fi interface.

The host had:

```text
IPv4: 192.168.1.24
Netmask: 255.255.255.0
```

VMware networking interfaces were also present:

```text
vmnet1
192.168.82.1

vmnet8
192.168.27.1
```

### SOC relevance

`ifconfig` helps establish the local network configuration of a Linux endpoint before investigating connectivity or network-related alerts.

---

# `ping`

## Purpose

`ping` uses ICMP Echo Requests and Echo Replies to test connectivity to a destination.

### Examples

```cmd
ping 8.8.8.8
```

```bash
ping -c 4 8.8.8.8
```

### What it proves

A successful result provides evidence that:

```text
Source
  ↓
Network path
  ↓
Destination
  ↓
ICMP reply
```

was successful.

### What it does NOT prove

Successful ping does not prove:

- DNS is working
    
- TCP connectivity is working
    
- Port 80 or 443 is open
    
- HTTPS is working
    
- The application is healthy
    

Likewise, failed ping does not necessarily mean that the destination is unavailable because ICMP can be filtered.

### Lab observation

Windows successfully reached:

```text
192.168.27.2
8.8.8.8
```

Ubuntu successfully reached:

```text
8.8.8.8
```

An initial Ubuntu test showed packet loss, but subsequent tests succeeded. This demonstrated that a single failed test should not automatically be treated as proof of a persistent connectivity failure.

---

# `nslookup`

## Purpose

`nslookup` is used to query DNS and test hostname resolution.

### Example

```cmd
nslookup google.com
```

### Basic question

> Can this hostname be resolved to DNS information?

Conceptually:

```text
Hostname
   ↓
DNS query
   ↓
DNS response
   ↓
IP address
```

### Lab observation

On Windows:

```cmd
nslookup google.com
```

was affected by the configured:

```text
localdomain
```

DNS suffix.

Using:

```cmd
nslookup google.com.
```

queried the fully qualified name directly.

### SOC relevance

DNS resolution can help an analyst determine:

- Which IP a domain resolves to
    
- Whether DNS resolution is failing
    
- Which resolver is being used
    
- Whether DNS behavior differs between endpoints
    

A DNS result alone does not prove that a domain is malicious.

---

# `resolvectl status`

## Purpose

`resolvectl status` displays DNS resolver configuration and status on systems using `systemd-resolved`.

### Example

```bash
resolvectl status
```

### Lab observation

Ubuntu used:

```text
Local DNS stub:
127.0.0.53
```

The active Wi-Fi interface was:

```text
wlp0s20f3
```

The configured DNS servers included:

```text
192.168.1.1
fe80::1
```

### DNS path

```text
Application
     ↓
127.0.0.53
     ↓
systemd-resolved
     ↓
Configured DNS server
     ↓
DNS resolution
```

### Important distinction

```text
nslookup
    ↓
Tests DNS resolution

resolvectl status
    ↓
Shows DNS resolver configuration/status
```

---

# `curl`

## Purpose

`curl` is a command-line tool for making network requests.

For this lab, it was used to test HTTP/HTTPS connectivity.

### Example

```bash
curl https://example.com
```

### What a successful result indicates

If HTML content is returned, the system successfully:

```text
Resolved destination
        ↓
Established network connection
        ↓
Established HTTPS communication
        ↓
Received application response
```

This provides stronger evidence about web connectivity than `ping` alone.

### Important limitation

A successful `ping` does not guarantee that `curl` will work.

For example:

```text
ping → SUCCESS
curl → FAIL
```

could indicate a problem involving:

- DNS
    
- TCP connectivity
    
- Port 443
    
- Proxy
    
- TLS
    
- Application/service configuration
    
- Security controls
    

### Lab observation

Both Windows and Ubuntu successfully executed:

```text
curl https://example.com
```

and received HTML content.

---

# Layered Troubleshooting

These commands can be combined to isolate problems.

```text
1. Local configuration
       ↓
ipconfig / ifconfig
       ↓
2. IP connectivity
       ↓
ping
       ↓
3. DNS resolution
       ↓
nslookup
       ↓
4. Application connectivity
       ↓
curl
```

Each command provides different evidence.

---

# Example Investigation

Suppose an endpoint reports:

```text
Cannot access example.com
```

An analyst could investigate:

### Step 1: Configuration

```bash
ifconfig
```

Determine whether the interface has appropriate network configuration.

### Step 2: IP connectivity

```bash
ping 8.8.8.8
```

Determine whether external IP connectivity exists.

### Step 3: DNS

```bash
nslookup example.com
```

Determine whether the hostname resolves.

### Step 4: Application

```bash
curl https://example.com
```

Determine whether the HTTPS service responds.

The exact order may vary depending on the available evidence and environment.

---

# SOC Interpretation

The key principle is:

> **Do not confuse successful connectivity at one layer with successful connectivity at every layer.**

Examples:

```text
ping succeeds
      ≠
website works
```

```text
DNS succeeds
      ≠
TCP connection succeeds
```

```text
TCP connection succeeds
      ≠
TLS succeeds
```

```text
TLS succeeds
      ≠
application works correctly
```

Each layer requires its own evidence.

---

# Command-to-Evidence Reference

| Command | Evidence obtained | Does not prove |
|---|---|---|
| `ipconfig` | Windows network configuration | Internet/service availability |
| `ifconfig` | Linux interface configuration | End-to-end connectivity |
| `ping` | ICMP reachability | Application availability |
| `nslookup` | DNS resolution | Service availability |
| `resolvectl status` | DNS resolver configuration | Successful application connectivity |
| `curl` | HTTP/HTTPS application response | Overall network health |

---

# Day 13 Lab Evidence

The commands were tested against the actual lab environment.

### Windows 11 VM

```text
ipconfig
ping gateway
ping 8.8.8.8
nslookup
ping google.com
curl https://example.com
```

### Ubuntu Linux Host

```text
ifconfig
ping 8.8.8.8
nslookup google.com
resolvectl status
curl https://example.com
```

---

# Lessons Learned

- Network troubleshooting should be evidence-driven.
    
- `ipconfig` and `ifconfig` establish local configuration.
    
- `ping` tests ICMP connectivity.
    
- `nslookup` tests DNS resolution.
    
- `resolvectl status` reveals Linux DNS resolver configuration.
    
- `curl` tests application-layer HTTP/HTTPS communication.
    
- A single failed ping is not enough to declare a network outage.
    
- Different systems can use different DNS resolvers and receive different valid DNS answers.
    
- DNS anomalies require investigation rather than immediate assumptions of compromise.
    
- Successful ICMP connectivity does not prove that an application is working.
    

---

# Related Notes

- [DHCP & DNS](dhcp-and-dns.md)
    
- [IP Addressing](ip-addressing.md)
    
- [Ports & Protocols](ports-and-protocols.md)
    
- [Protocol Comparison](protocol-comparison.md)
    
- [Network Observation Lab](../../labs/week-2-networking/network-observation-lab.md)
    
- [Home Lab Network Diagram](home-lab-network-diagram.md) 
- [Networking Fundamentals](networking-fundamentals.md)
