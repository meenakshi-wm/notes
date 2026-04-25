# Compiler Design — Revision Notes for GATE 2027

> **GATE Weightage:** 5–8 marks | **Priority:** Very High

---

## 1. Compiler Phases
```
Source → Lexical(tokens) → Syntax(parse tree) → Semantic(annotated tree) 
     → ICG(3AC) → Optimizer → Code Gen(target code)
```
- **Symbol Table:** shared across all phases
- Preprocessor → Compiler → Assembler → Linker → Loader

---

## 2. Lexical Analysis
- Groups characters into **tokens** (type) with **lexemes** (value)
- Implemented as **DFA** from regular expressions
- **Maximal munch:** always pick longest match
- `iffy` → single identifier, NOT `if` + `fy`

---

## 3. Parsing Hierarchy

```
LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1) = LR(1)
LL(1) ⊂ LR(1)
LL(1) and LR(0) are INCOMPARABLE
Every LR(k) grammar is UNAMBIGUOUS
Ambiguous → NOT in ANY LR or LL class
```

---

## 4. FIRST & FOLLOW Rules

**FIRST(X):**
- Terminal X: FIRST = {X}
- X → ε: add ε
- X → Y₁Y₂...: add FIRST(Y₁)−{ε}; if ε∈FIRST(Y₁), continue

**FOLLOW(A):**
- Start symbol: add **$**
- A → αBβ: add FIRST(β)−{ε} to FOLLOW(B)
- A → αB or ε∈FIRST(β): add FOLLOW(A) to FOLLOW(B)

---

## 5. LL(1) Conditions
For A → α | β:
1. FIRST(α) ∩ FIRST(β) = ∅
2. If ε ∈ FIRST(α): FIRST(β) ∩ FOLLOW(A) = ∅

**Not LL(1) if:** ambiguous, left-recursive, has common prefix (needs left factoring)

### Left Recursion Elimination
A → Aα | β → **A → βA', A' → αA' | ε**

### Left Factoring
A → αβ₁ | αβ₂ → **A → αA', A' → β₁ | β₂**

---

## 6. Bottom-Up Parsing

### LR(0) Items
- Production with dot: A → α•β
- **Closure:** If A → α•Bβ, add all B → •γ
- **Goto(I, X):** Advance dot past X

### Parser Comparison
| Parser | States | Resolves Conflicts Using |
|--------|--------|-------------------------|
| LR(0) | Fewest | No lookahead |
| SLR(1) | = LR(0) | **FOLLOW** sets |
| LALR(1) | = LR(0) | **Merged lookaheads** |
| CLR(1) | Most | **Exact lookaheads** |

### Key Facts
- LALR = merge CLR states with same core → same # states as SLR
- LALR merge can create **reduce-reduce** conflicts (NOT shift-reduce)
- CLR(1) can have exponentially more states than SLR(1)
- **YACC/Bison** = LALR(1)
- **Handle** = RHS that reduces to LHS in rightmost derivation reverse

---

## 7. SDT & SDD

### Attributes
| Type | Computed From | Compatible With |
|------|--------------|-----------------|
| **Synthesized** | Children | Bottom-up (LR) |
| **Inherited** | Parent + left siblings | Top-down (LL) |

- **S-attributed:** Only synthesized → evaluate bottom-up → post-order
- **L-attributed:** Inherited from left/parent only → left-to-right depth-first
- **Every S-attributed SDD is also L-attributed**

---

## 8. Intermediate Code

### Three Address Code Forms
- **Quadruple:** (op, arg1, arg2, result) — explicit result
- **Triple:** (op, arg1, arg2) — implicit result (by index)
- **Indirect Triple:** Pointer array to triples (reorderable)

### SSA
- Each variable assigned **exactly once**
- **φ-function** at join points: `x₃ = φ(x₁, x₂)`

### Basic Blocks & CFG
**Leaders:**
1. First statement
2. Target of any goto/branch
3. Statement after a goto/branch

---

## 9. Code Optimization

| Optimization | What It Does |
|-------------|-------------|
| Constant Folding | Evaluate constants at compile time |
| Constant Propagation | Replace variable with its known constant value |
| Copy Propagation | After x=y, replace x with y |
| Dead Code Elimination | Remove code whose result is never used |
| CSE | Reuse previously computed expressions |
| Loop Invariant Code Motion | Move unchanging code outside loop |
| Strength Reduction | Replace expensive ops (×) with cheap (<<) |

