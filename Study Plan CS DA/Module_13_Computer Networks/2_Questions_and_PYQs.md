# Computer Networks — Questions & Previous Year Questions (PYQs) for GATE 2027

> **Total: 75 Questions** — Covering all modules with GATE-style MCQs and NATs.
> Detailed solutions in `3_Solutions.md`.

---

## Section A: IP Addressing, Subnetting & Supernetting (15 Questions)

### Q1. [GATE 2015 — 2 marks]
An IP router with a MTU of 1500 bytes has to forward an IP datagram of size 4000 bytes (including IP header of 20 bytes) on a link. How many fragments will be created?

(a) 2  
(b) 3  
(c) 4  
(d) 5

---

### Q2. [GATE 2017 — 1 mark]
Consider the subnet `200.1.1.0/24`. If this subnet is divided into 8 equal subnets, what is the subnet mask for each subnet?

(a) 255.255.255.224  
(b) 255.255.255.240  
(c) 255.255.255.248  
(d) 255.255.255.192

---

### Q3. [GATE 2019 — 2 marks]
An organization is allotted a block of IP addresses from `14.24.74.0/24`. It needs to create 5 subnets, the largest requiring 50 hosts. What is the minimum number of address bits needed in the host part for the largest subnet?

(a) 5  
(b) 6  
(c) 7  
(d) 8

---

### Q4. [Practice — 2 marks]
A router has the following forwarding table:

| Prefix | Next Hop |
|---|---|
| `12.0.0.0/8` | A |
| `12.1.0.0/16` | B |
| `12.1.2.0/24` | C |
| `0.0.0.0/0` | D |

Where will a packet destined for `12.1.2.5` be forwarded?

(a) A  
(b) B  
(c) C  
(d) D

---

### Q5. [Practice — 2 marks]
A Class B network `172.16.0.0/16` is subnetted with a subnet mask of `255.255.240.0`. How many subnets and hosts per subnet are available?

(a) 16 subnets, 4094 hosts  
(b) 14 subnets, 4096 hosts  
(c) 16 subnets, 4096 hosts  
(d) 16 subnets, 4094 hosts

---

### Q6. [GATE 2014 — 2 marks]
Consider the following IP addresses:  
`140.10.0.0/16`, `140.11.0.0/16`, `140.12.0.0/16`, `140.13.0.0/16`

What is the aggregated (supernet) address?

(a) `140.10.0.0/14`  
(b) `140.8.0.0/13`  
(c) `140.10.0.0/13`  
(d) `140.10.0.0/15`

---

### Q7. [Practice — 1 mark]
An IP address is `192.168.50.137/28`. What is the network address and broadcast address?

---

### Q8. [Practice — 2 marks]
An organization needs to allocate IP addresses for 4 departments with the following requirements: Dept A = 100 hosts, Dept B = 50 hosts, Dept C = 25 hosts, Dept D = 10 hosts. Starting from `10.0.0.0/24`, assign subnet addresses using VLSM. What is the subnet address for Dept B?

---

### Q9. [GATE 2020 — 2 marks]
A host in an organization has an IP address `30.10.5.7`. The subnet mask is `255.255.255.224`. What is the maximum number of hosts that can be present in the subnet?

(a) 14  
(b) 30  
(c) 62  
(d) 126

---

### Q10. [Practice — 1 mark]
In classless addressing, the block `200.40.50.0/22` has how many usable host addresses?

(a) 510  
(b) 1022  
(c) 1024  
(d) 2046

---

### Q11. [GATE 2016 — 1 mark]
The minimum number of bits required to represent all subnets if a Class C network is divided into 6 subnets of equal size is:

(a) 2  
(b) 3  
(c) 4  
(d) 6

---

### Q12. [Practice — NAT — 2 marks]
A router has the forwarding table entries: `10.1.0.0/16`, `10.1.1.0/24`, `10.1.1.128/25`, and default route `0.0.0.0/0`. For the destination IP `10.1.1.200`, which entry is matched using longest prefix matching?

---

### Q13. [Practice — 2 marks]
Four networks `192.168.4.0/24, 192.168.5.0/24, 192.168.6.0/24, 192.168.7.0/24` need to be aggregated. Can they be aggregated into a single CIDR block? If yes, what is the aggregated block?

