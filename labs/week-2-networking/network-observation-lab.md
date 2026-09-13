---
type: lab-note
status: active
topic: Network Observation Lab
tags:
  - networking
  - dns
  - http
  - https
  - troubleshooting
  - vmware
  - soc
---

# Network Observation Lab

# Part A — DNS, HTTP & HTTPS Observation (Windows)

## Purpose

Observe basic DNS, HTTP, and HTTPS behavior from a Windows endpoint using command-line tools.

The purpose of this exercise was to connect the networking concepts learned during Day 9 with observable behavior on a real system.

---

## Environment

| Component | Details |
| ---------------- | ---------------------------------- |
| Operating System | Windows |
| Tools | Command Prompt, `nslookup`, `curl` |
| DNS target | `example.com` |
| Network | Local network connection |

> **Security note:** Private network information observed during the exercise is intentionally not documented here.

---

## 1. DNS Resolution

### Command

```text
nslookup example.com
```

### Observation

The command successfully queried the configured DNS resolver and returned multiple addresses for `example.com`.

The response included both:

- IPv4 addresses
    
- IPv6 addresses
    

### What this demonstrates

DNS allows a host to resolve a human-readable domain name into network addresses.

```text
example.com
      ↓
DNS resolver
      ↓
IPv4 / IPv6 addresses
```

A DNS query does not necessarily return only one address.

### SOC relevance

DNS activity can provide useful investigation evidence.

An analyst may want to determine:

- Which host made the DNS query
    
- Which user was active
    
- Which process generated the activity
    
- What domain was requested
    
- What addresses were returned
    
- Whether the domain was expected
    
- Whether a network connection followed the DNS query
    

---

## 2. HTTPS Observation

### Command

```text
curl -I https://example.com
```

### Observation

The command successfully contacted the HTTPS endpoint and returned HTTP response headers.

The response included:

```text
HTTP/1.1 200 OK
```

Additional headers were also returned.

### What this demonstrates

HTTPS can carry HTTP requests and responses through a TLS-protected communication channel.

Simplified flow:

```text
curl
  ↓
HTTPS
  ↓
TLS
  ↓
Network
  ↓
Web server
  ↓
HTTP response
  ↓
curl
```

The `200 OK` status indicates that the server processed the request successfully and returned an OK response.

---

## 3. HTTP Observation

### Command

```text
curl -I http://example.com
```

### Observation

The HTTP endpoint also returned an HTTP response during the test.

### What this demonstrates

HTTP and HTTPS are distinct.

HTTP:

```text
HTTP
 ↓
No TLS protection
```

HTTPS:

```text
HTTP
 ↓
TLS protection
```

An HTTP endpoint does not necessarily have to fail simply because an HTTPS endpoint also exists.

Server configuration determines how HTTP requests are handled. Some servers redirect HTTP requests to HTTPS, while others may respond directly.

---

## 4. Comparison of Observations

| Observation | HTTP | HTTPS |
|---|---|---|
| Application protocol | HTTP | HTTP |
| TLS protection | No | Yes |
| Typical port | 80 | 443 |
| Response observed | Yes | Yes |
| Data protection in transit | No TLS protection | TLS protection |

The important distinction is not whether an HTTP response is received.

The important distinction is whether the communication is protected by TLS.

---

## 5. SOC Interpretation

A normal web request can produce evidence across multiple layers:

```text
DNS query
    ↓
Domain
    ↓
Resolved IP
    ↓
Network connection
    ↓
Transport protocol
    ↓
Destination port
    ↓
TLS information
    ↓
HTTP activity where visible
    ↓
Endpoint process
    ↓
User
```

A SOC analyst should correlate these pieces rather than treating one field as proof of malicious activity.

For example:

```text
TCP/443
```

commonly indicates HTTPS-related traffic, but it does not prove:

- The destination is legitimate
    
- The connection is malicious
    
- The application is definitely HTTPS
    
- The user intentionally initiated the connection
    