### Data Flow Analysis
| Analysis | Direction | Confluence | Application |
|----------|-----------|-----------|-------------|
| **Liveness** | **Backward** | Union (∪) | Dead code, register alloc |
| **Available Expressions** | **Forward** | Intersection (∩) | CSE |
| **Reaching Definitions** | Forward | Union (∪) | Constant propagation |

**Liveness equations:**
- IN[B] = USE[B] ∪ (OUT[B] − DEF[B])
- OUT[B] = ∪ IN[S] for successors S

**Available expression equations:**
- OUT[B] = GEN[B] ∪ (IN[B] − KILL[B])
- IN[B] = ∩ OUT[P] for predecessors P

### Peephole Optimization
- Examines small window of consecutive instructions
- Redundant load/store elimination, jump-to-jump, algebraic identity (x+0, x*1), strength reduction, unreachable code

### Reaching Definitions Equations
- OUT[B] = GEN[B] ∪ (IN[B] − KILL[B])
- IN[B] = ∪ OUT[P] for predecessors P (forward, union)

---

## 9a. Code Generation

### Register Allocation
- **Graph coloring:** Build interference graph → color with k colors (k = registers)
- Not k-colorable → **spill** to memory
- **getReg:** Select register — prefer (1) same register if possible, (2) free register, (3) spill least-needed

### Instruction Selection
- Template matching: map 3AC → machine instructions
- Tree tiling: DP on expression trees

---

## 9b. Runtime Environments ⭐

### Activation Record (stack frame)
Top → Bottom: Actual params | Return value | Control link (dynamic) | Access link (static) | Saved registers | Locals | Temporaries

### Parameter Passing
| Mechanism | Behavior |
|-----------|---------|
| Call by Value | Copy; original unchanged |
| Call by Reference | Pass address; changes affect original |
| Call by Value-Result | Copy in + copy back on return |
| Call by Name | Thunk evaluated each time used |

### Static vs Dynamic Scoping
- **Static:** Determined by program text; access links
- **Dynamic:** Determined by call stack at runtime
- Example: `f()` called from `g()` — static sees enclosing scope, dynamic sees caller's scope

### Display
- Array D[0..d] — D[i] = pointer to active record at depth i
- O(1) access to any enclosing scope (vs O(d-k) with access links)

---

## 9c. Operator Precedence Parsing

- For operator grammars only (no ε-productions, no adjacent non-terminals)
- Relations: ⋖ (yields to), ⋗ (takes precedence), ≐ (equal)
- Higher precedence → ⋗; same precedence + left-assoc → ⋗; right-assoc → ⋖
- $ ⋖ everything; everything ⋗ $

### Error Recovery
| Strategy | Description |
|----------|-------------|
| Panic Mode | Skip to synchronizing token |
| Phrase Level | Local correction (insert/delete) |
| Error Productions | Grammar augmented for common errors |
| Global Correction | Minimum edit distance (impractical) |

---

## 10. Critical Traps

| # | Trap | Reality |
|---|------|---------|
| 1 | LL(1) ⊂ SLR(1) | **FALSE** — they're incomparable |
| 2 | LL(1) ⊂ LR(1) | **TRUE** |
| 3 | Not LR(1) = ambiguous | **FALSE** — unambiguous can be non-LR(1) |
| 4 | LALR merge creates SR conflicts | **FALSE** — only RR conflicts |
| 5 | S-attributed needs inherited attrs | **FALSE** — only synthesized |
| 6 | Liveness is forward analysis | **FALSE** — backward |
| 7 | CSE uses backward analysis | **FALSE** — available expressions is forward |
| 8 | FOLLOW includes ε | **NEVER** — FOLLOW never contains ε |
| 9 | LR(0) states ≠ SLR(1) states | **FALSE** — same state count |
| 10 | Triple saves space over quadruple | **TRUE** — no explicit result field |

---

## 11. Number Crunching

- Items for production of length n = **n + 1** dot positions
- Total LR(0) items = Σ(length_i + 1) for all productions
- LL(1) table size = |non-terminals| × |terminals + $|
- LR parsing table = |states| × |terminals + non-terminals|

---

*Quick revision complete — Compiler Design for GATE 2027* ⚡
