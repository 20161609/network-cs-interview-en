# Network CS Interview – 6-Chapter Curriculum

> Goal: A compact, interview-oriented networking curriculum with hands-on notes.

## How to Use
1) Read 1 chapter per day (about 60–90 min).
2) Do the matching practice after each chapter.
3) Summarize answers in your own words at the end of each file.

## Chapters
- **01 Foundations** — OSI vs TCP/IP, frames/packets, latency vs bandwidth.
- **02 IP & Subnetting** — IPv4/IPv6, CIDR, routing basics.
- **03 Transport (TCP/UDP)** — handshake, reliability, congestion control.
- **04 Routing & Switching** — L2 vs L3, STP/VLAN, OSPF/BGP basics.
- **05 HTTP/DNS/TLS** — request/response, caching, name resolution, TLS flow.
- **06 Troubleshooting & Security** — ping/traceroute, MTU/MSS, firewalls, NAT.

## Practice
Check the `practice/` folder for CLI drills, packet peeking, and TCP hands-on.

## Interview Quick Prep
- Be able to whiteboard: TCP 3-way/4-way, TLS 1.3 flow, DNS resolution path.
- Explain trade-offs: TCP vs UDP, CDN vs origin, NAT vs public IP.
- Diagnose a slow website with only CLI tools (ping, traceroute, curl).

## Repository Structure
``` bash
network-cs-interview/
├─ README.md
├─ 01_foundations.md
├─ 02_ip_subnetting.md
├─ 03_transport.md
├─ 04_routing_switching.md
├─ 05_http_dns_tls.md
├─ 06_troubleshooting_security.md
└─ practice/
   ├─ 01_cli_networking.md
   ├─ 02_packet_peek.md
   ├─ 03_tcp_hands_on.md
   └─ assets/   # Where to put pictures/captures (empty folder)
```

## Link
https://github.com/20161609/network-cs-interview-en