---

### Q14. [GATE 2012 — 1 mark]
A host is connected to a network with IP address `192.168.1.100/26`. The default gateway of the host is `192.168.1.65`. How many more hosts can the subnet accommodate (excluding the gateway)?

(a) 60  
(b) 61  
(c) 62  
(d) 63

---

### Q15. [Practice — 2 marks]
An ISP is granted the block `200.10.0.0/21`. The ISP needs to distribute these among 4 organizations, each needing 128 addresses. What CIDR blocks should be allocated?

---

## Section B: Data Link Layer — Errors, Delays & Protocols (20 Questions)

### Q16. [GATE 2009 — 2 marks]
A bit string `1101011111` is transmitted with a CRC generator polynomial $G(x) = x^4 + x + 1$. What is the transmitted codeword (data followed by CRC bits)?

---

### Q17. [GATE 2013 — 1 mark]
In a communication channel, the minimum Hamming distance required for a code to detect up to 5-bit errors AND correct up to 2-bit errors simultaneously is:

(a) 6  
(b) 7  
(c) 8  
(d) 10

---

### Q18. [GATE 2018 — 2 marks]
A 12-bit Hamming code is received as `100110001110`. Determine if there is a single-bit error. If so, what is the corrected codeword? (Assume even parity.)

---

### Q19. [Practice — 2 marks]
A sender transmits data across a link with bandwidth = 1 Mbps, propagation delay = 5 ms, and frame size = 1000 bits. Calculate the efficiency of Stop-and-Wait protocol.

---

### Q20. [GATE 2005 — 2 marks]
Two computers are connected by a 1 Gbps link with propagation delay of 10 ms. The maximum frame size is 10,000 bits. What is the minimum number of bits needed for sequence numbers if the link is to be fully utilized using Go-Back-N protocol?

---

### Q21. [Practice — 2 marks]
For a link with bandwidth $B = 10$ Mbps, propagation delay $T_p = 20$ ms, and frame size $L = 5000$ bits, calculate the efficiencies of:
(a) Stop-and-Wait  
(b) Go-Back-N with window size 15  
(c) Selective Repeat with window size 15

---

### Q22. [GATE 2004 — 2 marks]
A 1 km long cable operates at 10 Mbps. The propagation delay is $10 \;\mu s$. If the cable is used in a store-and-forward network with packet size 12,000 bits, how long does it take to send a packet from one end to the other?

(a) $1.21$ ms  
(b) $1.201$ ms  
(c) $1.01$ ms  
(d) $1.210$ ms

---

### Q23. [Practice — 2 marks]
In Selective Repeat protocol, if the sequence number field is 4 bits, what is the maximum sender window size?

(a) 7  
(b) 8  
(c) 15  
(d) 16

---

### Q24. [GATE 2007 — 2 marks]
A channel has a bit rate of 4 kbps and propagation delay of 20 ms. For what range of frame sizes does Stop-and-Wait give an efficiency of at least 50%?

---

### Q25. [Practice — 1 mark]
In Go-Back-N protocol with 3-bit sequence numbers, what is the maximum sender window size?

(a) 3  
(b) 4  
(c) 7  
(d) 8

---

### Q26. [GATE 2011 — 2 marks]
A CRC generator polynomial is $G(x) = x^3 + 1$ (binary: 1001). If the data is `1010000`, what is the CRC remainder?

---

### Q27. [Practice — 2 marks]
A frame of 1000 bits is sent on a 1 Mbps link. The round-trip propagation delay is 50 ms. If Go-Back-N is used with a window size of 20, what is the utilization of the link?

---

### Q28. [Practice — 1 mark]
In the Internet checksum method, if two 16-bit words are `1010 0101 1110 0011` and `0111 0001 0100 1101`, compute the checksum.

---

### Q29. [GATE 2014 — 1 mark]
The minimum number of parity bits required to construct a Hamming code for a 16-bit data word is:

(a) 3  
(b) 4  
(c) 5  
(d) 6

---

### Q30. [Practice — 2 marks]
A network path has 3 links with bandwidths 10 Mbps, 5 Mbps, and 2 Mbps respectively. Each link has a propagation delay of 2 ms. A packet of 4000 bits is sent using store-and-forward switching. What is the total end-to-end delay?

