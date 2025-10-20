# 01 — Foundations

> Goal: Build a clear mental model of how data moves across layers so you can localize issues fast in interviews.

---

## Learning Objectives
- Map real-world issues to **OSI (L1–L7)** and **TCP/IP (Link/Internet/Transport/Application)**.
- Explain **encapsulation/decapsulation** and data units (Frame/Packet/Segment).
- Distinguish **latency vs bandwidth vs throughput** and why users perceive “slow”.

---

## TL;DR
- **Layers are a debugging language.** Say “L3 routing problem” or “L7 caching miss,” not “the internet is broken.”
- **Units:** Frame(L2) → Packet(L3) → Segment(L4) → Data(L7).
- **Performance:** Latency (delay) ≠ Bandwidth (capacity). Throughput is what you actually get.
- **MTU/MSS:** Path MTU limits L2 payload; MSS limits TCP payload. Mismatch → fragmentation/blackhole.

---

## Big Picture (ASCII)
Sender  
`App → TCP/UDP → IP → Ethernet/Wi-Fi → [Switch] → [Router] → Internet ...`  
Receiver  
`... → Ethernet/Wi-Fi → IP → TCP/UDP → App`

Encapsulation on send (headers added); decapsulation on receive (headers removed).

---

## OSI ↔ TCP/IP Mapping (Interview Level)
| OSI | TCP/IP | Example |
|---|---|---|
| L7 Application | Application | HTTP, DNS, TLS logic in user space |
| L6 Presentation | Application | Serialization, compression, TLS record |
| L5 Session | Application | Session tokens, app-level retries |
| L4 Transport | Transport | TCP (reliability, flow, congestion), UDP |
| L3 Network | Internet | IP, ICMP, routing/forwarding |
| L2 Data Link | Link | Ethernet, 802.11 Wi-Fi, VLAN, ARP |
| L1 Physical | Link | Fiber, copper, RF, signaling |

> In interviews, it’s fine to collapse L5–L7 into “Application.”

---

## Data Units & Headers (Mental Math)
- **L2 Frame:** `[L2 hdr][L3 pkt][L2 FCS]`
- **L3 Packet:** `[IP hdr][L4 seg]`
- **L4 Segment:** `[TCP/UDP hdr][App data]`

Typical sizes (no options): Ethernet hdr 14B, IPv4 hdr 20B, TCP hdr 20B, FCS 4B.  
**Rule of thumb:** On Ethernet with MTU 1500 → TCP MSS ≈ 1460 (1500 − 20 IP − 20 TCP).

---

## Latency, Bandwidth, Throughput
- **Latency:** time for one bit to travel (propagation + queueing + processing). Impacts TTFB.
- **Bandwidth:** pipe size (e.g., 100 Mbps).
- **Throughput:** useful data rate achieved; limited by latency (window/RTT), loss, congestion.

> High bandwidth + high latency can still feel slow (e.g., satellite links).

---

## Core Mechanisms You’ll Name-Drop
- **ARP (IPv4)/ND (IPv6):** resolve IP→MAC for local next-hop.
- **Default Gateway:** host forwards non-local traffic to router.
- **Longest Prefix Match:** routers pick the most specific IP route.
- **DNS:** name→IP; often the *real* reason “it’s slow.”
- **TLS:** crypto handshake adds RTTs; TLS 1.3 reduces round trips.

---

## Everyday Flow: “User types a URL”
1. Browser checks cache → OS resolver cache → **DNS** query (recursive → root/TLD/auth).
2. Gets server IP (maybe via **CDN**).
3. **TCP 3-way handshake** (or QUIC over UDP).
4. **TLS handshake** (usually 1.3).
5. **HTTP request/response**, possibly cached (ETag/Cache-Control).
6. Objects fetched in parallel; rendering starts as bytes arrive.

---

## Common Pitfalls (What interviewers probe)
- Equating **bandwidth** with **faster page loads** (ignores latency/handshake).
- Forgetting **MSS/MTU**: Path MTU too small → fragmentation or blackhole.
- Blaming “the network” without layer-localizing (start L1→L7).
- Ignoring **DNS/TLS** cost when measuring “slow first byte.”

---

## Quick Layered Troubleshooting Script
1. **L1/2:** Link up? `ipconfig/ifconfig`, Wi-Fi signal, duplex/speed, VLAN.
2. **L3:** IP/gateway correct? `ping gateway`, `ping 8.8.8.8`.
3. **L3 path:** `traceroute`/`tracert` to see where loss/latency spikes.
4. **L7 name:** `nslookup`/`dig` check DNS resolution + TTL.
5. **TLS/HTTP:** `curl -v https://site` (SNI, ALPN, TLS version, redirects).
6. **MTU:** try smaller pings with DF (OS-specific) or adjust MSS on client side to test.

---

## Mini Glossary
- **Encapsulation:** wrapping data with headers per layer.
- **Fragmentation:** splitting packets when exceeding MTU (IPv4 can fragment; IPv6 avoids).
- **Jitter:** latency variation; hurts real-time media.
- **Head-of-Line blocking:** in-order delivery stalls higher layers (e.g., TCP).

---

## Interview Prompts (with crisp bullets)
- **“OSI vs TCP/IP in 60s?”**  
  OSI is 7 ref layers; TCP/IP is pragmatic 4. Map L2↔Link, L3↔Internet, L4↔Transport, L5-7↔Application. Use layers to localize faults.
- **“What is MTU/MSS and why it breaks things?”**  
  MTU is L2 payload cap; MSS is TCP payload cap = MTU − IP − TCP. If PMTUD fails, large packets drop (no ICMP), connections hang.
- **“Latency vs bandwidth example?”**  
  High-bandwidth/high-latency path (e.g., satellite) still loads pages slowly due to handshakes and window/RTT dynamics.

---

## Tiny Cheats you can recall fast
- Ethernet MTU 1500 → **TCP MSS ≈ 1460** (no options).
- **Private IPv4:** 10/8, 172.16/12, 192.168/16.
- **DNS perf:** cache + keep connections warm (HTTP/2 or 3).

---

## Practice Hook
Do `practice/01_cli_networking.md` after reading:
- Capture `ipconfig/ifconfig`, gateway, DNS.
- `ping` local gateway vs public IP; compare latency.
- `traceroute` a public site and note the first hop outside your LAN.
- `curl -v https://example.com` and identify the TLS version and ALPN.

---

## Summary (Write in your own words)
- What changed in your mental model?
- Where would you start debugging a “site is slow” ticket?

---