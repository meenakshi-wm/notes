# Data Structures & Algorithms — Revision Notes (GATE 2027)

> Last-minute revision: complexity tables, key formulas, algorithm cheat sheets, comparison charts
> ⭐ = Most frequently tested

---

## 1. Asymptotic Growth Rate Ordering

$$1 < \log \log n < \log n < \sqrt{n} < n < n \log n < n^{1.5} < n^2 < n^3 < 2^n < n! < n^n$$

**Quick Rules:**
- $\log^k n = O(n^\epsilon)$ for any $\epsilon > 0$ (log grows slower than any polynomial)
- $n^k = O(c^n)$ for any $c > 1$ (polynomial grows slower than exponential)
- $2^{n+1} = O(2^n)$ ✓ but $2^{2n} \neq O(2^n)$ ✗

---

## 2. Master Theorem Cheat Sheet ⭐

$T(n) = aT(n/b) + f(n)$, let $c = \log_b a$

| Case | Condition | Result |
|------|-----------|--------|
| 1 | $f(n) = O(n^{c-\epsilon})$ | $\Theta(n^c)$ |
| 2 | $f(n) = \Theta(n^c \log^k n)$ | $\Theta(n^c \log^{k+1} n)$ |
| 3 | $f(n) = \Omega(n^{c+\epsilon})$ + regularity | $\Theta(f(n))$ |

**Instant Lookup Table:**

| Recurrence | Result |
|-----------|--------|
| $T(n) = T(n/2) + 1$ | $\Theta(\log n)$ |
| $T(n) = T(n/2) + n$ | $\Theta(n)$ |
| $T(n) = 2T(n/2) + 1$ | $\Theta(n)$ |
| $T(n) = 2T(n/2) + n$ | $\Theta(n \log n)$ — Merge Sort |
| $T(n) = 2T(n/2) + n^2$ | $\Theta(n^2)$ |
| $T(n) = 4T(n/2) + n$ | $\Theta(n^2)$ |
| $T(n) = 4T(n/2) + n^2$ | $\Theta(n^2 \log n)$ |
| $T(n) = 7T(n/2) + n^2$ | $\Theta(n^{2.81})$ — Strassen's |
| $T(n) = 9T(n/3) + n$ | $\Theta(n^2)$ |
| $T(n) = T(n-1) + 1$ | $\Theta(n)$ |
| $T(n) = T(n-1) + n$ | $\Theta(n^2)$ |
| $T(n) = 2T(n-1) + 1$ | $\Theta(2^n)$ |
| $T(n) = T(\sqrt{n}) + 1$ | $\Theta(\log \log n)$ |

---

## 3. Loop Complexity Patterns

| Pattern | Code | Complexity |
|---------|------|-----------|
| Linear | `for i=1 to n` | $O(n)$ |
| Nested | `for i, for j to n` | $O(n^2)$ |
| Dependent | `for i, for j to i` | $O(n^2)$ [$\sum i = n(n+1)/2$] |
| Log | `while i < n: i *= 2` | $O(\log n)$ |
| Sqrt | `while i > 1: i = sqrt(i)` | $O(\log \log n)$ |
| Geometric inner | `for i*=2, for j to i` | $O(n)$ [GP sum] |
| Harmonic | `for i=1 to n, for j+=i` | $O(n \log n)$ [$\sum n/i$] |
| Root | `while s<=n: i++, s+=i` | $O(\sqrt{n})$ |

---

## 4. Data Structure Operations — Master Table ⭐

| Structure | Search | Insert | Delete | Min/Max | Space |
|-----------|--------|--------|--------|---------|-------|
| Unsorted Array | $O(n)$ | $O(1)$* | $O(n)$ | $O(n)$ | $O(n)$ |
| Sorted Array | $O(\log n)$ | $O(n)$ | $O(n)$ | $O(1)$ | $O(n)$ |
| Singly LL | $O(n)$ | $O(1)$† | $O(n)$ | $O(n)$ | $O(n)$ |
| Doubly LL | $O(n)$ | $O(1)$† | $O(1)$‡ | $O(n)$ | $O(n)$ |
| Stack | — | $O(1)$ | $O(1)$ | — | $O(n)$ |
| Queue | — | $O(1)$ | $O(1)$ | — | $O(n)$ |
| BST (avg) | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(n)$ |
| BST (worst) | $O(n)$ | $O(n)$ | $O(n)$ | $O(n)$ | $O(n)$ |
| AVL / Red-Black | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(n)$ |
| Min-Heap | $O(n)$ | $O(\log n)$ | $O(\log n)$ | $O(1)$$^§$ | $O(n)$ |
| Hash Table (avg) | $O(1)$ | $O(1)$ | $O(1)$ | $O(n)$ | $O(n)$ |

