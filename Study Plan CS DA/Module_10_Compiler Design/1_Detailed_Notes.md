# ⚙️ Compiler Design — Detailed Notes for GATE 2027 🎯

> 🌟 **"A compiler is the magical translator that turns your human-readable code into the 0s and 1s that computers understand. Without compilers, programming would be like giving instructions in binary!"**

> 💡 **Beginner Analogy — The Language Translator 🌍:**
> Imagine you write a letter in English ✍️, but the recipient only speaks Japanese 🇯🇵:
> 1. First, understand each English word (**Lexical Analysis** — breaking into tokens) 📝
> 2. Check if the sentences are grammatically correct (**Syntax Analysis** — parsing) 📖
> 3. Understand the meaning (**Semantic Analysis** — does it make sense?) 🤔
> 4. Convert to Japanese (**Code Generation** — translate to target language) 🇯🇵
> 5. Polish the translation for fluency (**Optimization** — make it better!) ✨
>
> That's EXACTLY what a compiler does with your C/Java/Go code → machine code!

---

## 🏭 Where is Compiler Design Used in the Real World?

| 🌍 Company/Project | 🔧 What | 📌 Connection |
|---|---|---|
| **GCC** 🐧 | GNU Compiler (compiles Linux kernel!) | All phases of compilation |
| **LLVM/Clang** 🔧 | Powers Swift, Rust, and Chrome/Firefox JIT | Modern compiler architecture |
| **V8 Engine** 🚀 | JavaScript engine in Chrome & Node.js | JIT compilation (Just-In-Time) |
| **JVM (Java)** ☕ | Java Virtual Machine, used by billions | Bytecode compilation + JIT |
| **TypeScript Compiler** 📘 | tsc compiles TS → JS | Lexical + Syntax + Semantic analysis |
| **Babel** 🌐 | Modern JS → compatible JS | Code transformation |
| **TensorFlow XLA** 🤖 | ML model compiler for GPUs/TPUs | Optimization passes |
| **SQL Query Optimizer** 🗄️ | Oracle, PostgreSQL, MySQL | Parsing + semantic analysis + optimization |
| **Prettier/ESLint** ✨ | Code formatters and linters | Lexical + Syntax analysis |
| **Shader Compilers** 🎮 | GPU shader compilation (DirectX, Vulkan) | Full compilation pipeline |

> **GATE Weightage:** 5–8 marks (2–3 questions) | **Priority:** Very High 🔥🔥
> **Time to Spend:** 4–5 weeks | **Difficulty:** High
> **Preparation Order:** After TOC and before COA 📢

---

