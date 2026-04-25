# Computer Organization & Architecture — Questions and PYQs for GATE 2027

> **Total Questions:** 75 | **Sections:** 7
> **Difficulty Mix:** Easy (30%) | Medium (40%) | GATE PYQ level (30%)

---

## Section A: Basic Components & Memory Organization (10 Questions)

### Easy
**Q1.** A computer has 24 address lines and 16 data lines. If the memory is byte-addressable, what is the maximum memory capacity?

**Q2.** Store the hexadecimal value 0xABCD1234 starting at address 2000 in both big-endian and little-endian formats.

**Q3.** What is the width of the data bus if the processor's word size is 64 bits?

### Medium
**Q4.** A 1 MB byte-addressable memory is to be constructed using 256K × 4-bit memory chips. How many chips are needed?

**Q5.** A computer has a 16-bit address bus and 8-bit data bus. Memory consists of 2K × 8-bit RAM chips. How many chips are needed to fill the address space? How many address bits are used for chip select?

**Q6.** Explain the difference between memory-mapped I/O and isolated I/O with an example.

### GATE Level
**Q7.** [GATE] A processor uses 36-bit physical address and 32-bit virtual address. Page size is 4 KB. What is the number of entries in the page table?

**Q8.** A 4-way interleaved memory has a cycle time of 80 ns and a bus transfer time of 20 ns. What is the effective memory access time for sequential accesses?

**Q9.** [GATE] A machine has a 32-bit address space. Main memory is 512 MB. What fraction of the address space does main memory occupy?

**Q10.** In a byte-addressable system with 32-bit words, what is the address of the 3rd byte of the word at address 0x0008 in little-endian?

---

## Section B: Data Representation & Floating Point (10 Questions)

### Easy
**Q11.** Convert the decimal number 19.6875 to IEEE 754 single-precision format.

**Q12.** What decimal value does the IEEE 754 single-precision 0 10000100 01100000000000000000000 represent?

**Q13.** What is the range of normalized positive numbers in IEEE 754 single precision?

### Medium
**Q14.** Can the number 0.1 (decimal) be represented exactly in IEEE 754? Explain.

**Q15.** What is the difference between denormalized numbers and normalized numbers in IEEE 754?

**Q16.** Two IEEE 754 single-precision numbers are added: 1.0 × 2¹⁰ + 1.0 × 2⁻¹⁰. What precision issues arise?

**Q17.** [GATE] The IEEE 754 representation of a number is 0xC1480000. What is the decimal value?

### GATE Level
**Q18.** [GATE] In IEEE 754 single precision, what is the hexadecimal representation of −0?

**Q19.** [GATE 2015] The value 4.2 is stored in IEEE 754 single precision. How many bits are used for the exponent?

**Q20.** What is the smallest positive number greater than 1.0 that can be represented in IEEE 754 single-precision? What is the gap between 1.0 and this number?

---

## Section C: Instruction Set Architecture (10 Questions)

### Easy
**Q21.** What are the advantages of RISC over CISC architecture?

**Q22.** A 3-address instruction ADD R1, R2, R3 requires: how many operand fields in instruction?

**Q23.** In immediate addressing mode, where is the operand located?

### Medium
**Q24.** A machine has 16-bit instructions with 4-bit opcode and three 4-bit register fields. Using expanding opcode: if 12 three-address instructions are used, how many two-address instructions can be accommodated?

**Q25.** A processor has the following instruction mix: 43% ALU, 21% Load, 12% Store, 24% Branch. If ALU takes 4 cycles, Load 5, Store 4, Branch 3, what is the average CPI?

**Q26.** For each addressing mode, state the number of memory accesses needed to fetch the operand: (a) Immediate (b) Direct (c) Indirect (d) Register Indirect

**Q27.** In a stack machine (0-address), show how to compute X = (A+B) × (C−D) using only PUSH, POP, ADD, SUB, MUL.

### GATE Level
**Q28.** [GATE 2019] A processor with a clock frequency of 2.5 GHz takes 0.8 seconds to execute a program with 500 million instructions. What is the average CPI?