*End insert. †Head insert. ‡Given pointer. §Min only for min-heap.

---

## 5. Stack & Queue Key Facts

| Concept | Formula / Fact |
|---------|---------------|
| Stack permutations count | $C_n = \frac{1}{n+1}\binom{2n}{n}$ (Catalan) |
| Invalid pattern (stack) | $3,1,2$ pattern in output ✗ |
| Circular queue full | $(rear+1) \% size == front$ |
| Queue via 2 stacks | Amortized $O(1)$ both operations |
| Stack using 2 queues | One operation $O(n)$, other $O(1)$ |
| Infix→Postfix | Use stack for operators, precedence rules |
| Postfix evaluation | Scan L→R, push operands, pop for operators |

**Catalan numbers:** 1, 1, 2, 5, 14, 42, 132, 429, 1430, ...

---

## 6. Binary Tree Formulas ⭐

| Property | Formula |
|----------|---------|
| Max nodes at level $l$ | $2^l$ |
| Max nodes height $h$ | $2^{h+1} - 1$ |
| Min height for $n$ nodes | $\lfloor \log_2 n \rfloor$ |
| Leaves = 2-children nodes + 1 | $n_0 = n_2 + 1$ |
| NULL pointers | $n + 1$ |
| # unlabeled binary trees ($n$ nodes) | $C_n$ |
| # labeled binary trees | $C_n \cdot n!$ |
| # BSTs with $n$ keys | $C_n$ |

**Unique tree construction:**
- Inorder + Preorder → ✓ Unique
- Inorder + Postorder → ✓ Unique
- Preorder + Postorder → ✗ (unless full binary tree)

---

## 7. AVL Tree Quick Reference ⭐

| Property | Value |
|----------|-------|
| Balance factor | $-1, 0, +1$ |
| Max height for $n$ nodes | $O(\log n)$ |
| Min nodes for height $h$ | $N(h) = N(h-1)+N(h-2)+1$ |
| Insert rotations | At most **1** (single or double) |
| Delete rotations | Up to $O(\log n)$ |

**Min nodes:** 1, 2, 4, 7, 12, 20, 33, 54, 88, ...

| Imbalance | Fix |
|-----------|-----|
| LL | Right rotation |
| RR | Left rotation |
| LR | Left-Right double rotation |
| RL | Right-Left double rotation |

---

## 7a. Red-Black Tree Quick Reference ⭐

**Five Properties:**
1. Every node is Red or Black
2. Root is Black
3. Every NIL leaf is Black
4. Red node → both children are Black (no two consecutive reds)
5. All paths from a node to descendant NILs have the same black-height

| Property | Value |
|----------|-------|
| Max height | $2\log_2(n+1)$ |
| Black-height $bh$ | $\geq h/2$ |
| Min nodes for black-height $bh$ | $2^{bh} - 1$ |
| Insert fix-up | At most 2 rotations + recoloring |
| Delete fix-up | At most 3 rotations + recoloring |

**AVL vs Red-Black:**

| | AVL | Red-Black |
|--|-----|----------|
| Balance | Stricter ($h \leq 1.44\log n$) | Looser ($h \leq 2\log n$) |
| Search | Faster (shorter height) | Slightly slower |
| Insert/Delete | More rotations | Fewer rotations |
| Use case | Read-heavy | Insert/Delete-heavy |
| Used in | Databases | `std::map`, Linux kernel |