---

### Q31. [Practice — 1 mark]
How many redundant bits does the Hamming code use for 11 data bits?

(a) 3  
(b) 4  
(c) 5  
(d) 6

---

### Q32. [Practice — 2 marks]
The bit error rate on a link is $10^{-4}$. If Stop-and-Wait is used with frame size 1000 bits and bandwidth 1 Mbps and propagation delay 5 ms, what is the effective throughput of the link?

---

### Q33. [GATE 2022 — 2 marks]
A link has a transmission rate of 100 Mbps and a propagation delay of 25 ms. The frame size is 500 bytes. What is the minimum window size required for maximum utilization when using sliding window protocol?

---

### Q34. [Practice — 2 marks]
A packet of 4,500 bytes (including 20-byte IP header) needs to pass through a link with MTU = 1500 bytes. How many fragments are created? What are the offset values?

---

### Q35. [GATE 2006 — 1 mark]
In a Stop-and-Wait protocol, if the transmission time for a frame is 20 ms and the propagation delay is 50 ms, the link utilization is approximately:

(a) 8.3%  
(b) 16.7%  
(c) 50%  
(d) 100%

---

## Section C: MAC Protocols & LAN Technologies (10 Questions)

### Q36. [GATE 2008 — 2 marks]
In a Pure ALOHA system, if the frame transmission time is $T$, and traffic load is $G = 0.5$, what is the throughput?

(a) $0.184$  
(b) $0.368$  
(c) $0.5$  
(d) $0.25$

---

### Q37. [Practice — 2 marks]
In CSMA/CD, if the maximum distance between two stations is 2 km and the propagation speed is $2 \times 10^8$ m/s, what is the minimum frame size for 100 Mbps Ethernet?

---

### Q38. [Practice — 1 mark]
The maximum throughput of Slotted ALOHA is:

(a) $1/(2e) \approx 18.4\%$  
(b) $1/e \approx 36.8\%$  
(c) $50\%$  
(d) $100\%$

---

### Q39. [GATE 2010 — 2 marks]
A network has 5 switches connected in a full mesh topology. To prevent loops, Spanning Tree Protocol is applied. How many ports will be in blocking state? (Each link is a direct connection between two switches.)

---

### Q40. [Practice — 1 mark]
In a network with 3 hubs and 2 switches connecting 12 hosts total, how many collision domains and broadcast domains exist?

---

### Q41. [Practice — 2 marks]
In CSMA/CD with binary exponential backoff, after the 3rd collision, what is the maximum number of slots a station may wait?

(a) 3  
(b) 4  
(c) 7  
(d) 8

---

### Q42. [Practice — 1 mark]
ARP is used to find the __________ address from a known __________ address.

(a) IP, MAC  
(b) MAC, IP  
(c) IP, DNS  
(d) MAC, DNS

---

### Q43. [GATE 2015 — 1 mark]
The DHCP protocol uses which transport layer protocol?

(a) TCP  
(b) UDP  
(c) ICMP  
(d) ARP

---

### Q44. [Practice — 2 marks]
An Ethernet network has a maximum segment length of 500 m, signal propagation speed $2 \times 10^8$ m/s, and link rate 10 Mbps. What is the efficiency of CSMA/CD for 1500-byte frames?

---

### Q45. [Practice — 1 mark]
In the DHCP DORA process, which messages are broadcast?

(a) Only Discover  
(b) Discover and Request  
(c) All four (Discover, Offer, Request, ACK)  
(d) Only Offer and ACK

---

## Section D: Network Layer (10 Questions)

### Q46. [GATE 2016 — 2 marks]
Consider a distance vector routing protocol. Router X has the following routing table:

| Dest | Cost | Next Hop |
|---|---|---|
| A | 3 | B |
| B | 1 | B |
| C | 5 | C |

X receives a DV from B: {A: 1, C: 3}. Link cost from X to B is 1. What are the updated distances from X to A and C?

---

### Q47. [Practice — 2 marks]
In Dijkstra's algorithm applied to the following network, find the shortest path from S to all nodes:

```
S --2-- A --3-- D
|       |       |
1       4       1
|       |       |
B --2-- C --5-- E
```

---