## Table of Contents
1. [Introduction & Compiler Phases](#1-introduction--compiler-phases)
2. [Lexical Analysis](#2-lexical-analysis)
3. [Syntax Analysis — Top-Down Parsing](#3-syntax-analysis--top-down-parsing)
4. [Syntax Analysis — Bottom-Up Parsing](#4-syntax-analysis--bottom-up-parsing)
5. [Syntax Directed Translation & SDD](#5-syntax-directed-translation--sdd)
6. [Intermediate Code Generation](#6-intermediate-code-generation)
7. [Code Optimization](#7-code-optimization)

---

## 1. Introduction & Compiler Phases 🛠️

### What is a Compiler? 🤔

> 🏭 **Deep Insight — The Scale of Modern Compilers:**
> The GCC compiler has over **15 million lines of code** and can compile C, C++, Fortran, and more. LLVM processes over **200 optimization passes** on your code. When you type `gcc hello.c`, it does more work in 1 second than a human could do in months! 🤯
>
> Fun fact: The first compiler was written by Grace Hopper 👩‍💻 in 1952. When she told people computers could understand English-like commands, nobody believed her! She literally invented the concept of "compiling" code. 🏆

A compiler translates **source code** (high-level language) into **target code** (machine language/assembly) in one complete pass through the program.

**Real-World Analogy:** A compiler is like a **translator** converting an entire book from English to Hindi — they read the whole thing, understand the structure, and produce a complete translated version.

### Compiler vs Interpreter
| Feature | Compiler | Interpreter |
|---------|----------|-------------|
| Translation | Entire program at once | Line by line |
| Speed (execution) | Fast | Slow |
| Error detection | After compilation | Immediate per line |
| Output | Machine code / object file | Direct execution |
| Example | C, C++ | Python, JavaScript |

### Phases of Compilation

```
Source Code
    ↓
┌──────────────────┐
│ 1. Lexical Analyzer │ → Tokens
│ 2. Syntax Analyzer  │ → Parse Tree
│ 3. Semantic Analyzer │ → Annotated Tree
│ 4. Intermediate Code │ → Three Address Code
│ 5. Code Optimizer    │ → Optimized Code
│ 6. Code Generator    │ → Target Machine Code
└──────────────────┘
    ↓
Target Code
```

**Symbol Table** — shared across all phases, stores identifiers + attributes  
**Error Handler** — reports errors detected in any phase

### Language Processing System
```
Source → Preprocessor → Compiler → Assembler → Linker → Loader → Execution
```

- **Preprocessor:** Handles #include, #define, macros
- **Assembler:** Assembly → Machine code (.o / .obj)
- **Linker:** Combines multiple .o files + libraries → executable
- **Loader:** Loads executable into memory for execution

---

## 2. Lexical Analysis

### Role
The lexical analyzer (scanner) reads the source code character-by-character and groups them into meaningful sequences called **tokens**.

### Key Terminology
| Term | Meaning | Example |
|------|---------|---------|
| **Token** | Category/type of lexeme | `ID`, `NUM`, `IF`, `ASSIGN` |
| **Lexeme** | Actual character sequence | `count`, `42`, `if`, `=` |
| **Pattern** | Rule describing token | `letter(letter|digit)*` for ID |

**Example:** `int count = 42;`
| Lexeme | Token |
|--------|-------|
| int | KEYWORD |
| count | IDENTIFIER |
| = | ASSIGN |
| 42 | INTEGER_LITERAL |
| ; | SEMICOLON |

### Lexical Analyzer as DFA
The scanner is typically implemented as a DFA (Deterministic Finite Automaton):
1. Define token patterns as **regular expressions**
2. Convert regex → NFA → DFA
3. DFA processes input character by character

**Maximal Munch Rule:** When multiple tokens could match, the lexer always picks the **longest possible match**.

Example: `ifcount` → token ID("ifcount"), NOT keyword IF + ID("count")

### Regular Expressions for Tokens
| Token | Pattern |
|-------|---------|
| Identifier | `letter(letter|digit)*` |
| Integer | `digit+` |
| Float | `digit+.digit+` |
| Relational Op | `< | > | <= | >= | == | !=` |

---

## 3. Syntax Analysis — Top-Down Parsing

### Context-Free Grammars (CFG)
A CFG G = (V, T, P, S) where:
- V = set of non-terminals (variables)
- T = set of terminals (tokens)
- P = set of production rules
- S = start symbol

**Example:**
```
E → E + T | T
T → T * F | F
F → ( E ) | id
```

### Derivation
- **Leftmost derivation:** Always expand the leftmost non-terminal
- **Rightmost derivation:** Always expand the rightmost non-terminal

### Ambiguity
A grammar is **ambiguous** if a string has more than one:
- Parse tree, OR
- Leftmost derivation, OR
- Rightmost derivation

**Classic ambiguous grammar:** `E → E + E | E * E | id`

For `id + id * id`: two parse trees exist (different precedence).

**Fixing ambiguity:**
1. **Operator precedence:** Create hierarchy (E → T → F)
2. **Associativity:** Left-recursive for left-associative operators

### Left Recursion Elimination
**Direct left recursion:** A → Aα | β  
**Transformed to:** A → βA', A' → αA' | ε

**Example:**
```
E → E + T | T
```
becomes:
```
E → TE'
E' → +TE' | ε
```

### Left Factoring
When two productions have a common prefix:
```
A → αβ₁ | αβ₂
```
becomes:
```
A → αA'
A' → β₁ | β₂
```

**Example:**
```
S → iEtS | iEtSeS | a
```
becomes:
```
S → iEtSS' | a
S' → eS | ε
```

### ⭐ FIRST and FOLLOW Sets (GATE Favorite!)

**FIRST(α):** Set of terminals that begin strings derived from α.

**Rules for FIRST:**
1. If X is terminal: FIRST(X) = {X}
2. If X → ε: add ε to FIRST(X)
3. If X → Y₁Y₂...Yₖ: add FIRST(Y₁) − {ε}; if ε ∈ FIRST(Y₁), add FIRST(Y₂) − {ε}; continue...

**FOLLOW(A):** Set of terminals that can appear immediately to the right of A.

**Rules for FOLLOW:**
1. Add $ to FOLLOW(S) where S is start symbol
2. If A → αBβ: add FIRST(β) − {ε} to FOLLOW(B)
3. If A → αB or A → αBβ where ε ∈ FIRST(β): add FOLLOW(A) to FOLLOW(B)

**Example:**
```
E → TE'
E' → +TE' | ε
T → FT'
T' → *FT' | ε
F → (E) | id
```

| Non-terminal | FIRST | FOLLOW |
|-------------|-------|--------|
| E | {(, id} | {$, )} |
| E' | {+, ε} | {$, )} |
| T | {(, id} | {+, $, )} |
| T' | {*, ε} | {+, $, )} |
| F | {(, id} | {*, +, $, )} |

### ⭐ LL(1) Parser

**LL(1):** Left-to-right, Leftmost derivation, 1 lookahead symbol.

**Condition for LL(1):** For every pair of productions A → α | β:
1. FIRST(α) ∩ FIRST(β) = ∅
2. If ε ∈ FIRST(α), then FIRST(β) ∩ FOLLOW(A) = ∅

**LL(1) Parsing Table Construction:**  
For each production A → α:
1. For each terminal a ∈ FIRST(α): add A → α to M[A, a]
2. If ε ∈ FIRST(α): for each terminal b ∈ FOLLOW(A), add A → α to M[A, b]

**If any cell has multiple entries → NOT LL(1)**

**Key facts:**
- Ambiguous grammar → NOT LL(1)
- Left-recursive grammar → NOT LL(1)
- LL(1) grammar is never ambiguous

### Recursive Descent Parser
- One function per non-terminal
- Uses lookahead to choose production
- Can handle LL(1) grammars directly
- May need **backtracking** for non-LL(1) grammars

---

## 4. Syntax Analysis — Bottom-Up Parsing

### Concept
Build parse tree from **leaves to root** (reduce tokens to start symbol).

**Shift-Reduce Parsing:**
- **Shift:** Push next input symbol onto stack
- **Reduce:** Replace handle (top of stack) with non-terminal using a production
- **Accept:** Stack has start symbol, input is empty
- **Error:** No valid action

### Handle
A **handle** is a substring of sentential form that matches RHS of a production AND whose reduction leads to the rightmost derivation in reverse.

### Viable Prefix
A prefix of a right sentential form that can appear on the stack without triggering an error. The set of viable prefixes is a **regular language** (recognized by the LR automaton).

### ⭐ LR Parsing Family

| Parser | Power | States | Lookahead |
|--------|-------|--------|-----------|
| LR(0) | Weakest | Fewest | None |
| SLR(1) | Medium | Same as LR(0) | FOLLOW set |
| CLR(1) / LR(1) | Strongest | Most | Exact lookahead |
| LALR(1) | Near CLR(1) | Same as SLR(1) | Merged lookahead |

**Power hierarchy:** LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1)

### LR(0) Items and Automaton

An **LR(0) item** is a production with a dot (•) showing parsing progress:
- A → •XYZ (haven't seen anything yet)
- A → X•YZ (seen X)
- A → XYZ• (complete item → can reduce)

**Closure(I):**
If A → α•Bβ is in I, add all B → •γ to I.

**Goto(I, X):**
Move dot past symbol X for all items in I where dot is before X.

### LR(0) Parsing
- Build canonical collection of LR(0) item sets
- **Conflict-free condition:** No state has both:
  - shift item AND complete item (shift-reduce conflict)
  - two complete items (reduce-reduce conflict)

### SLR(1) Parsing
Same states as LR(0), but uses **FOLLOW** sets to resolve conflicts:
- Reduce A → α only if lookahead ∈ FOLLOW(A)
- **Not LL(1) does NOT mean not SLR(1)** (they're different parser types)

### CLR(1) / LR(1) Parsing
- Each item includes a **lookahead terminal:** [A → α•β, a]
- More states than SLR(1) but more powerful
- Resolves conflicts that SLR(1) cannot

**LR(1) item:** [A → α•β, a] means "reduce using A → αβ only when lookahead is a"

### LALR(1) Parsing
- **Merge LR(1) states** that have same core (same items, different lookaheads)
- Same number of states as SLR(1)
- More powerful than SLR(1) (uses merged lookaheads instead of FOLLOW)
- Can introduce **reduce-reduce conflicts** that CLR(1) doesn't have
- **Cannot introduce shift-reduce conflicts** (compared to CLR(1))
- Used by tools like **YACC/Bison**

### Grammar Hierarchy Relationships

```
LL(1) ⊂ LR(1)
LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊂ LR(1)
Every LL(1) grammar is also LR(1)
Every LL(1) grammar is also LALR(1)
LL(1) and LR(0) are incomparable
```

**Key GATE facts:**
- Every LR(k) grammar is unambiguous
- Not every unambiguous grammar is LR(1)
- LL(1) ∩ LR(0) ≠ ∅ (some grammars are both)
- If grammar is ambiguous → NOT in any LR class

### Number of States
For a grammar with N items:
- LR(0) states = SLR(1) states = LALR(1) states ≤ CLR(1) states
- CLR(1) can have exponentially more states

### Operator Precedence Parsing ⭐

A special case of bottom-up parsing for **operator grammars** (no two adjacent non-terminals, no ε-productions).

**Precedence Relations between terminals a and b:**

| Relation | Meaning |
|----------|---------|
| a ⋖ b | a yields precedence to b (a has lower precedence) |
| a ≐ b | a has equal precedence to b |
| a ⋗ b | a takes precedence over b (a has higher precedence) |

**Algorithm:**
1. Scan input left to right
2. Find the leftmost ⋗ (handle's right end)
3. Scan back left to find matching ⋖ (handle's left end)
4. Reduce the handle
5. Repeat until only start symbol remains

**Operator Precedence Table Construction:**
- If operator θ₁ has higher precedence than θ₂: θ₁ ⋗ θ₂
- If equal precedence:
  - Left-associative: θ₁ ⋗ θ₂
  - Right-associative: θ₁ ⋖ θ₂
- For $ (end marker): $ ⋖ everything, everything ⋗ $
- For id: everything ⋖ id, id ⋗ everything

**Example Precedence Table (for +, *, id):**

|   | + | * | id | $ |
|---|---|---|----|-|
| + | ⋗ | ⋖ | ⋖ | ⋗ |
| * | ⋗ | ⋗ | ⋖ | ⋗ |
| id| ⋗ | ⋗ |  | ⋗ |
| $ | ⋖ | ⋖ | ⋖ |  |

**Key Facts:**
- Cannot handle unary operators directly
- If no relation exists between two terminals → **error**
- Simpler than full LR parsing but limited to operator grammars
- Used in simple expression parsers

### Error Recovery in Parsing ⭐

| Strategy | Description | Recovery Quality |
|----------|-------------|------------------|
| **Panic Mode** | Discard tokens until a synchronizing token (e.g., `;`, `}`) is found | Fast, may skip much input |
| **Phrase Level** | Perform local correction (insert/delete/replace tokens) | Better, but predefined |
| **Error Productions** | Augment grammar with productions matching common errors | Anticipates specific errors |
| **Global Correction** | Find minimum-edit-distance correction to make input valid | Best quality, impractical (exponential) |

**In LR Parsing:** On error (empty table entry):
1. Pop states until a state with a GOTO on error non-terminal is found
2. Shift error non-terminal
3. Discard input until a token in FOLLOW of error non-terminal

**In LL Parsing (Panic Mode):** Skip input until a token in FOLLOW(A) is found, then pop A.

---

## 5. Syntax Directed Translation & SDD

### Syntax Directed Definitions (SDD)
An SDD associates **semantic rules** (attributes) with grammar productions.

**Types of Attributes:**
| Type | Computed From | Direction |
|------|--------------|-----------|
| **Synthesized** | Children's attributes | Bottom-up (leaf → root) |
| **Inherited** | Parent/siblings' attributes | Top-down (root → leaf) |

### S-Attributed SDD
- Uses ONLY synthesized attributes
- Can be evaluated **bottom-up** on any parse tree  
- Compatible with **LR parsing**

### L-Attributed SDD
- Inherited attributes depend only on:
  - Inherited attributes of parent
  - Attributes of left siblings
- Can be evaluated **left-to-right, depth-first**
- Compatible with **LL parsing**
- **Every S-attributed SDD is also L-attributed**

### Syntax Directed Translation Schemes (SDT)
SDTs embed **semantic actions** (code fragments) within productions.

**Example:**
```
E → E₁ + T  { E.val = E₁.val + T.val }
E → T       { E.val = T.val }
T → id      { T.val = id.val }
```

For input: `3 + 5`
```
E.val = E₁.val + T.val = 3 + 5 = 8
```

### Annotated Parse Tree
A parse tree decorated with attribute values at each node.

**Evaluation Order:**
- S-attributed: post-order traversal
- L-attributed: pre-order/in-order traversal

**Dependency Graph:** If attribute b depends on attribute a, draw edge a → b. Evaluation order = topological sort of dependency graph.

---

## 6. Intermediate Code Generation

### Why Intermediate Code?
- Machine-independent optimization
- Easier to retarget to different architectures
- Separates front-end from back-end

### Three-Address Code (3AC)
Each instruction has **at most 3 addresses** (operands).

**Forms:**
| Form | Example |
|------|---------|
| Assignment | `x = y op z` |
| Copy | `x = y` |
| Conditional jump | `if x relop y goto L` |
| Unconditional jump | `goto L` |
| Indexed | `x = y[i]` or `x[i] = y` |
| Pointer | `x = *y` or `*x = y` |
| Call | `param x; call p, n` |

**Example:** `a = b * c + d`
```
t1 = b * c
t2 = t1 + d
a = t2
```

### Representations of 3AC

**Quadruple:** (operator, arg1, arg2, result)
```
(*, b, c, t1)
(+, t1, d, t2)
(=, t2, _, a)
```

**Triple:** (operator, arg1, arg2) — result is implicit (index of triple)
```
(0) (*, b, c)
(1) (+, (0), d)
(2) (=, (1), a)     -- actually: (=, a, (1))
```

**Indirect Triple:** Array of pointers to triples (allows reordering without changing triples).

### Static Single Assignment (SSA)
Each variable is assigned **exactly once**. Use subscripts for different definitions.

**Example:**
```
Original:           SSA form:
x = 5               x₁ = 5
x = x + 1           x₂ = x₁ + 1
y = x * 2           y₁ = x₂ * 2
```

At join points (if-else), use **φ-function:** `x₃ = φ(x₁, x₂)`

### Control Flow Graph (CFG)
- Nodes = **basic blocks** (maximal sequence of straight-line code)
- Edges = control flow between blocks

**Leader identification:**
1. First statement is a leader
2. Target of a conditional/unconditional jump is a leader
3. Statement immediately after a jump is a leader

---

## 7. Code Optimization

### Types of Optimization
| Level | Where |
|-------|-------|
| Local | Within a basic block |
| Global | Across basic blocks (within a function) |
| Inter-procedural | Across functions |

### Common Optimizations

| Optimization | Description | Example |
|-------------|-------------|---------|
| **Constant Folding** | Evaluate constant expressions at compile time | `x = 3 + 5` → `x = 8` |
| **Constant Propagation** | Replace variable with known constant | After `x = 5`: `y = x + 1` → `y = 6` |
| **Copy Propagation** | After `x = y`, replace x with y | |
| **Dead Code Elimination** | Remove code with unused results | |
| **Common Subexpression Elimination** | Reuse computed values | `a*b` computed twice → compute once |
| **Loop Invariant Code Motion** | Move loop-invariant code outside loop | |
| **Strength Reduction** | Replace expensive ops with cheaper ones | `x * 2` → `x << 1` |
| **Induction Variable Elimination** | Eliminate redundant loop variables | |

### Liveness Analysis (Data Flow Analysis)

**Live variable:** A variable x is live at point p if there exists a path from p to a use of x that doesn't pass through a redefinition of x.

**Equations (backward flow):**
- **IN[B]** = USE[B] ∪ (OUT[B] − DEF[B])
- **OUT[B]** = ∪ IN[S] for all successors S of B

Where:
- USE[B] = variables used before definition in B
- DEF[B] = variables defined before use in B

**Applications:**
1. Dead code elimination (if variable not live after assignment → remove)
2. Register allocation (live variables need registers)

### Available Expression Analysis

**Available expression:** Expression e is available at point p if on ALL paths to p, e has been computed and no operand of e has been redefined.

**Equations (forward flow):**
- **OUT[B]** = GEN[B] ∪ (IN[B] − KILL[B])
- **IN[B]** = ∩ OUT[P] for all predecessors P of B

**Application:** Common subexpression elimination

### DAG (Directed Acyclic Graph)
- Represents expressions in a basic block
- **Leaves** = variables/constants
- **Internal nodes** = operators
- **Shared nodes** = common subexpressions

**Benefits:**
- Automatically identifies common subexpressions
- Can determine which variables hold the same value
- Helps with dead code detection

### Peephole Optimization ⭐

A local optimization technique that examines a small window ("peephole") of instructions.

| Optimization | Before | After |
|-------------|--------|-------|
| Redundant load/store | `STORE R1, a; LOAD R1, a` | `STORE R1, a` |
| Unreachable code | `goto L2; x = 5; L2:` | `goto L2; L2:` |
| Flow of control | `goto L1; L1: goto L2` | `goto L2` |
| Algebraic simplification | `x = x + 0` or `x = x * 1` | remove |
| Strength reduction | `x = x * 2` | `x = x << 1` |
| Use of machine idioms | `x = x + 1` | `INC x` |

### Reaching Definitions Analysis ⭐

**Reaching definition:** A definition d: `x = ...` reaches point p if there is a path from d to p along which x is not redefined.

**Equations (forward flow):**
- **OUT[B]** = GEN[B] ∪ (IN[B] − KILL[B])
- **IN[B]** = ∪ OUT[P] for all predecessors P of B

Where:
- GEN[B] = definitions generated (last definition of each variable in B)
- KILL[B] = all other definitions of variables defined in B

**Initialization:** IN[entry] = ∅, OUT[B] = GEN[B] for all B initially.

**Applications:** Constant propagation, use-def chains, undefined variable detection.

### Data Flow Analysis Summary Table

| Analysis | Direction | Meet (⊓) | Transfer: OUT[B] | Initialization |
|----------|-----------|---------|-------------------|----------------|
| Reaching Definitions | Forward | ∪ | GEN ∪ (IN−KILL) | OUT = GEN |
| Available Expressions | Forward | ∩ | GEN ∪ (IN−KILL) | OUT = U (all) |
| Live Variables | Backward | ∪ | USE ∪ (OUT−DEF) | IN = ∅ |
| Very Busy Expressions | Backward | ∩ | USE ∪ (IN−KILL) | IN = U |

---

## 8. Code Generation ⭐

### Register Allocation

**Problem:** Map unlimited temporaries/variables to a limited set of machine registers.

**Graph Coloring Approach:**
1. Build **interference graph**: nodes = variables, edges between variables that are simultaneously live
2. Color the graph with k colors (k = number of registers)
3. If k-colorable → assign registers; otherwise **spill** some variables to memory

**Heuristic (Chaitin's):**
1. Simplify: remove nodes with degree < k (push to stack)
2. If stuck: select a node to spill (remove + push)
3. Select: pop stack and assign colors

**Other strategies:**
- Linear scan allocation: sort by live range, assign registers greedily (used in JIT compilers)
- Usage count: allocate registers to most frequently used variables in inner loops

### Instruction Selection

**Simple approach:** Template matching — map each 3AC instruction to a machine instruction template.

| 3AC | Instruction |
|-----|-------------|
| `x = y + z` | `MOV R1, y; ADD R1, z; MOV x, R1` |
| `x = y` | `MOV x, y` |
| `if x < y goto L` | `CMP x, y; JLT L` |

**Tree-based instruction selection:** Represent expressions as trees, tile with machine instruction patterns to minimize cost (dynamic programming / maximal munch).

**"getReg" algorithm:** For 3AC `x = y op z`:
1. If y is in register R and not needed later → use R for result
2. If a free register exists → use it
3. If register R holds a value not used later / has copy in memory → use R
4. Otherwise spill: pick R, store its value to memory, use R

---

## 9. Runtime Environments ⭐⭐

### Activation Records (Stack Frames)

Each function call creates an **activation record** on the runtime stack.

**Typical layout (top to bottom):**

| Field | Content |
|-------|--------|
| Actual parameters | Arguments passed by caller |
| Return value | Space for function return value |
| Control link | Pointer to caller's activation record (dynamic link) |
| Access link | Pointer to enclosing scope's activation record (static link) |
| Saved registers | Caller/callee-saved registers |
| Local variables | Function's local data |
| Temporaries | Compiler-generated temporaries |

### Parameter Passing Mechanisms

| Mechanism | How it works | Example |
|-----------|-------------|--------|
| **Call by Value** | Copy of actual parameter | C, Java (primitives) |
| **Call by Reference** | Pass address of actual parameter | C++ (&), Fortran |
| **Call by Value-Result** (Copy-Restore) | Copy in, execute, copy back | Ada (in out) |
| **Call by Name** | Substitute expression (like macro) / thunk | Algol 60 |

**GATE trap:** In call-by-reference, if two formal parameters are aliased to the same actual parameter, changes through one affect the other.

### Static vs Dynamic Scoping

| | Static (Lexical) Scoping | Dynamic Scoping |
|--|-------------------------|----------------|
| Binding | Determined by program text | Determined by calling sequence at runtime |
| Used by | C, Java, Python, most modern languages | LISP (old), Bash, some Perl |
| Implementation | Access links (static links) in activation record | Search the call stack at runtime |
| Advantage | Predictable, easier to reason about | More flexible |

**Example:**
```
int x = 10;
void f() { print(x); }   // Which x?
void g() { int x = 20; f(); }
g();
```
- **Static scoping:** prints 10 (f sees the global x)
- **Dynamic scoping:** prints 20 (f sees g's x on the call stack)

### Display (for deeply nested scopes)
- Array D[0..max_depth] where D[i] points to the most recent activation at nesting depth i
- Faster than traversing access links: O(1) access to any enclosing scope
- Update D on function entry/exit

### Heap Management
- **Stack allocation:** LIFO — for local variables, activation records
- **Heap allocation:** dynamic (malloc/new) — for data that outlives its creating function
- **Garbage collection:** automatic reclamation (Mark-Sweep, Reference Counting, Copying, Generational)
- **Fragmentation:** External (free blocks scattered) vs Internal (allocated block > needed)

---

## Worked Examples

### Example 1: FIRST and FOLLOW (Easy)
**Grammar:**
```
S → AB
A → aA | ε
B → bB | c
```

| | FIRST | FOLLOW |
|--|-------|--------|
| S | {a, b, c} | {$} |
| A | {a, ε} | {b, c} |
| B | {b, c} | {$} |

FIRST(S) = FIRST(A)−{ε} ∪ FIRST(B) = {a, b, c}

---

### Example 2: LL(1) Table (Medium)
Build LL(1) table for E → TE', E' → +TE' | ε, T → FT', T' → *FT' | ε, F → (E) | id

| | id | + | * | ( | ) | $ |
|--|---|---|---|---|---|---|
| E | E→TE' | | | E→TE' | | |
| E'| | E'→+TE'| | | E'→ε | E'→ε |
| T | T→FT' | | | T→FT' | | |
| T'| | T'→ε | T'→*FT'| | T'→ε | T'→ε |
| F | F→id | | | F→(E) | | |

No multiply-defined entries → **LL(1) grammar** ✓

---

### Example 3: LR(0) Item Sets (Medium-Hard)
**Grammar:** S → AA, A → aA | b

**I₀:** Closure({S' → •S})
```
S' → •S
S → •AA
A → •aA
A → •b
```

**Goto(I₀, S) = I₁:** S' → S•  (accept state)

**Goto(I₀, A) = I₂:**
```  
S → A•A
A → •aA
A → •b
```

**Goto(I₀, a) = I₃:**
```
A → a•A
A → •aA
A → •b
```

**Goto(I₀, b) = I₄:** A → b•  (reduce item)

**Goto(I₂, A) = I₅:** S → AA•  (reduce item)

**Goto(I₂, a) = I₃** (same as above)
**Goto(I₂, b) = I₄** (same as above)

**Goto(I₃, A) = I₆:** A → aA•  (reduce item)
**Goto(I₃, a) = I₃** (self-loop)
**Goto(I₃, b) = I₄**

States: I₀ through I₆.

---

### Example 4: Three Address Code (GATE Level)
**Convert:** `a = b * (c + d) - e / (f + g)`

```
t1 = c + d
t2 = b * t1
t3 = f + g
t4 = e / t3
t5 = t2 - t4
a = t5
```

**Quadruples:**
| # | Op | Arg1 | Arg2 | Result |
|---|-----|------|------|--------|
| 0 | + | c | d | t1 |
| 1 | * | b | t1 | t2 |
| 2 | + | f | g | t3 |
| 3 | / | e | t3 | t4 |
| 4 | − | t2 | t4 | t5 |
| 5 | = | t5 | | a |

---

### Example 5: Liveness Analysis (GATE Level)
**Basic Block:**
```
1: a = b + c
2: d = a - b
3: e = a + d
4: b = e - a
```
Variables live at exit: {b, e}

**Working backward:**
- After stmt 4: live = {b, e}
- Before stmt 4: use(4) = {e, a}, def(4) = {b}. live = {e, a} ∪ ({b,e} − {b}) = {e, a}
- Before stmt 3: use(3) = {a, d}, def(3) = {e}. live = {a, d} ∪ ({e,a} − {e}) = {a, d}
- Before stmt 2: use(2) = {a, b}, def(2) = {d}. live = {a, b} ∪ ({a,d} − {d}) = {a, b}
- Before stmt 1: use(1) = {b, c}, def(1) = {a}. live = {b, c} ∪ ({a,b} − {a}) = {b, c}

Variables live at entry: **{b, c}**

---

## Common Mistakes

| # | Mistake | Correct |
|---|---------|---------|
| 1 | FOLLOW of start symbol missing $ | Always include $ in FOLLOW(S) |
| 2 | FIRST includes ε even when it shouldn't | ε only if entire string can derive ε |
| 3 | Confusing SLR with CLR power | SLR ⊂ LALR ⊂ CLR |
| 4 | LL(1) grammar assumed to be LR(0) | They're incomparable |
| 5 | Left recursive grammar used with LL parser | Must eliminate left recursion first |
| 6 | SDT: confusing synthesized vs inherited | Synthesized = from children, Inherited = from parent/left siblings |
| 7 | Quadruple vs Triple confusion | Triple: result is implicit by index |
| 8 | LALR merge creates shift-reduce conflicts | NO — only reduce-reduce possible |
| 9 | Liveness direction | Backward (from exit to entry) |
| 10 | Available expressions direction | Forward (from entry to exit) |

---

## 🔬 Deep Dive: Lexical Analysis — NFA, DFA, and Minimization

### Thompson's Construction (Regex → NFA)

**Rules:**

| Regex | NFA |
|---|---|
| Single char `a` | `→(q0)─a→((q1))` |
| Concatenation `RS` | Connect accept of R to start of S |
| Union `R\|S` | New start → ε → start(R) and ε → start(S); accept(R) and accept(S) → ε → new accept |
| Kleene Star `R*` | New start → ε → start(R); accept(R) → ε → start(R); new start → ε → new accept; accept(R) → ε → new accept |

**Worked Example:** Convert `(a|b)*abb` to NFA

Step 1: `a` → NFA₁: q0 →a→ q1
Step 2: `b` → NFA₂: q2 →b→ q3
Step 3: `a|b` → NFA₃: 
```
        ε→ q0 →a→ q1 →ε
q4 →ε                      → q5
        ε→ q2 →b→ q3 →ε
```

Step 4: `(a|b)*` → NFA₄:
```
              ε→ q4 → ... → q5 →ε
q6 →ε                                → q7
              ←←←←←←ε←←←←←←←←←
      ε→→→→→→→→→→→→→→→→→→→→→→→→ε
```

Step 5: `(a|b)*abb` → Concatenate with a, b, b:
```
q6 → NFA₄ → q7 →a→ q8 →b→ q9 →b→ q10(accept)
```

Total states: ~11 states (Thompson's creates at most 2× regex length states)

### Subset Construction (NFA → DFA)

**Algorithm:**
```
ε-closure(s) = set of all states reachable from s via ε-transitions only
ε-closure(T) = ∪ ε-closure(t) for all t ∈ T
move(T, a) = set of states reachable from any state in T via symbol a
```

**Worked Example:** NFA for `(a|b)*abb`

Let ε-closure({q0}) = A = {q0, q1, q2, q4, q6, q7} (start state of DFA)

| DFA State | NFA States | move(_, a) | move(_, b) |
|---|---|---|---|
| A | {q0,q1,q2,q4,q6,q7} | B | C |
| B | {q1,q2,q3,q4,q6,q7,q8} | B | D |
| C | {q1,q2,q4,q5,q6,q7} | B | C |
| D | {q1,q2,q4,q5,q6,q7,q9} | B | E |
| E | {q1,q2,q4,q5,q6,q7,q10} | B | C |

E is accept state (contains q10).

**DFA Transition Table:**

| State | a | b |
|---|---|---|
| →A | B | C |
| B | B | D |
| C | B | C |
| D | B | **E** |
| **E** | B | C |

### DFA Minimization (Hopcroft's Algorithm)

**Goal:** Produce DFA with minimum number of states.

**Algorithm:**
1. Partition states into two groups: {accepting} and {non-accepting}
2. For each group, check if all states agree on transitions (go to same group)
3. If not, split the group
4. Repeat until no more splits

**Trace on above DFA:**

Initial partition: P₀ = {{A, B, C, D}, {E}}

Check group {A, B, C, D}:
- On input `a`: A→B, B→B, C→B, D→B → all go to same group ✅
- On input `b`: A→C, B→D, C→C, D→E → D goes to {E}, others stay in {A,B,C,D} → SPLIT!

P₁ = {{A, B, C}, {D}, {E}}

Check group {A, B, C}:
- On `a`: A→B, B→B, C→B → all go to {A,B,C} ✅
- On `b`: A→C, B→D, C→C → B goes to {D}, A and C stay → SPLIT!

P₂ = {{A, C}, {B}, {D}, {E}}

Check group {A, C}:
- On `a`: A→B, C→B → both go to {B} ✅
- On `b`: A→C, C→C → both go to {A,C} ✅ → NO SPLIT

**Final minimized DFA:** 4 states: {A,C}, {B}, {D}, {E}

| State | a | b |
|---|---|---|
| →{A,C} | {B} | {A,C} |
| {B} | {B} | {D} |
| {D} | {B} | **{E}** |
| **{E}** | {B} | {A,C} |

> 🎯 **GATE fact:** DFA minimization is unique (up to renaming). Minimum DFA states = number of equivalence classes under Myhill-Nerode theorem.

### Myhill-Nerode Theorem

Two strings x and y are **equivalent** if for ALL strings z: xz ∈ L iff yz ∈ L.

**Number of equivalence classes = number of states in minimum DFA.**

This is also how you PROVE a language needs at least $k$ DFA states.

---

## 🔬 Deep Dive: FIRST & FOLLOW — Complex Examples

### Example: Grammar with Chained ε-Productions

```
S → ABCD
A → a | ε
B → b | ε
C → c | ε
D → d | ε
```

**FIRST:**
- FIRST(A) = {a, ε}
- FIRST(B) = {b, ε}
- FIRST(C) = {c, ε}
- FIRST(D) = {d, ε}
- FIRST(S) = {a, b, c, d, ε} (since all can be ε, S can derive ε)

**FOLLOW:**
- FOLLOW(S) = {$}
- FOLLOW(A): S → A•BCD → FIRST(BCD) − {ε} ∪ (if BCD ⇒* ε then FOLLOW(S))
  = {b, c, d} ∪ {$} = {b, c, d, $}
- FOLLOW(B): S → AB•CD → FIRST(CD) − {ε} ∪ (if CD ⇒* ε then FOLLOW(S))
  = {c, d} ∪ {$} = {c, d, $}
- FOLLOW(C): S → ABC•D → FIRST(D) − {ε} ∪ (if D ⇒* ε then FOLLOW(S))
  = {d} ∪ {$} = {d, $}
- FOLLOW(D): S → ABCD• → FOLLOW(S) = {$}

**LL(1) Check:** For A → a | ε:
- FIRST(a) ∩ FOLLOW(A) = {a} ∩ {b,c,d,$} = ∅ ✅
- Similarly for B, C, D → **LL(1)** ✅

### Example: Grammar with Multiple Non-Terminals

```
S → aBDh
B → cC
C → bC | ε
D → EF
E → g | ε
F → f | ε
```

**FIRST:**

| Symbol | FIRST |
|---|---|
| S | {a} |
| B | {c} |
| C | {b, ε} |
| D | {g, f, ε} |
| E | {g, ε} |
| F | {f, ε} |

FIRST(D) = FIRST(E) − {ε} ∪ FIRST(F) − {ε} ∪ {ε} (since both E and F can be ε)
= {g, f, ε}

**FOLLOW:**

| Symbol | FOLLOW |
|---|---|
| S | {$} |
| B | {g, f, h} |
| C | {g, f, h} |
| D | {h} |
| E | {f, h} |
| F | {h} |

Derivation:
- FOLLOW(B): S → aB•Dh → FIRST(Dh) − {ε} = FIRST(D) − {ε} ∪ FIRST(h) = {g, f, h}
- FOLLOW(C): B → cC• → FOLLOW(B) = {g, f, h}
- FOLLOW(D): S → aBD•h → {h}
- FOLLOW(E): D → E•F → FIRST(F) − {ε} ∪ (ε ∈ FIRST(F) → FOLLOW(D)) = {f, h}
- FOLLOW(F): D → EF• → FOLLOW(D) = {h}

### Example: Ambiguous Grammar — Dangling Else

```
S → iEtSS' | a
S' → eS | ε
E → b
```

**FIRST:**
- FIRST(S) = {i, a}
- FIRST(S') = {e, ε}
- FIRST(E) = {b}

**FOLLOW:**
- FOLLOW(S) = {$, e} (from S' → eS•)
- FOLLOW(S') = FOLLOW(S) = {$, e}
- FOLLOW(E) = {t}

**LL(1) Check for S' → eS | ε:**
- FIRST(eS) ∩ FOLLOW(S') = {e} ∩ {$, e} = {e} ≠ ∅ → **NOT LL(1)!**

This is the classic **dangling else** ambiguity. We resolve by convention: "match each `else` with the closest unmatched `then`" → put S' → eS in M[S', e].

---

## 🔬 Deep Dive: LL(1) Parsing — Complete Trace

### Parsing `id + id * id` with the Expression Grammar

**Grammar (after left-recursion elimination):**
```
E  → T E'
E' → + T E' | ε
T  → F T'
T' → * F T' | ε
F  → ( E ) | id
```

**Parsing Table:**

| | id | + | * | ( | ) | $ |
|--|---|---|---|---|---|--|
| E | TE' | | | TE' | | |
| E'| | +TE' | | | ε | ε |
| T | FT' | | | FT' | | |
| T'| | ε | *FT' | | ε | ε |
| F | id | | | (E) | | |

**Trace:**

| Stack | Input | Action |
|---|---|---|
| $E | id + id * id $ | E → TE' |
| $E'T | id + id * id $ | T → FT' |
| $E'T'F | id + id * id $ | F → id |
| $E'T'id | id + id * id $ | match id |
| $E'T' | + id * id $ | T' → ε |
| $E' | + id * id $ | E' → +TE' |
| $E'T+ | + id * id $ | match + |
| $E'T | id * id $ | T → FT' |
| $E'T'F | id * id $ | F → id |
| $E'T'id | id * id $ | match id |
| $E'T' | * id $ | T' → *FT' |
| $E'T'F* | * id $ | match * |
| $E'T'F | id $ | F → id |
| $E'T'id | id $ | match id |
| $E'T' | $ | T' → ε |
| $E' | $ | E' → ε |
| $ | $ | **ACCEPT** ✅ |

**Parse tree built:** E(T(F(id), T'(ε)), E'(+, T(F(id), T'(*, F(id), T'(ε))), E'(ε)))

This correctly gives `id + (id * id)` — multiplication has higher precedence!

---

## 🔬 Deep Dive: SLR(1) Parsing — Complete Construction

### Grammar: S → CC, C → cC | d

**Step 1: Augmented Grammar**
```
S' → S
S  → CC
C  → cC
C  → d
```

**Step 2: LR(0) Item Sets**

**I₀:** Closure({S' → •S})
```
S' → •S
S  → •CC
C  → •cC
C  → •d
```

**I₁ = Goto(I₀, S):**
```
S' → S•
```

**I₂ = Goto(I₀, C):**
```
S → C•C
C → •cC
C → •d
```

**I₃ = Goto(I₀, c):**
```
C → c•C
C → •cC
C → •d
```

**I₄ = Goto(I₀, d):**
```
C → d•
```

**I₅ = Goto(I₂, C):**
```
S → CC•
```

**Goto(I₂, c) = I₃** (same)
**Goto(I₂, d) = I₄** (same)

**I₆ = Goto(I₃, C):**
```
C → cC•
```

**Goto(I₃, c) = I₃** (self-loop)
**Goto(I₃, d) = I₄** (same)

**States: I₀ through I₆** (7 states)

**Step 3: FOLLOW Sets**
- FOLLOW(S) = {$}
- FOLLOW(C): From S → C•C: FIRST(C) = {c, d}. From S → CC•: FOLLOW(S) = {$}
  → FOLLOW(C) = {c, d, $}

**Step 4: SLR(1) Parsing Table**

| State | c | d | $ | S | C |
|---|---|---|---|---|---|
| 0 | s3 | s4 | | 1 | 2 |
| 1 | | | acc | | |
| 2 | s3 | s4 | | | 5 |
| 3 | s3 | s4 | | | 6 |
| 4 | r3 | r3 | r3 | | |
| 5 | | | r1 | | |
| 6 | r2 | r2 | r2 | | |

Where: r1 = S→CC, r2 = C→cC, r3 = C→d

**No conflicts → SLR(1) grammar** ✅

**Step 5: Parse `ccdd`**

| Stack | Input | Action |
|---|---|---|
| 0 | c c d d $ | shift 3 |
| 0 c 3 | c d d $ | shift 3 |
| 0 c 3 c 3 | d d $ | shift 4 |
| 0 c 3 c 3 d 4 | d $ | reduce C → d |
| 0 c 3 c 3 C 6 | d $ | reduce C → cC |
| 0 c 3 C 6 | d $ | reduce C → cC |
| 0 C 2 | d $ | shift 4 |
| 0 C 2 d 4 | $ | reduce C → d |
| 0 C 2 C 5 | $ | reduce S → CC |
| 0 S 1 | $ | **ACCEPT** ✅ |

---

## 🔬 Deep Dive: CLR(1) vs LALR(1) — When They Differ

### Example Where SLR(1) Fails but CLR(1) Works

**Grammar:**
```
S → L = R | R
L → * R | id
R → L
```

**SLR(1) FOLLOW sets:**
- FOLLOW(R) = {$, =}

In state with items [L → id•] and [R → L•], on lookahead `=`:
- For R → L: should reduce? FOLLOW(R) = {$, =} → yes
- But we should NOT reduce R → L when `=` follows (that's for assignment!)
- **SLR(1) has conflict!**

**CLR(1) resolves this:**
- [R → L•, $] — only reduce when lookahead is $
- [R → L•, =] would NOT appear in the correct state

This grammar IS CLR(1) but NOT SLR(1).

### LALR(1) Merge Problem

When merging CLR(1) states with same core but different lookaheads:
- Can introduce **reduce-reduce** conflicts (not shift-reduce)
- Example: States {[A→α•, a], [B→β•, b]} and {[A→α•, b], [B→β•, a]}
  - Merged: {[A→α•, a/b], [B→β•, a/b]} → on 'a': reduce A or B? CONFLICT!

> 🎯 **GATE key:** LALR(1) can have reduce-reduce conflicts that CLR(1) doesn't, but NEVER shift-reduce conflicts.

---

## 🔬 Deep Dive: Operator Precedence Parsing — Complete Trace

### Parse `id + id * id` using operator precedence

**Precedence table:**

| | + | * | id | $ |
|---|---|---|---|---|
| + | ⋗ | ⋖ | ⋖ | ⋗ |
| * | ⋗ | ⋗ | ⋖ | ⋗ |
| id | ⋗ | ⋗ | | ⋗ |
| $ | ⋖ | ⋖ | ⋖ | |

**Trace:**

| Stack | Relation | Input | Action |
|---|---|---|---|
| $ | ⋖ | id + id * id $ | shift id |
| $ id | ⋗ | + id * id $ | reduce id → E |
| $ E | | + id * id $ | (E not in table, skip) |
| $ | ⋖ | + id * id $ | shift + |
| $ + | ⋖ | id * id $ | shift id |
| $ + id | ⋗ | * id $ | reduce id → E |
| $ + E | | * id $ | |
| $ + | ⋖ | * id $ | shift * |
| $ + * | ⋖ | id $ | shift id |
| $ + * id | ⋗ | $ | reduce id → E |
| $ + * E | | $ | |
| $ + * | ⋗ | $ | reduce E * E → E |
| $ + E | | $ | |
| $ + | ⋗ | $ | reduce E + E → E |
| $ E | | $ | **ACCEPT** ✅ |

Order of reductions: id, id, id, E*E, E+E → respects * before + precedence!

---

## 🔬 Deep Dive: SDT Evaluation — Detailed Examples

### Example: Desk Calculator (S-Attributed)

**Grammar with semantic rules:**
```
L → E '\n'      { print(E.val) }
E → E₁ + T      { E.val = E₁.val + T.val }
E → T            { E.val = T.val }
T → T₁ * F      { T.val = T₁.val × F.val }
T → F            { T.val = F.val }
F → ( E )        { F.val = E.val }
F → digit        { F.val = digit.lexval }
```

**Evaluate: `3 * 5 + 4 \n`**

Parse tree (bottom-up):
```
F.val=3  F.val=5     F.val=4
  |        |           |
T.val=3  T₁.val=3    T.val=4
  |        |           |
  |     T.val=15       |
  |        |           |
  |     E₁.val=15      |
  |        \        /
  |       E.val=19
  |          |
        L → print(19)
```

**Output: 19** ✅

### Example: Type Checking (L-Attributed / Inherited)

**Grammar:**
```
D → T L          { L.inh = T.type }
T → int          { T.type = integer }
T → float        { T.type = float }
L → L₁ , id     { L₁.inh = L.inh; addType(id.entry, L.inh) }
L → id           { addType(id.entry, L.inh) }
```

**Input: `int a, b, c`**

```
D → T L
  T → int → T.type = integer
  L.inh = integer (inherited from parent)
  L → L₁ , id(c)
    L₁.inh = integer
    addType(c, integer)
    L₁ → L₂ , id(b)
      L₂.inh = integer
      addType(b, integer)
      L₂ → id(a)
        addType(a, integer)
```

Result: a, b, c all get type `integer` in symbol table ✅

### Example: Array Address Computation (SDT)

For array element `A[i][j]` (2D, row-major):

**Address formula:** `base(A) + ((i - low₁) * n₂ + (j - low₂)) * w`

Where: n₂ = number of columns, w = element width, low₁/low₂ = lower bounds

**Three-address code generation:**
```
t1 = i - low₁
t2 = t1 * n₂
t3 = j - low₂
t4 = t2 + t3
t5 = t4 * w
t6 = base(A) + t5      // address of A[i][j]
```

---

## 🔬 Deep Dive: Intermediate Code — Complex Constructs

### Boolean Expressions — Short-Circuit Evaluation

**For `if (a < b || c < d && e < f) goto L_true else goto L_false`:**

```
    if a < b goto L_true          // short-circuit: a<b is true → whole OR is true
    goto L1
L1: if c < d goto L2             // check c<d for AND
    goto L_false
L2: if e < f goto L_true         // both c<d and e<f must be true
    goto L_false
```

**Precedence:** AND (&&) binds tighter than OR (||)

### While Loop

**For `while (a < b) { a = a + 1; }`:**

```
L_begin:
    if a < b goto L_body
    goto L_after
L_body:
    t1 = a + 1
    a = t1
    goto L_begin
L_after:
```

### For Loop

**For `for (i = 0; i < n; i++) { sum = sum + a[i]; }`:**

```
    i = 0
L_begin:
    if i < n goto L_body
    goto L_after
L_body:
    t1 = i * 4              // assuming 4 bytes per element
    t2 = a[t1]
    t3 = sum + t2
    sum = t3
    t4 = i + 1
    i = t4
    goto L_begin
L_after:
```

### Switch/Case Statement

**Method 1: Sequential if-else chain:**
```
switch(x) { case 1: s1; case 2: s2; case 3: s3; default: s_def; }

    if x == 1 goto L1
    if x == 2 goto L2
    if x == 3 goto L3
    goto L_def
L1: <s1>; goto L_after
L2: <s2>; goto L_after
L3: <s3>; goto L_after
L_def: <s_def>
L_after:
```

**Method 2: Jump table** (better for dense ranges):
```
    t1 = x - 1              // normalize
    if t1 < 0 goto L_def
    if t1 > 2 goto L_def
    goto JumpTable[t1]      // indexed jump
```

### Function Call

**For `x = f(a, b, c)`:**

```
    param c          // push parameters (right to left in C)
    param b
    param a
    call f, 3        // call f with 3 parameters
    x = return_val   // get return value
```

---

## 🔬 Deep Dive: DAG Construction for Basic Block

### Example Basic Block:
```
a = b + c
b = a - d
c = b + c
d = a - d
```

**Step-by-step DAG construction:**

1. `a = b + c`: Create node +(b₀, c₀), label it a
2. `b = a - d`: Create node -(a, d₀), label it b  
3. `c = b + c`: Create node +(b, c₀). But c₀ is the ORIGINAL c, not the one from step 1!
4. `d = a - d`: a - d₀ → SAME as step 2! → Just add label d to existing node

**DAG:**
```
        a, (1)
       +
      / \
    b₀   c₀
      \  /
    b,d (2)
      -
     / \
    a   d₀
    
    c (3)
      +
     / \
   b,d  c₀
```

**Observations from DAG:**
- d = b (common subexpression: both are `a - d₀`)
- c uses the new value of b (statement 2's result)
- Optimal code: compute `a = b₀ + c₀`, `b = a - d₀`, `d = b`, `c = b + c₀`

> 🎯 **GATE fact:** DAG automatically detects common subexpressions! Two identical computations point to the same node.

---

## 🔬 Deep Dive: Data Flow Analysis — Complete Example

### Reaching Definitions

**Control Flow Graph:**
```
Block B1: d1: i = m - 1
          d2: j = n
          d3: a = u1

Block B2: d4: i = i + 1

Block B3: d5: j = j - 1

Block B4: d6: a = u2

Edges: B1→B2, B2→B3, B3→B4, B3→B2 (back edge), B4→B2 (back edge)
       Also B2→exit (some condition)
```

**GEN and KILL sets:**

| Block | GEN | KILL |
|---|---|---|
| B1 | {d1, d2, d3} | {d4, d5, d6} |
| B2 | {d4} | {d1} |
| B3 | {d5} | {d2} |
| B4 | {d6} | {d3} |

**Iterative computation (IN = ∪ OUT of predecessors, OUT = GEN ∪ (IN − KILL)):**

**Iteration 1:**

| Block | IN | OUT |
|---|---|---|
| B1 | ∅ | {d1,d2,d3} |
| B2 | {d1,d2,d3} | {d2,d3,d4} |
| B3 | {d2,d3,d4} | {d3,d4,d5} |
| B4 | {d3,d4,d5} | {d4,d5,d6} |

**Iteration 2:**
- IN[B2] = OUT[B1] ∪ OUT[B3] ∪ OUT[B4] = {d1,d2,d3} ∪ {d3,d4,d5} ∪ {d4,d5,d6} = {d1,d2,d3,d4,d5,d6}
- OUT[B2] = {d4} ∪ ({d1,d2,d3,d4,d5,d6} − {d1}) = {d2,d3,d4,d5,d6}
- IN[B3] = OUT[B2] = {d2,d3,d4,d5,d6}
- OUT[B3] = {d5} ∪ ({d2,d3,d4,d5,d6} − {d2}) = {d3,d4,d5,d6}
- IN[B4] = OUT[B3] = {d3,d4,d5,d6}
- OUT[B4] = {d6} ∪ ({d3,d4,d5,d6} − {d3}) = {d4,d5,d6}

**Iteration 3:**
- IN[B2] = {d1,d2,d3} ∪ {d3,d4,d5,d6} ∪ {d4,d5,d6} = {d1,d2,d3,d4,d5,d6} → same as before → **CONVERGED** ✅

**Result:** At entry of B2, ALL definitions reach — this means the compiler cannot do constant propagation for i, j, or a at B2's entry.

### Live Variable Analysis — Complete Trace

**Same CFG, variables live at program exit: ∅**

**USE and DEF sets:**

| Block | USE (used before defined) | DEF (defined before used) |
|---|---|---|
| B1 | {m, n, u1} | {i, j, a} |
| B2 | {i} | {i} |
| B3 | {j} | {j} |
| B4 | {u2} | {a} |

**Backward analysis (OUT = ∪ IN of successors, IN = USE ∪ (OUT − DEF)):**

**Iteration 1:**

| Block | OUT | IN |
|---|---|---|
| B4 | ∅ (back to B2) | {u2} |
| B3 | {u2} (to B4 and B2) | {j, u2} |
| B2 | {j, u2} (to B3 and exit) | {i, j, u2} |
| B1 | {i, j, u2} | {m, n, u1, u2} |

**Iteration 2:**
- OUT[B4] = IN[B2] = {i, j, u2}
- IN[B4] = {u2} ∪ ({i, j, u2} − {a}) = {i, j, u2}
- OUT[B3] = IN[B4] ∪ IN[B2] = {i, j, u2} ∪ {i, j, u2} = {i, j, u2}
- IN[B3] = {j} ∪ ({i, j, u2} − {j}) = {i, j, u2}
- OUT[B2] = IN[B3] ∪ ∅ = {i, j, u2}
- IN[B2] = {i} ∪ ({i, j, u2} − {i}) = {i, j, u2} → same → **CONVERGED** ✅

**Result:** At B1 entry, m, n, u1, u2 must be live (defined before this block).

---

## 🔬 Deep Dive: Code Optimization — Trace Examples

### Loop Optimization — Complete Example

**Original code:**
```
for (i = 0; i < n; i++) {
    t = x * y;          // loop invariant!
    a[i] = t + i;
}
```

**Three-address code:**
```
    i = 0
L1: if i >= n goto L2
    t = x * y           // invariant: x, y not modified in loop
    t1 = i * 4          // induction variable expression
    t2 = a + t1
    *t2 = t + i
    i = i + 1
    goto L1
L2:
```

**After loop invariant code motion:**
```
    i = 0
    t = x * y           // ← moved out of loop!
L1: if i >= n goto L2
    t1 = i * 4
    t2 = a + t1
    *t2 = t + i
    i = i + 1
    goto L1
L2:
```

**After strength reduction (i*4 → incremental addition):**
```
    i = 0
    t = x * y
    t1 = 0              // replaces i * 4
    t2 = a              // replaces a + t1
L1: if i >= n goto L2
    *t2 = t + i
    i = i + 1
    t1 = t1 + 4         // ← strength reduction: addition instead of multiplication
    t2 = t2 + 4         // ← incremental update
    goto L1
L2:
```

**After induction variable elimination (i is no longer needed if only used for loop control):**
```
    t = x * y
    t1 = 0
    t2 = a
    t_limit = n * 4     // pre-compute loop bound
L1: if t1 >= t_limit goto L2
    *t2 = t + t1/4      // or keep separate counter
    t1 = t1 + 4
    t2 = t2 + 4
    goto L1
L2:
```

### Common Subexpression Elimination — Trace

**Before:**
```
a = b + c
d = b + c       // common subexpression!
e = a + d
f = b + c       // again!
```

**After CSE:**
```
a = b + c
d = a           // reuse a (= b + c)
e = a + d
f = a           // reuse a (= b + c, still valid since b,c unchanged)
```

### Dead Code Elimination — Trace

```
a = b + c       // a is used in line 3
d = e * f       // d is NEVER used → DEAD CODE
x = a + 1       // x is used later
return x
```

**After dead code elimination:**
```
a = b + c
x = a + 1
return x
```

---

## 📝 Practice Set 1: Lexical Analysis & Parsing Basics (P1 – P40)

**P1.** Regular expression for identifiers starting with letter, followed by letters/digits, max length 8?
→ `letter(letter|digit){0,7}` or `letter(letter|digit)^{0,7}`

**P2.** How many tokens in: `int main() { return 0; }`?
→ int(1), main(2), ((3), )(4), {(5), return(6), 0(7), ;(8), }(9) → **9 tokens**

**P3.** NFA for `a*b` has how many states using Thompson's construction?
→ a* needs 4 states (2 for `a` + 2 for star), concatenation with b adds 2 → **6 states**
(Thompson's: each operator adds 2 states)

**P4.** Minimum DFA for language `{w | w has even number of a's}` over {a, b}?
→ 2 states: even(start, accept) and odd. 
→ **2 states**

**P5.** Minimum DFA for `(a|b)*abb`?
→ As shown in deep dive: **4 states**

**P6.** Language of regex `(0|1)*0`?
→ All binary strings ending in 0. **Set of all even binary numbers including 0**

**P7.** Is `{aⁿbⁿ | n ≥ 0}` regular?
→ **NO** (requires counting, not recognizable by DFA — Pumping Lemma)

**P8.** Can a lexer handle nested comments `/* ... /* ... */ ... */`?
→ **NO** with just regex/DFA (need context-free grammar for nesting). 
→ Workaround: use a counter (not pure DFA).

**P9.** Maximal munch: lexer sees "iffy". Token?
→ ID("iffy"), NOT keyword IF + ID("fy"). **Maximal munch gives single identifier "iffy"**

**P10.** Number of tokens for `a+++b` in C?
→ Maximal munch: a(ID), ++(INCR), +(PLUS), b(ID) → **4 tokens** (parsed as `(a++) + b`)

**P11-P20** — FIRST and FOLLOW:

**P11.** Grammar: S → aB | bA, A → a | aS | bAA, B → b | bS | aBB
FIRST(S) = {a, b}, FIRST(A) = {a, b}, FIRST(B) = {a, b}
FOLLOW(S) = {$} ∪ FOLLOW(A) ∪ FOLLOW(B) ... (complex — need full computation)

**P12.** Grammar: S → ABC, A → a | ε, B → b | ε, C → c
FIRST(S) = {a, b, c}
FOLLOW(A) = FIRST(BC) − {ε} ∪ (if BC→ε? B can be ε, C cannot) = {b, c}
FOLLOW(B) = {c}
FOLLOW(C) = {$}

**P13.** Is the following grammar LL(1)? S → aS | a
→ FIRST(aS) = {a}, FIRST(a) = {a} → FIRST(aS) ∩ FIRST(a) = {a} ≠ ∅ → **NOT LL(1)** (need left factoring)

**P14.** After left factoring: S → aS', S' → S | ε. Now LL(1)?
→ FIRST(S) = {a}, FIRST(ε) = {ε}. Check: FIRST(S) ∩ FOLLOW(S') = {a} ∩ {$} = ∅ ✅ → **YES, LL(1)** 

**P15.** Grammar: E → E + E | id. Leftmost derivation of id + id + id?
→ Two possible: E ⇒ E+E ⇒ E+E+E ⇒ id+E+E ⇒ id+id+E ⇒ id+id+id
  OR: E ⇒ E+E ⇒ id+E ⇒ id+E+E ⇒ id+id+E ⇒ id+id+id
→ **Two different leftmost derivations → AMBIGUOUS**

**P16.** Eliminate left recursion: A → Aa | Ab | c | d
→ A → cA' | dA', A' → aA' | bA' | ε

**P17.** Left factor: S → iEtS | iEtSeS | a
→ S → iEtSS' | a, S' → eS | ε

**P18.** FOLLOW(S) for S → (L) | a, L → L,S | S:
→ FOLLOW(S) = {$} ∪ {)} ∪ {,} = {$, ), ,}

**P19.** LL(1) table conflict check for: S → AB, A → a | ε, B → a | b
→ FIRST(a) for A → a = {a}. For A → ε, check FOLLOW(A) ∩ {a}. FOLLOW(A) = FIRST(B) = {a, b}. 
→ {a} ∩ {a, b} = {a} ≠ ∅ → **NOT LL(1)!**

**P20.** How many entries in LL(1) table for grammar with 5 non-terminals and 8 terminals (+$)?
→ Table size = 5 × 9 = **45 entries** (some empty)

**P21-P30** — LR Parsing:

**P21.** Number of LR(0) items for grammar S → AB, A → a, B → b?
→ Augmented: S'→S, S→AB, A→a, B→b
→ Items: S'→•S, S'→S•, S→•AB, S→A•B, S→AB•, A→•a, A→a•, B→•b, B→b•
→ **9 items**

**P22.** How many LR(0) states for grammar in P21?
→ I₀: {S'→•S, S→•AB, A→•a}
→ I₁: Goto(I₀,S) = {S'→S•}
→ I₂: Goto(I₀,A) = {S→A•B, B→•b}
→ I₃: Goto(I₀,a) = {A→a•}
→ I₄: Goto(I₂,B) = {S→AB•}
→ I₅: Goto(I₂,b) = {B→b•}
→ **6 states**

**P23.** SLR(1) table for above grammar — any conflicts? → **No conflicts** (each state has either shifts or a single reduce)

**P24.** Grammar: S → Sa | b. Is it LR(0)?
→ State with {S → b•, S → S•a}: complete item + shift item → **shift-reduce conflict → NOT LR(0)**

**P25.** Is the grammar in P24 SLR(1)?
→ Reduce S → b when lookahead ∈ FOLLOW(S) = {a, $}. But shift on 'a' also requested.
→ FOLLOW(S) = {a, $}. On 'a': shift AND reduce → **CONFLICT → NOT SLR(1)**

Wait — let me reconsider. S → Sa | b. FOLLOW(S) = {$, a}. In state {S → b•}: reduce on FOLLOW(S) = {$, a}. Is there a shift on 'a' in this state? State: just {S → b•} — no shift. In the state with {S → S•a}: shift on 'a'. No reduce here.

Actually I₀: S'→•S, S→•Sa, S→•b
I₁: Goto(I₀,S) = S'→S•, S→S•a → on 'a': shift(Goto(I₁,a)), on '$': reduce S'→S → **No conflict in I₁** (shift on a, reduce on $)
I₂: Goto(I₀,b) = S→b• → reduce on all FOLLOW(S) = {a,$}
I₃: Goto(I₁,a) = S→Sa• → reduce on FOLLOW(S) = {a,$}

→ **SLR(1)!** ✅ (no conflicts)

**P26.** Every LL(1) grammar is also: (a) LR(0) (b) SLR(1) (c) LALR(1) (d) Both b and c
→ **(d) Both SLR(1) and LALR(1)** (and also CLR(1)). Not necessarily LR(0).

**P27.** LALR(1) merging can create: (a) shift-reduce conflicts (b) reduce-reduce conflicts (c) both (d) neither
→ **(b) reduce-reduce conflicts only**

**P28.** Which parser does YACC/Bison use? → **LALR(1)**

**P29.** Grammar: S → Aa | bAc | Bc | bBa, A → d, B → d. Is this SLR(1)?
→ FOLLOW(A) = {a, c}, FOLLOW(B) = {c, a}. State with {A→d•, B→d•}: reduce A→d on {a,c}, reduce B→d on {c,a}. Overlap on both a and c → **reduce-reduce conflict → NOT SLR(1)!** But it IS LALR(1)/CLR(1).

**P30.** Number of states in SLR(1) vs CLR(1) for grammar with n LR(0) items?
→ SLR(1) states = LR(0) states ≤ CLR(1) states. CLR(1) can be exponentially larger.

**P31-P40** — Mixed:

| # | Question | Answer |
|---|---|---|
| P31 | Compiler phase that uses DFA | Lexical Analysis |
| P32 | Compiler phase that uses CFG | Syntax Analysis |
| P33 | Compiler phase that uses SDT | Semantic Analysis |
| P34 | Symbol table is accessed by which phases? | ALL phases |
| P35 | Regular expression for C-style comments `/* ... */` | `/\*([^*]|\*+[^*/])*\*+/` (complex!) |
| P36 | Is `{ww | w ∈ {a,b}*}` context-free? | NO (context-sensitive) |
| P37 | Recursive descent parser = which parsing? | Top-down (LL) |
| P38 | Shift-reduce parser = which parsing? | Bottom-up (LR) |
| P39 | Handle in bottom-up parsing corresponds to? | RHS of production to reduce |
| P40 | Viable prefix property is related to? | LR parsing (stack contents) |

---

## 📝 Practice Set 2: SDT, ICG, and Code Optimization (P41 – P80)

**P41.** (2-mark) Generate 3AC for: `a = b * -c + b * -c`

Without optimization:
```
t1 = -c
t2 = b * t1
t3 = -c
t4 = b * t3
t5 = t2 + t4
a = t5
```

With CSE:
```
t1 = -c
t2 = b * t1
t3 = t2 + t2     // reuse t2
a = t3
```

**P42.** Number of temporaries needed for `a = b + c * d - e / f`?
```
t1 = c * d
t2 = b + t1
t3 = e / f
t4 = t2 - t3
a = t4
```
→ **4 temporaries** (t1 can be reused after t2, so with optimization: 2)

**P43.** Convert to SSA form:
```
x = 1
x = x + 1
y = x * 2
```
→ SSA: `x₁ = 1; x₂ = x₁ + 1; y₁ = x₂ * 2`

**P44.** Is the following S-attributed or L-attributed?
```
A → BC  { B.inh = A.inh; C.inh = B.syn }
```
→ **L-attributed** (B.inh depends on parent's inherited; C.inh depends on left sibling B's synthesized) ✅

**P45.** Quadruple for `a[i] = b + c`:
```
(0) (+, b, c, t1)
(1) ([]=, a, i, t1)    // or (store, t1, a[i], _)
```

**P46.** Triple for `if x < y goto L`:
→ `(0) (<, x, y)    (1) (jmpT, (0), L)` OR `(0) (jlt, x, y, L)` (varies by notation)

**P47.** Identify leaders in:
```
(1) i = 1           ← leader (first stmt)
(2) j = 1
(3) t1 = 10 * n
(4) t2 = i + j      ← leader (target of goto in 12)
(5) t3 = 8 * t2
(6) t4 = t3 - 88
(7) a[t4] = 0.0
(8) j = j + 1
(9) if j <= 10 goto (4)
(10) i = i + 1      ← leader (after conditional jump)
(11) if i <= 10 goto (4)
(12) i = 1          ← leader (after conditional jump)
```
→ **Leaders: 1, 4, 10, 12**. Basic blocks: {1-3}, {4-9}, {10-11}, {12+}

**P48.** CSE in: `t1 = a + b; t2 = a + b; t3 = t1 + t2`
→ After CSE: `t1 = a + b; t2 = t1; t3 = t1 + t1`
→ After copy propagation + constant folding: `t1 = a + b; t3 = t1 + t1` (t2 eliminated if unused)

**P49.** Available expressions at end of B2:
```
B1: t1 = a + b
    goto B2 or B3

B2: t2 = a + b     // a+b available at entry? Only if available at exit of ALL predecessors
    t3 = t2 * c
```
If B1 is only predecessor of B2 and a,b not modified → `a+b` available at B2 entry → reuse!

**P50.** Dead code in:
```
x = 5         // x redefined before use
x = y + z     
return x
```
→ `x = 5` is dead code (immediately overwritten). **Remove it.**

**P51-P60** — Data Flow Analysis:

**P51.** Reaching definitions use which set operation at join points? → **Union (∪)** (a definition reaches if it comes along ANY path)

**P52.** Available expressions use which set operation? → **Intersection (∩)** (expression available only if available along ALL paths)

**P53.** Live variables analysis direction? → **Backward** (from uses back to definitions)

**P54.** Reaching definitions analysis direction? → **Forward** (from definitions to uses)

**P55.** If variable x is NOT live after `x = a + b`, what optimization? → **Dead code elimination** (remove the assignment)

**P56.** If `a + b` is available at a point and used again, what optimization? → **Common subexpression elimination**

**P57.** Loop invariant: `t = a * b` inside loop, a and b not modified in loop. Optimization? → **Code motion** (move outside loop)

**P58.** `x = x * 2` inside loop. Strength reduction gives? → `x = x + x` (addition instead of multiplication) or `x = x << 1`

**P59.** Peephole: `goto L1; L1: goto L2`. Optimized? → `goto L2` (jump chain elimination)

**P60.** Peephole: `MOV R1, a; MOV a, R1`. Optimized? → Remove second instruction (redundant)

**P61-P70** — Runtime Environments:

**P61.** In static scoping, `x` in inner function refers to the x in the? → **Textually enclosing scope** (determined by program structure)

**P62.** In dynamic scoping, `x` refers to? → **Most recently active scope** on the call stack that defines x

**P63.** Access link (static link) points to? → **Activation record of the immediately enclosing scope** (in terms of nesting)

**P64.** Control link (dynamic link) points to? → **Activation record of the caller**

**P65.** Display D[i] stores? → **Pointer to most recent activation record at nesting depth i**

**P66.** Call-by-reference: will `swap(a, a)` work correctly?
→ **NO!** Both parameters alias to the same variable. After `temp = a; a = a;` temp gets overwritten.

**P67.** What happens in call-by-name for `swap(i, a[i])`?
→ Each reference to the formal parameter re-evaluates the actual expression. If `i` changes during swap, `a[i]` refers to a different element! **Incorrect behavior possible.**

**P68.** Tail recursion can be optimized to? → **Iteration** (no stack growth — $O(1)$ space)

**P69.** Heap allocation: malloc uses? → **Free list** (first fit, best fit, or buddy system)

**P70.** Mark-and-sweep GC: time complexity? → **$O(\text{heap size})$** (must traverse all reachable objects + sweep all memory)

**P71-P80** — Code Generation:

**P71.** Two variables live at the same point — in register allocation graph? → **Edge between them** (cannot share register)

**P72.** k-coloring of interference graph: what does k represent? → **Number of available registers**

**P73.** If graph is not k-colorable? → **Spill** some variables to memory

**P74.** Linear scan register allocation time? → **$O(n)$** (one pass over sorted live intervals). Used in JIT compilers.

**P75.** `getReg` for `x = y + z`: if y is in R1 and y is dead after this point? → **Use R1 for x** (y's value no longer needed)

**P76.** Machine code for `if a < b goto L` (2-address machine)?
→ `CMP a, b; JLT L` or `SUB R1, a, b; BLTZ R1, L`

**P77.** Instruction selection: `a = b + c * d`. RISC (3-address): 
```
MUL R1, c, d
ADD R2, b, R1
MOV a, R2
```
→ **3 instructions**

**P78.** Same expression on 2-address machine?
```
MOV R1, c
MUL R1, d     // R1 = c * d
MOV R2, b
ADD R2, R1    // R2 = b + R1
MOV a, R2
```
→ **5 instructions**

**P79.** Same on stack machine?
```
PUSH b
PUSH c
PUSH d
MUL           // TOS = c*d
ADD           // TOS = b + c*d
POP a
```
→ **6 instructions**

**P80.** Basic block with 10 temporaries, 4 registers available. Minimum spills?
→ Depends on interference graph. Best case: if chromatic number ≤ 4, zero spills. Worst case: up to 6 spills.

---

## 🔬 Deep Dive: CLR(1) Parsing — Full Item Set Construction

### Grammar: S → CC, C → cC | d

**Augmented:** S' → S

**Step 1: LR(1) Items**

An LR(1) item: [Production with dot, lookahead]

**I₀:** Closure({[S' → •S, $]})
```
[S' → •S, $]
[S  → •CC, $]
[C  → •cC, c/d]     ← lookahead = FIRST(C$) − {ε} = {c, d}
[C  → •d, c/d]
```

**I₁ = Goto(I₀, S):**
```
[S' → S•, $]
```

**I₂ = Goto(I₀, C):**
```
[S → C•C, $]
[C → •cC, $]         ← lookahead = FIRST($) = {$}
[C → •d, $]
```

**I₃ = Goto(I₀, c):**
```
[C → c•C, c/d]
[C → •cC, c/d]
[C → •d, c/d]
```

**I₄ = Goto(I₀, d):**
```
[C → d•, c/d]
```

**I₅ = Goto(I₂, C):**
```
[S → CC•, $]
```

**I₆ = Goto(I₂, c):**
```
[C → c•C, $]
[C → •cC, $]
[C → •d, $]
```

**I₇ = Goto(I₂, d):**
```
[C → d•, $]
```

**Goto(I₃, C) = I₈:**
```
[C → cC•, c/d]
```

**Goto(I₃, c) = I₃** (self-loop)
**Goto(I₃, d) = I₄**

**I₉ = Goto(I₆, C):**
```
[C → cC•, $]
```

**Goto(I₆, c) = I₆** (self-loop)
**Goto(I₆, d) = I₇**

**CLR(1) states: I₀ through I₉ = 10 states**

**Compare with SLR(1): 7 states. The extra states (I₆, I₇, I₉) arise from different lookaheads.**

**LALR(1): Merge states with same core:**
- I₃ and I₆ have same core (C → c•C, C → •cC, C → •d) but different lookaheads → merge
- I₄ and I₇: same core (C → d•) → merge
- I₈ and I₉: same core (C → cC•) → merge

**LALR(1) = 7 states** (same as SLR(1))

In this case: SLR(1) = LALR(1) = 7 states, CLR(1) = 10 states.

> 🎯 **GATE question type:** "How many states in CLR(1) vs LALR(1) for grammar G?" Answer: CLR(1) ≥ LALR(1).

---

## 🔬 Deep Dive: Left Recursion Elimination — Indirect Case

### Direct vs Indirect Left Recursion

**Direct:** A → Aα | β (A immediately recurses)

**Indirect:** A → Bα, B → Aβ (cycle of non-terminals)

### Algorithm for Eliminating All Left Recursion

```
Order non-terminals A₁, A₂, ..., Aₙ
for i = 1 to n:
    for j = 1 to i-1:
        Replace each Aᵢ → Aⱼγ with Aᵢ → δ₁γ | δ₂γ | ...
        where Aⱼ → δ₁ | δ₂ | ... are all current Aⱼ-productions
    Eliminate direct left recursion for Aᵢ
```

### Worked Example

**Grammar:**
```
S → Aa | b
A → Ac | Sd | ε
```

**Step 1:** Order: S(A₁), A(A₂)

**Step 2:** For i=2 (A), j=1 (S):
Replace A → Sd with A → Aad | bd (substituting S → Aa | b)

Now A's productions: A → Ac | Aad | bd | ε

**Step 3:** Eliminate direct left recursion for A:
```
A → bdA' | A'
A' → cA' | adA' | ε
```

Wait: A → ε means A → A' with A' → ε.

Corrected: A → Ac | Aad | bd | ε
Non-left-recursive alternatives: β₁ = bd, β₂ = ε
Left-recursive suffixes: α₁ = c, α₂ = ad

```
A → bdA' | A'      (but A' → ε means A → ε is covered)
A' → cA' | adA' | ε
```

Simplified: A → bdA' | ε | ... Actually:

Standard formula: A → β₁A' | β₂A', A' → α₁A' | α₂A' | ε

```
A → bdA' | εA' = bdA' | A'
A' → cA' | adA' | ε
```

**Final grammar (no left recursion):**
```
S → Aa | b
A → bdA' | A'
A' → cA' | adA' | ε
```

---

## 🔬 Deep Dive: Symbol Table Management

### Symbol Table Structure

| Field | Content |
|---|---|
| Name | Identifier string |
| Type | int, float, char*, function, array, struct |
| Scope | Global, local, block number |
| Offset | Memory offset from base |
| Size | Number of bytes |
| Dimension | For arrays: number of elements per dimension |
| Parameters | For functions: parameter list |
| Line Number | First declaration line |

### Implementation Techniques

| Method | Search | Insert | Delete |
|---|---|---|---|
| Linear list | $O(n)$ | $O(1)$ | $O(n)$ |
| Binary tree | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ |
| **Hash table** | $O(1)$ avg | $O(1)$ avg | $O(1)$ avg |

> Hash table is most commonly used (in GCC, LLVM, etc.)

### Scope Management

**Block-structured languages (C, Java):**
- On entering block: push new scope level
- On exiting block: pop scope level (remove all symbols in that scope)

**Implementation:** Stack of hash tables or a single hash table with scope chains.

```
{                    // scope 0
    int x;           // x in scope 0
    {                // scope 1
        float x;     // x in scope 1 (shadows scope 0's x)
        x = 3.14;    // refers to scope 1's x
    }                // scope 1 ends, float x removed
    x = 5;           // refers to scope 0's int x
}
```

---

## 🔬 Deep Dive: Error Recovery in Detail

### Panic Mode Recovery in LL Parsing

**Algorithm:**
1. Error at M[A, a] is empty
2. Check FOLLOW(A): if current token ∈ FOLLOW(A), pop A (assume A → ε)
3. Otherwise, skip tokens until a token in FIRST(A) ∪ FOLLOW(A) is found
4. Resume parsing

**Trace:** Parsing `id * + id` with expression grammar

| Stack | Input | Action |
|---|---|---|
| $E | id * + id $ | E → TE' |
| $E'T | id * + id $ | T → FT' |
| $E'T'F | id * + id $ | F → id |
| $E'T'id | id * + id $ | match id |
| $E'T' | * + id $ | T' → *FT' |
| $E'T'F* | * + id $ | match * |
| $E'T'F | + id $ | **ERROR!** M[F, +] = empty |
| | | F should produce id or (, but + found |
| | | Skip + (not in FIRST(F) or FOLLOW(F)) |
| $E'T'F | id $ | F → id |
| $E'T'id | id $ | match id |
| $E'T' | $ | T' → ε |
| $E' | $ | E' → ε |
| $ | $ | **ACCEPT** (with error recovery) |

### Error Recovery in LR Parsing

**Algorithm:**
1. On error (empty entry in action table)
2. Pop states and symbols until finding a state s that has a GOTO entry for a non-terminal A
3. Push A and GOTO(s, A)
4. Discard input tokens until a token ∈ FOLLOW(A) is found
5. Resume parsing from that token

---

## 🔬 Deep Dive: Backpatching

### What is Backpatching?

In single-pass compilation, when generating jump instructions, we may not yet know the target address. **Backpatching** fills in these targets later.

### Mechanism

**Maintain lists of incomplete jumps:**
- `truelist`: jumps for when condition is TRUE
- `falselist`: jumps for when condition is FALSE
- `nextlist`: jumps to the next statement

**Operations:**
- `makelist(i)`: create list with single index i
- `merge(p₁, p₂)`: merge two lists
- `backpatch(p, i)`: fill in target address i for all jumps in list p

### Example: `if (a < b || c < d) x = 1`

```
100: if a < b goto _          truelist = {100}
101: goto _                   falselist = {101}
102: if c < d goto _          truelist = {102}, falselist = {102's false}
103: goto _                   falselist = {103}
```

For `||`: backpatch falselist of first to start of second
- backpatch({101}, 102): `101: goto 102`
- truelist = merge({100}, {102}) = {100, 102}
- falselist = {103}

For `if` statement:
- backpatch({100, 102}, 104): `100: if a < b goto 104`, `102: if c < d goto 104`
- `104: x = 1`
- backpatch({103}, 105): `103: goto 105`
- `105: (next statement)`

**Final code:**
```
100: if a < b goto 104
101: goto 102
102: if c < d goto 104
103: goto 105
104: x = 1
105: ...
```

---

## 📝 Practice Set 3: GATE PYQ Style — Parsing (P81 – P130)

**P81.** (2-mark) For the grammar:
```
S → aSb | bSa | SS | ε
```
What language does this generate?
→ Strings with **equal number of a's and b's**. For any string of length 2n with n a's and n b's, a derivation exists.

**P82.** (1-mark) Which parsing technique is used by GCC for C/C++?
→ **Recursive descent** (hand-written) with some table-driven components. NOT YACC-generated.
(YACC/Bison = LALR(1), but GCC moved away from it for better error messages)

**P83.** (2-mark) Grammar: S → aABe, A → Abc | b, B → d. Parse "abbcde" using shift-reduce.

| Stack | Input | Action |
|---|---|---|
| $ | abbcde$ | shift a |
| $a | bbcde$ | shift b |
| $ab | bcde$ | reduce A → b |
| $aA | bcde$ | shift b |
| $aAb | cde$ | shift c |
| $aAbc | de$ | reduce A → Abc |
| $aA | de$ | shift d |
| $aAd | e$ | reduce B → d |
| $aAB | e$ | shift e |
| $aABe | $ | reduce S → aABe |
| $S | $ | **ACCEPT** ✅ |

**P84.** (2-mark) FIRST and FOLLOW for:
```
S → ACB | Cbb | Ba
A → da | BC
B → g | ε
C → h | ε
```

FIRST(A) = {d} ∪ FIRST(B) − {ε} ∪ (if B→ε: FIRST(C)) = {d, g, h, ε}
FIRST(B) = {g, ε}
FIRST(C) = {h, ε}
FIRST(S) = FIRST(A)−{ε} ∪ FIRST(C)−{ε} ∪ FIRST(B)−{ε} ∪ ... = {d, g, h, a, b, ε}

FOLLOW(S) = {$}
FOLLOW(A) = FIRST(CB) − ... (complex — full computation needed for exam)
FOLLOW(B) = {FOLLOW(S) when in S→Ba} ∪ ... = {a, $, h, g, ...}

(This is a GATE-level tedious computation — practice the mechanical steps!)

**P85.** (1-mark) A grammar that is LR(1) but not LALR(1) will have what type of conflict in LALR?
→ **Reduce-reduce conflict**

**P86.** (2-mark) How many states in the LR(0) automaton for:
```
S' → S
S → (S)S | ε
```

Items: S'→•S, S'→S•, S→•(S)S, S→(•S)S, S→(S•)S, S→(S)•S, S→(S)S•, S→•

I₀: {S'→•S, S→•(S)S, S→•} → 3 items
I₁: Goto(I₀,S) = {S'→S•}
I₂: Goto(I₀,'(') = {S→(•S)S, S→•(S)S, S→•}
I₃: Goto(I₂,S) = {S→(S•)S}
I₄: Goto(I₃,')') = {S→(S)•S, S→•(S)S, S→•}
I₅: Goto(I₄,S) = {S→(S)S•}
Goto(I₄,'(') = I₂
Goto(I₂,'(') = I₂

**States: I₀ through I₅ = 6 states**

**P87.** (2-mark) Is the grammar `S → Sa | a` SLR(1)?
→ LR(0) items:
I₀: {S'→•S, S→•Sa, S→•a}
I₁: Goto(I₀,S) = {S'→S•, S→S•a}
I₂: Goto(I₀,a) = {S→a•}
I₃: Goto(I₁,a) = {S→Sa•}

FOLLOW(S) = {$, a}
I₁: S'→S• (reduce on $) and S→S•a (shift on a). Shift/reduce on 'a'? S' → S reduce only on $ ∈ FOLLOW(S'). S→S•a shift on a. → shift on a, reduce on $ → **No conflict → SLR(1)** ✅

**P88.** (1-mark) Operator precedence parsing cannot handle?
→ (a) Unary minus (b) Adjacent non-terminals (c) ε-productions (d) All of above
→ **(d) All of the above** (operator grammar: no ε, no adjacent non-terminals)

**P89.** (2-mark) For input `id + id * id$`, how many shift and reduce actions in SLR(1) parsing?
Using the expression grammar:
Shifts: id, +, id, *, id, total = **5 shifts**
Reduces: F→id, T→FT', E'→ε, T'→ε, ... (trace carefully)

Actually with the standard augmented grammar (E→E+T|T, T→T*F|F, F→(E)|id):
Shifts: id(1), +(2), id(3), *(4), id(5) = 5
Reduces: F→id(1), T→F(2), F→id(3), T→F(4), F→id(5), T→T*F(6), E→T(wait...)

Let me trace: id+id*id$
```
shift id → reduce F→id → reduce T→F → reduce E→T → shift + → shift id → reduce F→id → reduce T→F → shift * → shift id → reduce F→id → reduce T→T*F → reduce E→E+T → accept
```
Shifts: 5 (id, +, id, *, id). Reduces: 6 + accept. **Total: 5 shifts, 6 reduces + accept**

**P90.** (1-mark) Maximum number of handles at any point during parsing?
→ **Exactly 1** — the handle is unique in a rightmost derivation (for LR grammars).

**P91-P100** — True/False:

| # | Statement | T/F |
|---|---|---|
| P91 | Every LL(1) grammar is unambiguous | TRUE |
| P92 | Every unambiguous grammar is LL(1) | FALSE |
| P93 | Every LL(1) grammar is LR(1) | TRUE |
| P94 | Every LR(1) grammar is LL(1) | FALSE |
| P95 | LALR(1) has same states as SLR(1) | TRUE (same as LR(0)) |
| P96 | CLR(1) always has more states than LALR(1) | FALSE (can be equal) |
| P97 | Left recursive grammar can be LR(1) | TRUE (e.g., S → Sa \| a) |
| P98 | Left recursive grammar can be LL(1) | FALSE (never) |
| P99 | Right recursive grammar is always LL(1) | FALSE (may have other issues) |
| P100 | Ambiguous grammar can be LR(k) for some k | FALSE (never) |

**P101-P110** — Operator Precedence:

**P101.** Build precedence table for +, -, *, / with usual precedence (*, / > +, -), all left-associative.

| | + | - | * | / | id | $ |
|---|---|---|---|---|---|---|
| + | ⋗ | ⋗ | ⋖ | ⋖ | ⋖ | ⋗ |
| - | ⋗ | ⋗ | ⋖ | ⋖ | ⋖ | ⋗ |
| * | ⋗ | ⋗ | ⋗ | ⋗ | ⋖ | ⋗ |
| / | ⋗ | ⋗ | ⋗ | ⋗ | ⋖ | ⋗ |
| id | ⋗ | ⋗ | ⋗ | ⋗ | | ⋗ |
| $ | ⋖ | ⋖ | ⋖ | ⋖ | ⋖ | |

**P102.** Parse `id - id * id` using above table:
→ Reduce: id, id, id (each → E), then E*E (higher prec), then E-E → **correct precedence!**

**P103.** Right-associative operator `^` with highest precedence. Table entry for `^ ⋖,⋗ ^`?
→ Right-associative: `^` ⋖ `^` (yield to right occurrence)

**P104.** Can operator precedence parser handle parentheses?
→ **YES** — `(` ⋖ everything, everything ⋗ `)`, `(` ≐ `)`

**P105-P110** — Quick:

| # | Q | A |
|---|---|---|
| P105 | Leading terminal of E in E + T \| T | + (from E + T) |
| P106 | Trailing terminal of E | + (from E + T), and terminals from T |
| P107 | What is handle pruning? | Repeatedly finding and reducing handles |
| P108 | Shift-reduce conflict example | Grammar: S → if E then S \| if E then S else S |
| P109 | Reduce-reduce conflict example | Two complete items in same state |
| P110 | Precedence of operators resolves which conflicts? | Shift-reduce conflicts in ambiguous grammars |

**P111-P120** — SDT and Semantic Analysis:

**P111.** (2-mark) Annotated parse tree for `3 * 5 + 4` with E.val attribute:
```
      E.val = 19
     / | \
  E.val=15 + T.val=4
   / | \      |
  T.val=15  F.val=4
  / | \      |
T.val=3 * F.val=5  4
  |         |
F.val=3    5
  |
  3
```

**P112.** Is `{A.s = B.s + C.s}` for `A → BC` — synthesized or inherited?
→ A.s computed from B.s and C.s (children) → **Synthesized**

**P113.** Is `{B.i = A.i}` for `A → BC` — what kind?
→ B.i from A.i (parent's inherited) → B.i is **Inherited**

**P114.** S-attributed SDD: can be evaluated on? → **Bottom-up parse** (e.g., LR parsing with stack-based evaluation)

**P115.** L-attributed SDD: evaluation order? → **Left-to-right, depth-first traversal**

**P116.** Type widening: `int + float` → result type?
→ `float` (implicit type conversion / coercion)

**P117.** What does type checker verify?
→ Operand types compatible with operator, function call arguments match parameters, assignment types compatible

**P118.** Overloading resolution in `f(3, 4.5)` with `f(int,int)` and `f(int,float)`?
→ Choose `f(int, float)` — exact match on 2nd parameter wins

**P119.** Intermediate code for `a = b[i]` (array access)?
```
t1 = i * elem_size
t2 = b + t1        // address computation (base + offset)
a = *t2            // or a = b[t1] in notation
```

**P120.** 3AC for `a[i] = b + c`?
```
t1 = b + c
t2 = i * elem_size
t3 = a + t2
*t3 = t1           // or a[t2] = t1
```

**P121-P130** — Advanced Parsing:

**P121.** (2-mark) Grammar: S → L = R | R, L → *R | id, R → L. Number of SLR(1) states?
→ This is the classic grammar that is NOT SLR(1). LR(0) automaton has ~10 states. The conflict occurs in the state containing {R → L•} and {S → L• = R}.

**P122.** How does CLR(1) resolve the conflict in P121?
→ [R → L•, $] — reduce only when $ follows (end of input)
→ [S → L• = R, $] — shift = 
→ With exact lookahead {$} for reduce and {=} for shift → **no conflict!**

**P123.** (1-mark) Yacc resolves shift-reduce conflict by?
→ **Shift** (default). Reduce-reduce: choose the production listed first.

**P124.** (2-mark) For grammar with `%left '+' '-'` and `%left '*' '/'` in Yacc, what's the precedence?
→ Later declaration = higher precedence. So `* /` have higher precedence than `+ -`. Left-associative for both.

**P125.** Error production for missing semicolon in C-like language?
```
stmt → expr SEMI
     | expr error SEMI    // recover if ; is missing
```

**P126.** How many reduce actions in parsing "aabb" with S → aSb | ab?
→ S → aSb → aabb (2 derivation steps → 2 reduces in bottom-up parsing)
Trace: shift a, shift a, shift b, reduce S→ab, shift b, reduce S→aSb → **2 reduces**

**P127.** (2-mark) For grammar G: S → AB, A → a, B → b | ε. Strings in L(G)?
→ S → AB → ab | a (if B→ε). **L(G) = {a, ab}**

**P128.** Is the grammar in P127 LL(1)?
→ B → b | ε. FIRST(b) = {b}. FOLLOW(B) = FOLLOW(S) = {$}.
→ {b} ∩ {$} = ∅ → **YES, LL(1)** ✅

**P129.** (NAT) How many entries in LL(1) parsing table for grammar with 4 non-terminals and 6 terminals?
→ Table: 4 rows × (6+1) columns (+$ column) = 4 × 7 = **28 cells**

**P130.** Predictive parser needs how much lookahead for LL(k)? → **k tokens** (LL(1) needs 1 lookahead)

---

## 📝 Practice Set 4: Comprehensive GATE Simulation (P131 – P190)

**P131.** (2-mark) Consider a language L over {a, b}. A lexer for L uses the following token definitions:
- keyword: "if" | "else"  
- identifier: [a-z]+

What token is produced for input "iffy"?
→ Maximal munch: entire "iffy" matches identifier pattern → **ID("iffy")**

**P132.** (1-mark) Preprocessor handles which of the following?
(a) #include (b) #define macros (c) Conditional compilation (d) All of above
→ **(d) All of the above**

**P133.** (2-mark) How many tokens in `x = x+++y;` in C?
→ Maximal munch: x(ID), =(ASSIGN), x(ID), ++(INCR), +(PLUS), y(ID), ;(SEMI)
→ **7 tokens** (interpreted as `x = (x++) + y`)

**P134.** (2-mark) The cross-compiler is defined as?
→ A compiler that runs on platform A but produces code for platform B. Example: compiling ARM code on x86.

**P135.** (1-mark) Bootstrapping in compilers means?
→ Writing a compiler for language L **in language L itself**. First version compiled by another compiler.

**P136.** (2-mark NAT) CFG:
```
S → aS | aSbS | ε
```
Number of parse trees for string "aab"?
→ Leftmost: S → aS → aaSbS → aa..bS. 
Let me enumerate:
1. S → aS → aaSbS → aaεb(S→ε) → aab ✓
2. S → aSbS → a(S→aS→aε)b(S→ε) → aab... wait, that gives aab ✓
So there are **2 parse trees** → grammar is ambiguous.

**P137.** (2-mark) Construct LL(1) table for:
```
S → iEtSS' | a
S' → eS | ε
E → b
```
Is there a conflict?

| | a | b | e | i | t | $ |
|---|---|---|---|---|---|---|
| S | S→a | | | S→iEtSS' | | |
| S' | | | S'→eS, S'→ε | | | S'→ε |
| E | | E→b | | | | |

**M[S', e] has TWO entries → CONFLICT → NOT LL(1)** (dangling else)

**P138.** (1-mark) Yacc specification has three sections separated by? → **%%** (double percent)
```
%{declarations%}
%%
rules
%%
user code
```

**P139.** (2-mark) Lex generates: (a) parser (b) lexer (c) code optimizer (d) linker → **(b) Lexer**

**P140.** (1-mark) YACC generates? → **LALR(1) parser**

**P141-P150** — 3AC Generation:

**P141.** 3AC for `for (i=1; i<=10; i++) a[i] = 0;`
```
    i = 1
L1: if i > 10 goto L2
    t1 = i * 4
    t2 = a + t1
    *t2 = 0
    i = i + 1
    goto L1
L2:
```

**P142.** 3AC for `x = a > b ? a : b;` (ternary operator)
```
    if a > b goto L1
    goto L2
L1: t1 = a
    goto L3
L2: t1 = b
L3: x = t1
```

**P143.** 3AC for `a = b && c || d`
```
    if b goto L1
    goto L2          // b is false → skip c
L1: if c goto L3     // b && c both true
    goto L2
L2: if d goto L3     // try d
    goto L4
L3: a = 1            // true
    goto L5
L4: a = 0            // false
L5:
```

**P144.** Quadruple representation of `t1 = -b; t2 = c + d; t3 = t1 * t2; a = t3`:

| # | Op | Arg1 | Arg2 | Result |
|---|---|---|---|---|
| 0 | uminus | b | | t1 |
| 1 | + | c | d | t2 |
| 2 | * | t1 | t2 | t3 |
| 3 | = | t3 | | a |

**P145.** Triple representation of same:

| # | Op | Arg1 | Arg2 |
|---|---|---|---|
| 0 | uminus | b | |
| 1 | + | c | d |
| 2 | * | (0) | (1) |
| 3 | = | a | (2) |

**P146.** Advantage of triple over quadruple? → **Less space** (no explicit result field)
Disadvantage? → **Cannot reorder** (references by index break). Use indirect triples instead.

**P147.** Number of 3AC instructions for `a = b + c * d - e`?
```
t1 = c * d
t2 = b + t1
t3 = t2 - e
a = t3
```
→ **4 instructions**

**P148.** SSA form of:
```
if (flag) x = 1; else x = 2;
y = x + 3;
```
→ 
```
if (flag) x₁ = 1; else x₂ = 2;
x₃ = φ(x₁, x₂);
y₁ = x₃ + 3;
```

**P149.** What is a φ-function used for?
→ **Merging different definitions** of the same variable at join points in control flow.

**P150.** How many basic blocks in:
```
(1) read n
(2) i = 1
(3) if i > n goto (8)
(4) a[i] = 0
(5) i = i + 1
(6) goto (3)
(7) write a[i]       -- unreachable!
(8) halt
```
Leaders: 1 (first), 3 (target of goto), 4 (after conditional), 7 (after goto), 8 (target)
Wait: leader identification:
- 1: first statement → leader
- 3: target of goto(6) → leader  
- 4: statement after conditional jump(3) → NOT the one right after, actually the fall-through of stmt 3
Hmm, re-read: stmt 3 is `if i > n goto (8)`, so stmt 4 is the fall-through → leader
- 7: statement after unconditional goto(6) → leader
- 8: target of goto in stmt 3 → leader

BB1: {1, 2}, BB2: {3}, BB3: {4, 5, 6}, BB4: {7}, BB5: {8} → **5 basic blocks**

**P151-P170** — Optimization:

**P151.** Identify loop invariant in:
```
L: t1 = a * b      // a, b not modified in loop → INVARIANT
   t2 = t1 + i
   i = i + 1
   if i < n goto L
```
→ `t1 = a * b` is loop invariant → move before loop

**P152.** After code motion:
```
t1 = a * b          // ← moved
L: t2 = t1 + i
   i = i + 1
   if i < n goto L
```

**P153.** Strength reduction in loop: `t = i * 8` where i starts at 0 and increments by 1.
→ Replace with: `t = 0` before loop, `t = t + 8` inside loop.

**P154.** Copy propagation: after `x = y`, replace all uses of x with y (if x not redefined).
```
x = y
z = x + 1    →    z = y + 1
w = x * 2    →    w = y * 2
```

**P155.** Algebraic identity: `x / x` → `1` (if x ≠ 0)

**P156.** Algebraic identity: `x ^ 0` → `1` (for integer power)

**P157.** `x + 0` → `x`, `x * 1` → `x`, `x * 0` → `0` (if no side effects)

**P158.** Loop unrolling: for loop with body B and trip count 100.
→ Unrolled ×4: 25 iterations, each executing B four times. Fewer loop control overhead.

**P159.** Inline expansion: replace function call with function body.
→ Eliminates call/return overhead. Enables further optimization (CSE, constant propagation across call boundary).

**P160.** Tail call optimization: `return f(x+1)` → jump to f with modified parameter (no new frame).

**P161-P170** — Data Flow Quick:

| # | Q | A |
|---|---|---|
| P161 | Forward analysis examples (name 2) | Reaching definitions, Available expressions |
| P162 | Backward analysis examples (name 2) | Live variables, Very busy expressions |
| P163 | Must analysis uses ∩ or ∪? | ∩ (intersection — must hold on ALL paths) |
| P164 | May analysis uses ∩ or ∪? | ∪ (union — may hold on SOME path) |
| P165 | Reaching defs: may or must? | May (∪) |
| P166 | Available exprs: may or must? | Must (∩) |
| P167 | Live variables: may or must? | May (∪ of successors) |
| P168 | Convergence guaranteed by? | Monotone framework + finite lattice |
| P169 | Max iterations for bit-vector analysis on n blocks? | $O(n)$ (bounded by lattice height) |
| P170 | What ensures iterative algorithm terminates? | Transfer functions are monotone + lattice has finite height |

**P171-P190** — Runtime + Code Gen + Mixed:

**P171.** (2-mark) Given:
```
void f() {
    int x = 10;
    void g() {
        int y = x + 1;    // needs x from f's frame
    }
    g();
}
```
How does g access x? 
→ Through **access link** (static link): g's activation record has a pointer to f's activation record. g follows this link to find x.

**P172.** (2-mark) Static scoping example:
```
int x = 1;
void f() { printf("%d", x); }        // which x?
void g() { int x = 2; f(); }
main() { g(); }
```
Output with static scoping → **1** (f sees global x)
Output with dynamic scoping → **2** (f sees g's x on call stack)

**P173.** How many activation records on stack when main→g→f is active?
→ **3** (one for main, one for g, one for f)

**P174.** Tail recursive `fact(n, acc) = if n==0 then acc else fact(n-1, n*acc)` optimized to?
→ Iteration: `while(n>0) { acc = n*acc; n = n-1; } return acc;`
→ Stack depth: $O(1)$ instead of $O(n)$

**P175.** Display array for nesting depth 3: what's D[2]?
→ Pointer to the most recent activation record of a procedure at nesting depth 2.

**P176.** (1-mark) Number of activation records for `fib(5)` (naive recursive)?
→ At peak: fib(5)→fib(4)→fib(3)→fib(2)→fib(1) = **5 frames** on stack simultaneously

**P177.** Register allocation: two variables live at the same time → share a register?
→ **NO** — they interfere. Must have different registers or one must be spilled.

**P178.** Graph coloring with 3 colors: nodes A,B,C,D with edges A-B, B-C, C-D, A-C.
→ A=1, B=2, C=3, D=1 (or D=2). 3-colorable → **3 registers sufficient**

**P179.** Instruction selection: which method does LLVM use?
→ **SelectionDAG** (tree-based pattern matching with dynamic programming)

**P180.** Peephole optimization window size?
→ Typically **2-3 instructions** (small window)

**P181-P190** — Comprehensive MCQ:

| # | Question | Options | Answer |
|---|---|---|---|
| P181 | Phase detecting type mismatch | (a)Lexical (b)Syntax (c)**Semantic** (d)Code Gen | (c) |
| P182 | Which phase creates symbol table entries | (a)**Lexical** (first entry) (b)Syntax (c)Semantic (d)All | (d) all use it, (a) first adds |
| P183 | Parse tree vs AST | Parse tree: all grammar symbols; AST: essential structure only | — |
| P184 | Intermediate code advantage | Machine-independent optimization + easy retargeting | — |
| P185 | Lexer output for `float pi = 3.14;` | float(KW), pi(ID), =(ASSIGN), 3.14(FLOAT), ;(SEMI) → 5 tokens | 5 |
| P186 | What is a sentential form? | Any string derivable from start symbol | — |
| P187 | What is a sentence? | A sentential form with only terminals | — |
| P188 | Regular grammar ⊂ CFG ⊂ CSG ⊂ RE | TRUE (Chomsky hierarchy) | TRUE |
| P189 | Can CFG express `{a^n b^n c^n}`? | NO (context-sensitive) | NO |
| P190 | Bootstrapping: T-diagram shows? | Source lang, Implementation lang, Target lang | — |

---

## 📝 Practice Set 5: GATE Numerical & Tracing (P191 – P250)

**P191.** (2-mark NAT) Grammar:
```
S → aSb | SS | ε
```
How many parse trees are there for the string "aabb"?
→ Derivation 1: S → aSb → aSSb → a(ε)(S→aSb→a(ε)b)b → aabb ✓ Wait, let me be careful.
S → aSb → a(S→aSb→a(S→ε)b)b → a·a·ε·b·b = aabb ✓ (Tree 1)
S → SS → (S→aSb→a(S→ε)b)(S→aSb→a(S→ε)b) → ab·ab = abab ✗ (not aabb)
S → aSb → a(S→SS→(S→ε)(S→aSb→a(ε)b))b → a·ε·ab·b → aabb ✓ (Tree 2)
S → aSb → a(S→SS→(S→aSb→a(ε)b)(S→ε))b → a·ab·ε·b → aabb ✓ (Tree 3)

→ **3 parse trees** → grammar is highly ambiguous for "aabb"

**P192.** (2-mark) Compute FIRST and FOLLOW:
```
S → AaAb | BbBa
A → ε
B → ε
```

FIRST(S) = FIRST(Aa) ∪ FIRST(Bb) = {a} ∪ {b} = {a, b}
FIRST(A) = {ε}, FIRST(B) = {ε}

FOLLOW(A): From S → AaAb: First A: FOLLOW has {a}. Second A: FOLLOW has {b}.
→ **FOLLOW(A) = {a, b}**

FOLLOW(B): From S → BbBa: First B: FOLLOW has {b}. Second B: FOLLOW has {a}.
→ **FOLLOW(B) = {a, b}**

**P193.** (2-mark) Is the grammar in P192 LL(1)?
→ For S → AaAb | BbBa: FIRST(AaAb) = {a}, FIRST(BbBa) = {b}. {a} ∩ {b} = ∅ ✅
→ For A → ε: only one production → OK
→ For B → ε: only one production → OK
→ **YES, LL(1)** ✅

**P194.** (2-mark NAT) LR(0) automaton for S' → S, S → (S) | a. Number of states?

I₀: {S'→•S, S→•(S), S→•a}
I₁: Goto(I₀,S) = {S'→S•}
I₂: Goto(I₀,'(') = {S→(•S), S→•(S), S→•a}
I₃: Goto(I₀,a) = {S→a•}
I₄: Goto(I₂,S) = {S→(S•)}
Goto(I₂,'(') = I₂
Goto(I₂,a) = I₃
I₅: Goto(I₄,')') = {S→(S)•}

→ **6 states**

**P195.** (1-mark) Is the grammar in P194 SLR(1)?
→ No state has conflicts. I₁ has accept on $, I₃ and I₅ have single reduces.
→ **YES, SLR(1)** ✅

**P196.** (2-mark) Parse "(a)" using SLR(1) from P194:

| Stack | Input | Action |
|---|---|---|
| 0 | (a)$ | shift ( → 2 |
| 0 ( 2 | a)$ | shift a → 3 |
| 0 ( 2 a 3 | )$ | reduce S → a → goto(2,S)=4 |
| 0 ( 2 S 4 | )$ | shift ) → 5 |
| 0 ( 2 S 4 ) 5 | $ | reduce S → (S) → goto(0,S)=1 |
| 0 S 1 | $ | accept ✅ |

**Actions: 2 shifts + 2 reduces + accept = 5 actions total**

**P197.** (2-mark) Generate 3AC for:
```
while (a < b) {
    if (c < d) 
        x = y + z;
    else 
        x = y - z;
}
```

```
L1: if a < b goto L2
    goto L5
L2: if c < d goto L3
    goto L4
L3: t1 = y + z
    x = t1
    goto L1
L4: t2 = y - z
    x = t2
    goto L1
L5:
```

**P198.** (1-mark) Number of 3AC instructions in P197? → **Count: 9** (2 conditional jumps + 2 unconditional + 2 assignments + 2 additions + 1 store). Actually: 10 lines of code.

**P199.** (2-mark) Build DAG for:
```
a = b + c
d = b + c
e = d - b
f = a - b
```

- Statement 1: node +(b,c), label: a
- Statement 2: b+c already computed → label same node: a, d
- Statement 3: -(d, b) = -(a, b) since d = a → node -(a,b), label: e
- Statement 4: -(a, b) already computed → label same node: e, f

**DAG has 4 leaf nodes (a, b, c, d/original) and 2 internal nodes**: +(b,c) → labeled a,d and -(a,b) → labeled e,f

**CSE found:** `b + c` (used by both a and d), `a - b` (used by both e and f)

**P200.** (2-mark) How many dead stores in:
```
a = 1        // dead if a is immediately redefined
b = 2        // b is used in line 4
a = 3        // a is used in line 5 (kills previous a=1)
c = b + a    // uses b=2, a=3
return c
```
→ `a = 1` is dead (overwritten by `a = 3` before use). **1 dead store**

**P201-P210** — Liveness & Registers:

**P201.** Basic block:
```
1: t1 = a + b
2: t2 = c + d
3: t3 = e - t2
4: t4 = t1 + t3
```
Live at exit: {t4}

Liveness at each point:
- After 4: {t4}
- Before 4: {t1, t3}
- After 3: {t1, t3}
- Before 3: {t1, e, t2}
- After 2: {t1, e, t2}
- Before 2: {t1, c, d, e}
- After 1: {t1, c, d, e}
- Before 1: {a, b, c, d, e}

Max live variables at any point: **5** (before 1) → need 5 registers ideally

**P202.** Interference graph for P201:
- After stmt 1: t1 live with {c,d,e} → edges: t1-c, t1-d, t1-e
- After stmt 2: t2 live with {t1, e} → edges: t2-t1, t2-e
- After stmt 3: t3 live with {t1} → edge: t3-t1
- t4 doesn't interfere (only one alive at exit)

Graph: t1 connects to c, d, e, t2, t3. t2 connects to t1, e.
Chromatic number: t1 needs unique color, others can share → approximately 3-4 registers

**P203.** If only 2 registers available for P201? → Need to spill. Spill t1 (most interferences) or the variable with longest live range.

**P204-P210** — Quick:

| # | Q | A |
|---|---|---|
| P204 | Register allocation is which type of problem? | Graph coloring (NP-complete in general) |
| P205 | getReg prefers which variable to spill? | One not used soon / has copy in memory |
| P206 | Stack machine needs how many registers? | 0 (all on stack) — but usually 1 (accumulator) |
| P207 | 3-address machine: max operands per instruction? | 3 (two sources + one destination) |
| P208 | 2-address machine: result overwrites? | First operand (e.g., ADD R1, R2 → R1 = R1+R2) |
| P209 | 1-address machine uses? | Accumulator (ACC = ACC op operand) |
| P210 | 0-address machine uses? | Stack (PUSH, POP, operators use stack top) |

---

## 📝 Practice Set 6: Advanced Parsing & Conflict Resolution (P211 – P260)

**P211.** (2-mark) Grammar:
```
S → id | V := E
V → id
E → V | num
```
Show SLR(1) has a conflict.

FOLLOW(V) and FOLLOW(E) both may contain `:=` → problem state where we have {V → id•} and need to decide: reduce to V (anticipating `:=`) or treat as S → id•?

State after shifting id: {S → id•, V → id•}
On `:=`: S → id• would be erroneous (S has no `:=`). V → id• should reduce.
FOLLOW(S) = {$}, FOLLOW(V) = {:=, $}.
On `$`: reduce S → id AND reduce V → id → **reduce-reduce conflict! NOT SLR(1)**

**P212.** How LALR(1)/CLR(1) resolves P211:
→ CLR(1) items: [S → id•, $] and [V → id•, :=]
→ On $: reduce S → id; on :=: reduce V → id → **No conflict!** → CLR(1).

**P213.** (2-mark) Is every LALR(1) grammar also LL(1)?
→ **NO.** Example: S → Sa | a is LALR(1) (left-recursive) but NOT LL(1).

**P214.** (2-mark) Is every LL(1) grammar also SLR(1)?
→ **YES.** LL(1) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1).

**P215.** (1-mark) Grammar hierarchy (from weakest to strongest parsing power):
→ LL(1) < LR(0) ← these are incomparable actually.
Correct: LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊂ LR(1)
And: LL(1) ⊂ LR(1), but LL(1) and LR(0) are incomparable.

**P216.** (2-mark) Ambiguous grammar for expressions: `E → E+E | E*E | id`
Using precedence/associativity in YACC: `%left +`, `%left *` (higher priority for *)
→ Shift-reduce conflicts resolved by: comparing production's precedence with lookahead's precedence.
→ Input `id + id * id`: after reducing id+id, should we shift * or reduce E+E? Since * > +, **shift** → correct!

**P217-P220** — Conflict Types:

| # | Grammar | Conflict Type | In Which Parser |
|---|---|---|---|
| P217 | S → if E then S \| if E then S else S | Shift-reduce | LR(0), SLR |
| P218 | S → aAc \| aBc, A → d, B → d | Reduce-reduce | SLR (FOLLOW overlap) |
| P219 | S → Sa \| a (left-recursive) | None (surprisingly!) | SLR(1) ✅ |
| P220 | S → A \| B, A → aAb \| ε, B → aBb \| ε | Reduce-reduce | LR(0) (both ε-reduce) |

**P221-P230** — Parsing Table Construction:

**P221.** (2-mark NAT) Grammar: S → AB, A → aA | ε, B → b.
Number of SLR(1) parsing table states?

I₀: {S'→•S, S→•AB, A→•aA, A→•} (ε production!)
I₁: Goto(I₀,S) = {S'→S•}
I₂: Goto(I₀,A) = {S→A•B, B→•b}
I₃: Goto(I₀,a) = {A→a•A, A→•aA, A→•}
I₄: Goto(I₂,B) = {S→AB•}
I₅: Goto(I₂,b) = {B→b•}
Goto(I₃,A) = I₆: {A→aA•}
Goto(I₃,a) = I₃ (self-loop)

→ **7 states** (I₀ through I₆)

**P222.** Is the grammar in P221 SLR(1)?
I₀ has {A→•}: reduce A→ε. Lookahead: FOLLOW(A) = FIRST(B) = {b}.
I₀ also has shift on 'a'. No overlap: shift a, reduce on b → **No conflict → SLR(1)** ✅

I₃ has {A→•}: reduce on FOLLOW(A) = {b}. Shift on 'a'. → No conflict ✅

**P223.** (NAT) Total entries in ACTION table for P221: states × (terminals + $) = 7 × 3 = **21**

**P224.** Total entries in GOTO table: states × non-terminals = 7 × 3 = **21** (S, A, B columns)

**P225-P230** — Parsing Trace:

**P225.** Parse "aab" using SLR(1) from P221:

| Stack | Input | Action |
|---|---|---|
| 0 | aab$ | shift a → 3 |
| 0 a 3 | ab$ | shift a → 3 |
| 0 a 3 a 3 | b$ | reduce A → ε (lookahead b ∈ FOLLOW(A)) → goto(3,A)=6 |
| 0 a 3 a 3 A 6 | b$ | reduce A → aA → goto(3,A)=6 |
| 0 a 3 A 6 | b$ | reduce A → aA → goto(0,A)=2 |
| 0 A 2 | b$ | shift b → 5 |
| 0 A 2 b 5 | $ | reduce B → b → goto(2,B)=4 |
| 0 A 2 B 4 | $ | reduce S → AB → goto(0,S)=1 |
| 0 S 1 | $ | **ACCEPT** ✅ |

**P226.** Parse "b" (A derives ε, so S → εB → b):

| Stack | Input | Action |
|---|---|---|
| 0 | b$ | reduce A → ε (b ∈ FOLLOW(A)) → goto(0,A)=2 |
| 0 A 2 | b$ | shift b → 5 |
| 0 A 2 b 5 | $ | reduce B → b → goto(2,B)=4 |
| 0 A 2 B 4 | $ | reduce S → AB → goto(0,S)=1 |
| 0 S 1 | $ | **ACCEPT** ✅ |

**P227-P230** — Error Detection:

**P227.** Parse "aa$" (missing b): After shifting a,a: reduce A→ε on b expected. But input is $! FOLLOW(A)={b}, $ ∉ FOLLOW(A) → **ERROR** at state 3, input $. No valid action.

**P228.** Parse "ba$" (b before a): State 0, input b → reduce A→ε. Then state 2, input b → shift. Then state 5, input a → reduce B→b → goto(2,B)=4. State 4, input a → **ERROR** (S→AB• complete, but $ expected, not a).

**P229.** What type of error? → **Syntax error** — unexpected token after valid S derivation

**P230.** Best recovery? → **Panic mode**: skip 'a' and subsequent tokens until $ or synchronizing token found.

---

## 📝 Practice Set 7: Mixed GATE Simulation (P231 – P300)

**P231.** (1-mark) The handle of the string "aABbcd" w.r.t. grammar S → aABe, A → Abc | b, B → d:
→ First, what rightmost derivation produced this? We need to find which production's RHS matches a suffix of the stack that can be reduced.
→ Looking at "aABbcd": can we reduce anything? Check: does "bcd" match? A → Abc needs Abc. "Bbc" → no. Hmm.
Actually this may not be a valid sentential form. Let me check: S → aABe → aAde → a(A→Abc→bbc)de... 

The proper sentential form would be: S ⇒ aABe ⇒ aAde ⇒... Let me try: rightmost derivation of "abbcde":
S ⇒ aABe ⇒ aAde ⇒ aAbcde ⇒ abbcde. So at "aAbcde", handle = "Abc" (reduce A → Abc).
At "aAde", handle = "d" (reduce B → d).

**P232.** (2-mark) Grammar: E → E + n | n. Parse "n + n + n" bottom-up (leftmost reduction):

| Stack | Input | Action |
|---|---|---|
| | n+n+n$ | shift n |
| n | +n+n$ | reduce E → n |
| E | +n+n$ | shift + |
| E+ | n+n$ | shift n |
| E+n | +n$ | reduce E → E+n |
| E | +n$ | shift + |
| E+ | n$ | shift n |
| E+n | $ | reduce E → E+n |
| E | $ | **ACCEPT** ✅ |

**Left-associative:** (n + n) + n ← reduces left-to-right. **Correct!**

**P233.** (2-mark) Same grammar but right-recursive: E → n + E | n. Parse "n + n + n":

| Stack | Input | Action |
|---|---|---|
| | n+n+n$ | shift n |
| n | +n+n$ | shift + (don't reduce! need to see if E follows) |
| n+ | n+n$ | shift n |
| n+n | +n$ | shift + |
| n+n+ | n$ | shift n |
| n+n+n | $ | reduce E → n |
| n+n+E | $ | reduce E → n+E |
| n+E | $ | reduce E → n+E |
| E | $ | **ACCEPT** ✅ |

**Right-associative:** n + (n + n) ← reduces right-to-left. **Correct!**

**P234.** (1-mark) Left-recursive grammar gives which associativity in bottom-up parsing?
→ **Left-associative** (reduces from left)

**P235.** (1-mark) Right-recursive grammar gives which associativity?
→ **Right-associative** (shifts all then reduces from right)

**P236-P250** — Quick Fire:

| # | Question | Answer |
|---|---|---|
| P236 | Token for `<=` | RELOP (or LE) |
| P237 | Token for `==` | RELOP (or EQ) |
| P238 | Lexer error for `@` in C | Invalid character (not in alphabet) |
| P239 | Syntax error detected at which phase? | Syntax analysis (parsing) |
| P240 | `int float x;` — which phase detects error? | Syntax (invalid token sequence) |
| P241 | `int x = "hello";` — which phase? | Semantic (type mismatch) |
| P242 | Undeclared variable — which phase? | Semantic analysis |
| P243 | Division by zero — which phase? | Runtime (or maybe constant folding at compile time) |
| P244 | `#include` missing file — which phase? | Preprocessor |
| P245 | LL(k) using how many lookahead? | k tokens |
| P246 | LL(1) can handle left recursion? | NO — must eliminate first |
| P247 | LR(1) can handle left recursion? | YES |
| P248 | LALR(1) parser generator tool? | YACC / Bison |
| P249 | Lexer generator tool? | Lex / Flex |
| P250 | Parser combinator is which type? | Top-down (recursive descent) |

**P251-P260** — Optimization Deep:

**P251.** (2-mark) Apply constant folding and propagation:
```
x = 3
y = x + 5
z = y * 2
w = z + x
```
→ After constant propagation: x=3 everywhere
→ y = 3 + 5 = 8 (folding)
→ z = 8 * 2 = 16 (folding)
→ w = 16 + 3 = 19 (folding)
→ **Final: x=3, y=8, z=16, w=19** (all constants!)

**P252.** (2-mark) Given loop:
```
for (i=0; i<100; i++) {
    k = 7;              // invariant
    a[i] = k * i;
}
```
After optimization:
```
k = 7;                  // moved out
t = 0;                  // strength reduction: k*i → increment by k
for (i=0; i<100; i++) {
    a[i] = t;
    t = t + 7;
}
```

**P253.** Inline function `int square(int x) { return x*x; }` called as `y = square(5)`:
→ After inlining: `y = 5 * 5`
→ After constant folding: `y = 25`

**P254.** Tail call: `int f(int n) { if (n==0) return 1; return n * f(n-1); }`
→ Is this tail recursive? **NO!** There's a multiplication after the recursive call.
→ Transform: `int f(int n, int acc) { if (n==0) return acc; return f(n-1, n*acc); }`
→ NOW it's tail recursive! Can be optimized to loop.

**P255-P260** — Peephole:

| # | Before | After | Rule |
|---|---|---|---|
| P255 | `MOV R1, R1` | (delete) | Redundant move |
| P256 | `GOTO L1; L1: GOTO L2` | `GOTO L2` | Jump through jumps |
| P257 | `ADD R1, #0` | (delete) | Algebraic identity |
| P258 | `MUL R1, #2` | `SHL R1, #1` | Strength reduction |
| P259 | `MUL R1, #0` | `MOV R1, #0` | Algebraic simplification |
| P260 | `CMP R1, #0; JEQ L` | `JZ R1, L` | Machine idiom |

---

## 📝 Practice Set 8: Full GATE Paper Simulation (P261 – P330)

**P261.** (2-mark NAT) Consider the grammar:
```
S → Aa | bB
A → b | ε
B → a | ε
```
Number of strings of length exactly 2 in L(G)?
→ S → Aa: A=b → "ba", A=ε → "a" (length 1, not 2)
→ S → bB: B=a → "ba", B=ε → "b" (length 1, not 2)
→ Length 2 strings: "ba" (from both paths). **Answer: 1** unique string of length 2.

**P262.** (2-mark) Which of the following is NOT a context-free language?
(a) $\{a^n b^n | n \geq 0\}$ (b) $\{a^n b^n c^n | n \geq 0\}$ (c) $\{ww^R | w \in \{a,b\}^*\}$ (d) $\{a^n b^m | n \leq m\}$
→ **(b)** requires counting three symbols simultaneously — context-sensitive.

**P263.** (1-mark) Which is more powerful: PDA or DFA?
→ **PDA** (recognizes CFLs ⊃ regular languages)

**P264.** (2-mark) LL(1) parsing table: what does an empty cell indicate?
→ **Syntax error** — the current non-terminal cannot derive a string starting with the current input symbol.

**P265.** (2-mark NAT) Grammar G:
```
S → AB
A → a | ε  
B → bC
C → c | ε
```
Number of strings in L(G)?
→ S → AB → (a|ε)(bC) → (a|ε)(bc|b)
→ Strings: abc, ab, bc, b → **4 strings**

**P266.** (2-mark) In parsing `if a then if b then c else d`, how is "else d" associated?
→ **Nearest unmatched "then"** → `if b then c else d` (inner if-else)
→ Outer: `if a then (if b then c else d)`

**P267.** (2-mark NAT) Code optimization: how many temporary variables after CSE?
```
t1 = a + b
t2 = c * d
t3 = a + b     // = t1
t4 = c * d     // = t2
t5 = t1 + t4   
t6 = t3 + t2   // = t1 + t2 = t5
```
After CSE: t3 = t1, t4 = t2, t6 = t5. **Unique computations: t1, t2, t5 = 3 temporaries needed**

**P268.** (1-mark) What does "bootstrapping" a compiler achieve?
→ The compiler for language L is written in L itself. Self-hosting.

**P269.** (2-mark) T-diagrams: If compiler C₁ translates Java→x86 written in C, and compiler C₂ translates C→x86 written in x86:
→ Run C₂ on the source of C₁ → get a Java→x86 compiler running on x86.
→ This is **cross-compilation** (compile C₁ using C₂).

**P270.** (1-mark) Pass in compilation: complete traversal of source. Multi-pass compiler reads source?
→ **Multiple times** (but can be organized as separate phases feeding each other)

**P271-P290** — Mixed Topics Table:

| # | Topic | Question | Answer |
|---|---|---|---|
| P271 | Lexical | RE for unsigned integers | `[0-9]+` or `digit+` |
| P272 | Lexical | RE for C identifiers | `[a-zA-Z_][a-zA-Z0-9_]*` |
| P273 | Lexical | RE for C float literals | `digit+\.digit+(e[+-]?digit+)?` |
| P274 | Parsing | Chomsky Normal Form: all productions are? | A → BC or A → a (2 non-terminals or 1 terminal) |
| P275 | Parsing | Greibach Normal Form: productions are? | A → aα (terminal first, then non-terminals) |
| P276 | Parsing | CYK algorithm time? | $O(n^3 \|G\|)$ for CNF grammar |
| P277 | Parsing | Earley parser handles? | ALL CFGs in $O(n^3)$, $O(n^2)$ for unambiguous, $O(n)$ for LR |
| P278 | SDT | Marker non-terminal purpose? | Enable bottom-up evaluation of inherited attributes |
| P279 | SDT | Attribute grammar = SDT + ? | No side effects (pure declarative) |
| P280 | ICG | Advantage of SSA form? | Simplifies optimizations (def-use is trivial) |
| P281 | ICG | φ-function placed at? | Join points (where control flow merges) |
| P282 | Opt | Loop-invariant detection test? | Definition's operands: all reaching defs are outside or loop-invariant |
| P283 | Opt | Dominance in CFG: block A dominates B means? | Every path from entry to B passes through A |
| P284 | Opt | Loop header? | Unique entry point (dominator of all loop nodes) |
| P285 | Opt | Natural loop? | Set of nodes where header dominates all, has back edge |
| P286 | Opt | Back edge in DFS of CFG? | Edge from descendant to ancestor → indicates loop |
| P287 | CG | Calling convention: cdecl? | Caller cleans stack, right-to-left params |
| P288 | CG | Calling convention: stdcall? | Callee cleans stack |
| P289 | RE | Sethi-Ullman labeling computes? | Minimum registers needed for expression tree |
| P290 | RE | Activation tree? | Tree of function calls (parent = caller, child = callee) |

**P291-P300** — Classification Questions:

| # | Statement | Phase |
|---|---|---|
| P291 | Converting `count` to token ID | Lexical Analysis |
| P292 | Checking `if (a + b` missing `)` | Syntax Analysis |
| P293 | Checking `int x = "hello"` type error | Semantic Analysis |
| P294 | Converting `a + b * c` to `t1 = b*c; t2 = a+t1` | ICG |
| P295 | Moving `x = y*z` outside loop | Code Optimization |
| P296 | Allocating R1 for variable `x` | Code Generation |
| P297 | Resolving `#include <stdio.h>` | Preprocessor |
| P298 | Converting .o to executable | Linker |
| P299 | Loading executable into memory | Loader |
| P300 | Resolving external function references | Linker |

---

## 🎯 100 Compiler Design Quick-Recall Facts

1. Compiler translates entire program; interpreter executes line by line
2. 6 phases: Lexical → Syntax → Semantic → ICG → Optimization → Code Gen
3. Symbol table and error handler span ALL phases
4. Lexer uses DFA (converted from RE via NFA)
5. Maximal munch: always match longest possible token
6. FIRST(α) = set of terminals that begin strings derivable from α
7. FOLLOW(A) = terminals that can appear right after A
8. Always add $ to FOLLOW(start symbol)
9. LL(1): Left-to-right, Leftmost derivation, 1 lookahead
10. LL(1) condition: FIRST(α) ∩ FIRST(β) = ∅ for each A → α | β
11. If ε ∈ FIRST(α): FIRST(β) ∩ FOLLOW(A) = ∅ required
12. Left recursive grammar is NEVER LL(1)
13. Ambiguous grammar is NEVER LL(k) for any k
14. LR parsing: Left-to-right, Rightmost derivation in reverse
15. LR(0) item: production with dot showing progress
16. Closure: if dot before non-terminal, add its productions
17. Goto: advance dot past a symbol
18. LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1) (parsing power)
19. SLR uses FOLLOW for reduce decisions
20. CLR uses exact lookahead per item
21. LALR = merge CLR states with same core
22. LALR states = SLR states ≤ CLR states
23. LALR can introduce reduce-reduce (NOT shift-reduce) conflicts vs CLR
24. Every LL(1) grammar is LR(1)
25. Not every LR(1) grammar is LL(1) (e.g., left-recursive)
26. YACC/Bison generates LALR(1) parser
27. Lex/Flex generates lexical analyzer
28. Ambiguous grammar → NOT in any LR class
29. Every LR(k) grammar is unambiguous
30. Operator precedence: no ε-productions, no adjacent non-terminals
31. S-attributed: only synthesized → evaluate bottom-up
32. L-attributed: inherited depends on parent/left siblings → left-to-right DFS
33. Every S-attributed is L-attributed
34. 3AC: at most 3 operands per instruction
35. Quadruple: (op, arg1, arg2, result)
36. Triple: (op, arg1, arg2) — result implicit by index
37. SSA: each variable assigned exactly once
38. φ-function merges definitions at control flow joins
39. Basic block: maximal straight-line code sequence
40. Leader: first stmt, jump target, stmt after jump
41. DAG detects common subexpressions automatically
42. Constant folding: evaluate constant expressions at compile time
43. Constant propagation: replace variable with known constant
44. Copy propagation: after x=y, use y instead of x
45. Dead code elimination: remove if result unused
46. CSE: reuse previously computed expression
47. Loop invariant code motion: move outside loop
48. Strength reduction: expensive op → cheaper (multiply → add)
49. Induction variable elimination: remove redundant loop counters
50. Peephole: small window of instructions
51. Reaching definitions: forward, union at joins
52. Available expressions: forward, intersection at joins
53. Live variables: backward, union at joins
54. Very busy expressions: backward, intersection at joins
55. Data flow converges because lattice is finite + transfer is monotone
56. Register allocation ≡ graph coloring (NP-complete)
57. Interference graph: edge = simultaneously live variables
58. k registers → k-coloring needed
59. Spill: store variable in memory when not enough registers
60. Activation record: local vars, saved registers, return addr, control link, access link
61. Static scoping: determined by program text
62. Dynamic scoping: determined by call stack at runtime
63. Access link (static link): points to enclosing scope's activation
64. Control link (dynamic link): points to caller's activation
65. Display: array D[i] → most recent activation at depth i
66. Call-by-value: copy of argument
67. Call-by-reference: address of argument
68. Call-by-name: textual substitution (thunk)
69. Tail recursion → can be optimized to iteration
70. Heap: for dynamic allocation (malloc), managed by GC or manually
71. Backpatching: fill in jump targets later (single-pass)
72. Short-circuit: && and || don't evaluate second operand if unnecessary
73. Error recovery: panic mode (skip to sync token), phrase-level, error productions
74. Cross-compiler: compiles on platform A for platform B
75. Bootstrapping: compiler written in its own source language
76. T-diagram: shows source language, implementation language, target language
77. Multi-pass: better optimization; Single-pass: faster compilation
78. Preprocessor phase 0: handles #include, #define, #ifdef
79. Assembler: assembly → machine code
80. Linker: combines object files + resolves external references
81. Loader: places executable in memory + relocates addresses
82. DFA minimization: merge equivalent states
83. Minimum DFA is unique (Myhill-Nerode theorem)
84. NFA → DFA: subset construction (can have exponential blowup)
85. Thompson's NFA: at most 2×|regex| states
86. Chomsky hierarchy: Regular ⊂ CFL ⊂ CSL ⊂ RE
87. Lexer = DFA = regular; Parser = PDA = context-free
88. Type checking: semantic analysis phase
89. Type coercion: implicit type conversion (widening)
90. Overloading resolution: choose best matching function
91. Intermediate code = machine-independent representation
92. Optimization should not change program behavior (meaning-preserving)
93. SSA simplifies many optimizations (dead code, constant prop)
94. Natural loop: has single entry (header) and a back edge
95. Dominator: A dominates B if every path from entry to B goes through A
96. Immediate dominator: closest dominator
97. Dominator tree: parent = immediate dominator
98. Instruction selection: map IR to machine instructions
99. CGS (Code Generation using Sets): Sethi-Ullman optimal labeling
100. JIT compilation: compile at runtime (V8, JVM HotSpot)

---

## 🔥 40 GATE Traps — Compiler Design

| # | ❌ Common Mistake | ✅ Correct Understanding |
|---|---|---|
| 1 | FIRST includes ε always | ε in FIRST(X) only if X can derive ε |
| 2 | FOLLOW includes ε | FOLLOW **never** contains ε |
| 3 | Forget $ in FOLLOW(S) | ALWAYS add $ to start symbol's FOLLOW |
| 4 | LL(1) handles left recursion | NEVER — must eliminate first |
| 5 | SLR = LALR = CLR in power | SLR < LALR < CLR |
| 6 | SLR has different states than LALR | Same states! (= LR(0) states) |
| 7 | LALR merge creates shift-reduce | Only reduce-reduce conflicts possible |
| 8 | Ambiguous grammar is LR | Ambiguous → NOT in any LR class |
| 9 | Every unambiguous CFG is LL(1) | False — many unambiguous grammars aren't LL(1) |
| 10 | LL(1) and LR(0) same | Incomparable (some grammars are LL(1) not LR(0) and vice versa) |
| 11 | Lexer handles nested comments | Cannot (need PDA for nesting) |
| 12 | Handle = any substring matching RHS | Handle must lead to rightmost derivation in reverse |
| 13 | Viable prefix can contain handle | It can, up to and including handle |
| 14 | Bottom-up builds tree top-down | Bottom-up: leaves → root |
| 15 | S-attributed needs inherited attrs | S-attributed = synthesized ONLY |
| 16 | L-attributed = inherited only | L-attributed can have synthesized too |
| 17 | SDT actions execute during parsing | Depends on parser type (LR: at reduce; LL: at expand) |
| 18 | 3AC has exactly 3 operands always | "At most" 3 — some have fewer (copy: x = y) |
| 19 | Triple better than quadruple always | Triple saves space but harder to reorder |
| 20 | Reaching defs uses intersection | Union (∪) — "may reach" analysis |
| 21 | Available exprs uses union | Intersection (∩) — "must be available" |
| 22 | Live variables is forward analysis | Backward analysis |
| 23 | CSE works across basic blocks always | Local CSE is within block; global needs data flow |
| 24 | Loop invariant: move any invariant code | Must check: definition dominates all uses, not in other paths |
| 25 | Constant folding at runtime | At compile time! |
| 26 | Register allocation is in P | NP-complete (graph coloring) |
| 27 | All variables need registers | Only live variables need registers simultaneously |
| 28 | Static scoping = static allocation | Different concepts entirely |
| 29 | Dynamic scoping = dynamic allocation | Different concepts |
| 30 | Call-by-name = call-by-reference | Different! Call-by-name re-evaluates expression each access |
| 31 | Tail recursion always possible | Only when recursive call is the LAST operation |
| 32 | `n * fact(n-1)` is tail recursive | NOT tail recursive (multiplication after call) |
| 33 | Display array has unlimited size | Size = max nesting depth (bounded) |
| 34 | Linker and loader are same | Different: linker combines, loader loads into memory |
| 35 | Preprocessor is part of compiler | Separate phase (runs before compilation proper) |
| 36 | DFA always has fewer states than NFA | Not necessarily — DFA can have up to $2^n$ states for NFA with $n$ states |
| 37 | Minimum DFA can have multiple solutions | Unique (up to renaming of states) |
| 38 | Grammar with no left recursion is LL(1) | Not necessarily (may need left factoring, or other issues) |
| 39 | Backpatching needs two passes | Single pass! (that's the whole point) |
| 40 | Peephole sees the whole basic block | No — small window (2-3 instructions) |

---

## 📊 Master Formula Card — Compiler Design

### Parsing Hierarchy
```
LL(1) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1) = LR(1)
LL(1) ⊂ LR(1)
LR(0) ⊂ SLR(1)
LL(1) and LR(0) are incomparable
```

### State Counts
```
SLR(1) states = LALR(1) states = LR(0) states
CLR(1) states ≥ LALR(1) states
```

### Thompson's Construction
- Regex of length $n$ → NFA with at most $2n$ states
- NFA → DFA: subset construction → at most $2^n$ states

### DFA Minimization
- Start: 2 partitions (accept, non-accept)
- Each iteration: refine by splitting
- Final: one state per equivalence class

### LL(1) Table Size
- Rows = non-terminals, Columns = terminals + $
- Empty cell = error

### LR Parsing Table Size
- ACTION: states × (terminals + $)
- GOTO: states × non-terminals

### Data Flow Equations

| Analysis | Direction | Confluence | Transfer |
|---|---|---|---|
| Reaching Defs | Forward | ∪ | OUT = GEN ∪ (IN − KILL) |
| Avail Exprs | Forward | ∩ | OUT = GEN ∪ (IN − KILL) |
| Live Vars | Backward | ∪ | IN = USE ∪ (OUT − DEF) |
| Very Busy | Backward | ∩ | IN = USE ∪ (OUT − KILL) |

### Sethi-Ullman Register Labeling
For expression tree node:
- Leaf: label = 1 if left child, 0 if right
- Internal node with children $l₁, l₂$:
  - If $l₁ = l₂$: label = $l₁ + 1$
  - If $l₁ ≠ l₂$: label = $\max(l₁, l₂)$

Root label = minimum registers needed

---

## 🗺️ Compiler Design Topic Dependency Map

```
Lexical Analysis (RE, NFA, DFA)
    ├── Token definition (RE)
    ├── Thompson's (RE → NFA)
    ├── Subset construction (NFA → DFA)
    └── DFA Minimization

Syntax Analysis
    ├── CFG, Derivations, Parse Trees
    ├── Ambiguity, Left Recursion, Left Factoring
    ├── Top-Down: FIRST/FOLLOW → LL(1) → Recursive Descent
    └── Bottom-Up: LR(0) Items → SLR(1) → LALR(1) → CLR(1)
                   └── Operator Precedence

Semantic Analysis
    ├── SDT: S-Attributed, L-Attributed
    ├── Annotated Parse Trees
    └── Type Checking, Coercion

Intermediate Code Generation
    ├── 3AC, Quadruples, Triples
    ├── Boolean expressions (short-circuit)
    ├── Control flow (if, while, for, switch)
    ├── SSA Form (φ-functions)
    └── Backpatching

Code Optimization
    ├── Local: DAG, Peephole
    ├── Global: Data Flow Analysis
    │   ├── Reaching Definitions
    │   ├── Available Expressions
    │   ├── Live Variables
    │   └── Very Busy Expressions
    └── Loop: Invariant Code Motion, Strength Reduction, Induction Variable

Code Generation
    ├── Register Allocation (Graph Coloring)
    ├── Instruction Selection
    └── Runtime Environments
        ├── Activation Records (Stack Frames)
        ├── Parameter Passing (value/reference/name)
        ├── Static vs Dynamic Scoping
        └── Heap Management
```

---

## 🏆 Exam Day Quickcheck — Compiler Design

Before the exam, review these 10 critical points:

1. **FIRST/FOLLOW rules:** ε handling, $ in FOLLOW(S), cascading ε in FIRST
2. **LL(1) condition:** no FIRST-FIRST conflict, no FIRST-FOLLOW conflict
3. **LR hierarchy:** LR(0) ⊂ SLR ⊂ LALR ⊂ CLR. LALR states = SLR states.
4. **Parsing traces:** know how to trace both LL(1) (stack + input) and SLR(1) (shift/reduce)
5. **Data flow:** Forward vs Backward, ∪ vs ∩, GEN/KILL computation
6. **3AC generation:** for if-else, while, boolean expressions, array access
7. **Optimization types:** constant folding, CSE, dead code elimination, loop invariant code motion
8. **S-attributed vs L-attributed:** S = synthesized only = bottom-up; L = left-to-right = top-down
9. **Runtime:** static vs dynamic scoping, call-by-value vs reference vs name
10. **NFA→DFA states:** can blow up to $2^n$; minimum DFA is unique

> 💡 Most GATE compiler design questions test FIRST/FOLLOW computation, LL(1) table, LR parsing traces, and identifying grammar classes. Practice these until you can do them in your sleep!

---

## 🎓 GATE PYQ Distribution — Compiler Design

| Year | Questions | Marks | Key Topics |
|---|---|---|---|
| GATE 2024 | 2-3 | 5-6 | FIRST/FOLLOW, SLR parsing, SDT |
| GATE 2023 | 2-3 | 5-7 | LL(1) table, LR(0) items, 3AC |
| GATE 2022 | 2-3 | 5-6 | FOLLOW sets, LALR vs CLR, optimization |
| GATE 2021 | 2-3 | 5-7 | Parsing traces, data flow, runtime env |
| GATE 2020 | 2-3 | 5-6 | Grammar classification, register alloc |

**Highest frequency topics:**
1. ★★★★★ FIRST and FOLLOW computation
2. ★★★★★ LL(1) / SLR(1) / LALR(1) classification
3. ★★★★☆ Parsing trace (bottom-up shift-reduce)
4. ★★★★☆ LR(0) items / state construction
5. ★★★☆☆ 3AC generation
6. ★★★☆☆ Code optimization identification
7. ★★☆☆☆ Data flow analysis
8. ★★☆☆☆ Runtime environments / scoping

---

## 🔬 Deep Dive: Ambiguity Proofs and Resolution

### Proving a Grammar is Ambiguous

**Method:** Find a string with TWO different leftmost derivations (or parse trees).

**Example:** E → E + E | E * E | id

String: `id + id * id`

**Derivation 1 (+ first):**
```
E ⇒ E + E ⇒ id + E ⇒ id + E * E ⇒ id + id * E ⇒ id + id * id
```

**Derivation 2 (* first):**
```
E ⇒ E * E ⇒ E + E * E ⇒ id + E * E ⇒ id + id * E ⇒ id + id * id
```

Two different leftmost derivations → **AMBIGUOUS** ✅

### Resolving Ambiguity — Precedence & Associativity

**Unambiguous grammar for arithmetic:**
```
E → E + T | T         // + is lowest precedence, left-associative
T → T * F | F         // * is higher precedence, left-associative  
F → ( E ) | id        // grouping has highest precedence
```

**Parse tree for id + id * id:**
```
        E
       / \
      E   +   T
      |      / | \
      T     T  *  F
      |     |     |
      F     F    id₃
      |     |
     id₁   id₂
```

Only ONE parse tree → unambiguous! Multiplication binds tighter (T level) than addition (E level).

### Right-Associative Operator

For exponentiation `^` (right-associative, higher precedence than * and +):

```
E → E + T | T
T → T * P | P
P → F ^ P | F        // right-recursive for right-associativity!
F → ( E ) | id
```

`id ^ id ^ id` parses as `id ^ (id ^ id)` — correct!

### Dangling Else — Unambiguous Grammar

**Ambiguous:**
```
S → if E then S else S | if E then S | other
```

**Unambiguous (every else matches nearest then):**
```
S → matched | unmatched
matched → if E then matched else matched | other
unmatched → if E then S | if E then matched else unmatched
```

---

## 🔬 Deep Dive: LR(0) Automaton — Complete Example with Parsing

### Grammar: E → E + T | T, T → (E) | id

**Augmented:** E' → E

**LR(0) Item Sets:**

**I₀:** Closure({E'→•E})
```
E' → •E
E  → •E + T
E  → •T
T  → •(E)
T  → •id
```

**I₁ = Goto(I₀, E):**
```
E' → E•
E  → E• + T
```
(shift-reduce conflict on +? Accept on $, shift on +.)

**I₂ = Goto(I₀, T):**
```
E → T•
```
(Complete item → reduce)

**I₃ = Goto(I₀, '('):**
```
T → (•E)
E → •E + T
E → •T
T → •(E)
T → •id
```

**I₄ = Goto(I₀, id):**
```
T → id•
```

**I₅ = Goto(I₁, '+'):**
```
E → E +•T
T → •(E)
T → •id
```

**I₆ = Goto(I₃, E):**
```
T → (E•)
E → E• + T
```

**Goto(I₃, T) = I₂** 
**Goto(I₃, '(') = I₃** (self-loop)
**Goto(I₃, id) = I₄**

**I₇ = Goto(I₅, T):**
```
E → E + T•
```

**Goto(I₅, '(') = I₃**
**Goto(I₅, id) = I₄**

**I₈ = Goto(I₆, ')'):**
```
T → (E)•
```

**Goto(I₆, '+') = I₅**

**Total: 9 states (I₀ through I₈)**

### SLR(1) Table

**FOLLOW sets:**
- FOLLOW(E) = {$, +, )}
- FOLLOW(T) = {$, +, )}

**ACTION Table:**

| State | id | + | ( | ) | $ |
|---|---|---|---|---|---|
| 0 | s4 | | s3 | | |
| 1 | | s5 | | | acc |
| 2 | | r2 | | r2 | r2 |
| 3 | s4 | | s3 | | |
| 4 | | r4 | | r4 | r4 |
| 5 | s4 | | s3 | | |
| 6 | | s5 | | s8 | |
| 7 | | r1 | | r1 | r1 |
| 8 | | r3 | | r3 | r3 |

Productions: r1: E→E+T, r2: E→T, r3: T→(E), r4: T→id

**GOTO Table:**

| State | E | T |
|---|---|---|
| 0 | 1 | 2 |
| 3 | 6 | 2 |
| 5 | | 7 |

### Complete Parse Trace: `id + (id + id)`

| Stack | Input | Action |
|---|---|---|
| 0 | id+(id+id)$ | s4 |
| 0 id 4 | +(id+id)$ | r4: T→id, goto(0,T)=2 |
| 0 T 2 | +(id+id)$ | r2: E→T, goto(0,E)=1 |
| 0 E 1 | +(id+id)$ | s5 |
| 0 E 1 + 5 | (id+id)$ | s3 |
| 0 E 1 + 5 ( 3 | id+id)$ | s4 |
| 0 E 1 + 5 ( 3 id 4 | +id)$ | r4: T→id, goto(3,T)=2 |
| 0 E 1 + 5 ( 3 T 2 | +id)$ | r2: E→T, goto(3,E)=6 |
| 0 E 1 + 5 ( 3 E 6 | +id)$ | s5 |
| 0 E 1 + 5 ( 3 E 6 + 5 | id)$ | s4 |
| 0 E 1 + 5 ( 3 E 6 + 5 id 4 | )$ | r4: T→id, goto(5,T)=7 |
| 0 E 1 + 5 ( 3 E 6 + 5 T 7 | )$ | r1: E→E+T, goto(3,E)=6 |
| 0 E 1 + 5 ( 3 E 6 | )$ | s8 |
| 0 E 1 + 5 ( 3 E 6 ) 8 | $ | r3: T→(E), goto(5,T)=7 |
| 0 E 1 + 5 T 7 | $ | r1: E→E+T, goto(0,E)=1 |
| 0 E 1 | $ | **ACCEPT** ✅ |

**Total: 9 shifts, 8 reduces, 1 accept = 18 parser actions**

---

## 🔬 Deep Dive: Lex and YACC Specifications

### Lex (.l file) Structure

```
%{
/* C declarations */
#include <stdio.h>
%}

/* Definitions */
digit    [0-9]
letter   [a-zA-Z]

%%
/* Rules */
"if"            { return IF; }
"else"          { return ELSE; }
"while"         { return WHILE; }
{letter}({letter}|{digit})*  { return ID; }
{digit}+        { return NUM; }
"+"             { return PLUS; }
"*"             { return TIMES; }
"="             { return ASSIGN; }
";"             { return SEMI; }
[ \t\n]         { /* skip whitespace */ }
.               { printf("Unknown char: %c\n", yytext[0]); }
%%

/* User code */
int yywrap() { return 1; }
```

**Key Lex facts:**
- `yytext`: matched string
- `yyleng`: length of matched string
- `yylval`: semantic value
- Patterns matched top-to-bottom (first match wins)
- Longer match preferred (maximal munch)
- `yylex()`: generated scanner function

### YACC (.y file) Structure

```
%{
#include <stdio.h>
extern int yylex();
void yyerror(const char *s);
%}

%token ID NUM IF ELSE WHILE PLUS TIMES ASSIGN SEMI

%left PLUS
%left TIMES

%%
program: stmt_list
stmt_list: stmt_list stmt | stmt
stmt: ID ASSIGN expr SEMI  { printf("Assignment\n"); }
    | IF expr stmt          { printf("If\n"); }
expr: expr PLUS expr        { $$ = $1 + $3; }
    | expr TIMES expr       { $$ = $1 * $3; }
    | ID                    { $$ = lookup($1); }
    | NUM                   { $$ = $1; }
%%

void yyerror(const char *s) { fprintf(stderr, "%s\n", s); }
int main() { return yyparse(); }
```

**Key YACC facts:**
- `$$`: LHS attribute value
- `$1, $2, ...`: RHS symbol attribute values
- `%left`, `%right`, `%nonassoc`: precedence/associativity
- Later declarations have higher precedence
- `yyparse()`: generated parser function
- Default action: `$$ = $1`
- Conflict resolution: shift preferred over reduce (for S/R); first production preferred (for R/R)

---

## 🔬 Deep Dive: Type Systems & Type Checking

### Type Expressions

**Primitive types:** int, float, char, bool, void

**Constructed types:**
- Array: `array(index_type, element_type)` e.g., `array(1..10, int)`
- Record/Struct: `record((field₁, type₁), (field₂, type₂), ...)`
- Pointer: `pointer(type)` e.g., `pointer(int)` = `int*`
- Function: `type₁ × type₂ → type₃` e.g., `int × int → int` for `+`

### Type Checking Rules

```
E → E₁ + E₂
    if E₁.type == int AND E₂.type == int:
        E.type = int
    else if E₁.type == float OR E₂.type == float:
        E.type = float    // widening
    else:
        type_error()

E → E₁[E₂]
    if E₁.type == array(s, t) AND E₂.type == int:
        E.type = t  // element type
    else:
        type_error()

E → *E₁
    if E₁.type == pointer(t):
        E.type = t   // dereferenced type
    else:
        type_error()
```

### Coercion Hierarchy

```
char → int → long → float → double
```

Widening (implicit, safe) vs Narrowing (explicit cast, may lose data)

### Structural vs Name Equivalence

| Feature | Structural | Name |
|---|---|---|
| Types equal if? | Same structure | Same name/declaration |
| C uses? | Structural for most; name for struct/enum | Both |
| Java uses? | Name for classes | Name |
| Example | `typedef int A; typedef int B;` A ≡ B? | Structural: YES; Name: NO (different names) |

---

## 🔬 Deep Dive: Loop Detection and Optimization

### Identifying Natural Loops

**Back edge:** Edge (n, d) where d **dominates** n.

**Natural loop of back edge (n, d):** Set of nodes that can reach n without going through d, plus d.

**Algorithm to find natural loop:**
```
loop = {d}
stack = {n}
while stack not empty:
    m = stack.pop()
    if m not in loop:
        loop.add(m)
        push all predecessors of m onto stack
```

**Example CFG:**
```
B1 → B2
B2 → B3, B4
B3 → B2        (back edge!)
B4 → exit
```

Dominator tree: B1 dom B2, B2 dom B3, B2 dom B4.
Back edge: B3 → B2 (B2 dominates B3 ✅).
Natural loop: Start from B3, find nodes reaching B3 without B2: just B3 itself.
→ Loop = {B2, B3}

### Loop Optimization Checklist

1. **Identify loops** (via back edges in dominator tree)
2. **Find loop invariants** (definitions whose operands are all constants or defined outside loop or are themselves loop-invariant)
3. **Move invariants** outside loop (to preheader)
4. **Identify induction variables** (variables that increment by constant each iteration)
5. **Strength reduce** induction variables (multiply → add)
6. **Eliminate** dead induction variables

### Preheader

A new block inserted before the loop header:
```
Before:
    → B_header → loop body → B_header

After:
    → B_preheader → B_header → loop body → B_header
```

Loop-invariant code is moved to B_preheader.

---

## 📝 Practice Set 9: Cross-Topic Integration (P301 – P370)

**P301.** (2-mark) Given the following 3AC with 2 variables live at exit (a, b):
```
1: c = a + b
2: d = c * 2  
3: a = d + c
4: b = a - d
```
Build interference graph and determine minimum registers.

Live sets (backward):
- After 4: {a, b}
- Before 4: {a, d} (kill b, use a,d)
- After 3: {a, d}
- Before 3: {d, c} (kill a, use d,c)
- After 2: {d, c}
- Before 2: {c} (kill d, use c) [wait, c was used but is it live?]

Actually: Before 2 live = USE(2) ∪ (live_after_2 − DEF(2)) = {c} ∪ ({d,c} − {d}) = {c}
Before 1 live = USE(1) ∪ (live_after_1 − DEF(1)) = {a,b} ∪ ({c} − {c}) = {a,b}

Interference:
- c and d live simultaneously (before/after stmt 2) → c-d edge
- a and d live simultaneously (after stmt 3) → a-d edge
- a and b both live at exit → a-b edge

Graph: a—b, a—d, c—d. Chromatic number: 2 colors? a=1, b=2, d=2, c=1 → YES!
→ **Minimum 2 registers!**

**P302.** (2-mark NAT) How many 3AC instructions for boolean expression `(a < b) && (c > d || e == f)`?

Short-circuit code:
```
1: if a < b goto 2
   goto 6         // entire false (short-circuit &&)
2: if c > d goto 5
   goto 3
3: if e == f goto 5
   goto 6
5: (true target)
6: (false target)
```
→ **6 instructions** (3 conditional jumps + 3 unconditional gotos, but the targets 5,6 aren't full instructions)
→ Counting just the jumps/labels: 3 if-gotos + 3 gotos = **6 3AC instructions**

**P303.** (1-mark) In static single assignment, the number of assignments of variable x equals?
→ **1** (by definition — each variable version is assigned exactly once)

**P304.** (2-mark) Consider:
```
for (i=0; i<n; i++)
    for (j=0; j<n; j++)
        a[i][j] = b[i][j] + c[i][j];
```
How many multiplication operations for address computation per iteration as 3AC?
→ `a[i][j]`: addr = base_a + (i*n + j)*4 → 2 multiplications (i*n, result*4)
→ `b[i][j]`: addr = base_b + (i*n + j)*4 → 2 multiplications
→ `c[i][j]`: addr = base_c + (i*n + j)*4 → 2 multiplications
→ **6 multiplications per inner iteration**

After strength reduction: can reduce to mostly additions → significant speedup!

**P305.** (2-mark) Given grammar:
```
S → (L) | a
L → L, S | S
```
Number of tokens to parse "((a,a),a,(a))"?

Tokens: ( ( a , a ) , a , ( a ) ) → **13 tokens**

**P306.** (1-mark) In a multi-pass compiler, which pass needs the most memory?
→ Usually the **optimization pass** (needs data-flow information for the entire function/program)

**P307-P320** — Fill-in Blank:

| # | Statement | Answer |
|---|---|---|
| P307 | Lexer reads _____ and produces _____ | Characters; Tokens |
| P308 | Parser reads _____ and produces _____ | Tokens; Parse tree |
| P309 | Semantic analyzer produces _____ | Annotated parse tree |
| P310 | ICG produces _____ | Three-address code (or IR) |
| P311 | FIRST(ε) = _____ | {ε} |
| P312 | FOLLOW never contains _____ | ε |
| P313 | ε in FIRST(A) means _____ | A can derive the empty string |
| P314 | SLR resolves reduce by checking _____ | FOLLOW set of LHS non-terminal |
| P315 | CLR resolves reduce by checking _____ | Exact lookahead in the item |
| P316 | In 3AC, `param x` is for _____ | Passing argument x to a function |
| P317 | _____ analysis determines which defs reach a point | Reaching definitions |
| P318 | _____ analysis determines if expression is available | Available expressions |
| P319 | _____ analysis determines if variable is needed later | Live variable (liveness) |
| P320 | Back edge + dominance → _____ | Natural loop |

**P321-P340** — True/False Challenge:

| # | Statement | T/F |
|---|---|---|
| P321 | A parser for regular languages is as powerful as a PDA | FALSE (PDA > DFA) |
| P322 | Context-free grammar can generate all regular languages | TRUE (regular ⊂ CFL) |
| P323 | Every syntactically correct program is semantically correct | FALSE |
| P324 | Phases and passes are the same thing | FALSE (phases = logical; passes = physical traversals) |
| P325 | GOTO in LR table cannot have empty entries | FALSE (many entries empty in GOTO table) |
| P326 | Token, lexeme, and pattern are the same | FALSE |
| P327 | DFA and NFA accept the same class of languages | TRUE (both recognize regular) |
| P328 | NFA is faster to construct from regex | TRUE (Thompson's is direct) |
| P329 | DFA is faster to execute | TRUE (no nondeterminism, O(n) for string of length n) |
| P330 | Converting NFA to DFA always increases states | FALSE (can decrease!) |
| P331 | LR parser is more powerful than LL parser | TRUE (strictly) |
| P332 | Every LL(1) grammar is LR(0) | FALSE (incomparable) |
| P333 | LL(1) grammar can have right recursion | TRUE |
| P334 | Regular expressions can describe C comments | Yes for `/* ... */` (non-nested); No for nested |
| P335 | Dead code can include entire functions | TRUE (if never called) |
| P336 | Loop invariant: `a[i] = 5` where i changes each iteration | NOT invariant (a[i] changes!) |
| P337 | Peephole optimization can change time complexity | Generally NO (just constant factor) |
| P338 | SSA requires more variable names | TRUE (subscripted versions) |
| P339 | φ-function is executed at runtime | NO (resolved during compilation) |
| P340 | Access link is needed with dynamic scoping | NO (just search call stack) |

**P341-P370** — Comprehensive MCQ Bank:

**P341.** (2-mark) Grammar G: S → AB | ε, A → aB, B → bA | ε. Language L(G)?
→ S → AB → aBbA (if B→bA) → ab(bA→...) ... generates strings of even length with alternating a,b patterns.
Actually: S → ε | AB. A → aB → a(ε) = a or a(bA) = ab(A→aB→...).
L(G) includes: ε, a, ab, aba, abab, ... → strings of form (ab)* a? → actually need careful analysis.
S → ε; S → AB → aB(B) where inner B = ε gives A=a, outer B=ε → S → a · ε = a.
A=a, B=bA=ba → S → a·ba = aba. And so on.
**L(G) = (ab)* with possible prefix** → needs more careful derivation for GATE.

**P342.** (1-mark) What is a "reduce" action in bottom-up parsing?
→ Replace the handle (RHS of production) on top of stack with the LHS non-terminal.

**P343.** (2-mark) How many viable prefixes are there for grammar S → aS | b?
→ Viable prefixes: ε, a, aa, aaa, ..., aaab, aab, ab, b + sentential form prefixes with S.
→ **Infinitely many** (a*, a*S, a*b — regular set)

**P344.** (2-mark NAT) Grammar: S → AS | b, A → a. How many LR(0) items total?
→ S' → S, S → AS, S → b, A → a
→ Items: S'→•S, S'→S•, S→•AS, S→A•S, S→AS•, S→•b, S→b•, A→•a, A→a•
→ **9 items**

**P345.** (2-mark) Generate 3AC for `x = -a * b + -a * b`:
Without CSE:
```
t1 = -a
t2 = t1 * b
t3 = -a
t4 = t3 * b  
t5 = t2 + t4
x = t5
```
With CSE:
```
t1 = -a
t2 = t1 * b
t3 = t2 + t2
x = t3
```
→ **CSE saves 2 instructions** (from 6 to 4)

**P346.** (1-mark) Which of these is NOT a peephole optimization?
(a) Redundant instruction elimination (b) Flow-of-control simplification (c) Register allocation (d) Strength reduction
→ **(c) Register allocation** (global optimization, not peephole)

**P347.** (2-mark) Static vs dynamic scoping for:
```
int x = 10;
int f() { return x; }
int g() { int x = 20; return f(); }
print(g());
```
→ Static: f sees global x = 10 → g returns **10**
→ Dynamic: f sees g's x = 20 → g returns **20**

**P348.** (1-mark) In call-by-value-result (copy-restore), when is the final copy-back done?
→ **When the function returns** (the final value of the formal parameter is copied back to the actual parameter)

**P349.** (2-mark) Difference between call-by-reference and call-by-value-result for `swap(a, b)`:
→ Call-by-reference: changes visible immediately (both params are aliases)
→ Call-by-value-result: works on local copies, copies back at end → **same result for swap**

But `swap(x, x)` with call-by-value-result: copies x to both a and b, swaps locally (a=old_x, b=old_x → after swap: a=old_x, b=old_x → copy back: x=old_x. **No change!**

With call-by-reference: both params alias x → temp=x, x=x, x=temp → **No change either!** (Both wrong for swap(x,x))

**P350.** (2-mark) Activation tree for:
```
main() calls f(3)
f(3) calls f(2) then g(1)
f(2) calls f(1)
f(1) returns
```

```
        main
          |
        f(3)
       /    \
    f(2)    g(1)
      |
    f(1)
```

Max stack depth: main → f(3) → f(2) → f(1) = **4 frames**

**P351-P360** — Mixed Quick:

| # | Q | A |
|---|---|---|
| P351 | What is a thunk? | Code + environment for lazy evaluation of argument (call-by-name) |
| P352 | What is closure? | Function + its referencing environment |
| P353 | Static link depth for accessing variable k nesting levels up? | Follow k static links |
| P354 | Maximum display array size = ? | Maximum nesting depth of procedures |
| P355 | What is left sentinel in lexer? | Special character placed at end of input buffer |
| P356 | Buffer pairs in lexer: two buffers of size? | N each (e.g., 4096 chars) to handle long tokens |
| P357 | Keyword vs identifier: how to distinguish? | Reserved word table lookup after DFA match |
| P358 | Two-buffer scheme: when to reload? | When forward pointer reaches end of current buffer |
| P359 | What is attribute grammar? | CFG + attributes + semantic rules (no side effects) |
| P360 | Circular dependency in attribute grammar? | No valid evaluation order exists → error |

**P361.** (2-mark NAT) For the grammar:
```
E → T + E | T
T → F * T | F
F → id
```
(Note: right-recursive — right-associative operators)

LL(1)? Check for T → F * T | F:
After left factoring: T → FT', T' → *T | ε
FIRST(*T) = {*}, FIRST(ε) = {ε}, FOLLOW(T') = FOLLOW(T)
FOLLOW(T) = {+, $, )} → FIRST(*T) ∩ FOLLOW(T') = {*} ∩ {+,$,)} = ∅ ✅

Similarly E → TE', E' → +E | ε. FIRST(+E) ∩ FOLLOW(E') = {+} ∩ {$,)} = ∅ ✅
→ **YES, LL(1) after left factoring** 

Is original grammar (without left factoring) LL(1)?
T → F*T | F: FIRST(F*T) = {id}, FIRST(F) = {id} → overlap! → **NOT LL(1)** without left factoring.

**P362.** (2-mark) Is the grammar `S → aSa | bSb | a | b` LL(1)?
→ For S → aSa | a: FIRST(aSa) = {a}, FIRST(a) = {a} → conflict! → **NOT LL(1)**
(Cannot distinguish without seeing past 'a')

**P363.** (2-mark) For the same grammar, is it LL(2)?
→ With 2 lookahead: 'a' followed by 'a','b' or '$':
  - If 'aa' or 'ab': S → aSa (recursive)
  - If 'a$': S → a (base case)
→ **YES, LL(2)** (2 lookahead tokens suffice)

**P364.** (2-mark NAT) Three-address code: how many temporaries for `(a + b) * (c + d) - (a + b)`?
```
t1 = a + b
t2 = c + d
t3 = t1 * t2
t4 = a + b     // but this is = t1 (CSE!)
t5 = t3 - t4
```

Without CSE: **5 temporaries**. With CSE: t4 = t1, so `t4 = t3 - t1` → **3 temporaries** {t1, t2, t3}

**P365.** (1-mark) Minimum number of registers to evaluate expression tree `(a + b) * (c + d)`?
→ Sethi-Ullman: Left subtree (a+b): label 2 (both operands are leaves). Right subtree (c+d): label 2.
Root (*): both equal → 2+1 = 3? Actually: evaluate one subtree first using 2 regs, store, evaluate other using 2 regs, combine → **Need 2 registers** (evaluate a+b into R1, evaluate c+d into R2, multiply R1*R2).

Wait, Sethi-Ullman for `(a+b)*(c+d)`:
- Left child (+): left=a(1), right=b(0) → max(1,0+1)=1. Actually label(a)=1, label(b)=0 for left/right leaves.
- For node a+b: labels 1 and 0 → max(1, 0+1) = 1
Hmm, this depends on convention. Standard: leaf label = 1 if leftmost, 0 otherwise.

Actually the standard algorithm: leaves get 1 if left child, 0 if right. But for this tree shape:
```
    *
   / \
  +   +
 / \ / \
a  b c  d
```
a=1,b=0 → (+)=max(1,1)=1; c=1,d=0 → (+)=max(1,1)=1; root(*)=max(1,1)+1=2? No, equal children → result+1: 1+1=2.
→ **2 registers needed**

**P366-P370** — Speed Round:

| # | Q | A |
|---|---|---|
| P366 | What tool generates a parser from a grammar? | YACC / Bison |
| P367 | What tool generates a scanner from regex patterns? | Lex / Flex |
| P368 | `yytext` in Lex contains? | The matched lexeme (string) |
| P369 | `$$` in YACC refers to? | Attribute value of LHS non-terminal |
| P370 | Conflict: `%left '+'; %left '*'` for `id+id*id`: What happens? | * has higher precedence → shift * over reducing + → correct parse |

---

## 📝 Practice Set 10: Final Stretch — Speed Drills (P371 – P440)

**P371-P390** — One-line answers:

| # | Question | Answer |
|---|---|---|
| P371 | Minimum DFA states for accepting strings of length divisible by 3 over {a,b}? | 3 states (count mod 3) |
| P372 | Minimum DFA states for `{w | w ends with "01"}` over {0,1}? | 3 states |
| P373 | Regular expression equivalent to (a+b)* ? | (a*b*)* or (a|b)* |
| P374 | Can every DFA be converted to a regular expression? | YES (Arden's theorem or state elimination) |
| P375 | Number of DFA states for union of L₁ (m states) and L₂ (n states)? | At most m × n (product construction) |
| P376 | Is intersection of two CFLs always a CFL? | NO (context-free not closed under intersection) |
| P377 | Is union of two CFLs always a CFL? | YES (context-free closed under union) |
| P378 | Left factoring changes the language? | NO (same language, different productions) |
| P379 | Left recursion elimination changes the language? | NO (same language — but parse tree changes!) |
| P380 | What is augmented grammar? | Grammar with added production S' → S for LR parsing |
| P381 | What is the kernel of an LR item set? | Items with dot not at leftmost position (+ initial item S'→•S) |
| P382 | Non-kernel items are always? | Products of closure (dot at start: A → •α) |
| P383 | What is shift-reduce parsing also called? | Bottom-up parsing / LR parsing |
| P384 | What is top-down parsing also called? | Predictive/recursive descent / LL parsing |
| P385 | Expression trees: inorder traversal gives? | Infix expression (with implicit parentheses) |
| P386 | Expression trees: preorder traversal gives? | Prefix expression (Polish notation) |
| P387 | Expression trees: postorder traversal gives? | Postfix expression (Reverse Polish notation) |
| P388 | Global CSE requires which analysis? | Available expressions |
| P389 | Dead code elimination requires which analysis? | Live variable analysis |
| P390 | Constant propagation requires which analysis? | Reaching definitions |

**P391-P410** — Sorting Compiler Phases:

Order the following actions in compiler pipeline:

**P391.** Token generation → Parse tree → Type checking → 3AC → Optimized code → Machine code
→ **Correct order!** (Lexical → Syntax → Semantic → ICG → Optimization → Code Gen)

**P392.** Match phase to output:

| Phase | Primary Output |
|---|---|
| Lexical Analyzer | Token stream |
| Syntax Analyzer | Parse tree (or AST) |
| Semantic Analyzer | Annotated AST + type info |
| ICG | Three-address code / IR |
| Code Optimizer | Optimized IR |
| Code Generator | Assembly / machine code |

**P393-P410** — Error Phase Identification:

| # | Error | Detected By |
|---|---|---|
| P393 | `int 123abc = 5;` (invalid identifier starting with digit) | Lexer |
| P394 | `int x = ;` (missing expression) | Parser (syntax error) |
| P395 | `int x = "hello" + 5;` | Semantic (type error: string + int) |
| P396 | `x = 5;` (x undeclared) | Semantic (symbol table lookup fails) |
| P397 | `break;` outside loop | Semantic (context-dependent check) |
| P398 | Division by zero: `x = 5/0;` | Could be compile-time (constant folding) or runtime |
| P399 | Array out of bounds | Runtime (most languages) |
| P400 | Stack overflow | Runtime |
| P401 | Missing `#include` file | Preprocessor |
| P402 | Undefined external function | Linker |
| P403 | Multiple definitions of same function | Linker |
| P404 | Missing main() function | Linker |
| P405 | `int f(int x, float x)` duplicate parameter | Semantic |
| P406 | `return "hello"` from `int f()` | Semantic (type mismatch) |
| P407 | Infinite recursion | Runtime (stack overflow) |
| P408 | Memory leak | Runtime (resource issue) |
| P409 | Dangling pointer access | Runtime (undefined behavior) |
| P410 | `goto` to non-existent label | Semantic (label not found) |

**P411-P430** — Optimization Identification:

Given the transformation, identify the optimization:

| # | Before | After | Optimization |
|---|---|---|---|
| P411 | `x = 3 + 5;` | `x = 8;` | Constant folding |
| P412 | `x = 5; y = x + 1;` | `y = 6;` | Constant propagation + folding |
| P413 | `x = y; z = x + 1;` | `z = y + 1;` | Copy propagation |
| P414 | `x = a + b; (x unused)` | (removed) | Dead code elimination |
| P415 | `t1 = a+b; t2 = a+b;` | `t1 = a+b; t2 = t1;` | CSE |
| P416 | Loop: `t = a*b;` (invariant) | Move before loop | Loop invariant code motion |
| P417 | Loop: `t = i * 8;` | `t += 8;` | Strength reduction |
| P418 | `goto L1; L1: goto L2;` | `goto L2;` | Peephole (jump through jumps) |
| P419 | `MOV R1, R1;` | (removed) | Peephole (redundant load) |
| P420 | `x = x * 1;` | (removed) | Algebraic identity |
| P421 | Dead function never called | (removed) | Dead code elimination |
| P422 | `if (false) { ... }` | (entire block removed) | Dead code / unreachable code |
| P423 | Small function `f()` replaced inline | Body substituted at call site | Function inlining |
| P424 | `x = x + x;` | `x = x << 1;` | Strength reduction |
| P425 | Tail-recursive `f(n) = f(n-1)` | Loop | Tail call optimization |
| P426 | Loop body executed 4× per iteration | Fewer iterations | Loop unrolling |
| P427 | Two separate loops with same bounds | One loop with combined body | Loop fusion |
| P428 | One loop doing two things | Two separate loops | Loop fission |
| P429 | Nested loop: inner invariant moved? | To outer loop (not before function!) | Partial code motion |
| P430 | Entire if-branch dead | Branch + condition removed | Dead branch elimination |

**P431-P440** — Complexity Table:

| # | Compiler Phase/Operation | Time Complexity |
|---|---|---|
| P431 | Thompson's NFA construction | $O(|r|)$ where r = regex |
| P432 | Subset construction (NFA→DFA) | $O(2^n)$ worst case |
| P433 | DFA minimization | $O(n \log n)$ (Hopcroft's) |
| P434 | DFA string recognition | $O(|w|)$ (one pass) |
| P435 | LL(1) parsing | $O(n)$ (linear in input length) |
| P436 | SLR/LALR/CLR parsing | $O(n)$ (linear) |
| P437 | CYK parsing (general CFG) | $O(n^3)$ |
| P438 | Earley parsing (general CFG) | $O(n^3)$ worst, $O(n)$ for LR grammars |
| P439 | Data flow analysis (iterative) | $O(n^2)$ for bit-vector (n = basic blocks) |
| P440 | Graph coloring (register allocation) | NP-complete (exponential worst) |

---

## 🧩 Commonly Confused Pairs — Compiler Design

| Pair | Key Difference |
|---|---|
| Token vs Lexeme | Token = category (ID, NUM); Lexeme = actual string ("count", "42") |
| NFA vs DFA | NFA: multiple transitions + ε; DFA: exactly one transition per (state, symbol) |
| FIRST vs FOLLOW | FIRST: what CAN start derivation; FOLLOW: what CAN appear after |
| LL vs LR | LL: top-down, leftmost; LR: bottom-up, rightmost in reverse |
| SLR vs CLR | SLR: uses FOLLOW for reduce; CLR: uses exact lookahead |
| LALR vs CLR | LALR: merged CLR states; fewer states but can lose precision |
| Synthesized vs Inherited | Synth: from children; Inh: from parent/left siblings |
| S-Attributed vs L-Attributed | S: synth only + bottom-up; L: synth + restricted inherited + left-to-right |
| Quadruple vs Triple | Quad: explicit result field; Triple: implicit by index |
| Token vs Pattern | Token = category name; Pattern = regex defining the category |
| Phase vs Pass | Phase = logical stage; Pass = physical traversal of source |
| Compiler vs Interpreter | Compiler: whole program → executable; Interpreter: line by line |
| Static vs Dynamic Scoping | Static: by text structure; Dynamic: by call stack at runtime |
| Access link vs Control link | Access: to enclosing scope (static chain); Control: to caller (dynamic chain) |
| Call-by-ref vs Call-by-name | Ref: address passed; Name: expression re-evaluated each access |
| Lexeme vs Pattern | Lexeme = specific string; Pattern = rule (regex) matching many lexemes |
| Handle vs Viable prefix | Handle: specific RHS to reduce; Viable prefix: valid stack content |
| Back edge vs Cross edge | Back: to dominator (loop indicator); Cross: to non-dominator/non-descendant |
| CSE vs Copy propagation | CSE: reuse computed expression; Copy prop: after x=y, use y for x |
| Dead code vs Unreachable code | Dead: computed but unused; Unreachable: control never reaches it |
| Constant folding vs propagation | Folding: evaluate const expr; Propagation: substitute known const |

---

## 🏆 Final Revision Roadmap — Compiler Design

### Week Before Exam
- Day 7: Regular expressions, NFA→DFA, DFA minimization
- Day 6: CFG, ambiguity, left recursion/factoring, FIRST/FOLLOW (practice 5+ grammars)
- Day 5: LL(1) parsing table construction + traces
- Day 4: LR(0) items, SLR(1) table construction + parsing traces
- Day 3: LALR vs CLR, operator precedence parsing
- Day 2: SDT, 3AC generation, optimization (CSE, dead code, loop opts)
- Day 1: Data flow analysis, runtime environments, quick recall

### Night Before Exam
1. Review 40 GATE traps table
2. Review parser hierarchy: LR(0) ⊂ SLR ⊂ LALR ⊂ CLR
3. FOLLOW never contains ε. Always $ in FOLLOW(S).
4. SLR states = LALR states. LALR merge → reduce-reduce only.
5. Data flow directions: Reaching defs (forward, ∪), Avail expr (forward, ∩), Live vars (backward, ∪)
6. Sleep!

---

## 📝 Practice Set 11: Advanced Parsing — Full Constructions (P441 – P510)

**P441.** (2-mark) Construct FIRST and FOLLOW for:
```
S → ABc
A → a | ε
B → b | ε
```

FIRST:
- FIRST(A) = {a, ε}
- FIRST(B) = {b, ε}
- FIRST(S) = FIRST(A) − {ε} ∪ FIRST(B) − {ε} ∪ {c} = {a, b, c}
  (Because A can be ε → look at B; B can be ε → look at c)

FOLLOW:
- FOLLOW(S) = {$}
- FOLLOW(A) = FIRST(B) − {ε} ∪ FIRST(c) = {b, c}
  (ε ∈ FIRST(B) → also add FIRST(c) = {c}, already there)
- FOLLOW(B) = FIRST(c) = {c}

Verification: S → ABc with A,B both ε → S →* c. FIRST(S) contains c ✅

**P442.** (2-mark) Construct LL(1) parsing table for above grammar.

| | a | b | c | $ |
|---|---|---|---|---|
| S | S→ABc | S→ABc | S→ABc | |
| A | A→a | A→ε | A→ε | |
| B | | B→b | B→ε | |

Parsing for input "ac":
| Stack | Input | Action |
|---|---|---|
| $S | ac$ | S→ABc |
| $cBA | ac$ | A→a |
| $cBa | ac$ | match a |
| $cB | c$ | B→ε |
| $c | c$ | match c |
| $ | $ | ACCEPT ✅ |

**P443.** (2-mark) Is the following grammar LL(1)?
```
S → iEtSS' | a
S' → eS | ε
E → b
```
(Dangling else grammar: i=if, E=expr, t=then, S=stmt, e=else)

Check S' → eS | ε:
FIRST(eS) = {e}, FOLLOW(S') = FOLLOW(S) = {e, $}
FIRST(eS) ∩ FOLLOW(S') = {e} ∩ {e, $} = {e} ≠ ∅ → **NOT LL(1)** ❌

**P444.** (2-mark) Construct SLR(1) for: S → CC, C → cC | d

Augmented: S' → S

**I₀:** S'→•S, S→•CC, C→•cC, C→•d
**I₁ = Goto(I₀,S):** S'→S•
**I₂ = Goto(I₀,C):** S→C•C, C→•cC, C→•d
**I₃ = Goto(I₀,c):** C→c•C, C→•cC, C→•d
**I₄ = Goto(I₀,d):** C→d•
**I₅ = Goto(I₂,C):** S→CC•
**Goto(I₂,c) = I₃**
**Goto(I₂,d) = I₄**
**I₆ = Goto(I₃,C):** C→cC•
**Goto(I₃,c) = I₃** (self-loop!)
**Goto(I₃,d) = I₄**

**Total: 7 states (I₀ through I₆)**

FOLLOW(S) = {$}, FOLLOW(C) = {c, d, $}

ACTION/GOTO Table:

| State | c | d | $ | S | C |
|---|---|---|---|---|---|
| 0 | s3 | s4 | | 1 | 2 |
| 1 | | | acc | | |
| 2 | s3 | s4 | | | 5 |
| 3 | s3 | s4 | | | 6 |
| 4 | r3 | r3 | r3 | | |
| 5 | | | r1 | | |
| 6 | r2 | r2 | r2 | | |

Productions: r1: S→CC, r2: C→cC, r3: C→d

**Parse "cdcd":**
| Stack | Input | Action |
|---|---|---|
| 0 | cdcd$ | s3 |
| 0c3 | dcd$ | s4 |
| 0c3d4 | cd$ | r3: C→d, goto(3,C)=6 |
| 0c3C6 | cd$ | r2: C→cC, goto(0,C)=2 |
| 0C2 | cd$ | s3 |
| 0C2c3 | d$ | s4 |
| 0C2c3d4 | $ | r3: C→d, goto(3,C)=6 |
| 0C2c3C6 | $ | r2: C→cC, goto(2,C)=5 |
| 0C2C5 | $ | r1: S→CC, goto(0,S)=1 |
| 0S1 | $ | **ACCEPT** ✅ |

**P445.** (2-mark) Why does this SLR fail but CLR(1) succeeds?

Grammar: S → L=R | R, L → *R | id, R → L

SLR analysis:
- In state with I₂: S→L•=R, R→L•
- ACTION[I₂, =]: shift (S→L•=R)
- Reduce R→L• on FOLLOW(R) = {=, $} → ACTION[I₂, =]: reduce!
- → **Shift-reduce conflict!**

CLR(1) analysis:
- Item R→L•, but with specific lookahead $
- Only reduce on $, not on =
- No conflict! → **CLR(1) handles it**

This is the **classic GATE example** proving SLR < CLR.

**P446.** (2-mark NAT) For grammar S → AA, A → aA | b:
Number of CLR(1) states?

CLR(1) items include lookahead. Let's construct:

**I₀:** S'→•S,$; S→•AA,$ ; A→•aA,a/b ; A→•b,a/b
**I₁ = Goto(I₀,S):** S'→S•,$ 
**I₂ = Goto(I₀,A):** S→A•A,$ ; A→•aA,$ ; A→•b,$
**I₃ = Goto(I₀,a):** A→a•A,a/b ; A→•aA,a/b ; A→•b,a/b
**I₄ = Goto(I₀,b):** A→b•,a/b
**I₅ = Goto(I₂,A):** S→AA•,$
**I₆ = Goto(I₂,a):** A→a•A,$ ; A→•aA,$ ; A→•b,$
**I₇ = Goto(I₂,b):** A→b•,$
**I₈ = Goto(I₃,A):** A→aA•,a/b
**Goto(I₃,a) = I₃** (self-loop)
**Goto(I₃,b) = I₄**
**I₉ = Goto(I₆,A):** A→aA•,$
**Goto(I₆,a) = I₆** (self-loop)
**Goto(I₆,b) = I₇**

**Total CLR(1) states: 10** (I₀ through I₉)
**LALR states: 7** (I₃,I₆ merge; I₄,I₇ merge; I₈,I₉ merge)

**P447.** (2-mark) Which items can be merged in LALR from above?
→ I₃ and I₆: same core {A→a•A}, different lookahead {a/b} vs {$} → merge to {a/b/$}
→ I₄ and I₇: same core {A→b•}, different lookahead {a/b} vs {$} → merge to {a/b/$}
→ I₈ and I₉: same core {A→aA•}, different lookahead {a/b} vs {$} → merge to {a/b/$}
→ **3 pairs merged, 10 → 7 states**

**P448.** (2-mark) Can the LALR merge above introduce new conflicts?
→ Check merged states: no reduce-reduce conflicts in merged states (each only has one reduce item).
→ **No new conflicts introduced** → LALR = CLR for this grammar.

**P449.** (2-mark) In operator precedence parsing: given precedence:
```
+ ⋖ *  (+ yields to *)
* ⋗ +  (* takes over +)
id ⋗ +  (id takes over +)
id ⋗ *  (id takes over *)
+ ⋖ id
* ⋖ id
+ ⋖ (
* ⋖ (
( ⋖ +, ( ⋖ *, ( ⋖ id, ( ⋖ (
) ⋗ +, ) ⋗ *, ) ⋗ ), ) ⋗ $
+ ⋗ ), * ⋗ ), id ⋗ )
+ ⋗ $, * ⋗ $, id ⋗ $
$ ⋖ +, $ ⋖ *, $ ⋖ id, $ ⋖ (
```

Parse `id * (id + id)`:

| Stack | Relation | Input |
|---|---|---|
| $ | ⋖ | id * (id + id)$ |
| $id | ⋗ | * (id + id)$ → reduce id to E |
| $E | ⋖ | * (id + id)$ |
| $E* | ⋖ | (id + id)$ |
| $E*( | ⋖ | id + id)$ |
| $E*(id | ⋗ | + id)$ → reduce id to E |
| $E*(E | ⋖ | + id)$ |
| $E*(E+ | ⋖ | id)$ |
| $E*(E+id | ⋗ | )$ → reduce id to E |
| $E*(E+E | ⋗ | )$ → reduce E+E to E |
| $E*(E | ≐ | )$ → shift |
| $E*(E) | ⋗ | $ → reduce (E) to E |
| $E*E | ⋗ | $ → reduce E*E to E |
| $E | | $ → **ACCEPT** ✅ |

**P450.** (2-mark) Determine FIRST and FOLLOW for:
```
S → ABCDE
A → a | ε
B → b | ε
C → c
D → d | ε
E → e | ε
```

FIRST(S) = {a, b, c} (A→ε → B→ε → C must produce c)
FIRST(A) = {a, ε}
FIRST(B) = {b, ε}
FIRST(C) = {c}
FIRST(D) = {d, ε}
FIRST(E) = {e, ε}

FOLLOW(S) = {$}
FOLLOW(E) = FOLLOW(S) = {$}
FOLLOW(D) = (FIRST(E) − {ε}) ∪ FOLLOW(S) = {e, $}
FOLLOW(C) = (FIRST(D) − {ε}) ∪ FOLLOW(D) = {d, e, $}
FOLLOW(B) = (FIRST(C)) = {c}
FOLLOW(A) = (FIRST(B) − {ε}) ∪ FIRST(C) = {b, c}

**P451-P460** — Data Flow Analysis Practice:

**P451.** (2-mark) Given the following CFG (basic blocks B1–B4):
```
B1: d1: x = 5
    d2: y = 1
    → B2

B2: d3: z = x + y  
    if z < 10 → B3
    else → B4

B3: d4: x = x + 1
    d5: y = y * 2
    → B2

B4: print(z)
```

**Reaching Definitions Analysis (forward, ∪):**

GEN and KILL:
- B1: GEN = {d1, d2}, KILL = {d4, d5} (d4 redefines x, d5 redefines y)
- B2: GEN = {d3}, KILL = {} (z not redefined elsewhere... well d3 redefines z)
- B3: GEN = {d4, d5}, KILL = {d1, d2} (d4 kills d1, d5 kills d2)
- B4: GEN = {}, KILL = {}

Dataflow equations: OUT[B] = GEN[B] ∪ (IN[B] − KILL[B]); IN[B] = ∪ OUT[pred(B)]

**Iteration 1:**
- IN[B1] = {} → OUT[B1] = {d1, d2}
- IN[B2] = OUT[B1] ∪ OUT[B3] = {d1, d2} ∪ {} = {d1, d2}
  OUT[B2] = {d3} ∪ ({d1, d2} − {}) = {d1, d2, d3}
- IN[B3] = OUT[B2] = {d1, d2, d3}
  OUT[B3] = {d4, d5} ∪ ({d1, d2, d3} − {d1, d2}) = {d3, d4, d5}
- IN[B4] = OUT[B2] = {d1, d2, d3}
  OUT[B4] = {d1, d2, d3}

**Iteration 2:**
- IN[B2] = OUT[B1] ∪ OUT[B3] = {d1, d2} ∪ {d3, d4, d5} = {d1, d2, d3, d4, d5}
  OUT[B2] = {d3} ∪ {d1, d2, d3, d4, d5} = {d1, d2, d3, d4, d5}
- IN[B3] = {d1, d2, d3, d4, d5}
  OUT[B3] = {d4, d5} ∪ ({d1, d2, d3, d4, d5} − {d1, d2}) = {d3, d4, d5}
- IN[B4] = {d1, d2, d3, d4, d5} (changed!)
  OUT[B4] = {d1, d2, d3, d4, d5}

**Iteration 3:** No change → converged!

**Result: Definitions reaching B4 = {d1, d2, d3, d4, d5}** — all definitions reach!

**P452.** (2-mark) Available Expressions Analysis for same CFG:

AE equations: OUT[B] = GEN[B] ∪ (IN[B] − KILL[B]); IN[B] = ∩ OUT[pred(B)]
Initialize OUT to ALL expressions (for ∩ to work: universal set).

Expressions: {x+y, x+1, y*2, z<10}

GEN_AE and KILL_AE:
- B1: GEN = {}, KILL = {x+y, x+1} (x,y redefined)
- B2: GEN = {x+y}, KILL = {z<10}? No — GEN = {x+y, z<10}, KILL = {} (z=x+y generates x+y before killing the old z expression)
- B3: GEN = {x+1, y*2}, KILL = {x+y} (x redefined kills x+y)
- B4: GEN = {}, KILL = {}

At IN[B2] = OUT[B1] ∩ OUT[B3]:
- After convergence: the expression x+y is NOT available at B2 entry because B3 kills it.
→ **x+y is not a common subexpression at B2 entry** (not available from both paths)

**P453.** (2-mark) Live Variable Analysis for same CFG (backward, ∪):

Equations: IN[B] = USE[B] ∪ (OUT[B] − DEF[B]); OUT[B] = ∪ IN[succ(B)]

USE and DEF:
- B1: USE = {}, DEF = {x, y}
- B2: USE = {x, y}, DEF = {z}
- B3: USE = {x, y}, DEF = {x, y}
- B4: USE = {z}, DEF = {}

**Iteration 1:**
- OUT[B4] = {} → IN[B4] = {z}
- OUT[B2] = IN[B3] ∪ IN[B4]
- IN[B3] = {x, y} ∪ (OUT[B3] − {x, y}) = {x, y} (USE includes x,y)
  OUT[B3] = IN[B2] (unknown yet, init to {})
  so IN[B3] = {x, y}
- OUT[B2] = {x, y} ∪ {z} = {x, y, z}
  IN[B2] = {x, y} ∪ ({x, y, z} − {z}) = {x, y}
- OUT[B3] = IN[B2] = {x, y}
  IN[B3] = {x, y} ∪ ({x, y} − {x, y}) = {x, y} ← no change
- OUT[B1] = IN[B2] = {x, y}
  IN[B1] = {} ∪ ({x, y} − {x, y}) = {} ← nothing live at entry! Makes sense, all vars are defined in B1.

**Converged in 2 iterations.**

At B4 entry: z is live. x, y are dead (not used after).
At B3 entry: x, y are live (used by B2 after looping back).

**P454-P460** — Quick DFA Drills:

| # | Question | Answer |
|---|---|---|
| P454 | Reaching definitions flow direction? | **Forward** |
| P455 | Available expressions flow direction? | **Forward** |
| P456 | Live variables flow direction? | **Backward** |
| P457 | Very busy expressions flow direction? | **Backward** |
| P458 | Reaching defs meet operator? | **Union** (∪) — may reach |
| P459 | Available expressions meet operator? | **Intersection** (∩) — must be available |
| P460 | Live variables meet operator? | **Union** (∪) — needed on any path |

---

## 📝 Practice Set 12: Runtime & Code Generation (P461 – P530)

**P461.** (2-mark) Draw the activation record for:
```c
int f(int a, int b) {
    int x, y;
    x = a + b;
    return x;
}
```

Activation record (bottom to top of stack):
```
+-------------------+  ← High address
| Return value      |
| Actual parameter b|
| Actual parameter a|
| Return address    |
| Control link (old BP) |
| Local var x       |
| Local var y       |
| Temporaries       |
+-------------------+  ← Low address (top of stack = SP)
```

Size: 2 params + return addr + control link + 2 locals + return val = **7 slots** (minimum)

**P462.** (2-mark) For nested functions:
```
procedure A {
    int x;
    procedure B {
        int y;
        procedure C {
            // accesses x from A
        }
    }
}
```

To access x from C:
- C's nesting depth = 3, A's nesting depth = 1
- Difference = 2 → Follow **2 access links** (static links)
- C → B (1 link) → A (2 links) → find x in A's activation record

**Display method:** Display[1] always points to most recent activation of depth-1 procedure.
C reads Display[1] directly → **O(1) access** (no chain following).

**P463.** (2-mark) Call by name vs call by need:

```
int i = 1;
int A[3] = {10, 20, 30};

void f(name int x) {
    i = 2;
    print(x);    // x is "A[i]" → evaluates to A[2] = 30 (not A[1]!)
}

f(A[i]);
```

**Call by name:** A[i] re-evaluated each time → after i=2, x = A[2] = **30**
**Call by need (lazy):** A[i] evaluated once on first access, cached → first access after i=2 → A[2] = **30**, second access → cached **30**
**Call by value:** A[i] evaluated at call → A[1] = **20**, i change has no effect

**P464.** (2-mark) Generate code using 2 registers (R0, R1) for: `t1 = a + b; t2 = c + d; t3 = t1 * t2`

```
MOV a, R0      // R0 = a
ADD b, R0      // R0 = a + b = t1
MOV c, R1      // R1 = c
ADD d, R1      // R1 = c + d = t2
MUL R1, R0     // R0 = t1 * t2 = t3
MOV R0, t3     // store result
```
→ **6 instructions**, 2 registers used

What if only 1 register? Need memory spills:
```
MOV a, R0
ADD b, R0      // R0 = t1
MOV R0, t1     // spill t1 to memory
MOV c, R0
ADD d, R0      // R0 = t2
MUL t1, R0     // R0 = t1 * t2
MOV R0, t3
```
→ **7 instructions** (1 extra for spill)

**P465.** (2-mark) Sethi-Ullman labeling for: `(a + b) * (c - (d / e))`

Expression tree:
```
        *
       / \
      +    -
     / \  / \
    a   b c   /
             / \
            d   e
```

Labels:
- a=1, b=0 → (+)= max(1,0+1) = 1
- d=1, e=0 → (/)= max(1,0+1) = 1
- c=1, (/)=1 → equal → (-)= 1+1 = 2
- (+)=1, (-)=2 → unequal → (*)= max(1,2) = **2 registers needed**

Optimal evaluation order: evaluate right subtree (c - d/e) first (needs 2 regs), then left subtree.

```
MOV d, R0
DIV e, R0      // R0 = d/e
MOV c, R1
SUB R0, R1     // R1 = c - d/e
MOV a, R0
ADD b, R0      // R0 = a + b
MUL R1, R0     // R0 = result
```
→ **7 instructions**, 2 registers, 0 spills!

**P466.** (2-mark) Code improvement through DAG:

Basic block:
```
t1 = a + b
t2 = c * d
t3 = a + b    ← same as t1!
t4 = t1 - t3  ← t1 - t1 = 0!
t5 = t2 + t4  ← t2 + 0 = t2!
```

DAG construction eliminates t3 (same as t1), simplifies t4 to 0, t5 to t2:
```
t1 = a + b
t2 = c * d
t5 = t2        ← simplified!
```
→ **Reduced from 5 to 2 essential computations**

**P467-P480** — Scoping Problems:

**P467.** (2-mark) What prints? Static scoping:
```
int x = 1;
void f() { printf("%d", x); }
void g() { int x = 2; f(); }
main() { g(); }
```
→ f() sees global x → prints **1** (static/lexical scoping)

**P468.** Same with dynamic scoping:
→ f() sees g's x on call stack → prints **2** (dynamic scoping)

**P469.** (2-mark) Static scoping:
```
int x = 10;
void f() { x = x + 5; }
void g() { int x = 20; f(); printf("%d", x); }
main() { g(); printf("%d", x); }
```
→ Static: f modifies global x → global x becomes 15.
→ g prints its local x = **20**. main prints global x = **15**.
→ Output: **20 15**

**P470.** Same with dynamic scoping:
→ Dynamic: f sees g's x on call stack → x(g's) = 20+5 = 25.
→ g prints x = **25**. main prints global x = **10** (unchanged).
→ Output: **25 10**

**P471-P475** — Parameter Passing:

**P471.** (2-mark) Given:
```
void f(int x, int y) { x = x + 1; y = y + x; }
int a = 5, b = 10;
f(a, b);
print a, b;
```

| Method | After f() | a | b |
|---|---|---|---|
| Call by value | Changes to x,y lost | 5 | 10 |
| Call by reference | x=&a, y=&b → a=6, b=16 | 6 | 16 |
| Call by value-result | copies: x=5→6, y=10→16, copy back | 6 | 16 |

**P472.** (2-mark) What about `f(a, a)` with call-by-reference?
```
void f(int x, int y) { x = x + 1; y = y + x; }
f(a, a);  // both x and y alias a!
```
→ x = a+1 = 6 (a becomes 6); y = y+x = 6+6 = 12 (a becomes 12)
→ **a = 12**

With call-by-value-result?
→ copies: x=5, y=5. x=5+1=6, y=5+6=11. Copy back: a=x=6 then a=y=11? Or a=y=11 then a=x=6?
→ **Order-dependent!** (Implementation-defined behavior)

**P473.** (1-mark) Call-by-name is also called?
→ **Textual substitution** (replace parameter name with actual argument text)

**P474.** (2-mark) Swap problem with call-by-name:
```
void swap(name int a, name int b) {
    int temp = a; a = b; b = temp;
}
int i = 1;
int A[3] = {10, 20, 30};
swap(i, A[i]);
```

With call-by-name, a="i", b="A[i]":
- temp = i = 1
- i = A[i] = A[1] = 20 → now i = 20!
- A[i] = temp → A[20] = 1 → **Buffer overflow / wrong index!**

→ Call-by-name can cause **Jensen's device** problems — the index changes mid-swap!

**P475.** (1-mark) Which parameter passing avoids aliasing?
→ **Call by value** (copies made, no aliasing possible)

**P476-P480** — Heap Management:

| # | Q | A |
|---|---|---|
| P476 | Mark-and-sweep GC: what is "mark" phase? | Trace all reachable objects from roots, mark them as "alive" |
| P477 | Mark-and-sweep: what is "sweep" phase? | Free all unmarked objects (unreachable = garbage) |
| P478 | Reference counting: main weakness? | Cannot detect **cycles** (A→B→A, both ref count > 0 but garbage) |
| P479 | Copying GC: divides heap into? | Two semispaces: fromSpace and toSpace |
| P480 | Generational GC key insight? | Most objects die young → collect young generation more frequently |

**P481-P510** — Grammar/Language Classification:

| # | Language/Grammar | Classification |
|---|---|---|
| P481 | {aⁿbⁿ | n ≥ 0} | CFL, not regular |
| P482 | {aⁿbⁿcⁿ | n ≥ 0} | CSL, not CFL |
| P483 | {ww | w ∈ {a,b}*} | CSL, not CFL |
| P484 | {ww^R | w ∈ {a,b}*} | CFL (palindromes) |
| P485 | Regular expression → DFA? | Always possible (via Thompson's + subset construction) |
| P486 | CFG → PDA? | Always possible (empty stack or final state PDA) |
| P487 | Can LL parser handle left-recursive grammar? | NO (infinite loop!) |
| P488 | Can LR parser handle left-recursive grammar? | YES (natural for LR) |
| P489 | Can LL parser handle right-recursive grammar? | YES (no issues) |
| P490 | Is every regular grammar LL(1)? | YES (after possible left factoring) |
| P491 | Is every LL(1) grammar LR(1)? | YES (LL(1) ⊂ LR(1)) |
| P492 | Is every LR(0) grammar LL(1)? | NO (incomparable — LR(0) can have left recursion) |
| P493 | Is every unambiguous grammar LR(1)? | NO (even unambiguous grammars may not be LR(1)) |
| P494 | Is every LR(1) grammar unambiguous? | YES (all LR(k) grammars are unambiguous) |
| P495 | Regex → CFG? | Always (regular ⊂ context-free) |
| P496 | CFG → Regex? | Only if the CFL is also regular |
| P497 | Can LL(k) parse LL(k+1)? | NO (strictly more powerful with more lookahead) |
| P498 | All LL(k) combined = all LR(1)? | NO (∪ₖLL(k) ⊂ LR(1)) |
| P499 | Is the grammar S → SS | a ambiguous? | YES (aa: S→SS→aS→aa or S→SS→Sa→aa but these give same tree... actually S→SS: (SS)S vs S(SS) for aaa → YES ambiguous) |
| P500 | Can ambiguous grammar be LR(k) for any k? | NO (ambiguous → not LR(k) for any k) |

**P501-P510** — Expression Code Generation:

**P501.** (2-mark) Generate 3AC for: `a = b + c * d - e / f`

Respecting operator precedence (* and / before + and -):
```
t1 = c * d
t2 = e / f
t3 = b + t1
t4 = t3 - t2
a = t4
```
→ **5 three-address instructions**

**P502.** (2-mark) Convert to quadruples:

| # | Op | Arg1 | Arg2 | Result |
|---|---|---|---|---|
| 0 | * | c | d | t1 |
| 1 | / | e | f | t2 |
| 2 | + | b | t1 | t3 |
| 3 | - | t3 | t2 | t4 |
| 4 | = | t4 | | a |

**P503.** (2-mark) Convert to triples:

| # | Op | Arg1 | Arg2 |
|---|---|---|---|
| 0 | * | c | d |
| 1 | / | e | f |
| 2 | + | b | (0) |
| 3 | - | (2) | (1) |
| 4 | = | a | (3) |

**P504.** (2-mark) Convert to indirect triples:
Statement list: [0] → triple 0, [1] → triple 1, ... (indirection allows reordering)

**P505.** (2-mark) 3AC for `if (a > b && c < d) x = 1; else x = 2;`

Short-circuit:
```
    if a > b goto L1
    goto L3            // short-circuit: a > b false → skip c < d
L1: if c < d goto L2
    goto L3            // c < d false → else
L2: x = 1
    goto L4
L3: x = 2
L4: (next statement)
```
→ **8 instructions** (2 conditional, 2 unconditional jumps, 2 assignments, 2 labels as targets)

With backpatching:
- truelist of (a>b) = {line 0 target}
- falselist of (a>b) = {line 1 target} → backpatch to L3 when we know it
- truelist of (c<d) = {line 2 target} → backpatch to L2
- For &&: backpatch truelist(a>b) to beginning of c<d test

**P506.** (2-mark) Generate 3AC for while loop:
```
while (a < b) {
    a = a + 1;
}
```
```
L1: if a < b goto L2
    goto L3
L2: t1 = a + 1
    a = t1
    goto L1
L3: (after while)
```
→ **5 instructions**

**P507.** (2-mark) Generate 3AC for switch:
```
switch(x) {
    case 1: y = 10; break;
    case 2: y = 20; break;
    default: y = 0;
}
```

Method 1 (sequential comparison):
```
    if x == 1 goto L1
    if x == 2 goto L2
    goto L3          // default
L1: y = 10
    goto L4
L2: y = 20
    goto L4
L3: y = 0
L4: (after switch)
```

Method 2 (jump table — for dense cases):
```
    if x < 1 goto L3
    if x > 2 goto L3
    goto JumpTable[x]   // indirect jump
L1: y = 10; goto L4
L2: y = 20; goto L4
L3: y = 0
L4: ...
```

**P508.** (2-mark) 3AC for array access: `x = A[i][j]` (row-major, A is 10×10 of ints, 4 bytes each)

```
t1 = i * 10       // row offset
t2 = t1 + j       // linear index
t3 = t2 * 4       // byte offset
t4 = addr(A) + t3 // address
x = *t4            // indirect load
```
→ **5 instructions, 2 multiplications**

After strength reduction in a loop (i changes by 1 each iteration):
`t1 += 10` instead of `t1 = i * 10` → multiplication → addition!

**P509.** (1-mark) What is a basic block leader?
→ First instruction, target of jump, instruction after jump → **3 rules**

**P510.** (2-mark) Identify basic blocks in:
```
1: i = 1
2: j = 1
3: t1 = 10 * i
4: t2 = t1 + j
5: t3 = 8 * t2
6: t4 = t3 - 88
7: a[t4] = 0.0
8: j = j + 1
9: if j <= 10 goto 3
10: i = i + 1
11: if i <= 10 goto 2
12: i = 1
```

Leaders: 1 (first), 2 (target of goto 11), 3 (target of goto 9), 10 (after conditional 9), 12 (after conditional 11)

Basic blocks:
- B1: {1} (just instruction 1, but actually 1-2 share no jumps... Let me re-check)
  - Leader 1 → B1 starts at 1
  - Leader 2 → B2 starts at 2 (target of goto at 11)
  - Actually instruction 2 is a target, so B1 = {1}, B2 = {2}, B3 = {3-9}, B4 = {10-11}, B5 = {12}

Wait: Leaders are 1, 2, 3, 10, 12.
- B1: {1} → goes to B2
- B2: {2} → goes to B3
- B3: {3,4,5,6,7,8,9} → if true → B3 (loop!), else → B4
- B4: {10, 11} → if true → B2, else → B5
- B5: {12, ...}

→ **5 basic blocks**

---

## 📝 Practice Set 13: GATE Predicted Questions (P511 – P580)

**P511.** (2-mark) A lexer uses maximum munch. For input `ifx = 10`, what is the first token?
→ "ifx" matches as identifier (not "if" keyword + "x") because maximal munch picks longest match → **Token: ID("ifx")**

**P512.** (2-mark) For which grammar is the number of SLR states equal to CLR states?
→ When no states need to be split for lookahead. Example: **S → a** has 2 states in both SLR and CLR.
→ General answer: when the SLR grammar has no inadequate states requiring lookahead-based splitting.

**P513.** (2-mark NAT) For S → aAd | bBd | aBe | bAe; A → c; B → c:
Is this SLR(1)?

LR(0) items — in the state with A→c• and B→c•:
FOLLOW(A) = {d, e}, FOLLOW(B) = {d, e}
FOLLOW(A) ∩ FOLLOW(B) = {d, e} ≠ ∅ → **reduce-reduce conflict!**
→ **NOT SLR(1)** but IS CLR(1) (lookahead distinguishes)

**P514.** (2-mark) Minimum number of passes needed for a compiler?
→ **1 pass** is theoretically possible (single-pass compiler, e.g., Pascal was designed for this)
→ But most modern compilers use 2+ passes for optimization

**P515.** (2-mark) A language has keywords that are not reserved. How does the lexer handle `if = 5`?
→ Without reserved words, "if" could be an identifier → `if` is an ID → assignment is valid!
→ Parser/semantic phase must resolve based on context.
→ In FORTRAN (early versions): keywords were not reserved, leading to: `IF(IF) = THEN` being valid!

**P516.** (2-mark NAT) Given DFA with 5 states, how many entries in the transition table?
→ |states| × |alphabet| = 5 × |Σ|. For binary alphabet: 5 × 2 = **10 entries**

**P517.** (1-mark) In bottom-up parsing, a "handle" is?
→ A substring that matches the RHS of a production AND whose reduction leads to a valid rightmost derivation in reverse.

**P518.** (2-mark) What is the difference between canonical LR and look-ahead LR?
→ CLR: full item sets with individual lookaheads → more states, no merge
→ LALR: merge CLR states with same core (differ only in lookahead) → fewer states, possible new R/R conflicts

**P519.** (2-mark) Given the SDT:
```
E → E₁ + T  { E.val = E₁.val + T.val }
E → T       { E.val = T.val }
T → T₁ * F  { T.val = T₁.val * F.val }
T → F       { T.val = F.val }
F → (E)     { F.val = E.val }
F → digit   { F.val = digit.lexval }
```

Evaluate for `3 + 5 * 2`:
- F.val = 3, T.val = 3, E₁.val = 3
- F.val = 5, T₁.val = 5
- F.val = 2, T.val = 5*2 = 10
- E.val = 3 + 10 = **13**

**P520.** (2-mark) Given inherited attribute SDT:
```
D → TL     { L.type = T.type }
T → int    { T.type = integer }
T → float  { T.type = real }
L → L₁, id { L₁.type = L.type; addtype(id.entry, L.type) }
L → id     { addtype(id.entry, L.type) }
```

Input: `int a, b, c`
- T.type = integer
- L.type = integer (inherited from D)
- Process: L → L₁, id(c) → L₁ → L₂, id(b) → L₂ → id(a)
- All of a, b, c get type "integer" ✅

This is **L-attributed** (inherited attributes from left siblings and parent only).

**P521-P540** — True/FALSE Advanced:

| # | Statement | T/F | Explanation |
|---|---|---|---|
| P521 | LL(1) can have both left and right recursion | FALSE (can have right, not left!) |
| P522 | LALR(1) may have more R/R conflicts than SLR(1) | FALSE (LALR merges CLR, never adds S/R; may add R/R) |
| P523 | Every LR(0) grammar is also LL(1) | FALSE (LR(0) can be left-recursive) |
| P524 | SLR is always a proper subset of CLR | TRUE (SLR ⊂ LALR ⊂ CLR strictly) |
| P525 | Removing unreachable productions changes the language | NO (unreachable → no effect on language) |
| P526 | Removing useless non-terminals may change the grammar | YES (grammar changes, language preserved) |
| P527 | Intermediate code is machine-independent | TRUE (that's the purpose) |
| P528 | Three-address code has at most 3 operands per instruction | TRUE (by definition) |
| P529 | SSA makes CSE trivial | TRUE (just check if same expression already assigned to some var) |
| P530 | Induction variable elimination can change loop behavior | NO (preserves semantics) |
| P531 | A reduced grammar can still be ambiguous | TRUE (reduced ≠ unambiguous) |
| P532 | Every DCFL has an LR(1) grammar | TRUE |
| P533 | Peephole optimization looks at the entire program | FALSE (sliding window of instructions) |
| P534 | Register allocation uses graph coloring | TRUE (interference graph) |
| P535 | Spilling means storing a register value to memory | TRUE |
| P536 | A basic block can have multiple entry points | FALSE (single entry, exits can be multiple?) Actually: single entry by definition |
| P537 | A basic block always ends with a branch | NO (can fall through to next block) |
| P538 | Loop unrolling always improves performance | FALSE (can increase code size → cache issues) |
| P539 | Type inference is always decidable | Not always (depends on type system; Hindley-Milner: decidable) |
| P540 | A bootstrapped compiler compiles itself | TRUE |

**P541-P560** — MCQ Rapid:

**P541.** Number of items in LR(0) for grammar with P productions of total length L?
→ Each production A → α of length |α| gives |α|+1 items (dot positions 0 through |α|)
→ Total items = L + P (sum of (|αᵢ|+1) = L + P)

**P542.** After left factoring A → αβ₁ | αβ₂, new productions?
→ A → αA', A' → β₁ | β₂ → **2 new productions replacing 2 old ones**

**P543.** Thompson's NFA for regex `a*` has how many states?
→ NFA for `a`: 2 states. Then Kleene star adds 2 more → **4 states** (with ε transitions for loop + bypass)

**P544.** Thompson's NFA for `(a|b)*` has how many states?
→ NFA(a): 2, NFA(b): 2, union(a|b): 2+2+2=6, Kleene star: 6+2 = **8 states**

**P545.** Subset construction worst case: NFA with n states → DFA with up to?
→ $2^n$ states (exponential blowup) — though typically much less in practice

**P546.** What token does `3.14e-2` represent?
→ **Floating-point literal** (scientific notation: 3.14 × 10⁻² = 0.0314)

**P547.** In flex/lex, `.` matches what?
→ Any character **except newline** (unless %option dotall or s flag)

**P548.** What happens if shift-reduce conflict and no precedence declared?
→ Default: **shift** is preferred (in YACC/Bison)

**P549.** What happens if reduce-reduce conflict in YACC?
→ Default: first production (listed earlier in grammar file) is chosen for reduction

**P550.** T-diagram: compiler written in language A, running on machine B, translating language X to Y is written as?
→ X→Y in A, running on B → T-diagram with X on left arm, Y on right arm, A at bottom of T

**P551.** Cross-compiler?
→ A compiler that runs on machine A but generates code for machine B (A ≠ B)

**P552.** Self-hosting compiler?
→ A compiler written in the language it compiles (e.g., C compiler written in C)

**P553.** Bootstrapping process?
→ Write minimal compiler in assembly → use it to compile a slightly better compiler → repeat until full compiler

**P554.** Frontend vs Backend?
→ Frontend: lexer + parser + semantic (language-dependent, machine-independent)
→ Backend: code gen + optimization (language-independent, machine-dependent)

**P555.** Which optimization is NP-complete?
→ **Optimal register allocation** (graph coloring with k colors)

**P556.** What is Sethi-Ullman algorithm for?
→ Optimal code generation for expression trees with n registers (minimizes spills)

**P557.** What is Ershov number?
→ Another name for the Sethi-Ullman label (minimum registers for evaluation)

**P558.** What is a "use-def chain"?
→ Links each USE of a variable to all its reaching DEFinitions (used in data flow)

**P559.** What is a "def-use chain"?
→ Links each DEFinition to all its USEs (used for dead code elimination: if def has no uses → dead)

**P560.** φ-function in SSA is placed where?
→ At the **merge point** (join point) of control flow where different definitions of same variable meet

---

## 📝 Practice Set 14: Marathon Set (P561 – P660)

**P561.** (2-mark) Given: FIRST(A) = {a, ε}, FIRST(B) = {b, ε}, FIRST(C) = {c}
For production S → ABC, what is FIRST(S)?
→ FIRST(ABC) = {a} ∪ (since ε ∈ FIRST(A)) {b} ∪ (since ε ∈ FIRST(B)) {c}
→ **FIRST(S) = {a, b, c}**
Note: ε ∉ FIRST(S) because C cannot derive ε.

**P562.** (2-mark) FIRST(S) if S → ABCD, FIRST(A)={a,ε}, FIRST(B)={b,ε}, FIRST(C)={c,ε}, FIRST(D)={d}?
→ {a, b, c, d} (chain through all nullable non-terminals until D which can't produce ε)
ε ∉ FIRST(S) since D cannot derive ε.

**P563.** (2-mark) FIRST(S) if S → ABCD, all four A,B,C,D can derive ε?
→ {a, b, c, d, ε} — ε included because ALL can derive ε → **S can derive ε**

**P564.** (1-mark) How many items in augmented grammar: S'→S, S→AB, A→a, B→b?
→ S'→•S, S'→S• (2) + S→•AB, S→A•B, S→AB• (3) + A→•a, A→a• (2) + B→•b, B→b• (2) = **9 items total**

**P565.** (1-mark) How many LR(0) states for same grammar?
Items:
I₀: S'→•S, S→•AB, A→•a
I₁: S'→S•
I₂: S→A•B, B→•b
I₃: A→a•
I₄: S→AB•
I₅: B→b•
→ **6 states**

**P566-P580** — Fill-in-the-blank Advanced:

| # | Blank | Answer |
|---|---|---|
| P566 | In LR parsing, the stack holds alternating _____ and _____ | Symbols (terminals/non-terminals) and states |
| P567 | Accept action in LR: stack has _____ and input is _____ | S' → S on stack (state 0, S, accept state); input = $ |
| P568 | The action "sj" means _____ | Shift input symbol and push state j |
| P569 | The action "rk" means _____ | Reduce by production k |
| P570 | Goto(Iᵢ, X) means _____ | Transition from state Iᵢ on grammar symbol X |
| P571 | Closure adds items with _____ | Dot before a non-terminal (A → α•Bβ → add B → •γ) |
| P572 | A grammar is in CNF if every production is _____ | A → BC or A → a (two non-terminals or single terminal) |
| P573 | A grammar is in GNF if every production is _____ | A → aα (terminal followed by non-terminals) |
| P574 | Left factor needed when two productions share _____ | A common prefix |
| P575 | Top-down parser can be implemented as _____ | Recursive descent or table-driven predictive parser |
| P576 | Inherited attributes flow _____ in parse tree | Downward and leftward |
| P577 | Synthesized attributes flow _____ in parse tree | Upward (from leaves to root) |
| P578 | In SDT, actions embedded at end of production: _____ attributed | S-attributed (bottom-up friendly) |
| P579 | In YACC, `%prec UMINUS` is used for? | Changing precedence of a rule (e.g., unary minus) |
| P580 | Marker non-terminal in SDT is used for? | Inserting mid-rule actions (converted to separate production) |

**P581-P600** — Matching:

Match the optimization with its classification:

| # | Optimization | Classification |
|---|---|---|
| P581 | Constant folding | Local (within expression) |
| P582 | Constant propagation | Global (across basic blocks) |
| P583 | CSE within basic block | Local |
| P584 | CSE across basic blocks | Global |
| P585 | Dead code elimination | Global |
| P586 | Copy propagation | Global |
| P587 | Loop invariant code motion | Loop |
| P588 | Strength reduction in loops | Loop |
| P589 | Induction variable elimination | Loop |
| P590 | Peephole optimization | Machine-level / local |
| P591 | Function inlining | Interprocedural |
| P592 | Tail call optimization | Interprocedural |
| P593 | Loop unrolling | Loop / machine-level |
| P594 | Register allocation | Global (entire function) |
| P595 | Instruction scheduling | Machine-level |
| P596 | Branch prediction hints | Machine-level |
| P597 | Algebraic simplification | Local |
| P598 | Loop fusion | Loop |
| P599 | Loop tiling/blocking | Loop (cache optimization) |
| P600 | Interprocedural constant prop | Interprocedural |

**P601-P620** — Miscellaneous Quick:

| # | Question | Answer |
|---|---|---|
| P601 | What is lexer generator output? | C source file containing DFA-based scanner |
| P602 | What is parser generator output? | C source file containing LR parser tables + driver |
| P603 | LALR(1) = SLR(1) + ? | Lookahead refinement (exact lookahead from CLR items, not just FOLLOW) |
| P604 | First input to a compiler? | Source program (character stream) |
| P605 | Final output of a compiler? | Target code (assembly or machine code) |
| P606 | Symbol table is used in which phases? | ALL phases (created in lexer, used through code gen) |
| P607 | Error handler is activated in which phases? | ALL phases (each can detect different errors) |
| P608 | How does parser recover from error in panic mode? | Skip tokens until synchronizing token found (;, }, etc.) |
| P609 | What is phrase-level error recovery? | Replace erroneous prefix with what parser expects |
| P610 | What is error production recovery? | Augment grammar with error rules (e.g., E → error) |
| P611 | What makes a grammar operator grammar? | No two adjacent non-terminals, no ε-productions |
| P612 | Operator grammar + precedence relations = ? | Operator precedence parser |
| P613 | Can every CFG be made operator grammar? | NO (some CFGs cannot be transformed) |
| P614 | What is canonical collection? | Set of all LR(0)/LR(1) item sets for a grammar |
| P615 | GOTO graph is also called? | DFA of viable prefixes (in LR parsing) |
| P616 | What is a sentential form? | Any string derivable from start symbol |
| P617 | What is a right sentential form? | Sentential form obtainable by rightmost derivation |
| P618 | What is right-most derivation in reverse? | Bottom-up parsing (reduce = reverse of rightmost derive) |
| P619 | What is a viable prefix? | Prefix of a right sentential form that can appear on LR parser stack |
| P620 | What does "reducing a handle" mean? | Replace handle (RHS of production on stack top) with LHS |

**P621-P640** — MCQ Challenge:

**P621.** (2-mark) Which parsing technique is used by GCC?
→ (a) LL(1) (b) SLR(1) (c) LALR(1) (d) Hand-written recursive descent
→ **(d)** — GCC switched from YACC-based LALR to hand-written recursive descent (since GCC 4.1 for C++)

**P622.** (1-mark) Python's parser uses?
→ **PEG parser** (since Python 3.9, replaced LL(1))

**P623.** (2-mark) In a bottom-up parser, we never reduce a non-terminal. True or false?
→ **TRUE** — we reduce terminals and non-terminals on stack to a single non-terminal. But the "reduction" replaces a mix of terminals and non-terminals (the handle). We never "reduce" a lone non-terminal in isolation unless it IS the handle.
Actually: FALSE — consider S → A (unit production); we reduce A (a non-terminal on stack) to S.
→ **FALSE** — unit productions reduce single non-terminals!

**P624.** (2-mark) What is the handle of: E + T * F in a rightmost derivation of E→E+T, T→T*F, F→id?
→ The handle is the specific production to reduce. In rightmost derivation reverse:
If input was assembled as ... E + T * F, and F is the result of F→id already done.
The handle is **T * F** (reduce T → T * F since * has higher precedence).

**P625.** (2-mark NAT) In a shift-reduce parser for `id * id + id` using arithmetic grammar:
How many shift actions?
→ 3 id shifts + 1 * shift + 1 + shift = **5 shifts**
How many reduce actions?
→ id→F (×3) + F→T + T*F→T + T→E + E+T→E = **7 reduces**

**P626.** (2-mark) Can a recursive descent parser have backtracking?
→ **YES** — recursive descent with backtracking explores alternatives (inefficient, exponential worst case)

**P627.** (1-mark) Predictive parser = recursive descent + ?
→ No backtracking (exactly one production chosen per non-terminal using lookahead)

**P628-P640** — Targeted Review:

| # | Topic | Key Formula/Fact |
|---|---|---|
| P628 | NFA → DFA states | At most $2^n$ (power set construction) |
| P629 | Minimum DFA states for complement | Same (just swap final/non-final states) |
| P630 | DFA for L₁ ∩ L₂ states | At most m × n (product construction) |
| P631 | Thompson's NFA states for regex r | At most $2 \cdot |r|$ states |
| P632 | LR(0) items for production of length k | k + 1 items |
| P633 | LL(1) parsing table size | |Non-terminals| × |Terminals + $| |
| P634 | SLR(1) table size | |States| × |Terminals + $ + Non-terminals| |
| P635 | FIRST(αβ) when ε ∈ FIRST(α) | (FIRST(α) − {ε}) ∪ FIRST(β) |
| P636 | FIRST(αβ) when ε ∉ FIRST(α) | FIRST(α) (just the first symbol group) |
| P637 | If A → αBβ then FOLLOW(B) ⊇? | (FIRST(β) − {ε}); if ε ∈ FIRST(β) then also FOLLOW(A) |
| P638 | If A → αB then FOLLOW(B) ⊇? | FOLLOW(A) |
| P639 | Data flow: reaching defs bit-vector size | Number of definitions in program |
| P640 | Data flow: available expr bit-vector size | Number of expressions in program |

**P641-P660** — Express Round (30 seconds each):

| # | Q | A |
|---|---|---|
| P641 | What is a token's value? | The attribute (e.g., pointer to symbol table entry, numeric value) |
| P642 | What does `yylval` hold in Lex? | Token attribute value passed to YACC |
| P643 | What does `yyerror()` do? | Called when parser encounters syntax error |
| P644 | Regular expression for C identifier? | `[a-zA-Z_][a-zA-Z_0-9]*` |
| P645 | Regular expression for integer literal? | `[0-9]+` or `[1-9][0-9]*|0` |
| P646 | Regular expression for C string literal? | `\"([^\"\\]|\\.)*\"` (escaped chars allowed) |
| P647 | Activation record also called? | Stack frame |
| P648 | Display array replaces what? | Chain of access links (for faster O(1) non-local access) |
| P649 | What is a liverange? | Interval where a variable holds a useful value (for register allocation) |
| P650 | Interference graph: neighbors cannot share? | Same register (conflict → different colors) |
| P651 | What is spilling? | When not enough colors → store some variables in memory instead of registers |
| P652 | What is coalescing? | Merging two non-interfering variables into same register (eliminates copy) |
| P653 | What is caller-saved vs callee-saved register? | Caller-saved: caller must save before call; Callee-saved: callee must preserve |
| P654 | How many quadruple fields? | 4: operator, arg1, arg2, result |
| P655 | How many triple fields? | 3: operator, arg1, arg2 (result implied by index) |
| P656 | Advantage of triples over quadruples? | Less space (no result field) |
| P657 | Advantage of indirect triples? | Easier to rearrange code (indirection layer) |
| P658 | What is abstract syntax tree (AST)? | Condensed parse tree (no redundant nodes for grammar structure) |
| P659 | AST vs parse tree? | AST omits non-terminals used only for grammar structure |
| P660 | Syntax tree for `a + b * c`? | Root: +, left: a, right: * with children b, c |

---

## 🎯 Score Maximization Strategy — Compiler Design

### Expected GATE Distribution (CD: ~5-8 marks)

| Sub-topic | Expected Marks | Priority |
|---|---|---|
| Parsing (LL/LR family) | 2-4 marks | 🔴 HIGHEST |
| FIRST/FOLLOW | 1-2 marks | 🔴 HIGHEST |
| Lexical Analysis (DFA/NFA) | 1-2 marks | 🟡 HIGH |
| SDT/Attribute Grammars | 1-2 marks | 🟡 HIGH |
| 3AC / ICG | 1-2 marks | 🟡 HIGH |
| Code Optimization | 0-1 marks | 🟢 MEDIUM |
| Runtime Environments | 0-1 marks | 🟢 MEDIUM |
| Code Generation | 0-1 marks | 🟢 MEDIUM |

### Time Allocation (per 2-mark question)

| Task | Time |
|---|---|
| FIRST/FOLLOW computation | 3-4 min |
| Parsing table construction | 4-5 min |
| LR item set construction | 5-6 min |
| 3AC generation | 2-3 min |
| Data flow analysis iteration | 3-4 min |
| Grammar classification | 1-2 min |

### Common Error Prevention

1. **FIRST:** Always check for ε — chain through nullable non-terminals
2. **FOLLOW:** Never put ε in FOLLOW set! Always include $ in FOLLOW(S)
3. **SLR vs CLR:** Count states correctly — SLR = LALR ≤ CLR
4. **LR parsing:** Push STATE after every shift/goto, not just symbol
5. **3AC:** Don't forget labels/gotos for control flow — count them as instructions
6. **Optimization:** CSE requires SAME expression with no intervening redefinition of operands

---

## 🔮 GATE 2025-2027 Predicted Hot Topics

1. **SLR vs CLR conflict example** (classic S→L=R|R grammar) — almost guaranteed!
2. **FIRST/FOLLOW + LL(1) table for grammar with ε** — frequent
3. **Boolean short-circuit 3AC generation** — high probability
4. **Data flow analysis iteration** (reaching defs or live variables) — moderate
5. **Static vs dynamic scoping** output comparison — moderate
6. **Number of LR states counting** — frequent in NAT
7. **Grammar ambiguity proof** — common 1-mark
8. **Lexer: maximal munch + keyword priority** — emerging topic
9. **SDT: S-attributed vs L-attributed classification** — regular
10. **Optimization identification** (name the optimization) — regular 1-mark

---

## 📊 Complete Topic-wise Difficulty Rating

| Topic | Conceptual | Numerical | GATE Frequency |
|---|---|---|---|
| Lexical analysis (DFA/NFA) | ★★☆☆☆ | ★★★☆☆ | ★★★☆☆ |
| FIRST/FOLLOW | ★★☆☆☆ | ★★★★☆ | ★★★★★ |
| LL(1) parsing | ★★★☆☆ | ★★★★☆ | ★★★★☆ |
| SLR/CLR/LALR | ★★★★☆ | ★★★★★ | ★★★★★ |
| Operator precedence | ★★★☆☆ | ★★★☆☆ | ★★☆☆☆ |
| SDT/SDD | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ |
| 3AC generation | ★★☆☆☆ | ★★★☆☆ | ★★★★☆ |
| Code optimization | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ |
| Data flow analysis | ★★★★☆ | ★★★★☆ | ★★★☆☆ |
| Runtime environments | ★★☆☆☆ | ★★☆☆☆ | ★★☆☆☆ |
| Code generation | ★★☆☆☆ | ★★★☆☆ | ★★☆☆☆ |

---

## 📝 Practice Set 15: Rapid Fire — Last Mile (P661 – P730)

**P661.** (1-mark) ε-closure of a state q?
→ Set of all states reachable from q using **only ε-transitions** (including q itself)

**P662.** (1-mark) If DFA has n states, minimum DFA has at most?
→ **n states** (minimization can only reduce or keep same)

**P663.** (2-mark) For grammar S → aB | bA, A → aS | bAA | a, B → bS | aBB | b:
What language does this generate?
→ Equal number of a's and b's! L = {w ∈ {a,b}* | #a(w) = #b(w), |w| > 0}

**P664.** (1-mark) What type of grammar is above?
→ **Context-free** (CFG — generates a CFL that is not regular)

**P665.** (2-mark) Construct DAG for: a = b + c; b = a - d; c = b + c; d = a - d
- Stmt 1: a₀ = b₀ + c₀
- Stmt 2: b₁ = a₀ - d₀ (new def of b)
- Stmt 3: c₁ = b₁ + c₀ (uses new b₁, original c₀)
- Stmt 4: d₁ = a₀ - d₀ (SAME as stmt 2! CSE with b₁!)

DAG: Node for (b₀+c₀)=a₀, Node for (a₀-d₀)=b₁=d₁ (shared!), Node for (b₁+c₀)=c₁
→ **4 statements reduced to 3 computations** (stmts 2 and 4 share the same DAG node)

**P666.** (2-mark) Identify the optimization sequence possible:
```
x = 5;
y = x * 3;
z = y + x;
w = z;
```
1. Constant propagation: x=5 → y = 5*3
2. Constant folding: y = 15
3. Constant propagation: z = 15+5
4. Constant folding: z = 20
5. Copy propagation: w = 20
→ **Final: x=5, y=15, z=20, w=20** — all constants!

**P667.** (1-mark) What is algebraic identity for `x + 0`?
→ x (additive identity) — can be eliminated

**P668.** (1-mark) What is algebraic identity for `x * 1`?
→ x (multiplicative identity)

**P669.** (1-mark) What is algebraic identity for `x ** 2` (exponentiation)?
→ `x * x` (strength reduction — avoid expensive power function)

**P670.** (1-mark) Reduction in strength: `x * 2` →?
→ `x + x` or `x << 1` (shift left by 1)

**P671.** (1-mark) Reduction in strength: `x / 2` →?
→ `x >> 1` (arithmetic shift right by 1, for positive x)

**P672-P690** — Error Detection Phase:

| # | Erroneous code | Which phase detects? |
|---|---|---|
| P672 | `\x` (invalid escape in string) | Lexer |
| P673 | `1abc` (digit starting identifier) | Lexer |
| P674 | `/*` without closing `*/` | Lexer (unterminated comment) |
| P675 | `if (x) { else }` | Parser |
| P676 | `int + float ;` | Parser |
| P677 | `f(1, 2, 3)` where f expects 2 args | Semantic |
| P678 | `int x = "hello";` | Semantic (type mismatch) |
| P679 | `int x; int x;` in same scope | Semantic (redeclaration) |
| P680 | `return` in void function with value | Semantic |
| P681 | Use of uninitialized variable | Semantic or runtime (language-dependent) |
| P682 | Null pointer dereference | Runtime |
| P683 | Integer overflow | Runtime (or undefined behavior in C) |
| P684 | Accessing freed memory | Runtime (use-after-free) |
| P685 | Missing library at link time | Linker |
| P686 | Conflicting types across files | Linker |
| P687 | `sizeof(void)` | Semantic (void has no size, though GCC allows as extension=1) |
| P688 | Assigning to const variable | Semantic |
| P689 | Breaking from non-loop/switch | Semantic |
| P690 | `goto` crossing variable declaration | Semantic (C99+ / C++) |

**P691-P710** — Analytical:

**P691.** (2-mark) How many passes does a simple one-pass compiler make?
→ **1 pass** — reads source once, generates code directly (requires forward declaration or backpatching for forward jumps)

**P692.** (2-mark) Why can't C be compiled in one pass without forward declarations?
→ Functions can be called before they are defined → compiler doesn't know the function signature.
→ Solution: forward declarations (`int f(int);`) or header files, or use multi-pass

**P693.** (2-mark) Why was Pascal designed for one-pass compilation?
→ All declarations must come before use, procedure/function declarations have forward declarations, and nested scope rules allow single-pass processing

**P694.** (2-mark) What is a syntax-directed definition (SDD) vs syntax-directed translation (SDT)?
→ SDD: grammar + semantic rules (no order specification)
→ SDT: grammar + semantic actions embedded at specific positions (order matters)
→ SDD is more abstract; SDT is implementation-oriented

**P695.** (2-mark) Can S-attributed SDDs always be evaluated in one bottom-up pass?
→ **YES** — synthesized attributes are computed at reduce time in bottom-up parsing

**P696.** (2-mark) Can L-attributed SDDs always be evaluated in one left-to-right pass?
→ **YES** — by definition, L-attributed means evaluable in single left-to-right, depth-first traversal

**P697.** (1-mark) Can L-attributed SDDs be evaluated during bottom-up parsing?
→ Not always straightforwardly — need to convert inherited attributes using markers/magic.
→ With tricks (marker non-terminals): **YES** for many cases

**P698.** (2-mark) Explain marker non-terminal technique:

Given: A → {action} B C (action before B)
Transform: A → M B C, M → ε {action}
Now M reduces before B is seen → action executes during bottom-up parse!

But: M → ε could introduce conflicts (if parser can't decide when to reduce M)

**P699.** (2-mark) What is the maximum number of S/R conflicts possible in SLR table?
→ For each state × terminal: one shift possibility vs one reduce → max |states| × |terminals| entries can have conflicts (theoretical upper bound, rarely all conflicted)

**P700.** (1-mark) If a grammar is LR(1), is it unambiguous?
→ **YES** — all LR(k) grammars are unambiguous

**P701.** (1-mark) If a grammar is unambiguous, is it LR(1)?
→ **NO** — some unambiguous grammars are not LR(k) for any k (inherently non-LR)
→ Example: S → aAd | aBd, A → bAc | bc, B → bBc | bbc

**P702-P710** — Grammar Transformations:

**P702.** Remove left recursion from: E → E + T | E - T | T
→ E → T E', E' → + T E' | - T E' | ε

**P703.** Left factor: S → if E then S else S | if E then S
→ S → if E then S S', S' → else S | ε

**P704.** Remove indirect left recursion: A → Ba | a, B → Ab | b
→ Substitute A into B: B → Bab | ab | b
→ Remove direct left recursion: B → abB' | bB', B' → abB' | ε
→ A → Ba | a (no change needed — A is not left-recursive!)

**P705.** Convert to CNF: S → ABc
→ Introduce C_c → c. S → AB C_c. Already CNF if A,B don't have issues.
If A → a: A → a is fine (single terminal). 
Final: S → A D, D → B C_c, C_c → c, A → a, B → ... ✅

**P706.** (2-mark) Eliminate ε-productions from: S → aB, B → bB | ε
→ Nullable: {B}. New productions: S → aB | a (B can be ε). B → bB | b.
→ Check: L(old) = L(new)? Old: a, ab, abb, abbb,... New: same! ✅

**P707.** (1-mark) Eliminate unit productions from: S → A, A → B, B → a | b
→ S → A → B → a|b. Remove chain: S → a | b, A → a | b, B → a | b

**P708-P710** — Quick verify:

| # | Grammar | LL(1)? | Why? |
|---|---|---|---|
| P708 | S → aS | a | NO (FIRST conflict on 'a') |
| P709 | S → aS | b | YES (FIRST sets {a}, {b} disjoint) |
| P710 | S → aSb | ε | YES (FIRST(aSb)={a}, FOLLOW(S)={b,$}, {a} ∩ {b,$} = ∅) |

**P711-P730** — Final Express Round:

| # | Q | A |
|---|---|---|
| P711 | What is peephole window size typically? | 2-3 instructions |
| P712 | What is basic block? | Maximal sequence of instructions with single entry, single exit |
| P713 | Flow graph = ? | Graph of basic blocks with edges showing control flow |
| P714 | Dominator: A dominates B means? | Every path from entry to B goes through A |
| P715 | Immediate dominator of B? | Closest strict dominator of B |
| P716 | Dominator tree root? | Entry node |
| P717 | Post-dominator? | A post-dominates B if every path from B to EXIT goes through A |
| P718 | SSA: what does phi-function do? | Selects correct variable version based on which predecessor executed |
| P719 | What is semi-pruned SSA? | Phi-functions only for variables live at block entry |
| P720 | What is pruned SSA? | Phi-functions only where variable is live AND has multiple reaching definitions |
| P721 | What is interval analysis? | Grouping nodes into intervals for structured control flow analysis |
| P722 | What is t-diagram used for? | Showing relationships between compilers, source/target languages |
| P723 | Cross-compilation t-diagram? | Compiler for language X→Y, written in Z, run on machine M |
| P724 | What is an abstract syntax? | Grammar-like rules defining AST node types |
| P725 | What is a concrete syntax? | The actual grammar of the source language |
| P726 | Left-corner parsing combines? | Top-down + bottom-up (hybrid approach) |
| P727 | Chart parsing? | Dynamic programming for general CFG (Earley, CYK variants) |
| P728 | GLR parser? | Generalized LR — handles ambiguous grammars by forking |
| P729 | PEG vs CFG? | PEG uses ordered choice (no ambiguity); CFG uses unordered alternatives |
| P730 | What is predictive parsing? | LL parsing with 1 token lookahead, no backtracking |

---

## 📋 Compiler Design Mastery Checklist

- Can I convert regex → NFA → DFA → minimized DFA?
- Can I compute FIRST and FOLLOW for any grammar with ε?
- Can I construct LL(1) parsing table and trace a parse?
- Can I build LR(0) item sets (canonical collection)?
- Can I construct SLR(1) parsing table?
- Can I identify SLR vs CLR conflicts (S→L=R|R example)?
- Can I trace an LR parse (shift/reduce steps)?
- Can I write SDT rules (synthesized and inherited)?
- Can I generate 3AC for arithmetic, boolean, arrays, loops?
- Can I perform backpatching for boolean expressions?
- Can I identify and classify optimizations (CSE, dead code, etc.)?
- Can I do data flow analysis iterations (reaching defs, live vars)?
- Can I apply Sethi-Ullman for register allocation?
- Can I determine scoping output (static vs dynamic)?
- Can I compute parameter passing results for all methods?
- Can I count LR states, items, DFA states from a grammar?

If YES to all → **You are GATE-ready for Compiler Design!** 🏆

---

## 🧠 50 One-Liner GATE Rapid Recall — Compiler Design

1. Lexer output = token stream. Parser output = parse tree.
2. Phases ≠ passes. Phase = logical, Pass = physical traversal.
3. Regular language < CFL < CSL < REL. Lexer handles regular, parser handles CFL.
4. NFA to DFA: subset construction. DFA minimization: Hopcroft's $O(n \log n)$.
5. Thompson's: regex of size k → NFA with at most 2k states.
6. Maximal munch: lexer always picks the longest match.
7. Keywords recognized: either reserved word table or DFA states.
8. FIRST(ε) = {ε}. FOLLOW never has ε. FOLLOW(S) always has $.
9. If A → αBβ: FOLLOW(B) gets FIRST(β)−{ε}. If ε ∈ FIRST(β): FOLLOW(B) gets FOLLOW(A).
10. LL(1) condition: for A → α|β, FIRST(α) ∩ FIRST(β) = ∅. If ε involved: also check FOLLOW.
11. LL cannot handle left recursion. LR can handle both left and right recursion.
12. Left recursion A → Aα|β → A → βA', A' → αA'|ε.
13. Left factoring A → αβ₁|αβ₂ → A → αA', A' → β₁|β₂.
14. LL(1) ⊂ LR(1). Every LL(1) grammar is also LR(1), not vice versa.
15. LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1) — strictly increasing power.
16. SLR and LALR have same number of states. CLR may have more.
17. SLR uses FOLLOW for reduce decisions. CLR uses exact lookahead.
18. LALR = CLR states merged by core. May introduce reduce-reduce (never shift-reduce) conflicts.
19. Ambiguous grammar ⇒ not LR(k) for any k.
20. LR(k) grammar ⇒ unambiguous.
21. Handle = substring matching RHS, where reducing gives valid rightmost derivation.
22. Viable prefix = prefix of right sentential form that can be on stack.
23. Augmented grammar: add S' → S to define accept condition.
24. Kernel items: dot not at start (except S' → •S).
25. Closure: for A → α•Bβ, add all B → •γ.
26. Goto(I, X): move dot past X, then closure.
27. Operator grammar: no two adjacent non-terminals, no ε-productions.
28. Synthesized = from children. Inherited = from parent/siblings.
29. S-attributed SDD: only synthesized → evaluate bottom-up.
30. L-attributed SDD: inherited only from left → evaluate left-to-right.
31. 3AC: at most one operator per instruction (plus assignment).
32. Quadruple = (op, arg1, arg2, result). Triple = (op, arg1, arg2) — result by index.
33. Boolean short-circuit: && skips second if first false. || skips if first true.
34. Backpatching: fill in jump targets later when known.
35. Basic block: maximal straight-line code (one entry, one exit).
36. Leaders: first instr, jump target, instruction after jump.
37. DAG of basic block: detects CSE, dead code, reordering opportunities.
38. Reaching definitions: forward, union, GEN/KILL per block.
39. Available expressions: forward, intersection.
40. Live variables: backward, union.
41. Very busy expressions: backward, intersection.
42. Loop = back edge + dominance. Natural loop = all nodes that reach tail without going through head.
43. Loop invariant: all operands constant or defined outside loop.
44. Strength reduction: multiply → add for induction variables.
45. Peephole: local optimization on small window (2-3 instructions).
46. Sethi-Ullman: minimum registers for expression tree. Labels from leaves up.
47. Static scoping: determined by program text. Dynamic: by runtime call stack.
48. Access link / static link: for accessing non-local variables in nested scopes.
49. Display: array indexed by nesting depth → O(1) non-local access.
50. Activation record: return value, parameters, return address, control link, locals, temps.

---

## 🎓 Study Priority Matrix — Last 30 Days

| Days Left | Focus Area | What to Do |
|---|---|---|
| 30-21 | Fundamentals | FIRST/FOLLOW, LL(1), LR(0) items from scratch |
| 20-11 | Core problems | SLR table construction, 3AC generation, SDT evaluation |
| 10-6 | Advanced + PYQs | CLR/LALR conflicts, data flow analysis, optimization identification |
| 5-3 | Mixed practice | Full practice sets (timed), weak topic revision |
| 2-1 | Quick recall | 100 facts list, 40 traps, formula card, confused pairs |
| Exam day | Glance only | Mastery checklist, parser hierarchy, data flow directions |

---

## ⚡ Emergency Formulas — Compiler Design

| Formula | Value |
|---|---|
| Thompson's NFA states | ≤ 2|r| for regex r |
| Subset construction max states | $2^n$ for NFA with n states |
| DFA minimization time | $O(n \log n)$ (Hopcroft) |
| LL(1) table size | |V_N| × (|V_T| + 1) |
| LR table size | |States| × (|V_T| + |V_N| + 1) |
| LR(0) items for production len k | k + 1 |
| Total LR(0) items | Σ(|αᵢ| + 1) = L + P |
| Product DFA (L₁ ∩ L₂) states | m × n |
| Complement DFA states | Same as original n |
| Sethi-Ullman labels | leaf: 1(left)/0(right); internal: max(l,r) if l≠r, l+1 if l=r |
| Data flow convergence | $O(d \times n)$ where d=depth, n=nodes |
| CYK parsing time | $O(n^3 \times |G|)$ |
| Earley parsing time | $O(n^3)$ worst, $O(n^2)$ unambiguous, $O(n)$ LR |

---

## 🏁 Final Quick-Reference: Parser Hierarchy (GATE Guaranteed!)

```
Regular Grammars
    ⊂ LL(0) ⊂ LL(1) ⊂ LL(2) ⊂ ... ⊂ LL(k)
    ⊂ LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1)
    ⊂ Unambiguous CFG
    ⊂ All CFG (including ambiguous)
```

**Key relationships:**
- LL(1) ⊂ LR(1) ✅ (every LL(1) is LR(1))
- LR(0) ⊄ LL(1) and LL(1) ⊄ LR(0) (incomparable!)
- All LR(k) grammars are unambiguous ✅
- Ambiguous grammar → NOT LR(k) for any k ✅
- SLR states = LALR states ≤ CLR states ✅
- LALR merge of CLR: can only introduce R/R conflicts (never S/R) ✅

**State count relationship:**
```
|SLR states| = |LALR states| ≤ |CLR states| ≤ 2^(|SLR states|)
```

**Data Flow Analysis Summary Table:**

| Analysis | Direction | Meet (∧) | Transfer | Init |
|---|---|---|---|---|
| Reaching Definitions | Forward | ∪ (Union) | GEN ∪ (IN−KILL) | ∅ |
| Available Expressions | Forward | ∩ (Intersect) | GEN ∪ (IN−KILL) | Universal |
| Live Variables | Backward | ∪ (Union) | USE ∪ (OUT−DEF) | ∅ |
| Very Busy Expressions | Backward | ∩ (Intersect) | USE ∪ (OUT−KILL) | Universal |

**Remember: Union → MAY analysis (optimistic). Intersection → MUST analysis (conservative).**

---

*End of Detailed Notes — Compiler Design for GATE 2027*