**Insert fix-up cases:**
- Uncle is Red → Recolor parent, uncle, grandparent
- Uncle is Black, zig-zag → Rotate to zig-zig, then fix
- Uncle is Black, zig-zig → Rotate grandparent + recolor

---

## 7b. Optimal BST (DP) ⭐

$$e[i,j] = \min_{i \leq r \leq j}\{e[i, r-1] + e[r+1, j] + w(i,j)\}$$

where $w(i,j) = \sum_{k=i}^{j} p_k + \sum_{k=i-1}^{j} q_k$

| Property | Value |
|----------|-------|
| Time | $O(n^3)$ |
| Space | $O(n^2)$ |
| Knuth's optimization | $O(n^2)$ |
| $p_k$ | Search probability for key $k$ |
| $q_k$ | Search probability for dummy key $d_k$ |

---

## 8. Heap Quick Reference ⭐

| Operation | Complexity |
|-----------|-----------|
| Find min/max | $O(1)$ |
| Insert | $O(\log n)$ |
| Extract min/max | $O(\log n)$ |
| **Build heap** | **$O(n)$** ← NOT $O(n \log n)$ |
| Heap sort | $O(n \log n)$ |
| $k$-th smallest (min-heap) | $O(k \log k)$ |
| Find min in max-heap | $O(n)$ — must check leaves |

**Array indexing (1-based):** Parent = $\lfloor i/2 \rfloor$, Left = $2i$, Right = $2i+1$

---

## 9. Hashing Quick Reference ⭐

**Chaining:** Average search = $O(1 + \alpha)$ where $\alpha = n/m$

**Open Addressing:**

| Method | Probe formula | Clustering |
|--------|--------------|-----------|
| Linear | $(h(k)+i) \bmod m$ | Primary clustering |
| Quadratic | $(h(k)+c_1 i+c_2 i^2) \bmod m$ | Secondary clustering |
| Double | $(h_1(k)+i \cdot h_2(k)) \bmod m$ | No clustering |

**Expected probes (open addressing, uniform):**
- Unsuccessful: $\frac{1}{1-\alpha}$
- Successful: $\frac{1}{\alpha}\ln\frac{1}{1-\alpha}$

**Key rules:**
- Deletion needs **lazy delete** in open addressing
- Table size should be **prime** (especially for quadratic probing)
- $h_2(k) \neq 0$ in double hashing

---

## 9a. B-Tree Quick Reference ⭐

B-Tree of minimum degree $t$:

| Property | Value |
|----------|-------|
| Min keys per node (non-root) | $t - 1$ |
| Max keys per node | $2t - 1$ |
| Min children (non-root) | $t$ |
| Max children | $2t$ |
| Root min keys | $1$ |
| Height | $O(\log_t n)$ |
| Search / Insert / Delete | $O(t \cdot \log_t n)$ |
| Disk accesses per operation | $O(\log_t n)$ |

**Height bound:** $h \leq \log_t \frac{n+1}{2}$

**Insert:** If node full ($2t-1$ keys), split into two nodes of $t-1$ keys and push median up.

**Delete cases:** (i) Key in leaf → direct remove; (ii) Key in internal → replace with predecessor/successor; (iii) Underflow → borrow from sibling or merge.

---

## 9b. B+ Tree Quick Reference ⭐

| Feature | B-Tree | B+ Tree |
|---------|--------|--------|
| Data stored at | All nodes | Leaves only |
| Leaf linked list | No | Yes (sequential access) |
| Internal keys | Actual keys | Copy of keys (separators) |
| Range queries | Slow | Fast (follow leaf pointers) |
| Duplicate keys | Not allowed | Allowed in leaves |

**Order $p$:** Max $p$ children, max $p-1$ keys per internal node.
**Leaf order $p_L$:** Max $p_L$ data pointers per leaf.
**Min fill:** Internal: $\lceil p/2 \rceil$ children, Leaf: $\lceil p_L/2 \rceil$ records.

---

## 9c. Trie Quick Reference

| Operation | Time | Space |
|-----------|------|-------|
| Search | $O(L)$ | — |
| Insert | $O(L)$ | $O(\Sigma \cdot L)$ per word |
| Delete | $O(L)$ | — |
| Prefix search | $O(L + k)$ | — |

