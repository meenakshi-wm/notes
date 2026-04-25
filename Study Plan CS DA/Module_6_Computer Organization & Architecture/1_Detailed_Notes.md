# 🖥️ Computer Organization & Architecture — Detailed Notes for GATE 2027 🎯

> 🌟 **"COA is the bridge between software and hardware — it answers the question: HOW does a computer ACTUALLY execute your code?"**

> 💡 **Beginner Analogy:** Think of a computer as a restaurant 🍽️:
> - **CPU** = The Chef 👨‍🍳 (does all the actual work/cooking)
> - **ALU** = Chef's knife and stove (tools for arithmetic and logic)
> - **Registers** = Chef's countertop (tiny, super-fast workspace right next to the chef)
> - **Cache** = Kitchen shelf (small but fast — frequently used ingredients)
> - **RAM** = Refrigerator (bigger, slower than countertop)
> - **Hard Disk/SSD** = Warehouse 🏢 (huge storage, but far away and slow to access)
> - **I/O Devices** = Waiters and customers (input orders, output food!)
> - **Bus** = The hallway connecting kitchen, fridge, and warehouse 🚶
>
> The entire COA subject is about making this "restaurant" serve customers (programs) as FAST as possible! 🚀

---

## 🏭 Where is COA Knowledge Used in the Real World?

| 🌍 Industry | 🔧 Application | 📌 COA Concept |
|---|---|---|
| **Intel/AMD/ARM** 🩸 | Designing next-gen CPUs | Pipelining, ISA design, Cache hierarchy |
| **Apple M-series chips** 🍎 | Industry-leading performance/watt | Pipeline optimization, Memory hierarchy |
| **NVIDIA GPUs** 🎮 | Graphics + AI acceleration | Parallel processing, Memory bandwidth |
| **Cloud Computing (AWS/Azure)** ☁️ | Instance type selection (compute vs memory) | Cache sizes, Memory bandwidth |
| **Game Development** 🎮 | Optimizing for 60fps/4K | Cache-friendly code, Memory access patterns |
| **Embedded Systems (IoT)** 💡 | Smartwatch, sensor nodes | Power-efficient architecture, DMA |
| **High-Frequency Trading** 📈 | Nanosecond-level speed | Cache optimization, Branch prediction |
| **Compiler Optimization (GCC/LLVM)** ⚙️ | Making code run faster | Pipeline awareness, Register allocation |
| **Database Systems** 🗄️ | Query engine optimization | Cache-conscious algorithms |

> **GATE Weightage:** 4–6 marks (2–3 questions) | **Priority:** High 🔥
> **Time to Spend:** 4–5 weeks | **Difficulty:** Medium-Hard
> **Preparation Order:** After Digital Logic 📢

---

## Table of Contents
1. Basic Computer Components 🧱
2. Data Representation & Floating Point 🔢
3. Instruction Set Architecture 📋
4. CPU Design & Datapath ⚙️
5. Pipelining 🔄
6. Memory Hierarchy, Cache & Virtual Memory 💾
8. I/O Organization ⚡
9. RAID, Storage & Performance Benchmarks
10. Memory Technology Details
11. Power and Energy in Processors
12. Advanced Topics for GATE (Flynn's, NUMA, Interconnects, Parallel Computing)
13. Number System Deep Dive
14. MIPS Instruction Set Reference

---

## 1. Basic Computer Components 🧱

### 1.1 Von Neumann Architecture 🏗️

> 💡 **Beginner Analogy — The Recipe Follower 📝:**
> Von Neumann architecture is like a person who reads a recipe step by step:
> 1. **Fetch** the next instruction from the recipe book (memory) 📖
> 2. **Decode** what it means ("beat 2 eggs" → understand the action) 🤔
> 3. **Execute** the instruction (actually beat the eggs!) 🥚
> 4. **Store** the result (put beaten eggs in a bowl) 🥣
> 5. Move to the next step and repeat!
>
> This **Fetch-Decode-Execute** cycle is what EVERY computer does, billions of times per second! Your smartphone does this cycle about 3 billion times/sec! 🚀

> 🏭 **Historical Insight:** This architecture was published in 1945 by John von Neumann, but the concept was actually conceived by J. Presper Eckert and John Mauchly (ENIAC creators). The famous "stored program" idea revolutionized computing — before this, computers were physically rewired for each new program! 🔌

```
[CPU (ALU + CU + Registers)] ←→ [System Bus] ←→ [Main Memory]
                                      ↕
                                  [I/O Devices]
```

**Key Principle:** Instructions and data stored in the same memory (single memory space). Instructions fetched and executed sequentially.

**Von Neumann Bottleneck:** Since data and instructions share the same bus, only ONE can be fetched at a time → bandwidth limitation. This is the fundamental bottleneck of this architecture!

### 1.2 Harvard Architecture 🏛️

> 💡 **Analogy:** Harvard architecture is like having two separate doors into the kitchen — one for recipes (instructions) and one for ingredients (data). Both can come in simultaneously! 🚪🚪

```
[CPU] ←→ [Instruction Memory]    (Instruction Bus)
  ↕
[CPU] ←→ [Data Memory]           (Data Bus)
```

| Feature | Von Neumann | Harvard |
|---|---|---|
| Memory spaces | Single (shared) | Separate instruction + data |
| Buses | Single bus (bottleneck) | Dual buses (parallel fetch) |
| Speed | Slower (bus contention) | Faster (simultaneous access) |
| Flexibility | Instructions can be treated as data | Harder to modify instructions |
| Cost | Less hardware | More hardware (2 memories, 2 buses) |
| Used in | General-purpose (x86, ARM user mode) | DSPs (TMS320), Microcontrollers (AVR, PIC, ARM Cortex-M) |

> 🎯 **GATE Insight:** Most modern processors use a **Modified Harvard Architecture** — separate L1 I-cache and L1 D-cache (Harvard at cache level), but unified main memory (Von Neumann at memory level). This gives the speed of Harvard with the flexibility of Von Neumann!

### 1.3 Stored Program Concept

The revolutionary idea behind modern computers:
1. Both **instructions and data** are stored in main memory as binary patterns
2. Instructions are fetched from memory, decoded, and executed **sequentially**
3. The **Program Counter (PC)** determines which instruction executes next
4. Programs can **modify themselves** (since instructions are just data) — though this is now considered bad practice!

#### Visual: Same Memory Holds Both Code and Data 🧠

```
Main Memory (single shared space)

Address    Content                  Meaning (depends on use)
0x1000  -> 10110000 0001....       instruction (if PC points here)
0x1004  -> 00000000 0010....       instruction
0x1008  -> 00000000 00101010       data value (42)
0x100C  -> 11111111 11110000       data value

CPU context decides interpretation:
- Instruction fetch path -> treat bits as instruction
- Load/store path        -> treat bits as data
```

> 🎯 GATE Insight: This idea made software flexible. New task means loading new bits into memory, not rewiring hardware.

### 1.4 Functional Units of a Computer

```
┌─────────────────────────────────┐
│            CPU                  │
│  ┌──────────┐  ┌────────────┐  │
│  │   CU     │  │    ALU     │  │
│  │ (Control │←→│ (Arithmetic│  │
│  │   Unit)  │  │  Logic Unit│  │
│  └────┬─────┘  └─────┬──────┘  │
│       │               │        │
│  ┌────┴───────────────┴──────┐ │
│  │     Register File         │ │
│  │  (PC, IR, MAR, MDR, ACC) │ │
│  └───────────────────────────┘ │
└──────────────┬──────────────────┘
               │ System Bus
    ┌──────────┼──────────┐
    │          │          │
┌───┴───┐ ┌───┴───┐ ┌───┴───┐
│Input  │ │ Main  │ │Output │
│Devices│ │Memory │ │Devices│
│(Kbd,  │ │(RAM)  │ │(Mon,  │
│Mouse) │ │       │ │Print) │
└───────┘ └───────┘ └───────┘
```

**Control Unit (CU):**
- Fetches instructions from memory
- Decodes the instruction (opcode → control signals)
- Generates timing and control signals for all other units
- Two types: Hardwired CU and Microprogrammed CU

**Arithmetic Logic Unit (ALU):**
- Performs arithmetic operations: +, −, ×, ÷
- Performs logical operations: AND, OR, NOT, XOR
- Sets condition flags: Zero (Z), Carry (C), Overflow (V), Sign/Negative (N)

**Register File:**
- Fastest storage in the computer
- Typical register access time: <1 ns (vs. cache: 1-10 ns, RAM: 50-100 ns)
- General Purpose Registers (GPR): R0-R31 in MIPS, EAX/EBX/ECX/EDX in x86
- Special Purpose Registers: PC, SP, IR, PSW/Flags

### 1.5 Main Memory Organization

> 💡 **Goal:** Understand this clearly: **Address Space** tells "how many locations can be named," while **Memory Size/Memory Space** tells "how much data can be stored."

- The CPU needs data → it must specify where that data is in memory  
- It sends a binary number (like 101010...)  
- Each bit travels on a separate address line

👉 So:
Address lines = wires that carry the address (location number)

> 🔢 **How it works:**  
If there are n address lines:  
Each line carries $0$ or $1$  
Total combinations = $2^n$ addresses  
So the CPU can access $2^n$ memory locations

**If address lines are n:**
- Lowest address = $0$
- Highest address = $2^n - 1$

**For n = 20:**
- Range: 0 to $2^{20}-1$ = 0 to 1,048,575
- In hex: 0x00000 to 0xFFFFF

### Address Space vs Memory Space (Most Important Difference) ⭐⭐⭐

| Term | Meaning | Unit | Depends On |
|---|---|---|---|
| **Address Space** | Total number of distinct addresses CPU can generate | locations | Number of address lines (n) |
| **Memory Size / Memory Space** | Total data storable in memory | bytes/words | Address space x size per location |

**Core formulas:**

$$\text{Address Space (locations)} = 2^n$$

$$\text{Memory Size} = 2^n \times (\text{size of 1 addressable location})$$

### A) What is "Addressable Unit"? ⭐⭐

An address points to one "unit." That unit can be:
- **1 byte** (byte-addressable system, common in modern CPUs)
- **1 word** (word-addressable system, old architectures and some exam problems)

| System type | One address points to | Example |
|---|---|---|
| Byte-addressable | 8 bits (1 byte) | x86, ARM, MIPS (modern usage) |
| Word-addressable | 1 word (say 32 bits = 4 bytes) | Classic/academic architectures |

> ⚠️ **GATE Trap:** Same number of address lines can mean different memory size depending on byte- or word-addressability.

#### 1. Byte-Addressable Memory 📦

Suppose addresses are 0 to 15 (4 address lines).

```
Address  Data (1 byte each)
0x0   -> [B0]
0x1   -> [B1]
0x2   -> [B2]
...
0xF   -> [B15]
```

- Number of addresses = $2^4 = 16$
- Each address stores 1 byte
- Total memory size = $16 \times 1 = 16$ bytes

#### 2. Word-Addressable Memory 📦

Same 4 address lines, but one address now points to **1 word = 4 bytes**:

```
Address  Data (4 bytes each word)
0x0   -> [B3 B2 B1 B0]
0x1   -> [B7 B6 B5 B4]
0x2   -> [B11 B10 B9 B8]
...
0xF   -> [word 15]
```

- Number of addresses = $2^4 = 16$ words
- Each address stores 4 bytes
- Total memory size = $16 \times 4 = 64$ bytes

### 1.6 Endianness
> **Endianness = the order in which bytes are stored in memory**  
When a number needs multiple bytes (like 4 bytes), the system must decide:  
👉 Which byte goes at the lowest memory address?
- **Big-Endian:** MSB stored at lowest address (most significant byte first)
- **Little-Endian:** LSB stored at lowest address (least significant byte first)

#### Visual Byte Layout (same 32-bit value) 🧩

Take value: `0x12345678`

```
Register view (MSB -> LSB):
┌────┬────┬────┬────┐
│ 12 │ 34 │ 56 │ 78 │
└────┴────┴────┴────┘

Stored at base address A:

Big-endian:
Address:   A    A+1  A+2  A+3
Bytes:    [12] [34] [56] [78]

Little-endian:
Address:   A    A+1  A+2  A+3
Bytes:    [78] [56] [34] [12]
```

#### 32-bit Load Reconstruction Rule

- Big-endian read: `M[A] M[A+1] M[A+2] M[A+3]`
- Little-endian read: `M[A+3] M[A+2] M[A+1] M[A]`

**Worked Example:**
Store the 32-bit integer 0xDEADBEEF starting at address 0x100 (assume byte-addressable):

| Address | Big-Endian | Little-Endian |
|---|---|---|
| 0x100 | DE | EF |
| 0x101 | AD | BE |
| 0x102 | BE | AD |
| 0x103 | EF | DE |

Quick reverse check:
- Dump `EF BE AD DE` at 0x100..0x103 means
  - Little-endian value = `0xDEADBEEF`
  - Big-endian value = `0xEFBEADDE`

**Multi-byte String Storage:**
Strings ("Hello") are stored in the same order regardless of endianness because each character is 1 byte.

**Real-World:** x86 = Little-Endian. Network byte order = Big-Endian. ARM = Bi-Endian (configurable).

> ⚠️ **GATE Trap:** Endianness only matters for **multi-byte** data types! Single bytes are stored the same way in both.

### 1.7 System Bus ⭐

> 👉 A bus is a group of physical wires (or traces) that carry address, data, or control signals between CPU, memory, and I/O devices.  
> 💡 **Analogy:** The system bus is like a highway system 🛣️:
> - **Data Bus** = Cargo lanes (carries the goods — data)
> - **Address Bus** = Destination signs (tells where to go)
> - **Control Bus** = Traffic signals (controls flow direction and timing)

- **Data Bus:** Carries actual data bits
  - **Function:** Carries actual data bits (values being read/written)
  - **Width:** 8, 16, 32, 64, 128 bits
  - **Direction:** Bidirectional (CPU ↔ Memory / I/O)
  - **Controls:** How much data moves per transfer
  - **Hardware reality:** A set of parallel wires (metal traces) on the motherboard or inside the CPU. Each wire carries 1 bit.  
  Example: 32-bit data bus = 32 physical wires
- **Address Bus:** Carries location number (which memory/IO address)
  - **Function:** Carries location (which memory or I/O address)
  - **Direction:** Mostly unidirectional (CPU → Memory/I/O)
  - **Capacity:** n address lines → $2^n$ locations
  - **Hardware reality:** Also a bundle of wires. Each wire represents 1 bit of the address.
  Example: 20 address lines = 20 wires
  - **These wires connect:** CPU → RAM chips → I/O devices
- **Control Bus:** Carries "what operation to do"
  - **Function:** Carries control signals (what operation to perform)
  - **Signals include:** MemRead, MemWrite, IORead, IOWrite, Clock, Interrupt, Reset
  - **Hardware reality:** Not a single bus, but a collection of individual control lines (wires)  
  Some are:  
  1-bit signals (e.g., Read = 1, Write = 0)  
  Some are timing signals (clock pulses)

#### Full Bus Transaction Diagram (Read Cycle) 🧭

```
CPU                                Memory
 |                                   |
 |-- Address bus: 0x1000 ----------->|   (which location?)
 |-- Control bus: MemRead=1 -------->|   (read request)
 |                                   |
 |<===== Data bus: 0xABCD1234 =======|   (data returned)
 |                                   |
 |-- Control bus: MemRead=0 -------->|   (cycle ends)
```

#### Full Bus Transaction Diagram (Write Cycle) 🧭

```
CPU                                Memory
 |                                   |
 |-- Address bus: 0x2000 ----------->|
 |==== Data bus: 0x55AA55AA ========>|
 |-- Control bus: MemWrite=1 ------->|   (store this data)
 |                                   |
 |-- Control bus: MemWrite=0 ------->|   (cycle ends)
```

#### Why Tri-state Buffers Are Needed ⚙️

Only one device should drive the data bus at a time.

```
If CPU and Memory both drive bus simultaneously:
  CPU drives 1, Memory drives 0 on same line -> bus contention -> error/power spike

So each device uses tri-state output:
  0, 1, or Z (high impedance / disconnected)
```

#### Bus Width vs Address Width (Common Confusion) ⭐⭐

| Property | Controlled by | Affects |
|---|---|---|
| Data bus width | Number of data lines | bytes transferred per cycle |
| Address bus width | Number of address lines | maximum addressable memory |

**Example:**
- 32-bit data bus, 20-bit address bus
- Addressable memory (byte-addressable) = 2^20 bytes = 1 MB
- Data per transfer = 32 bits = 4 bytes

#### Bus Bandwidth Formula

$$\text{Bandwidth} = \frac{\text{Data bus width (bits)}}{8} \times \text{Bus frequency}$$

**Example:** 64-bit bus @ 800 MHz

$$\text{BW} = (64/8) \times 800\times10^6 = 8\times800\times10^6 = 6.4\times10^9\,\text{B/s} = 6.4\,\text{GB/s}$$

> ⚠️ GATE Trap: Do not use CPU clock unless the question explicitly says bus runs at CPU clock. Many systems use different bus frequency.

### 1.8 Memory Interfacing ⭐⭐

Memory interfacing means connecting small memory chips to build required total memory.

#### Step-by-step Interfacing Method 🧩

1. **Capacity expansion:** Need more locations? Add chips in banks.
2. **Word-size expansion:** Need wider data word? Put chips in parallel.
3. **Chip select generation:** Use higher address bits + decoder.

#### Interfacing Diagram (4K x 8 chips -> 16K x 8 memory)

```
CPU Address: A13 .... A12 A11 .... A0
         |      \___________
         |                  \
       +---v---+               \--> all chips A11..A0
       | 2:4   |
       |decoder|
       +---+---+
         | CS0  CS1  CS2  CS3
         |   |    |    |    |
       +---v-+ +---v-+ +---v-+ +---v-+
       |4Kx8 | |4Kx8 | |4Kx8 | |4Kx8 |
       |Chip0| |Chip1| |Chip2| |Chip3|
       +-----+ +-----+ +-----+ +-----+
         \      \      \      \
          +------+------ +------+----> Data bus D7..D0 (shared)
```

#### Address Mapping Table

```
Address lines A13-A0:
- A11-A0 → All chips (address within chip)
- A13-A12 → 2:4 Decoder → CS0, CS1, CS2, CS3

| A13 | A12 | Active Chip | Address Range |
|-----|-----|------------|---------------|
|  0  |  0  |   Chip 0   | 0000-0FFF     |
|  0  |  1  |   Chip 1   | 1000-1FFF     |
|  1  |  0  |   Chip 2   | 2000-2FFF     |
|  1  |  1  |   Chip 3   | 3000-3FFF     |
```

#### How CPU accesses one byte here

Suppose CPU accesses address 0x2A35.
- Binary high bits A13A12 = 10 -> Chip2 selected.
- Low 12 bits A11..A0 = A35 -> internal address within Chip2.
- Data appears on D7..D0.

So address is split as:

$$\text{CPU address} = [\text{chip select bits}] + [\text{offset inside chip}]$$

#### Word-size Expansion Example (for understanding) ⭐

Need 4K x 16 using 4K x 8 chips:
- Keep same address lines for both chips.
- One chip gives lower byte D7..D0.
- Second chip gives upper byte D15..D8.

```
          A11..A0 -> both chips

Chip L (4Kx8) -> D7..D0
Chip H (4Kx8) -> D15..D8

Together => 4K x 16
```

> 🎯 GATE Trick: Capacity expansion changes number of addresses. Word-size expansion changes bits per address.

**Memory Bandwidth vs Latency:**
| Metric | Definition | Analogy |
|---|---|---|
| **Latency** | Time for a single memory access | How long to get one item from warehouse |
| **Bandwidth** | Data transferred per unit time | How many items per hour the warehouse ships |

#### Chip Select Equation (logic view) ⭐

For 4 chips selected by A13,A12:

$$CS_0 = \overline{A13}\,\overline{A12},\quad CS_1 = \overline{A13}A12,\quad CS_2 = A13\overline{A12},\quad CS_3 = A13A12$$

Only one CS is active at a time.

### 1.9 Instruction Execution Cycle ⭐⭐⭐

```
┌─────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  FETCH   │ →  │  DECODE  │ →  │ EXECUTE  │ →  │ MEMORY   │ →  │WRITE BACK│
└─────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

#### Beginner View: What exactly happens when one instruction runs?

```
Program in memory -> CPU picks one instruction -> executes it -> moves to next
```

Detailed flow:

```
         +----------------------+
         | 1) FETCH             |
         | IR <- M[PC]          |
         | PC <- PC + 4         |
         +----------+-----------+
                    |
                    v
         +----------------------+
         | 2) DECODE            |
         | opcode decode        |
         | read source regs     |
         +----------+-----------+
                    |
                    v
         +----------------------+
         | 3) EXECUTE           |
         | ALU op / address calc|
         +----------+-----------+
                    |
                    v
         +----------------------+
         | 4) MEMORY (if needed)|
         | load/store only      |
         +----------+-----------+
                    |
                    v
         +----------------------+
         | 5) WRITE BACK        |
         | write result to reg  |
         +----------------------+
```

#### Micro-operations (RTL) per stage

```
T0: MAR <- PC
T1: MDR <- M[MAR]
T2: IR <- MDR, PC <- PC + 4
T3: Decode opcode, read registers
T4: ALU operation / effective address
T5: Memory access if load/store
T6: Register write-back
```

#### Example Walkthrough: ADD R1, R2, R3

```
Meaning: R1 <- R2 + R3
```

1. Fetch instruction bits from memory using PC.
2. Decode says "ADD, src=R2,R3, dest=R1".
3. Read R2 and R3 from register file.
4. ALU computes sum.
5. Write result to R1.
6. PC already points to next instruction.

#### Example Walkthrough: LW R1, 8(R2)

1. Fetch and decode.
2. Read base register R2.
3. Compute effective address: EA = R2 + 8.
4. Read memory at EA.
5. Write fetched data into R1.

#### Why this matters for GATE

Questions ask:
- Which stage causes stalls?
- Which instruction uses MEM stage?
- How many cycles in single-cycle/multi-cycle/pipeline?

> 🎯 GATE Trap: Not all instructions use all 5 stages equally. For example, many ALU instructions do not need data-memory access.

### 1.10 Computer Performance Metrics ⭐⭐

| Metric | Formula | What it measures |
|---|---|---|
| **Clock Rate (f)** | f = 1/T_clock | Cycles per second (Hz) |
| **CPI** | CPI = CPU_cycles / IC | Average cycles per instruction |
| **CPU Time** | T = IC × CPI / f | Total execution time |
| **MIPS** | f / (CPI × 10⁶) | Million instructions per second |
| **MFLOPS** | FP_operations / (T × 10⁶) | Million FP ops per second |
| **Throughput** | Instructions / Time | Work done per unit time |
| **IPC** | 1 / CPI | Instructions per cycle (inverse of CPI) |

**CPI with Instruction Mix:**
$$CPI = \sum_{i=1}^{n} CPI_i \times f_i$$
where $f_i$ = fraction of instructions of type i, $CPI_i$ = cycles for that type

**Worked Example:**
| Instruction Type | Frequency | CPI |
|---|---|---|
| ALU | 40% | 1 |
| Load | 30% | 4 |
| Store | 10% | 3 |
| Branch | 20% | 2 |

CPI = 0.4×1 + 0.3×4 + 0.1×3 + 0.2×2 = 0.4+1.2+0.3+0.4 = **2.3**

For a 2 GHz processor running 10⁹ instructions:
- CPU Time = 10⁹ × 2.3 / (2×10⁹) = **1.15 seconds**
- MIPS = 2000/2.3 = **870 MIPS**

#### Visual Relationship Map 📈

```
Clock Rate (f)  -> affects cycle time (1/f)
Instruction Count (IC)
Average CPI
  |
  v
CPU Time = IC x CPI / f
```

#### Performance Triangle (easy memory rule)

```
   Faster Program
    /         \
   Fewer Instructions   Lower CPI
    \         /
    Higher Clock
```

Any improvement usually targets one or more of these three knobs.

#### IPC vs CPI (very important)

$$IPC = \frac{1}{CPI}$$

- High IPC means more instructions completed each cycle.
- Lower CPI means better performance.

Example:
- CPI = 2 -> IPC = 0.5
- CPI = 0.5 -> IPC = 2

#### A complete quick compare example ✅

Program has IC = 2 x 10^9

Processor A: f = 2 GHz, CPI = 1.8

$$T_A = \frac{2\times10^9\times1.8}{2\times10^9} = 1.8s$$

Processor B: f = 3 GHz, CPI = 2.4

$$T_B = \frac{2\times10^9\times2.4}{3\times10^9} = 1.6s$$

So B is faster despite higher CPI, because clock is much higher.

> 🎯 GATE Trap: Never compare processors using only clock rate or only MIPS. Always compute execution time.

#### Throughput vs Latency

| Term | Meaning | Example |
|---|---|---|
| Latency | time for one task | one program finishes in 2 s |
| Throughput | tasks completed per second | 50 jobs/sec in server |

You can improve throughput (pipeline, multicore) without reducing single-job latency much.

### 1.11 Register Types and Classification ⭐

#### Register Organization Diagram 🧱

```
                 CPU
┌──────────────────────────────────────┐
│ General Registers: R0 R1 R2 ...     │
│                                      │
│ Special Registers:                   │
│  PC   -> next instruction address    │
│  IR   -> current instruction bits    │
│  MAR  -> memory address              │
│  MDR  -> memory data                 │
│  SP   -> stack top                   │
│  PSW  -> flags (Z,C,V,N,...)         │
└──────────────────────────────────────┘
```

| Register | Width | Purpose |
|---|---|---|
| **PC** (Program Counter) | Word | Address of next instruction |
| **IR** (Instruction Register) | Word | Holds current instruction |
| **MAR** (Memory Address Register) | Address width | Address for memory access |
| **MDR/MBR** (Memory Data/Buffer) | Data bus width | Data to/from memory |
| **SP** (Stack Pointer) | Word | Top of stack address |
| **ACC** (Accumulator) | Word | Default arithmetic result storage |
| **PSW/FLAG** (Status Word) | Word | Condition codes: Z, C, V, N, I |

**Flag Bits Explained:**
| Flag | Name | Set When |
|---|---|---|
| Z | Zero | Result = 0 |
| C | Carry | Unsigned overflow/borrow |
| V | Overflow | Signed overflow (result doesn't fit) |
| N/S | Negative/Sign | Result MSB = 1 |
| I | Interrupt | Interrupt enable/disable |
| P | Parity | Even number of 1s in result |

Flag-generation view:

```
ALU Result -> PSW flags

Result == 0        -> Z = 1
Carry out from MSB -> C = 1
Signed overflow    -> V = 1
MSB of result = 1  -> N = 1
```

**Example — Flag Setting:**
ADD of 127 + 1 in 8-bit signed (+127 = 01111111, +1 = 00000001):
```
  01111111
+ 00000001
----------
  10000000  = -128 in signed (OVERFLOW!)
```
- Z = 0 (result ≠ 0)
- C = 0 (no unsigned carry)
- V = **1** (signed overflow: positive + positive = negative!)
- N = **1** (MSB = 1)

### 1.12 Micro-Operations and RTL ⭐⭐

**Register Transfer Language (RTL)** describes operations at the micro-level.

#### Datapath View of a Micro-Operation

```
R1 ---->\
     \        ┌─────────┐
      +------>│   ALU   │-----> Destination Register
     /        └─────────┘
R2 ---->/             ^
           |
        Control Unit
      (select add/sub/and/shift)
```

**Basic RTL Notation:**
| RTL | Meaning |
|---|---|
| R1 ← R2 | Transfer R2 contents to R1 |
| R1 ← R1 + R2 | Add R2 to R1 |
| M[AR] ← R1 | Store R1 to memory at address in AR |
| R1 ← M[AR] | Load from memory at address AR |
| If (Z=1): PC ← AR | Conditional transfer (branch if zero) |
| R1 ← shl(R1) | Shift R1 left by 1 |

**Complete RTL for Common Instructions:**

**ADD R1, R2, R3 (R-type):**
```
T₀: MAR ← PC
T₁: MDR ← M[MAR], PC ← PC+4
T₂: IR ← MDR
T₃: A ← R[rs], B ← R[rt]
T₄: ALU_out ← A + B
T₅: R[rd] ← ALU_out
```

**LOAD R1, offset(R2) (Memory access):**
```
T₀: MAR ← PC
T₁: MDR ← M[MAR], PC ← PC+4
T₂: IR ← MDR
T₃: A ← R[rs]
T₄: ALU_out ← A + sign_ext(offset)
T₅: MAR ← ALU_out
T₆: MDR ← M[MAR]
T₇: R[rt] ← MDR
```

**BEQ R1, R2, offset (Branch):**
```
T₀: MAR ← PC
T₁: MDR ← M[MAR], PC ← PC+4
T₂: IR ← MDR
T₃: A ← R[rs], B ← R[rt]
T₄: ALU_out ← A − B, Target ← PC + sign_ext(offset)×4
T₅: If (Z=1): PC ← Target
```

### 1.13 Bus Arbitration Methods ⭐

When multiple devices want to use the bus simultaneously, arbitration is needed.

| Method | Type | Description | Pros | Cons |
|---|---|---|---|---|
| **Daisy Chain** | Centralized | Priority signal passes through devices in chain | Simple, cheap | Fixed priority, single point of failure |
| **Centralized Parallel** | Centralized | Each device has separate request line to arbiter | Flexible priority | More wires |
| **Distributed/Self-Selection** | Distributed | Devices resolve among themselves | No single point of failure | Complex logic |
| **Collision Detection** | Distributed | Detect collision, backoff and retry | Simple | Non-deterministic latency (Ethernet) |

**Daisy Chain Bus Arbitration:**
```
                  Bus Grant signal
Arbiter → Device 1 → Device 2 → Device 3 → ...
                  ↑              ↑              ↑
         (if not requesting,  pass grant along)
```
- Leftmost device has highest priority
- Device intercepts grant if it needs bus; otherwise passes it down
- **Problem:** Low-priority devices may starve!

**Centralized Parallel Arbitration (visual):**

```
Req1 ----\
Req2 -----\
Req3 ------> [Central Arbiter] ---> Grant1/Grant2/Grant3
Req4 -----/

Arbiter picks one requester based on policy.
```

### 1.14 Synchronous vs Asynchronous Bus ⭐

#### Timing Diagram (Synchronous) ⏱️

```
Clock:   _|-|_|-|_|-|_|-|_
Address: === AAAAAA ======
Control: === READ  =======
Data:        ----< DDDD >--
```

#### Timing Diagram (Asynchronous Handshake) 🤝

```
Master Req:   ____|------|____
Slave Ack:    ________|----|__
Data Valid:      < DDDD >
```

| Feature | Synchronous | Asynchronous |
|---|---|---|
| Clock | Shared clock signal | No clock (handshaking) |
| Speed | Faster (for matched speeds) | Handles different speeds |
| Design | Simpler | More complex |
| Bus length | Limited (clock skew) | Longer distances possible |
| Device compatibility | All must match clock | Any speed device |
| Example | PCI, Front Side Bus | USB, SCSI handshake |

---

## 2. Data Representation & Floating Point 🔢

### 2.1 IEEE 754 Floating Point ⭐⭐⭐

#### Quick Visual 🧩

```
IEEE 754 (single precision: 32 bits)

 bit31      bit30........23       bit22..................0
┌───────┬─────────────────────┬────────────────────────────┐
│ Sign  │ Exponent (8 bits)   │ Fraction/Mantissa (23 bits)│
└───────┴─────────────────────┴────────────────────────────┘

Value = (-1)^S x (1.Fraction) x 2^(Exponent - 127)
```

| Format | Sign | Exponent | Mantissa | Bias |
|--------|------|----------|----------|------|
| Single (32-bit) | 1 | 8 | 23 | 127 |
| Double (64-bit) | 1 | 11 | 52 | 1023 |
| Half (16-bit) | 1 | 5 | 10 | 15 |

**Value = (−1)ˢ × 1.M × 2^(E−Bias)** (normalized)

**Special Values:**
| Exponent | Mantissa | Meaning |
|----------|----------|---------|
| 0 | 0 | ±0 |
| 0 | ≠0 | Denormalized: (−1)ˢ × 0.M × 2^(1−Bias) |
| All 1s | 0 | ±∞ |
| All 1s | ≠0 | NaN |

**Single Precision Range:**
- Smallest positive normalized: 1.0 × 2⁻¹²⁶ ≈ 1.18 × 10⁻³⁸
- Largest positive: (2−2⁻²³) × 2¹²⁷ ≈ 3.4 × 10³⁸
- Smallest positive denormalized: 2⁻²³ × 2⁻¹²⁶ = 2⁻¹⁴⁹

**Example:** Convert −6.75 to IEEE 754 single precision:
1. Sign: 1 (negative)
2. 6.75 = 110.11₂ = 1.1011 × 2²
3. Exponent: 2 + 127 = 129 = 10000001
4. Mantissa: 10110000...0 (23 bits)
5. Result: **1 10000001 10110000000000000000000** = 0xC0D80000

**Example 2:** Convert 0.1 to IEEE 754 single precision:
1. Sign: 0 (positive)
2. 0.1₁₀ = 0.000110011001100110011...₂ (repeating! → 0.1 cannot be represented exactly)
3. = 1.10011001100110011001101 × 2⁻⁴ (rounded)
4. Exponent: −4 + 127 = 123 = 01111011
5. Result: **0 01111011 10011001100110011001101**

> ⚠️ **GATE Insight:** This is why 0.1 + 0.2 ≠ 0.3 in floating point! 0.1 has no exact binary representation.

**Example 3:** What decimal number does 0 10000100 01100000000000000000000 represent?
1. Sign = 0 → positive
2. Exponent = 10000100₂ = 132, so E = 132 − 127 = 5
3. Mantissa = 1.011₂ (add hidden 1)
4. Value = 1.011 × 2⁵ = 101100₂ = 44

**IEEE 754 Precision Comparison:**

| Property | Half (16-bit) | Single (32-bit) | Double (64-bit) |
|---|---|---|---|
| Sign bits | 1 | 1 | 1 |
| Exponent bits | 5 | 8 | 11 |
| Mantissa bits | 10 | 23 | 52 |
| Bias | 15 | 127 | 1023 |
| Max exponent | 15 | 127 | 1023 |
| Min exponent | −14 | −126 | −1022 |
| Decimal digits precision | ~3.3 | ~7.2 | ~15.9 |
| Smallest normalized | 2⁻¹⁴ | 2⁻¹²⁶ | 2⁻¹⁰²² |
| Largest | 65504 | ~3.4×10³⁸ | ~1.8×10³⁰⁸ |

**Rounding Modes in IEEE 754:**
| Mode | Rule | Example (to integer) |
|---|---|---|
| Round to nearest even | Default; tie → even | 2.5→2, 3.5→4 |
| Round toward +∞ | Always round up | 2.1→3, −2.9→−2 |
| Round toward −∞ | Always round down | 2.9→2, −2.1→−3 |
| Round toward 0 | Truncate | 2.9→2, −2.9→−2 |

### 2.2 Signed Number Representation ⭐⭐

#### Quick Visual 🧩

```
8-bit number layouts

