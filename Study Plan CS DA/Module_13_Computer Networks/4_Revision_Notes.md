# Computer Networks — Revision Notes for GATE 2027

> **Last-day crisp revision** — All key concepts, formulas, and traps at a glance.

---

## 🔥 GATE Weightage: 3–5 marks every year | High priority

---

## 1. IP Addressing & Subnetting

### Classful Addressing
| Class | Range | Default Mask | Hosts |
|-------|-------|-------------|-------|
| A | 1–126 | /8 | 2²⁴−2 |
| B | 128–191 | /16 | 2¹⁶−2 |
| C | 192–223 | /24 | 2⁸−2 |
| D | 224–239 | Multicast | — |
| E | 240–255 | Reserved | — |

- **127.x.x.x** → Loopback (NOT Class A usable)
- **Private:** 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

### Subnetting Quick Rules
- Usable hosts = **2ⁿ − 2** (n = host bits)
- Block size = **2^(host bits)**
- Network addr: first address (host bits all 0)
- Broadcast addr: last address (host bits all 1)
- VLSM: **Allocate largest subnet first**

### Supernetting / Route Aggregation
- Find common prefix bits across all networks
- Aggregated prefix length = number of common bits
- **Longest Prefix Match** → router picks most specific entry

---

## 2. Data Link Layer

### Error Detection & Correction
| Method | Detects | Corrects |
|--------|---------|----------|
| Single Parity | 1-bit errors | Nothing |
| 2D Parity | All 1,2,3-bit; most 4-bit | 1-bit |
| Hamming | 2-bit detect | 1-bit correct |
| CRC | Burst errors up to degree | Detects only |
| Checksum | Errors | Detects only |

**Hamming Code:**
- Parity bits at positions 2⁰, 2¹, 2², ...
- **2ʳ ≥ m + r + 1** (r = parity bits, m = data bits)
- Error position = binary value of failed parity checks

**CRC:**
- Append (degree of G(x)) zeros to data
- XOR divide by generator polynomial
- Remainder = CRC bits
- Can detect: all single-bit, all double-bit (if G has ≥3 terms), all odd-bit (if (x+1) is factor), bursts ≤ degree

**Hamming Distance:**
- Detect d errors: d_min ≥ d + 1
- Correct t errors: d_min ≥ 2t + 1  
- Detect d AND correct t simultaneously: **d_min ≥ d + t + 1** (d > t)

### Delays
| Delay | Formula |
|-------|---------|
| Transmission | Tt = L / B |
| Propagation | Tp = d / v |
| Total (1 link) | Tt + Tp |
| RTT | 2 × Tp |
| Store & Forward (k links) | k×Tt + Tp_total (if same BW) |

- **a = Tp/Tt** (critical ratio for efficiency)

### Sliding Window Protocols

| Protocol | Max Window | Efficiency | On Error |
|----------|-----------|------------|----------|
| Stop-Wait | 1 | 1/(1+2a) | Retransmit 1 |
| GBN | 2ⁿ − 1 | min(1, W/(1+2a)) | Retransmit from error |
| SR | 2ⁿ⁻¹ | min(1, W/(1+2a)) | Retransmit only lost |

- **GBN:** W_sender ≤ 2ⁿ − 1, W_receiver = 1
- **SR:** W_sender + W_receiver ≤ 2ⁿ, usually W = 2ⁿ⁻¹
- Full utilization: **W ≥ 1 + 2a**

---

## 3. MAC Sublayer

### Random Access Protocols
| Protocol | Max Throughput | At G = |
|----------|---------------|--------|
| Pure ALOHA | **1/(2e) ≈ 18.4%** | 0.5 |
| Slotted ALOHA | **1/e ≈ 36.8%** | 1.0 |

- Vulnerable period: Pure = 2T, Slotted = T
- S = G × e^(−2G) (Pure), S = G × e^(−G) (Slotted)

### CSMA/CD
- **Min frame size = 2 × Tp × B** (must detect collision before transmission ends)
- Collision detection time ≤ 2 × propagation delay (RTT)
- **Binary Exponential Backoff:** After kth collision, wait random [0, 2^k − 1] slots

### Key Devices
| Device | Layer | Breaks Collision Domain? | Breaks Broadcast Domain? |
|--------|-------|-------------------------|-------------------------|
| Hub | 1 | No | No |
| Switch | 2 | Yes | No |
| Router | 3 | Yes | Yes |

### ARP / DHCP
- **ARP:** IP → MAC resolution (broadcast request, unicast reply)
- **RARP:** MAC → IP (obsolete, replaced by BOOTP/DHCP)
- **DHCP DORA:** Discover → Offer → Request → ACK (uses UDP 67/68)