- Data was successfully transferred
    

Additional telemetry is required.

---

## 6. Evidence

### Screenshot

One screenshot was captured containing the command-line observations.

It demonstrates:

- DNS resolution
    
- HTTPS request
    
- HTTP response
    

![Windows Command Prompt showing `nslookup example.com`, `curl -I https://example.com`, and `curl -I http://example.com`, with DNS resolution and HTTP 200 OK responses for both](../../assets/day-09-network-observation.png)

#### Filename

```text
day-09-network-observation.png
```

---

## 7. Lessons Learned

### DNS

A domain can resolve to multiple IPv4 and IPv6 addresses.

### HTTP

HTTP provides application-level web communication without TLS protection.

### HTTPS

HTTPS provides HTTP communication protected by TLS.

### Network Analysis

A network connection becomes more useful when correlated across multiple layers.

```text
Domain
  ↓
IP
  ↓
Port
  ↓
Protocol
  ↓
Process
  ↓
User
  ↓
Context
```

### SOC Principle

> **Network indicators provide evidence and context. They do not automatically establish intent or maliciousness.**

---

## 8. Future Use

The concepts and observations from this lab will be reused during later SOC exercises involving:

- DNS investigation
    
- Suspicious outbound connections
    
- SIEM network telemetry
    
- Endpoint process investigation
    
- Command and control analysis
    
- Web and phishing investigations


# Part B — Windows & Ubuntu Network Troubleshooting Lab

> **Purpose**
>
> This lab documents basic network observation and connectivity troubleshooting using the Windows 11 lab VM and the Ubuntu Linux host. The objective was to use `ipconfig` / `ifconfig`, `ping`, `nslookup`, `resolvectl`, and `curl` to identify network configuration, test IP connectivity, investigate DNS resolution, and verify application-layer connectivity.

---

## Environment

### Systems

| System | Role | Network |
|---|---|---|
| Ubuntu Linux | Physical host | Wi-Fi |
| Windows 11 | VMware virtual machine | VMware virtual network |

The Ubuntu system is the physical host. There is no separate Ubuntu VM in this lab.

---

## Network Architecture