Unsigned:         [ b7 b6 b5 b4 b3 b2 b1 b0 ]
Sign-magnitude:   [ S  m6 m5 m4 m3 m2 m1 m0 ]
1's complement:   negative = bitwise NOT(positive)
2's complement:   negative = NOT(positive) + 1
```

| Representation | For n=8 bits | Range | Zeros |
|---|---|---|---|
| **Unsigned** | 0 to 255 | 0 to 2ⁿ−1 | One zero |
| **Sign-Magnitude** | −127 to +127 | −(2ⁿ⁻¹−1) to +(2ⁿ⁻¹−1) | Two zeros (+0, −0) |
| **1's Complement** | −127 to +127 | −(2ⁿ⁻¹−1) to +(2ⁿ⁻¹−1) | Two zeros |
| **2's Complement** | −128 to +127 | −2ⁿ⁻¹ to +(2ⁿ⁻¹−1) | One zero |

**2's Complement Operations:**
- Negate: Flip all bits + 1
- Shortcut: Copy from right until first 1, then flip remaining bits
- Sign extension: Copy the sign bit to fill new positions
  - Example: 1011 (4-bit, −5) → 1111 1011 (8-bit, still −5)

**Overflow Detection (2's Complement):**
- **Addition:** Overflow when both operands have same sign but result has different sign
- V = Cₙ₋₁ ⊕ Cₙ (carry INTO MSB XOR carry OUT of MSB)
- Adding positive + negative → NEVER overflows!

**Worked Example — Overflow:**
In 4-bit 2's complement: 5 + 4 = ?
- 0101 + 0100 = 1001 = −7 (WRONG! Overflow occurred)
- Both positive, result negative → overflow detected!

### 2.3 Fixed-Point Representation ⭐

#### Quick Visual 🧩

```
Qm.n format (example Q4.4):

┌──── integer part (4 bits) ────┬── fractional part (4 bits) ──┐
│ b7 b6 b5 b4                    │ b3 b2 b1 b0                   │
└────────────────────────────────┴───────────────────────────────┘

Resolution = 2^-n
```

| Type | Integer bits (m) | Fraction bits (n) | Range | Resolution |
|---|---|---|---|---|
| Unsigned | m | n | 0 to 2ᵐ − 2⁻ⁿ | 2⁻ⁿ |
| Signed (2's comp) | m | n | −2ᵐ⁻¹ to 2ᵐ⁻¹ − 2⁻ⁿ | 2⁻ⁿ |

**Used in DSP, embedded systems, FPGAs** where floating point hardware is expensive.

### 2.4 Booth's Multiplication Algorithm ⭐⭐⭐

> 🏭 **Most asked COA numerical in GATE!** Works for signed (2's complement) multiplication.

**Algorithm (Multiply M × Q, n-bit):**
1. Initialize: A=0, Q=multiplicand, Q₋₁=0, M=multiplier, count=n
2. Check Q₀Q₋₁:
   - **10 → A = A − M** (subtract)
   - **01 → A = A + M** (add)
   - **00, 11 → no operation**
3. **Arithmetic right shift** AQQ₋₁ (preserve sign bit!)
4. Decrement count; repeat until count=0
5. Result: AQ (2n bits)

**Example:** 3 × (−4) using 4-bit Booth's:
- M=0011 (+3), Q=1100 (−4), A=0000, Q₋₁=0

| Step | A | Q | Q₋₁ | Action |
|---|---|---|---|---|
| Init | 0000 | 1100 | 0 | Q₀Q₋₁=00→NOP |
| ASR | 0000 | 0110 | 0 | |
| 2 | 0000 | 0110 | 0 | Q₀Q₋₁=00→NOP |
| ASR | 0000 | 0011 | 0 | |
| 3 | 0000 | 0011 | 0 | Q₀Q₋₁=10→A=A−M |
| | 1101 | 0011 | 0 | A=0000−0011=1101 |
| ASR | 1110 | 1001 | 1 | |
| 4 | 1110 | 1001 | 1 | Q₀Q₋₁=11→NOP |
| ASR | 1111 | 0100 | 1 | |

Result: AQ = 1111 0100 = −12 ✅ (3 × −4 = −12)

> ⚠️ **GATE Trap:** Use ARITHMETIC right shift (sign bit preserved), not LOGICAL!

**Booth's Recoding Table:**
| Q₀ | Q₋₁ | Action | Meaning |
|---|---|---|---|
| 0 | 0 | No operation | Middle of 0s block |
| 0 | 1 | A = A + M | End of 1s block |
| 1 | 0 | A = A − M | Beginning of 1s block |
| 1 | 1 | No operation | Middle of 1s block |

**When Does Booth's Outperform Normal Multiplication?**
- When the multiplier has long runs of 0s or 1s (fewer add/subtract operations)
- Worst case: alternating 0s and 1s (e.g., 0101...01) → every step requires operation

### 2.5 Modified Booth's Algorithm (Radix-4) ⭐⭐

> 💡 **Booth's does 1 bit per step (n steps). Modified Booth's does 2 bits per step (n/2 steps) — TWICE as fast!**

**Recoding (examine 3 bits at a time: Q₂ᵢ₊₁, Q₂ᵢ, Q₂ᵢ₋₁):**

| Q₂ᵢ₊₁ | Q₂ᵢ | Q₂ᵢ₋₁ | Operation |
|---|---|---|---|
| 0 | 0 | 0 | +0 (no operation) |
| 0 | 0 | 1 | +M |
| 0 | 1 | 0 | +M |
| 0 | 1 | 1 | +2M |
| 1 | 0 | 0 | −2M |
| 1 | 0 | 1 | −M |
| 1 | 1 | 0 | −M |
| 1 | 1 | 1 | +0 (no operation) |

After each operation, **arithmetic right shift by 2 positions**.

### 2.6 Array Multiplier ⭐

> 💡 **Analogy:** Like doing long multiplication by hand — each partial product is a row, and we add them all up!

For multiplying two n-bit numbers:
- Generates n partial products using AND gates
- Each partial product is shifted by 1 position
- Uses (n−1) rows of n-bit adders to sum partial products

**Hardware:**
- AND gates: n² (one per bit-pair)
- Half adders: n
- Full adders: n(n−2)
- **Delay:** (2n−2) gate delays (critical path through adders)

**Comparison of Multiplication Methods:**
| Method | Steps | Hardware | Speed |
|---|---|---|---|
| Sequential (shift-add) | n clock cycles | 1 adder | Slowest |
| Booth's | n clock cycles | 1 adder + subtractor | Same as sequential |
| Modified Booth's | n/2 clock cycles | More complex | 2x faster |
| Array multiplier | Combinational | n² area | Fast (no clock) |
| Wallace tree | Combinational | Complex | Fastest combinational |

### 2.7 Restoring Division Algorithm ⭐⭐

#### Quick Visual 🧩

```
Restoring division loop:

Start A,Q
  |
  v
Left shift AQ
  |
  v
A = A - M
  |
  +--> A < 0 ? -- Yes --> A = A + M (restore), Q0=0
  |                         |
  |                         v
  +----------- No --------> Q0=1
                            |
                            v
                        repeat n times
```

1. Initialize: A=0, Q=Dividend, M=Divisor, count=n
2. **Left shift AQ** (combined)
3. A = A − M
4. If A < 0: **restore** A = A + M, Q₀=0
5. If A ≥ 0: Q₀=1
6. Decrement count; repeat
7. Result: Q=Quotient, A=Remainder

### 2.8 Non-Restoring Division Algorithm ⭐⭐

> 🎯 **Faster than restoring — avoids the restore step!**

1. Initialize same as above
2. If A ≥ 0: Left shift AQ; A = A − M
3. If A < 0: Left shift AQ; A = A + M
4. If A ≥ 0: Q₀=1; If A < 0: Q₀=0
5. Repeat n times
6. **If final A < 0: restore** by A = A + M

**Key Difference:** Restoring always subtracts, then restores if negative. Non-restoring alternates add/subtract based on sign of A.

**Worked Example — Restoring Division: 7 ÷ 3 (4-bit):**
- A=0000, Q=0111 (dividend=7), M=0011 (divisor=3), count=4

| Step | Action | A | Q | Q₀ |
|---|---|---|---|---|
| Init | | 0000 | 0111 | |
| 1 | Left shift AQ | 0000 | 111_ | |
| | A = A − M | 1101 | 1110 | |
| | A < 0: Restore | 0000 | 1110 | Q₀=0 |
| 2 | Left shift AQ | 0001 | 110_ | |
| | A = A − M | 1110 | 1100 | |
| | A < 0: Restore | 0001 | 1100 | Q₀=0 |
| 3 | Left shift AQ | 0011 | 100_ | |
| | A = A − M | 0000 | 1000 | |
| | A ≥ 0: Keep | 0000 | 1001 | Q₀=1 |
| 4 | Left shift AQ | 0001 | 001_ | |
| | A = A − M | 1110 | 0010 | |
| | A < 0: Restore | 0001 | 0010 | Q₀=0 |

**Result:** Q = 0010 (quotient = 2), A = 0001 (remainder = 1) ✅
**Verify:** 7 = 3 × 2 + 1 ✅

**Worked Example — Non-Restoring Division: 7 ÷ 3 (4-bit):**
- A=0000, Q=0111, M=0011, count=4

| Step | A sign | Action | A | Q |
|---|---|---|---|---|
| Init | ≥0 | | 0000 | 0111 |
| 1 | ≥0 | SHL, A=A−M | 1101 | 1110, Q₀=0 |
| 2 | <0 | SHL, A=A+M | 1101 | 1100 → SHL → 1011 100_ → A+M → 1110 | Q₀=0 |
| 3 | <0 | SHL, A=A+M → A becomes 0000 | Q₀=1 |
| 4 | ≥0 | SHL, A=A−M → A becomes 1110 | Q₀=0 |
| Final | <0 | Restore: A=A+M | 0001 | 0010 |

**Result:** Q = 0010, A = 0001 ✅ (Same answer, fewer operations)

**Division Methods Comparison:**
| Feature | Restoring | Non-Restoring |
|---|---|---|
| Operations per step | Subtract + (possibly restore) | Only add OR subtract |
| Max operations | 2n (n subtracts + n restores) | n + 1 (n ops + final restore) |
| Speed | Slower | Faster |
| Final restore needed? | No | Yes (if A < 0) |
| GATE frequency | Medium | High |

---

### 2.9 Floating-Point Addition & Subtraction Algorithm ⭐⭐⭐

> 🏭 **GATE loves asking step-by-step FP operations.** This is the hardware algorithm inside the FPU.

**Algorithm (A ± B in IEEE 754):**

```
Step 1: Compare Exponents
   ┌──────────────────────────┐
   │ Find d = |Exp_A − Exp_B| │
   └──────────┬───────────────┘
              ▼
Step 2: Align Mantissas
   ┌─────────────────────────────────────┐
   │ Right-shift smaller mantissa by d   │
   │ (shift introduces guard/round bits) │
   └──────────┬──────────────────────────┘
              ▼
Step 3: Add/Subtract Mantissas
   ┌───────────────────────────────────┐
   │ If same sign → Add mantissas      │
   │ If diff sign → Subtract mantissas │
   └──────────┬────────────────────────┘
              ▼
Step 4: Normalize Result
   ┌──────────────────────────────────────┐
   │ If mantissa ≥ 2.0 → right-shift,    │
   │    increment exponent                │
   │ If mantissa < 1.0 → left-shift,     │
   │    decrement exponent (repeat)       │
   │ Check for overflow/underflow         │
   └──────────┬───────────────────────────┘
              ▼
Step 5: Round Result
   ┌─────────────────────────────────┐
   │ Apply rounding mode (default:    │
   │ round to nearest even)           │
   │ May require re-normalization!    │
   └──────────────────────────────────┘
```

**Worked Example: 1.5 + 0.375 in IEEE 754 single precision** 🔢

1. 1.5₁₀ = 1.1₂ × 2⁰ → Exp = 127, Mantissa = 10000...
2. 0.375₁₀ = 1.1₂ × 2⁻² → Exp = 125, Mantissa = 10000...
3. **Exponent difference** d = 127 − 125 = 2
4. **Align:** Right-shift 0.375's mantissa by 2: 1.1₂ → 0.011₂ (now both have Exp = 127)
5. **Add mantissas:**
   ```
     1.100 000 000...
   + 0.011 000 000...
   ─────────────────
     1.111 000 000...
   ```
6. **Already normalized** (1.111 × 2⁰ = 1.875₁₀)
7. Result = 1.875 ✅ (1.5 + 0.375 = 1.875)

**Worked Example: 1.0 − 0.9375 in IEEE 754** 🔢

1. 1.0₁₀ = 1.000 × 2⁰, Exp = 127
2. 0.9375₁₀ = 1.111 × 2⁻¹, Exp = 126
3. d = 1, align 0.9375: 1.111 → 0.1111
4. **Subtract:**
   ```
     1.0000
   − 0.1111
   ────────
     0.0001  = 1.000 × 2⁻⁴ (after normalization)
   ```
5. **Left-shift 4 times**, exponent becomes 127 − 4 = 123
6. Result = 1.0 × 2⁻⁴ = 0.0625₁₀ ✅

> ⚠️ **GATE Trap:** Subtraction of nearly equal numbers causes **catastrophic cancellation** — many leading bits cancel, losing significant precision! In this example, we went from 23 mantissa bits to effectively ~19 significant bits.

**FP Addition Hardware Block Diagram:**
```
┌─────────┐    ┌─────────┐
│ Exp_A   │    │ Exp_B   │
└────┬────┘    └────┬────┘
     └───────┬──────┘
         ┌───▼───┐
         │Compare│ → exponent difference d
         │ & Swap│ → select smaller
         └───┬───┘
             │
     ┌───────▼───────┐
     │ Right Shift   │ ← shift smaller mantissa by d
     │ Alignment     │
     └───────┬───────┘
             │
     ┌───────▼───────┐
     │  Adder/Sub    │ ← add or subtract mantissas
     └───────┬───────┘
             │
     ┌───────▼───────┐
     │ Normalizer    │ ← count leading zeros, shift
     │ (LZD + Shift) │
     └───────┬───────┘
             │
     ┌───────▼───────┐
     │  Rounder      │ ← apply rounding
     └───────┬───────┘
             ▼
        Result (S, E, M)
```

### 2.10 Floating-Point Multiplication Algorithm ⭐⭐

#### Quick Visual 🧩

```
FP Multiply datapath:

Sign:      S = SA XOR SB
Exponent:  E = EA + EB - Bias
Mantissa:  M = MA x MB
        |
        v
      Normalize -> Round -> Pack result
```

**Algorithm (A × B):**
1. **Sign** = Sign_A ⊕ Sign_B (XOR)
2. **Exponent** = Exp_A + Exp_B − Bias
3. **Mantissa** = Mantissa_A × Mantissa_B (unsigned integer multiplication)
4. **Normalize:** If product ≥ 2.0, right-shift mantissa, increment exponent
5. **Round** and check for overflow/underflow

**Worked Example: 2.5 × 1.5 in IEEE 754** 🔢

1. 2.5 = 1.01 × 2¹ (Exp = 128), 1.5 = 1.1 × 2⁰ (Exp = 127)
2. Sign = 0 ⊕ 0 = 0 (positive)
3. Exponent = 128 + 127 − 127 = 128
4. Mantissa: 1.01 × 1.1 = (101 × 11) >> fixed = 1.111 (or just binary multiply)
   ```
       1.01
     × 1.1
     ─────
       101
      101
     ─────
     1.111
   ```
5. 1.111 × 2¹ = 11.11₂ = 3.75₁₀ ✅ (2.5 × 1.5 = 3.75)
6. Product = 1.111, already normalized. Exponent = 128.

> 🎯 **GATE Trick for FP multiply:** Just multiply mantissas like integers, add exponents, subtract one bias. Easier than addition!

### 2.11 Floating-Point Division Algorithm ⭐⭐

#### Quick Visual 🧩

```
FP Divide datapath:

Sign:      S = SA XOR SB
Exponent:  E = EA - EB + Bias
Mantissa:  M = MA / MB
        |
        v
      Normalize -> Round -> Pack result
```

**Algorithm (A ÷ B):**
1. **Sign** = Sign_A ⊕ Sign_B
2. **Exponent** = Exp_A − Exp_B + Bias
3. **Mantissa** = Mantissa_A ÷ Mantissa_B (using restoring/non-restoring/SRT division)
4. **Normalize** and **Round**

**SRT Division (Sweeney-Robertson-Tocher):**
- Used in real processors (Intel, AMD)
- Produces 1-2 quotient bits per cycle
- Uses lookup table to estimate quotient digits
- The Pentium FDIV bug (1994) was caused by 5 missing entries in SRT lookup table! 🐛

> 🎯 **GATE Insight:** FP division is the slowest FP operation (20-30+ cycles vs 3-5 for add, 4-8 for multiply).

### 2.12 Guard, Round, and Sticky Bits ⭐⭐

#### Quick Visual 🧩

```
Extended mantissa bits during computation:

1 . f22 f21 ... f1 f0 | G | R | tail bits...
                 \___ OR ___/
                   |
                   v
                   S (Sticky)
```

> 💡 During FP arithmetic, intermediate results may have more precision than the final format. These extra bits ensure correct rounding.

| Bit | Position | Purpose |
|---|---|---|
| **Guard (G)** | 1st bit beyond mantissa width | Extends precision for intermediate computation |
| **Round (R)** | 2nd bit beyond mantissa width | Helps determine rounding direction |
| **Sticky (S)** | OR of all remaining bits | Indicates if any truncated bits were non-zero |

**Rounding Decision Table (Round to Nearest Even):**

| G | R | S | Action |
|---|---|---|---|
| 0 | X | X | Truncate (round down) |
| 1 | 0 | 0 | Round to even (check LSB of mantissa) |
| 1 | 0 | 1 | Round up |
| 1 | 1 | X | Round up |

**Example:** If mantissa result = 1.0101|0|1|1 (Guard=0, Round=1, Sticky=1)
→ G=0 → Truncate → Result = 1.0101

**Example:** If mantissa = 1.0101|1|1|0 (G=1, R=1, S=0)
→ G=1, R=1 → Round up → Result = 1.0110

> 🎯 **GATE Fact:** Without GRS bits, FP operations would have double the rounding error. IEEE 754 mandates their use for all operations!

### 2.13 Carry Save Adder (CSA) ⭐⭐

> 💡 **Problem:** Adding many numbers sequentially with carry-propagate adders is slow. CSA avoids carry propagation by keeping Sum and Carry as separate vectors!

**Principle:** A CSA takes 3 inputs (A, B, C) and produces Sum and Carry — all in O(1) time (no ripple!). This is essentially a column of full adders with NO carry chain.

```
Input: Three n-bit numbers A, B, C
       ┌─────────────────┐
  A →  │                 │ → Sum  (n bits)
  B →  │  Full Adder     │
  C →  │  Array (no      │ → Carry (n bits, shifted left by 1)
       │  carry chain!)  │
       └─────────────────┘
```

$$\text{Sum}_i = A_i \oplus B_i \oplus C_i$$
$$\text{Carry}_i = A_iB_i + B_iC_i + A_iC_i$$

**CSA in Multipliers:**
```
Partial Products:    PP0
                     PP1  → CSA1 → (S1, C1)
                     PP2            ↓
                                  CSA2 → (S2, C2)
                     PP3            ↓
                                  CSA3 → (S3, C3)
                                          ↓
                                  Final CPA (Carry Propagate Adder)
                                          ↓
                                       Product
```

**Adding k numbers using CSA tree:**
- Regular adder: (k−1) × O(n) delay = O(kn) total
- CSA tree: O(log₁.₅ k) CSA levels + 1 final CPA = **O(log k × 1 + n)** total

> 🎯 **GATE:** CSA reduces multi-operand addition from O(kn) to O(n + log k). Critical for fast multipliers!

### 2.14 Wallace Tree Multiplier ⭐⭐

> 🚀 The fastest combinational multiplier! Uses CSA tree to reduce partial products optimally.

**Algorithm:**
1. Generate all n² partial product bits using AND gates
2. Group partial products into sets of 3 → pass through CSA (3:2 compressor)
3. Remaining carry and sum become inputs for next level
4. Repeat until only 2 numbers remain
5. Final addition with a fast CPA (CLA)

**For n-bit × n-bit multiplication:**
- Number of CSA levels ≈ ⌈log₁.₅(n)⌉
- Final CPA delay = O(log n) with CLA

**Wallace Tree for 4-bit multiplication (A × B):**
```
Level 0: 4 partial products (PP0, PP1, PP2, PP3)
Level 1: Group (PP0, PP1, PP2) into CSA → gives S1, C1
          PP3 passes through
Level 2: (S1, C1, PP3) into CSA → gives S2, C2
Level 3: S2 + C2 using final CPA → Product!
```

**Comparison of Multiplier Architectures:**
| Multiplier | Delay | Hardware | Technique |
|---|---|---|---|
| Shift-and-Add | O(n²) | O(n) | Sequential |
| Array Multiplier | O(n) | O(n²) | Ripple CSA |
| Wallace Tree | O(log₁.₅ n × 1 + log n) | O(n²) | CSA tree + CLA |
| Dadda Multiplier | ~same as Wallace | Slightly less | Optimized grouping |
| Booth + Wallace | O(log₁.₅(n/2) + log n) | O(n²) | Radix-4 + CSA tree |

> 🎯 **GATE Insight:** Modern ALUs use Modified Booth encoding (reduces partial products by half) + Wallace tree (fast reduction) + CLA (fast final add). This combination gives O(log n) multiplication latency!

### 2.15 Barrel Shifter ⭐

> 💡 A barrel shifter can shift/rotate by ANY amount in a single cycle — O(log n) delay!

**Structure for 8-bit barrel shifter:**
```
           Input: D₇ D₆ D₅ D₄ D₃ D₂ D₁ D₀
                        │
            ┌───────────▼───────────┐
Layer 1:    │   Shift by 4 or 0    │  ← controlled by shift_amount[2]
            └───────────┬───────────┘
            ┌───────────▼───────────┐
Layer 2:    │   Shift by 2 or 0    │  ← controlled by shift_amount[1]
            └───────────┬───────────┘
            ┌───────────▼───────────┐
Layer 3:    │   Shift by 1 or 0    │  ← controlled by shift_amount[0]
            └───────────┬───────────┘
                        │
           Output (shifted by 0-7 positions)
```

**Each layer:** n MUXes (2:1), each selects between shifted and unshifted input.

**For n-bit barrel shifter:**
- Layers: log₂(n)
- MUXes: n × log₂(n)
- Delay: O(log n) MUX delays

**32-bit barrel shifter:** 5 layers × 32 MUXes = 160 MUXes, delay = 5 MUX delays.

> 🎯 **GATE Fact:** Barrel shifters enable single-cycle variable shifts. Without one, shifting by k requires k clock cycles (or k stages of MUXes in series).

### 2.16 BCD (Binary-Coded Decimal) Arithmetic ⭐⭐

#### Quick Visual 🧩

```
BCD digit encoding (one decimal digit per nibble):

Decimal: 0 1 2 3 4 5 6 7 8 9
BCD:    0000 ...               1001

Invalid BCD codes: 1010 to 1111