### Q48. [GATE 2003 — 1 mark]
In the count-to-infinity problem of distance vector routing, which technique advertises a route with cost infinity to the neighbor from which the route was learned?

(a) Split Horizon  
(b) Poison Reverse  
(c) Triggered Update  
(d) Hold-down Timer

---

### Q49. [Practice — 2 marks]
An IP datagram of 5000 bytes (20-byte header) passes through a network with MTU = 1500 bytes. How many fragments are created? List the Total Length, MF flag, and Fragment Offset for each fragment.

---

### Q50. [Practice — 1 mark]
The `traceroute` utility works by sending packets with incrementally increasing values of which field?

(a) Source Port  
(b) Identification  
(c) TTL  
(d) Fragment Offset

---

### Q51. [GATE 2019 — 2 marks]
Consider 4 routers R1, R2, R3, R4 connected as:
- R1–R2: cost 1, R2–R3: cost 2, R3–R4: cost 1, R1–R4: cost 10

Using distance vector routing (Bellman-Ford), what is the shortest path cost from R1 to R4?

(a) 4  
(b) 3  
(c) 10  
(d) 5

---

### Q52. [Practice — 1 mark]
In NAT (Network Address Translation), which of the following is NOT TRUE?

(a) NAT modifies IP headers  
(b) NAT allows multiple private hosts to share one public IP  
(c) NAT maintains end-to-end principle  
(d) NAT uses port numbers for translation in PAT

---

### Q53. [Practice — 2 marks]
Describe the count-to-infinity problem with an example of 3 nodes A–B–C, link costs all = 1. What happens when A–B link fails? How does Poison Reverse mitigate this?

---

### Q54. [GATE 2021 — 1 mark]
The ICMP message "Time Exceeded" (Type 11) is sent when:

(a) The packet is too large  
(b) The destination port is not reachable  
(c) TTL field reaches zero  
(d) There is a checksum error

---

### Q55. [Practice — 2 marks]
A router has 4 interfaces. The forwarding table is:

| Prefix | Interface |
|---|---|
| `10.0.0.0/8` | eth0 |
| `10.1.0.0/16` | eth1 |
| `10.1.1.0/24` | eth2 |
| Default | eth3 |

For each of the following IPs, determine the outgoing interface:
(a) `10.1.1.5`  
(b) `10.1.2.3`  
(c) `10.2.0.1`  
(d) `172.16.0.1`

---

## Section E: Transport Layer — TCP (15 Questions)

### Q56. [GATE 2017 — 2 marks]
In TCP, the initial congestion window is 1 MSS and the slow start threshold is 32 MSS. After how many RTTs does the congestion window first reach the slow start threshold (assuming no loss)?

(a) 4  
(b) 5  
(c) 6  
(d) 32

---

### Q57. [Practice — 2 marks]
A TCP Reno connection starts with cwnd = 1 MSS and ssthresh = 16 MSS. At what round does cwnd reach 22 MSS for the first time? (Assume no losses and RTT = constant.)

---

### Q58. [GATE 2020 — 2 marks]
In TCP congestion control (Reno), if cwnd = 20 MSS and 3 duplicate ACKs are received, what are the new values of ssthresh and cwnd?

(a) ssthresh = 10, cwnd = 1  
(b) ssthresh = 10, cwnd = 13  
(c) ssthresh = 20, cwnd = 1  
(d) ssthresh = 10, cwnd = 10

---

### Q59. [Practice — 2 marks]
TCP uses the 3-way handshake for connection establishment. If client's initial sequence number is 200 and server's is 500, what are the sequence and acknowledgment numbers in each of the three segments?

---

### Q60. [Practice — 1 mark]
The maximum segment size (MSS) in TCP is typically:

(a) 65,535 bytes  
(b) 1460 bytes  
(c) 536 bytes  
(d) 1500 bytes

---

### Q61. [GATE 2018 — 2 marks]
A TCP connection uses a leaky bucket for traffic shaping with a bucket size of 100 KB and an output rate of 10 Mbps. If data arrives at 20 Mbps, how long can the burst last before packets are dropped?

---

### Q62. [Practice — 2 marks]
A Token Bucket has capacity $b = 500$ KB and token arrival rate $r = 2$ MBps. The link rate is $R = 10$ MBps. Calculate:
(a) Maximum burst size  
(b) Maximum burst duration at link rate  
(c) Maximum data that can be sent in 5 seconds

