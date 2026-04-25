# Compiler Design — Questions & PYQs for GATE 2027

> **Total: 70 Questions** | Detailed solutions in `3_Solutions.md`

---

## Section A: Lexical Analysis (8 Questions)

### Q1. [GATE 2015 — 1 mark]
Which of the following is NOT a token in C?
(a) `int`  (b) `42`  (c) `//comment`  (d) `+`

### Q2. [Practice — 2 marks]
The regular expression for identifiers in C (start with letter or underscore, followed by letters, digits, or underscores) is:
(a) `(letter|_)(letter|digit|_)*`  (b) `letter(letter|digit)*`  (c) `(letter|digit|_)+`  (d) `letter+digit*`

### Q3. [GATE 2017 — 1 mark]
A lexical analyzer uses the maximal munch rule. For input `iffy`, the token produced is:
(a) keyword `if` + identifier `fy`  (b) identifier `iffy`  (c) keyword `if` + keyword `fy`  (d) error

### Q4. [Practice — 2 marks]
How many tokens are in: `int x = a + b * 2;`?

### Q5. [Practice — 1 mark]
The lexical analyzer outputs tokens to which phase?
(a) Preprocessor  (b) Syntax analyzer  (c) Semantic analyzer  (d) Code generator

### Q6. [Practice — 2 marks]
Given the regex `a(b|c)*d`, which of the following strings are accepted? (i) ad (ii) abcd (iii) abd (iv) abbd (v) acd

### Q7. [GATE 2012 — 1 mark]
Which phase of the compiler identifies tokens?
(a) Syntax analysis  (b) Semantic analysis  (c) Lexical analysis  (d) Code generation

### Q8. [Practice — 2 marks]
A DFA for recognizing integer and float tokens has states. Float is `digit+.digit+`. For input "12.34", which token is produced and why?

---

## Section B: Top-Down Parsing — FIRST, FOLLOW, LL(1) (15 Questions)

### Q9. [GATE 2010 — 2 marks]
For grammar: S → aBDh, B → cC, C → bC | ε, D → EF, E → g | ε, F → f | ε
Compute FIRST(S) and FOLLOW(C).

### Q10. [Practice — 2 marks]
Compute FIRST and FOLLOW for:
```
S → ACB | CbB | Ba
A → da | BC
B → g | ε
C → h | ε
```

### Q11. [GATE 2016 — 2 marks]
Which of the following grammars is LL(1)?
(a) S → Sa | b  (b) S → aS | a  (c) S → aB, B → bB | ε  (d) S → aSb | ab

### Q12. [Practice — 2 marks]
Build the LL(1) parsing table for: E → TE', E' → +TE' | ε, T → id
Is this LL(1)?

### Q13. [GATE 2019 — 2 marks]
Given grammar: S → Aa | b, A → Ac | Sd | ε
After eliminating left recursion and left factoring, is the resulting grammar LL(1)?

### Q14. [Practice — 1 mark]
Which of the following is TRUE?
(a) Every LL(1) grammar is ambiguous  
(b) Every LL(1) grammar is unambiguous  
(c) Some LL(1) grammars are ambiguous  
(d) LL(1) grammars cannot handle ε-productions

### Q15. [GATE 2013 — 2 marks]
For grammar S → aABb, A → c | ε, B → d | ε, what is FOLLOW(A)?

### Q16. [Practice — 2 marks]
Trace the LL(1) parser for input `id + id * id` using the standard expression grammar.

### Q17. [Practice — 1 mark]
Left factoring is needed when two productions for the same non-terminal share a common:
(a) Suffix  (b) Prefix  (c) FIRST set  (d) FOLLOW set

### Q18. [GATE 2020 — 2 marks]
Eliminate left recursion from: A → Aab | Aba | c | d

### Q19. [Practice — 2 marks]
A grammar has productions A → α | β. If FIRST(α) ∩ FIRST(β) ≠ ∅, can this grammar be LL(1)?

### Q20. [GATE 2011 — 2 marks]
Given S → iCtSS' | a, S' → eS | ε, C → b
Is this grammar LL(1)? If not, which conflict arises?

### Q21. [Practice — 2 marks]
For a recursive descent parser: if the grammar is S → aSb | ε, write the parsing function for S.

### Q22. [Practice — 1 mark]
The LL(1) parsing table for an unambiguous grammar can have at most _____ entry per cell.
(a) 0 (b) 1 (c) 2 (d) Unlimited