**Q29.** [GATE] In a 32-bit instruction format, the first 6 bits are opcode. The instruction uses base-register + offset addressing. If there are 8 general-purpose registers, and the maximum displacement is ±32K, how are the remaining bits allocated?

**Q30.** [GATE] Processor A has CPI=1.5 at 3 GHz. Processor B has CPI=1.0 at 2 GHz. For the same program (same IC), which is faster and by how much?

---

## Section D: Pipelining (15 Questions)

### Easy
**Q31.** A 4-stage pipeline with stage delays of 1ns, 1.5ns, 1ns, 2ns. What is the clock cycle time?

**Q32.** What is the speedup of a 6-stage pipeline executing 1000 instructions?

**Q33.** Name the three types of pipeline hazards.

### Medium
**Q34.** Consider the following MIPS code:
```
ADD R1, R2, R3
SUB R4, R1, R5
AND R6, R1, R7
```
Identify the data hazard(s). How many stall cycles are needed without forwarding (5-stage pipeline)?

**Q35.** With forwarding (EX-EX and MEM-EX), how many stall cycles are needed for:
```
LW R1, 0(R2)
ADD R3, R1, R4
```

**Q36.** A 5-stage pipeline processes 100 instructions. 20% are branches, and branches are resolved at EX stage. Assuming no prediction (stall until resolved), what is the effective CPI?

**Q37.** A branch predictor has 90% accuracy. Branch penalty is 3 cycles. 15% of instructions are branches. What is CPI?

**Q38.** Explain the difference between RAW, WAR, and WAW hazards with examples.

### GATE Level
**Q39.** [GATE 2017] A 5-stage pipeline has IF=1ns, ID=1.5ns, EX=2ns, MEM=1ns, WB=0.5ns. Pipeline register delay is 0.1ns. What is the clock period and latency?

**Q40.** [GATE] In a 5-stage pipeline, a branch instruction is in the ID stage. If the processor uses branch prediction and the prediction is wrong, how many instructions need to be flushed?

**Q41.** [GATE 2016] A non-pipelined processor takes 10ns per instruction. A 5-stage pipelined version has 2.5ns per stage with a 0.5ns pipeline register overhead. For a program with 20% branch instructions (no stalls otherwise), what is the speedup if branches incur 2-cycle penalty?

**Q42.** [GATE] Consider a 5-stage pipeline where data forwarding from EX/MEM and MEM/WB is implemented. The instruction sequence is:
```
LW R1, 0(R2)
ADD R3, R1, R4
SW R3, 0(R5)
```
How many stall cycles are needed?

**Q43.** A processor issues instructions out-of-order. Identify all hazard types in:
```
R1 = R2 + R3    (I1)
R4 = R1 × R5    (I2)
R1 = R6 − R7    (I3)
R8 = R1 + R4    (I4)
```

**Q44.** [GATE] In a pipeline with k stages, each having delay d nanoseconds, what is the minimum number of instructions for the pipeline to achieve at least 80% of maximum speedup?

**Q45.** What is the purpose of a branch delay slot? How does the compiler fill it?

---

## Section E: Cache Memory (20 Questions)

### Easy
**Q46.** A 64 KB direct-mapped cache with 32-byte blocks. Address is 32 bits. How many tag bits, index bits, and offset bits?

**Q47.** What are the three types of cache misses (3 Cs)?

**Q48.** Explain write-through vs write-back cache policies.

### Medium
**Q49.** A 256 KB 4-way set-associative cache with 64-byte blocks. Address is 32 bits. Calculate tag, index, offset bits.

**Q50.** A cache has hit time = 2 ns, miss rate = 10%, and miss penalty = 100 ns. What is the AMAT?

**Q51.** [GATE-style] A direct-mapped cache with 8 blocks (block size = 1 word). Memory has 32 blocks. What is the miss rate for address sequence: 0, 4, 8, 0, 4, 8, 0, 4 (using block addresses)?

**Q52.** For the same sequence in Q51, what would the miss rate be with a 2-way set-associative cache (4 sets, LRU)?

**Q53.** A 2-level cache: L1 has 1-cycle hit time and 5% miss rate. L2 has 10-cycle hit time and 2% miss rate (local). Main memory is 200 cycles. What is the AMAT?