where $L$ = key length, $\Sigma$ = alphabet size, $k$ = number of matches.

**Compressed Trie (Patricia):** Merge single-child chains → $O(n)$ nodes for $n$ strings.

**Applications:** Autocomplete, spell-checking, IP routing (longest prefix match), dictionary.

---

## 10. Sorting Algorithm Master Table ⭐⭐

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|----------|
| Bubble | $O(n)$* | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ✓ | ✓ |
| Insertion | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ✓ | ✓ |
| Selection | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ✗ | ✓ |
| Merge | $O(n\log n)$ | $O(n\log n)$ | $O(n\log n)$ | $O(n)$ | ✓ | ✗ |
| Quick | $O(n\log n)$ | $O(n\log n)$ | $O(n^2)$ | $O(\log n)$ | ✗ | ✓ |
| Heap | $O(n\log n)$ | $O(n\log n)$ | $O(n\log n)$ | $O(1)$ | ✗ | ✓ |
| Counting | $O(n+k)$ | $O(n+k)$ | $O(n+k)$ | $O(n+k)$ | ✓ | ✗ |
| Radix | $O(d(n+k))$ | $O(d(n+k))$ | $O(d(n+k))$ | $O(n+k)$ | ✓ | ✗ |

*With early termination.

**Key sorting facts:**
- Lower bound for comparison sorts: $\Omega(n \log n)$ (decision tree)
- Min comparisons for $n$ elements: $\lceil \log_2(n!) \rceil$
- Min swaps to sort = $n - \text{(number of cycles)}$
- Inversions in reverse-sorted array: $n(n-1)/2$
- Counting inversions: modified merge sort $O(n \log n)$

---

## 11. Graph Algorithm Complexities ⭐⭐

| Algorithm | Time | Space | Notes |
|-----------|------|-------|-------|
| BFS | $O(V+E)$ | $O(V)$ | Queue, unweighted shortest path |
| DFS | $O(V+E)$ | $O(V)$ | Stack/recursion |
| Topological Sort | $O(V+E)$ | $O(V)$ | DAG only |
| Dijkstra (bin heap) | $O((V+E)\log V)$ | $O(V)$ | Non-negative weights |
| Dijkstra (Fib heap) | $O(E+V\log V)$ | $O(V)$ | Best for dense |
| Dijkstra (array) | $O(V^2)$ | $O(V)$ | Dense graphs |
| Bellman-Ford | $O(VE)$ | $O(V)$ | Handles negative weights |
| DAG shortest path | $O(V+E)$ | $O(V)$ | Topo sort + relax |
| Floyd-Warshall | $O(V^3)$ | $O(V^2)$ | All-pairs |
| Kruskal | $O(E\log V)$ | $O(V)$ | Uses Union-Find |
| Prim (bin heap) | $O((V+E)\log V)$ | $O(V)$ | |
| Prim (Fib heap) | $O(E+V\log V)$ | $O(V)$ | |
| SCC (Kosaraju/Tarjan) | $O(V+E)$ | $O(V)$ | |

---

## 12. DFS Edge Classification ⭐

| Edge $(u,v)$ | Color of $v$ | Time Relation |
|-------------|-------------|--------------|
| Tree | WHITE | $d[u] < d[v]$ |
| Back | GRAY | $d[v] < d[u] < f[u] < f[v]$ |
| Forward | BLACK, desc | $d[u] < d[v] < f[v] < f[u]$ |
| Cross | BLACK, non-desc | $d[v] < f[v] < d[u]$ |

- **Undirected graphs:** Only tree + back edges (no forward/cross)
- **Cycle exists ↔ back edge exists**
- **DAG ↔ no back edges**

---

## 13. Shortest Path Decision Table

| Scenario | Algorithm | Time |
|----------|----------|------|
| Unweighted | BFS | $O(V+E)$ |
| Non-negative weights | Dijkstra | $O((V+E)\log V)$ |
| Negative weights (no neg cycle) | Bellman-Ford | $O(VE)$ |
| DAG (any weights) | Topo sort + relax | $O(V+E)$ |
| All pairs | Floyd-Warshall | $O(V^3)$ |
| All pairs (no neg weights) | Dijkstra from each vertex | $O(V(V+E)\log V)$ |
| Detect negative cycle | Bellman-Ford ($V$-th pass) | $O(VE)$ |

