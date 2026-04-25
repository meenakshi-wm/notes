# Digital Logic — Solutions for GATE 2027

> Complete solutions for all 70 questions from the Questions & PYQs file.

---

## Section A: Boolean Algebra & Laws

### Q1. Simplify F = AB + AB' + A'B
F = A(B + B') + A'B = A·1 + A'B = A + A'B = A + B (by absorption: A + A'B = A + B)

**Answer: F = A + B**

---

### Q2. Complement of F = (A + B)(A'C + B')
Apply De Morgan's:
F' = (A + B)' + (A'C + B')' = A'B' + (A'C)'(B')' = A'B' + (A + C')B = A'B' + AB + BC'

**Answer: F' = A'B' + AB + BC'**

---

### Q3. Dual of F = AB + A'C + 0
Swap AND↔OR and 0↔1 (don't complement variables):
Fᵈ = (A+B)(A'+C)·1 = (A+B)(A'+C)

**Answer: Fᵈ = (A+B)(A'+C)**

---

### Q4. Simplify F = A'BC + A'BC' + AB'C + AB'C' + ABC
F = A'B(C + C') + AB'(C + C') + ABC
= A'B + AB' + ABC
= A'B + A(B' + BC)
= A'B + A(B' + C) [absorption: B' + BC = B' + C]
= A'B + AB' + AC

**Answer: F = A'B + AB' + AC = A⊕B + AC**

---

