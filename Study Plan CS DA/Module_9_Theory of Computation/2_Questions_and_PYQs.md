# Theory of Computation — Questions and PYQs for GATE 2027

> **Total Questions:** 70 | **Sections:** 7
> **Difficulty Mix:** Easy (30%) | Medium (40%) | GATE PYQ level (30%)

---

## Section A: DFA & NFA (12 Questions)

### Easy
**Q1.** Construct a DFA over {a,b} that accepts strings with an even number of a's.

**Q2.** Construct a DFA over {0,1} for strings that end with "01".

**Q3.** How many states does the minimum DFA for L = {w ∈ {a,b}* | w contains "aba"} have?

### Medium
**Q4.** Convert the following NFA to DFA:
States = {q₀, q₁, q₂}, Σ = {0,1}, start = q₀, final = {q₂}.
δ(q₀,0)={q₀,q₁}, δ(q₀,1)={q₀}, δ(q₁,0)=∅, δ(q₁,1)={q₂}, δ(q₂,0)=∅, δ(q₂,1)=∅.

**Q5.** Construct DFAs for L₁ = "strings with even number of 0s" and L₂ = "strings with odd number of 1s". Using product construction, build DFA for L₁ ∩ L₂.

**Q6.** Minimize the following DFA:
States = {A,B,C,D,E}, Σ = {0,1}, start = A, final = {D}.
δ: A→(B,C), B→(D,C), C→(B,C), D→(D,E), E→(D,C).
(Format: state→(on 0, on 1))

**Q7.** An NFA has 5 states. What is the maximum number of states in the equivalent DFA?

### GATE Level
**Q8.** [GATE 2019] The minimum number of states in a DFA that accepts L = {w ∈ {0,1}* | the number of 0s is divisible by 3 AND the number of 1s is divisible by 5} is _____.

**Q9.** [GATE 2015] Let L = {w ∈ {0,1}* | w has equal number of 01 and 10 substrings}. Is L regular?

**Q10.** [GATE] The number of strings of length n over {0,1} accepted by a DFA with 2 states is always: (a) 0 (b) 2ⁿ (c) depends on DFA (d) 2ⁿ⁻¹

**Q11.** [GATE 2020] If L₁ is regular and L₁ ∪ L₂ is regular, is L₂ necessarily regular?

**Q12.** [GATE] Given DFA M with n states. The complement language L(M)' is accepted by a DFA with how many states?

---

## Section B: Regular Expressions (8 Questions)

### Easy
**Q13.** Write a regular expression for strings over {a,b} that start with 'a' and end with 'b'.

**Q14.** Write a regular expression for strings over {0,1} with at least two 0s.

### Medium
**Q15.** Convert to RE: DFA with states {q₀, q₁}, start=q₀, final={q₁}. δ(q₀,a)=q₁, δ(q₀,b)=q₀, δ(q₁,a)=q₀, δ(q₁,b)=q₁.

**Q16.** Using Arden's theorem, derive the RE for the the DFA in Q15.

**Q17.** Is the language described by (a+b)*a(a+b)*b(a+b)* a subset of a(a+b)*b? Justify.

### GATE Level
**Q18.** [GATE] Which of the following REs denotes the language of strings over {0,1} where every 0 is immediately followed by 1?
(a) (1+01)* (b) 1*(01*)* (c) (1*01*)* (d) (1+01)*0

**Q19.** [GATE 2017] The regular expression 0*(10*)* is equivalent to:
(a) (0+1)* (b) 0*1* (c) (01)* (d) 0*(10*)*

**Q20.** Simplify: (a+b)*(a+ε)(b+ε)

---

## Section C: Pumping Lemma & Non-Regular Languages (10 Questions)

### Easy
**Q21.** Use the pumping lemma to prove {0ⁿ1ⁿ | n ≥ 0} is not regular.

**Q22.** Is L = {w ∈ {a,b}* | |w| is even} regular? Justify.

### Medium
**Q23.** Prove {aⁿ² | n ≥ 1} is not regular using the pumping lemma.

