# 🔌 Digital Logic — Comprehensive Notes for GATE 2027 CS/IT/DA 🎯

> 🌟 **"Digital Logic is literally the foundation of EVERY computer, phone, and smart device on the planet!"**

> 💡 **Beginner Analogy:** Think of Digital Logic as the "alphabet" of computers 💻. Just like English uses 26 letters to build all words and books, computers use just **2 symbols (0 and 1)** to do EVERYTHING — from playing YouTube videos to running ChatGPT! ✨

---

## 📑 Table of Contents

| Part | Topics |
|---|---|
| **PART 1: Boolean Algebra** 🔢 | Laws, Duality, Self-Dual, XOR/XNOR, Minterms/Maxterms, Canonical Forms, Shannon Expansion, Threshold Functions, CMOS Logic |
| **PART 2: Minimization** 📉 | K-Maps (2–6 var), Don't Cares, PI/EPI, Quine-McCluskey, Petrick's Method, POS Minimization, Multi-Output Minimization |
| **PART 3: Number Systems & Codes** 🔢 | Binary/Octal/Hex, Signed Numbers, IEEE 754, Fixed Point, Gray/BCD/Excess-3/ASCII, Hamming Code, CRC, SECDED, Interleaving |
| **PART 4: Combinational Circuits** ⚡ | MUX, DEMUX, Decoder, Encoder, Adders (RCA/CLA/CSA), Booth's Multiplier, BCD Adder, Comparator, ALU, Code Converters, Hazards |
| **PART 5: PLDs & Completeness** 🏗️ | Post's Theorem, ROM/PROM/EPROM/EEPROM/Flash, PLA, PAL, CPLD, FPGA, LUT Mapping |
| **PART 6: Sequential Circuits** 🔄 | Latches (SR/D), FFs (D/T/JK/SR), Excitation Tables, FF Conversions, Timing (Setup/Hold/Skew/Metastability), Mealy/Moore, State Encoding, State Minimization, Sequence Detectors, Registers/LFSR, Counters (Sync/Async/Ring/Johnson), ASM Charts |
| **Practice Problems** 📝 | 50 GATE-level solved problems covering all topics |
| **Appendix** 🏆 | 20 Worked Examples, Formula Card (25+), GATE Tricks (20+), Super Tables, IC Reference, Mathematical Proofs |

---

## 🏭 Real-World Applications

| 🌍 Industry | 🔧 Application | 📌 DL Concept |
|---|---|---|
| **Every CPU ever** 🩸 | Intel, AMD, Apple M-series | Boolean algebra, Gates |
| **Smartphones** 📱 | Qualcomm Snapdragon, MediaTek | Combinational + Sequential |
| **RAM & Storage** 💾 | DDR5, SSD controllers | Flip-flops, Registers |
| **Traffic Lights** 🚦 | Automated control | FSM |
| **Error-Free Comms** 📡 | 4G/5G, WiFi, Satellite | Hamming, CRC, Parity |
| **Bitcoin Mining** ⛏️ | SHA-256 hash | Adders, Boolean functions |
| **FPGA/ASIC** ⚙️ | Xilinx, Intel PSG | ALL concepts |

> **GATE Weightage:** 3–5 marks (1–2 questions) | **Priority:** High 🔥 | **Time:** 3–4 weeks

---

## ✅ Micro-Checklist 🎯

- All Boolean laws, duality, complement, self-dual count ($2^{2^{n-1}}$)
- XOR/XNOR properties, parity counting, XOR does NOT distribute over OR
- K-map: 2/3/4/5-var, grouping, don't cares, EPI identification
- Quine-McCluskey tabular method, Petrick's method for multiple solutions
- 2's complement: range, overflow detection (Cᵢₙ⊕Cₒᵤₜ), sign extension
- 1's complement: end-around carry, two zeros
- IEEE 754: single/double precision, bias, denormalized numbers, special values (±∞, NaN)
- Hamming code: encoding, syndrome, SECDED (detect 2, correct 1)
- CRC: polynomial division, generator polynomial, burst error detection
- MUX: function implementation (2ⁿ:1 and 2ⁿ⁻¹:1 methods), building larger MUX
- DEMUX: decoder with enable = DEMUX
- Adders: Half, Full, Ripple (2n delay), CLA (4-delay), Carry Select, Carry Skip
- BCD adder: add 0110 when sum>9 or carry
- Hazards: static-1, static-0, dynamic, detection via K-map, fix with consensus terms
- Post's theorem: 5 classes (T₀, T₁, S, M, L), must break all for completeness
- ROM vs PLA vs PAL vs FPGA: comparison, size calculations
- CMOS: transistor counts, NAND/NOR preferred (fewer transistors)
- All FFs: truth tables + characteristic equations + excitation tables
- FF conversions: D↔T, D↔JK, JK↔T, SR↔D
- Timing: setup, hold, propagation delay, max frequency, clock skew, metastability
- Mealy vs Moore: comparison, conversion, state counts
- Sequence detector design: overlapping vs non-overlapping
- State minimization: implication table method
- State encoding: binary, Gray, one-hot
- Counters: ripple (max freq=1/ntₚ) vs synchronous, Mod-N design
- Ring counter: N states/N FFs, one-hot, not self-starting
- Johnson counter: 2N states/N FFs, complement feedback, 2-input decode
- LFSR: max length 2ⁿ−1, primitive polynomials, CRC connection
- ASM charts: state box, decision box, conditional output

---

# 🔢 PART 1: BOOLEAN ALGEBRA

## 1. Boolean Algebra Fundamentals 🧱

> 💡 **The Light Switch Language:** Boolean algebra only knows ON (1) and OFF (0). Series switches = AND, parallel switches = OR, an inverter = NOT. George Boole invented this in 1854! 🚀

### 1.1 Three Fundamental Operations

| Operation | Symbol | Also Written | Truth Condition |
|---|---|---|---|
| AND | · | A∧B, AB | 1 only when both 1 |
| OR | + | A∨B | 1 when at least one 1 |
| NOT | ' or ‾ | ¬A, Ā | Inverts value |

### 1.2 Boolean Algebra Laws 📜