---

## 4. Network Layer

### IP Header Key Fields
- Version, IHL, Total Length, TTL, Protocol, Source IP, Destination IP
- **Identification, Flags (MF, DF), Fragment Offset** → for fragmentation

### IP Fragmentation
- Max data per fragment = **MTU − 20** (must be multiple of 8)
- Fragment offset = **cumulative_data / 8**
- MF = 1 for all except last fragment
- Each fragment gets its own header (20 bytes)
- Number of fragments = ⌈original_payload / (MTU − 20)⌉

### Routing Protocols

| Feature | Distance Vector (RIP) | Link State (OSPF) |
|---------|----------------------|-------------------|
| Algorithm | Bellman-Ford | Dijkstra |
| Shares | Distance table (to neighbors) | Full topology (to all) |
| Convergence | Slow | Fast |
| Problem | Count-to-infinity | Memory intensive |
| Timer | Periodic (30s for RIP) | Event-triggered |
| Metric | Hop count (max 15) | Link cost |

**Count-to-Infinity Solutions:**
- Split Horizon (don't advertise back)
- Poison Reverse (advertise ∞ back)
- Hold-down timer
- Triggered updates

### ICMP
- Network layer protocol inside IP
- Key messages: Echo (ping), Dest Unreachable, Time Exceeded (traceroute), Redirect

### NAT
- Translates private ↔ public IP
- PAT/NAPT: uses port numbers for multiplexing

---

## 5. Transport Layer

### TCP vs UDP
| Feature | TCP | UDP |
|---------|-----|-----|
| Type | Connection-oriented | Connectionless |
| Reliability | Yes (ACK, retransmit) | No |
| Header | 20 bytes min | 8 bytes |
| Flow Control | Yes (window) | No |
| Congestion Control | Yes | No |
| Ordering | Yes (seq numbers) | No |

### TCP Header Key Fields
- Source/Dest Port, Sequence Number, ACK Number
- Flags: SYN, ACK, FIN, RST, PSH, URG
- Window Size (for flow control)

### TCP Connection
- **3-way handshake:** SYN → SYN-ACK → ACK
- **4-way termination:** FIN → ACK → FIN → ACK
- **TIME_WAIT** = 2 × MSL (after sending final ACK)

### TCP Congestion Control

| Phase | cwnd Growth | Trigger to Next |
|-------|------------|-----------------|
| Slow Start | Doubles each RTT (exponential) | cwnd ≥ ssthresh → CA |
| Congestion Avoidance | +1 MSS per RTT (linear) | Loss detected |

**On Timeout (Tahoe & Reno):**
- ssthresh = cwnd/2
- cwnd = 1 MSS
- Enter Slow Start

**On 3 Duplicate ACKs (Reno):**
- ssthresh = cwnd/2
- cwnd = ssthresh (NOT 1)
- Enter Fast Recovery / CA

**Tahoe on 3 dup ACKs:** cwnd = 1 (same as timeout)

### TCP vs Tahoe vs Reno

| Event | Tahoe | Reno |
|-------|-------|------|
| Timeout | cwnd=1, SS | cwnd=1, SS |
| 3 dup ACK | cwnd=1, SS | cwnd=ssthresh, CA |

### Flow Control
- Receiver advertises **rwnd** (receive window)
- Sender: effective window = min(cwnd, rwnd)
- **Silly window syndrome:** avoided by Nagle's algorithm (sender) + delayed ACK (receiver)

### Token Bucket & Leaky Bucket
- **Leaky Bucket:** constant output rate, smooths all bursts
- **Token Bucket:** allows controlled bursts
  - Max burst duration = C/(R − r) where C = capacity, R = max output, r = token rate
  - Max burst data = C + r × burst_duration

---

## 6. Application Layer

### Key Protocols & Ports
| Protocol | Port | Transport | Purpose |
|----------|------|-----------|---------|
| HTTP | 80 | TCP | Web |
| HTTPS | 443 | TCP | Secure web |
| FTP ctrl/data | 21/20 | TCP | File transfer |
| SMTP | 25 | TCP | Send email |
| POP3 | 110 | TCP | Retrieve email |
| IMAP | 143 | TCP | Sync email |
| DNS | 53 | UDP/TCP | Name resolution |
| DHCP | 67/68 | UDP | IP assignment |
| TELNET | 23 | TCP | Remote access |
| SSH | 22 | TCP | Secure remote |

### DNS
- Hierarchical: Root → TLD → Authoritative
- Recursive vs Iterative resolution
- Uses UDP for queries, TCP for zone transfers

### HTTP
- HTTP/1.0: non-persistent (new TCP per object)
- HTTP/1.1: persistent (multiple objects, pipelining)
- **RTT calculation:**
  - Non-persistent: 2×RTT per object
  - Persistent non-pipelined: RTT + RTT per object
  - Persistent pipelined: RTT + RTT (all objects together)

### FTP
- 2 connections: control (21, persistent) + data (20, per transfer)
- Active mode: server → client data connection
- Passive mode: client → server data connection

---

## 7. Critical Traps & Common Mistakes

1. **Fragmentation offset is in units of 8 bytes**, not bytes
2. **GBN window ≤ 2ⁿ − 1**, NOT 2ⁿ (SR can be 2ⁿ⁻¹)
3. **CRC:** append zeros equal to DEGREE of polynomial, not number of terms
4. **Usable hosts = 2ⁿ − 2** (not 2ⁿ) — subtract network & broadcast
5. **Longest prefix match**, not first match
6. **ARP is broadcast request, unicast reply** (not both broadcast)
7. **TCP Reno on 3 dup ACKs: cwnd = ssthresh** (not 1)
8. **Efficiency formula applies only when W < 1+2a**; if W ≥ 1+2a, η = 100%
9. **CSMA/CD min frame size**: Tt ≥ 2×Tp, so L_min = 2 × Tp × B
10. **In Pure ALOHA, vulnerable time = 2T** (not T like Slotted)

---

## 8. OSI vs TCP/IP Model

```
OSI Model          TCP/IP Model
-----------        ------------
Application   ─┐
Presentation   ├── Application
Session       ─┘
Transport     ──── Transport
Network       ──── Internet
Data Link     ─┐
Physical      ─┴── Network Access
```

**PDU names:** Frame (DLL) → Packet (Network) → Segment (Transport) → Data (Application)

---

## 9. Spanning Tree Protocol (STP)

- Elects **Root Bridge** (lowest Bridge ID)
- Each non-root switch picks **Root Port** (lowest cost to root)
- Each segment picks **Designated Port**
- All other ports → **Blocking state**
- A spanning tree of N switches has **N−1 active links**

---

## 10. Physical Layer & Channel Capacity

- **Nyquist (noiseless):** $C = 2B\log_2 L$ bps ($B$=Hz, $L$=signal levels)
- **Shannon (noisy):** $C = B\log_2(1 + \text{SNR})$
- SNR dB→linear: $\text{SNR} = 10^{dB/10}$
- Actual capacity ≤ min(Nyquist, Shannon)

### Multiplexing
| FDM | Frequency bands + guard bands |
|-----|------|
| TDM | Time slots, stat TDM = on-demand |
| WDM | FDM for optical fiber |
| CDMA | Orthogonal chip codes, decode by inner product |

### Line Encoding
- Manchester: transition at mid-bit always → excellent clock recovery
- NRZ-I: transition for 1, none for 0
- AMI: alternating ±V for 1, 0V for 0

---

## 11. IPv6

- 128-bit addresses, fixed 40-byte header
- No checksum, no router fragmentation, no broadcast (multicast instead)
- NDP replaces ARP, IPSec mandatory
- Transition: Dual Stack, Tunneling (6to4), NAT64

---

## 12. Network Security

### Cryptography
- Symmetric: same key (AES, DES). Fast.
- Asymmetric: public+private key (RSA). Slow.
- RSA: $C = M^e \mod n$, $M = C^d \mod n$
- DH: shared key $g^{ab} \mod p$ (vulnerable to MITM without certs)

### Security Services
- Digital signature: sign with private, verify with public → auth + integrity + non-repudiation
- TLS: asymmetric for key exchange, symmetric for data
- IPSec: AH (auth only), ESP (auth + encryption)

### Firewall Types
| Packet Filter | Network layer, IP/port |
|---|---|
| Stateful | Transport, connection tracking |
| Application Gateway | App layer, content inspection |

---

## 13. Advanced Routing

| | RIP | OSPF | BGP |
|--|-----|------|-----|
| Type | DV | LS | Path Vector |
| Algo | Bellman-Ford | Dijkstra | Best path |
| Metric | Hops (max 15) | Cost | Policy |
| Scope | Intra-AS | Intra-AS | Inter-AS |
| Transport | UDP/520 | IP/89 | TCP/179 |

- BGP: AS-PATH prevents loops
- OSPF: areas, LSAs, faster convergence

---

## 14. HDLC & PPP
- HDLC: bit-oriented, bit stuffing (0 after 5 ones)
- PPP: byte-oriented, byte stuffing (7D escape), LCP/NCP/PAP/CHAP

---

*Quick revision complete — Computer Networks for GATE 2027* ⚡