**Q24.** Prove {ww | w ∈ {0,1}*} is not regular.

**Q25.** Using Myhill-Nerode theorem, prove {aⁿbⁿ | n ≥ 0} is not regular by showing infinitely many equivalence classes.

**Q26.** Is L = {aⁿbᵐ | n ≤ m} regular? Prove or disprove.

### GATE Level
**Q27.** [GATE] Which of the following is regular?
(a) {aⁿbⁿ | n ≥ 0} (b) {aⁿ | n is prime} (c) {aⁿbᵐ | n ≠ m} (d) {w ∈ {a,b}* | #a(w) = #b(w)}

**Q28.** [GATE 2016] Let L₁ = {aⁿbⁿ | n ≥ 0} and L₂ = {aⁿ | n ≥ 0}. Is L₁ ∩ L₂ regular?

**Q29.** [GATE] Is {w ∈ {a,b}* | w is a palindrome} regular? CFL? Justify.

**Q30.** [GATE] If L is regular and L' is not regular (where L' ∩ R = L for some regular R), what can be said about L'?

---

## Section D: CFG & CFL (10 Questions)

### Easy
**Q31.** Write a CFG for L = {aⁿbⁿ | n ≥ 1}.

**Q32.** Write a CFG for balanced parentheses.

**Q33.** Is the grammar S → aS | Sa | a ambiguous?

### Medium
**Q34.** Convert to CNF: S → ASB | ε, A → aAS | a, B → SbS | A | bb.

**Q35.** Show that the grammar E → E+E | E*E | (E) | id is ambiguous for "id+id*id".

**Q36.** Design a CFG for L = {aⁱbʲcᵏ | i + k = j, i ≥ 0, k ≥ 0}.

**Q37.** Prove {aⁿbⁿcⁿ | n ≥ 0} is NOT a CFL using the pumping lemma for CFLs.

### GATE Level
**Q38.** [GATE 2018] The language L = {aⁱbʲcᵏ | i = j or j = k} is:
(a) Regular (b) CFL but not regular (c) CSL but not CFL (d) Inherently ambiguous CFL

**Q39.** [GATE] A CFG in CNF generating strings of length n requires exactly _____ derivation steps.

**Q40.** [GATE] Which of the following is NOT a CFL?
(a) {aⁿbⁿ | n ≥ 0} (b) {ww^R | w ∈ {a,b}*} (c) {aⁿbⁿcⁿ | n ≥ 0} (d) {aⁱbʲ | i ≠ j}

---

## Section E: Pushdown Automata (8 Questions)

### Easy
**Q41.** Design a PDA (by empty stack) for L = {aⁿbⁿ | n ≥ 1}.

**Q42.** Can a DPDA accept {ww^R | w ∈ {a,b}*}?