### Q5. Prove: AB + A'C + BC = AB + A'C
LHS = AB + A'C + BC
= AB + A'C + BC(A + A')  [since A + A' = 1]
= AB + A'C + ABC + A'BC
= AB(1 + C) + A'C(1 + B)
= AB + A'C = RHS ✓

This is the **Consensus Theorem.** BC is the consensus term and is redundant.

---

### Q6. F(A,B,C) = A + B'C in canonical SOP
Expand each term to include all variables:
A = A(B+B')(C+C') = ABC + ABC' + AB'C + AB'C' → m₇ + m₆ + m₅ + m₄
B'C = B'C(A+A') = AB'C + A'B'C → m₅ + m₁

F = Σm(1,4,5,6,7)

**Answer: F = Σm(1,4,5,6,7)**

---

### Q7. Convert F = Σm(0,2,4,5,6) to POS
The maxterms are the missing minterms: {1, 3, 7}
F = ΠM(1,3,7)
= (A+B+C')(A+B'+C')(A'+B'+C')

**Answer: F = ΠM(1,3,7) = (A+B+C')(A+B'+C')(A'+B'+C')**

---

### Q8. F = A'C + B'C' + A'B
Apply consensus theorem: A'B is the consensus of A'C and B'C'.
Check: A'C + B'C'. The resolution variable? A'C has A' and C. B'C' has B' and C'.
Actually let's verify by truth table or simplification.

A'C + B'C' + A'B. Try: A'B = redundant?
A'C covers: A=0, C=1 → (001), (011)
B'C' covers: B=0, C=0 → (000), (100)
A'B covers: A=0, B=1 → (010), (011)

Union: {000, 001, 010, 011, 100} = Σm(0,1,2,3,4)
A'C + B'C' covers: {001, 011} ∪ {000, 100} = {000, 001, 011, 100} — missing m₂ (010)!

So A'B is NOT redundant here. Let's check: A'C + A'B = A'(B+C). And B'C' covers the rest.
F = A'(B+C) + B'C'. Let's check which option matches.

(a) A'C + B'C': misses m₂ → ✗
(b) A'B + B'C': Σ = {010, 011, 000, 100} = misses m₁ → ✗ 
(d) A'C + AB': wait check... this is {001,011} ∪ {100,110} — doesn't match.

The original covers {0,1,2,3,4}. So (a) is missing 010. None of the reduced forms have 3 terms. The answer must be the one that also gives {0,1,2,3,4}.

Actually re-examining: the original = F = A'C + B'C' + A'B covers m₀,m₁,m₂,m₃,m₄, which equals A'+B'C' = A'(1)+B'C'? A' = Σm(0,1,2,3). B'C' adds m₄. So F = A' + B'C'.

Check: (a) A'C + B'C' = {001,011} ∪ {000,100} = {0,1,3,4} ≠ {0,1,2,3,4} ✗
None match exactly. But the question says "equivalent" — so F simplifies to A' + B'C'.

**Answer: F = A' + B'C'** (none of the given options if exact; closest interpretation often credits (a) with different expansion)

---

### Q9. Self-dual functions with 3 variables
For n variables: 2^(2^(n-1)) self-dual functions.
n = 3: 2^(2²) = 2⁴ = **16**

**Answer: 16 self-dual functions**

---

### Q10. Simplify F = (A⊕B)C + (A⊕B)'C' + AC
Let X = A⊕B. Then F = XC + X'C' + AC = (X⊙C) + AC = (X XNOR C) + AC.

X⊙C = XC + X'C' = (A⊕B)C + (A⊕B)'C'.

Expand: (A⊕B)C = (AB'+A'B)C = AB'C + A'BC
(A⊕B)'C' = (AB+A'B')C' = ABC' + A'B'C'

F = AB'C + A'BC + ABC' + A'B'C' + AC
= A'B'C' + A'BC + AB'C + ABC' + AC(B+B')
= A'B'C' + A'BC + AB'C + ABC' + ABC + ABC'
Wait, AC = ABC + AB'C.

F = A'B'C' + A'BC + AB'C + ABC' + ABC + AB'C [AB'C already there]
= A'B'C' + A'BC + AB'C + ABC' + ABC
= A'(B'C' + BC) + A(B'C + BC' + BC)
= A'(B⊙C) + A(B'C + B) [since BC' + BC = B]
= A'(B⊙C) + A(B + C) [B + B'C = B + C]
= A'(B⊙C) + AB + AC

Hmm, let me try K-map. Minterms of F:
A'B'C'(1), A'B'C(0), A'BC'(0), A'BC(1), AB'C'(0), AB'C(1), ABC'(1), ABC(1)
= Σm(0, 3, 5, 6, 7)

K-map:
```
       BC=00 01 11 10
A=0  |  1  | 0 | 1 | 0 |
A=1  |  0  | 1 | 1 | 1 |
```
Groups: {3,7} = BC, {5,7} = AC, {6,7} = AB, {0,?} — m₀ alone needs A'B'C'.
Wait: {0} pairs with nothing directly. But group {0,2}? m₂ = A'BC' = 0. No.

**F = A'B'C' + AC + AB + BC**? Check: this has 4 terms.
Actually: {5,7} = AC. {6,7} = AB. {3,7} = BC. And m₀ = A'B'C' stands alone.
So F = A'B'C' + AB + AC + BC.

Note AB+AC+BC = majority function. F = A'B'C' + AB+AC+BC.
Or F = A'B'C' + (at least 2 of A,B,C are 1).

**Answer: F = A'B'C' + AB + AC + BC**

---

## Section B: K-Map and Minimization

### Q11. F(A,B,C) = Σm(0,2,4,6)
These are all minterms where C=0:
```
       BC=00 01 11 10
A=0  |  1  | 0 | 0 | 1 |
A=1  |  1  | 0 | 0 | 1 |
```
One group of 4: {0,2,4,6} = **C'**

**Answer: F = C'**

---

### Q12. F(A,B,C) = Σm(1,3,5,7)
All minterms where C=1:

**Answer: F = C**

---

### Q13. F(A,B,C,D) = Σm(0,1,2,5,8,9,10) + d(3,7,11,15)
```
        CD=00 01 11 10
AB=00 |  1  | 1 | d | 1 |
AB=01 |  0  | 1 | d | 0 |
AB=11 |  0  | 0 | d | 0 |
AB=10 |  1  | 1 | d | 1 |
```
Groups:
- {0,1,2,3,8,9,10,11} = B' (using don't cares d3, d11) — covers m₀,m₁,m₂,m₈,m₉,m₁₀ with d₃,d₁₁
- {1,3,5,7} = D (using don't cares d3, d7) — covers m₁,m₅ with d₃,d₇
- {3,7,11,15} = CD — all don't cares, optional

Wait, let me recheck. m₅ is at AB=01, CD=01. We need to cover m₅.
{1,5,3,7} = C'D... no. {1,5} in column CD=01: AB=00 and AB=01 → A'D
Actually {1,3,5,7}: all CD columns with D=1 from AB=00 and AB=01 → A'D.

B'(group {0,1,2,3,8,9,10,11}) covers m₀,m₁,m₂,m₈,m₉,m₁₀ + don't cares.  
A'D(group {1,3,5,7}) covers m₁,m₅ + don't cares d₃,d₇.

All required minterms covered: {0,1,2,5,8,9,10} ✓

**Answer: F = B' + A'D**

---

### Q14. F(A,B,C,D) = Σm(4,5,6,7,12,13,14,15)
All minterms where B=1:
```
        CD=00 01 11 10
AB=00 |  0  | 0 | 0 | 0 |
AB=01 |  1  | 1 | 1 | 1 |
AB=11 |  1  | 1 | 1 | 1 |
AB=10 |  0  | 0 | 0 | 0 |
```

**Answer: F = B**

---

### Q15. F(A,B,C,D) = Σm(0,2,3,5,7,8,10,11,14,15)
```
        CD=00 01 11 10
AB=00 |  1  | 0 | 1 | 1 |
AB=01 |  0  | 1 | 1 | 0 |
AB=11 |  0  | 0 | 1 | 1 |
AB=10 |  1  | 0 | 1 | 1 |
```

Prime implicants:
- {0,2,8,10} = B'D' (covers m₀,m₂,m₈,m₁₀)
- {2,3,10,11} = B'C (covers m₂,m₃,m₁₀,m₁₁)
- {3,7} = A'CD (covers m₃,m₇)
- {5,7} = A'BD (covers m₅,m₇)
- {14,15} = ABC (covers m₁₄,m₁₅)
- {10,11,14,15} = AC (covers m₁₀,m₁₁,m₁₄,m₁₅)

EPIs: 
- B'D' is only PI covering m₀,m₈ → **EPI**
- A'BD is only PI covering m₅ → **EPI**
- AC is only PI covering m₁₄ → **EPI** (also covers m₁₅)

After EPIs: B'D' covers {0,2,8,10}, A'BD covers {5,7}, AC covers {10,11,14,15}
Remaining uncovered: m₃. Can use B'C or A'CD.

Minimum: **F = B'D' + A'BD + AC + B'C** (or F = B'D' + A'BD + AC + A'CD)

**Answer: EPIs are B'D', A'BD, AC. One additional PI (B'C or A'CD) needed.**

---

### Q16. Minimize POS: F(A,B,C,D) = ΠM(0,1,2,5,8,9,10)
Maxterms are where F=0. So F'=Σm(0,1,2,5,8,9,10).
Plot F' on K-map and minimize, then complement to get POS form.

Or directly: mark 0s in K-map for F.
```
        CD=00 01 11 10
AB=00 |  0  | 0 | 1 | 0 |
AB=01 |  0  | 0 | 1 | 1 |
AB=11 |  1  | 1 | 1 | 1 |
AB=10 |  0  | 0 | 1 | 0 |
```

Group the 0s for F': {0,1,8,9} = B'C', {0,2,8,10} = B'D', {1,5} = A'C'D
F' = B'C' + B'D' + A'C'D
F = (F')' = (B+C)(B+D)(A+C+D') [by De Morgan's]

**Answer: F = (B+C)(B+D)(A+C+D')**

---

### Q17. [GATE 2015] F(w,x,y,z) = Σm(0,1,2,3,7,8,10)
```
        yz=00 01 11 10
wx=00 |  1  | 1 | 1 | 1 |  
wx=01 |  0  | 0 | 1 | 0 |
wx=11 |  0  | 0 | 0 | 0 |
wx=10 |  1  | 0 | 0 | 1 |
```

PIs:
- {0,1,2,3} = w'x' — EPI (only cover for m₁, m₃)
- {0,2,8,10} = x'z' — EPI (only cover for m₈, m₁₀)
- {3,7} = x'yz... wait. m₃=wx=00,yz=11. m₇=wx=01,yz=11. → {3,7} = w'yz. This covers m₇.

After EPIs w'x' and x'z': covered = {0,1,2,3,8,10}. Remaining: m₇.
{3,7} = w'yz covers m₇. This is the only PI covering m₇ → **EPI**.

**Answer: 3 essential prime implicants**

---

### Q18. [GATE 2016] F(A,B,C,D) = Σm(0,2,5,7,8,10,13,15)
```
        CD=00 01 11 10
AB=00 |  1  | 0 | 0 | 1 |
AB=01 |  0  | 1 | 1 | 0 |
AB=11 |  0  | 1 | 1 | 0 |
AB=10 |  1  | 0 | 0 | 1 |
```

Pattern: F = BD + B'D'. Let's verify PIs.
{0,2,8,10} = B'D' (size 4)
{5,7,13,15} = BD (size 4)

These are the only 2 PIs and both are essential.

**Answer: (a) 2**

---

### Q19. F(A,B,C,D) = Σm(1,3,4,5,6,7,9,11,12,13,14,15)
F' = Σm(0,2,8,10) = B'D'
F = (B'D')' = B + D

SOP: F = B + D (2 literals)
POS: F = B + D (same, already a sum)

**Answer: F = B + D (minimal with 2 literals)**

---

### Q20. F(A,B,C,D) = Σm(0,2,5,7,8,10,13,15)
From Q18: Only 2 PIs exist: B'D' and BD. Both essential.
Only **1 minimum SOP expression** exists: F = B'D' + BD = B⊙D

**Answer: 1 minimum SOP expression**

---

## Section C: Number Systems & Codes

### Q21. Convert (247)₈ to binary and hex
2₈ = 010, 4₈ = 100, 7₈ = 111
Binary: **010 100 111** = 10100111

Hex: 1010 0111 = **A7₁₆**

**Answer: Binary = 10100111, Hex = A7**

---

### Q22. −35 in 8-bit representations
35 = 0010 0011

(a) **Sign-magnitude:** 1010 0011
(b) **1's complement:** 1101 1100
(c) **2's complement:** 1101 1100 + 1 = **1101 1101**

---

### Q23. 1101.101₂ to decimal
= 1×2³ + 1×2² + 0×2¹ + 1×2⁰ + 1×2⁻¹ + 0×2⁻² + 1×2⁻³
= 8 + 4 + 0 + 1 + 0.5 + 0 + 0.125 = **13.625**

---

### Q24. 45 − 78 using 8-bit 2's complement
45 = 0010 1101
78 = 0100 1110, −78 in 2's: 1011 0010

45 + (−78): 0010 1101 + 1011 0010 = 1101 1111

Carry out of MSB: 0. Carry into MSB: 0. So no overflow.
Result: 1101 1111 → negative → magnitude = 0010 0001 = 33 → **result = −33** ✓ (45−78=−33)

**Answer: 1101 1111 = −33, no overflow**

---

### Q25. 12-bit 2's complement range
Range: −2¹¹ to 2¹¹−1 = **−2048 to +2047**

---

### Q26. Binary 10110111 to Gray code
B: 1 0 1 1 0 1 1 1
G: 1, 1⊕0=1, 0⊕1=1, 1⊕1=0, 1⊕0=1, 0⊕1=1, 1⊕1=0, 1⊕1=0
Wait, Gray: G₇=B₇, G₆=B₇⊕B₆, G₅=B₆⊕B₅, ...

G₇ = 1
G₆ = 1⊕0 = 1
G₅ = 0⊕1 = 1
G₄ = 1⊕1 = 0
G₃ = 1⊕0 = 1
G₂ = 0⊕1 = 1
G₁ = 1⊕1 = 0
G₀ = 1⊕1 = 0

**Gray code: 11101100**

Reverse (Gray to Binary): B₇=G₇=1, B₆=B₇⊕G₆=1⊕1=0, B₅=B₆⊕G₅=0⊕1=1, B₄=B₅⊕G₄=1⊕0=1, B₃=B₄⊕G₃=1⊕1=0, B₂=B₃⊕G₂=0⊕1=1, B₁=B₂⊕G₁=1⊕0=1, B₀=B₁⊕G₀=1⊕0=1
Binary: **10110111** ✓

---

### Q27. BCD of 397 and validity check
3 → 0011, 9 → 1001, 7 → 0111
BCD: **0011 1001 0111**

1010 0110: first nibble 1010 = 10 (invalid BCD digit!), second nibble 0110 = 6 (valid).
**Not valid BCD** since 1010 > 1001.

---

### Q28. 8-bit addition overflow check
A = 0110 0011 = 99
B = 0101 1010 = 90
A+B: 99+90 = 189 > 127 (max for 8-bit signed)

Binary: 0110 0011 + 0101 1010 = 1011 1101

Carry into MSB: 1. Carry out of MSB: 0. Since they differ → **Overflow!**

**Answer: Result = 1011 1101, Overflow = Yes (two positives gave negative)**

---

### Q29. Excess-3 of 47
4 → 4+3 = 7 = 0111, 7 → 7+3 = 10 = 1010

**Answer: 0111 1010 in Excess-3**

---

### Q30. −43 in 8-bit 2's complement
43 = 0010 1011
Complement: 1101 0100
Add 1: **1101 0101**

**Answer: 1101 0101 = D5 in hex**

---

## Section D: Combinational Circuits

### Q31. Half Adder equations
**Sum = A ⊕ B, Carry = A · B**

---

### Q32. 5:32 decoder from 3:8 decoders
A 5:32 decoder needs 32 outputs. Use 2 high-order bits (A₄A₃) as enable signals.
Top-level: one 2:4 decoder (or part of a 3:8) to generate 4 enable signals.
Each enable activates one 3:8 decoder.

Total: **4 × (3:8 decoders)** for outputs + **1 × (2:4 decoder)** for enables.
If built with 3:8 only: 4 + 1 = **5 decoders** (one 3:8 used as 2:4 with one input fixed).

**Answer: 5 decoders (4 for outputs, 1 for enable generation)**

---

### Q33. F(A,B) = A'B + AB' = A⊕B using 2:1 MUX
With A as select:
- A=0: F = B → I₀ = B
- A=1: F = B' → I₁ = B'

**Answer: I₀ = B, I₁ = B', Select = A**

---

### Q34. F(A,B,C) = Σm(0,2,6,7) using 8:1 MUX
Connect: I₀=1, I₁=0, I₂=1, I₃=0, I₄=0, I₅=0, I₆=1, I₇=1

**Answer: I₀=I₂=I₆=I₇=1, all others = 0, with A,B,C as select**

---

### Q35. F(A,B,C) = Σm(1,3,5,6) using 4:1 MUX, A,B select, C data

| A | B | Minterms | C=0 | C=1 | F(C) | Input |
|---|---|----------|-----|-----|------|-------|
| 0 | 0 | m₀=0,m₁=1 | 0 | 1 | C | I₀=C |
| 0 | 1 | m₂=0,m₃=1 | 0 | 1 | C | I₁=C |
| 1 | 0 | m₄=0,m₅=1 | 0 | 1 | C | I₂=C |
| 1 | 1 | m₆=1,m₇=0 | 1 | 0 | C' | I₃=C' |

**Answer: I₀=I₁=I₂=C, I₃=C'**

---

### Q36. F(A,B,C) = Σm(0,3,5,6) using 3:8 decoder
Connect outputs 0, 3, 5, 6 to an OR gate.
F = m₀ + m₃ + m₅ + m₆

**Answer: F = D₀ + D₃ + D₅ + D₆ (OR of decoder outputs)**

---

### Q37. 4-bit ripple carry adder delay
Each full adder: carry delay = 2 gate delays (through AND-OR).
4 FAs in chain: carry propagation = 4 × 2 = 8 gate delays.
Final sum bit: +1 XOR delay.
**Total worst case for carry-out = 2×4 = 8 gate delays.**
(With initial XOR for first generate: 2×4 + 1 = 9 for sum.)

**Answer: 8 gate delays for carry output (2n for n-bit)**

---

### Q38. C₃ in CLA
C₁ = G₀ + P₀C₀
C₂ = G₁ + P₁G₀ + P₁P₀C₀
**C₃ = G₂ + P₂G₁ + P₂P₁G₀ + P₂P₁P₀C₀**

---

### Q39. BCD to Excess-3 converter
Add 3 (0011) to each BCD digit.
For 4-bit input ABCD → output WXYZ = ABCD + 0011.
Use a 4-bit adder with one input = BCD, other = 0011 (B₃=0, B₂=0, B₁=1, B₀=1).

---

### Q40. 2:1 MUX needed for 16:1 MUX
16:1 = two 8:1 → one 2:1 at top
8:1 = two 4:1 → one 2:1 at top (×2)
4:1 = two 2:1 → one 2:1 at top (×4)
2:1 = base (×8)

Count: 8 (bottom) + 4 + 2 + 1 = **15**

**Answer: 15 two-input MUXes**

---

### Q41. [GATE 2017] 4:1 MUX special case
Output = I₀·S₁'S₀' + I₁·S₁'S₀ + I₂·S₁S₀' + I₃·S₁S₀
= 1·S₁'S₀' + S₁'S₀·S₁'S₀ + 0 + S₁S₀'·S₁S₀

Wait: I₁ = S₁'S₀, I₃ = S₁S₀'.
= S₁'S₀' + (S₁'S₀)(S₁'S₀) + 0 + (S₁S₀')(S₁S₀)
= S₁'S₀' + S₁'S₀ + 0 [since S₁S₀'·S₁S₀ = S₁·S₀'·S₀ = 0]
= S₁'(S₀' + S₀) = S₁'

**Answer: F = S₁'**

---

### Q42. Priority encoder with D₁=D₃=1
D₃ has highest priority and D₃=1, so output encodes 3.
Output = **11** (binary for 3)

---

### Q43. CLA vs Ripple delay
Ripple carry (n-bit): O(n) gate delays — specifically 2n.
CLA (n-bit, single level): O(1) — constant, about 4 gate delays regardless of n.
**CLA is dramatically faster for large n.**

---

### Q44. ALU for add/subtract
Use XOR gates: B_i XOR SUB (control). When SUB=0: pass B_i (addition). When SUB=1: complement B_i, and set C_in=1 → 2's complement subtraction.

---

### Q45. 64-bit CLA with 4-bit blocks
16 four-bit CLA blocks → need another level.
Level 1: 4 gate delays (within each 4-bit block)
Level 2: 4 gate delays (group CLA for 16 groups)... but 16 groups need another level.
With 4-bit CLA at each level: 16 blocks → 4 group blocks → 1 super block.
3 levels total.

Delay: 2 (PG generation) + 2 (first CLA) + 2 (second CLA) + 2 (third CLA) + 1 (sum XOR) ≈ **8-10 gate delays** depending on model.

Common answer: **8 gate delays** for a two-level 64-bit CLA hierarchy.

---

## Section E: Functional Completeness

### Q46. NAND is universal
NOT: A NAND A = (A·A)' = A'
AND: (A NAND B) NAND (A NAND B) = ((AB)')' = AB
OR: (A NAND A) NAND (B NAND B) = A' NAND B' = (A'B')' = A+B ✓

---

### Q47. {AND, XOR} functional completeness?
Check Post's 5 classes:
- T₀: AND(0,0)=0 ✓, XOR(0,0)=0 ✓ → Both in T₀ → **NOT functionally complete!**

Since both functions preserve 0, we can never generate the constant 1 from all-zero inputs. Therefore {AND, XOR} is **NOT** functionally complete.

**Answer: No, {AND, XOR} is NOT functionally complete (both preserve 0).**

---

### Q48. {NOR, 1} functionally complete?
- T₀: NOR(0,0)=1 (∉ T₀) ✓, 1 ∉ T₀ ✓ → class broken
- T₁: NOR(1,1)=0 (∉ T₁) → broken
- S: NOR dual = NAND ≠ NOR → broken
- M: NOR is not monotone (increasing input can decrease output) → broken
- L: NOR(A,B) = (A+B)' = A'B' — not linear → broken

All 5 classes broken → **Yes, functionally complete**

---

### Q49. Functionally complete set
(a) {AND, OR}: both monotone → NOT complete
(b) {NAND}: universal gate → **complete** ✓
(c) {XOR, AND}: both preserve 0 → NOT complete
(d) {XOR, OR}: both preserve 0 → NOT complete

**Answer: (b) {NAND}**

---

### Q50. {→} functionally complete?
A→B = A'+B. Check Post's theorem:
- T₀: 0→0 = 1 (∉ T₀) → broken
- T₁: 1→1 = 1 (∈ T₁). Need to check: only one function, so if it's in T₁, class not broken. Hmm... but we can construct: A→0... wait, do we have constant 0?

With only →: We need constants from variables.
A→A = A'+A = 1 (constant 1!)
(A→A)→A... = 1→A = 1'+A = A
A→(A→A) = A→1 = A'+1 = 1

Getting NOT: we need A'. 
Consider: A→0. But we need 0 first.
0 from 1: need to check if we can get 0. A→A=1. 1→1=1. Can't get 0 from just →.

Actually: A→(A→B) = A→(A'+B) = A'+(A'+B) = A'+B. Hmm.
What about ((A→B)→A)→A = (A'+B)'→A... this is Peirce's law = tautology.

Actually, {→} with variables is NOT complete because we can't produce constant 0 (T₁ not broken: f(1,1,...,1)=1 always for →).

BUT {→, 0} IS functionally complete: A→0 = A'+0 = A' (gives NOT), and A'→B' = A+B (gives OR).

**Answer: {→} alone is NOT functionally complete (preserves 1). Need {→, ⊥(false)} for completeness.**

---

## Section F: Sequential Circuits

### Q51. Characteristic equations
- **JK:** Q(t+1) = JQ' + K'Q
- **D:** Q(t+1) = D
- **T:** Q(t+1) = T⊕Q = TQ' + T'Q

---

### Q52. D FF with D=Q', initial Q=0
Clock 1: D=Q'=1 → Q=1
Clock 2: D=Q'=0 → Q=0
Clock 3: D=Q'=1 → Q=1
Clock 4: D=Q'=0 → Q=0

**Answer: States cycle: 0 → 1 → 0 → 1 → 0 (toggles every clock = T flip-flop behavior)**

---

### Q53. Latch vs Flip-Flop
**Latch:** Level-sensitive, transparent while enable is active. Output follows input continuously.
**Flip-Flop:** Edge-sensitive, captures input only at clock edge. Not transparent.

---

### Q54. JK to D conversion
Need: Q(t+1) = D. JK gives: Q(t+1) = JQ'+K'Q.
Set J = D, K = D'. Then: DQ' + D·Q = DQ' + DQ = D(Q'+Q) = D ✓

**Answer: Connect J = D, K = D'**

---

### Q55. D to T conversion
Need: Q(t+1) = T⊕Q. D FF gives: Q(t+1) = D.
Set D = T⊕Q.

**Answer: Connect D = T ⊕ Q**

---

### Q56. Q(t+1) = XQ' + X'Q = X⊕Q
This is a T flip-flop with T = X (toggles when X=1, holds when X=0).

**Answer: T flip-flop behavior**

---

### Q57. Mealy machine for "101" detection
States: S₀ (initial), S₁ (seen 1), S₂ (seen 10)

| State | Input | Next State | Output |
|-------|-------|-----------|--------|
| S₀ | 0 | S₀ | 0 |
| S₀ | 1 | S₁ | 0 |
| S₁ | 0 | S₂ | 0 |
| S₁ | 1 | S₁ | 0 |
| S₂ | 0 | S₀ | 0 |
| S₂ | 1 | S₁ | **1** |

Output 1 when in S₂ and input=1 (detected "101"). Overlapping: goes to S₁ to catch next "10...".

---

### Q58. Race-around problem
In level-triggered JK (J=K=1 with enable high): output toggles repeatedly during the entire enable-high period → uncontrolled oscillations.

**Master-Slave solution:** Master captures on clock HIGH, slave transfers on clock LOW. Since master is isolated during slave transfer, no feedback oscillation occurs. Output changes only once per clock cycle.

---

### Q59. DA = A⊕B, DB = A, Initial A=1, B=0

| Clock | A | B | DA=A⊕B | DB=A | A⁺ | B⁺ |
|-------|---|---|--------|------|-----|-----|
| 0 | 1 | 0 | 1 | 1 | 1 | 1 |
| 1 | 1 | 1 | 0 | 1 | 0 | 1 |
| 2 | 0 | 1 | 1 | 0 | 1 | 0 |
| 3 | 1 | 0 | 1 | 1 | 1 | 1 |

After 4 clock pulses: **A=1, B=1**

---

### Q60. Moore machine for "1011" (overlapping)
States for Moore: S₀(ε), S₁(1), S₂(10), S₃(101), S₄(1011→output 1)
After detection (S₄), overlap: "1011" → suffix "1" matches start of pattern → go to S₁.
But S₄ outputs 1 and the "1" at end could be start of new "1...".

Need to also handle: from S₄ on input 0, the "...10110" has suffix "0" → S₀.
From S₄ on input 1, suffix "1" → S₁.

**Answer: 5 states minimum (S₀, S₁, S₂, S₃, S₄)**

---

### Q61. Mealy to Moore conversion
A Mealy machine with n states and output alphabet of m symbols can have at most **n × m** states in equivalent Moore machine (each Mealy state split by possible outputs).

**Answer: Maximum n × m states**

---

### Q62. T FF with T = Q⊕X, initial Q=0
| Step | X | Q | T=Q⊕X | Q⁺=T⊕Q |
|------|---|---|--------|---------|
| 1 | 1 | 0 | 1 | 1 |
| 2 | 0 | 1 | 1 | 0 |
| 3 | 1 | 0 | 1 | 1 |
| 4 | 1 | 1 | 0 | 1 |
| 5 | 0 | 1 | 1 | 0 |

**Outputs Q: 0 → 1 → 0 → 1 → 1 → 0**

---

### Q63. Two JK FFs: J₁=Q₂, K₁=Q₂', J₂=Q₁', K₂=Q₁
Q₁: J₁=Q₂, K₁=Q₂' → when Q₂=0: J₁=0,K₁=1 → Q₁⁺=0. When Q₂=1: J₁=1,K₁=0 → Q₁⁺=1.
So **Q₁⁺ = Q₂**

Q₂: J₂=Q₁', K₂=Q₁ → when Q₁=0: J₂=1,K₂=0 → Q₂⁺=1. When Q₁=1: J₂=0,K₂=1 → Q₂⁺=0.
So **Q₂⁺ = Q₁'**

| Clock | Q₁ | Q₂ | Q₁⁺ | Q₂⁺ |
|-------|----|----|------|------|
| 0 | 0 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 | 1 |
| 2 | 1 | 1 | 1 | 0 |
| 3 | 1 | 0 | 0 | 0 |
| 4 | 0 | 0 | → repeats |

Sequence (Q₁Q₂): 00 → 01 → 11 → 10 → 00 (Gray code counter!)

**Answer: 4-state Gray code counter**

---

### Q64. Max clock frequency
f_max = 1/(tₚ + tₛ) = 1/(5ns + 2ns) = 1/7ns ≈ **142.86 MHz**

---

### Q65. Setup time violation check
Setup time = 2ns means data must be stable 2ns before clock edge.
If data changes 1ns before → data stable for only 1ns < 2ns required.
**Yes, setup time is violated.**

---

## Section G: Counters

### Q66. Max frequency of 3-bit ripple counter
Max delay = n × tₚ = 3 × 10ns = 30ns
f_max = 1/30ns = **33.33 MHz**

---

### Q67. Mod-5 synchronous counter using T FFs
States: 000→001→010→011→100→000

| Q₂Q₁Q₀ | Q₂⁺Q₁⁺Q₀⁺ | T₂ | T₁ | T₀ |
|---------|-----------|----|----|-----|
| 000 | 001 | 0 | 0 | 1 |
| 001 | 010 | 0 | 1 | 1 |
| 010 | 011 | 0 | 0 | 1 |
| 011 | 100 | 1 | 1 | 1 |
| 100 | 000 | 1 | 0 | 0 |

T₀ = Q₂' (from K-map with don't cares at 101,110,111)
T₁ = Q₁Q₀ (toggle Q₁ when Q₁Q₀=11)
Hmm, let me use K-maps properly.

T₂: 1s at 011,100. Don't cares at 101,110,111.
T₁: 1s at 001,011. Don't cares at 101,110,111.
T₀: 1s at 000,001,010,011. Don't cares at 101,110,111.

With don't cares: T₀ = Q₂', T₁ = Q₀, T₂ = Q₁Q₀ + Q₂

**Answer: T₀ = Q₂', T₁ = Q₀, T₂ = Q₁Q₀ + Q₂**

---

### Q68. Ring counter with 4 FFs
A ring counter cycles through states where exactly one FF is 1:
1000 → 0100 → 0010 → 0001 → 1000

**Modulus = 4 (same as number of flip-flops)**
Unique states: **4**

---

### Q69. Johnson counter (4-bit), initial 0000
| Clock | Q₃Q₂Q₁Q₀ |
|-------|-----------|
| 0 | 0000 |
| 1 | 1000 |
| 2 | 1100 |
| 3 | 1110 |
| 4 | 1111 |
| 5 | 0111 |
| 6 | 0011 |
| 7 | 0001 |
| 8 | 0000 → repeats |

**Modulus = 2n = 2×4 = 8 states**

---

### Q70. Async counter 0 to 11
Counts 0–11 (12 states). Needs ⌈log₂12⌉ = **4 flip-flops**.
Reset when count = 12 = 1100.
Reset logic: detect Q₃Q₂ = 11 (since Q₃Q₂Q₁Q₀ = 1100).
**Reset = Q₃ · Q₂** (AND gate on the two MSBs, fed to async CLEAR).

Note: There will be a brief glitch at state 1100 before reset occurs.

**Answer: 4 FFs, Reset logic = Q₃ AND Q₂**

---

*End of Solutions — Digital Logic for GATE 2027*
