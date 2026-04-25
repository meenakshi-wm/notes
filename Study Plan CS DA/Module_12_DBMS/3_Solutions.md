# DBMS — Solutions to All Questions

> Detailed solutions for all 95 questions from `2_Questions_and_PYQs.md`.

---

## Section A: Relational Model & Keys

### Q1. Solution

$R(A,B,C,D,E)$, FDs: $\{A \to BC, CD \to E, B \to D\}$

**Step 1:** Attributes only on LHS (not on RHS): $A$ appears on LHS. $C$ appears on both sides. Check: RHS attributes are $\{B,C,D,E\}$. So $A$ never appears on RHS → $A$ must be in every candidate key.

**Step 2:** Compute $A^+ = \{A, B, C, D, E\}$ (by $A \to BC$, then $B \to D$, then $CD \to E$) = $R$

**Candidate key = {A}**. Only one candidate key.

**Super keys:** Any superset of $\{A\}$. Size of candidate key = 1, total attributes = 5.
$$\text{Super keys} = 2^{5-1} = 16$$

---

### Q2. Solution

$R(A,B,C,D,E)$, FDs: $\{AB \to C, C \to D, D \to A\}$

- $(AB)^+ = \{A,B,C,D,E\}$ ✓ (candidate key)
- $(BC)^+ = \{B,C,D,A,E\}$ ✓ (candidate key)
- Check if $A$ alone or $B$ alone works: $A^+ = \{A\}, B^+ = \{B\}$ — No
- $(ABC)^+$: contains AB, so works, but not minimal (AB already works)

**Answer: (d) Both AB and BC are candidate keys.** (BD is also a CK: $(BD)^+ = \{B,D,A,C,E\}$)

---

### Q3. Solution

6 attributes, candidate key of size 2. Super keys = all supersets of the candidate key.

$$\text{Super keys} = 2^{6-2} = 2^4 = 16$$

**Answer: 16**

---

### Q4. Solution

$R(A,B,C,D,E)$, FDs: $\{A \to B, B \to C, C \to D, D \to E\}$

$A^+ = \{A,B,C,D,E\} = R$. So $A$ is a candidate key.

No other single attribute determines all: $B^+ = \{B,C,D,E\}$, $C^+ = \{C,D,E\}$, etc.

$A$ is the **only** candidate key. **Answer: 1**

---

### Q5. Solution

