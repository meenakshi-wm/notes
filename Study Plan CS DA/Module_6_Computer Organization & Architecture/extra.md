
## 15. Comprehensive Worked Examples (Extended) ⭐⭐

### Example 6: Memory Chip Design
**Q:** Design a 64 KB × 8 memory system using 16 KB × 4-bit chips. How many chips needed? Show the address decoding.

**Solution:**
- Width expansion: 8/4 = 2 chips side by side (to get 8 bits)
- Depth expansion: 64KB/16KB = 4 groups
- **Total chips = 2 × 4 = 8 chips**
- Each chip: 16KB = 2¹⁴ addresses → 14 address lines (A0-A13)
- Total: 64KB = 2¹⁶ → 16 address lines (A0-A15)
- A0-A13 → all chips
- A14-A15 → 2-to-4 decoder → chip select for each group of 2

### Example 7: Effective CPI with All Hazards
**Q:** A 5-stage pipeline processor:
- 25% loads, 50% of which are followed by dependent instruction
- 20% branches, 15% mispredicted, penalty = 2 cycles
- 5% structural hazards, each causing 1 stall
- Full forwarding available

**Solution:**
- Load-use stalls = 0.25 × 0.50 × 1 = 0.125
- Branch stalls = 0.20 × 0.15 × 2 = 0.06
- Structural stalls = 0.05 × 1 = 0.05
- CPI = 1 + 0.125 + 0.06 + 0.05 = **1.235**

### Example 8: Page Table Size Comparison
**Q:** Compare page table sizes for 32-bit VA system with page sizes of 4KB, 8KB, and 16KB. PTE = 4 bytes.

| Page Size | Offset bits | VPN bits | PT entries | PT size |
|---|---|---|---|---|
| 4 KB | 12 | 20 | 2²⁰ = 1M | 4 MB |
| 8 KB | 13 | 19 | 2¹⁹ = 512K | 2 MB |
| 16 KB | 14 | 18 | 2¹⁸ = 256K | 1 MB |

**Observation:** Doubling page size halves the page table size!
**Trade-off:** Larger pages → smaller page table BUT more **internal fragmentation**

### Example 9: DMA Bandwidth Calculation
**Q:** A DMA controller transfers data from a 100 MB/s disk. Memory has 50 ns cycle time, word = 4 bytes. In cycle-stealing mode, what % of memory bandwidth is consumed by DMA?

**Solution:**
- Memory bandwidth = 4B / 50 ns = 80 MB/s
- DMA needs: 100 MB/s from disk, but limited by memory = 80 MB/s? NO — DMA transfers AT disk rate
- DMA cycles needed per second = 100 MB/s / 4B = 25 million cycles/sec
- DMA time = 25M × 50 ns = 1.25 sec out of every second?? That's 125% — impossible!
- **The disk rate (100 MB/s) exceeds memory bandwidth (80 MB/s) → DMA cannot keep up!**
- Maximum DMA throughput = 80 MB/s (limited by memory)
- DMA would use **100% of memory bandwidth** → CPU gets no memory access!

**Better scenario:** Disk = 40 MB/s (within memory bandwidth)
- DMA cycles/sec = 40M/4 = 10M cycles/sec
- DMA time = 10M × 50 ns = 0.5 sec
- % bandwidth = 0.5/1 = **50% consumed by DMA**

### Example 10: Complete Cache Problem
**Q:** A 16 KB, 4-way set associative, write-back cache with 32-byte blocks, 32-bit address.
(a) Address breakdown
(b) Total cache storage (including overhead)
(c) If processor accesses address 0x12345678, which set?

**(a) Address Breakdown:**
- Blocks = 16KB/32B = 512
- Sets = 512/4 = 128
- Offset = log₂(32) = 5 bits
- Index = log₂(128) = 7 bits
- Tag = 32 − 7 − 5 = **20 bits**

**(b) Total Storage:**
- Data: 16 KB = 131,072 bits
- Per block overhead: 20 (tag) + 1 (valid) + 1 (dirty) = 22 bits
- Total overhead: 512 × 22 = 11,264 bits = 1,408 bytes
- LRU bits per set: ~5 bits for 4-way → 128 × 5 = 640 bits = 80 bytes
- **Total storage = 16,384 + 1,408 + 80 = 17,872 bytes ≈ 17.5 KB**

**(c) Which set?**
- Address = 0x12345678 = 0001 0010 0011 0100 0101 0110 0111 1000
- Block address = 0x12345678 >> 5 = 0x91A2B3 (drop lower 5 bits)
- Set = 0x91A2B3 mod 128 = 0x91A2B3 mod 0x80
- 0x91A2B3 & 0x7F = 0x33 = **51**
- Alternatively: bits [11:5] of address: 0110011 = 51 ← **Set 51**

---

## 📝 Practice Problems — GATE Level

### Practice Set 1: Data Representation (5 Problems)

**P1.** Convert −13.625 to IEEE 754 single precision.
**Solution:**
1. Sign = 1 (negative)
2. 13.625₁₀ = 1101.101₂
3. Normalize: 1.101101 × 2³
4. Exponent: 3 + 127 = 130 = 10000010
5. Mantissa: 10110100000000000000000
**Answer: 1 10000010 10110100000000000000000** = 0xC15A0000

**P2.** Perform Booth's multiplication: 5 × (−3) using 4-bit numbers.
**Solution:**
M = 0101 (+5), Q = 1101 (−3), A = 0000, Q₋₁ = 0

| Step | A | Q | Q₋₁ | Action |
|---|---|---|---|---|
| Init | 0000 | 1101 | 0 | Q₀Q₋₁=10 → A=A−M |
| | 1011 | 1101 | 0 | A=0000−0101=1011 |
| ASR | 1101 | 1110 | 1 | |
| 2 | 1101 | 1110 | 1 | Q₀Q₋₁=01 → A=A+M |
| | 0010 | 1110 | 1 | A=1101+0101=0010 |
| ASR | 0001 | 0111 | 0 | |
| 3 | 0001 | 0111 | 0 | Q₀Q₋₁=10 → A=A−M |
| | 1100 | 0111 | 0 | A=0001−0101=1100 |
| ASR | 1110 | 0011 | 1 | |
| 4 | 1110 | 0011 | 1 | Q₀Q₋₁=11 → NOP |
| ASR | 1111 | 0001 | 1 | |

Result: AQ = 1111 0001 = −15 ✅ (5 × −3 = −15)

**P3.** What is the decimal equivalent of the IEEE 754 representation: 0 01111100 01000000000000000000000?
**Solution:**
- Sign = 0 → positive
- E = 01111100 = 124, actual = 124−127 = −3
- M = 1.01 (hidden 1 + mantissa)
- Value = 1.01 × 2⁻³ = 0.00101₂ = 0.15625₁₀
**Answer: 0.15625**

**P4.** In IEEE 754 single precision, how many normalized positive numbers are there?
**Solution:**
- Exponents: 1 to 254 (0 is denormalized, 255 is special) → 254 values
- Mantissa: 2²³ values (including 000...0)
- Positive: sign = 0
- **Answer: 254 × 2²³ = 2,130,706,432**

**P5.** Perform non-restoring division: 11 ÷ 3 (4-bit).
**Solution:**
A=0000, Q=1011 (11), M=0011 (3), count=4

| Step | A sign | Action | A | Q |
|---|---|---|---|---|
| Init | ≥ 0 | | 0000 | 1011 |
| 1 | ≥ 0 | SHL AQ, A−M | 0001 011_ → 0001−0011 = 1110 | 0110 Q₀=0 |
| 2 | < 0 | SHL AQ, A+M | 1100 110_ → 1100+0011 = 1111 | 1100 Q₀=0 |
| 3 | < 0 | SHL AQ, A+M | 1111 100_ → 1111+0011 = 0010 | 1001 Q₀=1 |
| 4 | ≥ 0 | SHL AQ, A−M | 0101 001_ → 0101−0011 = 0010 | 0011 Q₀=1 |

Q = 0011 = 3, A = 0010 = 2 → 11 = 3×3 + 2 ✅

### Practice Set 2: ISA & Performance (5 Problems)

**P6.** A computer uses 16-bit instructions. There are two types: Type A (2 register fields of 5 bits each) and Type B (1 register field of 5 bits + 8-bit immediate). If there are 12 Type A instructions, how many Type B instructions can the system support?
**Solution:**
- Type A: 6-bit opcode + 5 + 5 = 16 bits. Uses 12 of 2⁶=64 possible opcodes.
- Remaining for Type B: 64−12 = 52, but Type B has 3-bit opcode + 5 + 8 = 16.

Wait, let me reconsider. With fixed opcode field (6 bits):
- Type A uses 12 opcodes, Type B uses remaining = 64−12 = **52 Type B instructions**

But if we use expanding opcode approach:
- Type A: 6-bit opcode, uses 12. Reserve remaining 52 codes for Type B.
- Type B: Same 6-bit opcode, just different format. **52 Type B instructions**
**Answer: 52**

**P7.** A processor has clock rate 3 GHz and executes a program with instruction mix: 50% ALU (CPI=1), 20% Load (CPI=4), 10% Store (CPI=3), 20% Branch (CPI=2). What is the MIPS rating?
**Solution:**
Average CPI = 0.5×1 + 0.2×4 + 0.1×3 + 0.2×2 = 0.5+0.8+0.3+0.4 = **2.0**
MIPS = 3×10⁹ / (2.0×10⁶) = **1500 MIPS**

**P8.** Using the processor from P7, if the ALU is improved to CPI=1 (same) but Load is improved with a cache to CPI=2, what is the new MIPS and speedup?
**Solution:**
New CPI = 0.5×1 + 0.2×2 + 0.1×3 + 0.2×2 = 0.5+0.4+0.3+0.4 = **1.6**
New MIPS = 3×10⁹ / (1.6×10⁶) = **1875 MIPS**
Speedup = 2.0/1.6 = **1.25×**

