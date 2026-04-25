# Computer Organization & Architecture — Solutions for GATE 2027

> Complete solutions for all 75 questions.

---

## Section A: Basic Components & Memory Organization

### Q1. Memory capacity with 24 address lines
2²⁴ addresses × 1 byte = 16,777,216 bytes = **16 MB**

---

### Q2. 0xABCD1234 at address 2000
| Address | Big-Endian | Little-Endian |
|---------|-----------|---------------|
| 2000 | AB | 34 |
| 2001 | CD | 12 |
| 2002 | 12 | CD |
| 2003 | 34 | AB |

---

### Q3. Data bus width for 64-bit word
**64 bits** (data bus width = word size)

---

### Q4. Chips for 1 MB from 256K × 4-bit
Memory needed: 1 MB = 1M × 8 bits
Each chip: 256K × 4 bits

Along depth: 1M/256K = 4 chips (address expansion)
Along width: 8/4 = 2 chips (data expansion)
Total: 4 × 2 = **8 chips**

---

### Q5. 16-bit address, 2K × 8 RAM chips
Total address space: 2¹⁶ = 64 KB
Each chip: 2K = 2¹¹ bytes
Number of chips: 64K/2K = **32 chips**

Chip select bits: log₂(32) = **5 bits** (upper 5 of 16 address bits)
Each chip uses 11 address bits internally.

---

### Q6. Memory-mapped vs Isolated I/O
**Memory-mapped:** I/O devices assigned addresses in same space as memory. CPU uses LOAD/STORE for I/O. Example: ARM, MIPS.

**Isolated I/O:** Separate I/O address space with dedicated IN/OUT instructions. Example: x86 with ports.

---

### Q7. Page table entries
Virtual address = 32 bits, page size = 4 KB = 2¹² bytes
Number of pages = 2³²/2¹² = 2²⁰ = **1,048,576 entries**

(Physical address bits are irrelevant for page table size; entries needed = virtual pages.)

---

### Q8. 4-way interleaved memory
Cycle time = 80 ns, bus transfer = 20 ns
Without interleaving: access time = 80 ns per word
With 4-way interleaving: after initial 80 ns, new word available every 80/4 = 20 ns.
But transfer takes 20 ns, and memory delivers every 20 ns.

Effective access time = (80 + 3×20)/4 = 140/4 = 35 ns per word
Or simply: first word at 80 ns, subsequent at max(80/4, 20) = 20 ns each.

For 4 sequential words: 80 + 3×20 = 140 ns, average = **35 ns per word**

---

### Q9. Fraction of address space
Address space = 2³² = 4 GB
Main memory = 512 MB
Fraction = 512 MB / 4 GB = 512/4096 = **1/8 = 12.5%**

---

### Q10. 3rd byte of word at 0x0008
Word at 0x0008 occupies bytes: 0x0008, 0x0009, 0x000A, 0x000B
3rd byte (0-indexed: byte 2) = address **0x000A**

In little-endian, the 3rd byte (index 2) contains the 3rd most significant byte of the word.

---

## Section B: Data Representation & Floating Point

### Q11. 19.6875 in IEEE 754
19 = 10011₂, 0.6875 = 0.1011₂
19.6875 = 10011.1011₂ = 1.00111011 × 2⁴

Sign = 0, Exponent = 4+127 = 131 = 10000011
Mantissa = 00111011000...0

**Answer: 0 10000011 00111011000000000000000**
Hex: 0x419D8000

---

### Q12. 0 10000100 01100000...0
Sign = 0 (+), E = 132, e = 132−127 = 5
Value = 1.011 × 2⁵ = 10110.0 = 22.0

**Answer: 22.0**

---

### Q13. Range of normalized positive (single precision)
Smallest: 1.0 × 2^(1−127) = 2⁻¹²⁶ ≈ **1.175 × 10⁻³⁸**
Largest: (2−2⁻²³) × 2¹²⁷ ≈ **3.403 × 10³⁸**

---

### Q14. Can 0.1 be exact in IEEE 754?
**No.** 0.1₁₀ = 0.0001100110011...₂ (repeating). Cannot be represented exactly in finite binary, so there's a rounding error.

---