---

## 14. MST Key Properties ⭐

- MST has $V - 1$ edges
- **Distinct weights → unique MST**
- **Cut property:** lightest edge crossing a cut is in MST
- **Cycle property:** heaviest edge in a cycle is NOT in MST
- Adding edge to MST creates cycle; remove heaviest → new MST

| Algorithm | Best for | Time |
|-----------|----------|------|
| Kruskal | Sparse ($E \sim V$) | $O(E \log V)$ |
| Prim | Dense ($E \sim V^2$) | $O(E + V \log V)$ |

---

## 14a. Strongly Connected Components (SCC) ⭐

**Kosaraju's Algorithm (2-pass DFS):**
1. DFS on original graph → record finish times
2. Transpose graph (reverse all edges)
3. DFS on transposed graph in decreasing finish-time order → each DFS tree = one SCC

**Tarjan's Algorithm (1-pass DFS):**
- Uses `disc[]` (discovery time) and `low[]` (lowest reachable ancestor)
- Node $u$ is SCC root if `disc[u] == low[u]`
- Pop stack up to $u$ → one SCC

| Property | Fact |
|----------|------|
| Time | $O(V+E)$ both algorithms |
| Condensation graph | DAG of SCCs |
| \# SCCs in DAG | = \# SCCs (trivially) |
| Single SCC | Entire graph is strongly connected |
| Applications | 2-SAT, deadlock detection, reachability |

---

## 14b. Network Flow Quick Reference ⭐

**Max-Flow Min-Cut Theorem:** Max flow = Min cut capacity

| Algorithm | Time |
|-----------|------|
| Ford-Fulkerson (DFS) | $O(E \cdot f^*)$ where $f^*$ = max flow |
| Edmonds-Karp (BFS) | $O(VE^2)$ |
| Dinic's | $O(V^2 E)$ |

**Key concepts:**
- Residual graph: forward edges (remaining capacity) + backward edges (flow to cancel)
- Augmenting path: $s \to t$ path in residual graph with positive capacity
- Min cut: partition $(S, T)$ with $s \in S, t \in T$ minimizing $\sum c(u,v)$ for $u \in S, v \in T$

**Applications:** Bipartite matching, edge-disjoint paths, project selection, baseball elimination.

---

## 14c. Disjoint Set / Union-Find ⭐

| Operation | Without optimization | With Union by Rank + Path Compression |
|-----------|---------------------|--------------------------------------|
| MakeSet | $O(1)$ | $O(1)$ |
| Find | $O(n)$ | $O(\alpha(n))$ ≈ $O(1)$ |
| Union | $O(n)$ | $O(\alpha(n))$ ≈ $O(1)$ |

where $\alpha(n)$ = inverse Ackermann function (≤ 4 for all practical $n$).

**$m$ operations on $n$ elements:** $O(m \cdot \alpha(n))$ total.

**Applications:** Kruskal's MST, cycle detection, connected components, equivalence classes.

---

## 14d. Amortized Analysis ⭐

| Method | Idea |
|--------|------|
| Aggregate | Total cost / \# operations |
| Accounting | Charge extra for cheap ops, save credit for expensive ones |
| Potential | $\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$ |

**Classic examples:**

| Data Structure | Worst case | Amortized |
|---------------|-----------|----------|
| Dynamic array (doubling) | $O(n)$ resize | $O(1)$ insert |
| Stack with multipop | $O(n)$ multipop | $O(1)$ per op |
| Binary counter increment | $O(k)$ bits flip | $O(1)$ per increment |
| Splay tree | $O(n)$ single op | $O(\log n)$ per op |
| Fibonacci heap decrease-key | $O(1)$ amortized | — |

**Dynamic array proof (Aggregate):** $n$ insertions cost $n + \sum_{i=0}^{\lfloor\log n\rfloor} 2^i = n + 2n - 1 < 3n \Rightarrow O(1)$ amortized.