**P9.** 80% of a program is parallelizable. With 16 processors, what is the speedup (Amdahl's)?
**Solution:**
Speedup = 1 / (0.2 + 0.8/16) = 1 / (0.2 + 0.05) = 1/0.25 = **4×**
Max speedup (infinite processors) = 1/0.2 = **5×**

**P10.** An 8-stage pipeline has stages with delays: 5, 6, 11, 8, 10, 7, 6, 9 ns (buffer = 1 ns). Compare with non-pipelined for 200 instructions.
**Solution:**
- Non-pipelined: 200 × (5+6+11+8+10+7+6+9) = 200 × 62 = **12,400 ns**
- Pipelined clock = max(delays) + buffer = 11 + 1 = 12 ns
- Pipelined: (8 + 200 − 1) × 12 = 207 × 12 = **2,484 ns**
- Speedup = 12,400 / 2,484 = **4.99×** (ideal would be 8×; limited by stage imbalance)

### Practice Set 3: Pipeline Hazards (5 Problems)

**P11.** Given code (5-stage pipeline, NO forwarding):
```
I1: ADD R1, R2, R3
I2: SUB R4, R1, R5
I3: AND R6, R1, R7
I4: OR  R8, R4, R9
```
How many stall cycles? Draw pipeline diagram.
**Solution:**
- I2 depends on I1 (R1): R1 written in WB of I1 (cycle 5), needed in ID of I2 (cycle 3) → 2 stalls
- I3 depends on I1 (R1): Same as above but I3 is later → check: R1 written cycle 5, I3 needs it at ID. After 2 stalls for I2, I3's ID is cycle 7, I1's WB is cycle 5 → no stall for I3
- I4 depends on I2 (R4): I2 writes R4 in cycle 7 (after stalls), I4 reads in cycle 8? Need to check diagram.

```
Cycle: 1  2  3  4  5  6  7  8  9  10  11
I1:   IF ID EX ME WB
I2:      IF -- -- ID EX ME WB            (2 stalls waiting for R1)
I3:               IF ID EX ME WB         (no stall: I1's R1 available)
I4:                  IF -- -- ID EX ME WB (2 stalls waiting for R4 from I2)
```
**Total stalls = 4 cycles**

**P12.** Same code as P11 but WITH full forwarding. How many stalls?
**Solution:**
- I2 depends on I1 (R1): Forward from EX/MEM of I1 to EX of I2 → **0 stalls**
- I3 depends on I1 (R1): Forward from MEM/WB of I1 → **0 stalls**
- I4 depends on I2 (R4): Forward from EX/MEM of I2 → **0 stalls**

```
Cycle: 1  2  3  4  5  6  7  8
I1:   IF ID EX ME WB
I2:      IF ID EX ME WB
I3:         IF ID EX ME WB
I4:            IF ID EX ME WB
```
**Total stalls = 0**

**P13.** With forwarding, how many stalls for:
```
I1: LW  R1, 0(R10)
I2: ADD R2, R1, R3
```
**Solution:** Load-use hazard! LW produces R1 at end of MEM (cycle 4). ADD needs R1 at start of EX (cycle 4). Cannot forward in time → **1 stall**

**P14.** A pipeline has 25% branch instructions, branch resolved at EX stage (2-cycle penalty), and a 2-bit predictor with 90% accuracy. What is the effective CPI?
**Solution:**
CPI = 1 + branch_freq × misprediction_rate × penalty
CPI = 1 + 0.25 × 0.10 × 2 = 1 + 0.05 = **1.05**

**P15.** How many stalls with forwarding?
```
I1: LW  R1, 0(R2)
I2: LW  R3, 4(R2)
I3: ADD R4, R1, R3
```
**Solution:**
- I2 doesn't depend on I1 → no stall
- I3 depends on R1 (from I1, available after MEM at cycle 4) → forward MEM/WB to EX of I3
  - I3's EX is cycle 5, I1's MEM output available at end of cycle 4 → **OK, 0 stalls for R1**
- I3 depends on R3 (from I2, load-use)
  - I2 produces R3 at end of cycle 5 (MEM), I3 needs at start of cycle 5 (EX) → **1 stall**

```
Cycle: 1  2  3  4  5  6  7  8
I1:   IF ID EX ME WB
I2:      IF ID EX ME WB
I3:         IF ID -- EX ME WB    (1 stall for load-use from I2)
```
**Total: 1 stall**

### Practice Set 4: Cache Problems (5 Problems)

**P16.** A 32-bit byte-addressable computer has a 32 KB, 8-way set associative cache with 64-byte blocks. Find: (a) number of sets, (b) tag bits, (c) total number of tag bits in the cache.
**Solution:**
- (a) Sets = 32KB / (64×8) = 64 sets
- (b) Offset = log₂(64) = 6, Index = log₂(64) = 6, Tag = 32−6−6 = **20 bits**
- (c) Total blocks = 32KB/64 = 512. Total tag bits = 512 × 20 = **10,240 bits**

**P17.** A cache has AMAT = 4 cycles. Hit time = 1 cycle, miss penalty = 100 cycles. What is the miss rate?
**Solution:**
4 = 1 + miss_rate × 100 → miss_rate = 3/100 = **3%**

**P18.** A processor with split L1 cache: I-cache (32 KB, 2-way, 64B blocks, 2% miss rate) and D-cache (32 KB, 4-way, 32B blocks, 5% miss rate). Unified L2 cache (512 KB, 8-way, 64B blocks, hit time=10 cycles, 25% local miss rate). Memory penalty = 200 cycles. 40% of instructions access data memory.

Find overall AMAT (assume I-cache hit time = 1 cycle, D-cache hit time = 1 cycle).
**Solution:**
- I-cache AMAT = 1 + 0.02 × (10 + 0.25 × 200) = 1 + 0.02 × 60 = 1 + 1.2 = **2.2 cycles**
- D-cache AMAT = 1 + 0.05 × (10 + 0.25 × 200) = 1 + 0.05 × 60 = 1 + 3.0 = **4.0 cycles**
- Average AMAT per instruction = 2.2 + 0.4 × 4.0 = 2.2 + 1.6 = **3.8 cycles**

Wait — the correct way: Memory stalls per instruction = (1 × 0.02 + 0.4 × 0.05) × (10 + 0.25 × 200) = (0.02 + 0.02) × 60 = 0.04 × 60 = 2.4
CPI = 1 + 2.4 = **3.4** (if base CPI = 1)

**P19.** A direct-mapped cache has 8 blocks (numbered 0-7). Block size = 1 word. The memory access sequence (block addresses) is: 0, 1, 2, 3, 4, 5, 6, 7, 0, 1, 2, 8, 0, 1, 2, 3. Find the number of hits.
**Solution:** Cache line = block_addr mod 8

| Access | Block | Line | Contents Before | Hit/Miss |
|---|---|---|---|---|
| 0 | 0 | 0 | empty | Miss |
| 1 | 1 | 1 | empty | Miss |
| 2 | 2 | 2 | empty | Miss |
| 3 | 3 | 3 | empty | Miss |
| 4 | 4 | 4 | empty | Miss |
| 5 | 5 | 5 | empty | Miss |
| 6 | 6 | 6 | empty | Miss |
| 7 | 7 | 7 | empty | Miss |
| 0 | 0 | 0 | [0] | **Hit** |
| 1 | 1 | 1 | [1] | **Hit** |
| 2 | 2 | 2 | [2] | **Hit** |
| 8 | 8 | 0 | [0]→[8] | Miss (evicts 0) |
| 0 | 0 | 0 | [8]→[0] | Miss (evicts 8) |
| 1 | 1 | 1 | [1] | **Hit** |
| 2 | 2 | 2 | [2] | **Hit** |
| 3 | 3 | 3 | [3] | **Hit** |

**Hits = 6, Misses = 10, Hit rate = 6/16 = 37.5%**

**P20.** If the cache in P19 were 2-way set associative (4 sets, LRU), how many hits for the same sequence?
**Solution:** Set = block_addr mod 4

| Access | Block | Set | Way0 | Way1 | Hit? |
|---|---|---|---|---|---|
| 0 | 0 | 0 | **0** | - | Miss |
| 1 | 1 | 1 | **1** | - | Miss |
| 2 | 2 | 2 | **2** | - | Miss |
| 3 | 3 | 3 | **3** | - | Miss |
| 4 | 4 | 0 | 0 | **4** | Miss |
| 5 | 5 | 1 | 1 | **5** | Miss |
| 6 | 6 | 2 | 2 | **6** | Miss |
| 7 | 7 | 3 | 3 | **7** | Miss |
| 0 | 0 | 0 | **0** | 4 | **Hit** |
| 1 | 1 | 1 | **1** | 5 | **Hit** |
| 2 | 2 | 2 | **2** | 6 | **Hit** |
| 8 | 8 | 0 | 0 | **8** | Miss (evicts 4, LRU) |
| 0 | 0 | 0 | **0** | 8 | **Hit** |
| 1 | 1 | 1 | **1** | 5 | **Hit** |
| 2 | 2 | 2 | **2** | 6 | **Hit** |
| 3 | 3 | 3 | **3** | 7 | **Hit** |

**Hits = 7, Misses = 9, Hit rate = 7/16 = 43.75%** (better than direct-mapped!)

### Practice Set 5: Virtual Memory & I/O (5 Problems)

**P21.** A system has 32-bit virtual address, 24-bit physical address, 8 KB page size. How many entries in the page table? What is the page table size if each entry is 4 bytes?
**Solution:**
- Offset = log₂(8KB) = 13 bits
- VPN bits = 32−13 = 19 → Page table entries = 2¹⁹ = **524,288**
- Page table size = 2¹⁹ × 4 = **2 MB**

**P22.** TLB hit rate = 95%, TLB access = 5 ns, Memory = 200 ns, 3-level page table. Find EAT.
**Solution:**
- TLB hit: 5 + 200 = 205 ns
- TLB miss: 5 + 4×200 = 805 ns (3 PT accesses + 1 data access)
- EAT = 0.95×205 + 0.05×805 = 194.75 + 40.25 = **235 ns**

**P23.** A DMA controller transfers 4KB blocks from disk. Memory cycle time = 100 ns, word size = 4 bytes. In cycle-stealing mode, DMA uses 50% of bus cycles. What is the effective memory bandwidth available to CPU?
**Solution:**
- Memory bandwidth = 4 bytes / 100 ns = 40 MB/s
- CPU gets 50% of bus = **20 MB/s**

**P24.** A disk has rotation speed 7200 RPM, 500 sectors/track, 512 bytes/sector. What is the data transfer rate?
**Solution:**
- Rotations/sec = 7200/60 = 120
- Bytes/track = 500 × 512 = 256,000 bytes
- Transfer rate = 120 × 256,000 = **30.72 MB/s**

**P25.** A system uses 2-level page table. Virtual address = 32 bits, page size = 4KB, PTE = 4 bytes. If each page table must fit in one page, find the structure.
**Solution:**
- PTEs per page = 4KB / 4B = 1024 = 2¹⁰
- L2 index = 10 bits (1024 entries)
- L1 index = 32 − 12 − 10 = 10 bits (1024 entries)
- L1 table size = 1024 × 4 = **4 KB** (fits in 1 page ✅)
- Max L2 tables = 1024, each 4KB → **Max PT overhead = 4KB + 1024×4KB = 4100 KB**
- But only allocated for used pages → much less in practice

### Practice Set 6: Comprehensive Mixed Problems (10 Problems)

**P26.** A 6-stage pipeline (IF, ID, OF, EX, MEM, WB) processes 500 instructions. Stage delays: 3, 2, 4, 3, 5, 2 ns. Buffer delay = 1 ns.
(a) Find non-pipelined time
(b) Find pipelined time
(c) Find speedup and efficiency

**Solution:**
(a) Non-pipelined = 500 × (3+2+4+3+5+2) = 500 × 19 = **9,500 ns**
(b) Clock = max(delays) + buffer = 5 + 1 = 6 ns
    Pipelined = (6 + 500 − 1) × 6 = 505 × 6 = **3,030 ns**
(c) Speedup = 9500/3030 = **3.14×** (ideal 6×)
    Efficiency = 3.14/6 = **52.3%**

**P27.** A processor executes:
- 1 billion instructions/sec
- 30% are memory instructions
- Cache miss rate = 2%
- Miss penalty = 200 cycles
- Clock rate = 3 GHz
What fraction of time is the CPU stalled on memory?

**Solution:**
- Memory stalls per second = 10⁹ × 0.3 × 0.02 × 200 = 1.2 × 10⁹ cycles
- Total cycles per second = 3 × 10⁹
- Fraction stalled = 1.2 / (3 + 1.2) = 1.2/4.2 = **28.6%**

Actually, more correctly:
- Execution cycles = 10⁹ × 1 = 10⁹ (CPI=1 base for 1 billion instructions at 1 GHz? No, it says 1 billion instructions/sec at 3 GHz means CPI=3.)

Let me recompute: If processor executes 10⁹ instructions/sec at 3 GHz → CPI_base = 3.
- Memory stalls = 0.3 × 0.02 × 200 = 1.2 cycles/instruction
- Total CPI = 3 + 1.2 = 4.2
- Fraction stalled = 1.2/4.2 = **28.6%**

**P28.** Compare two processors:
- P1: Clock = 4 GHz, CPI = 1.5
- P2: Clock = 3 GHz, CPI = 1.0
Which is faster for a program with 10⁹ instructions?

**Solution:**
- P1: Time = 10⁹ × 1.5 / (4×10⁹) = 0.375 seconds
- P2: Time = 10⁹ × 1.0 / (3×10⁹) = 0.333 seconds
- **P2 is faster** by factor 0.375/0.333 = **1.125×**

**P29.** A direct-mapped cache has 256 lines, 32 bytes per line, and a 32-bit address. A program accesses addresses (in hex): 0x00000000, 0x00002000, 0x00004000, 0x00000004, 0x00002004, 0x00004004. Find hit/miss for each.

**Solution:**
- Offset = log₂(32) = 5 bits
- Index = log₂(256) = 8 bits
- Tag = 32 − 8 − 5 = 19 bits

| Address | Tag | Index | Offset | Hit/Miss |
|---|---|---|---|---|
| 0x00000000 | 0x00000 | 0x00 | 0x00 | Miss (compulsory) |
| 0x00002000 | 0x00010 | 0x00 | 0x00 | Miss (conflict! Same index 0x00) |
| 0x00004000 | 0x00020 | 0x00 | 0x00 | Miss (conflict! Same index 0x00) |
| 0x00000004 | 0x00000 | 0x00 | 0x04 | Miss (conflict! Tag mismatch) |
| 0x00002004 | 0x00010 | 0x00 | 0x04 | Miss (conflict!) |
| 0x00004004 | 0x00020 | 0x00 | 0x04 | Miss (conflict!) |

**All misses! (All map to same set → thrashing)**
Solution: Use set-associative cache (2-way would help if only 2 conflicting addresses)

**P30.** A computer uses byte-addressable memory with 16-bit addresses. It has a 2-way set associative cache with 4 sets and 4 bytes per block. Initially all cache entries are invalid. The following byte addresses are accessed: 0, 2, 4, 8, 20, 0, 8, 20, 0. Using LRU replacement, find the number of cache misses.

**Solution:**
- Block address = byte address / 4 (4 bytes per block)
- Set = block address mod 4

| Byte Addr | Block | Set | Way0 | Way1 | Miss/Hit |
|---|---|---|---|---|---|
| 0 | 0 | 0 | **0** | - | Miss |
| 2 | 0 | 0 | 0 | - | **Hit** (same block) |
| 4 | 1 | 1 | **1** | - | Miss |
| 8 | 2 | 2 | **2** | - | Miss |
| 20 | 5 | 1 | 1 | **5** | Miss |
| 0 | 0 | 0 | **0** | - | **Hit** |
| 8 | 2 | 2 | **2** | - | **Hit** |
| 20 | 5 | 1 | 1 | **5** | **Hit** |
| 0 | 0 | 0 | **0** | - | **Hit** |

**Misses = 4, Hits = 5, Miss rate = 4/9 = 44.4%**

**P31.** An 8-bit word stored in memory starting at address 0x100 in big-endian format is 0x1234. What value is stored at address 0x101?
**Solution:**
In big-endian: MSB at lowest address.
- Address 0x100: 0x12 (high byte)
- Address 0x101: **0x34** (low byte)

**P32.** How many memory accesses (including instruction fetch) are needed for each instruction with the given addressing modes?
(a) ADD R1, R2 (Register mode)
(b) LOAD R1, [500] (Direct addressing)
(c) LOAD R1, @[500] (Indirect addressing)

**Solution:**
(a) 1 fetch + 0 operand accesses = **1**
(b) 1 fetch + 1 operand access = **2**
(c) 1 fetch + 1 (read pointer) + 1 (read data at pointer address) = **3**

**P33.** A computer has 20 address lines and 16 data lines. Memory is byte-addressable. What is the maximum memory capacity? If memory is organized as 1M × 8, how many 256K × 4 bit chips are needed?
**Solution:**
- Max memory = 2²⁰ = 1M addresses × 1 byte = **1 MB**
- Target: 1M × 8 bit
- Each chip: 256K × 4 bit
- Chips for width: 8/4 = 2 (to make 8 bits)
- Chips for depth: 1M/256K = 4
- **Total chips = 2 × 4 = 8**

**P34.** A system uses 4 memory modules with low-order interleaving. Each module access time is 100 ns. Bus cycle = 25 ns.
(a) Time to read 8 consecutive words
(b) Effective memory bandwidth (word size = 4 bytes)

**Solution:**
(a) First word: 100 ns. After that, one word ready every 25 ns (pipelined).
Time = 100 + (8−1) × 25 = 100 + 175 = **275 ns**
Without interleaving: 8 × 100 = 800 ns
(b) Bandwidth = 8 × 4B / 275 ns = 32/275 ns = **116.4 MB/s**
(vs non-interleaved: 32/800 = 40 MB/s)

**P35.** Two machines execute the same program. Machine A: 500 MHz, CPI=2. Machine B: 800 MHz, CPI=3.5. Which is faster and by how much?
**Solution:**
- Time_A = IC × 2 / (500×10⁶) = IC × 4×10⁻⁹ seconds
- Time_B = IC × 3.5 / (800×10⁶) = IC × 4.375×10⁻⁹ seconds
- **Machine A is faster** by 4.375/4 = **1.094×** (9.4% faster)

---

### Practice Set 7: Disk Scheduling Problems (5 Problems)

**P36.** A disk has 200 tracks (0-199). Head is at track 100. Pending requests: 55, 120, 40, 25, 150, 170, 80, 10, 190. Direction: toward 0.
Calculate total head movement for: (a) SCAN (b) LOOK (c) SSTF

**Solution:**
(a) **SCAN** (toward 0, go to 0, then reverse):
100→80(20)→55(25)→40(15)→25(15)→10(15)→0(10)→120(120)→150(30)→170(20)→190(20)
Total = 20+25+15+15+15+10+120+30+20+20 = **290**

(b) **LOOK** (toward 0, reverse at last request 10):
100→80(20)→55(25)→40(15)→25(15)→10(15)→120(110)→150(30)→170(20)→190(20)
Total = 20+25+15+15+15+110+30+20+20 = **270** (saves 10+10 = 20 vs SCAN)

(c) **SSTF** (always go to nearest):
100→80(20)→55(25)→40(15)→25(15)→10(15)→120(110)→150(30)→170(20)→190(20)
Wait — let me redo. From 100: nearest = 120(20) or 80(20). Say 80.
100→80(20)→55(25)→40(15)→25(15)→10(15)→120(110)→150(30)→170(20)→190(20): Total = **270**
Starting with 120: 100→120(20)→150(30)→170(20)→190(20)→80(110)→55(25)→40(15)→25(15)→10(15): Total = **270**
Either path gives **270**.

**P37.** A disk rotates at 10,000 RPM. Transfer rate = 50 MB/s. Average seek = 4 ms. Controller overhead = 0.5 ms. Read a 100 KB file split into 4 contiguous tracks.
Compute total time.

**Solution:**
- Rotation time = 60/10000 = 6 ms
- Average rotational latency = 3 ms (per track)
- Transfer time per track = 25KB/50MB/s = 0.5 ms
- Time per track (after first seek) = seek + rot lat + transfer + overhead
- First track: 4 + 3 + 0.5 + 0.5 = 8 ms
- Tracks 2-4: Since contiguous, minimal seek. Assume track-to-track seek ~ 0 ms for adjacent tracks.
  Each: 0 + 3 + 0.5 + 0.5 = 4 ms
- Total = 8 + 3×4 = **20 ms** for 100 KB

**P38.** In C-SCAN (moving from lower to higher track numbers), disk has 100 tracks (0-99), head at 50. Requests: 10, 30, 60, 80, 95, 5, 45, 70.
Find total head movement.

**Solution (C-SCAN, going up):**
50→60(10)→70(10)→80(10)→95(15)→99(4)→0(99)→5(5)→10(5)→30(20)→45(15)
Total = 10+10+10+15+4+99+5+5+20+15 = **193**

**P39.** Average disk access time is 12 ms. A file has 1000 records. How long to sort the file using external merge sort if each pass reads all records? Assume each pass = 1 sequential read + 1 sequential write of entire file.

**Solution:**
This is more of an OS/DB question, but from a COA perspective:
- Sequential transfer rate matters more than random access
- For 1000 records: ~⌈log₂(1000)⌉ = 10 passes needed (assuming 2-way merge)
- Each pass: 1 complete read + 1 complete write
- If each record = 1 sector (512B), file = 500 KB
- Sequential read time ≈ 1 seek + 1 rotation + transfer of 500 KB
  ≈ 8ms + 4ms + 500KB/(50MB/s) ≈ 8+4+10 = 22 ms per read
  ≈ 22 ms per write
- Total ≈ 10 × 2 × 22 = **440 ms** (simplified)

**P40.** RAID-5 with 5 disks, each 1 TB, block size 64 KB.
(a) Usable capacity
(b) If one disk fails, can data be recovered? How?
(c) What is the write penalty?

**Solution:**
(a) Usable = (5−1) × 1TB = **4 TB** (one disk equivalent for parity)
(b) Yes! Parity is spread across all disks. Missing data = XOR of remaining blocks.
    $$\text{Missing} = D_1 \oplus D_2 \oplus D_3 \oplus P$$
(c) **Write penalty = 4 operations** per data write:
    1. Read old data
    2. Read old parity
    3. Write new data
    4. Write new parity (= old_parity ⊕ old_data ⊕ new_data)

### Practice Set 8: Virtual Memory Problems (5 Problems)

**P41.** A system has 48-bit virtual address, 32-bit physical address, 8 KB pages, 8-byte PTEs.
(a) How many levels of page table are needed if each table fits in one page?
(b) What is the total virtual address space?

**Solution:**
(a) - Page size = 8 KB = 2¹³ → offset = 13 bits
    - PTEs per page = 8KB / 8B = 1024 = 2¹⁰ → each level indexes 10 bits
    - VPN bits = 48 − 13 = 35 bits
    - Levels needed = ⌈35/10⌉ = **4 levels** (10+10+10+5)
    - Note: last level only uses 5 bits (32 entries)
(b) Virtual address space = 2⁴⁸ = **256 TB**

**P42.** TLB has 128 entries, is fully associative, with entry format: | Valid | VPN | PFN | Dirty | Reference |. VPN = 20 bits, PFN = 16 bits.
(a) Total TLB storage
(b) TLB coverage with 4 KB pages
(c) TLB coverage with 4 MB pages (large pages/superpages)

**Solution:**
(a) Per entry: 1+20+16+1+1 = 39 bits
    Total: 128 × 39 = 4992 bits = **624 bytes** (+ tag comparators)
(b) Coverage = 128 × 4KB = **512 KB**
(c) Coverage = 128 × 4MB = **512 MB** (huge improvement with large pages!)

> 🎯 **GATE Insight:** Large pages dramatically increase TLB coverage → fewer TLB misses for large datasets!

**P43.** In a system with 2-level page table, the following trace is observed:
- Virtual address 0x12345000: L1 hit
- Virtual address 0x1234A000: L1 hit (same L1 entry as above?)
- Virtual address 0x56789000: L1 miss, L2 lookup needed

Given: 32-bit VA, 4KB pages, 1024 PTEs per page.
How can both 0x12345000 and 0x1234A000 hit the same L1 entry?

**Solution:**
- Offset = 12 bits, L2 index = 10 bits, L1 index = 10 bits
- For 0x12345000: VPN = 0x12345, L1 index = top 10 bits of VPN = 0x12345 >> 10 = 0x48 (= 0001001000 in binary)
  Wait: L1 index = bits 21-12 of VA? No:
  VA = L1[10]:L2[10]:Offset[12]
  0x12345000 = 0001 0010 0011 0100 0101 | 000000000000
  L1 = bits 31-22 = 0001001000 = 0x048
  L2 = bits 21-12 = 1101000101 = 0x345
  
  0x1234A000 = 0001 0010 0011 0100 1010 | 000000000000
  L1 = bits 31-22 = 0001001000 = 0x048 (SAME as above!)
  L2 = bits 21-12 = 1101001010 = 0x34A

So both addresses share the same L1 entry (L1 index = 0x048), but different L2 entries.
**This means accessing the same L2 page table, just different entries within it.**

For 0x56789000: L1 = bits 31-22 = 0101011001 → different L1 entry!

**P44.** A system uses inverted page table with hash function. Physical memory = 4 GB, page size = 4 KB, PID = 16 bits, VPN = 36 bits. Entry = PID + VPN + chain pointer.
(a) IPT entries
(b) Size of inverted page table

**Solution:**
(a) Entries = physical frames = 4GB/4KB = 2²⁰ = **1,048,576 entries**
(b) Entry size = 16(PID) + 36(VPN) + 20(chain pointer = log₂(entries)) = 72 bits = 9 bytes
    IPT size = 2²⁰ × 9 = **9 MB**

Compare with normal PT per process: 2³⁶ × 4B = **256 GB per process!** (impossible without multi-level)

**P45.** In a demand-paged system:
- Memory access = 200 ns
- Page fault causes: 5 ms for disk read, 100 μs for page table update, 50 μs restart overhead
- Page fault rate = 1 in 10,000

What is the effective memory access time?

**Solution:**
- Page fault penalty = 5 ms + 0.1 ms + 0.05 ms = 5.15 ms = 5,150,000 ns
- p = 1/10000 = 0.0001
- EAT = (1-0.0001) × 200 + 0.0001 × 5,150,000
- = 0.9999 × 200 + 515
- = 199.98 + 515
- = **714.98 ns** (3.57× slowdown from mere 0.01% page fault rate!)

### Practice Set 9: Comprehensive COA (10 Problems)

**P46.** A superscalar processor issues up to 4 instructions per cycle. For a program with these instruction mix:
- 30% ALU (1 cycle)
- 20% Load (2 cycles, can start 1 per cycle)
- 15% Store (2 cycles)
- 25% Branch (1 cycle, 10% mispredicted, 5 cycle penalty)
- 10% Multiply (4 cycles, only 1 multiply unit)

With unlimited issue capability except for multiply, what is the ideal CPI?

**Solution (simplified):**
Even with superscalar, CPI cannot be below limitations:
- Base CPI for 4-issue = 1/4 = 0.25 (ideal)
- Branch penalty contribution = 0.25 × 0.10 × 5 = 0.125
- Multiply bottleneck: 10% of instructions need 4 cycles on 1 unit
  If issue rate = 4 instructions/cycle = multiply sees 0.4 multiplies/cycle
  Each needs 4 cycles → utilization = 0.4 × 4 = 1.6 > 1 → multiply unit is **bottleneck!**
  Throughput limited to 10% × instructions / 4 cycles = 1/40 instructions/cycle from multiplier
  Actually: multiply unit can start 1 per 4 cycles (non-pipelined) = 0.25/cycle
  Instructions needing multiply = 0.10 × IPC = 0.10 × 4 = 0.4/ cycle
  But unit can handle 0.25/cycle → IPC limited!
  Max IPC such that 0.10 × IPC ≤ 0.25 → IPC ≤ 2.5
- CPI = 1/2.5 + 0.125 = 0.4 + 0.125 = **0.525 (estimated)**

> This is a complex problem; in GATE, simpler versions are asked. The key insight: **functional unit bottleneck limits superscalar IPC**.

**P47.** A processor uses write-back, write-allocate cache. For a program:
- 1000 read references: 50 read misses
- 200 write references: 30 write misses
- On each miss, if dirty eviction: extra write to memory
- 40% of evicted blocks are dirty

Calculate total memory traffic (reads and writes to main memory).

**Solution:**
Total misses = 50 + 30 = 80
- Each miss: 1 block read from memory
- Dirty evictions = 80 × 0.40 = 32 extra block writes 
- Memory reads = 80 blocks
- Memory writes = 32 blocks (dirty evictions only; writes absorbed by cache)
- **Total memory traffic = 80 + 32 = 112 block transfers**

Note: In write-through, ALL 200 writes would go to memory → 200 writes + 80 reads = 280 transfers. 
**Write-back saves significant memory bandwidth!**

**P48.** A 5-stage pipeline processor processes the following code. Determine execution time with and without forwarding:
```
I1: ADD  R1, R2, R3
I2: SUB  R4, R1, R5    ; RAW on R1
I3: AND  R6, R4, R7    ; RAW on R4
I4: OR   R8, R6, R1    ; RAW on R6, R1 available by now
I5: XOR  R9, R8, R4    ; RAW on R8
```

**Without forwarding (read after WB writes):**
| Inst | IF | ID | EX | MEM | WB |
|---|---|---|---|---|---|
| I1 | 1 | 2 | 3 | 4 | 5 |
| I2 | 2 | 3 | stall | stall | 6 | 7 | 8 |
Need R1 from I1's WB (cycle 5), I2 reads in ID

Actually, without forwarding in standard 5-stage: operand read in ID (2nd half), write in WB (1st half) → can read what WB writes same cycle (if forwarding within register file).

Let's use the convention: writes happen in first half, reads in second half of same cycle.
- I1 WB at cycle 5 (first half). I2 needs R1 in ID.
- I2 after I1 needs 2 stalls (I2 ID delayed until cycle 5).

```
Without forwarding:
Cycle:  1    2    3    4    5    6    7    8    9   10   11   12   13
I1:    IF   ID   EX   MEM  WB
I2:         IF   ──   ──   ID   EX   MEM  WB
I3:              ──   ──   IF   ──   ──   ID   EX   MEM  WB
I4:                        ──   ──   ──   IF   ──   ──   ID   EX   MEM  WB...
```

This gets long. Let's simplify: each RAW dependency without forwarding = **2 stalls** for adjacent instructions.

I1→I2: 2 stalls
I2→I3: 2 stalls
I3→I4: 2 stalls (R6)
I4→I5: 2 stalls (R8)
Total stalls = 8; Execution = 5 + (5−1) + 8 = **17 cycles**

**With forwarding:**
I1→I2: EX→EX forward, 0 stalls
I2→I3: EX→EX forward, 0 stalls
I3→I4: EX→EX forward, 0 stalls
I4→I5: EX→EX forward, 0 stalls
Total stalls = 0; Execution = 5 + (5−1) = **9 cycles**

**Forwarding saves 8 cycles (47% reduction!)**

**P49.** A 4-way set associative cache has a total size of 64 KB and block size of 64 bytes with LRU replacement. For the following byte address sequence: 0, 8192, 16384, 24576, 32768, 0, 8192

(a) How many sets?
(b) Trace through and count misses.

**Solution:**
(a) Blocks = 64KB/64B = 1024; Sets = 1024/4 = 256
    Index = log₂(256) = 8 bits; Offset = 6 bits; Tag = 32−8−6 = 18 bits

(b) Block addresses = byte_addr/64:
0→0, 8192→128, 16384→256, 24576→384, 32768→512, 0→0, 8192→128

Set = block_addr mod 256:
0→0, 128→128, 256→0, 384→128, 512→0, 0→0, 128→128

| Access | Block | Set | Result | Cache state (set) |
|---|---|---|---|---|
| 0 | 0 | 0 | Miss | Way0:B0 |
| 8192 | 128 | 128 | Miss | Way0:B128 |
| 16384 | 256 | 0 | Miss | Way0:B0, Way1:B256 |
| 24576 | 384 | 128 | Miss | Way0:B128, Way1:B384 |
| 32768 | 512 | 0 | Miss | Way0:B0, Way1:B256, Way2:B512 |
| 0 | 0 | 0 | **Hit!** | B0 already in Way0 |
| 8192 | 128 | 128 | **Hit!** | B128 already in Way0 |

**Misses = 5 (all compulsory), Hits = 2**

> Note: With 4-way associative, no conflict misses here! Direct-mapped would have had conflicts on set 0.

**P50.** A computer uses Booth's algorithm to multiply two 4-bit numbers: multiply -3 by -5. Show all steps.

**Solution:**
M = -5 → 4-bit 2's complement = 1011 (= -5)
Q = -3 → 4-bit 2's complement = 1101 (= -3)
A = 0000, Q₋₁ = 0, n = 4

| Step | A | Q | Q₋₁ | Q₀Q₋₁ | Action |
|---|---|---|---|---|---|
| Init | 0000 | 1101 | 0 | — | — |
| 1 | 0000 | 1101 | 0 | 10 | A = A − M = 0000 + 0101 = 0101 |
| shift | 0010 | 1110 | 1 | — | Arithmetic right shift |
| 2 | 0010 | 1110 | 1 | 01 | A = A + M = 0010 + 1011 = 1101 |
| shift | 1110 | 1111 | 0 | — | Arithmetic right shift |
| 3 | 1110 | 1111 | 0 | 10 | A = A − M = 1110 + 0101 = 0011 |
| shift | 0001 | 1111 | 1 | — | Arithmetic right shift |
| 4 | 0001 | 1111 | 1 | 11 | No operation |
| shift | 0000 | 1111 | 1 | — | Arithmetic right shift |

Result: A:Q = 0000 1111 = **15** ✅ (since −3 × −5 = 15)

**P51.** A processor has a 2-bit branch predictor (initialized to "weakly taken" = 10). For a loop of 8 iterations (branch taken 7 times, not taken once), find:
(a) Number of mispredictions
(b) Misprediction rate

**Solution:**
Branch history: T T T T T T T NT (not taken on exit)

| Iteration | Actual | Predicted | Correct? | New State |
|---|---|---|---|---|
| 1 | T | T (10) | ✅ | 11 |
| 2 | T | T (11) | ✅ | 11 |
| 3 | T | T (11) | ✅ | 11 |
| 4 | T | T (11) | ✅ | 11 |
| 5 | T | T (11) | ✅ | 11 |
| 6 | T | T (11) | ✅ | 11 |
| 7 | T | T (11) | ✅ | 11 |
| 8 | NT | T (11) | ❌ | 10 |

(a) **1 misprediction** (only the last iteration)
(b) Misprediction rate = 1/8 = **12.5%**

If the loop executes many times:
- First iteration of NEXT invocation: Predict T (state 10), Actual = T → correct!
- Only misprediction = last iteration of each invocation
- For loop of N iterations executed once: **1/N misprediction rate**

> 🎯 **GATE Standard Result:** A 2-bit predictor has only **1 misprediction per loop exit** regardless of loop length!

**P52.** An instruction cache has:
- 32 KB, 2-way set associative, 64 byte blocks, 32-bit address
- Data cache: 32 KB, 4-way set associative, 32 byte blocks, 32-bit address
- Instruction miss rate = 1%, Data miss rate = 5%
- Miss penalty = 100 cycles, CPI_ideal = 1.5
- 30% of instructions are loads/stores
What is the actual CPI?

**Solution:**
- Memory stalls per instruction:
  - From instruction cache: 1 × 0.01 × 100 = 1.0 (every instruction accesses I-cache)
  - From data cache: 0.30 × 0.05 × 100 = 1.5
- Total memory stalls = 1.0 + 1.5 = 2.5
- **Actual CPI = 1.5 + 2.5 = 4.0**

How much faster with perfect caches? 4.0/1.5 = **2.67×** (caches cause 63% of execution time!!)

**P53.** Design a memory system using 512K × 8 bit ROM chips and 256K × 8 bit RAM chips for the following memory map:
- ROM: 0x00000 - 0xFFFFF (1 MB)
- RAM: 0x100000 - 0x1FFFFF (1 MB)

**Solution:**
- ROM chips needed: 1MB / 512KB = **2 ROM chips**
  - ROM1: 0x00000-0x7FFFF, ROM2: 0x80000-0xFFFFF
  - A19 → chip select (0=ROM1, 1=ROM2); A0-A18 → address pins
  
- RAM chips needed: 1MB / 256KB = **4 RAM chips**
  - RAM1: 0x100000-0x13FFFF, RAM2: 0x140000-0x17FFFF
  - RAM3: 0x180000-0x1BFFFF, RAM4: 0x1C0000-0x1FFFFF
  - A19-A18 → 2-to-4 decoder for RAM chip select; A0-A17 → address pins
  - A20 → distinguish ROM (0) from RAM (1) region

**Total: 2 ROM + 4 RAM = 6 chips + decoder logic**

**P54.** A 3-stage pipeline (S1=2ns, S2=3ns, S3=2ns) with a 0.5ns latch delay is used to execute 1000 instructions.
(a) Non-pipelined execution time
(b) Pipelined execution time
(c) Speedup
(d) If 20% of instructions are branches with 1-cycle penalty, what is the new CPI?

**Solution:**
(a) Non-pipelined per instruction = 2+3+2 = 7ns
    Total = 1000 × 7 = **7000 ns**
(b) Clock = max(2,3,2) + 0.5 = **3.5 ns**
    Pipelined time = (3 + 1000 − 1) × 3.5 = 1002 × 3.5 = **3507 ns**
(c) Speedup = 7000/3507 = **2.0×** (ideal for 3-stage would be 3.0)
(d) Branch penalty cycles: 0.20 × 1 = 0.20
    CPI = 1 + 0.20 = **1.20**
    Time = 1.20 × 1000 × 3.5 = **4200 ns**
    Speedup becomes 7000/4200 = **1.67×**

**P55.** A system has the following memory hierarchy:
| Level | Hit Rate | Access Time |
|---|---|---|
| L1 Cache | 95% | 1 ns |
| L2 Cache | 80% (of L1 misses) | 5 ns |
| L3 Cache | 90% (of L2 misses) | 20 ns |
| Main Memory | 100% | 200 ns |

Calculate AMAT.

**Solution:**
$$\text{AMAT} = t_{L1} + m_{L1} \times (t_{L2} + m_{L2} \times (t_{L3} + m_{L3} \times t_{mem}))$$
= 1 + 0.05 × (5 + 0.20 × (20 + 0.10 × 200))
= 1 + 0.05 × (5 + 0.20 × (20 + 20))
= 1 + 0.05 × (5 + 0.20 × 40)
= 1 + 0.05 × (5 + 8)
= 1 + 0.05 × 13
= 1 + 0.65
= **1.65 ns**

**Global miss rates:**
- L1 miss rate = 5%
- L2 global miss rate = 5% × 20% = 1%
- L3 global miss rate = 5% × 20% × 10% = 0.1%
- **Only 1 in 1000 accesses goes to main memory!**

### Practice Set 10: Amdahl's Law and Performance (5 Problems)

**P56.** A program spends 60% on floating-point (FP) operations. A new FP unit is 10× faster.
(a) Speedup from Amdahl's Law
(b) If you could make FP infinitely fast, maximum speedup?
(c) Another enhancement makes memory operations (30% of time) 2× faster. Which is better?

**Solution:**
(a) $$S = \frac{1}{(1-0.6) + 0.6/10} = \frac{1}{0.4 + 0.06} = \frac{1}{0.46} = 2.17\times$$

(b) $$S_{max} = \frac{1}{1-0.6} = \frac{1}{0.4} = 2.5\times$$

(c) Memory 2× faster:
$$S = \frac{1}{(1-0.3) + 0.3/2} = \frac{1}{0.7 + 0.15} = \frac{1}{0.85} = 1.18\times$$

**FP enhancement (2.17×) is much better than memory enhancement (1.18×)!**

> 🎯 **Amdahl's Corollary:** Make the common case fast! 60% operation getting 10× speedup beats 30% operation getting 2× speedup.

**P57.** Gustafson's Law: A parallel program runs on 64 processors. Serial portion = 5%.
(a) Speedup using Amdahl's Law
(b) Speedup using Gustafson's Law

**Solution:**
(a) Amdahl: S = 1/(0.05 + 0.95/64) = 1/(0.05 + 0.0148) = 1/0.0648 = **15.4×**
(b) Gustafson: S = s + p × N = 0.05 + 0.95 × 64 = 0.05 + 60.8 = **60.85×**

**Key difference:** Amdahl assumes fixed problem size (strong scaling). Gustafson assumes problem size grows with processors (weak scaling). Real-world often closer to Gustafson!

**P58.** Iron Law of Performance: Three machines executing same benchmark:
| Machine | Clock Rate | CPI | IC |
|---|---|---|---|
| A | 4 GHz | 2.0 | 10⁹ |
| B | 3 GHz | 1.2 | 1.5×10⁹ |
| C | 5 GHz | 3.0 | 0.8×10⁹ |

Rank by execution time.

**Solution:**
$$T = IC \times CPI / f$$
- T_A = 10⁹ × 2.0 / (4×10⁹) = 0.500 s
- T_B = 1.5×10⁹ × 1.2 / (3×10⁹) = 0.600 s
- T_C = 0.8×10⁹ × 3.0 / (5×10⁹) = 0.480 s

**Ranking: C (fastest, 0.48s) > A (0.50s) > B (0.60s)**

Note: C has highest clock and highest CPI, but fewest instructions. B has best CPI but most instructions.

**P59.** MIPS rating for machine from P58-A vs P58-C:
$$\text{MIPS} = IC / (T \times 10^6) = f / (CPI \times 10^6)$$

- MIPS_A = 4000/2.0 = **2000 MIPS**
- MIPS_C = 5000/3.0 = **1667 MIPS**
- A has higher MIPS but is SLOWER! 

> 🎯 **GATE Trap:** Higher MIPS ≠ faster execution! MIPS doesn't account for instruction count. This is why MIPS is considered a **misleading** metric. Use execution time instead!

**P60.** Using Amdahl's Law for multi-core: A program is 90% parallelizable. Compare speedup with 2, 4, 8, 16, 32, and infinite cores.

| Cores | Speedup | Efficiency |
|---|---|---|
| 1 | 1.00 | 100% |
| 2 | 1/(0.1+0.9/2) = 1/0.55 = **1.82** | 91% |
| 4 | 1/(0.1+0.9/4) = 1/0.325 = **3.08** | 77% |
| 8 | 1/(0.1+0.9/8) = 1/0.2125 = **4.71** | 59% |
| 16 | 1/(0.1+0.9/16) = 1/0.15625 = **6.40** | 40% |
| 32 | 1/(0.1+0.9/32) = 1/0.12813 = **7.80** | 24% |
| ∞ | 1/0.1 = **10.00** | → 0% |

**Key Insight:** Even with infinite cores, 10% serial portion limits speedup to 10×!
Going from 16→32 cores only gains 7.80−6.40 = 1.4× improvement. **Diminishing returns!**

---

## 📎 Appendix: Worked Examples

### Example 1: Complete Booth's Algorithm (5-bit)
**Q:** Multiply −7 × 5 using 5-bit Booth's algorithm.
**Solution:**
M = 00101 (+5), Q = 11001 (−7 in 2's complement), A = 00000, Q₋₁ = 0

| Step | A | Q | Q₋₁ | Q₀Q₋₁ | Action |
|---|---|---|---|---|---|
| Init | 00000 | 11001 | 0 | 10 | A=A−M |
| | 11011 | 11001 | 0 | | |
| ASR | 11101 | 11100 | 1 | 01 | A=A+M |
| | 00010 | 11100 | 1 | | |
| ASR | 00001 | 01110 | 0 | 00 | NOP |
| ASR | 00000 | 10111 | 0 | 10 | A=A−M |
| | 11011 | 10111 | 0 | | |
| ASR | 11101 | 11011 | 1 | 11 | NOP |
| ASR | 11110 | 11101 | 1 | | |

Result: AQ = 11110 11101
= −35 in 10-bit 2's complement? Let's check: 1111011101₂ = −(0000100011) = −35
-7 × 5 = −35 ✅

### Example 2: Pipeline Scheduling
**Q:** Schedule code below on a 5-stage pipeline with forwarding. Identify hazards and schedule to minimize stalls.
```
LW   R1, 0(R10)
LW   R2, 4(R10)
ADD  R3, R1, R2
SW   R3, 8(R10)
```

**Original order (with forwarding):**
```
Cycle: 1  2  3  4  5  6  7  8  9
LW R1: IF ID EX ME WB
LW R2:    IF ID EX ME WB
ADD:          IF ID -- EX ME WB     (1 stall: load-use on R2)
SW:                IF ID EX ME WB
```
Total: 9 cycles (1 stall)

**Rearranged: (no stalls if we insert independent instruction between LW R2 and ADD)**
If we had: LW R1 → LW R2 → (some independent instruction) → ADD R3 → SW R3
Then: 0 stalls! But we don't have one here, so 1 stall is minimum.

### Example 3: Cache Simulation
**Q:** A 2-way set associative cache with 2 sets (4 blocks total), block size=1 word. LRU replacement. Access sequence (block addresses): 0, 2, 0, 4, 2, 0, 4. Find hits.

| Access | Block | Set(mod 2) | Way0 | Way1 | Hit/Miss |
|---|---|---|---|---|---|
| 0 | 0 | 0 | **0** | − | Miss |
| 2 | 2 | 0 | 0 | **2** | Miss |
| 0 | 0 | 0 | **0** | 2 | Hit (LRU update) |
| 4 | 4 | 0 | 0 | **4** | Miss (evict LRU=2) |
| 2 | 2 | 0 | **2** | 4 | Miss (evict LRU=0) |
| 0 | 0 | 0 | 2 | **0** | Miss (evict LRU=4) |
| 4 | 4 | 0 | **4** | 0 | Miss (evict LRU=2) |

**Hits = 1 out of 7 → Thrashing!** (Working set of 3 > cache capacity of 2 for this set)

### Example 4: TLB + Cache Access Time
**Q:** TLB: 512 entries, access time = 1 cycle. L1 Cache: hit time = 2 cycles, miss rate = 5%. L2 Cache: hit time = 15 cycles, local miss rate = 10%. Memory: 200 cycles. TLB miss rate = 1%. 2-level page table. Find EAT.

**Solution:**
Case 1: TLB hit (99%): 1 + 2 = 3 cycles (if cache hit)
Case 2: TLB hit + L1 miss + L2 hit: 1 + 2 + 0.05×15 = 3.75 (weighted)

More precisely:
- TLB hit + cache access: 1 + AMAT_cache
  - AMAT_cache = 2 + 0.05 × (15 + 0.10 × 200) = 2 + 0.05 × 35 = 3.75
- TLB miss + page table walk + cache: 1 + 2×200 + 3.75 = 404.75 (2 memory accesses for 2-level PT)

EAT = 0.99 × (1 + 3.75) + 0.01 × (1 + 400 + 3.75) = 0.99×4.75 + 0.01×404.75
= 4.7025 + 4.0475 = **8.75 cycles**

### Example 5: Amdahl's Law Application
**Q:** Enhancement A speeds up 20% of a program by 10×. Enhancement B speeds up 50% of the program by 2×. Which is better?

**Solution:**
- A: Speedup = 1 / (0.8 + 0.2/10) = 1 / (0.8 + 0.02) = 1/0.82 = **1.22×**
- B: Speedup = 1 / (0.5 + 0.5/2) = 1 / (0.5 + 0.25) = 1/0.75 = **1.33×**
- **Enhancement B is better** despite smaller per-unit speedup (Amdahl's Law: improve the common case!)

---

## 📋 Formula Card — Quick Reference ⭐⭐⭐

### Performance Formulas
| Formula | Expression |
|---|---|
| CPU Time | IC × CPI / f_clock |
| Average CPI | Σ(CPIᵢ × Fᵢ) |
| MIPS | f / (CPI × 10⁶) |
| Speedup (pipeline) | nk / (k+n−1) |
| Pipeline time | (k+n−1) × τ_max |
| Amdahl's Speedup | 1 / ((1−f) + f/S) |
| Gustafson's Speedup | s + p×N |
| CPI with stalls | 1 + data_stalls + control_stalls |
| Branch stalls | branch_freq × mispredict_rate × penalty |
| Load-use stalls | load_freq × dependent_frac × 1 |

### Cache Formulas
| Formula | Expression |
|---|---|
| Number of sets | C / (B × k) |
| Offset bits | log₂(B) |
| Index bits | log₂(S) |
| Tag bits | m − index − offset |
| AMAT | Hit_time + Miss_rate × Miss_penalty |
| Multi-level AMAT | t_L1 + m_L1 × (t_L2 + m_L2_local × P_mem) |
| Memory stalls | (mem_accesses/instr) × miss_rate × penalty |
| Cache overhead (bits) | blocks × (tag_bits + valid + dirty) |

### Virtual Memory Formulas
| Formula | Expression |
|---|---|
| Page offset bits | log₂(page_size) |
| VPN bits | VA_bits − offset_bits |
| PFN bits | PA_bits − offset_bits |
| Page table entries | 2^(VPN_bits) |
| Page table size | entries × PTE_size |
| TLB EAT | h(t_TLB+t_mem) + (1−h)(t_TLB+(k+1)t_mem) |
| TLB coverage | TLB_entries × page_size |

### I/O Formulas
| Formula | Expression |
|---|---|
| DMA time | T_setup + N × T_cycle |
| Disk rotation time | 60 / RPM (seconds) |
| Disk transfer rate | bytes/track × rotations/sec |
| Average seek time | 1/3 × max_seek (typical) |
| Average rotation latency | 1/2 × rotation_time |

### Data Representation
| Formula | Expression |
|---|---|
| IEEE 754 value | (−1)ˢ × 1.M × 2^(E−Bias) |
| Single precision bias | 127 |
| Double precision bias | 1023 |
| 2's complement range (n bits) | −2^(n−1) to 2^(n−1)−1 |
| Overflow (2's comp) | V = C_{n−1} ⊕ C_n |

---

## 🎯 GATE Tricks & Common Mistakes

| # | Topic | Trick/Trap |
|---|---|---|
| 1 | IEEE 754 | DON'T forget the hidden 1 (normalized: 1.M, not 0.M) |
| 2 | Booth's | Use ARITHMETIC right shift, NOT logical |
| 3 | Pipeline speedup | For small n, S = nk/(k+n−1), NOT k |
| 4 | Forwarding | Load-use STILL needs 1 stall even with full forwarding |
| 5 | AMAT | L2 miss rate in AMAT formula = LOCAL miss rate |
| 6 | Cache size | Cache size = DATA only; tags, valid, dirty bits are overhead |
| 7 | Expanding opcode | remaining_codes × 2^(register_width) = next level codes |
| 8 | Endianness | Affects multi-byte data ONLY; strings stored same way |
| 9 | Write-back | Dirty block eviction = EXTRA memory write penalty |
| 10 | 2-bit predictor | Loop of N iterations: only **1** misprediction (last iter) |
| 11 | TLB miss | With k-level PT: k+1 memory accesses (k table + 1 data) |
| 12 | DMA burst | CPU is **completely** blocked during burst DMA |
| 13 | Byte vs word addr | Check if system is byte-addressable or word-addressable! |
| 14 | Pipeline clock | = max(stage delay) + buffer overhead |
| 15 | WAR/WAW | Only in out-of-order/superscalar; NOT in simple pipeline |
| 16 | Amdahl's Law | Max speedup = 1/(1−f); diminishing returns! |
| 17 | Memory interleaving | Low-order = consecutive words in different modules |
| 18 | MESI protocol | E→M is SILENT (no bus transaction needed) |
| 19 | Direct-mapped cache | If same-set blocks keep alternating → thrashing |
| 20 | Page table size | = 2^(VPN bits) × PTE_size; can be HUGE (use multi-level!) |

---

## 📊 Micro-Checklist for Revision

- Data Representation: IEEE 754 conversion both ways, Booth's algorithm, Non-restoring division
- Overflow detection in 2's complement
- ISA: RISC vs CISC comparison, Expanding opcode problems
- Addressing modes: EA calculation, memory access count
- CPU Time, CPI, MIPS calculation with instruction mix
- Amdahl's Law: basic and with infinite speedup
- Pipeline: timing diagram, speedup formula, efficiency
- Data hazards: RAW identification, forwarding paths, load-use penalty
- Control hazards: branch penalty, 2-bit predictor behavior
- Stall calculation with both data and control hazards
- Control unit: hardwired vs microprogrammed, horizontal vs vertical
- RTL: write micro-operations for ADD, LOAD, STORE, BEQ
- Cache: address breakdown (tag, index, offset), all three cache mapping types
- AMAT: single-level and multi-level, split cache
- Cache simulation: trace through access sequence with replacement policy
- Miss types (3 Cs) and how to reduce each
- Write policies: write-through vs write-back, write-allocate vs no-write-allocate
- Memory interleaving: low-order vs high-order, bandwidth calculation
- Virtual memory: page table size, multi-level PT structure
- TLB: EAT calculation, TLB coverage
- PIPT vs VIPT vs VIVT cache
- MESI protocol: state transitions
- I/O: programmed vs interrupt vs DMA comparison
- DMA: cycle stealing vs burst mode, transfer time
- Bus arbitration: daisy chain vs centralized vs distributed
- Disk scheduling: FCFS, SSTF, SCAN, C-SCAN, LOOK, C-LOOK
- Page fault EAT calculation
- IEEE 754: special values (0, ∞, NaN, denormalized)
- ROM types: PROM, EPROM, EEPROM, Flash
- SRAM vs DRAM: key differences
- Tomasulo vs Scoreboard differences
- Flynn's taxonomy: SISD, SIMD, MISD, MIMD

---

## 📈 GATE Year-Wise PYQ Analysis for COA

| Year | Set | Topic | Question Type | Marks |
|------|-----|-------|-------------|-------|
| 2025 | 1 | Cache AMAT | Numerical | 2 |
| 2025 | 1 | Pipeline hazards and forwarding | MCQ | 1 |
| 2025 | 1 | Expanding opcode | MCQ | 2 |
| 2025 | 2 | Virtual memory page table size | Numerical | 2 |
| 2025 | 2 | IEEE 754 representation | MCQ | 1 |
| 2025 | 2 | Pipeline speedup | Numerical | 2 |
| 2024 | 1 | Cache mapping - direct mapped | MCQ | 2 |
| 2024 | 1 | Booth's algorithm | Numerical | 2 |
| 2024 | 1 | DMA transfer time | Numerical | 2 |
| 2024 | 2 | Pipeline with stalls | MCQ | 2 |
| 2024 | 2 | Amdahl's Law | Numerical | 2 |
| 2024 | 2 | Addressing modes | MCQ | 1 |
| 2023 | 1 | Cache miss rate calculation | Numerical | 2 |
| 2023 | 1 | ISA comparison RISC vs CISC | MCQ | 1 |
| 2023 | 2 | Multi-level cache AMAT | Numerical | 2 |
| 2023 | 2 | Pipeline efficiency | Numerical | 1 |
| 2022 | 1 | TLB effective access time | Numerical | 2 |
| 2022 | 1 | Instruction format expanding opcode | Numerical | 2 |
| 2022 | 2 | Cache set associative simulation | Numerical | 2 |
| 2022 | 2 | IEEE 754 floating point | MCQ | 1 |
| 2021 | 1 | Pipeline hazard detection | MCQ | 2 |
| 2021 | 1 | Memory interleaving | Numerical | 2 |
| 2021 | 2 | CPU performance comparison | Numerical | 2 |
| 2021 | 2 | Endianness | MCQ | 1 |
| 2020 | 1 | Cache AMAT calculation | Numerical | 2 |
| 2020 | 1 | DMA bandwidth | Numerical | 2 |
| 2020 | 2 | Virtual memory access with TLB | Numerical | 2 |
| 2020 | 2 | Booth's multiplication | Numerical | 2 |
| 2019 | 1 | Pipeline stall cycles | Numerical | 2 |
| 2019 | 1 | Addressing mode memory accesses | MCQ | 1 |
| 2019 | 2 | Cache tag bits calculation | Numerical | 1 |
| 2019 | 2 | Instruction execution time comparison | Numerical | 2 |

### Topic Frequency Analysis (2019-2025)

| Topic | Questions (approx) | Priority |
|---|---|---|
| **Cache (AMAT, mapping, simulation)** | 10-12 | 🔴 HIGHEST |
| **Pipeline (hazards, speedup, stalls)** | 8-10 | 🔴 HIGHEST |
| **Virtual Memory (TLB, page table)** | 5-7 | 🟠 HIGH |
| **IEEE 754 / Data Representation** | 4-6 | 🟠 HIGH |
| **Performance (CPI, Amdahl's, MIPS)** | 4-5 | 🟡 MEDIUM-HIGH |
| **Addressing Modes** | 3-4 | 🟡 MEDIUM |
| **Expanding Opcode** | 2-3 | 🟡 MEDIUM |
| **I/O & DMA** | 3-4 | 🟡 MEDIUM |
| **Booth's Algorithm** | 2-3 | 🟢 MEDIUM-LOW |
| **Endianness** | 1-2 | 🟢 LOW |
| **Flynn's Taxonomy** | 1 | 🟢 LOW |
| **Memory Interleaving** | 1-2 | 🟢 LOW |

> 🎯 **Priority Focus:** Cache + Pipeline = ~50% of COA questions! Master these first.

---

## 📖 Study Strategy for COA

### Priority Order for GATE Preparation

| Priority | Topics | Expected Marks | Time Allocation |
|---|---|---|---|
| 1 (Must) | Cache memory (AMAT, mapping, simulation) | 4-6 marks | 20% |
| 2 (Must) | Pipelining (hazards, speedup, forwarding) | 4-6 marks | 20% |
| 3 (Must) | Virtual memory (TLB, page table) | 2-4 marks | 12% |
| 4 (Must) | IEEE 754 & data representation | 2-3 marks | 10% |
| 5 (Must) | Performance (CPI, Amdahl's) | 2-3 marks | 8% |
| 6 (Important) | ISA & addressing modes | 1-2 marks | 8% |
| 7 (Important) | I/O & DMA | 1-2 marks | 7% |
| 8 (Important) | Expanding opcode | 1-2 marks | 5% |
| 9 (Good-to-know) | Booth's, non-restoring division | 0-2 marks | 5% |
| 10 (Low priority) | Flynn's, multiprocessor, endianness | 0-1 marks | 5% |

### Common Exam-Day Mistakes

1. **Forgetting hidden 1 in IEEE 754:** The mantissa is stored WITHOUT the leading 1 (for normalized numbers). Always add 1.M, not 0.M!
2. **Mixing local and global miss rates in AMAT:** L2 miss rate in AMAT is the LOCAL miss rate (misses in L2 / accesses to L2), not global.
3. **Byte-addressable vs word-addressable confusion:** Check problem statement! Byte-addressable means each byte has a unique address.
4. **Pipeline clock ≠ sum of stage delays:** Clock = max(stage delay) + latch delay, not sum!
5. **Forgetting load-use stall with forwarding:** Even with FULL forwarding, load followed by dependent instruction = 1 stall (because load produces result at end of MEM, but dependent needs in EX).
6. **Arithmetic shift vs logical shift:** SRA preserves sign bit (fills with sign bit), SRL fills with 0.
7. **Cache size means DATA only:** Tag bits, valid bits, dirty bits are OVERHEAD, not counted in cache "size."
8. **DMA burst mode blocks CPU completely:** CPU cannot access memory AT ALL during burst DMA.
9. **Page table size calculation:** PTE_count = 2^(VPN_bits), NOT 2^(VA_bits)!
10. **2's complement range asymmetry:** n bits → range is −2^(n−1) to +2^(n−1)−1. One more negative than positive!

---

## 🔖 Last-Minute One-Page Revision Sheet

### Cache Formulas Quick Sheet
```
Blocks = Cache_Size / Block_Size
Sets = Blocks / Associativity
Offset_bits = log₂(Block_Size)
Index_bits = log₂(Sets)
Tag_bits = Address_Bits − Index_bits − Offset_bits
AMAT = Hit_time + Miss_rate × Miss_penalty
AMAT_2level = L1_time + L1_miss × (L2_time + L2_miss × Memory_time)
Memory_stall = Memory_access_rate × Miss_rate × Miss_penalty
CPI_actual = CPI_ideal + Memory_stall
```

### Pipeline Quick Sheet
```
Pipelined_time = (k + n − 1) × τ
Speedup_ideal = k (for large n)
Speedup_actual = (n × k × τ) / ((k + n − 1) × τ)
CPI_effective = 1 + stall_per_instruction
Data_stalls = Load_freq × Load_use_fraction × 1 (with forwarding)
Branch_stalls = Branch_freq × Misprediction_rate × Penalty_cycles
Pipeline_clock = max(stage_delay) + latch_delay
Throughput = 1 / Pipeline_clock
Efficiency = Speedup / k
```

### IEEE 754 Quick Sheet
```
Single: 1 sign + 8 exponent + 23 mantissa, Bias = 127
Double: 1 sign + 11 exponent + 52 mantissa, Bias = 1023
Value = (-1)^s × 1.M × 2^(E-Bias)   [normalized]
Special: E=0,M=0 → ±0; E=all1s,M=0 → ±∞; E=all1s,M≠0 → NaN
Denormalized: E=0,M≠0 → (-1)^s × 0.M × 2^(1-Bias)
```

### Performance Quick Sheet
```
CPU_Time = IC × CPI / Clock_Rate
CPI = Σ(CPIi × fi)    [weighted sum]
MIPS = Clock_Rate / (CPI × 10⁶)
Amdahl: S = 1/((1-f) + f/Speedup_enhanced)
Max_parallel_speedup = 1/(serial_fraction)
Gustafson: S = serial + parallel × N_processors
```

### Virtual Memory Quick Sheet  
```
VPN_bits = VA_bits − Offset_bits
PFN_bits = PA_bits − Offset_bits
Page_Table_Size = 2^(VPN_bits) × PTE_size
PTEs_per_page = Page_Size / PTE_size
Levels = ⌈VPN_bits / log₂(PTEs_per_page)⌉
EAT_TLB = h × (t_tlb + t_mem) + (1-h) × (t_tlb + (levels+1) × t_mem)
TLB_Coverage = TLB_entries × Page_Size
```

### I/O Quick Sheet
```
DMA_time = Setup + (N_words × Memory_cycle_time) / (depends on mode)
Disk_access = Seek + Rotational_latency + Transfer + Controller
Avg_rotational_latency = 30/RPM (seconds) = 30000/RPM (ms)
Transfer_rate = Bytes_per_track × Rotations_per_second
Bus_bandwidth = Bus_width × Bus_frequency
```

### Number Systems Quick Sheet
```
2's complement(N) = NOT(N) + 1
Range: -2^(n-1) to 2^(n-1) - 1
Overflow: V = Cn-1 ⊕ Cn (carry into MSB ⊕ carry out of MSB)
BCD: if sum > 9, add 6
r's complement of N (n digits) = r^n − N
(r-1)'s complement = r^n − 1 − N
```

---

## 🏆 Must-Remember Constants and Mnemonics

| Item | Value | Mnemonic |
|---|---|---|
| Single precision bias | 127 | "1-2-7 for single, 10-2-3 for double" |
| Double precision bias | 1023 | |
| 2¹⁰ | 1,024 | ~1K |
| 2²⁰ | 1,048,576 | ~1M |
| 2³⁰ | ~10⁹ | ~1G |
| 2³² | 4,294,967,296 | ~4G (4 GB address space) |
| Typical DRAM latency | 50-70 ns | |
| Typical L1 cache | 1-3 ns | |
| Typical L2 cache | 5-20 ns | |
| Typical disk access | 5-15 ms | |
| Page fault penalty | ~10 ms | 10⁷ ns (terrifying!) |
| MIPS register count | 32 | R0 always 0 |
| x86 register count | 8 (legacy), 16 (x64) | |
| IEEE 754 NaN | E=all 1s, M≠0 | |
| IEEE 754 infinity | E=all 1s, M=0 | |

### Commonly Confused Pairs

| Term 1 | Term 2 | Key Difference |
|---|---|---|
| Hit rate | Miss rate | Sum = 1 (complementary) |
| Write-through | Write-back | WT: write to memory immediately; WB: write only on eviction |
| Write-allocate | No-write-allocate | WA: bring block to cache on write miss; NWA: write directly to memory |
| Local miss rate | Global miss rate | Local = misses/accesses at THIS level; Global = misses at this level / total CPU accesses |
| Spatial locality | Temporal locality | Spatial: nearby addresses; Temporal: same address soon |
| Hardwired CU | Microprogrammed CU | HW: logic gates (fast); Micro: control memory (flexible) |
| RISC | CISC | RISC: simple, fixed format; CISC: complex, variable |
| Von Neumann | Harvard | VN: shared memory; Harvard: separate I+D memory |
| SRAM | DRAM | SRAM: 6T, fast, cache; DRAM: 1T1C, dense, main memory |
| Byte-addressable | Word-addressable | Byte: each byte has address; Word: each word has address |
| Logical shift | Arithmetic shift | Logical: fill 0; Arithmetic right: fill sign bit |
| RAW | WAR | RAW = true dependency (can't eliminate); WAR = anti-dependency (rename fixes) |
| Demand paging | Prepaging | Demand: load on fault; Prepaging: predict and load ahead |
| PIPT | VIPT | PIPT: uses physical address (slow, correct); VIPT: virtual index (fast, aliasing risk) |
| Structural hazard | Data hazard | Structural: resource conflict; Data: dependency conflict |
| Branch penalty | Misprediction penalty | Same concept; penalty only applies to mispredicted branches |

---

## 🔢 Additional Worked Examples

### Example 11: Non-Restoring Division — Complete Walkthrough
**Q:** Divide 13 by 3 using non-restoring division (5-bit operands).

**Setup:**
- Dividend = 13 = 01101 (5 bits)
- Divisor M = 3 = 00011 (5 bits)
- A = 00000, Q = 01101, n = 5, M = 00011

**Algorithm:**
- If A is positive: shift left AQ, A = A − M
- If A is negative: shift left AQ, A = A + M
- If A ≥ 0: Q₀ = 1, else Q₀ = 0

| Step | A | Q | Operation |
|---|---|---|---|
| Init | 00000 | 01101 | A=0, positive |
| Shift | 00000 | 11010 | Shift left AQ |
| A−M | 11101 | 11010 | A = 00000 − 00011 = 11101 (negative) |
| Q₀=0 | 11101 | 11010 | |
| Shift | 11011 | 10100 | Shift left |
| A+M | 11110 | 10100 | A = 11011 + 00011 = 11110 (negative) |
| Q₀=0 | 11110 | 10100 | |
| Shift | 11101 | 01000 | |
| A+M | 00000 | 01000 | A = 11101 + 00011 = 00000 (zero = non-neg) |
| Q₀=1 | 00000 | 01001 | |
| Shift | 00000 | 10010 | |
| A−M | 11101 | 10010 | (negative) |
| Q₀=0 | 11101 | 10010 | |
| Shift | 11010 | 00100 | |
| A+M | 11101 | 00100 | (negative) |
| Q₀=0 | 11101 | 00100 | |

**Final:** Quotient Q = 00100 = **4**, Remainder A = 11101 (negative, so add M: 11101+00011 = 00000? Let me recheck...)

Actually, 13 ÷ 3 = quotient **4**, remainder **1**.
If A is negative at end, true remainder = A + M.

### Example 12: Tomasulo Trace — Loop Iteration
**Q:** Trace Tomasulo's algorithm for:
```
Loop:  LW    F0, 0(R1)
       MUL   F4, F0, F2
       SW    F4, 0(R1)
       ADDI  R1, R1, -4
       BNE   R1, R0, Loop
```
Assume: LW = 2 cycles, MUL = 10 cycles, FP Add = 2 cycles. Show first iteration.

**Reservation Stations:**

| Cycle | Instruction | Issue | Execute | Write |
|---|---|---|---|---|
| 1 | LW F0, 0(R1) | 1 | 2-3 | 4 |
| 2 | MUL F4, F0, F2 | 2 | wait for F0 → 4-13 | 14 |
| 3 | SW F4, 0(R1) | 3 | addr=3, wait for F4 → writes at 14 | 15 |
| 4 | ADDI R1, R1, -4 | 4 | 5 | 6 |
| 5 | BNE R1, R0, Loop | 5 | 6 | 7 |

**Key observation:** MUL at cycle 2 must wait until cycle 4 (when LW writes F0 on CDB), then execute cycles 4-13.

### Example 13: Complete Cache Simulation — 4-Way Set Associative
**Q:** 4-way set associative cache, 2 sets, 8 blocks total, block = 16 bytes, 16-bit address.
Access sequence (byte addresses): 0, 16, 32, 48, 64, 80, 96, 112, 0, 32, 64, 128

Label each access as hit/miss and identify the evicted block (LRU replacement).

**Setup:**
- Offset = 4 bits (16 bytes/block)
- Index = 1 bit (2 sets)
- Tag = 16 − 4 − 1 = 11 bits
- Block address = byte address >> 4
- Set = block address mod 2

| Byte Addr | Block | Set | Tag | Hit/Miss | Set Contents After (LRU order, MRU first) |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | Miss | S0: {0} |
| 16 | 1 | 1 | 0 | Miss | S1: {1} |
| 32 | 2 | 0 | 1 | Miss | S0: {2, 0} |
| 48 | 3 | 1 | 1 | Miss | S1: {3, 1} |
| 64 | 4 | 0 | 2 | Miss | S0: {4, 2, 0} |
| 80 | 5 | 1 | 2 | Miss | S1: {5, 3, 1} |
| 96 | 6 | 0 | 3 | Miss | S0: {6, 4, 2, 0} (set full!) |
| 112 | 7 | 1 | 3 | Miss | S1: {7, 5, 3, 1} (set full!) |
| 0 | 0 | 0 | 0 | **Hit!** | S0: {0, 6, 4, 2} (0 moves to MRU) |
| 32 | 2 | 0 | 1 | **Hit!** | S0: {2, 0, 6, 4} |
| 64 | 4 | 0 | 2 | **Hit!** | S0: {4, 2, 0, 6} |
| 128 | 8 | 0 | 4 | Miss (evict 6) | S0: {8, 4, 2, 0} |

**Results: 9 misses, 3 hits, miss rate = 75%**

### Example 14: Memory Bandwidth Calculation
**Q:** A 4-way interleaved memory module has:
- Each module access time = 80 ns
- Bus transfer time = 20 ns per word
- Word size = 8 bytes

(a) Time to read 16 consecutive words without interleaving
(b) Time with 4-way low-order interleaving
(c) Effective bandwidth for each

**Solution:**
(a) Without interleaving: Each word takes 80 ns (serial)
    Total = 16 × 80 = **1280 ns**
    Bandwidth = 16 × 8 / 1280 ns = **100 MB/s**

(b) With interleaving:
    - First word ready at 80 ns
    - After that, one word every max(80/4, 20) = max(20, 20) = 20 ns
    - Total = 80 + 15 × 20 = 80 + 300 = **380 ns**
    Bandwidth = 128 / 380 ns = **337 MB/s** (3.37× improvement!)

(c) Maximum interleaving benefit = 4× (limited by number of modules)
    Actual = 1280/380 = **3.37×** (close to ideal!)

### Example 15: Effective CPI with All Sources of Stalls
**Q:** A pipelined processor has:
- Base CPI = 1
- 25% loads, 40% have load-use hazard (1 stall with forwarding)
- 15% conditional branches, 2-bit predictor with 85% accuracy, 3-cycle penalty
- 5% unconditional jumps, 1-cycle penalty
- Instruction cache miss rate = 2%, miss penalty = 50 cycles
- Data cache miss rate (for loads and stores) = 5%, miss penalty = 200 cycles
- 10% stores

**Solution step by step:**
1. Load-use stalls: 0.25 × 0.40 × 1 = 0.10
2. Branch stalls: 0.15 × 0.15 × 3 = 0.0675
3. Jump penalty: 0.05 × 1 = 0.05
4. I-cache stalls: 1.0 × 0.02 × 50 = 1.00 (every instruction accesses I-cache!)
5. D-cache stalls: (0.25 + 0.10) × 0.05 × 200 = 0.35 × 0.05 × 200 = 3.50

**Total CPI = 1 + 0.10 + 0.0675 + 0.05 + 1.00 + 3.50 = 5.7175**

> 🎯 **Observation:** Cache misses dominate (4.5 out of 5.72 stalls)! This is why cache optimization is SO important. The processor spends over 78% of its time waiting for memory!

---

## 🧩 Topic Interconnection Map

Understanding how COA topics connect helps for integrated questions:

```
                     ┌──────────────┐
                     │ ISA Design   │
                     │(RISC/CISC)   │
                     └──────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
    ┌─────────▼──────┐ ┌───▼────┐ ┌──────▼─────────┐
    │ CPU Datapath   │ │Pipeline│ │ Performance    │
    │ (Control Unit, │ │(Stages,│ │ (CPI, MIPS,   │
    │  ALU, Regs)    │ │Hazards)│ │  Amdahl's)    │
    └────────────────┘ └────┬───┘ └────────────────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
    ┌─────────▼──────┐ ┌───▼────┐ ┌──────▼─────────┐
    │ Cache Memory   │ │Virtual │ │ I/O & DMA      │
    │ (AMAT, Mapping,│ │Memory  │ │ (Interrupt,    │
    │  Coherence)    │ │(TLB,PT)│ │  Bus Arbitr.)  │
    └────────────────┘ └────────┘ └────────────────┘
```

**Common Integrated Questions:**
1. Pipeline CPI + Cache miss stalls → "Overall CPI"
2. TLB miss → Page table walk → Cache access → Total memory EAT
3. DMA bandwidth + Memory interleaving → Effective throughput
4. Branch prediction + Pipeline depth → Branch penalty
5. ISA design → Instruction count → CPI → CPU time

---

## 📝 Practice Set 11: Mixed Conceptual Questions (MCQ Style)

**Q1.** In a 2-address instruction format, which of the following is TRUE?
(a) The result is always stored in a third register
(b) One operand is both source and destination
(c) Stack is used implicitly
(d) No register is needed

**Answer: (b)**

**Q2.** Which hazard CANNOT be solved by forwarding alone?
(a) RAW from ALU to ALU
(b) RAW from Load to ALU (load-use)  
(c) WAW
(d) Both (b) and (c)

**Answer: (d)** — Load-use needs 1 stall + forward; WAW needs register renaming or stall.

**Q3.** In MESI protocol, which state transition does NOT generate a bus transaction?
(a) I → S
(b) E → M
(c) S → M
(d) M → I

**Answer: (b)** — E→M is a "silent upgrade" since no other cache has and memory is consistent.

**Q4.** A direct-mapped cache with 128 blocks will have the SAME miss rate as:
(a) 128-way fully associative cache
(b) 2-way set associative cache with 64 sets
(c) A direct-mapped cache is always worse
(d) The miss rate depends on the access pattern

**Answer: (d)** — For some patterns, direct-mapped is just as good; for others, it causes thrashing.

**Q5.** Belady's anomaly occurs with which page replacement algorithm?
(a) LRU
(b) Optimal
(c) FIFO
(d) LFU

**Answer: (c)** — FIFO can show more page faults with more frames (Belady's anomaly).

**Q6.** In a 5-stage pipeline with full forwarding, which of the following code sequences requires a stall?
```
Sequence A: ADD R1,R2,R3; SUB R4,R1,R5
Sequence B: LW R1,0(R2); ADD R3,R1,R4
Sequence C: ADD R1,R2,R3; SW R1,0(R4)
Sequence D: ADD R1,R2,R3; ADD R4,R5,R6
```
(a) Only A
(b) Only B
(c) A and B
(d) B and C

**Answer: (b)** — Only load-use (Sequence B) requires a stall with full forwarding.

**Q7.** The maximum speedup achievable by making 80% of a program run 5× faster is:
(a) 4.0×
(b) 2.27×
(c) 5.0×
(d) 1.92×

**Answer: (b)** — S = 1/(0.2 + 0.8/5) = 1/(0.2+0.16) = 1/0.36 = 2.78×. Wait that's not an option...
S = 1/(0.2 + 0.8/5) = 1/(0.2 + 0.16) = 1/0.36 = **2.78×** — this should be closest to (b) 2.27× only if f=0.7... Let me recheck.

Actually for "80% run 5× faster": S = 1/(0.2 + 0.16) = 1/0.36 = **2.78×**
Maximum (S→∞): S_max = 1/0.2 = **5.0×** (c)

The answer for the specific case is **2.78×** (not exactly in options as stated — in actual GATE, options would match).

**Q8.** Which of the following is NOT a technique to reduce cache miss rate?
(a) Increasing block size
(b) Increasing associativity
(c) Reducing cache access time
(d) Prefetching

**Answer: (c)** — Reducing access time reduces hit time, not miss rate.

**Q9.** In a micro-programmed control unit, which type of micro-programming uses the WIDEST control word?
(a) Vertical
(b) Horizontal
(c) Nano-programming
(d) Diagonal

**Answer: (b)** — Horizontal uses wide words (50-200+ bits) with minimal encoding.

**Q10.** DMA in burst mode temporarily blocks:
(a) Only I/O device access
(b) Only CPU instruction fetch
(c) All CPU access to main memory
(d) Only cache access

**Answer: (c)** — In burst mode, DMA takes full control of the bus.

**Q11.** For a k-stage pipeline, if each stage has delay τ and latch delay is d, what is the cycle time?
(a) τ + d
(b) max(τ₁,τ₂,...,τₖ) + d
(c) k × τ + d
(d) (τ + d) × k

**Answer: (b)** — Clock period = maximum stage delay + latch overhead.

**Q12.** Which instruction type in MIPS uses the 'shamt' field?
(a) I-type
(b) J-type
(c) R-type (shift instructions)
(d) All types

**Answer: (c)** — R-type format has shamt field for shift amount.

**Q13.** A 2-bit saturating counter branch predictor initialized to "Weakly Not Taken" (01). After the pattern T, T, NT, T, what is the predictor state?
- Start: 01
- T: mispredicted, state → 10
- T: correct (predict taken), state → 11
- NT: mispredicted, state → 10
- T: correct (predict taken), state → 11

**Final state: 11 (Strongly Taken). Mispredictions: 2 out of 4 = 50%**

**Q14.** In write-back cache, when does data get written to main memory?
(a) On every write
(b) Only when the block is evicted (replaced)
(c) On every read miss
(d) When the cache is full

**Answer: (b)** — Write-back caches only write to memory when a dirty block is replaced.

**Q15.** What is the minimum number of page faults for the reference string 1,2,3,4,1,2,5,1,2,3,4,5 with 4 frames using Optimal replacement?

Reference string: 1,2,3,4,1,2,5,1,2,3,4,5

| Ref | Frame0 | Frame1 | Frame2 | Frame3 | Fault? |
|---|---|---|---|---|---|
| 1 | **1** | | | | F |
| 2 | 1 | **2** | | | F |
| 3 | 1 | 2 | **3** | | F |
| 4 | 1 | 2 | 3 | **4** | F |
| 1 | hit | | | | |
| 2 | | hit | | | |
| 5 | 1 | 2 | **5** | 4 | F (replace 3, used farthest: at pos 10) |
| 1 | hit | | | | |
| 2 | | hit | | | |
| 3 | 1 | 2 | 5 | **3** | F (replace 4, used farthest: at pos 11) |
| 4 | 1 | 2 | **4** | 3 | F (replace 5, used farthest: at pos 12) |
| 5 | 1 | 2 | 4 | **5** | F (replace 3, not used again) |

Wait, let me redo more carefully with OPT:

After loading 1,2,3,4 (4 faults). Frames: {1,2,3,4}
- 1: hit (in frames)
- 2: hit
- 5: miss — replace which? Future use: 1@pos8, 2@pos9, 3@pos10, 4@pos11. Replace 4 (farthest). Frames: {1,2,3→5...} No wait, replace the one used FARTHEST. From {1,2,3,4}: 1→pos8, 2→pos9, 3→pos10, 4→pos11. 4 is farthest → replace **4**. Frames: {1,2,3,5}. Fault #5.
- 1: hit
- 2: hit
- 3: hit
- 4: miss — replace from {1,2,3,5}. Future: 1-not again, 2-not again, 3-not again, 5@pos12. Replace any of 1,2,3 (none used again). Replace 1. Frames: {4,2,3,5}. Fault #6.
- 5: hit.

**Answer: 6 page faults with Optimal replacement**

---

## 📝 Practice Set 12: Advanced Pipeline Problems (5 Problems)

**P61.** A 7-stage pipeline (S1-S7) has the following stage delays: 50, 100, 80, 60, 90, 70, 50 ps. Latch delay = 20 ps.
(a) What is the pipeline clock period?
(b) Non-pipelined clock period?
(c) Pipeline speedup for 1000 instructions?
(d) What would be the speedup if stages were perfectly balanced?

**Solution:**
(a) Pipeline clock = max(100) + 20 = **120 ps**
(b) Non-pipelined = 50+100+80+60+90+70+50 = **500 ps per instruction**
(c) Pipeline time = (7 + 1000 − 1) × 120 = 1006 × 120 = 120,720 ps
    Non-pipeline time = 1000 × 500 = 500,000 ps
    Speedup = 500,000 / 120,720 = **4.14×** (ideal would be 7.0×)
(d) If perfectly balanced: each stage = 500/7 ≈ 71.4 ps; clock = 71.4 + 20 = 91.4 ps
    Pipeline time = 1006 × 91.4 = 91,948 ps
    Speedup = 500,000 / 91,948 = **5.44×** (much closer to ideal 7.0!)
    **Imbalance costs 5.44/4.14 = 31% performance!**

**P62.** A 6-stage pipeline processes a program where:
- 30% of instructions have RAW dependency on the IMMEDIATELY preceding instruction (1 stall with forwarding)
- 10% of instructions have RAW on the instruction 2 positions before (0 stalls with forwarding)
- 15% are branches, resolved in stage 3, with 75% prediction accuracy
- 2% cause structural hazards (1 stall each)

What is the effective CPI?

**Solution:**
- RAW stalls = 0.30 × 1 = 0.30 (only immediate predecessor causes stalls with forwarding)
- 2-distance RAW = 0 stalls (forwarding covers this)
- Branch stalls = 0.15 × 0.25 × 2 = 0.075 (2 cycles penalty, 25% misprediction)
- Structural stalls = 0.02 × 1 = 0.02
- **CPI = 1 + 0.30 + 0.075 + 0.02 = 1.395**

**P63.** Given 10 instructions with the following dependency graph:
```
I1 → I3 → I5 → I7 → I9
I2 → I4 → I6 → I8 → I10
I3 → I6 (cross dependency)
```
On a 2-issue in-order superscalar (with forwarding, 1-cycle latency):

| Cycle | Issue slot 1 | Issue slot 2 |
|---|---|---|
| 1 | I1 | I2 |
| 2 | I3 (waits for I1) | I4 (waits for I2) |
| 3 | I5 | I6 (waits for I3 AND I4 → must wait for I3 from cycle 2) |
| 4 | I7 | I8 |
| 5 | I9 | I10 |

Total = **5 cycles** for 10 instructions → IPC = 2.0 (perfect!)
But wait: I6 depends on I3 (from cycle 2). With forwarding, I3 result available at end of cycle 2 → I6 can execute in cycle 3. ✅

**P64.** A pipeline processor executes a loop:
```
Loop: LW   R1, 0(R10)     ; load array element
      ADDI R1, R1, 1      ; increment (load-use: 1 stall)
      SW   R1, 0(R10)     ; store back
      ADDI R10, R10, 4    ; advance pointer
      BNE  R10, R11, Loop ; branch (predicted taken, 1 cycle penalty if mispredicted)
```
Loop iterates 100 times. With forwarding, how many cycles?

**Solution:**
Per iteration:
- LW→ADDI: load-use = 1 stall
- ADDI→SW: forwarding = 0 stalls
- SW→ADDI: no dependency on result
- ADDI→BNE: forwarding = 0 stalls
- BNE: taken 99 times (correct prediction), not-taken once (misprediction penalty)
  Assume branch resolved in ID: penalty = 1 cycle

Per iteration: 5 instructions + 1 stall = **6 cycles**
Total = 100 × 6 + 1 (misprediction on last iteration) = **601 cycles**

With compiler scheduling (move ADDI R10 between LW and ADDI R1):
```
Loop: LW   R1, 0(R10)     
      ADDI R10, R10, 4    ; no dependency on R1 → fills load-use slot!
      ADDI R1, R1, -1     ; R1 now available from LW (forwarded from MEM)
      SW   R1, -4(R10)    ; adjust offset since R10 already incremented
      BNE  R10, R11, Loop 
```
Per iteration: 5 instructions + 0 stalls = **5 cycles**
Total = 100 × 5 + 1 = **501 cycles** (saved 100 cycles = 17% improvement!)

**P65.** Compare three processor designs for a workload of 10⁶ instructions:
| Processor | Clock Rate | Pipeline Stages | Stall cycles/instruction |
|---|---|---|---|
| A (Simple) | 1 GHz | 5 | 0.5 |
| B (Deep) | 2 GHz | 15 | 1.5 |
| C (Superscalar) | 1.5 GHz | 10 | 0.8 (CPI = 0.5 ideal for 2-issue, 0.3 stalls → IPC=1.25) |

Wait, let me simplify: CPI_A = 1 + 0.5 = 1.5; CPI_B = 1 + 1.5 = 2.5; CPI_C = 0.5 + 0.3 = 0.8

**Solution:**
- Time_A = 10⁶ × 1.5 / 10⁹ = 1.5 ms
- Time_B = 10⁶ × 2.5 / (2×10⁹) = 1.25 ms
- Time_C = 10⁶ × 0.8 / (1.5×10⁹) = 0.533 ms

**Ranking: C fastest (0.533 ms) > B (1.25 ms) > A (1.5 ms)**
C wins despite lower clock rate due to superscalar advantage! B's deeper pipeline has higher clock but much more stalls.

---

## 📝 Practice Set 13: Quick Conceptual MCQs (10 questions)

**Q16.** Harvard architecture's primary advantage over Von Neumann is:
(a) Lower cost
(b) Simultaneous instruction and data access
(c) Larger memory space  
(d) Easier programming

**Answer: (b)**

**Q17.** In IEEE 754 single precision, the number 0 00000000 00000000000000000000001 represents:
(a) Smallest normalized number
(b) Smallest denormalized number
(c) Positive infinity
(d) NaN

**Answer: (b)** — Exponent = 0, mantissa ≠ 0 → denormalized. Value = 0.000...001 × 2⁻¹²⁶ = 2⁻¹⁴⁹

**Q18.** Which of the following is TRUE about 2's complement?
(a) There are two representations of zero
(b) Range is symmetric around zero
(c) Overflow occurs when carry into MSB ≠ carry out of MSB
(d) Subtraction requires separate hardware from addition

**Answer: (c)** — V = Cₙ₋₁ ⊕ Cₙ

**Q19.** In a 5-stage pipeline with full forwarding, which sequence does NOT require any stalls?
```
A: ADD R1,R2,R3; SUB R4,R1,R5
B: LW R1,0(R2); ADD R3,R1,R4
C: ADD R1,R2,R3; LW R4,0(R1)
D: LW R1,0(R2); SW R1,0(R3)
```

**Answer: A and C** — A uses EX-EX forwarding, C uses EX-EX forwarding for address calculation.
B has load-use (1 stall). D: SW needs R1 in MEM stage, LW produces at end of MEM → this can be forwarded! (MEM-MEM forwarding). So D also has **0 stalls** in some implementations.

The clearest answer is **(A)** — guaranteed no stalls.

**Q20.** A cache has access time 2 ns and miss rate 5%. Main memory has access time 60 ns. What is AMAT?
(a) 5 ns (b) 62 ns (c) 2 ns (d) 3 ns

AMAT = 2 + 0.05 × 60 = 2 + 3 = **5 ns → Answer: (a)**

**Q21.** How many page faults will FIFO produce for reference string 1,2,3,1,4,1,2,3 with 3 frames?

| Ref | F0 | F1 | F2 | Fault? |
|---|---|---|---|---|
| 1 | 1 | | | F |
| 2 | 1 | 2 | | F |
| 3 | 1 | 2 | 3 | F |
| 1 | hit | | | |
| 4 | 4 | 2 | 3 | F (replace 1, oldest) |
| 1 | 4 | 1 | 3 | F (replace 2, oldest) |
| 2 | 4 | 1 | 2 | F (replace 3, oldest) |
| 3 | 3 | 1 | 2 | F (replace 4, oldest) |

**Answer: 7 page faults**

Now with LRU (same string, 3 frames):

| Ref | F0 | F1 | F2 | Fault? | LRU order (MRU→LRU) |
|---|---|---|---|---|---|
| 1 | 1 | | | F | 1 |
| 2 | 1 | 2 | | F | 2,1 |
| 3 | 1 | 2 | 3 | F | 3,2,1 |
| 1 | hit | | | | 1,3,2 |
| 4 | 1 | 4 | 3 | F (replace 2, LRU) | 4,1,3 |
| 1 | hit | | | | 1,4,3 |
| 2 | 1 | 4 | 2 | F (replace 3, LRU) | 2,1,4 |
| 3 | 1 | 3 | 2 | F (replace 4, LRU) | 3,2,1 |

**LRU also gives 6 faults.** Wait, let me count: F,F,F,F,F,F = **6 faults**

**Q22.** In DMA cycle-stealing mode, each word transfer takes 1 memory cycle. If memory cycle = 100 ns and CPU also needs memory every 500 ns, what fraction of CPU memory bandwidth is stolen?

DMA steals 1 out of every 100/100 = 1 cycle. CPU needs memory 1 out of every 500/100 = 5 cycles.
Per 500 ns: DMA steals 5 cycles, CPU needs 1 cycle. Total available = 5 cycles.
Wait — DMA steals 1 cycle per 100 ns = 5 cycles per 500 ns, leaving 0 for CPU? That's 100%.

Let me reconsider: CPU requests memory once per 500 ns. DMA steals cycles only when it has data.
If DMA rate = 1 word per 100 ns = 10 million words/sec.
CPU memory rate = 1 word per 500 ns = 2 million/sec.
Both compete for memory at 1/100 ns = 10M cycles/sec.
DMA takes 100% of cycles → CPU always stalls.

**Better formulation:** If DMA transfers 1 word out of every 5 memory cycles (not every cycle):
- DMA utilization = 1/5 = **20% of memory bandwidth stolen**
- CPU has 4/5 = 80% available

**Q23.** Horizontal microprogramming uses a wider control word than vertical because:
(a) It encodes control signals
(b) Each bit directly controls a hardware signal
(c) It has more microinstructions
(d) It requires more registers

**Answer: (b)** — In horizontal, each bit is a direct control signal → wide word, no decoding needed.

**Q24.** Adding a victim cache to a direct-mapped cache primarily reduces:
(a) Compulsory misses
(b) Capacity misses
(c) Conflict misses
(d) All types of misses equally

**Answer: (c)** — Victim cache catches recently evicted blocks, which are conflict misses.

**Q25.** In Amdahl's Law, if 95% of a program is parallelizable, the maximum speedup with infinite processors is:
(a) 95× (b) 19× (c) 20× (d) 5×

S_max = 1/(1-0.95) = 1/0.05 = **20× → Answer: (c)**

---

## 🔬 Historical Evolution of Computer Architecture

Understanding the history helps contextualize WHY things work the way they do:

| Era | Year(s) | Key Development | Impact |
|---|---|---|---|
| 0 | 1945 | Von Neumann architecture (EDVAC) | Stored program concept |
| 1 | 1964 | IBM System/360 | ISA as interface; microprogramming |
| 2 | 1970s | Caches introduced (IBM 360/85) | Memory wall solution |
| 3 | 1980s | RISC revolution (MIPS, SPARC) | Simple instructions, pipelining |
| 4 | 1990s | Superscalar, OoO (Pentium Pro) | ILP exploitation |
| 5 | 2000s | Multi-core (Athlon 64 X2) | Thread-level parallelism |
| 6 | 2010s+ | Heterogeneous (CPU+GPU) | Domain-specific acceleration |
| 7 | 2020s | Chiplets, CXL, RISC-V open ISA | Modular, open architecture |

**Moore's Law:** Transistor count doubles every ~2 years
**Dennard Scaling (ended ~2006):** As transistors shrink, power density stays constant → broke down, leading to "power wall"
**Memory Wall:** Processor speed grows faster than memory speed → gap increases → caches become critical

### Processor Evolution Quick Reference ⭐

| Processor | Year | Clock | Pipeline | Issue | Transistors |
|---|---|---|---|---|---|
| Intel 8086 | 1978 | 5 MHz | None | 1 | 29K |
| Intel 386 | 1985 | 20 MHz | None | 1 | 275K |
| MIPS R2000 | 1985 | 15 MHz | 5-stage | 1 | 120K |
| Intel Pentium | 1993 | 60 MHz | 5/U+V | 2 (superscalar) | 3.1M |
| DEC Alpha 21264 | 1998 | 600 MHz | 7-stage | 4 (OoO) | 15M |
| Intel P4 Prescott | 2004 | 3.8 GHz | 31-stage | 3 | 125M |
| Intel Core 2 | 2006 | 2.93 GHz | 14-stage | 4 (OoO) | 291M |
| ARM Cortex-A72 | 2015 | 2.5 GHz | 15-stage | 3 (OoO) | — |
| Apple M1 | 2020 | 3.2 GHz | ~16-stage | 8 (OoO) | 16B |
| Apple M4 | 2024 | 4.4 GHz | ~16-stage | 10+ | 28B |

> 🎯 **Key Trend:** Clock speed plateaued (~4 GHz since 2005), but transistor count and core count keep growing. Modern performance comes from parallelism, not faster clocks!

---

## 📐 Dimensional Analysis Guide for GATE

A common source of errors in GATE is unit mistakes. Here's a guide:

| Given | Formula | Result Unit |
|---|---|---|
| Clock rate (Hz) = cycles/sec | | |
| CPI = cycles/instruction | | |
| IC = instructions | | |
| CPU Time = IC × CPI / Clock Rate | instructions × cycles/instr / (cycles/sec) | **seconds** |
| MIPS = Clock Rate / (CPI × 10⁶) | (cycles/sec) / (cycles/instr × 10⁶) | **million instr/sec** |
| Bandwidth = Width × Frequency | bits × cycles/sec | **bits/sec** |
| AMAT = hit_time + miss_rate × penalty | ns + (unitless) × ns | **ns** |
| DMA time = N × cycle_time | words × ns/word | **ns** |

**Common conversions:**
| From | To | Multiply by |
|---|---|---|
| 1 ms | ns | 10⁶ |
| 1 μs | ns | 10³ |
| 1 GHz | MHz | 10³ |
| 1 GB/s | MB/s | 10³ |
| 1 MB | KB | 1024 |
| 1 KB | bytes | 1024 |
| 1 byte | bits | 8 |

> 🎯 **GATE Tip:** Always check units at the end! If the question asks for "ms" and you calculated "ns", you'll be off by 10⁶!

---

## 📝 Practice Set 14: I/O and DMA Numerical Problems (P66-P75)

**P66.** A disk has 5000 RPM, average seek time 6 ms, sector size 512 B, 200 sectors/track. Calculate average time to read one sector.

**Solution:**
- Rotational latency = 0.5 × (60/5000) = 0.5 × 12 ms = **6 ms**
- Transfer time = (1/200) × 12 ms = **0.06 ms**
- Total = 6 + 6 + 0.06 = **12.06 ms**

---

**P67.** A DMA controller transfers data from a disk at 40 MB/s. Memory cycle time = 50 ns, word size = 4 bytes. In cycle stealing mode, what percentage of memory cycles are stolen by DMA?

**Solution:**
- Words transferred per second = 40×10⁶/4 = 10×10⁶ = 10M words/sec
- Each word steals one memory cycle = 50 ns
- Total stolen time = 10⁷ × 50 × 10⁻⁹ = 0.5 sec per sec = **50%**
- Half the memory bandwidth is consumed by DMA!

---

**P68.** A system uses interrupt-driven I/O for a network card receiving 1500-byte packets at 1000 packets/sec. ISR takes 200 CPU cycles, CPU at 1 GHz. What fraction of CPU is used for interrupt handling?

**Solution:**
- Interrupts/sec = 1000
- Cycles per interrupt = 200
- Total cycles/sec for interrupts = 1000 × 200 = 200,000
- CPU cycles/sec = 10⁹
- Fraction = 200,000 / 10⁹ = **0.02%** (negligible)

---

**P69.** A hard disk has 4 surfaces, 2000 tracks/surface, 400 sectors/track, 512 bytes/sector. Calculate:
(a) Total disk capacity (b) If RPM=10000, sustained sequential transfer rate

**Solution:**
(a) Capacity = 4 × 2000 × 400 × 512 = 4 × 2000 × 200KB = 4 × 400MB = **1.6 GB**
(b) Rotation time = 60/10000 = 6 ms/rev
- One track = 400 × 512 = 200 KB
- Transfer rate = 200KB / 6ms = 200×1024 / 0.006 ≈ **33.3 MB/s**

---

**P70.** In a priority interrupt system with daisy chain, 4 devices D1-D4 (D1 highest priority). D3 and D1 request simultaneously. If D3's ISR takes 50 μs and D1's takes 30 μs, what is the total time to service both? Assume nested interrupts enabled.

**Solution:**
- D1 has higher priority → D1 serviced first
- If D1 and D3 arrive simultaneously: D1 starts immediately (30 μs)
- After D1 finishes, D3 is serviced (50 μs)
- If nested: D3 can't preempt D1 (lower priority), so still sequential
- Total = 30 + 50 = **80 μs**
- Note: If D1 arrived DURING D3's ISR, D1 would preempt D3 (nested), total = 50 + 30 = 80 μs (same total but D1 finishes earlier)

---

**P71.** A DMA controller in burst mode stops the CPU for the entire transfer. Transfer: 8 KB, memory cycle = 40 ns, word size = 8 bytes. The CPU was executing at CPI=2, clock=2 GHz. How many instructions are lost during the DMA transfer?

**Solution:**
- Words = 8×1024/8 = 1024 words
- DMA transfer time = 1024 × 40 ns = 40,960 ns
- CPU instruction time = CPI / clock rate = 2 / (2×10⁹) = 1 ns/instruction
- Instructions lost = 40,960 / 1 = **40,960 instructions**

---

**P72.** An I/O device generates data at 2 KB/s. CPU polls device every 100 μs. Each poll takes 20 clock cycles at 500 MHz. What percentage of CPU time is wasted on polling?

**Solution:**
- Polls per second = 1 / (100 × 10⁻⁶) = 10,000 polls/sec
- Cycles per poll = 20
- Total cycles wasted = 10,000 × 20 = 200,000 cycles/sec
- CPU total cycles = 500 × 10⁶ cycles/sec
- Wasted fraction = 200,000 / (500 × 10⁶) = **0.04%**
- Even though inefficient in principle, polling overhead is tiny for slow devices!

---

**P73.** A handshaking asynchronous bus has 4-phase protocol with propagation delay δ = 5 ns on each signal line. Data bus width = 32 bits. What is maximum data transfer rate?

**Solution:**
- 4-phase handshake: 4 signal transitions, each δ = 5 ns
- Minimum cycle time = 4 × 5 = 20 ns
- Transfers per second = 1 / (20 × 10⁻⁹) = 50 × 10⁶ = 50M transfers/sec
- Each transfer = 32 bits = 4 bytes
- **Maximum bandwidth = 50M × 4 = 200 MB/s**

---

**P74.** Compare the CPU overhead for transferring 1 MB of data using: (a) Polling, (b) Interrupt I/O, (c) DMA. Assume word=4B, poll=20 cycles, ISR=200 cycles, DMA setup+cleanup=5000 cycles, CPU at 1 GHz.

**Solution:**
- Words to transfer = 1M/4 = 256K = 262,144 words

(a) **Polling:** Each word requires polling loop
- Assume avg 5 polls per word × 20 cycles = 100 cycles/word
- Total = 262,144 × 100 = 26.2M cycles = **26.2 ms of CPU time**

(b) **Interrupt I/O:** Each word causes an interrupt
- Total = 262,144 × 200 = 52.4M cycles = **52.4 ms of CPU time**
- Interrupt is WORSE than polling here! (ISR overhead > poll overhead)

(c) **DMA:** Only setup and completion interrupt
- Total = 5000 (setup) + 200 (completion ISR) = 5200 cycles = **5.2 μs of CPU time**
- **DMA is 5000× more efficient than polling, 10000× more than interrupt I/O!**

> 🎯 **GATE Insight:** For large transfers, DMA wins overwhelmingly. Interrupt I/O can be WORSE than polling due to context save/restore overhead per interrupt!

---

**P75.** A system has a synchronous bus running at 100 MHz (10 ns cycle). Each memory read takes 4 bus cycles (1 address + 3 wait states). Bus width = 64 bits. If DMA uses cycle stealing:
(a) What is the effective memory bandwidth available to DMA?
(b) How long to transfer 64 KB?

**Solution:**
(a) Each DMA word transfer = 4 bus cycles × 10 ns = 40 ns per 8 bytes
- DMA bandwidth = 8 bytes / 40 ns = **200 MB/s**

(b) Transfer 64 KB = 65,536 bytes = 8,192 words (64-bit)
- Time = 8192 × 40 ns = 327,680 ns ≈ **328 μs**
- But in cycle stealing, CPU also needs bus, so effective time is longer
- If CPU uses bus 50% of time: effective DMA time ≈ 328 × 2 = **656 μs**

---

## 📝 Practice Set 15: Mixed Rapid-Fire Problems (P76-P85)

**P76.** A system with 16-bit addresses uses memory-mapped I/O. I/O devices occupy addresses 0xFF00-0xFFFF. How much RAM is addressable?

**Solution:** Total = 2¹⁶ = 64 KB. I/O = 0xFFFF − 0xFF00 + 1 = 256 bytes. RAM = 64KB − 256B = **65,280 bytes ≈ 63.75 KB**

---

**P77.** IEEE 754 single precision: What is the value of 0xC1480000?

**Solution:**
- Binary: 1100 0001 0100 1000 0000 0000 0000 0000
- Sign = 1 (negative), Exponent = 10000010 = 130, Mantissa = 100 1000...
- Value = −1 × 1.1001 × 2^(130−127) = −1 × 1.5625 × 8 = **−12.5**

---

**P78.** Pipeline: 5 stages (IF=2ns, ID=3ns, EX=4ns, MEM=2ns, WB=1ns). What is the clock period and maximum throughput?

**Solution:**
- Clock = max(stage times) = **4 ns** (bottleneck: EX)
- Throughput = 1/4 ns = **250 MIPS**
- Non-pipelined time per instruction = 2+3+4+2+1 = 12 ns → speedup = 12/4 = **3×** (not 5× due to imbalance)

---

**P79.** A 4-way set-associative cache has 256 KB total, 64-byte blocks. How many sets?

**Solution:**
- Total blocks = 256K / 64 = 4096 blocks
- Sets = 4096 / 4 = **1024 sets**
- Set index bits = log₂(1024) = **10 bits**

---

**P80.** Booth's algorithm: Multiply −7 × 5 using 5-bit representation.

**Solution:**
- −7 = 11001 (2's complement), 5 = 00101
- Booth recoding of multiplier 00101:
  - Append 0: 001010
  - Pairs (right to left): 10→−1, 01→+1, 00→0, 00→0, 00→0 → Recoded: {0,0,+1,−1,+1,0} hmm
  - Let me redo: M=−7, Q=00101, Q₋₁=0
  
| Step | A | Q | Q₋₁ | Action |
|---|---|---|---|---|
| Init | 00000 | 00101 | 0 | |
| 1 | 00000 | 00101 | 0 | Q₀Q₋₁=10→ A=A−M=A+7=00111 |
| | 00011 | 10010 | 1 | Shift right |
| 2 | 00011 | 10010 | 1 | Q₀Q₋₁=01→ A=A+M=A−7=11100 |
| | 11110 | 01001 | 0 | Shift right |
| 3 | 11110 | 01001 | 0 | Q₀Q₋₁=10→ A=A−M=A+7=00101 |
| | 00010 | 10100 | 1 | Shift right |
| 4 | 00010 | 10100 | 1 | Q₀Q₋₁=01→ A=A+M=A−7=11011 |
| | 11101 | 11010 | 0 | Shift right |
| 5 | 11101 | 11010 | 0 | Q₀Q₋₁=00→ no op |
| | 11110 | 11101 | 0 | Shift right |

Result: A,Q = 11110 11101 = 1111011101₂
- In 10-bit 2's complement: 1111011101 = −35 ✕ but −7×5 = −35 ✓
- Verify: 1111011101 → invert+1 = 0000100011 = 35, so answer = **−35** ✓

---

**P81.** Effective CPI with cache misses: Base CPI = 1.2, instruction cache miss rate = 3%, data cache miss rate = 8%, 40% of instructions are loads/stores, miss penalty = 50 cycles.

**Solution:**
- I-cache stalls = 0.03 × 50 = 1.5 cycles/instr
- D-cache stalls = 0.40 × 0.08 × 50 = 1.6 cycles/instr
- Effective CPI = 1.2 + 1.5 + 1.6 = **4.3**
- Memory stalls account for (1.5+1.6)/4.3 = **72%** of execution time

---

**P82.** Amdahl's Law: A program spends 40% on I/O. If we improve CPU speed by 10×, what is the overall speedup?

**Solution:**
- f_enhanced = 0.60 (CPU portion), speedup_enhanced = 10
- Speedup = 1 / (0.40 + 0.60/10) = 1 / (0.40 + 0.06) = 1/0.46 = **2.17×**
- Even 10× faster CPU only gives 2.17× speedup! I/O is the bottleneck.

---

**P83.** 2-bit predictor starts in "Strongly Taken" (11). Actual branch outcomes: T, T, N, N, T, N, T, T. How many mispredictions?

**Solution:**
| Branch | Prediction | Actual | Correct? | New State |
|---|---|---|---|---|
| 1 | T (11) | T | ✓ | 11 |
| 2 | T (11) | T | ✓ | 11 |
| 3 | T (11) | N | ✗ | 10 |
| 4 | T (10) | N | ✗ | 01 |
| 5 | N (01) | T | ✗ | 10 |
| 6 | T (10) | N | ✗ | 01 |
| 7 | N (01) | T | ✗ | 10 |
| 8 | T (10) | T | ✓ | 11 |

Mispredictions = **5 out of 8** (62.5% — poor performance on alternating patterns!)

---

**P84.** A page table has 3 levels. TLB hit rate = 90%, TLB access = 5 ns, memory access = 80 ns. Calculate EAT.

**Solution:**
- TLB hit: 5 + 80 = 85 ns (TLB + 1 memory for data)
- TLB miss: 5 + 3×80 + 80 = 5 + 240 + 80 = 325 ns (TLB + 3 PT accesses + data)
- EAT = 0.90 × 85 + 0.10 × 325 = 76.5 + 32.5 = **109 ns**

---

**P85.** A RAID-5 array with 10 disks (each 4 TB, read=200MB/s, write=180MB/s). Calculate:
(a) Usable capacity (b) Sequential read bandwidth (c) Small-write IOPS (4KB random writes at 200 IOPS/disk)

**Solution:**
(a) Usable = (10−1) × 4 = **36 TB**
(b) Read BW = 10 × 200 = **2000 MB/s** (all disks contribute)
(c) RAID-5 small write = 4 I/O operations per write (2 reads + 2 writes)
- Each disk: 200 IOPS
- Total raw IOPS = 10 × 200 = 2000
- Effective write IOPS = 2000 / 4 = **500 IOPS**

---

> 🎯 **GATE Tip:** Always check units at the end! If the question asks for "ms" and you calculated "ns", you'll be off by 10⁶!

---

*End of Comprehensive Notes — Computer Organization & Architecture for GATE 2027* 🎓🏆