### Q15. Denormalized vs Normalized
**Normalized:** Exponent ≠ 0 and ≠ all 1s. Implicit leading 1: value = 1.M × 2^(E−bias).
**Denormalized:** Exponent = 0, mantissa ≠ 0. No implicit 1: value = 0.M × 2^(1−bias). Fills gap between 0 and smallest normalized number.

---

### Q16. Precision in 2¹⁰ + 2⁻¹⁰
To add: align exponents → 1.0 × 2¹⁰ + 0.00000000000000000001 × 2¹⁰
The smaller number's mantissa shifts right by 20 positions — beyond 23-bit mantissa precision.
Result ≈ 1.0 × 2¹⁰ = 1024.0 (the small value is lost).

**Answer: Catastrophic cancellation / absorption — the smaller number is effectively ignored.**

---

### Q17. 0xC1480000 in decimal
C1480000 = 1100 0001 0100 1000 0...0
Sign = 1 (−), E = 10000010 = 130, e = 3
M = 10010000...0
Value = −1.1001 × 2³ = −1100.1 = **−12.5**

---

### Q18. −0 in IEEE 754 single precision
Sign = 1, Exponent = 00000000, Mantissa = all 0
**Hex: 0x80000000**

---

### Q19. Exponent bits for IEEE 754 single
**8 bits** for exponent (always, regardless of value stored).

---

### Q20. Smallest number > 1.0
1.0 = 1.000...0 × 2⁰. Next = 1.000...01 × 2⁰ = 1 + 2⁻²³
Gap = **2⁻²³ ≈ 1.19 × 10⁻⁷** (this is called machine epsilon for single precision)

---

## Section C: Instruction Set Architecture

### Q21. RISC advantages
- Simpler instructions → easier pipelining → higher throughput
- Fixed-length instructions → faster decode
- More registers → fewer memory accesses
- Simpler design → shorter design cycle
- Load/store architecture → clean separation

---

### Q22. Operand fields in ADD R1, R2, R3
**3 operand fields** (destination + 2 sources)

---

### Q23. Immediate addressing
Operand is **embedded in the instruction itself** (part of the instruction encoding).

---

### Q24. Expanding opcode
16-bit instruction: 4-bit opcode + three 4-bit register fields.
Total 4-bit opcodes = 16.
Three-address uses 12 opcodes → 4 remaining prefix patterns.

Two-address: 4 remaining × 16 (second register encodes opcode extension) = **64 two-address instructions**

---

### Q25. Average CPI
CPI = 0.43×4 + 0.21×5 + 0.12×4 + 0.24×3
= 1.72 + 1.05 + 0.48 + 0.72 = **3.97**

---

### Q26. Memory accesses for operand
(a) Immediate: **0** (operand in instruction)
(b) Direct: **1** (one memory access to address)
(c) Indirect: **2** (read pointer, then read data)
(d) Register Indirect: **1** (register provides address, one memory access)

---

### Q27. Stack: X = (A+B) × (C−D)
```
PUSH A
PUSH B
ADD        ; TOS = A+B
PUSH C
PUSH D
SUB        ; TOS = C-D
MUL        ; TOS = (A+B)×(C-D)
POP X
```

---

### Q28. CPI = 0.8 × 2.5 GHz / 500M
CPI = (Execution time × Clock rate) / IC = (0.8 × 2.5 × 10⁹) / (500 × 10⁶) = **4.0**

---

### Q29. 32-bit instruction bit allocation
6 bits opcode + 3 bits base register (8 regs = 2³) + 3 bits dest register + 16 bits offset (±32K = ±2¹⁵, need 16 bits for signed)
Total: 6+3+3+16 = 28. Remaining 4 bits for other fields.

