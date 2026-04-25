# DBMS — Revision Notes for GATE 2027

> Quick-reference sheet for last-minute revision. All key formulas, conditions, and decision rules.

---

## 1. Keys — Quick Rules

| Key | Definition | Rule |
|---|---|---|
| Super Key | Uniquely identifies tuples | Any superset of a candidate key |
| Candidate Key | Minimal super key | No proper subset is a super key |
| Primary Key | Chosen candidate key | Cannot be NULL |
| Foreign Key | References another table's PK | Must match a PK value or be NULL |

**Finding Candidate Keys:**
1. Attributes **only on LHS** (never on RHS) → must be in every CK
2. Attributes **only on RHS** → never in any CK
3. Compute closure of "must-be" attributes; if it equals $R$ → only CK
4. Otherwise, try adding "both-side" attributes one by one

**Super key count:** From CK of size $k$ in relation of degree $n$: $2^{n-k}$. For multiple CKs, use inclusion-exclusion.

---

## 2. Functional Dependency Laws

| Law | Rule |
|---|---|
| Reflexivity | $Y \subseteq X \Rightarrow X \to Y$ |
| Augmentation | $X \to Y \Rightarrow XZ \to YZ$ |
| Transitivity | $X \to Y, Y \to Z \Rightarrow X \to Z$ |
| Union | $X \to Y, X \to Z \Rightarrow X \to YZ$ |
| Decomposition | $X \to YZ \Rightarrow X \to Y, X \to Z$ |
| Pseudotransitivity | $X \to Y, WY \to Z \Rightarrow WX \to Z$ |

**Closure Algorithm:** Start with $X$, repeatedly add RHS of satisfied FDs until stable.

**Minimal Cover Steps:** (1) Single RHS → (2) Remove extraneous LHS → (3) Remove redundant FDs

---

## 3. Normal Forms — Decision Table

| Check | Condition | Violated NF |
|---|---|---|
| Non-atomic values? | Yes → NOT 1NF | 1NF |
| Non-prime attr depends on **proper subset** of CK? | Yes → partial dep → NOT 2NF | 2NF |
| Non-prime depends on non-superkey transitively? | Yes → NOT 3NF | 3NF |
| For every $X \to A$: is $X$ superkey OR $A$ prime? | No → NOT 3NF | 3NF |
| For every $X \to A$: is $X$ superkey? | No → NOT BCNF | BCNF |

**Hierarchy:** BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF

**Quick 3NF vs BCNF:** 3NF allows $X \to A$ where $A$ is prime (even if $X$ not superkey). BCNF does not.

---

## 4. Decomposition Rules

| Property | Test |
|---|---|
| **Lossless (binary)** | $R_1 \cap R_2 \to R_1 - R_2$ or $R_1 \cap R_2 \to R_2 - R_1$ must be in $F^+$ |
| **Lossless (n-ary)** | Chase test: if any row becomes all distinguished symbols |
| **Dependency Preserving** | $(F_1 \cup F_2 \cup \ldots)^+ = F^+$ |

| Decomposition Into | Lossless? | Dep. Preserving? |
|---|---|---|
| BCNF | Always ✓ | May NOT ✗ |
| 3NF (synthesis) | Always ✓ | Always ✓ |

---

## 5. Relational Algebra Cheat Sheet

| Operation | Symbol | Key Property |
|---|---|---|
| Selection | $\sigma_c(R)$ | Filters rows, same schema |
| Projection | $\pi_L(R)$ | Filters columns, removes duplicates |
| Union | $R \cup S$ | Union-compatible required |
| Difference | $R - S$ | Union-compatible required |
| Cartesian Product | $R \times S$ | Degree adds, cardinality multiplies |
| Natural Join | $R \bowtie S$ | Join on common attributes |
| Division | $R \div S$ | "For all" queries |

**Cardinality Bounds:**

| Op | Min | Max |
|---|---|---|
| $\sigma$ | 0 | $\|R\|$ |
| $\pi$ | 1 | $\|R\|$ |
| $R \cup S$ | $\max$ | sum |
| $R \cap S$ | 0 | $\min$ |
| $R \times S$ | product | product |
| $R \bowtie S$ | 0 | product |

**Division formula:** $R \div S = \pi_A(R) - \pi_A((\pi_A(R) \times S) - R)$

---

## 6. SQL Key Facts

| Concept | Remember |
|---|---|
| Execution order | FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY |
| `COUNT(*)` vs `COUNT(col)` | `*` counts all rows; `col` ignores NULLs |
| `NULL = NULL` | Evaluates to UNKNOWN, not TRUE |
| `IS NULL` | Correct way to check for NULL |
| `IN` vs `= ANY` | Equivalent |
| `NOT IN` with NULLs | Dangerous! If subquery has NULL, result is empty |
| Division in SQL | Double NOT EXISTS pattern |
| GROUP BY with NULL | All NULLs grouped together |
| HAVING | Filters groups (after GROUP BY), can use aggregates |
| WHERE | Filters rows (before GROUP BY), cannot use aggregates |

