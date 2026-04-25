# Digital Logic — Revision Notes for GATE 2027

> **Quick-reference sheet** — review before exam | **GATE Weightage:** 3–5 marks

---

## 1. Boolean Algebra Cheat Sheet

| Law | AND | OR |
|-----|-----|-----|
| Identity | A·1=A | A+0=A |
| Null | A·0=0 | A+1=1 |
| Complement | A·A'=0 | A+A'=1 |
| Idempotent | A·A=A | A+A=A |
| Absorption | A(A+B)=A | A+AB=A |
| Distributive | A(B+C)=AB+AC | A+BC=(A+B)(A+C) |
| De Morgan | (AB)'=A'+B' | (A+B)'=A'B' |
| Consensus | AB+A'C+BC=AB+A'C | (A+B)(A'+C)(B+C)=(A+B)(A'+C) |

**XOR:** A⊕B = A'B+AB'. Odd-1 detector. A⊕0=A, A⊕1=A', A⊕A=0, A⊕A'=1.

**Dual:** Swap AND↔OR, 0↔1 (DON'T complement variables).
**Self-dual:** F = Fᵈ. Count: 2^(2^(n-1)) for n variables.

---

## 2. K-Map Quick Rules

- Gray code ordering: 00, 01, **11**, 10
- Groups: powers of 2 (1, 2, 4, 8, 16)
- Wrap-around: top↔bottom, left↔right
- **Don't cares:** use as 1 to maximize groups
- **EPI:** PI that uniquely covers at least one minterm
- SOP: group 1s → products. POS: group 0s → sums.
- Conversion: F=Σm(list) ↔ F=ΠM(complement list)

---

## 3. Number Systems

| System | Range (n-bit signed) | Notes |
|--------|---------------------|-------|
| Sign-mag | ±(2ⁿ⁻¹−1) | Two zeros |
| 1's comp | ±(2ⁿ⁻¹−1) | Two zeros |
| **2's comp** | **−2ⁿ⁻¹ to 2ⁿ⁻¹−1** | **One zero, used in CPUs** |

**2's complement:** Negate → invert all bits + add 1.
**Overflow:** Carry_in to MSB ⊕ Carry_out of MSB = 1 → overflow.

**Gray ↔ Binary:**
- B→G: G[i] = B[i] ⊕ B[i+1], MSB same
- G→B: B[i] = G[i] ⊕ B[i+1], process from MSB

**BCD:** Each decimal digit = 4 bits. Invalid: 1010–1111.

---

## 4. Combinational Circuits

### Adders
| Type | Carry Delay |
|------|-------------|
| Half Adder | S=A⊕B, C=AB |
| Full Adder | S=A⊕B⊕Cᵢₙ, Cₒᵤₜ=AB+(A⊕B)Cᵢₙ |
| n-bit Ripple | **2n gate delays** |
| n-bit CLA | **O(1)** — constant |

**CLA formulas:** Gᵢ=AᵢBᵢ, Pᵢ=Aᵢ⊕Bᵢ, Cᵢ₊₁=Gᵢ+PᵢCᵢ

### MUX
- 2ⁿ:1 MUX = n select lines, 2ⁿ data inputs
- **n-var function with 2ⁿ:1:** minterms directly to inputs
- **n-var function with 2ⁿ⁻¹:1:** n−1 vars as select, express in terms of remaining
- 16:1 from 2:1 MUX: need **15** MUXes

### Decoder
- n:2ⁿ decoder: each output = one minterm
- Any function: OR the minterm outputs
- 5:32 from 3:8: need **5** decoders

---

## 5. Functional Completeness (Post's Theorem)

Must break ALL 5 classes:

| Class | Test |
|-------|------|
| T₀ (preserves 0) | f(0,...,0)=0? |
| T₁ (preserves 1) | f(1,...,1)=1? |
| S (self-dual) | f=fᵈ? |
| M (monotone) | No complementation? |
| L (linear) | f = XOR of variables? |

**Universal sets:** {NAND}, {NOR}, {AND,NOT}, {OR,NOT}
**NOT complete:** {AND,OR} (both M), {XOR,AND} (both T₀), {→} alone (T₁)

---

## 6. Flip-Flop Summary

### Characteristic Equations
| FF | Equation | Key Property |
|----|----------|-------------|
| SR | Q⁺ = S + R'Q (SR=0) | Set/Reset |
| **D** | **Q⁺ = D** | **Delay/Data** |
| **JK** | **Q⁺ = JQ' + K'Q** | **Universal FF** |
| **T** | **Q⁺ = T⊕Q** | **Toggle** |

### Excitation Table (MUST MEMORIZE)
| Q→Q⁺ | SR | D | JK | T |
|-------|-----|---|-----|---|
| 0→0 | 00 | 0 | 0X | 0 |
| 0→1 | 10 | 1 | 1X | 1 |
| 1→0 | 01 | 0 | X1 | 1 |
| 1→1 | 00 | 1 | X0 | 0 |

### Conversions
- JK→D: J=D, K=D'
- D→T: D=T⊕Q
- JK→T: J=K=T

### Timing
- f_max = 1/(tₚ + tₛ)
- Setup time: data stable BEFORE edge
- Hold time: data stable AFTER edge

---

## 7. Mealy vs Moore

| | Mealy | Moore |
|---|-------|-------|
| Output | f(state, input) | f(state) |
| States | Fewer | More (≤n×m) |
| Response | Immediate | 1 clock delayed |

---

## 8. Counters

| Counter | Mod | FFs needed | Speed |
|---------|-----|-----------|-------|
| Ring (n FF) | n | n | No extra logic |
| Johnson (n FF) | **2n** | n | Simple decode |
| Ripple (n FF) | 2ⁿ | n | Slow: n×tₚ |
| Synchronous | 2ⁿ | n | Fast: 1×tₚ |
| Mod-N (async) | N | ⌈log₂N⌉ | Reset glitch |

**Async counter max freq:** f_max = 1/(n × tₚ)

---

## 9. Critical GATE Formulas

| Formula | Value |
|---------|-------|
| Self-dual functions (n vars) | 2^(2^(n-1)) |
| Minterms for n vars | 2ⁿ |
| Boolean functions for n vars | 2^(2ⁿ) |
| 2's complement range (n bits) | −2ⁿ⁻¹ to 2ⁿ⁻¹−1 |
| MUX: 2ⁿ:1 from 2:1 | 2ⁿ−1 MUXes |
| CLA carry | Cᵢ₊₁ = Gᵢ + PᵢCᵢ |
| Johnson mod | 2n (n = #FFs) |
| Ring mod | n (n = #FFs) |

---

## 10. Common GATE Traps

1. **K-map column order** → Gray code, NOT binary
2. **Don't cares** → treat as 0 or 1 to maximize groups
3. **2's complement −2ⁿ⁻¹** → has no positive counterpart!
4. **Overflow** → only when same-sign addition gives opposite sign
5. **EPI** → must uniquely cover at least one minterm
6. **JK toggle** → J=K=1 gives Q⁺=Q' (not invalid like SR)
7. **Mealy→Moore** → max states multiply by output alphabet size
8. **Ripple counter glitch** → transient states exist during propagation

---

*End of Revision Notes — Digital Logic for GATE 2027*
