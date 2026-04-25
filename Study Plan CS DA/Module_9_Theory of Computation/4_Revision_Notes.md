# Theory of Computation — Revision Notes for GATE 2027

> **Quick-reference sheet** | **GATE Weightage:** 4–6 marks

---

## 1. Chomsky Hierarchy

| Type | Language | Machine | Closed Under |
|------|----------|---------|-------------|
| 3 | Regular | DFA/NFA | ∪, ∩, ', ·, *, ᴿ, h, h⁻¹ |
| 2 | CFL | NPDA | ∪, ·, *, ᴿ, h, h⁻¹ (NOT ∩, ') |
| - | DCFL | DPDA | ' , ∩Reg (NOT ∪, ·, *) |
| 1 | CSL | LBA | ∪, ∩, ·, *, ' |
| 0 | RE | TM | ∪, ∩, ·, * (NOT ') |

**Proper containment:** Regular ⊂ DCFL ⊂ CFL ⊂ CSL ⊂ Recursive ⊂ RE

---

## 2. Finite Automata

- **NFA → DFA:** Subset construction. Max 2ⁿ states.
- **ε-closure:** All states reachable via ε from a state.
- **DFA minimization:** Table-filling. Min states = Myhill-Nerode equivalence classes.
- **Product DFA:** |Q₁|×|Q₂| states. ∩ → both final. ∪ → either final.
- **Complement DFA:** Same DFA, swap final/non-final. Same #states.

---

## 3. Regular Languages — Key Facts

- **RE ↔ NFA ↔ DFA** (all equivalent)
- **Pumping Lemma:** For DISPROVING regularity only
  - w = xyz, |y|>0, |xy|≤p, xy^iz ∈ L ∀i
- **Arden's:** R = Q + RP → R = QP* (P must not contain ε)
- **Myhill-Nerode:** #equiv classes = min DFA states

### Quick Non-Regular Examples
aⁿbⁿ, ww, palindromes, aⁿ (n prime), aⁿ² 

---

## 4. Context-Free Languages

- **CFG ↔ NPDA** (equivalent)
- **DPDA ⊂ NPDA** (strict)
- **CNF:** A→BC | a. Derivation of length n: **2n−1** steps.
- **GNF:** A→aα. One terminal per step.
- **Ambiguous:** String has 2+ parse trees. Inherently ambiguous: EVERY grammar is ambiguous.

### CFL Pumping Lemma
w = uvxyz, |vy|>0, |vxy|≤p, uv^ixy^iz ∈ L ∀i

### Key Non-CFL Examples
aⁿbⁿcⁿ, ww, {aⁱbʲcᵏ | i<j<k}

### Critical: CFL ∩ Regular = CFL (always!)

---

## 5. Decision Properties

| Problem | Regular | CFL | CSL (LBA) | RE |
|---------|---------|-----|-----------|----|
| Membership | ✓ O(n) | ✓ O(n³) CYK | ✓ (finite configs) | ✓ (may not halt) |
| Emptiness | ✓ | ✓ | **✗** | ✗ (Rice's) |
| Finiteness | ✓ | ✓ | **✗** | ✗ (Rice's) |
| Equivalence | ✓ | **✗** | **✗** | ✗ |
| Universality | ✓ | **✗** | **✗** | ✗ |
| Ambiguity | — | **✗** | — | — |

---

## 5a. CYK Algorithm Quick Reference

- **Input:** CFG in CNF, string w of length n
- **Method:** Bottom-up DP, fill triangular table T[i,j]
- **Base:** T[i,i] = {A | A → aᵢ}
- **Fill:** T[i,j] = ∪ₖ {A | A → BC, B ∈ T[i,k], C ∈ T[k+1,j]} for all split points k
- **Accept:** S ∈ T[1,n]
- **Time:** $O(n^3 \cdot |G|)$, **Space:** $O(n^2)$
- Requires CNF — convert grammar first!

---

## 6. Turing Machines & Decidability

- **Decidable = Recursive:** TM halts on ALL inputs
- **RE (Recognizable):** TM accepts members; may loop on non-members
- **L decidable ↔ L ∈ RE ∩ co-RE**
- TM variants (multi-tape, NDTM, etc.) = same power as 1-tape DTM
- **Multi-tape → 1-tape:** O(t(n)²) time overhead
- **NDTM → DTM:** Exponential time overhead (explore all branches)
- **2-stack PDA = TM**

### LBA (Linear Bounded Automaton)
- TM restricted to input length tape
- Recognizes **CSL (Context-Sensitive Languages)**
- Membership: **Decidable** (bounded configs: |Q|·n·|Γ|ⁿ)
- Emptiness/Equivalence: **Undecidable**
- DLBA =? NLBA: **Open problem**

### Countability
- TMs (programs): **Countable** (finite encodings ⊆ Σ*)
- Languages: **Uncountable** (P(Σ*) by Cantor)
- → Most languages have NO TM at all

### Church-Turing Thesis
- Every effectively computable function is TM-computable
- NOT a theorem — a thesis (cannot be proved)
- All known models (λ-calculus, μ-recursive, RAM) equivalent to TM

### Enumerators
- TM with a printer that outputs strings
- L is RE ↔ some enumerator enumerates L
- L is decidable ↔ some enumerator enumerates L **in lexicographic order**

### Rice's Theorem
Any **non-trivial property** of the **language** of a TM is undecidable.
- Non-trivial = not always true, not always false
- Language property = about L(M), not about M's structure

### Halting Problem
- RE but NOT decidable
- Complement: NOT RE
- Proof: diagonalization

### Common Undecidable Problems
Is L(M)=∅? Is L(M) finite? Is L(M) regular? Is L(M)=Σ*?
Does M halt on w? Does M halt on all inputs? PCP (for |Σ|≥2).

**PCP key facts:**
- RE but not decidable
- Decidable for |Σ|=1
- Reduction chain: HP → MPCP → PCP → CFG Ambiguity

**Rice's Proof Idea:** Reduce halting problem — build M' whose language depends on whether M halts on w.

### Reduction: A ≤_m B
- B decidable → A decidable
- A undecidable → B undecidable
- B RE → A RE

---

## 7. GATE Traps

1. **Pumping lemma proves regularity** → WRONG (only disproves)
2. **CFL closed under intersection** → WRONG
3. **DPDA = NPDA** → WRONG (DPDA strictly weaker)
4. **Rice's for TM structure** → WRONG (only for language properties)
5. **CFL ∩ Regular not CFL** → WRONG (always CFL)
6. **NFA always has fewer states than DFA** → Not always, just sometimes
7. **Halting problem not RE** → WRONG (it IS RE, just not decidable)
8. **All TM language properties undecidable** → Only NON-TRIVIAL ones
9. **DCFL closed under union** → WRONG
10. **CNF steps for length n** → 2n−1 (not 2n or n)
11. **LBA emptiness is decidable** → WRONG (it's undecidable)
12. **All languages have a TM** → WRONG (uncountably many languages, countably many TMs)
13. **Church-Turing Thesis is a theorem** → WRONG (it's an unproved thesis)
14. **Enumerator must halt** → WRONG (can run forever, printing strings)
15. **NDTM is faster than DTM** → Not "faster" — it can "guess" (exponential simulation cost)

---

## 8. Equivalence Cheat Sheet

| = | = |
|---|---|
| DFA | NFA | ε-NFA | RE |
| NPDA | CFG |
| DPDA | LR(k) grammars |
| TM | 2-stack PDA | multi-tape TM | NDTM |
| LBA | CSL recognizer |
| Enumerator (any order) | RE language |
| Enumerator (sorted) | Decidable language |

---

*End of Revision Notes — Theory of Computation for GATE 2027*