---

### Q63. [GATE 2023 — 2 marks]
In TCP Reno, starting with cwnd = 1 MSS, ssthresh = 64 MSS. The connection experiences a timeout at cwnd = 40 MSS. What are the values of ssthresh and cwnd after the timeout?

(a) ssthresh = 20, cwnd = 1  
(b) ssthresh = 40, cwnd = 1  
(c) ssthresh = 32, cwnd = 1  
(d) ssthresh = 20, cwnd = 20

---

### Q64. [Practice — 2 marks]
Given the following sequence of TCP congestion window sizes (in MSS): 1, 2, 4, 8, 16, 17, 18, 19, 20. At round 10, a timeout occurs.
(a) At what round did slow start end?  
(b) What was the ssthresh?  
(c) What are the new ssthresh and cwnd after timeout?

---

### Q65. [Practice — 1 mark]
In TCP, the purpose of the persistence timer is:

(a) Keep the connection alive during idle periods  
(b) Retransmit lost segments  
(c) Probe a zero-window receiver  
(d) Wait before closing the connection

---

### Q66. [Practice — 2 marks]
A TCP sender has rwnd = 10 KB and cwnd = 6 KB, MSS = 1 KB. How many segments can the sender transmit before waiting for an ACK?

---

### Q67. [GATE 2024 — 2 marks]
In TCP Reno, the congestion window evolves as: 1, 2, 4, 8, 16, 32, 33, 34, 35, 36. At round 11, three duplicate ACKs are received. What is the value of cwnd at round 12?

---

### Q68. [Practice — 2 marks]
A TCP connection has EstimatedRTT = 100 ms and DevRTT = 10 ms. A new SampleRTT of 120 ms is observed. Using $\alpha = 0.125$ and $\beta = 0.25$, compute the new EstimatedRTT, DevRTT, and RTO.

---

### Q69. [Practice — 1 mark]
How many RTTs does TCP's 3-way handshake take?

(a) 1 RTT  
(b) 1.5 RTT  
(c) 2 RTT  
(d) 3 RTT

---

### Q70. [Practice — 2 marks]
TCP Reno starts with cwnd = 1 MSS, ssthresh = 8 MSS. Loss (3 dup ACKs) detected when cwnd = 12. Show cwnd evolution for 10 rounds starting from cwnd = 1.

---

## Section F: Application Layer (5 Questions)

### Q71. [GATE 2016 — 2 marks]
A web page has a base HTML file of size 1 KB and 10 embedded images, each of size 1 KB. RTT = 100 ms. Ignore transmission time. How many RTTs does it take to fetch the complete page using:
(a) Non-persistent HTTP without parallel connections  
(b) Persistent HTTP with pipelining

---

### Q72. [Practice — 1 mark]
DNS primarily uses which transport protocol for standard queries?

(a) TCP  
(b) UDP  
(c) ICMP  
(d) SCTP

---

### Q73. [Practice — 2 marks]
In FTP, what are the port numbers used for the control connection and data connection respectively?

(a) 20 and 21  
(b) 21 and 20  
(c) 25 and 80  
(d) 80 and 443

---

### Q74. [Practice — 1 mark]
Which of the following is a "push" protocol?

(a) POP3  
(b) IMAP  
(c) SMTP  
(d) HTTP

---

### Q75. [Practice — 2 marks]
A client needs to resolve `www.example.com`. Starting from the local DNS server (which has no cache), describe the iterative resolution process listing all queries and responses. Assume the root, `.com` TLD, and `example.com` authoritative servers are separate.

---

*End of Computer Networks Questions & PYQs — 75 Questions Total*

---

## Section G: Physical Layer, Multiplexing & Advanced Topics (25 Questions)

### Q76. [GATE] A noiseless channel has bandwidth 4000 Hz and uses 16 signal levels. The maximum data rate is:
(a) 16 kbps (b) 32 kbps (c) 64 kbps (d) 8 kbps

### Q77. [GATE 2018] A noisy channel has bandwidth 1 MHz and SNR = 63. The maximum capacity is:
(a) 6 Mbps (b) 8 Mbps (c) 12 Mbps (d) 2 Mbps