### Q23. [GATE 2018 — 2 marks]
Grammar: S → AB, A → a | ε, B → b | ε. Compute FIRST(S) and check if LL(1).

---

## Section C: Bottom-Up Parsing (15 Questions)

### Q24. [GATE 2005 — 2 marks]
Which of the following is a handle of the sentential form `aAbcde` if the grammar is S → aABe, A → Abc | b, B → d?
(a) Abc  (b) b  (c) d  (d) aAbcde

### Q25. [GATE 2014 — 2 marks]
How many LR(0) items are there for the grammar: S → AA, A → aA | b?

### Q26. [Practice — 2 marks]
Build the canonical collection of LR(0) item sets for: S' → S, S → CC, C → cC | d

### Q27. [GATE 2017 — 2 marks]
Grammar: S → aAb | ε, A → aAb | ε. Is this grammar SLR(1)?

### Q28. [Practice — 2 marks]
Construct the SLR(1) parsing table for: E → E+T | T, T → id

### Q29. [GATE 2007 — 2 marks]
For grammar S → Aa | bAc | dc | bda, A → d, the number of reduce-reduce conflicts in SLR(1) parser is:
(a) 0  (b) 1  (c) 2  (d) 3

### Q30. [Practice — 2 marks]
What is the relationship between CLR(1) and LALR(1) parsing tables? Can LALR(1) have conflicts that CLR(1) doesn't?

### Q31. [GATE 2022 — 2 marks]
The maximum number of states in the LALR(1) automaton for a grammar with k items is:
(a) Same as LR(0)  (b) Same as CLR(1)  (c) Less than LR(0)  (d) Between LR(0) and CLR(1)

### Q32. [Practice — 2 marks]
Grammar: S → L=R | R, L → *R | id, R → L
Is this SLR(1)? If not, is it LALR(1)?

### Q33. [GATE 2016 — 1 mark]
Every SLR(1) grammar is:
(a) LL(1)  (b) LR(0)  (c) LALR(1)  (d) Ambiguous

### Q34. [Practice — 2 marks]
Construct CLR(1) items for: S → aAd | bBd | aBe | bAe, A → c, B → c. Is it LALR(1)?

### Q35. [GATE 2009 — 2 marks]
The number of states in the LR(0) automaton for: S' → S, S → (S)S | ε is:

### Q36. [Practice — 1 mark]
Which of the following conflicts CAN be introduced when converting CLR(1) to LALR(1)?
(a) Shift-reduce  (b) Reduce-reduce  (c) Both  (d) Neither

### Q37. [Practice — 2 marks]
Verify whether the grammar S → AB, A → a, B → b is LR(0).

### Q38. [GATE 2021 — 2 marks]
A grammar G is not LR(1). Which of the following must be true?
(a) G is ambiguous  (b) G is not LALR(1)  (c) G cannot be parsed by shift-reduce  (d) Both (a) and (b)

---

## Section D: SDT & Semantic Analysis (8 Questions)

### Q39. [GATE 2008 — 2 marks]
In the SDD: E → E₁ + T {E.val = E₁.val + T.val}, E → T {E.val = T.val}, T → num {T.val = num.val}
For input "3 + 5 + 2", what is E.val?

### Q40. [Practice — 2 marks]
Classify each attribute as synthesized or inherited:
```
S → AB    { S.val = A.val + B.val }
A → a     { A.val = 1 }
B → b     { B.len = A.val }
```

### Q41. [GATE 2013 — 2 marks]
Which of the following is TRUE for S-attributed definitions?
(a) Can be evaluated during top-down parsing  
(b) Can be evaluated during bottom-up (LR) parsing  
(c) Require inherited attributes  
(d) None of the above

### Q42. [Practice — 2 marks]
Given the SDD below, construct the annotated parse tree for "3 * 5 + 4":
```
E → E + T  { E.val = E.val + T.val }
E → T      { E.val = T.val }
T → T * F  { T.val = T.val * F.val }
T → F      { T.val = F.val }
F → num    { F.val = num.val }
```

### Q43. [GATE 2015 — 2 marks]
In L-attributed SDD, inherited attributes of a symbol can depend on:
(a) Any sibling  (b) Only right siblings  (c) Only left siblings and parent  (d) Parent only

### Q44. [Practice — 1 mark]
Every S-attributed SDD is also L-attributed: TRUE or FALSE?

### Q45. [Practice — 2 marks]
Write an SDT to convert infix expression to postfix notation.