Or if offset is ±32K meaning 32K = 2¹⁵, need 16 bits (15 for magnitude + 1 sign, or 16-bit 2's complement).
**Answer: 3 bits register + 3 bits register + 16 bits displacement = 22 bits for fields (+6 opcode = 28). 4 bits spare.**

---

### Q30. Processor comparison
Time_A = IC × 1.5 / (3 × 10⁹) = 0.5 × IC ns
Time_B = IC × 1.0 / (2 × 10⁹) = 0.5 × IC ns

**Both are equally fast!** (Same execution time)

---

## Section D: Pipelining

### Q31. Clock cycle time for 4-stage pipeline
Clock period = max stage delay = max(1, 1.5, 1, 2) = **2 ns**

---

### Q32. Speedup of 6-stage, 1000 instructions
Speedup = nk/(k+n−1) = 1000×6/(6+999) = 6000/1005 = **5.97x ≈ 6x**

---

### Q33. Three hazard types
1. **Structural hazards** (resource conflicts)
2. **Data hazards** (dependencies: RAW, WAR, WAW)
3. **Control hazards** (branches)

---

### Q34. Data hazards without forwarding
```
ADD R1, R2, R3   ; writes R1 in WB (cycle 5)
SUB R4, R1, R5   ; reads R1 in ID (cycle 3) — needs value from cycle 5 → 2 stalls
AND R6, R1, R7   ; reads R1 in ID (cycle 4) — needs value from cycle 5 → 1 stall
```

RAW hazard on R1 for both SUB and AND.
Without forwarding: SUB needs **2 stall cycles**, AND needs **1 stall cycle** (but if SUB is stalled, AND slides too).
Net: **2 stall cycles total** (SUB stalls 2, AND follows naturally).

---

### Q35. Load-use with forwarding
```
LW R1, 0(R2)     ; R1 available after MEM stage
ADD R3, R1, R4   ; needs R1 at EX stage
```
With MEM-EX forwarding: R1 available end of MEM (cycle 4), ADD needs at start of EX (cycle 4).
Still 1 cycle late → **1 stall cycle** needed (load-use hazard).

---

### Q36. CPI with branch stalls (no prediction)
Branch resolved at EX → penalty = 2 cycles per branch.
CPI = 1 + 0.20 × 2 = **1.4**

---

### Q37. CPI with branch predictor
CPI = 1 + 0.15 × (1−0.90) × 3 = 1 + 0.15 × 0.10 × 3 = 1 + 0.045 = **1.045**

---

### Q38. RAW, WAR, WAW
**RAW (Read After Write):** I2 reads before I1 writes. True dependency.
```
I1: R1 = R2 + R3
I2: R4 = R1 + R5    ; RAW on R1
```

**WAR (Write After Read):** I2 writes before I1 reads (in out-of-order).
```
I1: R4 = R1 + R3
I2: R1 = R5 + R6    ; WAR on R1
```

**WAW (Write After Write):** I2 writes before I1 writes (in out-of-order).
```
I1: R1 = R2 + R3
I2: R1 = R5 + R6    ; WAW on R1
```

---

### Q39. Pipeline clock period and latency
Clock period = max(1, 1.5, 2, 1, 0.5) + 0.1 = 2 + 0.1 = **2.1 ns**
Latency = 5 × 2.1 = **10.5 ns**

---

### Q40. Instructions to flush on misprediction
Branch is in ID stage. Wrong instructions fetched: the instruction in IF stage = **1 instruction** to flush.

If branch resolved at EX: instructions in IF and ID = **2 instructions** to flush.

---

### Q41. Pipeline speedup with branches
Non-pipelined: 10 ns/instruction
Pipelined: stage time = 2.5 + 0.5 = 3 ns clock period
Effective CPI = 1 + 0.20 × 2 = 1.4

Pipeline time per instruction = 1.4 × 3 = 4.2 ns
Speedup = 10/4.2 = **2.38x**

---

### Q42. Stalls with forwarding
```
LW R1, 0(R2)    ; R1 available after MEM
ADD R3, R1, R4  ; needs R1 at EX → 1 stall (load-use)
SW R3, 0(R5)    ; needs R3 at MEM → R3 from EX-MEM forwarding, no stall
```
**1 stall cycle** (only for load-use between LW and ADD).

---

### Q43. Hazards in out-of-order sequence
```
I1: R1 = R2 + R3    
I2: R4 = R1 × R5    ; RAW on R1 (I1→I2)
I3: R1 = R6 − R7    ; WAW on R1 (I1→I3), WAR on R1 (I2→I3)
I4: R8 = R1 + R4    ; RAW on R1 (I3→I4), RAW on R4 (I2→I4)
```

---

### Q44. 80% of max speedup condition
Max speedup = k. Need nk/(k+n−1) ≥ 0.8k
→ n/(k+n−1) ≥ 0.8
→ n ≥ 0.8(k+n−1) = 0.8k+0.8n−0.8
→ 0.2n ≥ 0.8k−0.8
→ n ≥ 4k−4 = **4(k−1)**

**Answer: n ≥ 4(k−1) instructions**

---

### Q45. Branch delay slot
The instruction immediately after a branch is always executed (delay slot), regardless of branch outcome. Compiler fills it with:
1. An instruction from before the branch (best)
2. An instruction from the taken path (if safe)
3. NOP (worst case)

---

## Section E: Cache Memory

### Q46. 64 KB direct-mapped, 32-byte blocks, 32-bit address
Lines = 64KB/32 = 2048 = 2¹¹
Offset = log₂(32) = 5 bits
Index = log₂(2048) = 11 bits
Tag = 32 − 11 − 5 = **16 bits**

---

### Q47. Three Cs
1. **Compulsory:** First access to a block (cold miss)
2. **Capacity:** Working set > cache size
3. **Conflict:** Multiple blocks compete for same cache set

---

### Q48. Write-through vs Write-back
**Write-through:** Every write updates both cache and memory. Simple, consistent. Higher memory traffic.
**Write-back:** Write only to cache; set dirty bit. Write to memory on eviction. Lower traffic. More complex.

---

### Q49. 256 KB, 4-way, 64-byte blocks, 32-bit address
Sets = 256KB/(64×4) = 1024 = 2¹⁰
Offset = log₂(64) = 6 bits
Index = log₂(1024) = 10 bits
Tag = 32 − 10 − 6 = **16 bits**

---

### Q50. AMAT
AMAT = 2 + 0.10 × 100 = 2 + 10 = **12 ns**

---

### Q51. Direct-mapped, 8 blocks — sequence: 0,4,8,0,4,8,0,4
Block address mod 8:
0%8=0, 4%8=4, 8%8=0, 0%8=0, 4%8=4, 8%8=0, 0%8=0, 4%8=4

| Access | Block | Cache Set | Hit/Miss |
|--------|-------|-----------|----------|
| 0 | 0→set 0 | Empty→load 0 | Miss |
| 4 | 4→set 4 | Empty→load 4 | Miss |
| 8 | 8→set 0 | Has 0→evict, load 8 | Miss |
| 0 | 0→set 0 | Has 8→evict, load 0 | Miss (conflict!) |
| 4 | 4→set 4 | Has 4 | **Hit** |
| 8 | 8→set 0 | Has 0→evict, load 8 | Miss |
| 0 | 0→set 0 | Has 8→evict, load 0 | Miss |
| 4 | 4→set 4 | Has 4 | **Hit** |

Hits: 2, Total: 8, **Miss rate = 6/8 = 75%**

---

### Q52. 2-way set-associative, 4 sets, LRU — same sequence
Block mod 4: 0%4=0, 4%4=0, 8%4=0, 0%4=0, 4%4=0, 8%4=0, 0%4=0, 4%4=0

All map to set 0 (2 ways available):

| Access | Set 0 Ways | LRU | Hit/Miss |
|--------|----------|-----|----------|
| 0 | [0, _] | 0 older | Miss |
| 4 | [0, 4] | 0 older | Miss |
| 8 | [8, 4] | evict 0 (LRU) | Miss |
| 0 | [8, 0] | evict 4 (LRU) | Miss |
| 4 | [4, 0] | evict 8 (LRU) | Miss |
| 8 | [4, 8] | evict 0 (LRU) | Miss |
| 0 | [0, 8] | evict 4 (LRU) | Miss |
| 4 | [0, 4] | evict 8 (LRU) | Miss |

**Miss rate = 8/8 = 100%** (thrashing — 3 blocks compete for 2 ways)

---

### Q53. Two-level AMAT
AMAT = 1 + 0.05 × (10 + 0.02 × 200) = 1 + 0.05 × (10 + 4) = 1 + 0.05 × 14 = 1 + 0.7 = **1.7 cycles**

---

### Q54. LRU bits for 4-way
For exact LRU with k=4: need to store ordering of 4 elements.
Number of orderings = 4! = 24 → need ⌈log₂(24)⌉ = **5 bits per set**

(Approximate LRU schemes use fewer bits.)

---

### Q55. Cache size
128 lines × 64 bytes/line = 8192 bytes = **8 KB**

---

### Q56. Bits for tag + valid for entire cache
Cache blocks = 32KB/32 = 1024 = 2¹⁰
Offset = 5, Index = 10, Tag = 32−10−5 = 17
Each line: 17 tag + 1 valid = 18 bits
Total = 1024 × 18 = **18432 bits = 18 Kbits**

---

### Q57. 4 KB cache, 16-byte blocks, 16-bit address
Lines = 4KB/16 = 256 lines (direct-mapped)
Offset = 4 bits, Index = 8 bits, Tag = 16−8−4 = 4 bits

Addresses: 0x0000, 0x0040, 0x0080, 0x0000, 0x0040, 0x0080
Block addresses: 0/16=0, 64/16=4, 128/16=8
Index: 0%256=0, 4%256=4, 8%256=8 (all different sets!)

| Access | Index | Hit/Miss |
|--------|-------|----------|
| 0x0000 | 0 | Miss |
| 0x0040 | 4 | Miss |
| 0x0080 | 8 | Miss |
| 0x0000 | 0 | **Hit** |
| 0x0040 | 4 | **Hit** |
| 0x0080 | 8 | **Hit** |

**Hit ratio = 3/6 = 50%**

---

### Q58. 2-way, 256 blocks, 32 KB, 32-bit address
Block size = 32KB/256 = 128 bytes
Sets = 256/2 = 128
Offset = log₂(128) = 7, Index = log₂(128) = 7
Tag = 32 − 7 − 7 = **18 bits**

---

### Q59. Effective CPI with separate caches
Memory stalls per instruction:
- I-cache: 1 × 0.02 × 50 = 1.0 (every instruction fetched)
- D-cache: 0.30 × 0.05 × 50 = 0.75

Effective CPI = 1 + 1.0 + 0.75 = **2.75**

---

### Q60. Fully associative, 16 blocks, LRU — sequence 1,2,3,4,5,1,2,3,4,5
All 5 distinct blocks fit in 16-block cache.
| Access | Hit/Miss |
|--------|----------|
| 1 | Miss |
| 2 | Miss |
| 3 | Miss |
| 4 | Miss |
| 5 | Miss |
| 1 | **Hit** |
| 2 | **Hit** |
| 3 | **Hit** |
| 4 | **Hit** |
| 5 | **Hit** |

**Miss rate = 5/10 = 50%** (only compulsory misses)

---

### Q61. Belady's Anomaly
Increasing cache size (number of frames) can increase miss rate with **FIFO** replacement policy. Does NOT occur with LRU or Optimal.

---

### Q62. Two-level AMAT
AMAT = 1 + 0.10 × (10 + 0.05 × 100) = 1 + 0.10 × (10+5) = 1 + 0.10 × 15 = 1 + 1.5 = **2.5 ns**

---

### Q63. Write-back + write-allocate on write miss
1. Load the block from memory into cache (allocate)
2. Modify the cached block
3. Set dirty bit = 1
4. Block written back to memory only on eviction

---

### Q64. Sets in 1 MB, 16-way, 256-byte blocks
Sets = 1MB/(256×16) = 1,048,576/4096 = **256 sets**

---

### Q65. Effect of increasing block size

| Miss Type | Effect |
|-----------|--------|
| Compulsory | **Decreases** (more spatial locality exploited) |
| Capacity | **May increase** (fewer blocks in cache) |
| Conflict | **May increase** (fewer total blocks) |

Optimal block size is a tradeoff — too large increases miss penalty and capacity/conflict misses.

---

## Section F: I/O Organization

### Q66. Comparison of I/O techniques
| Feature | Programmed I/O | Interrupt I/O | DMA |
|---------|---------------|--------------|-----|
| CPU involvement | 100% | Only during ISR | Only setup |
| Efficiency | Low | Medium | High |
| Hardware | Simple | Interrupt controller | DMA controller |
| Best for | Slow, simple devices | Medium-speed | High-speed, bulk |

---

### Q67. DMA transfer time
Setup: 1000 ns
Transfer: 1000 bytes × 40 ns/byte = 40,000 ns
Total: 1000 + 40,000 = **41,000 ns = 41 µs**

---

### Q68. CPU speed reduction in cycle-stealing DMA
Each DMA transfer steals one memory cycle from CPU.
If CPU needs bus 60% of time and DMA steals cycles intermittently:
During DMA, CPU may be delayed when both need bus simultaneously.
Effective CPU slowdown depends on DMA rate.

If 1 DMA cycle per N CPU cycles: CPU speed reduced by 1/(N+1).
General: CPU slows proportionally to fraction of cycles stolen.

---

### Q69. CPU fraction for interrupt-driven I/O
Transfer rate: 1 MB/s ÷ 4 bytes/transfer = 250,000 interrupts/s
Each interrupt: 100 cycles / 500 MHz = 200 ns
Total CPU time: 250,000 × 200 ns = 0.05 s per second
**Fraction = 5%**

---

### Q70. Average time to read one sector
Avg seek = 8 ms
Avg rotational latency = (1/2) × (60/7200) s = 4.17 ms
Transfer time = 512 bytes / (50 MB/s) = 0.01 ms

Total = 8 + 4.17 + 0.01 ≈ **12.18 ms**

---

## Section G: Mixed / Comprehensive

### Q71. Page table size
Pages = 2³² / 4K = 2³² / 2¹² = 2²⁰ entries
Each entry = 4 bytes
Size = 2²⁰ × 4 = **4 MB**

---

### Q72. Effective CPI with cache
For memory instructions (30%): CPI = 5 + 0.10 × 50 = 10 (10% miss rate)
Wait: the question says CPI=5 for memory instructions and hit rate=90%.

Effective CPI = 0.70 × 4 + 0.30 × (5 + miss_rate × penalty)
Hmm, re-reading: "CPI=2 for non-memory (70%), CPI=5 for memory (30%). Cache hit rate 90%, miss penalty 50."

Memory instructions: CPI_mem = 5 + (1−0.90) × 50 = 5 + 5 = 10
Actually, CPI=5 already includes cache hit case? Let's assume CPI=5 is base memory CPI (hit case), and misses add penalty.

CPI_mem = 5 + 0.10 × 50 = 10
Overall CPI = 0.70 × 2 + 0.30 × 10 = 1.4 + 3.0 = **4.4**

Or if CPI=5 includes all memory time without cache:
With cache: CPI_mem = 0.90 × 1 + 0.10 × (1 + 50) = 0.9 + 5.1 = 6
Overall = 0.70 × 2 + 0.30 × 6 = 1.4 + 1.8 = 3.2

Most likely interpretation with explicit cache: **Effective CPI ≈ 4.4**

---

### Q73. Two-level page table sizes
Outer page table: 2¹⁰ entries × 4 bytes = **4 KB**
Inner page table: 2¹⁰ entries × 4 bytes = **4 KB each** (there are 2¹⁰ inner tables)
Total inner: 2¹⁰ × 4 KB = 4 MB (but only allocated as needed)

---

### Q74. Which processor is faster?
Time_A = IC × 1.5 / (4 × 10⁹) = 0.375 × IC ns
Time_B = IC × 1.0 / (3 × 10⁹) = 0.333 × IC ns

**Processor B is faster** by factor 0.375/0.333 = **1.125x (12.5% faster)**

---

### Q75. Maximum pipeline speedup
Max speedup = **k** (number of stages), achieved when:
- Large number of instructions (n >> k)
- No hazards/stalls
- All stages perfectly balanced
- No pipeline flush

---

## Section H: Amdahl's Law & Performance

### Q76. Maximum speedup with infinite processors (80% parallel)
Speedup_max = 1/(1 − f) = 1/(1 − 0.8) = 1/0.2 = **5x**

---

### Q77. FP speedup 10x, FP = 40% of time
Speedup = 1 / ((1 − 0.4) + 0.4/10) = 1 / (0.6 + 0.04) = 1/0.64 = **1.5625x**

---

### Q78. Program 100s, subroutine 70s, target 40s
Non-subroutine time = 30s (fixed)
Need subroutine time = 40 − 30 = 10s
Speedup needed = 70/10 = **7x**

---

### Q79. Gustafson's Law: s=0.05, N=64
Speedup = s + p × N = 0.05 + 0.95 × 64 = 0.05 + 60.8 = **60.85x**

---

### Q80. Ranking three processors
Time ∝ CPI / Clock Rate
- A: 1.5/4 = 0.375 ns/instruction
- B: 1.0/3 = 0.333 ns/instruction ← **Fastest**
- C: 2.0/5 = 0.400 ns/instruction ← Slowest

**Ranking: B > A > C** (B is fastest)

---

### Q81. Amdahl's with 8 processors (20% serial)
Speedup(8) = 1 / (0.2 + 0.8/8) = 1 / (0.2 + 0.1) = 1/0.3 = **3.33x**
Max speedup = 1/(1 − 0.8) = 1/0.2 = **5x**

---

### Q82. Speedup=3 with 4 processors, find f
3 = 1 / ((1−f) + f/4) → 3(1−f+f/4) = 1 → 3 − 3f + 3f/4 = 1
→ 3 − 3f + 0.75f = 1 → 3 − 2.25f = 1 → 2.25f = 2 → **f = 8/9 ≈ 0.889 (88.9%)**

---

## Section I: ILP, Superscalar & Memory Interleaving

### Q83. Superscalar vs VLIW
- **Superscalar:** Hardware dynamic scheduling; advantage: binary compatibility across generations
- **VLIW:** Compiler static scheduling; advantage: simpler hardware, lower power

---

### Q84. Register renaming and hazard types
- **WAR (anti-dependency):** Writing before a previous read completes. Renaming gives the writer a new physical register → old read still sees correct value.
- **WAW (output dependency):** Two writes to same register. Renaming gives each a different physical register → both writes succeed independently.
- **RAW (true dependency):** Reader NEEDS the value from writer. No renaming can create the value before it's computed. Can only be resolved by forwarding or stalling.

---

### Q85. 2-issue superscalar instruction pairing
Dependencies: I2 depends on I1 (R1), I4 depends on I3 (R6) and I2 (R4)
- **Cycle 1:** I1 + I3 (independent, can issue together)
- **Cycle 2:** I2 (needs R1 from I1, may get via forwarding)
- **Cycle 3:** I4 (needs R6 from I3 and R4 from I2)
IPC = 4/3 ≈ **1.33**

---

### Q86. 4-bank low-order interleaving
Access time T = 40 ns, bus cycle τ = 10 ns
With interleaving, a new access can start every τ = 10 ns (pipeline effect):
First word: 40 ns
Subsequent words: every 10 ns
Effective access time for sequential reads = **10 ns** (after initial latency)

---

### Q87. High-order interleaving, 8 modules × 1 KB
Each module = 1024 bytes (addresses 0-1023 in module 0, 1024-2047 in module 1, etc.)
Address 2500: Module = ⌊2500/1024⌋ = **Module 2** (addresses 2048-3071)
Offset within module = 2500 − 2048 = 452

---

### Q88. 4-bank interleaved, 200 ns access, 50 ns bus, 16 reads
**Interleaved:**
First word latency: 200 ns
Subsequent 15 words: every 50 ns = 15 × 50 = 750 ns
Total = 200 + 750 = 950 ns
Throughput = 16/950 ns = **16.84 Mwords/s**

**Non-interleaved:**
Each read: 200 ns
Total = 16 × 200 = 3200 ns
Throughput = 16/3200 ns = 5 Mwords/s

Speedup = 3200/950 = **3.37x**

---

### Q89. Superscalar 3-wide, issue pattern {3,2,3,2,1,1}
Total instructions = 3+2+3+2+1+1 = 12
Total cycles = 6
IPC = 12/6 = **2.0**
CPI = 6/12 = **0.5**

---

## Section J: Bus Arbitration & Cache Coherence

### Q90. Daisy chain priority
The device **closest to the arbiter** has the highest priority. Main disadvantage: **fixed priority** — low-priority devices may starve. Also, single point of failure (faulty device breaks chain).

---

### Q91. MESI states
- **M (Modified):** Only copy in this cache, has been written (dirty). Memory is stale.
- **E (Exclusive):** Only copy in this cache, matches memory (clean). Can write without bus transaction.
- **S (Shared):** Multiple caches may have copies, all clean, matches memory.
- **I (Invalid):** Cache line is not valid; must fetch from memory or another cache.

---

### Q92. MESI: Cache A has X in E, CPU B reads X
1. CPU B sends a read on the bus
2. Cache A snoops the read, detects it holds X in E state
3. Cache A responds with the data (cache-to-cache transfer)
4. **Cache A: E → S**
5. **Cache B: I → S**
Both caches now have X in **Shared** state.

---

### Q93. False sharing
**Definition:** Two processors access different variables (e.g., x and y) that reside on the same cache line. When one writes, the entire line is invalidated in the other cache, causing unnecessary misses.

**Performance impact:** Continuous invalidation/fetching of the cache line, turning independent operations into serialized cache misses (ping-pong effect).

**Fix:** Pad variables so they occupy separate cache lines:
```c
struct { int x; char pad[60]; } aligned1;  // Assuming 64-byte cache line
struct { int y; char pad[60]; } aligned2;
```

---

### Q94. Centralized parallel arbitration, 8 devices
Each device needs: 1 request line + 1 grant line = **8 request lines + 8 grant lines = 16 lines total**

---

### Q95. MESI: Cache 1 has X in M, Cache 2 writes X
1. Cache 2 issues a **write miss** (RWITM — Read With Intent To Modify) on bus
2. Cache 1 snoops, sees RWITM, and has X in M state
3. Cache 1 **writes back** X to memory (flushes dirty data)
4. Cache 1 transitions: **M → I** (invalidated)
5. Cache 2 receives data, loads into cache
6. Cache 2 transitions: **I → M** (modified)

Final: **Cache 1: I, Cache 2: M**

---

### Q96. Two-processor access pattern (write-invalidate)
Initial: X in memory only, both caches empty.

| Step | Action | Bus Transaction | Misses |
|------|--------|----------------|--------|
| P1: Read X | P1 cache miss | BusRd → fetch from memory | 1 |
| P1: Write X | P1 has E→M (or S→M) | BusUpgr (if S) / none (if E) | 0 |
| P2: Read X | P2 cache miss, P1 has M | BusRd → P1 writes back, both → S | 1 |
| P2: Write X | P2 S→M | BusUpgr → invalidate P1, P1→I | 0 |
| P1: Read X | P1 cache miss, P2 has M | BusRd → P2 writes back, both → S | 1 |

**Bus transactions: ~4** (BusRd, writeback/BusUpgr, BusRd+writeback, BusUpgr, BusRd+writeback)
**Cache misses: 3** (P1 first read, P2 first read, P1 last read)

---

### Q97. Sequential Consistency vs TSO
**Sequential Consistency:** Memory operations appear in program order — all processors agree on a single total order consistent with each processor's program order.

**TSO (Total Store Order):** Stores may be buffered (store buffer). A processor can read its own store before it's visible to others. Loads can overtake stores.

**Example where TSO allows but SC does not:**
```
Initially: x = 0, y = 0
P1: x = 1; r1 = y     P2: y = 1; r2 = x
```
Under SC: r1 = 0 AND r2 = 0 is impossible.
Under TSO: r1 = 0 AND r2 = 0 **is possible** (both stores in buffers, reads happen before stores are visible globally).

---

### Q98. Two-level page table with TLB
TLB hit: time = t_TLB + t_mem = 5 + 100 = 105 ns
TLB miss: time = t_TLB + 2×t_mem + t_mem = 5 + 100 + 100 + 100 = 305 ns (2 page table accesses + 1 data access)

EAT = 0.95 × 105 + 0.05 × 305 = 99.75 + 15.25 = **115 ns**

---

### Q99. Interleaved memory: m=4, T=200ns, τ=50ns
Each bank busy for T=200 ns after access. Banks are accessed in round-robin.
Requests spaced τ=50 ns apart. After accessing bank 0 at t=0, bank 0 is accessed again at t=4×50=200 ns = T → just ready.
Since mτ = 4×50 = 200 = T, the banks are fully utilized.
Bandwidth = 1 word per τ = 1/50ns = **20 Mwords/s** (vs 5 Mwords/s non-interleaved)

---

### Q100. IPC=2.5, Clock=3 GHz, 16 processors, serial=10%
MIPS = IPC × Clock = 2.5 × 3000 MHz = **7500 MIPS**
Amdahl's max speedup = 1/(0.1 + 0.9/16) = 1/(0.1 + 0.05625) = 1/0.15625 = **6.4x**

---

*End of Solutions — Computer Organization & Architecture for GATE 2027*