### Q78. [Practice — 2 marks]
A channel has bandwidth 3 kHz and SNR = 30 dB. Calculate the maximum theoretical data rate using Shannon's theorem.

### Q79. [GATE] Manchester encoding guarantees clock recovery because:
(a) It uses high voltage levels (b) There is a transition at the middle of every bit (c) It uses bipolar signals (d) It has no DC component

### Q80. [GATE] In synchronous TDM, if 10 channels each need 1 kbps, the total link data rate (ignoring overhead) is:
(a) 1 kbps (b) 5 kbps (c) 10 kbps (d) 20 kbps

### Q81. [Practice — 1 mark]
WDM is essentially FDM applied to:
(a) copper cables (b) satellite links (c) optical fiber (d) wireless channels

### Q82. [GATE] In PPP, byte stuffing replaces the flag byte 0x7E in data with:
(a) 0x7D 0x5E (b) 0x7E 0x7E (c) 0x00 0x7E (d) 0x7D 0x7D

### Q83. [GATE] An IPv6 address has how many bits?
(a) 32 (b) 64 (c) 128 (d) 256

### Q84. [Practice — 1 mark]
Which of the following is NOT present in an IPv6 header?
(a) Header checksum (b) Hop Limit (c) Flow Label (d) Next Header

### Q85. [GATE] The IPv6 transition mechanism that encapsulates IPv6 packets within IPv4 is called:
(a) Dual stack (b) Tunneling (c) NAT64 (d) Header translation

### Q86. [GATE] In RSA, the public key is (e, n) where n = pq. If p = 3, q = 11, and e = 7, what is the private key d?
(a) 3 (b) 7 (c) 13 (d) 23

### Q87. [GATE 2019] In Diffie-Hellman key exchange, p = 23, g = 5, Alice chooses a = 6, Bob chooses b = 15. What is the shared secret key?
(a) 2 (b) 8 (c) 15 (d) Need to compute

### Q88. [Practice — 2 marks]
Explain why Diffie-Hellman is vulnerable to man-in-the-middle attack and how digital certificates solve this.

### Q89. [GATE] A digital signature provides:
(a) Only confidentiality (b) Only integrity (c) Authentication, integrity, and non-repudiation (d) Confidentiality and authentication

### Q90. [GATE] Which firewall type operates at the application layer?
(a) Packet filter (b) Stateful (c) Application gateway/proxy (d) NAT

### Q91. [Practice — 1 mark]
IPSec ESP provides:
(a) Only authentication (b) Only encryption (c) Authentication + encryption (d) Neither

### Q92. [GATE] OSPF is a ______ routing protocol that uses ______ algorithm.
(a) Distance vector, Bellman-Ford (b) Link state, Dijkstra (c) Path vector, best path (d) Link state, Bellman-Ford

### Q93. [GATE] BGP uses which transport protocol?
(a) UDP (b) TCP (c) ICMP (d) IP directly

### Q94. [Practice — 1 mark]
BGP prevents routing loops by:
(a) TTL field (b) Split horizon (c) AS-PATH attribute (reject routes containing own AS) (d) Hop count limit

### Q95. [GATE] RIP uses a maximum hop count of:
(a) 8 (b) 15 (c) 32 (d) 255

### Q96. [Practice — 2 marks]
In CDMA, stations A, B, C have chip sequences [+1,-1,+1,-1], [+1,+1,-1,-1], [+1,-1,-1,+1]. If the received signal is [+2, 0, 0, -2], which stations transmitted what?

### Q97. [GATE] HDLC uses which stuffing technique?
(a) Byte stuffing (b) Bit stuffing (c) Character stuffing (d) No stuffing

### Q98. [GATE] Which of the following is true about TLS?
(a) It operates at the network layer (b) It uses only symmetric encryption (c) It uses asymmetric encryption for key exchange and symmetric for data (d) It replaces TCP

### Q99. [Practice — 2 marks]
Compute the RSA encryption of message M=4 with public key (e=3, n=33). What is the ciphertext?

### Q100. [GATE] In CSMA/CA, the RTS/CTS mechanism is used to solve:
(a) congestion (b) hidden terminal problem (c) exposed terminal problem (d) both (b) and (c)

---

*End of Computer Networks Questions & PYQs — 100 Questions Total*