---

## 7. Indexing Formulas

| Parameter | Formula |
|---|---|
| Blocking factor | $f = \lfloor B/R \rfloor$ |
| Data blocks | $b = \lceil n/f \rceil$ |
| Index entry size | $V + P$ (key + pointer) |
| Index BF | $f_i = \lfloor B/(V+P) \rfloor$ |
| Primary index entries | = number of data blocks |
| Clustering index entries | = number of distinct values |
| Secondary index entries | = $n$ (dense) |

| Index Type | Dense/Sparse | Data Sorted on Key? |
|---|---|---|
| Primary | Sparse | Yes |
| Clustering | Sparse | Yes |
| Secondary | Dense | No |

---

## 8. B-Tree vs B+ Tree

| Property | B-Tree (order $p$) | B+ Tree (internal $p$, leaf $p_l$) |
|---|---|---|
| Data pointers | All nodes | Leaf only |
| Max keys/node | $p - 1$ | Internal: $p-1$, Leaf: $p_l$ |
| Min keys (non-root) | $\lceil p/2 \rceil - 1$ | Internal: $\lceil p/2 \rceil - 1$, Leaf: $\lceil p_l/2 \rceil$ |
| Leaf linking | No | Yes (linked list) |
| Range queries | Slow | Fast |

**Order Formulas:**

| Tree | Order Formula |
|---|---|
| B-tree | $(p-1)(V+P_d) + p \cdot P_b \leq B$ |
| B+ internal | $(p-1) \cdot V + p \cdot P_b \leq B$ |
| B+ leaf | $p_l(V + P_d) + P_b \leq B$ |

---

## 9. ACID Properties

| Property | Meaning | Ensured By |
|---|---|---|
| **A**tomicity | All or nothing | Recovery (undo/redo logs) |
| **C**onsistency | Valid state → valid state | App logic + constraints |
| **I**solation | No interference between concurrent txns | Concurrency control |
| **D**urability | Committed = permanent | WAL, checkpointing |

---

## 10. Serializability Tests

**Conflict Serializability:**
1. Draw precedence graph (edge $T_i \to T_j$ for conflicting ops where $T_i$ first)
2. **Acyclic → conflict serializable** (topological sort = equivalent serial order)
3. Conflicting: different transactions, same item, at least one write

**View Serializability:** Check against ALL serial orders for:
1. Same initial reads
2. Same read-from relationships
3. Same final writes

**Hierarchy:** Conflict Serializable ⊂ View Serializable ⊂ Serializable

---

## 11. Recoverability Hierarchy

| Type | Rule | Property |
|---|---|---|
| **Recoverable** | If $T_j$ reads from $T_i$, then $T_i$ commits before $T_j$ commits | Avoids irrecoverable |
| **Cascadeless** | $T_j$ reads from $T_i$ → $T_i$ commits before $T_j$ reads | No cascading aborts |
| **Strict** | $T_j$ reads/writes $X$ written by $T_i$ → $T_i$ finishes first | Simple recovery |

Strict ⊂ Cascadeless ⊂ Recoverable

---

## 12. Two-Phase Locking (2PL) Variants

| Variant | Rule | Deadlock-Free? | Cascadeless? |
|---|---|---|---|
| **Basic 2PL** | Grow then shrink | No | No |
| **Conservative** | Lock ALL at start | **Yes** | No |
| **Strict 2PL** | Hold X-locks till commit | No | **Yes** |
| **Rigorous 2PL** | Hold ALL locks till commit | No | **Yes** |

**All 2PL variants guarantee conflict serializability.**

**Lock Compatibility:**

|  | S | X |
|---|---|---|
| **S** | ✓ | ✗ |
| **X** | ✗ | ✗ |

---

## 13. Timestamp Protocol Rules

**READ $X$ by $T_i$:**
- $TS(T_i) < W\_TS(X)$ → **Abort** $T_i$
- Else → Read, update $R\_TS(X) = \max(R\_TS(X), TS(T_i))$