---

## 15. Greedy Algorithms Summary ⭐

| Problem | Greedy Strategy | Complexity |
|---------|----------------|-----------|
| Huffman coding | Merge two smallest frequencies | $O(n \log n)$ |
| Activity selection | Sort by finish time, pick earliest | $O(n \log n)$ |
| Job scheduling | Sort by profit, latest available slot | $O(n^2)$ or $O(n \log n)$ with UF |
| Fractional knapsack | Sort by value/weight ratio | $O(n \log n)$ |
| Dijkstra | Pick closest unvisited vertex | $O((V+E) \log V)$ |
| Kruskal / Prim | Pick minimum weight edge/vertex | MST time |

**Greedy fails for:** 0-1 knapsack, LCS, matrix chain, coin change (general), all-pairs shortest path.

---

## 16. Dynamic Programming Summary ⭐⭐

| Problem | Recurrence | Time | Space |
|---------|-----------|------|-------|
| LCS | $\max(L[i-1,j], L[i,j-1])$ or +1 | $O(mn)$ | $O(mn)$ |
| Matrix Chain | $\min_k(m[i,k]+m[k+1,j]+p_{i-1}p_k p_j)$ | $O(n^3)$ | $O(n^2)$ |
| 0-1 Knapsack | $\max(K[i-1,w], K[i-1,w-w_i]+v_i)$ | $O(nW)$* | $O(nW)$ |
| Floyd-Warshall | $\min(D[i,j], D[i,k]+D[k,j])$ | $O(V^3)$ | $O(V^2)$ |
| LIS | $\max(L[j]+1)$ for $j < i, A[j] < A[i]$ | $O(n^2)$† | $O(n)$ |
| Edit Distance | $\min(\text{ins, del, rep})$ | $O(mn)$ | $O(mn)$ |
| Coin Change | $\min(dp[s-c_i]+1)$ | $O(nS)$ | $O(S)$ |
| Optimal BST | $\min_r(e[i,r-1]+e[r+1,j]+w(i,j))$ | $O(n^3)$ | $O(n^2)$ |

*Pseudo-polynomial. †Improvable to $O(n \log n)$.

**DP vs Greedy Quick Test:**
- If optimal substructure + greedy choice property → **Greedy**
- If optimal substructure + overlapping subproblems → **DP**

---

## 17. Divide & Conquer Summary

| Algorithm | Recurrence | Complexity |
|-----------|-----------|-----------|
| Binary Search | $T(n/2) + O(1)$ | $O(\log n)$ |
| Merge Sort | $2T(n/2) + O(n)$ | $O(n \log n)$ |
| Quick Sort (avg) | $2T(n/2) + O(n)$ | $O(n \log n)$ |
| Quick Sort (worst) | $T(n-1) + O(n)$ | $O(n^2)$ |
| Closest Pair | $2T(n/2) + O(n)$ | $O(n \log n)$ |
| Inversions | $2T(n/2) + O(n)$ | $O(n \log n)$ |
| Select (median of medians) | $T(n/5)+T(7n/10)+O(n)$ | $O(n)$ |
| Strassen's | $7T(n/2) + O(n^2)$ | $O(n^{2.81})$ |

---

## 18. Array Address Formulas

**Row Major ($A[l_1..u_1][l_2..u_2]$, element size $w$, base $B$):**
$$B + [(i-l_1)(u_2-l_2+1) + (j-l_2)] \times w$$

**Column Major:**
$$B + [(j-l_2)(u_1-l_1+1) + (i-l_1)] \times w$$

**0-indexed $A[M][N]$:**
- Row: $B + (iN + j)w$
- Column: $B + (jM + i)w$

---

## 19. Critical GATE Traps — Do NOT Get Wrong ⭐⭐

