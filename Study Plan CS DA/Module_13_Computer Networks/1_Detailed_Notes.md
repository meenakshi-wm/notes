# 🌐 Computer Networks — Detailed Notes for GATE 2027 CS/IT 🎯

> 🌟 **"Computer Networks connect 5 billion internet users, 15 billion devices, and make everything from WhatsApp messages to Netflix streams possible!"**

> 💡 **Beginner Analogy — The Postal System 📮:**
> Computer networking is EXACTLY like the postal system:
> - **Your Letter** = Data packet 📨
> - **Your Address** = IP address (e.g., 192.168.1.1) 🏠
> - **PIN Code** = Subnet mask (which locality/area) 📍
> - **Post Office** = Router (decides which direction to send your letter) 🧭
> - **Registered Post** = TCP (guaranteed delivery with acknowledgment) ✅
> - **Regular Post** = UDP (faster, but no guarantee of delivery) ⚡
> - **Courier Company** = Internet Service Provider (Jio, Airtel, BSNL) 🚚
>
> Every time you open a website, your computer sends thousands of "letters" (packets) through this "postal system"! 🚀

---

## 🏭 Where is Computer Networks Knowledge Used?

| 🌍 Application | 🔧 How | 📌 Network Concept |
|---|---|---|
| **Every website (HTTP/HTTPS)** 🌐 | Browser communicates with server | Application layer, TCP, DNS |
| **WhatsApp/Telegram** 💬 | Real-time messaging | TCP/UDP, End-to-end encryption |
| **Zoom/Teams Video Calls** 📹 | Real-time video streaming | UDP, RTP, Congestion control |
| **Online Gaming** 🎮 | Low-latency multiplayer | UDP (speed over reliability) |
| **Cloud Computing (AWS/Azure)** ☁️ | Virtual networks, load balancing | Subnetting, NAT, VPN |
| **5G Networks** 📶 | Ultra-fast mobile internet | MAC protocols, Multiplexing |
| **CDN (Cloudflare, Akamai)** ⚡ | Fast content delivery worldwide | Routing, Caching, DNS |
| **Blockchain/Crypto** ₿ | Peer-to-peer transaction verification | P2P networking, Consensus |
| **IoT (Smart Home)** 🏠 | Smart devices communication | IP addressing, Lightweight protocols |
| **VPN (NordVPN, etc.)** 🔒 | Secure remote access | Tunneling, Encryption |
| **IRCTC/Banking** 🚂🏦 | Secure transactions | HTTPS (TLS/SSL), TCP |

---

## 📊 GATE Weightage & Exam Strategy

| Parameter | Details |
|---|---|
| **Expected Marks** | 5–10 marks every year |
| **Questions** | 3–5 questions (mix of 1-mark and 2-mark) |
| **High-Yield Topics** | IP Addressing/Subnetting, Sliding Window Protocols, TCP Congestion Control, Delays |
| **Moderate-Yield** | Error Detection (CRC, Hamming), MAC Protocols, Routing Algorithms, Application Layer |
| **Low-Yield** | OSI Model theory, ARP/DHCP descriptive, FTP/SMTP specifics |

**Topic-wise Frequency (last 10 years):**

| Topic | Avg Questions/Year | Typical Marks |
|---|---|---|
| IP Addressing & Subnetting | 1–2 | 2–4 |
| Sliding Window Protocols | 1 | 2 |
| Delays (Propagation, Transmission) | 1 | 1–2 |
| Error Detection (CRC, Hamming) | 0–1 | 1–2 |
| TCP Congestion/Flow Control | 1 | 2 |
| Routing Algorithms | 0–1 | 1–2 |
| Application Layer (DNS, HTTP) | 0–1 | 1 |
| MAC Protocols (ALOHA, CSMA) | 0–1 | 1–2 |

---

## Module 1: IP Addressing, Subnetting & Supernetting

### 1.1 Classful IP Addressing

An IPv4 address = **32 bits** = 4 octets, written in dotted decimal.

| Class | Leading Bits | Network Bits | Host Bits | First Octet Range | #Networks | #Hosts/Network |
|---|---|---|---|---|---|---|
| **A** | 0 | 8 | 24 | 1–126 | $2^7 - 2 = 126$ | $2^{24} - 2$ |
| **B** | 10 | 16 | 16 | 128–191 | $2^{14} = 16384$ | $2^{16} - 2$ |
| **C** | 110 | 24 | 8 | 192–223 | $2^{21}$ | $2^{8} - 2 = 254$ |
| **D** | 1110 | — | — | 224–239 | Multicast | — |
| **E** | 1111 | — | — | 240–255 | Reserved | — |

**Special Addresses:**
- `0.0.0.0` — This network
- `127.x.x.x` — Loopback
- `255.255.255.255` — Limited broadcast
- Host bits all 0 — Network address
- Host bits all 1 — Directed broadcast address

**Private IP Ranges:**

| Class | Range | CIDR |
|---|---|---|
| A | 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| B | 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| C | 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

### 1.2 Classless Addressing (CIDR)

In Classless Inter-Domain Routing, any prefix length `/n` is allowed (not restricted to /8, /16, /24).

**Notation:** $a.b.c.d/n$, where $n$ = number of network (prefix) bits.

**Key Formulas:**
- Number of hosts = $2^{32-n} - 2$
- Subnet mask: first $n$ bits = 1, remaining = 0
- Block size = $2^{32-n}$
- Network address: IP AND Subnet Mask
- Broadcast address: Network address OR (NOT Subnet Mask)

**CIDR Subnet Masks Quick Reference:**

| Prefix | Subnet Mask | Hosts | Block Size |
|---|---|---|---|
| /24 | 255.255.255.0 | 254 | 256 |
| /25 | 255.255.255.128 | 126 | 128 |
| /26 | 255.255.255.192 | 62 | 64 |
| /27 | 255.255.255.224 | 30 | 32 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 4 |
| /31 | 255.255.255.254 | 2 (P2P) | 2 |
| /32 | 255.255.255.255 | 1 (host route) | 1 |

> 🎯 **GATE Trick:** To quickly find the network address: find block size = 2^(32-n). The network address's last octet (where subnetting happens) is a multiple of block size. Example: 200.1.2.130/26 → block=64 → 130/64=2 → network starts at 2×64=128 → 200.1.2.128

**Worked Example 1:** Given 200.1.2.130/26
- Prefix = 26 → Host bits = 6
- Subnet mask = 255.255.255.192
- Block size = $2^6 = 64$
- 130 / 64 = 2 remainder 2 → Subnet starts at $2 \times 64 = 128$
- Network address = 200.1.2.128
- Broadcast = 200.1.2.191 (128 + 63)
- Valid hosts: 200.1.2.129 – 200.1.2.190
- Number of valid hosts: 62

**Worked Example 2:** Given 172.16.50.200/20
- Prefix = 20 → Host bits = 12
- Block size = 2¹² = 4096 → In terms of octets: 3rd octet changes in blocks of 4096/256 = 16
- 3rd octet: 50 / 16 = 3 remainder 2 → network starts at 3×16 = 48
- Network address: 172.16.48.0
- Broadcast: 172.16.63.255 (48 + 15 in 3rd octet, 255 in 4th)
- Hosts: 172.16.48.1 to 172.16.63.254 = 4094 hosts

**Worked Example 3:** Which of these IPs are in the SAME subnet as 10.5.12.100/21?
(a) 10.5.8.1 (b) 10.5.15.255 (c) 10.5.16.0 (d) 10.5.11.200

/21 → block in 3rd octet = 2^(24-21) = 8
3rd octet: 12/8 = 1 remainder 4 → network starts at 1×8 = 8
Network: 10.5.8.0/21 → range: 10.5.8.0 – 10.5.15.255

(a) 10.5.8.1 → 8 in [8,15] ✓ **SAME**
(b) 10.5.15.255 → 15 in [8,15] ✓ **SAME** (this is broadcast!)
(c) 10.5.16.0 → 16 not in [8,15] ✗ **DIFFERENT** (next subnet)
(d) 10.5.11.200 → 11 in [8,15] ✓ **SAME**

### 1.3 Subnetting (Extended)

Subnetting = borrowing bits from the host portion to create sub-networks.

If original network has $h$ host bits and we borrow $s$ bits for subnets:
- Number of subnets = $2^s$
- Hosts per subnet = $2^{h-s} - 2$
- New prefix length = original prefix + $s$

**Worked Example 1:** Subnet 192.168.1.0/24 into 4 subnets.

- Need $2^s \geq 4 \Rightarrow s = 2$
- New prefix = /26
- Block size = $2^{32-26} = 64$

| Subnet | Network Address | First Host | Last Host | Broadcast |
|---|---|---|---|---|
| 0 | 192.168.1.0/26 | 192.168.1.1 | 192.168.1.62 | 192.168.1.63 |
| 1 | 192.168.1.64/26 | 192.168.1.65 | 192.168.1.126 | 192.168.1.127 |
| 2 | 192.168.1.128/26 | 192.168.1.129 | 192.168.1.190 | 192.168.1.191 |
| 3 | 192.168.1.192/26 | 192.168.1.193 | 192.168.1.254 | 192.168.1.255 |

**Worked Example 2:** Subnet 10.0.0.0/8 to have at least 500 hosts per subnet.
- Need $2^h - 2 \geq 500$ → $2^h \geq 502$ → $h = 9$ (2⁹ = 512 → 510 hosts)
- Borrowed bits = 24 - 9 = 15 (original /8 had 24 host bits)
- New prefix = /23
- Number of subnets = 2¹⁵ = 32,768

**Worked Example 3 (VLSM — GATE Favorite!):**
Given 192.168.10.0/24, allocate subnets for:
- LAN A: 100 hosts
- LAN B: 50 hosts
- LAN C: 25 hosts
- WAN link: 2 hosts

**Step 1:** Sort by requirement (largest first): A(100), B(50), C(25), WAN(2)

**Step 2:** Allocate each:
- A: need 100 → 2⁷=128≥102 → /25 (126 hosts). Range: 192.168.10.0/25 (0–127)
- B: need 50 → 2⁶=64≥52 → /26 (62 hosts). Range: 192.168.10.128/26 (128–191)
- C: need 25 → 2⁵=32≥27 → /27 (30 hosts). Range: 192.168.10.192/27 (192–223)
- WAN: need 2 → 2²=4≥4 → /30 (2 hosts). Range: 192.168.10.224/30 (224–227)

**Result:**
| Subnet | CIDR | Range | Hosts |
|---|---|---|---|
| LAN A | 192.168.10.0/25 | .0 – .127 | 126 |
| LAN B | 192.168.10.128/26 | .128 – .191 | 62 |
| LAN C | 192.168.10.192/27 | .192 – .223 | 30 |
| WAN | 192.168.10.224/30 | .224 – .227 | 2 |
| **Remaining** | 192.168.10.228 – .255 | | Available |

### 1.4 Supernetting (Route Aggregation)

Supernetting = combining contiguous networks into a single larger block (opposite of subnetting).

**Conditions for Supernetting:**
1. Networks must be **contiguous**
2. Number of networks must be a **power of 2**
3. First network address must be **evenly divisible** by the total block size

**Example 1:** Aggregate 200.1.0.0/24, 200.1.1.0/24, 200.1.2.0/24, 200.1.3.0/24
- 4 = $2^2$ networks → borrow 2 bits from prefix
- New prefix = 24 − 2 = /22
- Aggregated: 200.1.0.0/22

**Example 2:** Can we aggregate 192.168.2.0/24 and 192.168.3.0/24?
- 2 = 2¹ networks → new prefix = /23
- First network: 192.168.2.0. Block size at /23 = 2⁹ = 512 addresses. 2.0 → 2 is even → 2/2 = 1 → 1×2 = 2 ✓ (divisible by 2 in 3rd octet)
- **Yes: 192.168.2.0/23** ✓

**Example 3:** Can we aggregate 192.168.1.0/24 and 192.168.2.0/24?
- Block size at /23: 2 in 3rd octet. First: 1.0 → 1/2 = 0.5 → NOT divisible!
- **No!** These are not properly aligned. (1.0 starts at odd position) ✗

### 1.5 Longest Prefix Match (LPM) — Extended

When a router has multiple matching entries in the forwarding table, it selects the entry with the **longest prefix match** (most specific route).

**Example 1:**

| Entry | Destination | Next Hop |
|---|---|---|
| 1 | 200.1.0.0/22 | Interface A |
| 2 | 200.1.1.0/24 | Interface B |
| 3 | 200.1.1.128/25 | Interface C |

For packet destined to 200.1.1.200:
- Matches Entry 1 (/22) ✓
- Matches Entry 2 (/24) ✓
- Matches Entry 3 (/25) ✓
- **Selected: Entry 3** (longest prefix = /25)

**Example 2 — GATE Style:**
Routing table:
| Prefix | Interface |
|---|---|
| 12.0.0.0/8 | eth0 |
| 12.1.0.0/16 | eth1 |
| 12.1.2.0/24 | eth2 |
| 0.0.0.0/0 | eth3 (default) |

Where does 12.1.2.5 go? → Matches /8 ✓, /16 ✓, /24 ✓ → **eth2** (/24 is longest)
Where does 12.1.3.5 go? → Matches /8 ✓, /16 ✓ → **eth1** (/16 is longest)
Where does 12.2.1.1 go? → Matches /8 ✓ → **eth0**
Where does 13.0.0.1 go? → Only default match → **eth3**

> 🎯 **GATE Trick:** Convert IPs to binary for tricky LPM questions. Match bit-by-bit with each prefix. The entry with most matching prefix bits wins.

---

## Module 2: Data Link Layer — Part 1

### 2.1 OSI Reference Model

```
Layer 7: Application    — HTTP, DNS, SMTP, FTP
Layer 6: Presentation   — Encryption, Compression, Translation
Layer 5: Session        — Session management, Synchronization
Layer 4: Transport      — TCP, UDP (segments)
Layer 3: Network        — IP, ICMP, Routing (packets/datagrams)
Layer 2: Data Link      — Framing, Error Control, MAC (frames)
Layer 1: Physical       — Bits on wire, signaling, modulation
```

### 2.2 TCP/IP Model

| TCP/IP Layer | OSI Equivalent | PDU | Protocols |
|---|---|---|---|
| Application | 5, 6, 7 | Data/Message | HTTP, DNS, FTP, SMTP |
| Transport | 4 | Segment | TCP, UDP |
| Internet | 3 | Datagram/Packet | IP, ICMP, ARP |
| Network Access | 1, 2 | Frame | Ethernet, Wi-Fi |

### 2.3 Packet Switching vs Circuit Switching

| Feature | Circuit Switching | Packet Switching |
|---|---|---|
| Path setup | Dedicated path established before data | No dedicated path |
| Resource allocation | Reserved for entire duration | Shared dynamically |
| Delay | Setup delay + constant transmission | Variable delay (queuing) |
| Bandwidth usage | Wasteful during silence | Efficient (statistical multiplexing) |
| Reliability | Guaranteed once connected | Best effort |
| Example | Traditional telephone | Internet |

**Key Formula (Circuit Switching):**
- Max simultaneous users = Total bandwidth / Per-user bandwidth

**Key Formula (Packet Switching):**
- For $N$ users each active $p$ fraction of time, probability $> k$ users active simultaneously:
$$P(X > k) = \sum_{i=k+1}^{N} \binom{N}{i} p^i (1-p)^{N-i}$$

### 2.4 Framing

**Byte Stuffing (Character Stuffing):**
- Flag byte = delimiter (e.g., `01111110` or ASCII ESC)
- If flag or ESC appears in data, insert an extra ESC before it
- Receiver removes the stuffed escape character

**Bit Stuffing:**
- Flag = `01111110`
- After every 5 consecutive 1s in data, sender inserts a 0
- Receiver removes the 0 after 5 consecutive 1s

**Example 1:** Data = `011111110` → After bit stuffing = `0111110110`

**Worked Example — Bit Stuffing (GATE Style):**

Data: `01111111111011111101`

Step-by-step:
```
Input:  0 1 1 1 1 1 | 1 1 1 1 1 | 0 1 1 1 1 1 | 1 0 1
                   ↑insert 0      ↑insert 0
Output: 0 1 1 1 1 1 0 1 1 1 1 1 0 0 1 1 1 1 1 0 1 0 1
```
Original = 20 bits → Stuffed = 22 bits → Added 2 stuff bits

> 🎯 **GATE Trick:** Count groups of 5+ consecutive 1s. Each group needs one stuff bit. Number of stuff bits = number of such groups.

**Worked Example — Byte Stuffing:**

Flag = F, Escape = E. Data = `A E B F C E E D`
```
Stuffed: F | A E E B E F C E E E E D | F
              ↑E→EE    ↑F→EF     ↑E→EE
```

**Overhead of bit stuffing:** Worst case: all 1s → 1 stuff bit per 5 data bits → overhead = 20%

### 2.5 Error Detection & Correction

#### 2.5.1 Single Bit Parity

- **Even parity:** Total number of 1s (including parity bit) is even
- **Odd parity:** Total number of 1s is odd
- Detects **odd** number of bit errors; misses **even** number of errors

#### 2.5.2 Hamming Distance

- **Hamming distance** $d(x,y)$ = number of positions where $x$ and $y$ differ
- $d_{\min}$ = minimum Hamming distance of a code

| Capability | Required $d_{\min}$ |
|---|---|
| Detect up to $t$ errors | $d_{\min} \geq t + 1$ |
| Correct up to $t$ errors | $d_{\min} \geq 2t + 1$ |
| Detect $d$ errors AND correct $c$ errors ($c < d$) | $d_{\min} \geq d + c + 1$ |

#### 2.5.3 Hamming Code (Single Error Correction)

- For $m$ data bits, need $r$ parity bits where $2^r \geq m + r + 1$
- Parity bits are at positions $2^0, 2^1, 2^2, \ldots$ (positions 1, 2, 4, 8, ...)
- Each parity bit covers positions where its bit position is set in binary representation

| Parity Bit | Position | Covers Positions (binary check) |
|---|---|---|
| $P_1$ | 1 | 1, 3, 5, 7, 9, 11, ... |
| $P_2$ | 2 | 2, 3, 6, 7, 10, 11, ... |
| $P_4$ | 4 | 4, 5, 6, 7, 12, 13, ... |
| $P_8$ | 8 | 8, 9, 10, 11, 12, 13, ... |

**Minimum parity bits for $m$ data bits:**

| Data Bits ($m$) | Parity Bits ($r$) | Total Code Length |
|---|---|---|
| 1 | 2 | 3 |
| 4 | 3 | 7 |
| 8 | 4 | 12 |
| 11 | 4 | 15 |
| 16 | 5 | 21 |
| 26 | 5 | 31 |
| 57 | 6 | 63 |

**Error Detection:** Compute syndrome (XOR of parity checks). Syndrome = 0 → No error. Syndrome ≠ 0 → syndrome gives position of error (flip that bit).

**Worked Example — Hamming Code Encoding (7,4):**

Data bits: $D = 1011$ (4 data bits → need $r=3$ parity bits → code length = 7)

**Step 1:** Place data bits at non-power-of-2 positions:

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|
| Type | P₁ | P₂ | D₁ | P₄ | D₂ | D₃ | D₄ |
| Value | ? | ? | **1** | ? | **0** | **1** | **1** |

**Step 2:** Calculate parity bits (even parity):
- P₁ (pos 1): covers positions with bit 0 set in binary (1,3,5,7) → P₁ ⊕ 1 ⊕ 0 ⊕ 1 = 0 → **P₁ = 0**
- P₂ (pos 2): covers positions with bit 1 set (2,3,6,7) → P₂ ⊕ 1 ⊕ 1 ⊕ 1 = 0 → **P₂ = 1**
- P₄ (pos 4): covers positions with bit 2 set (4,5,6,7) → P₄ ⊕ 0 ⊕ 1 ⊕ 1 = 0 → **P₄ = 0**

**Step 3:** Final codeword: **0 1 1 0 0 1 1**

**Worked Example — Hamming Code Error Detection:**

Received: `0 1 1 0 1 1 1` (original was `0 1 1 0 0 1 1`)

Compute syndrome:
- S₁ = P₁ ⊕ D₁ ⊕ D₂ ⊕ D₄ = 0 ⊕ 1 ⊕ 1 ⊕ 1 = **1**
- S₂ = P₂ ⊕ D₁ ⊕ D₃ ⊕ D₄ = 1 ⊕ 1 ⊕ 1 ⊕ 1 = **0**
- S₃ = P₄ ⊕ D₂ ⊕ D₃ ⊕ D₄ = 0 ⊕ 1 ⊕ 1 ⊕ 1 = **1**

Syndrome = S₃S₂S₁ = **101** = 5 → Error at **position 5**!
Flip bit 5: `0 1 1 0 **0** 1 1` → Corrected! ✓

**Worked Example — How many parity bits for 32 data bits?**
$2^r \geq 32 + r + 1$
- r=5: 32 ≥ 38? No
- r=6: 64 ≥ 39? **Yes** → Need **6 parity bits**, total code = 38 bits

> 🎯 **GATE Trick:** For Hamming code, the redundancy ratio = $r/(m+r)$. For large $m$, this approaches 0 — very efficient!

#### 2.5.4 Cyclic Redundancy Check (CRC)

**Steps:**
1. Given data $D$ of $d$ bits and generator polynomial $G$ of degree $r$ ($r+1$ bits)
2. Append $r$ zero bits to data: $D \cdot 2^r$
3. Divide $D \cdot 2^r$ by $G$ using XOR division (modulo-2)
4. Remainder $R$ (of $r$ bits) is the CRC
5. Transmitted codeword = $D \cdot 2^r + R$ (equivalently: $D$ followed by $R$)

**Properties of CRC:**
- **CRC detects all single-bit errors** if $G$ has more than one term
- **Detects all double-bit errors** if $G$ has a factor with ≥ 3 terms
- **Detects all odd number of errors** if $G$ has $(x+1)$ as a factor
- **Detects all burst errors of length ≤ $r$**

**Common Generator Polynomials:**

| Standard | Polynomial | Degree |
|---|---|---|
| CRC-8 | $x^8 + x^2 + x + 1$ | 8 |
| CRC-16 | $x^{16} + x^{15} + x^2 + 1$ | 16 |
| CRC-32 | $x^{32} + x^{26} + x^{23} + \cdots + 1$ | 32 |

**Worked Example:** Data = 1101, Generator = 1011 ($x^3 + x + 1$, degree 3)

1. Append 3 zeros: 1101000
2. XOR division by 1011:
   ```
   1101000 ÷ 1011
   1011
   ------
    1100
    1011
    -----
     1110
     1011
     -----
      1010
      1011
      -----
       001  ← Remainder = 001
   ```
3. Transmitted: 1101**001**

**Worked Example 2 — CRC Full Trace:**
Data = 10011101, Generator = 1001 (degree 3)

1. Append 3 zeros: 10011101**000**
2. XOR division:
```
   10011101000
   1001
   ----
    0001
    0000
    ----
     0011
     0000
     ----
      0111
      0000
      ----
       1110
       1001
       ----
        1111
        1001
        ----
         1100
         1001
         ----
          1010
          1001
          ----
           0110
           0000
           ----
            110  ← Remainder
```
3. CRC = 110. Transmitted: 10011101**110**

**Verification at receiver:** Divide 10011101110 by 1001 → remainder should be 000

**Worked Example 3 — CRC GATE Problem:**
Generator polynomial: $x^3 + 1$ = **1001**. Data = 1010000.
- Append 3 zeros: 1010000**000**
- Divide by 1001:
```
  1010000000
  1001
  ----
   0110
   0000
   ----
    1100
    1001
    ----
     1010
     1001
     ----
      0110
      0000
      ----
       1100
       1001
       ----
        101  ← CRC
```
- Transmitted: 1010000**101**

> 🎯 **GATE Trick:** In CRC, the degree of generator polynomial = number of CRC bits = number of zeros appended. Generator of degree $r$ catches ALL burst errors of length ≤ $r$.

#### 2.5.5 Checksum

**Internet Checksum (used in IP, TCP, UDP):**
1. Divide data into 16-bit words
2. Add all words using 1's complement addition (end-around carry)
3. Take 1's complement of the sum → Checksum
4. Receiver adds all words including checksum → result should be all 1s (0xFFFF)

### 2.6 Delays in Computer Networks

$$d_{\text{total}} = d_{\text{prop}} + d_{\text{trans}} + d_{\text{queue}} + d_{\text{proc}}$$

| Delay | Formula | Description |
|---|---|---|
| **Propagation** | $d_{\text{prop}} = \dfrac{\text{Distance}}{\text{Propagation speed}}$ | Time for bit to travel from sender to receiver |
| **Transmission** | $d_{\text{trans}} = \dfrac{\text{Packet size (L)}}{\text{Bandwidth (B)}}$ | Time to push all bits onto the link |
| **Queuing** | Variable | Time spent waiting in router buffers |
| **Processing** | Variable | Time to examine header, determine route |

**Important Notes:**
- Propagation speed in copper ≈ $2 \times 10^8$ m/s
- Propagation speed in fiber ≈ $2 \times 10^8$ m/s (unless stated otherwise)
- Propagation speed in vacuum/air ≈ $3 \times 10^8$ m/s
- RTT (Round Trip Time) = $2 \times d_{\text{prop}}$ (ignoring transmission delay of ACK)

**Store-and-Forward Delay:**
- For a packet of size $L$ traversing $k$ links (each bandwidth $B$) with store-and-forward:
$$d_{\text{end-to-end}} = k \cdot \frac{L}{B} + (k-1) \cdot \frac{L}{B_{\text{intermediate}}} + d_{\text{prop-total}}$$
- Simplified (same bandwidth): $d = k \cdot \frac{L}{B} + d_{\text{prop}}$

**Cut-Through Switching:** Forward after reading only the header → Delay = $\frac{L}{B} + (k-1)\frac{H}{B} + d_{\text{prop}}$ where $H$ = header size.

**Worked Example 1 — Basic Delay Calculation:**
A packet of size 1000 bytes is sent over a link with bandwidth 1 Mbps and length 2000 km. Propagation speed = $2 \times 10^8$ m/s. Find total delay.

$T_{trans} = \frac{1000 \times 8}{10^6} = 8 \text{ ms}$

$T_{prop} = \frac{2000 \times 10^3}{2 \times 10^8} = 10 \text{ ms}$

$T_{total} = 8 + 10 = 18 \text{ ms}$

**Worked Example 2 — Store-and-Forward through multiple routers:**
Packet size = 1500 bytes, 3 links, each 100 Mbps, total propagation delay = 6 ms.

With store-and-forward, 3 links → packet must be fully received at each hop:
$T = 3 \times \frac{1500 \times 8}{100 \times 10^6} + 6 \text{ ms} = 3 \times 0.12 + 6 = 6.36 \text{ ms}$

**Worked Example 3 — Multiple Packets through a Pipeline:**
Send 5 packets of 1000 bits each through 3 links, each 1 Mbps. Propagation delay per link = 2 ms.

First packet arrives at destination after: $3 \times T_{trans} + 3 \times T_{prop} = 3 + 6 = 9$ ms
Remaining 4 packets arrive 1 $T_{trans}$ apart (pipeline effect):
$T_{total} = (3 + 5 - 1) \times T_{trans} + 3 \times T_{prop} = 7 \times 1 + 6 = 13$ ms

> 🔑 **General formula for $P$ packets over $k$ links:** $T = (k + P - 1) \times T_{trans} + k \times T_{prop}$

**Worked Example 4 — Finding the value of 'a':**
Link: 10 Mbps, 2000 km, propagation speed = $2 \times 10^8$ m/s. Frame size = 500 bytes.

$T_{trans} = \frac{500 \times 8}{10 \times 10^6} = 0.4$ ms

$T_{prop} = \frac{2000 \times 10^3}{2 \times 10^8} = 10$ ms

$a = \frac{T_{prop}}{T_{trans}} = \frac{10}{0.4} = 25$

This means: propagation delay is 25x the transmission delay → protocol efficiency will be very low for Stop-and-Wait!

**Worked Example 5 — Bandwidth-Delay Product (BDP):**

BDP = Bandwidth × RTT = amount of data "in flight" at any time

For 1 Gbps link with 40 ms RTT:
$BDP = 10^9 \times 0.04 = 40 \times 10^6 \text{ bits} = 5 \text{ MB}$

This is the receiver buffer size needed and approximate TCP window needed for full utilization.

> 🎯 **GATE Trick:** "Bandwidth-delay product" tells you how many bits can be in the pipe at once. If window < BDP, link is underutilized!

### 2.7 Sliding Window Protocols

#### 2.7.1 Stop-and-Wait ARQ

- Sender sends **one frame**, waits for ACK before sending next
- Window size: Sender = 1, Receiver = 1
- Sequence number bits needed: **1 bit** (0, 1)

**Efficiency:**
$$\eta = \frac{T_{\text{trans}}}{T_{\text{trans}} + 2 \times T_{\text{prop}} + T_{\text{ACK}}} = \frac{1}{1 + 2a}$$

where $a = \frac{T_{\text{prop}}}{T_{\text{trans}}}$ (assuming $T_{\text{ACK}} \approx 0$)

**Throughput:**
$$\text{Throughput} = \eta \times B = \frac{L}{T_{\text{trans}} + 2T_{\text{prop}}}$$

**With errors (frame error probability $p$):**
$$\eta = \frac{(1-p)}{1 + 2a}$$

#### 2.7.2 Go-Back-N (GBN)

- Sender window size = $N$ (where $N \leq 2^n - 1$, $n$ = sequence number bits)
- Receiver window size = **1**
- Receiver discards out-of-order frames → cumulative ACK
- On timeout, sender retransmits **all** frames from the lost one

**Efficiency (no errors):**
$$\eta = \begin{cases} 1 & \text{if } N \geq 1 + 2a \\ \frac{N}{1 + 2a} & \text{if } N < 1 + 2a \end{cases}$$

**Efficiency (with error probability $p$):**
$$\eta = \frac{N(1-p)}{(1+2a)(1-p+Np)}$$

**Key Constraints:**
- Sender window: $W_s \leq 2^n - 1$
- Receiver window: $W_r = 1$
- Sequence number range: $0$ to $2^n - 1$

#### 2.7.3 Selective Repeat (SR)

- Sender window size = Receiver window size = $N$
- Receiver buffers out-of-order frames
- Only the lost/damaged frame is retransmitted → individual ACK
- On timeout, sender retransmits **only** the specific lost frame

**Efficiency (no errors):**
$$\eta = \begin{cases} 1 & \text{if } N \geq 1 + 2a \\ \frac{N}{1 + 2a} & \text{if } N < 1 + 2a \end{cases}$$

**Efficiency (with error probability $p$):**
$$\eta = \frac{N(1-p)}{(1+2a)}$$

**Key Constraints:**
- $W_s + W_r \leq 2^n$
- Typically $W_s = W_r = 2^{n-1}$

#### 2.7.4 Protocol Comparison Table

| Feature | Stop & Wait | Go-Back-N | Selective Repeat |
|---|---|---|---|
| Sender Window | 1 | $\leq 2^n - 1$ | $\leq 2^{n-1}$ |
| Receiver Window | 1 | 1 | $\leq 2^{n-1}$ |
| Seq. No. Bits ($n$) | 1 | $\lceil \log_2(W_s + 1) \rceil$ | $\lceil \log_2(2W_s) \rceil$ |
| Retransmission | Only lost frame | All from lost frame | Only lost frame |
| ACK Type | Individual | Cumulative | Individual |
| Receiver Complexity | Low | Low | High (buffering) |
| Bandwidth Efficiency | Low | Moderate | High |
| Min Seq Numbers for $W$ | $W + 1 = 2$ | $W + 1$ | $2W$ |

**Piggybacking:** ACK is sent along with data frame traveling in opposite direction → reduces overhead.

### 2.8 Sliding Window — Extensive Worked Problems

**Problem 1 — Stop-and-Wait Efficiency:**
Frame size = 1000 bits, Bandwidth = 1 Mbps, Distance = 5000 km, Propagation speed = $2.5 \times 10^8$ m/s.

$T_{trans} = \frac{1000}{10^6} = 1$ ms
$T_{prop} = \frac{5000 \times 10^3}{2.5 \times 10^8} = 20$ ms
$a = 20/1 = 20$

$\eta_{SW} = \frac{1}{1 + 2a} = \frac{1}{1 + 40} = \frac{1}{41} = 2.44\%$

Throughput = $0.0244 \times 1 = 0.0244$ Mbps = 24.4 kbps → **terrible!**

**Problem 2 — Go-Back-N Window Size Needed:**
Using the same parameters as Problem 1. What sender window size achieves 100% efficiency?

Need $N \geq 1 + 2a = 1 + 40 = 41$

With 6 bits for sequence numbers: max window = $2^6 - 1 = 63 \geq 41$ ✓ (achievable)
With 5 bits: max window = $2^5 - 1 = 31 < 41$ ✗ (not enough)

∴ Need ≥ 6 sequence number bits for GBN to achieve 100% efficiency.

**Problem 3 — Selective Repeat Required:**
Same parameters. With only 5 sequence number bits, what's max SR efficiency?

SR: $W_s \leq 2^{n-1} = 2^4 = 16$

$\eta_{SR} = \frac{16}{41} = 39.02\%$

**Problem 4 — GBN with Errors:**
GBN, window = 7, $a = 3$, error probability $p = 0.1$

$\eta_{GBN} = \frac{N(1-p)}{(1+2a)(1-p+Np)} = \frac{7 \times 0.9}{7 \times (0.9 + 0.7)} = \frac{6.3}{7 \times 1.6} = \frac{6.3}{11.2} = 56.25\%$

**Problem 5 — SR with Errors (same params):**
$\eta_{SR} = \frac{N(1-p)}{1+2a} = \frac{7 \times 0.9}{7} = 90\%$

$\eta_{SR} > \eta_{GBN}$ because SR retransmits only the lost frame!

**Problem 6 — Minimum Sequence Numbers (GATE Favorite!):**

A protocol uses 3-bit sequence numbers: $2^3 = 8$ possible values (0–7)

| Protocol | Max Sender Window | Max Receiver Window |
|---|---|---|
| Stop-and-Wait | 1 | 1 |
| GBN | $2^3 - 1 = 7$ | 1 |
| SR | $2^2 = 4$ | 4 |

> ⚠️ **Why can't GBN use $W_s = 2^n$?** If sender sends frames 0..7 and all ACKs are lost, sender retransmits 0..7. Receiver (expecting frame 0 of NEXT cycle) accepts frame 0 as new data → **duplicate accepted as new!**

> ⚠️ **Why must SR have $W_s + W_r \leq 2^n$?** Similar problem: if $W_s = W_r = 5$ with 3-bit seq (0..7), receiver can't distinguish old frame 0 from new frame 0 → corruption!

**Problem 7 — Throughput Calculation:**
Link: 10 Mbps, RTT = 40 ms. Frame = 5000 bits. GBN with window = 127.

$T_{trans} = 5000/10^7 = 0.5$ ms
$a = 20/0.5 = 40$ (RTT/2 = 20 ms)
$1 + 2a = 81$
$N = 127 > 81$ → $\eta = 1$ → Full utilization!
Throughput = 10 Mbps

If changed to SR with $W_s = 64$: $64 < 81$ → $\eta = 64/81 = 79.01\%$
Throughput = $0.79 \times 10 = 7.9$ Mbps

**Problem 8 — Time to send a file:**
File size = 1 MB = $8 \times 10^6$ bits. Link = 1 Mbps, RTT = 100 ms. Frame = 1000 bits. Stop-and-Wait.