### Medium
**Q43.** Design a PDA for L = {w ∈ {a,b}* | #a(w) = #b(w)}.

**Q44.** Convert the following CFG to a PDA: S → aSb | ε.

**Q45.** Is L = {aⁿbⁿ | n ≥ 0} ∪ {aⁿbⁿcⁿ | n ≥ 0} a DCFL?

### GATE Level
**Q46.** [GATE] Which of the following is accepted by a DPDA but not by any DFA?
(a) {aⁿbⁿ | n ≥ 0} (b) {ww | w ∈ {a,b}*} (c) {aⁿbⁿcⁿ} (d) {a,b}*

**Q47.** [GATE 2017] If L is a DCFL and R is a regular language, is L ∩ R a DCFL?

**Q48.** [GATE] A PDA with 2 stacks is equivalent to: (a) DFA (b) TM (c) DPDA (d) LBA

---

## Section F: Closure & Decision Properties (10 Questions)

### Easy
**Q49.** Are regular languages closed under intersection? Union? Complement?

**Q50.** Is the emptiness problem decidable for CFLs?

### Medium
**Q51.** L₁ is CFL, L₂ is regular. Is L₁ ∩ L₂ a CFL?

**Q52.** L₁ and L₂ are both CFLs. Is L₁ ∩ L₂ necessarily a CFL?

**Q53.** Is the equivalence problem (L₁ = L₂) decidable for (a) regular languages (b) CFLs?

**Q54.** If L is regular, is Lᴿ (reversal) regular? If L is CFL, is Lᴿ CFL?

### GATE Level
**Q55.** [GATE] Which of the following problems is undecidable?
(a) Is a given CFG ambiguous? (b) Is L(G) = ∅ for CFG G? (c) Is w ∈ L(G) for CFG G? (d) Is L(G) finite?

**Q56.** [GATE 2019] L₁ is CFL, L₂ is CFL. Which is always true?
(a) L₁ ∩ L₂ is CFL (b) L₁ ∪ L₂ is CFL (c) L₁' is CFL (d) L₁ − L₂ is CFL

**Q57.** [GATE] If L is RE and L' is also RE, then L is: (a) RE (b) recursive (c) CFL (d) regular

**Q58.** Show that CFL is not closed under complement by giving a counterexample.

---

## Section G: Turing Machines & Decidability (12 Questions)

### Easy
**Q59.** Design a TM to accept {aⁿbⁿcⁿ | n ≥ 1} (describe the high-level idea).

**Q60.** What is the difference between decidable and recognizable languages?

### Medium
**Q61.** Prove using Rice's theorem that "L(M) is finite" is undecidable.

**Q62.** Is the halting problem recognizable? Co-recognizable?

**Q63.** Prove that {M | M is a TM and L(M) = Σ*} is not recognizable (not RE).

**Q64.** If A ≤_m B and B is decidable, what can we conclude about A?

### GATE Level
**Q65.** [GATE 2020] Which of the following is decidable?
(a) Does TM M accept ε? (b) Is L(M) regular? (c) Does M halt on all inputs? (d) Given DFA D, is L(D) = Σ*?

**Q66.** [GATE] The complement of the halting problem is:
(a) decidable (b) RE (c) not RE (d) regular

**Q67.** [GATE 2018] Consider: L₁ = {⟨M⟩ | M halts on every input}. L₁ is:
(a) decidable (b) RE but not decidable (c) not RE (d) recursive

**Q68.** [GATE] Which is true about Rice's theorem?
(a) It applies to structural properties of TMs
(b) It says every language property of TMs is undecidable
(c) It applies only to non-trivial language properties
(d) It proves the halting problem undecidable

**Q69.** [GATE 2016] If L₁ is RE and L₂ is RE, which is NOT necessarily RE?
(a) L₁ ∪ L₂ (b) L₁ ∩ L₂ (c) L₁' (d) L₁ · L₂

**Q70.** [GATE] The Post Correspondence Problem is:
(a) decidable (b) recognizable (c) not recognizable (d) decidable for unary alphabet

---

## Section H: CYK Algorithm & Parsing (5 Questions)

**Q71.** [GATE] Given the CNF grammar: S → AB | BC, A → BA | a, B → CC | b, C → AB | a. Using CYK, determine if "baa" ∈ L(G). Show the CYK table.

**Q72.** [GATE 2015] The CYK algorithm has time complexity:
(a) O(n²) (b) O(n³) (c) O(n² · |G|) (d) O(n³ · |G|)

**Q73.** [GATE] A CFG must be converted to which normal form before applying CYK?
(a) GNF (b) CNF (c) BNF (d) Any form works

**Q74.** [PYQ Variant] Consider G with S → AB, A → a, B → b | BC, C → c. Is "abc" in L(G) using CYK?

**Q75.** [GATE] The CYK algorithm uses which programming paradigm?
(a) Greedy (b) Divide & Conquer (c) Dynamic Programming (d) Backtracking

---

## Section I: LBA & Context-Sensitive Languages (5 Questions)

**Q76.** [GATE] The language recognized by a Linear Bounded Automaton is:
(a) Regular (b) Context-free (c) Context-sensitive (d) Recursively enumerable

**Q77.** [GATE 2017] Which of the following is decidable for LBAs?
(a) Emptiness (b) Membership (c) Equivalence (d) Universality

**Q78.** [GATE] The emptiness problem for LBAs is:
(a) decidable (b) undecidable (c) not RE (d) decidable iff the LBA is deterministic

**Q79.** [GATE] An LBA uses tape space:
(a) proportional to the number of states (b) equal to the input length (c) unbounded (d) logarithmic in input length

**Q80.** [PYQ Variant] Which language is CSL but not CFL?
(a) {aⁿbⁿ | n ≥ 1} (b) {aⁿbⁿcⁿ | n ≥ 1} (c) {aⁿ | n is prime} (d) Both (b) and (c)

---

## Section J: Countability, Church-Turing & Advanced TM (8 Questions)

**Q81.** [GATE] The set of all Turing machines over alphabet {0,1} is:
(a) finite (b) countably infinite (c) uncountably infinite (d) not a set

**Q82.** [GATE 2019] The set of all languages over {a,b} is:
(a) finite (b) countable (c) uncountable (d) recursively enumerable

**Q83.** [GATE] Which of the following is a consequence of the fact that languages are uncountable but TMs are countable?
(a) Every language has a TM (b) Some TMs recognize no language (c) There exist languages not recognized by any TM (d) P = NP

**Q84.** [GATE] The Church-Turing thesis states:
(a) Every language is decidable (b) Turing machines can solve any problem (c) Every effectively computable function is TM-computable (d) P ≠ NP

**Q85.** [GATE] A non-deterministic TM can be simulated by a deterministic TM with:
(a) polynomial time overhead (b) exponential time overhead (c) no overhead (d) logarithmic overhead

**Q86.** [GATE 2020] An enumerator E enumerates a language L. Which is true?
(a) L must be decidable (b) L must be RE (c) L must be finite (d) E must halt

**Q87.** [GATE] If an enumerator prints strings in lexicographic order, the language is:
(a) RE but not decidable (b) decidable (c) co-RE (d) context-free

**Q88.** [GATE] A 2-tape TM running in time t(n) can be simulated by a 1-tape TM in time:
(a) O(t(n)) (b) O(t(n) log t(n)) (c) O(t(n)²) (d) O(2^t(n))

---

## Section K: Advanced Undecidability & PCP (7 Questions)

**Q89.** [GATE] Rice's theorem CANNOT be used to prove undecidability of:
(a) "L(M) is regular" (b) "M has at most 5 states" (c) "L(M) is finite" (d) "L(M) contains ε"

**Q90.** [GATE 2016] Given the PCP instance: Pair 1: (ab, b), Pair 2: (b, ab), Pair 3: (a, aa). Does this instance have a solution? If yes, give the sequence.

**Q91.** [GATE] The reduction chain used to prove CFG ambiguity is undecidable is:
(a) HP → PCP → CFG Ambiguity (b) PCP → HP → CFG Ambiguity (c) HP → MPCP → PCP → CFG Ambiguity (d) CFG Ambiguity → PCP → HP

**Q92.** [GATE] Which of these problems is decidable?
(a) Does TM M halt on input ε? (b) Does M have exactly 42 states? (c) Is L(M) = {0,1}*? (d) Is L(M) regular?

**Q93.** [GATE 2014] L₁ is RE but not recursive. L₂ is the complement of L₁. Which is true?
(a) L₂ is RE (b) L₂ is recursive (c) L₂ is not RE (d) L₂ is regular

**Q94.** [GATE] If A ≤_m B and B is decidable, then:
(a) A is undecidable (b) A is decidable (c) A is not RE (d) Nothing can be concluded

**Q95.** [GATE] Consider: "Does TM M write the symbol '#' on its tape when run on input w?" This problem is:
(a) decidable (b) undecidable — by Rice's (c) undecidable — not by Rice's (reducible from HP) (d) not RE

---

*End of Questions — Theory of Computation for GATE 2027*