| Common Mistake | Correct Answer |
|---------------|----------------|
| Build heap = $O(n \log n)$ | **$O(n)$** |
| Preorder + Postorder → unique tree | **Only for full binary trees** |
| Dijkstra works with negative weights | **FAILS with negative weights** |
| Quick sort always $O(n \log n)$ | **Worst: $O(n^2)$** |
| AVL delete: max 2 rotations | **Up to $O(\log n)$** |
| Geometric loop sum = $O(n \log n)$ | **$O(n)$ — GP sum** |
| 0-1 knapsack is polynomial | **Pseudo-polynomial: $O(nW)$** |
| \# BSTs with $n$ nodes = $2^n$ | **Catalan: $C_n$** |
| Hash: empty slot on delete | **Lazy deletion needed** |
| Undirected DFS has forward edges | **Only tree + back edges** |
| Fractional knapsack = DP | **Greedy** |
| Selection sort best case < $O(n^2)$ | **Always $O(n^2)$ comparisons** |
| Red-Black insert = many rotations | **At most 2 rotations** (+ recoloring) |
| B-Tree order $m$ = max $m$ keys | **Max $m$ children, $m-1$ keys** |
| Union-Find = $O(\log n)$ per op | **$O(\alpha(n)) \approx O(1)$ with both optimizations** |
| Ford-Fulkerson always terminates | **May not terminate with irrational capacities** |
| Amortized = average case | **Amortized \u2260 average; it's worst-case guarantee over sequence** |

---

## 20. Key Formulas Sheet

$$C_n = \frac{1}{n+1}\binom{2n}{n} \quad \text{(Catalan number)}$$

$$\sum_{i=1}^{n} i = \frac{n(n+1)}{2}, \quad \sum_{i=1}^{n} i^2 = \frac{n(n+1)(2n+1)}{6}$$

$$\sum_{i=0}^{k} 2^i = 2^{k+1} - 1, \quad \sum_{i=0}^{k} ar^i = a\frac{r^{k+1}-1}{r-1}$$

$$\sum_{i=1}^{n} \frac{1}{i} = \ln n + \gamma \approx \ln n + 0.577 = O(\log n)$$

$$\log_2(n!) = \Theta(n \log n) \quad \text{(Stirling)}$$

$$n! \approx \sqrt{2\pi n}\left(\frac{n}{e}\right)^n \quad \text{(Stirling's approximation)}$$

**Minimum AVL nodes:** $N(h) = N(h-1) + N(h-2) + 1$
$$N(0)=1,\; N(1)=2,\; N(2)=4,\; N(3)=7,\; N(4)=12,\; N(5)=20,\; N(6)=33$$

---

## 21. One-Line Algorithm Summaries

| Algorithm | One-liner |
|-----------|----------|
| **BFS** | Queue-based, level by level, shortest path in unweighted |
| **DFS** | Stack/recursion, goes deep first, timestamps for edge classification |
| **Dijkstra** | Greedy: always extend shortest known path (min-heap) |
| **Bellman-Ford** | Relax ALL edges $V-1$ times; $V$-th pass detects neg cycle |
| **Floyd-Warshall** | Try each vertex as intermediate: $D[i][j] = \min(D[i][j], D[i][k]+D[k][j])$ |
| **Kruskal** | Sort edges, greedily add if no cycle (Union-Find) |
| **Prim** | Grow MST from single vertex, always add cheapest edge to frontier |
| **Huffman** | Repeatedly merge two smallest frequencies |
| **LCS** | Match → diagonal+1, mismatch → max(left, up) |
| **MCM** | Try all split points $k$: cost = left + right + multiply |
| **0-1 Knapsack** | Take or skip: $\max(K[i-1,w], K[i-1,w-w_i]+v_i)$ |
| **Optimal BST** | Minimize weighted search cost: try each root $r$, DP on left/right subtrees |
| **SCC (Kosaraju)** | DFS for finish times → transpose → DFS in reverse finish order |
| **SCC (Tarjan)** | Single DFS with `low[]` array; SCC root when `disc[u] == low[u]` |
| **Ford-Fulkerson** | Find augmenting path in residual graph, augment flow, repeat |
| **Union-Find** | MakeSet, Find (path compression), Union (by rank) — nearly $O(1)$ amortized |

---

> **Final advice:** In the last 2 days before GATE, revise this file + the GATE traps table. Practice 2-3 DSA questions daily. Time complexity analysis appears in almost EVERY GATE paper — master asymptotic analysis cold.