```text
                    Internet
                       │
                       │
                Home / Host Network
                       │
                Ubuntu Linux Host
                 Wi-Fi interface
                       │
                 VMware networking
                       │
                    vmnet8
                       │
                Windows 11 VM
````

The Windows VM uses a VMware virtual network, while the Ubuntu host also has physical Wi-Fi connectivity.

---

## Windows 11 Network Configuration

### Command

```cmd
ipconfig
```

### Observed configuration

```text
IPv4 Address:       192.168.27.128
Subnet Mask:        255.255.255.0
Default Gateway:   192.168.27.2
DNS Suffix:         localdomain
```

### What `ipconfig` proves

`ipconfig` provides local network configuration information such as:

- IPv4 address
    
- Subnet mask
    
- Default gateway
    
- DNS-related configuration
    

It establishes how the Windows system is configured before connectivity testing begins.

---

## Windows Connectivity Testing

### 1. Test the default gateway

#### Command

```cmd
ping 192.168.27.2
```

#### Result

```text
Packets Sent:     4
Packets Received: 4
Packet Loss:      0%
Average:          0 ms
```

#### Observation

The Windows VM successfully communicated with its default gateway using ICMP.

#### Conclusion

Basic connectivity between the Windows VM and its gateway is working.

This does not prove that:

- Internet connectivity works
    
- DNS works
    
- HTTP/HTTPS works
    
- Other network services are available
    

![Windows `ipconfig` output plus a successful `ping` to the VMware NAT gateway (192.168.27.2), 0% packet loss](../../assets/windows-ipconfig-ping-gateway.png)

---

### 2. Test external IP connectivity

#### Command

```cmd
ping 8.8.8.8
```

#### Result

```text
Packets Sent:     4
Packets Received: 4
Packet Loss:      0%
Average:          124 ms
```

Observed response times ranged from approximately:

```text
57 ms to 163 ms
```

#### Observation

The Windows VM successfully reached the external IP address `8.8.8.8` using ICMP.

#### Conclusion

The VM has working IP connectivity beyond its local gateway.

Because an IP address was supplied directly, DNS resolution was not required for this test.

---

### 3. Test DNS resolution

#### Command

```cmd
nslookup google.com
```

#### Result

The configured DNS server was:

```text
192.168.27.2
```

The response returned:

```text
google.com.localdomain
```

with an IPv4 address.

#### Observation

The Windows system has a configured DNS suffix:

```text
localdomain
```

The unqualified query caused the local suffix to be involved.

This does not mean DNS was broken.

![Windows ping to the VMware NAT gateway and to 8.8.8.8, both 0% packet loss, followed by an unqualified `nslookup google.com` returning `google.com.localdomain`](../../assets/windows-ping-external-nslookup-google.png)

---

### 4. Test an absolute DNS name

#### Command

```cmd
nslookup google.com.
```

The trailing period is intentional.

#### Result

The response returned:

```text
Name: google.com
```

with multiple IPv4 addresses and an IPv6 address.

#### Observation

The trailing period caused the query to be treated as the fully qualified DNS name `google.com` rather than a name to which the local DNS suffix was appended.

#### Conclusion

DNS resolution was working.

The earlier `google.com.localdomain` result was a consequence of the configured DNS suffix rather than a DNS failure.

---

### 5. Test hostname resolution and ICMP together

#### Command

```cmd
ping google.com
```

#### Result

Windows resolved the hostname to:

```text
142.250.122.139
```

and received:

```text
Packets Sent:     4
Packets Received: 4
Packet Loss:      0%
Average:          20 ms
```

#### Observation

This test demonstrated two stages:

```text
google.com
    ↓
DNS resolution
    ↓
142.250.122.139
    ↓
ICMP connectivity
    ↓
Reply received
```

#### Conclusion

The hostname could be resolved and the resulting IP address was reachable using ICMP.

---

### 6. Test HTTPS application connectivity

#### Command

```cmd
curl https://example.com
```

#### Result

The command successfully returned HTML content from the website.

#### Observation

The system was able to make an HTTPS request and receive an application-layer response.

#### Conclusion

This provides stronger evidence of web connectivity than `ping` alone because it tests communication with the HTTPS service.

![Windows `nslookup google.com.` (FQDN) resolving correctly, `ping google.com` succeeding, and `curl https://example.com` returning HTML content](../../assets/windows-nslookup-fqdn-ping-curl.png)

---

## Ubuntu Linux Network Configuration

### Command

```bash
ifconfig
```

### Relevant interfaces

#### Wi-Fi interface

```text
Interface:       wlp0s20f3
IPv4 Address:    192.168.1.24
Netmask:         255.255.255.0
```

This is the physical Wi-Fi interface used by the Ubuntu host.

#### VMware interfaces

```text
vmnet1
IPv4: 192.168.82.1

vmnet8
IPv4: 192.168.27.1
```

These interfaces are associated with VMware virtual networking.

The `vmnet8` network is relevant to the Windows VM because the Windows VM is configured on the `192.168.27.0/24` network.

![Ubuntu `ifconfig` output showing the loopback, vmnet1, vmnet8, and Wi-Fi interfaces with their addresses](../../assets/ubuntu-ifconfig-interfaces.png)

---

## Ubuntu IP Connectivity

### Command

```bash
ping -c 4 8.8.8.8
```

### Initial result

The first attempt returned:

```text
4 packets transmitted
0 received
100% packet loss
```

A subsequent continuous ping showed:

```text
5 packets transmitted
4 received
20% packet loss
```

A later controlled test using:

```bash
ping -c 4 8.8.8.8
```

returned:

```text
4 packets transmitted
4 received
0% packet loss
```

Average RTT was approximately:

```text
20.224 ms
```

### Observation

The initial packet loss was transient.

Repeating the test produced successful replies with 0% packet loss.

### Conclusion

The Ubuntu host had working IP connectivity to `8.8.8.8`.

The initial failed test was not sufficient evidence to conclude that external connectivity was unavailable.

This demonstrated the importance of repeating a connectivity test before declaring a network failure.

![Ubuntu `ping 8.8.8.8` results, both a plain run and `ping -c 4 8.8.8.8`, showing 0% packet loss](../../assets/ubuntu-ping-external-success.png)

---

## Ubuntu DNS Configuration

### Command

```bash
nslookup google.com
```

### Result

The local DNS resolver was:

```text
127.0.0.53
```

The query successfully returned multiple IPv4 and IPv6 addresses for `google.com`.

### Observation

`127.0.0.53` is the local DNS stub used by the Ubuntu system.

The `nslookup` command therefore communicates with the local resolver rather than directly querying the upstream DNS server.

---

## DNS Resolver Investigation

### Command

```bash
resolvectl status
```

### Relevant configuration

```text
Interface:        wlp0s20f3

Current DNS Server:
fe80::1

DNS Servers:
192.168.1.1
fe80::1

Default Route:
yes
```

### DNS resolution path

```text
Application
     ↓
127.0.0.53
     ↓
systemd-resolved
     ↓
192.168.1.1 / fe80::1
     ↓
DNS infrastructure
```

### What `resolvectl status` established

`nslookup` showed that DNS resolution worked.

`resolvectl status` showed how the Ubuntu host was configured to perform DNS resolution.

This distinction is important during troubleshooting:

```text
nslookup
    ↓
Tests DNS resolution

resolvectl status
    ↓
Shows DNS configuration and resolver status
```

![Ubuntu `nslookup google.com` returning multiple IPv4 and IPv6 addresses via the local resolver, plus `resolvectl status` showing the DNS server and interface configuration](../../assets/ubuntu-nslookup-resolvectl-status.png)

---

## Ubuntu HTTPS Connectivity

### Command

```bash
curl https://example.com
```

### Result

The command successfully returned HTML content from `example.com`.

### Conclusion

The Ubuntu host successfully communicated with the HTTPS service and received an application-layer response.

![Ubuntu `curl https://example.com` returning HTML content from the site](../../assets/ubuntu-curl-example-domain.png)

---

## Command Observation Summary

| Command | System | Result | What it proves |
|---|---|---|---|
| `ipconfig` | Windows | ✅ | Local network configuration |
| `ping 192.168.27.2` | Windows | ✅ | Gateway connectivity |
| `ping 8.8.8.8` | Windows | ✅ | External IP connectivity |
| `nslookup google.com` | Windows | ⚠️ | DNS resolution with local suffix behavior |
| `nslookup google.com.` | Windows | ✅ | Direct resolution of fully qualified name |
| `ping google.com` | Windows | ✅ | DNS resolution + ICMP connectivity |
| `curl https://example.com` | Windows | ✅ | HTTPS/application connectivity |
| `ifconfig` | Ubuntu | ✅ | Interface and network configuration |
| `ping -c 4 8.8.8.8` | Ubuntu | ✅ | External IP connectivity |
| `nslookup google.com` | Ubuntu | ✅ | DNS resolution |
| `resolvectl status` | Ubuntu | ✅ | DNS resolver configuration |
| `curl https://example.com` | Ubuntu | ✅ | HTTPS/application connectivity |

---

## Troubleshooting Model

The lab demonstrated a layered troubleshooting approach:

```text
1. Check local configuration
        ↓
   ipconfig / ifconfig
        ↓
2. Check IP connectivity
        ↓
   ping
        ↓
3. Check DNS resolution
        ↓
   nslookup
        ↓
4. Check application connectivity
        ↓
   curl
```

Each test answers a different question.

### `ipconfig` / `ifconfig`

> How is this system configured?