### Q46. [GATE 2019 — 2 marks]
For the SDTS: S → aA {print "1"} B {print "2"}, A → a {print "3"}, B → b {print "4"}
What is printed for input "aab"?

---

## Section E: Intermediate Code (8 Questions)

### Q47. [GATE 2012 — 2 marks]
Generate three-address code for: `a = b * c + d / (e - f)`

### Q48. [Practice — 2 marks]
Convert to quadruples, triples, and indirect triples:
```
t1 = a + b
t2 = c * t1
t3 = t2 - d
x = t3
```

### Q49. [GATE 2018 — 2 marks]
In Static Single Assignment (SSA) form, the purpose of the φ-function is:
(a) Function call  (b) Merge values at join points  (c) Loop optimization  (d) Error handling

### Q50. [Practice — 2 marks]
Identify basic blocks and draw the control flow graph for:
```
1: i = 1
2: if i > n goto 8
3: t1 = a[i]
4: t2 = t1 * 2
5: b[i] = t2
6: i = i + 1
7: goto 2
8: return
```

### Q51. [GATE 2014 — 2 marks]
How many basic blocks are in:
```
1: x = 1
2: y = 2
3: if x > y goto 6
4: z = x + y
5: goto 7
6: z = x - y
7: print z
```

### Q52. [Practice — 2 marks]
Convert to SSA form:
```
x = 5
if (cond) { x = 10 } else { x = 20 }
y = x + 1
```

### Q53. [Practice — 1 mark]
The advantage of triples over quadruples is:
(a) Faster execution  (b) Saves space (no result field)  (c) Easier to optimize  (d) Better error handling

### Q54. [Practice — 2 marks]
How many temporary variables are needed to generate three-address code for: `a = b + c * d - e / f + g`?

---

## Section F: Code Optimization & Data Flow (8 Questions)

### Q55. [GATE 2016 — 2 marks]
Apply the following optimizations to:
```
t1 = 4 * i
t2 = a[t1]
t3 = 4 * i
t4 = b[t3]
t5 = t2 + t4
```
Identify common subexpressions and optimize.

### Q56. [Practice — 2 marks]
Perform liveness analysis on:
```
B1: a = 5
    b = a + 1
B2: c = b * 2
    a = c - 1
B3: d = a + b
```
Live at exit of B3: {d}

### Q57. [GATE 2020 — 2 marks]
Which optimization moves loop-invariant computations outside the loop?
(a) Strength reduction  (b) Loop invariant code motion  (c) Dead code elimination  (d) Copy propagation

### Q58. [Practice — 2 marks]
Construct the DAG for the basic block:
```
a = b + c
d = b + c
e = a - d
f = a + e
```
How many nodes does the DAG have?

### Q59. [GATE 2013 — 2 marks]
Perform available expression analysis on a CFG with 3 blocks. IN and OUT sets.

### Q60. [Practice — 2 marks]
Identify which optimizations can be applied:
```
x = 3
y = x + 2
z = 5 * 2
w = y + z
x = w
d = x + y  // x, y, d are used later
```

### Q61. [Practice — 1 mark]
Liveness analysis is a _______ data flow problem.
(a) Forward  (b) Backward  (c) Bidirectional  (d) None

### Q62. [GATE 2022 — 2 marks]
Given DEF and USE sets for basic blocks, compute IN and OUT for liveness analysis.

---

## Section G: Mixed & GATE-Style (8 Questions)

### Q63. [GATE 2011 — 2 marks]
Match the compiler phase with its output:
A. Lexical Analysis → 1. Parse Tree  
B. Syntax Analysis → 2. Token Stream  
C. Semantic Analysis → 3. Intermediate Code  
D. Intermediate Code Gen → 4. Annotated Tree

### Q64. [Practice — 1 mark]
YACC is a tool for:
(a) Lexical analysis  (b) Syntax analysis  (c) Code optimization  (d) Code generation

### Q65. [GATE 2014 — 1 mark]
The symbol table is used in which phases?
(a) Only lexical analysis  (b) Only syntax analysis  (c) All phases  (d) Only code generation

### Q66. [Practice — 2 marks]
Given a grammar with 5 productions and 3 non-terminals, what is the maximum number of LR(0) items?

### Q67. [GATE 2009 — 2 marks]
The grammar hierarchy: LL(1) ⊂ SLR(1) — TRUE or FALSE?

### Q68. [Practice — 2 marks]
A compiler uses peephole optimization. Give 3 examples of peephole optimizations.