**WRITE $X$ by $T_i$:**
- $TS(T_i) < R\_TS(X)$ → **Abort** $T_i$
- $TS(T_i) < W\_TS(X)$ → **Thomas Write Rule: Skip** (don't abort)
- Else → Write, update $W\_TS(X) = TS(T_i)$

**Properties:** Deadlock-free, conflict serializable (without TWR), may not be recoverable.

---

## 14. Deadlock Prevention

| Protocol | Older → Younger | Younger → Older |
|---|---|---|
| **Wait-Die** | Wait | Die (younger aborts) |
| **Wound-Wait** | Wound (younger aborts) | Wait |

Both: deadlock-free + starvation-free (restarted txn keeps old timestamp).

---

## 15. ER to Relational Mapping

| ER Element | # Tables | How |
|---|---|---|
| Strong entity | 1 | All simple attrs, PK = key attr |
| Weak entity | 1 | Partial key + owner PK, FK to owner |
| 1:1 (both total) | 0 extra (merge possible) | FK on either side |
| 1:N | 0 extra | FK on N-side |
| M:N | 1 extra | New table, PK = both FKs |
| Ternary | 1 extra | New table, PK = all participant FKs |
| Multivalued attr | 1 extra | New table with FK to owner |
| Composite attr | 0 extra | Store component attrs only |
| Derived attr | 0 extra | Don't store |

### Generalization/Specialization
- **Generalization:** Bottom-up (merge sub-entities)
- **Specialization:** Top-down (split into sub-entities)
- ISA: sub-entities inherit super's attributes
- Disjoint/Overlapping × Total/Partial constraints
- Mapping: separate tables (FK to super), merged table, or sub-only tables

---

## 15a. 4NF/5NF & MVDs

- **MVD:** $X \twoheadrightarrow Y$ means Y-values for X are independent of $R-X-Y$ values
- Every FD is an MVD (not vice versa)
- **4NF:** Every non-trivial MVD $X \twoheadrightarrow Y$ → $X$ is superkey
- 4NF ⊂ BCNF ⊂ 3NF hierarchy: 1NF ⊃ 2NF ⊃ 3NF ⊃ BCNF ⊃ 4NF ⊃ 5NF
- **5NF (PJNF):** Every JD implied by candidate keys

---

## 15b. Recovery System

- **WAL:** Log flushed before data page. Ensures undo capability.
- **Undo-Redo logging:** Undo uncommitted + Redo committed
- **ARIES phases:** Analysis → Redo (repeat history) → Undo (use CLRs)
- **Checkpoint:** Reduces log scan; records active transactions + dirty pages

---

## 15c. Isolation Levels

| Level | Dirty Read | Non-Repeatable | Phantom |
|-------|-----------|---------------|---------|
| Read Uncommitted | ✓ | ✓ | ✓ |
| Read Committed | ✗ | ✓ | ✓ |
| Repeatable Read | ✗ | ✗ | ✓ |
| Serializable | ✗ | ✗ | ✗ |

- **MVCC:** Readers see snapshots, never block writers (PostgreSQL, Oracle)

---

## 15d. Hashing

| Type | Key Feature |
|------|------------|
| Static | Fixed buckets, overflow chains |
| Extendible | Directory (2^d entries), doubles on overflow at global depth |
| Linear | Split pointer moves sequentially, no directory |

- Extendible: local depth = global depth → directory doubles

---

## 15e. Query Processing

- Push selections early (most important!)
- Join costs: Block NL = ⌈br/(M-2)⌉×bs+br, Hash = 3(br+bs), Sort-Merge = sort + merge
- Selection cascade, join commutativity/associativity

---

## 15f. Views, Triggers, DRC

- View = virtual table (query stored). Updatable if single table, no agg, no GROUP BY
- Trigger = automatic action on INSERT/UPDATE/DELETE
- DRC: variables = domain values, not tuples. RA = TRC(safe) = DRC(safe)

---

## 16. Critical GATE Traps

1. **3NF vs BCNF**: If all attributes are prime, it's automatically 3NF (but may not be BCNF)
2. **COUNT(\*) vs COUNT(col)**: COUNT(\*) includes NULLs
3. **NOT IN with NULLs**: If subquery returns NULL, NOT IN returns empty!
4. **2PL guarantees conflict serializability** but NOT deadlock freedom (except conservative)
5. **BCNF decomposition always lossless** but may lose dependencies
6. **B+ tree leaves are linked** → efficient range queries
7. **Thomas Write Rule**: Skip obsolete write, DON'T abort
8. **Primary index = sparse**, Secondary index = dense
9. **Division = "for all"** queries (double NOT EXISTS in SQL)
10. **View serializable but not conflict serializable** → involves blind writes

---

## 17. Number Crunching Quick Reference

| What | Formula |
|---|---|
| Super keys from CK of size $k$, $n$ attrs | $2^{n-k}$ |
| Max candidate keys for $n$ attrs | $\binom{n}{\lfloor n/2 \rfloor}$ |
| B-tree min height | $\lceil \log_p(n+1) \rceil$ |
| B+ tree max records (height $h$, $p$, $p_l$) | $p^{h-1} \times p_l$ |
| Binary search cost | $\lceil \log_2(b) \rceil$ where $b$ = blocks |
| Multilevel index search | levels + 1 |

---

*End of Revision Notes — DBMS for GATE 2027*
