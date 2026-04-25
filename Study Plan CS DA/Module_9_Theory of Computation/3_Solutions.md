# Theory of Computation — Solutions for GATE 2027

> Complete solutions for all 70 questions.

---

## Section A: DFA & NFA

### Q1. DFA for even number of a's
States: {q₀(even), q₁(odd)}, start = q₀, final = {q₀}
- δ(q₀,a)=q₁, δ(q₀,b)=q₀
- δ(q₁,a)=q₀, δ(q₁,b)=q₁

**2 states.** q₀ is both start and final (ε has 0 a's = even).

---

### Q2. DFA for strings ending with "01"
States: {q₀, q₁, q₂}, start=q₀, final={q₂}
- q₀: haven't seen prefix. δ(q₀,0)=q₁, δ(q₀,1)=q₀
- q₁: last seen 0. δ(q₁,0)=q₁, δ(q₁,1)=q₂ 
- q₂: last two were 01. δ(q₂,0)=q₁, δ(q₂,1)=q₀

**3 states.**

---

### Q3. Minimum DFA states for "contains aba"
Track progress toward "aba": states for ε, a, ab, aba(final).
- q₀: no progress. On a→q₁, on b→q₀
- q₁: seen 'a'. On a→q₁, on b→q₂
- q₂: seen 'ab'. On a→q₃, on b→q₀
- q₃: seen 'aba' (final). On a→q₃, on b→q₃

**Answer: 4 states**

---

### Q4. NFA to DFA conversion
Start: {q₀}
- {q₀} on 0 → {q₀,q₁}, on 1 → {q₀}
- {q₀,q₁} on 0 → {q₀,q₁}, on 1 → {q₀,q₂}
- {q₀,q₂} on 0 → {q₀,q₁}, on 1 → {q₀} [q₂ is final → this is final]

DFA: 3 states. Final: {q₀,q₂}.
Language: strings over {0,1} where "01" appears as a substring (ending doesn't matter — accepts strings containing 01).

---

### Q5. Product construction L₁ ∩ L₂
L₁ (even 0s): A₀(even), A₁(odd). Final: A₀.
L₂ (odd 1s): B₀(even), B₁(odd). Final: B₁.

Product states: (A₀,B₀), (A₀,B₁), (A₁,B₀), (A₁,B₁)
Start: (A₀,B₀). Final for intersection: (A₀,B₁).

Transitions from (A₀,B₀): on 0→(A₁,B₀), on 1→(A₀,B₁)
(A₀,B₁): on 0→(A₁,B₁), on 1→(A₀,B₀)
(A₁,B₀): on 0→(A₀,B₀), on 1→(A₁,B₁)
(A₁,B₁): on 0→(A₀,B₁), on 1→(A₁,B₀)

**4 states, final = {(A₀,B₁)}**

---

### Q6. DFA Minimization
δ: A→(B,C), B→(D,C), C→(B,C), D→(D,E), E→(D,C). Final={D}.

Partition: {D} vs {A,B,C,E}

Check {A,B,C,E} on input 0: A→B, B→D, C→B, E→D
B and E both go to D (final), while A and C go to B (non-final) → split: {A,C} and {B,E}

Check {A,C} on input 0: A→B, C→B (same). On 1: A→C, C→C (same, both in {A,C}).
A and C are equivalent → merge.

Check {B,E} on input 0: B→D, E→D (same). On 1: B→C, E→C (same).
B and E are equivalent → merge.

**Minimized: 3 states — {A,C}, {B,E}, {D}**

---

### Q7. Max DFA states from 5-state NFA
**2⁵ = 32 states** (worst case)

---

### Q8. DFA for div 3 (0s) AND div 5 (1s)
States needed: 3 × 5 = **15 states** (product of counting modulo 3 and modulo 5)

---

### Q9. Equal 01 and 10 substrings — regular?
**Yes, L is regular.** Every string w has #(01) and #(10) differing by at most 1, and they're equal iff the string starts and ends with the same symbol. This condition is regular.

---

### Q10. Strings accepted by 2-state DFA
**(c) Depends on DFA.** Different 2-state DFAs accept different numbers of strings of length n.

---

### Q11. L₁ regular, L₁ ∪ L₂ regular → L₂ regular?
**No.** Counterexample: L₁ = Σ* (regular), L₂ = {aⁿbⁿ} (not regular). L₁ ∪ L₂ = Σ* (regular). But L₂ is not regular.

---

### Q12. Complement DFA states
The complement is recognized by the **same DFA** with final and non-final states swapped.
**Answer: n states** (same as original)

---

## Section B: Regular Expressions

### Q13. RE: starts with 'a', ends with 'b'
**a(a+b)*b**

---

### Q14. RE: at least two 0s
**(0+1)\*0(0+1)\*0(0+1)\***

---

### Q15. Convert DFA to RE
From state equations:
q₀ = q₁a + ε·b*... Using state elimination:

Path from q₀ to q₁: b*a(ba)*
Stay at q₁: (b + ab*a)*
After reaching q₁ with any continuation: b*a(b + ab*a)*

**RE: b\*a(b + ab\*a)\*** (or equivalently using Arden's)

---

### Q16. Arden's theorem
Equations:
q₀ = q₀b + q₁a + ε → q₀ = (ε + q₁a)b* = b* + q₁ab*
q₁ = q₀a + q₁b → q₁ = q₀ab* (Arden's)

Substitute: q₁ = (b* + q₁ab*)ab* = b*ab* + q₁ab*ab*
q₁ = b*ab*(ab*ab*)* (Arden's)

Since q₁ is final: **RE = b\*ab\*(ab\*ab\*)\***

---

### Q17. Subset check
(a+b)*a(a+b)*b(a+b)* describes strings containing both 'a' and 'b' (with a before some b).
a(a+b)*b describes strings starting with 'a' and ending with 'b'.

"aab" is in the first language (it contains 'a' then 'b'). Is "aab" in a(a+b)*b? Yes: a·a·b.
"ba" is in the first language (contains a and b). Is "ba" in a(a+b)*b? No.

**Not a subset.** "ba" is a counterexample.

---

### Q18. Every 0 followed by 1
No standalone 0 — every 0 is part of "01".
**(a) (1+01)\***

Check: allows 1s and 01-blocks. Any 0 is immediately followed by 1. ✓

---

### Q19. 0*(10*)*
This matches any string of 0s and 1s: starts with optional 0s, then optional groups of (1 followed by optional 0s). This generates all binary strings.
**(a) (0+1)\***

---

### Q20. Simplify (a+b)*(a+ε)(b+ε)
(a+b)* already includes everything. (a+ε)(b+ε) = ab+a+b+ε.
(a+b)*(ab+a+b+ε) = (a+b)* (since (a+b)* absorbs concatenation with any finite-length suffix over {a,b}).
**Answer: (a+b)\***

---

## Section C: Pumping Lemma & Non-Regular Languages

### Q21. {0ⁿ1ⁿ} not regular
Assume regular with pumping length p.
Choose w = 0ᵖ1ᵖ. w = xyz, |xy| ≤ p, |y| > 0.
y = 0ᵏ (k ≥ 1) since xy is within first p characters.
Pump i=2: xy²z = 0^(p+k)1ᵖ. Since p+k ≠ p, this ∉ L.
**Contradiction. Not regular.**

---

### Q22. {w | |w| is even} — regular?
**Yes, regular.** DFA with 2 states: even-length (final) ↔ odd-length.
RE: ((a+b)(a+b))* or (aa+ab+ba+bb)*

---

### Q23. {aⁿ² | n ≥ 1} not regular
Assume regular, pumping length p.
Choose w = a^(p²), |w| = p² ≥ p.
w = xyz, y = aᵏ, 1 ≤ k ≤ p.
|xy²z| = p² + k. Need p² + k to be a perfect square.
Next perfect square after p² is (p+1)² = p² + 2p + 1.
Since k ≤ p: p² < p² + k ≤ p² + p < p² + 2p + 1 = (p+1)².
So p² + k is NOT a perfect square → xy²z ∉ L.
**Contradiction. Not regular.**

---

### Q24. {ww | w ∈ {0,1}*} not regular
Assume regular, pumping length p.
Choose w = 0ᵖ1·0ᵖ1. This is in L (with w = 0ᵖ1).
xyz with |xy| ≤ p → y in first block of 0s: y = 0ᵏ.
xy²z = 0^(p+k)1·0ᵖ1. First half contains more 0s → NOT of form ww.
**Contradiction. Not regular.**

---

### Q25. Myhill-Nerode for {aⁿbⁿ}
Consider strings: ε, a, a², a³, ...
For aⁱ and aʲ (i ≠ j): append bⁱ → aⁱbⁱ ∈ L but aʲbⁱ ∉ L.
So aⁱ and aʲ are distinguishable → infinitely many equivalence classes.
**By Myhill-Nerode, L is not regular.**

---

### Q26. {aⁿbᵐ | n ≤ m} — regular?
**Not regular.** Use pumping lemma with w = aᵖbᵖ. Pump y (in a's) up → too many a's > b's.

---

### Q27. Which is regular?
(a) {aⁿbⁿ}: not regular
(b) {aⁿ | n prime}: not regular
(c) {aⁿbᵐ | n ≠ m}: not regular
(d) {w | #a = #b}: not regular

**None of these are regular.** (The question likely has a different option set; answer depends on actual GATE options.)

---

### Q28. L₁ ∩ L₂ = {aⁿbⁿ} ∩ {aⁿ} = ∅ (no string is both aⁿbⁿ and aⁿ for n>0, except ε).
L₁ ∩ L₂ = {ε} which is **regular (finite language)**.

---

### Q29. Palindromes
{w | w = wᴿ} is NOT regular (use pumping lemma with aᵖbaᵖ).
It IS a CFL: S → aSa | bSb | a | b | ε.

**Answer: Not regular, but CFL.**

---

### Q30. L regular, L' such that L' ∩ R = L
Nothing definitive about L'. L' could be anything from regular to undecidable. The intersection with a regular language R restricts it to L but doesn't constrain L' itself.

---

## Section D: CFG & CFL

### Q31. CFG for {aⁿbⁿ | n ≥ 1}
**S → aSb | ab**

---

### Q32. Balanced parentheses
**S → SS | (S) | ε**

---

### Q33. S → aS | Sa | a — ambiguous?
For "aa": S → aS → a·a and S → Sa → a·a. Both give same tree? 
Actually: S → aS → a(a) vs S → Sa → (a)a — different parse trees!
**Yes, ambiguous.**

---

### Q34. CNF conversion
1. Remove ε: S → ε. Nullable: S. New: S → ASB | AB, A → aAS | a | aA, B → SbS | bS | Sb | b | A | bb
2. Remove unit: B → A becomes B → aAS | a | aA
3. Introduce terminals: a→Cₐ, b→C_b. Split long productions.
Final CNF: all rules A → BC or A → a form.

(Full conversion is mechanical and lengthy — the process matters for GATE.)

---

### Q35. Ambiguity of E → E+E | E*E | (E) | id
For "id+id*id":
Tree 1: E → E+E → id + E*E → id + id * id (multiplication first)
Tree 2: E → E*E → E+E * id → id + id * id (addition first)
Two distinct parse trees → **ambiguous** ✓

---

### Q36. CFG for {aⁱbʲcᵏ | i+k = j}
j = i+k, so we need i a's, then i b's (matching a's) then k b's and k c's (matching c's).
**S → AB, A → aAb | ε, B → bBc | ε**

Generates: aⁱbⁱbᵏcᵏ = aⁱb^(i+k)cᵏ ✓

---

### Q37. {aⁿbⁿcⁿ} not CFL
Assume CFL, pumping length p.
Choose w = aᵖbᵖcᵖ. w = uvxyz, |vy|>0, |vxy|≤p.

Since |vxy| ≤ p, vxy cannot span all three symbols. So v and y together touch at most 2 of {a,b,c}.

Pump i=2: uv²xy²z. The count of the untouched symbol stays at p, but the other counts increase. The three counts become unequal → ∉ L.

**Contradiction. Not CFL.**

---

### Q38. {aⁱbʲcᵏ | i=j or j=k}
This is a CFL (union of two CFLs: {aⁱbⁱcᵏ} ∪ {aⁱbᵏcᵏ}).
It is **inherently ambiguous** — every grammar generating it is ambiguous.

**Answer: (d) Inherently ambiguous CFL**

---

### Q39. CNF derivation steps for string of length n
In CNF, each A → BC adds one non-terminal to the sentential form. Starting from 1 non-terminal to get n terminals: need n−1 branching steps + n terminal steps = **2n − 1 steps**.

---

### Q40. Not a CFL
(a) CFL ✓ (b) CFL ✓ (palindromes) (c) NOT CFL ✓ (d) CFL ✓
**Answer: (c) {aⁿbⁿcⁿ}**

---

## Section E: Pushdown Automata

### Q41. PDA for {aⁿbⁿ | n ≥ 1}
- On reading 'a': push 'A' onto stack
- On reading 'b': pop 'A' from stack
- Accept by empty stack (all A's popped = equal a's and b's)

States: q₀ (reading a's), q₁ (reading b's).
δ(q₀, a, Z₀) = (q₀, AZ₀), δ(q₀, a, A) = (q₀, AA)
δ(q₀, b, A) = (q₁, ε), δ(q₁, b, A) = (q₁, ε)
δ(q₁, ε, Z₀) = (q₂, ε) — accept by empty stack or final state q₂.

---

### Q42. DPDA for {wwᴿ}?
**No.** {wwᴿ | w ∈ {a,b}*} is CFL but NOT DCFL. The PDA needs to non-deterministically guess the middle of the string. (Note: {wcwᴿ} with center marker c IS a DCFL.)

---

### Q43. PDA for {w | #a = #b}
Push/pop strategy: push 'A' for each unmatched 'a', push 'B' for each unmatched 'b'.
- Read 'a': if top is B, pop B; else push A
- Read 'b': if top is A, pop A; else push B
- Accept by empty stack.

---

### Q44. CFG S → aSb | ε to PDA
Standard conversion: single-state PDA.
State q, start with S on stack.
- (q, ε, S) → (q, aSb) and (q, ε, S) → (q, ε)
- (q, a, a) → (q, ε) and (q, b, b) → (q, ε)

Accept by empty stack when input consumed and stack empty.

---

### Q45. {aⁿbⁿ} ∪ {aⁿbⁿcⁿ} — DCFL?
Both individually: {aⁿbⁿ} is DCFL, {aⁿbⁿcⁿ} is not even CFL.
Union: is it CFL? {aⁿbⁿ} ∪ {aⁿbⁿcⁿ} ⊆ {aⁿbⁿ} (since every string in {aⁿbⁿcⁿ} is NOT in {aⁿbⁿ} for n>0 unless we consider the format). Wait: aⁿbⁿ has no c's, aⁿbⁿcⁿ has c's. They're disjoint (for n≥1).

{aⁿbⁿ} is CFL. {aⁿbⁿcⁿ} is not CFL. Their union is: a string is accepted if it's aⁿbⁿ (no c's) OR aⁿbⁿcⁿ.

This is CFL: simply combine PDA for {aⁿbⁿ} (when c's are absent) with non-deterministic choice.
Actually, {aⁿbⁿ | n≥0} ∪ {aⁿbⁿcⁿ | n≥0} IS CFL (NPC can guess which case).
But it's likely NOT DCFL.

**Answer: CFL but not DCFL**

---

### Q46. DPDA but not DFA
**(a) {aⁿbⁿ | n ≥ 0}** is DCFL (accepted by DPDA) but not regular (not DFA).
(b) {ww} is not even CFL. (c) not CFL. (d) regular.

---

### Q47. L (DCFL) ∩ R (regular) = DCFL?
**Yes.** DCFLs are closed under intersection with regular languages.

---

### Q48. PDA with 2 stacks
A PDA with 2 stacks is equivalent to a **Turing Machine** (can simulate tape using two stacks).
**Answer: (b) TM**

---

## Section F: Closure & Decision Properties

### Q49. Regular closure properties
**Yes** to all: regular languages are closed under intersection, union, and complement.

---

### Q50. Emptiness for CFLs
**Yes, decidable.** Check if any terminal string is derivable from start symbol (check if start symbol is "generating"). O(n) algorithm.

---

### Q51. CFL ∩ Regular = CFL
**Yes, always CFL.** Build product of PDA with DFA.

---

### Q52. CFL ∩ CFL = CFL?
**Not necessarily.** Counterexample: L₁ = {aⁿbⁿcᵐ} and L₂ = {aᵐbⁿcⁿ}, both CFL. L₁∩L₂ = {aⁿbⁿcⁿ} which is NOT CFL.

---

### Q53. Equivalence decidability
(a) **Decidable** for regular languages (minimize and compare DFAs, or check L₁⊕L₂ = ∅).
(b) **Undecidable** for CFLs.

---

### Q54. Reversal closure
**Regular:** Yes, closed under reversal (reverse the DFA/NFA).
**CFL:** Yes, closed under reversal (reverse all productions).

---

### Q55. Undecidable problem
**(a) Ambiguity of CFG is undecidable.**
(b) Emptiness: decidable. (c) Membership: decidable (CYK). (d) Finiteness: decidable.

---

### Q56. Always true for two CFLs
**(b) L₁ ∪ L₂ is CFL.** CFLs are closed under union.
(a) Not closed under intersection. (c) Not closed under complement. (d) Not closed under difference.

---

### Q57. L is RE and L' is RE
**Answer: (b) L is recursive (decidable).** This is a fundamental theorem.

---

### Q58. CFL not closed under complement
L = {aⁱbʲcᵏ | i≠j or j≠k} is CFL.
L' = {aⁿbⁿcⁿ | n≥0} ∪ {strings not of form a*b*c*} — actually, complement of L would need to have i=j AND j=k, giving {aⁿbⁿcⁿ} ∪ {non a*b*c* strings}... this gets complex.

Simpler: L₁ = {aⁿbⁿcᵐ}, L₂ = {aᵐbⁿcⁿ}. Both CFL. If CFL closed under complement, then L₁' CFL, and CFL ∩ CFL = CFL → L₁ ∩ L₂ = {aⁿbⁿcⁿ} would be CFL. Contradiction.

---

## Section G: Turing Machines & Decidability

### Q59. TM for {aⁿbⁿcⁿ}
High-level: 
1. Scan left to right: mark one 'a', one 'b', one 'c' in each pass
2. Replace first unmarked 'a' with X, then find first unmarked 'b' → Y, then first unmarked 'c' → Z
3. Return to start, repeat
4. Accept if all symbols are marked; reject if counts don't match

---

### Q60. Decidable vs Recognizable
**Decidable:** TM halts on ALL inputs — either accepts or rejects.
**Recognizable (RE):** TM accepts strings in L; may loop forever on strings not in L.

---

### Q61. "L(M) is finite" — undecidable by Rice's
Property P: "L(M) is finite."
- Some TMs have finite languages (e.g., TM accepting only "hello")
- Some TMs have infinite languages (e.g., TM accepting a*)
P is non-trivial → by Rice's theorem, **undecidable**. ✓

---

### Q62. Halting problem: RE? co-RE?
**RE (recognizable):** Yes. Simulate M on w; if it halts, accept. (If it doesn't halt, we loop forever.)
**Co-RE:** No. The complement of the halting problem is NOT recognizable. (If it were, halting problem would be decidable, contradiction.)

---

### Q63. {M | L(M) = Σ*} not RE
L(M) = Σ* means M accepts everything. 
Complement: L(M) ≠ Σ* means ∃w not accepted by M.
The complement is RE (enumerate w's and simulate).
If {M | L(M) = Σ*} were RE too, it would be decidable (both RE and co-RE). But "L(M) = Σ*" is non-trivial language property → undecidable by Rice's. Contradiction.
**Therefore not RE.**

---

### Q64. A ≤_m B and B decidable → A?
A is **decidable.** If we can reduce A to B, and B has a decider, we can decide A by converting instances and using B's decider.

---

### Q65. Which is decidable?
(a) Does M accept ε? — Non-trivial language property → undecidable (Rice's)
(b) Is L(M) regular? — undecidable (Rice's)
(c) Does M halt on all inputs? — undecidable
(d) Given DFA D, is L(D) = Σ*? — **Decidable!** (Check if all states reachable and all are final, or check complement DFA for emptiness.)

**Answer: (d)**

---

### Q66. Complement of halting problem
HP is RE but not decidable. HP' (complement) is **not RE (not recognizable)**.
**Answer: (c) not RE**

---

### Q67. L₁ = {⟨M⟩ | M halts on every input}
This is the "totality" problem. This is co-RE (complement is RE: find some input where M doesn't halt).
But it's not RE itself (if it were, combined with co-RE it would be decidable; but it's undecidable by Rice's).

Actually: L₁ is asking about a language property (L(M) defined on all strings if M is total). This is actually a property of the TM behavior, not just its language, so Rice's doesn't directly apply.

By reduction from halting problem: **not RE**.
Actually, some texts say it's neither RE nor co-RE. But the standard answer:
**Answer: (c) not RE**

---

### Q68. Rice's theorem
**(c) It applies only to non-trivial language properties of TMs.**
(a) Wrong — structural properties can be decidable. (b) Wrong — trivial properties are decidable. (d) Wrong — halting problem is proved differently (diagonalization).

---

### Q69. Not necessarily RE from L₁ RE, L₂ RE
(a) L₁∪L₂: RE ✓ (b) L₁∩L₂: RE ✓ (c) **L₁': NOT necessarily RE** ✓ (d) L₁·L₂: RE ✓

**Answer: (c) L₁'**

---

### Q70. Post Correspondence Problem
PCP is RE (we can enumerate all possible sequences and check).
But PCP is not decidable.
**(b) Recognizable (RE but not decidable)**

For unary alphabet (|Σ|=1), PCP IS decidable.

---

## Section H: CYK Algorithm & Parsing

### Q71. CYK for "baa"
Grammar: S → AB | BC, A → BA | a, B → CC | b, C → AB | a

CYK table (T[i,j] = set of variables deriving substring from position i to j):

**Length 1:** T[1,1]={B}, T[2,2]={A,C}, T[3,3]={A,C}

**Length 2:**
- T[1,2]: split k=1: B∈T[1,1], {A,C}∈T[2,2] → check BA→A, BC→S → T[1,2] = {S, A}
- T[2,3]: split k=2: {A,C}∈T[2,2], {A,C}∈T[3,3] → check AA=∅, AC=∅, CA=∅, CC→B → T[2,3] = {B}

**Length 3:**
- T[1,3]: split k=1: T[1,1]={B}, T[2,3]={B} → BB=∅
  split k=2: T[1,2]={S,A}, T[3,3]={A,C} → SA=∅, SC=∅, AA=∅, AC=∅ → T[1,3] = ∅

S ∉ T[1,3] → **"baa" ∉ L(G)**

---

### Q72. CYK time complexity
**Answer: (d) O(n³ · |G|)**

For each of O(n²) cells, we check O(n) split points, and for each we check all |G| productions.

---

### Q73. Normal form for CYK
**Answer: (b) CNF**

CYK requires all productions in the form A → BC or A → a (Chomsky Normal Form).

---

### Q74. CYK for "abc"
Grammar: S → AB, A → a, B → b | BC, C → c

T[1,1]={A}, T[2,2]={B}, T[3,3]={C}
T[1,2]: split k=1: A∈T[1,1], B∈T[2,2] → AB→S → T[1,2]={S}
T[2,3]: split k=2: B∈T[2,2], C∈T[3,3] → BC→B → T[2,3]={B}
T[1,3]: split k=1: A∈T[1,1], B∈T[2,3]={B} → AB→S → T[1,3]={S}
  split k=2: S∈T[1,2], C∈T[3,3] → SC=∅

S ∈ T[1,3] → **"abc" ∈ L(G)** ✓

---

### Q75. CYK programming paradigm
**Answer: (c) Dynamic Programming**

CYK fills a triangular table bottom-up, combining solutions of smaller subproblems (substrings).

---

## Section I: LBA & Context-Sensitive Languages

### Q76. Language recognized by LBA
**Answer: (c) Context-sensitive**

LBAs are the machine model for Type 1 (context-sensitive) languages in the Chomsky hierarchy.

---

### Q77. Decidable for LBAs
**Answer: (b) Membership**

For LBA M and input w of length n, the number of possible configurations is bounded by |Q| · n · |Γ|ⁿ (finite). We can simulate M for that many steps; if it hasn't halted, it's looping → reject.

Emptiness, equivalence, and universality are all undecidable for LBAs.

---

### Q78. Emptiness for LBAs
**Answer: (b) Undecidable**

The emptiness problem for LBAs is undecidable. It can be proved by reducing the halting problem.

---

### Q79. LBA tape space
**Answer: (b) Equal to the input length**

"Linear Bounded" means the tape head stays within the cells occupied by the input. The TM cannot use additional blank tape beyond the input boundaries.

---

### Q80. CSL but not CFL
{aⁿbⁿ} is CFL (recognized by PDA). {aⁿbⁿcⁿ} is CSL but not CFL (proved by CFL pumping lemma). {aⁿ | n is prime} is actually decidable (and thus CSL), and not CFL.

**Answer: (d) Both (b) and (c)**

---

## Section J: Countability, Church-Turing & Advanced TM

### Q81. Set of all TMs
**Answer: (b) Countably infinite**

Every TM has a finite encoding as a string over some alphabet. The set of all finite strings Σ* is countably infinite, so the set of all TM encodings ⊆ Σ* is also countable.

---

### Q82. Set of all languages
**Answer: (c) Uncountable**

The set of all languages over {a,b} = P({a,b}*) = P(countably infinite set), which by Cantor's theorem is uncountable. (|P(S)| > |S| for any set S.)

---

### Q83. Consequence of countable TMs, uncountable languages
**Answer: (c) There exist languages not recognized by any TM**

Since there are uncountably many languages but only countably many TMs, the mapping from TMs to languages cannot be surjective. Many languages have no TM at all.

---

### Q84. Church-Turing thesis
**Answer: (c) Every effectively computable function is TM-computable**

Note: It does NOT say TMs can solve "any" problem — only that anything effectively computable is TM-computable. It's a thesis, not a theorem.

---

### Q85. NDTM to DTM simulation overhead
**Answer: (b) Exponential time overhead**

A DTM simulates an NDTM with branching factor b and time t(n) by exploring the computation tree: at most b^t(n) paths → exponential overhead.

---

### Q86. Enumerator and language class
**Answer: (b) L must be RE**

A language is RE if and only if some enumerator enumerates it. The enumerator need not halt (it can run forever, printing strings). The language need not be finite or decidable.

---

### Q87. Enumerator printing in lexicographic order
**Answer: (b) Decidable**

If an enumerator prints strings in lexicographic order, to decide if w ∈ L: run the enumerator. If it prints w → accept. If it prints something lexicographically greater than w → reject (w will never appear). This always halts → L is decidable.

---

### Q88. 2-tape to 1-tape TM simulation
**Answer: (c) O(t(n)²)**

The 1-tape TM encodes both tapes on one tape, sweeping back and forth. Each step of the 2-tape TM requires O(t(n)) steps on the 1-tape TM (to sweep the content), and there are t(n) steps total → O(t(n)²).

---

## Section K: Advanced Undecidability & PCP

### Q89. Rice's theorem doesn't apply to
**Answer: (b) "M has at most 5 states"**

This is a property of the TM structure, not of its language L(M). Rice's theorem only applies to non-trivial properties of the language recognized by a TM. The number of states is a decidable structural property.

---

### Q90. PCP instance solution
Pairs: (ab, b), (b, ab), (a, aa)

Try sequence 1, 2: Top = ab·b = abb, Bottom = b·ab = bab ✗
Try sequence 2, 1: Top = b·ab = bab, Bottom = ab·b = abb ✗
Try sequence 1, 3: Top = ab·a = aba, Bottom = b·aa = baa ✗
Try sequence 3, 1: Top = a·ab = aab, Bottom = aa·b = aab ✓

**Solution: sequence (3, 1)** → both produce "aab"

---

### Q91. Reduction chain for CFG ambiguity
**Answer: (c) HP → MPCP → PCP → CFG Ambiguity**

The halting problem reduces to Modified PCP, which reduces to PCP, which reduces to the ambiguity problem for CFGs.

---

### Q92. Which is decidable?
(a) Undecidable — special case of halting problem
(b) **Decidable** — count states of M's encoding; purely structural property
(c) Undecidable — by Rice's theorem (non-trivial language property)
(d) Undecidable — by Rice's theorem

**Answer: (b)**

---

### Q93. L₁ RE but not recursive, L₂ = complement
**Answer: (c) L₂ is not RE**

If L₁ is RE but not recursive, then L₁' is NOT RE. (Because if both L₁ and L₁' were RE, L₁ would be recursive — contradiction.)

---

### Q94. A ≤_m B and B decidable
**Answer: (b) A is decidable**

If A ≤_m B (A reduces to B) and B is decidable, then A is decidable: to decide A, compute the reduction function f, then decide f(x) ∈ B.

---

### Q95. "Does TM M write '#'?"
**Answer: (c) Undecidable — not by Rice's (reducible from HP)**

This is a property about the TM's behavior (what it writes), not about its language L(M). Rice's theorem doesn't apply. However, it's still undecidable — reduce from the halting problem: given (M,w), construct M' that simulates M on w and writes '#' iff M halts.

---

*End of Solutions — Theory of Computation for GATE 2027*