- (a) False: A relation can have multiple candidate keys
- (b) False: Primary key cannot be NULL (entity integrity)
- (c) False: A super key may not be minimal
- **(d) TRUE: Every candidate key is a super key** (a CK uniquely identifies tuples, and it's a minimal super key, hence also a super key)

---

### Q6. Solution

$R(A,B,C,D)$, FDs: $\{AB \to CD, D \to A\}$

- $(AB)^+ = \{A,B,C,D\} = R$ → AB is CK
- $(BD)^+ = \{B,D,A,C\} = R$ → BD is CK
- Check subsets: $A^+ = \{A\}$, $B^+ = \{B\}$, $D^+ = \{D,A\}$ — none gives $R$

**Candidate keys: AB, BD**

Super keys: supersets of AB or BD.
- Supersets of AB: $\{AB, ABC, ABD, ABCD\}$ = $2^2 = 4$
- Supersets of BD: $\{BD, BCD, ABD, ABCD\}$ = $2^2 = 4$
- Overlap (supersets of both): $\{ABD, ABCD\}$ = $2^1 = 2$

By inclusion-exclusion: $4 + 4 - 2 = 6$

**Answer: 6 super keys**

---

### Q7. Solution

If all $n$ attributes form the only candidate key, the only super key is the set of all $n$ attributes.

$$\text{Super keys} = 2^{n-n} = 2^0 = 1$$

**Answer: 1**

---

### Q8. Solution

$R(A,B,C,D,E,F)$, FDs: $\{A \to B, B \to C, C \to A, D \to E, CF \to D\}$

Since $A \to B \to C \to A$, we have $A \equiv B \equiv C$ (equivalent in terms of closure).

Try $AF$: $(AF)^+ = \{A,F,B,C,D,E\} = R$ ✓ → CK
Try $BF$: $(BF)^+ = \{B,F,C,A,D,E\} = R$ ✓ → CK
Try $CF$: $(CF)^+ = \{C,F,A,D,B,E\} = R$ ✓ → CK

Check minimality: $A^+ = \{A,B,C\}$, $F^+ = \{F\}$ — neither alone gives $R$. So each is minimal.

**Answer: 3 candidate keys: AF, BF, CF**

---

### Q9. Solution

**Answer: (b)** A foreign key value must match a primary key value in the referenced table or be NULL.

---

### Q10. Solution

With `ON DELETE CASCADE`, when a department is deleted from `Department`, **all employees with that `dept_id` are also deleted** from `Employee`.

---

## Section B: Functional Dependencies & Normalization

### Q11. Solution

$F = \{A \to B, B \to C, A \to C, AB \to D\}$

**Step 1: Decompose RHS** (already single attributes)

**Step 2: Remove extraneous LHS attributes:**
- $AB \to D$: Is $B$ extraneous? Check if $A \to D$ under remaining FDs. $A^+ = \{A,B,C\}$ — no $D$. So $B$ is not extraneous.
- Is $A$ extraneous in $AB \to D$? Check $B^+ = \{B,C\}$ — no $D$. Not extraneous.

**Step 3: Remove redundant FDs:**
- $A \to C$: Under $\{A \to B, B \to C, AB \to D\}$, is $C \in A^+$? $A^+ = \{A,B,C,D\}$ — yes! Remove $A \to C$.

**Minimal cover:** $F_c = \{A \to B, B \to C, AB \to D\}$

Now check: Is $B$ extraneous in $AB \to D$ again? $A^+ = \{A,B,C\}$ under $\{A \to B, B \to C\}$ — no $D$. Keep.

Wait, with all three FDs: $A^+ = \{A,B,C\}$ (using $A \to B$, $B \to C$), still no $D$. So $AB \to D$ stays.

But since $A \to B$, we have $AB \to D$ simplifies: $A^+$ already includes $B$, so $A \to D$? No — we need $AB$ together in the FD $AB \to D$. Since $A^+ = \{A,B,C\}$ (no $D$), $AB \to D$ cannot be simplified.

**Minimal Cover:** $\boxed{F_c = \{A \to B, B \to C, AB \to D\}}$

Actually, let's recheck extraneous: In $AB \to D$, check if $B$ is extraneous. Compute $A^+$ using **all** FDs in $F_c$: $A \to B$, $B \to C$, $AB \to D$. So $A^+ = \{A,B,C,D\}$. Yes! $D \in A^+$. So $B$ IS extraneous. Replace $AB \to D$ with $A \to D$.

**Minimal Cover:** $\boxed{F_c = \{A \to B, B \to C, A \to D\}}$

---

### Q12. Solution

$R(A,B,C)$, FDs: $\{A \to B, B \to C, C \to A\}$

$A^+ = \{A,B,C\}$, $B^+ = \{B,C,A\}$, $C^+ = \{C,A,B\}$

Every single attribute is a candidate key! So **all attributes are prime**.

For BCNF: Every FD's LHS must be a superkey. $A \to B$: $A^+ = R$ ✓. $B \to C$: $B^+ = R$ ✓. $C \to A$: $C^+ = R$ ✓.

**Answer: (d) BCNF**

---

### Q13. Solution

$R(A,B,C,D,E)$, FDs: $\{AB \to CD, C \to E, A \to B\}$

Find CKs: Since $A \to B$, $A$ determines $B$. $(A)^+ = \{A,B,C,D,E\} = R$ (via $A \to B$, $AB \to CD$, $C \to E$).

So $A$ is the only candidate key. Prime = $\{A\}$, Non-prime = $\{B,C,D,E\}$.

Check 2NF: $A$ is the only CK (single attribute), so no partial dependency possible. → In 2NF.

Check 3NF: $A \to B$ where $A$ is superkey ✓. $AB \to CD$: $AB$ is superkey ($A$ alone is) ✓. $C \to E$: $C$ is not superkey, $E$ is not prime → **violates 3NF**.

**Answer: (b) 2NF but not 3NF**

---

### Q14. Solution

$R(A,B,C,D,E)$, FDs: $\{A \to B, BC \to D, D \to E\}$

**(a) Candidate keys:**
- RHS: $\{B,D,E\}$. Only LHS: $A,C$ never appear on RHS.
- $(AC)^+ = \{A,C,B,D,E\} = R$ ✓

Is it minimal? $A^+ = \{A,B\}$, $C^+ = \{C\}$ — neither alone gives $R$.

**Candidate key: AC** (only one).

**(b) Normal form:**
- Prime attributes: $\{A, C\}$, Non-prime: $\{B, D, E\}$
- $A \to B$: $A$ is a proper subset of CK $AC$, $B$ is non-prime → **Partial dependency!**
- **R is in 1NF but NOT in 2NF**

---

### Q15. Solution

$R(A,B,C,D)$, FDs: $\{A \to B, B \to C, C \to D\}$

CK: $A$ (since $A^+ = \{A,B,C,D\} = R$). Non-prime: $\{B,C,D\}$.

- $A \to B$: $A$ is superkey ✓
- $B \to C$: $B$ not superkey → violates BCNF (and 3NF since $C$ not prime)
- Transitive: $A \to B \to C$, $A \to B \to C \to D$

**Normal form: 2NF** (no partial dependency since CK is single attribute, but transitive dependencies exist → not 3NF).

**BCNF Decomposition:**
1. $B \to C$ violates: Decompose $R$ into $R_1(B,C)$ and $R_2(A,B,D)$
2. In $R_2$: FDs are $\{A \to B\}$ and from $B \to C, C \to D$: $B \to D$ (transitively via $B \to C \to D$). Wait — $C$ is not in $R_2$. Project FDs onto $R_2(A,B,D)$: $A \to B$, and $B \to D$ (since $B \to C \to D$ and we need to check: $B^+ = \{B,C,D\}$, projected to $\{B,D\}$: $B \to D$).
   - $B \to D$: $B$ not superkey in $R_2$ (CK of $R_2$ is $A$) → violates BCNF
3. Decompose $R_2$ into $R_3(B,D)$ and $R_4(A,B)$

**Final BCNF decomposition:** $R_1(B,C)$, $R_3(B,D)$, $R_4(A,B)$

Check: dependency $C \to D$ is NOT preserved (C and D are in different relations).

---

### Q16. Solution

**Answer: (b) Transitive dependency.** 2NF eliminates partial dependencies. If it's in 2NF but not 3NF, there must be a transitive dependency.

---

### Q17. Solution

$R(A,B,C,D,E)$, FDs: $\{AB \to C, C \to D, D \to A\}$

CKs: AB, BC, BD (as computed in Q2).

Prime attributes: $\{A,B,C,D\}$. Non-prime: $\{E\}$. But $E$ doesn't appear in any FD's RHS from non-superkey LHS... Wait, $E$ is not determined by any FD. Actually check: does any FD determine $E$? No FD has $E$ on RHS. So $E$ must be in every CK? Let me re-verify.

$(AB)^+ = \{A,B,C,D\}$. Missing $E$! So $AB$ is NOT a candidate key.

Recalculate: $(ABE)^+ = \{A,B,E,C,D\} = R$ ✓. Is it minimal? $(AB)^+ = \{A,B,C,D\} \neq R$. $(AE)^+ = \{A,E\}$. $(BE)^+ = \{B,E\}$. So $ABE$ is a CK.

Similarly: $(BCE)^+ = \{B,C,E,D,A\} = R$ ✓. $(BDE)^+ = \{B,D,E,A,C\} = R$ ✓.

CKs: **ABE, BCE, BDE**. Prime: $\{A,B,C,D,E\}$ — all attributes are prime!

Since all attributes are prime, no non-prime transitive or partial dependencies → **3NF**.

BCNF check: $C \to D$: $C^+ = \{C,D,A\} \neq R$ → C is not superkey → **NOT BCNF**.

**Answer: (b) 3NF but not BCNF**

---

### Q18. Solution

$R_1(A,B)$, $R_2(A,C,D)$. $R_1 \cap R_2 = \{A\}$.

Check: $A \to R_1 - R_2 = A \to B$? We have $A \to BC$ so $A \to B$ ✓.

**The decomposition is lossless** since $R_1 \cap R_2 \to R_1 - R_2$ is in $F^+$.

---

### Q19. Solution

$R_1(A,B)$, $R_2(B,C)$. FDs: $\{A \to B, B \to C\}$.

$F_1$ (on $R_1$): $\{A \to B\}$. $F_2$ (on $R_2$): $\{B \to C\}$.

$(F_1 \cup F_2)^+ = \{A \to B, B \to C\}^+ = F^+$. ✓

**Yes, the decomposition is dependency preserving.**

---

### Q20. Solution

$R(A,B,C,D,E)$, FDs: $\{A \to B, BC \to E, ED \to A\}$

Attributes not on any RHS: $\{C, D\}$ → must be in every CK.

$(CD)^+ = \{C,D\}$ — not sufficient. Add attributes:
- $(ACD)^+ = \{A,C,D,B,E\} = R$ ✓. Check if minimal: $(CD)^+ = \{C,D\} \neq R$, $(AC)^+ = \{A,C,B\}$, $(AD)^+ = \{A,D,B\}$ — neither gives $R$. So $ACD$ is CK.
- $(BCD)^+ = \{B,C,D,E,A\} = R$ ✓. Minimal? $(BC)^+ = \{B,C,E\}$, $(BD)^+ = \{B,D\}$ — so $BCD$ is CK.
- $(ECD)^+ = \{E,C,D,A,B\} = R$ ✓. Minimal? $(EC)^+ = \{E,C\}$, $(ED)^+ = \{E,D,A,B,C\} = R$! So $ECD$ is NOT minimal since $ED$ is a super key? Wait: $(ED)^+ = \{E,D,A,B,C\} = R$. But $D$ alone: $D^+ = \{D\}$, $E$ alone: $E^+ = \{E\}$. So $ED$ is a CK! But $E$ appears on RHS...

Check: $E$ appears on RHS of $BC \to E$, so $E$ CAN be in a CK. And $D$ is never on RHS.
$(ED)^+ = \{E,D,A,B,C\} = R$ ✓. Minimal? $E^+ = \{E\}, D^+ = \{D\}$ — yes, minimal.

So $CD$ must be in every CK? No — $ED$ is a CK and doesn't contain $C$. Let me recheck: $C$ appears on RHS? No, $C$ is not on RHS of any FD. So $C$ must be in every CK. But $ED$ doesn't contain $C$, yet $(ED)^+ = \{E,D\} \to$ by $ED \to A$: $\{E,D,A\} \to$ by $A \to B$: $\{E,D,A,B\} \to$ by $BC \to E$: need $C$... we don't have $C$.

$(ED)^+ = \{E,D,A,B\} \neq R$. I made an error. Let me redo:

$(ED)^+$: Start $\{E,D\}$. $ED \to A$: $\{E,D,A\}$. $A \to B$: $\{E,D,A,B\}$. $BC \to E$: need $C$ — don't have it. Stop. $(ED)^+ = \{E,D,A,B\} \neq R$.

So $C$ and $D$ must be in every CK. Back to:
- $(ACD)^+$: $\{A,C,D\} \to \{A,B,C,D\} \to \{A,B,C,D,E\} = R$ ✓ → CK
- $(BCD)^+$: $\{B,C,D\} \to BC \to E$: $\{B,C,D,E\} \to ED \to A$: $\{A,B,C,D,E\} = R$ ✓ → CK
- $(ECD)^+$: $\{E,C,D\} \to ED \to A$: $\{A,C,D,E\} \to A \to B$: $\{A,B,C,D,E\} = R$ ✓ → CK

Check minimality of each: all have $CD$ plus one more, and $CD$ alone doesn't suffice. ✓

**Answer: 3 candidate keys: ACD, BCD, ECD**

---

### Q21. Solution

$R(A,B,C,D)$, FDs: $\{AB \to C, C \to D, D \to B\}$

CKs: $(AB)^+ = \{A,B,C,D\} = R$. Also $(AC)^+ = \{A,C,D,B\} = R$, $(AD)^+ = \{A,D,B,C\} = R$.
CKs: **AB, AC, AD**. Prime: $\{A,B,C,D\}$ — all prime.

**3NF Synthesis Algorithm:**
1. Minimal cover: $F_c = \{AB \to C, C \to D, D \to B\}$ (already minimal)
2. Group by LHS: $R_1(A,B,C)$, $R_2(C,D)$, $R_3(D,B)$
3. Check if any CK is contained in some $R_i$: AB ⊂ $R_1$ ✓

**3NF decomposition: $R_1(A,B,C)$, $R_2(C,D)$, $R_3(D,B)$**

---

### Q22. Solution

**Answer: (a) Every BCNF relation is in 3NF.** BCNF is a stricter condition than 3NF.

---

### Q23. Solution

$R(A,B,C,D,E,F)$, FDs: $\{A \to B, A \to C, CD \to E, CD \to F, B \to D\}$

$A^+ = \{A,B,C,D,E,F\} = R$ (via $A \to B$, $A \to C$, $B \to D$, $CD \to E$, $CD \to F$).

**Key: A**. Non-prime: $\{B,C,D,E,F\}$.

Check 2NF: CK is single attribute $A$, so no partial dependency → in 2NF.

Check 3NF: $B \to D$: $B$ is not superkey, $D$ is not prime → **violates 3NF**.

**Highest normal form: 2NF**

---

### Q24. Solution (Chase Test)

$R(A,B,C,D)$, FDs: $\{A \to B, B \to C, C \to D\}$, Decomposition: $R_1(A,B), R_2(B,C), R_3(C,D)$.

**Initial tableau:**

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a_1$ | $a_2$ | $b_{13}$ | $b_{14}$ |
| $R_2$ | $b_{21}$ | $a_2$ | $a_3$ | $b_{24}$ |
| $R_3$ | $b_{31}$ | $b_{32}$ | $a_3$ | $a_4$ |

**Apply $A \to B$:** Rows 1 and 2 agree on A? No ($a_1 \neq b_{21}$). Rows 1 and 3? No. No changes.

Wait — we look for rows agreeing on LHS. $A \to B$: rows with same $A$ value? Only row 1 has $a_1$, rows 2,3 have $b$ values. No match.

**Apply $B \to C$:** Rows 1 and 2 agree on $B$ ($a_2 = a_2$). So $C$ must agree: change $b_{13}$ to $a_3$.

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a_1$ | $a_2$ | $a_3$ | $b_{14}$ |
| $R_2$ | $b_{21}$ | $a_2$ | $a_3$ | $b_{24}$ |
| $R_3$ | $b_{31}$ | $b_{32}$ | $a_3$ | $a_4$ |

**Apply $C \to D$:** Rows 1, 2, 3 all have $a_3$ for $C$. So $D$ must agree: change $b_{14}$ and $b_{24}$ to $a_4$.

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a_1$ | $a_2$ | $a_3$ | $a_4$ |
| $R_2$ | $b_{21}$ | $a_2$ | $a_3$ | $a_4$ |
| $R_3$ | $b_{31}$ | $b_{32}$ | $a_3$ | $a_4$ |

Row 1 is all $a$'s! → **Lossless decomposition** ✓

---

### Q25. Solution

$R(A,B,C)$, FDs: $\{AB \to C, C \to B\}$

CKs: $(AB)^+ = \{A,B,C\}$ ✓. $(AC)^+ = \{A,C,B\}$ ✓. $A^+ = \{A\}$. So CKs: **AB, AC** (2 candidate keys).

Prime: $\{A,B,C\}$ — all prime.

3NF: $C \to B$: $C$ is not superkey, but $B$ is prime → **3NF satisfied**.

BCNF: $C \to B$: $C$ is not superkey → **NOT BCNF**.

**Answer: 2 candidate keys. Highest NF: 3NF (not BCNF).**

---

### Q26. Solution

$F = \{A \to B, B \to C\}$, $G = \{A \to B, A \to C, B \to C\}$

**Check $F \subseteq G^+$:** $A \to B$: $(A)^+_G = \{A,B,C\} \ni B$ ✓. $B \to C$: $(B)^+_G = \{B,C\} \ni C$ ✓.

**Check $G \subseteq F^+$:** $A \to B$ ✓ (in $F$). $A \to C$: $(A)^+_F = \{A,B,C\} \ni C$ ✓. $B \to C$ ✓ (in $F$).

**Yes, $F$ and $G$ are equivalent.**

---

### Q27. Solution

**Answer: (d) Both (b) and (c).** We need $R_1 \cup R_2 = R$ (no attribute loss) AND the common attributes must functionally determine at least one of the differences.

---

### Q28. Solution

$R(A,B,C,D,E)$, FDs: $\{A \to BC, C \to DE\}$

$A^+ = \{A,B,C,D,E\} = R$. CK: **A** (only one).

Non-prime: $\{B,C,D,E\}$. $C \to DE$: $C$ not superkey, $D,E$ not prime → violates 3NF.
Also $A \to BC$ then $C \to DE$: transitive dependency.

**Highest NF: 2NF** (CK is single attribute → no partial dependency, but transitive exists → not 3NF).

**BCNF Decomposition:**
1. $C \to DE$ violates. Decompose: $R_1(C,D,E)$, $R_2(A,B,C)$
2. $R_1$: CK is $C$, $C \to DE$ ✓ BCNF
3. $R_2$: FDs: $\{A \to BC\}$, CK is $A$, $A \to BC$ ✓ BCNF

**Decomposition: $R_1(C,D,E)$, $R_2(A,B,C)$**

Dependency preserving? $A \to BC$ is in $R_2$ ✓. $C \to DE$ is in $R_1$ ✓. **Yes, dependency preserving.**

---

## Section C: Relational Algebra

### Q29. Solution

$R \bowtie S$ (natural join on common attribute $B$):

| A | B | C |
|---|---|---|
| 1 | 2 | 5 |
| 3 | 4 | 6 |
| 3 | 4 | 7 |

---

### Q30. Solution

"Find $A$ values where for every $B$ in $S$, there is a tuple in $R$ with the same $B$."

This is a division: $\pi_{A,B}(R) \div \pi_B(S)$

Or equivalently: $\pi_A(R) - \pi_A((\pi_A(R) \times \pi_B(S)) - \pi_{A,B}(R))$

---

### Q31. Solution

Projection removes duplicates. If all 100 tuples have the same $A$ value, result is 1. If all different, result is 100.

**Maximum: 100 tuples.**

---

### Q32. Solution

$R \div S$: Find $A$ values associated with ALL $B$ values in $S = \{2, 3\}$.

- $A = 1$: appears with $B \in \{2, 3\}$ ✓ (both in $S$)
- $A = 2$: appears with $B \in \{3, 4\}$, missing $B = 2$ ✗

**$R \div S = \{(1)\}$**

---

### Q33. Solution

$|R| = 5, |S| = 3$, common = 2.

- $|R \cup S| = 5 + 3 - 2 = 6$
- $|R \cap S| = 2$
- $|R - S| = 5 - 2 = 3$

---

### Q34. Solution

"Names of students enrolled in all 4-credit courses."

$$\pi_{\text{name}}(\text{Student} \bowtie (\pi_{\text{sid,cid}}(\text{Enrolled}) \div \pi_{\text{cid}}(\sigma_{\text{credits}=4}(\text{Course}))))$$

---

### Q35. Solution

Left outer join using basic operators:

$R \,⟕\, S = (R \bowtie_{R.A=S.A} S) \cup ((R - \pi_{R.*}(R \bowtie_{R.A=S.A} S)) \times \{(\text{NULL}, \text{NULL}, \ldots)\})$

(Pad unmatched R tuples with NULLs for S's attributes.)

---

### Q36. Solution

**Answer: (b)** Degree = $a + b$, cardinality = $m \times n$.

---

### Q37. Solution

$$\pi_{\text{sid}}(\sigma_{\text{cid}='C1'}(\text{Enrolled})) - \pi_{\text{sid}}(\sigma_{\text{cid}='C2'}(\text{Enrolled}))$$

---

### Q38. Solution

$\sigma_{B=2}(R)$:

| A | B | C |
|---|---|---|
| 1 | 2 | 3 |
| 4 | 2 | 5 |

$\pi_A(\sigma_{B=2}(R)) = \{1, 4\}$

---

### Q39. Solution

If the join attribute in $R$ is a FK referencing $S$ (every FK value matches exactly one PK in $S$), then every tuple in $R$ joins with exactly one tuple in $S$.

**Result: 1000 tuples** (same as $|R|$).

---

### Q40. Solution

**Projection** ($\pi$). Projection inherently removes duplicates in relational algebra (set semantics), which corresponds to `SELECT DISTINCT` in SQL.

---

### Q41. Solution

$$\text{EmpCount} = {}_{\text{dept\_id}} \mathcal{G}_{\text{COUNT(eid) AS cnt}}(\text{Employee})$$

$$\text{MaxCount} = \mathcal{G}_{\text{MAX(cnt)}}(\text{EmpCount})$$

$$\pi_{\text{dept\_id}}(\text{EmpCount} \bowtie_{\text{cnt = max\_cnt}} \text{MaxCount})$$

---

### Q42. Solution

"Find tuples with maximum $A$":

$$R - \pi_R(R \bowtie_{R.A < S.A} \rho_S(R))$$

Rename $R$ as $S$, find tuples with a smaller $A$ somewhere, subtract from $R$.

---

## Section D: Tuple Relational Calculus

### Q43. Solution

$$\{t \mid \exists r \in R (t[A] = r[A] \land \neg \exists s \in S (s[B] = r[B]))\}$$

"Find attribute $A$ of tuples in $R$ whose $B$ value does NOT appear in $S$."

Equivalent RA: $\pi_A(R - (R \ltimes S))$ or $\pi_A(R) - \pi_A(R \bowtie S)$ depending on schema.

---

### Q44. Solution

Let `Student(sid, name)`, `Enrollment(sid, cid)`, `Course(cid, dept)`.

$$\{t \mid \exists s \in \text{Student}(t[\text{name}] = s[\text{name}] \land \forall c \in \text{Course}(c[\text{dept}] \neq \text{'CS'} \lor \exists e \in \text{Enrollment}(e[\text{sid}] = s[\text{sid}] \land e[\text{cid}] = c[\text{cid}])))\}$$

---

### Q45. Solution

$\pi_A(\sigma_{B>5}(R))$ in TRC:

$$\{t \mid \exists r \in R(t[A] = r[A] \land r[B] > 5)\}$$

---

### Q46. Solution

**Answer: (b)** $\{t \mid \neg(t \in R)\}$ — This returns all tuples NOT in $R$, which is infinite (all possible tuples in the domain minus $R$). **Not safe.**

---

### Q47. Solution

$$\{t \mid \exists d \in \text{Depositor}(t[\text{cust\_name}] = d[\text{cust\_name}] \land$$
$$\forall b \in \text{Branch}(b[\text{city}] \neq \text{'Mumbai'} \lor$$
$$\exists a \in \text{Account}(a[\text{branch\_name}] = b[\text{branch\_name}] \land$$
$$\exists d_2 \in \text{Depositor}(d_2[\text{cust\_name}] = d[\text{cust\_name}] \land d_2[\text{acct\_no}] = a[\text{acct\_no}]))))\}$$

---

### Q48. Solution

The expression says: "Find names of employees who work on ALL projects."

It reads: there exists an employee $r$ such that for all projects $s$, there exists a WorksOn tuple $u$ linking $r$ to $s$, and the output is $r$'s name.

---

## Section E: SQL

### Q49. Solution

- `COUNT(*)` = 10 (counts all rows)
- `COUNT(salary)` = 7 (ignores 3 NULLs)

**Answer: 10 and 7**

---

### Q50. Solution

The query:
1. Groups employees by department
2. Keeps only departments with more than 5 employees
3. Returns department and average salary
4. Sorted by average salary in descending order

---

### Q51. Solution

Both queries produce **identical results**. `IN` is equivalent to `= ANY`.

---

### Q52. Solution

**Answer: (c) UNKNOWN.** In SQL's three-valued logic, comparing NULL with anything (including NULL) yields UNKNOWN.

---

### Q53. Solution

This is the **division** query in SQL. It returns **names of students enrolled in ALL courses**.

The double `NOT EXISTS` pattern:
- Outer NOT EXISTS: "there is no course..."
- Inner NOT EXISTS: "...for which the student has no enrollment"

---

### Q54. Solution

- `WHERE` filters individual rows **before** grouping
- `HAVING` filters groups **after** `GROUP BY`
- `HAVING` can use aggregate functions; `WHERE` cannot

---

### Q55. Solution

$R(A,B) = \{(1,\text{NULL}), (2,3), (\text{NULL},4)\}$

- `SUM(A)` = 1 + 2 = **3** (NULL ignored)
- `SUM(B)` = 3 + 4 = **7** (NULL ignored)
- `COUNT(*)` = **3**
- `COUNT(A)` = **2** (NULL ignored)

---

### Q56. Solution

$R = \{(1,2),(1,3),(\text{NULL},4),(\text{NULL},5),(2,\text{NULL})\}$

GROUP BY A:
- $A = 1$: 2 rows → COUNT = 2
- $A = 2$: 1 row → COUNT = 1
- $A = \text{NULL}$: 2 rows → COUNT = 2

ORDER BY A: NULL comes first (implementation-dependent, usually first or last)

| A | COUNT(*) |
|---|---|
| NULL | 2 |
| 1 | 2 |
| 2 | 1 |

---

### Q57. Solution

```sql
SELECT MAX(salary) FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

Or using `LIMIT/OFFSET`:
```sql
SELECT DISTINCT salary FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

---

### Q58. Solution

**Answer: (b) DISTINCT**

---

### Q59. Solution

```sql
SELECT s.name FROM Student s
WHERE NOT EXISTS (
    SELECT c.cid FROM Course c
    WHERE NOT EXISTS (
        SELECT 1 FROM Enrollment e
        WHERE e.sid = s.sid AND e.cid = c.cid
    )
);
```

Alternatively using COUNT:
```sql
SELECT s.name FROM Student s
JOIN Enrollment e ON s.sid = e.sid
GROUP BY s.sid, s.name
HAVING COUNT(DISTINCT e.cid) = (SELECT COUNT(*) FROM Course);
```

---

### Q60. Solution

- `COUNT(DISTINCT dept)`: Counts number of distinct non-NULL department values. Returns a single number.
- `DISTINCT COUNT(dept)`: Counts non-NULL dept values first, then applies DISTINCT to the single result. Since there's only one result row, DISTINCT does nothing.

They return different values. E.g., if there are 100 employees in 5 departments: first returns 5, second returns 97 (or however many have non-NULL dept).

---

### Q61. Solution

**Answer: (b) LIKE**

---

### Q62. Solution

```sql
SELECT e.name
FROM Employee e
JOIN Employee m ON e.manager_id = m.eid
WHERE m.salary > e.salary;
```

---

## Section F: File Organization & Indexing

### Q63. Solution

Records = 30,000. Record size = 100B. Block size = 1024B. Pointer = 6B. Key = 10B.

**(a)** Blocking factor $f = \lfloor 1024/100 \rfloor = 10$. Blocks = $\lceil 30000/10 \rceil = 3000$.

**(b)** Primary index: 3000 entries (one per block). Index entry = Key + Pointer = 10 + 6 = 16B.
Index blocking factor $f_i = \lfloor 1024/16 \rfloor = 64$.
Level 1: $\lceil 3000/64 \rceil = 47$ blocks.
Level 2: $\lceil 47/64 \rceil = 1$ block.

**2 levels** of index.

**(c)** Search cost = 2 (index levels) + 1 (data block) = **3 block accesses**.

---

### Q64. Solution

**Answer: (c) Secondary index on non-key.** Secondary indexes must be dense because the data file is not ordered on the index attribute, so every record (or every distinct value with bucket pointers) needs an index entry.

---

### Q65. Solution

B+ tree with $p = 4$ (max 4 pointers, 3 keys in internal nodes), $p_l = 3$ (max 3 key-pointer pairs in leaves).

After inserting 1–10 in order, the B+ tree would have:
- Root with keys splitting as insertions cause overflow
- Leaves linked left to right

Structure (one valid result after sequential insertion with splits):

```
                [4, 7]
              /    |    \
        [2]      [5,6]    [9]
       / \      / | \    / \
    [1,2] [3,4] [5,6] [7,8] [9,10]
```

(Exact structure depends on split policy; key point is all leaves at same level, linked.)

---

### Q66. Solution

B-tree order $p = 5$:

| Node Type | Min Keys | Max Keys | Min Children | Max Children |
|---|---|---|---|---|
| Root | 1 | 4 | 2 | 5 |
| Non-root internal | $\lceil 5/2 \rceil - 1 = 2$ | 4 | 3 | 5 |

---

### Q67. Solution

Block = 4096B, record pointer = 8B, block pointer = 8B, key = 32B.

**(a) B+ tree internal node order $p$:**

$(p-1) \times 32 + p \times 8 \leq 4096$

$32p - 32 + 8p \leq 4096$

$40p \leq 4128$

$p = \lfloor 4128/40 \rfloor = 103$

**Leaf node order $p_l$:**

$p_l \times (32 + 8) + 8 \leq 4096$

$40 \cdot p_l \leq 4088$

$p_l = \lfloor 4088/40 \rfloor = 102$

**(b) Levels:** For 1,000,000 records:
- Leaf nodes needed: $\lceil 1000000/102 \rceil = 9804$
- Level 1 (above leaves): $\lceil 9804/103 \rceil = 96$
- Level 2: $\lceil 96/103 \rceil = 1$

**3 levels** (root + 1 internal + leaf level), or equivalently **height = 3**.

**(c)** Block accesses = height + 1 (for data block) = 3 + 1 = **4 block accesses** (or just 3 if record pointer in leaf directly gives the record).

---

### Q68. Solution

- **(a) Range query on $A$:** Use **primary index** (data is sorted on $A$, range query benefits from sequential access)
- **(b) Equality query on $B$:** Use **secondary index** (direct to the record)
- **(c) Range query on $B$:** Secondary index could be used but with many random accesses. May be **cheaper to do sequential scan** depending on selectivity.

---

### Q69. Solution

**Answer: (c) Leaf nodes only.**

---

### Q70. Solution

Records = 8192, Block = 512B, Record = 128B, Key = 16B, Pointer = 8B.

$f = \lfloor 512/128 \rfloor = 4$ records/block. Blocks = $\lceil 8192/4 \rceil = 2048$.

**Binary search:** $\lceil \log_2(2048) \rceil = 11$ block accesses.

**Primary index:** Index entries = 2048. Index entry = 16 + 8 = 24B. $f_i = \lfloor 512/24 \rfloor = 21$.
Index blocks = $\lceil 2048/21 \rceil = 98$. Search = $\lceil \log_2(98) \rceil + 1 = 7 + 1 = 8$ block accesses (binary search on index + 1 data block).

Or with multilevel: Level 1 = 98 blocks. Level 2 = $\lceil 98/21 \rceil = 5$. Level 3 = 1.
Multilevel search = 3 + 1 = **4 block accesses**.

---

## Section G: Transactions & Concurrency Control

### Q71. Solution

**Answer: (b) Consistency.** The consistency property ensures the database transitions between consistent states.

---

### Q72. Solution

$S: R_1(A), R_2(A), W_1(A), W_2(A), R_1(B), R_2(B), W_1(B), W_2(B)$

**(a) Precedence graph:**

For item $A$:
- $R_1(A)$ before $W_2(A)$: $T_1 \to T_2$
- $R_2(A)$ before $W_1(A)$: $T_2 \to T_1$
- $W_1(A)$ before $W_2(A)$: $T_1 \to T_2$

For item $B$:
- $R_1(B)$ before $W_2(B)$: $T_1 \to T_2$
- $R_2(B)$ before $W_1(B)$: $T_2 \to T_1$
- $W_1(B)$ before $W_2(B)$: $T_1 \to T_2$

Cycle: $T_1 \to T_2 \to T_1$. **NOT conflict serializable.**

**(b) Recoverability:** No commits shown. Assuming both commit at end: $T_1$ reads no data written by $T_2$ (and vice versa in this sequence). Actually check: $T_2$ reads $A$ before $T_1$ writes $A$, so $R_2(A)$ reads the original value. $T_2$ reads $B$ before $T_1$ writes $B$. Neither reads the other's written value. **Recoverable** (vacuously — no dirty reads).

---

### Q73. Solution

$S: R_1(X), W_2(X), W_1(X), R_2(X), W_2(Y), R_1(Y)$

**(a) Conflict serializability — precedence graph:**

Item $X$:
- $R_1(X)$ before $W_2(X)$: $T_1 \to T_2$
- $W_2(X)$ before $W_1(X)$: $T_2 \to T_1$
- $W_1(X)$ before $R_2(X)$: $T_1 \to T_2$

Item $Y$:
- $W_2(Y)$ before $R_1(Y)$: $T_2 \to T_1$

Cycle: $T_1 \to T_2 \to T_1$. **NOT conflict serializable.**

**(b) View serializability:** Check against serial orders $T_1T_2$ and $T_2T_1$.

In $S$: Initial read of $X$: $T_1$. $R_2(X)$ reads value written by $T_1$ (last write before $R_2(X)$ is $W_1(X)$). Final write on $X$: $W_1(X)$... no, after $W_1(X)$ there's $R_2(X)$ but no more writes on $X$. Wait — let me list writes on X: $W_2(X), W_1(X)$. Last write on X is $W_1(X)$. Initial read of Y: $R_1(Y)$ reads value written by $T_2$ ($W_2(Y)$ is before $R_1(Y)$). Final write on Y: $W_2(Y)$.

Serial $T_1T_2$: Initial read X: $T_1$ ✓. $R_2(X)$ reads from $T_1$? In serial $T_1T_2$, $T_1$ writes X, then $T_2$ reads X — reads $T_1$'s value ✓. Final write X: $T_2$ (in serial $T_1T_2$, $T_2$ writes after $T_1$). But in $S$, final write on X is $T_1$. ✗ Not view equivalent to $T_1T_2$.

Serial $T_2T_1$: Initial read X: $T_2$ reads X first. But in $S$, $T_1$ reads X first. ✗.

**NOT view serializable** (checking both serial orders fails).

Actually, let me re-examine. In $S$: $R_1(X)$ reads initial value. In $T_2T_1$: $T_2$ goes first, and $T_2$ doesn't read $X$ initially (its first op on X is $W_2(X)$). Actually $R_2(X)$ comes later. In serial $T_2T_1$: $T_2$ does $W_2(X), W_2(Y)$, then $T_1$ does $R_1(X)$, which reads $T_2$'s value of $X$. But in $S$, $R_1(X)$ reads initial value. So initial reads don't match. ✗.

**NOT view serializable.**

**(c) Recoverability:** $R_2(X)$ reads value written by $T_1$ ($W_1(X)$). If $T_2$ commits before $T_1$ → irrecoverable. $R_1(Y)$ reads value written by $T_2$ ($W_2(Y)$). If $T_1$ commits before $T_2$ → irrecoverable. So $T_1$ must commit before $T_2$ (for $R_2$) and $T_2$ must commit before $T_1$ (for $R_1$) — **contradiction**! **Irrecoverable.**

---

### Q74. Solution

$S: R_1(A), R_2(A), W_1(A), R_3(B), W_2(A), W_3(B), R_1(B), W_1(B)$

Precedence graph:

Item $A$:
- $R_1(A) < W_2(A)$: $T_1 \to T_2$
- $R_2(A) < W_1(A)$: $T_2 \to T_1$

Cycle: $T_1 \to T_2 \to T_1$. **NOT conflict serializable.**

---

### Q75. Solution

**Answer: (b)** The lock point is when the last lock is acquired, i.e., the end of the growing phase. The serialization order of 2PL transactions is determined by their lock points.

---

### Q76. Solution

$T_1$ locks: $L_1(A) \ldots U_1(A) \ldots L_1(B) \ldots U_1(B)$

$T_1$ acquires $A$, releases $A$, then acquires $B$ → acquires after releasing → **NOT 2PL** for $T_1$.

$T_2$ locks: $L_2(B) \ldots L_2(A) \ldots U_2(B), U_2(A)$ — acquires two locks, then releases both → **2PL** for $T_2$.

Since $T_1$ violates 2PL, the **schedule does NOT follow 2PL**. It is also not strict 2PL.

---

### Q77. Solution

$T_5$ (TS=5) wants to WRITE $X$. $R\_TS(X) = 8$, $W\_TS(X) = 3$.

Rule: If $TS(T_i) < R\_TS(X)$, abort. Here $5 < 8$ → **$T_5$ is aborted and restarted** (some younger transaction has already read $X$, and $T_5$'s write would invalidate that read).

---

### Q78. Solution

**Answer: (a)** Every conflict serializable schedule is view serializable. The converse is NOT true (blind writes can make schedules view serializable but not conflict serializable).

---

### Q79. Solution

$S: W_1(X), W_2(X), W_2(Y), W_3(Y), W_1(Y)$

**Conflict serializability:**
Item X: $W_1(X) < W_2(X)$: $T_1 \to T_2$
Item Y: $W_2(Y) < W_3(Y)$: $T_2 \to T_3$. $W_2(Y) < W_1(Y)$: $T_2 \to T_1$. $W_3(Y) < W_1(Y)$: $T_3 \to T_1$.

Edges: $T_1 \to T_2$, $T_2 \to T_3$, $T_2 \to T_1$, $T_3 \to T_1$

Cycle: $T_1 \to T_2 \to T_1$. **NOT conflict serializable.**

**View serializability:**
No reads at all! Only writes (blind writes). Check serial order $T_2, T_3, T_1$:
- No initial reads to match (no reads at all) ✓
- No read-from to match ✓
- Final write on X: $W_1(X)$ (in $S$, last write on X is $W_2(X)$). Actually wait: writes on X are $W_1(X), W_2(X)$. Last is $W_2(X)$. In serial $T_2T_3T_1$: last write on X is $W_1(X)$. ✗.

Try $T_3, T_1, T_2$: Final write X = $W_2(X)$ ✓. Final write Y = $W_2(Y)$. In $S$, final write Y is $W_1(Y)$. ✗.

Try $T_3, T_2, T_1$: Final write X = $W_1(X)$. In $S$, final write X is $W_2(X)$. ✗.

Try $T_1, T_3, T_2$: Final write X = $W_2(X)$ ✓. Final write Y = $W_2(Y)$. In S, final write Y = $W_1(Y)$. ✗.

Try $T_2, T_1, T_3$: Final write X = $W_1(X)$. In S: $W_2(X)$. ✗.

Try $T_1, T_2, T_3$: Final write X = $W_2(X)$ ✓ (matches S). Final write Y = $W_3(Y)$. In S, final write Y is $W_1(Y)$. ✗.

Hmm, in $S$: writes on Y: $W_2(Y), W_3(Y), W_1(Y)$. Final write Y = $W_1(Y)$. Writes on X: $W_1(X), W_2(X)$. Final write X = $W_2(X)$.

Need serial order where final write on X is $T_2$ and final write on Y is $T_1$.
→ $T_1$ must come after $T_2$ in write-on-Y sense (so $T_1$ is last to write Y), but $T_2$ must come after $T_1$ in write-on-X sense.
→ Order: ...$T_3$...$T_1$...$T_2$ for X, ...$T_2$...$T_3$...$T_1$ for Y.

Try $T_3, T_2, T_1$: X writes: $W_3$ doesn't write X, $W_2(X), W_1(X)$. Final X = $T_1$. ✗ (need $T_2$).

No serial order works. **NOT view serializable? Actually, with blind writes, let me reconsider.**

Since there are no reads, the only condition is the final write. We need final write on X to be $T_2$ and on Y to be $T_1$. Since $T_1$ writes both X and Y, and $T_2$ writes both X and Y, $T_1$ must come after $T_2$ for Y (last writer of Y) but before $T_2$ for X (T_2 is last writer of X). This means $T_1$ both before and after $T_2$ in a serial order — impossible.

**NOT view serializable.**

---

### Q80. Solution

**Wait-Die** protocol:
- Older waits, younger dies.
- $T_1$ (TS=10, older) requests $B$ held by $T_2$ (TS=5, younger). Older requests from younger → $T_1$ **waits**.
- $T_2$ (TS=5, younger) requests $A$ held by $T_1$ (TS=10, older). Younger requests from older → $T_2$ **dies** (is aborted).

$T_2$ is aborted and releases lock on $B$. $T_1$ gets $B$. Deadlock resolved.

---

### Q81. Solution

**Answer: (a)** A schedule is cascadeless if no transaction reads data written by an uncommitted transaction.

---

### Q82. Solution

$S: R_1(A), W_2(A), \text{Commit}_2, R_1(A)$

$T_1$ reads $A$ (initial value), then $T_2$ writes $A$ and commits, then $T_1$ reads $A$ again.

- The second $R_1(A)$ reads $A$ written by $T_2$, and $T_2$ has already committed before this read.

**(a) Recoverable?** $T_1$ reads from $T_2$, and $T_2$ committed before $T_1$ reads. If $T_1$ commits after $T_2$, yes. **Recoverable** ✓.

**(b) Cascadeless?** $T_1$ reads data written by $T_2$, and $T_2$ committed before $T_1$'s read → **Cascadeless** ✓.

**(c) Strict?** $T_1$ reads the item written by $T_2$, and $T_2$ committed before the read ✓. But also: $T_1$'s first $R_1(A)$ happens before $W_2(A)$, so $T_1$ read the old value and $T_2$ then overwrote it. For strictness: no transaction reads OR writes an item written by another until that other commits. Here $R_1(A)$ initially reads the original value, not $T_2$'s. $W_2(A)$ writes after $R_1(A)$ — this doesn't violate strictness (strictness concerns reading/writing items written by *uncommitted* transactions). **Strict** ✓.

---

### Q83. Solution

$T_1$ (TS=10) wants to WRITE $X$. $R\_TS(X) = 5$, $W\_TS(X) = 15$.

Check rules for write:
1. $TS(T_1) = 10 < R\_TS(X) = 5$? No ($10 \not< 5$).
2. $TS(T_1) = 10 < W\_TS(X) = 15$? Yes.

**Thomas Write Rule:** Since $TS(T_1) < W\_TS(X)$, the write is **obsolete** (a newer transaction already wrote $X$). **Skip the write, do NOT abort $T_1$.**

---

## Section H: ER Model

### Q84. Solution

**Answer: (d) Both (b) and (c).** A weak entity depends on an identifying entity and always has total participation in the identifying relationship.

---

### Q85. Solution

M:N relationship with a relationship attribute → must create a separate relationship table.

Tables: Table for $A$, table for $B$, table for $R$.

**Answer: 3 tables.**

---

### Q86. Solution

- `Student` → 1 table (sid, name)
- `Phone` (multivalued) → 1 separate table (sid, phone)
- `Course` → 1 table (cid, title)
- `Enrolled` (M:N) → 1 table (sid, cid, grade)

**Answer: 4 tables.**

---

### Q87. Solution

1:N with total participation on many side: Add FK on the many side. No separate relationship table needed.

**Answer: (b) 2 tables** (one for each entity).

---

### Q88. Solution

- $E_1$: 1 table
- $E_2$: 1 table
- $E_3$: 1 table
- Ternary relationship: 1 table
- Multivalued attribute of $E_1$: 1 table

**Answer: 5 tables.**

---

### Q89. Solution

`Employee(eid, ename, ...)`

`Dependent(eid, dname, relationship, ...)` where:
- Primary key = (eid, dname)
- eid is a foreign key referencing Employee(eid) with ON DELETE CASCADE

---

### Q90. Solution

Options:
1. **3 tables**: Table for $A$, table for $B$, relationship table with $a$ and $b$ as FK
2. **2 tables**: Add FK $b$ to table $A$ (or FK $a$ to table $B$) — works since both have total participation, no NULLs
3. **1 table**: Merge both into one table $(a, b, \text{attrs of A}, \text{attrs of B})$ — possible with 1:1 total-total

**Minimum: 1 table.**

---

## Bonus Questions

### Q91. Solution

B+ tree: $p = 200$, $p_l = 150$, 3 levels.

Level 1 (root): 1 node, max 199 keys, 200 pointers
Level 2: max 200 internal nodes, each with max 200 pointers = $200 \times 200 = 40000$ pointers
Level 3 (leaves): max 40000 leaf nodes, each with max 150 records

Max records = $200 \times 200 \times 150 = 6,000,000$

---

### Q92. Solution

$S: R_1(X), R_2(Y), W_1(Y), R_3(Z), W_2(X), W_3(Y), C_1, C_2, C_3$

**(a) Precedence graph:**
- Item X: $R_1(X) < W_2(X)$: $T_1 \to T_2$
- Item Y: $R_2(Y) < W_1(Y)$: $T_2 \to T_1$. $R_2(Y) < W_3(Y)$: $T_2 \to T_3$. $W_1(Y) < W_3(Y)$: $T_1 \to T_3$.

Edges: $T_1 \to T_2$, $T_2 \to T_1$, $T_2 \to T_3$, $T_1 \to T_3$.

Cycle: $T_1 \to T_2 \to T_1$. **NOT conflict serializable.**

**(b) Recoverability:** $T_2$ reads $Y$ (initial value, before $W_1(Y)$), $T_1$ reads $X$ (initial value). $R_3(Z)$ reads initial. No dirty reads → **Recoverable** (and cascadeless).

**(c) Strict 2PL:** Cannot determine without explicit lock operations. But since it's not conflict serializable, it could NOT have been produced by 2PL (2PL guarantees conflict serializability). **Does NOT follow 2PL.**

---

### Q93. Solution

$R(A,B,C,D,E)$, FDs: $\{A \to B, BC \to D, D \to E, E \to A\}$

Note: $A \to B$, $B \to$ (nothing alone), $E \to A \to B$, $D \to E \to A \to B$.

$(AC)^+ = \{A,C,B,D,E\} = R$. CK.
$(EC)^+ = \{E,C,A,B,D\} = R$. CK.
$(DC)^+ = \{D,C,E,A,B\} = R$. CK.
$(BC)^+ = \{B,C,D,E,A\} = R$. CK.

All have $C$ (since $C$ never appears on RHS). Check: $A^+ = \{A,B\}$, $B^+ = \{B\}$, $C^+ = \{C\}$, etc. None alone suffices.

**CKs: AC, BC, DC, EC** (4 candidate keys). Prime: $\{A,B,C,D,E\}$ — all prime!

3NF: All RHS of non-trivial FDs are prime. ✓ **3NF.**

BCNF: $A \to B$: $A^+ = \{A,B\} \neq R$ → **NOT BCNF**.

**Highest NF: 3NF (not BCNF). 4 candidate keys.**

---

### Q94. Solution

```sql
SELECT DISTINCT E.name
FROM Employee E, Department D
WHERE E.dept_id = D.dept_id
  AND E.age > 25
  AND D.location = 'Delhi';
```

---

### Q95. Solution

Records = 1,000,000. Record = 200B, Block = 4096B, Key = 20B, Pointer = 8B.

$f = \lfloor 4096/200 \rfloor = 20$. Data blocks = $\lceil 1000000/20 \rceil = 50000$.

**(a) Sequential scan:** 50,000 block accesses (worst case).

**(b) Binary search:** $\lceil \log_2(50000) \rceil = 16$ block accesses.

**(c) B+ tree:**
Internal order: $p = \lfloor (4096 + 20)/(20 + 8) \rfloor = \lfloor 4116/28 \rfloor = 147$.
Leaf order: $p_l = \lfloor (4096 - 8)/(20 + 8) \rfloor = \lfloor 4088/28 \rfloor = 146$.

Leaves: $\lceil 1000000/146 \rceil = 6850$.
Level 2: $\lceil 6850/147 \rceil = 47$.
Level 3: $\lceil 47/147 \rceil = 1$.

Height = 3. Search = **3 + 1 = 4 block accesses** (3 tree levels + 1 data block).

---

## Section I: 4NF/5NF, MVDs, Recovery, Hashing, Query Processing

### Q96. **Answer: (b) BCNF but not 4NF**
R(A,B,C) with MVDs A ↠ B and A ↠ C. A is not a superkey, so the MVDs are non-trivial and violate 4NF. But there are no FD violations for BCNF (MVDs don't affect BCNF).

---

### Q97. **Answer:**
MVDs: Student ↠ Course, Student ↠ Hobby (courses and hobbies are independent).
Student is not a superkey of R → violates 4NF.
Decompose: **R1(Student, Course)**, **R2(Student, Hobby)**. Both are in 4NF.

---

### Q98. **Answer: (a) 3NF ⊂ BCNF ⊂ 4NF ⊂ 5NF**
5NF is the strictest. The set of relations in 5NF ⊂ 4NF ⊂ BCNF ⊂ 3NF (as sets of valid relations). As "normal form level": 1NF → 2NF → 3NF → BCNF → 4NF → 5NF (increasing strictness).

---

### Q99. **Answer: (c) Domain values**
In DRC, variables range over individual domain values (attribute values), not entire tuples.

---

### Q100. **Answer:**
$\{\langle n \rangle \mid \exists r, \exists d, \exists g (\text{Student}(r, n, d, g) \wedge g > 8)\}$

---

### Q101. **Answer: (b) Log records must be flushed before modified data pages**
WAL ensures that if a data page is written to disk, all log records pertaining to that page have already been flushed to stable storage.

---

### Q102. **Answer:**
After checkpoint, active = {T2}. T1 committed before checkpoint.
- T2: started before checkpoint, did NOT commit → **Undo T2** (restore C to 50)
- T1: committed → **Redo T1** (ensure A = 20)

T2's update (C: 50→60) after checkpoint is undone. T1's updates before checkpoint are guaranteed by checkpoint, but we redo from checkpoint to confirm.

---

### Q103. **Answer:**
Three ARIES phases:
1. **Analysis:** Scan log from last checkpoint → build Dirty Page Table (DPT) and Active Transaction Table (ATT)
2. **Redo:** Start from earliest LSN in DPT → redo ALL logged updates (even uncommitted) → "repeating history" = restore DB to exact state at crash time
3. **Undo:** Walk backward undoing all updates from uncommitted transactions, logging CLR records

"Repeating history" means we redo ALL updates (committed and uncommitted) to bring the database to the pre-crash state, then undo the uncommitted ones.

---

### Q104. **Answer: (b) Reduce the amount of log to process during recovery**
A checkpoint records active transactions and dirty pages, allowing recovery to start from the checkpoint instead of scanning the entire log.

---

### Q105. **Answer: (c) Read Uncommitted**
Read Uncommitted allows dirty reads. Read Committed prevents dirty reads. Repeatable Read also prevents non-repeatable reads. Serializable prevents all anomalies.

---

### Q106. **Answer:**
1. **Dirty read:** T reads data written by uncommitted T' → Prevented at **Read Committed** level
2. **Non-repeatable read:** T reads X, T' modifies X, T reads X again (different value) → Prevented at **Repeatable Read** level
3. **Phantom read:** T does range query, T' inserts/deletes in range, T repeats query (different rows) → Prevented at **Serializable** level

---

### Q107. **Answer: (c) Never block and are never blocked**
In MVCC, readers see a consistent snapshot (appropriate version). Writers create new versions. So readers and writers don't interfere with each other.

---

### Q108. **Answer: (b) When a bucket with local depth = global depth overflows**
If local depth < global depth, only the bucket splits (directory stays same). If local depth = global depth, the directory must double (global depth increases by 1).

---

### Q109. **Answer:**
Bucket '00' has local depth 2 = global depth 2 → **directory doubles** to global depth 3 (8 entries).
Then bucket '00' splits: entries rehashed using bit 3 into buckets '000' (local depth 3) and '100' (local depth 3).
New global depth = **3**.

---

### Q110. **Answer: (d) Both (b) and (c)**
In linear hashing, the split pointer moves sequentially through buckets (0, 1, 2, ..., N-1) and always points to the next bucket to be split, regardless of which bucket overflowed.

---

### Q111. **Answer: (c) Hash join**
Hash join: partition phase costs 2(br+bs), probe phase costs (br+bs) → total = 3(br+bs).

---

### Q112. **Answer:**
Block nested loop: ⌈br/(M-2)⌉ × bs + br
= ⌈5000/50⌉ × 200 + 5000
= 100 × 200 + 5000
= 20000 + 5000 = **25,000 block transfers**
(M-2 because 1 buffer for inner, 1 for output)

---

### Q113. **Answer: (b) Push selections before joins**
Pushing selections early reduces the size of intermediate relations dramatically, which is the single most impactful optimization.

---

### Q114. **Answer: (b) Virtual tables (queries stored, not data)**
Views store the defining query, not the result. The query is executed when the view is referenced.

---

### Q115. **Answer: (d) Must have a WHERE clause**
A view is updatable if it's based on a single table, has no aggregates, no GROUP BY, no DISTINCT, no complex expressions. Having a WHERE clause is fine — it's not a restriction.

---

### Q116. **Answer: (b) Bottom-up**
Generalization combines lower-level entities into a higher-level entity (bottom-up). Specialization is top-down.

---

### Q117. **Answer:**
(i) **Separate table per entity (3 tables):**
- Employee(eid, name, salary)
- Manager(eid, bonus) — FK: eid → Employee
- Engineer(eid, skill) — FK: eid → Employee

(ii) **Only sub-entity tables (2 tables)** — possible because disjoint + total:
- Manager(eid, name, salary, bonus)
- Engineer(eid, name, salary, skill)

---

### Q118. **Answer: (b) An action automatically fired on a database event**
Triggers fire automatically in response to INSERT, UPDATE, or DELETE events.

---

### Q119. **Answer:**
MVDs: A ↠ BC and A ↠ DE. A is not a superkey of R(A,B,C,D,E) → violates 4NF.
Decompose: R1(A,B,C) and R2(A,D,E).
In R1: FD A → B holds. A+ = {A,B} ≠ {A,B,C} → A is not superkey of R1 → R1 is NOT in BCNF.
Decompose R1: R11(A,B), R12(A,C).
Final: **R11(A,B), R12(A,C), R2(A,D,E)** — all in 4NF.

---

### Q120. **Answer: (a) Redo T1 and undo T2**
In undo-redo logging: committed transactions (T1) must be redone to ensure durability. Uncommitted transactions (T2) must be undone to ensure atomicity.

---

*End of Solutions — DBMS for GATE 2027*