$a = 50/1 = 50$, $\eta = 1/101 \approx 0.99\%$
Effective throughput = $0.0099 \times 1 = 9.9$ kbps
Time = $8 \times 10^6 / 9900 \approx 808$ seconds ≈ 13.5 minutes!

With GBN, window = 127: $\eta = 127/101 > 1$ → $\eta = 1$
Time = $8 \times 10^6 / 10^6 = 8$ seconds (85x faster!)

### 2.9 Sliding Window — Timeline Diagram

**Stop-and-Wait Timeline:**
```
Sender                              Receiver
  |                                    |
  |------ Frame 0 --------->          |
  |                         (process) |
  |          <--------- ACK 0 --------|
  |------ Frame 1 --------->          |
  |                         (process) |
  |          <--------- ACK 1 --------|
  |------ Frame 0 --------->          |
  ...
  
  [Idle time between frames = 2×Tprop]
```

**GBN with Error — Timeline:**
```
Sender                              Receiver
  |-- F0 --→  ✓                       |
  |-- F1 --→  ✓                       |
  |-- F2 --→  ✗ (lost!)              |
  |-- F3 --→  ✗ (discarded, expects F2)|
  |-- F4 --→  ✗ (discarded)          |
  |      ←-- ACK0 --|                 |
  |      ←-- ACK1 --|                 |
  | (timeout for F2!)                 |
  |-- F2 --→  ✓   ← retransmit from F2
  |-- F3 --→  ✓                       |
  |-- F4 --→  ✓                       |
```

**SR with Error — Timeline:**
```
Sender                              Receiver
  |-- F0 --→  ✓                       |
  |-- F1 --→  ✓                       |
  |-- F2 --→  ✗ (lost!)              |
  |-- F3 --→  ✓ (buffered, NAK/timeout)|
  |-- F4 --→  ✓ (buffered)           |
  |      ←-- ACK0 --|                 |
  |      ←-- ACK1 --|                 |
  |      ←-- NAK2 --|                 |
  |-- F2 --→  ✓  ← retransmit ONLY F2
  |      (receiver delivers F2,F3,F4) |
```

---

## Module 3: Data Link Layer — Part 2

### 3.1 Multiple Access (MAC) Protocols

#### 3.1.1 Pure ALOHA

- Station transmits whenever it has data
- If collision → wait random time, retransmit
- Vulnerable period = $2 \times T_{\text{frame}}$

$$S = G \cdot e^{-2G}$$

- Maximum throughput: $S_{\max} = \frac{1}{2e} \approx 18.4\%$ at $G = 0.5$

#### 3.1.2 Slotted ALOHA

- Time divided into equal slots = frame transmission time
- Transmit only at beginning of a slot
- Vulnerable period = $T_{\text{frame}}$

$$S = G \cdot e^{-G}$$

- Maximum throughput: $S_{\max} = \frac{1}{e} \approx 36.8\%$ at $G = 1$

#### 3.1.3 ALOHA Comparison & Worked Problems

| Feature | Pure ALOHA | Slotted ALOHA |
|---|---|---|
| Vulnerable period | $2T_{frame}$ | $T_{frame}$ |
| Max throughput | $\frac{1}{2e} = 18.4\%$ | $\frac{1}{e} = 36.8\%$ |
| Optimal load $G$ | 0.5 | 1.0 |
| Synchronization | Not required | Required (clock) |
| Throughput formula | $Ge^{-2G}$ | $Ge^{-G}$ |

**Worked Example 1 — Pure ALOHA Throughput:**
1000 stations, each transmitting 1 frame/minute. Frame time = 1 ms.

$G = 1000 \times \frac{1}{60000} = \frac{1}{60} \approx 0.0167$

$S = 0.0167 \times e^{-0.0333} = 0.0167 \times 0.9672 = 0.0161$ → 1.61% throughput

**Worked Example 2 — Slotted ALOHA: Expected attempts:**
If $G = 1$, what's the average number of attempts before a successful transmission?

$P(\text{success}) = e^{-G} = e^{-1} \approx 0.368$

Expected attempts = $\frac{1}{P} = e \approx 2.72$ attempts

**Worked Example 3 — GATE Problem:**
In Pure ALOHA, 500 stations share a 200 kbps channel. If each station generates 15 frames/second and frame size is 200 bits, find throughput.

$T_{frame} = 200/200000 = 0.001$ s = 1 ms
$G = 500 \times 15 \times 0.001 = 7.5$
$S = 7.5 \times e^{-15} = 7.5 \times 3.06 \times 10^{-7} \approx 0$ → Almost zero throughput! (massively overloaded)

> 🎯 **GATE Trick:** If $G >> 1$, the network is overloaded and throughput ≈ 0. If asked "what happens when load doubles beyond optimal," throughput actually DECREASES (instability).

#### 3.1.4 CSMA (Carrier Sense Multiple Access)

- **1-persistent:** If channel idle → transmit immediately; if busy → wait and transmit as soon as idle (greedy)
- **Non-persistent:** If busy → wait random time, then sense again
- **p-persistent:** If idle → transmit with probability $p$, defer with probability $(1-p)$

#### 3.1.4 CSMA/CD (Collision Detection)

Used in **Ethernet (802.3)**. Station listens while transmitting.

**Minimum Frame Size Constraint:**
$$T_{\text{frame}} \geq 2 \times T_{\text{prop}} = RTT$$
$$L_{\min} = 2 \times T_{\text{prop}} \times B$$

For standard Ethernet (10 Mbps, 2500m max):
- $L_{\min} = 64$ bytes = 512 bits

**Efficiency of CSMA/CD:**
$$\eta = \frac{1}{1 + 6.44a}$$
where $a = \frac{T_{\text{prop}}}{T_{\text{trans}}}$

**Binary Exponential Backoff:**
- After $k$-th collision, choose random $R \in \{0, 1, 2, \ldots, 2^k - 1\}$
- Wait time = $R \times$ slot time (slot time = $2 \times T_{\text{prop}}$)
- $k$ is capped at 10 (max attempts = 16, then give up)

**Worked Example 1 — Minimum Frame Size:**
Ethernet: bandwidth = 10 Mbps, max distance = 2500m, propagation speed = $2 \times 10^8$ m/s.

$T_{prop} = 2500 / (2 \times 10^8) = 12.5 \mu s$
$RTT = 2 \times 12.5 = 25 \mu s$
$L_{min} = RTT \times B = 25 \times 10^{-6} \times 10 \times 10^6 = 250$ bits

(In practice, Ethernet uses 512 bits = 64 bytes due to additional delays)

**Worked Example 2 — GATE: Maximum cable length:**
Fast Ethernet (100 Mbps), minimum frame = 64 bytes. Find max cable length. Propagation speed = $2 \times 10^8$ m/s.

$T_{frame} = 64 \times 8 / (100 \times 10^6) = 5.12 \mu s$
$T_{prop,max} = T_{frame}/2 = 2.56 \mu s$
$d_{max} = 2.56 \times 10^{-6} \times 2 \times 10^8 = 512$ m

**Worked Example 3 — CSMA/CD Efficiency:**
Frame = 10000 bits, link = 10 Mbps, distance = 2500m, prop speed = $2 \times 10^8$ m/s.

$T_{trans} = 10000/10^7 = 1$ ms
$T_{prop} = 2500/(2 \times 10^8) = 12.5 \mu s$
$a = 12.5/1000 = 0.0125$
$\eta = 1/(1 + 6.44 \times 0.0125) = 1/1.0805 = 92.5\%$

> 🎯 **GATE Trick:** For CSMA/CD, efficiency depends on the ratio $a = T_{prop}/T_{trans}$. Small frame on long link → poor efficiency. Large frame on short link → great efficiency!

### 3.2 Ethernet Frame Format

```
┌─────────┬────────┬──────┬──────┬──────────────┬──────┬─────┐
│Preamble │  SFD   │ Dest │ Src  │ Type/Length   │ Data │ CRC │
│ 7 bytes │1 byte  │ MAC  │ MAC  │  2 bytes      │      │ 4B  │
│         │        │ 6B   │ 6B   │              │46-1500│     │
└─────────┴────────┴──────┴──────┴──────────────┴──────┴─────┘
```

| Field | Size | Purpose |
|---|---|---|
| Preamble | 7 bytes | Synchronization (10101010...) |
| SFD | 1 byte | Start Frame Delimiter (10101011) |
| Dest MAC | 6 bytes | Destination MAC address |
| Src MAC | 6 bytes | Source MAC address |
| Type/Length | 2 bytes | ≤1500 = length; >1536 = type (EtherType) |
| Data + Padding | 46–1500 bytes | Payload (padded to min 46 bytes) |
| CRC (FCS) | 4 bytes | CRC-32 error detection |

**Min frame size** = 6+6+2+46+4 = **64 bytes** (excluding preamble/SFD)
**Max frame size** = 6+6+2+1500+4 = **1518 bytes**

**Ethernet Types:**

| Standard | Speed | Cable | Max Distance |
|---|---|---|---|
| 10Base-T | 10 Mbps | Twisted Pair | 100m |
| 100Base-TX | 100 Mbps | Cat 5 | 100m |
| 1000Base-T | 1 Gbps | Cat 5e/6 | 100m |
| 10GBase-T | 10 Gbps | Cat 6a | 100m |
| 1000Base-SX | 1 Gbps | Multimode Fiber | 550m |
| 1000Base-LX | 1 Gbps | Single-mode Fiber | 5000m |

### 3.3 VLANs (Virtual LANs)

- Logically segment a physical LAN into multiple broadcast domains
- Configured at switch level (port-based or tag-based)
- Uses IEEE 802.1Q tag (4 bytes added to Ethernet frame)

**802.1Q VLAN Tag:**
```
┌──────────┬─────────┬─────────┬──────────┐
│ TPID     │ Priority│ CFI     │ VLAN ID  │
│ 0x8100   │ 3 bits  │ 1 bit   │ 12 bits  │
│ (2 bytes)│         │         │          │
└──────────┴─────────┴─────────┴──────────┘
```

- VLAN ID: 12 bits → 4096 possible VLANs (0 and 4095 reserved → 4094 usable)
- **Trunk port:** Carries frames from multiple VLANs (tagged)
- **Access port:** Belongs to one VLAN (untagged)

**Inter-VLAN Routing:** Devices in different VLANs need a Layer 3 device (router or L3 switch) to communicate.

> 🎯 **GATE Trick:** VLANs reduce broadcast domain size. Without VLANs, one switch = one broadcast domain. With VLANs, one switch = multiple broadcast domains.

#### 3.1.5 CSMA/CA (Collision Avoidance)

Used in **Wi-Fi (802.11)**:
1. Sense channel → if idle for DIFS, transmit
2. If busy → backoff with random timer
3. Use **RTS/CTS** to reserve channel (optional)
4. Receiver sends ACK after SIFS

### 3.2 ARP (Address Resolution Protocol)

- Maps **IP address → MAC address** within a LAN
- ARP Request: Broadcast (FF:FF:FF:FF:FF:FF)
- ARP Reply: Unicast
- ARP Cache: Stores mappings with TTL (typically 20 min)
- **Reverse ARP (RARP):** MAC → IP (obsolete, replaced by BOOTP/DHCP)

### 3.3 DHCP (Dynamic Host Configuration Protocol)

Assigns IP addresses dynamically. Uses **UDP** (Server: port 67, Client: port 68).

**DORA Process:**

| Step | Message | Src → Dst | Cast |
|---|---|---|---|
| 1 | **D**iscover | 0.0.0.0 → 255.255.255.255 | Broadcast |
| 2 | **O**ffer | Server → 255.255.255.255 | Broadcast |
| 3 | **R**equest | 0.0.0.0 → 255.255.255.255 | Broadcast |
| 4 | **A**ck | Server → 255.255.255.255 | Broadcast |

### 3.4 Spanning Tree Protocol (STP)

- Prevents **loops** in switched networks (IEEE 802.1D)
- Builds a loop-free logical tree from a physical mesh
- **Root Bridge:** Switch with lowest Bridge ID (priority + MAC)
- **Root Port:** Port on non-root switch closest to root bridge
- **Designated Port:** One port per segment, closest to root
- **Blocked Port:** Neither root nor designated → no forwarding

**Algorithm Steps:**
1. Elect root bridge (lowest ID)
2. Each non-root switch selects root port (lowest cost to root)
3. Each segment selects designated port (lowest cost to root from that segment)
4. All other ports are blocked

### 3.5 Networking Devices

| Device | Layer | Collision Domain | Broadcast Domain | Intelligence |
|---|---|---|---|---|
| **Hub** | Physical (L1) | Single (shared) | Single | None (repeats all) |
| **Switch** | Data Link (L2) | Per port (separate) | Single | MAC address table |
| **Bridge** | Data Link (L2) | Per port | Single | MAC learning |
| **Router** | Network (L3) | Per port | Per port (separate) | Routing table |

**Collision & Broadcast Domains:**
- $N$ devices connected by hub → 1 collision domain, 1 broadcast domain
- $N$ devices connected by switch → $N$ collision domains, 1 broadcast domain
- $N$ devices connected by router → $N$ collision domains, $N$ broadcast domains

---

## Module 4: Network Layer

### 4.1 IPv4 Header

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |    ToS/DSCP   |         Total Length          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|    Fragment Offset      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |       Header Checksum         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if IHL > 5)                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