| # | Law | AND Form | OR Form |
|---|---|---|---|
| 1 | Identity | A·1 = A | A+0 = A |
| 2 | Null | A·0 = 0 | A+1 = 1 |
| 3 | Idempotent | A·A = A | A+A = A |
| 4 | Complement | A·A' = 0 | A+A' = 1 |
| 5 | Commutative | A·B = B·A | A+B = B+A |
| 6 | Associative | (AB)C = A(BC) | (A+B)+C = A+(B+C) |
| 7 | Distributive | A(B+C) = AB+AC | **A+BC = (A+B)(A+C)** ⭐ |
| 8 | Absorption | A(A+B) = A | A+AB = A |
| 9 | De Morgan's | (AB)' = A'+B' | (A+B)' = A'·B' |
| 10 | Consensus | AB+A'C+BC = AB+A'C | (A+B)(A'+C)(B+C) = (A+B)(A'+C) |
| 11 | Involution | (A')' = A | — |

> ⚠️ **GATE Trap:** OR distributive `A+BC = (A+B)(A+C)` is unique to Boolean algebra!

**Derived Identities:**
- A + A'B = A + B ⭐ (very useful for simplification!)
- A(A'+B) = AB
- AB + AB' = A

#### 1.2.1 Proving Boolean Identities — Step by Step 📝

> 💡 **Three Methods of Proof:** (1) Algebraic manipulation using laws, (2) Perfect Induction (truth table), (3) Duality principle.

**Example 1: Prove Consensus Theorem** — AB + A'C + BC = AB + A'C

| Step | Expression | Law Used |
|---|---|---|
| 1 | AB + A'C + BC | Given |
| 2 | AB + A'C + BC(A+A') | Complement: A+A'=1 |
| 3 | AB + A'C + ABC + A'BC | Distributive |
| 4 | AB(1+C) + A'C(1+B) | Rearrange + Factor |
| 5 | AB·1 + A'C·1 | Null: 1+X=1 |
| 6 | **AB + A'C** ✅ | Identity |

**Example 2: Prove A + A'B = A + B**

| Step | Expression | Law Used |
|---|---|---|
| 1 | A + A'B | Given |
| 2 | (A+A')(A+B) | OR distributive! |
| 3 | 1·(A+B) | Complement |
| 4 | **A + B** ✅ | Identity |

**Example 3: Simplify F = A'BC + AB'C + ABC' + ABC**

| Step | Expression | Law Used |
|---|---|---|
| 1 | A'BC + AB'C + ABC' + ABC | Given |
| 2 | A'BC + AB'C + ABC' + ABC + ABC + ABC | Idempotent (add ABC twice) |
| 3 | (A'BC + ABC) + (AB'C + ABC) + (ABC' + ABC) | Regroup |
| 4 | BC(A'+A) + AC(B'+B) + AB(C'+C) | Factor |
| 5 | BC + AC + AB | Complement |
| 6 | **AB + BC + AC** ✅ | Commutative |

> 🎯 **GATE Trick:** When simplifying, try adding redundant terms (X+X=X) to create groups that factor nicely!

#### 1.2.2 De Morgan's Theorem — Extended & Generalized ⭐⭐

**Two-variable:** (AB)' = A'+B', (A+B)' = A'·B'

**n-variable generalization:**
- $(X_1 \cdot X_2 \cdot ... \cdot X_n)' = X_1' + X_2' + ... + X_n'$
- $(X_1 + X_2 + ... + X_n)' = X_1' \cdot X_2' \cdot ... \cdot X_n'$

**Applied De Morgan's (for complex expressions):**
To find complement of any expression:
1. Swap every AND ↔ OR
2. Complement every variable
3. Swap 0 ↔ 1

**Example:** Find complement of F = AB' + CD
- F' = (A'+B)(C'+D') ✅

**Example:** Find complement of F = (A+B)(C'+D)E
- F' = A'B' + (CD') + E' → then combine: F' = A'B' + CD' + E' ✅

> ⚠️ **GATE Trap:** When applying De Morgan's to complex nested expressions, work from outermost operation inward. Don't skip levels!

**Nested Example:** F = ((AB)' + C)'
- Step 1 outer complement: (AB)' · C' = (swap + to ·, complement terms)
- Wait — actually: ((AB)' + C)' = ((AB)')' · C' = AB · C' = ABC' ✅

#### 1.2.3 Boolean Function Representation 📊

Every Boolean function of n variables can be represented as:

| Form | Description | Example (F=AB+C) |
|---|---|---|
| **Truth table** | List all 2ⁿ input combinations | 8-row table |
| **Sum of Minterms** | OR of product terms where F=1 | Σm(3,4,5,6,7) |
| **Product of Maxterms** | AND of sum terms where F=0 | ΠM(0,1,2) |
| **SOP (Sum of Products)** | OR of AND terms | AB + C |
| **POS (Product of Sums)** | AND of OR terms | (A+C)(B+C) |
| **Algebraic** | Any Boolean expression | AB + C |
| **Logic diagram** | Gate connections | Circuit schematic |
| **K-map** | 2D truth table grid | Visual grouping |

**Number of Boolean functions of n variables = $2^{2^n}$**

| n | Input combinations | Total functions | Example |
|---|---|---|---|
| 1 | 2 | 4 | {0, X, X', 1} |
| 2 | 4 | 16 | AND, OR, XOR, NAND, NOR, ... |
| 3 | 8 | 256 | Most are unnamed |
| 4 | 16 | 65,536 | — |

#### 1.2.4 Switching Functions & Logic Gates 🔌

**Basic Gates:**

| Gate | Symbol | Boolean | Truth Table (2-input) | Transistor Count (CMOS) |
|---|---|---|---|---|
| **AND** | — | Y=A·B | 0,0,0,1 | 6 |
| **OR** | — | Y=A+B | 0,1,1,1 | 6 |
| **NOT** | △ | Y=A' | 1,0 | 2 |
| **NAND** | — | Y=(AB)' | 1,1,1,0 | 4 |
| **NOR** | — | Y=(A+B)' | 1,0,0,0 | 4 |
| **XOR** | — | Y=A⊕B | 0,1,1,0 | 8-12 |
| **XNOR** | — | Y=A⊙B | 1,0,0,1 | 8-12 |
| **Buffer** | △ | Y=A | 1,0→0,1 | 4 |

> 🏭 **Why NAND/NOR have fewer transistors than AND/OR in CMOS:** NAND/NOR are naturally implemented in CMOS (just PMOS+NMOS network). AND = NAND + NOT, OR = NOR + NOT — extra inverter stage!

**Gate Propagation Delays:**
- Each gate introduces a delay (typically 1-10 ns in modern CMOS)
- **Critical path** = path with maximum accumulated delay
- **Fan-in:** Number of inputs to a gate. High fan-in → more delay.
- **Fan-out:** Number of gates driven by output. High fan-out → more delay + potential signal degradation.

**Universal Gate Implementation:**

Using NAND gates only:
```
NOT A = A NAND A = (AA)' = A'
A AND B = (A NAND B) NAND (A NAND B) = ((AB)')' = AB
A OR B = (A NAND A) NAND (B NAND B) = A'·B' (wait no)
         Actually: (A NAND A) NAND (B NAND B) = (A')·(B'))' = A'+B'... 
         Hmm, let me redo: NOT(NOT A · NOT B) = A+B
         = (A' NAND B') = ((A NAND A) NAND (B NAND B))
A OR B = 3 NAND gates
```

Using NOR gates only:
```
NOT A = A NOR A = (A+A)' = A'
A OR B = (A NOR B) NOR (A NOR B) = ((A+B)')' = A+B
A AND B = (A NOR A) NOR (B NOR B) = (A'+B')' = AB
A AND B = 3 NOR gates
```

| Function | NAND gates needed | NOR gates needed |
|---|---|---|
| NOT | 1 | 1 |
| AND | 2 | 3 |
| OR | 3 | 2 |
| XOR | 4 | 5 |
| XNOR | 5 | 4 |

> 🎯 **GATE Trick:** NAND naturally generates SOP (AND-OR). NOR naturally generates POS (OR-AND).

#### 1.2.5 CMOS Logic Implementation ⭐ (Cross-topic with VLSI)

**CMOS = Complementary MOS** — uses both PMOS and NMOS transistors.

**Principle:**
- **PMOS** (pull-up): ON when input=0, connects output to VDD (logic 1)
- **NMOS** (pull-down): ON when input=1, connects output to GND (logic 0)
- **CMOS inverter:** 1 PMOS + 1 NMOS = 2 transistors

**CMOS Gate Design Rules:**
1. NMOS network: implements the COMPLEMENT of the desired function
   - Variables in SERIES = AND, Variables in PARALLEL = OR
2. PMOS network: DUAL of NMOS (series↔parallel swap)
   - Together they implement a NAND, NOR, or complex gate

**Transistor count for basic gates:**

| Gate | PMOS | NMOS | Total Transistors |
|---|---|---|---|
| NOT | 1 | 1 | **2** |
| NAND2 | 2 (parallel) | 2 (series) | **4** |
| NOR2 | 2 (series) | 2 (parallel) | **4** |
| AND2 | 3 | 3 | **6** (NAND + NOT) |
| OR2 | 3 | 3 | **6** (NOR + NOT) |
| XOR2 | ~6 | ~6 | **~12** (complex) |
| 3-input NAND | 3 parallel | 3 series | **6** |
| 3-input NOR | 3 series | 3 parallel | **6** |

**Why NAND/NOR are preferred in CMOS:**
- Fewer transistors (4 vs 6 for AND/OR)
- One level of inversion is "free"
- Both have complementary networks

> 🎯 **GATE Trick:** For CMOS: n-input NAND or NOR needs **2n transistors**. Any gate from the AND/OR family needs 2n+2 (extra inverter).

**Complex CMOS Gate Example:**
F = (AB + C)' → NMOS: A and B in series, parallel with C → connected to GND.
PMOS: A and B in parallel, series with C' → connected to VDD.
Total: 3 NMOS + 3 PMOS = **6 transistors**.

**Pass-Transistor Logic (PTL):**
Uses transistors as switches to pass signals. Can implement XOR with fewer transistors but has voltage degradation issues.

### 1.3 Complement and Dual

**Complement (F'):** Replace variables with complements, AND↔OR, 0↔1
**Dual (Fᵈ):** Swap AND↔OR and 0↔1, **DON'T** complement variables
- F' = Fᵈ with all variables complemented
- Dual of dual = original
- **Principle of Duality:** If F=G is valid, then Fᵈ = Gᵈ is also valid

### 1.4 Self-Dual Functions ⭐

F is **self-dual** if F(x₁,...,xₙ) = Fᵈ(x₁,...,xₙ), equivalently F(x₁',...,xₙ') = F'(x₁,...,xₙ).

**Properties:**
- Self-dual has exactly **2ⁿ⁻¹ minterms** (half the truth table is 1)
- Complementary minterms must have opposite values

> 📝 **Count of self-dual functions of n variables = $2^{2^{n-1}}$**

| n | Total functions | Self-dual |
|---|---|---|
| 1 | 4 | 2 |
| 2 | 16 | **4** |
| 3 | 256 | **16** |
| n | $2^{2^n}$ | $2^{2^{n-1}}$ |

**Test:** Pair complementary minterms (m₀↔m₇, m₁↔m₆, m₂↔m₅, m₃↔m₄ for 3 vars). In each pair, exactly one should be 1.

#### 1.4.1 How to Check Self-Duality — Worked Example ⭐⭐

**Problem:** Is F(A,B,C) = Σm(0, 3, 5, 6) self-dual?

**Step 1:** List complementary minterm pairs for 3 variables:
- m₀(000) ↔ m₇(111)
- m₁(001) ↔ m₆(110)
- m₂(010) ↔ m₅(101)
- m₃(011) ↔ m₄(100)

**Step 2:** Check if exactly one of each pair is in F:
- m₀ ∈ F? Yes. m₇ ∈ F? No. ✅ (exactly one)
- m₃ ∈ F? Yes. m₄ ∈ F? No. ✅
- m₅ ∈ F? Yes. m₂ ∈ F? No. ✅
- m₆ ∈ F? Yes. m₁ ∈ F? No. ✅

**Result:** F IS self-dual ✅ (has exactly 4 = 2³⁻¹ minterms, and all complementary pairs have exactly one 1)

**Problem:** Is G(A,B,C) = Σm(0, 1, 2, 6) self-dual?

- m₀ ∈ G? Yes. m₇ ∈ G? No. ✅
- m₁ ∈ G? Yes. m₆ ∈ G? Yes. ❌ **BOTH in G!**

**Result:** G is NOT self-dual ❌

> 🎯 **Quick Test:** Count minterms. If count ≠ 2ⁿ⁻¹, immediately NOT self-dual!

#### 1.4.2 Generating Self-Dual Functions

For n=2: Pick exactly one from each pair {m₀,m₃} and {m₁,m₂}.
- 2 choices × 2 choices = 4 self-dual functions ✅
- They are: Σm(0,1), Σm(0,2), Σm(1,3), Σm(2,3)
- In Boolean: A'(=Σm(0,1)), B'(=Σm(0,2)), B(=Σm(1,3)), A(=Σm(2,3))

> 💡 **Interesting:** For 2 variables, the 4 self-dual functions are exactly {A, A', B, B'}!

### 1.5 XOR and XNOR Properties ⭐

| Property | XOR (⊕) | XNOR (⊙) |
|---|---|---|
| Definition | A'B + AB' | AB + A'B' |
| Identity | A⊕0 = A | A⊙1 = A |
| Complement | A⊕1 = A' | A⊙0 = A' |
| Self-inverse | A⊕A = 0 | A⊙A = 1 |
| Associative | Yes | Yes |
| **Parity** | **Odd # of 1s → 1** | **Even # of 1s → 1** |

> 🏭 **XOR is used in RAID parity, CRC, encryption, swap-without-temp!**

#### 1.5.1 XOR Advanced Properties ⭐⭐

**Multi-variable XOR:**
- A⊕B⊕C = 1 when **odd number** of variables are 1
- This is the foundation of **parity checking** in computers

**Algebraic Properties (comprehensive list):**

| # | Property | Expression |
|---|---|---|
| 1 | Commutativity | A⊕B = B⊕A |
| 2 | Associativity | (A⊕B)⊕C = A⊕(B⊕C) |
| 3 | Identity | A⊕0 = A |
| 4 | Self-inverse | A⊕A = 0 |
| 5 | Complement | A⊕1 = A' |
| 6 | **Cancellation** | If A⊕B = A⊕C, then B=C ⭐ |
| 7 | AND distribution | A·(B⊕C) = AB ⊕ AC ✅ |
| 8 | OR distribution | A+(B⊕C) ≠ (A+B)⊕(A+C) ❌ |
| 9 | De Morgan's analog | (A⊕B)' = A⊙B = A'⊕B = A⊕B' |
| 10 | Complement relation | A⊕B = (A⊙B)' |

> ⚠️ **GATE TRAP:** XOR distributes over AND but NOT over OR! This is the #1 XOR trap.

**XOR in Arithmetic:**
- A⊕B gives the **SUM bit** of a half adder
- A⊕B⊕Cᵢₙ gives the **SUM bit** of a full adder
- XOR chain = parity of all bits

**Practical Applications:**

| Application | How XOR is Used |
|---|---|
| **Parity bit** | P = D₁⊕D₂⊕...⊕Dₙ |
| **CRC computation** | XOR-based polynomial division |
| **RAID 5/6** | Data recovery: D₁⊕D₂⊕P=0, so lost disk = XOR of others |
| **Swap without temp** | a=a⊕b; b=a⊕b; a=a⊕b |
| **Gray code** | Binary↔Gray conversion uses XOR |
| **Stream cipher** | Plaintext ⊕ Key = Ciphertext |
| **Error detection** | Syndrome = received ⊕ computed |
| **Toggle bit** | Flip bit k: value ⊕ (1<<k) |

#### 1.5.2 XNOR Deep Dive

**XNOR = Equivalence function:** Output 1 when inputs are EQUAL.

$$A \odot B = AB + A'B' = (A \oplus B)'$$

**Multi-variable XNOR:**
- For **even** number of variables: A⊙B⊙C⊙D = A⊕B⊕C⊕D (same as XOR!)
- For **odd** number of variables: A⊙B⊙C = (A⊕B⊕C)' (complement of XOR!)

> 🎯 **GATE Memorize:** n-variable XNOR:
> - n even → same as n-variable XOR
> - n odd → complement of n-variable XOR

**Example:** A⊙B⊙C where A=1, B=0, C=1
- Method 1 (3 vars, odd): = (1⊕0⊕1)' = (0)' = 1 ✅
- Method 2 (direct): (1⊙0)⊙1 = (0)⊙1 = 0⊙1 = 0... wait
- Actually: A⊙B = 1·0 + 0·1 = 0. Then 0⊙1 = 0·1 + 1·0 = 0.
- Hmm — so 3-var XNOR with chain evaluation gives 0, but (XOR)' gives 1.

> ⚠️ **Critical:** Multi-variable XNOR is NOT simply chaining 2-input XNORs! The standard definition of n-input XNOR is (X₁⊕X₂⊕...⊕Xₙ)' for odd n, and = X₁⊕X₂⊕...⊕Xₙ for even n. But in GATE, they usually mean **cascaded 2-input XNOR gates**, which IS associative. Be careful about the question's intent!

### 1.6 Minterms and Maxterms

For n variables: **2ⁿ** minterms and maxterms.

**Minterm mᵢ:** Product term, variable uncomplemented if bit=1, complemented if bit=0
**Maxterm Mᵢ:** Sum term, **opposite** convention (complemented if bit=1)

| i | Binary | Minterm (A,B,C) | Maxterm |
|---|---|---|---|
| 0 | 000 | A'B'C' | A+B+C |
| 1 | 001 | A'B'C | A+B+C' |
| 2 | 010 | A'BC' | A+B'+C |
| 3 | 011 | A'BC | A+B'+C' |
| 4 | 100 | AB'C' | A'+B+C |
| 5 | 101 | AB'C | A'+B+C' |
| 6 | 110 | ABC' | A'+B'+C |
| 7 | 111 | ABC | A'+B'+C' |

**Relationship:** mᵢ' = Mᵢ, Mᵢ' = mᵢ

#### 1.6.1 Properties of Minterms and Maxterms ⭐

**Minterm Properties:**
- Any two different minterms: mᵢ · mⱼ = 0 (when i≠j) — they are **mutually exclusive**
- Sum of ALL minterms: m₀ + m₁ + ... + m_{2ⁿ-1} = 1
- Each minterm is 1 for exactly ONE input combination

**Maxterm Properties:**
- Any two different maxterms: Mᵢ + Mⱼ = 1 (when i≠j) — they are **mutually exhaustive**
- Product of ALL maxterms: M₀ · M₁ · ... · M_{2ⁿ-1} = 0
- Each maxterm is 0 for exactly ONE input combination

> 🎯 **GATE Application:** These properties are used to verify simplification results. If your simplified expression gives output 1 for exactly the minterms listed, it's correct!

#### 1.6.2 Converting Between Forms — Step by Step

**SOP → Standard SOP (Canonical):**
If a product term is missing a variable X, expand: term = term·(X+X') = term·X + term·X'

**Example:** F = AB + C (3 variables A,B,C)
- AB = AB(C+C') = ABC + ABC' → m₇ + m₆
- C = C(A+A') = AC + A'C = AC(B+B') + A'C(B+B') = ABC + AB'C + A'BC + A'B'C → m₇+m₅+m₃+m₁

F = Σm(1, 3, 5, 6, 7) (remove duplicate m₇)

**POS → Standard POS (Canonical):**
If a sum term is missing X, expand: term = term + XX' = (term+X)(term+X')

**Example:** F = (A+B)(C) → needs all 3 variables in each factor
- (A+B) = (A+B+CC') = (A+B+C)(A+B+C') → M₀ · M₁... 
  Wait: A+B = (A+B+C)(A+B+C') → maxterm at 000 (A+B+C stays A+B+C) and 001
  Actually A+B is 0 only when A=0,B=0, regardless of C → Minterms where F=0: m₀, m₁
  So (A+B) = ΠM(0,1)
- C is 0 when C=0 → m₀,m₂,m₄,m₆ → ΠM(0,2,4,6)
- F = ΠM(0,1,2,4,6)

### 1.7 Canonical Forms

- **SOP = Σm(list of 1s)**, **POS = ΠM(list of 0s)**
- **Conversion:** Σm(1,3,5,7) ↔ ΠM(0,2,4,6) (complement sets!)
- **F':** Σm(0,2,4,6) = ΠM(1,3,5,7)

#### 1.7.1 Conversion Between Canonical Forms — Complete Guide ⭐

| From | To | Method |
|---|---|---|
| Σm → ΠM | Use missing minterms as maxterm indices | Σm(1,3,5) for 3 vars → ΠM(0,2,4,6,7) |
| ΠM → Σm | Use missing maxterm indices as minterm indices | ΠM(0,2,4,6,7) → Σm(1,3,5) |
| F → F' (SOP) | Swap included/excluded minterms | F=Σm(1,3,5) → F'=Σm(0,2,4,6,7) |
| F → F' (POS) | Swap included/excluded maxterms | F=ΠM(0,2,4,6,7) → F'=ΠM(1,3,5) |

**Worked Example — Full Conversion:**

Given: F(A,B,C) = Σm(1, 2, 5, 7)

| Representation | Form |
|---|---|
| SOP (canonical) | A'B'C + A'BC' + AB'C + ABC |
| POS (canonical) | F = ΠM(0,3,4,6) = (A+B+C)(A+B'+C')(A'+B+C)(A'+B'+C) |
| F' (SOP) | F' = Σm(0,3,4,6) = A'B'C' + A'BC + AB'C' + ABC' |
| F' (POS) | F' = ΠM(1,2,5,7) |

### 1.8 Shannon's Expansion Theorem ⭐

$$F(A,B,C,...) = A \cdot F(1,B,C,...) + A' \cdot F(0,B,C,...)$$

> 🏭 **Foundation for MUX implementation and Binary Decision Diagrams (BDDs)!**

**Example:** F = AB + BC → F_A = B+BC = B, F_{A'} = BC → F = A·B + A'·BC ✓

#### 1.8.1 Shannon Expansion — Detailed Walkthrough ⭐⭐

**General form (expand on variable xᵢ):**
$$F(x_1,...,x_i,...,x_n) = x_i \cdot F(x_1,...,1,...,x_n) + x_i' \cdot F(x_1,...,0,...,x_n)$$

This is called the **positive cofactor** ($F_{x_i}$) and **negative cofactor** ($F_{x_i'}$).

**Double Expansion (on two variables):**
$$F = AB \cdot F_{AB} + AB' \cdot F_{AB'} + A'B \cdot F_{A'B} + A'B' \cdot F_{A'B'}$$

This directly gives us the MUX implementation with 2 select lines!

**Full Example:** F(A,B,C) = A'BC + AB'C + ABC' + ABC

Expand on A:
- $F_A = F(1,B,C) = 0 \cdot BC + 1 \cdot B'C + 1 \cdot BC' + 1 \cdot BC = B'C + BC' + BC = B'C + B = B + C$
- $F_{A'} = F(0,B,C) = 1 \cdot BC + 0 + 0 + 0 = BC$

So: F = A(B+C) + A'(BC) ✅

Now expand $F_A = B+C$ on B:
- $F_{AB} = 1+C = 1$
- $F_{AB'} = 0+C = C$

And expand $F_{A'} = BC$ on B:
- $F_{A'B} = C$
- $F_{A'B'} = 0$

So: **F = AB·1 + AB'·C + A'B·C + A'B'·0**

This directly gives us a 4:1 MUX implementation:
- Select lines: A, B
- Data inputs: I₃=1, I₂=C, I₁=C, I₀=0

> 🎯 **This is exactly how we implement functions using MUX with n-1 select lines!**

#### 1.8.2 Boolean Difference (Reed-Muller Expansion) ⭐

The **Boolean Difference** of F with respect to xᵢ:
$$\frac{\partial F}{\partial x_i} = F_{x_i} \oplus F_{x_i'}$$

This tells us: **for which input combinations does changing xᵢ change the output?**

**Application:** If $\frac{\partial F}{\partial x_i} = 0$, then F does not depend on xᵢ at all (can be eliminated).

**Reed-Muller (XOR) Canonical Form:**
Any Boolean function can be written as XOR of AND terms:
$$F = a_0 \oplus a_1 x_1 \oplus a_2 x_2 \oplus ... \oplus a_{12} x_1 x_2 \oplus ...$$

where each aᵢ ∈ {0,1}.

> 🏭 **Used in testing, error correction, and FPGA synthesis!**

### 1.9 Counting Functions Table 📊

| Property | Formula | n=2 | n=3 |
|---|---|---|---|
| Total Boolean functions | $2^{2^n}$ | 16 | 256 |
| Self-dual | $2^{2^{n-1}}$ | 4 | 16 |
| F(0,...,0)=0 | $2^{2^n-1}$ | 8 | 128 |
| Linear functions | $2^{n+1}$ | 8 | 16 |

### 1.10 Threshold Functions

A function F is a **threshold function** if there exist weights w₁,...,wₙ and threshold T such that:
- F = 1 when Σwᵢxᵢ ≥ T
- F = 0 otherwise

**Examples:** AND(A,B) is threshold with w₁=w₂=1, T=2. OR is threshold with T=1. **XOR is NOT a threshold function!**

#### 1.10.1 Threshold Function Testing ⭐

**How to check if a function is threshold:**
1. Write the truth table
2. Try to find weights (w₁,...,wₙ) and threshold T satisfying all rows
3. If no consistent assignment exists → NOT threshold

**Example: Is XOR a threshold function?**

| A | B | XOR | Condition |
|---|---|---|---|
| 0 | 0 | 0 | 0 < T → T > 0 |
| 0 | 1 | 1 | w₂ ≥ T |
| 1 | 0 | 1 | w₁ ≥ T |
| 1 | 1 | 0 | w₁+w₂ < T |

From rows 2,3: w₁≥T, w₂≥T → w₁+w₂ ≥ 2T
From row 4: w₁+w₂ < T
→ 2T ≤ w₁+w₂ < T → 2T < T → T < 0. But from row 1, T > 0. CONTRADICTION!

**XOR is NOT a threshold function!** ✅

**Known threshold functions:**

| Function | Weights | Threshold |
|---|---|---|
| AND(A,B) | w₁=1, w₂=1 | T=2 |
| OR(A,B) | w₁=1, w₂=1 | T=1 |
| NOT(A) | w₁=−1 | T=0 |
| NAND(A,B) | w₁=−1, w₂=−1 | T=−1 |
| NOR(A,B) | w₁=−1, w₂=−1 | T=0 |
| Majority(A,B,C) | w₁=w₂=w₃=1 | T=2 |

> 🏭 **Connection to AI/ML:** Threshold functions are exactly what a single-layer perceptron can learn! XOR's non-threshold nature is the famous limitation of single-layer neural networks (Minsky & Papert, 1969). Need at least 2 layers!

### 1.11 Boolean Function Decomposition ⭐

#### 1.11.1 Decomposition Theorem

Any Boolean function can be decomposed:
$$F(x_1,...,x_n) = x_1 \cdot F_{x_1} + x_1' \cdot F_{x_1'}$$

where $F_{x_1} = F(1,x_2,...,x_n)$ (cofactor with respect to $x_1$)

**Recursive Application:** Repeatedly applying gives the BDD (Binary Decision Diagram) structure.

#### 1.11.2 BDD (Binary Decision Diagram) ⭐

> 🏭 **Modern tool for Boolean function analysis — used in formal verification!**

A BDD is a rooted, directed acyclic graph representing a Boolean function:
- Internal nodes: test one variable (branch on 0 or 1)
- Terminal nodes: 0 or 1 (function value)
- OBDD (Ordered BDD): variable ordering is fixed on all paths
- ROBDD (Reduced OBDD): no redundant tests, no duplicate subtrees → **canonical form!**

**Key property:** Two functions are equivalent ↔ their ROBDDs are identical (given same variable ordering)

**Size can vary dramatically with variable ordering:**
- F = x₁x₂ + x₃x₄: Good ordering (x₁,x₂,x₃,x₄) → small BDD
- Same function, bad ordering (x₁,x₃,x₂,x₄) → larger BDD!

> 🎯 **Not commonly asked in GATE but appears in advanced questions about function representation.**

### 1.12 Functional Properties of Boolean Functions ⭐

**Symmetric Functions:**
A function is symmetric if its output depends only on the NUMBER of 1s in the input, not which specific inputs are 1.

**Examples:** AND (all 1s), OR (≥1), XOR (odd number), Majority.
**Non-example:** F=AB+C (depends on which inputs are 1)

**Unate Functions:**
- **Positive unate in xᵢ:** Changing xᵢ from 0→1 can only change F from 0→1 (never 1→0)
- **Negative unate in xᵢ:** Opposite
- **Unate:** Function is positive or negative unate in EVERY variable

**Monotone functions are positive unate in all variables!**

---

# 📉 PART 2: MINIMIZATION

## 2. K-Maps (Karnaugh Maps) 🗺️

> 💡 **Beginner Analogy:** A K-map is like a Sudoku puzzle for Boolean functions 🧩. You lay out all possible inputs in a 2D grid, mark where the function is 1, and then draw the biggest possible rectangles around groups of 1s. Each rectangle gives you a simpler term!

> 🏭 **History:** Introduced by Maurice Karnaugh at Bell Labs in 1953. Before K-maps, simplification was done purely algebraically — slow, error-prone, and didn't guarantee minimum result!

### 2.1 K-Map Layouts

**2-Variable:**
```
       B=0  B=1
A=0 |  m₀ | m₁ |
A=1 |  m₂ | m₃ |
```

**3-Variable (Gray code columns!):**
```
         BC=00  BC=01  BC=11  BC=10
A=0  |   m₀  |  m₁  |  m₃  |  m₂  |
A=1  |   m₄  |  m₅  |  m₇  |  m₆  |
```

> ⚠️ **CRITICAL:** Column order is 00, 01, **11, 10** (Gray code), NOT 00, 01, 10, 11!

**4-Variable:**
```
         CD=00  CD=01  CD=11  CD=10
AB=00 |   m₀  |  m₁  |  m₃  |  m₂  |
AB=01 |   m₄  |  m₅  |  m₇  |  m₆  |
AB=11 |  m₁₂ | m₁₃  | m₁₅  | m₁₄  |
AB=10 |   m₈  |  m₉  | m₁₁  | m₁₀  |
```

> ⚠️ **Row order also Gray:** 00, 01, **11, 10** — NOT 00, 01, 10, 11!

**5-Variable:** Two 4-var maps (A=0 and A=1) side by side; corresponding cells are adjacent.

```
A=0 Map:                          A=1 Map:
         CD=00 CD=01 CD=11 CD=10          CD=00 CD=01 CD=11 CD=10
BC=00 |  m₀  | m₁  | m₃  | m₂  | BC=00 | m₁₆ | m₁₇ | m₁₉ | m₁₈ |
BC=01 |  m₄  | m₅  | m₇  | m₆  | BC=01 | m₂₀ | m₂₁ | m₂₃ | m₂₂ |
BC=11 | m₁₂  | m₁₃ | m₁₅ | m₁₄ | BC=11 | m₂₈ | m₂₉ | m₃₁ | m₃₀ |
BC=10 |  m₈  | m₉  | m₁₁ | m₁₀ | BC=10 | m₂₄ | m₂₅ | m₂₇ | m₂₆ |
```

**6-Variable:** Four 4-var maps in a 2×2 arrangement.

### 2.2 Grouping Rules

1. Groups = powers of 2: {1, 2, 4, 8, 16}
2. Rectangles only (including wrap-around!)
3. **Larger groups → simpler terms** (group of 2ᵏ eliminates k variables)
4. Every 1 covered at least once
5. Minimum groups for minimum expression
6. Groups CAN overlap
7. **4 corners** form a valid group (m₀,m₂,m₈,m₁₀ = B'D')

#### 2.2.1 What Each Group Size Means ⭐

| Group Size | Variables Eliminated | Term Has | Example (4-var map) |
|---|---|---|---|
| 1 cell | 0 eliminated | 4 variables | ABCD (one minterm) |
| 2 cells | 1 eliminated | 3 variables | ABC (two adjacent) |
| 4 cells | 2 eliminated | 2 variables | AB (a row or column) |
| 8 cells | 3 eliminated | 1 variable | A (half the map) |
| 16 cells | 4 eliminated | 0 variables | **1** (all cells = tautology) |

> 🎯 **GATE Formula:** Group of $2^k$ cells in n-variable map → term with $n-k$ literals.

#### 2.2.2 Wrap-Around Adjacencies — Complete List (4-var) ⭐⭐

These are the adjacencies students MISS:

| Wrap Type | Adjacent Cells | Common Term |
|---|---|---|
| **Left-Right** | m₀-m₂, m₁-m₃ (wrong! these ARE adjacent in Gray) | — |
| **Left-Right** | m₀↔m₂, m₄↔m₆, m₈↔m₁₀, m₁₂↔m₁₄ | ...D' (leftmost & rightmost cols) |
| **Top-Bottom** | m₀↔m₈, m₁↔m₉, m₂↔m₁₀, m₃↔m₁₁ | ...A' ↔ ...A (top & bottom rows) |
| **All 4 corners** | m₀, m₂, m₈, m₁₀ | B'D' |
| **Column wrap** | m₀,m₂,m₈,m₁₀ 'edges | B'D' |

> **Mnemonic:** "The K-map is a torus (donut 🍩)!" Top connects to bottom, left connects to right. Imagine wrapping the paper into a cylinder, then bending it into a donut.

### 2.3 Don't Cares

Can be 0 or 1 to maximize group sizes. Don't cares stay as don't cares in F' too!

> 💡 **When to use don't cares:**
> - BCD functions: inputs 1010-1111 never occur → 6 don't cares
> - Seven-segment display decoder: inputs 10-15 are don't cares
> - Any input combination that's physically impossible

**Rules:**
- Include don't cares in groups if they help make groups BIGGER
- Do NOT make groups of ONLY don't cares (they must include at least one real 1)
- Don't cares do NOT need to be covered (unlike real 1s)
- Different minimized expressions may treat don't cares differently

### 2.4 Prime Implicants (PI) & Essential PIs (EPI) ⭐⭐

- **Implicant:** Any group of 1s (and don't cares) in the K-map.
- **PI:** Largest possible group (can't be made bigger) — **a group that isn't a subset of any larger group**
- **EPI:** Covers at least one minterm that no other PI covers → **must include!**

**Steps:** Find all PIs → Identify EPIs → Cover remaining with minimum PIs.

#### 2.4.0 Counting PIs and EPIs — GATE Shortcuts ⭐⭐

**How many PIs?** A PI is a maximal group of 1s (and don't cares). Count ALL maximally large groups.

**How many EPIs?** An EPI is a PI that covers at least one minterm uniquely (no other PI covers that minterm).

**GATE Shortcut for counting PIs:**
1. Draw the K-map
2. Find every possible maximal group
3. A group is NOT a PI if it's completely contained in a larger group

**Common patterns and their PI counts:**

| Pattern | # PIs | # EPIs |
|---|---|---|
| Single isolated 1 | 1 | 1 |
| Two adjacent 1s (not part of bigger) | 1 | Usually 1 |
| Checkerboard (XOR-like) | Many | Many |
| All 1s (tautology) | 1 | 1 |
| Line of 4 (no bigger group) | 1 | Usually 1 |

> 🎯 **GATE Trick for counting PIs:** In a 4-variable map:
> - Total PIs can be found by Q-M method or visual inspection
> - If asking "how many EPIs exist?" → look for minterms covered by exactly ONE PI
> - If ALL PIs are essential, the minimum SOP = set of all EPIs

**Example of many PIs:** F(A,B,C,D) = Σm(0,5,7,8,10,14,15)

This function has an "XOR-like" pattern with scattered 1s → many PIs, many EPIs.
Let's count: m₀,m₅ not adjacent. m₅,m₇ adjacent (group BD). m₈,m₁₀ adjacent (AD'). m₁₄,m₁₅ adjacent (ABD'? No: 1110,1111→AB). m₇,m₁₅ adjacent (BC). m₁₀,m₁₄ adjacent (AC).

PIs: {A'B'C'D'(m₀)}, {A'BD(m₅,m₇)}, {AB'D'(m₈,m₁₀)}, {ABD(m₁₅... need to check), {AB(m₁₄,m₁₅)}, {ACD'(m₁₀,m₁₄)}, {BCD(m₇,m₁₅)}

m₀ is isolated (alone) → PI by itself = A'B'C'D', definitely EPI.
Each minterm covered by only one or few PIs → counting requires care.

#### 2.4.1 PI/EPI Identification — Complete Procedure ⭐⭐⭐

**Step 1:** List ALL prime implicants (every maximal group).
**Step 2:** Create a PI chart — rows=PIs, columns=minterms that must be covered.
**Step 3:** Find \*-columns (columns with only ONE ✓). The PI in that row is an EPI.
**Step 4:** Include all EPIs. Remove covered minterms.
**Step 5:** If uncovered minterms remain, choose minimum additional PIs.

**Worked Example: F(A,B,C,D) = Σm(0,2,3,5,7,8,9,10,11,13,15)**

**Step 1 — Draw K-map:**
```
         CD=00  CD=01  CD=11  CD=10
AB=00 |   1   |  0   |  1   |  1   |    m₀,m₃,m₂
AB=01 |   0   |  1   |  1   |  0   |    m₅,m₇
AB=11 |   0   |  1   |  1   |  0   |    m₁₃,m₁₅
AB=10 |   1   |  1   |  1   |  1   |    m₈,m₉,m₁₁,m₁₀
```

**Step 2 — Find all PIs:** (each maximal rectangular group of 1s)
- PI₁: m₀,m₂,m₈,m₁₀ → B'D' (4 cells)
- PI₂: m₂,m₃,m₁₀,m₁₁ → B'C (4 cells)
- PI₃: m₃,m₇,m₁₁,m₁₅ → CD (4 cells)
- PI₄: m₅,m₇,m₁₃,m₁₅ → BD (4 cells)
- PI₅: m₈,m₉,m₁₀,m₁₁ → AB' (4 cells)

**Step 3 — PI Chart:**

| PI | m₀ | m₂ | m₃ | m₅ | m₇ | m₈ | m₉ | m₁₀ | m₁₁ | m₁₃ | m₁₅ |
|---|---|---|---|---|---|---|---|---|---|---|---|
| B'D' | ✓ | ✓ | | | | ✓ | | ✓ | | | |
| B'C | | ✓ | ✓ | | | | | ✓ | ✓ | | |
| CD | | | ✓ | | ✓ | | | | ✓ | | ✓ |
| BD | | | | ✓ | ✓ | | | | | ✓ | ✓ |
| AB' | | | | | | ✓ | ✓ | ✓ | ✓ | | |

**Step 4 — Find EPIs:**
- m₀: only covered by B'D' → **EPI: B'D'** ⭐
- m₅: only covered by BD → **EPI: BD** ⭐
- m₉: only covered by AB' → **EPI: AB'** ⭐

After including B'D', BD, AB': covered = {m₀,m₂,m₅,m₇,m₈,m₉,m₁₀,m₁₃,m₁₅}
Uncovered: {m₃, m₁₁}

**Step 5:** Both B'C and CD cover m₃ and m₁₁. Choose either.
- Pick B'C (or CD — both give same cost)

**Result: F = B'D' + BD + AB' + B'C** ✅

> 🎯 **Counting PIs and EPIs is a common GATE question!** In this example: 5 PIs, 3 EPIs.

#### 2.4.2 Counting Prime Implicants — GATE Shortcut ⭐⭐

**For F = Σm(list with don't cares d(list)):**
A prime implicant is any group of 2ᵏ adjacent cells (including don't cares) that cannot be made larger.

**Common counting questions:**

| Function | PIs | EPIs |
|---|---|---|
| F = Σm(0,2,5,7) (3-var) | 4 | 4 |
| F = Σm(0,1,2,3) (2-var) | 1 | 1 |
| F = Σm(all) | 1 (constant 1) | 1 |

### 2.5 POS Minimization

Group the **0s** instead of 1s. Each group gives a maxterm factor.

> 💡 **When to use POS:** When there are **fewer 0s than 1s** in the truth table, POS gives a simpler expression!

**Procedure:**
1. Put 0s in the K-map (where F=0)
2. Group the 0s (same grouping rules)
3. Read off the **complemented** terms (each group gives a sum term)
4. OR the variables that DON'T change in the group, using the **complement** of their value

**Example:** F(A,B,C) = Σm(1,3,5,6,7)
- F=0 at m₀(000), m₂(010), m₄(100)
- Group m₀,m₄ → C' stays 0 → Maxterm factor = (B+C) 
  Wait — let me redo properly.

For POS: Group the 0s.
- m₀(000), m₂(010): A=0 in both, C=0 in both, B varies → term: A+C (variables that are 0 in the group, UNcomplemented; variables that are 1 in group, complemented. For POS: opposite of SOP reading)

Actually the correct way: For each group of 0s, the maxterm factor is formed by:
- Variable = 0 in group → variable appears uncomplemented
- Variable = 1 in group → variable appears complemented
- Variable changes → eliminated

m₀(000), m₂(010): A=0→A, B=varies→eliminated, C=0→C. Factor = **(A+C)** ✅
m₀(000), m₄(100): A=varies→eliminated, B=0→B, C=0→C. Factor = **(B+C)** ✅

F = (A+C)(B+C) ✅

#### 2.5.1 SOP vs POS Comparison ⭐

| Feature | SOP (Σ) | POS (Π) |
|---|---|---|
| Group what? | 1s in K-map | 0s in K-map |
| Output form | Sum of products | Product of sums |
| Gate level-1 | AND gates | OR gates |
| Gate level-2 | OR gate | AND gate |
| Better when | Fewer 1s | Fewer 0s |
| NAND-NAND | SOP → direct | Need conversion |
| NOR-NOR | Need conversion | POS → direct |

## 3. Quine-McCluskey Method ⭐⭐

> 🏭 **Systematic method for >5 variables, computerizable!**

> 💡 **Why Q-M when we have K-maps?** K-maps become impractical beyond 5-6 variables. Q-M works for ANY number of variables and can be programmed! Used in EDA tools like Espresso.

### 3.1 Algorithm

**Step 1 — Generate PIs:**
1. List minterms by # of 1s
2. Compare adjacent groups — combine if differ in 1 bit (replace with '-')
3. Repeat until no more combinations
4. Uncombined = Prime Implicants

**Step 2 — PI Chart:**
1. Rows=PIs, Columns=Minterms
2. EPIs = columns with single ✓
3. Cover remaining with Petrick's method if needed

### 3.2 Quine-McCluskey — Full Worked Example ⭐⭐⭐

**Problem:** Minimize F(A,B,C,D) = Σm(0, 1, 2, 5, 6, 7, 8, 9, 10, 14)

**Step 1 — Group minterms by number of 1s:**

| Group (# of 1s) | Minterms | Binary (ABCD) |
|---|---|---|
| 0 | m₀ | 0000 |
| 1 | m₁, m₂, m₈ | 0001, 0010, 1000 |
| 2 | m₅, m₆, m₉, m₁₀ | 0101, 0110, 1001, 1010 |
| 3 | m₇, m₁₄ | 0111, 1110 |

**Step 2 — First-round comparison (combine adjacent groups):**

| Pair | Combined | Dash Form | Term |
|---|---|---|---|
| m₀,m₁ | 000- | A'B'C' | |
| m₀,m₂ | 00-0 | A'B'D' | |
| m₀,m₈ | -000 | B'C'D' | |
| m₁,m₅ | 0-01 | A'C'D | |
| m₁,m₉ | -001 | B'C'D | |
| m₂,m₆ | 0-10 | A'C'D' ... wait |

Let me redo more carefully:
- m₂(0010) + m₆(0110) → 0-10 → A'CD' (B eliminated)
- m₂(0010) + m₁₀(1010) → -010 → B'CD' (A eliminated)
- m₈(1000) + m₉(1001) → 100- → AB'C' (D eliminated)
- m₈(1000) + m₁₀(1010) → 10-0 → AB'D' (C eliminated)
- m₅(0101) + m₇(0111) → 01-1 → A'BD (C eliminated)
- m₆(0110) + m₇(0111) → 011- → A'BC (D eliminated)
- m₆(0110) + m₁₄(1110) → -110 → BCD' (A eliminated)
- m₉(1001) + (none in group 3 differ by 1 bit from 1001)
- m₁₀(1010) + m₁₄(1110) → 1-10 → ACD' (B eliminated)

**Step 3 — Second-round comparison:**

| Pair | Combined | Dash Form |
|---|---|---|
| (m₀,m₁)+(m₈,m₉) | -00- | B'C' |
| (m₀,m₂)+(m₈,m₁₀) | -0-0 | B'D' |
| (m₀,m₈)+(m₁,m₉) | -00- | B'C' (same!) |
| (m₀,m₈)+(m₂,m₁₀) | -0-0 | B'D' (same!) |
| (m₂,m₆)+(m₁₀,m₁₄) | --10 | CD' |
| (m₂,m₁₀)+(m₆,m₁₄) | --10 | CD' (same!) |

**Step 4 — Identify PIs (unchecked terms):**

| PI | Term | Covers |
|---|---|---|
| B'C' | -00- | m₀,m₁,m₈,m₉ |
| B'D' | -0-0 | m₀,m₂,m₈,m₁₀ |
| CD' | --10 | m₂,m₆,m₁₀,m₁₄ |
| A'C'D | 0-01 | m₁,m₅ |
| A'BD | 01-1 | m₅,m₇ |
| A'BC | 011- | m₆,m₇ |
| BCD' | -110 | m₆,m₁₄ |

**Step 5 — PI Chart and EPIs:**

Check for columns with single ✓:
- m₅: covered by A'C'D and A'BD → not unique
- m₇: covered by A'BD and A'BC → not unique
- m₉: covered by B'C' only → **EPI: B'C'** ⭐
- m₁₄: covered by CD' and BCD' → not unique

After selecting B'C (covers m₀,m₁,m₈,m₉) and CD' (covers m₂,m₆,m₁₀,m₁₄):
- Still need: m₅, m₇
- A'BD covers both m₅ and m₇ ✅

**Result: F = B'C' + CD' + A'BD** ✅ (3 product terms)

### 3.3 Petrick's Method

For uncovered minterms: write OR of covering PIs for each, form AND of all, expand to SOP, pick minimum cost product term.

#### 3.3.1 Petrick's Method — Detailed Example ⭐

After selecting EPIs, suppose minterms m₅ and m₇ remain uncovered.

Covering options:
- m₅: P₁ (A'C'D) or P₂ (A'BD)
- m₇: P₂ (A'BD) or P₃ (A'BC)

**Form product of sums:**
(P₁ + P₂)(P₂ + P₃) = P₁P₂ + P₁P₃ + P₂P₂ + P₂P₃ = P₁P₂ + P₁P₃ + P₂ + P₂P₃

Apply absorption (P₂ absorbs P₁P₂ and P₂P₃):
= **P₂** + P₁P₃

**Minimum:** P₂ alone (A'BD) covers both! → choose P₂.

If costs are equal, pick the solution with fewer PIs. If still tied, pick the one with fewer literals.

### 3.4 Multi-Output Minimization ⭐

When implementing multiple functions, shared product terms reduce total hardware.

**Example:** F₁ = Σm(0,2,3,7), F₂ = Σm(0,2,6,7) (3 variables)

Individual minimization:
- F₁ = A'B' + BC (2 terms)
- F₂ = A'C' + AB (2 terms)
- Total: 4 product terms

Shared minimization:
- Both use A'B'C' (m₀) and A'BC' (m₂, only in F₂)... let me think.
- Shared term A'C' covers m₀,m₂ for both functions when applicable.
- F₁ = A'C' + BC (A'C' covers m₀,m₂, and BC covers m₃,m₇)
  Check: m₀(000)✅, m₂(010)✅, m₃(011)✅, m₇(111)✅ ✅
- F₂ = A'C' + AB (A'C' covers m₀,m₂, and AB covers m₆,m₇)
  Check: m₀✅, m₂✅, m₆(110)✅, m₇(111)✅ ✅
- Shared: A'C'. Total unique terms: {A'C', BC, AB} = **3 terms** (saved 1!)

> 🏭 **This is why PLA benefits from shared product terms! Minimizing total terms is crucial.**

### 3.5 K-Map Solved Examples — By Variable Count ⭐⭐

#### 3.5.1 Two-Variable Example

**F(A,B) = Σm(1,2,3)**

```
     B=0  B=1
A=0 | 0  | 1 |
A=1 | 1  | 1 |
```

Groups: m₁,m₃ → B. m₂,m₃ → A. Or group all three: m₁,m₂,m₃ → not a power-of-2-shaped rectangle.
Actually m₁(01), m₃(11) → B. m₂(10), m₃(11) → A.
**F = A + B** ✅

#### 3.5.2 Three-Variable Example

**F(A,B,C) = Σm(0,2,4,5,6)**

```
      BC=00  BC=01  BC=11  BC=10
A=0 |  1   |  0   |  0   |  1   |
A=1 |  1   |  1   |  0   |  1   |
```

Groups:
- m₀,m₂,m₄,m₆ (all C=0 cells, form a column) → C'
- m₄,m₅ → AB' (actually A=1,B=0: wait m₄=100, m₅=101: AB' with C varying → AB'? But m₅ has B=0, and m₄ has B=0. So the pair is A=1,B=0 → AB'. But we need to check if they're adjacent: m₄(100) and m₅(101) differ in C → yes! Group = AB'.)

**F = C' + AB'** ✅ (2 product terms, 3 literals)

#### 3.5.3 Four-Variable Full Example

**F(A,B,C,D) = Σm(0,1,2,4,5,6,8,9,12,13,14)**

```
         CD=00  CD=01  CD=11  CD=10
AB=00 |   1   |  1   |  0   |  1   |
AB=01 |   1   |  1   |  0   |  1   |
AB=11 |   1   |  1   |  0   |  1   |
AB=10 |   1   |  1   |  0   |  0   |
```

The CD=11 column is all 0! And CD=00 and CD=01 columns are all 1s (8 cells).

Groups:
- m₀,m₁,m₄,m₅,m₈,m₉,m₁₂,m₁₃ (CD=00 and CD=01 columns) → D' (8 cells = 3 vars eliminated, 1 left)
  Wait: CD=00 means C=0,D=0. CD=01 means C=0,D=1. Together: C=0, D varies → C'. But all A,B combinations → C' is correct? Let me verify:
  
  m₀(0000)✓, m₁(0001)✓, m₄(0100)✓, m₅(0101)✓, m₈(1000)✓, m₉(1001)✓, m₁₂(1100)✓, m₁₃(1101)✓
  
  All have C=0. Yep → **C'**. (8-cell group eliminates 3 variables!)

- Still uncovered: m₂(0010), m₆(0110), m₁₄(1110)
  m₂,m₆,m₁₄: in CD=10 column.
  m₂(0010), m₆(0110): B varies → A'CD' 
  m₂(0010), m₆(0110), m₁₄(1110): can we group? 0010, 0110, 1110: not a power of 2! (3 cells)
  m₂,m₆ → A'CD' (2 cells)
  m₆,m₁₄ → BCD' (2 cells)
  Or m₂,m₆,m₁₀(not in F),m₁₄ → need m₁₀? m₁₀=1010, F(1010)=0 ✗

So: m₂,m₆ = {0010, 0110} → A'CD' (A=0, C=1, D=0, B varies)
m₆,m₁₄ = {0110, 1110} → BCD' (B=1, C=1, D=0, A varies)

We need both to cover m₂ and m₁₄.

**F = C' + A'CD' + BCD'** ✅

Can simplify? A'CD' + BCD' = CD'(A'+B). Hmm, that's multi-level. Staying in SOP:
**F = C' + A'CD' + BCD'** (3 terms) ✅

#### 3.5.4 Five-Variable Example ⭐⭐

**F(A,B,C,D,E) = Σm(0,2,4,6,9,11,13,15,17,21,25,29)**

This uses two 4-variable maps:

A=0 Map (minterms 0-15):
```
         DE=00  DE=01  DE=11  DE=10
BC=00 |  m₀=1 | m₁=0 | m₃=0 | m₂=1 |
BC=01 |  m₄=1 | m₅=0 | m₇=0 | m₆=1 |
BC=11 | m₁₂=0| m₁₃=1| m₁₅=1| m₁₄=0|
BC=10 |  m₈=0 | m₉=1 | m₁₁=1| m₁₀=0|
```

A=1 Map (minterms 16-31):
```
         DE=00  DE=01  DE=11  DE=10
BC=00 | m₁₆=0| m₁₇=1| m₁₉=0| m₁₈=0|
BC=01 | m₂₀=0| m₂₁=1| m₂₃=0| m₂₂=0|
BC=11 | m₂₈=0| m₂₉=1| m₃₁=0| m₃₀=0|
BC=10 | m₂₄=0| m₂₅=1| m₂₇=0| m₂₆=0|
```

Groups in A=0 map:
- m₀,m₂,m₄,m₆ → A'C'E' (4 cells, B varies, D=0 always... wait: m₀=00000, m₂=00010, m₄=00100, m₆=00110. B varies, D varies? No: m₀ DE=00, m₂ DE=10, m₄ DE=00 with BC=01, m₆ DE=10 with BC=01. So C=0 for all (BC=00 and BC=01 have C=0? No! BC=01 has B=0,C=1... wait).

Actually for 5 variables, let me reconsider the labeling. If ABCDE, then m₀=00000, m₂=00010, m₄=00100, m₆=00110.

OK this is getting very complex for a 5-variable example. The key point is:

> 🎯 **5-variable K-map rule:** Corresponding cells in the A=0 and A=1 maps are ADJACENT. You can group across maps!

Groups in A=1 map: m₁₇,m₂₁,m₂₅,m₂₉ → all in DE=01 column → ADE (B and C vary, D=0, E=1)

> 🎯 **For GATE:** 5-variable K-map problems are rare. If they appear, focus on finding groups within each sub-map first, then check for cross-map groups.

---

# 🔢 PART 3: NUMBER SYSTEMS & CODES

## 4. Number Systems

> 💡 **Beginner Analogy:** Imagine you only have a certain number of symbols to count with. We normally use 10 symbols (0-9) because we have 10 fingers 🖐️🖐️. But computers only have ON/OFF switches, so they use 2 symbols (0,1). Different bases are just different "alphabets" for numbers!

### 4.1 Conversion Table

| Base | Name | Digits | C Prefix |
|---|---|---|---|
| 2 | Binary | 0,1 | 0b |
| 8 | Octal | 0-7 | 0 |
| 10 | Decimal | 0-9 | — |
| 16 | Hex | 0-F | 0x |

**Binary↔Octal:** Group by 3 bits. **Binary↔Hex:** Group by 4 bits.

#### 4.1.1 Base Conversion — Complete Method Guide ⭐

**Decimal → Any Base B:**
1. **Integer part:** Repeatedly divide by B, collect remainders (bottom to top)
2. **Fraction part:** Repeatedly multiply by B, collect integer parts (top to bottom)

**Example: 45.6875₁₀ → Binary**

Integer 45: 45÷2=22 r **1**, 22÷2=11 r **0**, 11÷2=5 r **1**, 5÷2=2 r **1**, 2÷2=1 r **0**, 1÷2=0 r **1**
→ 101101₂

Fraction 0.6875: 0.6875×2=**1**.375, 0.375×2=**0**.75, 0.75×2=**1**.5, 0.5×2=**1**.0
→ .1011₂

Result: **45.6875₁₀ = 101101.1011₂** ✅

**Any Base B → Decimal:** Weighted positional sum
- $d_n B^n + d_{n-1}B^{n-1} + ... + d_0 B^0 + d_{-1}B^{-1} + ...$

**Example: 2AF.8₁₆ → Decimal**
= 2×16² + 10×16¹ + 15×16⁰ + 8×16⁻¹
= 512 + 160 + 15 + 0.5 = **687.5₁₀** ✅

**Between non-decimal bases:** Convert via decimal or binary as intermediate.
- Octal→Hex: Octal→Binary (group by 3)→Hex (group by 4)

**Example: 357₈ → Hex**
- 3→011, 5→101, 7→111 → 011101111₂
- Group by 4 from right: 0|1110|1111 → 0EF₁₆ ✅

#### 4.1.2 Fractional Base Conversion — Tricky Cases ⭐

> ⚠️ **GATE Trap:** Some decimal fractions have NON-TERMINATING binary representations!

**Example: 0.1₁₀ → Binary**
0.1×2=0.2, 0.2×2=0.4, 0.4×2=0.8, 0.8×2=1.6, 0.6×2=1.2, 0.2×2=0.4...
→ 0.0001100110011... (repeating!) → 0.1₁₀ cannot be represented exactly in binary!

> 🏭 **This is why floating-point arithmetic has errors!** 0.1+0.2 ≠ 0.3 in most programming languages.

**Commonly tested exact conversions:**

| Decimal | Binary | Hex |
|---|---|---|
| 0.5 | 0.1 | 0.8 |
| 0.25 | 0.01 | 0.4 |
| 0.125 | 0.001 | 0.2 |
| 0.75 | 0.11 | 0.C |
| 0.875 | 0.111 | 0.E |
| 0.6875 | 0.1011 | 0.B |

> 🎯 **GATE Trick:** A decimal fraction has an exact binary representation iff its denominator is a power of 2 when reduced to lowest terms. So 0.1 = 1/10 → no! 0.75 = 3/4 → yes!

#### 4.1.2 Base Conversion — Between Non-Decimal Bases ⭐

**Shortcut for Binary ↔ Octal:** Group binary digits in groups of 3 (from radix point).
**Shortcut for Binary ↔ Hex:** Group binary digits in groups of 4 (from radix point).

**Example:** Binary 1101011.10110₂

To Octal: Group by 3: 001 101 011 . 101 100 → **153.54₈**
To Hex: Group by 4: 0110 1011 . 1011 0000 → **6B.B0₁₆**

**Octal → Hex (or Hex → Octal):** Convert via Binary as intermediate.

**Example:** 2AF₁₆ → Binary → Octal
2AF₁₆ = 0010 1010 1111₂ = 001 010 101 111₂ = **1257₈**

#### 4.1.3 Rapid Conversion Table (MUST know for GATE!) ⭐

| Hex | Binary | Decimal | Octal |
|---|---|---|---|
| 0 | 0000 | 0 | 00 |
| 1 | 0001 | 1 | 01 |
| 2 | 0010 | 2 | 02 |
| 3 | 0011 | 3 | 03 |
| 4 | 0100 | 4 | 04 |
| 5 | 0101 | 5 | 05 |
| 6 | 0110 | 6 | 06 |
| 7 | 0111 | 7 | 07 |
| 8 | 1000 | 8 | 10 |
| 9 | 1001 | 9 | 11 |
| A | 1010 | 10 | 12 |
| B | 1011 | 11 | 13 |
| C | 1100 | 12 | 14 |
| D | 1101 | 13 | 15 |
| E | 1110 | 14 | 16 |
| F | 1111 | 15 | 17 |

#### 4.1.4 Powers of 2 — MUST Memorize! ⭐

| n | 2ⁿ | Notes |
|---|---|---|
| 0 | 1 | — |
| 1 | 2 | — |
| 2 | 4 | — |
| 3 | 8 | — |
| 4 | 16 | — |
| 5 | 32 | — |
| 6 | 64 | — |
| 7 | 128 | — |
| 8 | 256 | — |
| 9 | 512 | — |
| 10 | 1024 | 1K |
| 12 | 4096 | 4K |
| 14 | 16384 | 16K |
| 16 | 65536 | 64K |
| 20 | 1,048,576 | 1M |
| 24 | 16,777,216 | 16M |
| 30 | 1,073,741,824 | 1G |
| 32 | 4,294,967,296 | 4G |

> 🎯 **GATE Trick:** $2^{10} \approx 10^3$ (1024 ≈ 1000). Use this for quick estimation:
> $2^{20} \approx 10^6$ (1M), $2^{30} \approx 10^9$ (1G), $2^{40} \approx 10^{12}$ (1T)

#### 4.1.5 Base Conversion GATE Problems — Common Types ⭐

**Type 1: "What is the decimal equivalent of..."**
Direct positional notation: $N = \sum d_i \times B^i$

**Type 2: "How many bits needed to represent N?"**
Answer: $\lceil \log_2(N+1) \rceil$ for unsigned. Add 1 for signed.

**Type 3: "What is the hexadecimal of 2¹⁰ − 1?"**
$2^{10} - 1 = 1023 = 0x3FF$ (10 bits all 1s = 3FF)

**Type 4: "What is the octal of 2¹² − 256?"**
$2^{12} - 2^8 = 4096 - 256 = 3840 = 0x0F00 = 0b111100000000 = 7400₈$

### 4.2 Signed Representations ⭐

| Type | Range (n bits) | −0? | Addition Circuit |
|---|---|---|---|
| Sign-Magnitude | −(2ⁿ⁻¹−1) to +(2ⁿ⁻¹−1) | Yes | Complex |
| 1's Complement | −(2ⁿ⁻¹−1) to +(2ⁿ⁻¹−1) | Yes | End-around carry |
| **2's Complement** | **−2ⁿ⁻¹ to +(2ⁿ⁻¹−1)** | **No** | **Simple!** |

> 🎯 **Memorize:** 8-bit: −128 to +127. 16-bit: −32768 to +32767. 32-bit: −2³¹ to +2³¹−1.

#### 4.2.1 How Each Representation Works ⭐⭐

**Sign-Magnitude:**
- MSB = sign (0=+, 1=−), remaining bits = magnitude
- +5 in 8-bit: 00000101, −5: 10000101
- Two zeros: +0 = 00000000, −0 = 10000000
- Simple to understand, hard to do arithmetic (need to compare magnitudes)

**1's Complement:**
- Positive: same as binary
- Negative: flip all bits (bitwise NOT)
- +5 = 00000101, −5 = 11111010
- Two zeros: +0 = 00000000, −0 = 11111111
- **End-around carry:** If addition overflows, add the carry back to LSB!

**1's Complement Addition Example:** −3 + 5
- −3 = 11111100, +5 = 00000101
- 11111100 + 00000101 = 100000001 (9-bit result)
- End-around carry: 00000001 + 1 = **00000010** = +2 ✅

**2's Complement:**
- Positive: same as binary
- Negative: flip all bits + add 1 (or: copy from right including first 1, flip the rest)
- +5 = 00000101, −5 = 11111011
- **Only one zero:** 00000000
- **One extra negative:** −128 (10000000) has no positive counterpart in 8-bit!

> ⚠️ **GATE Trap:** Negating −128 in 8-bit → should be +128, but +128 doesn't fit! Result: still −128 (overflow!)

**2's Complement shortcut:** Copy from right including first 1, flip the rest!
- Example: 01101000 → find −01101000
  - Copy from right: ...1000 → 1000
  - Flip rest: 011010... → 100101...
  - Result: **10011000** ✅

#### 4.2.2 2's Complement Arithmetic ⭐⭐⭐

**Addition:** Just add the numbers (including sign bits). Ignore carry out.
**Subtraction:** A − B = A + (−B) = A + (2's complement of B)

**Example — Add −3 and −5 (8-bit):**
- −3 = 11111101, −5 = 11111011
- 11111101 + 11111011 = **1**11111000
- Ignore carry out → 11111000 = −8 ✅

**Sign Extension ⭐:**
To extend n-bit to m-bit (m>n): copy the sign bit into all new high-order bits.
- +5 in 4-bit: 0101 → 8-bit: **0000**0101
- −3 in 4-bit: 1101 → 8-bit: **1111**1101 (sign bit 1 extended)

### 4.3 Overflow Detection ⭐⭐

**Overflow when:** V = Cₙ ⊕ Cₙ₋₁ (carry into MSB ≠ carry out of MSB) = 1

**Alternative check:** Overflow occurs when:
- Adding two positive numbers gives negative result
- Adding two negative numbers gives positive result
- **NEVER overflow when adding numbers of different signs!**

**Example:** +70 + +80 in 8-bit:
- 01000110 + 01010000 = 10010110 = −106 ❌ (should be +150, but range is −128 to +127)
- V = 1 (overflow!) ⚠️

> 🎯 **GATE Formula:** V = Cₙ ⊕ Cₙ₋₁ = carry_into_MSB ⊕ carry_out_of_MSB

**Overflow for unsigned:** Overflow = Carry out of MSB (Cₙ = 1)
**Overflow for signed:** V = Cₙ ⊕ Cₙ₋₁

### 4.4 Gray Code ⭐

**Binary→Gray:** G[i] = B[i+1]⊕B[i], MSB stays.
**Gray→Binary:** B[i] = G[i]⊕B[i+1], from MSB down.
**Example:** 1011→Gray: 1, 1⊕0=1, 0⊕1=1, 1⊕1=0 → **1110**

#### 4.4.1 Gray Code — Complete Table (4-bit) ⭐

| Decimal | Binary | Gray | Decimal | Binary | Gray |
|---|---|---|---|---|---|
| 0 | 0000 | 0000 | 8 | 1000 | 1100 |
| 1 | 0001 | 0001 | 9 | 1001 | 1101 |
| 2 | 0010 | 0011 | 10 | 1010 | 1111 |
| 3 | 0011 | 0010 | 11 | 1011 | 1110 |
| 4 | 0100 | 0110 | 12 | 1100 | 1010 |
| 5 | 0101 | 0111 | 13 | 1101 | 1011 |
| 6 | 0110 | 0101 | 14 | 1110 | 1001 |
| 7 | 0111 | 0100 | 15 | 1111 | 1000 |

**Key property:** Consecutive numbers differ in exactly 1 bit → **no glitches** during transitions!

> 🏭 **Applications:** K-map ordering, rotary encoders, ADC, error-minimizing codes.

> ⚠️ **GATE Trap:** You CANNOT do arithmetic in Gray code! Convert to binary first, do arithmetic, convert back.

#### 4.4.2 Gray Code Properties ⭐

- Total n-bit Gray codes: 2ⁿ (same as binary)
- **Cyclic:** The last code and first code differ in exactly 1 bit (wraps around!)
- **Reflected:** An n-bit Gray code can be generated by reflecting (n-1)-bit code:
  1. List (n-1)-bit codes
  2. Write them again in reverse order below
  3. Prefix top half with 0, bottom half with 1

### 4.5 BCD & Excess-3

**BCD:** Each decimal digit = 4 bits. Invalid: 1010-1111.
**BCD Addition:** If sum>9 or carry, add 0110.
**Excess-3:** BCD+3. Self-complementing (complement = 9's complement).

#### 4.5.1 BCD (Binary Coded Decimal) — Detailed ⭐⭐

| Decimal | BCD | Excess-3 |
|---|---|---|
| 0 | 0000 | 0011 |
| 1 | 0001 | 0100 |
| 2 | 0010 | 0101 |
| 3 | 0011 | 0110 |
| 4 | 0100 | 0111 |
| 5 | 0101 | 1000 |
| 6 | 0110 | 1001 |
| 7 | 0111 | 1010 |
| 8 | 1000 | 1011 |
| 9 | 1001 | 1100 |

**BCD Addition Algorithm:**
1. Add two BCD digits as if they were binary
2. If result > 9 (1010 or above) OR carry out → add 0110 (correction factor)

**BCD Addition Example: 47 + 35 = 82**

```
  0100 0111    (47 in BCD)
+ 0011 0101    (35 in BCD)
-----------
  0111 1100    (binary sum)
       ↑ 1100 > 9! Add correction:
  0111 1100
+      0110
-----------
  1000 0010    (82 in BCD) ✅
```

**BCD Addition Example: 98 + 97 = 195 (3-digit result)**
```
  1001 1000    (98)
+ 1001 0111    (97)
-----------
1 0010 1111    (carry + >9 corrections needed)
```
First nibble: 1000+0111=1111 > 9 → add 0110 → 1 0101 (carry=1, digit=5)
Second nibble: 1001+1001+1(carry)= 1 0011 → carry=1, 0011 < 9 ok but carry → add 0110 → 1001
Hundreds: 1

Result: 0001 1001 0101 = **195** ✅

> 🏭 **Where is BCD used?** Financial calculations (banks, ATMs), digital clocks, calculators — anywhere exact decimal representation matters!

#### 4.5.2 Other Codes ⭐

| Code | Type | Self-complementing? | Weighted? |
|---|---|---|---|
| BCD (8421) | Weighted | No | Yes (8,4,2,1) |
| Excess-3 | Unweighted | **Yes** | No |
| 2421 | Weighted | **Yes** | Yes (2,4,2,1) |
| 8421 | Weighted | No | Yes |
| Gray | Unweighted | No | No |

**Self-complementing property:** 9's complement of number = bitwise complement of code.
- Example (Excess-3): 3₁₀ = 0110, complement = 1001 = 6₁₀ = 9-3 ✅

### 4.6 ASCII

A=65, a=97, 0=48. Upper↔Lower: toggle bit 5 (±32).

**Common ASCII values to memorize:**

| Character | Decimal | Hex | Binary |
|---|---|---|---|
| '0' | 48 | 0x30 | 0110000 |
| '9' | 57 | 0x39 | 0111001 |
| 'A' | 65 | 0x41 | 1000001 |
| 'Z' | 90 | 0x5A | 1011010 |
| 'a' | 97 | 0x61 | 1100001 |
| 'z' | 122 | 0x7A | 1111010 |
| Space | 32 | 0x20 | 0100000 |
| NULL | 0 | 0x00 | 0000000 |

> 🎯 **GATE Trick:** 'a' − 'A' = 32 = 2⁵. So to toggle case: XOR with 0x20 (flip bit 5)!

### 4.7 IEEE 754 Floating Point Representation ⭐⭐ (Cross-topic with COA)

> 💡 This topic bridges Digital Logic and COA. GATE frequently asks 1-2 mark questions on IEEE 754.

**Format:** $(-1)^S \times 1.M \times 2^{E-Bias}$ (normalized form has implicit leading 1)

| Format | Total bits | Sign | Exponent | Mantissa | Bias |
|---|---|---|---|---|---|
| **Single (float)** | 32 | 1 | 8 | 23 | 127 |
| **Double** | 64 | 1 | 11 | 52 | 1023 |
| **Half** | 16 | 1 | 5 | 10 | 15 |

**Biased Exponent:** Stored value = E + Bias. This allows representing negative exponents without a separate sign bit.

#### 4.7.1 Special Values ⭐⭐

| Exponent (E) | Mantissa (M) | Value | Description |
|---|---|---|---|
| 0 | 0 | **±0** | +0 (S=0) and −0 (S=1). Both compare equal |
| 0 | ≠ 0 | **Denormalized** | $(-1)^S × 0.M × 2^{1-Bias}$ (no implicit 1!) |
| All 1s (255/2047) | 0 | **±∞** | Result of 1/0, overflow |
| All 1s (255/2047) | ≠ 0 | **NaN** | 0/0, ∞−∞, √(−1) |
| 1 to 254 (SP) | Any | **Normalized** | Normal numbers with implicit leading 1 |

> ⚠️ **GATE Trap:** Denormalized numbers use E=1−Bias (not E=0−Bias) and NO implicit 1!

#### 4.7.2 Range Analysis (Single Precision) ⭐

**Smallest positive normalized:** E=1, M=0 → $1.0 × 2^{1-127} = 2^{-126} ≈ 1.175 × 10^{-38}$
**Largest positive normalized:** E=254, M=all 1s → $(2-2^{-23}) × 2^{127} ≈ 3.403 × 10^{38}$
**Smallest positive denormalized:** E=0, M=0...01 → $2^{-23} × 2^{-126} = 2^{-149}$

> 🎯 **GATE Formula:** For n-bit exponent with bias B:
> - Smallest normalized: $2^{1-B}$
> - Largest normalized: $(2-2^{-m}) × 2^{2^n-2-B}$ where m = mantissa bits

#### 4.7.3 Conversion Examples ⭐⭐

**Example 1: Convert −6.75₁₀ to IEEE 754 Single Precision**

Step 1: Convert to binary
−6.75 → Sign = 1 (negative)
6 = 110₂, 0.75 = 0.11₂
6.75 = 110.11₂

Step 2: Normalize
110.11 = 1.1011 × 2²

Step 3: Extract fields
- S = 1
- E = 2 + 127 = 129 = 10000001₂
- M = 1011 + (19 zeros) = 10110000000000000000000

**Result: 1 10000001 10110000000000000000000**
Hex: **0xC0D80000** ✅

**Example 2: What decimal does 0 10000010 01000000000000000000000 represent?**

- S = 0 → positive
- E = 10000010₂ = 130. Actual = 130 − 127 = 3
- M = 01 → 1.01 (add implicit 1)
- Value = +1.01₂ × 2³ = 1010₂ = **10.0** ✅

**Example 3: Convert 0.1₁₀ to IEEE 754**

0.1₁₀ in binary: 0.000110011001100110011... (repeating!)

This is why 0.1 cannot be represented exactly in floating point!
Normalized: 1.100110011... × 2⁻⁴
E = −4 + 127 = 123 = 01111011₂
The mantissa gets truncated/rounded → introduces representation error.

> ⚠️ **GATE Trap:** Not all decimals have exact binary floating-point representations. 0.1, 0.2, 0.3 are all approximate in IEEE 754!

#### 4.7.4 Floating Point Arithmetic Issues ⭐

**Rounding Modes:**
| Mode | Description | Example (round to integer) |
|---|---|---|
| Round to nearest even | Default; ties go to even | 2.5→2, 3.5→4 |
| Round toward +∞ | Ceiling | 2.1→3, −2.1→−2 |
| Round toward −∞ | Floor | 2.9→2, −2.1→−3 |
| Round toward 0 | Truncate | 2.9→2, −2.9→−2 |

**Precision Issues:**
- Single: ~7 decimal digits of precision
- Double: ~15-16 decimal digits of precision
- Addition of very different magnitudes → loss of smaller number (catastrophic cancellation)

**Special arithmetic rules:**
- (+∞) + (+∞) = +∞
- (+∞) − (+∞) = NaN
- (±∞) × 0 = NaN
- x / 0 = ±∞ (sign matches x's sign)
- 0 / 0 = NaN
- NaN ≠ NaN (NaN is not equal to itself!)

#### 4.7.5 Quick Reference Table ⭐

| Property | Single (32-bit) | Double (64-bit) |
|---|---|---|
| Exponent range | -126 to +127 | -1022 to +1023 |
| Decimal precision | ~7 digits | ~15-16 digits |
| Smallest normalized | ~1.18 × 10⁻³⁸ | ~2.23 × 10⁻³⁰⁸ |
| Largest | ~3.40 × 10³⁸ | ~1.80 × 10³⁰⁸ |
| Smallest denormalized | ~1.40 × 10⁻⁴⁵ | ~5.00 × 10⁻³²⁴ |
| ∞ representation | 0 11111111 000...0 | 0 11111111111 000...0 |
| NaN representation | 0 11111111 (M≠0) | 0 11111111111 (M≠0) |

### 4.8 Fixed Point Representation ⭐

Fixed-point places the binary point at a predetermined location.

**Q-notation:** Qm.n means m integer bits and n fractional bits (total m+n+1 with sign).

**Example:** Q3.4 in 2's complement:
- 8 total bits (1 sign + 3 integer + 4 fraction)
- Range: $-2^3$ to $2^3 - 2^{-4}$ = −8 to 7.9375
- Resolution: $2^{-4}$ = 0.0625

**Conversion:** Binary 0110.1100 in unsigned Q4.4:
= 4 + 2 + 0.5 + 0.25 = 6.75

> 🎯 **When to use Fixed vs Floating?**
> - Fixed: Faster hardware (no exponent adjustment), predictable precision. Used in DSP, embedded systems.
> - Floating: Larger dynamic range. Used in scientific computing.

### 4.9 Residue Number System (RNS) ⭐

A number is represented by its residues modulo several coprime bases (moduli).

**Example:** With moduli {3, 5, 7}:
- 15₁₀ → (15 mod 3, 15 mod 5, 15 mod 7) = (0, 0, 1)
- 23₁₀ → (23 mod 3, 23 mod 5, 23 mod 7) = (2, 3, 2)

**Range:** 0 to (3×5×7 − 1) = 0 to 104

**Advantage:** Addition and multiplication are carry-free (each digit computed independently).
**Disadvantage:** Comparison, division, and overflow detection are hard.

> 💡 This is rarely asked in GATE but understanding residues helps with modular arithmetic questions.

### 4.10 Complement Arithmetic — Deep Dive ⭐⭐

#### 4.10.1 r's Complement and (r−1)'s Complement

For base r with n digits:
- **r's complement** of N = $r^n - N$ (for N≠0, complement of 0 is 0)
- **(r−1)'s complement** of N = $r^n - 1 - N$

| Base | r's complement | (r-1)'s complement |
|---|---|---|
| Binary (r=2) | 2's complement | 1's complement |
| Decimal (r=10) | 10's complement | 9's complement |

**Example:** 9's complement of 3250 = 9999 − 3250 = **6749**
**Example:** 10's complement of 3250 = 10000 − 3250 = **6750** (or 9's complement + 1)

#### 4.10.2 Subtraction Using Complements

**A − B using r's complement:**
1. Take r's complement of B
2. Add to A: A + r's complement(B)
3. If carry out: Result is positive, discard carry
4. If no carry: Result is negative, take r's complement of sum

**Example (decimal):** 72532 − 13250 using 10's complement
10's complement of 13250 = 86750
72532 + 86750 = **1**59282 → Carry! Discard carry → **59282** ✅ (72532−13250=59282) ✅

**Example (decimal):** 13250 − 72532 using 10's complement
10's complement of 72532 = 27468
13250 + 27468 = 40718 → No carry! → Negative → take 10's complement
10's comp of 40718 = 59282 → Answer: **−59282** ✅

**Example (binary):** 1010100 − 1000011 using 2's complement (7-bit)
2's complement of 1000011 = 0111101
1010100 + 0111101 = **1**0010001 → Carry! Discard → **0010001** = 17₁₀
Check: 84 − 67 = 17 ✅

> 🎯 **GATE Trick:** For binary subtraction using 2's complement:
> - Complement all bits of subtrahend
> - Add 1
> - Add to minuend
> - If carry: positive result (discard carry)
> - If no carry: negative (complement the result)

## 5. Error Detection & Correction ⭐⭐

> 🏭 **Every download, stream, and message uses error detection!**

> 💡 **Beginner Analogy:** Imagine mailing a letter ✉️. Errors are like ink smudges that change words. Error detection is like adding a checkword at the end — if the checkword doesn't match, you know something went wrong. Error correction is like adding enough redundancy that you can figure out what the original word was, even with a smudge!

### 5.1 Hamming Distance

d(x,y) = # of differing bits = popcount(x⊕y)

| d_min | Detect | Correct |
|---|---|---|
| 2 | 1 error | 0 |
| 3 | 2 or correct 1 | 1 |
| 4 | 3 or detect 2+correct 1 | 1+detect 2 |

**Formulas:** Detect t errors: d_min ≥ t+1. Correct t: d_min ≥ 2t+1.

> 🎯 **GATE Formula:** To detect t AND simultaneously correct c errors: d_min ≥ t + c + 1 (where t > c)

#### 5.1.1 Hamming Distance — Worked Examples ⭐

**Example 1:** Find Hamming distance between 10110 and 01101.
- XOR: 10110 ⊕ 01101 = 11011
- Count 1s: 4
- d(10110, 01101) = **4** ✅

**Example 2:** A code has codewords {000, 011, 101, 110}. Find d_min.
- d(000,011) = 2, d(000,101) = 2, d(000,110) = 2
- d(011,101) = 2, d(011,110) = 2, d(101,110) = 2
- d_min = **2** → can detect 1 error, cannot correct any.

**Example 3:** How many check bits needed to correct 2-bit errors in 8-bit data?
- Need d_min ≥ 2×2+1 = 5
- With r check bits for m=8 data bits: total = m+r = 8+r
- Need 2ʳ ≥ 8+r+1 for minimum redundancy, but for d_min=5 need more specific code design.

### 5.2 Parity Bit

**Even parity:** Total 1s = even. **Odd parity:** Total 1s = odd.
Detects single errors (d_min=2). Cannot correct.

**Example:** Data = 1011001. Even parity → P = 0 (already 4 ones = even). Send: 10110010.
If receiver gets 10110110 → count 1s = 5 (odd) → **ERROR DETECTED!** ✅

> ⚠️ **Limitation:** Cannot detect EVEN number of errors (2, 4, 6 bit flips)!

**2D Parity (Row + Column parity):**
```
Data:       Row parity:
1 0 1 1     1
1 1 0 1     1
0 1 1 0     0
-----------
0 0 0 0     ← Column parity
```
- Can detect AND locate single-bit errors (correct them!)
- Can detect (but not correct) 2 or 3 bit errors
- Fails for certain 4-bit error patterns (rectangle pattern)

### 5.3 Hamming Code ⭐⭐ (SEC)

**Formula:** $2^r \geq m + r + 1$ (r parity bits for m data bits)

| Data (m) | Parity (r) | Total | Code |
|---|---|---|---|
| 4 | 3 | 7 | **(7,4)** ⭐ |
| 8 | 4 | 12 | (12,8) |
| 11 | 4 | 15 | (15,11) |
| 26 | 5 | 31 | (31,26) |
| 57 | 6 | 63 | (63,57) |

**Parity positions:** Powers of 2 (1,2,4,8,16,...)
**Each Pᵢ checks positions** whose binary has a 1 in bit position i.

**Encoding (7,4):** Positions 1,2,3,4,5,6,7 → P₁,P₂,D₃,P₄,D₅,D₆,D₇
- P₁ covers {1,3,5,7}, P₂ covers {2,3,6,7}, P₄ covers {4,5,6,7}

**Syndrome:** S = S₄S₂S₁. If S≠0 → S = error position in binary. Flip that bit!

#### 5.3.1 Hamming (7,4) — Complete Encoding Example ⭐⭐⭐

**Problem:** Encode data word D = 1011 using Hamming (7,4) code with even parity.

**Step 1:** Place data bits at non-power-of-2 positions:

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|
| Type | P₁ | P₂ | D₁ | P₄ | D₂ | D₃ | D₄ |
| Value | ? | ? | **1** | ? | **0** | **1** | **1** |

**Step 2:** Calculate parity bits (even parity):

P₁ checks positions with bit 0 set in binary: {1,3,5,7} → {P₁, 1, 0, 1} → P₁⊕1⊕0⊕1 = 0 → **P₁ = 0**

P₂ checks positions with bit 1 set: {2,3,6,7} → {P₂, 1, 1, 1} → P₂⊕1⊕1⊕1 = 0 → **P₂ = 1**

P₄ checks positions with bit 2 set: {4,5,6,7} → {P₄, 0, 1, 1} → P₄⊕0⊕1⊕1 = 0 → **P₄ = 0**

**Step 3:** Complete codeword:

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|
| Value | 0 | 1 | 1 | 0 | 0 | 1 | 1 |

**Codeword: 0110011** ✅

#### 5.3.2 Hamming (7,4) — Error Detection & Correction Example ⭐⭐⭐

**Problem:** Received codeword = 0110**1**11 (error in position 5). Detect and correct it.

**Step 1:** Compute syndrome bits:

S₁ = positions {1,3,5,7} = 0⊕1⊕**1**⊕1 = **1**
S₂ = positions {2,3,6,7} = 1⊕1⊕1⊕1 = **0**
S₄ = positions {4,5,6,7} = 0⊕**1**⊕1⊕1 = **1**

**Step 2:** Syndrome = S₄S₂S₁ = **101** = **5** in decimal

**Step 3:** Error is at position 5! Flip bit 5:

Original received: 0110**1**11 → Corrected: 0110**0**11 ✅

> 🎯 **GATE Shortcut:** Syndrome directly gives the error position in binary!

#### 5.3.3 Hamming (11,7) — Encoding Example ⭐

**Problem:** Encode data D = 1010011 using Hamming (11,7).

Need 4 parity bits: positions 1, 2, 4, 8.

| Pos | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Type | P₁ | P₂ | D₁ | P₄ | D₂ | D₃ | D₄ | P₈ | D₅ | D₆ | D₇ |
| Data | ? | ? | 1 | ? | 0 | 1 | 0 | ? | 0 | 1 | 1 |

P₁ (bit 0 set): {1,3,5,7,9,11} → P₁⊕1⊕0⊕0⊕0⊕1 = 0 → **P₁ = 0**
P₂ (bit 1 set): {2,3,6,7,10,11} → P₂⊕1⊕1⊕0⊕1⊕1 = 0 → **P₂ = 0**
P₄ (bit 2 set): {4,5,6,7} → P₄⊕0⊕1⊕0 = 0 → **P₄ = 1**
P₈ (bit 3 set): {8,9,10,11} → P₈⊕0⊕1⊕1 = 0 → **P₈ = 0**

**Codeword: 00110100011** ✅

#### 5.3.4 Hamming Code — Key Formulas ⭐⭐ (GATE Must-Know!)

**Number of parity bits (r) for m data bits:** $2^r \geq m + r + 1$

| Data bits (m) | Parity bits (r) | Total bits (n=m+r) | Code notation |
|---|---|---|---|
| 1 | 2 | 3 | (3,1) |
| 4 | 3 | 7 | (7,4) |
| 11 | 4 | 15 | (15,11) |
| 26 | 5 | 31 | (31,26) |
| 57 | 6 | 63 | (63,57) |

**Code rate:** R = m/n (efficiency). For (7,4): R = 4/7 ≈ 0.571

**Minimum distance of Hamming code:** d_min = 3 (can correct 1 error OR detect 2 errors)

**With SECDED (extra parity bit):** d_min = 4 (can correct 1 AND detect 2)

> 🎯 **GATE Formula Summary:**
> - Detect t errors: $d_{min} \geq t + 1$
> - Correct t errors: $d_{min} \geq 2t + 1$
> - Correct t and detect d errors (d>t): $d_{min} \geq t + d + 1$

#### 5.3.5 Hamming Code — Error Patterns ⭐

**What if the syndrome points to a parity bit position?**
- If syndrome = 1 (position 1): error is in P₁
- If syndrome = 2 (position 2): error is in P₂
- If syndrome = 4 (position 4): error is in P₄
- These are VALID corrections — parity bits can have errors too!

**What about 2-bit errors?**
- Standard Hamming (SEC) with 2-bit error → syndrome is non-zero but points to WRONG position
- "Correcting" it actually introduces a 3rd error! ⚠️
- Solution: Use SECDED to detect (but not correct) double errors

**What about burst errors?**
- Hamming codes are designed for random single-bit errors
- For burst errors → use CRC or interleaving + Hamming

### 5.4 SECDED

Add overall parity P₀ covering ALL bits:
- S=0, P₀ ok → No error
- S≠0, P₀ wrong → Single error (correct it)
- S≠0, P₀ ok → **Double error** (detected, uncorrectable!)

| Syndrome | Overall Parity | Meaning | Action |
|---|---|---|---|
| 0 | OK | No error | Accept |
| ≠0 | Wrong | **Single error** | Correct (flip bit at syndrome position) |
| ≠0 | OK | **Double error** | Report error (cannot correct!) |
| 0 | Wrong | Error in P₀ only | Correct P₀ |

> 🎯 **SECDED total bits:** For (7,4): Need 7+1=8 bits. For (15,11): Need 15+1=16 bits.

### 5.5 CRC (Cyclic Redundancy Check) ⭐⭐

> 🏭 **Used in Ethernet, WiFi, USB, ZIP, PNG!**

1. Append r zeros to data (r = generator degree)
2. XOR divide by generator polynomial
3. Remainder = CRC bits, append to data
4. Receiver divides by same generator → remainder 0 = no error

#### 5.5.1 CRC — Complete Worked Example ⭐⭐⭐

**Problem:** Data = 110101, Generator = 1011 (x³+x+1). Find CRC.

**Step 1:** Generator degree = 3, so append 3 zeros: 110101**000**

**Step 2:** Perform XOR (modulo-2) division:

```
          110101000 ÷ 1011
          
          1 1 0 1 0 1 0 0 0
Step 1:   1 0 1 1
          -------
          0 1 1 0 0
Step 2:     1 0 1 1
            -------
            0 1 1 1 1
Step 3:       1 0 1 1
              -------
              0 1 0 0 0
Step 4:         1 0 1 1
                -------
                0 0 1 1 0
Step 5:           0 0 0 0    (leading 0, skip or use 0)
                  -------
                  0 1 1 0 0
Step 6:             1 0 1 1
                    -------
                    0 1 1 1  ← REMAINDER
```

Wait, let me redo this more carefully:
```
      110111   ← Quotient (not needed for CRC)
     ___________
1011 | 110101000
       1011
       ----
        1101
        1011
        ----
         1100
         1011
         ----
          1111
          1011
          ----
           1000
           1011
           ----
            0110
            0000
            ----
             110  ← REMAINDER = CRC
```

**Step 3:** CRC = 110. Transmitted codeword = 110101**110**

**Step 4 — Verification:** Divide 110101110 by 1011 → remainder should be 000 ✅

#### 5.5.2 CRC Standard Polynomials ⭐

| CRC Standard | Polynomial | Degree | Used In |
|---|---|---|---|
| CRC-8 | x⁸+x²+x+1 | 8 | ATM header |
| CRC-16 | x¹⁶+x¹⁵+x²+1 | 16 | Modbus, USB |
| CRC-32 | x³²+x²⁶+x²³+x²²+x¹⁶+x¹²+x¹¹+x¹⁰+x⁸+x⁷+x⁵+x⁴+x²+x+1 | 32 | Ethernet, ZIP, PNG |
| CRC-CCITT | x¹⁶+x¹²+x⁵+1 | 16 | Bluetooth, HDLC |

**Detection capabilities of CRC with degree r:**
- All single-bit errors ✅
- All double-bit errors (if generator has ≥3 terms) ✅
- All odd-number-of-bit errors (if generator has (x+1) as factor) ✅
- All burst errors of length ≤ r ✅
- Most burst errors of length > r (probability of missing = 2⁻ʳ)

### 5.6 Checksum

Sum all data words, take 1's complement. Used in IP/TCP/UDP. Weaker than CRC.

**Internet Checksum Algorithm:**
1. Divide data into 16-bit words
2. Add all words using 1's complement addition (wrap around carries)
3. Take 1's complement of sum = checksum
4. Receiver: add all words + checksum → should get all 1s (0xFFFF)

**Example:** Data words: 0x4500, 0x003C, 0x1C46
- Sum: 0x4500 + 0x003C = 0x453C
- 0x453C + 0x1C46 = 0x6182
- Checksum = ~0x6182 = **0x9E7D**

### 5.7 Error Detection & Correction — Comparison Matrix ⭐⭐

| Method | Extra Bits | Detect | Correct | Overhead | Use Case |
|---|---|---|---|---|---|
| Simple parity | 1 bit | 1 error | 0 | Very low | Memory (basic) |
| 2D parity | r+c+1 | Burst + 3 errors | 1 error | Medium | Legacy protocols |
| Hamming SEC | $r \approx \log_2 n$ | 2 errors | 1 error | Low | ECC RAM |
| Hamming SECDED | $r+1$ | 2 errors | 1 error | Low | ECC RAM |
| CRC-r | r bits | Burst ≤ r, most > r | 0 | Low | Network, storage |
| Checksum | Fixed (16/32) | Some errors | 0 | Very low | IP/TCP/UDP |
| Reed-Solomon | 2t symbols | 2t symbols | t symbols | Higher | CD/DVD, QR codes |
| LDPC | Variable | Statistical | Statistical | High | WiFi 6, 5G |
| Turbo codes | Variable | Statistical | Statistical | High | 3G/4G |
| Convolutional | Continuous | Statistical | Statistical | Medium | Satellite, deep space |

> 💡 **GATE focuses on:** Parity, Hamming, CRC. Others are mentioned for completeness.

### 5.8 Interleaving ⭐

**Problem:** Hamming code corrects 1-bit errors. But burst errors affect consecutive bits.

**Solution:** Interleave multiple codewords so burst errors are spread across different codewords.

**Example:** 4 Hamming codewords C₁, C₂, C₃, C₄, each 7 bits:
- Without interleaving: C₁C₂C₃C₄ → burst of 4 bits destroys one codeword
- With interleaving: send bit 1 of C₁, bit 1 of C₂, bit 1 of C₃, bit 1 of C₄, bit 2 of C₁, ...
- Now a burst of 4 consecutive bits affects 1 bit in each of 4 codewords → each correctable!

**Interleaving depth d:** A burst of length ≤ d×t can be corrected (where t = correction capability per codeword).

---

# ⚡ PART 4: COMBINATIONAL CIRCUITS

## 6. Multiplexer (MUX) ⭐⭐

> 💡 **Beginner Analogy:** A MUX is like a TV remote control 📺. You have many channels (data inputs), and the select lines are the channel number you press. The output is whichever channel you selected!

### 6.1 Basics

**2ⁿ:1 MUX:** 2ⁿ data inputs, n select lines, 1 output = Σ(Iᵢ·mᵢ)

**MUX sizes and their inputs:**

| MUX Size | Data Inputs | Select Lines | Enable |
|---|---|---|---|
| 2:1 | I₀, I₁ | S₀ | Optional |
| 4:1 | I₀, I₁, I₂, I₃ | S₁, S₀ | Optional |
| 8:1 | I₀–I₇ | S₂, S₁, S₀ | Optional |
| 16:1 | I₀–I₁₅ | S₃, S₂, S₁, S₀ | Optional |

**Boolean expression for 4:1 MUX:**
$$Y = S_1'S_0'I_0 + S_1'S_0I_1 + S_1S_0'I_2 + S_1S_0I_3$$

**Boolean expression for 8:1 MUX:**
$$Y = \sum_{i=0}^{7} m_i \cdot I_i$$
where $m_i$ is the minterm of select variables for input $i$.

### 6.2 Function Implementation ⭐⭐

**Method 1 (Direct):** 2ⁿ:1 MUX for n vars — all vars as select, connect 0/1 to data inputs.

**Method 2 (n-1 select):** 2ⁿ⁻¹:1 MUX — use n-1 vars as select, remaining var determines data input:

| F(X=0) | F(X=1) | Data Input |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | X |
| 1 | 0 | X' |
| 1 | 1 | 1 |

#### 6.2.1 MUX Implementation — Complete Worked Example 1 ⭐⭐⭐

**Problem:** Implement F(A,B,C) = Σm(1,2,6,7) using an 8:1 MUX.

**Method 1 — Direct (8:1 MUX, 3 select lines):**
- Select lines: A=S₂, B=S₁, C=S₀
- Connect each data input = corresponding minterm value:

| Input | Minterm | F value | Connect to |
|---|---|---|---|
| I₀ | m₀ | 0 | **0** |
| I₁ | m₁ | 1 | **1** |
| I₂ | m₂ | 1 | **1** |
| I₃ | m₃ | 0 | **0** |
| I₄ | m₄ | 0 | **0** |
| I₅ | m₅ | 0 | **0** |
| I₆ | m₆ | 1 | **1** |
| I₇ | m₇ | 1 | **1** |

#### 6.2.2 MUX Implementation — Complete Worked Example 2 ⭐⭐⭐

**Problem:** Implement F(A,B,C) = Σm(1,2,6,7) using a 4:1 MUX.

**Method 2 — (n−1) select lines, use C as data variable:**
- Select lines: A=S₁, B=S₀
- Group minterms by AB values and determine C's role:

| A | B | AB Group Minterms | F(C=0) | F(C=1) | Data Input |
|---|---|---|---|---|---|
| 0 | 0 | m₀,m₁ | F(0,0,0)=0 | F(0,0,1)=1 | **C** |
| 0 | 1 | m₂,m₃ | F(0,1,0)=1 | F(0,1,1)=0 | **C'** |
| 1 | 0 | m₄,m₅ | F(1,0,0)=0 | F(1,0,1)=0 | **0** |
| 1 | 1 | m₆,m₇ | F(1,1,0)=1 | F(1,1,1)=1 | **1** |

**4:1 MUX connections:** I₀=C, I₁=C', I₂=0, I₃=1, S₁=A, S₀=B ✅

#### 6.2.3 MUX Implementation — 4-Variable Function Example ⭐⭐

**Problem:** Implement F(A,B,C,D) = Σm(0,1,3,4,8,9,15) using an 8:1 MUX.

- Select: A=S₂, B=S₁, C=S₀ (3 select lines for 8:1 MUX)
- Data variable: D

| A | B | C | ABC Group | F(D=0) | F(D=1) | Input |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | m₀,m₁ | 1 | 1 | **1** |
| 0 | 0 | 1 | m₂,m₃ | 0 | 1 | **D** |
| 0 | 1 | 0 | m₄,m₅ | 1 | 0 | **D'** |
| 0 | 1 | 1 | m₆,m₇ | 0 | 0 | **0** |
| 1 | 0 | 0 | m₈,m₉ | 1 | 1 | **1** |
| 1 | 0 | 1 | m₁₀,m₁₁ | 0 | 0 | **0** |
| 1 | 1 | 0 | m₁₂,m₁₃ | 0 | 0 | **0** |
| 1 | 1 | 1 | m₁₄,m₁₅ | 0 | 1 | **D** |

**8:1 MUX:** I₀=1, I₁=D, I₂=D', I₃=0, I₄=1, I₅=0, I₆=0, I₇=D ✅

> 🎯 **GATE Trick:** For 2ⁿ⁻¹:1 MUX implementation, choose the variable that appears most in the function as the data variable — often simplifies the circuit!

#### 6.2.4 Building Larger MUX from Smaller MUX ⭐⭐

**Problem:** Build a 16:1 MUX using 2-input MUX only.

**Answer:** Need **15** 2:1 MUXes (arranged in 4 levels: 8+4+2+1)

**General Formula:** To build 2ⁿ:1 MUX from 2:1 MUX → need **2ⁿ−1** MUXes.

| Target MUX | # of 2:1 MUX needed | Levels |
|---|---|---|
| 4:1 | 3 | 2 |
| 8:1 | 7 | 3 |
| 16:1 | 15 | 4 |
| 32:1 | 31 | 5 |

**Building 8:1 from 4:1 MUX + 2:1 MUX:**
- Two 4:1 MUX at first level (each handles 4 inputs)
- One 2:1 MUX at second level (selects between the two)
- Total: 2 (4:1) + 1 (2:1) = **3 MUXes**
- MSB of select → goes to final 2:1 MUX
- Lower 2 bits → go to both 4:1 MUXes

> 🎯 **GATE PYQ:** Number of 2-input MUX for 16:1 = **15** ⭐

### 6.3 MUX as Universal Gate

2:1 MUX can make NOT, AND, OR → functionally complete!

**Proof:**
- **NOT:** Connect input A to data₀, connect 0 to data₁, connect A to select → Y = A'·A + 0·... hmm, let me think...

Actually: For 2:1 MUX: Y = S'·I₀ + S·I₁
- **NOT A:** S=A, I₀=1, I₁=0 → Y = A'·1 + A·0 = A' ✅
- **AND:** S=A, I₀=0, I₁=B → Y = A'·0 + A·B = AB ✅
- **OR:** S=A, I₀=B, I₁=1 → Y = A'·B + A·1 = A'B + A = A+B ✅

Since we can make AND, OR, NOT from 2:1 MUX → **2:1 MUX is functionally complete!** ✅

### 6.4 Demultiplexer (DEMUX) ⭐

**DEMUX is the inverse of MUX** — routes one input to one of 2ⁿ outputs based on select lines.

**1:4 DEMUX truth table (with data input D):**

| S₁ | S₀ | Y₀ | Y₁ | Y₂ | Y₃ |
|---|---|---|---|---|---|
| 0 | 0 | D | 0 | 0 | 0 |
| 0 | 1 | 0 | D | 0 | 0 |
| 1 | 0 | 0 | 0 | D | 0 |
| 1 | 1 | 0 | 0 | 0 | D |

**Key insight:** A decoder with enable IS a DEMUX! The enable input acts as the data input.

| Component | Function |
|---|---|
| MUX | Many-to-one (data selector) |
| DEMUX | One-to-many (data distributor) |
| Decoder | n-to-2ⁿ (minterm generator) |
| Encoder | 2ⁿ-to-n (reverse of decoder) |

### 6.5 MUX-Based Function Generation — Advanced ⭐⭐

**Using MUX as function generator:**
- 2ⁿ:1 MUX: All n variables as select → truth table values on data inputs
- 2ⁿ⁻¹:1 MUX: n-1 variables as select → remaining variable or its complement on data inputs
- 2ⁿ⁻²:1 MUX: n-2 variables as select → expression of 2 remaining variables on data inputs

**With 2ⁿ⁻²:1 MUX (e.g., 4:1 MUX for 4 variables):**

For F(A,B,C,D) with A,B as select of 4:1 MUX, each input is a function of C,D:

| AB | Minterms | Data Input could be |
|---|---|---|
| 00 | m₀,m₁,m₂,m₃ | 0, 1, C, C', D, D', CD, CD', C'D, C'D', C⊕D, C⊙D, C+D, (C+D)', etc. |

This requires a 16-possibility lookup for each of the 4 MUX inputs, each involving 2 variables.

> 🎯 **GATE Application:** This technique is used when asking "implement using 4:1 MUX" for 4-variable functions.

## 7. Decoder & Encoder

**n:2ⁿ Decoder:** Each output = minterm. Implement any function by ORing selected outputs.
**Priority Encoder:** Multiple inputs active → outputs highest priority.

### 7.1 Decoder Tree Construction ⭐

> 🎯 **GATE PYQ:** Implement 4-to-16 decoder using 2-to-4 decoders with enable.

**Formula:** For n-to-2ⁿ decoder using k-to-2ᵏ decoders:
- Need $\frac{2^n - 1}{2^k - 1}$ decoders total (tree structure)
- **4-to-16 using 2-to-4:** 1 (top) + 4 (bottom) = **5 decoders**
- **4-to-16 using 3-to-8:** 1 (top 1-to-2) + 2 (bottom 3-to-8) = 2 decoders + 1 inverter

**Wiring:** Top decoder outputs connect to enable inputs of bottom decoders. High-order address bits → top decoder. Low-order → bottom decoders.

#### 7.1.1 Decoder as Function Generator ⭐⭐

> 💡 **Key insight:** An n:2ⁿ decoder generates ALL minterms of n variables. To implement any Boolean function, just OR the required minterms!

**Example:** Implement F(A,B,C) = Σm(1,3,5,7) using a 3:8 decoder + OR gate.
- Connect decoder inputs: A=I₂, B=I₁, C=I₀
- OR gate inputs: connect outputs D₁, D₃, D₅, D₇
- F = D₁ + D₃ + D₅ + D₇ ✅

**Implementing MULTIPLE functions with one decoder:**
A single n:2ⁿ decoder can implement ANY number of functions of the same n variables — just use different OR gates for each function!

**Example:** Using a 3:8 decoder, implement F₁ = Σm(0,1,4) AND F₂ = Σm(2,5,6,7):
- F₁ = D₀ + D₁ + D₄
- F₂ = D₂ + D₅ + D₆ + D₇

> 🏭 **This is exactly how ROM works!** The address decoder generates all minterms, and the OR array selects which ones go to each output.

#### 7.1.2 Decoder with Enable (for cascading) ⭐

**2:4 Decoder with Enable:**

| E | A₁ | A₀ | D₃ | D₂ | D₁ | D₀ |
|---|---|---|---|---|---|---|
| 0 | X | X | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 |

When E=0, all outputs are 0 (disabled). This enable input allows **cascading** decoders!

#### 7.1.3 Decoder Tree — Detailed Wiring Example ⭐⭐

**Problem:** Build 4:16 decoder from 2:4 decoders with enable.

**Structure:** 5 decoders total.
- 1 "top" decoder: inputs A₃, A₂ → 4 outputs (each enables one bottom decoder)
- 4 "bottom" decoders: each has inputs A₁, A₀ → 4 outputs each

**Wiring:**
```
Top Decoder (2:4):
  Inputs: A₃, A₂
  Enable: always 1
  Outputs: E₀, E₁, E₂, E₃

Bottom Decoder 0: Enable=E₀, Inputs=A₁,A₀ → outputs D₀,D₁,D₂,D₃
Bottom Decoder 1: Enable=E₁, Inputs=A₁,A₀ → outputs D₄,D₅,D₆,D₇
Bottom Decoder 2: Enable=E₂, Inputs=A₁,A₀ → outputs D₈,D₉,D₁₀,D₁₁
Bottom Decoder 3: Enable=E₃, Inputs=A₁,A₀ → outputs D₁₂,D₁₃,D₁₄,D₁₅
```

Only ONE bottom decoder is active at any time! ✅

#### 7.1.4 Priority Encoder — Detailed ⭐⭐

**4:2 Priority Encoder:**

| D₃ | D₂ | D₁ | D₀ | Y₁ | Y₀ | V (Valid) |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | X | X | 0 |
| 0 | 0 | 0 | 1 | 0 | 0 | 1 |
| 0 | 0 | 1 | X | 0 | 1 | 1 |
| 0 | 1 | X | X | 1 | 0 | 1 |
| 1 | X | X | X | 1 | 1 | 1 |

**Key Points:**
- D₃ has highest priority → if D₃=1, output Y=11 regardless of others
- V (valid) output indicates at least one input is active
- X = don't care — lower priority inputs are ignored when higher priority is active

**Boolean expressions:**
- Y₁ = D₃ + D₂
- Y₀ = D₃ + D₁D₂'
- V = D₀ + D₁ + D₂ + D₃

> 🏭 **Application:** Interrupt priority encoder in CPU — highest priority interrupt is serviced first!

#### 7.1.5 Encoder vs Decoder — Comparison ⭐

| Feature | Encoder | Decoder |
|---|---|---|
| Direction | 2ⁿ inputs → n outputs | n inputs → 2ⁿ outputs |
| Function | Encodes input position to binary | Decodes binary to output line |
| Active inputs | One (or priority) | All always decoded |
| Application | Keyboard encoder, priority interrupt | Address decoder, function generator |
| Implementation | OR gates | AND gates + NOT |
| Reverse of | Decoder | Encoder |

#### 7.1.6 BCD to Binary and Binary to BCD Conversion ⭐

**BCD to Binary:** Each BCD digit (4 bits) represents 0-9. Multi-digit BCD → adjust weights.

**Example:** BCD 0010 0101 = 25₁₀ → Binary = 11001₂

**Binary to BCD (Double-Dabble Algorithm):**
1. Start with binary number in a register
2. Shift left 1 bit into BCD digits
3. Before each shift: if any BCD digit ≥ 5, add 3 to that digit
4. Repeat for each bit

**Example:** Convert binary 11111 (=31) to BCD:
```
     Tens  Ones  Binary
Init   0    0    11111
Shift  0    1    1111(0)   
Check  (no digit ≥5)
Shift  0    3    111(10)
Check  (no digit ≥5)  
Shift  0    7    11(100)
Check  Ones=7≥5 → add 3: 0+3=3... wait, it's the tens.
```

Actually: Ones=7 → add 3 → 10, but that's 2 digits. Let me use the full algorithm with proper BCD columns.

This algorithm is complex to trace by hand but is used in dedicated BCD conversion hardware circuits.

#### 7.1.7 Seven-Segment Decoder as Decoder Application ⭐

The BCD-to-Seven-Segment decoder (IC 7447) is a specialized decoder:
- Input: 4-bit BCD (A₃A₂A₁A₀)
- Output: 7 segment control lines (a,b,c,d,e,f,g)

Each segment is a Boolean function of the 4 BCD inputs. For example:
- Segment 'a' (top bar): ON for digits 0,2,3,5,6,7,8,9 → OFF for 1,4
- Segment 'g' (middle bar): ON for digits 2,3,4,5,6,8,9 → OFF for 0,1,7

Display patterns:
```
 _      _  _     _  _  _  _  _
| |  | _| _||_||_ |_   ||_||_|
|_|  ||_  _|  | _||_|  ||_| _|
 0  1  2  3  4  5  6  7  8  9
```

### 7.2 NAND/NOR Only Implementation ⭐

> 🎯 **GATE PYQ:** Minimum NAND gates for XOR = A'B + AB'

**NAND-only:** Convert SOP → replace AND-OR with NAND-NAND (double inversion).
**NOR-only:** Convert POS → replace OR-AND with NOR-NOR.

**Common gate counts:**

| Function | Min NAND gates | Min NOR gates |
|---|---|---|
| NOT | 1 | 1 |
| AND (2-input) | 2 | 3 |
| OR (2-input) | 3 | 2 |
| XOR (2-input) | **4** | 5 |
| XNOR (2-input) | **5** | 4 |
| Half Adder (S+C) | 5 | — |
| Full Adder | 9 | — |

**NAND for XOR trick:** A⊕B = ((A·(A·B)')·(B·(A·B)')') — but simplified circuit uses 4 NANDs:
1. G1 = (AB)' → NAND
2. G2 = (A·G1)' → NAND
3. G3 = (B·G1)' → NAND
4. G4 = (G2·G3)' → NAND = A⊕B ✅

#### 7.2.1 Two-Level NAND-NAND Implementation ⭐⭐

**Procedure for SOP → NAND-NAND:**
1. Get minimum SOP expression
2. Replace each AND gate with NAND gate
3. Replace the OR gate with NAND gate

**Why this works:** Double inversion cancels!
- F = AB + CD (AND-OR)
- = ((AB)')' + ((CD)')' 
- = ((AB)' · (CD)')' (De Morgan's on the outer complement... actually)

Really: AND-OR = NAND-NAND because:
- Level 1: NAND gives (AB)', (CD)'
- Level 2: NAND gives ((AB)' · (CD)')' = ((AB)')' + ((CD)')' = AB + CD ✅ (De Morgan's!)

**For complemented inputs:** If the SOP has complemented variables (like A'), use an extra NAND as NOT (1 gate per complement needed).

**Example:** F = A'B + CD
- Need NOT A first: NAND(A,A) = A' → 1 NAND gate
- Level 1: NAND(A',B) = (A'B)' → 1 NAND gate
- Level 1: NAND(C,D) = (CD)' → 1 NAND gate
- Level 2: NAND((A'B)', (CD)') = A'B + CD → 1 NAND gate
- **Total: 4 NAND gates** ✅

#### 7.2.2 Two-Level NOR-NOR Implementation ⭐⭐

**Procedure for POS → NOR-NOR:**
1. Get minimum POS expression
2. Replace each OR gate with NOR gate
3. Replace the AND gate with NOR gate

**Example:** F = (A+B)(C+D)
- Level 1: NOR(A,B) = (A+B)', NOR(C,D) = (C+D)'
- Level 2: NOR((A+B)', (C+D)') = ((A+B)' + (C+D)')' = (A+B)(C+D) ✅

#### 7.2.3 Multi-Level Logic Optimization ⭐

**Two-level vs Multi-level:**

| Metric | Two-Level | Multi-Level |
|---|---|---|
| Gate delay | Minimum (2 levels) | Higher (3+ levels) |
| Gate count | Can be large (wide gates) | Often smaller |
| Fan-in | Can require high fan-in | Lower fan-in |
| Optimization | K-map, Q-M | Algebraic, technology mapping |

**Algebraic Factoring:** Reduce gate count by extracting common subexpressions.

**Example:**
F = AC + AD + BC + BD
= A(C+D) + B(C+D)  ← extract (C+D)
= (A+B)(C+D) ← further factor

Original: 4 AND + 1 OR (4-input) = 5 gates, 2 levels
Factored: 2 OR + 1 AND = 3 gates, 3 levels (but more delay!)

**Example:**
F₁ = AB + AC + AD
F₂ = AB + AE
Common: AB can be shared!
Gate count without sharing: 3 AND + 1 OR(3) + 2 AND + 1 OR(2) = 7 gates
With sharing AB: 1 AND(AB) + 2 AND + 1 OR(3) + 1 AND + 1 OR(2) = 6 gates

> 🎯 **GATE Relevance:** Multi-level is less commonly asked, but understanding factoring helps with gate counting problems.

#### 7.2.4 NAND/NOR Implementation — Gate Counting Rules ⭐⭐

**For any SOP expression with m product terms and k unique complemented variables:**
- NAND-NAND: m NAND gates (level 1) + 1 NAND gate (level 2) + k NOT gates
- If m=1: Need extra inverter at output (single product term)

**For any POS expression with m sum terms and k unique complemented variables:**
- NOR-NOR: m NOR gates (level 1) + 1 NOR gate (level 2) + k NOT gates

**Example gate count comparison:**

F(A,B,C,D) = Σm(0,1,2,3,4,5) = A'B' + A'C' + ... 
Minimum SOP: F = A' + B'C' (from K-map)

| Implementation | Gates | Total |
|---|---|---|
| AND-OR | 1 NOT(A) + 1 NOT(B) + 1 AND(B'C') + 1 OR(2-input) | 4 gates |
| NAND-NAND | 1 NAND(A,A)=A' + 1 NAND(B',C') + 1 NAND(level2) | 3 NANDs + 1 NAND(B)... |
| NOR only | Convert to POS first, then implement | varies |

> 💡 **Quick Rule:** For NAND-NAND, SOP with p product terms needs at most p+1+v NAND gates, where v = number of variables needing inversion.

## 8. Arithmetic Circuits 🧮

> 💡 **Beginner Analogy:** These are the "calculators" of the digital world 🧮. Every time your computer adds two numbers, it uses circuits made of AND, OR, and XOR gates!

### 8.1 Half Adder

**Inputs:** A, B
**Outputs:** Sum = A⊕B, Carry = AB

| A | B | Sum | Carry |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

**Gate count:** 1 XOR + 1 AND = **2 gates**

> 🏭 **Limitation:** Cannot handle carry-in from a previous stage! That's why we need a Full Adder.

### 8.2 Full Adder ⭐

**Inputs:** A, B, Cᵢₙ (carry-in)
**Outputs:** Sum = A⊕B⊕Cᵢₙ, Cₒᵤₜ = AB + (A⊕B)Cᵢₙ

| A | B | Cᵢₙ | Sum | Cₒᵤₜ |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

**Alternative Cₒᵤₜ expressions:**
- Cₒᵤₜ = AB + BCᵢₙ + ACᵢₙ (majority function) 
- Cₒᵤₜ = AB + (A⊕B)Cᵢₙ = Gᵢ + Pᵢ·Cᵢₙ (for CLA)

**Gate count:** 2 XOR + 2 AND + 1 OR = **5 gates** (or 2 half adders + 1 OR)

**Building Full Adder from Half Adders:**
```
HA1: A⊕B = P, AB = G₁
HA2: P⊕Cᵢₙ = Sum, P·Cᵢₙ = G₂
Cₒᵤₜ = G₁ + G₂ = AB + (A⊕B)Cᵢₙ
```
Full Adder = 2 Half Adders + 1 OR gate ✅

### 8.3 Ripple Carry Adder (RCA) ⭐

**Structure:** Chain n full adders. Carry output of stage i feeds carry input of stage i+1.

```
       A₃B₃    A₂B₂    A₁B₁    A₀B₀
         |        |        |        |
Cₒᵤₜ ←[FA₃]←c₃←[FA₂]←c₂←[FA₁]←c₁←[FA₀]← C₀(=0 for add)
         |        |        |        |
         S₃       S₂       S₁       S₀
```

**Delay Analysis:**
- Each Full Adder: Sum takes 2 gate delays (XOR+XOR), Carry takes 2 gate delays (AND+OR)
- **Carry propagation delay for n-bit RCA = 2n gate delays** ⭐
- **Total delay for Sum = 2n + 1 gate delays** (wait for carry, then one more XOR)
- For last bit's sum: 2(n-1) + 2 = 2n gate delays for carry chain, then +1 for last XOR

> ⚠️ **This is SLOW for large n!** A 64-bit ripple carry adder needs 128 gate delays!

### 8.4 Carry Look-Ahead Adder (CLA) ⭐⭐

**Core Idea:** Pre-compute ALL carries simultaneously using generate (G) and propagate (P) signals!

**Generate:** Gᵢ = AᵢBᵢ (carry generated regardless of carry-in)
**Propagate:** Pᵢ = Aᵢ⊕Bᵢ (carry propagated if carry-in exists)

**Carry equations (derived by substitution):**
$$C_1 = G_0 + P_0C_0$$
$$C_2 = G_1 + P_1G_0 + P_1P_0C_0$$
$$C_3 = G_2 + P_2G_1 + P_2P_1G_0 + P_2P_1P_0C_0$$
$$C_4 = G_3 + P_3G_2 + P_3P_2G_1 + P_3P_2P_1G_0 + P_3P_2P_1P_0C_0$$

**Delay: O(1) = 4 gate delays** regardless of width!
1. 1 gate delay: compute all Gᵢ and Pᵢ (AND/XOR in parallel)
2. 2 gate delays: compute all carries (AND-OR in CLA logic)
3. 1 gate delay: compute all sums (XOR with carries)
Total = **4 gate delays** for ANY width! ✅

> 🏭 **Reality check:** Pure CLA becomes impractical beyond 4 bits (fan-in gets huge). Real CPUs use **hierarchical CLA** (CLA blocks of 4 bits, with CLA logic between blocks).

| Adder Type | Carry Delay | Total Delay | Hardware Complexity |
|---|---|---|---|
| Ripple (RCA) | O(n) = 2n gates | 2n+1 | Simple (n FAs) |
| CLA (4-bit) | O(1) = 4 gates | 4 | Complex |
| Block CLA | O(log n) | ~8 for 16-bit | Moderate |
| Carry Skip | O(√n) | Better than RCA | Moderate |
| Carry Select | O(√n) | Fast | More hardware |

#### 8.4.1 Block CLA (Hierarchical CLA) ⭐

**For 16-bit addition:**
- Use 4 CLA blocks of 4 bits each
- Each block generates block-level G and P:
  - $G^* = G_3 + P_3G_2 + P_3P_2G_1 + P_3P_2P_1G_0$ (group generate)
  - $P^* = P_3P_2P_1P_0$ (group propagate)
- Second-level CLA uses G*, P* to compute block carries
- **Total delay: ~8 gate delays** (compared to 32 for 16-bit RCA!)

#### 8.4.2 Carry Select Adder ⭐

**Idea:** For each block, compute sum TWICE in parallel:
1. Once assuming carry-in = 0
2. Once assuming carry-in = 1
3. When actual carry arrives, MUX selects the correct result

**Delay:** √(2n) × t (where t is one-block delay). For n=16: ~8 gate delays.

**Trade-off:** Uses approximately 2× the hardware of RCA, but much faster.

#### 8.4.3 Carry Skip Adder ⭐

**Idea:** If P₀P₁P₂P₃=1 (all propagate), then Cₒᵤₜ = Cᵢₙ (carry ripples through entire block without generating). Skip the block!

**Delay:** O(√n). Intermediate between RCA and CLA in both speed and hardware.

### 8.5 Half Subtractor
**Diff = A⊕B, Borrow = A'B**

| A | B | Diff | Borrow |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |

### 8.6 Full Subtractor
**Diff = A⊕B⊕Bᵢₙ, Bₒᵤₜ = A'B + A'Bᵢₙ + BBᵢₙ**

Alternatively: Bₒᵤₜ = A'B + (A⊙B)Bᵢₙ = A'B + (A⊕B)'Bᵢₙ

> 🎯 **Subtraction = Addition with complement!** A−B = A + B' + 1 (controlled by XOR with mode bit M)

**Adder-Subtractor Circuit:**
- XOR each bit of B with mode signal M
- M=0 → addition (B passes through unchanged), Cᵢₙ=0
- M=1 → subtraction (B is complemented), Cᵢₙ=1 → adds 2's complement of B

### 8.7 BCD Adder ⭐

Add BCD digits using 4-bit adder. If sum>9 or carry: add 0110. Correction = Cₒᵤₜ + S₃S₂ + S₃S₁

**Circuit structure:**
```
4-bit BCD Digit A ─┐
                    ├→ [4-bit Binary Adder 1] → SUM₁ (5 bits: C₄S₃S₂S₁S₀)
4-bit BCD Digit B ─┘                              ↓
                                             [Correction Logic]
                                         Correction = C₄ + S₃S₂ + S₃S₁
                                                 ↓ (if correction needed)
              SUM₁ ─┐                            
                     ├→ [4-bit Binary Adder 2] → Final BCD Sum (4 bits)
              0110 ─┘   (adds 6 only if correction=1)
```

> 🎯 **GATE Shortcut:** BCD sum needs correction when: (1) binary sum > 9, OR (2) carry out = 1.

### 8.8 Magnitude Comparator ⭐

**1-bit comparator:**
- A>B = AB'
- A<B = A'B
- A=B = A⊙B = AB + A'B' (XNOR)

**n-bit comparator (MSB-first approach):**
Start from MSB. If bits differ → decision made. If equal → check next bit.
- (A>B) = A₃B₃' + (A₃⊙B₃)·A₂B₂' + (A₃⊙B₃)(A₂⊙B₂)·A₁B₁' + (A₃⊙B₃)(A₂⊙B₂)(A₁⊙B₁)·A₀B₀'

**IC 7485:** 4-bit magnitude comparator with cascade inputs (A>B, A=B, A<B from previous stage).

### 8.9 Binary Multiplier

n×n multiplier: Array of AND gates (partial products) + adders. Result = 2n bits.

**Structure for 4×4 multiplier:**
```
                    A₃   A₂   A₁   A₀    (Multiplicand)
                ×   B₃   B₂   B₁   B₀    (Multiplier)
                ─────────────────────────
Row 0:          A₃B₀ A₂B₀ A₁B₀ A₀B₀    (partial product 0)
Row 1:     A₃B₁ A₂B₁ A₁B₁ A₀B₁         (shifted left by 1)
Row 2: A₃B₂ A₂B₂ A₁B₂ A₀B₂              (shifted left by 2)
Row 3: A₃B₃ A₂B₃ A₁B₃ A₀B₃               (shifted left by 3)
─────────────────────────────────────────
P₇   P₆   P₅   P₄   P₃   P₂   P₁   P₀  (8-bit product)
```

**Hardware:** n² AND gates for partial products + (n-1) n-bit adders.
**Delay:** O(n) for array multiplier (can be reduced with Wallace tree to O(log n)).

#### 8.9.1 Wallace Tree Multiplier ⭐

**Problem with array multiplier:** Addition of partial products is sequential → O(n) delay.

**Wallace tree approach:**
1. Generate all partial products in parallel (1 level of AND gates)
2. Group partial products into sets of 3
3. Reduce each set using carry-save adders (CSAs) → reduces 3 numbers to 2
4. Repeat until only 2 numbers remain
5. Final stage: one fast adder (CLA) to add the last 2 numbers

**Delay:** O(log₃/₂ n) levels of CSA + 1 CLA stage = **O(log n)** total
**Example:** 8×8 multiplier generates 8 partial products:
- Stage 1: 8→ reduce to ~6 (using 2 CSAs on 2 groups of 3, 2 remain)
- Stage 2: 6→4
- Stage 3: 4→3
- Stage 4: 3→2
- Stage 5: CLA adds final 2 numbers

#### 8.9.2 Booth's Multiplication Algorithm ⭐⭐ (Cross-topic with COA)

**For signed multiplication:** Takes advantage of 0-runs and 1-runs in the multiplier.

**Recoding rules (bit pairs):** Look at current bit and previous bit (right):

| Current bit (Bᵢ) | Previous bit (Bᵢ₋₁) | Operation |
|---|---|---|
| 0 | 0 | No operation (0 in middle of 0-run) |
| 0 | 1 | Add multiplicand (end of 1-run) |
| 1 | 0 | Subtract multiplicand (start of 1-run) |
| 1 | 1 | No operation (0 in middle of 1-run) |

**Example:** Multiply 7 × 3 (0111 × 0011)
Multiplier = 0011, imaginary bit to right: 0011**0**

| Position | Bᵢ | Bᵢ₋₁ | Action |
|---|---|---|---|
| 0 | 1 | 0 | Subtract (−M) |
| 1 | 1 | 1 | No operation |
| 2 | 0 | 1 | Add (+M) |
| 3 | 0 | 0 | No operation |

Result: +M×2² − M×2⁰ = M(4−1) = 3M = 3×7 = 21 ✅

> 🎯 **GATE Relevance:** Booth's algorithm is more commonly asked in COA, but understanding the logic circuit design aspect helps in Digital Logic questions too.

### 8.10 Code Converters

BCD→Excess-3: Add 0011. Binary→Gray: Gᵢ=Bᵢ₊₁⊕Bᵢ. Gray→Binary: Bᵢ=Gᵢ⊕Bᵢ₊₁.

### 8.11 Parity Generator/Checker

Even parity: P = D₁⊕D₂⊕...⊕Dₙ. Checker: Error = D₁⊕D₂⊕...⊕Dₙ⊕P (1=error).

### 8.12 ALU (Arithmetic Logic Unit) ⭐

> 🏭 **The ALU is the computational heart of every CPU!**

**Basic ALU Operations:**

| OpCode | Operation | Description |
|---|---|---|
| 000 | A AND B | Bitwise AND |
| 001 | A OR B | Bitwise OR |
| 010 | A + B | Addition |
| 110 | A − B | Subtraction (A + B' + 1) |
| 111 | SLT | Set on Less Than |

**1-bit ALU slice** combines:
- AND gate (for A AND B)
- OR gate (for A OR B)
- Full Adder (for A+B or A-B)
- MUX to select which result to output (based on OpCode)
- Binvert input to XOR B for subtraction

**n-bit ALU:** Chain n 1-bit slices. For subtraction: set Binvert=1, CarryIn₀=1.

**Overflow Detection in ALU:**
- Set overflow = Cₙ ⊕ Cₙ₋₁ (for addition/subtraction of signed numbers)
- Also: V = overflow when both inputs same sign and result different sign

### 8.13 Iterative Circuits ⭐

**Concept:** Some combinational circuits are structured as an array of identical cells connected in series, where each cell passes information to the next.

**Examples:**
- Ripple carry adder = iterative (carry chain)
- Magnitude comparator = iterative (comparison chain MSB→LSB)
- Parity checker = iterative (XOR chain)

**Advantage:** Regular structure → easy to design and scale.
**Disadvantage:** Delay proportional to chain length O(n).

### 8.14 Seven-Segment Display Decoder ⭐

**Converts 4-bit BCD to 7 segment control signals (a-g).**

```
   a
  ---
f|   |b
  ---  ← g
e|   |c
  ---
   d
```

| Digit | a | b | c | d | e | f | g |
|---|---|---|---|---|---|---|---|
| 0 | 1 | 1 | 1 | 1 | 1 | 1 | 0 |
| 1 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| 2 | 1 | 1 | 0 | 1 | 1 | 0 | 1 |
| 3 | 1 | 1 | 1 | 1 | 0 | 0 | 1 |
| 4 | 0 | 1 | 1 | 0 | 0 | 1 | 1 |
| 5 | 1 | 0 | 1 | 1 | 0 | 1 | 1 |
| 6 | 1 | 0 | 1 | 1 | 1 | 1 | 1 |
| 7 | 1 | 1 | 1 | 0 | 0 | 0 | 0 |
| 8 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| 9 | 1 | 1 | 1 | 1 | 0 | 1 | 1 |

Inputs 10-15 (1010-1111) are don't cares in BCD → use them for K-map simplification!

Each segment (a through g) is a separate Boolean function of 4 inputs (BCD digits A,B,C,D) — a perfect K-map exercise!

**Example: Segment a**
```
         CD=00  CD=01  CD=11  CD=10
AB=00 |   1   |  0   |  1   |  1   |
AB=01 |   0   |  1   |  1   |  1   |
AB=11 |   d   |  d   |  d   |  d   |
AB=10 |   1   |  1   |  d   |  d   |
```

a = A + C + BD + B'D' (using don't cares for maximum group sizes)

## 9. Hazards in Combinational Circuits ⭐

> 🏭 **Hazards cause glitches in real circuits — must be eliminated in FPGA/ASIC design!**

> 💡 **Beginner Analogy:** Imagine a relay race 🏃. The baton (signal) travels through different paths. If one path is slower than another, there's a brief moment where the wrong answer appears — that's a glitch!

### 9.1 Types

| Type | Effect | Cause | Detection |
|---|---|---|---|
| **Static-1** | Output glitches 1→0→1 | Adjacent 1s in K-map not in same group | K-map analysis |
| **Static-0** | Output glitches 0→1→0 | Adjacent 0s in K-map not in same group | K-map of F' |
| **Dynamic** | Multiple transitions (1→0→1→0) | Multi-level circuits | Timing analysis |

### 9.2 Detection & Elimination (Static-1) ⭐

1. Draw K-map, find minimum SOP
2. Check for adjacent 1s NOT in same group → hazard!
3. **Fix: Add redundant consensus terms** to bridge those adjacent cells

**Example:** F=AB+A'C has hazard between m₃ and m₇. Fix: F=AB+A'C+**BC**

#### 9.2.1 Static-1 Hazard — Detailed Example ⭐⭐

**F(A,B,C) = Σm(1,3,5,7)** — Minimized SOP: F = C (just the variable C)

Is there a hazard? No — there's only one product term, so no transition between terms possible.

**F(A,B,C) = Σm(3,4,5,7)** — Let's minimize:

K-map:
```
      BC=00  BC=01  BC=11  BC=10
A=0 |  0   |  0   |  1   |  0   |
A=1 |  1   |  1   |  1   |  0   |
```

Groups: AB=m₄,m₅,m₇ → formed as A·B'→actually let me regroup:
- m₃(011), m₇(111) → BC (A varies) 
- m₄(100), m₅(101) → AB' (C varies)

F = BC + AB'

**Hazard check:** Consider transition from m₅(101) to m₇(111):
- m₅ is in group AB' (A=1,B=0,C=1)
- m₇ is in group BC (B=1,C=1)
- These two minterms are NOT in the same group!
- When transitioning (B changes 0→1), there's a brief moment where:
  - AB' becomes 0 (B is now 1)
  - BC becomes 1 (B is now 1)
  - During transition, both could be 0 briefly → **GLITCH!** ⚡

**Fix:** Add redundant term AC (covers m₅ and m₇ together):
F = BC + AB' + **AC** ← eliminates the hazard!

> 🎯 **Rule:** In a hazard-free SOP, every pair of adjacent 1s in the K-map must be covered by at least one common product term.

#### 9.2.2 Static-0 Hazard ⭐

**Detection:** Analyze F' using POS form. Group the 0s in the K-map.

**Elimination:** Add redundant sum terms in the POS expression.

> 💡 **Key insight:** SOP implementations can only have static-1 hazards. POS implementations can only have static-0 hazards. A circuit in SOP form is free of static-0 hazards, and vice versa!

#### 9.2.3 Hazard-Free Design Summary ⭐

| Implementation | Possible Hazards | How to Fix |
|---|---|---|
| SOP (AND-OR) | Static-1 only | Add consensus terms |
| POS (OR-AND) | Static-0 only | Add consensus sum terms |
| Multi-level | Both static + dynamic | Redesign or add delays |

> 🏭 **In practice:** Modern synthesis tools automatically handle hazards. But GATE loves asking about them!

### 9.3 Dynamic Hazards ⭐

**Dynamic hazard:** Output changes multiple times (e.g., 1→0→1→0 instead of 1→0) during a single input transition.

**Cause:** Unequal path delays in multi-level circuits where a signal and its complement travel through different numbers of gate levels.

**Example:** In a 3-level circuit:
```
A ──→ [AND] ──→ [OR] ──→ [AND] ──→ F
A ──→ [NOT] ──→ [AND] ──→ [OR] ──┘
```

If A changes 0→1, the direct path is fast but the NOT-path has extra delay. The output may see: 1→0→1→0 before settling to the final value.

**Dynamic hazard existence:** A dynamic hazard can exist ONLY if a static hazard exists. If all static hazards are eliminated, dynamic hazards cannot occur.

**Fix for dynamic hazards:**
1. Eliminate all static hazards first
2. If still present: reduce circuit to 2 levels (SOP or POS) → guaranteed no dynamic hazards
3. Or use hazard-free factored forms

> 🎯 **GATE Key Point:** Two-level circuits (SOP/POS) can have static hazards but NEVER dynamic hazards. Dynamic hazards only appear in circuits with 3+ levels.

### 9.4 Function Hazards ⭐

**Function hazard:** Inherent in the function definition — cannot be eliminated by any circuit implementation.

**Occurs when:** Multiple inputs change simultaneously, and the output is expected to remain constant, but the function value CHANGES for some intermediate input combinations.

**Example:** F(A,B) = A⊕B. If A and B change from (0,0) to (1,1):
- F(0,0) = 0, F(1,1) = 0 → output should stay 0
- But intermediate states (0,1) or (1,0) have F=1 → output may glitch to 1 briefly

**Key distinction:**
- **Logic hazard (static/dynamic):** Can be fixed by adding redundant gates
- **Function hazard:** Cannot be fixed — it's a property of the function itself
- Function hazards are NOT testable in GATE but understanding them helps with timing analysis

### 9.5 Hazard Detection — Quick Method ⭐

**For SOP:**
1. Draw K-map for F
2. Find minimum SOP cover
3. For each pair of adjacent 1-cells: check if they share a prime implicant
4. If NOT shared → hazard exists on the transition between them

**For POS:**
1. Draw K-map for F
2. Find minimum POS cover (group the 0s)
3. For each pair of adjacent 0-cells: check if they share a maxterm
4. If NOT shared → static-0 hazard exists

**Quick counting:** Minimum number of consensus terms needed = number of "unshared adjacent pairs" in the K-map.

---

# 🏗️ PART 5: PLDs & FUNCTIONAL COMPLETENESS

## 10. Functional Completeness ⭐

> 💡 **Beginner Analogy:** Think of functional completeness like a cooking set 🍳. If you have a pan, pot, and oven, you can make ANY dish. Similarly, a functionally complete set of gates can implement ANY Boolean function!

### 10.1 Universal Sets

{NAND} ✅, {NOR} ✅, {AND,NOT} ✅, {OR,NOT} ✅. But {AND,OR} ❌ (can't make NOT).

**Why are AND and OR alone not complete?**
- AND(0,0)=0, AND(0,1)=0, AND(1,0)=0, AND(1,1)=1 → both inputs 0 gives 0
- OR(0,0)=0, OR(0,1)=1, OR(1,0)=1, OR(1,1)=1 → both inputs 1 gives 1
- Starting from all 0s, you can never produce 1 without an input being 1
- Starting from all 1s, you can never produce 0 without an input being 0
- Formally: AND and OR both preserve 0 AND preserve 1, so they're in T₀∩T₁ → not complete (Post's theorem)

### 10.2 Post's Theorem ⭐⭐

S is NOT complete if ALL functions in S belong to one of:

| Class | Property | Test | Example Fn IN class | Example Fn NOT in class |
|---|---|---|---|---|
| T₀ | Preserves 0 | f(0,...,0)=0 | AND, OR, XOR | NOT, NAND, NOR |
| T₁ | Preserves 1 | f(1,...,1)=1 | AND, OR, XNOR | NOT, NAND, NOR |
| S | Self-dual | f=fᵈ | NOT, Majority | AND, OR, XOR |
| M | Monotone | x≤y ⟹ f(x)≤f(y) | AND, OR, 0, 1 | NOT, NAND, XOR |
| L | Linear | f=c₀⊕c₁x₁⊕...⊕cₙxₙ | XOR, NOT, 0, 1 | AND, OR, NAND |

**Complete iff** for EACH class, at least one function in S ∉ that class!

> 🎯 **GATE Method:** Make a 5-column table. For each function in S, mark which classes it's NOT in. If every column has at least one ✓, the set is complete!

**Quick reference:**

| Gate | ∉T₀ | ∉T₁ | ∉S | ∉M | ∉L |
|---|---|---|---|---|---|
| NAND | ✅ | ✅ | ✅ | ✅ | ✅ |
| NOR | ✅ | ✅ | ✅ | ✅ | ✅ |
| AND | | | ✅ | | ✅ |
| OR | | | ✅ | | ✅ |
| NOT | ✅ | ✅ | | ✅ | |
| XOR | | ✅ | ✅ | ✅ | |
| XNOR | ✅ | | ✅ | ✅ | |
| 0 (const) | | ✅ | | | |
| 1 (const) | ✅ | | | | |

#### 10.2.1 Post's Theorem — Worked Examples ⭐⭐⭐

**Example 1:** Is {AND, XOR} complete?

| Gate | ∉T₀ | ∉T₁ | ∉S | ∉M | ∉L |
|---|---|---|---|---|---|
| AND | | | ✅ | | ✅ |
| XOR | | ✅ | ✅ | ✅ | |

Check columns: ∉T₀ = ❌ (neither breaks T₀) → **NOT COMPLETE!** ❌

> Both AND(0,0)=0 and XOR(0,0)=0, so both preserve 0. Can never produce NOT(0)=1 from all-0 inputs!

**Example 2:** Is {OR, NOT} complete?

| Gate | ∉T₀ | ∉T₁ | ∉S | ∉M | ∉L |
|---|---|---|---|---|---|
| OR | | | ✅ | | ✅ |
| NOT | ✅ | ✅ | | ✅ | |

Check columns: ∉T₀=✅(NOT), ∉T₁=✅(NOT), ∉S=✅(OR), ∉M=✅(NOT), ∉L=✅(OR) → **COMPLETE!** ✅

**Example 3:** Is {XOR, 1} complete? (1 = constant 1 function)

| Gate | ∉T₀ | ∉T₁ | ∉S | ∉M | ∉L |
|---|---|---|---|---|---|
| XOR | | ✅ | ✅ | ✅ | |
| 1 | ✅ | | | | |

Check: ∉T₀=✅(1), ∉T₁=✅(XOR), ∉S=✅(XOR), ∉M=✅(XOR), ∉L=❌ → **NOT COMPLETE!** ❌

> XOR and 1 are both linear functions. You can only generate linear functions (XOR of subsets of variables ± constant). Cannot generate AND!

**Example 4:** Is {NAND} alone complete?

| Gate | ∉T₀ | ∉T₁ | ∉S | ∉M | ∉L |
|---|---|---|---|---|---|
| NAND | ✅ | ✅ | ✅ | ✅ | ✅ |

ALL columns ✅ → **COMPLETE!** ✅ (NAND alone breaks ALL 5 closed classes!)

#### 10.2.2 Monotone Functions Deep Dive ⭐

**Definition:** f is monotone if for inputs x ≤ y (bitwise), f(x) ≤ f(y).

**Intuition:** Changing any input from 0 to 1 can NEVER cause the output to go from 1 to 0.

**Test:** In the truth table, if you compare any two rows where one input vector dominates the other (has 1s everywhere the other does, plus maybe more), the output must not decrease.

**Examples of monotone functions:** AND, OR, constant 0, constant 1, any single variable
**NOT monotone:** NOT (0→1 but output goes 1→0), XOR, NAND, NOR

#### 10.2.3 Linear Functions Deep Dive ⭐

**Definition:** f is linear if it can be written as $f = c_0 \oplus c_1x_1 \oplus c_2x_2 \oplus ... \oplus c_nx_n$

**Examples:**
- XOR(A,B) = A⊕B → linear (c₀=0, c₁=1, c₂=1) ✅
- XNOR(A,B) = 1⊕A⊕B → linear ✅
- NOT(A) = 1⊕A → linear ✅
- AND(A,B) = AB → NOT linear ❌ (can't write as c₀⊕c₁A⊕c₂B)

**Counting:** Number of linear functions of n variables = $2^{n+1}$ (choose each cᵢ as 0 or 1)

| n | Linear functions | Total functions |
|---|---|---|
| 1 | 4 | 4 |
| 2 | 8 | 16 |
| 3 | 16 | 256 |
| 4 | 32 | 65536 |

## 11. Programmable Logic Devices

> 💡 **Beginner Analogy:** PLDs are like LEGO sets for digital circuits 🧱. ROM gives you ALL possible building blocks pre-made (every minterm). PLA lets you customize which blocks to make AND how to combine them. PAL is in between — custom blocks but fixed combining rules.

### 11.1 Comparison Table ⭐

| Feature | ROM | PLA | PAL |
|---|---|---|---|
| AND array | **Fixed** (decoder) | Programmable | Programmable |
| OR array | Programmable | Programmable | **Fixed** |
| Flexibility | Maximum | High | Medium |
| Speed | Slowest | Medium | **Fastest** |
| Minimization? | No | Yes (critical!) | Yes |
| Share products? | N/A | Yes | No |
| Cost | Depends on inputs | Cheapest for complex | Cheapest for simple |

### 11.2 ROM (Read-Only Memory) ⭐⭐

**ROM:** 2ⁿ×m — generates ALL minterms, selects for each output.

**Structure:**
```
n inputs → [n:2ⁿ Decoder] → 2ⁿ AND lines (all minterms)
                                    ↓
                              [Programmable OR array]
                                    ↓
                              m outputs (each = OR of selected minterms)
```

**Why ROM doesn't need minimization:** The decoder already generates every minterm. You just select which minterms go to each output. No simplification needed!

**ROM Size:** For n inputs and m outputs: $2^n \times m$ bits

**Types of ROM:**
| Type | Write | Erase | Use Case |
|---|---|---|---|
| Mask ROM | Factory only | Never | Mass production |
| PROM | Once (fuse) | Never | Prototyping |
| EPROM | Multiple (UV erase) | UV light | Development |
| EEPROM | Multiple (electrical) | Electrically | BIOS, firmware |
| Flash | Multiple (block erase) | Electrically | SSD, USB drive |

> 🏭 **ROM implements a truth table directly!** Each address = input combination, stored data = output values.

**Example:** Implement F₁(A,B,C) = Σm(0,2,4,6) and F₂(A,B,C) = Σm(1,3,5,7) using ROM.

ROM size needed: 2³ × 2 = **8 × 2 = 16 bits**

| Address (ABC) | F₁ | F₂ |
|---|---|---|
| 000 | 1 | 0 |
| 001 | 0 | 1 |
| 010 | 1 | 0 |
| 011 | 0 | 1 |
| 100 | 1 | 0 |
| 101 | 0 | 1 |
| 110 | 1 | 0 |
| 111 | 0 | 1 |

Note: F₁ = C', F₂ = C. But ROM stores all 16 bits regardless — no minimization benefit!

### 11.3 PLA (Programmable Logic Array) ⭐⭐

**PLA:** k product terms, programmable to any output.

**Structure:**
```
n inputs → [Programmable AND array] → k product terms
                                           ↓
                                    [Programmable OR array]
                                           ↓
                                    m outputs (each = OR of selected products)
```

**Why PLA needs minimization:** The number of product terms (AND gates) is LIMITED! You must minimize to fit within the PLA's constraints.

**PLA Size:** (2n × k) + (k × m) programmable connections

**Advantage:** Product terms can be SHARED between outputs!

**Example:** Implement using PLA with 3 inputs, 4 product terms, 2 outputs:
- F₁(A,B,C) = Σm(0,2,6,7) = A'C' + AB
- F₂(A,B,C) = Σm(2,3,6,7) = A'B + AB = B

Product terms needed: A'C', AB, A'B, B — but that's 4, and B = A'B + AB, so:
- Actually we can use: A'C', AB, B (3 product terms — B is shared concept)

Wait, let me minimize more carefully:
- F₁ = A'C' + AB (2 terms)
- F₂ = B (1 term)
- Can F₂'s term (B) help F₁? No.
- Total product terms needed: 3 (A'C', AB, B) → fits in 4-term PLA ✅

PLA programming:
| Product Term | A | B | C | F₁ | F₂ |
|---|---|---|---|---|---|
| A'C' | 0 | - | 0 | ✓ | |
| AB | 1 | 1 | - | ✓ | |
| B | - | 1 | - | | ✓ |

### 11.4 PAL (Programmable Array Logic) ⭐

**PAL:** Each output has fixed # of AND terms.

**Structure:**
```
n inputs → [Programmable AND array] → product terms (fixed # per output)
                                           ↓
                                    [Fixed OR array]
                                           ↓
                                    m outputs (each OR's its dedicated AND terms)
```

**Key difference from PLA:** Product terms CANNOT be shared between outputs!

**PAL vs PLA - When to use each:**
| Scenario | Better Choice |
|---|---|
| Many shared product terms | PLA |
| Simple functions, speed critical | PAL |
| Prototype with many I/O | PAL |
| Complex, many functions of same vars | PLA |

### 11.5 FPGA (Field-Programmable Gate Array) ⭐

> 🏭 **Modern digital design tool — used for prototyping, production, and even in data centers!**

**FPGA Architecture:**
```
[CLB][CLB][CLB]...[I/O]
[CLB][CLB][CLB]...[I/O]
  ...  Programmable    [I/O]
[CLB][CLB][CLB]...[I/O]
       Interconnect
```

**Components:**
- **CLB (Configurable Logic Block):** Contains LUTs + flip-flops
- **LUT (Look-Up Table):** k-input LUT = $2^k \times 1$ memory → can implement ANY k-input Boolean function!
- **Programmable Interconnect:** Connects CLBs to each other and I/O
- **I/O Blocks:** Interface with external pins

**LUT details:**
- 4-input LUT: 16-bit memory → any 4-input function
- 6-input LUT: 64-bit memory → any 6-input function
- Modern FPGAs use 6-input LUTs (Xilinx, Intel/Altera)

| PLD Type | Customize | Speed | Flexibility | Key Feature |
|---|---|---|---|---|
| ROM | OR array | Slow | Highest | All minterms always available |
| PLA | AND+OR | Medium | High | Shared product terms |
| PAL | AND only | Fast | Medium | Fixed OR, no sharing |
| FPGA | LUTs+routing | Variable | Very High | Reprogrammable, sequential logic |

### 11.6 PLD Size Calculations ⭐⭐ (GATE Favorite!)

**ROM size (in bits):**
$$\text{ROM size} = 2^n \times m$$
where n = address inputs, m = data output width.

**Example:** ROM with 10 address lines and 8 data lines:
Size = 2¹⁰ × 8 = 1024 × 8 = **8192 bits = 8 Kbits = 1 KB** ✅

**PLA connections:**
- AND array: $2n \times k$ connections (n inputs, each has true/complement = 2n, k product terms)
- OR array: $k \times m$ connections (k product terms, m outputs)
- Total: $2nk + km$ programmable connections

**Example:** PLA with 12 inputs, 20 product terms, 6 outputs:
- AND connections: 2×12×20 = 480
- OR connections: 20×6 = 120
- Total: **600 programmable connections** ✅

**PAL connections:**
- AND array: $2n \times k_{\text{total}}$ connections
- OR array: FIXED (not programmable)
- Total: $2n \times k_{\text{total}}$ programmable connections

| Calculation | ROM | PLA | PAL |
|---|---|---|---|
| # AND gates | $2^n$ (decoder) | k (programmable) | k (programmable) |
| # OR gates | m | m | m |
| Programmable bits | $2^n × m$ | $2nk + km$ | $2nk$ |
| Can share product terms? | N/A | Yes | No |

### 11.7 ROM Types ⭐

| Type | Full Name | Programmability | Erase Method |
|---|---|---|---|
| **Mask ROM** | Mask-programmed ROM | Factory programmed | Cannot erase |
| **PROM** | Programmable ROM | One-time programmable | Cannot erase (fuse-based) |
| **EPROM** | Erasable PROM | Reprogrammable | UV light (bulk erase) |
| **EEPROM** | Electrically Erasable PROM | Reprogrammable | Electrical (byte erase) |
| **Flash** | Flash Memory | Reprogrammable | Electrical (block erase) |

**Key distinction:**
- ROM/PROM: Non-volatile, permanent
- EPROM: Reusable but must remove from circuit to erase
- EEPROM: In-circuit erasable, byte-level
- Flash: In-circuit erasable, block-level, used in SSDs/USB drives

### 11.8 CPLD vs FPGA ⭐

| Feature | CPLD | FPGA |
|---|---|---|
| Architecture | PAL-like macrocells | LUT-based CLBs |
| Logic capacity | Small-medium | Medium-very large |
| Timing | Predictable | Variable (routing dependent) |
| Non-volatile? | Yes (flash-based) | Mostly no (SRAM-based) |
| Power-on time | Instant | Needs configuration |
| Best for | Glue logic, control | Complex algorithms, DSP |
| Typical size | 100s of macrocells | 1000s-millions of LUTs |

### 11.9 Technology Mapping to LUTs ⭐

**Problem:** Given a Boolean function, how many k-input LUTs are needed?

**4-input LUT:** Can implement ANY function of ≤4 variables.
- 4-variable function: 1 LUT
- 5-variable function: 2 LUTs (split using Shannon expansion, combine with MUX/another LUT)
- n-variable function: up to $2^{n-4}$ LUTs for n>4

**6-input LUT (modern FPGAs):** Can implement ANY function of ≤6 variables.

**Example:** How many 4-input LUTs needed for F(A,B,C,D,E) = ABC + DE?
- Split on E: F = E·(ABC+D) + E'·(ABC)
- F(E=1) = ABC+D (4 vars → 1 LUT)
- F(E=0) = ABC (3 vars → 1 LUT)
- MUX/combine with E → 1 more LUT
- **Total: 3 LUTs** (or possibly 2 with clever mapping)

---

# 🔄 PART 6: SEQUENTIAL CIRCUITS

## 12. Latches & Flip-Flops ⭐⭐

> 💡 **Beginner Analogy:** A latch is like a door 🚪 — when the enable is HIGH, data flows through (door open). A flip-flop is like a turnstile — data only passes on the clock edge (one moment). The flip-flop "captures" the data at that precise instant!

### 12.1 Latch vs FF

| Feature | Latch | Flip-Flop |
|---|---|---|
| Triggering | Level-sensitive | Edge-sensitive |
| Transparent | Yes (output follows input when enabled) | No (output changes only at edge) |
| Used in | Simple circuits, level-sensitive designs | **Synchronous design** (standard!) |
| Timing | Simpler but race-prone | setup/hold requirements |
| Notation | Q follows input while enable=1 | Q changes at ↑ or ↓ edge only |

### 12.2 SR Latch (NOR-based)

| S | R | Q | State |
|---|---|---|---|
| 0 | 0 | Q(t) | Hold (memory) |
| 0 | 1 | 0 | Reset |
| 1 | 0 | 1 | Set |
| 1 | 1 | ? | ❌ **Invalid** (Q and Q' both 0!) |

**SR Latch with Enable (Gated SR Latch):**

| E | S | R | Q |
|---|---|---|---|
| 0 | X | X | Q(t) | Hold (disabled) |
| 1 | 0 | 0 | Q(t) | Hold |
| 1 | 0 | 1 | 0 | Reset |
| 1 | 1 | 0 | 1 | Set |
| 1 | 1 | 1 | ? | Invalid |

**NAND-based SR Latch:** S̄R̄ Latch — inputs are active LOW!
- S̄=0, R̄=1 → Set
- S̄=1, R̄=0 → Reset
- S̄=1, R̄=1 → Hold  
- S̄=0, R̄=0 → **Invalid** ❌

> ⚠️ **GATE Trap:** NOR-based SR: S=R=1 is invalid. NAND-based S̄R̄: both 0 is invalid. Don't confuse!

#### 12.2.1 SR Latch — Circuit Analysis ⭐

**NOR-based SR Latch Cross-coupled Design:**
```
S ─┐
    ├→[NOR]──→ Q
R̄ ─┘     ↑
          │
S̄ ─┐     │
    ├→[NOR]──→ Q'
R ─┘
```

Wait, the correct cross-coupled NOR:
```
S ──→ [NOR₁] ──→ Q ──┐
        ↑              │
        └──────────── Q' ←── [NOR₂] ←── R
                              ↑    │
                              └────┘
```

Each NOR gate's output feeds back to the other's input.

**Timing behavior:**
1. Initially stable (S=R=0, Q holds)
2. S goes to 1: NOR₁ output (Q) → depends on S and Q'. Since S=1, Q=0... wait.

Actually: In NOR-based latch:
- Q = NOR(S, Q') → wait, this would give Q=1 only when both S and Q' are 0.

Let me use the standard circuit:
- Q = NOR(R, Q')  → Q = (R + Q')' → Q = R'·Q = R'·(Q')' = ... hmm

Standard NOR SR latch:
- Top gate: Q = (R + Q̄)' 
- Bottom gate: Q̄ = (S + Q)'

When S=1, R=0: Q̄=(1+Q)'=0, then Q=(0+0)'=1 → SET ✅
When S=0, R=1: Q=(1+Q̄)'=0, Q̄=(0+0)'=1 → RESET ✅
When S=0, R=0: Q=(0+Q̄)', Q̄=(0+Q)' → stable, holds previous value ✅
When S=1, R=1: Q=(1+Q̄)'=0, Q̄=(1+Q)'=0 → both 0 → INVALID ❌

**Race Condition:** If S and R go from (1,1) to (0,0) simultaneously → output depends on which gate is slightly faster → unpredictable! This is called a **race condition**.

### 12.3 D Latch (Transparent Latch) ⭐

**D latch eliminates the invalid state** by ensuring S and R are never both 1.

**Internal structure:** D → S, D' → R. With enable:
- When E=1: Q follows D (transparent) — "see-through" mode
- When E=0: Q holds (latched)

| E | D | Q(t+1) |
|---|---|---|
| 0 | X | Q(t) (hold) |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

**Characteristic equation (when enabled):** Q⁺ = D

**Transparency problem:** While E=1, any change in D immediately affects Q → can cause timing issues in synchronous circuits. That's why FFs (edge-triggered) are preferred!

#### 12.3.1 D Latch Timing Diagram ⭐

```
CLK:  ‾‾‾‾‾‾‾‾‾
     _|         |_
      ↑Enable    ↑Disable

D:    ___   ___
     |   |_|   |_

Q:    ___   ___
     |   |_|   |_     (Q follows D while E=1)
         ↑         
         After E→0: Q latches last value
```

**Key issues with latch-based design:**
1. **Timing sensitivity:** Data must be stable at the moment E goes low
2. **Hold time violation risk:** If D changes too quickly after E falls
3. **Race-through:** In multi-stage designs, data can "race through" multiple latches in one clock cycle

> 💡 **Solution:** Use edge-triggered flip-flops or two-phase non-overlapping clocks with latches.

### 12.4 All Flip-Flops — Characteristic Tables ⭐⭐

> 💡 **Four main FF types:** D, T, JK, SR. Each has a characteristic table (input→output), characteristic equation, and excitation table (output→input).

**D Flip-Flop (Data/Delay FF):**

**D FF:** Q⁺ = D (output captures input at clock edge)
- Most commonly used FF in modern design
- No invalid state!
- Simple and reliable

| D | Q⁺ | Meaning |
|---|---|---|
| 0 | 0 | Reset |
| 1 | 1 | Set |

**T FF:** Q⁺ = T⊕Q (toggles when T=1)

| T | Q⁺ | Meaning |
|---|---|---|
| 0 | Q | Hold |
| 1 | Q' | Toggle |

**JK FF:** Q⁺ = JQ' + K'Q (most versatile!)

| J | K | Q⁺ | Meaning |
|---|---|---|---|
| 0 | 0 | Q | Hold |
| 0 | 1 | 0 | Reset |
| 1 | 0 | 1 | Set |
| 1 | 1 | Q' | **Toggle** |

> 🎯 **JK = "improved SR"** — the invalid state SR=11 becomes useful TOGGLE in JK!

**SR FF:** Q⁺ = S + R'Q (SR=0 constraint)

| S | R | Q⁺ | Meaning |
|---|---|---|---|
| 0 | 0 | Q | Hold |
| 0 | 1 | 0 | Reset |
| 1 | 0 | 1 | Set |
| 1 | 1 | ? | ❌ Invalid |

### 12.5 Characteristic Equations Summary ⭐⭐

| FF Type | Characteristic Equation | Notes |
|---|---|---|
| **D** | Q⁺ = D | Simplest |
| **T** | Q⁺ = T⊕Q = TQ' + T'Q | Toggle on T=1 |
| **JK** | Q⁺ = JQ' + K'Q | Most versatile |
| **SR** | Q⁺ = S + R'Q, with SR=0 | Has constraint |

### 12.6 Excitation Tables ⭐⭐⭐ (MUST MEMORIZE!)

> 🎯 **Excitation tables are the REVERSE of characteristic tables:** "Given current state Q and desired next state Q⁺, what inputs are needed?"

| Q→Q⁺ | SR | D | JK | T |
|---|---|---|---|---|
| 0→0 | 00 | 0 | 0X | 0 |
| 0→1 | 10 | 1 | 1X | 1 |
| 1→0 | 01 | 0 | X1 | 1 |
| 1→1 | 00 | 1 | X0 | 0 |

> 🎯 **JK trick:** J=Q⁺ when Q=0, K=Q⁺' when Q=1, otherwise X.

**Alternative way to remember JK excitation:**
- If Q=0: J determines what Q⁺ becomes (J=0→stay 0, J=1→go to 1), K=don't care
- If Q=1: K determines what happens (K=0→stay 1, K=1→go to 0), J=don't care

**Why JK has don't cares:** The X values give more flexibility in K-map simplification → fewer gates in the combinational logic!

#### 12.6.1 Using Excitation Tables — Full Design Example ⭐⭐⭐

**Problem:** Design a synchronous sequential circuit with state table:

| Present State (Q₁Q₀) | Next State (Input=0) | Next State (Input=1) |
|---|---|---|
| 00 | 01 | 10 |
| 01 | 10 | 00 |
| 10 | 11 | 01 |
| 11 | 00 | 10 |

**Using D flip-flops:**

For D FFs: D = Q⁺ (excitation = next state directly!)

| Q₁ | Q₀ | X | Q₁⁺ | Q₀⁺ | D₁ | D₀ |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 | 0 | 1 |
| 0 | 0 | 1 | 1 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | 1 | 1 | 1 |
| 1 | 0 | 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 1 | 1 | 1 | 0 | 1 | 0 |

Now minimize D₁ and D₀ using K-maps:

**K-map for D₁(Q₁,Q₀,X):**
```
         X=0   X=1
Q₁Q₀=00 | 0  |  1 |
Q₁Q₀=01 | 1  |  0 |
Q₁Q₀=11 | 0  |  1 |
Q₁Q₀=10 | 1  |  0 |
```
D₁ = Q₁'Q₀'X + Q₁'Q₀X' + Q₁Q₀'X' + Q₁Q₀X = Q₁⊕Q₀⊕X (it's XOR/parity!)

Actually let me do the K-map properly with Gray code ordering:
```
          X=0    X=1
Q₁Q₀=00 |  0  |  1  |
Q₁Q₀=01 |  1  |  0  |
Q₁Q₀=11 |  0  |  1  |
Q₁Q₀=10 |  1  |  0  |
```

This is exactly Q₁⊕Q₀⊕X. So **D₁ = Q₁ ⊕ Q₀ ⊕ X** ✅

Similarly for D₀:
```
          X=0    X=1
Q₁Q₀=00 |  1  |  0  |
Q₁Q₀=01 |  0  |  0  |
Q₁Q₀=11 |  0  |  0  |
Q₁Q₀=10 |  1  |  1  |
```

D₀ = Q₁Q₀'X' + Q₁Q₀'X + Q₁'Q₀'X' = Q₁Q₀' + Q₁'Q₀'X'
Hmm, let me group: m₀(001 mapping)... actually: 
- 1s at: Q₁Q₀X = 000(→1), 100(→1), 101(→1)
- D₀ = Q₁Q₀' + Q₁'Q₀'X' ✅

### 12.7 FF Conversion ⭐⭐

Convert X→Y: Write Y's characteristic table, use X's excitation table for inputs.

#### 12.7.1 Conversion Examples — All Pairs

**D→T Conversion:**
Need: T FF behavior from D FF.
T FF: Q⁺ = T⊕Q. D FF: Q⁺ = D.
So: D = Q⁺ = T⊕Q → **D = T⊕Q** ✅

**D→JK Conversion:**
JK FF: Q⁺ = JQ' + K'Q. D FF: Q⁺ = D.
So: **D = JQ' + K'Q** ✅

**JK→D Conversion:**
D FF: Q⁺ = D. From JK excitation table:
- Q=0→Q⁺: J=D, K=X → Set J=D, K=D' (works for both Q=0 and Q=1 transitions)
So: **J = D, K = D'** ✅

**JK→T Conversion:**
T FF: Q⁺ = T⊕Q. From JK: Q⁺ = JQ'+K'Q.
Need J and K in terms of T:
- When Q=0: Q⁺ = J = T (from T: 0→T means J=T)
- When Q=1: Q⁺ = K' = T⊕1 means K = T⊕1⊕1 = T... 
Actually: J=T, K=T works! Because JK with J=K=T:
- T=0: J=K=0 → Hold ✅
- T=1: J=K=1 → Toggle ✅

So: **J = T, K = T** (simply connect T to both J and K!) ✅

**T→D Conversion:**
D FF: Q⁺ = D. T FF: Q⁺ = T⊕Q.
So: T = D⊕Q → **T = D ⊕ Q** ✅

**SR→D Conversion:**
S = D, R = D' ✅ (guaranteed SR=0 since D and D' are complements!)

| Conversion | Formula |
|---|---|
| D → T | D = T⊕Q |
| D → JK | D = JQ'+K'Q |
| JK → D | J=D, K=D' |
| JK → T | J=T, K=T |
| T → D | T = D⊕Q |
| SR → D | S=D, R=D' |

### 12.8 Timing ⏱️

- **Setup time tₛ:** Data must be stable BEFORE clock edge
- **Hold time tₕ:** Data must be stable AFTER clock edge
- **Clock-to-Q tᵢₓ (propagation delay):** Time from edge to output change
- **Max freq:** f = 1/(tᵢₓ + t_comb + tₛ)
- **Min clock period:** T_min = t_cq + t_comb_max + t_setup

> ⚠️ **Hold violation** (t_cq + t_comb_min < t_hold) **cannot be fixed by slowing clock!**

#### 12.8.1 Timing Analysis — Detailed Worked Example ⭐⭐

**Problem:** FF1 → combinational logic → FF2. Given:
- t_cq (FF1) = 2 ns
- t_comb (min) = 1 ns, t_comb (max) = 5 ns
- t_setup (FF2) = 1 ns
- t_hold (FF2) = 0.5 ns

**Maximum clock frequency:**
$$T_{min} = t_{cq} + t_{comb\_max} + t_{setup} = 2 + 5 + 1 = 8 \text{ ns}$$
$$f_{max} = 1/T_{min} = 1/8\text{ns} = \textbf{125 MHz}$$

**Hold time check:**
$$t_{cq} + t_{comb\_min} \geq t_{hold}$$
$$2 + 1 = 3 \geq 0.5 \text{ ✅ No hold violation!}$$

**If t_hold were 4 ns:**
$$2 + 1 = 3 < 4 \text{ ❌ HOLD VIOLATION!}$$
Fix: Add delay buffers in the data path (NOT by changing clock speed!)

#### 12.8.2 Edge Triggering Types ⭐

| Type | Trigger | Symbol |
|---|---|---|
| Positive edge-triggered | Rising edge (0→1) | ↑ (triangle at clock input) |
| Negative edge-triggered | Falling edge (1→0) | ↓ (bubble + triangle) |
| Master-Slave | Samples at one edge, outputs at other | Two latches in series |

**Master-Slave JK FF:**
- Master latch: captures input on rising edge (but doesn't output)
- Slave latch: transfers to output on falling edge
- **Potential issue:** "Ones catching" — if J or K glitches while master is transparent, wrong value captured
- Modern edge-triggered FFs use different design (don't have ones-catching problem)

#### 12.8.3 Clock Skew ⭐⭐

**Clock skew** = difference in arrival time of clock signal at different flip-flops.

**Positive skew:** Clock arrives at FF2 LATER than FF1
$$T_{min} = t_{cq} + t_{comb\_max} + t_{setup} - t_{skew}$$
$$t_{cq} + t_{comb\_min} \geq t_{hold} + t_{skew}$$

Positive skew helps setup but hurts hold!

**Negative skew:** Clock arrives at FF2 EARLIER than FF1
$$T_{min} = t_{cq} + t_{comb\_max} + t_{setup} + |t_{skew}|$$
$$t_{cq} + t_{comb\_min} \geq t_{hold} - |t_{skew}|$$

Negative skew hurts setup but helps hold!

**Example:** Given t_cq=2ns, t_comb_max=5ns, t_setup=1ns, t_skew=+1ns:
$$T_{min} = 2 + 5 + 1 - 1 = 7 \text{ ns} \rightarrow f_{max} = 142.8 \text{ MHz}$$
(Compare without skew: 125 MHz — positive skew improved frequency!)

But hold: $t_{cq} + t_{comb\_min} \geq t_{hold} + t_{skew}$ → $2+1 \geq 0.5+1 = 1.5$ → 3≥1.5 ✅

> ⚠️ **GATE Trap:** Positive clock skew can increase max frequency but may cause hold violations!

#### 12.8.4 Metastability ⭐

When setup or hold time is violated, FF may enter a **metastable state** — output oscillates or settles to unpredictable value after a random delay.

**Mean Time Between Failures (MTBF):**
$$MTBF = \frac{e^{t_r/\tau}}{T_0 \cdot f_{clk} \cdot f_{data}}$$

Where $t_r$ = resolution time, $\tau$ = time constant, $T_0$ = window of vulnerability.

**Solution: Synchronizer** — Two or more FFs in series to reduce metastability probability:
```
Async Input → [FF1] → [FF2] → Synchronized Output
                CLK      CLK
```

Each additional synchronizer stage multiplies MTBF by a large factor ($e^{T_{clk}/\tau}$).

> 🎯 **GATE Relevance:** Metastability questions appear when crossing clock domains or sampling asynchronous inputs.

#### 12.8.5 Timing Diagram Analysis ⭐⭐

**How to draw timing diagrams:**
1. Start with clock waveform
2. Mark active edges (rising/falling)
3. At each active edge, evaluate FF inputs
4. After t_cq delay, draw new output level
5. Account for combinational delay in feedback paths

**Example:** D FF with D connected to Q' (toggle behavior):

| Clock edge | D = Q' | Q (after t_cq) |
|---|---|---|
| 1st ↑ | Q=0, D=1 | Q→1 |
| 2nd ↑ | Q=1, D=0 | Q→0 |
| 3rd ↑ | Q=0, D=1 | Q→1 |
| ... | toggles | toggles |

This creates a **frequency divider** — output frequency = clock/2.

#### 12.8.6 Setup/Hold Time GATE Problems ⭐⭐

**Type 1: Find maximum clock frequency**
Given: t_cq, t_comb_max, t_setup → $f_{max} = 1/(t_{cq} + t_{comb\_max} + t_{setup})$

**Type 2: Find minimum combinational delay to avoid hold violation**
Given: t_cq, t_hold → $t_{comb\_min} \geq t_{hold} - t_{cq}$
(If result is negative, no hold constraint on combinational logic)

**Type 3: With clock skew**
- Setup: $T \geq t_{cq} + t_{comb\_max} + t_{setup} - t_{skew}$
- Hold: $t_{cq} + t_{comb\_min} \geq t_{hold} + t_{skew}$

**Type 4: Pipeline stage timing**
If pipeline has k stages with balanced delays:
$$f_{max} = \frac{1}{t_{cq} + t_{stage\_max} + t_{setup}}$$
$$\text{Throughput} = f_{max}, \text{ Latency} = k/f_{max}$$

### 12.9 Preset and Clear ⭐

**Asynchronous inputs** that override synchronous behavior:
- **Preset (PR):** Forces Q=1 regardless of clock
- **Clear (CLR):** Forces Q=0 regardless of clock
- Both active-LOW in most ICs (PR̄, CLR̄)
- **PR=CLR=0 is INVALID** (trying to set and reset simultaneously)

Used for **initialization** — set FFs to known state at power-on or reset.

## 13. Mealy & Moore Machines ⭐⭐

> 💡 **Beginner Analogy:** 
> - **Moore machine** = a vending machine with a display 🏧. The display (output) shows based on the current state ("$1.50 inserted") regardless of what you're doing right now.
> - **Mealy machine** = a calculator ➕. The output depends on what button you're pressing NOW (input) combined with what's already been entered (state).

| Feature | Mealy | Moore |
|---|---|---|
| Output depends on | State + Input | State only |
| Output on | Transitions (edges in state diagram) | States (inside state bubbles) |
| States needed | **Fewer** | More (need separate states for different outputs) |
| Output timing | Changes with input (asynchronous-ish) | Changes only at clock (synchronous) |
| Output glitches | Possible (input-dependent) | Glitch-free (state-dependent) |
| Response speed | Faster (reacts to input immediately) | Slower (delayed by 1 clock cycle) |

**Conversion:** Mealy→Moore: split states with different incoming outputs. Moore states ≤ Mealy × |outputs|.

#### 13.1 State Diagram Notation ⭐

**Moore:** Output written INSIDE state circle: (State/Output)
```
[S0/0] ---input--→ [S1/1]
```

**Mealy:** Output written ON transitions: input/output
```
[S0] ---0/0--→ [S1]
     ---1/1--→ [S2]
```

#### 13.2 Mealy ↔ Moore Conversion ⭐⭐

**Moore → Mealy:**
1. For each transition, the output = destination state's output
2. Number of states stays same or decreases

**Mealy → Moore:**
1. For each state with different outputs on incoming transitions: split into multiple states
2. Each new state gets one of the output values
3. Number of states may increase

**Example:** Convert Mealy machine:

| State | Input | Next State | Output |
|---|---|---|---|
| A | 0 | A | 0 |
| A | 1 | B | 1 |
| B | 0 | A | 1 |
| B | 1 | B | 0 |

**Convert to Moore:**
- State A: incoming outputs are 0 (from A,0) and 1 (from B,0). Different! Split into A₀ and A₁.
- State B: incoming outputs are 1 (from A,1) and 0 (from B,1). Different! Split into B₀ and B₁.

| Moore State | Output | Input 0 → | Input 1 → |
|---|---|---|---|
| A₀ (output 0) | 0 | A₀ | B₁ |
| A₁ (output 1) | 1 | A₀ | B₁ |
| B₀ (output 0) | 0 | A₁ | B₀ |
| B₁ (output 1) | 1 | A₁ | B₀ |

> 🎯 **GATE:** Moore machine's output is delayed by 1 clock compared to equivalent Mealy machine!

### 13.3 State Encoding Methods ⭐

| Encoding | FFs for N states | Description | Advantages |
|---|---|---|---|
| **Binary** | ⌈log₂N⌉ | Sequential: 0,1,2,...,N-1 | Minimum FFs |
| **Gray code** | ⌈log₂N⌉ | Adjacent states differ by 1 bit | Reduces glitches |
| **One-hot** | N | Only one FF=1 at a time | Simple next-state logic |
| **Johnson** | ⌈N/2⌉ | Twisted ring encoding | Good for some FSMs |

#### 13.3.1 One-Hot Encoding Deep Dive ⭐

In one-hot: each state uses one FF. Only ONE FF is 1 at any time.

**Example:** 4 states → 4 FFs
- S₀ = 1000, S₁ = 0100, S₂ = 0010, S₃ = 0001

**Advantages:**
- Next-state logic is very simple (usually just AND of current state and input)
- Good for FPGA implementation (FPGAs have lots of FFs but limited routing)
- Easy to decode output (just check one FF)

**Disadvantages:**
- Uses more FFs than binary (N vs log₂N)
- Not practical for many states in ASIC design

**Example:** 3-state one-hot FSM with input X:

| Current | X=0 Next | X=1 Next |
|---|---|---|
| 100 (S₀) | 010 (S₁) | 100 (S₀) |
| 010 (S₁) | 001 (S₂) | 100 (S₀) |
| 001 (S₂) | 010 (S₁) | 100 (S₀) |

Next-state equations:
- D₀ = Q₀X + Q₁X + Q₂X = X(Q₀+Q₁+Q₂) = X (since exactly one is always 1!)
- D₁ = Q₀X' + Q₂X'
- D₂ = Q₁X'

**Much simpler** than binary encoding! ✅

### 13.4 State Assignment Heuristics ⭐

**Goal:** Choose state codes to minimize combinational logic.

**Guidelines:**
1. States with the same next state should have adjacent codes (differ in 1 bit)
2. States that are next states of the same present state should have adjacent codes
3. States with the same output should have adjacent codes

> 💡 **GATE Tip:** State assignment affects circuit complexity but not the number of states or the FSM behavior. Different assignments give functionally equivalent but differently sized circuits.

### 13.5 FSM Design — Complete Methodology ⭐⭐

**Step-by-step FSM design process:**

1. **Problem specification:** Understand inputs, outputs, behavior
2. **State diagram:** Draw states, transitions, outputs
3. **State table:** Tabulate present state, inputs, next state, outputs
4. **State minimization:** Merge equivalent states
5. **State assignment:** Choose binary codes for states
6. **Choose FF type:** D (simplest), JK (most versatile), T (toggle)
7. **Derive excitation inputs:** Use excitation tables + K-maps
8. **Derive output logic:** From state table
9. **Draw circuit:** FFs + combinational logic + clock

> 🎯 **GATE typically asks steps 2-7.** Practice deriving K-maps from excitation tables quickly!

## 14. State Minimization ⭐

### 14.1 Equivalent States

Two states are equivalent if:
1. They have the **same output** (Moore) or same output for each input (Mealy)
2. Their **next states** are equivalent for all inputs

### 14.2 Implication Table Method ⭐⭐

1. Create triangular table of all state pairs
2. Mark pairs with **different outputs** as distinguishable (×)
3. For unmarked pairs: if next-state pair is marked → mark this pair
4. Repeat step 3 until no more changes (stable)
5. Merge all unmarked (equivalent) pairs

#### 14.2.1 Implication Table — Worked Example ⭐⭐⭐

**Given Moore machine:**

| State | Output | Input=0 | Input=1 |
|---|---|---|---|
| A | 0 | B | C |
| B | 0 | D | A |
| C | 1 | A | B |
| D | 0 | B | C |
| E | 1 | A | B |

**Step 1 — Initial marking (different outputs):**

```
    A   B   C   D
B | - |   |   |   |
C | × | × |   |   |    ← C has output 1, A and B have 0
D | - | - | × |   |    ← D has output 0, C has 1
E | × | × | - | × |    ← E has output 1
```
(× = different outputs = distinguishable)

**Step 2 — Check unmarked pairs:**

(A,B): A→{B,C}, B→{D,A}. Need (B,D) and (C,A) to be equivalent.
- (B,D): B→{D,A}, D→{B,C}. Need (D,B)=same as (B,D) and (A,C)=×. Since (A,C)=× → **(B,D) is ×!** → **(A,B) depends on (B,D)=×** → **Mark (A,B) as ×!**

Wait, let me redo. (A,B): Next states are (B,C) for input 0 and (A,D) for input 0... let me re-read.

(A,B): input=0: A→B, B→D → need (B,D) equiv. input=1: A→C, B→A → need (C,A) equiv.
- (C,A) is marked ×. So **(A,B) is ×** ✅

(A,D): input=0: A→B, D→B → same! input=1: A→C, D→C → same!
- **(A,D) is EQUIVALENT!** ✅ (next states identical!)

(B,D): input=0: B→D, D→B → need (D,B) equiv = need (B,D) equiv → circular dependency
- input=1: B→A, D→C → need (A,C) → × !
- **(B,D) is ×** ✅

(C,E): input=0: C→A, E→A → same! input=1: C→B, E→B → same!
- **(C,E) is EQUIVALENT!** ✅

**Step 3 — Result:** Merge A≡D and C≡E.
- Reduced machine: 3 states {A(=D), B, C(=E)}

| State | Output | Input=0 | Input=1 |
|---|---|---|---|
| A | 0 | B | C |
| B | 0 | A | A |
| C | 1 | A | B |

Reduced from 5 states to **3 states**! ✅

## 15. Sequence Detectors ⭐⭐

> 💡 **Beginner Analogy:** A sequence detector is like a password lock 🔒. It watches a stream of inputs and "unlocks" (outputs 1) only when the correct sequence appears. After detecting, it may reset (non-overlapping) or keep watching from a partial match (overlapping).

### 15.1 Design Steps

1. Define pattern (e.g., "1011")
2. Choose: Mealy/Moore, overlapping/non-overlapping
3. States = prefixes matched
4. On correct input → advance. On wrong → go to longest matching suffix (overlapping) or initial (non-overlapping)
5. Build state table → state assignment → derive FF inputs via excitation tables + K-maps

#### 15.1.1 Overlapping vs Non-overlapping ⭐

| Type | After Detection | Example: Detect "101" in 10101 |
|---|---|---|
| **Overlapping** | Keep prefix that could start new match | Detects TWICE: 1**101**01 and 101**01** — wait  101**01** |
| **Non-overlapping** | Reset to initial state | Detects ONCE: **101**01 |

> For detecting "101" in input stream "1010101":
> - Overlapping: detects at positions 3, 5, 7 (every other bit after first match)
> - Non-overlapping: detects at positions 3, 6

#### 15.1.2 Detect "110" — Mealy, Overlapping ⭐⭐

**States:**
- S₀: No prefix matched (initial)
- S₁: "1" matched
- S₂: "11" matched

| Present State | Input=0 | Input=1 |
|---|---|---|
| S₀ | S₀/0 | S₁/0 |
| S₁ | S₀/0 | S₂/0 |
| S₂ | S₀/**1** ⭐ | S₂/0 |

At S₂+input 0: "110" detected! Output=1.
At S₂+input 1: still have "11" prefix → stay in S₂.

#### 15.1.3 Detect "1011" — Mealy, Overlapping — Full Design ⭐⭐⭐

**States:**
- S₀: No match
- S₁: "1" matched
- S₂: "10" matched
- S₃: "101" matched

**State Transition Table:**

| State | Input=0 | Output | Input=1 | Output |
|---|---|---|---|---|
| S₀ | S₀ | 0 | S₁ | 0 |
| S₁ | S₂ | 0 | S₁ | 0 |
| S₂ | S₀ | 0 | S₃ | 0 |
| S₃ | S₂ | 0 | S₁ | **1** ⭐ |

**Key transitions explained:**
- S₃ + input 1 → "1011" detected! Output=1. Next state = S₁ (the last "1" could be start of new "1011")
- S₃ + input 0 → "1010" → the "10" at end is prefix of "1011" → go to S₂
- S₁ + input 1 → "11" → only last "1" matters → stay at S₁
- S₂ + input 0 → "100" → no useful prefix → back to S₀

**State Assignment:** S₀=00, S₁=01, S₂=10, S₃=11

**Using D Flip-Flops (D = Q⁺):**

| Q₁ | Q₀ | X | Q₁⁺ | Q₀⁺ | D₁ | D₀ | Z |
|---|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 | 1 | 0 | 1 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 1 | 1 | 1 | 0 |
| 1 | 1 | 0 | 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 1 | 0 | 1 | 0 | 1 | **1** |

**K-map minimization:**
- D₁ = Q₁Q₀'X + Q₁'Q₀X' + Q₁Q₀X' = Q₀X' + Q₁Q₀'X... let me do properly:

K-map for D₁(Q₁,Q₀,X):
```
         X=0   X=1
Q₁Q₀=00 | 0  | 0 |
Q₁Q₀=01 | 1  | 0 |
Q₁Q₀=11 | 1  | 0 |
Q₁Q₀=10 | 0  | 1 |
```
D₁ = Q₀X' + Q₁Q₀'X

K-map for D₀(Q₁,Q₀,X):
```
         X=0   X=1
Q₁Q₀=00 | 0  | 1 |
Q₁Q₀=01 | 0  | 1 |
Q₁Q₀=11 | 0  | 1 |
Q₁Q₀=10 | 0  | 1 |
```
D₀ = X (always!)

Output Z: Z = Q₁Q₀X (only 1 when state=11 and input=1)

**Final circuit:**
- D₁ = Q₀X' + Q₁Q₀'X
- D₀ = X
- Z = Q₁Q₀X

#### 15.1.4 Detect "1011" — Moore Machine Version ⭐⭐

For Moore: need 5 states (one extra for detection output):
- S₀(out=0): No match
- S₁(out=0): "1"
- S₂(out=0): "10"
- S₃(out=0): "101"
- S₄(out=1): "1011" detected

| State/Output | Input=0 | Input=1 |
|---|---|---|
| S₀/0 | S₀ | S₁ |
| S₁/0 | S₂ | S₁ |
| S₂/0 | S₀ | S₃ |
| S₃/0 | S₂ | S₄ |
| **S₄/1** | S₂ | S₁ |

> 🎯 **Key difference:** Moore needs 5 states vs Mealy's 4 states. Moore output appears 1 clock later.

## 16. Registers & LFSR

> 💡 **Registers are groups of flip-flops that store multi-bit data.**

### 16.1 Shift Register Types ⭐

| Type | Input | Output | Application |
|---|---|---|---|
| **SISO** (Serial In Serial Out) | Serial | Serial | Delay line |
| **SIPO** (Serial In Parallel Out) | Serial | Parallel | Serial→Parallel conversion |
| **PISO** (Parallel In Serial Out) | Parallel | Serial | Parallel→Serial conversion |
| **PIPO** (Parallel In Parallel Out) | Parallel | Parallel | Buffer/storage |
| **Universal** | Both | Both | All operations |

**Shift directions:** Left shift (multiply by 2), right shift (divide by 2), bidirectional.

**Universal Shift Register has select inputs:**

| S₁ | S₀ | Operation |
|---|---|---|
| 0 | 0 | No change (hold) |
| 0 | 1 | Shift right |
| 1 | 0 | Shift left |
| 1 | 1 | Parallel load |

#### 16.1.1 Shift Register Applications ⭐

| Application | Register Type | How It Works |
|---|---|---|
| **Serial communication** | PISO (transmit), SIPO (receive) | Parallel data → serial bits → parallel data |
| **Delay line** | SISO | Data delayed by n clock cycles |
| **Ring counter** | SISO with feedback | Circular shift of one-hot pattern |
| **Sequence generator** | SISO with feedback | LFSR generates pseudo-random sequence |
| **Multiplication by 2ᵏ** | Left shift k times | Shift register with k clock pulses |
| **Division by 2ᵏ** | Right shift k times | Arithmetic shift preserves sign |
| **Serial-parallel conversion** | SIPO | Deserializer (UART receive) |
| **Parallel-serial conversion** | PISO | Serializer (UART transmit) |

#### 16.1.2 Shift Register as a Delay Element ⭐

An n-bit SISO shift register delays the input by n clock cycles.

```
Input → [D-FF₁] → [D-FF₂] → [D-FF₃] → ... → [D-FFₙ] → Output
          CLK        CLK        CLK              CLK
```

Data entering at clock t appears at output at clock t+n.

**Application:** FIR filter implementation in DSP, where delayed versions of the signal are needed.

#### 16.1.3 Bidirectional Shift Register ⭐

Each FF needs a MUX at its input to select between:
- Shift right: data from left neighbor
- Shift left: data from right neighbor
- Parallel load: external data input
- Hold: current value (feedback from own output)

**IC 74194:** 4-bit bidirectional universal shift register with above 4 modes controlled by S₁S₀.

### 16.2 LFSR (Linear Feedback Shift Register) ⭐

> 🏭 **Used in CRC computation, BIST, stream ciphers, pseudo-random number generation!**

**Structure:** XOR of selected FF outputs fed back to input.

**Max length n-bit LFSR: 2ⁿ−1 states** (all states except all-0s).

**Example — 3-bit LFSR with feedback from positions 2 and 3 (XOR):**
```
D_in = Q₂ ⊕ Q₃  →  [Q₁] → [Q₂] → [Q₃]
```

If initial state = 001:
001 → 100 → 110 → 111 → 011 → 101 → 010 → 001 (repeats)
= 7 states = 2³−1 ✅ (maximum length sequence!)

**Primitive polynomial:** A feedback polynomial that generates the maximum length sequence. For n=3: x³+x+1 or x³+x²+1.

#### 16.2.1 LFSR as Pseudo-Random Number Generator ⭐

The output sequence of a max-length LFSR has properties resembling random noise:
- **Balance:** In one period of 2ⁿ−1, the number of 1s exceeds 0s by exactly 1
- **Run-length distribution:** Half the runs have length 1, quarter have length 2, etc.
- **Shift-and-add property:** XOR of sequence with shifted version gives another shift

#### 16.2.2 Primitive Polynomials for Common n ⭐

| n (bits) | Max period (2ⁿ−1) | Primitive Polynomial |
|---|---|---|
| 2 | 3 | x²+x+1 |
| 3 | 7 | x³+x+1 |
| 4 | 15 | x⁴+x+1 |
| 5 | 31 | x⁵+x²+1 |
| 8 | 255 | x⁸+x⁴+x³+x²+1 |
| 16 | 65535 | x¹⁶+x¹²+x³+x+1 |
| 32 | ~4.3×10⁹ | x³²+x²²+x²+x+1 |

**CRC connection:** CRC generators ARE LFSRs! The generator polynomial determines the feedback taps.

#### 16.2.3 LFSR Types ⭐

**Fibonacci LFSR (external feedback):**
```
  ┌──XOR──┐
  │       │
Input → [FF₁] → [FF₂] → [FF₃] → [FF₄] → Output
           └─────────────→ XOR
```
Feedback from output of selected FFs → XOR → input of first FF.

**Galois LFSR (internal feedback):**
```
[FF₁] → XOR → [FF₂] → [FF₃] → XOR → [FF₄] → Output
  ↑──────────────────────────────────────────┘
```
Output fed back to XOR gates between selected FFs.

**Both types generate same sequence** (but in different order). Galois is faster in hardware (shorter critical path through single XOR vs chained XOR).

#### 16.2.4 BIST (Built-In Self-Test) using LFSR ⭐

In chip testing:
- LFSR generates pseudo-random test patterns → applied to Circuit Under Test (CUT)
- Signature Analyzer (another LFSR) compresses outputs
- Final signature compared with expected → pass/fail

This reduces the need for expensive external test equipment!

## 17. Counters 🔢

> 💡 **Counters are sequential circuits that go through a specific sequence of states.**

### 17.1 Asynchronous (Ripple) Counter ⭐

Each FF clocked by previous output. Delay = n×tₚ. Max freq = 1/(n×tₚ). Glitch-prone.

**Structure (4-bit ripple counter):**
```
CLK → [T-FF₀] → Q₀ → CLK[T-FF₁] → Q₁ → CLK[T-FF₂] → Q₂ → CLK[T-FF₃] → Q₃
       T=1           T=1              T=1              T=1
```

**Key Issues:**
- **Propagation delay accumulates:** Each FF adds its delay before triggering the next
- **Glitching:** Intermediate states appear briefly during transition
- **Not suitable for synchronous systems** (FFs aren't aligned to same clock)

**Max frequency:** $f_{max} = \frac{1}{n \times t_p}$ where n = number of FFs, tₚ = FF propagation delay

**Example:** 4-bit ripple counter, tₚ = 10ns per FF
- Total delay = 4 × 10 = 40 ns
- f_max = 1/40ns = **25 MHz**
- For comparison, synchronous counter with same FFs: f_max = 1/10ns = **100 MHz**!

### 17.2 Synchronous Counter ⭐

All FFs share clock. No accumulated delay. Need combinational logic for state transitions.

**4-bit Synchronous Up Counter using T FFs:**
- T₀ = 1 (always toggle — LSB toggles every clock)
- T₁ = Q₀ (toggle when Q₀=1)
- T₂ = Q₀Q₁ (toggle when Q₀=Q₁=1)
- T₃ = Q₀Q₁Q₂ (toggle when Q₀=Q₁=Q₂=1)

**General pattern:** Tᵢ = Q₀ · Q₁ · ... · Qᵢ₋₁

**4-bit Synchronous Up Counter using JK FFs:**
- J₀ = K₀ = 1
- J₁ = K₁ = Q₀
- J₂ = K₂ = Q₀Q₁
- J₃ = K₃ = Q₀Q₁Q₂

### 17.3 Mod-N Counter Design ⭐⭐

**FFs required:** $\lceil \log_2 N \rceil$

**Example: Mod-6 Counter (counts 0-5, then resets)**
- FFs needed: ⌈log₂6⌉ = 3 (Q₂Q₁Q₀)
- States: 000→001→010→011→100→101→(000 reset)

**Method 1 — Detect state 6 and reset:**
When Q₂Q₁Q₀ = 110 (=6), force reset to 000.
Reset = Q₂Q₁ (since we never reach states 110, 111 in normal operation, Q₂Q₁=1 only at state 6)

**Method 2 — Design from state table using excitation tables** (more reliable, no glitches)

#### 17.3.1 Mod-N Counter — Complete Design Example ⭐⭐⭐

**Problem:** Design a Mod-5 synchronous counter (states 0,1,2,3,4) using JK flip-flops.

**State Table:**

| Q₂ | Q₁ | Q₀ | Q₂⁺ | Q₁⁺ | Q₀⁺ |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 | 1 | 1 |
| 0 | 1 | 1 | 1 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | d | d | d |
| 1 | 1 | 0 | d | d | d |
| 1 | 1 | 1 | d | d | d |

(States 5,6,7 are don't cares since they never occur)

**JK Excitation:**

| Q₂Q₁Q₀ | Q₂→Q₂⁺ | J₂K₂ | Q₁→Q₁⁺ | J₁K₁ | Q₀→Q₀⁺ | J₀K₀ |
|---|---|---|---|---|---|---|
| 000 | 0→0 | 0X | 0→0 | 0X | 0→1 | 1X |
| 001 | 0→0 | 0X | 0→1 | 1X | 1→0 | X1 |
| 010 | 0→0 | 0X | 1→1 | X0 | 0→1 | 1X |
| 011 | 0→1 | 1X | 1→0 | X1 | 1→0 | X1 |
| 100 | 1→0 | X1 | 0→0 | 0X | 0→0 | 0X |
| 101 | d | dd | d | dd | d | dd |
| 110 | d | dd | d | dd | d | dd |
| 111 | d | dd | d | dd | d | dd |

**K-map minimization:**

J₂ K-map (entries: 0,0,0,1,d,d,d,d): J₂ = Q₁Q₀
K₂ K-map (entries: X,X,X,X,1,d,d,d): K₂ = 1 (or any — all are 1 or don't care)
J₁ K-map: J₁ = Q₀
K₁ K-map: K₁ = Q₀
J₀ K-map: J₀ = Q₂' (or Q₁' — depends on grouping with don't cares)
K₀ K-map: K₀ = 1

> 💡 The don't cares from unused states make the logic simpler!

### 17.4 Special Counters ⭐⭐

| Counter | FFs for Mod-N | Unique States | Self-starting? |
|---|---|---|---|
| **Binary** | ⌈log₂N⌉ | N | Depends on design |
| **Ring** | **N** | N | No (needs init) |
| **Johnson** | **N/2** | 2N | No (needs init) |

#### 17.4.1 Ring Counter ⭐

**Structure:** Shift register with output fed back to input (NO inversion).

**Operation (4-bit):** 1000 → 0100 → 0010 → 0001 → 1000 (repeat)
- Only ONE bit is 1 at any time (**one-hot encoding**)
- Mod-4 counter using 4 FFs (wasteful! Binary counter needs only 2 FFs)
- **N states using N flip-flops**

**State decoding:** No extra logic needed! Each FF directly represents a state.

**Problem:** Not self-starting. If initialized to invalid state (e.g., 1100), stays in wrong cycle.
**Fix:** Add feedback logic to self-correct.

#### 17.4.2 Johnson (Twisted Ring) Counter ⭐⭐

**Structure:** Shift register with **COMPLEMENTED** output fed back to input.

**Operation (3-bit):**
000 → 100 → 110 → 111 → 011 → 001 → 000 (repeat)

| Clock | Q₂ | Q₁ | Q₀ | State# |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 1 | 1 | 0 | 0 | 1 |
| 2 | 1 | 1 | 0 | 2 |
| 3 | 1 | 1 | 1 | 3 |
| 4 | 0 | 1 | 1 | 4 |
| 5 | 0 | 0 | 1 | 5 |
| 6 | 0 | 0 | 0 | 0 (repeat) |

**Mod-2N counter using N flip-flops!** (3 FFs → Mod-6 = 6 states)

**State decoding:** Each state decoded by just 2 variables (adjacent pair):

| State | Decode Logic |
|---|---|
| 0 (000) | Q₂'Q₀' |
| 1 (100) | Q₂Q₁' |
| 2 (110) | Q₁Q₀' |
| 3 (111) | Q₁Q₀ |
| 4 (011) | Q₂'Q₁ |
| 5 (001) | Q₂'Q₀ |

> 🎯 **Advantage over ring:** Uses only N/2 FFs for same number of states! And decoding uses only 2-input AND gates (simpler than binary counter decoding).

### 17.5 Frequency Division ⭐

T-FF: f_out = f_in/2. Mod-N counter: f_out = f_in/N.

**Example:** Design a circuit to divide 1 MHz clock by 6.
- Use Mod-6 counter
- Output = MSB (toggles at f/6 approximately, but may not be 50% duty cycle)
- For exact 50% duty cycle at f/6: more complex design needed

**Prescaler cascade:** Divide by 12 = divide by 4 (Mod-4) → then divide by 3 (Mod-3).

**Frequency Division Summary:**

| Source | Divider | Output Frequency | Circuit |
|---|---|---|---|
| f | 1 T-FF | f/2 | Single toggle flip-flop |
| f | 2 cascaded T-FFs | f/4 | Two T-FFs in series |
| f | n cascaded T-FFs | f/2ⁿ | n T-FFs in series |
| f | Mod-N counter | f/N | N-state counter |
| f | Johnson (n FFs) | f/2n | n-FF twisted ring |
| f | Ring (n FFs) | f/n | n-FF ring counter |

> 🎯 **GATE Trick:** If question asks "output frequency of bit k in an n-bit binary counter clocked at f," answer = f/2^(k+1) (since bit k toggles every 2^(k+1) clocks).
>
> Example: In a 4-bit counter with f_clk = 16 MHz, Q₂ frequency = 16/2³ = 2 MHz.

### 17.6 Up/Down Counter ⭐

**Bidirectional counter:** Count up or down based on control signal.

**Control signal U/D:**
- U/D = 1: Count up (0→1→2→...→N-1→0)
- U/D = 0: Count down (N-1→N-2→...→1→0→N-1)

**Implementation:** Use MUX to select between up-count and down-count logic for each FF input.

## 18. ASM Charts ⭐

> 💡 **ASM = Algorithmic State Machine — a more structured way to describe sequential circuits than state diagrams.**

**Components:**
- **State box** (rectangle): Contains state name and Moore outputs. ONE entry, ONE exit.
- **Decision box** (diamond): Tests an input condition. ONE entry, TWO exits (T/F).
- **Conditional output box** (oval): Mealy outputs (depend on input during state).

**Rules:**
- One ASM block = **one clock cycle** (all state + decision + conditional output boxes)
- Each ASM block has exactly ONE state box
- Decision boxes and conditional output boxes are optional

**Example ASM block:**
```
┌─────────────┐
│   State S₀  │  ← State box (Moore output: Z=0)
│   Z = 0     │
└──────┬──────┘
       │
   ╱───┴───╲
  ╱  X = 1?  ╲   ← Decision box
  ╲         ╱
   ╲───┬───╱
   0   │   1
   │   │
   │  ┌┴────────┐
   │  │  Y = 1  │  ← Conditional output (Mealy: Y=1 only when X=1 in S₀)
   │  └─────────┘
   │       │
   ↓       ↓
  S₁      S₂       ← Next states
```

**Advantages over state diagrams:** Better for complex controllers, easier to translate to HDL (Verilog/VHDL), clearly shows one-clock-cycle boundary.

### 18.1 ASM Chart Design Example ⭐

**Problem:** Design an ASM chart for a serial adder that adds two n-bit numbers one bit at a time.

**States:**
- S₀: IDLE (wait for start signal)
- S₁: ADD (add current bits + carry, shift)

**ASM Chart:**
```
┌──────────────┐
│    S₀/IDLE   │  Output: Done=1
│    Done = 1  │
└──────┬───────┘
       │
   ╱───┴───╲
  ╱  Start?  ╲
  ╲         ╱
   ╲───┬───╱
   0   │   1
   │   │
   ↓   ↓
  S₀  ┌──────────────┐
      │   S₁/ADD      │  Output: Done=0, Shift=1
      │  Done=0       │
      │  Shift=1      │
      └──────┬────────┘
             │
         ╱───┴───╲
        ╱ Count=n? ╲
        ╲          ╱
         ╲───┬───╱
         0   │   1
         │   │
         ↓   ↓
        S₁   S₀
```

---

# 📚 PRACTICE PROBLEMS — GATE Level

## Practice Set 1: Boolean Algebra & Simplification

### P1. (1 mark) Simplify using Boolean algebra:
**F = A'B'C + A'BC + AB'C + ABC**

**Solution:**
F = A'C(B'+B) + AC(B'+B) = A'C + AC = C(A'+A) = **C** ✅

### P2. (1 mark) Find the complement of:
**F = AB + CD' + A'D**

**Solution:**
F' = (AB)'·(CD')'·(A'D)' = (A'+B')(C'+D)(A+D') ✅

### P3. (2 marks) How many Boolean functions of 3 variables have the property F(0,0,0) = 0 and F(1,1,1) = 1?

**Solution:**
Total truth table has 2³=8 rows. Two rows are fixed (row 0 output=0, row 7 output=1).
Remaining 6 rows can be 0 or 1 freely.
Answer: $2^6 = \textbf{64}$ ✅

### P4. (1 mark) The dual of F = A'B + AB' + A'B' is?

**Solution:**
Dual: swap AND↔OR, 0↔1, DON'T complement variables.
F = A'B + AB' + A'B'
Fᵈ = (A'+B)(A+B')(A'+B') ✅

### P5. (2 marks) Simplify F(A,B,C,D) = Σm(0,1,4,5,7,8,9,12,13,15) using K-map.

**Solution:**
```
         CD=00  CD=01  CD=11  CD=10
AB=00 |   1   |  1   |  0   |  0   |
AB=01 |   1   |  1   |  1   |  0   |
AB=11 |   1   |  1   |  1   |  0   |
AB=10 |   1   |  1   |  0   |  0   |
```

Groups:
- All 1s in CD=00 column (4 cells) → D'C' (wait: CD=00 means C=0,D=0 → C'D')
  Actually: m₀,m₄,m₈,m₁₂ → AB vary, CD=00 → C'D'
- All 1s in CD=01 column (4 cells) → C'D
  m₁,m₅,m₉,m₁₃ → AB vary, CD=01 → C'D
  
Combining: C'D' + C'D = C'(D'+D) = **C'** (8 cells!)

Remaining: m₇(0111), m₁₅(1111) → in CD=11 column, rows AB=01 and AB=11. These are adjacent (B=1 in both, A varies). → BD.

Wait, more: m₅(0101) and m₇(0111) share? m₅ is in CD=01, m₇ in CD=11. m₅,m₇: 0101,0111 differ in C → A'D. But m₅ is already covered by C'.

For m₇ and m₁₅: both have BD (B=1,D=1). What about m₅,m₇,m₁₃,m₁₅? 0101,0111,1101,1111 → BD (B=1,D=1 in all, A and C vary). ✅

**F = C' + BD** ✅

## Practice Set 2: Number Systems

### P6. (1 mark) Convert −43₁₀ to 8-bit 2's complement.

**Solution:**
+43 = 00101011
Complement: 11010100
Add 1: **11010101** ✅

### P7. (1 mark) What is the decimal value of the 8-bit 2's complement number 10001011?

**Solution:**
MSB=1 → negative. Negate: 01110100 + 1 = 01110101 = 64+32+16+4+1 = 117
Answer: **−117** ✅... wait: 01110101 = 64+32+16+0+4+0+1 = 117. So the number is −117.

Actually let me check: 10001011.
Method 2: −2⁷ + 0 + 0 + 0 + 2³ + 0 + 2¹ + 2⁰ = −128 + 8 + 2 + 1 = **−117** ✅

### P8. (1 mark) What is the range of numbers that can be represented in 6-bit 2's complement?

**Solution:**
$-2^5$ to $+2^5-1$ = **−32 to +31** ✅

### P9. (2 marks) Perform addition of +83 and +44 in 8-bit 2's complement. Is there overflow?

**Solution:**
+83 = 01010011
+44 = 00101100
Sum:  01111111 = +127

Check: Carry into MSB = 0, Carry out of MSB = 0. V = 0⊕0 = 0.
**No overflow!** Result = **127** ✅

### P10. (2 marks) Perform addition of +83 and +47 in 8-bit 2's complement. Is there overflow?

**Solution:**
+83 = 01010011
+47 = 00101111
Sum:  10000010 = −126 (wrong!)

Check: Both positive, result negative → **OVERFLOW!** ✅
V = C₇⊕C₆ = 0⊕1 = 1 ✅

## Practice Set 3: Combinational Circuits

### P11. (2 marks) Implement F(A,B,C,D) = Σm(1,3,4,11,12,13,14,15) using an 8:1 MUX with A,B,C as select.

**Solution:**

| ABC | Minterms (D=0,D=1) | F(0) | F(1) | Input |
|---|---|---|---|---|
| 000 | m₀,m₁ | 0 | 1 | D |
| 001 | m₂,m₃ | 0 | 1 | D |
| 010 | m₄,m₅ | 1 | 0 | D' |
| 011 | m₆,m₇ | 0 | 0 | 0 |
| 100 | m₈,m₉ | 0 | 0 | 0 |
| 101 | m₁₀,m₁₁ | 0 | 1 | D |
| 110 | m₁₂,m₁₃ | 1 | 1 | 1 |
| 111 | m₁₄,m₁₅ | 1 | 1 | 1 |

I₀=D, I₁=D, I₂=D', I₃=0, I₄=0, I₅=D, I₆=1, I₇=1 ✅

### P12. (1 mark) A 3:8 decoder with an enable input can be used to implement any Boolean function of:

**Answer:** 4 variables! (3 address inputs + 1 enable = 4 input variables)

The enable acts as the 4th variable. When E=0, all outputs are 0 (function returns 0 for those inputs).

### P13. (2 marks) Design a 4-bit CLA adder's C₃ equation.

**Solution:**
C₃ = G₂ + P₂G₁ + P₂P₁G₀ + P₂P₁P₀C₀

This requires: 4 AND gates (with 1,2,3,4 inputs) + 1 OR gate (4 inputs)
Gate delay: 2 levels (one AND, one OR)

## Practice Set 4: Sequential Circuits

### P14. (2 marks) A sequential circuit uses 2 D flip-flops. The inputs are:
D₁ = Q₁⊕Q₀, D₀ = Q₀'

Starting from Q₁Q₀=00, list 8 successive states.

**Solution:**
| Clock | Q₁ | Q₀ | D₁=Q₁⊕Q₀ | D₀=Q₀' | Next Q₁Q₀ |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 | 01 |
| 1 | 0 | 1 | 1 | 0 | 10 |
| 2 | 1 | 0 | 1 | 1 | 11 |
| 3 | 1 | 1 | 0 | 0 | 00 |
| 4 | 0 | 0 | 0 | 1 | 01 |
| ... repeats ... |

**Sequence: 00→01→10→11→00→...** (It's a 2-bit Gray code counter!) ✅

### P15. (1 mark) How many flip-flops are required in the design of a Mod-12 counter?

**Solution:** ⌈log₂(12)⌉ = ⌈3.585⌉ = **4 flip-flops** ✅

### P16. (2 marks) A 4-bit synchronous counter uses JK flip-flops. Starting from 0000, what is the state after 13 clock pulses?

**Solution:**
4-bit counter counts 0→1→2→...→15→0→...
After 13 pulses: state = 13₁₀ = **1101₂** ✅

### P17. (1 mark) The minimum number of states required in a Moore machine to detect the sequence "10" is:

**Solution:**
States: S₀(no match/out=0), S₁(got "1"/out=0), S₂(got "10"/out=1)
**3 states** ✅ (Moore needs extra state for output)

For equivalent Mealy machine: only 2 states needed (output on transition to S₂).

### P18. (2 marks) In a JK flip-flop, if J=Q', K=1, and initial Q=0, what are the next 4 states?

**Solution:**
JK characteristic: Q⁺ = JQ' + K'Q

When K=1: Q⁺ = JQ' + 0·Q = JQ' (since K'=0)
When J=Q': Q⁺ = Q'·Q' = Q' (complement!)

Wait: Q⁺ = JQ' + K'Q with J=Q', K=1:
- Q=0: Q⁺ = Q'·Q' + 0·Q = 1·1 = 1
  Actually: J=Q'=1 when Q=0. K=1. JK=11 → Toggle. Q=0→Q=1 ✅
- Q=1: J=Q'=0. K=1. JK=01 → Reset. Q=1→Q=0 ✅

So it oscillates: 0→1→0→1→0

**States: 0, 1, 0, 1** (toggles every clock) ✅

## Practice Set 5: Error Detection & PLDs

### P19. (2 marks) A (15,11) Hamming code has how many check bits? How many single-bit errors can it correct?

**Solution:**
Check bits = 15 − 11 = **4** ✅
It can correct **1** single-bit error (SEC) ✅

### P20. (1 mark) A ROM with 12 address lines and 8 data lines can implement:

**Solution:**
12 address lines → 2¹² = 4096 addresses (words)
8 data lines → 8 output functions
It implements **8 Boolean functions of 12 variables each** ✅

### P21. (1 mark) For a PLA with 10 inputs, 8 product terms, and 6 outputs, the total number of programmable connections is:

**Solution:**
AND array: Each input has true and complement → 2×10 = 20 inputs to each AND gate
Total AND connections: 20 × 8 = **160**
OR array: 8 product terms × 6 outputs = **48**
Total: 160 + 48 = **208** programmable connections ✅

### P22. (2 marks) Design the CRC for data 10011101 with generator 1001.

**Solution:**
Generator degree = 3. Append 3 zeros: 10011101**000**

```
      10011101000 ÷ 1001

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
               110  ← CRC
```

**CRC = 110.** Transmitted: 10011101**110** ✅

## Practice Set 6: Advanced Boolean Algebra ⭐⭐

### P23. (1 mark) How many self-dual functions of 3 variables exist?

**Solution:**
For n variables: Self-dual functions = $2^{2^{n-1}}$
n=3: $2^{2^2} = 2^4 = \textbf{16}$ ✅

### P24. (2 marks) Is the set {NAND, XOR} functionally complete?

**Solution:**
From NAND alone, we can derive NOT, AND, OR → NAND is complete by itself.
So {NAND, XOR} is trivially complete (NAND alone suffices).
**Yes, functionally complete.** ✅

### P25. (1 mark) A function F is monotone increasing. If F(000)=0 and F(111)=1 and F(010)=1, what can we say about F(011)?

**Solution:**
Monotone means: if x≤y (bitwise), then F(x) ≤ F(y).
010 ≤ 011 (bitwise). Since F(010)=1, F(011) ≥ 1 → **F(011) = 1** ✅

### P26. (2 marks) Prove that XOR of n variables is 1 if and only if an odd number of inputs are 1.

**Solution by induction:**
- **Base case (n=2):** A⊕B = 1 iff exactly 1 input is 1 (odd). ✅
- **Inductive step:** Assume true for n-1. For n variables:
  $X_1⊕X_2⊕...⊕X_n = (X_1⊕X_2⊕...⊕X_{n-1}) ⊕ X_n$
  Let P = (X₁⊕...⊕X_{n-1}). By induction, P=1 iff odd number of X₁...X_{n-1} are 1.
  - If X_n=0: Result=P → odd among first n-1 → odd among all n ✅
  - If X_n=1: Result=P' → even among first n-1, plus one more → odd among all n ✅ ✅

### P27. (1 mark) Given F(A,B,C) = Σm(0,3,5,6), is F self-dual?

**Solution:**
Self-dual check: F(x) ≠ F(x') for all x.
- F(000)=1 → check F(111)=? → m₇ not in set → F(111)=0. 1≠0 ✅
- F(001)=0 → check F(110)=? → m₆ in set → F(110)=1. 0≠1 ✅
- F(010)=0 → check F(101)=? → m₅ in set → F(101)=1. 0≠1 ✅
- F(011)=1 → check F(100)=? → m₄ not in set → F(100)=0. 1≠0 ✅
All complementary pairs have opposite values → **F is self-dual!** ✅

### P28. (2 marks) Find the number of Boolean functions of 2 variables that belong to each of Post's 5 classes.

**Solution:**
16 total functions of 2 variables (F₀ through F₁₅).

| Class | Condition | Functions | Count |
|---|---|---|---|
| T₀ (0-preserving) | F(0,0)=0 | F₀,F₁,F₂,F₃,F₄,F₅,F₆,F₇ | **8** |
| T₁ (1-preserving) | F(1,1)=1 | F₈,F₉,...F₁₅ → actually: all F where row 11→1 | **8** |
| S (Self-dual) | F(x') = F'(x) | AND(F₁), OR(F₇), A(F₁₀), B(F₁₂), A'(F₅), B'(F₃) | **4** wait... |

Actually for 2 variables: $2^{2^{n-1}} = 2^2 = 4$ self-dual functions.

| Class | Count (n=2) |
|---|---|
| T₀ | 8 |
| T₁ | 8 |
| S | 4 |
| M (Monotone) | 6 (0, A, B, AB, A+B, 1) |
| L (Linear/Affine) | 8 (0, 1, A, A', B, B', A⊕B, A⊕B') |

### P29. (1 mark) How many prime implicants does the function F(A,B,C) = Σm(0,1,2,3,4,5,6,7) have?

**Solution:**
F = 1 for all inputs → F = 1. Only one prime implicant: **1** (the constant function).
Number of PIs = **1** ✅

> 🎯 **Trick:** F = Σ(all minterms) has exactly 1 PI. F = any single minterm has 1 PI (the minterm itself).

### P30. (2 marks) Simplify using Quine-McCluskey: F(A,B,C) = Σm(0,2,3,6,7)

**Step 1 — Group by number of 1s:**
| Group | Minterm | ABC |
|---|---|---|
| 0 | m₀ | 000 |
| 1 | m₂ | 010 |
| 2 | m₃ | 011 |
|   | m₆ | 110 |
| 3 | m₇ | 111 |

**Step 2 — Combine:**
(m₀,m₂): 0-0 → A'C' ✓
(m₂,m₃): 01- → A'B ✓
(m₂,m₆): -10 → BC' ✓
(m₃,m₇): -11 → BC ✓
(m₆,m₇): 11- → AB ✓

**Step 3 — Further combine:**
(A'B, BC) → can't (differ in >1 position)
(m₂,m₃,m₆,m₇): 01-,11- → -1- → B ✓

**PIs:** A'C', B
**PI Chart:**

| | m₀ | m₂ | m₃ | m₆ | m₇ |
|---|---|---|---|---|---|
| A'C' | × | × | | | |
| B | | × | × | × | × |

B covers m₂,m₃,m₆,m₇ → EPI
m₀ only covered by A'C' → EPI

**F = A'C' + B** ✅

## Practice Set 7: Flip-Flops & Timing ⭐⭐

### P31. (2 marks) Design a synchronous circuit using JK flip-flops that generates the sequence: 0→1→3→2→0→...

**Solution:**
States: 00→01→11→10→00 (Gray code!)

**State table:**
| Q₁ Q₀ | Q₁⁺ Q₀⁺ | J₁ K₁ | J₀ K₀ |
|---|---|---|---|
| 00 | 01 | 0 X | 1 X |
| 01 | 11 | 1 X | X 0 |
| 11 | 10 | X 0 | X 1 |
| 10 | 00 | X 1 | 0 X |

**K-map for J₁:**
```
     Q₀=0  Q₀=1
Q₁=0 | 0  | 1  |
Q₁=1 | X  | X  |
```
J₁ = Q₀

**K-map for K₁:**
```
     Q₀=0  Q₀=1
Q₁=0 | X  | X  |
Q₁=1 | 1  | 0  |
```
K₁ = Q₀'

**K-map for J₀:**
```
     Q₀=0  Q₀=1
Q₁=0 | 1  | X  |
Q₁=1 | 0  | X  |
```
J₀ = Q₁'

**K-map for K₀:**
```
     Q₀=0  Q₀=1
Q₁=0 | X  | 0  |
Q₁=1 | X  | 1  |
```
K₀ = Q₁

**Final: J₁=Q₀, K₁=Q₀', J₀=Q₁', K₀=Q₁** ✅

### P32. (1 mark) A system has t_cq = 3ns, t_comb = 8ns, t_setup = 2ns. What is the maximum clock frequency?

**Solution:**
$T_{min} = 3 + 8 + 2 = 13$ ns
$f_{max} = 1/13\text{ns} = \textbf{76.9 MHz}$ ✅

### P33. (2 marks) Given: t_cq = 5ns, t_setup = 3ns, t_hold = 2ns, positive clock skew = 1ns. Find minimum combinational delay to avoid hold violation.

**Solution:**
Hold condition with positive skew: $t_{cq} + t_{comb\_min} \geq t_{hold} + t_{skew}$
$5 + t_{comb\_min} \geq 2 + 1 = 3$
$t_{comb\_min} \geq -2$

Since delay can't be negative, **any non-negative delay is fine** — no hold violation possible! ✅

### P34. (1 mark) What is the output of a 3-bit ring counter after 7 clock pulses if initial state is 100?

**Solution:**
Ring counter shifts the single 1 through positions:
100→010→001→100→010→001→100→010
After 7 pulses: **010** ✅
(Period = 3 for 3-bit ring counter. 7 mod 3 = 1, so 1 position shifted)

### P35. (2 marks) A Johnson counter with 4 FFs. How many states does it enter? List the first 6 states starting from 0000.

**Solution:**
Johnson counter = twisted ring counter (complement of LSB fed back)
States: 2n = 2×4 = **8 states**

| Clock | Q₃Q₂Q₁Q₀ |
|---|---|
| 0 | 0000 |
| 1 | 1000 |
| 2 | 1100 |
| 3 | 1110 |
| 4 | 1111 |
| 5 | 0111 |
| 6 | 0011 |
| 7 | 0001 |
| 8 | 0000 (repeats) |

First 6 states: **0000, 1000, 1100, 1110, 1111, 0111** ✅

## Practice Set 8: State Machines & Minimization ⭐⭐

### P36. (2 marks) Design a Mealy machine to detect the sequence "01" with overlapping.

**Solution:**
States:
- S₀: Initial/got "1"
- S₁: Got "0"

| State | Input=0 | Output | Input=1 | Output |
|---|---|---|---|---|
| S₀ | S₁ | 0 | S₀ | 0 |
| S₁ | S₁ | 0 | S₀ | **1** |

Only 2 states needed! When in S₁ (last bit was 0) and input=1 → "01" detected.

**Trace on input 00101:**
S₀→0→S₁, S₁→0→S₁, S₁→1→S₀(out=1!), S₀→0→S₁, S₁→1→S₀(out=1!)
Detects at positions 3 and 5. ✅

### P37. (2 marks) Minimize the following Moore machine:

| State | Output | x=0 | x=1 |
|---|---|---|---|
| A | 0 | B | C |
| B | 0 | A | D |
| C | 1 | E | F |
| D | 0 | A | D |
| E | 1 | E | F |
| F | 1 | B | F |

**Step 1:** Different outputs → distinguishable
- {A,B,D} have output 0, {C,E,F} have output 1
- So (A,C), (A,E), (A,F), (B,C), (B,E), (B,F), (D,C), (D,E), (D,F) are all ×

**Step 2:** Check within same-output groups:

(A,B): x=0→(B,A), x=1→(C,D) → (C,D) is × → **(A,B) is ×**
(A,D): x=0→(B,A) need (A,B), x=1→(C,D) → (C,D) is × → **(A,D) is ×**
(B,D): x=0→(A,A)=same!, x=1→(D,D)=same! → **(B,D) EQUIVALENT!** ✅

(C,E): x=0→(E,E)=same!, x=1→(F,F)=same! → **(C,E) EQUIVALENT!** ✅
(C,F): x=0→(E,B) → different output groups → × 
(E,F): x=0→(E,B) → different output groups → ×

**Result:** Merge B≡D and C≡E. 4 states: {A, B(=D), C(=E), F}

| State | Output | x=0 | x=1 |
|---|---|---|---|
| A | 0 | B | C |
| B | 0 | A | B |
| C | 1 | C | F |
| F | 1 | B | F |

### P38. (1 mark) A sequential circuit has n flip-flops. What is the maximum number of states?

**Answer:** $2^n$ ✅

> 🎯 **Extension:** A Mealy machine with n FFs and m input bits can have at most $2^n$ states with $2^m$ transitions per state.

### P39. (2 marks) Design a non-overlapping sequence detector for "111" using a Moore machine.

**Solution:**
States: S₀(0): No match, S₁(0): Got "1", S₂(0): Got "11", S₃(1): Got "111"

| State/Output | Input=0 | Input=1 |
|---|---|---|
| S₀/0 | S₀ | S₁ |
| S₁/0 | S₀ | S₂ |
| S₂/0 | S₀ | S₃ |
| S₃/1 | S₀ | S₁ |

Non-overlapping: After detecting "111" in S₃, if next input is 1 → go to S₁ (only keep last "1"), not S₂.

Wait, for overlapping it would be different:
- S₃+input=1 → S₂ (since "11" is a valid prefix for overlapping)

For non-overlapping: S₃+input=1 → S₁ (start fresh, only current "1" counts) ✅

**State Assignment:** S₀=00, S₁=01, S₂=10, S₃=11

## Practice Set 9: Combinational Circuit Design ⭐⭐

### P40. (2 marks) Design a circuit that detects if a 4-bit BCD input is valid (0-9) using a single 4:16 decoder.

**Solution:**
Invalid BCD: 1010 through 1111 (values 10-15)
VALID = outputs 0 through 9 = D₀+D₁+D₂+...+D₉
Or: INVALID = D₁₀+D₁₁+D₁₂+D₁₃+D₁₄+D₁₅
VALID = (INVALID)' ← use one NOR gate on outputs 10-15

### P41. (1 mark) What is the minimum number of 2:1 MUXes needed to implement a 4:1 MUX?

**Solution:** 3 MUXes (two in first level, one in second level forming a MUX tree) ✅

**General:** n:1 MUX from 2:1 MUXes needs (n−1) 2:1 MUXes.

| Target MUX | 2:1 MUXes needed |
|---|---|
| 4:1 | 3 |
| 8:1 | 7 |
| 16:1 | 15 |
| 2ⁿ:1 | 2ⁿ−1 |

### P42. (2 marks) Implement F(A,B,C) = A'B + BC' + ABC using only NOR gates.

**Solution:**
First convert to POS: F = Σm(2,4,5,6,7) → F' = Σm(0,1,3) = A'B' + A'C
F = (A+B)(A+C') → this is the POS form

Wait: F = A'B + BC' + ABC = A'B + BC' + AB (since ABC ⊂ AB)
K-map: m₂,m₄,m₅,m₆,m₇
F = B + AC' → Simplified!

POS form: F = B + AC' → this is already SOP.
For NOR-only: Use DeMorgan's approach:
F = B + AC' = ((B + AC')')' 
= ((B)' · (AC'))' 
= (B' · (AC'))' ... not NOR form

Better: F = B + AC'. NOR implementation:
- X₁ = (A'+C) = (A NOR C')... Hmm, this is getting messy.

**Standard approach for NOR-only:** Convert to POS form first, then double complement.

F = B + AC'. 
POS: Need maxterms where F=0: m₀(000),m₁(001),m₃(011)
M₀ = A+B+C, M₁ = A+B+C', M₃ = A+B'+C'
F = (A+B+C)(A+B+C')(A+B'+C')

NOR-NOR implementation: Each maxterm becomes a NOR:
Step 1: Complement each maxterm: 
(A+B+C)' = A'B'C' → NOR(A,B,C)
(A+B+C')' = A'B'C → NOR(A,B,C')
(A+B'+C')' = A'BC → NOR(A,B',C')

Step 2: AND them = NOR of their complements:
F = NOR(NOR(A,B,C), NOR(A,B,C'), NOR(A,B',C'))... this gives (sum)' which is wrong.

Actually: For POS with NOR, we need:
F = ΠMᵢ. Each Mᵢ = (sum term).
Using bubbled-OR-AND: 
Level 1: Complement each literal, NOR them → gives (sum term)' 
Level 2: NOR all level-1 outputs → gives product of sum terms... nope.

**Correct NOR-only POS approach:**
$F = M_0 \cdot M_1 \cdot M_3 = \overline{\overline{M_0} + \overline{M_1} + \overline{M_3}}$

This is a NOR of the complements of the maxterms:
- $\overline{M_0}$ = m₀ = A'B'C' = NOR(A,B,C)
- $\overline{M_1}$ = m₁ = A'B'C = need NOT-NOR combination

This gets complex. The simpler answer: **NOR-only directly implements POS in 2 levels** with complemented inputs at first level.

Total NOR gates for this function: 3 (first level) + 1 (second level) + needed inverters = **approximately 7 NOR gates** ✅

### P43. (1 mark) An 8:1 MUX has data inputs D₀ to D₇ and select inputs S₂S₁S₀. What is the output when S₂S₁S₀ = 101?

**Answer:** Output = D₅ (since 101₂ = 5₁₀) ✅

### P44. (2 marks) Using a 4:16 decoder and OR gates, implement:
F₁(A,B,C,D) = Σm(0,1,4,5)
F₂(A,B,C,D) = Σm(3,7,11,15)

**Solution:**
F₁ = D₀ + D₁ + D₄ + D₅ (OR gate with 4 decoder outputs)
F₂ = D₃ + D₇ + D₁₁ + D₁₅ (OR gate with 4 decoder outputs)

Both share the same decoder — this is the advantage of decoders for multi-output functions!

Note: F₁ = A'C' (when A=0, C=0). F₂ = CD (when C=1, D=1). ✅

## Practice Set 10: Mixed Advanced Problems ⭐⭐⭐

### P45. (2 marks) A digital system has two 4-bit numbers A=A₃A₂A₁A₀ and B=B₃B₂B₁B₀. How many basic gates (AND,OR,NOT) are needed for a 4-bit equality comparator?

**Solution:**
Equality: A=B iff each bit matches.
For each bit pair: Eᵢ = Aᵢ⊙Bᵢ = AᵢBᵢ + Aᵢ'Bᵢ' (XNOR)
Each XNOR needs: 2 NOT + 2 AND + 1 OR = 5 gates
Total for 4 bits: 4 × 5 = 20 gates for individual comparisons
Final AND: E = E₀·E₁·E₂·E₃ = 1 AND gate (4-input, or 3 cascaded 2-input ANDs)

**Total: 20 + 3 = 23 gates** (using 2-input gates)
Or with 4-input AND: 20 + 1 = **21 gates** ✅

### P46. (1 mark) In a positive-edge-triggered D-FF, if D changes from 1 to 0 exactly at the rising edge, what is captured?

**Solution:**
If the change happens exactly at the edge, this is a **setup/hold time violation!**
The output is **unpredictable (metastable)** — it could be 0, 1, or oscillate. ✅

### P47. (2 marks) A 5-variable K-map F(A,B,C,D,E) has how many cells? How many adjacent cells does each cell have?

**Solution:**
Cells = 2⁵ = **32**
Each cell has exactly **5 adjacent cells** (one for each variable that can change) ✅

In general: n-variable K-map → each cell has n adjacent cells.

### P48. (1 mark) The number of essential prime implicants of F(A,B,C) = Σm(0,1,2,3,4,5) is:

**Solution:**
K-map (3-var):
```
      BC=0  BC=1  BC=11 BC=10
A=0 |  1  |  1  |  1  |  1  |
A=1 |  1  |  1  |  0  |  0  |
```

Groups: A'(covers m₀,m₁,m₂,m₃), C'(covers m₀,m₂,m₄... wait)
Actually: m₀,m₁,m₂,m₃ → A' (4 cells)
m₀,m₁,m₄,m₅ → B' (4 cells)

A' covers {0,1,2,3}. B' covers {0,1,4,5}.
m₂,m₃ only in A' → A' is EPI
m₄,m₅ only in B' → B' is EPI

**F = A' + B'** (2 EPIs, both essential, covers everything) ✅

### P49. (2 marks) A sequential circuit with 3 FFs (Q₂Q₁Q₀) and input X has the following state table. Find the output sequence for input X = 1,0,1,1 starting from state 000.

| State | X=0 next/out | X=1 next/out |
|---|---|---|
| 000 | 001/0 | 100/1 |
| 001 | 010/0 | 101/0 |
| 010 | 011/1 | 110/0 |
| 011 | 100/0 | 111/1 |
| 100 | 101/0 | 000/0 |
| 101 | 110/1 | 001/0 |
| 110 | 111/0 | 010/1 |
| 111 | 000/1 | 011/0 |

**Trace:**
1. State=000, X=1 → Next=100, Output=1
2. State=100, X=0 → Next=101, Output=0
3. State=101, X=1 → Next=001, Output=0
4. State=001, X=1 → Next=101, Output=0

**Output sequence: 1, 0, 0, 0** ✅

### P50. (2 marks) For a 6-bit CLA adder, express the block generate (G*) and block propagate (P*) for the entire 6-bit addition.

**Solution:**
Individual: Gᵢ = AᵢBᵢ, Pᵢ = Aᵢ⊕Bᵢ (or Aᵢ+Bᵢ for CLA)

Block generate for bits 0-5:
$G^* = G_5 + P_5G_4 + P_5P_4G_3 + P_5P_4P_3G_2 + P_5P_4P_3P_2G_1 + P_5P_4P_3P_2P_1G_0$

Block propagate:
$P^* = P_5P_4P_3P_2P_1P_0$

**Carry out:** $C_6 = G^* + P^*C_0$

This is often split into two 3-bit blocks for practical implementation:
- Block 1 (bits 0-2): $G_1^* = G_2 + P_2G_1 + P_2P_1G_0$, $P_1^* = P_2P_1P_0$
- Block 2 (bits 3-5): $G_2^* = G_5 + P_5G_4 + P_5P_4G_3$, $P_2^* = P_5P_4P_3$
- Overall: $G^* = G_2^* + P_2^*G_1^*$, $P^* = P_2^*P_1^*$

This is the **Two-Level (Block) CLA** structure — adds delay but reduces hardware complexity. ✅

---

# 🏆 APPENDIX

## 📝 Worked Examples — GATE Level Problems

### Example 1: K-Map Minimization with Don't Cares (2 marks)

**Q:** F(A,B,C,D) = Σm(0,1,2,5,8,9,10) with d(3,7,11,15). Find minimum SOP.

**K-map:**
```
         CD=00  CD=01  CD=11  CD=10
AB=00 |   1   |  1   |  d   |  1   |
AB=01 |   0   |  1   |  d   |  0   |
AB=11 |   0   |  0   |  d   |  0   |
AB=10 |   1   |  1   |  d   |  1   |
```

Note all d(3,7,11,15) cells are in column CD=11!

**Groups:**
- m₀,m₁,m₂,m₃(d),m₈,m₉,m₁₀,m₁₁(d) → **B'** (8 cells, AB has rows 00,10 = B'=0, CD all)
  Wait: rows 00 and 10 differ only in A, and all 4 columns → 8 cells = B'
- m₁,m₅,m₃(d),m₇(d) → **C'D** (4 cells)... 
  Actually let me re-examine: m₁(0001), m₅(0101), m₃(0011,d), m₇(0111,d) → columns CD=01 and CD=11, rows AB=00 and AB=01 → A'D (B varies, C varies cancel → A'D? Let me check: A=0 in both rows, D=1 in both columns → A'D).

Hmm, but we already cover m₁ and m₅ through B'? No — m₅(0101) has B=1, so B' doesn't cover it.

Let me redo carefully:
- Group 1: Top-left 8 cells = rows AB=00 and AB=10, all columns. These cover m₀,m₁,m₃(d),m₂,m₈,m₉,m₁₁(d),m₁₀ → variable B=0 in both rows → **B'**

This covers everything except m₅. 

- Group 2: m₅(0101) with m₇(d)(0111) → C'D and A'D? m₅ and m₇: 0101,0111 differ in bit C → **A'BD** (no, let me just find the biggest group containing m₅).
  m₁(0001),m₅(0101),m₃(0011,d),m₇(0111,d) → these form a 4-cell group: A'D (A=0, D=1, B and C vary)

**Minimum SOP:** F = B' + A'D ✅ (only 2 terms!)

### Example 2: Functional Completeness Check (1 mark)

**Q:** Is {XOR, AND, 1} functionally complete?

| Function | ∉T₀ | ∉T₁ | ∉S | ∉M | ∉L |
|---|---|---|---|---|---|
| XOR | | ✅ | ✅ | ✅ | |
| AND | | | ✅ | | ✅ |
| 1 | ✅ | | | | |

All 5 columns have ✅ → **YES, functionally complete!** ✅

**Verify:** NOT A = A ⊕ 1. OR = can derive from AND + NOT (De Morgan's).

### Example 3: Hamming Code Syndrome (2 marks)

**Q:** In a (7,4) Hamming code with even parity, the received codeword is 1001010. Find the error position (if any) and the corrected codeword.

**Syndrome calculation:**
Positions: P₁=1, P₂=0, D₃=0, P₄=1, D₅=0, D₆=1, D₇=0

S₁ = P₁⊕D₃⊕D₅⊕D₇ = 1⊕0⊕0⊕0 = **1**
S₂ = P₂⊕D₃⊕D₆⊕D₇ = 0⊕0⊕1⊕0 = **1**
S₄ = P₄⊕D₅⊕D₆⊕D₇ = 1⊕0⊕1⊕0 = **0**

Syndrome = S₄S₂S₁ = **011** = position **3**

Corrected codeword: 10**1**1010 (flip bit 3: 0→1) ✅

### Example 4: CLA Carry Computation (2 marks)

**Q:** For a 4-bit CLA adder with A=1010, B=0111, C₀=0. Find all carries C₁ to C₄.

**Step 1:** Compute G and P:
- G₀=A₀B₀=0·1=0, P₀=A₀⊕B₀=0⊕1=1
- G₁=A₁B₁=1·1=1, P₁=A₁⊕B₁=1⊕1=0
- G₂=A₂B₂=0·1=0, P₂=A₂⊕B₂=0⊕1=1
- G₃=A₃B₃=1·0=0, P₃=A₃⊕B₃=1⊕0=1

**Step 2:** CLA equations:
- C₁ = G₀+P₀C₀ = 0+1·0 = **0**
- C₂ = G₁+P₁G₀+P₁P₀C₀ = 1+0+0 = **1**
- C₃ = G₂+P₂G₁+P₂P₁G₀+P₂P₁P₀C₀ = 0+1·1+0+0 = **1**
- C₄ = G₃+P₃G₂+P₃P₂G₁+P₃P₂P₁G₀+P₃P₂P₁P₀C₀ = 0+1·0+1·1·1+0+0 = **1**

**Step 3:** Sum bits:
- S₀ = P₀⊕C₀ = 1⊕0 = 1
- S₁ = P₁⊕C₁ = 0⊕0 = 0
- S₂ = P₂⊕C₂ = 1⊕1 = 0
- S₃ = P₃⊕C₃ = 1⊕1 = 0

Result: C₄S₃S₂S₁S₀ = 1|0001 → **10001** = 17₁₀

Check: 1010₂(10) + 0111₂(7) = 10001₂(17) ✅

### Example 5: MUX Function Implementation (2 marks)

**Q:** Implement F(A,B,C,D) = Σm(0,1,3,5,7,8,9,14,15) using an 8:1 MUX with A,B,C as select lines.

**Group by ABC and check D's role:**

| A | B | C | Minterms (D=0,D=1) | F(D=0) | F(D=1) | Input |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | m₀,m₁ | 1 | 1 | **1** |
| 0 | 0 | 1 | m₂,m₃ | 0 | 1 | **D** |
| 0 | 1 | 0 | m₄,m₅ | 0 | 1 | **D** |
| 0 | 1 | 1 | m₆,m₇ | 0 | 1 | **D** |
| 1 | 0 | 0 | m₈,m₉ | 1 | 1 | **1** |
| 1 | 0 | 1 | m₁₀,m₁₁ | 0 | 0 | **0** |
| 1 | 1 | 0 | m₁₂,m₁₃ | 0 | 0 | **0** |
| 1 | 1 | 1 | m₁₄,m₁₅ | 1 | 1 | **1** |

**Answer:** I₀=1, I₁=D, I₂=D, I₃=D, I₄=1, I₅=0, I₆=0, I₇=1 ✅

### Example 6: Sequence Detector (2 marks)

**Q:** Design a Mealy machine to detect the sequence "101" with overlapping. How many states? What is the output for input 1010101?

**States:** S₀(no match), S₁("1"), S₂("10"). Need 3 states.

| State | In=0 | In=1 |
|---|---|---|
| S₀ | S₀/0 | S₁/0 |
| S₁ | S₂/0 | S₁/0 |
| S₂ | S₀/0 | S₁/**1** |

**Trace for 1010101:**
| Input | State Before | Next State | Output |
|---|---|---|---|
| 1 | S₀ | S₁ | 0 |
| 0 | S₁ | S₂ | 0 |
| 1 | S₂ | S₁ | **1** ⭐ |
| 0 | S₁ | S₂ | 0 |
| 1 | S₂ | S₁ | **1** ⭐ |
| 0 | S₁ | S₂ | 0 |
| 1 | S₂ | S₁ | **1** ⭐ |

**Output: 0010101** (3 detections) ✅

### Example 7: Ring Counter Question (1 mark)

**Q:** A ring counter with 4 flip-flops is initialized to 1000. After 10 clock pulses, what is the state?

Ring counter period = 4 (cycles through 1000→0100→0010→0001→1000...)

10 mod 4 = **2**

After 2 pulses: 1000 → 0100 → **0010** ✅

### Example 8: Johnson Counter (1 mark)

**Q:** How many flip-flops needed for a Johnson counter with 12 valid states?

Johnson counter with n FFs has **2n** valid states.
2n = 12 → n = **6 flip-flops** ✅

### Example 9: 2's Complement Range (1 mark)

**Q:** The most negative number representable in 12-bit 2's complement is?

Range: $-2^{n-1}$ to $+2^{n-1}-1$

Most negative = $-2^{11}$ = **−2048** ✅

### Example 10: Overflow Detection (1 mark)

**Q:** In 8-bit 2's complement, what is the result of 01111111 + 00000001? Is there overflow?

01111111 = +127
00000001 = +1
10000000 = −128 (in 2's complement!)

**Overflow: YES** ✅ (two positives gave a negative → V = 1)

### Example 11: Self-Dual Function Count (1 mark)

**Q:** The number of self-dual Boolean functions possible with 4 variables is?

Formula: $2^{2^{n-1}} = 2^{2^3} = 2^8 = \textbf{256}$ ✅

### Example 12: Prime Implicants Count (2 marks)

**Q:** F(A,B,C,D) = Σm(0,2,5,7,8,10,13,15). How many prime implicants are there?

K-map:
```
         CD=00  CD=01  CD=11  CD=10
AB=00 |   1   |  0   |  0   |  1   |
AB=01 |   0   |  1   |  1   |  0   |
AB=11 |   0   |  1   |  1   |  0   |
AB=10 |   1   |  0   |  0   |  1   |
```

PIs:
- m₀,m₂,m₈,m₁₀ → B'D' (4 cells)
- m₅,m₇,m₁₃,m₁₅ → BD (4 cells)

These two cover everything! And each is maximal.

**Answer: 2 prime implicants** ✅

### Example 13: Decoder Implementation (2 marks)

**Q:** A 4-to-16 decoder is to be built using 2-to-4 decoders with enable. What is the minimum number needed?

**Answer:** 1 (top-level) + 4 (bottom-level) = **5 decoders** ✅

The top decoder generates enable signals for the 4 bottom decoders using the 2 high-order address bits.

### Example 14: Shannon Expansion ⭐⭐ (2 marks)

**Q:** Using Shannon's expansion on variable A, decompose F(A,B,C) = AB + BC + A'C'

**Solution:**
F = A·F(1,B,C) + A'·F(0,B,C)

F(1,B,C) = 1·B + BC + 0·C' = B + BC = B (absorption)
F(0,B,C) = 0·B + BC + 1·C' = BC + C'

F = A·B + A'·(BC + C')
F = AB + A'BC + A'C' ✅

**Verify:** Original F = AB + BC + A'C'. Let's check m₂(010): AB=0,BC=0,A'C'=1·1=1 → F=1
Shannon: AB=0, A'BC=1·0·0=0, A'C'=1·1=1 → F=1 ✅

### Example 15: Mealy-to-Moore Conversion ⭐⭐ (2 marks)

**Q:** Convert the following Mealy machine to Moore:

| State | x=0 Next/Out | x=1 Next/Out |
|---|---|---|
| A | A/0 | B/1 |
| B | A/1 | B/0 |

**Solution:**
Check each state — does it have different outputs on incoming transitions?

State A: Incoming from (A,0→A,out=0) and (B,0→A,out=1). Different outputs → **Split A into A₀(out=0) and A₁(out=1)!**
State B: Incoming from (A,1→B,out=1) and (B,1→B,out=0). Different outputs → **Split B into B₀(out=0) and B₁(out=1)!**

| Moore State | Output | x=0 | x=1 |
|---|---|---|---|
| A₀ | 0 | A₀ | B₁ |
| A₁ | 1 | A₀ | B₁ |
| B₀ | 0 | A₁ | B₀ |
| B₁ | 1 | A₁ | B₀ |

4 Moore states for 2 Mealy states. In general: Moore can have up to |States_Mealy| × |Outputs| states. ✅

### Example 16: IEEE 754 Denormalized Number ⭐⭐ (2 marks)

**Q:** What is the smallest positive number representable in IEEE 754 half-precision (16-bit)?

**Half precision:** 1 sign + 5 exponent + 10 mantissa, bias=15

**Smallest positive denormalized:**
- S=0, E=00000 (all zero), M=0000000001 (smallest non-zero M)
- Value = $(-1)^0 × 0.0000000001_2 × 2^{1-15} = 2^{-10} × 2^{-14} = 2^{-24}$
- $= \textbf{5.96 × 10⁻⁸}$ ✅

### Example 17: LFSR Sequence ⭐⭐ (2 marks)

**Q:** A 4-bit LFSR has feedback: D_in = Q₃ ⊕ Q₀. Starting from 1000, list the first 8 states.

**Solution:**
```
State: Q₃Q₂Q₁Q₀
1000 → D_in = 1⊕0 = 1 → shift right → 1100
1100 → D_in = 1⊕0 = 1 → 1110
1110 → D_in = 1⊕0 = 1 → 1111
1111 → D_in = 1⊕1 = 0 → 0111
0111 → D_in = 0⊕1 = 1 → 1011
1011 → D_in = 1⊕1 = 0 → 0101
0101 → D_in = 0⊕1 = 1 → 1010
1010 → D_in = 1⊕0 = 1 → 1101
```

States: 1000→1100→1110→1111→0111→1011→0101→1010→1101→...
This is a maximal length sequence (period=15=2⁴-1). ✅

### Example 18: Counter Design with Unused States ⭐⭐ (2 marks)

**Q:** Design a Mod-6 counter (counts 0-5) with JK FFs. What happens with unused states 6 and 7?

**State table (Q₂Q₁Q₀):**
| Current | Next | J₂K₂ | J₁K₁ | J₀K₀ |
|---|---|---|---|---|
| 000 | 001 | 0X | 0X | 1X |
| 001 | 010 | 0X | 1X | X1 |
| 010 | 011 | 0X | X0 | 1X |
| 011 | 100 | 1X | X1 | X1 |
| 100 | 101 | X0 | 0X | 1X |
| 101 | 000 | X1 | 0X | X1 |
| 110 | XXX | XX | XX | XX |
| 111 | XXX | XX | XX | XX |

K-maps for J₂ (with don't cares for states 6,7):
```
       Q₁Q₀=00  01  11  10
Q₂=0 |   0   | 0 | 1 | 0 |
Q₂=1 |   X   | X | X | X |
```
J₂ = Q₁Q₀

K-map for K₂:
```
       Q₁Q₀=00  01  11  10
Q₂=0 |   X   | X | X | X |
Q₂=1 |   0   | 1 | X | X |
```
K₂ = Q₀

Similarly: J₁ = Q₂'Q₀, K₁ = Q₀, J₀ = 1 (always toggle LSB), K₀ = 1

Wait, let me redo J₁:
```
       Q₁Q₀=00  01  11  10
Q₂=0 |   0   | 1 | X | X |
Q₂=1 |   0   | 0 | X | X |
```
J₁ = Q₂'Q₀

And K₁:
```
       Q₁Q₀=00  01  11  10
Q₂=0 |   X   | X | 1 | 0 |
Q₂=1 |   X   | X | X | X |
```
K₁ = Q₀

**Unused state analysis:** States 110 and 111 with derived equations:
- State 110: J₂=0,K₂=0(hold),J₁=0,K₁=0(hold),J₀=1,K₀=1(toggle) → Next=111
- State 111: J₂=Q₁Q₀=1,K₂=Q₀=1(toggle)→Q₂=0, J₁=0,K₁=1→Q₁=0, J₀=1,K₀=1→Q₀=0 → Next=000

**Self-correcting!** 110→111→000 → re-enters valid sequence within 2 clocks! ✅

### Example 19: MUX with Complemented Variable ⭐ (2 marks)

**Q:** Implement F(A,B,C,D) = Σm(0,2,6,7,8,10,14,15) using a 4:1 MUX with A and C as select lines.

**Solution:**
Grouping by select values (AC):

| AC | Minterms | BD values | F pattern | Input |
|---|---|---|---|---|
| 00 | m₀(0000),m₁(0001),m₂(0010),m₃(0011) | F(m₀)=1,F(m₁)=0,F(m₂)=1,F(m₃)=0 | D' | D' |
| 01 | m₄(0100),m₅(0101),m₆(0110),m₇(0111) | F(m₄)=0,F(m₅)=0,F(m₆)=1,F(m₇)=1 | B | B |
| 10 | m₈(1000),m₉(1001),m₁₀(1010),m₁₁(1011) | F=1,0,1,0 | D' | D' |
| 11 | m₁₂(1100),m₁₃(1101),m₁₄(1110),m₁₅(1111) | F=0,0,1,1 | B | B |

I₀=D', I₁=B, I₂=D', I₃=B

Wait, but we need B and D as the "remaining" variables. Let me re-check:
When AC=00: remaining vars are B,D. The four minterms are BD=00,01,10,11 → F=1,0,1,0 → this is D'.
When AC=01: BD=00,01,10,11 → F=0,0,1,1 → this is B.
When AC=10: BD=00,01,10,11 → F=1,0,1,0 → this is D'.
When AC=11: BD=00,01,10,11 → F=0,0,1,1 → this is B.

**I₀=D', I₁=B, I₂=D', I₃=B** ✅

Actually: F = BD + D'(B)' + ... → Let's verify: F = B⊕D' simplification? 
K-map confirms: F = BD + B'D' = B⊙D = (B⊕D)' ✅

### Example 20: Functional Completeness — Advanced ⭐⭐ (2 marks)

**Q:** Is {A→B} (implication) alone a functionally complete set?

**A→B = A'+B** (if A then B = not-A or B)

Check Post's criteria:
- T₀: F(0,0) = 0'+0 = 1+0 = 1 ≠ 0 → **A→B ∉ T₀** ✅ (already breaks T₀!)
- T₁: F(1,1) = 1'+1 = 0+1 = 1 → A→B ∈ T₁
- S: F(0,0)=1, F(1,1)=1 but F'(0,0)=0→ self-dual requires F(0,0)≠F(1,1) since (0,0)'=(1,1). F(0,0)=1=F(1,1) → **not self-dual ∉ S** ✅
- M: F(0,0)=1, F(0,1)=1, F(1,0)=0, F(1,1)=1. Since (0,0)≤(1,0) but F(0,0)=1>F(1,0)=0 → **not monotone ∉ M** ✅
- L: F=A'+B = A⊕B⊕AB⊕1 (since A'=A⊕1, and expanding). Contains AB term → **not linear ∉ L** ✅

Breaks T₀, S, M, L (breaks 4 of the 5 classes) → **Functionally complete!** ✅

**Deriving NOT and AND:**
- NOT A = A→0? No, we don't have constant 0.
- NOT A = A→A gives A'+A = 1 (not helpful)
- NOT A = (A→A)→... Hmm.

Actually: 
- A→A = 1 (always)
- NOT: We can get A' = A→0, but we need 0 first.
Wait — with single function {A→B}, constants aren't available as inputs.

Let me reconsider: A→(A→A) = A→1 = A'+1 = 1. Not helpful.
(A→B)→A = (A'+B)→A = (A'+B)'+A = AB'+A = A(B'+1) = A. Not helpful.

**Getting NOT A:** $(A \to (A \to B)) \to (A \to B)$... this is complex.

Actually: We know $\neg A = A \to \bot$ but we need ⊥ first.

**Better approach:** Since {→} is functionally complete (proven by Post's), we CAN derive everything, but the construction is non-trivial. The key insight is that from Post's theorem, we KNOW it's complete without needing to construct every gate. ✅

## Formula Card 📝

| Formula | Expression |
|---|---|
| Self-dual functions | $2^{2^{n-1}}$ |
| Total functions | $2^{2^n}$ |
| Linear functions | $2^{n+1}$ |
| Hamming: parity bits | $2^r \geq m+r+1$ |
| CLA carry delay | 4 gate delays (constant) |
| Ripple carry delay | 2n gate delays |
| Max FF frequency | $1/(t_{cq}+t_{comb}+t_s)$ |
| Ring counter | Mod-n with n FFs |
| Johnson counter | Mod-2n with n FFs |
| Ripple counter max freq | $1/(n \times t_p)$ |
| FFs for Mod-N | $\lceil\log_2 N\rceil$ |
| Error detect | d_min ≥ t+1 |
| Error correct | d_min ≥ 2t+1 |
| Detect t + correct c | d_min ≥ t+c+1, t>c |
| 2:1 MUX for 2ⁿ:1 | 2ⁿ−1 MUX needed |
| 2ⁿ:1 MUX for n-var fn | All vars as select |
| 2ⁿ⁻¹:1 MUX for n-var fn | n-1 vars as select, 1 as data |
| Boolean functions of n vars | $2^{2^n}$ |
| Minterms of n vars | $2^n$ |
| Overflow (2's comp) | V = Cₙ ⊕ Cₙ₋₁ |
| 2's comp range (n bits) | $-2^{n-1}$ to $+2^{n-1}-1$ |
| LFSR max period (n bits) | $2^n - 1$ |

## 🎯 GATE Tricks & Traps — Comprehensive

| # | Trap/Trick | Correct Approach |
|---|---|---|
| 1 | K-map column order wrong | Gray: 00,01,**11,10** (not 00,01,10,11!) |
| 2 | No K-map wrap-around | Top↔Bottom, Left↔Right, 4 corners valid! |
| 3 | Overflow ignored | V = Cᵢₙ⊕Cₒᵤₜ at MSB (2's comp only) |
| 4 | Gray code arithmetic | Convert to binary first! No direct arithmetic! |
| 5 | All vars as MUX select | Better: n-1 select, 1 as data variable |
| 6 | Hamming positions wrong | Parity at **powers of 2** (1,2,4,8,...) |
| 7 | CRC: no zero-append | Append r zeros before division |
| 8 | BCD >9 not corrected | Add 0110 when sum>9 or carry |
| 9 | Static hazard ignored | Add consensus terms to eliminate |
| 10 | Post's: incomplete check | Must break ALL 5 classes for completeness |
| 11 | JK toggle forgotten | J=K=1 → Q'(t), NOT invalid like SR! |
| 12 | Ripple delay wrong | Accumulated: n×tₚ (not just one stage!) |
| 13 | Moore output timing | Delayed 1 clock vs Mealy |
| 14 | Hold violation fix | **Cannot fix by slowing clock!** Add delay buffers |
| 15 | PLA vs PAL confusion | PLA: both programmable. PAL: AND only |
| 16 | Don't cares in F' | Don't cares stay as don't cares in F' |
| 17 | XOR distributes over OR | **NO!** XOR distributes over AND only |
| 18 | Negating −128 in 8-bit | Result is still −128 (overflow!) |
| 19 | Ring counter self-starting | **Not self-starting** without extra logic |
| 20 | LFSR all-zeros state | All-zeros is NOT part of max-length sequence |

## 📊 Memorization Tables

### Gate Comparison Super Table

| Gate | AND | OR | NAND | NOR | XOR | XNOR | NOT |
|---|---|---|---|---|---|---|---|
| f(0,0) | 0 | 0 | 1 | 1 | 0 | 1 | — |
| f(0,1) | 0 | 1 | 1 | 0 | 1 | 0 | — |
| f(1,0) | 0 | 1 | 1 | 0 | 1 | 0 | — |
| f(1,1) | 1 | 1 | 0 | 0 | 0 | 1 | — |
| ∈T₀? | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |
| ∈T₁? | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| ∈S? | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| ∈M? | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| ∈L? | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ |
| Universal? | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ |
| NAND count | 2 | 3 | 1 | 4 | 4 | 5 | 1 |
| NOR count | 3 | 2 | 4 | 1 | 5 | 4 | 1 |

### FF Super Table

| Property | D | T | JK | SR |
|---|---|---|---|---|
| Char. equation | Q⁺=D | Q⁺=T⊕Q | Q⁺=JQ'+K'Q | Q⁺=S+R'Q |
| 0→0 | D=0 | T=0 | J=0,K=X | S=0,R=0 |
| 0→1 | D=1 | T=1 | J=1,K=X | S=1,R=0 |
| 1→0 | D=0 | T=1 | J=X,K=1 | S=0,R=1 |
| 1→1 | D=1 | T=0 | J=X,K=0 | S=0,R=0 |
| Invalid? | No | No | No | S=R=1 |
| Toggle? | D=Q' | T=1 | J=K=1 | N/A |
| Hold? | D=Q | T=0 | J=K=0 | S=R=0 |

### Counter Quick Reference

| Counter Type | FFs for N states | Decoding | Max Freq | Self-starting? |
|---|---|---|---|---|
| Binary (sync) | ⌈log₂N⌉ | Complex | f_clk/(logic delay) | Yes (if designed) |
| Binary (async) | ⌈log₂N⌉ | Complex | f_clk/(n×tₚ) | Yes (if designed) |
| Ring | N | None (one-hot) | f_clk | No |
| Johnson | N/2 | 2-input AND | f_clk | No |

### Error Code Quick Reference

| Code | Bits Added | Detect | Correct | Used In |
|---|---|---|---|---|
| Parity (1-bit) | 1 | 1 error | 0 | Memory |
| 2D Parity | row+col | 3 errors | 1 error | Old protocols |
| Hamming SEC | r bits ($2^r≥m+r+1$) | 2 errors | 1 error | ECC RAM |
| Hamming SECDED | r+1 bits | 2 errors | 1 error | ECC RAM |
| CRC-32 | 32 | Burst ≤32 | 0 (detection only) | Ethernet, ZIP |

## GATE PYQ Frequency

| Topic | Frequency | Marks | Typical Question Type |
|---|---|---|---|
| K-map/minimization | ⭐⭐⭐⭐⭐ | 1-2 | Simplify, count PI/EPI |
| MUX implementation | ⭐⭐⭐⭐ | 2 | Implement using 8:1 or 4:1 MUX |
| Self-dual/counting functions | ⭐⭐⭐ | 1 | How many self-dual/total/linear functions |
| Flip-flop characteristic | ⭐⭐⭐ | 1 | Excitation table, conversion |
| Sequential design | ⭐⭐⭐ | 2 | Counter design, sequence detector |
| 2's complement | ⭐⭐⭐ | 1 | Range, overflow, negation |
| Hamming/CRC | ⭐⭐⭐ | 1-2 | Encode/decode, find error |
| Functional completeness (Post's) | ⭐⭐ | 1 | Is {set} complete? |
| Mealy/Moore | ⭐⭐ | 1-2 | Design, conversion |
| Decoder/encoder | ⭐⭐ | 1 | Decoder tree, function implementation |
| ROM/PLA/PAL | ⭐⭐ | 1 | Size calculation, comparison |
| Hazards | ⭐ | 1 | Identify/eliminate static hazard |
| IEEE 754 (cross with COA) | ⭐⭐ | 1 | Binary→float, special values |

## 📖 Topic-Wise Summary — Quick Revision ⭐⭐⭐

### Boolean Algebra Quick Revision
- 2ⁿ minterms/maxterms for n variables
- $2^{2^n}$ total functions, $2^{2^{n-1}}$ self-dual, $2^{n+1}$ linear/affine
- De Morgan's: complement all vars + swap AND↔OR
- Dual: swap AND↔OR + swap 0↔1 (DON'T complement vars)
- Consensus: AB + A'C + BC = AB + A'C (redundant term removal)
- A + A'B = A + B (key simplification identity)

### Minimization Quick Revision
- K-map column order: Gray code (00,01,11,10)
- Groups must be power of 2 (1,2,4,8,16)
- Wrap-around allowed: top↔bottom, left↔right, 4 corners
- Don't cares: use in grouping but don't need to cover
- PI: group that can't be made larger. EPI: PI that uniquely covers a minterm
- Q-M: group by #1s, combine adjacent groups, iterate
- Petrick's method: for multiple solutions when EPIs don't cover everything

### Number Systems Quick Revision
- 2's complement range: $-2^{n-1}$ to $2^{n-1}-1$
- 1's complement range: $-(2^{n-1}-1)$ to $2^{n-1}-1$ (two zeros!)
- Overflow: V = Cₙ₋₁ ⊕ Cₙ (carry into MSB ⊕ carry out of MSB)
- Gray↔Binary: MSB same, then XOR from left to right
- BCD: Add 0110 when digit sum > 9 or carry
- IEEE 754 single: bias=127. Double: bias=1023
- Denormalized: E=0, no implicit 1, exponent=1−bias

### Combinational Circuits Quick Revision
- MUX 2ⁿ:1: n select lines, 2ⁿ data inputs → any n-var function
- MUX 2ⁿ⁻¹:1: use (n-1) vars as select, remaining var as data (0, 1, var, var')
- Decoder 2ⁿ outputs: each = one minterm. OR selected outputs.
- CLA: constant carry delay (2 levels). RCA: 2n gate delays
- Generator: Gᵢ = AᵢBᵢ. Propagate: Pᵢ = Aᵢ⊕Bᵢ (or Aᵢ+Bᵢ for CLA)
- Hazards: SOP has static-1, POS has static-0. Fix with consensus terms.

### Sequential Circuits Quick Revision
- Latch: level-triggered. FF: edge-triggered
- D-FF: simplest, Q⁺=D. JK: most versatile, Q⁺=JQ'+K'Q. T: toggle, Q⁺=T⊕Q. SR: Q⁺=S+R'Q (S·R=0)
- Excitation table: given Q→Q⁺, find FF inputs (MUST memorize for design!)
- Timing: $T_{min} = t_{cq} + t_{comb\_max} + t_{setup}$. Hold: $t_{cq} + t_{comb\_min} \geq t_{hold}$
- Mealy: output = f(state, input). Moore: output = f(state). Moore has 1 more state typically.
- State minimization: implication table method — mark different-output pairs, then propagate
- Sequence detector: states = prefixes of pattern. Overlapping keeps partial match.

### Counters & Registers Quick Revision
- Async counter: n FFs cascaded, max freq = 1/(n·tₚ), glitchy
- Sync counter: all share clock, T₀=1, T₁=Q₀, T₂=Q₀Q₁, ...
- Ring: N states with N FFs (one-hot), not self-starting
- Johnson: 2N states with N FFs, complement feedback, not self-starting
- Mod-N: needs ⌈log₂N⌉ FFs
- LFSR: max period = 2ⁿ−1 (all except all-zeros)

### PLDs & Completeness Quick Revision
- ROM: fixed AND (full decoder) + programmable OR. Implements any function!
- PLA: programmable AND + programmable OR. Most flexible.
- PAL: programmable AND + fixed OR. Most common.
- FPGA: CLBs (LUTs + FFs) connected by programmable routing
- Post's 5 classes: T₀, T₁, S, M, L. Complete iff breaks all 5.
- NAND/NOR: single gate can implement everything (universal)

## 🎯 Common Mistakes in GATE — Avoid These! ⚠️

### Mistake 1: Forgetting K-map wrap-around
Students often miss that K-map edges are adjacent. In a 4-variable K-map:
- Row 00 is adjacent to Row 10 (top-bottom wrap)
- Column 00 is adjacent to Column 10 (left-right wrap)
- All four corners (00-00, 00-10, 10-00, 10-10) form a valid group!

### Mistake 2: Wrong don't care handling
- Don't cares can be 0 or 1 for simplification
- You CAN include don't cares in groups to make them larger
- You do NOT need to cover all don't cares
- Don't care handling is INDEPENDENT for F and F' (same don't cares for both!)

### Mistake 3: Mixing up Mealy and Moore state counts
"Detect sequence X using minimum states" — Mealy always needs ≤ Moore states.
For pattern of length k:
- Mealy: k states minimum
- Moore: k+1 states minimum (extra state for detection output)

### Mistake 4: Overflow vs Carry
- **Carry out** ≠ **Overflow** in 2's complement!
- Carry out: indicates magnitude result doesn't fit in unsigned representation
- Overflow: indicates signed result is wrong (V = Cₙ₋₁ ⊕ Cₙ)
- Example: 1000 + 1000 = 0000 with carry=1 but V=0 (valid result −8 + (−8) = −16... wait, that IS overflow in 4-bit!)
  Actually: 1000(−8) + 1000(−8): Carry into MSB=0, Carry out=1, V=0⊕1=1 → overflow! ✅

### Mistake 5: LFSR initial state
- All-zeros state is NEVER part of the LFSR sequence (XOR of all zeros = zero → stuck!)
- Max length = 2ⁿ − 1 (all states except all-zeros)
- To include all-zeros: use XNOR feedback → max length includes all-zeros but NOT all-ones

### Mistake 6: IEEE 754 denormalized numbers
- When exponent field = all zeros: number is DENORMALIZED
- No implicit leading 1 (mantissa = 0.M, not 1.M)
- Effective exponent = 1 − bias (NOT 0 − bias!)
- This provides gradual underflow near zero

## 🧪 Digital Logic Lab Reference — IC Numbers (7400 Series) 🔌

For reference in certain GATE questions or lab-based questions:

| IC Number | Gate Type | Gates per IC | Inputs |
|---|---|---|---|
| 7400 | Quad 2-input NAND | 4 | 2 |
| 7402 | Quad 2-input NOR | 4 | 2 |
| 7404 | Hex Inverter (NOT) | 6 | 1 |
| 7408 | Quad 2-input AND | 4 | 2 |
| 7410 | Triple 3-input NAND | 3 | 3 |
| 7420 | Dual 4-input NAND | 2 | 4 |
| 7432 | Quad 2-input OR | 4 | 2 |
| 7447 | BCD to 7-segment (active LOW) | 1 | 4 |
| 7474 | Dual D FF (positive edge) | 2 | — |
| 7476 | Dual JK FF (negative edge) | 2 | — |
| 7483 | 4-bit Full Adder | 1 | — |
| 7485 | 4-bit Magnitude Comparator | 1 | — |
| 7486 | Quad 2-input XOR | 4 | 2 |
| 74138 | 3-to-8 Decoder | 1 | 3 |
| 74151 | 8:1 MUX | 1 | 3 select |
| 74153 | Dual 4:1 MUX | 2 | 2 select |
| 74163 | 4-bit Sync Counter (loadable) | 1 | — |
| 74164 | 8-bit SIPO Shift Register | 1 | — |
| 74194 | 4-bit Universal Shift Register | 1 | — |
| 74283 | 4-bit Full Adder (fast carry) | 1 | — |

> 💡 These IC numbers are rarely asked directly in GATE but knowing them helps visualize circuits and understand gate packaging.

## 📐 Mathematical Proofs for GATE ⭐

### Proof 1: Number of Self-Dual Functions = $2^{2^{n-1}}$

A function F is self-dual iff F(x) = F'(x̄) for all x, equivalently F(x) ≠ F(x̄).

For n variables, there are 2ⁿ input combinations, forming 2ⁿ/2 = 2ⁿ⁻¹ complementary pairs.
For each pair (x, x̄), we must have F(x) ≠ F(x̄), so choosing F(x) automatically determines F(x̄).

We have 2ⁿ⁻¹ independent choices, each with 2 options.
Total self-dual functions = $2^{2^{n-1}}$ ∎

### Proof 2: Number of Monotone Functions (Dedekind Numbers)

There is NO closed-form formula. The Dedekind numbers are:
| n | D(n) |
|---|---|
| 0 | 2 |
| 1 | 3 |
| 2 | 6 |
| 3 | 20 |
| 4 | 168 |

These grow super-exponentially. GATE typically only asks for n=2 or n=3.

### Proof 3: NAND/NOR are Complete

**NAND is complete:**
1. NOT a = a NAND a ✅
2. a AND b = NOT(a NAND b) = (a NAND b) NAND (a NAND b) ✅
3. a OR b = NOT(NOT a AND NOT b) = (a NAND a) NAND (b NAND b) ✅
Since we can make NOT, AND, OR → complete! ∎

**NOR is complete (by duality):**
1. NOT a = a NOR a ✅
2. a OR b = NOT(a NOR b) ✅
3. a AND b = (a NOR a) NOR (b NOR b) ✅ ∎

## 📊 Year-Wise GATE PYQ Analysis ⭐

| Year | Set | Topic | Marks | Question Type |
|---|---|---|---|---|
| 2025 Set 1 | K-map minimization | 2 | Find minimum SOP |
| 2025 Set 1 | Self-dual functions | 1 | Count self-dual |
| 2025 Set 2 | Sequential circuit | 2 | State diagram analysis |
| 2025 Set 2 | Number system | 1 | 2's complement |
| 2024 Set 1 | MUX implementation | 2 | Implement with 8:1 MUX |
| 2024 Set 1 | Functional completeness | 1 | Post's theorem |
| 2024 Set 2 | Flip-flop | 1 | Characteristic equation |
| 2024 Set 2 | K-map | 2 | Count prime implicants |
| 2023 | Sequence detector | 2 | Design Mealy machine |
| 2023 | Hamming code | 1 | Find error position |
| 2022 | Boolean simplification | 1 | Algebraic method |
| 2022 | Counter design | 2 | Mod-N with JK FFs |
| 2021 | CRC | 2 | Polynomial division |
| 2021 | IEEE 754 | 1 | Convert to float |
| 2020 | Decoder | 1 | Decoder tree |
| 2020 | State minimization | 2 | Implication table |
| 2019 | XOR properties | 1 | Parity function |
| 2019 | Adder delay | 2 | CLA vs RCA comparison |

> 💡 **Trend Analysis:** K-map/minimization is the MOST frequently asked topic (almost every year). MUX implementation and sequential design are the next most common 2-mark questions.

## 🎓 Study Strategy for Digital Logic

### Priority Order (based on PYQ frequency):
1. 🔴 **HIGH:** K-map minimization (PI/EPI/don't cares) — Practice 10+ problems
2. 🔴 **HIGH:** MUX function implementation — 8:1 and 4:1 methods
3. 🔴 **HIGH:** Self-dual/linear/total function counting formulas
4. 🟡 **MEDIUM:** 2's complement arithmetic and overflow
5. 🟡 **MEDIUM:** Hamming code encoding/decoding
6. 🟡 **MEDIUM:** Sequence detector design
7. 🟡 **MEDIUM:** Counter design (Mod-N, Ring, Johnson)
8. 🟢 **LOW:** CRC polynomial division
9. 🟢 **LOW:** Post's theorem completeness check
10. 🟢 **LOW:** Hazard detection and elimination

### Time Allocation:
- Boolean Algebra + Minimization: 40% of study time
- Number Systems + Error Codes: 15%
- Combinational Circuits: 20%
- Sequential Circuits: 25%

### Common Exam-Day Mistakes to Avoid:
1. Forgetting Gray code ordering in K-maps (00, 01, **11**, 10)
2. Missing wrap-around adjacencies in K-maps
3. Confusing NAND-SR (active LOW) with NOR-SR (active HIGH) invalid conditions
4. Computing 2's complement overflow incorrectly (V = C_in ⊕ C_out at MSB, NOT just carry out!)
5. Mixing up Mealy (output on transition) and Moore (output on state) in state diagrams
6. Forgetting that Moore machine needs 1 more state than equivalent Mealy
7. Not using don't cares properly in counter design (unused states = don't care)
8. Confusing PLA (both programmable) with PAL (only AND programmable)
9. Forgetting that Ring counter is NOT self-starting
10. Assuming LFSR includes all-zeros state (it doesn't!)

## 🧮 Last-Minute One-Page Revision Sheet

### Boolean Algebra Essentials
- **Absorption:** a + ab = a, a(a + b) = a
- **Consensus:** ab + a'c + bc = ab + a'c (bc is consensus term — REMOVE IT)
- **XOR trick:** a ⊕ 1 = a', a ⊕ 0 = a, a ⊕ a = 0, a ⊕ a' = 1
- **Self-dual count:** 2^(2^(n-1)), for n=3: 16, for n=4: 256
- **Threshold function count:** n=2: 5, n=3: 14
- **Total Boolean functions of n vars:** 2^(2^n)

### K-Map Shortcuts
- **Group of 2^k** = eliminates k variables from the term
- **PI count trick:** In n-variable K-map with 2^n minterms, only 1 PI exists (= tautology)
- **EPI test:** If a minterm appears in exactly ONE PI, that PI is essential
- **POS K-map:** Group the 0s, complement the result (or use maxterm K-map)

### Number System Quick Facts
- **2's complement range (n bits):** -2^(n-1) to 2^(n-1) - 1
- **1's complement range (n bits):** -(2^(n-1) - 1) to 2^(n-1) - 1
- **BCD addition:** If sum > 9 or carry = 1, add 0110 (6)
- **IEEE 754 bias:** Single = 127, Double = 1023, Half = 15
- **Gray code MSB** = Binary MSB, Gray(i) = Binary(i) ⊕ Binary(i+1)
- **Gray to Binary:** Binary MSB = Gray MSB, Binary(i) = Binary(i+1) ⊕ Gray(i)

### Combinational Circuit Formulas
- **RCA delay (n-bit):** 2n gate delays (each full adder = 2 gate delays for carry)
- **CLA delay:** 4 gate delays regardless of n (for single-level CLA)
- **n-bit magnitude comparator:** Uses cascading; MSB comparison has highest priority
- **2^n:1 MUX** can implement ANY n-variable function
- **2^(n-1):1 MUX** can implement ANY n-variable function (using complement technique)
- **2^(n-2):1 MUX** can implement SOME n-variable functions (residue method)

### Sequential Circuit Formulas
- **Async counter max freq:** f_max = 1 / (n × t_pd) where n = number of flip-flops
- **Ring counter states:** n (uses n flip-flops)
- **Johnson counter states:** 2n (uses n flip-flops)
- **LFSR states (max):** 2^n - 1 (excludes all-zeros)
- **Mod-N counter flip-flops needed:** ⌈log₂(N)⌉
- **Mealy → Moore:** May add states; Moore → Mealy: May reduce states
- **One-hot encoding:** n states → n flip-flops; Binary: n states → ⌈log₂(n)⌉ flip-flops

### Error Detection/Correction Formulas
- **Hamming rule:** 2^r ≥ m + r + 1
- **Hamming distance d:**
  - Detect up to (d-1) errors
  - Correct up to ⌊(d-1)/2⌋ errors
- **SECDED (Hamming + overall parity):** d = 4; detect 2, correct 1
- **CRC with degree-r polynomial:** Detects ALL burst errors of length ≤ r

### PLD Quick Reference
- **ROM:** 2^n × m bits (n inputs, m outputs, full decoder)
- **PLA:** AND array fuses + OR array fuses (both programmable)
- **PAL:** AND array fuses + OR array fixed
- **FPGA k-input LUT:** Can implement any function of k variables (stores 2^k bits)

### Timing Quick Reference
- **Setup time (t_su):** Data must be stable BEFORE clock edge
- **Hold time (t_h):** Data must be stable AFTER clock edge
- **Max clock frequency:** f_max = 1 / (t_pd + t_su + t_skew)
- **Clock skew (positive):** Receiving FF gets clock LATER → may cause hold violation
- **Clock skew (negative):** Receiving FF gets clock EARLIER → reduces setup margin
- **Metastability:** Occurs when setup/hold violated; MTBF = e^(t_r/τ) / (T₀ × f_clk × f_data)

## 🔑 Must-Remember Constants and Magic Numbers

| Constant | Value | Context |
|---|---|---|
| NAND/NOR gate for NOT | 1 gate (tie inputs) | Universal gate implementation |
| Gates for 2-input AND from NAND | 2 NAND gates | NAND-NAND implementation |
| Gates for 2-input OR from NAND | 3 NAND gates | Requires complement + NAND |
| Half adder gate count | 1 XOR + 1 AND = 2 gates | Basic building block |
| Full adder from half adders | 2 HA + 1 OR = 5 gates | Standard construction |
| Full adder transistors (CMOS) | 28 transistors | Static CMOS |
| Inverter transistors (CMOS) | 2 (1 NMOS + 1 PMOS) | Simplest gate |
| 2-input NAND transistors | 4 (2 NMOS + 2 PMOS) | Universal gate |
| SR latch gates | 2 NOR or 2 NAND | Cross-coupled |
| D flip-flop gates | ~6 NAND gates (master-slave) | Positive edge-triggered |
| JK flip-flop gates | ~8 NAND gates | With toggle capability |

## ⚡ Quick Differentiation Table (Commonly Confused Pairs)

| Concept A | Concept B | Key Difference |
|---|---|---|
| Combinational | Sequential | No memory vs has memory (FFs) |
| Latch | Flip-Flop | Level-sensitive vs edge-triggered |
| Mealy | Moore | Output depends on input+state vs state only |
| Synchronous | Asynchronous | Common clock vs no common clock |
| NAND | NOR | Both universal; NAND has S=R=0 invalid, NOR has S=R=1 invalid |
| PLA | PAL | Both arrays programmable vs only AND programmable |
| Encoder | Decoder | Many-to-few vs few-to-many |
| MUX | DEMUX | Many-to-one vs one-to-many |
| Ring counter | Johnson counter | n states vs 2n states for n FFs |
| Gray code | Binary code | Only 1 bit changes vs multiple bits may change |
| 1's complement | 2's complement | Two zeros vs one zero; 2's is standard |
| Setup time | Hold time | Before clock edge vs after clock edge |

---

*End of Comprehensive Notes — Digital Logic for GATE 2027* 🎓🏆