**Q54.** A cache holds 512 blocks. LRU replacement is used in a 4-way set-associative cache. How many LRU bits per set?

**Q55.** A direct-mapped cache has 128 lines, with each line being 64 bytes. What is the cache size?

### GATE Level
**Q56.** [GATE 2014] A 32 KB direct-mapped cache with 32-byte blocks. 32-bit address. How many bits are needed for tag + valid bit for the entire cache?

**Q57.** [GATE 2018] A processor with a 4 KB direct-mapped cache, block size 16 bytes. Main memory is byte-addressable with 16-bit addresses. A program accesses memory at addresses: 0x0000, 0x0040, 0x0080, 0x0000, 0x0040, 0x0080. What is the hit ratio?

**Q58.** [GATE 2016] Consider a 2-way set-associative cache with 256 blocks, total size 32 KB. 32-bit address. Find the number of tag bits.

**Q59.** [GATE] A processor has separate instruction and data caches. I-cache miss rate = 2%, D-cache miss rate = 5%. 30% of instructions are load/store. Miss penalty = 50 cycles. Base CPI = 1. What is the effective CPI?

**Q60.** [GATE] In a fully associative cache with 16 blocks and LRU replacement, what is the miss rate for the sequence 1, 2, 3, 4, 5, 1, 2, 3, 4, 5 (cache initially empty)?

**Q61.** What is Belady's anomaly? Which replacement policy exhibits it?

**Q62.** [GATE 2017] A system has a 2-level cache. L1 has a local miss rate of 10% and access time of 1 ns. L2 has a local miss rate of 5% and access time of 10 ns. Main memory access is 100 ns. What is the average memory access time?

**Q63.** A cache uses write-back with write-allocate. On a write miss, describe the sequence of events.

**Q64.** [GATE] The number of sets in a 1 MB, 16-way set-associative cache with 256-byte blocks is ______.

**Q65.** How does increasing block size affect each of the 3 Cs of cache misses?

---

## Section F: I/O Organization (5 Questions)

### Easy
**Q66.** Compare programmed I/O, interrupt-driven I/O, and DMA.

### Medium
**Q67.** A DMA controller transfers 1000 bytes from disk to memory. Memory cycle time = 40 ns. DMA setup takes 1000 ns. What is the total DMA transfer time?

**Q68.** In cycle-stealing DMA, if memory cycle = 50 ns and CPU needs the bus 60% of the time, what is the effective CPU speed reduction?

### GATE Level
**Q69.** [GATE] A device transfers data at 1 MB/s and each transfer is 4 bytes. If interrupt overhead is 100 cycles and CPU runs at 500 MHz, what fraction of CPU time is consumed?

**Q70.** [GATE] A disk has: rotation speed = 7200 RPM, seek time = 8 ms, transfer rate = 50 MB/s, sector size = 512 bytes. What is the average time to read one sector?

---

## Section G: Mixed / Comprehensive (5 Questions)

### GATE Level
**Q71.** [GATE] A computer uses 32-bit virtual addresses and 4 KB pages. Each page table entry is 4 bytes. What is the size of the page table?

**Q72.** [GATE] Consider a processor with CPI=2 for non-memory instructions (70%) and CPI=5 for memory instructions (30%). With a cache (hit rate 90%, miss penalty 50 cycles), what is effective CPI?

**Q73.** A two-level page table: 10 bits for outer page, 10 bits for inner page, 12 bits offset. Page table entry is 4 bytes. What are the sizes of the outer and inner page tables?

**Q74.** [GATE] Processor A: 4 GHz, CPI=1.5. Processor B: 3 GHz, CPI=1.0. Which processor is faster?

**Q75.** [GATE] In a 5-stage pipeline, what is the maximum speedup achievable? Under what conditions is it achieved?

---

## Section H: Amdahl's Law & Performance (10 Questions)

### Easy
**Q76.** A program has 80% parallelizable code. Using Amdahl's Law, what is the maximum speedup with infinite processors?

**Q77.** A processor enhancement speeds up floating-point operations by 10x. If FP operations constitute 40% of execution time, what is the overall speedup?