| Field | Size | Description |
|---|---|---|
| Version | 4 bits | 4 = IPv4 |
| IHL | 4 bits | Header length in 32-bit words (min 5 = 20 bytes) |
| Total Length | 16 bits | Total packet size in bytes (max 65535) |
| Identification | 16 bits | Unique ID for fragmentation/reassembly |
| Flags | 3 bits | Bit 0: Reserved, Bit 1: DF (Don't Fragment), Bit 2: MF (More Fragments) |
| Fragment Offset | 13 bits | Offset in units of **8 bytes** |
| TTL | 8 bits | Decremented by each router; packet dropped at 0 |
| Protocol | 8 bits | 6=TCP, 17=UDP, 1=ICMP |
| Header Checksum | 16 bits | Checksum of header only (recomputed at each hop) |

### 4.2 IP Fragmentation

When a datagram exceeds the MTU (Maximum Transmission Unit) of a link, the router fragments it.

**Key Rules:**
- Only the **source** or **intermediate routers** fragment; **destination** reassembles
- Fragment offset is in units of **8 bytes**
- Each fragment carries the **same Identification** field
- All fragments except last have **MF = 1**
- Maximum data per fragment = $\lfloor \frac{\text{MTU} - \text{Header}}{8} \rfloor \times 8$

**Worked Example:** Datagram = 4000 bytes (Header = 20B, Data = 3980B), MTU = 1500B

- Max data per fragment = $\lfloor \frac{1500 - 20}{8} \rfloor \times 8 = 1480$ bytes
- Number of fragments = $\lceil \frac{3980}{1480} \rceil = 3$

| Fragment | Total Len | Data | MF | Offset (in 8B units) |
|---|---|---|---|---|
| 1 | 1500 | 1480 | 1 | 0 |
| 2 | 1500 | 1480 | 1 | 185 ($= 1480/8$) |
| 3 | 1040 | 1020 | 0 | 370 ($= 2960/8$) |

**Total overhead** = 3 headers × 20 = 60 bytes (original had 20 → 40 bytes extra)

**Worked Example 2 — Fragmentation with Options:**
Datagram = 5000 bytes (Header = 40B with options, Data = 4960B), MTU = 1500B.

Note: All fragments carry options, so header = 40B for all fragments.
Max data per fragment = $\lfloor \frac{1500 - 40}{8} \rfloor \times 8 = \lfloor 182.5 \rfloor \times 8 = 182 \times 8 = 1456$ bytes

Fragments = $\lceil 4960/1456 \rceil = 4$

| Fragment | Total Len | Data | MF | Offset |
|---|---|---|---|---|
| 1 | 1496 | 1456 | 1 | 0 |
| 2 | 1496 | 1456 | 1 | 182 |
| 3 | 1496 | 1456 | 1 | 364 |
| 4 | 632 | 592 | 0 | 546 |

Check: 1456 + 1456 + 1456 + 592 = 4960 ✓

**Worked Example 3 — Fragment of a Fragment:**
Datagram = 2400B (20B header + 2380B data), passes through two links:
- Link 1: MTU = 1500 → fragments into 2 fragments
- Link 2: MTU = 800 → further fragmentation!

**Link 1 (MTU=1500):**
Max data = $\lfloor 1480/8 \rfloor \times 8 = 1480$
| Frag | Data | MF | Offset |
|---|---|---|---|
| A | 1480 | 1 | 0 |
| B | 900 | 0 | 185 |

**Link 2 (MTU=800) — Fragment A splits:**
Max data = $\lfloor 780/8 \rfloor \times 8 = 776$
| Frag | Data | MF | Offset |
|---|---|---|---|
| A1 | 776 | 1 | 0 |
| A2 | 704 | 1 | 97 |

**Link 2 — Fragment B also splits:**
| Frag | Data | MF | Offset |
|---|---|---|---|
| B1 | 776 | 1 | 185 |
| B2 | 124 | 0 | 282 |

Final: 4 fragments arrive at destination (reassembled using same Identification + offsets).

> 🎯 **GATE Trick:** Fragment offset is RELATIVE to original datagram, not to the parent fragment. So when A is re-fragmented, A1 has offset 0 and A2 has offset 776/8 = 97 (same base as A's offset 0).

**Worked Example 4 — GATE: Given fragments, reconstruct original:**

| Frag | ID | MF | Offset | Total Length |
|---|---|---|---|---|
| 1 | 100 | 1 | 0 | 520 |
| 2 | 100 | 1 | 62 | 520 |
| 3 | 100 | 0 | 124 | 420 |

Header = 20B in each. Data: 500, 500, 400.
Offset of last fragment = 124 → 124 × 8 = 992
Last fragment data = 400
Original data = 992 + 400 = 1392 bytes
Original datagram = 1392 + 20 = **1412 bytes**

### 4.3 ICMP (Internet Control Message Protocol)

- Network layer protocol encapsulated in IP (Protocol = 1)
- Used for error reporting and diagnosis (NOT data transfer)

| Type | Message | Usage |
|---|---|---|
| 0 | Echo Reply | `ping` response |
| 3 | Destination Unreachable | Various codes (net/host/port unreachable) |
| 5 | Redirect | Better route available |
| 8 | Echo Request | `ping` request |
| 11 | Time Exceeded | TTL expired (used by `traceroute`) |

**Traceroute:** Sends packets with incrementing TTL (1, 2, 3...). Each router that decrements TTL to 0 sends back ICMP Time Exceeded → reveals path.

### 4.4 NAT (Network Address Translation)

- Translates private IP addresses to public IP addresses
- **NAT Table:** Maps (Private IP, Private Port) ↔ (Public IP, Public Port)
- Types: Static NAT, Dynamic NAT, **PAT/NAPT** (most common — many-to-one using ports)
- Issues: Breaks end-to-end connectivity, problems with some protocols

### 4.5 Distance Vector Routing (Bellman-Ford)

Each router maintains a vector of distances to all destinations and shares it with **neighbors only**.

**Bellman-Ford Equation:**
$$D_x(y) = \min_{v \in \text{neighbors}(x)} \{ c(x,v) + D_v(y) \}$$

**Algorithm:**
1. Initialize: $D_x(y) = c(x,y)$ if neighbor, $\infty$ otherwise
2. When a neighbor sends its DV, update own DV
3. If DV changes, send updated DV to all neighbors
4. Converges to shortest paths

**Count-to-Infinity Problem:**
- When a link cost increases or a node fails, routers may slowly increment distance to infinity
- Solution approaches:
  - **Split Horizon:** Don't advertise route back to neighbor you learned it from
  - **Poison Reverse:** Advertise route to learning neighbor with $\infty$ cost
  - **Triggered Updates:** Send updates immediately when change detected
  - **Hold-down Timer:** Ignore updates for a failed route for some time

**Example of Count-to-Infinity:** Network A—B—C, each link cost 1.
- If link A–B breaks: B thinks "I can reach A via C (cost = C's distance to A + 1 = 2+1=3)"
- C updates: "A via B = 3+1=4". And so on... counts up slowly to infinity.

### 4.6 Link State Routing (Dijkstra's Algorithm)

Each router discovers all routers, measures cost to neighbors, broadcasts link state packets (LSPs) to **all** routers, then computes shortest paths locally.

**Dijkstra's Algorithm:**
1. Start with source node, set its distance = 0, all others = ∞
2. Pick unvisited node $u$ with smallest distance
3. For each neighbor $v$ of $u$: if $d(u) + c(u,v) < d(v)$, update $d(v)$
4. Mark $u$ as visited
5. Repeat until all nodes visited

| Feature | Distance Vector | Link State |
|---|---|---|
| Algorithm | Bellman-Ford | Dijkstra |
| Knowledge | Only neighbors' DVs | Full topology |
| Updates | Periodic, to neighbors | Event-driven, to all (flooding) |
| Convergence | Slow (count-to-infinity) | Fast |
| Example | RIP | OSPF |
| Complexity | $O(V \times E)$ | $O(V^2)$ or $O(V \log V + E)$ |

### 4.7 Dijkstra's Algorithm — Complete Worked Example

**Graph:**
```
        2       3
   A--------B--------C
   |        |        |
  1|       4|       1|
   |        |        |
   D--------E--------F
       5        2
```

**Edge weights:** A-B=2, B-C=3, A-D=1, B-E=4, C-F=1, D-E=5, E-F=2

**Source: A. Find shortest paths to all nodes.**

| Step | Visited | A | B | C | D | E | F | Pick |
|---|---|---|---|---|---|---|---|---|
| Init | {} | **0** | ∞ | ∞ | ∞ | ∞ | ∞ | A(0) |
| 1 | {A} | 0 | **2**(A) | ∞ | **1**(A) | ∞ | ∞ | D(1) |
| 2 | {A,D} | 0 | 2(A) | ∞ | 1 | **6**(D) | ∞ | B(2) |
| 3 | {A,D,B} | 0 | 2 | **5**(B) | 1 | 6(D) | ∞ | C(5) |
| 4 | {A,D,B,C} | 0 | 2 | 5 | 1 | 6(D) | **6**(C) | E(6) or F(6) |
| 5 | {A,D,B,C,E} | 0 | 2 | 5 | 1 | 6 | 6(C) | F(6) |
| 6 | {A,D,B,C,E,F} | 0 | 2 | 5 | 1 | 6 | 6 | Done |

**Shortest paths from A:**
- A→B: cost 2 (direct)
- A→C: cost 5 (A→B→C)
- A→D: cost 1 (direct)
- A→E: cost 6 (A→D→E)
- A→F: cost 6 (A→B→C→F)

### 4.8 Distance Vector — Complete Worked Example

**Same graph.** Show DV convergence.

**Initial (each node knows only direct neighbors):**

| | A | B | C | D | E | F |
|---|---|---|---|---|---|---|
| A | 0 | 2 | ∞ | 1 | ∞ | ∞ |
| B | 2 | 0 | 3 | ∞ | 4 | ∞ |
| C | ∞ | 3 | 0 | ∞ | ∞ | 1 |
| D | 1 | ∞ | ∞ | 0 | 5 | ∞ |
| E | ∞ | 4 | ∞ | 5 | 0 | 2 |
| F | ∞ | ∞ | 1 | ∞ | 2 | 0 |

**After Round 1 (exchange with neighbors):**
A receives DVs from B and D:
- A→C: min(∞, 2+3, 1+∞) = 5 via B
- A→E: min(∞, 2+4, 1+5) = 6 via B or D
- A→F: min(∞, 2+∞, 1+∞) = ∞ (B and D don't know F yet)

**After Round 2:**
- A→F: A gets updated DV from B (who now knows F via E or C)
- A→F = min(previous, 2+B→F) → converges

> After $k$ rounds, each node knows shortest paths using at most $k+1$ hops. Full convergence after at most $|V|-1$ rounds.

### 4.9 NAT — Extended

**NAT Translation Table Example:**

| Inside Local | Inside Global | Outside |
|---|---|---|
| 192.168.1.10:3000 | 203.0.113.1:5000 | 8.8.8.8:53 |
| 192.168.1.20:4000 | 203.0.113.1:5001 | 1.1.1.1:80 |
| 192.168.1.10:3001 | 203.0.113.1:5002 | 142.250.0.1:443 |

**PAT (Port Address Translation):** Maps multiple private IPs to single public IP using different port numbers. With 16-bit port → up to ~65,000 simultaneous connections.

**NAT Issues for GATE:**
- Violates end-to-end principle (modifies packet headers)
- Breaks IPSec (AH checks entire packet including NAT-modified fields)
- NAT traversal needed for incoming connections → port forwarding, STUN, UPnP
- IP addresses in application data (e.g., FTP) not translated → Application Layer Gateway (ALG) needed

---

## Module 5: Transport Layer

### 5.1 UDP (User Datagram Protocol)

- **Connectionless**, unreliable, no flow/congestion control
- Header = **8 bytes** (minimal overhead)

```
 0      7 8     15 16    23 24    31
+--------+--------+--------+--------+
|     Source Port  |   Dest Port     |
+--------+--------+--------+--------+
|     Length       |   Checksum      |
+--------+--------+--------+--------+
```

- Length field = Header (8B) + Data
- Checksum: Optional in IPv4, mandatory in IPv6
- Used by: DNS, DHCP, SNMP, RTP, VoIP

### 5.2 TCP (Transmission Control Protocol)

- **Connection-oriented**, reliable, full-duplex, byte-stream
- Header = **20 bytes** minimum (up to 60 bytes with options)

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Offset|  Res  |U|A|P|R|S|F|         Window Size              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |       Urgent Pointer          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (variable)                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

| Flag | Name | Purpose |
|---|---|---|
| SYN | Synchronize | Initiate connection |
| ACK | Acknowledge | Acknowledge received data |
| FIN | Finish | Terminate connection |
| RST | Reset | Abort connection |
| PSH | Push | Deliver data immediately |
| URG | Urgent | Urgent data present |

### 5.3 TCP Connection Management

**3-Way Handshake (Connection Establishment):**

```
Client                          Server
  |---- SYN (seq=x) ------------>|
  |<--- SYN+ACK (seq=y, ack=x+1)|
  |---- ACK (seq=x+1, ack=y+1)->|
  |       Connection ESTABLISHED  |
```

**4-Way Handshake (Connection Termination):**

```
Client                          Server
  |---- FIN (seq=u) ------------>|
  |<--- ACK (ack=u+1) ----------|
  |       (Server can still send)|
  |<--- FIN (seq=v) ------------|
  |---- ACK (ack=v+1) --------->|
  |       Connection CLOSED       |
```

**TIME_WAIT State:** After sending final ACK, client waits $2 \times$ MSL (Maximum Segment Lifetime) before closing, to handle retransmitted FINs.

### 5.4 TCP Flow Control

- Uses **sliding window** mechanism
- **Receiver Window (rwnd):** Advertised by receiver → amount of free buffer space
- Sender can send: $\min(\text{rwnd}, \text{cwnd})$ bytes
- If rwnd = 0, sender sends **probe segments** (1 byte) to check if window opens

**Silly Window Syndrome:** Avoid sending tiny segments.
- **Nagle's Algorithm (Sender):** Buffer small data; send only when ACK received or buffer full
- **Clark's Solution (Receiver):** Don't advertise tiny windows; wait until buffer has MSS or half-buffer free

### 5.5 TCP Congestion Control

TCP adjusts its sending rate based on perceived network congestion.

**Key Variables:**
- $\text{cwnd}$ = Congestion Window (maintained by sender)
- $\text{ssthresh}$ = Slow Start Threshold
- Effective window = $\min(\text{cwnd}, \text{rwnd})$

#### Phase 1: Slow Start

- Start: $\text{cwnd} = 1$ MSS (or initial window)
- After each ACK: $\text{cwnd} = \text{cwnd} + 1$ MSS
- After each RTT: $\text{cwnd}$ **doubles** (exponential growth)
- Continue until $\text{cwnd} \geq \text{ssthresh}$ → switch to Congestion Avoidance

#### Phase 2: Congestion Avoidance (Additive Increase)

- After each RTT: $\text{cwnd} = \text{cwnd} + 1$ MSS (linear growth)
- Per ACK: $\text{cwnd} = \text{cwnd} + \frac{\text{MSS}^2}{\text{cwnd}}$

#### Events on Packet Loss:

| Event | Action (TCP Tahoe) | Action (TCP Reno) |
|---|---|---|
| **Timeout** | $\text{ssthresh} = \text{cwnd}/2$, $\text{cwnd} = 1$ MSS, Slow Start | Same as Tahoe |
| **3 Duplicate ACKs** | $\text{ssthresh} = \text{cwnd}/2$, $\text{cwnd} = 1$ MSS, Slow Start | $\text{ssthresh} = \text{cwnd}/2$, $\text{cwnd} = \text{ssthresh} + 3$ MSS, **Fast Recovery** |

#### Fast Retransmit & Fast Recovery (TCP Reno)

- **Fast Retransmit:** On receiving 3 duplicate ACKs, retransmit lost segment immediately (don't wait for timeout)
- **Fast Recovery:** After fast retransmit:
  1. $\text{ssthresh} = \text{cwnd}/2$
  2. $\text{cwnd} = \text{ssthresh} + 3$ MSS
  3. For each additional duplicate ACK: $\text{cwnd} = \text{cwnd} + 1$ MSS
  4. When new ACK arrives: $\text{cwnd} = \text{ssthresh}$, enter Congestion Avoidance

**CWND Growth Example (TCP Reno):**

Initial: $\text{cwnd} = 1$ MSS, $\text{ssthresh} = 16$ MSS

| RTT | Phase | cwnd (MSS) |
|---|---|---|
| 1 | Slow Start | 1 → 2 |
| 2 | Slow Start | 2 → 4 |
| 3 | Slow Start | 4 → 8 |
| 4 | Slow Start | 8 → 16 |
| 5 | Congestion Avoidance | 16 → 17 |
| 6 | Congestion Avoidance | 17 → 18 |
| ... | ... | ... |
| 10 | 3 Dup ACKs! | ssthresh = 22/2 = 11, cwnd = 11+3 = 14 (Fast Recovery) |
| 11 | Cong. Avoidance | cwnd = 11 → 12 (after new ACK, cwnd = ssthresh) |

### 5.5.1 TCP Congestion Control — Extensive Worked Problems

**Problem 1 — cwnd Trace (TCP Tahoe, ssthresh=8):**

| RTT | Event | cwnd (before) | cwnd (after) | Phase |
|---|---|---|---|---|
| 1 | - | 1 | 2 | Slow Start |
| 2 | - | 2 | 4 | Slow Start |
| 3 | - | 4 | 8 | Slow Start (reached ssthresh) |
| 4 | - | 8 | 9 | Congestion Avoidance |
| 5 | - | 9 | 10 | Congestion Avoidance |
| 6 | - | 10 | 11 | Congestion Avoidance |
| 7 | - | 11 | 12 | Congestion Avoidance |
| 8 | **Timeout!** | 12 | **1** | ssthresh=6, Slow Start |
| 9 | - | 1 | 2 | Slow Start |
| 10 | - | 2 | 4 | Slow Start |
| 11 | - | 4 | 6 | Slow Start (reached new ssthresh=6) |
| 12 | - | 6 | 7 | Congestion Avoidance |

**Total data sent in RTTs 1–7:** 1+2+4+8+9+10+11 = **45 MSS** = 45 segments

**Problem 2 — TCP Reno with 3 Dup ACKs (ssthresh=32):**

| RTT | Event | cwnd | Phase |
|---|---|---|---|
| 1 | - | 1→2 | Slow Start |
| 2 | - | 2→4 | Slow Start |
| 3 | - | 4→8 | Slow Start |
| 4 | - | 8→16 | Slow Start |
| 5 | - | 16→32 | Slow Start (=ssthresh) |
| 6 | - | 32→33 | Congestion Avoidance |
| 7 | - | 33→34 | Congestion Avoidance |
| 8 | **3 Dup ACKs** | ssthresh=34/2=**17**, cwnd=17+3=**20** | Fast Recovery |
| 9 | New ACK | cwnd=**17** | Congestion Avoidance |
| 10 | - | 17→18 | Congestion Avoidance |
| 11 | - | 18→19 | Congestion Avoidance |

**Key difference:** Tahoe resets cwnd to 1 on 3 dup ACKs. Reno only halves (much faster recovery)!

**Problem 3 — GATE: Total data transmitted:**
TCP Reno, MSS = 1 KB, ssthresh = 64 KB, cwnd starts at 1 MSS. After how many RTTs is 63 KB transmitted?

Slow Start: cwnd doubles each RTT.
RTT 1: send 1 KB (total: 1 KB)
RTT 2: send 2 KB (total: 3 KB)
RTT 3: send 4 KB (total: 7 KB)
RTT 4: send 8 KB (total: 15 KB)
RTT 5: send 16 KB (total: 31 KB)
RTT 6: send 32 KB (total: 63 KB) ✓

**Answer: 6 RTTs**

**Problem 4 — GATE: Effective throughput:**
TCP, MSS = 500 bytes, RTT = 200 ms. Loss detected every 100 segments. Find average throughput.

In one cycle: cwnd grows from W/2 to W, then loss occurs.
Average window ≈ $\frac{3W}{4}$ where $W$ = window at loss.

If loss every 100 segments:
$\sum_{i=W/2}^{W} i \approx 100$ → $W/2 \approx \sqrt{200} \approx 14$ → $W \approx 20$

Average window ≈ 15 MSS = 15 × 500 = 7500 bytes
Throughput ≈ 7500/0.2 = **37,500 bytes/s ≈ 37.5 KB/s**

**TCP Throughput Formula (GATE):**
$$\text{Throughput} \approx \frac{C \times \text{MSS}}{\text{RTT} \times \sqrt{p}}$$

where $p$ = packet loss probability, $C \approx 1.22$

**Problem 5 — GATE: Window size at RTT k:**

Slow start: $\text{cwnd}(k) = 2^k$ MSS (after $k$ RTTs, if starting from 1)
Congestion avoidance: $\text{cwnd}(k) = \text{ssthresh} + (k - k_0)$ where $k_0$ = RTT when CA started

**If ssthresh = 16:** After 4 RTTs in SS: cwnd = 16. Then RTT 5: 17, RTT 6: 18, ... RTT $n$: $16 + (n-4)$

> 🎯 **GATE Trick:** In Slow Start, cwnd = $2^k$ after $k$ RTTs. Data sent in first $k$ RTTs of SS = $2^k - 1$ MSS (geometric sum). In CA, data sent = arithmetic sum.

> 🎯 **GATE Trick (Tahoe vs Reno):** On **timeout**, both behave identically (cwnd=1). On **3 dup ACKs**, only Reno does fast recovery (cwnd = ssthresh + 3). Tahoe treats 3 dup ACKs same as timeout!

### 5.6 TCP Timers

| Timer | Purpose |
|---|---|
| **Retransmission Timer** | Retransmit if ACK not received within RTO |
| **Persistence Timer** | Probe zero-window receiver periodically |
| **Keep-Alive Timer** | Check if connection is still alive (idle detection) |
| **TIME_WAIT Timer** | Wait 2×MSL after connection close |

**RTO Estimation:**
$$\text{EstimatedRTT} = (1 - \alpha) \times \text{EstimatedRTT} + \alpha \times \text{SampleRTT}$$
$$\text{DevRTT} = (1 - \beta) \times \text{DevRTT} + \beta \times |\text{SampleRTT} - \text{EstimatedRTT}|$$
$$\text{RTO} = \text{EstimatedRTT} + 4 \times \text{DevRTT}$$

Typical: $\alpha = 0.125$, $\beta = 0.25$

### 5.7 Traffic Shaping: Leaky & Token Bucket

#### Leaky Bucket

- Output rate is **constant** (= leak rate $r$)
- Bucket size = $b$ bytes (excess is dropped)
- Smooths bursty traffic to constant rate
- **Max output in time $t$:** $r \times t$

#### Token Bucket

- Tokens added at rate $r$ (tokens/sec)
- Bucket capacity = $b$ tokens
- To send 1 byte/packet, consume 1 token
- Allows **bursts** up to bucket capacity

**Max data in time $t$:**
$$M(t) = b + r \times t$$

**Max burst duration at link rate $R$ (where $R > r$):**
$$t_{\text{burst}} = \frac{b}{R - r}$$

| Feature | Leaky Bucket | Token Bucket |
|---|---|---|
| Output rate | Constant = $r$ | Variable, up to link rate |
| Burst handling | No bursts | Allows bursts |
| Max data in $t$ | $r \cdot t$ | $b + r \cdot t$ |
| Tokens | No | Yes |

**Worked Example 1 — Token Bucket:**
Token rate $r$ = 10 MB/s, Bucket capacity $b$ = 5 MB, Link rate $R$ = 25 MB/s.

(a) Max burst size = $b$ = 5 MB

(b) Max burst duration at link rate: $t = \frac{b}{R-r} = \frac{5}{25-10} = \frac{1}{3}$ s

(c) Max data in 1 second: $b + r \times t = 5 + 10 \times 1 = 15$ MB

(d) Max data in 2 seconds: $5 + 10 \times 2 = 25$ MB

**Worked Example 2 — Token Bucket GATE:**
$r$ = 100 Mbps, $b$ = 1 Mb, $R$ = 500 Mbps. How long can host send at 500 Mbps?

$t = \frac{b}{R-r} = \frac{1}{500-100} = \frac{1}{400}$ s = 2.5 ms

Data sent during burst = $R \times t = 500 \times 2.5/1000 = 1.25$ Mb = $b + r \times t = 1 + 0.25 = 1.25$ Mb ✓

**Worked Example 3 — Leaky Bucket:**
Leaky rate = 5 packets/sec, bucket capacity = 10 packets.
Input: 8 packets arrive at t=0, 5 at t=1, 3 at t=2.

| Time | Arrivals | Bucket Before | Leaked | Dropped | Bucket After |
|---|---|---|---|---|---|
| 0 | 8 | 0 | 5 | 0 | 3 |
| 1 | 5 | 3 | 5 | 0 | 3 |
| 2 | 3 | 3 | 5 | 0 | 1 |
| 3 | 0 | 1 | 1 | 0 | 0 |

> 🎯 **GATE Trick:** "Maximum burst size" for token bucket = bucket capacity $b$. "Sustained rate" = token generation rate $r$. The formula $b + rt$ gives maximum data transmitted in interval $t$.

---

## Module 6: Application Layer

### 6.1 HTTP (HyperText Transfer Protocol)

- Uses **TCP** on port **80** (HTTP) or **443** (HTTPS)
- Request-response model

**HTTP Versions:**

| Feature | HTTP/1.0 | HTTP/1.1 |
|---|---|---|
| Connection | Non-persistent (1 request/TCP) | **Persistent** (default) |
| Pipelining | No | Yes |
| Objects per TCP | 1 | Multiple |

**Non-Persistent HTTP:** Each object requires a new TCP connection.
- Time for 1 object = $2 \times RTT + \text{File transmission time}$
- Time for base HTML + $N$ objects (sequential) = $(N+1)(2 \times RTT) + \text{transmission times}$

**Persistent HTTP (without pipelining):**
- 1 TCP connection: $RTT$ (handshake) + $RTT$ per object
- Time = $RTT + (N+1) \times RTT + \text{transmission times}$

**Persistent HTTP (with pipelining):**
- All objects requested in parallel after HTML parsed
- Time ≈ $2 \times RTT + RTT + \text{transmission time}$

**Important Methods:** GET, POST, PUT, DELETE, HEAD

### 6.2 DNS (Domain Name System)

- Maps **domain names ↔ IP addresses**
- Uses both **UDP** (queries, port 53) and **TCP** (zone transfers, port 53)

**DNS Hierarchy:**
```
Root (.)
├── .com    ├── .org    ├── .edu    ├── .in
│   ├── google.com      ├── iitb.ac.in
│   │   ├── www         │   ├── www
│   │   ├── mail        │   ├── cse
```

**Resolution Types:**
- **Recursive:** DNS resolver does all the work, returns final answer
- **Iterative:** Resolver returns referral; client queries next server itself

**DNS Record Types:**

| Type | Maps | Example |
|---|---|---|
| A | Name → IPv4 | `www.example.com → 93.184.216.34` |
| AAAA | Name → IPv6 | `www.example.com → 2606:2800:...` |
| CNAME | Alias → Canonical name | `www → web-server.example.com` |
| MX | Domain → Mail server | `example.com → mail.example.com` |
| NS | Domain → Name server | `example.com → ns1.example.com` |
| PTR | IP → Name (reverse) | `34.216.184.93 → www.example.com` |

**DNS Caching:** Each record has a TTL. Local DNS caches results → reduces query load.

### 6.3 Email Protocols: SMTP, POP3, IMAP

| Protocol | Port | Direction | Description |
|---|---|---|---|
| **SMTP** | 25 | Push (send) | Send mail: Client → Server, Server → Server |
| **POP3** | 110 | Pull (retrieve) | Download mail to client (delete from server or keep) |
| **IMAP** | 143 | Pull (retrieve) | Access mail on server (sync across devices) |

**SMTP:**
- Uses TCP, text-based commands (HELO, MAIL FROM, RCPT TO, DATA, QUIT)
- Push protocol: sender initiates transfer

### 6.4 HTTP Timing — Worked Problems (GATE Favorite!)

**Problem 1 — Non-persistent HTTP:**
A webpage has 1 HTML file + 10 embedded images. RTT = 100 ms. Each file is very small (ignore transmission time).

Non-persistent: each object needs its own TCP connection.
Each connection: 1 RTT (SYN/SYN-ACK) + 1 RTT (GET/Response) = 2 RTT

Total = 2 RTT (for HTML) + 10 × 2 RTT (for 10 images) = **22 RTT = 2.2 seconds**

**Problem 2 — Persistent HTTP without pipelining:**
Same page. Persistent = 1 TCP handshake, then sequential requests.

TCP handshake: 1 RTT
HTML: 1 RTT
10 images sequentially: 10 × 1 RTT

Total = 1 + 1 + 10 = **12 RTT = 1.2 seconds**

**Problem 3 — Persistent HTTP with pipelining:**
Same page. All embedded objects requested in parallel.

TCP handshake: 1 RTT
HTML: 1 RTT
All 10 images (pipelined): 1 RTT (all requests sent together, all responses come back)

Total = 1 + 1 + 1 = **3 RTT = 0.3 seconds**

**Problem 4 — Non-persistent with parallel connections (max 5):**
Using 5 parallel TCP connections for 10 images:

TCP handshake for HTML: 2 RTT
5 images in parallel: 2 RTT
5 more images in parallel: 2 RTT

Total = 2 + 2 + 2 = **6 RTT = 0.6 seconds**

**Comparison Table:**

| Method | Time (RTTs) | Time (ms) |
|---|---|---|
| Non-persistent, sequential | 22 | 2200 |
| Non-persistent, 5 parallel | 6 | 600 |
| Persistent, no pipelining | 12 | 1200 |
| Persistent, pipelining | 3 | 300 |

### 6.5 DNS Resolution — Worked Example

Query: `www.iitb.ac.in`

**Step-by-step iterative resolution:**
```
Client → Local DNS: "What is www.iitb.ac.in?"
Local DNS → Root Server: "Who handles .in?"
Root → Local DNS: "Ask .in NS (ns.registry.in)"
Local DNS → .in NS: "Who handles ac.in?"
.in NS → Local DNS: "Ask ac.in NS"
Local DNS → ac.in NS: "Who handles iitb.ac.in?"
ac.in NS → Local DNS: "Ask iitb.ac.in NS (dns.iitb.ac.in)"
Local DNS → iitb.ac.in NS: "What is www.iitb.ac.in?"
iitb.ac.in NS → Local DNS: "A record: 103.21.124.1"
Local DNS → Client: "103.21.124.1"
```

**Total DNS queries = 4** (to root + .in + ac.in + iitb.ac.in)
Each query = 1 RTT → **Total DNS resolution time = 4 RTTs**

> If local DNS has `.in` cached: skip root → **3 RTTs**
> If local DNS has `iitb.ac.in NS` cached: **1 RTT**

### 6.6 FTP (File Transfer Protocol)

- Uses **two TCP connections:**
  - **Control connection:** Port 21 (persistent, for commands)
  - **Data connection:** Port 20 (created per transfer)
- **Active mode:** Server connects to client's data port
- **Passive mode:** Client connects to server's ephemeral port (firewall-friendly)
- Only transfers ASCII text (MIME for attachments)

**POP3 vs IMAP:**

| Feature | POP3 | IMAP |
|---|---|---|
| Mail storage | Downloaded locally | Remains on server |
| Multi-device sync | No | Yes |
| Offline access | Yes (once downloaded) | Features available |
| Server load | Low | Higher |

### 6.4 FTP (File Transfer Protocol)

- Uses **TCP**, two connections:
  - **Control connection:** Port 21 (persistent, for commands)
  - **Data connection:** Port 20 (non-persistent, for file transfer)
- **Active Mode:** Server opens data connection to client
- **Passive Mode:** Client opens data connection to server
- Commands: USER, PASS, LIST, RETR, STOR, QUIT
- FTP is **stateful** (remembers current directory, logged-in user)

---

## Module 7: Physical Layer & Multiplexing ⭐

### 7.1 Nyquist Theorem (Noiseless Channel)
$$C = 2B \log_2 L \text{ bps}$$
where $B$ = bandwidth (Hz), $L$ = number of signal levels.

**Maximum signaling rate** (Nyquist rate) = $2B$ symbols/sec (bauds).

**Baud Rate vs Bit Rate:**
- Baud rate = symbols/sec = $2B$
- Bit rate = Baud rate × $\log_2 L$
- Each symbol carries $\log_2 L$ bits

**Worked Example 1:** B = 4000 Hz, 16 signal levels.
$C = 2 \times 4000 \times \log_2 16 = 8000 \times 4 = 32000$ bps = 32 kbps
Baud rate = 8000 symbols/sec, each symbol carries 4 bits.

**Worked Example 2:** B = 200 kHz, need 1 Mbps data rate. Minimum signal levels?
$10^6 = 2 \times 200000 \times \log_2 L$
$\log_2 L = 2.5$
$L = 2^{2.5} \approx 5.66$ → Need at least **6 levels** (but typically use power of 2 → **8 levels**)

**Worked Example 3:** If modulation is QAM-64, what's the bit rate at 4000 baud?
64 levels → each symbol = $\log_2 64 = 6$ bits
Bit rate = 4000 × 6 = **24000 bps = 24 kbps**

### 7.2 Shannon's Theorem (Noisy Channel) ⭐
$$C = B \log_2(1 + \text{SNR})$$
where SNR = Signal-to-Noise Ratio (linear, not in dB).

**dB conversion:** $\text{SNR}_{dB} = 10 \log_{10}(\text{SNR})$, $\text{SNR}_{linear} = 10^{\text{SNR}_{dB}/10}$

**Worked Example 1:** B = 3 kHz, SNR = 30 dB.
$\text{SNR} = 10^{30/10} = 10^3 = 1000$
$C = 3000 \times \log_2(1001) = 3000 \times 9.97 \approx 29,910$ bps ≈ 30 kbps

**Worked Example 2:** B = 4 kHz, SNR = 20 dB. Find max capacity and minimum signal levels for Nyquist.
$\text{SNR} = 10^{20/10} = 100$
Shannon: $C = 4000 \times \log_2(101) = 4000 \times 6.66 = 26,640$ bps

For Nyquist to achieve this: $26640 = 2 \times 4000 \times \log_2 L$
$\log_2 L = 3.33$, $L = 2^{3.33} \approx 10$ → Need ≥ 10 signal levels

**Worked Example 3 — GATE: Combined Nyquist + Shannon:**
Channel: B = 1 MHz, SNR = 63. What data rate with 4 signal levels?

Nyquist: $C_N = 2 \times 10^6 \times \log_2 4 = 4$ Mbps
Shannon: $C_S = 10^6 \times \log_2(64) = 6$ Mbps

Actual capacity = min($C_N$, $C_S$) = **4 Mbps** (Nyquist is the limit with only 4 levels)

To achieve Shannon's limit: $6 \times 10^6 = 2 \times 10^6 \times \log_2 L$ → $L = 8$ signal levels needed.

**Worked Example 4 — What SNR (in dB) is needed for 50 kbps at 5 kHz bandwidth?**
$50000 = 5000 \times \log_2(1 + \text{SNR})$
$\log_2(1 + \text{SNR}) = 10$
$\text{SNR} = 2^{10} - 1 = 1023$
$\text{SNR}_{dB} = 10 \log_{10}(1023) \approx 30.1$ dB

> 🎯 **GATE Trick:** Shannon gives MAXIMUM possible capacity regardless of signal levels. If Nyquist capacity < Shannon, increase signal levels. If Nyquist > Shannon, noise prevents achieving that rate!

### 7.3 Multiplexing

| Type | Division | Description |
|------|----------|-------------|
| **FDM** | Frequency | Each channel gets a separate frequency band with guard bands |
| **TDM** | Time | Each channel gets a time slot in rotation |
| **Synchronous TDM** | Time | Fixed slots; wasted if channel idle |
| **Statistical TDM** | Time | Slots assigned on demand; more efficient |
| **WDM** | Wavelength | FDM for optical fiber (different light wavelengths) |
| **CDM/CDMA** | Code | Each channel gets unique code; all transmit simultaneously |

**TDM calculations:**
- If $n$ channels, each at $r$ bps → link rate = $n \times r$ bps
- Frame duration = 1 time slot × $n$ channels
- Overhead for synchronization bits in each frame

**CDMA:** Each station assigned a chip sequence. Inner product of two different stations' codes = 0 (orthogonal). Inner product with self = 1. To decode station $k$: multiply received signal by station $k$'s code and sum.

**CDMA Worked Example:**
Station A code: [+1, -1, +1, +1]
Station B code: [+1, +1, -1, +1]

Verify orthogonality: A·B = (+1)(+1) + (-1)(+1) + (+1)(-1) + (+1)(+1) = 1 - 1 - 1 + 1 = **0** ✓

Station A sends bit 1 (encoded as +1): transmits [+1, -1, +1, +1]
Station B sends bit 0 (encoded as -1): transmits [-1, -1, +1, -1]

Combined signal: [+1-1, -1-1, +1+1, +1-1] = [0, -2, +2, 0]

**Decoding A:** (Combined · A's code) / 4 = (0×1 + (-2)×(-1) + 2×1 + 0×1) / 4 = 4/4 = **+1** → A sent bit 1 ✓

**Decoding B:** (Combined · B's code) / 4 = (0×1 + (-2)×1 + 2×(-1) + 0×1) / 4 = -4/4 = **-1** → B sent bit 0 ✓

> 🎯 **GATE Trick:** In CDMA, if you have $N$ stations, each chip code has length $N$. Use Walsh-Hadamard matrix to generate orthogonal codes.

### 7.4 Line Encoding

| Encoding | Properties | Clock Recovery |
|----------|-----------|----------------|
| NRZ-L | +V for 0, -V for 1 (or vice versa) | Poor |
| NRZ-I | Transition for 1, no transition for 0 | Better for 1s |
| Manchester | XOR data with clock; transition at mid-bit | Excellent |
| Differential Manchester | Transition at mid-bit always; transition at start = 0 | Excellent |
| AMI (Bipolar) | 0→0V, 1→alternating +V/-V | Good |

**Line Encoding Waveforms for Data: 1 0 1 1 0 0 1:**
```
Bit:       | 1 | 0 | 1 | 1 | 0 | 0 | 1 |
           |---|---|---|---|---|---|---|
NRZ-L:     |___|‾‾‾|___|___|‾‾‾|‾‾‾|___|
(1=low)    

NRZ-I:     |‾↓_|___|_↑‾|‾↓_|___|___|_↑‾|
(transition=1)

Manchester: |_↑‾|‾↓_|_↑‾|_↑‾|‾↓_|‾↓_|_↑‾|
(1=low→high, 0=high→low)

AMI:       |+V |0V |-V |+V |0V |0V |-V |
(alternating polarity for 1s)
```

**Key Properties:**

| Encoding | Baseline Wander | Bandwidth | Clock | DC Component |
|---|---|---|---|---|
| NRZ-L | Yes (long 0s/1s) | B = N/2 | No self-clocking | Yes |
| NRZ-I | Less (no long 1s) | B = N/2 | Partial | Possible |
| Manchester | No | B = N | Yes (guaranteed transitions) | No |
| Diff Manchester | No | B = N | Yes | No |
| AMI | No | B = N/2 | No for long 0s | No for 1s |

> 🎯 **GATE Trick:** Manchester encoding has TWICE the bandwidth of NRZ (because mid-bit transitions double the frequency). But it provides self-clocking. Trade-off: bandwidth vs. synchronization.

**Bandwidth for Manchester:** If bit rate = R bps, Manchester needs minimum bandwidth = **R Hz** (vs. R/2 for NRZ).

**4B/5B + NRZI:** Used in Fast Ethernet. 4 data bits encoded as 5-bit code (ensures no more than 3 consecutive 0s), then transmitted using NRZ-I. Efficiency = 4/5 = 80%.

### 7.5 HDLC and PPP

**HDLC (High-Level Data Link Control):**
- Bit-oriented protocol
- Frame: Flag(01111110) | Address | Control | Data | FCS | Flag
- Bit stuffing: After five consecutive 1s, insert a 0
- Three types of frames: I (information), S (supervisory), U (unnumbered)

**PPP (Point-to-Point Protocol):**
- Byte-oriented protocol for dial-up/WAN links
- Frame: Flag(7E) | Address(FF) | Control(03) | Protocol | Data | FCS | Flag
- Byte stuffing: ESC (7D) before any 7E or 7D in data (XOR with 0x20)
- Sub-protocols: LCP (link setup/teardown), NCP (network config), authentication (PAP/CHAP)
- **PAP:** Password sent in cleartext (insecure)
- **CHAP:** Challenge-response with hash (more secure)
- PPP does NOT provide: error correction, flow control, addressing (point-to-point)

---

## Module 8: IPv6 ⭐

### 8.1 IPv6 Address
- **128-bit** addresses (vs IPv4's 32-bit)
- Written as 8 groups of 4 hex digits: `2001:0db8:0000:0000:0000:ff00:0042:8329`
- Shorthand: omit leading zeros, replace consecutive zero groups with `::` (once only)

### 8.2 IPv6 Header
- **Fixed 40-byte header** (simpler than IPv4's variable-length)
- Key fields: Version(6), Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source, Destination
- **No checksum** in IPv6 header (offloaded to upper layers)
- **No fragmentation by routers** — only source fragments; Path MTU Discovery used
- Extension headers (optional): Hop-by-Hop, Routing, Fragment, Authentication, ESP

### 8.3 IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address size | 32 bits | 128 bits |
| Header size | 20–60 bytes (variable) | 40 bytes (fixed) |
| Checksum | Yes | No |
| Fragmentation | Routers + source | Source only |
| Broadcast | Yes | No (multicast used) |
| ARP | Yes | NDP (Neighbor Discovery Protocol) |
| NAT | Common | Unnecessary (enough addresses) |
| IPSec | Optional | Mandatory |

### 8.4 Transition Mechanisms
- **Dual Stack:** Devices run both IPv4 and IPv6
- **Tunneling:** IPv6 packets encapsulated inside IPv4 (6to4, Teredo)
- **NAT64:** Translate between IPv4 and IPv6

---

## Module 9: Network Security & Cryptography ⭐⭐

### 9.1 Symmetric Key Cryptography
- Same key for encryption and decryption
- Fast, but key distribution problem
- Examples: DES (56-bit key, insecure), 3DES, **AES** (128/192/256-bit key, standard)
- Block cipher modes: ECB (insecure), CBC, CTR, GCM

### 9.2 Asymmetric Key Cryptography (Public Key)
- Two keys: public key (encrypt) and private key (decrypt)
- Slow, but solves key distribution
- Example: **RSA**

**RSA Algorithm:**
1. Choose two large primes $p, q$. Compute $n = pq$, $\phi(n) = (p-1)(q-1)$
2. Choose $e$ such that $\gcd(e, \phi(n)) = 1$
3. Compute $d = e^{-1} \mod \phi(n)$ (using extended Euclidean)
4. Public key = $(e, n)$, Private key = $(d, n)$
5. Encrypt: $C = M^e \mod n$, Decrypt: $M = C^d \mod n$

**RSA Worked Example:**
$p = 7, q = 11$
$n = 77, \phi(n) = 6 \times 10 = 60$
Choose $e = 13$ (gcd(13,60) = 1 ✓)
Find $d$: $13d \equiv 1 \pmod{60}$ → $d = 37$ (since $13 \times 37 = 481 = 8 \times 60 + 1$)

Public key: (13, 77), Private key: (37, 77)

Encrypt $M = 2$: $C = 2^{13} \mod 77 = 8192 \mod 77 = 30$
Decrypt: $M = 30^{37} \mod 77 = 2$ ✓

> 🎯 **GATE Trick:** To compute $a^b \mod n$ efficiently, use repeated squaring:
> $2^{13} = 2^{8} \times 2^{4} \times 2^{1}$
> $2^2 = 4, 2^4 = 16, 2^8 = 16^2 = 256 \mod 77 = 25$
> $2^{13} = 25 \times 16 \times 2 = 800 \mod 77 = 800 - 10 \times 77 = 30$ ✓

### 9.3 Diffie-Hellman Key Exchange
1. Public: prime $p$, generator $g$
2. Alice: secret $a$, sends $A = g^a \mod p$
3. Bob: secret $b$, sends $B = g^b \mod p$
4. Shared secret: $K = B^a \mod p = A^b \mod p = g^{ab} \mod p$
- Vulnerable to **man-in-the-middle attack** (no authentication)

**DH Worked Example:**
$p = 23, g = 5$
Alice: $a = 6$, sends $A = 5^6 \mod 23 = 15625 \mod 23 = 8$
Bob: $b = 15$, sends $B = 5^{15} \mod 23$
$5^2 = 2, 5^4 = 4, 5^8 = 16, 5^{15} = 5^8 \times 5^4 \times 5^2 \times 5 = 16 \times 4 \times 2 \times 5 = 640 \mod 23 = 640 - 27 \times 23 = 640 - 621 = 19$

Shared secret:
Alice: $K = B^a = 19^6 \mod 23 = 2$
Bob: $K = A^b = 8^{15} \mod 23 = 2$ ✓

**Security Properties Comparison:**

| Property | Symmetric | Asymmetric | Digital Signature | Hash |
|---|---|---|---|---|
| Confidentiality | ✓ | ✓ | ✗ | ✗ |
| Integrity | ✗ | ✗ | ✓ | ✓ |
| Authentication | ✗ | ✗ | ✓ | ✗ (alone) |
| Non-repudiation | ✗ | ✗ | ✓ | ✗ |
| Speed | Fast | Slow | Slow | Fast |

**Common Hash Functions:**
| Hash | Output Size | Status |
|---|---|---|
| MD5 | 128 bits | Broken (collisions found) |
| SHA-1 | 160 bits | Deprecated (collisions found) |
| SHA-256 | 256 bits | Secure (current standard) |
| SHA-512 | 512 bits | Secure |

### 9.4 Digital Signatures
- Sign with **private key**, verify with **public key** (reverse of encryption)
- Process: Hash(message) → Sign hash with private key → Attach signature
- Provides: Authentication, Integrity, Non-repudiation
- **Does NOT provide confidentiality** (message isn't encrypted)

### 9.5 Certificates & PKI
- **Certificate Authority (CA)** issues digital certificates binding public key to identity
- **X.509 format**: subject, public key, issuer, validity, signature
- **Certificate chain:** Root CA → Intermediate CA → End entity

### 9.6 SSL/TLS
- **TLS (Transport Layer Security)** — successor to SSL
- Operates between Transport and Application layer
- Handshake: negotiate cipher suite, authenticate server (optional client), exchange keys
- Uses symmetric encryption for data (fast) + asymmetric for key exchange (secure)

### 9.7 Firewalls
| Type | Layer | Filtering Based On |
|------|-------|------|
| **Packet Filter** | Network | IP addresses, port numbers, protocol |
| **Stateful** | Transport | Connection state tracking |
| **Application Gateway (Proxy)** | Application | Content inspection |

### 9.8 IPSec
- Network-layer security protocol suite
- **AH (Authentication Header):** Authentication + integrity (no encryption)
- **ESP (Encapsulating Security Payload):** Authentication + integrity + **encryption**
- Modes: Transport mode (end-to-end) vs Tunnel mode (gateway-to-gateway, VPN)

---

## Module 10: Advanced Routing Protocols

### 10.1 OSPF (Open Shortest Path First) ⭐
- **Link-state** protocol, uses Dijkstra's algorithm
- Supports **areas** (hierarchical routing): Backbone (Area 0) + non-backbone areas
- **LSA (Link State Advertisement):** Each router floods its link info
- Metric: cost (bandwidth-based by default)
- Supports VLSM, CIDR, authentication
- Convergence: faster than RIP

### 10.2 BGP (Border Gateway Protocol) ⭐
- **Path-vector** protocol for inter-AS (Autonomous System) routing
- Uses TCP (port 179)
- **eBGP:** Between different ASes
- **iBGP:** Within the same AS
- BGP Attributes: AS-PATH, NEXT-HOP, LOCAL-PREF, MED
- **Policy-based routing** — can override shortest path
- Prevents loops using AS-PATH (reject routes containing own AS number)

### 10.3 RIP vs OSPF vs BGP

| | RIP | OSPF | BGP |
|--|-----|------|-----|
| Type | Distance Vector | Link State | Path Vector |
| Algorithm | Bellman-Ford | Dijkstra | Best path selection |
| Metric | Hop count (max 15) | Cost (bandwidth) | Policy + attributes |
| Scope | Intra-AS | Intra-AS | Inter-AS |
| Convergence | Slow | Fast | Moderate |
| Transport | UDP (520) | IP (protocol 89) | TCP (179) |

---

## Important Formulas Summary

### IP Addressing
- Hosts in $/n$ network = $2^{32-n} - 2$
- Subnets from $s$ borrowed bits = $2^s$
- Block size = $2^{32-n}$
- Network address = IP AND Mask
- Broadcast = Network OR (NOT Mask)

### Delays
- $d_{\text{prop}} = \text{Distance} / \text{Speed}$
- $d_{\text{trans}} = L / B$
- $a = T_{\text{prop}} / T_{\text{trans}}$
- Store-and-forward: $T = k \times L/B + T_{prop}$
- Multiple packets: $T = (k + P - 1) \times L/B + k \times T_{prop}$
- Bandwidth-Delay Product = $B \times \text{RTT}$

### Sliding Window Efficiency
- Stop & Wait: $\eta = \frac{1}{1+2a}$
- GBN/SR: $\eta = \frac{N}{1+2a}$ (when $N < 1+2a$)
- S&W with errors: $\eta = \frac{1-p}{1+2a}$
- GBN with errors: $\eta = \frac{N(1-p)}{(1+2a)(1-p+Np)}$
- SR with errors: $\eta = \frac{N(1-p)}{1+2a}$

### Window Constraints
- GBN: $W_s \leq 2^n - 1$, $W_r = 1$
- SR: $W_s + W_r \leq 2^n$, typically $W_s = W_r = 2^{n-1}$

### MAC Protocols
- Pure ALOHA: $S = Ge^{-2G}$, $S_{\max} = 1/(2e) \approx 18.4\%$ at $G=0.5$
- Slotted ALOHA: $S = Ge^{-G}$, $S_{\max} = 1/e \approx 36.8\%$ at $G=1$
- CSMA/CD efficiency: $\eta = 1/(1+6.44a)$
- Min frame size: $L_{\min} = 2 \times T_{\text{prop}} \times B$

### IP Fragmentation
- Max data per fragment = $\lfloor \frac{MTU - Header}{8} \rfloor \times 8$
- Fragment offset in units of 8 bytes
- Number of fragments = $\lceil \frac{Data}{MaxDataPerFrag} \rceil$

### TCP Congestion
- Slow Start: cwnd doubles per RTT ($\text{cwnd} = 2^k$ after $k$ RTTs)
- Congestion Avoidance: cwnd += 1 MSS per RTT
- Data sent in $k$ RTTs of SS: $2^k - 1$ MSS
- Throughput ≈ $\frac{C \times MSS}{RTT \times \sqrt{p}}$
- Token Bucket max data in $t$: $b + rt$
- Token Bucket burst duration: $b/(R-r)$

### Channel Capacity
- Nyquist: $C = 2B\log_2 L$ (noiseless)
- Shannon: $C = B\log_2(1 + \text{SNR})$ (noisy)
- Actual capacity ≤ min(Nyquist, Shannon)
- SNR dB to linear: $\text{SNR} = 10^{\text{SNR}_{dB}/10}$
- Baud rate = $2B$, Bit rate = Baud rate × $\log_2 L$

### RSA
- $n = pq$, $\phi(n) = (p-1)(q-1)$
- Public key $(e, n)$: $\gcd(e, \phi) = 1$
- Private key: $d = e^{-1} \mod \phi(n)$
- Encrypt: $C = M^e \mod n$, Decrypt: $M = C^d \mod n$

### HTTP Timing
- Non-persistent: 2 RTT per object
- Persistent no pipelining: 1 RTT first time + 1 RTT per object
- Persistent with pipelining: ~3 RTT total

---

## 📝 Practice Problem Sets

### Practice Set 1: IP Addressing & Subnetting (P1–P15)

**P1.** An organization is given the network 150.100.0.0/16. It needs 60 subnets, each supporting at least 500 hosts. What subnet mask should be used?
- Need $2^s \geq 60 \Rightarrow s = 6$
- Hosts per subnet: $2^{16-6} - 2 = 2^{10} - 2 = 1022 \geq 500$ ✓
- Subnet mask: /22 = 255.255.252.0

**P2.** What is the network address of 172.31.128.200/18?
- Block in 3rd octet: $2^{24-18} = 64$
- $128/64 = 2$ → Network at $2 \times 64 = 128$
- **Answer: 172.31.128.0/18**

**P3.** How many /30 subnets can be created from a /24 network?
- Each /30 = 4 addresses. /24 = 256 addresses.
- $256/4 = 64$ subnets.
- **Answer: 64**

**P4.** Given 192.168.5.0/24, allocate subnets via VLSM:
- Department A: 120 hosts → /25 (126 hosts): 192.168.5.0/25
- Department B: 60 hosts → /26 (62 hosts): 192.168.5.128/26
- Department C: 30 hosts → /27 (30 hosts): 192.168.5.192/27
- Link 1: 2 hosts → /30 (2 hosts): 192.168.5.224/30
- Link 2: 2 hosts → /30: 192.168.5.228/30

**P5.** Can these be superneted: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16?
- Only 3 networks → NOT power of 2. **No!**

**P6.** Router forwarding table:
| Prefix | Next Hop |
|---|---|
| 135.46.56.0/22 | A |
| 135.46.60.0/22 | B |
| 135.46.56.0/21 | C |
| 0.0.0.0/0 | D |

Where does 135.46.63.10 go?
- 56.0/22: range 56.0–59.255. 63.10 → No
- 60.0/22: range 60.0–63.255. 63.10 → Yes ✓ (/22)
- 56.0/21: range 56.0–63.255. 63.10 → Yes ✓ (/21)
- LPM: /22 beats /21 → **Answer: B**

**P7.** How many IP addresses in 10.0.0.0/8?
$2^{24} = 16,777,216$ total, $16,777,216 - 2 = 16,777,214$ usable hosts.

**P8.** Given the IP 172.16.35.123/20, what's the broadcast address?
Block in 3rd octet: 16. $35/16 = 2$. Network: 172.16.32.0
Broadcast: 172.16.47.255 (32 + 15 = 47 in 3rd, 255 in 4th)

**P9.** A network 200.10.10.0/24 is subnetted with mask 255.255.255.248. How many subnets and hosts per subnet?
/29 mask. Subnets: $2^{29-24} = 32$. Hosts: $2^3 - 2 = 6$.

**P10.** Which of these is a valid host address in 10.10.10.0/29?
(a) 10.10.10.0 → Network address ✗
(b) 10.10.10.1 → Valid ✓
(c) 10.10.10.7 → Broadcast ✗
(d) 10.10.10.8 → Next subnet ✗

**P11.** Given five /24 networks: 192.168.0.0, 192.168.1.0, 192.168.2.0, 192.168.3.0, 192.168.4.0. What's the minimal supernet that covers all?
- Need to cover 0–4 in 3rd octet → need 8 networks (/21 covers 0–7)
- **192.168.0.0/21** (but also covers 5.0, 6.0, 7.0 — wasteful but required for supernetting)

**P12.** An ISP has 200.200.0.0/16 and wants to give each customer a /28 block. Max customers?
Available addresses: $2^{16} = 65536$
Per customer: $2^4 = 16$ addresses
Max customers: $65536/16 = 4096$

**P13.** What's the 100th usable host in 10.0.0.0/24?
Network: 10.0.0.0, First host: 10.0.0.1
100th host: **10.0.0.100**

**P14.** Two routers connected via /30 link. Router A = 192.168.1.1, Router B = ?
/30 = 4 addresses: .0 (network), .1 (A), .2 (B), .3 (broadcast)
**Answer: 192.168.1.2**

**P15.** Host A: 10.1.1.5/24. Host B: 10.1.2.5/24. Can they communicate directly?
A's network: 10.1.1.0/24. B's network: 10.1.2.0/24. **Different networks → need router!**

### Practice Set 2: Delays & Sliding Window (P16–P30)

**P16.** A 10 Mbps link, RTT = 20 ms, frame = 200 bytes. Find efficiency of Stop-and-Wait.
$T_{trans} = 200 \times 8 / (10 \times 10^6) = 0.16$ ms
$T_{prop} = 10$ ms (RTT/2)
$a = 10/0.16 = 62.5$
$\eta = 1/(1 + 125) = 0.79\%$ → Terrible!

**P17.** For P16, how many seq bits needed for GBN to achieve 100% efficiency?
$N \geq 1 + 2a = 126$
GBN: $2^n - 1 \geq 126$ → $n \geq 7$ (**7 bits**, max window = 127 ≥ 126) ✓

**P18.** For P16 with SR, how many seq bits?
$W_s = 2^{n-1} \geq 126$ → $2^{n-1} \geq 126$ → $n \geq 8$ (**8 bits**, max window = 128)

**P19.** GBN with 3-bit seq numbers. Window = 7. Frame error rate = 0.2. $a = 2$.
$\eta = \frac{7 \times 0.8}{5 \times (0.8 + 1.4)} = \frac{5.6}{11} = 50.9\%$

**P20.** 1 Gbps satellite link, one-way delay = 270 ms. Frame = 10000 bits. Find $a$.
$T_{trans} = 10000 / 10^9 = 0.01$ ms
$a = 270 / 0.01 = 27000$
S&W efficiency: $1/54001 \approx 0.00185\%$ → Need window ≥ 54001!

**P21.** Send 1 MB file using Stop-and-Wait over 1 Mbps link. RTT = 2ms. Frame = 1000 bits. Time?
$a = 1/1 = 1$, $\eta = 1/3 = 33.3\%$
Effective throughput = 333.3 kbps
Time = $8 \times 10^6 / 333333 = 24$ seconds

With GBN (window=3): $\eta = 3/3 = 100\%$
Time = $8 \times 10^6 / 10^6 = 8$ seconds

**P22.** Two packets sent over 3 links (each 100 Mbps). Packet = 1500 bytes. Prop delay per link = 1 ms.
$T_{trans} = 1500 \times 8 / (100 \times 10^6) = 0.12$ ms
$T = (3 + 2 - 1) \times 0.12 + 3 \times 1 = 0.48 + 3 = 3.48$ ms

**P23.** Cut-through switch: frame = 1000 bytes, header = 20 bytes. 5 links each 10 Mbps. Total prop delay = 5 ms.
$T = \frac{1000 \times 8}{10^7} + 4 \times \frac{20 \times 8}{10^7} + 5 = 0.8 + 0.064 + 5 = 5.864$ ms

**P24.** In Selective Repeat with 4-bit sequence numbers, what is the maximum window size?
$W_{max} = 2^{4-1} = 2^3 = 8$

**P25.** GBN, window = 4, seq bits = 3. Frames 0,1,2,3 sent. ACK 0 lost. ACK 1 arrives. What does sender do?
ACK 1 is cumulative → means frames 0 AND 1 are acknowledged. Sender slides window to [2,3,4,5].

**P26.** SR, window = 4. Frames 0,1,2,3 sent. Frame 2 lost. What happens?
Receiver buffers 0,1,3 (sends individual ACKs). Timer for frame 2 expires → sender retransmits ONLY frame 2. Receiver delivers 0,1,2,3 in order.

**P27.** What is the throughput of a 100 Mbps link using S&W with RTT = 10 ms and frame = 5000 bits?
$T_{trans} = 5000/(100 \times 10^6) = 0.05$ ms
$\eta = 0.05/(0.05 + 10) = 0.497\%$
Throughput = $0.00497 \times 100 = 0.497$ Mbps

**P28.** How much time to transmit 100 frames of 1000 bits each with Stop-and-Wait? Link = 1 Mbps, RTT = 10 ms.
Per frame: $T_{trans} + RTT = 1 + 10 = 11$ ms
Total: 100 × 11 = **1100 ms = 1.1 seconds**

**P29.** A protocol uses 2-bit sequence numbers with SR. Max frames in transit?
$W = 2^{2-1} = 2$. At most **2 frames** in transit.

**P30.** Sliding window efficiency is 25%. Link = 10 Mbps, frame = 5000 bits, ACK = 0 bits. Find one-way propagation delay.
$0.25 = 0.5/(0.5 + 2T_{prop})$ → $0.5 + 2T_{prop} = 2$ → $T_{prop} = 0.75$ ms

### Practice Set 3: MAC Protocols & CSMA/CD (P31–P40)

**P31.** In Pure ALOHA, $G = 0.5$. What's the throughput?
$S = 0.5 \times e^{-1} = 0.5 \times 0.368 = 0.184 = 18.4\%$ (maximum!)

**P32.** Slotted ALOHA with 100 stations. Each transmits with probability 0.01 per slot. Expected throughput?
$G = 100 \times 0.01 = 1.0$
$S = 1 \times e^{-1} = 36.8\%$ (optimal load!)

**P33.** CSMA/CD: Ethernet 10 Mbps, cable length 500m, prop speed = $2 \times 10^8$ m/s. Min frame size?
$T_{prop} = 500/(2 \times 10^8) = 2.5 \mu s$
$L_{min} = 2 \times 2.5 \times 10^{-6} \times 10^7 = 50$ bits = 6.25 bytes
(Rounded up: 7 bytes. In practice, 64 bytes is standard)

**P34.** Gigabit Ethernet (1000 Mbps): min frame for max distance 200m?
$T_{prop} = 200/(2 \times 10^8) = 1 \mu s$
$L_{min} = 2 \times 10^{-6} \times 10^9 = 2000$ bits = 250 bytes
(Gigabit Ethernet uses carrier extension to pad to 512 bytes)

**P35.** CSMA/CD collision: Station A starts at $t=0$, station B at $t=\tau$ (just before A's signal reaches B). When does A detect collision?
B detects collision at $t=\tau$. B's jam signal reaches A at $t=2\tau$. A detects collision at $t = 2\tau$.

**P36.** After 4th collision in binary exponential backoff, what are the possible wait times?
$k=4$: $R \in \{0, 1, 2, ..., 15\}$ → 16 possible values.
Wait = $R \times$ slot time.

**P37.** A hub connects 10 devices at 100 Mbps. What's the effective per-device bandwidth?
Hub = shared medium → all 10 share 100 Mbps → **10 Mbps** effective per device.
A switch would give each device 100 Mbps (separate collision domains).

**P38.** How many collision and broadcast domains in a network with 3 switches, 2 routers, 20 hosts?
- Each switch port: 1 collision domain → ~20 collision domains (one per host)
- Each switch: 1 broadcast domain → 3 broadcast domains from switches
- But routers separate broadcast domains → depends on topology

**P39.** CSMA/CD efficiency for frame = 1518 bytes on 10 Mbps Ethernet with max length 2500m.
$T_{trans} = 1518 \times 8 / 10^7 = 1.2144$ ms
$T_{prop} = 2500/(2 \times 10^8) = 12.5 \mu s = 0.0125$ ms
$a = 0.0125/1.2144 = 0.0103$
$\eta = 1/(1 + 6.44 \times 0.0103) = 1/1.0663 = 93.8\%$

**P40.** In Pure ALOHA, if average frames generated per frame time = 2, what's the throughput?
$G = 2$, $S = 2e^{-4} = 2 \times 0.0183 = 0.0366 = 3.66\%$ (network overloaded!)

### Practice Set 4: Fragmentation & Network Layer (P41–P55)

**P41.** Packet = 2400 bytes (header=20, data=2380). Link MTU = 700 bytes. How many fragments?
Max data = $\lfloor 680/8 \rfloor \times 8 = 680$
Fragments = $\lceil 2380/680 \rceil = 4$
| Frag | Data | MF | Offset |
|---|---|---|---|
| 1 | 680 | 1 | 0 |
| 2 | 680 | 1 | 85 |
| 3 | 680 | 1 | 170 |
| 4 | 340 | 0 | 255 |

**P42.** If a fragment with MF=1, offset=100, total length=200, header=20. What bytes does it carry?
Start: $100 \times 8 = 800$. Data = 180 bytes. End: 979. Carries bytes **800–979**.

**P43.** Packet = 4000 bytes, header = 20. Passes through MTU=1500 then MTU=576 links. Total fragments at destination?
Link 1 (MTU=1500): max data = 1480, Frags = $\lceil 3980/1480 \rceil = 3$ (1480, 1480, 1020)
Link 2 (MTU=576): max data = $\lfloor 556/8 \rfloor \times 8 = 552$
Frag 1 (1480 bytes): $\lceil 1480/552 \rceil = 3$ sub-fragments
Frag 2 (1480 bytes): 3 sub-fragments
Frag 3 (1020 bytes): $\lceil 1020/552 \rceil = 2$ sub-fragments
Total: 3 + 3 + 2 = **8 fragments**

**P44.** TTL = 64. After how many routers will the packet be dropped?
TTL decremented by 1 at each router → dropped at **64th router** (TTL becomes 0).

**P45.** In DV routing: A→B cost 1, B→C cost 2, A→C cost 10. After convergence, A's distance to C?
$D_A(C) = \min(c(A,C), c(A,B) + D_B(C)) = \min(10, 1+2) = 3$ via B.

**P46.** Dijkstra: Find shortest path from S to T in this graph.
```
S --5-- A --2-- T
|       |       |
3       1       4
|       |       |
B --6-- C --3-- D
```
Edges: S-A=5, A-T=2, S-B=3, A-C=1, T-D=4, B-C=6, C-D=3

| Step | Visited | S | A | B | C | D | T | Pick |
|---|---|---|---|---|---|---|---|---|
| Init | {} | 0 | ∞ | ∞ | ∞ | ∞ | ∞ | S |
| 1 | {S} | 0 | 5 | 3 | ∞ | ∞ | ∞ | B(3) |
| 2 | {S,B} | 0 | 5 | 3 | 9 | ∞ | ∞ | A(5) |
| 3 | {S,B,A} | 0 | 5 | 3 | 6 | ∞ | 7 | C(6) |
| 4 | {S,B,A,C} | 0 | 5 | 3 | 6 | 9 | 7 | T(7) |

S→T: cost 7 (S→A→T)

**P47.** Maximum number of entries in an IPv4 routing table?
Theoretically: $2^{32}$ (one per IP address), but practically much fewer. With CIDR, around 900,000+ in global Internet BGP tables.

**P48.** NAT: internal hosts 10.0.0.1–10.0.0.254 mapped to single public IP. Max simultaneous connections?
With PAT using 16-bit port numbers: ~65,000 connections (ports 1024–65535 typically).

**P49.** Count-to-infinity: A—B—C, all costs 1. Link A-B fails. If using poison reverse, how many updates to converge?
B poisons route to A via A (already knows). B hears A unreachable immediately. Converges in **1 update** (much faster than plain DV).

**P50.** RIP uses max 15 hops. What is the maximum diameter of a RIP network?
**15 hops** (16 = infinity → unreachable).

**P51.** OSPF area 0 is called the **backbone** area. All other areas must connect to area 0.

**P52.** In BGP, if AS path for route A is {AS1, AS2, AS3} and for route B is {AS4, AS5}, which is preferred?
Shorter AS-PATH preferred → **Route B** (2 vs 3 ASes).

**P53.** What protocol does traceroute use?
Sends UDP packets (to high port) OR ICMP Echo with incrementing TTL. Receives **ICMP Time Exceeded** from each hop. Final destination returns **ICMP Port Unreachable** (UDP) or **Echo Reply** (ICMP).

**P54.** Total overhead of IP fragmentation: Packet = 10000 bytes (header = 20), MTU = 500.
Max data: $\lfloor 480/8 \rfloor \times 8 = 480$
Fragments: $\lceil 9980/480 \rceil = 21$
Total headers: 21 × 20 = 420 bytes. Original: 20 bytes. Overhead: **400 bytes**

**P55.** IPv4 header field that prevents packets from looping forever: **TTL (Time To Live)**

### Practice Set 5: TCP Congestion Control (P56–P70)

**P56.** TCP Reno, ssthresh=8, cwnd starts at 1 MSS. Trace cwnd through RTTs:
| RTT | cwnd | ssthresh | Phase |
|---|---|---|---|
| 1 | 1→2 | 8 | SS |
| 2 | 2→4 | 8 | SS |
| 3 | 4→8 | 8 | SS→CA |
| 4 | 8→9 | 8 | CA |
| 5 | 9→10 | 8 | CA |
| 6 | Timeout! | 5 (was 10/2) | cwnd=1, SS |
| 7 | 1→2 | 5 | SS |
| 8 | 2→4 | 5 | SS |
| 9 | 4→5 | 5 | SS→CA |
| 10 | 5→6 | 5 | CA |

**P57.** TCP Reno, cwnd=16 when 3 dup ACKs received. ssthresh?
$\text{ssthresh} = 16/2 = 8$. cwnd = 8 + 3 = **11** (fast recovery).

**P58.** Total MSS sent in 10 RTTs starting from cwnd=1, ssthresh=8?
RTTs 1-3 (SS): 1+2+4 = 7
RTT 4 (transition at cwnd=8): 8
RTTs 5-10 (CA): 9+10+11+12+13+14 = 69
Total: 7 + 8 + 69 = **84 MSS**

Wait, let me recalculate. After RTT 3, cwnd becomes 8 (=ssthresh), so:
RTT 1: send 1, RTT 2: send 2, RTT 3: send 4, RTT 4: send 8 (now at ssthresh, switch to CA)
RTT 5: send 9, RTT 6: send 10, ..., RTT 10: send 14
Total = 1+2+4+8+9+10+11+12+13+14 = **84 MSS** ✓

**P59.** After how many RTTs does cwnd reach 64 MSS? ssthresh=32.
SS (cwnd doubles): 1,2,4,8,16,32 → 5 RTTs to reach 32
CA (cwnd +1): 33,34,...,64 → 32 more RTTs
Total: 5 + 32 = **37 RTTs**

**P60.** TCP throughput: MSS=1460 bytes, RTT=50ms, loss=0.01.
$\text{Throughput} \approx \frac{1.22 \times 1460}{0.05 \times \sqrt{0.01}} = \frac{1781.2}{0.005} = 356,240$ bytes/s ≈ **356 KB/s**

**P61.** Max cwnd before first loss in Tahoe if ssthresh=16:
SS: 1,2,4,8,16 (enters CA at 16). CA: 17,18,19... until loss.
If loss at cwnd=24: ssthresh becomes 12, cwnd=1 (Tahoe).

**P62.** Difference between Tahoe and Reno when 3 dup ACKs arrive at cwnd=20:
- Tahoe: ssthresh=10, cwnd=**1**, enter Slow Start
- Reno: ssthresh=10, cwnd=**13** (10+3), enter Fast Recovery

**P63.** TCP connection: window = 64 KB, RTT = 100 ms, MSS = 1 KB. What's max throughput?
64 KB / 100 ms = 640 KB/s = **5.12 Mbps**

**P64.** In TCP, if receiver advertises rwnd=0, what happens?
Sender enters **persist mode**: sends probe segments (1 byte) periodically until receiver opens window.

**P65.** TCP uses byte-stream: if MSS=1000 and application writes 3500 bytes, how many segments?
$\lceil 3500/1000 \rceil = 4$ segments (1000, 1000, 1000, 500).

**P66.** TCP seq number: First segment has seq=100, length=500. Next segment's seq?
**Seq = 600** (100 + 500). TCP uses byte numbering!

**P67.** 3-way handshake: Client seq=1000, Server seq=2000. After handshake:
- Client's seq = 1001, expecting ACK = 2001
- Server's seq = 2001, expecting ACK = 1001

**P68.** What's the effective window?
cwnd = 20 KB, rwnd = 15 KB → Effective window = min(20, 15) = **15 KB**

**P69.** TCP Reno: cwnd reaches 40 MSS. 3 dup ACKs, then timeout during fast recovery.
At 3 dup ACKs: ssthresh = 20, cwnd = 23 (fast recovery)
At timeout: ssthresh = 23/2 = 11 (half of current cwnd), cwnd = **1** (slow start)

**P70.** How many RTTs for Slow Start to go from cwnd=1 to cwnd=W?
$2^k = W$ → $k = \log_2 W$ RTTs. For $W=1024$ → **10 RTTs**.

### Practice Set 6: Physical Layer & Application (P71–P85)

**P71.** Telephone line: B = 3100 Hz, SNR = 25 dB. Max bit rate?
$\text{SNR} = 10^{2.5} = 316.2$
$C = 3100 \times \log_2(317.2) = 3100 \times 8.31 = 25,761$ bps ≈ **25.8 kbps**

**P72.** V.34 modem uses 3429 baud rate with 14 bits/symbol. Bit rate?
$R = 3429 \times 14 = 48,006$ bps ≈ 48 kbps ✓

**P73.** Optical channel: B = 10 GHz. How many signal levels for 100 Gbps?
$10^{11} = 2 \times 10^{10} \times \log_2 L$
$\log_2 L = 5$ → $L = 32$ levels

**P74.** Manchester encoding at 10 Mbps bit rate. What bandwidth needed?
Manchester: bandwidth = bit rate = **10 MHz** (double of NRZ's 5 MHz)

**P75.** CDMA: 4 stations, Station C code = [-1,+1,-1,+1]. Received signal = [+2,0,-2,0]. What did C send?
$(+2)(-1) + (0)(+1) + (-2)(-1) + (0)(+1) = -2 + 0 + 2 + 0 = 0$
$0/4 = 0$ → Station C **did not transmit** (or sent silence).

**P76.** TDM: 20 channels, each 64 kbps. Overhead = 1 sync bit per frame. Frame rate = 8000 frames/sec. Required link rate?
Data: 20 × 64,000 = 1,280,000 bps
Sync overhead: 8000 × 1 = 8000 bps
Total: **1,288,000 bps** ≈ 1.288 Mbps

**P77.** Web page: 1 HTML file (5 KB) and 6 images (2 KB each). RTT = 100 ms. Link = 1 Mbps. Non-persistent HTTP, sequential.
$T_{trans}^{HTML} = 5 \times 8/1 = 40$ ms
$T_{trans}^{img} = 2 \times 8/1 = 16$ ms
Non-persistent: each needs 2 RTT + transmission
HTML: 200 + 40 = 240 ms
6 images: 6 × (200 + 16) = 1296 ms
Total = 240 + 1296 = **1536 ms**

**P78.** Same page with persistent pipelining:
TCP handshake: 100 ms
HTML: 100 + 40 = 140 ms
All 6 images (pipelined): 100 + 6 × 16 = 196 ms
Total = 100 + 140 + 196 = **436 ms** (3.5x faster!)

**P79.** DNS uses UDP because:
- Queries are small (fit in one UDP datagram)
- Low overhead (no connection setup)
- Fast (no 3-way handshake)
DNS uses TCP for zone transfers (large data) and responses > 512 bytes.

**P80.** How many DNS queries for iterative resolution of `mail.cs.iitb.ac.in` (nothing cached)?
Root → `.in` → `ac.in` → `iitb.ac.in` → `cs.iitb.ac.in` = **5 queries** (potentially)

**P81.** FTP uses two connections because:
- **Control** (port 21): persistent, for commands
- **Data** (port 20): opened/closed per transfer
This is called **out-of-band signaling**.

**P82.** SMTP is a **push** protocol. POP3/IMAP are **pull** protocols.

**P83.** NRZ-I encoding of data 1100110:
Start high. 1=transition, 0=no transition.
```
Data: 1   1   0   0   1   1   0
      ↓   ↓           ↓   ↓
NRZ-I:H→L L→H H   H  H→L L→H H
```

**P84.** What's the minimum sampling rate (Nyquist rate) for a signal with max frequency 4 kHz?
$f_s = 2 \times f_{max} = 2 \times 4000 = 8000$ samples/sec (this is telephone standard)

**P85.** HTTP/2 improvements: binary framing, multiplexing multiple streams over single TCP, header compression (HPACK), server push. HTTP/3 uses QUIC (UDP-based).

### Practice Set 7: Network Security (P86–P95)

**P86.** RSA: $p=3, q=11$. Find public and private keys with $e=7$.
$n = 33, \phi(n) = 2 \times 10 = 20$
$\gcd(7, 20) = 1$ ✓
$d$: $7d \equiv 1 \pmod{20}$ → $d = 3$ (since $7 \times 3 = 21 = 1 \times 20 + 1$)
Public: (7, 33), Private: (3, 33)

Encrypt M=5: $C = 5^7 \mod 33 = 78125 \mod 33 = 14$
Decrypt: $M = 14^3 \mod 33 = 2744 \mod 33 = 5$ ✓

**P87.** DH: $p=11, g=2$. Alice: $a=3$, Bob: $b=5$.
$A = 2^3 \mod 11 = 8$
$B = 2^5 \mod 11 = 32 \mod 11 = 10$
$K_{Alice} = 10^3 \mod 11 = 1000 \mod 11 = 10$
$K_{Bob} = 8^5 \mod 11 = 32768 \mod 11 = 10$ ✓

**P88.** Digital signature provides: Authentication, Integrity, Non-repudiation. It does NOT provide Confidentiality.

**P89.** Firewall rule: "Allow TCP from any to 10.0.0.1 port 80." What does this allow?
Allows HTTP traffic to web server at 10.0.0.1. Blocks all other ports and protocols to that server.

**P90.** IPSec transport mode vs tunnel mode:
- Transport: Original IP header preserved, only payload encrypted. For end-to-end.
- Tunnel: Entire original packet encrypted, new IP header added. For VPN/gateway.

**P91.** AES key sizes: 128, 192, or 256 bits. Block size: always **128 bits**.

**P92.** Man-in-the-middle attack is possible in DH because there's no authentication of the parties. Fix: use **authenticated DH** (with digital signatures or certificates).

**P93.** Why is ECB mode insecure?
Same plaintext block always produces same ciphertext block → patterns visible. Use CBC, CTR, or GCM instead.

**P94.** SSL/TLS handshake steps:
1. ClientHello (supported cipher suites, random number)
2. ServerHello (chosen cipher suite, random number, certificate)
3. Client verifies certificate, generates pre-master secret, encrypts with server's public key
4. Both derive session keys from pre-master secret + random numbers
5. ChangeCipherSpec + Finished messages

**P95.** In RSA, if $e = 1$, then $C = M^1 = M$ → **no encryption!** $e$ must be > 1 and coprime to $\phi(n)$.

### Practice Set 8: Mixed GATE-Style MCQs (P96–P120)

**P96.** Which protocol operates at the network layer?
(a) FTP (b) TCP (c) **ICMP** (d) SMTP
Answer: (c) — ICMP is encapsulated directly in IP.

**P97.** Slotted ALOHA max throughput is approximately:
(a) 18% (b) **37%** (c) 50% (d) 100%

**P98.** TCP is a _____ protocol.
(a) Connectionless, unreliable (b) Connection-oriented, unreliable
(c) **Connection-oriented, reliable** (d) Connectionless, reliable

**P99.** Which of the following is NOT a function of the network layer?
(a) Routing (b) Forwarding (c) Fragmentation (d) **Flow control**
Answer: (d) — Flow control is transport layer.

**P100.** The minimum number of bits needed for sequence numbers in Selective Repeat with window size 4 is:
(a) 2 (b) **3** (c) 4 (d) 8
SR: $2W \leq 2^n$ → $8 \leq 2^n$ → $n \geq 3$

**P101.** ARP maps:
(a) MAC to IP (b) **IP to MAC** (c) Domain name to IP (d) Port to process

**P102.** Which layer adds MAC address?
(a) Physical (b) **Data Link** (c) Network (d) Transport

**P103.** DHCP uses:
(a) TCP (b) **UDP** (c) ICMP (d) ARP

**P104.** Maximum hosts in a /20 network:
$2^{12} - 2 = 4094$

**P105.** If Hamming code has 7 total bits, data bits =?
$m + r = 7$, $2^r \geq m + r + 1 = 8$ → $r \geq 3$. So $r = 3, m = 4$. **(7,4) Hamming code**

**P106.** CRC with generator polynomial of degree 5. Number of check bits appended:
**5 bits**

**P107.** IPv6 address size:
**128 bits** (vs IPv4's 32 bits)

**P108.** Which routing protocol uses TCP?
(a) RIP (b) OSPF (c) **BGP** (Port 179) (d) None

**P109.** Token bucket: $r=100$ packets/sec, $b=500$ packets, link rate = 1000 packets/sec. Max burst duration?
$t = 500/(1000-100) = 500/900 = 0.556$ seconds

**P110.** In TCP 3-way handshake, the second message contains:
(a) SYN only (b) ACK only (c) **SYN + ACK** (d) FIN + ACK

**P111.** Which device separates broadcast domains?
(a) Hub (b) Switch (c) Bridge (d) **Router**

**P112.** Binary exponential backoff: after 3rd collision, max slots to wait:
$2^3 - 1 = 7$ slots (choose from 0 to 7)

**P113.** TCP Reno: on timeout, cwnd is set to:
(a) 0 (b) **1 MSS** (c) ssthresh (d) cwnd/2

**P114.** Manchester encoding provides:
(a) Error correction (b) **Self-clocking** (c) Data compression (d) Encryption

**P115.** The TIME_WAIT duration in TCP is:
(a) MSL (b) **2 × MSL** (c) RTT (d) RTO

**P116.** OSPF is a _____ routing protocol.
(a) Distance vector (b) **Link state** (c) Path vector (d) Hybrid

**P117.** Propagation delay depends on:
(a) Bandwidth (b) Packet size (c) **Distance and medium** (d) Protocol

**P118.** Number of frames created when a 3000-byte packet (header=20) traverses a link with MTU=500:
Max data = $\lfloor 480/8 \rfloor \times 8 = 480$. Fragments = $\lceil 2980/480 \rceil = 7$

**P119.** Which is NOT an application layer protocol?
(a) HTTP (b) DNS (c) **ARP** (d) FTP
ARP operates at the boundary of network/link layer.

**P120.** In Stop-and-Wait, if $a = 10$, efficiency is:
$\eta = 1/(1+20) = 1/21 \approx 4.76\%$

### Practice Set 9: Numerical GATE Problems (P121–P140)

**P121.** Bandwidth = 8 kHz, SNR = 31 dB. Max bit rate?
$\text{SNR} = 10^{3.1} \approx 1259$
$C = 8000 \times \log_2(1260) \approx 8000 \times 10.3 = 82,400$ bps ≈ **82.4 kbps**

**P122.** A network has bandwidth 10 Mbps and propagation delay 10 ms. If S&W is used with 1000-byte frames, find utilization and effective throughput.
$T_{trans} = 8000/10^7 = 0.8$ ms
$a = 10/0.8 = 12.5$
$\eta = 1/(1+25) = 3.85\%$
Throughput = $0.0385 \times 10 = 385$ kbps

**P123.** Host A sends 5 TCP segments of 100 bytes each, starting with seq=200. What are the sequence numbers?
Seg 1: seq=200, Seg 2: seq=300, Seg 3: seq=400, Seg 4: seq=500, Seg 5: seq=600
ACK for all: ack=700

**P124.** Ethernet frame size range: **64–1518 bytes** (excluding preamble and SFD)

**P125.** Link: 100 Mbps, 1000 km, speed = $2 \times 10^8$ m/s. Bandwidth-delay product?
$T_{prop} = 1000 \times 10^3 / (2 \times 10^8) = 5$ ms
$\text{BDP} = 10^8 \times 5 \times 10^{-3} = 500,000$ bits = 62.5 KB

**P126.** Pure ALOHA: frame time = 2 ms. What is the vulnerable period?
Vulnerable period = $2 \times 2 = 4$ ms

**P127.** GBN with 4-bit sequence number. Maximum sender window size?
$W_{max} = 2^4 - 1 = 15$

**P128.** SR with 4-bit sequence number. Maximum sender window size?
$W_{max} = 2^3 = 8$

**P129.** IP packet: Total Length = 3000 bytes, Header = 20 bytes, ID = 444, DF = 0, MF = 0.
Sent over MTU = 1000 link. Fragment details:
Max data = $\lfloor 980/8 \rfloor \times 8 = 976$
Frags = $\lceil 2980/976 \rceil = 4$ (976 + 976 + 976 + 52)
| Frag | TL | ID | MF | Offset |
|---|---|---|---|---|
| 1 | 996 | 444 | 1 | 0 |
| 2 | 996 | 444 | 1 | 122 |
| 3 | 996 | 444 | 1 | 244 |
| 4 | 72 | 444 | 0 | 366 |

**P130.** Token bucket: rate = 1 Mbps, bucket = 10 Mb, link = 5 Mbps. Max data in 5 sec?
$M = 10 + 1 \times 5 = 15$ Mb

**P131.** TCP Reno: cwnd = 32 MSS, 3 dup ACKs. New cwnd immediately after?
$\text{ssthresh} = 16$, $\text{cwnd} = 16 + 3 = 19$ MSS

**P132.** How many levels needed for Nyquist capacity of 64 kbps with bandwidth 8 kHz?
$64000 = 2 \times 8000 \times \log_2 L$
$\log_2 L = 4$ → $L = 16$ levels

**P133.** 4-bit CRC: How many errors can it detect in a burst?
CRC of degree 4 → detects all burst errors of length ≤ 4 bits.

**P134.** Hamming code for 16 data bits: parity bits needed?
$2^r \geq 16 + r + 1 = 17 + r$
$r=5$: $32 \geq 22$ ✓ → **5 parity bits**, total = 21 bits

**P135.** HTTP 1.1 default: **persistent connections** (no need to specify `Connection: keep-alive`)

**P136.** A router has 4 ports. How many collision and broadcast domains?
**4 collision domains, 4 broadcast domains** (router separates both)

**P137.** Given two hosts on same /24 network with a hub and a switch:
- With hub: 1 collision domain, 1 broadcast domain
- With switch: 2 collision domains, 1 broadcast domain

**P138.** TCP uses **cumulative acknowledgments**. ACK number = next expected byte.

**P139.** Why does TCP use 3-way handshake instead of 2-way?
2-way: server can't confirm client received SYN-ACK → half-open connections possible. 3rd message (ACK) confirms both sides are ready.

**P140.** ICMP Time Exceeded (type 11) is sent when:
(a) **TTL = 0** (b) Port unreachable (c) Network down (d) Fragmentation needed

---

## 🎯 GATE CN Tricks & Tips (30 Important Points)

1. **IP trick:** Network address = IP AND Mask. This always works!
2. **Subnetting shortcut:** Block size = $256 - \text{last non-255 octet of mask}$. E.g., mask 255.255.255.192 → block = 256-192 = 64
3. **VLSM:** Always allocate largest subnet first, then subdivide remaining
4. **Fragmentation:** Offset is in units of 8 bytes. Fragment offset × 8 = first byte of data
5. **Fragment trick:** To find original packet size: last fragment offset × 8 + last fragment data = original data size
6. **CRC degree = check bits = zeros appended**
7. **Nyquist vs Shannon:** Take minimum of both for actual capacity
8. **dB conversion:** 3 dB ≈ double, 10 dB = 10x, 20 dB = 100x, 30 dB = 1000x
9. **Stop-and-Wait:** If $a >> 1$, efficiency ≈ $\frac{1}{2a}$ → nearly zero
10. **GBN vs SR:** For same efficiency, SR needs fewer retransmissions but more buffer/complexity
11. **Minimum sequence numbers:** GBN = $W+1$, SR = $2W$
12. **CSMA/CD:** Ethernet min frame = RTT × bandwidth
13. **Pure vs Slotted ALOHA:** Slotted = 2× throughput of Pure
14. **TCP cwnd in Slow Start:** After $n$ RTTs, cwnd = $2^n$ MSS
15. **Data sent in SS:** $2^0 + 2^1 + ... + 2^{n-1} = 2^n - 1$ MSS
16. **Tahoe vs Reno:** Both same on timeout. Different on 3 dup ACKs (Tahoe: cwnd=1, Reno: cwnd=ssthresh+3)
17. **TCP sequence numbers:** ACK number = next expected byte. ACK 500 means all bytes up to 499 received.
18. **3-way handshake consumes 1 seq number** (SYN uses one). Data transfer starts at seq+1.
19. **FIN also consumes 1 sequence number**
20. **HTTP non-persistent:** 2 RTT per object. Persistent with pipelining: much faster
21. **DNS:** Iterative = client does all queries. Recursive = server does all queries.
22. **ARP = broadcast request, unicast reply.** RARP is obsolete.
23. **Hub = L1 (no intelligence), Switch = L2 (MAC table), Router = L3 (routing table)**
24. **Router separates collision AND broadcast domains. Switch separates ONLY collision domains.**
25. **Token bucket:** Max data in $t$ = $b + rt$. Burst duration = $b/(R-r)$.
26. **IPv6:** No checksum, no fragmentation by routers, no broadcast (multicast only)
27. **RIP max = 15 hops**, OSPF = no limit (within an AS), BGP = inter-AS
28. **Digital signature = sign with private key, verify with public key** (NOT encryption direction!)
29. **Manchester encoding = 2× bandwidth of NRZ** but provides self-clocking
30. **BDP (Bandwidth-Delay Product)** = amount of data "in flight." TCP window ≥ BDP for full utilization.

---

## 🔄 Commonly Confused Pairs in CN

| Concept A | Concept B | Key Difference |
|---|---|---|
| Propagation delay | Transmission delay | Distance/speed vs. Size/bandwidth |
| Bandwidth (Hz) | Bandwidth (bps) | Analog frequency range vs. Digital data rate |
| Hub | Switch | Shared medium vs. Per-port collision domains |
| GBN | SR | Receiver buffers (SR) or discards (GBN) out-of-order |
| TCP | UDP | Reliable/ordered/connected vs. Unreliable/unordered/connectionless |
| Flow control | Congestion control | End-to-end (rwnd) vs. Network-wide (cwnd) |
| Circuit switching | Packet switching | Dedicated path vs. Shared, packet-by-packet |
| FDM | TDM | Frequency division vs. Time division |
| ARP | RARP | IP→MAC vs. MAC→IP (RARP obsolete) |
| Symmetric key | Asymmetric key | Same key both sides vs. Public/Private key pair |
| Authentication | Authorization | Who are you? vs. What can you do? |
| Fragmentation | Segmentation | Network layer (IP) vs. Transport layer (TCP) |
| SMTP | POP3/IMAP | Push (sending) vs. Pull (retrieving) email |
| RIP | OSPF | Distance vector/hop count vs. Link state/cost |
| NAT | PAT | Many-to-many IP mapping vs. Many-to-one using ports |
| Transport mode | Tunnel mode | IPSec end-to-end vs. Gateway-to-gateway |

---

## ⚠️ Common Mistakes in CN (GATE)

| # | Mistake | Correct Understanding |
|---|---|---|
| 1 | Confusing propagation and transmission delay | Propagation = distance/speed (medium property). Transmission = size/bandwidth (link/packet property) |
| 2 | Forgetting fragment offset is in 8-byte units | Offset field stores bytes/8. Multiply by 8 to get actual byte offset |
| 3 | Using $2^n$ as GBN max window | GBN max = $2^n - 1$ (not $2^n$!) |
| 4 | Forgetting SR needs $W_s + W_r \leq 2^n$ | SR typical: $W_s = W_r = 2^{n-1}$ |
| 5 | SNR in dB directly in Shannon formula | Must convert to linear: $\text{SNR} = 10^{dB/10}$ |
| 6 | Mixing up Nyquist baud rate and bit rate | Baud rate = $2B$. Bit rate = $2B \log_2 L$ |
| 7 | TCP ACK means "received byte X" | ACK = next expected byte. ACK 500 = received all up to 499 |
| 8 | Thinking routers look at MAC addresses | Routers use IP addresses (L3). Switches use MAC (L2) |
| 9 | CRC detects all errors | CRC detects all bursts ≤ degree. Can miss some larger bursts |
| 10 | Assuming CSMA/CD works for wireless | Wireless uses CSMA/**CA** (collision avoidance). Can't detect collisions while transmitting over air |
| 11 | Tahoe = Reno | They differ on 3 dup ACKs (same on timeout) |
| 12 | Token bucket rate = link rate | Token rate = sustained rate ≤ link rate. Burst can be at link rate |
| 13 | FTP uses single TCP connection | FTP uses TWO: control (21) and data (20) |
| 14 | Router reduces collision domains | Router reduces BOTH collision AND broadcast domains |
| 15 | Hamming code corrects burst errors | Hamming corrects SINGLE-bit errors only |

---

## 📊 Topic-wise GATE CN Distribution

| Topic | Expected Questions | Expected Marks |
|---|---|---|
| IP Addressing & Subnetting | 1–2 | 2–4 |
| Sliding Window Protocols | 1–2 | 2–4 |
| TCP Congestion Control | 1 | 2 |
| Delays & Throughput | 1 | 1–2 |
| Fragmentation | 0–1 | 1–2 |
| MAC Protocols (ALOHA, CSMA) | 1 | 1–2 |
| Application Layer (HTTP, DNS) | 0–1 | 1–2 |
| Physical Layer (Nyquist, Shannon) | 1 | 1–2 |
| Security (RSA, DH) | 0–1 | 1–2 |
| Routing (Dijkstra, DV) | 0–1 | 1–2 |
| **Total** | **8–12** | **12–20** |

---

## 🧮 Quick Reference Card

### Port Numbers
| Protocol | Port | Transport |
|---|---|---|
| FTP Data | 20 | TCP |
| FTP Control | 21 | TCP |
| SSH | 22 | TCP |
| Telnet | 23 | TCP |
| SMTP | 25 | TCP |
| DNS | 53 | UDP/TCP |
| DHCP | 67/68 | UDP |
| HTTP | 80 | TCP |
| POP3 | 110 | TCP |
| IMAP | 143 | TCP |
| SNMP | 161/162 | UDP |
| HTTPS | 443 | TCP |
| BGP | 179 | TCP |
| RIP | 520 | UDP |
| OSPF | 89 | IP (not TCP/UDP) |

### Protocol Numbers (IP Header)
| Protocol | Number |
|---|---|
| ICMP | 1 |
| TCP | 6 |
| UDP | 17 |
| OSPF | 89 |

### Ethernet Speeds
| Standard | Speed | Min Frame | Max Distance |
|---|---|---|---|
| 10Base-T | 10 Mbps | 64 B | 100m UTP |
| 100Base-TX | 100 Mbps | 64 B | 100m |
| 1000Base-T | 1 Gbps | 64 B (512 B with extension) | 100m |
| 10GBase-T | 10 Gbps | 64 B | 100m |

---

## 📋 Last-Minute Revision Checklist

### Layer-by-layer checklist:
- **Physical:** Nyquist theorem, Shannon theorem, dB conversion, encoding types (NRZ, Manchester, AMI), multiplexing (FDM/TDM/CDMA)
- **Data Link:** Framing (bit/byte stuffing), Hamming code, CRC, delays, sliding window (S&W, GBN, SR), ALOHA, CSMA/CD, CSMA/CA, Ethernet frame, MAC addressing, ARP, STP, VLANs
- **Network:** IP addressing, subnetting/VLSM/supernetting, IPv4 header, fragmentation, ICMP, NAT, routing (Dijkstra, Bellman-Ford), RIP/OSPF/BGP, IPv6
- **Transport:** TCP header, 3-way/4-way handshake, flow control (rwnd), congestion control (SS, CA, Tahoe, Reno), timers, UDP, token/leaky bucket
- **Application:** HTTP (1.0/1.1/2/3), DNS, SMTP/POP3/IMAP, FTP

### Must-memorize values:
- Pure ALOHA max: 1/(2e) ≈ 18.4%
- Slotted ALOHA max: 1/e ≈ 36.8%
- Ethernet min frame: 64 bytes (512 bits for 10 Mbps)
- IPv4 header: min 20 bytes, max 60 bytes
- TCP header: min 20 bytes, max 60 bytes
- UDP header: 8 bytes
- IPv6 header: 40 bytes (fixed)
- MSL in TCP: typically 30 sec or 2 min
- Propagation speed: ~$2 \times 10^8$ m/s in copper/fiber

### Key formulas to write on rough sheet first:
1. $\eta_{SW} = \frac{1}{1+2a}$
2. $a = T_{prop}/T_{trans}$
3. GBN window ≤ $2^n - 1$, SR window ≤ $2^{n-1}$
4. Pure ALOHA: $Ge^{-2G}$, Slotted: $Ge^{-G}$
5. CSMA/CD: $\eta = 1/(1+6.44a)$, $L_{min} = 2T_{prop} \times B$
6. Shannon: $C = B\log_2(1 + \text{SNR})$
7. Token bucket: $M(t) = b + rt$, burst = $b/(R-r)$
8. Fragmentation: offset × 8 = byte position
9. $P$ packets over $k$ links: $(k+P-1)T_{trans} + kT_{prop}$

---

## 🔢 CN Conceptual One-Liners (50 Quick Facts)

1. OSI has 7 layers; TCP/IP has 4 (or 5) layers
2. Router operates at Layer 3 (Network)
3. Switch operates at Layer 2 (Data Link)
4. Hub operates at Layer 1 (Physical) — simply repeats signals
5. MAC address = 48 bits (6 bytes), globally unique
6. IP address = 32 bits (IPv4) or 128 bits (IPv6)
7. ARP resolves IP → MAC; DNS resolves domain → IP
8. DHCP assigns IP addresses dynamically using DORA
9. TCP provides reliable, ordered, byte-stream delivery
10. UDP provides best-effort, unordered, message-based delivery
11. TCP uses 3-way handshake (SYN, SYN-ACK, ACK) to establish
12. TCP uses 4-way handshake (FIN, ACK, FIN, ACK) to close
13. MSS = Maximum Segment Size (typically 1460 bytes for Ethernet)
14. MTU = Maximum Transmission Unit (1500 bytes for Ethernet)
15. IP fragmentation happens at routers; reassembly at destination only
16. TTL prevents packets from looping forever
17. ICMP is used by ping (Echo/Reply) and traceroute (Time Exceeded)
18. NAT translates private ↔ public IP addresses
19. Subnet mask: 1s for network, 0s for host
20. Default gateway = router that forwards packets outside the subnet
21. CRC is the strongest error detection among parity, checksum, CRC
22. Hamming code can correct 1 error and detect 2 errors
23. Bit stuffing: insert 0 after five 1s. Byte stuffing: escape special bytes
24. Manchester encoding guarantees a transition every bit → self-clocking
25. Propagation delay depends on distance and medium speed
26. Transmission delay depends on packet size and bandwidth
27. Stop-and-Wait is simplest but most wasteful protocol
28. Go-Back-N wastes bandwidth retransmitting already-received frames
29. Selective Repeat is most efficient but needs receiver buffering
30. CSMA/CD is for wired (Ethernet); CSMA/CA is for wireless (Wi-Fi)
31. Collision domain ends at switch port or router port
32. Broadcast domain ends at router port only
33. VLANs create multiple broadcast domains on a single switch
34. Spanning Tree (STP) prevents loops in switched networks
35. RIP uses hop count (max 15); OSPF uses cost; BGP uses path vector
36. OSPF supports hierarchical routing with areas
37. BGP is the protocol that runs the Internet's inter-AS routing
38. Dijkstra's algorithm finds shortest paths in link-state routing
39. Bellman-Ford equation: $D_x(y) = \min_v\{c(x,v) + D_v(y)\}$
40. Count-to-infinity is a DV routing problem; solved by split horizon
41. Token bucket allows bursts; leaky bucket enforces constant rate
42. TCP Slow Start: cwnd doubles each RTT (exponential)
43. TCP Congestion Avoidance: cwnd +1 each RTT (linear)
44. Fast Retransmit: retransmit on 3 dup ACKs (don't wait for timeout)
45. TCP flow control uses receiver window (rwnd)
46. Effective window = min(cwnd, rwnd)
47. HTTP/1.1 default = persistent connections
48. DNS queries typically use UDP; zone transfers use TCP
49. FTP uses two connections: control (21) and data (20)
50. IPSec ESP provides encryption + authentication; AH provides only authentication

---

## 📐 Network Calculation Shortcuts

### Powers of 2 Quick Reference:
| $2^n$ | Value | Use |
|---|---|---|
| $2^7$ | 128 | /25 block |
| $2^8$ | 256 | /24 block (full Class C) |
| $2^{10}$ | 1024 ≈ 1K | /22 gives ~1K hosts |
| $2^{16}$ | 65,536 ≈ 64K | /16 block (Class B), port range |
| $2^{20}$ | 1,048,576 ≈ 1M | |
| $2^{24}$ | 16,777,216 ≈ 16M | /8 block (Class A) |
| $2^{32}$ | 4,294,967,296 ≈ 4G | Total IPv4 addresses |

### Log₂ Quick Values:
$\log_2 3 \approx 1.58$, $\log_2 5 \approx 2.32$, $\log_2 10 \approx 3.32$, $\log_2 100 \approx 6.64$, $\log_2 1000 \approx 9.97$

### Speed of Light/Electromagnetic Waves:
- Vacuum: $3 \times 10^8$ m/s
- Fiber/Copper: $2 \times 10^8$ m/s (unless stated otherwise)

### Quick Unit Conversions:
- 1 KB = 8 Kb = $2^{10}$ bytes ≈ 1000 bytes
- 1 MB = $10^6$ bytes = $8 \times 10^6$ bits
- 1 Mbps = $10^6$ bits/sec = 125 KB/sec
- 1 ms = $10^{-3}$ sec, 1 μs = $10^{-6}$ sec

---

## 🔁 Protocol Comparison Tables

### Error Detection Methods:
| Method | Detects | Corrects | Overhead |
|---|---|---|---|
| Parity (1 bit) | 1 error (odd) | None | 1 bit |
| 2D Parity | Up to 3 | 1 | $\sqrt{n}$ bits |
| Checksum | Many | None | 16 bits |
| CRC-$r$ | All burst ≤ $r$ | None | $r$ bits |
| Hamming | All 1+2 | 1 | $O(\log n)$ bits |

### Transport Protocol Comparison:
| Feature | TCP | UDP | SCTP |
|---|---|---|---|
| Connection | Yes | No | Yes |
| Reliability | Yes | No | Yes |
| Ordering | Yes | No | Partial (within streams) |
| Flow Control | Yes | No | Yes |
| Congestion Control | Yes | No | Yes |
| Header Size | 20–60 B | 8 B | 12 B |
| Multi-homing | No | No | Yes |

### Switching Comparison:
| Feature | Circuit | Packet (Datagram) | Packet (VC) |
|---|---|---|---|
| Setup | Yes | No | Yes |
| Path | Fixed | Variable | Fixed |
| Bandwidth | Reserved | Shared | Shared |
| Ordering | Guaranteed | Not guaranteed | Guaranteed |
| Example | Telephone | IP/Internet | ATM, MPLS |

---

## 🧩 CN Concept Flowcharts

### Packet Forwarding Decision:
```
Incoming Packet
      │
      ▼
Read Destination IP
      │
      ▼
Check Routing Table
(Longest Prefix Match)
      │
      ├── Match found → Forward to next hop
      │                 (decrement TTL, recompute checksum)
      │                 │
      │                 ├── TTL = 0? → Send ICMP Time Exceeded
      │                 ├── MTU < packet? → Fragment (if DF=0)
      │                 │                  → Send ICMP Fragmentation Needed (if DF=1)
      │                 └── Forward on outgoing interface
      │
      └── No match → Check default route
                     │
                     ├── Default exists → Forward
                     └── No default → Send ICMP Dest Unreachable
```

### TCP Connection Lifecycle:
```
Client                              Server
CLOSED                              LISTEN
  │                                    │
  │──── SYN ─────────────────────────→│
SYN_SENT                              │
  │                                 SYN_RCVD
  │←──── SYN+ACK ────────────────────│
  │                                    │
  │──── ACK ─────────────────────────→│
ESTABLISHED ◄──────────────────────► ESTABLISHED
  │                                    │
  │ ← Data transfer in both ways →    │
  │                                    │
  │──── FIN ─────────────────────────→│
FIN_WAIT_1                             │
  │                                 CLOSE_WAIT
  │←──── ACK ────────────────────────│
FIN_WAIT_2                             │
  │                                 LAST_ACK
  │←──── FIN ────────────────────────│
  │                                    │
  │──── ACK ─────────────────────────→│
TIME_WAIT                           CLOSED
  │
  │ (wait 2×MSL)
  │
CLOSED
```

### TCP Congestion Control State Machine:
```
                    ┌──────────────┐
                    │  Slow Start  │
                    │ cwnd doubles │
                    └───────┬──────┘
                            │
                   cwnd ≥ ssthresh
                            │
                            ▼
                    ┌──────────────────┐
                    │ Cong. Avoidance  │
                    │ cwnd += 1/RTT    │
                    └───────┬──────────┘
                            │
              ┌─────────────┼─────────────┐
              │                           │
         Timeout                   3 Dup ACKs
              │                           │
              ▼                           ▼
    ssthresh = cwnd/2          ┌──────────────────┐
    cwnd = 1 MSS               │ Fast Recovery    │
    → Slow Start               │ ssthresh=cwnd/2  │
                               │ cwnd=ssthresh+3  │
                               └────────┬─────────┘
                                        │
                                   New ACK
                                        │
                                        ▼
                               cwnd = ssthresh
                               → Cong. Avoidance
```

---

## 📝 Study Strategy for CN

1. **Start with IP addressing** — most frequent in GATE, pure calculation
2. **Master delay calculations** — every GATE has at least one
3. **Sliding window efficiency** — know all three protocols' formulas cold
4. **TCP congestion** — trace cwnd through scenarios (Tahoe AND Reno)
5. **Fragmentation** — practice offset calculations
6. **Physical layer** — Nyquist/Shannon are straightforward but easy points
7. **Application layer** — HTTP timing problems are computation-heavy
8. **Security** — RSA and DH small-number problems appear frequently
9. **MAC protocols** — ALOHA throughput is quick calculation
10. **Practice mental math** — CN is computation-heavy; speed matters

---

## Deep Dive: Bridge Learning & Forwarding

### Bridge/Switch MAC Address Table Learning

When a frame arrives on a port, the switch:
1. **Learns:** Records source MAC → incoming port in MAC table
2. **Forwards:** Looks up destination MAC in table
   - Found → forward to specific port (unicast)
   - Not found → flood to all ports except incoming (unknown unicast)
   - Broadcast → flood to all ports except incoming

**Worked Example — Bridge Learning:**

Bridge has 3 ports: P1, P2, P3. Initially table is empty.

| Step | Frame (Src→Dst) | Port In | Learn | Action | MAC Table After |
|---|---|---|---|---|---|
| 1 | A→B | P1 | A→P1 | Flood (B unknown) | A:P1 |
| 2 | B→A | P2 | B→P2 | Forward to P1 (A known) | A:P1, B:P2 |
| 3 | C→A | P3 | C→P3 | Forward to P1 (A known) | A:P1, B:P2, C:P3 |
| 4 | A→C | P1 | A→P1 (refresh) | Forward to P3 (C known) | A:P1, B:P2, C:P3 |
| 5 | D→E | P2 | D→P2 | Flood (E unknown) | A:P1, B:P2, C:P3, D:P2 |

**Key Points:**
- Entries have aging timer (default 300 sec in most switches)
- If a host moves to a different port, the table updates
- Broadcast frames (FF:FF:FF:FF:FF:FF) are always flooded

### Multiple Switches — Learning Scenario

```
        Switch 1               Switch 2
P1  P2  P3                P1  P2  P3
|   |   |                 |   |   |
A   B   ├────Link────────-┤   D   E
        |                 |
        └────────────────-┘
  (S1 Port3 ↔ S2 Port1)
```

When A sends to D:
1. S1 learns A on P1. D unknown → floods to P2, P3
2. Frame arrives at S2 on P1. S2 learns A on P1 (from its perspective). D unknown → floods to P2, P3
3. D receives. D replies: S2 learns D on P2. A is known on P1 → forward
4. Frame arrives at S1 on P3. S1 learns D on P3 (because frame came from S2). A is known on P1 → forward

After this exchange, both switches have correct entries!

---

## Deep Dive: TCP State Transitions — Extended

### TCP States (Full List):

| State | Description |
|---|---|
| CLOSED | No connection |
| LISTEN | Server waiting for SYN |
| SYN_SENT | Client sent SYN, waiting for SYN-ACK |
| SYN_RCVD | Server received SYN, sent SYN-ACK |
| ESTABLISHED | Connection active, data transfer |
| FIN_WAIT_1 | Sent FIN, waiting for ACK |
| FIN_WAIT_2 | Received ACK for FIN, waiting for peer's FIN |
| CLOSE_WAIT | Received FIN, sent ACK, waiting for application close |
| LAST_ACK | Sent FIN, waiting for ACK |
| TIME_WAIT | Waiting 2×MSL after final ACK |
| CLOSING | Both sides sent FIN simultaneously |

### Simultaneous Close:
```
Host A                          Host B
ESTABLISHED                     ESTABLISHED
  │── FIN ─────────────────────→│
FIN_WAIT_1                       │
  │                              │── FIN (crosses in transit)
  │←── FIN ──────────────────────│
  │                           FIN_WAIT_1
CLOSING                       CLOSING
  │── ACK ─────────────────────→│
  │←── ACK ──────────────────────│
TIME_WAIT                     TIME_WAIT
  │                              │
  (2×MSL)                       (2×MSL)
CLOSED                        CLOSED
```

### Why TIME_WAIT is 2×MSL:
- MSL = Maximum Segment Lifetime (~30 sec to 2 min)
- 2×MSL ensures that:
  1. If final ACK is lost, peer can retransmit FIN and receive ACK
  2. All old segments from this connection expire before sockets can be reused
- The "last ACK might be lost" scenario: takes at most MSL for the lost ACK + MSL for the retransmitted FIN = 2×MSL

### TCP RST (Reset):
Sent when:
- Connection request to non-listening port
- Abort an existing connection
- Response to segment on a half-open connection
- RST is **not acknowledged** — just accepted and connection destroyed

---

## Deep Dive: Subnetting Decision Trees

### Class-based vs Classless:

```
IP Address received
      │
      ▼
GATE specifies class? ──Yes──→ Use class rules
      │                         (A: /8, B: /16, C: /24)
      No
      │
      ▼
Use CIDR notation given → /n prefix
      │
      ▼
Calculate: hosts = 2^(32-n) - 2
           block size = 2^(32-n)
           network = IP AND mask
           broadcast = network OR (NOT mask)
```

### Subnetting Decision:
```
Need to subnet?
      │
      ▼
Fixed subnets (all same size)? ──Yes──→ Standard subnetting
      │                                 s bits borrowed, 2^s subnets
      No
      │
      ▼
Variable sizes needed? ──Yes──→ VLSM
      │                         Sort by size (largest first)
      │                         Allocate sequentially
      No
      │
      ▼
Need to combine networks? ──Yes──→ Supernetting
                                    Check: contiguous, power of 2,
                                    aligned to block size
```

---

## Deep Dive: Network Layer Protocols Extended

### ICMP Message Types — Complete Reference

| Type | Code | Message | Notes |
|---|---|---|---|
| 0 | 0 | Echo Reply | Response to ping |
| 3 | 0 | Net Unreachable | Routing failure |
| 3 | 1 | Host Unreachable | No ARP reply |
| 3 | 2 | Protocol Unreachable | Unknown transport protocol |
| 3 | 3 | Port Unreachable | No process listening |
| 3 | 4 | Fragmentation Needed + DF set | Path MTU Discovery |
| 4 | 0 | Source Quench | Congestion (deprecated) |
| 5 | 0 | Redirect | Better route available |
| 8 | 0 | Echo Request | Ping |
| 11 | 0 | TTL Exceeded in Transit | Traceroute |
| 11 | 1 | Fragment Reassembly Time Exceeded | Fragments didn't arrive in time |
| 12 | 0 | Parameter Problem | Bad IP header |

### Path MTU Discovery:
1. Source sends packet with DF (Don't Fragment) bit set
2. If router can't forward (MTU < packet size), sends ICMP type 3, code 4 with the MTU of the next link
3. Source reduces packet size and retries
4. Repeat until packet reaches destination
5. Source caches discovered path MTU

### IPv4 Options:
- **Record Route:** Each router adds its IP address
- **Timestamp:** Each router adds time
- **Source Route (Loose/Strict):** Source specifies route
  - Strict: packet MUST follow exact route
  - Loose: packet MUST visit specified routers (can take any path between them)

---

## Deep Dive: Application Protocol Details

### HTTP Methods in Detail:

| Method | Idempotent | Safe | Body | Purpose |
|---|---|---|---|---|
| GET | Yes | Yes | No | Retrieve resource |
| HEAD | Yes | Yes | No | Get headers only |
| POST | No | No | Yes | Submit data (create) |
| PUT | Yes | No | Yes | Replace resource |
| DELETE | Yes | No | No | Delete resource |
| PATCH | No | No | Yes | Partial update |
| OPTIONS | Yes | Yes | No | Get supported methods |

### HTTP Status Codes:

| Range | Category | Examples |
|---|---|---|
| 1xx | Informational | 100 Continue |
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirection | 301 Moved Permanently, 302 Found, 304 Not Modified |
| 4xx | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| 5xx | Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

### HTTP Caching:
- **Cache-Control:** max-age, no-cache, no-store
- **ETag:** Hash of resource for conditional requests
- **If-None-Match:** Client sends ETag; if matched → 304 Not Modified (no body)
- **If-Modified-Since:** Check by date; if unchanged → 304

### HTTP/2 Features:
- **Binary framing:** More efficient parsing
- **Multiplexing:** Multiple requests/responses on single TCP connection (no head-of-line blocking at HTTP level)
- **Header compression:** HPACK algorithm
- **Server push:** Server sends resources before client requests them
- **Stream prioritization:** Client can assign priorities

### HTTP/3:
- Uses **QUIC** protocol (over UDP instead of TCP)
- Built-in TLS 1.3 encryption
- Eliminates head-of-line blocking (per-stream flow control)
- Faster connection establishment (0-RTT or 1-RTT)

### DNS Extended:

**DNS Query Process — Detailed:**

```
Application calls getaddrinfo("www.example.com")
      │
      ▼
Check local DNS cache (browser cache, OS cache)
      │
      ├── Found? → Return cached IP
      │
      └── Not found → Query local DNS resolver
            │
            ▼
      Local resolver checks its cache
            │
            ├── Found? → Return cached IP
            │
            └── Not found → Start iterative/recursive resolution
                  │
                  ├── Iterative: resolver queries root → TLD → auth NS
                  │
                  └── Recursive: resolver asks upstream (ISP), which does all work
```

**DNS Zone Transfer:**
- Full zone transfer (AXFR): copies entire zone file
- Incremental zone transfer (IXFR): copies only changes
- Uses TCP (port 53) because zone files can be large
- Only authorized secondary servers can request transfers

---

## Deep Dive: Routing Algorithms Extended

### Dijkstra with Negative Edges?
Dijkstra does NOT work with **negative edge weights** (may find incorrect shortest paths). Use **Bellman-Ford** for negative edges (detects negative cycles too).

### Distance Vector: Split Horizon with Poison Reverse

**Without Split Horizon:** A tells B its distance to C = 2 (even though A learns this via B). If B-C link fails, B thinks "A can reach C for 2, so I can reach via A for 3..." → count-to-infinity.

**With Split Horizon:** A tells B its distance to C = ∞ (since A's route to C goes through B). If B-C link fails, B immediately knows C is unreachable from all neighbors.

**Limitations:** Split horizon doesn't solve 3-node loops.

```
A --1-- B
|       |
1       1
|       |
C --1-- D
```
If link B-D fails: C still advertises to B that it can reach D in 2 (via A→D if such path exists). Complex loops can still form.

### Count-to-Infinity: Worked Example

Network: A—B—C, all links cost 1.

Initial routing tables:
| | A | B | C |
|---|---|---|---|
| A | 0 | 1 | 2 |
| B | 1 | 0 | 1 |
| C | 2 | 1 | 0 |

**Link B-C fails.** B sets distance to C = ∞.
But A advertises: D_A(C) = 2.

Round 1: B updates: D_B(C) = min(∞, 1 + D_A(C)) = min(∞, 1+2) = 3
B tells A: D_B(C) = 3
Round 2: A updates: D_A(C) = min(∞, 1 + 3) = 4
Round 3: B updates: D_B(C) = min(∞, 1 + 4) = 5
... continues until reaching ∞ (e.g., 16 in RIP)

**Number of rounds to converge:** If ∞ = 16, and distance was 2 initially, it takes 16 - 2 = **14 rounds** to count to infinity.

### OSPF Details:

**OSPF Areas:**
```
       ┌────────────────┐
       │   Area 0       │
       │  (Backbone)    │
       │                │
       │  ABR1    ABR2  │
       └──┤────────┤───-┘
          │        │
     ┌────┤        ├────┐
     │Area 1│      │Area 2│
     │      │      │      │
     └──────┘      └──────┘
```

- **ABR (Area Border Router):** Connects non-backbone area to backbone
- **ASBR (AS Boundary Router):** Connects OSPF domain to external AS
- **Internal Router:** All interfaces in one area
- **Backbone Router:** Has interface in Area 0

**OSPF Packet Types:**
1. Hello — discover/maintain neighbors
2. DBD (Database Description) — summary of LSDB
3. LSR (Link State Request) — request specific LSAs
4. LSU (Link State Update) — actual LSA data
5. LSAck — acknowledge LSAs

**OSPF Cost Formula:** $\text{Cost} = \frac{\text{Reference Bandwidth}}{\text{Interface Bandwidth}}$
Default reference = 100 Mbps.
- 10 Mbps link → cost = 10
- 100 Mbps → cost = 1
- 1 Gbps → cost = 1 (need to change reference for modern networks)

### BGP Route Selection:

When BGP has multiple routes to a destination, it selects using these criteria (in order):

1. Highest LOCAL_PREF (administrative preference)
2. Shortest AS_PATH
3. Lowest ORIGIN type (IGP < EGP < Incomplete)
4. Lowest MED (Multi-Exit Discriminator) from same AS
5. eBGP over iBGP
6. Lowest IGP cost to next hop
7. Lowest router ID (tie-breaker)

---

## Deep Dive: Transport Layer Internals

### TCP Segment Size Negotiation:

MSS (Maximum Segment Size) is negotiated during 3-way handshake:
- Client advertises its MSS in SYN
- Server advertises its MSS in SYN-ACK
- Each side uses the SMALLER of the two for sending

**MSS calculation:**
$\text{MSS} = \text{MTU} - \text{IP header} - \text{TCP header} = 1500 - 20 - 20 = 1460$ bytes (typical)

### TCP Sequence Number Wraparound:

TCP uses 32-bit sequence numbers → max = $2^{32} - 1 \approx 4.3$ GB

At 1 Gbps: wraparound time = $2^{32} \times 8 / 10^9 \approx 34$ seconds!

**Problem:** Old duplicate segment might have same seq number as new segment.
**Solution:** TCP Timestamps option (PAWS — Protection Against Wrapped Sequences)

### Nagle's Algorithm:

```
if (data_to_send < MSS AND unacknowledged_data > 0):
    buffer data (wait for ACK)
else:
    send data immediately
```

Reduces number of tiny segments on the network.
**Conflict with Delayed ACK:** Receiver waits 200 ms before sending ACK (hoping to piggyback). Combined with Nagle → extra 200 ms delay. Solution: disable Nagle for interactive apps (TCP_NODELAY).

### TCP Window Scaling:

Original TCP window field = 16 bits → max window = 65535 bytes = 64 KB
For high-BDP links (e.g., satellite): need larger window.

**Window Scale Option:** Negotiated in SYN/SYN-ACK. Scale factor $S$: actual window = advertised × $2^S$
- $S$ ranges from 0 to 14
- Max window = 65535 × $2^{14}$ = 1,073,725,440 bytes ≈ **1 GB**

### Silly Window Syndrome — Detailed:

**Problem:** Receiver advertises tiny window (e.g., 1 byte). Sender sends 1-byte segment with 40 bytes header → 2.4% efficiency.

**Sender-side fix (Nagle):** Buffer small data
**Receiver-side fix (Clark's/Delayed ACK):**
- Don't advertise window until buffer has ≥ MSS or ≥ half the total buffer
- Delay ACK by up to 200 ms (piggyback)

---

## Deep Dive: Wireless Networks (802.11)

### Hidden Terminal Problem:
```
     A ←     range     → B ←     range     → C
     |────────────|────────────|
     
A and C are out of each other's range but both in B's range.
A transmits to B. C doesn't hear A → thinks channel free → transmits to B → collision at B!
```

**Solution: RTS/CTS**
1. A sends RTS (Request To Send) to B
2. B replies with CTS (Clear To Send) — heard by C
3. C knows channel is busy → defers
4. A transmits data
5. B sends ACK

### Exposed Terminal Problem:
```
     A ←     → B ←     → C ←     → D
     
B transmits to A. C hears B → thinks channel busy → won't transmit to D.
But C → D wouldn't interfere with B → A!
```

C unnecessarily defers → reduced throughput.

### 802.11 Frame Types:
| Type | Subtype | Purpose |
|---|---|---|
| Management | Beacon | AP announces presence |
| Management | Probe Req/Resp | Station discovers APs |
| Management | Auth/Deauth | Authentication |
| Management | Assoc/Disassoc | Association with AP |
| Control | RTS | Request to send |
| Control | CTS | Clear to send |
| Control | ACK | Acknowledgment |
| Data | Data | Payload carrying |

### 802.11 Timing:
```
DIFS → [Data] → SIFS → [ACK]

DIFS (DCF Inter-Frame Spacing) > SIFS (Short Inter-Frame Spacing)

Priority: SIFS < PIFS < DIFS < EIFS
```

- SIFS: highest priority (ACK, CTS)
- DIFS: normal data frames
- Time between end of data and start of ACK = SIFS

---

## Deep Dive: Error Control Protocols Extended

### Stop-and-Wait ARQ — Detailed Timeline with Lost Frame:

```
Sender                              Receiver
  |-- F0 --------→  ✓              |
  |          ←-------- ACK0 -------|
  |-- F1 --------→  ✗ (lost!)     |
  |                                |
  | (timeout!)                     |
  |-- F1 --------→  ✓ (retransmit)|
  |          ←-------- ACK1 -------|
  |-- F0 --------→  ✓              |
```

### Stop-and-Wait with Lost ACK:

```
Sender                              Receiver
  |-- F0 --------→  ✓              |
  |          ←--- ACK0 --✗ (lost!) |
  |                                |
  | (timeout!)                     |
  |-- F0 --------→  ✓ (duplicate!) |
  |              (discard, re-ACK) |
  |          ←-------- ACK0 -------|
  |-- F1 --------→                 |
```

Receiver detects duplicate (same seq number) → discards data but re-sends ACK.

### Go-Back-N — What Happens When Window Exhausted:

Window size = 4, seq bits = 3 (0–7).

```
Sender sends: F0, F1, F2, F3 (window full!)
       Sender WAITS (cannot send F4 until ACK0 arrives)

ACK0 arrives → window slides: can now send F4
ACK1 arrives → window slides: can now send F5
...

If ACK0, ACK1 both lost → Timer for F0 expires → retransmit F0, F1, F2, F3
```

### Selective Repeat — Receiver Buffer Management:

Window = 4. Expected frame = 3.

```
Receive F3 → deliver, advance window to expect F4
Receive F5 → buffer (out of order), send ACK5
Receive F4 → deliver F4, then deliver buffered F5, advance to expect F6
Receive F7 → buffer
Receive F6 → deliver F6, then deliver F7, advance to expect F8
```

### Efficiency with Processing/ACK Delay:

If ACK takes $T_{ack}$ time to transmit:

$$\eta_{SW} = \frac{T_{trans}}{T_{trans} + 2T_{prop} + T_{ack} + T_{proc}}$$

Usually $T_{ack}$ and $T_{proc}$ are negligible, but if given, include them!

---

## Practice Set 10: Rapid-Fire True/False (P141–P170)

**P141.** IPv4 header checksum covers both header and data. → **FALSE** (header only)

**P142.** TCP checksum covers both header and data. → **TRUE**

**P143.** UDP checksum is mandatory in IPv6. → **TRUE** (optional in IPv4)

**P144.** A switch can separate broadcast domains. → **FALSE** (only with VLANs)

**P145.** OSPF routers need to know the entire network topology. → **TRUE** (within their area)

**P146.** ARP request is unicast. → **FALSE** (broadcast)

**P147.** TCP guarantees message boundaries. → **FALSE** (byte stream — no message boundaries)

**P148.** BGP uses UDP. → **FALSE** (uses TCP port 179)

**P149.** IPv6 header has no checksum field. → **TRUE**

**P150.** CRC can correct errors. → **FALSE** (detects only — correction needs retransmission)

**P151.** Manchester encoding has twice the bandwidth of NRZ. → **TRUE**

**P152.** In CSMA/CD, collision detection is impossible in wireless networks. → **TRUE** (hence CSMA/CA)

**P153.** Slotted ALOHA requires synchronization among stations. → **TRUE**

**P154.** In TCP, closing a connection requires 3 segments minimum. → **FALSE** (4 segments for full close; 3 if FIN-ACK piggybacked)

**P155.** Token bucket allows burst of data. → **TRUE** (up to bucket capacity)

**P156.** Dijkstra's algorithm works with negative edge weights. → **FALSE** (use Bellman-Ford)

**P157.** In Go-Back-N, receiver can accept out-of-order frames. → **FALSE** (discards them)

**P158.** DNS primarily uses TCP. → **FALSE** (primarily UDP; TCP for zone transfers and large responses)

**P159.** NAT modifies only source IP in outgoing packets. → **FALSE** (also modifies port in PAT; modifies dest IP in incoming)

**P160.** IPSec AH provides confidentiality. → **FALSE** (only authentication + integrity; ESP provides encryption)

**P161.** HTTP/2 uses multiple TCP connections. → **FALSE** (multiplexes over single TCP connection)

**P162.** The maximum value of TCP window with scaling is approximately 1 GB. → **TRUE** ($65535 \times 2^{14}$)

**P163.** In Selective Repeat, $W_s + W_r > 2^n$ can cause errors. → **TRUE** (ambiguity in old vs new frames)

**P164.** PPP provides error correction. → **FALSE** (only error detection via FCS)

**P165.** DHCP uses TCP. → **FALSE** (uses UDP ports 67/68)

**P166.** A /31 network has 0 usable hosts. → **FALSE** (2 usable for point-to-point links, RFC 3021)

**P167.** Fast Retransmit in TCP Reno requires waiting for timeout. → **FALSE** (triggers on 3 dup ACKs, before timeout)

**P168.** RIP supports VLSM. → **FALSE** (RIPv1 doesn't; RIPv2 does)

**P169.** ICMP is a transport layer protocol. → **FALSE** (network layer, encapsulated in IP)

**P170.** The vulnerable period of Pure ALOHA is twice that of Slotted ALOHA. → **TRUE** ($2T_{frame}$ vs $T_{frame}$)

---

## Practice Set 11: GATE PYQ Simulation (P171–P190)

**P171. [GATE-style]** A host is sending 5000-byte data (no options) over a network. One link has MTU = 1500 bytes and another link after that has MTU = 576 bytes. How many fragments reach the destination?

MTU 1500: max data = 1480. Data = 4980B (assuming 20B header, total = 5000).
Frags: $\lceil 4980/1480 \rceil = 4$ (1480, 1480, 1480, 540)

MTU 576: max data = $\lfloor 556/8 \rfloor \times 8 = 552$
Frag 1 (1480B): $\lceil 1480/552 \rceil = 3$ sub-frags
Frag 2 (1480B): 3 sub-frags
Frag 3 (1480B): 3 sub-frags
Frag 4 (540B): $\lceil 540/552 \rceil = 1$ (fits!)

Total = 3 + 3 + 3 + 1 = **10 fragments**

**P172. [GATE-style]** TCP Reno, initial cwnd=1 MSS, ssthresh=32. After 10 RTTs, what is cwnd?
SS: RTT 1→2, RTT 2→4, RTT 3→8, RTT 4→16, RTT 5→32 (reached ssthresh)
CA: RTT 6→33, RTT 7→34, RTT 8→35, RTT 9→36, RTT 10→37
**Answer: cwnd = 37 MSS**

**P173. [GATE-style]** Link: 10 Mbps, distance 2000 km, propagation speed $2 \times 10^8$ m/s. Frame = 2000 bits. GBN with 3-bit seq numbers. Find throughput.
$T_{trans} = 2000/10^7 = 0.2$ ms
$T_{prop} = 2000000/(2 \times 10^8) = 10$ ms
$a = 10/0.2 = 50$
$N = 2^3 - 1 = 7$, $1 + 2a = 101$
$\eta = 7/101 = 6.93\%$
Throughput = $0.0693 \times 10 = 0.693$ Mbps = **693 kbps**

**P174. [GATE-style]** Bandwidth = 200 kHz, SNR = 15 dB. Find capacity and minimum signal levels.
$\text{SNR} = 10^{1.5} \approx 31.62$
Shannon: $C = 200000 \times \log_2(32.62) = 200000 \times 5.03 = 1,006,000$ bps ≈ **1 Mbps**
For Nyquist: $10^6 = 2 \times 200000 \times \log_2 L$ → $\log_2 L = 2.5$ → $L \approx 6$ → need **8 levels** (nearest power of 2)

**P175. [GATE-style]** TCP connection: MSS = 500 bytes, receiver buffer = 10 KB, RTT = 20 ms. If cwnd = 8 MSS and rwnd = 10 MSS, max throughput?
Effective window = min(8, 10) = 8 MSS = 4000 bytes
Throughput = 4000/0.02 = 200,000 bytes/s = **200 KB/s = 1.6 Mbps**

**P176. [GATE-style]** CRC generator polynomial $G(x) = x^4 + x + 1 = 10011$. Data = 10110011. Find CRC.
Append 4 zeros: 101100110000
Divide by 10011:
```
101100110000 ÷ 10011
10011
-----
 01010
 00000
 -----
  10101
  10011
  -----
   01101
   00000
   -----
    11011
    10011
    -----
     10000
     10011
     -----
      00110
      00000
      -----
       0110  ← CRC
```
CRC = **0110**. Transmitted: 10110011**0110**

**P177. [GATE-style]** Token bucket: token rate = 2 Mbps, bucket capacity = 16 Mb, link = 10 Mbps. How long can data be sent at full link rate?
$t = 16/(10-2) = 16/8 = 2$ seconds

**P178. [GATE-style]** RSA: $n=55$ ($p=5, q=11$), $e=3$. Find $d$ and decrypt $C=8$.
$\phi(55) = 4 \times 10 = 40$
$3d \equiv 1 \pmod{40}$. $d = 27$ (since $3 \times 27 = 81 = 2 \times 40 + 1$)
$M = 8^{27} \mod 55$

$8^2 = 64 \mod 55 = 9$
$8^4 = 81 \mod 55 = 26$
$8^8 = 676 \mod 55 = 676 - 12 \times 55 = 16$
$8^{16} = 256 \mod 55 = 256 - 4 \times 55 = 36$
$8^{27} = 8^{16} \times 8^8 \times 8^2 \times 8^1 = 36 \times 16 \times 9 \times 8$
$= 36 \times 16 = 576 \mod 55 = 576 - 10 \times 55 = 26$
$= 26 \times 9 = 234 \mod 55 = 234 - 4 \times 55 = 14$
$= 14 \times 8 = 112 \mod 55 = 112 - 2 \times 55 = 2$
**M = 2**

**P179. [GATE-style]** In a network with 5 routers, Dijkstra's algorithm takes how many iterations?
$V - 1 = 4$ iterations (source processed first, then 4 more)

**P180. [GATE-style]** Total time for DNS + HTTP: DNS takes 3 RTTs (iterative). HTTP non-persistent for 1 HTML + 3 images. RTT = 100 ms.
DNS: 3 × 100 = 300 ms
HTTP: 4 × 2 × 100 = 800 ms (4 objects × 2 RTT each)
**Total: 1100 ms = 1.1 seconds**

**P181. [GATE-style]** A switch has learned: A on P1, B on P2, C on P3. Frame arrives on P1 with src=D, dst=B. What does the switch do?
1. Learn D on P1. 2. Forward to P2 (B known). 3. Do NOT flood.

**P182. [GATE-style]** DHCP: Which step is NOT broadcast?
All 4 DORA steps are broadcast! (trick question: some implementations unicast the ACK, but standard says broadcast)

**P183. [GATE-style]** A class B network with 10 subnet bits. How many subnets and hosts per subnet?
Class B: /16 original. 10 subnet bits → /26
Subnets: $2^{10} = 1024$
Hosts: $2^6 - 2 = 62$ per subnet

**P184. [GATE-style]** GBN uses cumulative ACK. If ACK 5 is received, it means:
Frames 0, 1, 2, 3, 4 are all acknowledged. (ACK $n$ = all frames up to $n$ received)

**P185. [GATE-style]** What is the bandwidth of Manchester encoding for 100 Mbps Ethernet?
$B = 100$ MHz (Manchester needs bandwidth = bit rate)
NRZ would need only 50 MHz.

**P186. [GATE-style]** IPv6 extension headers are processed by:
- **Hop-by-Hop:** Every router on the path
- **All others:** Only the destination

**P187. [GATE-style]** In Pure ALOHA, if $G = 0.5$, what fraction of time is the channel idle?
$P(\text{idle}) = e^{-2G} = e^{-1} = 0.368 = 36.8\%$
(Idle in pure ALOHA: no transmission attempt in a 2× vulnerable period around a time point)

**P188. [GATE-style]** TCP uses **positive acknowledgment with retransmission (PAR)**. ACK confirms receipt. No ACK within timeout → retransmit.

**P189. [GATE-style]** A packet has TTL=1. What happens when it reaches the first router?
Router decrements TTL to 0 → drops packet → sends **ICMP Time Exceeded** to source.

**P190. [GATE-style]** How many 16-bit words are in the IP header (minimum)?
$20 \text{ bytes} / 2 = 10$ words of 16 bits.
IHL field stores in 32-bit words: $20/4 = 5$.

---

## Practice Set 12: Speed Round — 1-Line Problems (P191–P220)

**P191.** Number of layers in OSI: **7**
**P192.** Number of layers in TCP/IP: **4** (or 5)
**P193.** ARP table maps: **IP → MAC**
**P194.** DNS uses port: **53**
**P195.** TCP uses which IP protocol number: **6**
**P196.** UDP header size: **8 bytes**
**P197.** IPv4 header minimum size: **20 bytes**
**P198.** IPv6 header size: **40 bytes**
**P199.** Hamming distance to detect $d$ errors: **$d+1$**
**P200.** Hamming distance to correct $t$ errors: **$2t+1$**
**P201.** Maximum hop count in RIP: **15** (16 = infinity)
**P202.** OSPF operates on which transport: **IP directly** (protocol 89, no TCP/UDP)
**P203.** Ethernet maximum frame (excluding preamble): **1518 bytes**
**P204.** VLAN tag size: **4 bytes** (802.1Q)
**P205.** Maximum VLANs (12-bit ID): **4094** usable
**P206.** SYN flag occupies: **1 sequence number**
**P207.** FIN flag occupies: **1 sequence number**
**P208.** TCP window field size (without scaling): **16 bits** = 65535 bytes max
**P209.** CIDR introduced in: **1993** (RFC 1519)
**P210.** IPv6 does NOT have: **checksum** in header
**P211.** Which layer does NAT operate at: **Network layer (L3)** (also modifies L4 ports)
**P212.** Broadcast MAC address: **FF:FF:FF:FF:FF:FF**
**P213.** Number of bits in MAC address: **48 bits**
**P214.** Token bucket sustained rate equals: **token generation rate $r$**
**P215.** Shannon capacity depends on: **bandwidth and SNR**
**P216.** Modulation: changes properties of carrier signal (**amplitude, frequency, or phase**)
**P217.** Full duplex means: **simultaneous bidirectional communication**
**P218.** Half duplex means: **bidirectional but only one direction at a time**
**P219.** Simplex means: **unidirectional only**
**P220.** BDP stands for: **Bandwidth-Delay Product**

---

## 🔐 All Important Protocol Acronyms

| Acronym | Full Form |
|---|---|
| ARP | Address Resolution Protocol |
| BGP | Border Gateway Protocol |
| CRC | Cyclic Redundancy Check |
| CSMA | Carrier Sense Multiple Access |
| DHCP | Dynamic Host Configuration Protocol |
| DNS | Domain Name System |
| FTP | File Transfer Protocol |
| HTTP | HyperText Transfer Protocol |
| ICMP | Internet Control Message Protocol |
| IGMP | Internet Group Management Protocol |
| IMAP | Internet Message Access Protocol |
| IP | Internet Protocol |
| IPSec | Internet Protocol Security |
| LAN | Local Area Network |
| MAC | Media Access Control |
| MTU | Maximum Transmission Unit |
| MSS | Maximum Segment Size |
| NAT | Network Address Translation |
| NDP | Neighbor Discovery Protocol |
| NRZ | Non-Return to Zero |
| OSPF | Open Shortest Path First |
| PAT | Port Address Translation |
| POP3 | Post Office Protocol v3 |
| PPP | Point-to-Point Protocol |
| QoS | Quality of Service |
| RARP | Reverse ARP |
| RIP | Routing Information Protocol |
| RSA | Rivest-Shamir-Adleman |
| RTT | Round Trip Time |
| SMTP | Simple Mail Transfer Protocol |
| SNMP | Simple Network Management Protocol |
| STP | Spanning Tree Protocol |
| TCP | Transmission Control Protocol |
| TLS | Transport Layer Security |
| TTL | Time To Live |
| UDP | User Datagram Protocol |
| VLAN | Virtual Local Area Network |
| VPN | Virtual Private Network |
| WAN | Wide Area Network |

---

## Deep Dive: IP Addressing Advanced Patterns

### Zero Subnets and All-Ones Subnets

In modern networking (with CIDR), both the zero subnet and all-ones subnet are usable.

**Example:** 192.168.1.0/26 creates 4 subnets:
- 192.168.1.0/26 ← Zero subnet (used)
- 192.168.1.64/26
- 192.168.1.128/26
- 192.168.1.192/26 ← All-ones subnet (used)

Old IOS versions excluded these → only 2 usable subnets. Modern: all 4 usable.

> 🎯 **GATE Trick:** Unless specifically stated otherwise, all subnets (including zero and all-ones) are usable. If question says "following old convention," subtract 2.

### Special IP Addresses

| Address | Purpose |
|---|---|
| 0.0.0.0 | This network (default/unknown source) |
| 255.255.255.255 | Limited broadcast (not forwarded by routers) |
| 127.0.0.0/8 | Loopback (127.0.0.1 most common) |
| 169.254.0.0/16 | Link-local (APIPA — auto-configured when DHCP fails) |
| 10.0.0.0/8 | Private (Class A) |
| 172.16.0.0/12 | Private (Class B range) |
| 192.168.0.0/16 | Private (Class C range) |
| 224.0.0.0/4 | Multicast (Class D) |
| 240.0.0.0/4 | Reserved (Class E) |

### Directed Broadcast vs Limited Broadcast

- **Limited broadcast:** 255.255.255.255 — stays within local subnet, NOT forwarded by routers
- **Directed broadcast:** Network's broadcast address (e.g., 192.168.1.255 for /24) — CAN be forwarded by routers (but usually disabled for security → "Smurf attack" prevention)

### Multicast Addresses

- Class D: 224.0.0.0 to 239.255.255.255
- 224.0.0.1 = All hosts on subnet
- 224.0.0.2 = All multicast routers on subnet
- 224.0.0.5 = All OSPF routers
- 224.0.0.6 = All OSPF designated routers
- 224.0.0.9 = All RIPv2 routers

### CIDR Aggregation — Advanced Problems

**Problem:** An ISP owns 200.200.128.0/17. It has 4 customers:
- C1 needs /20 (4096 addresses)
- C2 needs /21 (2048 addresses)
- C3 needs /22 (1024 addresses)
- C4 needs /24 (256 addresses)

**Allocation:**
ISP range: 200.200.128.0 – 200.200.255.255

| Customer | Block | Start | End |
|---|---|---|---|
| C1 (/20) | 200.200.128.0/20 | 128.0 | 143.255 |
| C2 (/21) | 200.200.144.0/21 | 144.0 | 151.255 |
| C3 (/22) | 200.200.152.0/22 | 152.0 | 155.255 |
| C4 (/24) | 200.200.156.0/24 | 156.0 | 156.255 |
| Available | 200.200.157.0 – 200.200.255.255 | | |

Check alignment:
- C1: 128/16=8 (whole number) ✓ aligned to /20 boundary
- C2: 144/8=18 (whole number) ✓ aligned to /21 boundary
- C3: 152/4=38 (whole number) ✓ aligned to /22 boundary
- C4: 156/1=156 ✓ always aligned

---

## Deep Dive: Data Link Protocols Extended

### HDLC Frame Types Detailed

**I-Frame (Information):** Carries data + piggybacked ACK
```
| Flag | Address | Control | Data | FCS | Flag |
|      |         | N(S) P/F N(R) |     |      |     |
```
- N(S) = Send sequence number
- N(R) = Receive sequence number (ACK)
- P/F = Poll/Final bit

**S-Frame (Supervisory):** Flow and error control
- **RR (Receive Ready):** ACK, ready for more
- **RNR (Receive Not Ready):** ACK, but stop sending
- **REJ (Reject):** NAK, retransmit from N(R) — Go-Back-N style
- **SREJ (Selective Reject):** NAK, retransmit only frame N(R) — SR style

**U-Frame (Unnumbered):** Connection setup/teardown
- **SABM:** Set Asynchronous Balanced Mode (connect)
- **DISC:** Disconnect
- **UA:** Unnumbered Acknowledgment
- **FRMR:** Frame Reject

### PPP Authentication Detailed

**PAP (Password Authentication Protocol):**
```
Client → Server: Username + Password (cleartext!)
Server → Client: Accept/Reject
```
- Simple but insecure (passwords sent in plaintext)
- One-time authentication only

**CHAP (Challenge Handshake Authentication Protocol):**
```
Server → Client: Challenge (random number)
Client → Server: Hash(challenge + secret)
Server: Computes Hash(challenge + secret), compares
Server → Client: Accept/Reject
```
- Passwords never sent over the link
- Can re-authenticate periodically
- More secure than PAP

### Error Detection — 2D Parity

Arrange data in rows. Add parity bit to each row AND each column.

**Example:** 4×4 data with even parity:
```
Data:           With 2D parity:
1 0 1 1         1 0 1 1 | 1
0 1 1 0         0 1 1 0 | 0
1 1 0 1         1 1 0 1 | 1
0 0 1 0         0 0 1 0 | 1
                --------+---
                0 0 1 0 | 1
```

2D parity can:
- **Detect:** All 1, 2, and 3 bit errors
- **Correct:** All single-bit errors (row parity identifies row, column parity identifies column)
- **Miss:** Some 4-bit errors in a rectangle pattern

### Adaptive Sliding Window

In practice, TCP uses adaptive window sizing:
1. Start with initial window (usually 10 MSS in modern implementations)
2. Adjust based on congestion signals
3. **AIMD (Additive Increase, Multiplicative Decrease):**
   - Additive Increase: cwnd += 1 MSS per RTT (congestion avoidance)
   - Multiplicative Decrease: cwnd *= 0.5 on loss
4. This creates a "sawtooth" pattern of cwnd over time

```
cwnd
  |         /\      /\      /\
  |        /  \    /  \    /  \
  |       /    \  /    \  /    \
  |      /      \/      \/      \
  |     /                        \
  |    /                          
  |   /                            
  |  /                              
  | /                                
  |/________________________________ time
            Sawtooth Pattern
```

---

## Deep Dive: Congestion Control Variants

### TCP NewReno (Improvement over Reno)

**Problem with Reno:** If multiple packets are lost in one window, Reno exits fast recovery after first new ACK → re-enters slow start.

**NewReno fix:** Stays in fast recovery until ALL packets from the original window are ACKed. On "partial ACK" (acknowledging some but not all lost packets), retransmit next lost packet without leaving fast recovery.

### TCP CUBIC (Default in Linux)

- cwnd growth is a cubic function of time since last loss
- More aggressive than Reno on high-BDP networks
- Formula: $W(t) = C(t - K)^3 + W_{max}$
- Where $K = \sqrt[3]{\frac{W_{max} \times \beta}{C}}$, $\beta$ = reduction factor, $C$ = scaling constant

### TCP BBR (Bottleneck Bandwidth and RTT)

- Developed by Google
- Explicitly measures bottleneck bandwidth and RTT
- Doesn't rely on packet loss as congestion signal
- Better performance on lossy links (e.g., wireless)

### Comparison:

| Algorithm | Loss Response | Growth | Best For |
|---|---|---|---|
| Tahoe | cwnd=1 always | Slow start | Legacy |
| Reno | cwnd=ssthresh+3 (3 dup ACK) | AIMD | General |
| NewReno | Handles multiple losses | AIMD | Multiple losses |
| CUBIC | cwnd → cubic function | Cubic growth | High BDP |
| BBR | Model-based | Rate-based | Lossy/long links |

---

## Deep Dive: DNS Security & Advanced Topics

### DNS Amplification Attack
1. Attacker sends DNS queries with **spoofed source IP** (victim's IP) to open resolvers
2. DNS responses are much larger than queries (amplification factor ~50x)
3. Victim receives flood of DNS responses → DDoS

**Mitigation:** Rate limiting, response rate limiting (RRL), DNSSEC, blocking open resolvers

### DNSSEC
- Adds digital signatures to DNS records
- Chain of trust from root zone down
- New record types: RRSIG, DNSKEY, DS, NSEC
- Prevents DNS spoofing/cache poisoning
- Does NOT encrypt queries (still plaintext)

### DNS over HTTPS (DoH) / DNS over TLS (DoT)
- Encrypts DNS queries to prevent eavesdropping
- DoH: port 443 (mixed with HTTPS traffic)
- DoT: port 853 (separate, filterable)

---

## Deep Dive: IPv6 Extended

### IPv6 Address Types

| Type | Prefix | Purpose |
|---|---|---|
| Global Unicast | 2000::/3 | Internet routable |
| Link-Local | FE80::/10 | Same link only (auto-configured) |
| Unique Local | FC00::/7 | Like IPv4 private (not globally routable) |
| Multicast | FF00::/8 | One-to-many |
| Loopback | ::1/128 | Self |
| Unspecified | ::/128 | Unknown source (like 0.0.0.0) |

### IPv6 Address Shortening Rules:
1. Remove leading zeros in each group: `2001:0db8:0000:0001` → `2001:db8:0:1`
2. Replace ONE consecutive block of all-zero groups with `::`: `2001:db8:0:0:0:0:0:1` → `2001:db8::1`
3. `::` can only be used ONCE in an address

**Practice:**
- `FE80:0000:0000:0000:0000:0000:0000:0001` → `FE80::1`
- `2001:0DB8:0000:0000:0008:0800:200C:417A` → `2001:DB8::8:800:200C:417A`
- `FF02:0000:0000:0000:0000:0000:0000:0001` → `FF02::1`

### NDP (Neighbor Discovery Protocol) — IPv6's replacement for ARP:

| ICMPv6 Type | Message | Purpose |
|---|---|---|
| 133 | Router Solicitation | Host requests router info |
| 134 | Router Advertisement | Router announces prefix/gateway |
| 135 | Neighbor Solicitation | Like ARP request (multicast, not broadcast) |
| 136 | Neighbor Advertisement | Like ARP reply |
| 137 | Redirect | Better next-hop available |

### IPv6 vs IPv4 Quick Decision:

```
Has IPv6 feature? Check:
✓ 128-bit addresses
✓ Fixed 40-byte header
✓ No checksum in header
✓ No fragmentation by routers
✓ Extension headers instead of options
✓ Multicast replaces broadcast
✓ NDP replaces ARP
✓ IPSec is mandatory
✓ Flow label field (QoS support)
✓ Autoconfiguration (SLAAC)
```

---

## Deep Dive: Network Design Problems

### Problem 1: Design a Network

**Requirements:** Company with 4 departments:
- Engineering: 200 hosts
- Sales: 50 hosts
- HR: 25 hosts
- Management: 10 hosts
- 3 WAN links between offices

Given: 172.16.0.0/16

**Solution using VLSM:**

| Department | Hosts Needed | Prefix | Subnet | Range |
|---|---|---|---|---|
| Engineering | 200 | /24 (254 hosts) | 172.16.0.0/24 | .0.1–.0.254 |
| Sales | 50 | /26 (62 hosts) | 172.16.1.0/26 | .1.1–.1.62 |
| HR | 25 | /27 (30 hosts) | 172.16.1.64/27 | .1.65–.1.94 |
| Management | 10 | /28 (14 hosts) | 172.16.1.96/28 | .1.97–.1.110 |
| WAN Link 1 | 2 | /30 (2 hosts) | 172.16.1.112/30 | .1.113–.1.114 |
| WAN Link 2 | 2 | /30 | 172.16.1.116/30 | .1.117–.1.118 |
| WAN Link 3 | 2 | /30 | 172.16.1.120/30 | .1.121–.1.122 |

Total addresses used: 256 + 64 + 32 + 16 + 12 = 380 out of 65536 available.

### Problem 2: Spanning Tree

**Network:**
```
      S1 (BID: 1)
     /    \
    /      \
   S2       S3
(BID: 3)  (BID: 2)
    \      /
     \    /
      S4 (BID: 4)
```
All link costs = 1.

**Step 1:** Root bridge = S1 (lowest BID)

**Step 2:** Root ports (closest port to root):
- S2: port to S1 (cost 1) → Root Port
- S3: port to S1 (cost 1) → Root Port
- S4: port to S2 (cost 2) or S3 (cost 2) → tie → pick lower BID → via S3 (BID 2) → Root Port

**Step 3:** Designated ports:
- S1–S2 link: S1's port (cost 0 to root) → Designated
- S1–S3 link: S1's port → Designated
- S2–S4 link: S2's port (cost 1) vs S4's port (cost 2) → S2 Designated
- S3–S4 link: S3's port (cost 1) vs S4's port (cost 2) → S3 Designated

**Step 4:** Blocked ports:
- S4's port to S2: **Blocked** (neither root nor designated)

**Result:** Loop between S2–S4–S3 broken by blocking S4–S2 link.

### Problem 3: Routing Table Construction

Given the network:
```
   10.1.0.0/24          10.2.0.0/24
R1 ────────── R2 ────────── R3
│                            │
│ 10.3.0.0/24               │ 10.4.0.0/24
│                            │
LAN-A                      LAN-B
```

**R1's routing table:**

| Destination | Next Hop | Interface | Metric |
|---|---|---|---|
| 10.1.0.0/24 | Connected | eth0 | 0 |
| 10.3.0.0/24 | Connected | eth1 | 0 |
| 10.2.0.0/24 | 10.1.0.2 (R2) | eth0 | 1 |
| 10.4.0.0/24 | 10.1.0.2 (R2) | eth0 | 2 |

**R2's routing table:**

| Destination | Next Hop | Interface | Metric |
|---|---|---|---|
| 10.1.0.0/24 | Connected | eth0 | 0 |
| 10.2.0.0/24 | Connected | eth1 | 0 |
| 10.3.0.0/24 | 10.1.0.1 (R1) | eth0 | 1 |
| 10.4.0.0/24 | 10.2.0.2 (R3) | eth1 | 1 |

---

## Deep Dive: Network Performance Analysis

### Queuing Theory Basics (for GATE)

**M/M/1 Queue:**
- Arrival rate: $\lambda$ packets/sec
- Service rate: $\mu$ packets/sec
- Utilization: $\rho = \lambda/\mu$ (must be < 1 for stability)
- Average packets in system: $L = \frac{\rho}{1-\rho}$
- Average time in system: $T = \frac{1}{\mu - \lambda}$
- Average waiting time: $W = \frac{\rho}{\mu - \lambda}$

**Example:** Router processes packets at 1000 packets/sec. Arrival rate = 800 packets/sec.
$\rho = 0.8$
$L = 0.8/0.2 = 4$ packets in system
$T = 1/(1000-800) = 5$ ms average delay
$W = 0.8/200 = 4$ ms average waiting time

### Little's Law:
$$L = \lambda \times T$$
(Average packets in system = arrival rate × average time in system)

This is model-independent — works for any queuing system!

### Link Utilization:
$$\rho = \frac{\text{Average input rate}}{\text{Link capacity}} = \frac{\lambda \times L_{\text{avg}}}{C}$$

If $\rho > 1$: queue grows without bound → congestion collapse!

---

## Deep Dive: Socket Programming Concepts

### TCP Socket API:

**Server side:**
```
socket()    → Create socket
bind()      → Bind to port
listen()    → Start listening
accept()    → Accept connection (blocks until client connects)
recv()/send() → Data transfer
close()     → Close connection
```

**Client side:**
```
socket()    → Create socket
connect()   → Initiate 3-way handshake
send()/recv() → Data transfer
close()     → Initiate 4-way handshake
```

### Well-known vs Ephemeral Ports:
- **Well-known:** 0–1023 (require root/admin privileges)
- **Registered:** 1024–49151 (assigned by IANA)
- **Ephemeral/Dynamic:** 49152–65535 (auto-assigned to clients)

### TCP vs UDP Socket:
- TCP: `SOCK_STREAM` — reliable, ordered, connection-oriented
- UDP: `SOCK_DGRAM` — unreliable, unordered, connectionless
- UDP doesn't need `listen()`, `accept()`, or `connect()`

---

## Practice Set 13: Network Design & Practical (P221–P240)

**P221.** How many /24 subnets fit in a /16 network?
$2^{24-16} = 2^8 = 256$ subnets

**P222.** Minimum bits for 4000 hosts per subnet?
$2^h - 2 \geq 4000$ → $h = 12$ (4094 hosts). Mask = /20.

**P223.** Network 10.0.0.0/8 has how many /24 subnets?
$2^{24-8} = 2^{16} = 65536$ subnets

**P224.** A company has 3 offices with 500, 200, and 100 hosts. Using 172.16.0.0/16, assign subnets.
- Office 1 (500): /23 (510 hosts) → 172.16.0.0/23
- Office 2 (200): /24 (254 hosts) → 172.16.2.0/24
- Office 3 (100): /25 (126 hosts) → 172.16.3.0/25
- WAN links: two /30 → 172.16.3.128/30, 172.16.3.132/30

**P225.** In a switched network with 3 switches in a triangle, how many ports are blocked by STP?
STP blocks 1 port to break the loop. If all costs equal, the port on the switch with highest BID is blocked.

**P226.** Router receives packet for 192.168.1.1. Routing table:
| Dest | Interface |
|---|---|
| 192.168.1.0/25 | eth0 |
| 192.168.1.0/24 | eth1 |
| 0.0.0.0/0 | eth2 |

192.168.1.1: matches /25 (host in range .0–.127) and /24 (range .0–.255). LPM → **/25 → eth0**

**P227.** How many bits needed in IP fragment offset field for maximum IP packet (65535 bytes)?
Max data = 65535 - 20 = 65515 bytes. Max offset = 65515/8 ≈ 8189. $\lceil \log_2(8190) \rceil = 13$ bits ✓

**P228.** A bridge connects two LANs. LAN 1 has 100 hosts, LAN 2 has 50 hosts. A host on LAN 1 sends broadcast. How many hosts receive it?
Bridge forwards broadcasts → **all 150 hosts** (including 49 others on LAN 1 + 50 on LAN 2) receive it.

**P229.** Network delay: 3 hops, each adds 2 ms processing + 5 ms prop + 1 ms trans delay.
Total = 3 × (2 + 5 + 1) = 24 ms (store-and-forward at each hop)
Actually with pipelining: first packet: 3 × 1 (trans) + 3 × 5 (prop) + 3 × 2 (proc) = 3 + 15 + 6 = 24 ms

**P230.** Silly window: sender has 10 KB to send, receiver advertises window of 100 bytes. What should sender do?
Apply **Nagle's algorithm**: buffer data, wait for larger window advertisement or ACK. Don't send 100-byte segments with 40-byte headers (71% overhead!).

**P231.** A switch has 24 ports. All connected to hosts. How many collision domains and broadcast domains?
24 collision domains, 1 broadcast domain (no VLANs).

**P232.** Same switch with 3 VLANs (8 ports each):
24 collision domains, **3 broadcast domains**.

**P233.** If TCP initial sequence number (ISN) starts at 1000, and 500 bytes are sent, what's the next seq?
Next seq = 1000 + 500 = **1500**

**P234.** TCP: ACK number 5000 means data bytes ______ have been received.
All bytes with sequence numbers **up to 4999** are received. 5000 is the NEXT expected byte.

**P235.** If MTU = 296 and IP header = 20, what's the maximum data per fragment?
$\lfloor (296-20)/8 \rfloor \times 8 = \lfloor 276/8 \rfloor \times 8 = 34 \times 8 = 272$ bytes

**P236.** OSPF: Link with 10 Mbps bandwidth. What's the cost? (reference = 100 Mbps)
Cost = 100/10 = **10**

**P237.** How many DNS RTTs saved if TLD (.com) is cached?
Without cache: Root → TLD → Auth = 3 RTTs
With TLD cached: skip root → TLD → Auth = **2 RTTs** → saves **1 RTT**

**P238.** IPv6 has how many possible addresses?
$2^{128} = 3.4 \times 10^{38}$ addresses

**P239.** For a TCP connection, the first data byte after 3-way handshake has seq = ISN+1. If ISN = 300, first data byte seq = **301**.

**P240.** How many TCP connections for downloading 1 HTML + 5 images using HTTP/1.0?
HTTP/1.0 = non-persistent → **6 TCP connections** (one per object)

---

## Practice Set 14: Cross-Topic Integration (P241–P260)

**P241.** A web server is at 200.1.2.3. Client's DNS has nothing cached. How many RTTs before receiving the HTML page? (Assume 4 DNS queries needed, HTTP/1.1 persistent with pipelining)

DNS: 4 RTTs
TCP handshake: 1 RTT
HTTP GET + response: 1 RTT
**Total: 6 RTTs**

**P242.** Host behind NAT (192.168.1.5) accesses web server (93.184.216.34:80). How many protocol layers are modified by NAT?
NAT modifies: IP header (source IP) + TCP header (source port) + IP checksum + TCP checksum = modifications at **Layer 3 and Layer 4**.

**P243.** Ethernet frame encapsulating an IP packet containing a TCP segment carrying HTTP data. What's the overhead?
Ethernet header: 14 bytes + 4 bytes CRC = 18 bytes (or 26 with preamble/SFD)
IP header: 20 bytes (minimum)
TCP header: 20 bytes (minimum)
Total overhead: 18 + 20 + 20 = **58 bytes** out of max 1518 bytes (96% payload efficiency at max frame)

**P244.** Traceroute sends a packet with TTL=3. Which router generates ICMP Time Exceeded?
The **3rd router** (TTL decremented at each: 3→2→1→0, dropped at 3rd).

**P245.** ARP cache has entry for 10.0.0.5. Packet arrives for 10.0.0.5. Does the host send ARP request?
**No** — uses cached MAC address. ARP request only sent if entry is missing or expired.

**P246.** Switch receives frame with unknown destination MAC. It floods to all ports except incoming. How is this different from a hub?
Hub floods to ALL ports (including incoming). Switch floods to all except incoming. Both result in frame reaching destination, but switch is slightly more efficient (and doesn't waste incoming port bandwidth).

**P247.** Host sends DHCP Discover. What MAC and IP addresses are used?
Source MAC: Host's MAC. Dest MAC: FF:FF:FF:FF:FF:FF (broadcast)
Source IP: 0.0.0.0. Dest IP: 255.255.255.255 (limited broadcast)

**P248.** TCP connection: cwnd=10, rwnd=8, ssthresh=20. How many MSS can sender send?
Effective window = min(cwnd, rwnd) = min(10, 8) = **8 MSS**

**P249.** If the Hamming distance of a code is 5, it can:
- Detect up to **4** errors (d_min - 1 = 4)
- Correct up to **2** errors (⌊(d_min-1)/2⌋ = 2)
- Or detect 3 AND correct 1 (3 + 1 + 1 = 5) ✓

**P250.** A 100 Mbps network uses Manchester encoding. What's the minimum bandwidth and baud rate?
Bandwidth = 100 MHz (Manchester: BW = bit rate)
Baud rate = 200 MBaud (2 signal changes per bit)

Wait — let me reconsider. Manchester:
- Each bit has one transition at midpoint
- Bit rate = baud rate (in terms of symbol rate)
- Signal rate = 2 × bit rate (in terms of transitions)
- Minimum bandwidth = bit rate = **100 MHz**

**P251.** RIP update: Router A has {B:1, C:2, D:3}. Router B sends {A:1, C:3, D:1}. How does A update?
For each destination, A checks: min(current, cost_to_B + B's_distance)
- A→C: min(2, 1+3=4) = **2** (keep existing)
- A→D: min(3, 1+1=2) = **2 via B** (update!)

**P252.** 100Base-TX: What does "100" "Base" "TX" mean?
- 100: 100 Mbps
- Base: Baseband signaling
- TX: Twisted pair, full duplex (X)

**P253.** TCP segment has seq=100, len=0, ACK=500, SYN=1. What does this represent?
SYN with ACK → This is the **SYN-ACK** (second message of 3-way handshake). Server's ISN = 100, acknowledging client's ISN+1 = 500.

**P254.** How does traceroute determine the path?
Sends packets with TTL=1,2,3,... Each hop that drops the packet (TTL=0) sends ICMP Time Exceeded, revealing its IP. Destination responds with ICMP Port Unreachable (UDP version) or Echo Reply (ICMP version).

**P255.** Difference between broadcast and multicast:
- Broadcast: goes to ALL hosts in the network. Wastes bandwidth for hosts not interested.
- Multicast: goes only to hosts that have **joined the multicast group**. More efficient.

**P256.** Why is IPv6 header fixed at 40 bytes?
- Simplifies router processing (no variable-length options parsing)
- Extension headers are chained as separate headers (Next Header field)
- Routers only need to process Hop-by-Hop extension (all others for destination only)

**P257.** What's the total delay to establish a TCP connection AND send one HTTP request?
TCP: 1 RTT (SYN → SYN-ACK → ACK piggybacked with data)
Actually: 1 RTT for SYN/SYN-ACK, then data sent with ACK → total ~**1.5 RTT**
Or counting conservatively: **2 RTTs** (1 handshake + 1 request/response)

**P258.** In CRC, if the transmitted codeword is received without error, dividing by the generator gives remainder:
**0** (no remainder means no error detected)

**P259.** BGP attribute AS_PATH serves dual purpose:
1. **Loop prevention:** Reject routes containing own AS number
2. **Path selection:** Prefer shorter AS paths

**P260.** What transport protocol does OSPF use?
OSPF runs directly on **IP** (protocol number 89). It does NOT use TCP or UDP!

---

## 🎯 Final GATE CN Tips — The Last 20 Points

1. Always draw a timeline for sliding window problems — it prevents mistakes
2. In TCP cwnd tracing, carefully track whether you're in SS or CA at each RTT
3. Fragment offset × 8 = actual byte position — don't forget the ×8!
4. $a = T_{prop}/T_{trans}$ — getting this wrong ruins the entire calculation
5. For VLSM, sort by size DESCENDING before allocating
6. Supernetting: first address MUST be divisible by total block size
7. LPM: longer prefix = more specific = higher priority
8. CSMA/CD minimum frame: $L_{min} = 2 \times T_{prop} \times B$ — the factor is 2!
9. Token bucket: sustained rate = $r$, burst rate = $R$ (link rate), NOT $r+R$
10. TCP window = min(cwnd, rwnd) — never forget rwnd!
11. 3 dup ACKs = 4 copies of same ACK (original + 3 duplicates)
12. In GBN, cumulative ACK $n$ means ALL frames up to $n$ are received
13. Shannon capacity is the HARD limit — no encoding scheme can exceed it
14. Nyquist is the limit for a noiseless channel — real channels use Shannon
15. dB to linear: 10 dB = 10x, 20 dB = 100x, 30 dB = 1000x
16. CRC catches ALL single-bit, double-bit, odd-count errors, and bursts ≤ degree
17. RSA: public key encrypts/verifies, private key decrypts/signs
18. In DH, shared secret = $g^{ab} \mod p$ — both parties compute this independently
19. Ethernet frame: min 64B, max 1518B (data only), header = 14B, trailer = 4B
20. When in doubt about "which layer," remember: data → L7, segment → L4, packet → L3, frame → L2, bits → L1

---

## Deep Dive: Ethernet Evolution & Advanced Switching

### Ethernet Frame Processing at a Switch

```
Frame arrives on port P1
      │
      ▼
Read Source MAC
      │
      ▼
Update MAC table: Source MAC → P1 (with timestamp)
      │
      ▼
Read Destination MAC
      │
      ├── Broadcast/Multicast → Flood to all ports except P1
      │
      ├── Unknown unicast → Flood to all ports except P1
      │
      ├── Known unicast, same port as source → Drop (filtering)
      │
      └── Known unicast, different port → Forward to that port only
```

### Switch vs Bridge vs Router — Deep Comparison

| Feature | Hub | Bridge | Switch | Router |
|---|---|---|---|---|
| OSI Layer | 1 | 2 | 2 | 3 |
| Address Used | None | MAC | MAC | IP |
| Table | None | MAC table | MAC table | Routing table |
| Collision Domain | 1 (shared) | Per port | Per port | Per port |
| Broadcast Domain | 1 | 1 | 1 (or VLANs) | Per interface |
| Learning | No | Yes | Yes | No (configured) |
| Flooding | Always | Unknown only | Unknown only | Never |
| Full duplex | No | Yes | Yes | Yes |
| Speed | All same | All same | Per port | Per port |
| Store-and-Forward | No | Yes | Optional | Yes |
| Buffer | No | Small | Large | Large |

### Cut-through vs Store-and-Forward Switching

| Feature | Store-and-Forward | Cut-through | Fragment-free |
|---|---|---|---|
| Reads before forwarding | Entire frame | Only dest MAC (14 bytes) | First 64 bytes |
| Error checking | Full CRC check | None | Partial (runt detection) |
| Latency | High (frame-size dependent) | Very low (constant) | Medium |
| Error propagation | No (drops bad frames) | Yes | Partial protection |

### Spanning Tree Convergence Times

| Event | STP (802.1D) | RSTP (802.1w) |
|---|---|---|
| Topology Change | 30–50 seconds | <1 second |
| Root Port Failure | 30–50 seconds | <1 second |
| Forward Delay | 15 sec × 2 = 30 sec | Near-instant |

**STP Timers:**
- Hello: 2 seconds (BPDU interval)
- Max Age: 20 seconds (BPDU timeout)
- Forward Delay: 15 seconds per state (Listening → Learning → Forwarding)

**Port States:**

| State | STP | RSTP |
|---|---|---|
| Blocking | Yes | Discarding |
| Listening | Yes | (combined into Discarding) |
| Learning | Yes | Learning |
| Forwarding | Yes | Forwarding |
| Disabled | Yes | (combined into Discarding) |

---

## Deep Dive: TCP Advanced Topics

### TCP Options Field

| Option | Size | Purpose |
|---|---|---|
| MSS | 4 bytes | Maximum Segment Size (SYN only) |
| Window Scale | 3 bytes | Window scaling factor (SYN only) |
| SACK Permitted | 2 bytes | Enable Selective ACK |
| SACK | Variable | Report received block ranges |
| Timestamp | 10 bytes | RTT measurement + PAWS |
| NOP | 1 byte | Padding/alignment |

### TCP SACK (Selective Acknowledgment)

Without SACK (traditional TCP):
```
Received: 1-1000, 2001-3000, 4001-5000
ACK: 1001 (cumulative only — receiver can't tell sender what else it has)
```

With SACK:
```
ACK: 1001
SACK: [2001-3000], [4001-5000]  ← Sender knows exactly which blocks arrived
```

Benefits: Sender retransmits ONLY the missing segments (1001-2000, 3001-4000).

### TCP Retransmission Ambiguity (Karn's Algorithm)

**Problem:** If a segment is retransmitted and an ACK arrives, is the ACK for the original or the retransmission?

**Karn's Algorithm:**
1. Do NOT update RTT estimate for retransmitted segments
2. On retransmission: double the RTO (exponential backoff)
3. Only update RTT from non-ambiguous ACKs

### TCP Persist Timer — Detailed

```
Scenario: Receiver sends rwnd=0 (buffer full)
      │
Sender: Cannot send data. Starts persist timer.
      │
Timer expires →  Send window probe (1 byte)
      │
      ├── Receiver still rwnd=0 → Double timer, try again
      │
      └── Receiver now rwnd > 0 → Resume sending!
```

Without persist timer: deadlock possible! (Receiver's window update could be lost, and sender waits forever.)

### TCP Keep-Alive — Detailed

```
Connection idle for keep-alive time (default: 2 hours)
      │
      ▼
Send keep-alive probe (empty ACK with seq = last_ACK - 1)
      │
      ├── Response received → Connection alive
      │
      └── No response after N probes → Connection dead → RST + close
```

Probes: 9 retries, 75 seconds apart (typical). Total detection time: 2 hours + 9 × 75 = ~2 hours 11 minutes.

---

## Deep Dive: Network Layer Protocols — Extended

### IPv4 Header Worked Example

Raw hex dump of IP header:
```
45 00 00 3C 1C 46 40 00 40 06 B1 E6 AC 10 0A 63 AC 10 0A 0C
```

Decoding:
| Field | Hex | Value |
|---|---|---|
| Version | 4 | IPv4 |
| IHL | 5 | 5 × 4 = 20 bytes header |
| ToS/DSCP | 00 | Best effort |
| Total Length | 00 3C | 60 bytes |
| Identification | 1C 46 | 7238 |
| Flags+Offset | 40 00 | DF=1, MF=0, offset=0 |
| TTL | 40 | 64 hops |
| Protocol | 06 | TCP |
| Checksum | B1 E6 | Header checksum |
| Source IP | AC 10 0A 63 | 172.16.10.99 |
| Dest IP | AC 10 0A 0C | 172.16.10.12 |

### IP Header Checksum Calculation

**Steps:**
1. Set checksum field to 0
2. Split header into 16-bit words
3. Add all words (with carry wraparound)
4. Take 1's complement → checksum

**Verification:** Add all 16-bit words including checksum → should get 0xFFFF

### DHCP Relay Agent

When DHCP server is on a different subnet:
```
Client (10.0.0.0/24) → Broadcast DHCP Discover
      │
      ▼
Router (DHCP Relay) → Converts broadcast to unicast
                     → Adds giaddr (gateway IP = router's interface IP)
                     → Forwards to DHCP server on different subnet
      │
      ▼
DHCP Server → Assigns IP from pool matching giaddr's subnet
            → Responds (unicast to relay)
      │
      ▼
Router → Forwards to client (broadcast on local subnet)
```

### Proxy ARP

When hosts on different subnets are configured without a default gateway:
- Host sends ARP for destination IP
- Router configured as proxy ARP responds with its own MAC
- Host sends frame to router's MAC → router forwards at L3

Proxy ARP is NOT recommended (security and scalability issues).

---

## Deep Dive: Quality of Service (QoS)

### DiffServ (Differentiated Services)

- Uses DSCP (6 bits in IPv4 ToS field) to classify traffic
- **Per-Hop Behaviors (PHBs):**
  - Default (Best Effort): DSCP 000000
  - Expedited Forwarding (EF): Low delay, jitter, loss (DSCP 101110 = 46)
  - Assured Forwarding (AF): 4 classes × 3 drop precedences

### IntServ (Integrated Services)

- Per-flow QoS guarantees
- Uses RSVP (Resource Reservation Protocol) to reserve resources
- More complex, less scalable than DiffServ

### Scheduling Algorithms

| Algorithm | Description | Fairness |
|---|---|---|
| FIFO | First come, first served | Poor (no priority) |
| Priority Queuing | Higher priority always first | Unfair (starvation possible) |
| Round Robin | Serve each queue in turn | Fair (equal service) |
| Weighted Fair Queuing | Proportional service per weight | Fair (weighted) |

---

## Deep Dive: Wireless LAN — Extended

### 802.11 Standards

| Standard | Frequency | Max Speed | Notes |
|---|---|---|---|
| 802.11a | 5 GHz | 54 Mbps | OFDM |
| 802.11b | 2.4 GHz | 11 Mbps | DSSS, most legacy |
| 802.11g | 2.4 GHz | 54 Mbps | OFDM, backward compat with b |
| 802.11n | 2.4/5 GHz | 600 Mbps | MIMO, 4 spatial streams |
| 802.11ac | 5 GHz | 6.93 Gbps | MU-MIMO, beamforming |
| 802.11ax (Wi-Fi 6) | 2.4/5 GHz | 9.6 Gbps | OFDMA, improved efficiency |

### 802.11 Frame Format

```
┌──────┬──────┬───────┬───────┬───────┬──────┬──────┬──────┐
│Frame │Dur/  │Addr 1 │Addr 2 │Addr 3 │Seq   │Addr 4│Data  │FCS│
│Ctrl  │ID    │(6B)   │(6B)   │(6B)   │Ctrl  │(6B)  │      │   │
│(2B)  │(2B)  │       │       │       │(2B)  │      │0-2312│4B │
└──────┴──────┴───────┴───────┴───────┴──────┴──────┴──────┘
```

**Four address fields** (used based on To DS/From DS bits):
| To DS | From DS | Addr1 | Addr2 | Addr3 | Addr4 | Scenario |
|---|---|---|---|---|---|---|
| 0 | 0 | Dest | Source | BSSID | - | Ad-hoc |
| 1 | 0 | BSSID | Source | Dest | - | To AP |
| 0 | 1 | Dest | BSSID | Source | - | From AP |
| 1 | 1 | RA | TA | Dest | Source | WDS (bridge) |

### Power Management in 802.11

- Stations can enter **sleep mode** (save power)
- AP buffers frames for sleeping stations
- TIM (Traffic Indication Map) in beacon tells which stations have buffered frames
- Station wakes up periodically to check beacon for TIM

---

## Practice Set 15: Advanced Mixed Problems (P261–P280)

**P261.** An Ethernet switch has forwarding rate of 10 Gbps (wire speed) with 48 ports at 1 Gbps. Can it handle full wire speed? 
$48 \times 1 = 48$ Gbps bidirectional = 96 Gbps switching capacity needed. 10 Gbps <<< 96 Gbps.
**No** — this switch will have blocking under full load.

**P262.** TCP: Initial seq=100. Sends 3 segments of 500 bytes each. What ACK does receiver send for each?
Seg 1 (seq=100, 500B): ACK = 600
Seg 2 (seq=600, 500B): ACK = 1100
Seg 3 (seq=1100, 500B): ACK = 1600

**P263.** In GBN with window=4, frames 0,1,2,3 sent. Frame 1 lost. Receiver sends ACK0 then discards F2, F3. After timeout:
Sender retransmits: F1, F2, F3 (go back to F1).
Total frames sent: 4 (original) + 3 (retransmit) = **7 frames** for 4 data units.

**P264.** In SR with same scenario: Only F1 retransmitted.
Total frames: 4 + 1 = **5 frames**.

**P265.** Network 10.0.0.0/8. How many possible /24 subnets?
$2^{24-8} = 2^{16} = 65536$ subnets.

**P266.** TCP connection between hosts on same LAN (RTT ≈ 0.1 ms) vs across Internet (RTT ≈ 100 ms). How does cwnd grow differently?
Same LAN: cwnd doubles every 0.1 ms → reaches window_max very fast
Internet: cwnd doubles every 100 ms → takes much longer to ramp up
**Slow start convergence time ∝ RTT!**

**P267.** A DNS recursive resolver sends 4 queries to resolve a name. Each query RTT = 50 ms, and the DNS-to-HTTP RTT = 100 ms. HTTP/1.1 persistent pipelining for 1 HTML + 4 images.
DNS: 4 × 50 = 200 ms
TCP handshake: 100 ms
HTML: 100 ms
4 images pipelined: 100 ms
Total: 200 + 100 + 100 + 100 = **500 ms**

**P268.** Hamming(15,11): 11 data bits, 4 parity bits. Code rate?
$R = 11/15 = 0.733 = 73.3\%$ (27% redundancy)

**P269.** IP packet with source = 10.0.0.1, is this valid on the Internet?
**No** — 10.0.0.0/8 is private. Would be dropped by Internet routers. NAT needed.

**P270.** A leaky bucket with rate 5 Mbps and capacity 10 Mb. Input traffic is 20 Mbps for 1 second. What happens?
Input: 20 Mb in 1 sec. Bucket leaks 5 Mb in 1 sec. Net input = 15 Mb. Bucket = 10 Mb.
**5 Mb are dropped** (bucket overflow: 15 - 10 = 5 Mb lost)

**P271.** OSPF uses multicast addresses 224.0.0.5 (all OSPF routers) and 224.0.0.6 (designated routers). Why multicast instead of broadcast?
Efficiency: only OSPF routers process the packets. Non-OSPF hosts discard at NIC level (hardware filtering).

**P272.** What happens when you type `ping 127.0.0.1`?
Loopback: packet never leaves the host. TCP/IP stack processes it internally. Tests local network stack, not the NIC or cable.

**P273.** Is ARP needed to send packet on same subnet?
**Yes** — need destination MAC for L2 frame. Even on same subnet, must resolve IP→MAC via ARP.

**P274.** Is ARP needed to send packet to different subnet?
**Yes** — but ARP is for the **default gateway's MAC**, not the destination host's MAC.

**P275.** A router receives a packet with TTL=1 destined for a host on a directly connected network. What happens?
Router decrements TTL to 0. Even though destination is directly connected, router MUST drop the packet and send ICMP Time Exceeded.

Wait — actually this depends: if the destination is on a directly connected interface, the router can deliver it. The TTL check happens BEFORE forwarding. TTL=1 arrives, decrement to 0 → drop. But some implementations deliver to directly connected hosts. **In GATE: assume TTL=0 → drop**.

**P276.** Sliding window: Why is efficiency = 1 when $N ≥ 1 + 2a$?
Because the sender can continuously transmit. By the time the first ACK returns (after $1 + 2a$ frames in the pipeline), the sender hasn't run out of window → no idle time.

**P277.** Can two hosts on different VLANs communicate without a router?
**No** — VLANs are separate broadcast domains. Inter-VLAN communication requires L3 routing.

**P278.** What's maximum TCP throughput over a GbE link with rwnd=64 KB and RTT=10 ms?
$\text{Throughput} = \text{rwnd}/\text{RTT} = 65535/(0.01) = 6.5$ MB/s = **52 Mbps**
Only 5.2% of 1 Gbps! Need window scaling for full utilization.

**P279.** With window scaling (scale=7), max window?
$65535 \times 2^7 = 8,388,480$ bytes ≈ 8 MB
Throughput = 8 MB / 10 ms = **800 MB/s = 6.4 Gbps** (more than link → full utilization) ✓

**P280.** In a network with 100 hosts connected to a hub, and 100 hosts connected to a switch:
Hub section: 1 collision domain, 100 hosts share bandwidth
Switch section: 100 collision domains, each host gets full bandwidth
Both: 1 broadcast domain each (if no VLANs and connected by a bridge)

---

## Practice Set 16: Quick Numerical Drills (P281–P300)

**P281.** /28 network: how many hosts? $2^4 - 2 = 14$
**P282.** /22 network: how many hosts? $2^{10} - 2 = 1022$
**P283.** /19 network: how many hosts? $2^{13} - 2 = 8190$
**P284.** Block size for /25? $2^7 = 128$
**P285.** Block size for /21? $2^{11} = 2048$
**P286.** Subnet mask for /27? $255.255.255.224$
**P287.** Subnet mask for /18? $255.255.192.0$
**P288.** Subnet mask for /13? $255.248.0.0$
**P289.** 100.100.100.100/12 — network address? Block in 2nd octet: $2^{16-12}=16$. $100/16=6$. Network at $6 \times 16 = 96$. → **100.96.0.0**
**P290.** How many /30 subnets in a /24? $2^{30-24}/(2^{30-30}) = 2^6/1 = 64$
**P291.** S&W: $a=5$, efficiency? $1/11 = 9.09\%$
**P292.** S&W: $a=100$, efficiency? $1/201 = 0.497\%$
**P293.** GBN: $N=15, a=10$, efficiency? $15/21 = 71.4\%$
**P294.** SR: $N=8, a=20$, efficiency? $8/41 = 19.5\%$
**P295.** CSMA/CD: $a=0.01$, efficiency? $1/(1+0.0644) = 93.9\%$
**P296.** Pure ALOHA: $G=1$, throughput? $e^{-2} = 13.5\%$
**P297.** Slotted ALOHA: $G=0.5$, throughput? $0.5 \times e^{-0.5} = 30.3\%$
**P298.** Shannon: B=10 kHz, SNR=10 dB. $\text{SNR}=10$. $C=10000 \times \log_2(11) = 34,594$ bps
**P299.** Nyquist: B=5 kHz, L=4. $C = 2 \times 5000 \times 2 = 20,000$ bps
**P300.** Token bucket: $r=10$ Mbps, $b=50$ Mb, $R=100$ Mbps. Burst duration? $50/(100-10) = 0.556$ sec

---

## 🔍 CN Exam Patterns & Important Years

### GATE 2024 CN Questions (from PYQs):

Common patterns observed:
1. Subnetting + VLSM allocation problem (2 marks)
2. Sliding window efficiency with errors (2 marks)
3. TCP congestion control cwnd tracing (2 marks)
4. Fragmentation + reassembly (1-2 marks)
5. Protocol identification MCQ (1 mark)
6. Delay calculation (1-2 marks)
7. Physical layer (Nyquist/Shannon) (1 mark)

### High-frequency topic combinations:
- IP addressing + subnetting (appears almost every year)
- Sliding window + delay calculation (combo question)
- TCP cwnd + throughput calculation
- Fragmentation + offset arithmetic
- HTTP timing + DNS resolution steps

### Less frequent but still tested:
- CRC polynomial division (every 2-3 years)
- RSA small-number problem (every 2-3 years)
- CDMA encoding/decoding (rare but appears)
- STP root bridge election (rare)
- Routing table + forwarding decision (every 1-2 years)

---

## 📊 CN Numerical Answer Summary Card

**Quick answers for common GATE CN questions:**

| Question Type | Formula | Watch Out For |
|---|---|---|
| Hosts in /n | $2^{32-n} - 2$ | Subtract 2 for network + broadcast |
| S&W efficiency | $1/(1+2a)$ | $a = T_{prop}/T_{trans}$, not RTT/T |
| GBN max window | $2^n - 1$ | NOT $2^n$ |
| SR max window | $2^{n-1}$ | Per side; total = $2^n$ |
| ALOHA throughput | $Ge^{-2G}$ (pure), $Ge^{-G}$ (slotted) | Pure has 2G in exponent |
| Min frame CSMA/CD | $2 \times T_{prop} \times B$ | Factor is 2 (round trip) |
| Shannon capacity | $B \log_2(1 + \text{SNR})$ | Convert dB to linear first! |
| Fragments | $\lceil \text{Data} / \text{MaxPerFrag} \rceil$ | MaxPerFrag = ⌊(MTU-20)/8⌋ × 8 |
| Original size from fragments | last_offset × 8 + last_data + header | Offset is in 8B units |
| Token burst duration | $b/(R-r)$ | R = link rate, r = token rate |
| TCP cwnd at RTT $k$ (SS) | $2^k$ MSS | Starts at cwnd=1 |
| Data in $k$ RTTs (SS) | $2^k - 1$ MSS | Geometric sum |
| HTTP non-persistent | 2 RTT per object | Plus transmission time if given |
| BDP | $B \times \text{RTT}$ | Units: bits (if B in bps) |

---

## Deep Dive: Network Troubleshooting Tools & Concepts

### Ping
- Uses ICMP Echo Request (type 8) / Echo Reply (type 0)
- Measures RTT, packet loss
- TTL in reply shows how many hops (original TTL - remaining)
- Common TTL defaults: Windows = 128, Linux = 64, Cisco = 255

**Estimating hop count from TTL:**
If received TTL = 54 and sender is Linux (default 64): hops = 64 - 54 = **10 hops**

### Traceroute
- Sends packets with incrementally increasing TTL
- TTL=1 → first router sends ICMP Time Exceeded
- TTL=2 → second router sends ICMP Time Exceeded
- ...continues until destination reached

**Implementation varies:**
- Linux: sends UDP packets to high port (33434+). Destination replies ICMP Port Unreachable
- Windows (`tracert`): sends ICMP Echo Requests. Destination replies ICMP Echo Reply
- Both: intermediate routers always send ICMP Time Exceeded

### Netstat / ss
- Shows active network connections, listening ports, routing table
- `netstat -an` → all connections, numeric addresses
- `ss -tunlp` → TCP/UDP listeners with process info

### ARP Table
- `arp -a` → show ARP cache entries
- Entries have timeout (default ~20 minutes)
- Static vs Dynamic entries
- Gratuitous ARP: host broadcasts its own IP→MAC mapping (used after IP change, VRRP failover)

---

## Deep Dive: NAT Traversal Techniques

### Problem: Incoming Connections through NAT

NAT maintains outgoing connection state. Incoming connections from external hosts can't reach internal hosts without:

### Technique 1: Port Forwarding
- Manually configure NAT: External_IP:port → Internal_IP:port
- Static mapping, always available
- Used for servers behind NAT

### Technique 2: UPnP (Universal Plug and Play)
- Application dynamically requests NAT to open port
- No manual configuration needed
- Security concern: any app can open ports

### Technique 3: STUN (Session Traversal Utilities for NAT)
- Client discovers its external IP:port by contacting STUN server
- Shares this info with peer → peer connects directly
- Works for cone NAT, fails for symmetric NAT

### Technique 4: TURN (Traversal Using Relays around NAT)
- Falls back to relay server when direct connection impossible
- All traffic goes through TURN server → higher latency
- Used when STUN fails

### Technique 5: ICE (Interactive Connectivity Establishment)
- Framework combining STUN + TURN
- Tries direct → STUN → TURN (in order of preference)
- Used in WebRTC, VoIP

### NAT Types

| Type | Maps To | Allows Incoming From |
|---|---|---|
| Full Cone | Fixed external port | Any source IP:port |
| Restricted Cone | Fixed external port | Only IPs host has sent to |
| Port Restricted | Fixed external port | Only IP:port pairs host has sent to |
| Symmetric | Different external port per dest | Only the specific dest |

> 🎯 **GATE Trick:** NAT creates a mapping when outgoing packet is sent. The type of NAT determines how permissive it is for incoming packets using that mapping.

---

## Deep Dive: Congestion in Networks

### Causes of Congestion:
1. **Buffer overflow:** Router queue full → packets dropped
2. **Slow links:** Bottleneck link slower than input rate
3. **Bursty traffic:** Many sources send simultaneously
4. **Processing delay:** Router CPU overwhelmed

### Congestion Control vs Flow Control:

| Feature | Congestion Control | Flow Control |
|---|---|---|
| Purpose | Prevent network overload | Prevent receiver overload |
| Scope | Network-wide | End-to-end |
| Window | cwnd (sender-maintained) | rwnd (receiver-advertised) |
| Detection | Packet loss, delay increase | Window = 0 |
| Mechanism | AIMD, slow start | Sliding window |
| Who controls | Sender (based on network signals) | Receiver (based on buffer) |

### ECN (Explicit Congestion Notification)

Without ECN: routers signal congestion by DROP ping packets
With ECN: routers MARK packets instead of dropping them

**How:**
1. IP header: 2-bit ECN field (00=Not-ECN, 01/10=ECN capable, 11=Congestion Experienced)
2. TCP header: ECE and CWR flags
3. Router experiencing congestion: sets ECN field to 11 (CE)
4. Receiver sees CE bit → sets ECE flag in TCP ACK
5. Sender sees ECE → reduces cwnd (like 3 dup ACKs), sets CWR flag

**Benefits:** Avoids packet loss → faster recovery, less retransmission.

---

## Deep Dive: Error Control — Extended CRC Properties

### CRC Detection Capabilities — Formal

Given generator polynomial $G(x)$ of degree $r$:

| Error Pattern | Detected? | Condition |
|---|---|---|
| Single bit | Always | If $G(x)$ has more than 1 term |
| Two isolated bits | Always | If $G(x)$ does not divide $x^i + 1$ for any $i ≤ n-1$ |
| Odd number of bits | Always | If $(x+1)$ is a factor of $G(x)$ |
| Burst of length ≤ $r$ | Always | By definition of CRC |
| Burst of length $r+1$ | Probability $1 - 2^{-(r-1)}$ | |
| Burst of length > $r+1$ | Probability $1 - 2^{-r}$ | |

### Standard CRC Polynomials — Extended

| Standard | Polynomial | Binary | Degree | Used In |
|---|---|---|---|---|
| CRC-1 | $x + 1$ | 11 | 1 | Parity bit |
| CRC-8 | $x^8 + x^2 + x + 1$ | 100000111 | 8 | ATM, I²C |
| CRC-16 | $x^{16} + x^{15} + x^2 + 1$ | 11000000000000101 | 16 | USB, Modbus |
| CRC-CCITT | $x^{16} + x^{12} + x^5 + 1$ | 10001000000100001 | 16 | HDLC, X.25 |
| CRC-32 | (complex) | | 32 | Ethernet, ZIP, PNG |

### Why XOR Division (Modulo-2)?

Regular division involves subtraction and carrying. Modulo-2:
- No carries (each bit independent)
- XOR replaces subtraction
- Hardware implementation: simple shift register with XOR gates
- Very fast (can be done at wire speed)

---

## Deep Dive: Delay Analysis — Complex Scenarios

### Scenario 1: Message Segmentation

A 10 MB message sent over 3 links (each 1 Mbps, prop delay = 10 ms each).

**Without segmentation (1 big packet):**
$T = 3 \times \frac{10 \times 8 \times 10^6}{10^6} + 3 \times 10 = 240 + 30 = 270$ seconds

**With segmentation into 1000-bit packets (80,000 packets):**
$T = (3 + 80000 - 1) \times \frac{1000}{10^6} + 3 \times 10 = 80002 \times 1 + 30 = 80,032$ ms ≈ 80 seconds

Wait, that's worse! Because overhead of many small packets. Let me recalculate:

Without segmentation: $T = 3 \times 80 + 0.03 = 240.03$ seconds
With segmentation (1000-bit = 0.001 sec per packet): 
$T = (3 + 80000 - 1) \times 0.001 + 0.03 = 80.002 + 0.03 = 80.032$ seconds

**Three times faster with pipelining!** The pipeline effect of segmentation reduces total delay from 240 sec to ~80 sec.

**Optimal segment size:** Trade-off between pipeline gain and per-packet overhead.

### Scenario 2: Mixed Bandwidth Links

3 links: 10 Mbps, 1 Mbps, 10 Mbps. Packet = 10000 bits. Total prop = 5 ms.

Bottleneck: Link 2 (1 Mbps).
$T = \frac{10000}{10^7} + \frac{10000}{10^6} + \frac{10000}{10^7} + 5 = 1 + 10 + 1 + 5 = 17$ ms

For N packets: bottleneck determines throughput ≈ 1 Mbps (not 10 Mbps).
**Bottleneck link determines end-to-end throughput!**

### Scenario 3: Queuing at Bottleneck

Link 1 → Router → Link 2. Link 1 = 10 Mbps, Link 2 = 1 Mbps.

Packets arrive at router at 10 Mbps but leave at 1 Mbps → queue builds up.
Queue growth rate = 10 - 1 = 9 Mbps.
If router buffer = 9 Mb, buffer fills in 1 second → then packets dropped!

**Solution:** TCP congestion control detects loss → reduces sending rate to ≈ 1 Mbps.

---

## Deep Dive: Multiplexing Worked Problems

### FDM Problem

Voice channel bandwidth = 4 kHz. Guard band = 500 Hz. Available bandwidth = 100 kHz.
How many voice channels?

Per channel (including guard): 4 + 0.5 = 4.5 kHz
Channels = $\lfloor 100/4.5 \rfloor = 22$ channels

(Last guard band not needed → might fit 23: $22 \times 4.5 + 4 = 103$ kHz → No, 23rd doesn't fit)
**Answer: 22 channels**

### TDM Problem

24 voice channels, each sampled at 8000 samples/sec, 8 bits/sample. 1 sync bit per frame.
Frame rate = 8000 frames/sec.
Bits per frame = 24 × 8 + 1 = 193 bits.
Total bit rate = 193 × 8000 = **1,544,000 bps = 1.544 Mbps** (T1 line!)

Frame duration = 1/8000 = 125 μs.
Time per slot = 125/24 ≈ 5.2 μs.

### Statistical TDM vs Synchronous TDM

| Feature | Synchronous TDM | Statistical TDM |
|---|---|---|
| Slot assignment | Fixed | On demand |
| Empty slots | Wasted | No waste |
| Header needed | No (position = identity) | Yes (address in each slot) |
| Utilization | Low (if sources bursty) | High |
| Delay | Predictable | Variable |
| Output rate | ≥ sum of all input rates | Can be < sum |

---

## Deep Dive: IPv4 Addressing — Edge Cases for GATE

### /0: The Entire Internet
- 0.0.0.0/0 = default route → matches everything
- Used as "catch-all" in routing tables

### /31: Point-to-Point Links (RFC 3021)
- $2^1 = 2$ addresses, no network/broadcast
- Both addresses usable → perfect for router-to-router links
- Example: 10.0.0.0/31 and 10.0.0.1/31

### /32: Host Route
- Single IP address
- Used in routing tables to specify route to exactly one host
- No network/broadcast concept

### Loopback: 127.0.0.0/8
- Entire /8 block (16 million addresses) reserved for loopback
- Only 127.0.0.1 is commonly used
- Traffic to any 127.x.x.x never leaves the host

### Private vs Public IP Ranges

| Range | CIDR | # Addresses | Class |
|---|---|---|---|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 | 16,777,216 | A |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 | 1,048,576 | B |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | 65,536 | C |

> 🎯 **GATE Trick:** The /12 range is 172.16.0.0 to 172.31.255.255. It's NOT 172.16.0.0/16! The 12-bit prefix covers 172.16-31 in the second octet.

---

## Deep Dive: Advanced Security Topics

### Kerberos (Authentication Protocol)

```
1. Client → AS (Authentication Server): "I am Alice"
2. AS → Client: TGT (Ticket Granting Ticket) encrypted with Alice's key
3. Client → TGS (Ticket Granting Server): TGT + "I want to access File Server"
4. TGS → Client: Service Ticket (encrypted with File Server's key)
5. Client → File Server: Service Ticket
6. File Server: Validates ticket → grants access
```

- Uses **symmetric key cryptography** (DES/AES)
- Centralized authentication → single sign-on
- Time-based: tickets have expiry (prevents replay attacks)
- Vulnerable to password guessing on AS (offline attack)

### Types of Attacks

| Attack | Layer | Description |
|---|---|---|
| ARP Spoofing | L2 | Fake ARP replies to redirect traffic |
| MAC Flooding | L2 | Overflow switch MAC table → switch becomes hub |
| IP Spoofing | L3 | Forge source IP address |
| Smurf Attack | L3 | ICMP broadcast with spoofed source → DDoS |
| SYN Flood | L4 | Send many SYNs, never complete handshake |
| DNS Cache Poisoning | L7 | Insert fake DNS records in resolver cache |
| MITM | Any | Intercept and modify communication |
| Replay | Any | Capture and re-send valid messages |

### Firewall Rules — Example Table

| Rule# | Action | Protocol | Source | Dest | Src Port | Dst Port |
|---|---|---|---|---|---|---|
| 1 | Allow | TCP | Any | 10.0.0.1 | Any | 80 |
| 2 | Allow | TCP | Any | 10.0.0.1 | Any | 443 |
| 3 | Allow | UDP | Any | 10.0.0.2 | Any | 53 |
| 4 | Allow | TCP | 10.0.0.0/24 | Any | Any | Any |
| 5 | Deny | Any | Any | Any | Any | Any |

Rules evaluated top-to-bottom, first match applies.
- Rule 1-2: Allow web traffic to server
- Rule 3: Allow DNS queries to DNS server
- Rule 4: Allow all outbound from internal network
- Rule 5: Default deny (implicit)

### VPN Types

| Type | Layer | Protocol | Use Case |
|---|---|---|---|
| IPSec VPN | L3 | ESP/AH | Site-to-site, remote access |
| SSL/TLS VPN | L4-L7 | TLS | Remote access (browser-based) |
| L2TP/IPSec | L2 | L2TP + IPSec | Remote access |
| WireGuard | L3 | Custom (UDP) | Modern, fast VPN |
| PPTP | L2 | GRE + PPP | Legacy (insecure) |

---

## 🎓 Complete Revision: Key Differences Quick Table

| Question | Answer |
|---|---|
| Hub vs Switch | Hub = L1 shared, Switch = L2 per-port |
| TCP vs UDP | TCP = reliable/ordered, UDP = fast/unreliable |
| GBN vs SR | GBN discards out-of-order, SR buffers them |
| Tahoe vs Reno | Same on timeout; 3 dup ACKs: Tahoe→1, Reno→ssthresh+3 |
| RIP vs OSPF | DV/hops/slow vs LS/cost/fast |
| ARP vs RARP | IP→MAC vs MAC→IP |
| IPv4 vs IPv6 | 32-bit vs 128-bit, options vs ext headers |
| PAP vs CHAP | Plaintext password vs challenge-response |
| Circuit vs Packet | Dedicated path vs shared, per-packet |
| Symmetric vs Asymmetric | Same key vs public/private pair |
| Nyquist vs Shannon | Noiseless limit vs noisy limit |
| Propagation vs Transmission | Distance/speed vs size/bandwidth |
| Flooding vs Broadcasting | Unknown unicast vs dest=FF:FF:FF:FF:FF:FF |
| MSS vs MTU | Max TCP data vs max link frame payload |
| cwnd vs rwnd | Congestion window vs receiver window |
| DIFS vs SIFS | Normal wait vs short wait (802.11) |

---

## 🔬 Deep Dive: Sliding Window Protocol — Edge Cases & Advanced Analysis

### 🎯 Case 1: Sequence Number Wrap-Around in GBN

- **Setup:** 3-bit sequence numbers → range 0–7, window size W = 7 (GBN max = 2ⁿ - 1)
- Sender transmits frames 0,1,2,3,4,5,6
- All ACKs lost → timeout → retransmit 0,1,2,3,4,5,6
- Receiver expects frame 7, gets frame 0 → recognises new frame ✅
- **Key Insight:** GBN works because receiver only accepts the ONE expected sequence number

### 🎯 Case 2: Why SR needs W ≤ 2ⁿ⁻¹

- **Counter-example with W = 2ⁿ - 1 = 7 (3-bit, SR):**
  - Sender sends 0,1,2,3,4,5,6 → all received, ACKs sent
  - All ACKs lost → sender retransmits 0
  - Receiver's window has shifted to {7,0,1,2,3,4,5}
  - Receiver accepts retransmitted frame 0 as NEW frame 0 ❌ **DUPLICATE ACCEPTED!**
  
- **With W = 4 (= 2² = 2ⁿ⁻¹):**
  - Sender sends 0,1,2,3 → all received, ACKs sent
  - All ACKs lost → sender retransmits 0
  - Receiver's window shifted to {4,5,6,7}
  - Frame 0 NOT in window → correctly rejected ✅

### 🎯 Case 3: Piggybacking Analysis

- **Bidirectional communication:** Data frames carry ACKs for reverse direction
- ACK field in frame header = "I've received everything up to this seq #"
- **Timer issue:** If no reverse data to send, ACK waits too long → sender times out
- **Solution:** ACK timer (shorter than retransmission timer) forces ACK-only frame if no data

### 🎯 Case 4: Efficiency with Errors

**GBN Efficiency with error probability p:**
$$\eta_{GBN} = \frac{W(1-p)}{(1+2a)(1-p+Wp)}$$

**SR Efficiency with error probability p:**
$$\eta_{SR} = \frac{W(1-p)}{(1+2a)}$$

> 💡 SR is always ≥ GBN efficiency because SR only retransmits errored frames

**Worked Example:** 
- W = 7, a = 2, p = 0.1
- GBN: η = 7(0.9)/[3(0.9 + 0.7)] = 6.3/4.8 = **1.3125** → cap at **1.0** (but formula gives >1 means window is sufficient; actual η considering utilization = W(1-p)/(1+2a) when W > 1+2a)
- Actually for W ≥ 1+2a: η_GBN = (1-p)/(1+(2a)p) when W ≥ 1+2a
- η_GBN = 0.9/(1 + 4×0.1) = 0.9/1.4 = **0.643**
- η_SR = 1-p = **0.9** (when W ≥ 1+2a)

### 📊 GBN vs SR Efficiency Table (a = 4, W = 15)

| Error Prob p | GBN η | SR η | SR/GBN ratio |
|---|---|---|---|
| 0 | 1.0 | 1.0 | 1.0 |
| 0.01 | 0.917 | 0.99 | 1.08 |
| 0.05 | 0.689 | 0.95 | 1.38 |
| 0.10 | 0.526 | 0.90 | 1.71 |
| 0.20 | 0.357 | 0.80 | 2.24 |
| 0.50 | 0.125 | 0.50 | 4.00 |

> 🎯 **GATE Insight:** As error rate increases, SR advantage over GBN grows dramatically

---

## 🔬 Deep Dive: TCP Flow Control — Detailed Mechanics

### 📝 Receiver Window (rwnd) Dynamics

```
Sender                              Receiver
  |                                    |
  |--- Seq=1000, 500 bytes ---------->|  Buffer: [500/4000 used]
  |<-- ACK=1500, rwnd=3500 -----------|
  |--- Seq=1500, 1000 bytes --------->|  Buffer: [1500/4000 used]
  |<-- ACK=2500, rwnd=2500 -----------|  (App reads 500 bytes)
  |<-- ACK=2500, rwnd=3000 -----------|  Window update!
  |--- Seq=2500, 3000 bytes --------->|  Buffer: [3000/4000 used]
  |<-- ACK=5500, rwnd=1000 -----------|
```

### 🚨 Zero Window & Persist Timer

1. Receiver advertises rwnd = 0 → sender STOPS sending
2. Sender starts **persist timer** (exponential backoff: 1s, 2s, 4s, ..., max 60s)
3. On persist timer expiry: sender sends **zero window probe** (1 byte)
4. Receiver responds with current rwnd:
   - rwnd > 0 → sender resumes
   - rwnd = 0 → sender resets persist timer

> ⚠️ Without persist timer: deadlock possible if rwnd update ACK is lost!

### 🧮 Effective Window Calculation

```
EffectiveWindow = min(cwnd, rwnd) - (LastByteSent - LastByteAcked)
```

**Worked Example:**
- cwnd = 10 KB, rwnd = 8 KB → usable window = 8 KB
- Outstanding (unACKed) data = 3 KB
- Effective window = 8 - 3 = **5 KB** (can still send 5 KB)

### 📊 Silly Window Syndrome (SWS) Prevention

| Problem | Side | Solution |
|---|---|---|
| Receiver advertises tiny window | Receiver | **Clark's solution:** Don't advertise window < MSS or buffer/2 |
| Sender sends tiny segments | Sender | **Nagle's algorithm:** Collect data until ACK received or MSS accumulated |

**Nagle's Algorithm Rules:**
1. If data ≥ MSS → send immediately
2. If no unACKed data → send whatever available
3. Otherwise → buffer until ACK arrives or MSS accumulated

> 🎯 Nagle + Delayed ACK = bad combo (each waits for the other!) → disable Nagle for interactive apps (SSH, gaming)

---

## 🔬 Deep Dive: ARP — Complete Protocol Analysis

### 📝 ARP Operation Step-by-Step

**Scenario:** Host A (192.168.1.10) wants to send to Host B (192.168.1.20) on same LAN

```
Step 1: A checks ARP cache for 192.168.1.20 → miss
Step 2: A broadcasts ARP Request
        Src MAC: AA:AA:AA:AA:AA:AA  Dst MAC: FF:FF:FF:FF:FF:FF
        "Who has 192.168.1.20? Tell 192.168.1.10"
Step 3: All hosts on LAN receive broadcast
Step 4: Only B responds with ARP Reply (unicast!)
        Src MAC: BB:BB:BB:BB:BB:BB  Dst MAC: AA:AA:AA:AA:AA:AA
        "192.168.1.20 is at BB:BB:BB:BB:BB:BB"
Step 5: A caches mapping (typical timeout: 20 minutes)
Step 6: A sends IP packet encapsulated in frame to BB:BB:BB:BB:BB:BB
```

### 📝 ARP Across Subnets (Through Router)

```
A (192.168.1.10) → Router → B (192.168.2.20)

1. A sees B is on different subnet → sends to default gateway
2. A ARPs for router's MAC (192.168.1.1)
3. Frame: Src MAC=A, Dst MAC=Router, Src IP=A, Dst IP=B
4. Router receives, decrements TTL, looks up route
5. Router ARPs for B's MAC on 192.168.2.0/24
6. Frame: Src MAC=Router_port2, Dst MAC=B, Src IP=A, Dst IP=B
```

| Field | Hop 1 (A→Router) | Hop 2 (Router→B) |
|---|---|---|
| Src MAC | A's MAC | Router port 2 MAC |
| Dst MAC | Router port 1 MAC | B's MAC |
| Src IP | A's IP (unchanged) | A's IP (unchanged) |
| Dst IP | B's IP (unchanged) | B's IP (unchanged) |

> 🎯 **GATE Trick:** MAC changes at every hop, IP stays same (unless NAT)

### 📝 Proxy ARP

- Router answers ARP requests on behalf of hosts on another subnet
- Host thinks destination is on same LAN
- Used when hosts don't have proper subnet masks configured

### 📝 Gratuitous ARP

- Host sends ARP request for its OWN IP
- **Purposes:** (1) Detect duplicate IP, (2) Update ARP caches after MAC change, (3) Announce presence

---

## 🔬 Deep Dive: DHCP — Complete Process

### 📝 DORA Process

```
Client                                Server
(0.0.0.0)                          (192.168.1.1)
   |                                    |
   |--[1] DHCP DISCOVER (broadcast)--->|  UDP 68→67
   |   Src: 0.0.0.0:68                 |
   |   Dst: 255.255.255.255:67         |
   |                                    |
   |<--[2] DHCP OFFER (broadcast/uni)--|  "Here's 192.168.1.100"
   |   Offered IP, lease time, options  |
   |                                    |
   |--[3] DHCP REQUEST (broadcast)---->|  "I want 192.168.1.100"
   |   (broadcast so other servers      |
   |    know their offers declined)     |
   |                                    |
   |<--[4] DHCP ACK (broadcast/uni)----|  "Confirmed! Lease = 1 hour"
   |                                    |
   Client configures interface          |
```

**Why REQUEST is broadcast?** To inform ALL DHCP servers — declined servers retract their offers

### 📝 Lease Renewal

| Timer | Value | Action |
|---|---|---|
| T1 (Renewal) | 50% of lease | Unicast REQUEST to leasing server |
| T2 (Rebinding) | 87.5% of lease | Broadcast REQUEST to any server |
| Lease expiry | 100% | Release IP, restart DORA |

### 📝 DHCP Relay Agent

- Problem: DHCP Discover is broadcast → won't cross router
- Solution: Relay agent on router forwards DHCP messages to server on different subnet
- Relay adds **giaddr** (gateway IP address) so server knows which subnet pool to allocate from

> 🎯 **GATE Fact:** DHCP uses UDP (not TCP) — client has no IP yet, can't do 3-way handshake!

---

## 🔬 Deep Dive: ICMP — Extended Reference

### 📊 ICMP Message Types Complete Table

| Type | Code | Name | Description |
|---|---|---|---|
| 0 | 0 | Echo Reply | Response to ping |
| 3 | 0 | Dest Unreachable | Network unreachable |
| 3 | 1 | Dest Unreachable | Host unreachable |
| 3 | 2 | Dest Unreachable | Protocol unreachable |
| 3 | 3 | Dest Unreachable | Port unreachable |
| 3 | 4 | Dest Unreachable | Fragmentation needed, DF set |
| 4 | 0 | Source Quench | Congestion (deprecated) |
| 5 | 0 | Redirect | Better route for network |
| 5 | 1 | Redirect | Better route for host |
| 8 | 0 | Echo Request | Ping request |
| 9 | 0 | Router Advertisement | Router announces itself |
| 10 | 0 | Router Solicitation | Host asks for router |
| 11 | 0 | Time Exceeded | TTL expired (traceroute!) |
| 11 | 1 | Time Exceeded | Fragment reassembly timeout |
| 12 | 0 | Parameter Problem | Bad IP header |

### 📝 Traceroute Mechanism

```
1. Send UDP to destination with TTL=1
   → First router: TTL→0, sends back ICMP Type 11 (Time Exceeded)
   → Record router IP and RTT

2. Send with TTL=2
   → Second router: sends ICMP Time Exceeded
   
3. Continue incrementing TTL...

N. Destination receives packet:
   → Port unreachable (high port number used)
   → Sends ICMP Type 3, Code 3
   → Traceroute knows it reached destination!
```

> 🎯 **GATE Trick:** Traceroute uses UDP + ICMP Time Exceeded. Windows `tracert` uses ICMP Echo instead.

### 📝 Path MTU Discovery

1. Send packet with DF (Don't Fragment) bit set
2. If packet > link MTU → router sends ICMP Type 3, Code 4 (Fragmentation Needed)
3. ICMP message includes next-hop MTU
4. Sender reduces packet size and retries
5. Repeat until packet reaches destination

> Avoids fragmentation entirely → better performance!

---

## 📝 Practice Set 17: Protocol Behaviour Analysis (P301 – P315)

**P301.** In GBN with 4-bit sequence numbers, what is maximum sender window size?
- **Answer:** 2⁴ - 1 = **15**

**P302.** SR protocol, 3-bit seq numbers. Both sender and receiver window = 5. Can this cause issues?
- **Answer:** Yes! Max W for SR = 2ⁿ⁻¹ = 4. Window of 5 can cause ambiguity.

**P303.** TCP connection: MSS = 1460 bytes, rwnd = 65535 bytes. Max segments in flight?
- **Answer:** ⌊65535/1460⌋ = **44 segments**

**P304.** Host A pings Host B through 3 routers. How many ARP requests total (assuming empty caches)?
- **Answer:** 4 ARP requests (A→R1, R1→R2, R2→R3, R3→B)

**P305.** DHCP server assigns IP 10.0.0.50 with lease of 2 hours. When does renewal attempt start?
- **Answer:** T1 = 50% of lease = **1 hour** after assignment

**P306.** Traceroute to destination 5 hops away. Minimum ICMP messages received?
- **Answer:** 5 messages: 4 Time Exceeded + 1 Port Unreachable = **5**

**P307.** TCP Tahoe: cwnd = 32 KB, ssthresh = 16 KB. Three duplicate ACKs received. New cwnd and ssthresh?
- **Answer:** ssthresh = 32/2 = 16 KB, cwnd = 1 MSS (Tahoe always goes to 1)

**P308.** TCP Reno: same scenario. New cwnd and ssthresh?
- **Answer:** ssthresh = 32/2 = 16 KB, cwnd = 16 + 3 = **19 KB** (fast recovery)

**P309.** Channel: B = 4 kHz, SNR = 30 dB. Using 8-QAM. Maximum data rate?
- **Answer:** Nyquist: 2 × 4000 × log₂8 = 24 kbps. Shannon: 4000 × log₂(1001) ≈ 39.87 kbps. **Min = 24 kbps**

**P310.** Frame of 1000 bits sent over 3 links (1 Mbps, 0.5 Mbps, 2 Mbps) using store-and-forward. Propagation delay = 0 for simplicity. Total delay?
- **Answer:** Bottleneck is 0.5 Mbps. Total = 1000/10⁶ + 1000/0.5×10⁶ + 1000/2×10⁶ = 1 + 2 + 0.5 = **3.5 ms**

**P311.** CSMA/CD network. 10 Mbps, max distance 2.5 km, propagation speed 2×10⁸ m/s. Min frame size?
- **Answer:** RTT = 2 × 2500/(2×10⁸) = 25 μs. Min bits = 10×10⁶ × 25×10⁻⁶ = **250 bits** (32 bytes)

**P312.** Pure ALOHA with 1000 stations, each generating frame every 50 seconds. Frame time = 1 ms. Throughput?
- **Answer:** G = 1000 × 0.001/50... Wait: G = N × frame_time / interval = 1000 × 0.001/1 per second... 
  Actually: Each station generates 1 frame/50s = 0.02 frames/s. Total = 1000 × 0.02 = 20 frames/s. G = 20 × 0.001 = 0.02. S = G × e^(-2G) = 0.02 × e^(-0.04) ≈ 0.02 × 0.961 ≈ **0.0192** (≈ 19.2 frames/s throughput)

**P313.** Token bucket: rate = 10 Mbps, bucket size = 1 MB, max output rate = 25 Mbps. How long can host transmit at max rate?
- **Answer:** Drain: (25-10) Mbps consumes bucket. Time = (1×8 Mbit)/(15 Mbps) = 8/15 = **0.533 seconds**

**P314.** IPv4 packet: total length = 4020 bytes, header = 20 bytes, MTU of next link = 1500 bytes. How many fragments?
- **Answer:** Data = 4000 bytes. Max data per fragment = 1480 bytes (1500 - 20), must be multiple of 8 → 1480.
  Fragments: ⌈4000/1480⌉ = 3. Sizes: 1480 + 1480 + 1040 = 4000 ✅. **3 fragments**

**P315.** RSA: p=11, q=3. e=7. Encrypt M=5.
- **Answer:** n = 33, φ(n) = 20. d = e⁻¹ mod 20. 7d ≡ 1 mod 20 → d = 3. C = 5⁷ mod 33 = 78125 mod 33 = 78125 - 2367×33 = 78125 - 78111 = **14**

---

## 📝 Practice Set 18: Tricky GATE Conceptual MCQs (P316 – P340)

**P316.** Which layer adds MAC address? **(a)** Physical **(b)** Data Link **(c)** Network **(d)** Transport
- **Answer:** **(b)** Data Link layer adds source and destination MAC addresses

**P317.** In Ethernet, if collision is detected, what is transmitted?
- **Answer:** **Jam signal** (48 bits) to ensure all stations detect collision

**P318.** Which of these is NOT a valid reason for IP fragmentation?
**(a)** MTU mismatch **(b)** Congestion **(c)** Different network technologies **(d)** Source route through small-MTU link
- **Answer:** **(b)** Congestion does not cause fragmentation

**P319.** What happens when TCP receives a segment with checksum error?
- **Answer:** **Silently discards** — does NOT send NAK (TCP has no NAK mechanism)

**P320.** ARP request is _____, ARP reply is _____.
- **Answer:** ARP request = **broadcast**, ARP reply = **unicast**

**P321.** In a classful network, what is the default subnet mask for 150.10.0.0?
- **Answer:** Class B → **/16 or 255.255.0.0**

**P322.** Which timer prevents deadlock when receiver advertises zero window?
- **Answer:** **Persist timer**

**P323.** Maximum data rate of a noiseless channel with bandwidth 5 kHz using 4 signal levels?
- **Answer:** Nyquist: 2 × 5000 × log₂4 = **20,000 bps = 20 kbps**

**P324.** In OSPF, what algorithm is used for shortest path computation?
- **Answer:** **Dijkstra's algorithm** (SPF - Shortest Path First)

**P325.** Can two hosts on different VLANs communicate without a router?
- **Answer:** **No** — inter-VLAN routing requires a Layer 3 device

**P326.** What is the purpose of the TCP TIME_WAIT state?
- **Answer:** (1) Ensure final ACK reaches peer (2) Allow old duplicate segments to expire (duration = 2×MSL)

**P327.** In CDMA, if station A's chip code is (+1 -1 +1 -1) and received signal is (+1 -1 +3 -1), what did A send?
- **Answer:** Inner product = (1×1 + (-1)(-1) + 1×3 + (-1)(-1))/4 = (1+1+3+1)/4 = 6/4 = 1.5 → rounds to **+1 (bit 1)**

**P328.** HTTP 1.0 with 10 objects on a page. How many TCP connections for non-persistent?
- **Answer:** 1 (base HTML) + 10 objects = **11 TCP connections**

**P329.** What is the broadcast address for 192.168.10.64/26?
- **Answer:** Network: 192.168.10.64, hosts: 2⁶ - 2 = 62. Broadcast = 192.168.10.64 + 63 = **192.168.10.127**

**P330.** Which protocol resolves domain names to IP addresses?
- **Answer:** **DNS** (Domain Name System), uses UDP port 53 (TCP for zone transfers)

**P331.** Difference between persistent and non-persistent HTTP?
- **Answer:** Persistent reuses TCP connection for multiple objects; non-persistent opens new connection per object

**P332.** What is the maximum window size in TCP without window scaling?
- **Answer:** Window field = 16 bits → **65,535 bytes** (64 KB - 1)

**P333.** In distance vector routing, what is the "count to infinity" problem caused by?
- **Answer:** **Routing loops** — routers keep incrementing distance to unreachable network via each other

**P334.** CRC with generator G(x) = x³ + 1 (binary: 1001). How many check bits appended?
- **Answer:** Degree of G(x) = 3 → **3 check bits** appended

**P335.** In token ring, what happens if the token is lost?
- **Answer:** **Monitor station** detects missing token (timeout) and regenerates it

**P336.** Why does UDP have a checksum if it's "unreliable"?
- **Answer:** To detect **bit errors** in transit — unreliable means no retransmission, not no error detection

**P337.** What is the subnet mask for a network that needs exactly 30 usable host addresses?
- **Answer:** 2ⁿ - 2 ≥ 30 → n = 5 (32 - 2 = 30). Mask = /27 = **255.255.255.224**

**P338.** In CSMA/CA (WiFi), why can't collision detection be used?
- **Answer:** Wireless stations can't transmit and listen simultaneously (signal strength difference too large)

**P339.** What is the minimum number of IP addresses wasted in a /30 subnet?
- **Answer:** /30 = 4 addresses, 2 usable. **2 wasted** (network + broadcast). Used for point-to-point links.

**P340.** Sequence of layers a packet passes through when going from Application to Physical?
- **Answer:** Application → Transport (segment) → Network (packet) → Data Link (frame) → Physical (bits)

---

## 📝 Practice Set 19: Numerical Speed Round (P341 – P370)

**P341.** Subnet 10.0.0.0/20. Number of host addresses? → 2¹² - 2 = **4094**

**P342.** 16-QAM, bandwidth = 10 kHz. Data rate? → 2 × 10000 × 4 = **80 kbps**

**P343.** Propagation delay: 200 km, speed = 2×10⁸ m/s → 200000/(2×10⁸) = **1 ms**

**P344.** Go-Back-N, W=7, RTT=14ms, Tₜ=1ms. Efficiency? → 7/(1+14) = 7/15 = **46.7%**

**P345.** IP packet TTL=64 passes through 10 routers. Final TTL? → 64 - 10 = **54**

**P346.** Slotted ALOHA, G=1. Throughput? → S = 1 × e⁻¹ = **0.368**

**P347.** Token bucket: capacity=5MB, fill rate=2Mbps. Max burst duration at 10Mbps? → 5×8/(10-2) = 40/8 = **5 seconds**

**P348.** CRC generator 1011 (degree 3). Data=11001. Transmitted codeword length? → 5 + 3 = **8 bits**

**P349.** Class C network split into 8 subnets. Hosts per subnet? → 256/8 = 32 addresses, 32 - 2 = **30 hosts**

**P350.** TCP MSS=500 bytes, cwnd after 5 rounds of slow start? → 1,2,4,8,16,32 → **32 MSS = 16000 bytes**

**P351.** 10BaseT Ethernet. Data rate? → **10 Mbps**

**P352.** Hamming distance of code {000, 011, 101, 110}? → min pairwise distance = **2**

**P353.** SNR = 1000 (linear), B = 1 MHz. Shannon capacity? → 10⁶ × log₂(1001) ≈ 10⁶ × 9.97 ≈ **9.97 Mbps**

**P354.** Frame: 1000 bits, BW=1Mbps, Prop=5ms. Value of 'a'? → Tₜ = 1ms, a = 5/1 = **5**

**P355.** SR protocol, 4-bit sequence. Max window size? → 2³ = **8**

**P356.** n hosts in pure ALOHA, G=0.5. Throughput? → 0.5 × e⁻¹ = **0.184**

**P357.** How many class B networks possible? → 2¹⁴ = **16384** (128.0-191.255)

**P358.** Manchester encoding of 10110: symbols? → Each bit = 2 symbols. Total = 5 × 2 = **10 signal elements**

**P359.** TCP congestion: ssthresh=8, loss at cwnd=12. New ssthresh? → 12/2 = **6**

**P360.** IPv4 header minimum size? → **20 bytes** (5 × 32-bit words)

**P361.** Ethernet minimum frame size? → **64 bytes** (512 bits)

**P362.** ARP hardware type field for Ethernet? → **1**

**P363.** DNS iterative query: max DNS messages if 4 DNS servers consulted? → 2 per server = **8 messages**

**P364.** /28 subnet. Number of usable hosts? → 2⁴ - 2 = **14**

**P365.** CSMA/CD slot time with 2km cable, speed 2×10⁸ m/s → RTT = 2×2000/(2×10⁸) = **20 μs**

**P366.** TCP window=10KB, RTT=100ms. Max throughput? → 10KB/0.1s = **100 KB/s = 800 kbps**

**P367.** 802.11 DIFS = SIFS + 2×Slot. If SIFS=10μs, Slot=20μs, DIFS = ? → 10 + 40 = **50 μs**

**P368.** Leaky bucket: outflow=5Mbps, incoming burst=20Mbps for 2s. Bucket capacity needed? → Excess rate = 15 Mbps × 2s = 30 Mb = **30 Mbit = 3.75 MB**

**P369.** BGP uses which transport protocol? → **TCP** (port 179)

**P370.** Number of fragments for 5000-byte packet (20-byte header) with MTU=1500? → Data=4980, per frag=1480. ⌈4980/1480⌉ = **4 fragments**

---

## 📊 Final Master Formula Card — CN GATE Quick Reference

### 🔴 Must-Know Formulas (appear every year)

| # | Formula | When to Use |
|---|---|---|
| 1 | Hosts = 2ⁿ - 2 | Subnetting |
| 2 | Subnets = 2ˢ | VLSM/subnetting |
| 3 | Tₜ = L/BW | Transmission delay |
| 4 | Tₚ = d/v | Propagation delay |
| 5 | a = Tₚ/Tₜ | Protocol efficiency |
| 6 | η_SW = 1/(1+2a) | Stop-and-Wait |
| 7 | η = W/(1+2a) if W<1+2a | Sliding window |
| 8 | S = G·e⁻²ᴳ (max 0.184) | Pure ALOHA |
| 9 | S = G·e⁻ᴳ (max 0.368) | Slotted ALOHA |
| 10 | Nyquist = 2B·log₂L | Noiseless channel |
| 11 | Shannon = B·log₂(1+SNR) | Noisy channel |
| 12 | SNR_dB = 10·log₁₀(SNR) | dB conversion |
| 13 | Frags = ⌈(TotalLen-IHL)/(MTU-IHL)⌉ | IP fragmentation (offset must be ×8) |
| 14 | ssthresh_new = cwnd/2 | TCP loss event |
| 15 | Efficiency_CSMA/CD = 1/(1+6.44a) | Ethernet efficiency |
| 16 | Min frame = 2×Tₚ×BW | CSMA/CD constraint |
| 17 | Token bucket burst = C + ρ×t at rate M | Traffic shaping |
| 18 | E2E_SF = (k-1)×Tₜ + k×Tₜ + Tₚ... | Store-and-forward k links |
| 19 | Throughput = W×MSS/RTT | TCP throughput |
| 20 | BDP = BW × RTT | Bandwidth-delay product |

### 🟡 Unit Conversion Quick Reference

| From | To | Multiply by |
|---|---|---|
| Kbps | bps | × 10³ |
| Mbps | bps | × 10⁶ |
| Gbps | bps | × 10⁹ |
| KB | bytes | × 1024 |
| MB | bytes | × 1024² |
| ms | seconds | × 10⁻³ |
| μs | seconds | × 10⁻⁶ |
| km | meters | × 10³ |

> 🎯 **#1 GATE Trap:** Mixing kilo = 1000 (for bits/bandwidth) vs 1024 (for bytes/memory). In networking, usually use 10³!

---

## 🏁 Ultimate CN Exam Day Checklist

- Layer functions: Physical→bits, DLL→frames, Network→packets, Transport→segments, App→data
- IP addressing: Class ranges (A:1-126, B:128-191, C:192-223, D:224-239, E:240-255)
- Subnet formula: 2ⁿ-2 hosts, VLSM allocate largest first
- Fragmentation: offset in units of 8 bytes, MF=1 for all but last
- TCP: 3-way handshake (SYN, SYN-ACK, ACK), 4-way close (FIN, ACK, FIN, ACK)
- Congestion: Tahoe (always cwnd=1 on loss), Reno (cwnd=ssthresh on 3 dup ACKs)
- Sliding window: GBN max W=2ⁿ-1, SR max W=2ⁿ⁻¹
- ALOHA: Pure max=18.4%, Slotted max=36.8%
- CSMA/CD: min frame = 2×prop×BW, efficiency = 1/(1+6.44a)
- CRC: append (degree) zeros, XOR divide, remainder = check bits
- Nyquist and Shannon: take MINIMUM of both for actual capacity
- DNS=53, HTTP=80, HTTPS=443, FTP=20/21, SMTP=25, SSH=22, TELNET=23
- ARP=broadcast request+unicast reply, MAC changes per hop, IP doesn't
- DHCP=DORA process, uses UDP (67/68)
- RSA: n=pq, φ=(p-1)(q-1), ed≡1 mod φ, C=Mᵉ mod n

**Total Score Tip:** CN typically carries 10-13 marks in GATE. Master subnetting + delays + TCP congestion = 70% of CN marks! 🎯

---

## 🔬 Deep Dive: Store-and-Forward vs Cut-Through Switching

### 📝 Switching Techniques Comparison

| Feature | Store-and-Forward | Cut-Through | Fragment-Free |
|---|---|---|---|
| Reads entire frame? | Yes ✅ | No (reads dest MAC only) | Reads first 64 bytes |
| Error checking? | Full CRC check | None | Partial (collision fragments) |
| Latency | High (proportional to frame size) | Very low (fixed) | Medium |
| Forwards bad frames? | No ✅ | Yes ❌ | Filters runts |
| Used in | Most modern switches | Low-latency needs | Compromise approach |

### 📝 Latency Calculation

**Store-and-Forward:** Latency = Frame_Size / Link_Speed (must receive full frame)
- 1518 bytes on 100 Mbps: (1518×8)/10⁸ = **121.44 μs**

**Cut-Through:** Latency = Header_Read / Link_Speed (just destination MAC = 6 bytes + preamble)
- Typically reads 14 bytes: (14×8)/10⁸ = **1.12 μs** 

> 🎯 **GATE Insight:** For exam problems, unless specified otherwise, assume store-and-forward — add full transmission delay at each switch/router hop.

---

## 🔬 Deep Dive: Spanning Tree Protocol (STP) — IEEE 802.1D

### 📝 Why STP Is Needed

Redundant links in switched networks → **broadcast storms** + **MAC table instability** + **duplicate frames**

### 📝 STP Process (4 Steps)

1. **Elect Root Bridge:** Lowest Bridge ID (Priority:MAC) wins
2. **Select Root Ports:** Each non-root bridge picks port with lowest cost to root
3. **Select Designated Ports:** Each segment picks port with lowest cost to root  
4. **Block remaining ports:** Non-root, non-designated → Blocking state

### 📝 STP Port States

| State | Duration | Learns MACs? | Forwards Data? |
|---|---|---|---|
| Blocking | — | No | No |
| Listening | 15 sec | No | No |
| Learning | 15 sec | Yes | No |
| Forwarding | — | Yes | Yes |
| Disabled | — | No | No |

**Convergence time:** Blocking → Forwarding = 15 + 15 = **30 seconds** (listening + learning)

### 🧮 STP Worked Example

```
Bridge A (Priority 32768, MAC: 00:00:00:00:00:01)
Bridge B (Priority 32768, MAC: 00:00:00:00:00:02)  
Bridge C (Priority 32768, MAC: 00:00:00:00:00:03)

Links: A—B (cost 19), B—C (cost 19), A—C (cost 19)
```

**Step 1:** Root = Bridge A (same priority → lowest MAC wins)

**Step 2:** Root Ports:
- Bridge B: port toward A → Root Port (cost 19)
- Bridge C: port toward A → Root Port (cost 19)

**Step 3:** Designated Ports:
- Segment A—B: A's port (root is always designated) → Designated
- Segment A—C: A's port → Designated
- Segment B—C: B's port cost to root = 19, C's port cost to root = 19 → tie-break: B has lower Bridge ID → B's port = Designated

**Step 4:** C's port toward B = **Blocked** ❌

> 🎯 **GATE Fact:** RSTP (Rapid STP, 802.1w) reduces convergence to < 1 second vs STP's 30-50 seconds

---

## 🔬 Deep Dive: Network Address Translation — Port Address Translation (PAT)

### 📝 NAT Types Detailed

| Type | Mapping | Example |
|---|---|---|
| Static NAT | 1 private → 1 public (permanent) | Server hosting |
| Dynamic NAT | Pool of public IPs, 1:1 temp mapping | Enterprise outbound |
| PAT/NAPT | Many private → 1 public (port-based) | Home routers |

### 📝 PAT Translation Table Example

| Internal IP:Port | External IP:Port | Destination |
|---|---|---|
| 192.168.1.10:5000 | 203.0.113.5:40001 | 8.8.8.8:53 |
| 192.168.1.10:5001 | 203.0.113.5:40002 | 1.1.1.1:80 |
| 192.168.1.20:5000 | 203.0.113.5:40003 | 8.8.8.8:53 |
| 192.168.1.30:3000 | 203.0.113.5:40004 | 93.184.216.34:443 |

> Note: Same internal port (5000) from different hosts gets different external ports!

### 📝 NAT Hairpin / Loopback Problem

- Host A (internal) tries to reach internal server via public IP
- Without NAT hairpin: packet goes to router, gets confused
- **Solution:** NAT hairpin rewrites both source and destination

### 📝 Issues with NAT

1. **Breaks end-to-end:** External hosts can't initiate connections inward
2. **IPsec problems:** NAT modifies headers → authentication fails
3. **FTP active mode:** Server tries to connect to client's private IP
4. **Peer-to-peer:** Both sides behind NAT → need STUN/TURN
5. **IP fragmentation:** Only first fragment has port numbers → subsequent fragments can't be translated

> 🎯 **GATE Tip:** NAT violates the original Internet design principle of end-to-end connectivity

---

## 📝 Practice Set 20: Final GATE Simulation (P371 – P390)

**P371.** Network 172.16.0.0/16 divided into /22 subnets. How many subnets? → 2⁽²²⁻¹⁶⁾ = 2⁶ = **64 subnets**

**P372.** TCP connection: initial cwnd=1MSS, ssthresh=32MSS. After 10 RTTs (no loss), cwnd = ? → Slow start: 1,2,4,8,16,32 (5 RTTs to reach ssthresh). Then congestion avoidance: 33,34,35,36,37 (5 more RTTs). cwnd = **37 MSS**

**P373.** 5 packets of 1000 bits each over 2 links (1 Mbps each), prop=0 ms. Total time? → First packet: 1+1=2ms. Pipeline: 4 more packets × 1ms = 4ms. Total = **6 ms**

**P374.** Hamming(7,4) code. Data=1010. Parity bits p1,p2,p4? → p1=d1⊕d2⊕d4=1⊕0⊕0=1, p2=d1⊕d3⊕d4=1⊕1⊕0=0, p4=d2⊕d3⊕d4=0⊕1⊕0=1. Code = **1011010**

**P375.** DNS uses TCP for what purpose? → **Zone transfers** (and responses > 512 bytes)

**P376.** In OSPF, what is the cost of a 100 Mbps link if reference BW = 10⁸? → 10⁸/10⁸ = **1**

**P377.** Max payload of Ethernet frame? → **1500 bytes**

**P378.** IPv6 address size in bits? → **128 bits**

**P379.** What does TTL prevent? → **Infinite packet looping** in routing loops

**P380.** Well-known port range? → **0 – 1023**

**P381.** SMTP uses port? → **25** (submission: 587)

**P382.** In token ring, token holding time = 10ms, data rate = 4Mbps. Max frame size? → 10×10⁻³ × 4×10⁶ = **40,000 bits = 5000 bytes**

**P383.** Propagation speed in optical fiber ≈ ? → **2 × 10⁸ m/s** (≈ 2/3 speed of light)

**P384.** CIDR address 200.23.16.0/20. Range of addresses? → 200.23.16.0 — 200.23.31.255 (**4096 addresses**)

**P385.** TCP uses _____ for error detection → **Checksum** (mandatory, unlike UDP where optional in IPv4)

**P386.** Which ICMP message does traceroute rely on? → **Type 11: Time Exceeded** (TTL expired)

**P387.** Difference between flow control and congestion control? → Flow control: receiver capacity. Congestion control: network capacity.

**P388.** In a 10-node fully connected mesh, how many links? → n(n-1)/2 = 10×9/2 = **45 links**

**P389.** Maximum size of IPv4 packet? → **65,535 bytes** (total length field = 16 bits)

**P390.** CSMA/CD efficiency with a = 0.01? → 1/(1 + 6.44 × 0.01) = 1/1.0644 ≈ **93.95%**

---

*End of Computer Networks Detailed Notes — GATE 2027 CS/IT*