BCD add correction:
if (sum > 1001 OR carry=1) add 0110
```

> 💡 In BCD, each decimal digit is stored as 4 binary bits. Used in financial/business applications where exact decimal representation matters.

#### BCD Addition Rules 🔢

1. Add two BCD digits as normal 4-bit binary
2. If sum > 9 (1010 to 1111) OR there's a carry out → add correction factor **6 (0110)**
3. Generate carry to next BCD digit

**Detailed Example: 59 + 38 in BCD**
```
    0101 1001     (59 in BCD)
  + 0011 1000     (38 in BCD)
  ───────────
    1001 0001     (binary result)
         │
    Check each nibble:
    Lower: 0001 = 1 (≤9, no correction needed)
    Upper: 1001 = 9 (≤9, but there was a carry from lower? Let's redo:)

    Actually step by step:
    Lower nibble: 1001 + 1000 = 1|0001 → carry=1, result=0001
    (Wait: 9+8=17, 17 > 9) → add 0110:
    0001 + 0110 = 0111, carry = 1

    Upper nibble: 0101 + 0011 + 1(carry) = 1001
    1001 = 9 (≤ 9, no correction)

    Result: 1001 0111 = 97 in BCD ✅ (59 + 38 = 97)
```

**Shortcut for GATE:** After binary addition, check each BCD digit:
- If digit > 9 or carry from that digit → add 6 to that digit

#### BCD Subtraction using 10's and 9's Complement

**10's complement of BCD digit:** Subtract each digit from 9, then add 1 to result
**9's complement of BCD digit:** Subtract each digit from 9 (just like 1's complement for binary!)

**Example: 72 − 38 using 10's complement BCD**
1. 9's complement of 38 = 61 (9−3=6, 9−8=1)
2. 10's complement = 61 + 1 = 62
3. 72 + 62 = 134 → discard carry → **34** ✅

#### BCD to Binary Conversion
**BCD 0100 0010 (42) → Binary:**
- 42₁₀ = 32 + 8 + 2 = 101010₂

**Binary to BCD (Double-Dabble Algorithm):**
For converting binary 11011₂ (= 27) to BCD:
```
Step: Shift left, if any BCD digit ≥ 5 → add 3 before shifting
Start: 00000 | 11011
After shift 1: 00001 | 1011_
After shift 2: 00011 | 011__
After shift 3: 00110 | 11___ → 0110≥5? No. 0110=6>5! Add 3: 01001
After shift 4: 01001 | 1____ → shift → 10011 | 1_
                Wait: 0011 ≥5? No. Continue.
After shift 5: 00100 | 111 → shift → 0100111 → BCD: 0010 0111 = 27 ✅
```

> 🎯 **GATE Fact:** BCD takes ~20% more bits than binary for the same number, but preserves exact decimal values. Used in calculators and financial systems!

### 2.17 Number Codes: Gray, Excess-3, ASCII ⭐

#### Gray Code 🔢
Adjacent values differ in exactly **1 bit** — useful for encoders, Karnaugh maps, and error reduction.

**Binary to Gray conversion:** G[i] = B[i] ⊕ B[i+1], G[MSB] = B[MSB]
**Gray to Binary conversion:** B[MSB] = G[MSB], B[i] = G[i] ⊕ B[i+1]

| Decimal | Binary | Gray |
|---|---|---|
| 0 | 0000 | 0000 |
| 1 | 0001 | 0001 |
| 2 | 0010 | 0011 |
| 3 | 0011 | 0010 |
| 4 | 0100 | 0110 |
| 5 | 0101 | 0111 |
| 6 | 0110 | 0101 |
| 7 | 0111 | 0100 |
| 8 | 1000 | 1100 |
| 9 | 1001 | 1101 |
| 10 | 1010 | 1111 |
| 11 | 1011 | 1110 |
| 12 | 1100 | 1010 |
| 13 | 1101 | 1011 |
| 14 | 1110 | 1001 |
| 15 | 1111 | 1000 |

**Worked Example:** Convert binary 1011 to Gray:
- G₃ = B₃ = 1
- G₂ = B₃ ⊕ B₂ = 1 ⊕ 0 = 1
- G₁ = B₂ ⊕ B₁ = 0 ⊕ 1 = 1
- G₀ = B₁ ⊕ B₀ = 1 ⊕ 1 = 0
- Gray code = **1110** ✅

> 🎯 **GATE Application:** Gray code used in shaft encoders — if reading changes during rotation, error is only ±1 (vs binary where multiple bits may change simultaneously).

#### Excess-3 Code 🔢
- BCD + 3 for each digit
- Self-complementing: 9's complement = bitwise complement!

| Decimal | BCD | Excess-3 | 9's complement (bitwise NOT) |
|---|---|---|---|
| 0 | 0000 | 0011 | 1100 (= Excess-3 of 9) |
| 1 | 0001 | 0100 | 1011 (= Excess-3 of 8) |
| 2 | 0010 | 0101 | 1010 (= Excess-3 of 7) |
| 3 | 0011 | 0110 | 1001 (= Excess-3 of 6) |
| 4 | 0100 | 0111 | 1000 (= Excess-3 of 5) |
| 5 | 0101 | 1000 | 0111 (= Excess-3 of 4) |
| 6 | 0110 | 1001 | 0110 (= Excess-3 of 3) |
| 7 | 0111 | 1010 | 0101 (= Excess-3 of 2) |
| 8 | 1000 | 1011 | 0100 (= Excess-3 of 1) |
| 9 | 1001 | 1100 | 0011 (= Excess-3 of 0) |

> 🎯 **GATE Fact:** Excess-3 is self-complementing: To find 9's complement, just flip all bits! This makes BCD subtraction hardware simpler.

#### ASCII and EBCDIC
| Feature | ASCII | EBCDIC |
|---|---|---|
| Bits | 7 (+ parity) | 8 |
| Characters | 128 | 256 |
| Origin | ANSI | IBM mainframes |
| Digits 0-9 | 0x30 − 0x39 | 0xF0 − 0xF9 |
| Uppercase A-Z | 0x41 − 0x5A | 0xC1 − 0xE9 (non-contiguous!) |
| Used by | Almost everything | IBM mainframes |

> 🎯 **GATE Trick:** In ASCII, 'A'=65=0x41, 'a'=97=0x61. Difference = 32 = bit 5. So `char | 0x20` converts upper to lower case!

### 2.18 Overflow Detection — Complete Rules ⭐⭐

#### Quick Visual 🧩

```
Signed overflow (2's complement add):

 + + -> -   overflow
 - - -> +   overflow
 + - -> ?   no overflow

V = Carry_into_MSB XOR Carry_out_of_MSB
```

#### Unsigned Overflow
- **Addition:** Overflow if carry out of MSB = 1
- **Subtraction:** Overflow (borrow) if borrow out of MSB = 1

#### Signed (2's Complement) Overflow
- **Addition:** V = Cₙ₋₁ ⊕ Cₙ (carry INTO MSB ⊕ carry OUT of MSB)
  - Positive + Positive = Negative → OVERFLOW
  - Negative + Negative = Positive → OVERFLOW
  - Positive + Negative → **NEVER overflows**
- **Subtraction:** Convert to addition (A − B = A + (−B)), then apply same rule

#### Processor Condition Flags (PSW) ⭐⭐

| Flag | Symbol | Set When | Used For |
|---|---|---|---|
| **Zero (Z)** | Z | Result = 0 | BEQ, BNE |
| **Carry (C)** | C | Unsigned overflow (carry out) | ADDC, unsigned comparison |
| **Overflow (V)** | V | Signed overflow | Signed comparison |
| **Sign/Negative (N)** | N | Result MSB = 1 | Signed branches |
| **Parity (P)** | P | Even number of 1s in result | Error detection |
| **Auxiliary Carry (AC)** | AC | Carry from bit 3 to bit 4 | BCD arithmetic |

**Condition Code Usage for Comparisons (CMP A, B computes A − B):**

| Condition | Unsigned Test | Signed Test |
|---|---|---|
| A = B | Z = 1 | Z = 1 |
| A ≠ B | Z = 0 | Z = 0 |
| A > B | C = 0 AND Z = 0 | N = V AND Z = 0 |
| A < B | C = 1 | N ≠ V |
| A ≥ B | C = 0 | N = V |
| A ≤ B | C = 1 OR Z = 1 | N ≠ V OR Z = 1 |

> ⚠️ **GATE Trap:** Signed and unsigned comparisons use DIFFERENT flags! CMP sets ALL flags, but the branch instruction decides which to check. BGE checks N=V (signed), while BAE checks C=0 (unsigned).

**Worked Example:**
In 8-bit system: A = 200 (11001000₂), B = 50 (00110010₂)
CMP A, B → computes 200 − 50 = 150 → 10010110₂

Flags: Z=0, N=1 (MSB=1), C=0 (no borrow), V=1 (overflow: positive − positive = negative?)

Wait: As **unsigned**: 200 − 50 = 150 → correct, C=0 (no borrow) → A > B ✅
As **signed**: −56 − 50 = −106 → 10010110₂ = −106 → N=1, V=0 → N≠V → A < B ✅

This is correct! As unsigned 200 > 50, but as signed −56 < 50.

---

### 2.19 Error Detection & Correction in Memory ⭐⭐

#### Quick Visual 🧩

```
SECDED concept:

Data bits + Hamming parity bits + overall parity
  |
  v
Syndrome != 0 and overall parity mismatch -> single-bit error (correctable)
Syndrome == 0 and overall parity mismatch -> double-bit error (detect only)
```

> 💡 Memory errors can occur due to cosmic rays, electrical noise, or aging. Computers use error-detecting and correcting codes to ensure reliability.

#### Parity Bit ⭐
- **Single parity:** Add 1 bit to make total number of 1s even (even parity) or odd (odd parity)
- Can **detect** single-bit errors (odd number of bit errors)
- **Cannot correct** errors, cannot detect even number of errors

**Example (even parity):** Data = 1011001, Number of 1s = 4 (even) → parity bit = 0
Transmitted: 10110010. If bit flips: 10110110 → 5 ones (odd) → ERROR detected!

#### Hamming Code ⭐⭐

**For k data bits, need r check bits where:** 2^r ≥ k + r + 1

| Data bits (k) | Check bits (r) | Total (n = k+r) |
|---|---|---|
| 1 | 2 | 3 |
| 4 | 3 | 7 |
| 8 | 4 | 12 |
| 16 | 5 | 21 |
| 32 | 6 | 38 |
| 64 | 7 | 71 |

**Hamming Distance:**
- **Detect** up to d errors → need minimum distance d+1
- **Correct** up to t errors → need minimum distance 2t+1

| Code | Min Distance | Detects | Corrects |
|---|---|---|---|
| Single parity | 2 | 1 error | 0 |
| Hamming (7,4) | 3 | 2 errors | 1 error |
| Hamming + parity (SECDED) | 4 | 2 errors | 1 error |

**SECDED = Single Error Correction, Double Error Detection**
- Used in ECC memory (most server RAM)
- Adds 1 extra parity bit to Hamming code

**Worked Example — Hamming Code (7,4):**
Encode data word D = 1011.

Check bits at positions 1, 2, 4; data bits at 3, 5, 6, 7.

| Position | 7 | 6 | 5 | 4 | 3 | 2 | 1 |
|---|---|---|---|---|---|---|---|
| Bit type | D₄ | D₃ | D₂ | C₃ | D₁ | C₂ | C₁ |
| Data | 1 | 0 | 1 | ? | 1 | ? | ? |

**Calculate check bits:**
- C₁ (pos 1): covers positions with bit 0 set in binary (1,3,5,7)
  Positions: 1,3,5,7 → values: ?,1,1,1 → C₁ = 1⊕1⊕1 = 1
- C₂ (pos 2): covers positions with bit 1 set (2,3,6,7)
  Positions: 2,3,6,7 → values: ?,1,0,1 → C₂ = 1⊕0⊕1 = 0
- C₃ (pos 4): covers positions with bit 2 set (4,5,6,7)
  Positions: 4,5,6,7 → values: ?,1,0,1 → C₃ = 1⊕0⊕1 = 0

**Encoded: 1 0 1 0 1 0 1** (position 7 to 1)

**Error detection:** If position 5 flips (1→0): received = 1 0 **0** 0 1 0 1
- Syndrome S₁ = XOR of pos 1,3,5,7 = 1⊕1⊕0⊕1 = 1
- Syndrome S₂ = XOR of pos 2,3,6,7 = 0⊕1⊕0⊕1 = 0
- Syndrome S₃ = XOR of pos 4,5,6,7 = 0⊕0⊕0⊕1 = 1
- Syndrome = S₃S₂S₁ = 101₂ = **5** → Error at position 5! Flip it to correct.

### 2.20 Data Format Conversion Examples ⭐

#### Quick Visual 🧩

```
Decimal -> IEEE754 conversion flow:

Decimal value
  |
  +--> Sign bit
  |
  +--> Binary scientific form: 1.xxxxx x 2^e
  |
  +--> Exponent = e + bias
  |
  +--> Mantissa = fractional bits after leading 1
```

**Example: Convert −25.625 to IEEE 754 single precision**

1. Sign bit: 1 (negative)
2. Convert magnitude: 25.625₁₀
   - 25 = 11001₂
   - 0.625: 0.625×2=1.25 → 1; 0.25×2=0.5 → 0; 0.5×2=1.0 → 1
   - 0.625 = 0.101₂
   - 25.625 = 11001.101₂
3. Normalize: 1.1001101 × 2⁴
4. Exponent: 4 + 127 = 131 = 10000011₂
5. Mantissa (23 bits): 10011010000000000000000

**Result: 1 10000011 10011010000000000000000**
Hex: 0xC1CD0000

**Example: Decode IEEE 754: 0x42F60000**
1. Binary: 0100 0010 1111 0110 0000 0000 0000 0000
2. Sign = 0 (positive)
3. Exponent = 10000101₂ = 133, E = 133 − 127 = 6
4. Mantissa = 1.111 0110 0000... (add hidden 1)
5. Value = 1.1110110₂ × 2⁶ = 1111011.0₂ = **123.0**

**Example: What is the smallest gap between two consecutive single-precision floats near 1.0?**
Near 1.0: exponent = 127, the gap = 2^(E−23) = 2^(127−127−23) = 2⁻²³ ≈ **1.19 × 10⁻⁷**

This is called **machine epsilon** for single precision.

Near 2.0: exponent = 128, gap = 2^(128−127−23) = 2⁻²² ≈ 2.38 × 10⁻⁷
Near 1024.0: exponent = 137, gap = 2^(137−127−23) = 2⁻¹³ ≈ 1.22 × 10⁻⁴

> 🎯 **GATE Insight:** Floating-point precision DECREASES as the magnitude increases! Numbers near 0 have very fine granularity; numbers near 2¹²⁷ have huge gaps between consecutive values.

### 2.21 Signed vs Unsigned Arithmetic — When It Matters ⭐

#### Quick Visual 🧩

```
Same 8-bit pattern, different meaning:

11111111
  unsigned -> 255
  signed   -> -1

Hardware adder is same;
interpretation changes by which flag/branch you use.
```

**Same operation, different result:**
In 8-bit: 11111111₂ + 00000001₂ = 00000000₂

| Interpretation | Operation | Result | Overflow? |
|---|---|---|---|
| Unsigned | 255 + 1 | 0 (wrap around) | **Yes** (carry out) |
| Signed (2's comp) | −1 + 1 | 0 | **No** (correct!) |

**Same bit pattern, different conditions:**
The hardware performs the SAME addition! The difference is in which flags matter:
- **Unsigned overflow:** Check Carry flag (C)
- **Signed overflow:** Check Overflow flag (V) = Cₙ₋₁ ⊕ Cₙ

This is why processors set BOTH C and V flags simultaneously — the software decides which to check based on whether values are signed or unsigned!

---

## 3. Instruction Set Architecture 📋

> 💡 **Analogy:** ISA is like the **menu** of a restaurant 🍽️. It defines WHAT dishes (instructions) the restaurant (processor) can serve, but NOT how the kitchen (hardware) actually prepares them.

> 🏭 **Why ISA matters:** It's the CONTRACT between hardware and software. Software developers write programs using the ISA; hardware designers implement the ISA.

### 3.1 RISC vs CISC ⭐⭐⭐

#### Quick Visual 🧩

```
RISC path:  many simple instructions -> easy decode -> deep pipeline
CISC path:  fewer complex instructions -> heavy decode -> micro-ops
```

| Feature | RISC | CISC |
|---------|------|------|
| Instructions | Simple, fixed-length | Complex, variable-length |
| Execution | 1 CPI (ideally) | Multiple CPI |
| Addressing modes | Few (3-5) | Many (10+) |
| Registers | Many (32+) | Few (8-16) |
| Memory access | Load/Store only | Any instruction can access memory |
| Pipeline | Easy to pipeline | Hard to pipeline |
| Code size | Larger (more instructions) | Smaller (fewer, complex instructions) |
| Instruction decode | Simple (fixed format) | Complex (variable format) |
| Control unit | Hardwired | Microprogrammed |
| Power consumption | Lower | Higher |
| Examples | ARM, MIPS, RISC-V, SPARC | x86, VAX, Motorola 68k |

> 🎯 **GATE Insight:** Modern "CISC" processors (like Intel Core) internally translate CISC instructions into RISC-like micro-ops!

### 3.2 Instruction Format ⭐⭐⭐

#### Quick Visual 🧩

```
3-address: [OP][D][S1][S2]
2-address: [OP][D/S1][S2]
1-address: [OP][S] + ACC implicit
0-address: [OP] + stack implicit
```

**Types Based on Number of Addresses:**

**3-Address:** OP dest, src1, src2 → ADD R1, R2, R3 (R1 ← R2 + R3)
- Most flexible, largest instruction size
- Used in: MIPS, ARM, RISC-V

**2-Address:** OP dest/src1, src2 → ADD R1, R2 (R1 ← R1 + R2)
- One operand is both source and destination (destroyed)
- Used in: x86, PDP-11

**1-Address:** OP src → ADD R1 (ACC ← ACC + R1)
- Implicit accumulator as one operand
- Used in: Early computers, microcontrollers

**0-Address:** OP → ADD (TOS ← TOS + TOS_next)
- Operands implicitly on stack (push/pop)
- Used in: JVM, HP 3000, PostScript

**Comparison for evaluating X = (A + B) × (C + D):**

| Type | Instructions needed |
|---|---|
| 3-address | 3 (ADD T1,A,B; ADD T2,C,D; MUL X,T1,T2) |
| 2-address | 5 (MOV T1,A; ADD T1,B; MOV T2,C; ADD T2,D; MUL T1,T2; MOV X,T1) |
| 1-address | 7 (LOAD A; ADD B; STORE T1; LOAD C; ADD D; MUL T1; STORE X) |
| 0-address | 8 (PUSH A; PUSH B; ADD; PUSH C; PUSH D; ADD; MUL; POP X) |

> 🎯 **GATE:** More addresses per instruction → fewer instructions but longer instructions.

### 3.3 Expanding Opcode ⭐⭐⭐

#### Quick Visual 🧩

```
Use short opcodes for common formats,
reserve one pattern to "expand" into longer opcode space.

Level-1 opcode -> if reserved -> look at next field for more opcodes
```

> 💡 **Analogy:** Like a phone number system — short area codes for big cities (3-address: most common), longer for small towns (0-address: rare).

**Worked Example (GATE Favorite):**
16-bit instruction with three 4-bit fields:

```
| 4-bit opcode | 4-bit R1 | 4-bit R2 | 4-bit R3 |
```

**Standard GATE Problem:** 15 three-address, 14 two-address, 31 one-address → how many 0-address?
- 3-addr uses opcodes 0000-1110 (15 used, 1 remaining = 1111)
- 2-addr: 1111 + 4 bits = 16 possible, 14 used, 2 remaining
- 1-addr: 2 × 16 = 32 possible, 31 used, 1 remaining
- 0-addr: 1 × 16 = **16 zero-address instructions**

> 🎯 **General Formula:** At each level, remaining_codes × 16 = available codes for next level.

**Expanding Opcode — Additional GATE Problems:**

**Problem 2:** 12-bit instruction, 3-bit register field. 
Maximum 3-address instructions = 2⁶ − 1 = 63? No:
```
| opcode | R1(3) | R2(3) | R3(3) |
```
Wait, 3 + 3 + 3 = 9, so opcode = 12 − 9 = 3 bits. Max 3-addr = 2³ − 1 = 7 (reserve 1 pattern for expansion).

If we have 5 three-address, how many 2-address and 1-address?
- 3-addr: 5 used, remaining patterns = 2³ − 5 = 3
- 2-addr: 3 × 2³ = 24 possible. If we use 20: remaining = 4
- 1-addr: 4 × 2³ = 32. If we use all: **32 one-address**

**Problem 3:** 36-bit instruction, 5-bit register field, 3 register fields.
- Opcode = 36 − 15 = 21 bits for 3-address
- Max 3-address = 2²¹ = 2,097,152 (over 2 million!)
- If we want exactly 2²¹ − 1 three-address instructions:
  1 remaining × 2⁵ = 32 two-address possible
  If 31 used: 1 × 2⁵ = 32 one-address possible
  If 31 used: 1 × 2⁵ = 32 zero-address possible

**General Formula for Expanding Opcode:**

Let n = total instruction bits, r = register field bits, k = number of register fields in highest format.

Available bits for opcode at k-address level: n − k×r

At each level: $\text{available}_{next} = \text{remaining} \times 2^r$

**Key insight for GATE:** The number of instructions at each level is a trade-off. More at one level = fewer at the next.

### 3.4 Addressing Modes ⭐⭐⭐

#### Quick Visual 🧩

```
Immediate:     operand in instruction
Direct:        EA = address field
Indirect:      EA = M[address field]
Reg indirect:  EA = R[i]
Base+offset:   EA = R[base] + displacement
PC-relative:   EA = PC + offset
```

> 💡 **Analogy:** Different ways to tell the delivery driver where to go 🍕:
> - **Immediate:** "The data is right here in the instruction"
> - **Direct:** "Go to address 42" (fixed address in instruction)
> - **Indirect:** "Go to address 42, they'll tell you the real address"
> - **Indexed:** "Go to base + offset" (array access)

| Mode | Effective Address | Use Case | Example |
|------|--------------------------|---------|---------|
| Immediate | Operand = value in instruction | Constants | ADD R1, #5 |
| Register | EA = register content | Fast access | ADD R1, R2 |
| Direct/Absolute | EA = address in instruction | Global vars | LOAD R1, [500] |
| Indirect | EA = M[address in instruction] | Pointers | LOAD R1, @[500] |
| Register Indirect | EA = M[register] | Pointer deref | LOAD R1, [R2] |
| Displacement/Base | EA = register + offset | Struct access | LOAD R1, 100[R2] |
| Indexed | EA = base_addr + index_reg | Array access | LOAD R1, A[R2] |
| Base+Index | EA = base + index + offset | 2D arrays | LOAD R1, [R2+R3+d] |
| Auto-increment | EA = [R]; R ← R + d | Array traversal | LOAD R1, [R2]+ |
| Auto-decrement | R ← R − d; EA = [R] | Stack push | LOAD R1, -[R2] |
| PC-Relative | EA = PC + offset | Branch targets | BEQ loop |

**Memory Accesses to Fetch Operand (excluding instruction fetch):**

| Mode | Memory Accesses | Why |
|------|----------------|-----|
| Immediate | 0 | Data in instruction |
| Register | 0 | Data in register |
| Direct | 1 | One memory read |
| Register Indirect | 1 | One memory read at register address |
| Indirect (memory) | 2 | Read pointer, then read data |

> ⚠️ **GATE Trick:** Total memory accesses = instruction fetch accesses + operand accesses!

**Addressing Mode Usage:**
- **Local variables:** Displacement (SP + offset)
- **Global variables:** Direct/Absolute
- **Array elements:** Indexed (base + index × element_size)
- **Pointer dereferencing:** Register Indirect
- **Branch targets:** PC-Relative

### 3.5 Instruction Types ⭐

#### Quick Visual 🧩

```
Instruction classes:
[Data move] [ALU] [Logic] [Control] [Compare] [I/O]
```

| Category | Examples | Description |
|---|---|---|
| **Data Transfer** | LOAD, STORE, MOV, PUSH, POP | Data between registers/memory |
| **Arithmetic** | ADD, SUB, MUL, DIV, INC, DEC | Mathematical operations |
| **Logical** | AND, OR, NOT, XOR, SHL, SHR | Bitwise operations |
| **Control Flow** | JMP, BEQ, BNE, CALL, RET | Change execution flow |
| **Comparison** | CMP, TEST | Set flags |
| **I/O** | IN, OUT | Device communication |

### 3.6 Subroutine Call Mechanisms ⭐⭐

**Methods to save return address:**
1. **In a register:** CALL saves PC in link register (e.g., R31 in MIPS, LR in ARM)
   - Fast, but nested calls need stack to save LR
2. **On the stack:** CALL pushes PC onto stack, RET pops it
   - Supports unlimited nesting and recursion
3. **In memory at subroutine start:** Old method — CANNOT support recursion!

**Stack Frame (Activation Record):**
```
High Address
┌──────────────────┐
│   Parameters      │  ← Pushed by caller
├──────────────────┤
│   Return Address  │  ← Saved by CALL
├──────────────────┤
│   Saved FP        │  ← Old frame pointer
├──────────────────┤  ← FP (Frame Pointer) points here
│   Local Variables │
├──────────────────┤
│   Saved Registers │
├──────────────────┤  ← SP (Stack Pointer) points here
Low Address
```

### 3.7 Stack Machine & Zero-Address ISA ⭐⭐

> 💡 A **stack machine** has no general-purpose registers. All operations work on a stack — operands are pushed and results are pushed back.

**Zero-Address Instruction Format:**
```
| Opcode only |   (no operand fields — implicit stack)
```

**Example: Evaluate (A + B) × (C − D) on a stack machine** 📚
```
PUSH A          ; Stack: [A]
PUSH B          ; Stack: [A, B]
ADD             ; Stack: [A+B]
PUSH C          ; Stack: [A+B, C]
PUSH D          ; Stack: [A+B, C, D]
SUB             ; Stack: [A+B, C−D]
MUL             ; Stack: [(A+B)×(C−D)]    ← result!
POP  X          ; X = (A+B)×(C−D)
```

**Infix to Postfix (Reverse Polish Notation — RPN):**
- (A + B) × (C − D) → A B + C D − × (postfix)
- Stack machines execute postfix expressions directly!

**Conversion Examples:**
| Infix | Postfix (RPN) |
|---|---|
| A + B | A B + |
| (A + B) × C | A B + C × |
| A × B + C × D | A B × C D × + |
| (A + B) × (C − D) / E | A B + C D − × E / |

> 🎯 **GATE Trap:** Stack machine instructions are shorter (no operand fields) but you need MORE instructions. Total program bits may be similar to register machines!

**Real-world stack machines:** Java Virtual Machine (JVM) bytecode, HP 3000, PostScript, Forth language, WebAssembly (partially)

### 3.8 Register Windows (SPARC Architecture) ⭐

> 💡 Instead of saving/restoring registers on every function call, SPARC uses overlapping **register windows**.

```
┌──────────────────────────────────────────────────┐
│              Physical Register File (128+)         │
│                                                    │
│  Window N (current function):                      │
│  ┌────────┬──────────┬────────┬──────────┐        │
│  │ Global │   In     │ Local  │   Out    │        │
│  │ (8)    │  (8)     │  (8)   │  (8)     │        │
│  └────────┴──────────┴────────┴──────────┘        │
│                 ↑ overlap ↑                        │
│  Window N-1 (caller):                              │
│  ┌────────┬──────────┬────────┬──────────┐        │
│  │ Global │   In     │ Local  │   Out    │        │
│  │ (8)    │  (8)     │  (8)   │  (8)     │        │
│  └────────┴──────────┴────────┴──────────┘        │
│                                                    │
│  Caller's OUT registers = Callee's IN registers!  │
└──────────────────────────────────────────────────┘
```

**SAVE instruction:** Advances CWP (Current Window Pointer) → new window
**RESTORE instruction:** Decrements CWP → return to caller's window

**Advantages:** No stack push/pop for register saves — near-zero overhead function calls!
**Disadvantage:** Limited windows (typically 8); window overflow → save to memory (expensive)

> 🎯 **GATE Fact:** SPARC has 32 registers visible (8 global + 8 in + 8 local + 8 out), but 128+ physical registers organized in overlapping windows.

### 3.9 Processor Modes and Privilege Levels ⭐

| Mode | Also Called | Privilege | Can Access |
|---|---|---|---|
| **Kernel/Supervisor** | Ring 0 (x86) | Highest | All hardware, all memory, all instructions |
| **User** | Ring 3 (x86) | Lowest | Only user memory, no privileged instructions |

**Privileged Instructions (examples):**
- I/O instructions (IN, OUT)
- Modify page tables
- Disable/enable interrupts (CLI, STI)
- Halt processor (HLT)
- Change privilege level
- Access control registers (CR0-CR4 in x86)

**Mode Switch:** User → Kernel via system call (software interrupt), hardware interrupt, or exception.

```
User Mode                  Kernel Mode
┌──────────┐    syscall    ┌──────────┐
│ App code │ ──────────→  │ OS code  │
│          │ ← return ──  │          │
│ INT 0x80 │    IRET       │ ISR      │
└──────────┘              └──────────┘
```

> 🎯 **GATE Insight:** Executing a privileged instruction in user mode causes a **trap** (exception) → CPU switches to kernel mode → OS decides whether to allow or terminate the process.

### 3.10 Instruction Cycle Timing ⭐⭐

#### Quick Visual 🧩

```
IC x CPI x ClockPeriod
     |
     v
  CPU Time

Lower IC or lower CPI or faster clock -> lower execution time
```

```
Fetch → Decode → Execute → Memory → Write Back
```

**T_CPU = Instruction Count × CPI × Clock Period**
**T_CPU = IC × CPI / Clock Frequency**

**CPI (Cycles Per Instruction):**
Average CPI = Σ(CPIᵢ × Fractionᵢ) for weighted instruction mix.

**Worked Example:**
| Instruction Type | CPI | Frequency |
|---|---|---|
| ALU | 1 | 45% |
| Load | 3 | 25% |
| Store | 2 | 15% |
| Branch | 2 | 15% |

Average CPI = 0.45×1 + 0.25×3 + 0.15×2 + 0.15×2 = 0.45 + 0.75 + 0.30 + 0.30 = **1.80**

If clock = 2 GHz and program has 10⁶ instructions:
CPU Time = 10⁶ × 1.80 / (2 × 10⁹) = 0.9 × 10⁻³ = **0.9 ms**
MIPS = 2 × 10⁹ / (1.80 × 10⁶) = **1111 MIPS**

**MIPS = Clock Frequency / (CPI × 10⁶)**

### 3.11 Addressing Mode Worked Examples ⭐⭐

**Example 1: Calculate Effective Address**
Given: R1 = 500, R2 = 100, M[500] = 700, M[600] = 800, M[700] = 900

| Instruction | Mode | EA | Value |
|---|---|---|---|
| LOAD R3, #42 | Immediate | — | 42 |
| LOAD R3, 600 | Direct | 600 | M[600] = 800 |
| LOAD R3, (R1) | Register indirect | 500 | M[500] = 700 |
| LOAD R3, 100(R1) | Base+displacement | 500+100=600 | M[600] = 800 |
| LOAD R3, @600 | Memory indirect | M[600]=800 | M[800] = ? |
| LOAD R3, (R1)+ | Auto-increment | 500 (then R1←504) | M[500] = 700 |

**Example 2: Array Access with Indexed Addressing**
Array A starts at address 1000, element size = 4 bytes.
Access A[5]: EA = 1000 + 5×4 = 1020

In MIPS (base+displacement):
```
; R10 = base address of A (1000)
; Access A[5]:
LW R1, 20(R10)      ; offset = 5×4 = 20
```

In MIPS (indexed):
```
; R10 = base address of A
; R11 = index = 5
SLL R12, R11, 2      ; R12 = 5 × 4 = 20 (shift left by 2 = multiply by 4)
ADD R13, R10, R12    ; R13 = 1000 + 20 = 1020
LW R1, 0(R13)       ; Load A[5]
```

**Example 3: Pointer Dereferencing**
```c
int *p = &x;    // p stored in R5, x at address 2000
int y = *p;     // Dereference pointer
```
MIPS:
```
LW R6, 0(R5)        ; R6 = value at address pointed to by R5
```
If `int **pp = &p;` (pointer to pointer):
```
LW R7, 0(R8)        ; R7 = p (first level indirection)
LW R6, 0(R7)        ; R6 = *p (second level = **pp)
```
This is why memory indirect addressing is useful for double pointers!

**Example 4: Stack Operations**
```
; Push R1 onto stack (auto-decrement):
; SP initially = 0x7FFC
ADDI SP, SP, -4      ; SP = 0x7FF8
SW   R1, 0(SP)       ; Store R1 at 0x7FF8

; Pop into R2 (auto-increment):
LW   R2, 0(SP)       ; Load from 0x7FF8
ADDI SP, SP, 4       ; SP = 0x7FFC
```

### 3.12 Instruction Encoding Practice ⭐⭐

#### Quick Visual 🧩

```
MIPS 32-bit instruction formats:

R-type: [opcode(6)][rs(5)][rt(5)][rd(5)][shamt(5)][funct(6)]
I-type: [opcode(6)][rs(5)][rt(5)][imm(16)]
J-type: [opcode(6)][target(26)]
```

**MIPS R-Type Encoding:**
```
ADD R8, R9, R10
```
- Opcode = 000000 (R-type)
- Rs (R9) = 01001
- Rt (R10) = 01010
- Rd (R8) = 01000
- Shamt = 00000
- Funct (ADD) = 100000

**Binary: 000000 01001 01010 01000 00000 100000**
**Hex: 0x012A4020**

**MIPS I-Type Encoding:**
```
LW R5, 100(R6)
```
- Opcode (LW) = 100011
- Rs (R6) = 00110
- Rt (R5) = 00101
- Immediate (100) = 0000000001100100

**Binary: 100011 00110 00101 0000000001100100**
**Hex: 0x8CC50064**

**MIPS J-Type Encoding:**
```
J 0x00400020
```
- Opcode (J) = 000010
- Address = 0x00400020 / 4 = 0x00100008
- Lower 26 bits of word address = 00100008₁₆ = 00 0001 0000 0000 0000 0000 1000

**Binary: 000010 00000100000000000000001000**
**Hex: 0x08100008**

**Decoding Practice:**
**Q:** Decode the instruction 0x01095020.
1. Binary: 000000 01000 01001 01010 00000 100000
2. Opcode = 000000 → R-type
3. Rs = 01000 = R8, Rt = 01001 = R9, Rd = 01010 = R10
4. Shamt = 00000, Funct = 100000 = ADD
5. **Answer: ADD R10, R8, R9**

### 3.13 Effects of ISA on Program Size ⭐

**Example:** Compute R1 = M[R2+100] + R3 on different ISAs:

| ISA Type | Assembly | Instructions | Memory accesses |
|---|---|---|---|
| 3-address reg-mem | ADD R1, 100(R2), R3 | 1 | 2 (fetch+operand) |
| Load-Store (RISC) | LW R4, 100(R2); ADD R1, R4, R3 | 2 | 3 (2 fetch+1 operand) |
| 1-address (accumulator) | LOAD 100(R2); ADD R3; STORE R1 | 3 | 5 |
| 0-address (stack) | PUSH M[R2+100]; PUSH R3; ADD; POP R1 | 4 | 6+ |

> 🎯 **Trade-off:** More powerful addressing modes → fewer instructions → shorter programs BUT more complex hardware, harder to pipeline, longer clock cycle.

### 3.14 Compiler Optimization Techniques (ISA Impact) ⭐

#### Quick Visual 🧩

```
Compiler optimizations -> lower IC/CPI

Register allocation -> fewer memory ops
Instruction scheduling -> fewer stalls
Loop unrolling -> fewer branches
```

| Technique | Description | ISA Requirement |
|---|---|---|
| Register allocation | Keep frequently used variables in registers | Many registers (RISC) |
| Instruction scheduling | Reorder to avoid pipeline stalls | Visible pipeline (RISC) |
| Loop unrolling | Expand loop body to reduce iteration overhead | Large instruction cache |
| Function inlining | Replace function call with body | Code size tolerance |
| Strength reduction | Replace expensive ops with cheaper ones | Good ALU |
| Dead code elimination | Remove unreachable code | Static analysis |

### 3.15 Program Performance Analysis ⭐⭐

#### Quick Visual 🧩

```
Instruction mix -> weighted CPI -> CPU time

Workload profile
  |
  v
Avg CPI = sum(CPI_i x fraction_i)
  |
  v
Time = IC x CPI / f
```

**Example: Complete Performance Analysis**
A program has this instruction profile on Processor A (4 GHz, 5-stage pipeline):

| Type | Count | Base CPI | Cache Stalls | Pipeline Stalls |
|---|---|---|---|---|
| Integer ALU | 400M | 1 | 0 | 0.10 |
| FP ALU | 100M | 3 | 0 | 0.50 |
| Load | 200M | 1 | 0.20 × 100 = 20 | 0.30 |
| Store | 100M | 1 | 0.05 × 100 = 5 | 0 |
| Branch | 200M | 1 | 0 | 0.20 × 2 = 0.40 |

**Step 1: Compute weighted CPI**
| Type | Fraction | Effective CPI | Contribution |
|---|---|---|---|
| Integer ALU | 0.40 | 1 + 0 + 0.10 = 1.10 | 0.44 |
| FP ALU | 0.10 | 3 + 0 + 0.50 = 3.50 | 0.35 |
| Load | 0.20 | 1 + 20 + 0.30 = 21.30 | 4.26 |
| Store | 0.10 | 1 + 5 + 0 = 6.00 | 0.60 |
| Branch | 0.20 | 1 + 0 + 0.40 = 1.40 | 0.28 |

**Total CPI = 0.44 + 0.35 + 4.26 + 0.60 + 0.28 = 5.93**

**Step 2: Execution time**
T = 10⁹ × 5.93 / (4 × 10⁹) = **1.48 seconds**

**Step 3: Bottleneck analysis**
- Load cache misses contribute 4.26/5.93 = **71.8% of CPI!**
- If we could make cache perfect: CPI = 5.93 − (4.26 − 0.2×0.30 − 0.2×1) = wait...
  Without cache stalls: Load CPI = 1+0.30 = 1.30 → contribution = 0.20 × 1.30 = 0.26
  Store CPI = 1+0 = 1.0 → contribution = 0.10 × 1.0 = 0.10
  New CPI = 0.44 + 0.35 + 0.26 + 0.10 + 0.28 = **1.43**
  Speedup from perfect cache = 5.93/1.43 = **4.15×**

> 🎯 **This analysis shows why cache optimization is the #1 priority in processor design!**

### 3.16 Comparative Analysis: Real ISAs ⭐

| Feature | MIPS | x86 | ARM (v8) | RISC-V |
|---|---|---|---|---|
| Type | RISC | CISC | RISC | RISC |
| Registers | 32 × 32-bit | 16 × 64-bit | 31 × 64-bit | 32 × 32/64-bit |
| Instruction size | Fixed 32-bit | Variable 1-15 bytes | Fixed 32-bit (A64) | Variable 16/32-bit |
| Addressing modes | 3 | 10+ | 5 | 3 |
| Endianness | Big (usually) | Little | Bi (configurable) | Little |
| Load/Store? | Yes | No (mem-mem ops) | Yes | Yes |
| SIMD | Limited | SSE/AVX (512-bit) | NEON (128-bit) | Vector extension |
| Condition codes | No flags | EFLAGS register | NZCV flags | No flags |
| Status | Educational, embedded | Desktop/server dominant | Mobile/embedded dominant | Growing (open-source ISA) |

**Why RISC-V matters:**
- Open-source ISA — no licensing fees
- Modular: base integer ISA + extensions (M=multiply, F=float, A=atomic, V=vector)
- Used in: embedded, AI accelerators, research
- GATE may start including RISC-V questions in future!
**Speedup = T_old / T_new**

> ⚠️ **GATE Note:** MIPS is NOT a reliable performance metric because different ISAs have different instruction complexities!

### 3.17 Amdahl's Law ⭐⭐⭐

#### Quick Visual 🧩

```
Program = serial part + improvable part

Even infinite speedup on improvable part
cannot beat limit = 1 / serial_fraction
```

If a fraction **f** of a program can be sped up by a factor **S**, the overall speedup is:

$$\text{Speedup}_{\text{overall}} = \frac{1}{(1 - f) + \frac{f}{S}}$$

**Key Insights:**
- If f = 0.9 and S = 10: Overall speedup = 1/(0.1 + 0.09) = **5.26x** (not 10x!)
- Maximum possible speedup (S → ∞): Speedup_max = 1/(1 − f)
- If only 50% is parallelizable → max speedup = 2x regardless of cores

**Worked Example:** A program spends 60% time on FP operations. New FP unit is 8× faster.
- Speedup = 1 / (0.4 + 0.6/8) = 1 / (0.4 + 0.075) = 1/0.475 = **2.105×**
- Max speedup (if FP were infinitely fast) = 1/0.4 = **2.5×**

### 3.18 Gustafson's Law (Scaled Speedup) ⭐

$$\text{Speedup} = s + p \times N$$
where s = serial fraction, p = parallel fraction, N = number of processors, and s + p = 1.

Unlike Amdahl's (fixed problem size), Gustafson assumes problem size scales with processors.

**Amdahl vs Gustafson:**
| Feature | Amdahl's Law | Gustafson's Law |
|---|---|---|
| Problem size | Fixed | Scales with processors |
| Perspective | How much faster? | How much more work? |
| Pessimistic/Optimistic | Pessimistic | Optimistic |
| Formula | 1/((1-f)+f/S) | s + p×N |

### 3.19 Performance Equations Summary ⭐

#### Quick Visual 🧩

```
Iron law:
Time/Program = (Instructions/Program)
             x (Cycles/Instruction)
             x (Seconds/Cycle)
```

| Metric | Formula | Units |
|---|---|---|
| CPU Time | IC × CPI × Clock Period | seconds |
| CPU Time | IC × CPI / Clock Rate | seconds |
| CPI | Σ(CPIᵢ × Fᵢ) | cycles/instr |
| MIPS | Clock Rate / (CPI × 10⁶) | million instr/sec |
| MFLOPS | FP ops / (time × 10⁶) | million FP ops/sec |
| Speedup | T_old / T_new | dimensionless |

**Iron Law of Performance:**
$$\text{Time/Program} = \frac{\text{Instructions}}{\text{Program}} \times \frac{\text{Cycles}}{\text{Instruction}} \times \frac{\text{Seconds}}{\text{Cycle}}$$

> 🎯 **What affects each factor?**
> - Instructions/Program → Compiler, ISA (RISC vs CISC)
> - Cycles/Instruction (CPI) → Pipeline, cache, branch prediction
> - Seconds/Cycle (clock period) → Technology, circuit design

---

## 4. CPU Design & Datapath ⚙️

### 4.1 Register Set ⭐
#### Quick Visual 🧩

```
Instruction flow registers:
PC -> MAR -> Memory -> MDR -> IR

Execution helpers:
ACC (ALU result), SP (stack top), PSW (flags)
```

- **PC (Program Counter):** Address of next instruction
- **IR (Instruction Register):** Current instruction being executed
- **MAR (Memory Address Register):** Address to access memory
- **MDR/MBR (Memory Data Register):** Data to/from memory
- **ACC (Accumulator):** Result of ALU operations
- **SP (Stack Pointer):** Top of stack
- **PSW/Flags:** Status flags (Zero, Carry, Overflow, Sign)
- **FP (Frame Pointer):** Base of current stack frame

**Register File Organization:**
| Feature | MIPS | x86 | ARM |
|---|---|---|---|
| GPR count | 32 (R0-R31, R0=0) | 8 (16 in x86-64) | 16 (R0-R15) |
| Width | 32-bit | 32/64-bit | 32/64-bit |
| Special | R0 hardwired to 0 | ESP=stack, EBP=frame | R13=SP, R14=LR, R15=PC |

### 4.2 Control Unit Types ⭐⭐

**1. Hardwired Control Unit:**
- Combinational logic (gates, decoders, encoders)
- **Advantages:** Fast (no memory access for control)
- **Disadvantages:** Inflexible — changing ISA requires hardware redesign
- Used in: RISC processors

```
IR[opcode] → Decoder + Logic Network → Control Signals
Flags ──────→
Clock ──────→
```

**2. Microprogrammed Control Unit:**
- Control signals stored as **microinstructions** in **Control Memory (CM)**
- Each machine instruction maps to a sequence of microinstructions (firmware)
- **Advantages:** Flexible — change behavior by updating control memory
- **Disadvantages:** Slower (memory access needed)
- Used in: CISC processors

```
IR[opcode] → Mapping ROM → µPC → Control Memory → Control Signals
```

**Comparison:**
| Feature | Hardwired | Microprogrammed |
|---|---|---|
| Speed | Fast | Slower |
| Flexibility | Rigid | Easy to modify |
| Design | Complex logic | Simpler (sequential) |
| Typical use | RISC | CISC |

### 4.3 Microprogramming Details ⭐⭐

#### Quick Visual 🧩

```
Opcode -> Mapping -> micro-address -> Control Memory -> control signals

Horizontal: wide, direct, faster
Vertical: narrow, encoded, slower
```

**Horizontal vs Vertical Microprogramming:**

| Feature | Horizontal | Vertical |
|---|---|---|
| Width | Wide (50-200+ bits) | Narrow (15-40 bits) |
| Decoding | None (direct control) | Need decoder |
| Speed | Faster | Slower |
| Parallelism | High | Limited |
| Memory | Fewer words × more bits | More words × fewer bits |

**Nano-Programming:** Second-level microstore. Horizontal nano-instructions in nano-memory; vertical micro-instructions point to them. Saves space when many micro-instructions are identical.

### 4.4 Mano's Basic Computer — Complete Design ⭐⭐⭐

> 🏭 **From Morris Mano textbook (Chapter 5) — the most asked CPU design in GATE.** This is a simple 16-bit computer with 25 instructions.

#### Basic Computer Architecture 🏗️

```
┌─────────────────────────────────────────────────┐
│              BASIC COMPUTER                      │
│                                                  │
│   ┌─────┐   ┌─────┐   ┌──────────┐             │
│   │ PC  │   │ AR  │   │ Memory   │             │
│   │(12) │   │(12) │   │ 4096×16  │             │
│   └──┬──┘   └──┬──┘   └────┬─────┘             │
│      │         │            │                    │
│   ═══╪═════════╪════════════╪═══  (Common Bus)  │
│      │         │            │                    │
│   ┌──┴──┐   ┌──┴──┐   ┌────┴────┐              │
│   │ DR  │   │ IR  │   │  AC     │              │
│   │(16) │   │(16) │   │ (16)    │              │
│   └─────┘   └─────┘   └────┬────┘              │
│                             │                    │
│                       ┌─────┴─────┐              │
│   ┌─────┐            │   ALU     │              │
│   │ TR  │            │ (Add,AND, │              │
│   │(16) │            │ Comp,SHR, │              │
│   └─────┘            │ SHL,INC)  │              │
│                       └───────────┘              │
│   ┌─────┐   ┌─────┐                            │
│   │INPR │   │OUTR │   E (flip-flop)            │
│   │ (8) │   │ (8) │   I (indirect bit)          │
│   └─────┘   └─────┘                            │
└─────────────────────────────────────────────────┘
```

**Registers:**
| Register | Bits | Function |
|---|---|---|
| AC (Accumulator) | 16 | Main operand register |
| DR (Data Register) | 16 | Memory operand holder |
| AR (Address Register) | 12 | Memory address |
| PC (Program Counter) | 12 | Next instruction address |
| IR (Instruction Register) | 16 | Current instruction |
| TR (Temporary Register) | 16 | Temporary storage |
| INPR (Input Register) | 8 | Input device data |
| OUTR (Output Register) | 8 | Output device data |
| E (Extended bit) | 1 | Carry flip-flop for ALU |

#### Instruction Format 📋

```
Memory Reference:  | I(1) | Opcode(3) | Address(12) |
Register/IO:       | 0/1  | 111       | Operation(12) |
```

- **I = 0:** Direct addressing (EA = address field)
- **I = 1:** Indirect addressing (EA = M[address field])

#### Complete Instruction Set (25 Instructions) 📋

**Memory-Reference Instructions (Opcode 000-110):**
| Opcode | Symbol | I=0 | I=1 | RTL (I=0) |
|---|---|---|---|---|
| 000 | AND | Direct AND | Indirect AND | AC ← AC ∧ M[AR] |
| 001 | ADD | Direct ADD | Indirect ADD | AC ← AC + M[AR], E ← Carry |
| 010 | LDA | Direct Load | Indirect Load | AC ← M[AR] |
| 011 | STA | Direct Store | Indirect Store | M[AR] ← AC |
| 100 | BUN | Direct Jump | Indirect Jump | PC ← AR |
| 101 | BSA | Direct Call | Indirect Call | M[AR] ← PC, PC ← AR+1 |
| 110 | ISZ | Direct ISZ | Indirect ISZ | M[AR] ← M[AR]+1, if(M[AR]+1=0) PC++ |

**Register-Reference Instructions (Opcode=111, I=0):**
| Bit | Symbol | Operation |
|---|---|---|
| B₁₁ | CLA | AC ← 0 (Clear AC) |
| B₁₀ | CLE | E ← 0 (Clear E) |
| B₉ | CMA | AC ← AC' (Complement AC) |
| B₈ | CME | E ← E' (Complement E) |
| B₇ | CIR | Circular right shift (AC, E) |
| B₆ | CIL | Circular left shift (AC, E) |
| B₅ | INC | AC ← AC + 1 |
| B₄ | SPA | if (AC > 0) PC ← PC + 1 (Skip if positive) |
| B₃ | SNA | if (AC < 0) PC ← PC + 1 (Skip if negative) |
| B₂ | SZA | if (AC = 0) PC ← PC + 1 (Skip if zero) |
| B₁ | SZE | if (E = 0) PC ← PC + 1 (Skip if E zero) |
| B₀ | HLT | Halt computer |

**Input-Output Instructions (Opcode=111, I=1):**
| Bit | Symbol | Operation |
|---|---|---|
| B₁₁ | INP | AC[0-7] ← INPR, FGI ← 0 |
| B₁₀ | OUT | OUTR ← AC[0-7], FGO ← 0 |
| B₉ | SKI | if (FGI = 1) PC ← PC + 1 |
| B₈ | SKO | if (FGO = 1) PC ← PC + 1 |
| B₇ | ION | IEN ← 1 (Interrupt Enable) |
| B₆ | IOF | IEN ← 0 (Interrupt Disable) |

#### Instruction Cycle — Fetch, Decode, Execute 🔄

```
                    ┌──────────────┐
                    │   START      │
                    └──────┬───────┘
                           ▼
               ┌───────────────────────┐
    T₀:        │ AR ← PC               │  Fetch
               └───────────┬───────────┘
                           ▼
               ┌───────────────────────┐
    T₁:        │ IR ← M[AR], PC ← PC+1│  Fetch
               └───────────┬───────────┘
                           ▼
               ┌───────────────────────┐
    T₂:        │ Decode IR[12-14]      │  Decode
               │ AR ← IR[0-11]         │  (address part)
               │ I ← IR[15]            │  (indirect bit)
               └───────────┬───────────┘
                           ▼
                    ┌──────┴──────┐
                    │ D₇ = 1?     │─── Yes → Register or I/O
                    └──────┬──────┘
                     No    │
                           ▼
                    ┌──────┴──────┐
              T₃:   │ I = 1?      │─── Yes → AR ← M[AR] (indirect)
                    └──────┬──────┘
                     No    │
                           ▼
              T₃:   Execute memory-reference instruction
```

**RTL for Fetch Cycle (Common to ALL instructions):**
```
T₀: AR ← PC
T₁: IR ← M[AR], PC ← PC + 1
T₂: D₀...D₇ ← Decode(IR[12-14]), AR ← IR[0-11], I ← IR[15]
T₃: (D₇'·I·T₃): AR ← M[AR]     (indirect addressing)
```

> 🎯 **GATE Favorite:** "How many clock cycles does instruction X take?" Count T₀ through Tₙ. Memory-reference = 4-6 cycles. Register = 3 cycles. I/O = 3 cycles.

**Timing for each instruction:**
| Instruction | Direct (I=0) | Indirect (I=1) |
|---|---|---|
| AND | 5 cycles (T₀-T₄) | 6 cycles (T₀-T₅) |
| ADD | 5 cycles | 6 cycles |
| LDA | 5 cycles | 6 cycles |
| STA | 5 cycles | 6 cycles |
| BUN | 4 cycles | 5 cycles |
| BSA | 5 cycles | 6 cycles |
| ISZ | 6 cycles | 7 cycles |
| Register ref | 3 cycles (T₀-T₂ + execute at T₃) | — |
| I/O | 3 cycles | — |

#### Interrupt Cycle in Basic Computer 🔔

```
R (Interrupt flip-flop):
  At end of instruction cycle:
    if (IEN = 1 AND (FGI = 1 OR FGO = 1)):
      R ← 1

  Interrupt cycle (R = 1):
    T₀: AR ← 0, TR ← PC
    T₁: M[AR] ← TR, PC ← 0
    T₂: PC ← PC + 1, IEN ← 0, R ← 0, SC ← 0
    
  Effect: PC saved at M[0], execution resumes at address 1
  (ISR starts at memory location 1)
```

> 🎯 **GATE Trap:** Basic computer saves return address at M[0] and jumps to M[1]. This is NOT a stack-based save! Only ONE level of interrupt is supported (no nesting).

### 4.5 Hardwired Control Unit — Detailed Design ⭐⭐⭐

> 🏭 **This is the hardware implementation of the control unit — generates control signals using combinational logic.**

#### Control Unit Block Diagram
```
                    ┌───────────────────────────────┐
  IR[12-14] ──→    │  3×8 Decoder (Opcode)          │──→ D₀-D₇
                    └───────────────────────────────┘
                    
  SC (Sequence     ┌───────────────────────────────┐
  Counter) ──→     │  4×16 Decoder (Timing)         │──→ T₀-T₁₅
                    └───────────────────────────────┘
                    
                    ┌───────────────────────────────┐
  D₀-D₇  ──→      │                                │
  T₀-T₁₅ ──→      │  Combinational Logic Network   │──→ Control Signals
  I       ──→      │  (AND/OR gates implementing    │    (Read, Write,
  Flags   ──→      │   Boolean expressions)         │    ALU ops, etc.)
                    └───────────────────────────────┘
```

#### Control Signal Boolean Expressions 📝

Each control signal is a Boolean function of decoder outputs and timing signals:

**Memory Read signal (for basic computer):**
$$\text{Read} = T_1 + (D_7' \cdot I \cdot T_3) + D_0 \cdot T_4 + D_1 \cdot T_4 + D_2 \cdot T_4 + D_6 \cdot T_4$$

**AR Load signal:**
$$\text{AR\_load} = T_0 + T_2 + (D_7' \cdot I \cdot T_3)$$

**PC Increment signal:**
$$\text{PC\_inc} = T_1$$

**AC Clear signal:**
$$\text{AC\_clr} = D_7 \cdot I' \cdot T_3 \cdot B_{11}$$

**General pattern:** Each control signal = OR of all timing conditions that activate it.

#### Hardwired CU Design Steps
1. List all RTL micro-operations for every instruction at every timing state
2. For each control signal, collect all conditions that enable it
3. Write Boolean expression (OR of ANDs)
4. Implement with logic gates

**Advantage:** Very fast — no memory access, pure combinational logic
**Disadvantage:** Any change to instruction set requires redesigning the entire logic network

> 🎯 **GATE Insight:** Hardwired CU is used in RISC (simple, regular instruction set makes logic feasible). CISC uses microprogrammed CU because the complex instruction set would require enormous logic networks.

### 4.6 Microprogrammed Control Unit — Detailed Design ⭐⭐⭐

> 💡 **Instead of logic gates, we store the control signals as data in a special memory called Control Memory (CM).**

#### Microprogrammed CU Architecture
```
┌─────────────────────────────────────────────────┐
│               Microprogrammed CU                 │
│                                                  │
│  ┌────────────┐     ┌─────────────────────┐     │
│  │  µPC       │────→│  Control Memory      │     │
│  │(Micro PC)  │     │  (ROM/PROM)          │     │
│  └─────┬──────┘     │                      │     │
│        │            │  Each word = one       │     │
│  ┌─────▼──────┐     │  micro-instruction     │     │
│  │ Incrementer│     │  with control signals  │     │
│  │   + MUX    │     │  + next address info   │     │
│  └─────┬──────┘     └──────────┬────────────┘     │
│        │                       │                   │
│  ┌─────▼──────────────────────▼──────────┐        │
│  │     Micro-Instruction Register (µIR)   │        │
│  │  ┌────────────────┬──────────────────┐ │        │
│  │  │ Control Field  │ Address Field     │ │        │
│  │  │ (signals to    │ (next µaddr or    │ │        │
│  │  │  datapath)     │  branch condition)│ │        │
│  │  └────────────────┴──────────────────┘ │        │
│  └────────────────────────────────────────┘        │
│                                                    │
│  IR[opcode] → Mapping Logic → Starting µaddress   │
└─────────────────────────────────────────────────┘
```

#### Micro-Instruction Format (Horizontal) 📋

```
| F1(3) | F2(3) | F3(3) | CD(2) | BR(2) | AD(7) |
  ↓       ↓       ↓       ↓       ↓       ↓
 ALU    ALU2    Reg     Cond   Branch  Next
 ops    ops     ops     select  type   Address
```

**F1 Field (3 bits → 8 micro-operations):**
| Code | Symbol | Micro-operation |
|---|---|---|
| 000 | NOP | No operation |
| 001 | SUB | AC ← AC − DR |
| 010 | OR | AC ← AC ∨ DR |
| 011 | AND | AC ← AC ∧ DR |
| 100 | READ | DR ← M[AR] |
| 101 | ADD | AC ← AC + DR |
| 110 | WRITE | M[AR] ← DR |
| 111 | ACTDR | DR ← AC |

**F2 Field (3 bits):**
| Code | Symbol | Micro-operation |
|---|---|---|
| 000 | NOP | No operation |
| 001 | INC | AC ← AC + 1 |
| 010 | COM | AC ← AC' |
| 011 | SHR | shr AC, E |
| 100 | SHL | shl AC, E |
| 101 | INCPC | PC ← PC + 1 |
| 110 | ARTPC | PC ← AR |
| 111 | PCTAR | AR ← PC |

**F3 Field (3 bits):**
| Code | Symbol | Micro-operation |
|---|---|---|
| 000 | NOP | No operation |
| 001 | XOR | AC ← AC ⊕ DR |
| 010 | DRTAC | AC ← DR |
| 011 | TSAR | AR ← IR[0-11] (transfer address) |
| 100 | DRTAR | AR ← DR[0-11] |
| 101 | PCPLS | PC ← PC + 1 |
| 110 | CLRAC | AC ← 0 |
| 111 | INCDR | DR ← DR + 1 |

**CD (Condition) Field:**
| Code | Condition | Test |
|---|---|---|
| 00 | Always | Unconditional |
| 01 | I | Indirect bit |
| 10 | S | Sign of AC (AC[15]) |
| 11 | Z | Zero flag (AC = 0) |

**BR (Branch) Field:**
| Code | Symbol | Action |
|---|---|---|
| 00 | JMP | µPC ← AD (jump) |
| 01 | CALL | Save µPC, µPC ← AD |
| 10 | RET | µPC ← saved address |
| 11 | MAP | µPC ← mapping(IR opcode) |

#### Mapping Function 🗺️

The mapping function converts machine instruction opcode to starting microaddress in control memory:

**For Mano's Basic Computer:**
```
Opcode (3 bits): X₂ X₁ X₀
Mapped µaddress: 0 X₂ X₁ X₀ 00   (7 bits)
```

| Opcode | Instruction | Starting µaddress |
|---|---|---|
| 000 | AND | 0000000 (0) |
| 001 | ADD | 0000100 (4) |
| 010 | LDA | 0001000 (8) |
| 011 | STA | 0001100 (12) |
| 100 | BUN | 0010000 (16) |
| 101 | BSA | 0010100 (20) |
| 110 | ISZ | 0011000 (24) |

**The 2 trailing zeros leave room for 4 microinstructions per machine instruction.** If more are needed, the microprogram can jump to unused areas of control memory.

#### Microprogram Example — ADD Instruction 📝

```
; Fetch cycle (common to all instructions):
µaddr 64:  PCTAR      ; AR ← PC                (F2=111)
µaddr 65:  READ, INCPC ; DR ← M[AR], PC ← PC+1 (F1=100, F2=101)
µaddr 66:  TSAR       ; AR ← IR[0-11]          (F3=011)
           MAP        ; µPC ← opcode mapping    (BR=11)

; ADD microprogram (starts at µaddr 4):
µaddr 4:   NOP, CD=I, BR=JMP, AD=indirect_handler  ; if I=1 goto indirect
µaddr 5:   READ       ; DR ← M[AR]             (F1=100)
µaddr 6:   ADD        ; AC ← AC + DR           (F1=101)
           JMP 64     ; goto fetch              (BR=00, AD=64)
```

> 🎯 **GATE Fact:** Each machine instruction requires a **microprogram** — a sequence of microinstructions stored in control memory. The fetch cycle is shared (subroutine-like) across all instructions.

#### Control Memory Size Calculation ⭐⭐

**Formula:** CM size = Number of words × Word width

**Example:** 128 words × 20 bits = 2560 bits of control memory

**For the format above:** 3+3+3+2+2+7 = 20 bits per microinstruction, 128 words → 2560 bits

**Comparison:**
| Parameter | Horizontal | Vertical |
|---|---|---|
| Word width | 20+ bits (direct signals) | 10-15 bits (encoded) |
| Number of words | Fewer (more parallelism) | More (sequential) |
| Decoder needed? | No (each bit = 1 signal) | Yes (decode encoded fields) |
| Speed | Fast (1 control memory access) | Slower (access + decode) |
| CM size | Wide × few words | Narrow × many words |

### 4.7 General Register Organization ⭐⭐

> 💡 **From Mano Chapter 7 — a CPU with multiple general-purpose registers connected through a common bus to an ALU.**

```
┌──────────────────────────────────────────────────┐
│           General Register Organization           │
│                                                    │
│  R0 ──→ ┐                                         │
│  R1 ──→ │  MUX A               MUX B              │
│  R2 ──→ │  (SELA)              (SELB)             │
│  R3 ──→ │     │                    │               │
│  R4 ──→ │     ▼                    ▼               │
│  R5 ──→ │  ┌──────┐          ┌──────┐             │
│  R6 ──→ │  │Bus A │          │Bus B │             │
│  R7 ──→ ┘  └──┬───┘          └──┬───┘             │
│               │                   │                │
│               └──────┬───────────┘                 │
│                  ┌───▼───┐                         │
│                  │  ALU  │ ← OPR (operation select)│
│                  └───┬───┘                         │
│                      │                             │
│                  ┌───▼───┐                         │
│                  │Output │                         │
│                  └───┬───┘                         │
│                      │                             │
│    SELD ──→ Decoder ──→ Load appropriate register  │
└──────────────────────────────────────────────────┘
```

**Control Word Format:**
```
| SELA (3) | SELB (3) | SELD (3) | OPR (5) |
   Source A    Source B   Destination  ALU Op
```

**Total control word width = 3 + 3 + 3 + 5 = 14 bits**

**SELA/SELB Encoding (for 8 registers):**
| Code | Register | Description |
|---|---|---|
| 000 | Input | External input (for load) |
| 001 | R1 | Register 1 |
| 010 | R2 | Register 2 |
| ... | ... | ... |
| 111 | R7 | Register 7 |

**OPR Encoding (ALU operations):**
| Code | Symbol | Micro-operation |
|---|---|---|
| 00000 | TSFA | Transfer A (pass through) |
| 00001 | INCA | Increment A |
| 00010 | ADD | A + B |
| 00101 | SUB | A − B |
| 00110 | DECA | Decrement A |
| 01000 | AND | A ∧ B |
| 01010 | OR | A ∨ B |
| 01100 | XOR | A ⊕ B |
| 01110 | COMA | Complement A |
| 10000 | SHR | Shift right A |
| 11000 | SHL | Shift left A |

**Example micro-operation:** R3 ← R1 + R2
- SELA = 001 (R1), SELB = 010 (R2), SELD = 011 (R3), OPR = 00010 (ADD)
- Control word: **001 010 011 00010**

> 🎯 **GATE Insight:** With n registers, SELA/SELB/SELD each need ⌈log₂(n+1)⌉ bits (+1 for external input). For 8 registers → 3 bits each.

### 4.8 Stack Organization ⭐⭐

> 💡 **A stack is a LIFO (Last-In, First-Out) data structure. CPUs use stacks for subroutine calls, interrupt handling, and expression evaluation.**

#### Register Stack (Hardware Stack) 🔧

```
┌───────────────────────────────┐
│      Register Stack            │
│                                │
│  ┌─────┐                      │
│  │ SP  │→ Points to TOS       │
│  └─────┘                      │
│                                │
│  ┌─────┐  ←── SP (top)       │
│  │  C  │  Address 3           │
│  ├─────┤                      │
│  │  B  │  Address 2           │
│  ├─────┤                      │
│  │  A  │  Address 1           │
│  ├─────┤                      │
│  │     │  Address 0           │
│  └─────┘                      │
│                                │
│  FULL flag: SP = max          │
│  EMPTY flag: SP = 0           │
└───────────────────────────────┘
```

**PUSH Operation:**
```
SP ← SP + 1
M[SP] ← DR
if (SP = 0) then FULL ← 1
EMPTY ← 0
```

**POP Operation:**
```
DR ← M[SP]
SP ← SP − 1
if (SP = 0) then EMPTY ← 1
FULL ← 0
```

> 🎯 **GATE Fact:** Register stack has fixed size (typically 64-256 words). Overflow causes data loss or exception!

#### Memory Stack 📚

Uses a region of main memory as stack. SP register points to TOS.

**Convention (growing downward — most common):**
```
High address
┌──────────────┐
│   Stack base  │  ← Initial SP value (e.g., 0xFFF)
├──────────────┤
│   Data A      │
├──────────────┤
│   Data B      │
├──────────────┤  ← SP (current top)
│              │
│   (empty)     │
│              │
└──────────────┘
Low address
```

**PUSH:** SP ← SP − 1; M[SP] ← data
**POP:** data ← M[SP]; SP ← SP + 1

> ⚠️ **GATE Trap:** Some architectures grow UP (SP increments on push). Always check the convention!

**Stack expressions evaluation — complete example:**
Evaluate: (3 + 4) × (5 − 2) using a stack machine

Postfix: 3 4 + 5 2 − ×

```
Instruction    Stack           Comment
PUSH 3        [3]
PUSH 4        [3, 4]
ADD            [7]             3+4=7
PUSH 5        [7, 5]
PUSH 2        [7, 5, 2]
SUB            [7, 3]          5−2=3
MUL            [21]            7×3=21
```
Result = 21 ✅

### 4.9 Arithmetic, Logic & Shift Micro-Operations — Complete Reference ⭐⭐

> 💡 **From Mano Chapter 4 — these are the atomic operations that the ALU performs in a single clock cycle.**

#### Arithmetic Micro-Operations 🔢

| Symbol | Operation | Description |
|---|---|---|
| R3 ← R1 + R2 | Addition | Binary add |
| R3 ← R1 − R2 | Subtraction | Using 2's complement: R1 + R2' + 1 |
| R1 ← R1 + 1 | Increment | Add 1 |
| R1 ← R1 − 1 | Decrement | Subtract 1 |
| R1 ← R2' + 1 | Negate | 2's complement |
| R1 ← R2' | 1's Complement | Bitwise NOT |
| R3 ← R1 + R2 + 1 | Add with carry | Used for multi-word addition |

**Hardware for Add/Subtract:**
```
     R1          R2
      │           │
      │     ┌─────┴─────┐
      │     │ MUX (M)    │  M=0: pass R2
      │     │            │  M=1: pass R2'
      │     └─────┬──────┘
      │           │
  ┌───▼───────────▼───┐
  │   Binary Adder     │←── Cin = M (for subtract: add R2'+1)
  └───────┬───────────┘
          │
          ▼
       Result
```

**With M=0, Cin=0:** R1 + R2 (add)
**With M=0, Cin=1:** R1 + R2 + 1 (add with carry)
**With M=1, Cin=0:** R1 + R2' (subtract − 1... rarely used)
**With M=1, Cin=1:** R1 + R2' + 1 = R1 − R2 (subtract!)

#### Logic Micro-Operations ⭐

| Symbol | Operation | Application |
|---|---|---|
| R1 ← R1 ∧ R2 | AND | Masking (clear specific bits) |
| R1 ← R1 ∨ R2 | OR | Setting specific bits |
| R1 ← R1 ⊕ R2 | XOR | Toggle bits, complement selective bits |
| R1 ← R1' | NOT | Complement all bits |

**Bit Manipulation using Logic Operations:**

| Task | Operation | Mask | Example (8-bit) |
|---|---|---|---|
| Clear bits 3-0 | AND | 11110000 | 10110101 AND 11110000 = 10110000 |
| Set bits 7-4 | OR | 11110000 | 00001010 OR 11110000 = 11111010 |
| Toggle bit 5 | XOR | 00100000 | 10110101 XOR 00100000 = 10010101 |
| Test if bit 3=1 | AND | 00001000 | If result ≠ 0, bit 3 was 1 |

> 🎯 **GATE Application:** Logic micro-ops are used for bit manipulation, field extraction, and flag testing — very common in systems programming questions.

#### Shift Micro-Operations ⭐

**Three Types of Shifts:**

```
Logical Shift Left (SHL):
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 ← d₆ ← d₅ ← d₄ ← d₃ ← d₂ ← d₁ ← d₀ │ ← 0
└───┴───┴───┴───┴───┴───┴───┴───┘
MSB goes to Carry, 0 enters LSB

Logical Shift Right (SHR):
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 → d₇ → d₆ → d₅ → d₄ → d₃ → d₂ → d₁ │ → Carry
└───┴───┴───┴───┴───┴───┴───┴───┘
0 enters MSB, LSB goes to Carry

Arithmetic Shift Right (ASHR):
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ d₇ → d₇ → d₆ → d₅ → d₄ → d₃ → d₂ → d₁ │ → Carry
└───┴───┴───┴───┴───┴───┴───┴───┘
Sign bit preserved (MSB stays same)!

Circular Shift Left (Rotate Left):
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ MSB ← d₆ ← d₅ ← d₄ ← d₃ ← d₂ ← d₁ ← d₀ │ ← MSB
└───┴───┴───┴───┴───┴───┴───┴───┘
MSB wraps around to LSB (no data lost!)

Rotate Through Carry (RCL):
┌───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ C ← d₇ ← d₆ ← ... ← d₁ ← d₀ │ ← C
└───┴───┴───┴───┴───┴───┴───┴───┴───┘
Carry bit participates in rotation (n+1 bit rotation)
```

**Shift Summary Table:**
| Shift Type | Left | Right | Fill Value |
|---|---|---|---|
| Logical | SHL | SHR | 0 |
| Arithmetic | ASHL (=SHL) | ASHR | Sign bit (MSB) |
| Circular (Rotate) | ROL | ROR | Bit from other end |
| Rotate through Carry | RCL | RCR | Carry flag |

> 🎯 **GATE Fact:** Arithmetic shift left is SAME as logical shift left. Only arithmetic shift RIGHT is different (preserves sign for signed division by 2).

**Shift Applications:**
- **SHL by n** = multiply by 2ⁿ (unsigned and signed)
- **SHR by n** = unsigned divide by 2ⁿ
- **ASHR by n** = signed divide by 2ⁿ (rounds toward −∞, not toward 0!)

> ⚠️ **GATE Trap:** ASHR of −7 (11111001) by 1 = 11111100 = −4, NOT −3! It rounds toward −∞. For truncation toward 0, add (2ⁿ−1) before shifting if the number is negative.

### 4.10 Register Transfer Language (RTL) & Micro-Operations ⭐⭐
#### Quick Visual 🧩

```
RTL describes data movement per clock:

T0: source register -> bus
T1: ALU operation
T2: destination register <- result
```

```
T₀: MAR ← PC
T₁: MDR ← M[MAR], PC ← PC + 4
T₂: IR ← MDR
T₃: A ← R[rs1], B ← R[rs2]
T₄: ALUOut ← A + B
T₅: R[rd] ← ALUOut
```

**LOAD R1, offset(R2):**
```
T₀: MAR ← PC
T₁: MDR ← M[MAR], PC ← PC + 4
T₂: IR ← MDR
T₃: A ← R[rs], B ← sign_extend(offset)
T₄: ALUOut ← A + B
T₅: MDR ← M[ALUOut]
T₆: R[rt] ← MDR
```

**STORE R1, offset(R2):**
```
T₀-T₄: Same as LOAD (address calculation)
T₅: MDR ← R[rt]; M[ALUOut] ← MDR
```

**BEQ R1, R2, target:**
```
T₀-T₂: Fetch + Decode
T₃: A ← R[rs], B ← R[rt], ALUOut ← PC + sign_extend(offset)×4
T₄: if (A == B) then PC ← ALUOut
```

### 4.11 Single-Cycle vs Multi-Cycle Datapath ⭐⭐⭐

#### Quick Visual 🧩

```
Single-cycle: one long clock per instruction
Multi-cycle: multiple short clocks per instruction
Pipeline: overlap multiple instructions
```

| Feature | Single-Cycle | Multi-Cycle | Pipelined |
|---|---|---|---|
| Clock period | Longest instruction | Single stage | Single stage |
| CPI | 1 | 3-5 (varies) | 1 (ideal) |
| Hardware reuse | None | ALU, memory shared | None |
| Performance | Poor | Better | Best |
| Complexity | Simple | Medium | Complex (hazards) |

**MIPS Datapath Key Control Signals:**
| Signal | Purpose | Values |
|---|---|---|
| RegDst | Select dest register | 0=rt, 1=rd |
| ALUSrc | ALU second input | 0=register, 1=immediate |
| MemtoReg | Data to register | 0=ALU, 1=memory |
| RegWrite | Enable reg write | 0=no, 1=yes |
| MemRead | Enable mem read | 0=no, 1=yes |
| MemWrite | Enable mem write | 0=no, 1=yes |
| Branch | Branch instruction? | 0=no, 1=yes |
| ALUOp | ALU operation | 00=add, 01=sub, 10=funct |

**Control Signal Settings by Instruction:**
| Instruction | RegDst | ALUSrc | MemtoReg | RegWrite | MemRead | MemWrite | Branch |
|---|---|---|---|---|---|---|---|
| R-type | 1 | 0 | 0 | 1 | 0 | 0 | 0 |
| lw | 0 | 1 | 1 | 1 | 1 | 0 | 0 |
| sw | X | 1 | X | 0 | 0 | 1 | 0 |
| beq | X | 0 | X | 0 | 0 | 0 | 1 |

### 4.12 Arithmetic Shift vs Logical Shift ⭐

#### Quick Visual 🧩

```
Logical right:   fill 0 at MSB
Arithmetic right: copy sign bit at MSB
```

| Shift Type | Left | Right |
|---|---|---|
| **Logical** | Fill 0 at right | Fill 0 at left |
| **Arithmetic** | Same as logical left | **Sign bit preserved!** |
| **Circular (Rotate)** | MSB → LSB | LSB → MSB |

> 🎯 **SLL by n = ×2ⁿ, SRL by n = unsigned ÷2ⁿ, SRA by n = signed ÷2ⁿ**

### 4.13 ALU Design ⭐⭐

**1-bit ALU Operations:**
| Operation[1:0] | Output |
|---|---|
| 00 | a AND b |
| 01 | a OR b |
| 10 | a + b (full adder) |
| 11 | a − b (a + NOT(b) + 1) |

**32-bit ALU:** Chain 32 1-bit ALUs. Zero flag = NOR of all result bits.
**Overflow detection:** V = CarryIn[31] ⊕ CarryOut[31]

**Carry Lookahead Adder (CLA) ⭐:**
Problem: Ripple carry adder is slow (O(n) delay for n bits).
Solution: Compute all carries in parallel!

**Generate (G)** and **Propagate (P)** signals:
- Gᵢ = Aᵢ AND Bᵢ (carry generated here regardless of input carry)
- Pᵢ = Aᵢ XOR Bᵢ (carry propagated from previous stage)

**Carry equations:**
$$C_1 = G_0 + P_0 \cdot C_0$$
$$C_2 = G_1 + P_1 \cdot G_0 + P_1 \cdot P_0 \cdot C_0$$
$$C_3 = G_2 + P_2 \cdot G_1 + P_2 \cdot P_1 \cdot G_0 + P_2 \cdot P_1 \cdot P_0 \cdot C_0$$

**Delay:** O(1) for carry computation (2 gate levels regardless of width!)
**Cost:** More gates, fan-in increases with n → typically limited to 4-bit CLA blocks

**Comparison of Adder Types:**
| Adder | Delay | Hardware | Use |
|---|---|---|---|
| Ripple Carry | O(n) | O(n) | Simple, small designs |
| Carry Lookahead | O(log n) with cascading | O(n log n) | Fast, ALU |
| Carry Select | O(√n) | O(n) | Good balance |
| Carry Save | O(1) per addition | O(n) | Multipliers (many numbers) |

**Carry Select Adder:**
- Compute two results in parallel: one assuming Cin=0, another assuming Cin=1
- When actual carry arrives, select correct result via MUX
- Delay = delay_of_subadder + delay_of_mux

### 4.14 Multi-Cycle CPU Design ⭐⭐

#### Quick Visual 🧩

```
IF -> ID -> EX -> MEM -> WB

R-type uses: IF ID EX WB
Load uses:   IF ID EX MEM WB
Branch uses: IF ID EX
```

In a multi-cycle implementation, each instruction takes MULTIPLE cycles, but each cycle is shorter (= one ALU operation or one memory access).

**Advantage over single-cycle:** Clock period = MAX(single step), not MAX(entire instruction)
**Advantage over pipelined:** Simpler (no hazards, no forwarding needed)

**Multi-Cycle Steps for MIPS:**
| Step | Cycle | Action | Active Components |
|---|---|---|---|
| Instruction Fetch | 1 | IR ← M[PC]; PC ← PC+4 | Memory, PC |
| Instruction Decode | 2 | A ← R[rs]; B ← R[rt]; ALUOut ← PC + SE(imm)×4 | Register file, ALU |
| Execute (R-type) | 3 | ALUOut ← A op B | ALU |
| Execute (Load/Store) | 3 | ALUOut ← A + SE(imm) | ALU |
| Execute (Branch) | 3 | if (A==B) PC ← ALUOut | ALU, PC |
| Memory Access | 4 | MDR ← M[ALUOut] or M[ALUOut] ← B | Memory |
| Write Back | 5 | R[rd] ← ALUOut or R[rt] ← MDR | Register file |

**Instruction Cycle Counts:**
| Instruction | Cycles | Why |
|---|---|---|
| R-type (ADD, SUB) | 4 | IF, ID, EX, WB |
| Load (LW) | 5 | IF, ID, EX, MEM, WB |
| Store (SW) | 4 | IF, ID, EX, MEM |
| Branch (BEQ) | 3 | IF, ID, EX |
| Jump (J) | 3 | IF, ID, EX |

**CPI for multi-cycle = weighted average of instruction cycle counts!**

**Example:**
If instruction mix: 40% R-type, 25% Load, 15% Store, 15% Branch, 5% Jump:
CPI = 0.40×4 + 0.25×5 + 0.15×4 + 0.15×3 + 0.05×3 = 1.6+1.25+0.6+0.45+0.15 = **4.05**

### 4.15 Pipeline Registers ⭐⭐

#### Quick Visual 🧩

```
IF/ID -> ID/EX -> EX/MEM -> MEM/WB

Each latch stores stage output for next cycle.
```

Between each pipeline stage, there are **pipeline registers** (latches) that hold intermediate results:

```
IF → [IF/ID] → ID → [ID/EX] → EX → [EX/MEM] → MEM → [MEM/WB] → WB
```

**Contents of each pipeline register:**

| Register | Contents |
|---|---|
| **IF/ID** | Instruction word, PC+4 |
| **ID/EX** | Control signals, PC+4, Register data A & B, Sign-extended immediate, Rs, Rt, Rd |
| **EX/MEM** | Control signals, Branch target, ALU result, Zero flag, Register data B, Dest register |
| **MEM/WB** | Control signals, Memory data (from load), ALU result, Dest register |

> 🎯 **GATE Insight:** Pipeline registers add latency (latch delay) to each stage. This is why pipeline clock = max(stage_delay) + **latch_delay**.

**Pipeline register width for MIPS 32-bit:**
- IF/ID: 32 (instruction) + 32 (PC+4) = 64 bits
- ID/EX: ~200 bits (control + data + addresses)
- EX/MEM: ~150 bits
- MEM/WB: ~100 bits

---

## 5. Pipelining 🔄

> 💡 **Analogy:** Pipelining is like a **car wash** 🚗: While car #1 is being dried, car #2 is being rinsed, car #3 is being soaped, and car #4 is entering. All stages work simultaneously on different cars!

### 5.1 Basic Pipeline ⭐⭐⭐
#### Quick Visual 🧩

```
Instruction i:    IF ID EX MEM WB
Instruction i+1:    IF ID EX MEM WB
Instruction i+2:      IF ID EX MEM WB
```

Standard 5-stage RISC pipeline:
```
IF → ID → EX → MEM → WB
```
| Stage | Function |
|-------|----------|
| IF (Instruction Fetch) | Fetch instruction from memory |
| ID (Instruction Decode) | Decode + read registers |
| EX (Execute) | ALU operation / address calculation |
| MEM (Memory) | Load/Store memory access |
| WB (Write Back) | Write result to register |

### 5.2 Pipeline Performance ⭐⭐⭐

#### Quick Visual 🧩

```
Non-pipeline time: n x k x tau
Pipeline time:     (k + n - 1) x tau

As n gets large, speedup approaches k
```

**Without pipeline:** Time = n × k × τ (n instructions, k stages, τ = stage delay)
**With pipeline:** Time = (k + n − 1) × τ

**Speedup = n×k×τ / ((k+n−1)×τ) = nk / (k+n−1)**

For large n: **Speedup → k** (number of stages)

**Throughput = n / ((k+n−1)×τ)**
For large n: **Throughput → 1/τ** (one instruction per stage time)

**Efficiency = Speedup / k = n / (k+n−1)**
For large n: **Efficiency → 1** (100%)

> 🎯 **GATE Formula Summary:**
> - Execution time (pipelined) = (k + n − 1) × max(stage delays)
> - If stages have different delays: clock = max(stage delay) + register overhead

**Non-Uniform Pipeline Stages:**
If stage delays are: τ₁=2ns, τ₂=3ns, τ₃=2ns, τ₄=4ns, τ₅=2ns:
- Clock period = max(τᵢ) + buffer delay = 4 + 1 = **5 ns** (with 1ns buffer)
- Total time for 100 instructions = (5 + 100 − 1) × 5 = 520 ns

**Without pipeline:** 100 × (2+3+2+4+2) = 100 × 13 = 1300 ns
**Speedup** = 1300/520 = **2.5×** (far less than ideal 5× due to stage imbalance)

> ⚠️ **Key Insight:** Pipeline performance is limited by the SLOWEST stage! This is why balanced stage delays are critical.

### 5.3 Pipeline Timing Diagram ⭐⭐

```
Clock:    1    2    3    4    5    6    7    8    9
I1:      IF   ID   EX  MEM   WB
I2:           IF   ID   EX  MEM   WB
I3:                IF   ID   EX  MEM   WB
I4:                     IF   ID   EX  MEM   WB
I5:                          IF   ID   EX  MEM   WB
```

- **First instruction completes at cycle k (=5)**
- **Each subsequent instruction completes 1 cycle later**
- **All n instructions complete at cycle k + n − 1 = 5 + 5 − 1 = 9**

### 5.4 Pipeline Hazards ⭐⭐⭐

#### Quick Visual 🧩

```
Hazard types:
1) Structural (resource conflict)
2) Data (RAW/WAR/WAW)
3) Control (branch)
```

#### Structural Hazards
Two instructions need same hardware resource simultaneously.

**Example:** Single memory for both instructions and data:
```
Clock:    1    2    3    4    5    6
I1:      IF   ID   EX  MEM   WB
I2:           IF   ID   EX  MEM   WB
I3:                IF   ID   EX  MEM   WB
I4:                     IF ← CONFLICT! I4 needs IF (memory) but I1 needs MEM (memory)
```

**Solutions:**
- Separate I-cache and D-cache (Harvard at cache level)
- Duplicate hardware (multiple ALUs, register file ports)
- Stall the pipeline

#### Data Hazards ⭐⭐⭐
**RAW (Read After Write):** True dependency — most common and most important.
```
I1: ADD R1, R2, R3    ; writes R1 in WB (cycle 5)
I2: SUB R4, R1, R5    ; reads R1 in ID (cycle 3) — R1 not yet written!
```

**Without forwarding — needs 2 stall bubbles:**
```
Clock:    1    2    3    4    5    6    7    8
I1:      IF   ID   EX  MEM   WB
I2:           IF   -- stall --   ID   EX  MEM   WB
```

**With forwarding — NO stalls for R-type:**
```
Clock:    1    2    3    4    5    6
I1:      IF   ID   EX  MEM   WB
                    ↓ forward ALU result
I2:           IF   ID   EX  MEM   WB
```
ALU result available at end of EX (cycle 3) → forwarded to I2's EX (cycle 4). Wait... I2 reads in ID (cycle 3, needs R1) but I1 produces at end of EX (cycle 3). So forwarding from EX stage output to EX stage input of next instruction works!

**WAR (Write After Read):** Anti-dependency — only in out-of-order execution.
```
I1: ADD R1, R2, R3    ; reads R2
I2: SUB R2, R4, R5    ; writes R2 — must happen AFTER I1 reads R2
```

**WAW (Write After Write):** Output dependency — only in out-of-order or pipelines with variable latency.
```
I1: ADD R1, R2, R3    ; writes R1
I2: SUB R1, R4, R5    ; also writes R1 — must happen AFTER I1
```

> 🎯 **GATE Fact:** In a simple in-order pipeline, only **RAW** hazards matter! WAR and WAW only occur in out-of-order or superscalar.

**Forwarding Paths:**
| From → To | Scenario | Stalls needed |
|---|---|---|
| EX/MEM → EX | R-type followed by dependent R-type | 0 |
| MEM/WB → EX | R-type followed by gap, then dependent | 0 |
| MEM/WB → EX | LOAD followed by dependent R-type | **1 stall** (load-use) |
| No forward possible | LOAD immediately followed by dependent branch | **1+ stalls** |

**Load-Use Hazard (CRITICAL):** ⚠️
```
I1: LW R1, 0(R2)     ; R1 available after MEM (cycle 4)
I2: ADD R3, R1, R4    ; needs R1 at EX (cycle 4) — but LW result at END of cycle 4!
```
Even with forwarding, need **1 stall cycle**:
```
Clock:    1    2    3    4    5    6    7
LW:      IF   ID   EX  MEM   WB
                         ↓ forward from MEM output
ADD:          IF   ID  stall  EX  MEM   WB
```

> 🎯 **GATE Trick:** "Load followed immediately by a dependent instruction always needs 1 stall, even with full forwarding."

#### Control Hazards ⭐⭐⭐
Branches change PC, but next instructions already in pipeline.

**Branch Penalty Depends on Where Branch is Resolved:**
| Resolution Stage | Penalty (cycles) |
|---|---|
| ID (cycle 2) | 1 cycle |
| EX (cycle 3) | 2 cycles |
| MEM (cycle 4) | 3 cycles |

**Example with 2-cycle penalty:**
```
Clock:    1    2    3    4    5    6    7    8
BEQ:     IF   ID   EX  MEM   WB
                    ↑ branch resolved here
I_wrong:      IF   ID  ← FLUSH (wrong path)
I_wrong:           IF  ← FLUSH
I_target:              IF   ID   EX  MEM   WB
```

**Solutions to Control Hazards:**

**a) Stalling:** Wait until branch resolved. Simple but wastes cycles.

**b) Branch Prediction ⭐⭐:**
- **Static:** Always predict taken or not-taken
  - Predict not-taken: 0 penalty if correct, full penalty if wrong
  - Predict backward-taken, forward-not-taken: good for loops (~90% accuracy)
- **Dynamic:** Use history of branches

**1-Bit Predictor:**
- Single bit per branch: 0=not-taken, 1=taken
- Mispredicts TWICE for loop (first entry and last exit)

**2-Bit Saturating Counter Predictor ⭐⭐:**
```
States: 00(Strongly NT) → 01(Weakly NT) → 10(Weakly T) → 11(Strongly T)
```

| State | Prediction | On Taken | On Not-Taken |
|---|---|---|---|
| 00 (Strongly NT) | NT | → 01 | → 00 |
| 01 (Weakly NT) | NT | → 10 | → 00 |
| 10 (Weakly T) | T | → 11 | → 01 |
| 11 (Strongly T) | T | → 11 | → 10 |

> 🎯 **GATE:** For loop executing N times:
> - 2-bit predictor: mispredicts only **1 time** (last iteration)
> - 1-bit predictor: mispredicts **2 times** (first and last)

**c) Delayed Branching:**
- Execute instruction in branch delay slot REGARDLESS of branch outcome
- Compiler must fill delay slot with useful instruction
- If no useful instruction → fill with NOP

**d) Branch Target Buffer (BTB):**
- Cache: (branch PC → predicted target → prediction bits)
- Accessed in IF stage → eliminates penalty if prediction correct

**Branch Penalty Calculation:**
```
Effective CPI = Ideal CPI + Branch_frequency × Misprediction_rate × Penalty
```

**Worked Example:**
20% branches, 10% misprediction rate, penalty = 2 cycles:
CPI = 1 + 0.20 × 0.10 × 2 = 1 + 0.04 = **1.04**

**Another Example:** 25% branches, branch resolved at EX (2-cycle penalty), 75% accuracy:
CPI = 1 + 0.25 × 0.25 × 2 = 1 + 0.125 = **1.125**
Speedup vs stall-always = (1 + 0.25×2) / 1.125 = 1.5/1.125 = **1.33×**

### 5.5 Pipeline with Stalls — Complete Worked Example ⭐⭐⭐

**Given code (no forwarding):**
```
I1: LW   R1, 0(R10)      ; Load R1
I2: ADD  R2, R1, R3      ; RAW on R1 (needs 2 stalls without forwarding)
I3: SUB  R4, R2, R5      ; RAW on R2 (needs 2 stalls)
I4: AND  R6, R4, R7      ; RAW on R4 (needs 2 stalls)
I5: SW   R6, 100(R10)    ; RAW on R6 (needs 2 stalls)
```

**Without forwarding — pipeline diagram:**
```
Cycle: 1  2  3  4  5  6  7  8  9  10 11 12 13 14 15 16 17
I1:   IF ID EX ME WB
I2:      IF -- -- ID EX ME WB
I3:               IF -- -- ID EX ME WB
I4:                        IF -- -- ID EX ME WB
I5:                                 IF -- -- ID EX ME WB
```
Total cycles = 17 (5 + 4×2 stalls + 4×1 normal = 5 + 8 + 4 = 17)

**With full forwarding:**
```
Cycle: 1  2  3  4  5  6  7  8  9
I1:   IF ID EX ME WB
I2:      IF ID -- EX ME WB         ← 1 stall (load-use)
I3:            IF ID EX ME WB      ← No stall (forwarding from I2's EX)
I4:               IF ID EX ME WB   ← No stall
I5:                  IF ID EX ME WB ← No stall
```
Total cycles = 9 (only 1 stall for load-use hazard!)

### 5.6 Advanced Pipelining Topics ⭐⭐

#### Quick Visual 🧩

```
Superscalar: issue multiple instructions per cycle
Out-of-order: execute when operands ready
ROB: commit results in order
```

#### Instruction-Level Parallelism (ILP)

**Superscalar Processors:**
- Issue **multiple instructions per cycle** from a single instruction stream
- Hardware dynamically detects independent instructions
- **Issue width:** 2-wide, 4-wide, 8-wide superscalar
- CPI < 1 possible (IPC > 1)

**VLIW (Very Long Instruction Word):**
- **Compiler** packs multiple operations into one long instruction
- No hardware scheduling — compiler's responsibility
- Simpler hardware but NOP slots waste space

| Feature | Superscalar | VLIW |
|---|---|---|
| Scheduling | Hardware (dynamic) | Compiler (static) |
| Complexity | Complex hardware | Simple hardware |
| Code compatibility | Binary compatible | Recompile needed |
| Example | Intel Core, ARM | Intel Itanium |

**Superscalar CPI Calculation:**
For an N-issue superscalar: Ideal CPI = 1/N

**Example:** 4-issue superscalar, but only 2.5 independent instructions per cycle on average:
- Actual IPC = 2.5
- Actual CPI = 1/2.5 = **0.4**
- Speedup over scalar: 1/0.4 = **2.5×** (not 4× due to dependencies)

**Factors Limiting ILP:**

| Factor | Impact | Mitigation |
|---|---|---|
| True data dependencies (RAW) | Fundamental limit | Forwarding, speculation |
| Branch prediction accuracy | Wrong path wastes issue slots | Better predictors, trace cache |
| Memory ambiguity | Can't reorder loads/stores safely | Store buffers, speculation |
| Finite issue width | Hardware limits | Wider issue (diminishing returns) |
| Instruction window size | Can't see far-apart ILP | Larger ROB, deeper scheduling |

**Studies show:** Available ILP in real programs is typically 2-6 instructions per cycle, even with aggressive speculation and infinite resources.

#### EPIC (Explicitly Parallel Instruction Computing)
- Intel Itanium's approach
- Compiler explicitly marks instruction bundles
- Predication: every instruction has a predicate register
  - If predicate = true → execute; if false → NOP
  - Eliminates branches! Both paths execute, only one commits.

#### Dependency Types in Detail ⭐⭐

| Dependency | Also called | Can eliminate? | Method |
|---|---|---|---|
| RAW (Read After Write) | True/Flow | **NO** (fundamental) | Forward, schedule |
| WAR (Write After Read) | Anti-dependency | Yes | Register renaming |
| WAW (Write After Write) | Output dependency | Yes | Register renaming |
| Control | Branch dependency | Partially | Prediction, speculation |
| Memory | Ambiguous mem deps | Maybe | Address disambiguation |

**Data Dependency Graph:**
For the code:
```
I1: ADD R1, R2, R3
I2: MUL R4, R1, R5     ← RAW on R1 from I1
I3: SUB R6, R2, R7     ← Independent of I1, I2!
I4: AND R8, R4, R6     ← RAW on R4 (from I2) and R6 (from I3)
```

**Dependency graph:**
```
I1 → I2 → I4
I3 -------↗
```

**Critical path:** I1 → I2 → I4 (length 3)
**I3 can execute in parallel with I1 or I2!**

On a 2-issue superscalar:
- Cycle 1: I1, I3 (both issued)
- Cycle 2: I2 (depends on I1)
- Cycle 3: I4 (depends on I2 and I3)
Total = **3 cycles** for 4 instructions (IPC = 4/3 = 1.33)

**Reorder Buffer (ROB) in Detail:**

The ROB enables in-order commit despite out-of-order execution:

| ROB Entry | Fields |
|---|---|
| Instruction type | Branch, Store, Register |
| Destination | Register number or memory address |
| Value | Computed result |
| Ready | Whether execution is complete |
| Speculative | Whether instruction is speculative |

**ROB acts as a circular queue:**
- Instructions enter at tail (in program order)
- Instructions commit from head (in program order)
- If branch misprediction: flush all entries after the mispredicted branch

**Example ROB state:**
| Entry | Instruction | Dest | Value | Ready? |
|---|---|---|---|---|
| 0 | ADD R1 | R1 | 42 | ✅ → commit! |
| 1 | MUL R4 | R4 | 168 | ✅ → commit! |
| 2 | LW R2 | R2 | — | ❌ (cache miss) → stall commit |
| 3 | SUB R5 | R5 | 100 | ✅ (but can't commit — blocked by entry 2) |

This ensures precise exceptions: if entry 2 causes a page fault, the state is exactly as if I0 and I1 completed but I2 and I3 didn't.

#### Out-of-Order Execution ⭐

**Steps:**
1. **Fetch & Decode** in order
2. **Issue** to reservation stations
3. **Execute** out of order (when operands ready)
4. **Commit/Retire** in order (via Reorder Buffer — ROB)

**Register Renaming:** Eliminates WAR and WAW hazards by mapping architectural registers to physical registers.
- Only **RAW (true dependency)** cannot be eliminated by renaming!

**Tomasulo's Algorithm:**
- Uses reservation stations (distributed) instead of centralized scoreboard
- Register renaming via reservation station tags
- Common Data Bus (CDB) broadcasts results
- Enables dynamic loop unrolling

#### Speculative Execution
- Predict branch outcomes and execute speculatively
- Results held in ROB; committed only if prediction correct
- Misprediction → flush speculative instructions and restore state

#### Pipeline with Exceptions
- Precise exceptions: all instructions before faulting instruction are committed; all after are flushed
- Requires in-order commit (ROB helps)
- Imprecise exceptions: simpler hardware but harder for software

#### Loop Unrolling ⭐
Reduces loop overhead and exposes more ILP:
```
Original:               Unrolled 4x:
for i=0 to 99:          for i=0 to 99 step 4:
  A[i] = A[i]*2           A[i]=A[i]*2; A[i+1]=A[i+1]*2
                           A[i+2]=A[i+2]*2; A[i+3]=A[i+3]*2
```
- Reduces branch frequency by unroll factor
- Exposes dependencies between iterations for scheduling
- Increases code size (trade-off)

---

### 5.7 Tomasulo's Algorithm — Detailed Walkthrough ⭐⭐⭐

> 💡 **Why Tomasulo?** Static scheduling (compiler reorders) has limitations. Tomasulo allows the **hardware** to dynamically find parallelism and schedule instructions out-of-order.

**Key Components:**
1. **Reservation Stations (RS):** Buffers that hold instructions waiting for operands
2. **Common Data Bus (CDB):** Broadcasts results to all listening RS and register file
3. **Register File:** Maps architectural registers to values or RS tags
4. **Functional Units:** ALU, multiplier, load/store unit

**Three Stages:**
| Stage | Action |
|---|---|
| **Issue** | Decode instruction; allocate RS; read available operands from register file; set register result tag |
| **Execute** | When ALL operands available in RS → begin execution. Load/Store compute address |
| **Write Result** | Broadcast result on CDB → all RS and register file entries watching this tag get updated |

**Register Renaming in Tomasulo:**
- Instead of physical rename registers, Tomasulo uses RS entry names as "tags"
- Register file entry points to RS tag if result is pending
- This eliminates WAR and WAW hazards!

**Worked Example — Tomasulo Execution:**

Consider this code:
```
I1: MUL  F0, F2, F4     ; F0 = F2 × F4
I2: SUB  F8, F0, F2     ; F8 = F0 − F2  (RAW on F0 from I1)
I3: DIV  F10, F0, F6    ; F10 = F0 / F6  (RAW on F0 from I1)
I4: ADD  F6, F8, F2     ; F6 = F8 + F2   (RAW on F8 from I2; WAR on F6 with I3!)
```

Assume: ADD/SUB = 2 cycles, MUL = 10 cycles, DIV = 40 cycles

**Issue phase (in order):**
- Cycle 1: I1 issued to Multiply RS1. Register file: F0 → RS_Mult1
- Cycle 2: I2 issued to Add RS1. Operand F0 not ready → waits for RS_Mult1. Register file: F8 → RS_Add1
- Cycle 3: I3 issued to Divide RS. Operand F0 not ready → waits for RS_Mult1. Register file: F10 → RS_Div1
- Cycle 4: I4 issued to Add RS2. Operand F8 not ready → waits for RS_Add1. F2 is ready. Register file: F6 → RS_Add2

**Execution (out of order):**
- I1 (MUL): Executes cycles 2-11, writes result cycle 12 on CDB
- I2 (SUB): Gets F0 at cycle 12, executes cycles 12-13, writes cycle 14
- I3 (DIV): Gets F0 at cycle 12, executes cycles 12-51, writes cycle 52
- I4 (ADD): Gets F8 at cycle 14, executes cycles 14-15, writes cycle 16

**Cycle-by-Cycle Table:**
| Instruction | Issue | Execute Start | Execute End | Write Result |
|---|---|---|---|---|
| MUL F0,F2,F4 | 1 | 2 | 11 | 12 |
| SUB F8,F0,F2 | 2 | 12 | 13 | 14 |
| DIV F10,F0,F6 | 3 | 12 | 51 | 52 |
| ADD F6,F8,F2 | 4 | 14 | 15 | 16 |

**Key insights:**
- WAR on F6 (I3 reads F6, I4 writes F6) is NOT a problem because I3 already captured F6's value in its RS at issue time!
- WAW hazard would occur if I4 wrote to F0, but register renaming via tags prevents this
- I3 and I2 can begin execution in same cycle (12) — parallelism!

**Tomasulo vs Scoreboard:**
| Feature | Scoreboard | Tomasulo |
|---|---|---|
| Hazard detection | Centralized scoreboard | Distributed (reservation stations) |
| Register renaming | No | Yes (via RS tags) |
| WAR handling | Stall | Eliminated (values copied to RS) |
| WAW handling | Stall | Eliminated (only last write updates RF) |
| Forwarding | Through register file | Via CDB broadcast |
| Loop handling | Limited | Dynamic unrolling |
| Hardware cost | Lower | Higher (RS + CDB) |

---

### 5.8 Scoreboarding — CDC 6600 Approach ⭐⭐

**Scoreboard** is a centralized hardware mechanism for dynamic scheduling.

**Four Stages of Scoreboard:**
| Stage | Action | Stall condition |
|---|---|---|
| **Issue** | Decode, check for structural hazard | WAW or structural → stall |
| **Read Operands** | Wait until source operands available | RAW → wait |
| **Execute** | Compute result | Varies by FU |
| **Write Result** | Write to register file | WAR → wait until reader has read |

**Key Differences from Tomasulo:**
- Scoreboard stalls on WAR (must wait for reader). Tomasulo doesn't (copies values to RS)
- Scoreboard stalls on WAW (only one writer at a time). Tomasulo renames registers
- Scoreboard is **simpler hardware** but extracts **less parallelism**

**Scoreboard Data Structures:**
1. **Instruction Status Table:** Which stage each instruction is in
2. **Functional Unit Status Table:** Busy, Op, Fi(dest), Fj/Fk(src regs), Qj/Qk(producing FU), Rj/Rk(ready flags)
3. **Register Result Status Table:** Which FU will write to each register

---

### 5.9 Hardware Multithreading ⭐

**Fine-Grained Multithreading:**
- Switch between threads every cycle (round-robin)
- Can tolerate long latency events (cache miss) by running other threads
- Reduces single-thread performance
- Example: Sun UltraSPARC T1 (Niagara)

**Coarse-Grained Multithreading:**
- Switch only on costly stalls (L2 cache miss, TLB miss)
- Better single-thread performance
- Some pipeline bubbles on switch
- Example: IBM AS/400

**Simultaneous Multithreading (SMT) / Hyper-Threading:**
- Issue instructions from multiple threads every cycle
- Fills empty slots in superscalar pipeline
- Appears as multiple "logical processors"
- Example: Intel Hyper-Threading (2 threads per core)

| Feature | Fine-grained | Coarse-grained | SMT |
|---|---|---|---|
| Switch granularity | Every cycle | On stalls | Every cycle (mixed) |
| Throughput | High | Medium | Highest |
| Single-thread perf | Lower | Medium | Good |
| Hardware cost | Low | Low | High |
| Example | UltraSPARC T1 | IBM AS/400 | Intel HT |

---

### 5.10 VLIW (Very Long Instruction Word) Architecture ⭐⭐

> 💡 **VLIW shifts scheduling complexity from hardware to the compiler.** Each VLIW instruction word contains multiple operations that execute in parallel.

**VLIW Instruction Format:**
```
┌─────────────────────────────────────────────────────┐
│  INT ALU 1  │ INT ALU 2  │  FP ALU   │ Load/Store │ Branch │
│  (32 bits)  │ (32 bits)  │ (32 bits) │ (32 bits)  │(32bits)│
└─────────────────────────────────────────────────────┘
         One VLIW instruction = 160 bits (5 operations)
```

**Example VLIW instruction (Itanium-like):**
```
{ add r1=r2,r3 ; ld r4=[r5] ; fmul f1=f2,f3 }   // 3 ops in parallel
```

**Superscalar vs VLIW vs EPIC:**
| Feature | Superscalar | VLIW | EPIC (IA-64/Itanium) |
|---|---|---|---|
| Scheduling | Hardware (dynamic) | Compiler (static) | Compiler + hints |
| Instruction width | Standard | Very wide | Bundles of 3 |
| Hardware complexity | Very high | Low | Medium |
| Code compatibility | Binary compatible | Must recompile | Must recompile |
| NOP handling | No wasted slots | NOP slots waste space | NOP compression |
| Branch handling | Hardware prediction | Compiler predication | Both |
| Power efficiency | Lower (OOO hardware) | Higher | Medium |
| Example | Intel Core, AMD Zen | TI C6x DSP | Intel Itanium |

**Predication in VLIW/EPIC:**
```
; Instead of branch:
  if (R1 > 0) then R2 = R3 + R4
  else R2 = R3 - R4

; With predication:
  CMP.GT P1, P2 = R1, 0      ; P1=true if R1>0, P2=true otherwise
  (P1) ADD R2 = R3, R4        ; executes only if P1 is true
  (P2) SUB R2 = R3, R4        ; executes only if P2 is true
```

> 🎯 **GATE Insight:** Predication eliminates branches entirely → no branch misprediction penalty! But both paths execute (wasting one slot), so it's best for short if-then-else.

### 5.11 Precise vs Imprecise Exceptions ⭐⭐

> 💡 An **exception** (interrupt, trap, fault) during pipeline execution requires careful handling to maintain correct program state.

**Precise Exception:**
- All instructions before the faulting instruction have completed
- The faulting instruction and all after it have NOT modified state
- CPU state appears as if instructions executed sequentially

**Imprecise Exception:**
- Some instructions after the faulting one may have partially completed
- CPU state is "messy" — harder for OS to handle
- Simpler hardware

```
Pipeline state at exception on I3:

Precise:                         Imprecise:
I1: ✅ committed                 I1: ✅ committed
I2: ✅ committed                 I2: ✅ committed  
I3: ❌ faulting (not committed)  I3: ❌ faulting
I4: ❌ flushed                   I4: ⚠️ partially executed!
I5: ❌ flushed                   I5: ⚠️ partially executed!
```

**How ROB enables Precise Exceptions:**
1. Instructions enter ROB in program order
2. Execute out of order, store results in ROB
3. Commit (write to register file) in order from ROB head
4. On exception: flush ROB entries from faulting instruction onward
5. State is exactly as if instructions executed up to (but not including) faulting instruction

**Exception Types in Pipeline:**
| Exception | Stage Detected | Priority |
|---|---|---|
| Page fault (instruction) | IF | Highest (earliest) |
| Illegal opcode | ID | |
| Arithmetic overflow | EX | |
| Page fault (data) | MEM | |
| Hardware interrupt | Any (checked between instructions) | Varies |

> 🎯 **GATE Fact:** When multiple exceptions occur simultaneously (different instructions in pipeline), the one from the EARLIEST instruction (in program order) is handled first.

### 5.12 Superpipelining ⭐

> 💡 **Superpipelining** = increasing clock speed by dividing pipeline stages into sub-stages.

```
Regular 5-stage:     IF | ID | EX | MEM | WB
                     ←───── 1 cycle ─────→

Superpipelined (2×): IF₁|IF₂| ID₁|ID₂| EX₁|EX₂| MEM₁|MEM₂| WB₁|WB₂
                     ←─ ½ cycle ─→
```

**Effect:** Clock frequency doubled (each sub-stage is half the time)

**Comparison:**
| Approach | CPI | Clock Period | Throughput |
|---|---|---|---|
| Regular pipeline | 1 | τ | 1/τ |
| Superpipeline (degree m) | 1 | τ/m | m/τ |
| Superscalar (n-issue) | 1/n | τ | n/τ |
| Superpipe + Superscalar | 1/n | τ/m | m×n/τ |

**Problem with deep superpipelining:**
- Branch penalty = number of stages → deeper = worse misprediction cost
- Pipeline register overhead becomes significant fraction of cycle time
- Diminishing returns beyond ~15-20 stages

> 🎯 **GATE:** Superpipelining increases throughput by subdividing stages. Superscalar increases throughput by replicating stages. Modern processors combine both!

### 5.13 Pipeline Scheduling Example ⭐⭐

**Q:** Schedule the following code for a 5-stage pipeline with forwarding and 1-cycle load-use penalty:
```
LW   R1, 0(R10)
LW   R2, 4(R10)
ADD  R3, R1, R2
SW   R3, 8(R10)
LW   R4, 12(R10)
ADD  R5, R1, R4
SW   R5, 16(R10)
```

**Unscheduled (with stalls):**
```
LW   R1, 0(R10)       ; Cycles 1-5
LW   R2, 4(R10)       ; Cycles 2-6
      <stall>          ; Load-use: ADD needs R2 from LW
ADD  R3, R1, R2        ; Cycles 4-8
      <stall>          ; Load-use: SW needs R3 from ADD? No, SW reads in MEM. But EX-MEM forwarding works, so no stall for SW!
SW   R3, 8(R10)        ; Cycles 5-9 (forwarding from ADD's EX)
LW   R4, 12(R10)      ; Cycles 6-10
      <stall>          ; Load-use: ADD needs R4
ADD  R5, R1, R4        ; Cycles 8-12
SW   R5, 16(R10)       ; Cycles 9-13
```
Total = 13 cycles, 2 stalls

**Scheduled (reordered by compiler):**
```
LW   R1, 0(R10)       ; Cycles 1-5
LW   R2, 4(R10)       ; Cycles 2-6
LW   R4, 12(R10)      ; Cycles 3-7  (moved up!)
ADD  R3, R1, R2        ; Cycles 4-8  (R1 ready from cycle 4, R2 ready from cycle 5 forwarding)
                        ; Wait: R2 available after LW's MEM at cycle 5. ADD's EX is cycle 6.
                        ; Actually R2 is available at end of cycle 5 (MEM stage), forwarded to cycle 6 EX. No stall!
SW   R3, 8(R10)        ; Cycles 5-9
ADD  R5, R1, R4        ; Cycles 6-10  (R4 ready from LW at cycle 6 via forwarding — no stall!)
SW   R5, 16(R10)       ; Cycles 7-11
```
Total = 11 cycles, **0 stalls!** Saved 2 cycles by instruction scheduling.

> 🎯 **GATE Lesson:** Instruction scheduling (by compiler) can eliminate many load-use stalls without any hardware changes!

---

### 5.14 Advanced Branch Prediction ⭐⭐

#### Correlating Predictors (Two-Level)
- 1-bit/2-bit predictors: only use history of THIS branch
- Correlating predictor: uses **history of other branches** too
- (m,n) predictor: Use last m branch outcomes (global) to select among 2^m n-bit predictors
- (1,2) predictor: 1 bit of global history → 2 sets of 2-bit predictors

#### Tournament Predictor
- Combines local and global predictors
- Meta-predictor selects which to trust
- Used in Alpha 21264: 96% accuracy!

#### Branch Target Buffer (BTB) ⭐
```
+------------------+-------------------+--------------------+
| Branch PC (tag)  | Predicted Target  | Prediction State   |
+------------------+-------------------+--------------------+
| 0x0040           | 0x0100            | Taken (11)         |
| 0x0088           | 0x0020            | Not Taken (01)     |
+------------------+-------------------+--------------------+
```
- Indexed by lower bits of branch PC
- Hit → use predicted target in **same cycle** as fetch
- Miss → must wait for decode to know it's a branch

**Return Address Stack (RAS):**
- Special stack for predicting return addresses
- CALL: push PC+4 onto RAS
- RET: pop RAS for predicted target
- Very high accuracy (>99%) since returns match calls

---

### 5.15 Pipeline Depth Analysis ⭐⭐

**Deeper Pipeline Trade-offs:**

| Pipeline depth | Pros | Cons |
|---|---|---|
| Shallow (5-7) | Lower penalty per hazard, simpler | Less clock speedup |
| Medium (10-14) | Good balance | Moderate complexity |
| Deep (20+) | Very high clock speed | High branch penalty, more power |

**Optimal pipeline depth** balances cycle time reduction vs pipeline overhead:
$$SpeedUp = \frac{Pipeline\_depth}{1 + Pipeline\_depth \times stall\_fraction}$$

**Historical Examples:**
| Processor | Pipeline Depth | Year |
|---|---|---|
| MIPS R2000 | 5 | 1985 |
| Intel Pentium | 5 | 1993 |
| Intel P6 (Pentium Pro) | 14 | 1995 |
| Intel Pentium 4 (Prescott) | 31 | 2004 |
| Intel Core (Skylake) | 14-19 | 2015 |
| ARM Cortex-A73 | 11 | 2016 |

> 🎯 **Intel's NetBurst (Pentium 4):** 31-stage pipeline was too deep! Branch misprediction penalty was ~20 cycles. Intel went back to shorter pipelines with Core architecture. **Lesson: deeper isn't always better!**

### 5.16 Stall Calculations Summary ⭐⭐

**CPI with stalls:**
```
CPI_effective = CPI_ideal + Stalls_per_instruction
             = 1 + data_stalls + control_stalls + structural_stalls
```

**Data stalls (with forwarding):**
= Load_frequency × Fraction_followed_by_dependent × 1

**Control stalls:**
= Branch_frequency × Misprediction_rate × Penalty

**Worked Example:**
Given: 30% loads (50% followed by dependent instruction), 15% branches (20% mispredicted, penalty=2), full forwarding:
- Data stalls = 0.30 × 0.50 × 1 = 0.15
- Control stalls = 0.15 × 0.20 × 2 = 0.06
- CPI = 1 + 0.15 + 0.06 = **1.21**

---

## 6. Memory Hierarchy, Cache & Virtual Memory 💾

> 💡 **Analogy:** Memory hierarchy is like YOUR study setup 📚:
> - **Registers** = Notes on your desk (instant access, tiny)
> - **L1 Cache** = Textbook open in front of you (fast)
> - **L2/L3 Cache** = Books on your bookshelf (slightly slower)
> - **RAM** = Your room's bookshelf (need to walk)
> - **SSD/HDD** = Library across campus (far away, slow)

### 6.1 Memory Hierarchy ⭐

#### Quick Visual 🧩

```
Faster, smaller, expensive  -> Registers -> L1 -> L2 -> L3
                                            |
                                            v
                                  RAM -> SSD/HDD
Slower, larger, cheaper
```

| Level | Size | Speed | Cost/bit | Managed by |
|---|---|---|---|---|
| Registers | ~1 KB | < 1 ns | Highest | Compiler |
| L1 Cache | 32-64 KB | 1-3 ns | Very high | Hardware |
| L2 Cache | 256 KB-1 MB | 3-10 ns | High | Hardware |
| L3 Cache | 2-32 MB | 10-30 ns | Medium | Hardware |
| RAM | 4-64 GB | 50-100 ns | Low | OS |
| SSD | 256 GB-4 TB | ~100 µs | Very low | OS |
| HDD | 1-16 TB | 5-10 ms | Lowest | OS |

**Locality Principles:**
- **Temporal:** Recently accessed → likely accessed again (loop counter)
- **Spatial:** Nearby data → likely accessed soon (array traversal)

### 6.2 Cache Organization ⭐⭐

#### Quick Visual 🧩

```
Cache = Sets x Ways x BlockSize

Example lookup:
Address -> [Tag | SetIndex | BlockOffset]
```

**Cache Size = Number of Sets × Associativity × Block Size**

| Parameter | Symbol | Formula |
|-----------|--------|---------|
| Cache size | C | S × k × B |
| Block/line size | B | Given |
| Number of blocks | N | C/B |
| Associativity | k (ways) | Given |
| Number of sets | S | N/k = C/(B×k) |

### 6.3 Cache Mapping ⭐⭐⭐

#### Quick Visual 🧩

```
Direct mapped:     1 block -> exactly 1 line
Set associative:   1 block -> one set, any way in that set
Fully associative: 1 block -> any line in cache
```

#### Direct Mapped (k=1)
Each memory block maps to exactly one cache line.
- Set index = (Block address) mod S
- Fast but high **conflict misses**

#### Fully Associative (S=1)
Any block can go anywhere. No index bits.
- Tag = entire block address
- No conflict misses, but expensive (comparator per line)
- Used for: TLBs, small caches

#### Set Associative (k-way) ⭐
k blocks per set.
- Set index = (Block address) mod S, where S = C/(B×k)
- k comparators needed per set

### 6.4 Address Breakdown ⭐⭐⭐

#### Quick Visual 🧩

```
Address bits:
[ Tag | Set Index | Block Offset ]

Offset bits = log2(block size)
Set bits    = log2(number of sets)
Tag bits    = remaining upper bits
```

For byte-addressable memory with m-bit address:
```
| Tag (t bits) | Set Index (s bits) | Block Offset (b bits) |
```
- **Offset bits** b = log₂(B)
- **Set bits** s = log₂(S)
- **Tag bits** t = m − s − b

**Worked Example 1:** 32-bit address, 64 KB cache, 4-way, 64-byte blocks:
- S = 64KB/(64×4) = 256 sets
- Offset = log₂(64) = 6, Set = log₂(256) = 8, Tag = 32−8−6 = **18 bits**

**Worked Example 2:** 24-bit address, 16 KB direct-mapped, 32-byte blocks:
- S = 16KB/32 = 512
- Offset = 5, Set = 9, Tag = 24−9−5 = **10 bits**
- Total overhead = 512 × (10 + 1) = 5632 bits (tags + valid)

**Worked Example 3:** 32-bit address, 128 KB fully-associative, 128-byte blocks:
- Blocks = 128KB/128 = 1024, S = 1
- Offset = 7, Set = 0, Tag = 32−0−7 = **25 bits**

| Mapping | Sets (S) | Comparators | Conflict misses |
|---|---|---|---|
| Direct (k=1) | C/B | 1 | High |
| k-way | C/(B×k) | k per set | Medium |
| Fully assoc | 1 | C/B | None |

### 6.5 Cache Performance ⭐⭐⭐

#### Quick Visual 🧩

```
Access request
  |
  +--> Hit  -> fast return
  |
  +--> Miss -> fetch from lower level (penalty)

AMAT = HitTime + MissRate x MissPenalty
```

**AMAT (Average Memory Access Time):**
$$\text{AMAT} = \text{Hit Time} + \text{Miss Rate} \times \text{Miss Penalty}$$

**Multi-level:**
$$\text{AMAT} = t_{L1} + m_{L1} \times (t_{L2} + m_{L2,local} \times P_{mem})$$

**Worked Example 1:**
L1: hit=1cy, miss=5%. L2: hit=10cy, local miss=20%. Mem=100cy.
AMAT = 1 + 0.05 × (10 + 0.20 × 100) = 1 + 0.05 × 30 = **2.5 cycles**

**Worked Example 2:**
Split cache: I-miss=2%, D-miss=8%, 40% loads/stores, penalty=50cy:
- Memory stalls = (1.0×0.02 + 0.4×0.08) × 50 = 2.6 cycles/instruction
- Effective CPI = 1 + 2.6 = **3.6**

> ⚠️ **GATE Trap:** "L2 miss rate" — check if it means LOCAL or GLOBAL!

### 6.6 Miss Types (3 Cs) ⭐⭐

#### Quick Visual 🧩

```
Compulsory: first time ever
Capacity:   cache too small
Conflict:   mapping collision
```

| Type | Cause | Reduce by |
|------|-------|-----------|
| **Compulsory** | First access ever | Prefetching, larger blocks |
| **Capacity** | Cache too small | Larger cache |
| **Conflict** | Mapping collisions | Higher associativity |

### 6.7 Replacement Policies ⭐
- **LRU:** Replace least recently used (optimal for stack patterns)
  - 2-way: 1 bit/set. 4-way: ~5 bits/set
- **FIFO:** Replace oldest (may have Belady's anomaly)
- **Optimal:** Replace block needed furthest in future (theoretical)
- **Random:** Surprisingly close to LRU for high associativity

### 6.8 Write Policies ⭐⭐

#### Quick Visual 🧩

```
Write-through: cache + memory updated immediately
Write-back:    update cache now, memory later on eviction (dirty bit)
```

| Policy | On Write Hit | On Write Miss |
|---|---|---|
| **Write-through** | Write cache + memory | No-write-allocate (usually) |
| **Write-back** | Write cache only (dirty bit) | Write-allocate (usually) |

### 6.9 Cache Optimization ⭐

| Technique | What it Reduces | How it Works |
|---|---|---|
| Larger block | Compulsory misses | Exploit spatial locality |
| Larger cache | Capacity misses | More data fits in cache |
| Higher associativity | Conflict misses | More places per block |
| Multi-level caches | Miss penalty | Faster intermediate level |
| Prefetching | Compulsory misses | Fetch data before it's needed |
| Write buffer | Write stalls | Buffer writes, continue execution |
| Victim cache | Conflict misses | Small fully-assoc holding evicted blocks |
| Non-blocking cache | Miss stalls | Allow hits during miss processing |
| Compiler optimization | All types | Loop blocking, array padding |
| Critical word first | Miss penalty | Send requested word first in block fill |

### 6.10 Victim Cache ⭐

#### Quick Visual 🧩

```
Main cache miss?
  |
  +--> check victim cache
       |
       +--> hit: swap block back
       +--> miss: fetch from next level
```

A small **fully associative** cache (4-16 entries) that holds **recently evicted** blocks.

```
CPU ↔ Main Cache ↔ Victim Cache ↔ Next Level
```

**How it works:**
1. On cache miss: check victim cache FIRST
2. If found in victim cache → **swap** the evicted block with the victim cache entry
3. If not in victim cache → fetch from next level; evicted block goes to victim cache

**Why it helps:** For direct-mapped caches, conflict misses between 2-3 alternating blocks can be caught by a small victim cache. Jouppi (1990) showed 4-entry victim cache removes 20-95% of conflict misses!

### 6.11 Non-Blocking (Lockup-Free) Cache ⭐

#### Quick Visual 🧩

```
Miss in progress does not freeze all accesses:
Hit-under-miss, Miss-under-miss using MSHRs
```

Allows the CPU to continue accessing the cache even while a miss is being serviced.

**Terminology:**
- **Hit under miss:** Can service cache hits while one miss is outstanding
- **Hit under multiple misses:** Can service hits while multiple misses are outstanding
- **Miss under miss:** Can issue another miss while first miss is being processed

**Miss Status Holding Registers (MSHRs):** Track outstanding misses
- More MSHRs → more misses can be outstanding simultaneously
- Each MSHR stores: address, which byte requested, state

### 6.12 Prefetching Strategies ⭐

#### Quick Visual 🧩

```
Predict future addresses -> fetch early

If correct: lower miss penalty
If wrong: extra bandwidth waste
```

| Type | How | Pros | Cons |
|---|---|---|---|
| **Hardware prefetch** | Detect stride access patterns | Transparent to software | Hardware overhead |
| **Software prefetch** | Compiler inserts prefetch instructions | Precise control | Instruction overhead |
| **Stream buffer** | Prefetch next sequential blocks | Simple | Only works for sequential |
| **Stride prefetch** | Detect constant stride (e.g., array columns) | Good for matrix ops | Complex pattern detection |

**Prefetch effectiveness depends on:**
- Prefetch distance: too early → evicted before use; too late → data not ready
- Coverage: % of misses that are prefetched
- Accuracy: % of prefetched blocks actually used (useless prefetches waste bandwidth)

### 6.13 Write Buffer ⭐

#### Quick Visual 🧩

```
CPU write -> Write Buffer -> lower memory level

CPU continues while writes drain in background
```

Holds data waiting to be written to next memory level.

| Cache Policy | Write Buffer Behavior |
|---|---|
| Write-through | Buffers ALL writes to memory |
| Write-back | Buffers only dirty evictions |

**Problem:** If write buffer full → CPU stalls on writes (write buffer stall)
**Problem:** Read miss may need data that's still in write buffer → must check write buffer on reads!

### 6.14 Compiler Cache Optimizations ⭐

**1. Loop Interchange:** Change iteration order to improve spatial locality
```c
// Poor locality (column-major in row-major language):
for (j=0; j<N; j++)          // Improvement:
  for (i=0; i<N; i++)        for (i=0; i<N; i++)
    a[i][j] = 0;               for (j=0; j<N; j++)
                                  a[i][j] = 0;
```

**2. Loop Blocking (Tiling):** Break large array processing into cache-sized blocks
```c
// Without blocking: thrashes cache for large matrices
for (i=0; i<N; i++)
  for (j=0; j<N; j++)
    for (k=0; k<N; k++)
      C[i][j] += A[i][k] * B[k][j];

// With blocking (block size B):
for (ii=0; ii<N; ii+=B)
  for (jj=0; jj<N; jj+=B)
    for (kk=0; kk<N; kk+=B)
      for (i=ii; i<min(ii+B,N); i++)
        for (j=jj; j<min(jj+B,N); j++)
          for (k=kk; k<min(kk+B,N); k++)
            C[i][j] += A[i][k] * B[k][j];
```
- Choose B so that B² entries fit in cache
- Reduces capacity misses dramatically!

**3. Array Padding:** Add extra elements to arrays to avoid conflict misses
```c
// If cache has 256 sets and array rows are 256×4 = 1024 bytes:
int A[1024][1024];   // Row i and row i+1 map to SAME cache sets!
int A[1024][1028];   // Pad by 4 elements → different mapping → no conflicts!
```

### 6.15 Cache Coherence: Snooping Protocol Detail ⭐⭐

#### Quick Visual 🧩

```
P1 writes shared line X
 -> Bus invalidate/update
 -> Other cache copies become invalid/stale-safe
```

**How Snooping Works:**
1. All caches monitor (snoop) the shared bus
2. When any cache performs a memory operation, all caches see it
3. Each cache takes action based on the bus transaction and its current state

**Bus Transactions (MESI):**
| Bus Signal | Meaning |
|---|---|
| BusRd | Read request (another cache wants to read) |
| BusRdX | Read with intent to modify (exclusive read) |
| BusUpgr | Upgrade: I already have copy, want to write |
| Flush | Write dirty data back to memory |

**Complete MESI State Transition on Bus Transactions:**

**In Modified (M) state, observing:**
- BusRd → Flush data to bus, go to **S** (share with requester)
- BusRdX → Flush data to bus, go to **I** (hand over ownership)

**In Exclusive (E) state, observing:**
- BusRd → Go to **S** (now shared)
- BusRdX → Go to **I** (requester wants exclusive)

**In Shared (S) state, observing:**
- BusRdX → Go to **I** (requester wants exclusive)
- BusUpgr → Go to **I** (someone else is upgrading)

**In Invalid (I) state:**
- Not monitoring (already invalid)

### 6.16 Split vs Unified Cache ⭐

#### Quick Visual 🧩

```
Split cache:   L1I + L1D separate (less contention)
Unified cache: one shared cache for I+D (flexible use)
```

| Feature | Split Cache | Unified Cache |
|---|---|---|
| Organization | Separate I-cache + D-cache | Single cache for both |
| Structural hazards | None (separate ports) | Possible (I-fetch + D-access) |
| Flexibility | Fixed allocation | Can adapt to workload |
| Miss rate | Each may be higher | Usually lower (more capacity) |
| Used in | L1 (always split) | L2, L3 (always unified) |

**Why L1 is always split:**
- Pipeline needs simultaneous instruction fetch + data access
- Split cache eliminates structural hazard between IF and MEM stages
- Each can be optimized: I-cache is read-only → simpler!

### 6.17 Cache Performance Equation (Advanced) ⭐⭐

**Full memory stall model:**
$$\text{CPU Time} = IC \times (CPI_{execution} + \text{Memory Stalls per Instruction}) \times T_{clock}$$

$$\text{Memory Stalls} = \text{I-cache stalls} + \text{D-cache stalls}$$

$$\text{I-cache stalls} = \text{I-cache accesses/instr} \times \text{I-miss rate} \times \text{miss penalty}$$

$$\text{D-cache stalls} = \text{D-cache accesses/instr} \times \text{D-miss rate} \times \text{miss penalty}$$

**For unified cache:**
$$\text{Memory stalls} = \frac{\text{accesses}}{\text{instruction}} \times \text{miss rate} \times \text{miss penalty}$$

**Worked Example — Impact of Doubling Cache:**
Processor A: CPI_base = 2, I-miss = 4%, D-miss = 10%, 40% mem operations, penalty = 100
- Memory stalls = (1×0.04 + 0.4×0.10) × 100 = 8 cycles/instruction
- CPI_A = 2 + 8 = 10

Processor B (double cache): CPI_base = 2, I-miss = 2%, D-miss = 5%, penalty = 100
- Memory stalls = (1×0.02 + 0.4×0.05) × 100 = 4 cycles/instruction
- CPI_B = 2 + 4 = 6

**Speedup from doubling cache = 10/6 = 1.67×** — Huge improvement despite only halving miss rate!

### 6.18 Cache-Oblivious Algorithms ⭐

#### Quick Visual 🧩

```
Algorithm recursively divides problem
until subproblems naturally fit in cache,
without hardcoding cache size parameters.
```

Algorithms that perform well on any cache size WITHOUT knowing the cache parameters.

**Example:** Cache-oblivious matrix transpose
- Instead of naive row-by-row transpose (poor locality for large matrices)
- Recursively divide matrix into quadrants until they fit in cache
- Works well for any cache size!

> 🎯 **GATE usually doesn't ask about cache-oblivious algorithms, but understanding the concept helps with optimization questions.**

### 6.19 Associative Memory (Content Addressable Memory — CAM) ⭐⭐

> 💡 **Regular RAM:** You give an address → get data. **CAM:** You give data (or part of it) → get the address/match result! Used in TLB, cache tags, and network routers.

#### CAM Hardware Structure 🔧

```
┌──────────────────────────────────────────────────┐
│            Associative Memory (CAM)               │
│                                                    │
│  Argument Register (search key):                  │
│  ┌────┬────┬────┬────┬────┐                      │
│  │ A₄ │ A₃ │ A₂ │ A₁ │ A₀ │                      │
│  └────┴────┴────┴────┴────┘                      │
│                                                    │
│  Key Register (mask — which bits to compare):     │
│  ┌────┬────┬────┬────┬────┐                      │
│  │ K₄ │ K₃ │ K₂ │ K₁ │ K₀ │                      │
│  └────┴────┴────┴────┴────┘                      │
│  (Kᵢ = 1 → compare bit i; Kᵢ = 0 → don't care)  │
│                                                    │
│  Memory array (m words × n bits):                 │
│  Word 0: ┌────┬────┬────┬────┬────┐  Match₀      │
│          │    │    │    │    │    │→ [M₀]          │
│  Word 1: ├────┼────┼────┼────┼────┤  Match₁      │
│          │    │    │    │    │    │→ [M₁]          │
│  Word 2: ├────┼────┼────┼────┼────┤  Match₂      │
│          │    │    │    │    │    │→ [M₂]          │
│          └────┴────┴────┴────┴────┘               │
│                                                    │
│  Match Register: [M₀, M₁, M₂, ...]               │
│  Mᵢ = 1 if word i matches the masked argument    │
└──────────────────────────────────────────────────┘
```

**Match Logic per cell:**
$$M_i = \prod_{j=0}^{n-1} (K_j' + A_j \odot W_{ij})$$

Where ⊙ is XNOR (equality check): A_j ⊙ W_{ij} = 1 when A_j = W_{ij}

If K_j = 0 → that bit is "don't care" (K_j' = 1 → always passes)
If K_j = 1 → compare A_j with W_{ij}

**CAM vs RAM Comparison:**
| Feature | RAM | CAM (Associative Memory) |
|---|---|---|
| Input | Address | Data (search key) |
| Output | Data | Match result + address |
| Access time | O(1) | O(1) parallel comparison! |
| Hardware cost | Low | Very high (comparator per cell) |
| Used for | Main memory | TLB, cache tags, routers |
| Power | Low | High (all words searched simultaneously) |

**TCAM (Ternary CAM):**
- Each cell stores 0, 1, or X (don't care)
- Used in network routers for longest prefix matching
- Even more expensive than binary CAM

> 🎯 **GATE Application:** TLB is essentially a small CAM — the virtual page number is the search key, and the physical frame number is the associated data. All entries are searched in parallel → O(1) lookup!

### 6.20 Complete Address Translation: TLB + Cache Walkthrough ⭐⭐⭐

> 🏭 **This is the complete flow when CPU issues a memory access — the most important memory hierarchy concept for GATE.**

```
 CPU issues Virtual Address (VA)
         │
         ▼
  ┌──────────────────┐
  │ Split VA into     │
  │ VPN + Page Offset │
  └────────┬─────────┘
           │
           ▼
  ┌──────────────────┐     TLB Hit?
  │   TLB Lookup     │────── YES ──→ Get PFN from TLB
  │   (Parallel)     │                    │
  └────────┬─────────┘                    │
           │ NO (TLB Miss)               │
           ▼                              │
  ┌──────────────────┐                    │
  │ Page Table Walk   │                    │
  │ (1-4 mem accesses)│                   │
  └────────┬─────────┘                    │
           │                              │
    Page in memory?                       │
    ├── YES: Get PFN, update TLB          │
    │                                     │
    └── NO: PAGE FAULT! 💥               │
        → OS handles:                     │
        → Bring page from disk            │
        → Update page table               │
        → Restart instruction             │
           │                              │
           └──────────┬───────────────────┘
                      │
                      ▼
            Construct Physical Address
            PA = PFN | Page Offset
                      │
                      ▼
          ┌──────────────────────┐
          │    Cache Lookup       │
          │ (using PA or parts)   │
          └──────────┬───────────┘
                     │
              Cache Hit?
              ├── YES → Return data to CPU ✅
              │
              └── NO (Cache Miss) →
                  Fetch block from Main Memory
                  → Place in cache (may evict)
                  → Return data to CPU ✅
```

**Worked Example — Complete Address Translation** 🔢

**System parameters:**
- Virtual address: 32 bits
- Physical address: 28 bits
- Page size: 4 KB (12-bit offset)
- TLB: 64 entries, fully associative, access time = 2 ns
- L1 Cache: 16 KB, 4-way set associative, block size = 64 bytes, access time = 3 ns
- Main memory access time: 100 ns
- Page table: 2-level

**Address breakdown:**
```
Virtual Address (32 bits):
┌──────────────────┬──────────────┐
│   VPN (20 bits)   │ Offset (12)  │
│ ┌───────┬────────┐│              │
│ │L1(10) │L2(10)  ││              │
│ └───────┴────────┘│              │
└──────────────────┴──────────────┘

Physical Address (28 bits):
┌──────────────────┬──────────────┐
│   PFN (16 bits)   │ Offset (12)  │
└──────────────────┴──────────────┘

Cache Address (from PA):
┌──────┬───────────┬──────────────┐
│Tag(12)│ Index(6)  │ Block Off(6) │  (cache: 256 blocks, 64 sets)
│       │ + Way(2)  │   + Word     │
└──────┴───────────┴──────────────┘
```

**Scenario: CPU accesses VA = 0x00A0_3F40**

1. **VPN** = 0x00A03 (upper 20 bits), **Offset** = 0xF40

2. **TLB lookup** (2 ns): Search for VPN = 0x00A03
   - TLB Hit! → PFN = 0x1234 (from TLB entry)
   
3. **Construct PA** = 0x1234F40 (PFN concatenated with offset)

4. **Cache lookup** (3 ns):
   - Block offset = lower 6 bits of PA = 0x00 (0)
   - Index = next 6 bits = 0x3D (61)
   - Tag = upper 16 bits = 0x1234
   - Check set 61, all 4 ways → Tag match in way 2! Cache hit!

5. **Total access time** = TLB (2 ns) + Cache (3 ns) = **5 ns** ✅

**What if TLB miss + Cache hit?**
- TLB miss: page table walk = 2 × 100 ns = 200 ns
- Update TLB, then cache lookup: 3 ns
- Total: 200 + 2 + 3 = **205 ns** (40× slower!)

**What if TLB miss + Cache miss?**
- TLB miss: 200 ns
- Cache miss: 100 ns (fetch from memory)
- Total: 200 + 2 + 100 + 3 = **305 ns**

**What if Page Fault?**
- Page fault: disk access ≈ 10 ms = 10,000,000 ns 💥
- Total: ~**10 ms** (2,000,000× slower than cache hit!)

> 🎯 **GATE Insight:** This is why TLB hit rate matters SO much. Even a 1% TLB miss rate can double memory access time!

**Effective Memory Access Time (EMAT) — Complete Formula:**

$$\text{EMAT} = h_{TLB} \times [t_{TLB} + h_{cache} \times t_{cache} + (1-h_{cache})(t_{cache} + t_{mem})]$$
$$+ (1-h_{TLB}) \times [t_{TLB} + k \times t_{mem} + h_{cache} \times t_{cache} + (1-h_{cache})(t_{cache} + t_{mem})]$$

Where: $h_{TLB}$ = TLB hit rate, $h_{cache}$ = cache hit rate, $k$ = page table levels, $t_{TLB}$ = TLB access time, $t_{cache}$ = cache access time, $t_{mem}$ = memory access time

### 6.21 Page Fault Handling — Complete Flow ⭐⭐

```
   CPU accesses page not in memory
              │
              ▼
   ┌────────────────────────┐
   │ 1. Hardware detects    │
   │    Valid bit = 0 in PT │
   │    → TRAP to OS        │
   └──────────┬─────────────┘
              │
              ▼
   ┌────────────────────────┐
   │ 2. OS saves CPU state  │
   │    (registers, PC)     │
   └──────────┬─────────────┘
              │
              ▼
   ┌────────────────────────┐
   │ 3. Find free frame     │──→ No free frame?
   │    in physical memory  │    → Run replacement algorithm
   └──────────┬─────────────┘    → If victim is dirty → write to disk
              │
              ▼
   ┌────────────────────────┐
   │ 4. Issue disk I/O      │
   │    (bring page from    │
   │    swap space/disk)    │
   │    → Context switch!   │
   └──────────┬─────────────┘
              │ (disk I/O done)
              ▼
   ┌────────────────────────┐
   │ 5. Update page table   │
   │    (PFN, Valid=1,      │
   │    Dirty=0)            │
   └──────────┬─────────────┘
              │
              ▼
   ┌────────────────────────┐
   │ 6. Invalidate TLB entry│
   │    (old mapping)       │
   └──────────┬─────────────┘
              │
              ▼
   ┌────────────────────────┐
   │ 7. Restart the faulting│
   │    instruction         │
   └────────────────────────┘
```

**Page Replacement Algorithms (COA perspective):**
| Algorithm | Description | Optimal? |
|---|---|---|
| **FIFO** | Replace oldest page | No (Belady's anomaly possible) |
| **LRU** | Replace least recently used | Near-optimal |
| **Optimal (OPT)** | Replace page not used for longest | Yes (but needs future knowledge!) |
| **Clock (Second-chance)** | FIFO + reference bit | Good approximation of LRU |

> 🎯 **GATE Fact:** Page faults are handled by the OS, not hardware. The CPU hardware only detects the fault (valid bit = 0) and triggers the trap.

### 6.22 Memory Interleaving ⭐⭐

| Type | Addressing | Best For |
|---|---|---|
| Low-order | Addr mod m = module | Sequential access (cache fills) |
| High-order | Addr / size = module | Multi-process, independent |

With m modules, access time T: Effective time = T/m (low-order sequential)
Need: m ≥ T/bus_cycle for full bandwidth.

### 6.23 Virtual Memory (COA Perspective) ⭐⭐⭐

**Translation:**
```
VA: | VPN | Offset | → Page Table → PA: | PFN | Offset |
```

**Worked Example:** 32-bit VA, 28-bit PA, 4KB pages:
- Offset = 12 bits, VPN = 20 bits (1M pages), PFN = 16 bits (64K frames)
- PT size = 2²⁰ × 4B = **4 MB** → too large! → Use multi-level

**Multi-Level Page Table:** 32-bit VA, 4KB pages, 4B PTEs (1024 entries = 10 bits):
- L1 index=10, L2 index=10, offset=12
- Translation: 2 memory accesses + 1 data = 3 total (without TLB)

#### TLB (Translation Lookaside Buffer) ⭐⭐⭐
- Cache for PTEs, typically 64-512 entries, fully associative

**EAT with TLB (k-level page table):**
$$\text{EAT} = h \times (t_{TLB} + t_{mem}) + (1-h) \times (t_{TLB} + (k+1) \times t_{mem})$$

**Worked Example:**
TLB hit rate=98%, TLB=2ns, Mem=100ns, 2-level PT:
EAT = 0.98×102 + 0.02×302 = **106 ns** (vs 300 without TLB → 2.83× speedup)

**TLB Coverage = entries × page_size** (256 × 4KB = 1MB)

**Cache + TLB Interaction:**
| Type | Speed | Issues |
|---|---|---|
| PIPT (Physical/Physical) | Slow (wait for TLB) | No aliasing |
| VIPT (Virtual/Physical) | Fast (overlap) | Works if cache ≤ page_size × assoc |
| VIVT (Virtual/Virtual) | Fastest | Aliasing; flush on context switch |

#### Page Table Structures
| Structure | Space | Lookup |
|-----------|-------|--------|
| Single-level | Large | 1 memory access |
| Multi-level | Proportional to used pages | k accesses |
| Inverted | One entry per physical frame | Hash lookup |
| Hashed | Hash table + chain | ~1-2 lookups |

### 6.24 Multiprocessor Cache Coherence ⭐⭐

#### Cache Coherence Problem
Multiple processors with private caches → write by one may not be seen by others → stale data.

#### MESI Protocol ⭐⭐
Each cache line in one of 4 states:

| State | Meaning | Memory Consistent? |
|-------|---------|-------------------|
| **M** (Modified) | Only copy, dirty | No |
| **E** (Exclusive) | Only copy, clean | Yes |
| **S** (Shared) | Multiple copies, clean | Yes |
| **I** (Invalid) | Not valid | N/A |

**Key State Transitions:**
- **Read hit in S/E/M → stay** (no bus action)
- **Read miss (no other copy) → E** (fetched from memory)
- **Read miss (other has S/E) → S** (other stays S)
- **Read miss (other has M) → S** (other writes back, goes to S)
- **Write hit in M → M** (no bus action)
- **Write hit in E → M** (silent upgrade, no bus)
- **Write hit in S → M** (invalidate other copies on bus)
- **Write miss → M** (fetch + invalidate)

#### MESI Protocol Worked Example ⭐⭐

Two processors P1 and P2, both have a block X initially Invalid.

| Step | P1 Action | P1 State | P2 State | Bus Transaction | Memory |
|---|---|---|---|---|---|
| 1 | P1 reads X | **E** | I | BusRd, no other copy | Read from mem |
| 2 | P2 reads X | **S** | **S** | BusRd, P1 snoops, goes S | — |
| 3 | P1 writes X | **M** | **I** | BusUpgr (invalidate P2) | — |
| 4 | P2 reads X | **S** | **S** | BusRd, P1 flush+→S | Writeback |
| 5 | P2 writes X | **I** | **M** | BusUpgr (invalidate P1) | — |
| 6 | P1 reads X | **S** | **S** | BusRd, P2 flush+→S | Writeback |

**Key observations:**
- Step 1→2: E to S is a "downgrade" — no bus write, data is clean
- Step 2→3: S to M requires invalidation broadcast
- Step 3→4: M to S requires flush (writeback) because data is dirty
- Steps 3 and 5: Write to shared line always generates bus transaction (invalidation)

> 🎯 **GATE Scenario:** "How many bus transactions occur?" Count upgrades, invalidations, and flushes!

#### Write-Invalidate vs Write-Update
| Feature | Write-Invalidate | Write-Update (Write-Broadcast) |
|---------|-----------------|-------------------------------|
| On write | Invalidate other copies | Update other copies |
| Bus traffic | Lower (one invalidation) | Higher (every write broadcast) |
| Used by | Most modern CPUs (MESI) | Rare (Dragon protocol) |

#### False Sharing
Two processors access **different** variables that happen to be in the **same cache line**. Causes ping-pong invalidations even though there's no true sharing. Solution: padding to separate variables into different cache lines.

#### Memory Consistency Models
- **Sequential Consistency:** All processors see memory operations in the same global order. Strongest model.
- **Total Store Order (TSO):** Stores can be reordered after loads (x86 uses this).
- **Relaxed Models:** More reordering allowed, better performance, requires memory fences.

---

### 6.25 Disk Scheduling Algorithms ⭐⭐

Given a disk with 200 tracks (0-199), current head position at track 50, and request queue: 95, 180, 34, 119, 11, 123, 62, 64

#### FCFS (First Come First Served)
Service in order of arrival: 95→180→34→119→11→123→62→64
```
Head movement: |50-95| + |95-180| + |180-34| + |34-119| + |119-11| + |11-123| + |123-62| + |62-64|
             = 45 + 85 + 146 + 85 + 108 + 112 + 61 + 2 = 644 tracks
```

#### SSTF (Shortest Seek Time First)
Always go to nearest request:
```
50 → 62(12) → 64(2) → 34(30) → 11(23) → 95(84) → 119(24) → 123(4) → 180(57)
Total = 12+2+30+23+84+24+4+57 = 236 tracks
```
**Problem:** Starvation of far requests (like FCFS in scheduling)

#### SCAN (Elevator Algorithm) ⭐
Move in one direction, service all requests, reverse at end:
```
Direction: toward 0
50 → 34(16) → 11(23) → 0(11) [reverse] → 62(62) → 64(2) → 95(31) → 119(24) → 123(4) → 180(57)
Total = 16+23+11+62+2+31+24+4+57 = 230 tracks
```

#### C-SCAN (Circular SCAN)
Move in one direction, service requests, jump back to start:
```
Direction: toward 199
50 → 62(12) → 64(2) → 95(31) → 119(24) → 123(4) → 180(57) → 199(19) [jump to 0] → 11(11) → 34(23)
Total = 12+2+31+24+4+57+19+199+11+23 = 382 tracks (including jump)
```
**Note:** Jump time often discounted in some formulations (just count seek, not return)

#### LOOK (No End Reversal)
Like SCAN but reverses at last request (not disk end):
```
Direction: toward 0
50 → 34(16) → 11(23) [reverse at 11, not 0] → 62(51) → 64(2) → 95(31) → 119(24) → 123(4) → 180(57)
Total = 16+23+51+2+31+24+4+57 = 208 tracks
```

#### C-LOOK
Like C-SCAN but reverses at last request:
```
Direction: toward 199
50 → 62(12) → 64(2) → 95(31) → 119(24) → 123(4) → 180(57) [jump to 11] → 11(169) → 34(23)
Total = 12+2+31+24+4+57+169+23 = 322 tracks
```

**Comparison Table:**
| Algorithm | Total Seeks (above example) | Starvation? | Fairness | Performance |
|---|---|---|---|---|
| FCFS | 644 | No | Best | Worst |
| SSTF | 236 | Yes | Poor | Good |
| SCAN | 230 | Low | Good | Good |
| C-SCAN | 382* | No | Best | Medium |
| LOOK | 208 | Low | Good | Better |
| C-LOOK | 322* | No | Very Good | Medium |

> 🎯 **GATE Tip:** SCAN and LOOK are most commonly asked! Remember LOOK doesn't go to the disk edges.

---

### 6.26 Virtual Memory — Advanced Topics ⭐⭐

#### Inverted Page Table ⭐⭐
- **Problem:** In normal page table, entry for every virtual page → huge for 64-bit systems
- **Solution:** One entry per **physical frame** instead

**Structure:** | PID | VPN | Chain pointer |
- Size = number_of_physical_frames × entry_size (FIXED, independent of VA space)
- Lookup: Hash VPN → search chain for matching (PID, VPN)

**Trade-offs:**
| Normal PT | Inverted PT |
|---|---|
| Size ∝ virtual address space | Size ∝ physical memory |
| Fast lookup (direct index) | Slower lookup (hash + chain) |
| Large for 64-bit VA | Fixed size |
| Supports sharing easily | Sharing complex |
| Used by: x86, ARM | Used by: IBM PowerPC, HP PA-RISC |

**Example:** 64-bit VA, 32 GB RAM, 4 KB pages:
- Normal PT: 2⁵² entries × 8B = **32 PB per process!!** (impossible)
- Inverted PT: 32GB/4KB = 8M entries × 12B = **96 MB total** (shared)

#### Demand Paging ⭐⭐

**Page Fault Handling Steps:**
1. Instruction references a page not in memory
2. **Trap** to OS (page fault)
3. OS checks if reference is valid (invalid → segfault)
4. Find a free frame (or use page replacement if none)
5. Disk I/O: read page from swap space into free frame (~10 ms!)
6. Update page table entry (set valid bit, frame number)
7. Restart the instruction that caused the fault

**Effective Memory Access Time with Page Faults:**
$$\text{EAT} = (1-p) \times t_{mem} + p \times t_{page\_fault}$$

Where p = page fault rate, t_page_fault ≈ 10 ms (10,000,000 ns!)

**Example:** Memory access = 100 ns, page fault time = 10 ms, fault rate = 1/1000:
$$\text{EAT} = 0.999 \times 100 + 0.001 \times 10,000,000 = 99.9 + 10,000 = 10,099.9 \text{ ns}$$

Even 0.1% fault rate → **100× slowdown!**

To keep EAT < 110 ns (10% degradation):
$$(1-p) \times 100 + p \times 10^7 < 110$$
$$p < 10/(10^7) = 10^{-6}$$

Need fault rate < 1 in a million!

#### Page Replacement Algorithms (COA Perspective) ⭐⭐

| Algorithm | Method | Belady's Anomaly? | Optimal? |
|---|---|---|---|
| **Optimal (OPT/MIN)** | Replace page used farthest in future | No | Yes (theoretical) |
| **FIFO** | Replace oldest page | **Yes** | No |
| **LRU** | Replace least recently used | No | Near-optimal |
| **Clock (Second Chance)** | Modified FIFO with reference bit | Yes (mildly) | No |
| **LFU** | Replace least frequently used | No | Depends |

**Belady's Anomaly:** With FIFO, increasing frames can increase page faults!

**Example of Belady's Anomaly:**
Reference string: 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

| Frames | Page Faults (FIFO) |
|---|---|
| 3 | 9 |
| 4 | **10** (more faults with more frames!) |

> 🎯 **GATE Trap:** LRU does NOT exhibit Belady's anomaly. FIFO does. This is a very common GATE question!

#### Segmentation vs Paging ⭐

| Feature | Paging | Segmentation |
|---|---|---|
| Division | Fixed-size pages | Variable-size segments |
| VA format | Page number + offset | Segment number + offset |
| Fragmentation | Internal | External |
| Sharing | Page-level (difficult) | Segment-level (natural) |
| Protection | Page-level | Segment-level (matches program structure) |
| Size | Fixed | Variable (matches logical units) |
| Hardware | Simpler | More complex (segment table + bounds check) |

**Segmented Paging:** Combines both → Segment table points to page table for each segment.

---

### 6.27 MOESI Protocol (Extension of MESI) ⭐

| State | Meaning | Who has valid data? |
|---|---|---|
| **M** (Modified) | Only copy, dirty | Cache only |
| **O** (Owned) | Dirty, but may be shared | This cache + possibly shared copies |
| **E** (Exclusive) | Only copy, clean | Cache = Memory |
| **S** (Shared) | Clean copy | Cache = Memory (or cache if O elsewhere) |
| **I** (Invalid) | Not valid | Memory (or O/M holder) |

**Key difference from MESI:**
- In MESI, when M-state cache gets a read from another processor, it must write back to memory
- In MOESI, it goes to **O state** — provides data directly to requester WITHOUT writing to memory
- **Saves memory writes!** Used in AMD processors.

### 6.28 Directory-Based Cache Coherence ⭐

For large-scale systems (>64 processors), bus-based snooping doesn't scale.

**Solution:** Central directory tracks which caches have copies of each block.

**Directory entry per block:**
| Dirty bit | Sharing vector (1 bit per processor) |
|---|---|

**On read miss:** Check directory → forward to owning cache or memory
**On write miss:** Check directory → invalidate all sharers → grant exclusive access

**Comparison:**
| Feature | Snooping (Bus) | Directory-Based |
|---|---|---|
| Scalability | ~16-32 processors | 100+ processors |
| Latency | Low (broadcast) | Higher (directory indirection) |
| Bandwidth | High (every miss on bus) | Lower (point-to-point) |
| Hardware | Simpler | Complex (directory storage) |

---

## 8. I/O Organization ⚡

> 💡 **Analogy:** I/O is like communicating with the outside world 🌍. The CPU is the brain, and I/O devices are the eyes (input: camera, keyboard) and hands (output: monitor, printer). The question is: HOW does the brain coordinate with its limbs efficiently?

### 8.1 I/O Addressing ⭐

#### Quick Visual 🧩

```
Memory-mapped I/O:  CPU uses normal load/store to device addresses
Port-mapped I/O:    CPU uses special IN/OUT instructions on separate ports
```

| Type | I/O Address Space | Instructions | Example |
|---|---|---|---|
| **Memory-Mapped** | Shared with memory | Same (LOAD/STORE) | ARM, MIPS |
| **I/O-Mapped (Isolated)** | Separate | Special (IN/OUT) | x86 |

**Memory-Mapped I/O Advantages:**
- Can use all memory instructions (arithmetic, logical)
- No special I/O instructions needed
- Protection via virtual memory
- **Disadvantage:** Reduces memory address space

### 8.2 I/O Techniques ⭐⭐⭐

#### Programmed I/O (Polling)
```
while (device_status != READY)     ← CPU busy-waiting!
    continue;
transfer_data();
```
- CPU continuously polls device status (busy-wait loop)
- CPU involved 100% of time → wastes cycles
- Simple but inefficient. Only good for dedicated controllers.

#### Interrupt-Driven I/O ⭐⭐
CPU does other work; device sends **interrupt** when ready.
```
CPU running program → Interrupt signal → Save context → ISR executes → Restore context → Resume
```

**Interrupt Types:**
| Type | Source provides | CPU action |
|---|---|---|
| **Vectored** | Interrupt vector (ISR address) | Jump directly to ISR |
| **Non-vectored** | Just interrupt signal | CPU polls to find source |

**Interrupt Priority Methods:**
- **Daisy Chain:** Serial priority — closest device = highest priority
- **Priority Encoder:** Parallel — hardware encoder selects highest priority
- **Software Polling:** CPU polls each device in priority order (slow)

**Interrupt Latency = Time from interrupt request to first instruction of ISR**
Includes: finishing current instruction + saving context + vectoring

#### DMA (Direct Memory Access) ⭐⭐⭐

> 💡 **Analogy:** DMA is like hiring a moving company 🚛. Instead of the CEO (CPU) carrying boxes himself, he gives instructions to the movers (DMA controller) who handle the bulk transfer while the CEO does other work!

DMA controller transfers data directly between device and memory:
- CPU sets up DMA: **start address, byte count, direction (read/write)**
- DMA controller takes over bus and transfers data
- CPU interrupted only when transfer complete

**DMA Transfer Modes:**
| Mode | Description | CPU Impact |
|---|---|---|
| **Cycle Stealing** | DMA steals 1 bus cycle at a time | CPU briefly paused per word |
| **Burst Mode** | DMA takes bus for entire block | CPU blocked for whole transfer |
| **Interleaved** | DMA uses bus when CPU doesn't need it | Minimal CPU impact |

**DMA Transfer Time Calculation:**
$$T_{DMA} = T_{setup} + N \times T_{cycle} + T_{cleanup}$$

Where N = number of words, T_cycle = memory cycle time

**Worked Example:**
Transfer 1000 words from disk to memory. Setup = 1000 ns, memory cycle = 50 ns.
- **Burst mode:** T = 1000 + 1000×50 = 1000 + 50000 = **51,000 ns** (CPU blocked 50,000 ns)
- **Cycle stealing:** T = 1000 × (50 + overhead) — CPU only blocked 50 ns per word

**I/O Bandwidth Calculation:**
Device transfer rate = 10 MB/s, Word = 4 bytes:
- Words/sec = 10M/4 = 2.5M words/sec
- If cycle stealing: 2.5M bus cycles stolen per second

**Comparison of I/O Techniques:**
| Feature | Programmed I/O | Interrupt I/O | DMA |
|---|---|---|---|
| CPU involvement | 100% (polling) | Only during ISR | Only setup/cleanup |
| Data path | CPU ↔ Device | CPU ↔ Device | Device ↔ Memory |
| Hardware | Minimal | Interrupt controller | DMA controller |
| Efficiency | Worst | Good | Best for bulk |
| Use case | Simple embedded | Small transfers | Large block transfers |

### 8.3 Bus Architecture & Arbitration ⭐⭐

#### Quick Visual 🧩

```
Many bus masters -> arbitration logic -> one winner gets bus this cycle
```

**Bus Arbitration Methods:**

| Method | How it works | Fairness | Speed |
|---|---|---|---|
| **Daisy Chain** | Grant passes through serial chain | Low (fixed priority) | Fast |
| **Centralized Parallel** | Each device has own request/grant line | Configurable | Medium |
| **Distributed** | Devices collectively decide (self-select) | Fair | Medium |
| **Polling** | Arbiter polls devices sequentially | Fair | Slow |

**Synchronous vs Asynchronous Bus:**
| Feature | Synchronous | Asynchronous |
|---|---|---|
| Timing | Global clock | Handshake (req/ack) |
| Speed | Fixed by clock | Adapts to device |
| Design | Simple | Complex |
| Bus length | Short (clock skew) | Can be long |
| Example | PCI | SCSI |

### 8.4 I/O Interface — Detailed Block Diagram ⭐⭐

> 💡 **The I/O interface sits between the system bus and the peripheral device, handling data format conversion, timing differences, and control signals.**

```
┌───────────────────────────────────────────────────────────┐
│                    I/O Interface Module                     │
│                                                            │
│  System Bus Side              │     Device Side            │
│  ═══════════════              │     ═══════════            │
│                               │                            │
│  ┌──────────────┐            │    ┌─────────────────┐     │
│  │ Data Bus ←──→│ Data       │    │ Data to/from    │     │
│  │ (bidirectional)│ Register │←──→│ Device           │     │
│  └──────────────┘            │    └─────────────────┘     │
│                               │                            │
│  ┌──────────────┐            │    ┌─────────────────┐     │
│  │ Address Bus ─→│ Address   │    │ Device select   │     │
│  │               │ Decoder   │───→│ (chip select)   │     │
│  └──────────────┘            │    └─────────────────┘     │
│                               │                            │
│  ┌──────────────┐            │    ┌─────────────────┐     │
│  │ Control Bus ─→│ Control & │    │ Device-specific │     │
│  │ (RD/WR/IRQ) │ Status Reg │←──→│ control signals │     │
│  └──────────────┘            │    └─────────────────┘     │
│                               │                            │
│  ┌──────────────┐            │                            │
│  │ IRQ Line    ←─┤ Interrupt │                            │
│  │ (to CPU)     │ Logic     │                            │
│  └──────────────┘            │                            │
└───────────────────────────────────────────────────────────┘
```

**Interface Registers (accessible by CPU):**
| Register | Function | Access |
|---|---|---|
| **Data Register** | Holds data being transferred | Read/Write |
| **Status Register** | Device state (ready, busy, error) | Read only |
| **Command Register** | CPU commands to device | Write only |
| **Control Register** | Configuration (mode, speed) | Read/Write |

### 8.5 Priority Interrupt — Daisy Chain ⭐⭐⭐

> 💡 **Daisy chain** is the simplest hardware priority scheme. Devices are connected in series — the closest device to the CPU has highest priority.

```
CPU                Device 1         Device 2         Device 3
 │                (Highest)        (Medium)         (Lowest)
 │                  │                │                │
 │◄── IRQ ─────────┤────────────────┤────────────────┤
 │                  │                │                │
 │── INTA ────→ ┌───┴───┐      ┌───┴───┐      ┌───┴───┐
 │              │ Need   │──NO──│ Need   │──NO──│ Need   │
 │              │service?│      │service?│      │service?│
 │              └───┬───┘      └───┬───┘      └───┬───┘
 │                  │YES           │YES           │YES
 │              Block INTA     Block INTA     Block INTA
 │              (don't pass)   (don't pass)   (don't pass)
 │              Send vector    Send vector    Send vector
```

**Operation:**
1. Any device can assert the shared **IRQ** line (active low, wired-OR)
2. CPU asserts **INTA** (Interrupt Acknowledge) signal
3. INTA signal propagates through daisy chain in priority order
4. First device needing service **blocks INTA** from propagating further
5. That device places its **interrupt vector** on the data bus
6. CPU reads vector and jumps to corresponding ISR

**Advantage:** Simple hardware — just one INTA chain
**Disadvantage:** Fixed priority (closest = highest). Low-priority devices may **starve**.

> 🎯 **GATE Trap:** In daisy chain, if Device 1 continuously generates interrupts, Devices 2 and 3 will NEVER get serviced. This is called **starvation**!

### 8.6 Priority Interrupt — Parallel Priority (Priority Encoder) ⭐⭐

> 💡 Each device has its own interrupt request line → a hardware priority encoder selects the highest.

```
                    ┌────────────────────────┐
IRQ₀ (highest) ───→│                        │
IRQ₁          ───→│  Priority Encoder       │───→ Interrupt Vector
IRQ₂          ───→│  (Hardware)             │    (to CPU)
IRQ₃          ───→│                        │
IRQ₄          ───→│  Output: highest active │
IRQ₅          ───→│  IRQ number             │
IRQ₆          ───→│                        │
IRQ₇ (lowest) ───→│                        │
                    └────────────────────────┘
```

**Mask Register for Priority Control:**
```
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ M₇│ M₆│ M₅│ M₄│ M₃│ M₂│ M₁│ M₀│  Interrupt Mask Register
└───┴───┴───┴───┴───┴───┴───┴───┘
  Mᵢ = 1 → IRQᵢ disabled (masked)
  Mᵢ = 0 → IRQᵢ enabled
```

**Priority Resolution:**
- Hardware encoder outputs binary code of highest-priority unmasked request
- This code is used as index into Interrupt Vector Table (IVT)
- Much faster than daisy chain (parallel comparison vs serial propagation)

**Comparison:**
| Feature | Daisy Chain | Parallel Priority |
|---|---|---|
| Hardware | 1 INTA chain | n request lines + encoder |
| Speed | Slow (serial) | Fast (parallel) |
| Priority | Fixed (by position) | Programmable (via mask) |
| Starvation | Possible | Avoidable (rotating priority) |
| Cost | Low | Higher |

### 8.7 Serial Communication Protocols ⭐⭐

> 💡 **Serial communication** sends data one bit at a time. Used for long-distance and inter-chip communication.

#### UART (Universal Asynchronous Receiver/Transmitter) 📡

```
Idle (1) ─┐  ┌──┬──┬──┬──┬──┬──┬──┬──┬──┬───
          └──┤S │D₀│D₁│D₂│D₃│D₄│D₅│D₆│D₇│P │Stop(1)
             └──┴──┴──┴──┴──┴──┴──┴──┴──┴───
          Start  ←── 8 Data Bits ──→  Parity
          Bit                         Bit
```

| Parameter | Typical Value |
|---|---|
| Data bits | 5, 6, 7, or 8 |
| Parity | None, Even, Odd |
| Stop bits | 1 or 2 |
| Baud rate | 9600, 115200, etc. |

**Baud rate** = symbols per second = bits per second (for binary signaling)
**Throughput** = data bits / (start + data + parity + stop) × baud rate

**Example:** 8N1 at 9600 baud (8 data, No parity, 1 stop):
- Frame = 1(start) + 8(data) + 0(parity) + 1(stop) = 10 bits
- Effective data rate = 8/10 × 9600 = **7680 bps** = 960 bytes/sec

#### SPI (Serial Peripheral Interface) 📡

```
Master                              Slave
  │── SCLK (Clock) ─────────────→  │
  │── MOSI (Master Out Slave In) ─→  │
  │◄─ MISO (Master In Slave Out) ──  │
  │── SS (Slave Select, active low) →│
```

| Feature | SPI |
|---|---|
| Communication | Full duplex |
| Clock | Master provides |
| Wires | 4 (SCLK, MOSI, MISO, SS) |
| Speed | Up to 50+ MHz |
| Slaves | Multiple (one SS per slave) |
| Used for | SD cards, displays, sensors |

#### I²C (Inter-Integrated Circuit) 📡

```
Master ──── SDA (Data) ───── Slave 1 ──── Slave 2 ──── Slave 3
       ──── SCL (Clock) ────────────────────────────────────────
```

| Feature | I²C |
|---|---|
| Communication | Half duplex |
| Wires | 2 (SDA, SCL) |
| Speed | 100 kHz (standard), 400 kHz (fast), 3.4 MHz (high-speed) |
| Addressing | 7-bit or 10-bit slave address |
| Slaves | Up to 128 (7-bit address) on same bus |
| Used for | Sensors, EEPROMs, RTCs |

**Comparison of Serial Protocols:**
| Feature | UART | SPI | I²C |
|---|---|---|---|
| Wires | 2 (TX, RX) | 4+ | 2 |
| Speed | Low-Medium | Highest | Medium |
| Duplex | Full | Full | Half |
| Clock | Asynchronous | Synchronous | Synchronous |
| Multi-slave | No (point-to-point) | Yes (SS lines) | Yes (addressing) |
| Distance | Long (RS-232) | Short (PCB) | Short-Medium |
| Complexity | Simple | Medium | Medium |

> 🎯 **GATE (especially DA):** Know the basic operation and comparison of serial protocols. UART is most commonly asked.

### 8.8 I/O Processors and Channels ⭐

#### Quick Visual 🧩

```
CPU gives I/O program -> IOP executes device transfers -> CPU interrupted on completion
```

- **I/O Processor (IOP):** Dedicated processor for I/O operations
  - Has own instruction set for I/O programs
  - CPU sends I/O program to IOP; IOP executes independently
  - Used in mainframes (IBM channels)

- **Types:**
  | Type | Description | Analogy |
  |---|---|---|
  | Selector Channel | One device at a time (dedicated) | Single-lane road |
  | Multiplexor Channel | Multiple slow devices interleaved | Highway with many lanes |
  | Block Multiplexor | Block transfer from multiple fast devices | Express lanes |

### 8.9 Interrupt Handling Deep Dive ⭐⭐⭐

#### Complete Interrupt Processing Sequence

```
1. Device asserts IRQ line
2. CPU finishes current instruction
3. CPU checks for pending interrupts (IF flag set)
4. CPU acknowledges interrupt (INTA cycle)
5. Device provides interrupt vector number
6. CPU saves: PC, PSW (flags) → stack
7. CPU loads ISR address from IVT[vector × 4]
8. ISR executes (may save additional registers)
9. ISR sends EOI (End of Interrupt) to PIC
10. ISR restores registers, executes IRET/RTI
11. CPU restores PC, PSW from stack → resumes original program
```

#### Interrupt Latency Analysis ⭐⭐

**Interrupt Latency** = Time from IRQ assertion to first ISR instruction

$$T_{latency} = T_{finish\_instr} + T_{save\_context} + T_{vector\_fetch} + T_{pipeline\_flush}$$

**Worked Example:**
- Longest instruction = 5 cycles (multi-cycle DIV)
- Context save (PC + PSW) = 2 memory writes = 2 × 100 ns = 200 ns
- Vector fetch = 1 memory read = 100 ns
- Pipeline flush = 5-stage pipeline = 5 × 10 ns = 50 ns
- Clock = 10 ns/cycle

**Worst-case latency** = 5 × 10 + 200 + 100 + 50 = **400 ns**
**Best-case latency** = 1 × 10 + 200 + 100 + 50 = **360 ns**

> 🎯 **GATE Trick:** Interrupt latency questions often ask for worst-case. Always use the longest instruction execution time!

#### Nested Interrupts ⭐⭐

When a higher-priority interrupt occurs during ISR of lower-priority one:

```
Main Program → IRQ_low → ISR_low executing → IRQ_high arrives
                                              → Save ISR_low context
                                              → Execute ISR_high
                                              → Restore ISR_low
                                              → Continue ISR_low
                                              → Restore main program
```

**Stack grows:** Main context → Low ISR context → High ISR context (LIFO order)

**Key Points:**
- Only **higher priority** interrupts can nest (lower ones remain pending)
- Some architectures disable all interrupts during ISR (non-maskable exceptions still work)
- Stack overflow possible with many nested interrupts

#### Priority Interrupt Controller (PIC) Detail ⭐⭐

**8259A PIC (x86 systems):**
- Handles 8 IRQ lines (cascadable to 15 with master-slave)
- Programmable priority modes:
  - **Fixed Priority:** IRQ0 highest, IRQ7 lowest
  - **Rotating Priority:** After servicing IRQi, IRQi+1 becomes highest
  - **Specific EOI:** ISR specifies which interrupt to clear

| IRQ | Device (Typical PC) | Priority |
|---|---|---|
| IRQ 0 | System Timer | Highest |
| IRQ 1 | Keyboard | |
| IRQ 2 | Cascade (slave PIC) | |
| IRQ 3 | Serial Port COM2 | |
| IRQ 4 | Serial Port COM1 | |
| IRQ 5 | LPT2 / Sound Card | |
| IRQ 6 | Floppy Disk | |
| IRQ 7 | LPT1 | Lowest |

**Interrupt Mask Register (IMR):** Each bit masks one IRQ line
- Bit = 1 → interrupt disabled (masked)
- Bit = 0 → interrupt enabled
- IMR = 0xFB = 11111011₂ → only IRQ2 enabled

**Worked Example:** With IMR = 0xE3 = 11100011₂
- Masked (disabled): IRQ2, IRQ3, IRQ4 (bits 2,3,4 are 1)
- Enabled: IRQ0, IRQ1, IRQ5, IRQ6, IRQ7

> 🎯 **GATE Trick:** In PIC, bit position i in IMR corresponds to IRQi. Bit=1 means MASKED (disabled), not enabled!

#### Interrupt-Driven I/O: Complete Worked Example ⭐⭐

**Problem:** A keyboard generates interrupts at 10 characters/sec. Each ISR takes 100 μs. CPU runs at 500 MHz. What fraction of CPU time is spent handling keyboard interrupts?

**Solution:**
- Interrupts per second = 10
- ISR time per interrupt = 100 μs = 100 × 10⁻⁶ s
- Total ISR time per second = 10 × 100 × 10⁻⁶ = **1 ms/s = 0.1%**
- CPU cycles wasted = 0.001 × 500 × 10⁶ = **500,000 cycles/sec**

This is negligible → keyboard is fine with interrupt I/O!

**Contrast with disk:**
- Disk transfers at 50 MB/s, word = 4 bytes → 12.5M words/sec
- If each word causes an interrupt with 100 μs ISR:
- Total ISR time = 12.5M × 100 μs = **1250 sec/sec** → IMPOSSIBLE!
- This proves why **DMA is essential** for high-bandwidth devices.

### 8.10 DMA Advanced Topics ⭐⭐⭐

#### DMA Controller Registers

```
┌─────────────────────────────────┐
│      DMA Controller             │
│                                 │
│  [Starting Address Register]    │ ← Memory address for transfer
│  [Word Count Register]          │ ← Number of words to transfer
│  [Control Register]             │ ← Direction, mode, enable
│  [Status Register]              │ ← Done, error, channel active
│                                 │
│  Bus Request (BR) → CPU         │
│  Bus Grant (BG) ← CPU          │
│  Interrupt → CPU (on complete)  │
└─────────────────────────────────┘
```

#### DMA vs CPU: Bus Contention Analysis ⭐⭐

**Cycle Stealing Impact on CPU Performance:**

$$\text{CPU slowdown} = \frac{T_{DMA\_cycles}}{T_{total}} = \frac{N_{words} \times T_{mem}}{T_{program}}$$

**Worked Example 1:** CPU executing program that takes 10 ms. DMA transfers 5000 words with 50 ns memory cycles during this time.
- DMA bus usage = 5000 × 50 ns = 250 μs = 0.25 ms
- CPU slowdown = 0.25 / 10 = **2.5%**
- Effective program time = 10 + 0.25 = **10.25 ms**

**Worked Example 2:** Two DMA channels active simultaneously.
- Channel 1: 800 KB transfer, memory cycle = 40 ns, word = 4 bytes
- Channel 2: 400 KB transfer, memory cycle = 40 ns, word = 4 bytes
- Channel 1 words = 800K/4 = 200K, bus time = 200K × 40 ns = 8 ms
- Channel 2 words = 400K/4 = 100K, bus time = 100K × 40 ns = 4 ms
- Total bus steal = 8 + 4 = **12 ms** (channels take turns in cycle stealing)
- If CPU also needs bus for 20 ms of total execution time:
- CPU effective time = 20 + 12 = **32 ms** (60% slowdown)

> 🎯 **GATE Trick:** In cycle stealing, DMA steals bus for ONE memory cycle per word. In burst mode, it takes the bus for the ENTIRE transfer. Calculate accordingly!

#### DMA-Initiated vs CPU-Initiated Transfers

| Feature | DMA-Initiated | CPU-Initiated |
|---|---|---|
| Who starts | Device signals DMA | CPU programs DMA registers |
| Flow | Device → DMA → Memory | CPU → DMA setup → Device → Memory |
| Use case | Real-time data acquisition | Disk reads, network transfers |

#### Fly-By vs Fetch-and-Deposit DMA ⭐

| Mode | Data Path | Speed | Hardware |
|---|---|---|---|
| **Fly-By** | Device → Memory (direct) | Fast (1 bus cycle/word) | Simple, device must drive data bus |
| **Fetch-and-Deposit** | Device → DMA buffer → Memory | Slow (2 bus cycles/word) | Complex, DMA has internal buffer |

**Fly-by:** Data goes directly from device to memory on the bus — DMA just controls addressing. Used for simple, high-speed device-to-memory transfers.

**Fetch-and-deposit:** DMA first reads from device into its buffer, then writes from buffer to memory. Needed when device and memory have different bus widths or timing.

### 8.11 I/O Performance Comprehensive Analysis ⭐⭐

#### Quick Visual 🧩

```
Total I/O time = device latency + data transfer + software overhead
Usually device latency dominates.
```

#### I/O Bound vs CPU Bound Systems

| Characteristic | CPU Bound | I/O Bound |
|---|---|---|
| Bottleneck | Computation speed | I/O device speed |
| CPU utilization | High (>80%) | Low (<40%) |
| Solution | Faster CPU, better algorithm | DMA, caching, faster I/O |
| Example | Scientific computing | Database server |

#### Complete I/O System Performance Worked Example ⭐⭐

**Problem:** A system has:
- CPU: 2 GHz, CPI = 1.5
- Disk: 7200 RPM, avg seek = 8 ms, transfer rate = 150 MB/s, sector = 512 B
- DMA: burst mode, 40 ns/word (4 bytes/word)
- Interrupt latency: 500 ns

Calculate total time to read one 4 KB block from disk.

**Solution:**
1. **Disk access time:**
   - Rotational latency = 0.5 × (60/7200) = 0.5 × 8.33 ms = **4.17 ms**
   - Transfer time = 4096 / (150 × 10⁶) = **27.3 μs**
   - Total disk time = 8 + 4.17 + 0.0273 = **12.2 ms**

2. **DMA transfer time:**
   - Words = 4096/4 = 1024 words
   - Burst mode: 1024 × 40 ns = **40.96 μs**

3. **CPU overhead:**
   - DMA setup ≈ 10 instructions × 1.5 CPI × 0.5 ns = **7.5 ns**
   - Interrupt handling (completion) ≈ 500 ns + 50 instructions × 0.75 ns = **537.5 ns**

4. **Total time = 12.2 ms + 40.96 μs + 0.545 μs ≈ 12.24 ms**
   - Disk access dominates! (99.7% of total time)

> 🎯 **GATE Insight:** I/O performance is almost always dominated by mechanical device latency (seek + rotation). DMA transfer and CPU overhead are negligible in comparison.

#### Effective Disk Bandwidth with Overhead

$$\text{Effective BW} = \frac{\text{Data transferred}}{\text{Total time including overhead}}$$

**Example:** Sequential reads of 8 KB blocks:
- Seek = 0 (sequential), rotation = 4.17 ms (worst case), transfer = 8192/(150×10⁶) = 54.6 μs
- With overhead per block ≈ 0.1 ms
- Effective BW per block = 8192 / (4.17 + 0.0546 + 0.1) ms = 8192 / 4.32 ms ≈ **1.9 MB/s**
- Without rotational delay (back-to-back sectors): 8192 / 0.155 ms ≈ **52.9 MB/s**

This shows why **sequential access >> random access** for disks!

### 8.12 Handshaking Protocols for Asynchronous Transfer ⭐⭐

#### Source-Initiated Transfer (Output)

```
Source                     Destination
  │                           │
  ├──── Data Valid ──────────→│  (1) Source places data + asserts Data Valid
  │                           │
  │←──── Data Accepted ──────┤  (2) Destination reads data, asserts Accepted
  │                           │
  ├──── Remove Data Valid ───→│  (3) Source removes data and Data Valid
  │                           │
  │←── Remove Accepted ──────┤  (4) Destination deasserts Accepted
  │                           │
  ├──── Next transfer... ────→│
```

This is a **4-phase handshake** (full handshake). Each signal goes high then low.

#### Destination-Initiated Transfer (Input)

```
Destination                Source
  │                           │
  ├──── Ready for Data ──────→│  (1) Destination says "I'm ready"
  │                           │
  │←──── Data Valid ─────────┤  (2) Source places data, asserts Data Valid
  │                           │
  ├──── Data Accepted ───────→│  (3) Destination reads, asserts Accepted
  │                           │
  │←── Remove Data Valid ────┤  (4) Source removes data
  │                           │
```

**Timing for Asynchronous Transfer:**
- Each handshake step takes propagation delay δ
- 4-phase handshake: minimum cycle = 4δ
- Data rate limited by slowest participant

> 🎯 **GATE Trick:** Asynchronous buses use handshaking — no clock needed. Count the number of signal transitions (typically 4) to calculate the minimum transfer time.

### 8.13 Memory-Mapped I/O Complete Example ⭐⭐

#### Quick Visual 🧩

```
Address decoder sees 0x4001xxxx -> routes access to UART registers
```

**System:** ARM processor, memory-mapped I/O

| Address Range | Device |
|---|---|
| 0x00000000 - 0x3FFFFFFF | RAM (1 GB) |
| 0x40000000 - 0x4000FFFF | GPIO Registers |
| 0x40010000 - 0x4001FFFF | UART Controller |
| 0x40020000 - 0x4002FFFF | Timer Registers |
| 0x40030000 - 0x4003FFFF | DMA Controller |

**Accessing UART in C (memory-mapped):**
```c
#define UART_DATA    (*(volatile unsigned int *)0x40010000)
#define UART_STATUS  (*(volatile unsigned int *)0x40010004)
#define UART_CTRL    (*(volatile unsigned int *)0x40010008)

// Polling UART receive
while (!(UART_STATUS & 0x01));   // Wait for data ready bit
char c = (char)UART_DATA;         // Read received byte

// Transmit byte
while (!(UART_STATUS & 0x02));   // Wait for TX buffer empty
UART_DATA = 'A';                  // Write byte to transmit
```

**Why `volatile`?** Prevents compiler from optimizing away repeated reads of the same address — the hardware register value can change at any time!

**I/O-Mapped (x86) equivalent:**
```
IN AL, 0x3F8      ; Read from COM1 data port  
OUT 0x3F8, AL     ; Write to COM1 data port
```

**Comparison for exam:**
| Feature | Memory-Mapped | I/O-Mapped |
|---|---|---|
| Address space | Reduced RAM space | Separate I/O space |
| Instructions | All memory instructions work | Only IN/OUT |
| Protection | Memory protection works | Needs separate I/O protection |
| Hardware | No extra decoder needed | I/O address decoder needed |
| Programming | Natural (use pointers) | Requires special instructions |

---

## 9. RAID (Redundant Array of Inexpensive Disks) ⭐⭐

| RAID Level | Description | Min Disks | Pros | Cons |
|---|---|---|---|---|
| **RAID 0** | Striping (no redundancy) | 2 | Fastest, full capacity | No fault tolerance |
| **RAID 1** | Mirroring | 2 | Simple, read fast | 50% capacity used |
| **RAID 2** | Bit-level striping + Hamming | 3+ | Detects/corrects errors | Rarely used |
| **RAID 3** | Byte-level + dedicated parity | 3 | Good for large sequential | Parity disk bottleneck |
| **RAID 4** | Block-level + dedicated parity | 3 | Independent reads | Parity disk bottleneck |
| **RAID 5** | Block-level + distributed parity | 3 | Good read/write, no bottleneck | Write penalty (read-modify-write) |
| **RAID 6** | RAID 5 + extra parity | 4 | Tolerates 2 disk failures | More overhead |

**Usable capacity:**
- RAID 0: N disks × capacity each
- RAID 1: N/2 × capacity
- RAID 5: (N−1) × capacity
- RAID 6: (N−2) × capacity

### 9.1 RAID Worked Examples ⭐

#### Quick Visual 🧩

```
RAID 5 example:
Data blocks striped across disks + parity distributed
One disk fails -> rebuild using XOR parity
```

**Example 1:** 8 disks, each 2 TB, RAID 5. Calculate usable space and rebuild time.
- Usable = (8−1) × 2TB = **14 TB**
- If one disk fails: reconstruct from remaining 7 using XOR
- If each disk reads at 200 MB/s, rebuild time ≈ 2TB / 200 MB/s ≈ 10,000 sec ≈ **2.8 hours**

**Example 2:** Compare RAID 0, 1, 5, 6 for 6 × 1TB disks:
| Configuration | Usable Space | Fault Tolerance | Write Performance |
|---|---|---|---|
| RAID 0 | 6 TB | None (any fail = data loss) | Best (6× parallel) |
| RAID 1 | 3 TB | 1 disk per pair | Good |
| RAID 5 | 5 TB | 1 disk total | Medium (write penalty) |
| RAID 6 | 4 TB | 2 disks total | Worst (2 parity calcs) |

**RAID Write Penalty (RAID 5):**
- Small write: 1 data read + 1 parity read + 1 data write + 1 parity write = **4 operations**
- Full-stripe write: Just write all data + compute new parity = **N operations** (no reads needed)
- This is why RAID 5 is poor for random small writes but fine for sequential/full-stripe writes

### 9.2 Storage Performance Metrics ⭐

#### Quick Visual 🧩

```
Latency -> time per operation
IOPS    -> operations per second
Throughput -> bytes per second
```

| Metric | Definition | Units |
|---|---|---|
| IOPS | I/O Operations Per Second | ops/s |
| Throughput | Data transferred per unit time | MB/s, GB/s |
| Latency | Time per individual operation | ms, μs |
| Queue depth | Number of outstanding I/O requests | |

**HDD vs SSD Performance:**
| Metric | HDD (7200 RPM) | SSD (NVMe) |
|---|---|---|
| Random read IOPS | ~100 | ~500,000 |
| Random write IOPS | ~100 | ~100,000 |
| Sequential read | ~200 MB/s | ~3,500 MB/s |
| Sequential write | ~200 MB/s | ~3,000 MB/s |
| Latency | ~5-15 ms | ~0.01-0.1 ms |

---

### 9.3 Performance Benchmarks ⭐

#### Quick Visual 🧩

```
Real performance comparison should use execution time,
not raw MIPS numbers.
```

| Benchmark | Measures | Used For |
|---|---|---|
| **SPEC CPU** | Integer/FP performance | General CPU comparison |
| **SPEC INT** | Integer performance | Server, desktop |
| **SPEC FP** | Floating-point performance | Scientific computing |
| **Dhrystone** | Integer synthetic | Embedded systems (DMIPS) |
| **Whetstone** | FP synthetic | Scientific workloads |
| **Linpack** | Dense linear algebra | Supercomputer ranking (Top500) |
| **CoreMark** | Embedded benchmark | Microcontrollers |
| **TPC-C** | Database transactions | Server comparison |

**Why MIPS is Unreliable:**
| Problem | Example |
|---|---|
| Depends on ISA | CISC x86 does more per instruction than RISC |
| Ignores program | "NOP" program has infinite MIPS |
| Misleading comparisons | Higher MIPS ≠ faster execution |
| Simple ops inflated | ADD in 1 cycle vs MUL in 10 → same MIPS if only ADDs |

> 🎯 **GATE Position:** Use execution time for fair comparison, not MIPS or MFLOPS!

**Geometric Mean for Benchmark Suites:**
$$\text{GM} = \sqrt[n]{\prod_{i=1}^{n} \text{Ratio}_i}$$

Used to combine multiple benchmark results into a single score. Geometric mean is preferred because it handles ratios consistently — doubling speed on one benchmark and halving on another gives GM = 1 (neutral), unlike arithmetic mean which would show improvement.

### 9.4 Bus Standards Overview ⭐

#### Quick Visual 🧩

```
Old trend: wide parallel buses
New trend: fast serial lanes (PCIe x1/x4/x8/x16)
```

| Bus Standard | Width | Speed | Type | Used For |
|---|---|---|---|---|
| ISA | 16-bit | 8 MHz | Parallel | Legacy PC expansion |
| PCI | 32-bit | 33 MHz | Parallel | PC peripherals |
| PCI-X | 64-bit | 133 MHz | Parallel | Servers |
| PCIe 3.0 | Serial (16 lanes) | 1 GB/s/lane | Serial | Modern PC (GPU, NVMe) |
| PCIe 4.0 | Serial | 2 GB/s/lane | Serial | Current standard |
| PCIe 5.0 | Serial | 4 GB/s/lane | Serial | Latest standard |
| USB 2.0 | Serial | 480 Mbps | Serial | Peripherals |
| USB 3.0 | Serial | 5 Gbps | Serial | Fast peripherals |
| USB 4.0 | Serial | 40 Gbps | Serial | Universal |
| SATA III | Serial | 6 Gbps | Serial | HDDs, SSDs |
| NVMe (PCIe) | Serial | 32 Gbps+ | Serial | Fast SSDs |

**Trend:** Parallel buses → Serial buses (PCIe). Why?
- Serial: fewer pins, no clock skew, higher frequency per wire
- PCIe: uses lanes (×1, ×4, ×8, ×16) to scale bandwidth

---

### 9.5 Instruction and Data Flow Patterns ⭐

#### Quick Visual 🧩

```
Instruction mix + data access pattern -> CPI + cache behavior
```

Understanding how instructions flow through the CPU helps with pipeline and performance analysis.

**Instruction Mix Analysis — Typical Programs:**
| Workload | % ALU | % Loads | % Stores | % Branches | % FP |
|---|---|---|---|---|---|
| Integer (SPECint) | 40% | 25% | 10% | 20% | 5% |
| FP (SPECfp) | 30% | 20% | 10% | 10% | 30% |
| Embedded | 50% | 15% | 10% | 20% | 5% |

> 🎯 **GATE often gives instruction mix and asks for CPI or execution time!**

**Data Access Patterns:**
| Pattern | Locality Type | Example |
|---|---|---|
| Sequential array | Spatial | for(i=0; i<N; i++) a[i]... |
| Loop variable | Temporal | while(i < N) { ...i... } |
| Stride-k | Weak spatial | a[0], a[k], a[2k], ... |
| Random | None | Hash table lookup |
| Stack access | Both temporal + spatial | Function calls, local vars |

Understanding these patterns helps predict cache behavior and miss rates.

### 9.6 Memory System Design Trade-offs ⭐

#### Quick Visual 🧩

```
Increase block size:
  fewer compulsory misses
  but higher miss penalty and possible more conflict/capacity pressure
```

**Increasing block size:**
| Effect | Direction | Reason |
|---|---|---|
| Compulsory misses | ↓ Decrease | More spatial locality exploited |
| Capacity misses | ↑ Increase | Fewer blocks fit in cache |
| Conflict misses | ↑ Increase | Fewer sets |
| Miss penalty | ↑ Increase | More data to transfer |
| Bus traffic | ↑ Increase | Each miss transfers more data |

**Optimal block size** balances these effects — typically 32-128 bytes for modern CPUs.

**Increasing associativity:**
- Going from 1-way to 2-way: BIG reduction in conflict misses
- Going from 2-way to 4-way: Moderate reduction
- Going from 4-way to 8-way: Small reduction
- Beyond 8-way: Negligible benefit, significant hardware cost

> 🎯 **Rule of Thumb (Hill's Observation):** 2:1 cache rule — direct-mapped cache of size N has similar miss rate to 2-way cache of size N/2. So doubling associativity has same effect as doubling cache size!

---

## 10. Memory Technology Details ⭐⭐

### 10.1 SRAM vs DRAM ⭐⭐

#### Quick Visual 🧩

```
SRAM: flip-flop cell (fast, expensive) -> cache
DRAM: capacitor cell (dense, slower)  -> main memory
```

| Feature | SRAM | DRAM |
|---|---|---|
| Storage element | 6 transistors (flip-flop) | 1 transistor + 1 capacitor |
| Speed | Fast (1-3 ns) | Slower (50-70 ns) |
| Refresh needed? | No | Yes (every ~64 ms) |
| Density | Low (6T per bit) | High (1T1C per bit) |
| Cost per bit | High | Low |
| Power | Low (static) | Higher (refresh) |
| Used for | Cache (L1, L2, L3) | Main memory (RAM) |
| Volatile? | Yes | Yes |

**DRAM Organization:**
- Organized as rectangular array of cells
- Address sent in TWO halves: **RAS (Row Address Strobe)** then **CAS (Column Address Strobe)**
- This **multiplexing** halves the address pins needed

**DRAM Access Time:**
$$T_{DRAM} = T_{RAS} + T_{CAS} + T_{data}$$

**Burst Mode:** After first access, subsequent column addresses are auto-incremented
- First word: full latency. Following words: CAS latency only
- Example: CAS latency = 10 ns, RAS = 20 ns
  - First word: 20 + 10 = 30 ns
  - Each subsequent word: 10 ns

**Types of DRAM:**
| Type | Feature | Bandwidth |
|---|---|---|
| SDRAM | Synchronous (clocked) | ~1.6 GB/s |
| DDR SDRAM | Double data rate (both edges) | ~3.2 GB/s |
| DDR2 | Higher clock, lower voltage | ~6.4 GB/s |
| DDR3 | Higher clock, lower power | ~12.8 GB/s |
| DDR4 | Higher density, lower voltage | ~25.6 GB/s |
| DDR5 | Latest, higher bandwidth | ~51.2 GB/s |

**DDR = Double Data Rate:** Transfers data on BOTH rising AND falling clock edges → **2× bandwidth** vs SDR

### 10.2 ROM Types ⭐

#### Quick Visual 🧩

```
Mask ROM -> PROM -> EPROM/EEPROM -> Flash
More flexibility, but generally more write/erase circuitry.
```

| Type | Write | Erase | Use |
|---|---|---|---|
| Mask ROM | Factory only | Never | Mass production |
| PROM | Once (fuse) | Never | Prototyping |
| EPROM | Electrically | UV light | Development |
| EEPROM | Electrically | Electrically (byte) | Small data stores |
| Flash | Electrically | Electrically (block) | USB, SSD, phones |

### 10.3 Disk Performance ⭐⭐

#### Quick Visual 🧩

```
Disk access time = Seek + Rotational latency + Transfer
```

**Disk Access Time:**
$$T_{disk} = T_{seek} + T_{rotation} + T_{transfer}$$

Where:
- **Seek time:** Time to move head to correct track
  - Average seek = 1/3 × max_seek (approximation) or given value
- **Rotational latency:** Time for sector to rotate under head
  - Average = 1/2 × rotation_time = 30/RPM (in seconds)
- **Transfer time:** Time to read data
  - = bytes_to_transfer / transfer_rate
  - = bytes_to_transfer / (bytes_per_track × rotations_per_sec)

**Worked Example:**
Disk: 7200 RPM, average seek = 8 ms, 500 sectors/track, 512 bytes/sector. Read 4KB.
- Rotation time = 60/7200 = 8.33 ms
- Average rotational latency = 8.33/2 = 4.17 ms
- Bytes/track = 500 × 512 = 256 KB
- Transfer rate = 256 KB × 120 rot/sec = 30.72 MB/s
- Transfer time for 4KB = 4KB / 30.72 MB/s = 0.13 ms
- **Total: 8 + 4.17 + 0.13 = 12.30 ms**

### 10.4 SSD vs HDD ⭐

#### Quick Visual 🧩

```
HDD: mechanical movement dominates latency
SSD: no movement, much lower latency and higher IOPS
```

| Feature | HDD | SSD |
|---|---|---|
| Storage | Magnetic platter | Flash memory (NAND) |
| Moving parts | Yes (spinning disk, arm) | None |
| Random access time | 5-10 ms | 0.05-0.15 ms |
| Sequential speed | 100-200 MB/s | 500-7000 MB/s |
| Power | Higher | Lower |
| Durability | Fragile (moving parts) | Robust |
| Cost/GB | Lower | Higher |
| Write endurance | Unlimited | Limited (TBW) |

---

## 11. Power and Energy in Processors ⭐

### 11.1 Dynamic Power

#### Quick Visual 🧩

```
P_dynamic = alpha x C x V^2 x f
Voltage has quadratic impact on dynamic power.
```
$$P_{dynamic} = \alpha \times C \times V^2 \times f$$

Where:
- α = activity factor (fraction of gates switching per cycle)
- C = capacitance
- V = supply voltage
- f = clock frequency

**Key Insight:** Power ∝ V² × f. Reducing voltage by half → power reduced to **1/4**!

### 11.2 Static Power

#### Quick Visual 🧩

```
Leakage current flows even when gates are not switching
-> static power loss
```
$$P_{static} = I_{leak} \times V$$
- Due to leakage current (transistors not perfectly off)
- Increases with temperature and smaller transistors
- Becoming dominant in modern processors!

### 11.3 Energy per Operation

#### Quick Visual 🧩

```
Energy = Power x Time
Lower voltage/frequency tuning can reduce energy per task.
```
$$E = P \times T = \alpha CV^2 \times \text{(cycles per operation)}$$

> 🎯 **GATE Insight:** Performance per watt is now MORE important than raw performance. This drives multi-core designs (more cores at lower frequency/voltage = more work per watt).

### 11.4 Voltage-Frequency Scaling

#### Quick Visual 🧩

```
Lower f often enables lower V
-> big power savings with controlled performance loss
```
If we reduce frequency by half AND voltage by half:
- Performance: reduced by 2×
- Power = α × C × (V/2)² × (f/2) = **P/8** (power reduced 8×!)
- Energy per operation: reduced by 4× (half the power × same runtime)

---

## 12. Advanced Topics for GATE ⭐⭐

### 12.1 Flynn's Taxonomy ⭐⭐

#### Quick Visual 🧩

```
SISD: 1 instr, 1 data
SIMD: 1 instr, many data
MIMD: many instr, many data
```

| Category | Instruction Streams | Data Streams | Example |
|---|---|---|---|
| **SISD** | Single | Single | Traditional uniprocessor |
| **SIMD** | Single | Multiple | Vector processors, GPU cores |
| **MISD** | Multiple | Single | Rare (systolic arrays, some fault-tolerant) |
| **MIMD** | Multiple | Multiple | Multi-core, clusters |

**SIMD in Practice:**
- Intel SSE/AVX: process 4/8/16 floats simultaneously
- GPU: thousands of SIMD cores
- Great for: matrix operations, image processing, ML

**MIMD Subtypes:**
- **Shared Memory:** All processors share same memory space (UMA, NUMA)
  - Communication via shared variables
  - Need synchronization (locks, barriers)
  - Example: Multi-core Intel processor
- **Distributed Memory:** Each processor has private memory
  - Communication via message passing (MPI)
  - No cache coherence issue
  - Example: Computer clusters

### 12.2 UMA vs NUMA ⭐

#### Quick Visual 🧩

```
UMA: all CPUs see same memory latency
NUMA: local memory fast, remote memory slower
```

| Feature | UMA | NUMA |
|---|---|---|
| Full name | Uniform Memory Access | Non-Uniform Memory Access |
| Memory access time | Same for all processors | Depends on memory location |
| Local memory? | No (all memory equidistant) | Yes (faster for local) |
| Scalability | Limited (~8-16 processors) | Better (64+ processors) |
| Complexity | Simple | Complex (placement matters) |

### 12.3 Interconnection Networks ⭐

#### Quick Visual 🧩

```
Topology choice decides latency, bandwidth, and scalability.
```

| Network | Type | Bandwidth | Latency | Used in |
|---|---|---|---|---|
| Bus | Shared | Low | Low | Small SMP |
| Crossbar | Full connection | High | Low | Expensive; small systems |
| Ring | Point-to-point | Medium | Medium | IBM Token Ring |
| Mesh/Torus | Grid | High | Medium | GPU arrays, supercomputers |
| Hypercube | log₂N dimensions | High | O(log N) | Connection machines |
| Fat Tree | Hierarchical | High | O(log N) | InfiniBand, data centers |

**Crossbar Complexity:** N×N crossbar for N processors → N² switches
**Hypercube:** N = 2^k nodes, each connected to k neighbors, diameter = k

### 12.4 Parallel Performance Metrics ⭐

#### Quick Visual 🧩

```
Speedup S(N)=T1/TN
Efficiency E(N)=S(N)/N
```

**Speedup:** S(N) = T(1) / T(N)
**Efficiency:** E(N) = S(N) / N
**Ideal:** S(N) = N (linear speedup), E(N) = 1 (100%)

**Super-linear speedup:** When S(N) > N — possible due to:
- Aggregate cache size: N processors → N× cache → fewer misses
- Search problems: parallel search finds answer in different branches

### 12.5 Memory-Mapped I/O vs Port-Mapped I/O ⭐

#### Quick Visual 🧩

```
Memory-mapped: uses regular memory instructions
Port-mapped: uses dedicated I/O instructions
```

| Feature | Memory-Mapped | Port-Mapped (Isolated) |
|---|---|---|
| Address space | Shared with memory | Separate I/O space |
| Instructions | Same (LOAD/STORE) | Special (IN/OUT) |
| Protection | Via virtual memory | Via I/O privilege level |
| Address decoding | More complex | Simpler |
| Flexibility | Can use all addressing modes | Limited instructions |
| Example | ARM, MIPS | x86 |

### 12.6 Interrupt Processing Detail ⭐⭐

#### Quick Visual 🧩

```
IRQ -> finish current instruction -> save context -> ISR -> RETI -> resume
```

**Interrupt Handling Steps:**
1. Device asserts interrupt request line
2. CPU checks for interrupts between instructions
3. If interrupt enabled and priority sufficient:
   a. Finish current instruction
   b. Save PC and PSW (processor status word) on stack
   c. Disable interrupts (or mask current level)
   d. Load PC with ISR address (from interrupt vector table)
4. ISR executes:
   a. Save additional registers
   b. Service the device
   c. Restore registers
5. Execute RETI (return from interrupt):
   a. Restore PC and PSW from stack
   b. Re-enable interrupts
   c. Resume normal execution

**Interrupt Latency Components:**
| Component | Time |
|---|---|
| Finish current instruction | 1-10 cycles |
| Save context (min: PC + PSW) | 2-5 cycles |
| Vector lookup | 1-2 cycles |
| Branch to ISR | 1-2 cycles |
| **Total minimum latency** | **5-19 cycles** |

**Multiple Interrupt Handling:**
| Strategy | Description |
|---|---|
| Sequential | Disable all interrupts during ISR; service in order |
| Nested | Higher priority can interrupt current ISR |
| Masked | Priority-based masking; only higher priority gets through |

### 12.7 Asynchronous Data Transfer (Handshaking) ⭐

#### Quick Visual 🧩

```
REQ/ACK handshake lets fast and slow devices communicate safely without a shared clock.
```

**Source-initiated:**
```
Source places data on bus → asserts Data Valid → 
Destination reads data → asserts Data Accepted →
Source removes data → deasserts Data Valid →
Destination deasserts Data Accepted → Cycle complete
```

**Destination-initiated:**
```
Destination asserts Ready for Data →
Source places data → asserts Data Valid →
Destination reads data → deasserts Ready → asserts Data Accepted →
Source removes data → deasserts Data Valid
```

> 🎯 **Advantage over synchronous:** No shared clock needed → devices of different speeds can communicate!

---

## 13. Number System Deep Dive ⭐⭐

### 13.1 Complement Arithmetic ⭐⭐

#### Quick Visual 🧩

```
Subtraction as addition:
A - B = A + (complement of B)
```

**r's Complement:**
For n-digit number N in base r: r's complement = rⁿ − N

**Example:** 10's complement of 3456 (4 digits):
10⁴ − 3456 = 10000 − 3456 = **6544**

**(r−1)'s Complement:**
For n-digit number N: (r−1)'s complement = rⁿ − 1 − N

**Example:** 9's complement of 3456:
10⁴ − 1 − 3456 = 9999 − 3456 = **6543** (just subtract each digit from 9!)

**Example:** 1's complement of 1010:
1111 − 1010 = **0101** (just flip each bit!)

**Subtraction Using Complements:**
To compute A − B:
1. Find complement of B
2. Add: A + complement(B)
3. If carry out: Result is positive; discard carry (for r's) or add carry back (for (r-1)'s)
4. If no carry: Result is negative; complement the result

**Worked Example:** 72 − 45 using 10's complement:
- 10's complement of 45 = 100 − 45 = 55
- 72 + 55 = 127 → carry out = 1
- Discard carry: **27** ✅

**Worked Example:** 45 − 72 using 10's complement:
- 10's complement of 72 = 100 − 72 = 28
- 45 + 28 = 73 → no carry
- Result is negative: −(10's complement of 73) = −(100−73) = **−27** ✅

### 13.2 Binary Arithmetic Operations ⭐

#### Quick Visual 🧩

```
Full adder per bit + carry chain -> multi-bit addition
```

**Addition Rules:**
| A | B | Cin | Sum | Cout |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

**BCD Addition:**
1. Add two BCD digits normally
2. If sum > 9 OR carry out: add 6 (0110) to get valid BCD

**Example:** 27 + 35 in BCD:
```
  0010 0111
+ 0011 0101
-----------
  0101 1100  → 1100 > 9, add 0110
+ 0000 0110
-----------
  0110 0010  = 62 in BCD ✅
```

### 13.3 Base Conversion Shortcuts ⭐

#### Quick Visual 🧩

```
Binary<->Octal: group in 3 bits
Binary<->Hex:   group in 4 bits
```

| From | To | Method |
|---|---|---|
| Binary → Octal | Group by 3 bits from right | 110 101₂ = 65₈ |
| Binary → Hex | Group by 4 bits from right | 1010 1100₂ = AC₁₆ |
| Octal → Binary | Each octal digit → 3 bits | 25₈ = 010 101₂ |
| Hex → Binary | Each hex digit → 4 bits | 3F₁₆ = 0011 1111₂ |
| Any → Decimal | Multiply each digit by base^position | |
| Decimal → Any | Repeated division (integer) + repeated multiply (fraction) | |

**Powers of 2 to Remember:**
| Power | Value | Approx |
|---|---|---|
| 2¹⁰ | 1,024 | ~1K |
| 2²⁰ | 1,048,576 | ~1M |
| 2³⁰ | 1,073,741,824 | ~1G |
| 2³² | 4,294,967,296 | ~4G |
| 2⁴⁰ | 1,099,511,627,776 | ~1T |

---

## 14. MIPS Instruction Set Reference ⭐

### 14.1 MIPS Instruction Formats

#### Quick Visual 🧩

```
R-type: opcode rs rt rd shamt funct
I-type: opcode rs rt immediate
J-type: opcode target
```

**R-Type (Register):**
```
| opcode (6) | rs (5) | rt (5) | rd (5) | shamt (5) | funct (6) |
```
Used for: ADD, SUB, AND, OR, SLT, JR

**I-Type (Immediate):**
```
| opcode (6) | rs (5) | rt (5) | immediate (16) |
```
Used for: ADDI, LW, SW, BEQ, BNE

**J-Type (Jump):**
```
| opcode (6) | address (26) |
```
Used for: J, JAL

### 14.2 Key MIPS Instructions

#### Quick Visual 🧩

```
ALU ops: register-register
Load/Store: base + offset
Branch/Jump: control flow
```

| Instruction | Format | RTL | Operation |
|---|---|---|---|
| ADD rd,rs,rt | R | rd ← rs + rt | Signed add |
| SUB rd,rs,rt | R | rd ← rs − rt | Signed subtract |
| AND rd,rs,rt | R | rd ← rs & rt | Bitwise AND |
| OR rd,rs,rt | R | rd ← rs \| rt | Bitwise OR |
| SLT rd,rs,rt | R | rd ← (rs < rt) ? 1 : 0 | Set less than |
| ADDI rt,rs,imm | I | rt ← rs + sign_ext(imm) | Add immediate |
| LW rt,offset(rs) | I | rt ← M[rs + sign_ext(offset)] | Load word |
| SW rt,offset(rs) | I | M[rs + sign_ext(offset)] ← rt | Store word |
| BEQ rs,rt,offset | I | if (rs==rt) PC ← PC+4+offset×4 | Branch if equal |
| BNE rs,rt,offset | I | if (rs≠rt) PC ← PC+4+offset×4 | Branch if not equal |
| J target | J | PC ← PC[31:28] ∥ target×4 | Jump |
| JAL target | J | R31 ← PC+4; PC ← target×4 | Jump and link |
| JR rs | R | PC ← rs | Jump register |

**Branch Target Calculation:**
- BEQ/BNE: Target = PC + 4 + sign_extend(offset) × 4
- J/JAL: Target = {PC+4[31:28], address[25:0], 00}
  - 28-bit address = top 4 bits of PC + 26-bit address + 00
  - Can jump within same 256 MB segment

### 14.3 MIPS Addressing Modes

#### Quick Visual 🧩

```
Most data access in MIPS uses base+offset addressing.
```

| Mode | Example | Use |
|---|---|---|
| Register | ADD R1,R2,R3 | Most operations |
| Immediate | ADDI R1,R2,#5 | Constants |
| Base+Displacement | LW R1,100(R2) | Array/struct access |
| PC-relative | BEQ R1,R2,label | Branches |
| Pseudo-direct | J label | Jumps |

### 14.4 MIPS Pipeline Hazard Examples ⭐⭐

#### Quick Visual 🧩

```
RAW hazards resolved by forwarding when possible;
load-use often still needs one bubble.
```

**Example 1: RAW without forwarding**
```
ADD R1, R2, R3
SUB R4, R1, R5     ← RAW: R1 not yet in register file (needs 2 stalls)
```

**Example 2: Load-use with forwarding**
```
LW  R1, 0(R2)
ADD R3, R1, R4     ← 1 stall needed (MEM-EX forward, but LW result at end of MEM)
```

**Example 3: No hazard (distance ≥ 2)**
```
LW  R1, 0(R2)
NOP                 ← fills the gap
ADD R3, R1, R4     ← No stall! R1 forwarded from MEM/WB
```

**Example 4: Multiple dependencies**
```
LW  R1, 0(R10)
LW  R2, 4(R10)
ADD R3, R1, R2     ← 1 stall (load-use on R2; R1 is fine — enough distance)
SW  R3, 8(R10)     ← No stall (forwarding from EX/MEM)
```

### 14.5 Complete Pipeline Diagram with Forwarding ⭐⭐

#### Quick Visual 🧩

```
Forwarding paths reduce stalls by sending results directly
from later pipeline stages to earlier execute stage inputs.
```

For the code:
```
LW  R1, 0(R10)        ; Load R1
ADD R2, R1, R3        ; Uses R1 (load-use: 1 stall)
SUB R4, R2, R5        ; Uses R2 (forwarding from ADD's EX)
AND R6, R2, R4        ; Uses R2 (forwarding) and R4 (forwarding)
SW  R6, 0(R10)        ; Uses R6 (forwarding)
```

```
Cycle:  1    2    3    4    5    6    7    8    9   10
LW:    IF   ID   EX   ME   WB
               forward↓
ADD:        IF   ID   --   EX   ME   WB            (1 stall for load-use)
                     forward↓
SUB:             IF   --   ID   EX   ME   WB
AND:                  --   IF   ID   EX   ME   WB
SW:                             IF   ID   EX   ME  WB
```

**Total execution time = 10 cycles** (1 stall for load-use)
Without forwarding: would need 2 stalls per dependency → many more stalls!

---