### Medium
**Q78.** [GATE-style] A program runs in 100 seconds. 70 seconds is spent in a subroutine that can be sped up. How much speedup of the subroutine is needed to make the program run in 40 seconds?

**Q79.** Using Gustafson's Law, if serial fraction is 5% and there are 64 processors, what is the scaled speedup?

**Q80.** Processor A has CPI=1.5 at 4 GHz. Processor B has CPI=1.0 at 3 GHz. Processor C has CPI=2.0 at 5 GHz. Rank them by performance.

### GATE Level
**Q81.** [GATE 2019] A program has two phases: Phase 1 takes 20% of time and is sequential. Phase 2 can use p processors. What is the speedup with 8 processors? What is the maximum speedup with unlimited processors?

**Q82.** A system achieves a speedup of 3x with 4 processors. What fraction of the program is parallelizable (using Amdahl's Law)?

---

## Section I: ILP, Superscalar & Memory Interleaving (10 Questions)

### Easy
**Q83.** What is the difference between superscalar and VLIW processors? Give one advantage of each.

**Q84.** Explain why register renaming can eliminate WAR and WAW hazards but not RAW hazards.

### Medium
**Q85.** A 2-issue superscalar processor runs a program with the following dependency pattern. Which instructions can be issued together?
```
I1: R1 = R2 + R3
I2: R4 = R1 × R5
I3: R6 = R7 + R8
I4: R9 = R6 − R4
```

**Q86.** A memory system has 4 banks with low-order interleaving. Each bank has access time of 40 ns. Bus cycle time is 10 ns. What is the effective access time for sequential reads?

**Q87.** In high-order interleaving with 8 modules, each 1 KB, which module contains address 2500?

### GATE Level
**Q88.** [GATE-style] A 4-bank interleaved memory with 200 ns access time per bank and 50 ns bus transfer time. What is the throughput for 16 consecutive word reads? Compare with non-interleaved.

**Q89.** [GATE] A superscalar processor can issue up to 3 instructions per cycle. In a sequence of 12 instructions, dependency constraints allow at most the following issue pattern: {3,2,3,2,1,1}. What is the IPC and CPI?

---

## Section J: Bus Arbitration & Cache Coherence (10 Questions)

### Easy
**Q90.** In daisy chain bus arbitration, which device gets highest priority? What is the main disadvantage?

**Q91.** Define the four states of the MESI cache coherence protocol.

### Medium
**Q92.** In a MESI protocol, Cache A has block X in state E. CPU B requests to read X. What are the resulting states of X in both caches?

**Q93.** What is false sharing? Why does it degrade performance, and how can it be fixed?

**Q94.** A system uses centralized parallel bus arbitration with 8 devices. How many request and grant lines are needed?

### GATE Level
**Q95.** [GATE-style] In a MESI protocol, Cache 1 has block X in M state. Cache 2 issues a write to X. Describe the sequence of events and final states.

**Q96.** [GATE 2015-style] Consider a two-processor system with write-invalidate cache coherence. The access pattern is:
```
P1: Read X, Write X
P2: Read X, Write X
P1: Read X
```
Assume X starts in memory and both caches are cold. How many bus transactions occur? How many cache misses?

**Q97.** What is the difference between sequential consistency and total store order (TSO)? Give an example where TSO allows a result that sequential consistency does not.

**Q98.** [GATE-style] A two-level page table uses 10 bits for level-1, 10 bits for level-2, and 12 bits for offset. Page table entries are 4 bytes. If TLB hit rate is 95% and TLB access time is 5 ns, memory access time is 100 ns, what is the effective memory access time?

**Q99.** [GATE 2020-style] In an interleaved memory system with m=4 banks and access time T=200ns, bus cycle τ=50ns, how many cycles elapse before a bank can be accessed again? What bandwidth is achieved for sequential access?

**Q100.** A processor achieves IPC of 2.5 on a benchmark. Clock frequency is 3 GHz. How many MIPS does it achieve? If Amdahl's serial fraction is 10%, what is the maximum speedup with this processor as one of 16 parallel units?

---

*End of Questions — Computer Organization & Architecture for GATE 2027*