### Q69. [Practice — 1 mark]
Register allocation is typically done using:
(a) DAG  (b) Graph coloring  (c) Dynamic programming  (d) Greedy algorithm

### Q70. [GATE 2021 — 2 marks]
What is the relationship between the following: Every LR(1) grammar is unambiguous. TRUE or FALSE? And its converse?

---

## Section H: Operator Precedence & Error Recovery (5 Questions)

### Q71. [GATE] In operator precedence parsing, if + has lower precedence than *, the relation between + and * is:
(a) + ⋖ * (b) + ⋗ * (c) + ≐ * (d) No relation

### Q72. [Practice — 2 marks]
Construct the operator precedence table for the expression grammar with operators +, *, and id. Assume + is left-associative, * has higher precedence, and both are left-associative.

### Q73. [Practice — 1 mark]
Which error recovery strategy discards input tokens until a synchronizing token is found?
(a) Phrase level (b) Error productions (c) Panic mode (d) Global correction

### Q74. [GATE] In LR parsing error recovery using panic mode, the parser:
(a) inserts missing tokens (b) pops stack until a state with goto on error is found (c) corrects the source program (d) reports all errors at once

### Q75. [Practice — 1 mark]
Operator precedence parsing cannot handle:
(a) ε-productions in the grammar (b) left recursion (c) right recursion (d) both (a) and (b)

---

## Section I: Runtime Environments (8 Questions)

### Q76. [GATE 2018] In a typical activation record, the field that points to the activation record of the caller is:
(a) access link (b) control link (c) return address (d) saved registers

### Q77. [GATE 2015 — 2 marks]
```
int x = 10;
void f() { printf("%d", x); }
void g() { int x = 20; f(); }
main() { g(); }
```
What is printed with (i) static scoping? (ii) dynamic scoping?

### Q78. [Practice — 2 marks]
Given the following program:
```
void swap(int a, int b) {
    int t = a; a = b; b = t;
}
int x = 3, y = 5;
swap(x, y);
```
What are x, y after swap() under: (i) call by value (ii) call by reference (iii) call by value-result?

### Q79. [GATE] The display mechanism is used for:
(a) faster I/O (b) accessing non-local variables in nested scopes (c) register allocation (d) garbage collection

### Q80. [GATE 2019] In a language with static scoping and nested functions at depth d, accessing a variable at depth k < d requires traversing:
(a) d access links (b) d - k access links (c) k access links (d) 1 access link

### Q81. [Practice — 1 mark]
Which parameter passing mechanism may give different results if the actual parameters are aliased?
(a) Call by value (b) Call by reference (c) Both (d) Neither

### Q82. [Practice — 2 marks]
For the function call chain: main() → A() → B() → C(), draw the runtime stack showing activation records. Which fields link C's record to A's record?

### Q83. [GATE] In call by name, the actual parameters are evaluated:
(a) before the call (b) each time the formal parameter is used (c) once, after the call (d) never

---

## Section J: Code Generation & Peephole Optimization (7 Questions)

### Q84. [GATE] The getReg algorithm in code generation is used for:
(a) syntax analysis (b) selecting registers for operands (c) error recovery (d) intermediate code

### Q85. [Practice — 2 marks]
Given the following code with 2 registers (R0, R1) available:
```
t1 = a + b
t2 = c + d
t3 = t1 * t2
```
Show the generated assembly code. How many spills are needed?

### Q86. [GATE] Register allocation using graph coloring fails when:
(a) the graph is too large (b) the interference graph is not k-colorable (c) there are no variables (d) the program has no loops

### Q87. [Practice — 2 marks]
Apply peephole optimization to:
```
MOV R1, a
MOV a, R1
GOTO L1
x = x + 0
L1: GOTO L2
L2: MOV R2, b
```

### Q88. [Practice — 1 mark]
Reaching definitions analysis is a _______ data flow problem.
(a) forward, any-path (b) backward, any-path (c) forward, all-path (d) backward, all-path

### Q89. [GATE] Very Busy Expressions analysis is:
(a) forward, union (b) backward, intersection (c) forward, intersection (d) backward, union

### Q90. [Practice — 2 marks]
For the following CFG with 3 blocks, compute GEN and KILL sets for reaching definitions, then compute IN and OUT for each block:
```
B1: d1: x = 5
    d2: y = 3
B2: d3: x = y + 1
    d4: z = x * 2
B3: d5: y = z - x
    (uses x, y)
```
B1 → B2, B2 → B3, B3 → B2 (loop back edge)

---

*End of Questions — Compiler Design for GATE 2027*