### `ping`

> Can this system communicate with the destination using ICMP?

### `nslookup`

> Can the hostname be resolved to network information?

### `curl`

> Can the system communicate with the web application?

---

## SOC Relevance

A SOC analyst should avoid treating "the network is down" as a single diagnosis.

For example:

```text
ping 8.8.8.8
        ↓
SUCCESS

curl https://example.com
        ↓
FAILURE
```

This does not indicate a complete network outage.

It suggests that IP connectivity exists and that the failure should be investigated at a higher layer, such as:

- DNS
    
- TCP connectivity
    
- Proxy
    
- TLS
    
- HTTP/application behavior
    
- Endpoint security controls
    

Similarly:

```text
ping 8.8.8.8
        ↓
SUCCESS

nslookup example.com
        ↓
FAILURE
```

would direct investigation toward DNS rather than basic IP connectivity.

---

## Evidence-Based Investigation

A successful ping does not prove that a web application is functioning.

```text
ICMP connectivity
        ≠
Web application availability
```

Likewise, failed ICMP does not necessarily mean that a service is unavailable because firewalls may block ICMP while allowing TCP/HTTPS traffic.

A SOC analyst should therefore identify **which layer has actually been tested** before drawing a conclusion.

---

## Security Considerations

The lab used private network addresses for the local environment.

Private IP addresses and other internal network information should **not be published in a public GitHub repository**.

Before publishing screenshots:

- Redact private IP addresses where appropriate.
    
- Do not expose credentials or secrets.
    
- Do not expose personal information.
    
- Keep raw lab evidence locally if it contains unnecessary internal details.
    

---

## Lessons Learned

1. A host being reachable by ICMP does not mean every service on that host is working.
    
2. DNS resolution and IP connectivity are separate troubleshooting areas.
    
3. `ping` to an IP address does not require DNS resolution.
    
4. `ping` to a hostname requires name resolution before the ICMP request can be sent.
    
5. `nslookup` tests DNS resolution.
    
6. `resolvectl status` helps identify the local DNS resolver and upstream DNS configuration.
    
7. `curl` provides evidence about application-layer web connectivity.
    
8. DNS responses can differ between systems because systems may use different resolvers and configurations.
    
9. Different DNS answers do not automatically indicate malicious activity.
    
10. A single failed connectivity test is not enough evidence to declare a network outage.
    
11. Troubleshooting should proceed from configuration and connectivity toward higher application layers.
    
12. SOC conclusions should distinguish observed evidence from assumptions.
    

---

## Future Use

This lab provides the foundation for later SOC investigations involving:

- DNS activity
    
- Network connections
    
- Endpoint network configuration
    
- Suspicious domains
    
- Connection failures
    
- Firewall and proxy investigation
    
- SIEM network telemetry
    
- Correlation of DNS activity with subsequent connections
    

The same layered approach can later be applied to real SOC alerts:

```text
Alert
  ↓
Identify host
  ↓
Check network configuration
  ↓
Check IP connectivity
  ↓
Check DNS
  ↓
Check destination port/service
  ↓
Inspect endpoint/network telemetry
  ↓
Determine impact and next action
```

---

## Related Notes

- [Networking Fundamentals](../../notes/networking/networking-fundamentals.md)
    
- [IP Addressing](../../notes/networking/ip-addressing.md)
    
- [Ports & Protocols](../../notes/networking/ports-and-protocols.md)
    
- [Protocol Comparison](../../notes/networking/protocol-comparison.md)
    
- [Routing & Network Segmentation](../../notes/networking/routing-and-network-segmentation.md)
    
- [DHCP & DNS](../../notes/networking/dhcp-and-dns.md)
    
- [Home Lab Network Diagram](../../notes/networking/home-lab-network-diagram.md)
    
- [Windows 11 Lab](../week-1-lab-setup/windows-11-lab.md)
    
- [VMware Setup](../week-1-lab-setup/vmware-setup.md)
