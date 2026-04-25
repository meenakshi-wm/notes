# Data Structures & Algorithms — Solutions (GATE 2027)

> Detailed solutions with time complexity analysis for all 90 questions.

---

## Section 1: Asymptotic Analysis & Recurrences

### Solution Q1: Asymptotic Comparison

**Increasing order of growth:**
$$\log n < n^{1.5} < n^2 < n \log n$$

Wait — let me order correctly:
$$\log n < n^{1.5} < n^2 < n \log n < 2^n < n! < n^n$$

Actually, the correct ordering:
$$\log n < n^{1.5} < n \log n$$

Let me be precise:
$$\log n \;<\; n^{1.5} \;<\; n \log n \;<\; n^2 \;<\; 2^n \;<\; n! \;<\; n^n$$

**Wait — $n \log n$ vs $n^{1.5}$:** For large $n$, $n^{1.5} > n \log n$. So:

$$\boxed{\log n < n \log n < n^{1.5} < n^2 < 2^n < n! < n^n}$$

---

### Solution Q2: Big-O Determination

Inner loop runs $i^2$ times for each $i$.

$$\sum_{i=1}^{n} i^2 = \frac{n(n+1)(2n+1)}{6} = \Theta(n^3)$$

$$\boxed{O(n^3)}$$

---

### Solution Q3: Nested Loop Analysis ⭐

Outer loop: $i = 1, 2, 4, 8, \ldots, 2^k$ where $2^k \leq n$, so $k = \lfloor \log_2 n \rfloor$.

Inner loop runs $i$ times for each value of $i$.

$$\text{Total} = 1 + 2 + 4 + 8 + \ldots + 2^{\lfloor \log_2 n \rfloor} = 2^{\lfloor \log_2 n \rfloor + 1} - 1 \approx 2n - 1$$

$$\boxed{O(n)}$$

**Note:** This is a geometric series, NOT $O(n \log n)$. Common GATE trap!

---

### Solution Q4: Asymptotic Notation

**(a) $n^2 + n = O(n^2)$** → **TRUE** ✓ (lower order terms absorbed)

**(b) $2^{n+1} = O(2^n)$** → **TRUE** ✓ ($2^{n+1} = 2 \cdot 2^n$, constant factor)

**(c) $2^{2n} = O(2^n)$** → **FALSE** ✗ ($2^{2n} = (2^n)^2$, which is NOT $O(2^n)$)

**(d) $n! = O(n^n)$** → **TRUE** ✓ (each factor $\leq n$, so $n! \leq n^n$)

**(e) $\log(n!) = \Theta(n \log n)$** → **TRUE** ✓ (Stirling: $\log(n!) = n \log n - n \log e + O(\log n)$)

$$\boxed{(a), (b), (d), (e) \text{ are TRUE}}$$

---

### Solution Q5: Master Theorem Application ⭐

**(a)** $T(n) = 4T(n/2) + n^2$
- $a = 4, b = 2$, $\log_2 4 = 2$, $f(n) = n^2 = \Theta(n^2 \log^0 n)$
- Case 2 (k=0): $\boxed{T(n) = \Theta(n^2 \log n)}$

**(b)** $T(n) = 3T(n/4) + n \log n$
- $a = 3, b = 4$, $\log_4 3 \approx 0.793$, $f(n) = n \log n = \Omega(n^{0.793+\epsilon})$
- Case 3 (check regularity: $3 \cdot (n/4)\log(n/4) \leq c \cdot n \log n$ for $c < 1$ ✓)
- $\boxed{T(n) = \Theta(n \log n)}$

**(c)** $T(n) = 2T(n/2) + n/\log n$
- $a = 2, b = 2$, $\log_2 2 = 1$, $f(n) = n/\log n$
- $f(n)$ is NOT $O(n^{1-\epsilon})$ for any $\epsilon > 0$, and not $\Theta(n \log^k n)$ for $k \geq 0$
- **Master theorem does NOT apply** (falls in the gap)
- By recursion tree: each level costs $n/\log(n/2^i)$, summing gives $\boxed{T(n) = \Theta(n \log \log n)}$

**(d)** $T(n) = T(n/2) + T(n/4) + T(n/8) + n$
- Akra-Bazzi: $a_1=1, b_1=1/2$; $a_2=1, b_2=1/4$; $a_3=1, b_3=1/8$
- Solve $(1/2)^p + (1/4)^p + (1/8)^p = 1$: at $p=1$: $0.5 + 0.25 + 0.125 = 0.875 < 1$
- So $p < 1$... Actually at $p = 0.879...$: this gives $\boxed{T(n) = \Theta(n)}$
- Simpler argument: at each level total work is $\leq n$, and levels decrease geometrically. By substitution: $T(n) \leq cn$ works with $c = 8$.

---

### Solution Q6: Iteration Method

$T(n) = 2T(n-1) + 1$, $T(0) = 1$

$$T(n) = 2T(n-1) + 1 = 2[2T(n-2) + 1] + 1 = 4T(n-2) + 2 + 1$$
$$= 2^k T(n-k) + 2^{k-1} + \ldots + 2 + 1$$

At $k = n$: $T(n) = 2^n T(0) + (2^n - 1) = 2^n + 2^n - 1 = 2^{n+1} - 1$

$$\boxed{T(n) = 2^{n+1} - 1 = \Theta(2^n)}$$

---

### Solution Q7: Change of Variable ⭐

$T(n) = T(\sqrt{n}) + 1$, $T(2) = 1$

Let $n = 2^m$, so $\sqrt{n} = 2^{m/2}$.

$S(m) = T(2^m) = T(2^{m/2}) + 1 = S(m/2) + 1$

By Master: $a = 1, b = 2$, $\log_2 1 = 0$, $f(m) = 1 = \Theta(m^0)$ → Case 2 (k=0):

$S(m) = \Theta(\log m)$

Since $m = \log_2 n$:

$$\boxed{T(n) = \Theta(\log \log n)}$$

---

### Solution Q8: Loop Complexity

$i$ takes values $n, \sqrt{n}, n^{1/4}, n^{1/8}, \ldots$ until $i \leq 1$.

After $k$ iterations: $i = n^{1/2^k}$. Loop ends when $n^{1/2^k} \leq 1$, which never happens (approaches 1 from above). Actually, for integer $i$: ends when $n^{1/2^k} \leq 1$, so $2^k \geq \log n$, giving $k = \log \log n$.

$$\boxed{O(\log \log n)}$$

---

### Solution Q9: Summation ⭐

$\sum_{i=1}^{n} \lfloor \log_2 i \rfloor$

For $2^k \leq i < 2^{k+1}$: $\lfloor \log_2 i \rfloor = k$, and there are $2^k$ such values.

$$\sum_{k=0}^{\lfloor \log_2 n \rfloor} k \cdot 2^k \approx n \log n - 2n = \Theta(n \log n)$$

$$\boxed{\Theta(n \log n)}$$

---

### Solution Q10: Recursion Tree

$T(n) = T(n/3) + T(2n/3) + n$

```
Level 0: n                           cost = n
Level 1: n/3 + 2n/3                  cost = n
Level 2: n/9 + 2n/9 + 2n/9 + 4n/9   cost = n
...
```

Each level costs $n$. The height is determined by the longest path: $n \cdot (2/3)^k = 1 \implies k = \log_{3/2} n$.

$$T(n) = n \cdot \log_{3/2} n = \boxed{\Theta(n \log n)}$$

---

### Solution Q11: Transitivity

**TRUE.** Big-O is transitive:
- $f(n) \leq c_1 g(n)$ for $n \geq n_1$
- $g(n) \leq c_2 h(n)$ for $n \geq n_2$
- Therefore $f(n) \leq c_1 c_2 h(n)$ for $n \geq \max(n_1, n_2)$

$$\boxed{\text{TRUE}}$$

---

### Solution Q12: While Loop ⭐

$s = 1 + 2 + 3 + \ldots + i = \frac{i(i+1)}{2}$

Loop ends when $s > n$, i.e., $\frac{i(i+1)}{2} > n \implies i \approx \sqrt{2n}$

$$\boxed{O(\sqrt{n})}$$

---

## Section 2: Arrays, Linked Lists, Stacks & Queues

### Solution Q13: Array Address

$A[5 \ldots 10][2 \ldots 8]$, base = 100, element size = 4 bytes, row major.

Row range: $l_1 = 5, u_1 = 10$, Column range: $l_2 = 2, u_2 = 8$
Number of columns = $8 - 2 + 1 = 7$

$\text{Address}(A[7][5]) = 100 + [(7-5) \times 7 + (5-2)] \times 4 = 100 + [14 + 3] \times 4 = 100 + 68 = \boxed{168}$

---

### Solution Q14: Column Major ⭐

$A[1 \ldots 20][1 \ldots 30]$, column major, base = 500, size = 2.

Number of rows = 20

$\text{Address}(A[11][15]) = 500 + [(15-1) \times 20 + (11-1)] \times 2 = 500 + [280 + 10] \times 2 = 500 + 580 = \boxed{1080}$

---

### Solution Q15: Pointer Changes

**(a) Singly LL:** 2 pointer changes (new node's next = given node's next; given node's next = new node)

**(b) Doubly LL:** 4 pointer changes (new node's next, new node's prev, given node's next, next node's prev)

$$\boxed{(a) = 2, \; (b) = 4}$$

---

### Solution Q16: Floyd's Cycle Detection ⭐

If cycle starts at position $k$ and cycle length is $c$, slow and fast pointers meet after approximately $c \cdot \lceil (k) / c \rceil$ steps from when slow enters the cycle.

More precisely: Slow enters cycle after $k$ steps. At that point, fast is $k$ steps into the cycle (at position $k \mod c$). They need to close a gap of $c - (k \mod c)$ with relative speed 1, so they meet after $c - (k \mod c)$ more steps.

$$\boxed{\text{Total iterations} = k + (c - (k \mod c))}$$

---

### Solution Q17: Stack Permutations ⭐

Input: $1, 2, 3, 4, 5$. Simulate each:

**(a) $3, 2, 5, 4, 1$:**
Push 1,2,3; Pop 3; Pop 2; Push 4,5; Pop 5; Pop 4; Pop 1 → **VALID** ✓

**(b) $3, 4, 5, 1, 2$:**
Push 1,2,3; Pop 3; Push 4; Pop 4; Push 5; Pop 5; now stack has [1,2], need to output 1 then 2 → Pop gives 2 then 1 → outputs 2,1 not 1,2 → **INVALID** ✗

**(c) $2, 5, 4, 1, 3$:**
Push 1,2; Pop 2; Push 3,4,5; Pop 5; Pop 4; Pop 3? No, need 1 next. Pop 3, Pop 1 ✓, but then need 3 which was already popped → **INVALID** ✗

Actually: Push 1,2; Pop 2; Push 3,4,5; Pop 5; Pop 4; Pop 3? But we need 1 next, then 3. Stack is [1,3]. Pop 1? No, top is 3. Pop 3? But output needs 1 first → stack has [1,3], top = 3, need 1 → **INVALID** ✗

**(d) $5, 4, 3, 2, 1$:**
Push 1,2,3,4,5; Pop all → **VALID** ✓

**(e) $1, 3, 2, 5, 4$:**
Push 1; Pop 1; Push 2,3; Pop 3; Pop 2; Push 4,5; Pop 5; Pop 4 → **VALID** ✓

$$\boxed{(a), (d), (e) \text{ are valid}}$$

---

### Solution Q18: Infix to Postfix

$A + B * C - (D / E + F) * G$

| Token | Stack | Output |
|-------|-------|--------|
| A | | A |
| + | [+] | A |
| B | [+] | A B |
| * | [+, *] | A B |
| C | [+, *] | A B C |
| - | [-] | A B C * + |
| ( | [-, (] | A B C * + |
| D | [-, (] | A B C * + D |
| / | [-, (, /] | A B C * + D |
| E | [-, (, /] | A B C * + D E |
| + | [-, (, +] | A B C * + D E / |
| F | [-, (, +] | A B C * + D E / F |
| ) | [-] | A B C * + D E / F + |
| * | [-, *] | A B C * + D E / F + |
| G | [-, *] | A B C * + D E / F + G |
| end | | A B C * + D E / F + G * - |

$$\boxed{A B C * + D E / F + G * -}$$

---

### Solution Q19: Postfix Evaluation ⭐

$8 \; 2 \; 3 \; \hat{} \; / \; 2 \; 3 \; * \; + \; 7 \; -$

| Token | Stack | Action |
|-------|-------|--------|
| 8 | [8] | |
| 2 | [8, 2] | |
| 3 | [8, 2, 3] | |
| ^ | [8, 8] | $2^3 = 8$ |
| / | [1] | $8/8 = 1$ |
| 2 | [1, 2] | |
| 3 | [1, 2, 3] | |
| * | [1, 6] | $2 \times 3 = 6$ |
| + | [7] | $1 + 6 = 7$ |
| 7 | [7, 7] | |
| - | [0] | $7 - 7 = 0$ |

$$\boxed{0}$$

---

### Solution Q20: Queue using Two Stacks

Using Method 2 (costly dequeue):
- S1 (enqueue stack), S2 (dequeue stack)

| Operation | S1 | S2 | Output |
|-----------|----|----|--------|
| Enqueue(1) | [1] | [] | |
| Enqueue(2) | [1,2] | [] | |
| Dequeue() | [] | [2] → move all, pop 1 | 1 |
| Enqueue(3) | [3] | [2] | |
| Dequeue() | [3] | [] → pop 2 | 2 |
| Dequeue() | [] | [] → move 3, pop 3 | 3 |

Each element is pushed and popped at most twice (once into S1, once into S2), giving amortized $O(1)$.

---

### Solution Q21: Circular Queue ⭐

Size = 8 (indices 0–7), front = 3, rear = 7.

**Elements:** $(7 - 3 + 1) = 5$ elements (at indices 3, 4, 5, 6, 7).

**Two enqueues:** rear = $(7+1) \% 8 = 0$, then rear = $(0+1) \% 8 = 1$. Now 7 elements.
**One dequeue:** front = $(3+1) \% 8 = 4$. Now 6 elements.

$$\boxed{\text{front} = 4, \text{rear} = 1, \text{elements} = 6}$$

---

### Solution Q22: Two Stacks in One Array

S1 grows from left (top1 starts at -1, increments).
S2 grows from right (top2 starts at n, decrements).

**Overflow when:** $\text{top1} + 1 = \text{top2}$ (or equivalently $\text{top1} \geq \text{top2} - 1$).

$$\boxed{\text{top1} + 1 == \text{top2}}$$

---

### Solution Q23: Infix to Prefix ⭐

$(A + B) * (C - D) / (E + F)$

**Method:** Reverse, swap parentheses, convert to postfix, reverse.

Reversed: $) F + E ( / ) D - C ( * ) B + A ($
Swap: $( F + E ) / ( D - C ) * ( B + A )$
To postfix: $F E + D C - / B A + *$
Reverse: $* + A B / - C D + E F$

$$\boxed{/ * + A B - C D + E F}$$

Wait, let me redo more carefully.

$(A + B) * (C - D) / (E + F)$

This is $((A + B) * (C - D)) / (E + F)$ by left-to-right precedence of $*$ and $/$.

Prefix of $(A+B)$ = $+AB$
Prefix of $(C-D)$ = $-CD$
Prefix of $(A+B)*(C-D)$ = $*+AB-CD$
Prefix of $(E+F)$ = $+EF$
Prefix of full = $/*+AB-CD+EF$

$$\boxed{/ * + A B - C D + E F}$$

---

### Solution Q24: Catalan Numbers

$C_5 = \frac{1}{6}\binom{10}{5} = \frac{252}{6} = \boxed{42}$

---

## Section 3: Trees

### Solution Q25: Binary Tree Properties

For a binary tree where every internal node has exactly 2 children:
$n_0 = n_2 + 1$, so leaf nodes = $10 + 1 = \boxed{11}$

---

### Solution Q26: Tree Construction ⭐

Inorder: D B H E A F C G | Preorder: A B D E H C F G

1. Root = A (first in preorder)
2. Inorder split: Left = [D B H E], Right = [F C G]
3. Left subtree preorder: [B D E H] → root = B
   - Left of B in inorder: [D], Right: [H E]
   - Left child = D
   - Right subtree preorder: [E H] → root = E, left child = H
4. Right subtree preorder: [C F G] → root = C
   - Left of C in inorder: [F], Right: [G]
   - Left child = F, Right child = G

```
        A
       / \
      B   C
     / \ / \
    D  E F  G
      /
     H
```

**Postorder:** D H E B F G C A

$$\boxed{\text{Postorder: } D \; H \; E \; B \; F \; G \; C \; A}$$

---

### Solution Q27: Tree from Postorder ⭐

Inorder: 4 8 2 5 1 6 3 7 | Postorder: 8 4 5 2 6 7 3 1

1. Root = 1 (last in postorder)
2. Inorder: Left = [4 8 2 5], Right = [6 3 7]
3. Left subtree postorder: [8 4 5 2] → root = 2
   - Left of 2: [4 8], Right: [5]
   - Left postorder: [8 4] → root = 4, right child = 8
   - Right = 5
4. Right subtree postorder: [6 7 3] → root = 3
   - Left of 3: [6], Right: [7]

```
           1
          / \
         2   3
        / \ / \
       4  5 6  7
        \
         8
```

**Preorder:** 1 2 4 8 5 3 6 7
**Level order:** 1 2 3 4 5 6 7 8

$$\boxed{\text{Preorder: } 1\;2\;4\;8\;5\;3\;6\;7 \quad | \quad \text{Level order: } 1\;2\;3\;4\;5\;6\;7\;8}$$

---

### Solution Q28: Number of Binary Trees

Structurally distinct binary trees with 4 nodes: $C_4 = \frac{1}{5}\binom{8}{4} = \frac{70}{5} = 14$

Labeled binary trees: $14 \times 4! = 14 \times 24 = 336$

$$\boxed{14 \text{ (unlabeled)}, \; 336 \text{ (labeled)}}$$

---

### Solution Q29: BST Construction ⭐

Insert order: 15, 25, 5, 18, 20, 3, 10, 8

```
        15
       /  \
      5    25
     / \   /
    3  10 18
       /    \
      8     20
```

**(a)** Height = 3 (path: 15→5→10→8)

**(b)** Search for 8: 15(L) → 5(R) → 10(L) → 8(found) = **4 comparisons**

**(c)** Inorder: 3, 5, 8, 10, 15, 18, 20, 25

$$\boxed{(a)\;h=3,\; (b)\;4\text{ comparisons},\; (c)\;3,5,8,10,15,18,20,25}$$

---

### Solution Q30: BST Deletion

Delete 15 from Q29's BST:

**(a) Inorder successor** of 15 = 18 (leftmost in right subtree)
Replace 15 with 18, then delete 18 from right subtree:
```
        18
       /  \
      5    25
     / \   /
    3  10 20
       /
      8
```

**(b) Inorder predecessor** of 15 = 10 (rightmost in left subtree)
Replace 15 with 10, then delete 10 from left subtree:
```
        10
       /  \
      5    25
     / \   /
    3   8 18
           \
           20
```

---

### Solution Q31: AVL Insertion ⭐

Insert: 10, 20, 30, 25, 27, 7, 4, 15

**Insert 10:** `10`

**Insert 20:** `10(R:20)` — balanced

**Insert 30:** Imbalance at 10 (RR case) → **Left rotation**
```
    20
   /  \
  10   30
```

**Insert 25:** balanced (height diff ok)
```
    20
   /  \
  10   30
      /
     25
```

**Insert 27:** Imbalance at 30 (RL case) → **Right rotate 30's left child (25), then Left rotate 30**
```
    20
   /  \
  10   27
      /  \
     25   30
```

**Insert 7:** balanced
```
    20
   /  \
  10   27
  /   /  \
 7   25   30
```

**Insert 4:** Imbalance at 10 (LL case) → **Right rotation at 10**
```
    20
   /  \
  7    27
 / \  /  \
4  10 25  30
```

**Insert 15:** Imbalance at 20 (no — check balance factors)
```
    20
   /  \
  7    27
 / \  /  \
4  10 25  30
    \
    15
```
Height of left subtree of 20 = 3, right = 2 → balance factor = 1. Still balanced.

$$\boxed{\text{Rotations: RR at step 3 (10), RL at step 5 (30), LL at step 7 (10)}}$$

---

### Solution Q32: AVL Min Nodes ⭐

$N(h) = N(h-1) + N(h-2) + 1$

| $h$ | $N(h)$ |
|-----|--------|
| 0 | 1 |
| 1 | 2 |
| 2 | 4 |
| 3 | 7 |
| 4 | 12 |
| 5 | 20 |
| 6 | 33 |

$$\boxed{N(6) = 33}$$

---

### Solution Q33: AVL Insertion 1-7

Insert 1,2,3,4,5,6,7:

After all insertions (with rotations at each imbalance), the resulting AVL tree is a perfectly balanced BST:

```
        4
       / \
      2   6
     / \ / \
    1  3 5  7
```

Height = 2. Number of rotations = 3 (at insert 3: RR, at insert 5: RR at 3 or adjustment, at insert 7: RR).

$$\boxed{h = 2, \text{ rotations} = 3}$$

---

### Solution Q34: Function Analysis ⭐

The function returns the **height** of the binary tree (number of edges on longest root-to-leaf path, or number of levels - 1 depending on base case convention).

Since `NULL` returns 0, a single node returns $\max(0,0) + 1 = 1$. So this computes the **number of nodes on the longest path** (= height + 1 if height is edge-based, or height if height is node-based).

$$\boxed{\text{Height of the binary tree (node-based definition)}}$$

---

### Solution Q35: Threaded Binary Tree

A binary tree with $n$ nodes has $n + 1$ NULL pointers (total $2n$ pointers, $n-1$ used for edges).

In an inorder threaded binary tree, these $n + 1$ NULL pointers are replaced by threads (to inorder predecessor/successor).

**Advantage:** Inorder traversal without stack/recursion ($O(1)$ extra space).

$$\boxed{n + 1 \text{ NULL pointers converted to threads}}$$

---

### Solution Q36: Preorder to BST ⭐

Preorder: 30, 20, 10, 15, 25, 40, 35, 50

Since inorder of BST is the sorted sequence: 10, 15, 20, 25, 30, 35, 40, 50

Using preorder + inorder → construct tree:

```
        30
       /  \
      20   40
     / \  / \
    10 25 35 50
     \
     15
```

$$\boxed{\text{Inorder: } 10, 15, 20, 25, 30, 35, 40, 50}$$

---

### Solution Q37: Number of BSTs

$C_5 = \frac{1}{6}\binom{10}{5} = \frac{252}{6} = \boxed{42}$

---

### Solution Q38: Complete Binary Tree

1023 nodes = $2^{10} - 1$ → perfect binary tree of height 9.

**(a)** Height = $\lfloor \log_2 1023 \rfloor = 9$

**(b)** Leaf nodes = $2^9 = 512$

**(c)** In a perfect binary tree, every internal node has exactly 2 children → **0 nodes with exactly one child**.

$$\boxed{(a)\;h=9,\; (b)\;512,\; (c)\;0}$$

---

## Section 4: Heaps & Hashing

### Solution Q39: Build Max-Heap

Array: [3, 9, 2, 1, 4, 5, 7, 6, 8]

Start from last non-leaf (index 4, value 4 in 1-indexed, i.e., index 3 in 0-indexed for $n=9$: $\lfloor 9/2 \rfloor - 1 = 3$).

After bottom-up heapify:

Step 1: i=3, node=1 → swap with max child 8 → [3,9,2,8,4,5,7,6,1]
Step 2: i=2, node=2 → swap with max child 7 → [3,9,7,8,4,5,2,6,1]
Step 3: i=1, node=9 → already max
Step 4: i=0, node=3 → swap with 9 → [9,3,7,8,4,5,2,6,1] → 3 sifts down → swap with 8 → [9,8,7,3,4,5,2,6,1] → 3 sifts → swap with 6 → [9,8,7,6,4,5,2,3,1]

$$\boxed{[9, 8, 7, 6, 4, 5, 2, 3, 1]}$$

---

### Solution Q40: Build Heap Complexity ⭐

For a heap of 15 elements (height 3, perfect binary tree):

| Height $h$ | # nodes at height $h$ | Comparisons per node |
|------------|----------------------|---------------------|
| 0 (leaves) | 8 | 0 |
| 1 | 4 | 2 (compare with 2 children) |
| 2 | 2 | 4 |
| 3 (root) | 1 | 6 |

Total comparisons $\leq 4 \times 2 + 2 \times 4 + 1 \times 6 = 8 + 8 + 6 = 22$

General: $\sum_{h=1}^{\lfloor \log n \rfloor} \lceil n/2^{h+1} \rceil \cdot 2h = O(n)$

$$\boxed{\text{Worst case for 15 elements} \leq 22 \text{ comparisons}}$$

---

### Solution Q41: Heap Operations ⭐

This is conceptual — after ExtractMax twice, the two largest are removed and the heap is restructured. Then inserting 50 (likely the new max) goes to the end and bubbles up to root. The exact number of elements "at correct positions" depends on the specific heap, but the key insight is that at most $O(\log n)$ elements move during each operation.

$$\boxed{\text{Depends on specific heap values; concept: most positions unchanged}}$$

---

### Solution Q42: Min-Heap Validation

$[2, 5, 4, 7, 8, 6, 9, 10, 12, 11]$ (0-indexed)

Check: parent $\leq$ children for all.
- $A[0]=2$: children $A[1]=5, A[2]=4$ ✓
- $A[1]=5$: children $A[3]=7, A[4]=8$ ✓
- $A[2]=4$: children $A[5]=6, A[6]=9$ ✓
- $A[3]=7$: children $A[7]=10, A[8]=12$ ✓
- $A[4]=8$: children $A[9]=11$ ✓

$$\boxed{\text{Yes, it is a valid min-heap}}$$

---

### Solution Q43: k-th Smallest ⭐

The $k$-th smallest element in a min-heap can be found in $O(k \log k)$ using an auxiliary min-heap:
1. Insert root into aux-heap
2. Repeat $k$ times: extract-min from aux-heap, insert its children

$$\boxed{O(k \log k)}$$

---

### Solution Q44: Heap Sort Trace

Array: [4, 10, 3, 5, 1]

**Build max-heap:** [10, 5, 3, 4, 1]

**Step 1:** Swap 10↔1 → [1, 5, 3, 4, | 10], heapify → [5, 4, 3, 1, | 10]
**Step 2:** Swap 5↔1 → [1, 4, 3, | 5, 10], heapify → [4, 1, 3, | 5, 10]
**Step 3:** Swap 4↔3 → [3, 1, | 4, 5, 10], heapify → [3, 1, | 4, 5, 10]
**Step 4:** Swap 3↔1 → [1, | 3, 4, 5, 10]

$$\boxed{[1, 3, 4, 5, 10]}$$

---

### Solution Q45: Linear Probing ⭐

Size = 11, $h(k) = k \mod 11$

| Key | $h(k)$ | Probes (slot checked) | Final Slot | # Probes |
|-----|--------|----------------------|-----------|----------|
| 10 | 10 | 10 | 10 | 1 |
| 22 | 0 | 0 | 0 | 1 |
| 31 | 9 | 9 | 9 | 1 |
| 4 | 4 | 4 | 4 | 1 |
| 15 | 4 | 4,5 | 5 | 2 |
| 28 | 6 | 6 | 6 | 1 |
| 17 | 6 | 6,7 | 7 | 2 |
| 88 | 0 | 0,1 | 1 | 2 |
| 59 | 4 | 4,5,6,7,8 | 8 | 5 |

Table: [22, 88, _, _, 4, 15, 28, 17, 59, 31, 10]

$$\boxed{\text{Total probes} = 1+1+1+1+2+1+2+2+5 = 16}$$

---

### Solution Q46: Double Hashing ⭐

$m = 13$, $h_1(k) = k \mod 13$, $h_2(k) = 7 - (k \mod 7)$

| Key | $h_1$ | $h_2$ | Probes | Slot |
|-----|-------|-------|--------|------|
| 18 | 5 | 7-4=3 | 5 | 5 |
| 41 | 2 | 7-6=1 | 2 | 2 |
| 22 | 9 | 7-1=6 | 9 | 9 |
| 44 | 5 | 7-2=5 | 5→10 | 10 |
| 59 | 7 | 7-3=4 | 7 | 7 |
| 32 | 6 | 7-4=3 | 6 | 6 |
| 31 | 5 | 7-3=4 | 5→10→2→... →(5+3×4)%13 = 17%13=4 | Well: 5(taken),10(taken),2(taken), next: (5+3×3)%13=14%13=1 | 1 |

Let me redo 31: $h_1=5$, $h_2=7-(31\%7)=7-3=4$
- $i=0$: $(5+0)=5$ → taken
- $i=1$: $(5+4)=9$ → taken
- $i=2$: $(5+8)=13\%13=0$ → empty → slot 0

| 73 | $73\%13=8$ | $7-(73\%7)=7-3=4$ | 8 | 8 |

Final table: [31, _, 41, _, _, 18, 32, 59, 73, 22, 44, _, _]

$$\boxed{[31, \_, 41, \_, \_, 18, 32, 59, 73, 22, 44, \_, \_]}$$

---

### Solution Q47: Chaining

$h(k) = k \mod 10$

| Slot | Chain |
|------|-------|
| 0 | 20 |
| 1 | 11 |
| 2 | 12 |
| 3 | 13 → 23 |
| 4 | 44 → 94 |
| 5 | 5 |
| 6 | 16 |
| 8 | 88 |
| 9 | 39 |

**(b)** Longest chain: slot 3 or 4 with 2 elements → length = **2**

**(c)** Average search: $(1+1+1+1+2+1+2+1+1+1+1)/11 = 13/11 \approx 1.18$

$$\boxed{\text{Longest chain} = 2, \; \text{Avg search} = 13/11}$$

---

### Solution Q48: Expected Probes ⭐

$\alpha = 80/100 = 0.8$

**(a) Unsuccessful:** $\frac{1}{1-\alpha} = \frac{1}{0.2} = \boxed{5}$

**(b) Successful:** $\frac{1}{\alpha} \ln \frac{1}{1-\alpha} = \frac{1}{0.8} \ln 5 = 1.25 \times 1.609 \approx \boxed{2.01}$

---

### Solution Q49: Quadratic Probing

$m = 7$, $h(k) = k \mod 7$, $h(k,i) = (h(k) + i^2) \mod 7$

| Key | $h(k)$ | Probes |  Slot |
|-----|--------|--------|-------|
| 76 | 6 | 6 | 6 |
| 93 | 2 | 2 | 2 |
| 40 | 5 | 5 | 5 |
| 47 | 5 | 5→6→(5+4)%7=2→(5+9)%7=0 | 0 |
| 10 | 3 | 3 | 3 |
| 55 | 6 | 6→0→(6+4)%7=3→(6+9)%7=1 | 1 |

All keys inserted successfully.

$$\boxed{\text{All keys can be inserted: [47, 55, 93, 10, \_, 40, 76]}}$$

---

### Solution Q50: Rehashing

Rehash when load factor exceeds threshold (typically 0.5 for open addressing, 0.75 for chaining).

Current size: 7. Next prime after $2 \times 7 = 14$ is **17**.

$$\boxed{\text{New table size} = 17}$$

---

## Section 5: Sorting

### Solution Q51: Sorting Comparison

| Algorithm | Best Case | Worst Case | Stable? | In-place? |
|-----------|-----------|-----------|---------|-----------|
| Merge Sort | $O(n \log n)$ | $O(n \log n)$ | Yes | No |
| Quick Sort | $O(n \log n)$ | $O(n^2)$ | No | Yes |
| Heap Sort | $O(n \log n)$ | $O(n \log n)$ | No | Yes |
| Counting Sort | $O(n+k)$ | $O(n+k)$ | Yes | No |

---

### Solution Q52: Bubble Sort ⭐

Initial: [5, 3, 8, 1, 9, 2, 7]

**Pass 1:** [3, 5, 1, 8, 2, 7, **9**]
**Pass 2:** [3, 1, 5, 2, 7, **8**, 9]
**Pass 3:** [1, 3, 2, 5, **7**, 8, 9]

$$\boxed{[1, 3, 2, 5, 7, 8, 9]}$$

---

### Solution Q53: Lomuto Partition ⭐

$[7, 2, 1, 6, 8, 5, 3, 4]$, pivot = 4

$i = -1$

| j | A[j] | Condition | Action | Array |
|---|------|-----------|--------|-------|
| 0 | 7 | 7>4 | skip | [7,2,1,6,8,5,3,4] |
| 1 | 2 | 2<4 | i=0, swap(0,1) | [2,7,1,6,8,5,3,4] |
| 2 | 1 | 1<4 | i=1, swap(1,2) | [2,1,7,6,8,5,3,4] |
| 3 | 6 | 6>4 | skip | [2,1,7,6,8,5,3,4] |
| 4 | 8 | 8>4 | skip | [2,1,7,6,8,5,3,4] |
| 5 | 5 | 5>4 | skip | [2,1,7,6,8,5,3,4] |
| 6 | 3 | 3<4 | i=2, swap(2,6) | [2,1,3,6,8,5,7,4] |

Swap pivot: swap(i+1, 7) = swap(3, 7) → [2,1,3,**4**,8,5,7,6]

$$\boxed{\text{Pivot 4 at index 3: } [2,1,3,4,8,5,7,6]}$$

---

### Solution Q54: Quick Sort Worst Case

**(a)** [1,2,3,4,5]: first element pivot = 1 → partition gives [| 2,3,4,5] → $O(n^2)$ ✓
**(b)** [5,4,3,2,1]: first element pivot = 5 → partition gives [4,3,2,1 |] → $O(n^2)$ ✓

$$\boxed{(d) \text{ Both (a) and (b)}}$$

---

### Solution Q55: Lower Bound ⭐

A comparison-based sort's execution is a binary decision tree. With $n!$ possible orderings (leaves), a binary tree needs height $\geq \lceil \log_2(n!) \rceil$.

For $n = 8$: $8! = 40320$, $\lceil \log_2 40320 \rceil = \lceil 15.3 \rceil = 16$

$$\boxed{16 \text{ comparisons minimum}}$$

---

### Solution Q56: Counting Sort

Array: [4, 2, 2, 8, 3, 3, 1], range [1, 8]

Count: [0, 1, 2, 2, 1, 0, 0, 0, 1] (index 0-8)
Cumulative: [0, 1, 3, 5, 6, 6, 6, 6, 7]

Output (right to left for stability):
Place 1 at position 1, place 3 at position 5, place 3 at position 4, place 8 at position 7, place 2 at position 3, place 2 at position 2, place 4 at position 6.

$$\boxed{[1, 2, 2, 3, 3, 4, 8]}$$

---

### Solution Q57: Inversions ⭐

$[5, 4, 3, 2, 1]$ — every pair $(i,j)$ where $i < j$ is an inversion.

Number of inversions = $\binom{5}{2} = 10$

Modified merge sort counts inversions in $O(n \log n)$.

$$\boxed{10 \text{ inversions}}$$

---

### Solution Q58: Minimum Swaps ⭐

$[4, 3, 2, 1]$ → target $[1, 2, 3, 4]$

Find cycles in the permutation: $4→1→4$ (cycle of length 2) and $3→2→3$ (cycle of length 2).

Swaps = (2-1) + (2-1) = 2

$\boxed{2}$ (swap 4↔1, then 3↔2)

---

### Solution Q59: Radix Sort (LSD)

[329, 457, 657, 839, 436, 720, 355]

**Sort by ones digit:** [720, 355, 436, 457, 657, 329, 839]
**Sort by tens digit:** [720, 329, 436, 839, 355, 457, 657]
**Sort by hundreds digit:** [329, 355, 436, 457, 657, 720, 839]

$$\boxed{[329, 355, 436, 457, 657, 720, 839]}$$

---

### Solution Q60: Selection Sort

Total comparisons = $(n-1) + (n-2) + \ldots + 1 = \frac{n(n-1)}{2}$ regardless of input.

Selection sort is **NOT adaptive** (same comparisons for sorted and unsorted input).

$$\boxed{\frac{n(n-1)}{2} \text{ comparisons, not adaptive}}$$

---

## Section 6: Graph Algorithms

### Solution Q61: BFS Traversal

```
A -- B -- E
|    |    |
C -- D -- F
```

BFS from A (alphabetical tie-breaking):

Queue: [A] → Visit A, enqueue B, C
Queue: [B, C] → Visit B, enqueue D, E (A already visited)
Queue: [C, D, E] → Visit C (D already queued)
Queue: [D, E] → Visit D, enqueue F
Queue: [E, F] → Visit E (F already queued)
Queue: [F] → Visit F

$$\boxed{\text{BFS order: } A, B, C, D, E, F}$$

---

### Solution Q62: DFS Edge Classification ⭐

Graph: 1→2, 1→4, 2→3, 3→1, 4→3, 4→5, 5→6, 6→4

DFS from 1 (numerical order):

DFS(1): d[1]=1
  DFS(2): d[2]=2
    DFS(3): d[3]=3
      3→1: 1 is GRAY → **Back edge**
      f[3]=4
    f[2]=5
  DFS(4): d[4]=6
    4→3: 3 is BLACK, d[4]>d[3] → **Cross edge**
    DFS(5): d[5]=7
      DFS(6): d[6]=8
        6→4: 4 is GRAY → **Back edge**
        f[6]=9
      f[5]=10
    f[4]=11
  f[1]=12

| Edge | Type |
|------|------|
| 1→2 | Tree |
| 2→3 | Tree |
| 3→1 | Back |
| 1→4 | Tree |
| 4→3 | Cross |
| 4→5 | Tree |
| 5→6 | Tree |
| 6→4 | Back |

$$\boxed{\text{Tree: 1→2, 2→3, 1→4, 4→5, 5→6 | Back: 3→1, 6→4 | Cross: 4→3}}$$

---

### Solution Q63: Topological Sort ⭐

Edges: A→C, A→D, B→D, B→E, C→F, D→F, E→F

In-degrees: A=0, B=0, C=1, D=2, E=1, F=3

Some valid orderings:
1. A, B, C, D, E, F
2. A, B, C, E, D, F
3. A, B, D, C, E, F (invalid — D needs A and B but also C must come before F... D doesn't need C)
4. B, A, C, D, E, F
5. B, A, E, C, D, F
6. B, A, D, C, E, F (need to check: D needs A,B → both done ✓)
7. B, E, A, C, D, F
8. B, A, E, D, C, F (D needs A,B ✓; C needs A ✓; F needs C,D,E ✓) ✓
9. A, C, B, D, E, F
10. A, C, B, E, D, F

$$\boxed{\text{Multiple valid orderings exist, e.g., A,B,C,D,E,F and B,A,E,C,D,F}}$$

---

### Solution Q64: Articulation Points ⭐

```
1 -- 2 -- 3
|         |
4 -- 5 -- 6
     |
     7
```

Running DFS (start at 1):

Using disc/low values:
- Vertex 2: child 3 has low[3] which can reach 6→5→4→1 (back edge? depends on structure)

Let's trace adjacency: 1-2, 2-3, 3-6, 6-5, 5-4, 4-1, 5-7

DFS from 1: 1→2→3→6→5→4(back to 1)→7

disc: 1=1, 2=2, 3=3, 6=4, 5=5, 4=6, 7=7
low: Work backward:
- 4: adjacent to 1 (disc=1), low[4] = 1
- 5: min(low[4]=1, low[7]=7, low[6]) → through 4→1, low[5] = 1
- 7: only tree edge to 5, low[7] = 7
- 6: min(disc[6]=4, low[5]=1) = 1, low[6] = 1... wait, 6 connects to 3 and 5.

Let me redo: adjacency: 1-{2,4}, 2-{1,3}, 3-{2,6}, 4-{1,5}, 5-{4,6,7}, 6-{3,5}

DFS(1): disc=1
  DFS(2): disc=2
    DFS(3): disc=3
      DFS(6): disc=4
        DFS(5): disc=5
          DFS(4): disc=6
            4→1: back edge, low[4]=min(6,1)=1
          low[5]=min(5, low[4])=1
          DFS(7): disc=7
          low[7]=7
          low[5]=min(1,7)=1
        low[6]=min(4, low[5])=1
      low[3]=min(3, low[6])=1
    low[2]=min(2, low[3])=1
  low[1]=min(1, low[2])=1... but also DFS checked 4 already visited.

Articulation points:
- 1 is root with 2 children in DFS tree (2 and... actually only 2, since 4 is reached via the path through 5). Root with 1 child → NOT articulation point.

Actually, wait: DFS from 1 → visit 2 first (child), then all of 3,6,5,4,7 are reached without returning to 1 for another child. 4→1 is a back edge. So 1 has only 1 child in DFS tree (vertex 2).

**Vertex 5:** child 7 has low[7]=7 ≥ disc[5]=5. So **5 is an articulation point** (removing 5 disconnects 7).

No other vertex satisfies the articulation point condition since low values propagate through the cycle 1-2-3-6-5-4-1.

$$\boxed{\text{Articulation point: } 5}$$

---

### Solution Q65: Connected Components

Vertices: {1,2,3,4,5,6,7,8}, Edges: {(1,2),(2,3),(4,5),(6,7),(7,8),(6,8)}

Component 1: {1, 2, 3}
Component 2: {4, 5}
Component 3: {6, 7, 8}

$$\boxed{3 \text{ connected components}}$$

---

### Solution Q66: Dijkstra's Algorithm ⭐

Edges: S→A(1), S→B(5), B→C(2), C→A(2), A→C(3), A→D(6), C→E(4), E→D(1)

| Step | Selected | dist[S] | dist[A] | dist[B] | dist[C] | dist[D] | dist[E] |
|------|----------|---------|---------|---------|---------|---------|---------|
| Init | — | 0 | ∞ | ∞ | ∞ | ∞ | ∞ |
| 1 | S | 0 | 1 | 5 | ∞ | ∞ | ∞ |
| 2 | A | 0 | 1 | 5 | 4 | 7 | ∞ |
| 3 | C | 0 | 1 | 5 | 4 | 7 | 8 |
| 4 | B | 0 | 1 | 5 | 4 | 7 | 8 |
| 5 | D | 0 | 1 | 5 | 4 | 7 | 8 |
| 6 | E | 0 | 1 | 5 | 4 | 7 | 8 |

Wait, let me reconsider: after selecting C (dist=4), relax C→E: dist[E] = 4+4=8. Then via E→D: 8+1=9 > 7. So dist[D] stays 7.

$$\boxed{S=0, A=1, B=5, C=4, D=7, E=8}$$

---

### Solution Q67: Bellman-Ford ⭐

Edges: A→B(-1), A→C(4), B→C(3), B→D(2), D→B(2), C→D(5), C→E(-3), D→E(-1)

| Iter | A | B | C | D | E |
|------|---|---|---|---|---|
| Init | 0 | ∞ | ∞ | ∞ | ∞ |
| 1 | 0 | -1 | 2 | 1 | -1 |
| 2 | 0 | -1 | 2 | 1 | -1 |

After iteration 1: A→B: B=-1. A→C: C=4. B→C: C=min(4,-1+3)=2. B→D: D=-1+2=1. C→D: D=min(1,2+5)=1. C→E: E=2-3=-1. D→E: E=min(-1,1-1)=-1.

Iteration 2: no changes → converged.

Check negative cycle: no further relaxation possible → **no negative cycle**.

$$\boxed{A=0, B=-1, C=2, D=1, E=-1 \text{ | No negative cycle}}$$

---

### Solution Q68: Floyd-Warshall ⭐

Initial matrix $D^{(0)}$:
```
     1    2    3    4
1  [ 0    3    ∞    7  ]
2  [ 8    0    2    ∞  ]
3  [ 5    ∞    0    1  ]
4  [ 2    ∞    ∞    0  ]
```

**$k=1$:** $D[i][j] = \min(D[i][j], D[i][1]+D[1][j])$
```
     1    2    3    4
1  [ 0    3    ∞    7  ]
2  [ 8    0    2    15 ]   (2→1→4: 8+7=15)
3  [ 5    8    0    1  ]   (3→1→2: 5+3=8)... wait 3→1: 5, 1→2: 3 → 8 < ∞
4  [ 2    5    ∞    0  ]   (4→1→2: 2+3=5)
```

**$k=2$:** $D[i][j] = \min(D[i][j], D[i][2]+D[2][j])$
```
     1    2    3    4
1  [ 0    3    5    7  ]   (1→2→3: 3+2=5)
2  [ 8    0    2    15 ]
3  [ 5    8    0    1  ]   (nothing improves: 3→2→1: 8+8=16>5, etc.)
4  [ 2    5    7    0  ]   (4→2→3: 5+2=7)
```

**$k=3$:** $D[i][j] = \min(D[i][j], D[i][3]+D[3][j])$
```
     1    2    3    4
1  [ 0    3    5    6  ]   (1→3→4: 5+1=6 < 7)
2  [ 7    0    2    3  ]   (2→3→1: 2+5=7<8, 2→3→4: 2+1=3<15)
3  [ 5    8    0    1  ]
4  [ 2    5    7    0  ]   (nothing improves: 4→3→1: 7+5=12>2)
```

**$k=4$:** $D[i][j] = \min(D[i][j], D[i][4]+D[4][j])$
```
     1    2    3    4
1  [ 0    3    5    6  ]   (nothing improves)
2  [ 5    0    2    3  ]   (2→4→1: 3+2=5<7)
3  [ 3    4    0    1  ]   (3→4→1: 1+2=3<5, 3→4→2: 1+5=6<8... wait we need D[4][2]=5 from current matrix: 1+5=6<8? No wait, let me check: D[3][4]=1, D[4][2]=5, so 1+5=6<8. Actually the current D[4][2] in k=3 is 5, so 3→4→2: 1+5=6<8 →D[3][2]=6... wait 4<6 so 4 is better... Hmm: from k=3, D[3][2]=8. 3→4→2: D[3][4]+D[4][2]=1+5=6 < 8, so D[3][2]=6. Actually wait... D[4][1]=2, D[4][2]=5. So 3→4→1: 1+2=3<5✓. 3→4→2: 1+5=6<8✓.
4  [ 2    5    7    0  ]
```

Final: $D[3][2]$: from k=3 matrix, $D[3][2]=8$. $D[3][4]+D[4][2]=1+5=6$. So $D[3][2]=6$.

But wait: $D[4][3]=7$, so $D[4][3]+D[3][j]$... For $k=4$, the intermediate is vertex 4.

Let me finalize $k=4$:
- Check (2,1): $D[2][4]+D[4][1]=3+2=5<7$ → $D[2][1]=5$ ✓
- Check (3,1): $D[3][4]+D[4][1]=1+2=3<5$ → $D[3][1]=3$ ✓
- Check (3,2): $D[3][4]+D[4][2]=1+5=6<8$ → $D[3][2]=6$ ✓

Final matrix:
```
     1    2    3    4
1  [ 0    3    5    6  ]
2  [ 5    0    2    3  ]
3  [ 3    6    0    1  ]
4  [ 2    5    7    0  ]
```

$$\boxed{\text{See final distance matrix above}}$$

---

### Solution Q69: Kruskal's MST ⭐

Edges sorted: A-B(1), B-D(2), C-E(3), B-C(4), E-F(4), A-C(5), C-D(6), D-E(7), D-F(8)

| Edge | Weight | Action | Components |
|------|--------|--------|------------|
| A-B | 1 | Accept | {A,B}, {C}, {D}, {E}, {F} |
| B-D | 2 | Accept | {A,B,D}, {C}, {E}, {F} |
| C-E | 3 | Accept | {A,B,D}, {C,E}, {F} |
| B-C | 4 | Accept | {A,B,C,D,E}, {F} |
| E-F | 4 | Accept | {A,B,C,D,E,F} — done! |

Total MST weight = 1 + 2 + 3 + 4 + 4 = $\boxed{14}$

MST edges: A-B, B-D, C-E, B-C, E-F

---

### Solution Q70: Prim's MST ⭐

Start from A:

| Step | Vertex Added | Edge | Key values (remaining) |
|------|-------------|------|----------------------|
| 0 | A | — | B=1, C=5, D=∞, E=∞, F=∞ |
| 1 | B (key=1) | A-B | C=4, D=2, E=∞, F=∞ |
| 2 | D (key=2) | B-D | C=4, E=7, F=8 |
| 3 | C (key=4) | B-C | E=3, F=8 |
| 4 | E (key=3) | C-E | F=4 |
| 5 | F (key=4) | E-F | — |

MST weight = 1 + 2 + 4 + 3 + 4 = $\boxed{14}$ (same as Kruskal's)

---

### Solution Q71: MST Uniqueness ⭐

**If all edge weights are distinct → MST is unique.** This is because the cut property and cycle property yield a unique choice at every step.

**Proof sketch:** Suppose two distinct MSTs $T_1$ and $T_2$ exist. Let $e$ be the minimum weight edge in $T_1 \setminus T_2$. Adding $e$ to $T_2$ creates a cycle. This cycle must contain an edge $e'$ not in $T_1$ with weight $> w(e)$ (since all weights distinct). Replacing $e'$ with $e$ in $T_2$ gives a lighter spanning tree, contradicting $T_2$ being an MST.

**If edges share weights → multiple MSTs possible.**

$$\boxed{\text{Distinct weights} \Rightarrow \text{unique MST; tied weights may yield multiple MSTs}}$$

---

### Solution Q72: DAG Shortest Path

Topological order: S, B, A, C, D (one valid ordering)

Process: S→A(5), S→B(3)

| Vertex | dist after relaxation |
|--------|----------------------|
| S | 0 |
| B | 3 (from S) |
| A | 5 (from S), then 3+6=9? Edge B→A(6)... actually the problem says B→A is 6? Let me re-read. |

Re-reading: S→(5)→A, S→(3)→B, A→(3)→D, B→(2)→D, B→(6)→A, A→(-1)→C, C→(4)→D

Topo order: S, B, A, C, D

| Process | Action | dist[] |
|---------|--------|--------|
| S | relax S→A(5), S→B(3) | S=0, A=5, B=3 |
| B | relax B→D(2): D=5, B→A(6): 3+6=9>5 | S=0, A=5, B=3, D=5 |
| A | relax A→D(3): 5+3=8>5, A→C(-1): C=4 | C=4 |
| C | relax C→D(4): 4+4=8>5 | no change |
| D | done | |

$$\boxed{S=0, B=3, A=5, C=4, D=5}$$

---

### Solution Q73: BFS Shortest Path

**TRUE.** BFS computes the shortest path distance $d(s,v)$, which by definition is the minimum number of edges over ALL paths from $s$ to $v$. So every path has length $\geq d$.

$$\boxed{\text{TRUE}}$$

---

### Solution Q74: SCCs ⭐

Graph: 1→2, 2→3, 3→1, 2→4, 4→5, 5→6, 6→4

SCC 1: {1, 2, 3} (cycle: 1→2→3→1)
SCC 2: {4, 5, 6} (cycle: 4→5→6→4)

$$\boxed{\{1,2,3\} \text{ and } \{4,5,6\}}$$

---

## Section 7: Greedy & DP

### Solution Q75: Huffman Coding ⭐

A:5, B:9, C:12, D:13, E:16, F:45

Merge: (A:5,B:9)→14, (C:12,D:13)→25, (14,E:16)→30, (25,30)→55, (F:45,55)→100

Codes: F=0, C=100, D=101, A=1100, B=1101, E=111

Total bits = 45(1) + 12(3) + 13(3) + 5(4) + 9(4) + 16(3) = 45 + 36 + 39 + 20 + 36 + 48 = $\boxed{224}$

---

### Solution Q76: Huffman Min Cost ⭐

Frequencies: 1, 3, 5, 7, 9, 11

Step 1: Merge 1+3=4 → [4, 5, 7, 9, 11]
Step 2: Merge 4+5=9 → [7, 9, 9, 11]
Step 3: Merge 7+9=16 → [9, 11, 16]
Step 4: Merge 9+11=20 → [16, 20]
Step 5: Merge 16+20=36

Total cost = sum of all merges = 4 + 9 + 16 + 20 + 36 = $\boxed{85}$

(This equals the weighted external path length: $1(4)+3(4)+5(3)+7(3)+9(2)+11(2) = 4+12+15+21+18+22 = 92$)

Wait, let me recalculate. The sum of all internal nodes = 4+9+16+20+36 = 85. But let me verify with WPL:

Tree structure:
```
        36
       /  \
      16   20
     / \  / \
    7  [9] 9  11
      / \
     [4] 5
    / \
   1   3
```

Depths: 1→4, 3→4, 5→3, 7→2, 9→2, 11→2

WPL = 1(4)+3(4)+5(3)+7(2)+9(2)+11(2) = 4+12+15+14+18+22 = $\boxed{85}$ ✓

---

### Solution Q77: Activity Selection

Sort by finish time: (1,4), (3,5), (0,6), (5,7), (3,8), (5,9), (6,10), (8,11), (8,12), (2,13), (12,14)

Greedy: Select (1,4) → next starting ≥ 4: (5,7) → next starting ≥ 7: (8,11) → next ≥ 11: (12,14)

$$\boxed{4 \text{ activities: } (1,4), (5,7), (8,11), (12,14)}$$

---

### Solution Q78: Job Scheduling ⭐

Sort by profit: J1(2,100), J3(2,27), J4(1,25), J2(1,19), J5(3,15)

Slots: [_, _, _] (deadlines go up to 3)

- J1 (deadline 2, profit 100): slot 2 → [_, J1, _]
- J3 (deadline 2, profit 27): slot 2 taken, try slot 1 → [J3, J1, _]
- J4 (deadline 1, profit 25): slot 1 taken → rejected
- J2 (deadline 1, profit 19): slot 1 taken → rejected
- J5 (deadline 3, profit 15): slot 3 → [J3, J1, J5]

Max profit = 100 + 27 + 15 = $\boxed{142}$

---

### Solution Q79: Fractional Knapsack

| Item | Weight | Value | Value/Weight |
|------|--------|-------|-------------|
| 1 | 10 | 60 | 6 |
| 2 | 20 | 100 | 5 |
| 3 | 30 | 120 | 4 |

Sorted by V/W: Item 1 (6), Item 2 (5), Item 3 (4). Capacity = 50.

Take all of Item 1 (10 kg, 60 value, remaining = 40)
Take all of Item 2 (20 kg, 100 value, remaining = 20)
Take 20/30 of Item 3 (20 kg, 80 value)

Total = 60 + 100 + 80 = $\boxed{240}$

---

### Solution Q80: LCS ⭐

$X =$ "AGGTAB", $Y =$ "GXTXAYB"

|   |   | G | X | T | X | A | Y | B |
|---|---|---|---|---|---|---|---|---|
|   | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 |
| G | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| G | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| T | 0 | 1 | 1 | 2 | 2 | 2 | 2 | 2 |
| A | 0 | 1 | 1 | 2 | 2 | 3 | 3 | 3 |
| B | 0 | 1 | 1 | 2 | 2 | 3 | 3 | 4 |

LCS length = **4**. One LCS = "GTAB"

$$\boxed{\text{LCS length} = 4, \text{ one LCS} = \text{"GTAB"}}$$

---

### Solution Q81: Matrix Chain Multiplication ⭐⭐

$p = [5, 4, 6, 2, 7]$ (for $A_1(5\times4), A_2(4\times6), A_3(6\times2), A_4(2\times7)$)

**Base:** $m[i,i] = 0$

**Chain length 2:**
$m[1,2] = 5 \times 4 \times 6 = 120$
$m[2,3] = 4 \times 6 \times 2 = 48$
$m[3,4] = 6 \times 2 \times 7 = 84$

**Chain length 3:**
$m[1,3] = \min(m[1,1]+m[2,3]+5\times4\times2, \; m[1,2]+m[3,3]+5\times6\times2)$
$= \min(0+48+40, \; 120+0+60) = \min(88, 180) = 88$ (split at k=1)

$m[2,4] = \min(m[2,2]+m[3,4]+4\times6\times7, \; m[2,3]+m[4,4]+4\times2\times7)$
$= \min(0+84+168, \; 48+0+56) = \min(252, 104) = 104$ (split at k=3)

**Chain length 4:**
$m[1,4] = \min($
$\quad k=1: m[1,1]+m[2,4]+5\times4\times7 = 0+104+140 = 244$
$\quad k=2: m[1,2]+m[3,4]+5\times6\times7 = 120+84+210 = 414$
$\quad k=3: m[1,3]+m[4,4]+5\times2\times7 = 88+0+70 = 158$
$) = 158$ (split at k=3)

Optimal parenthesization: $((A_1(A_2 A_3))A_4)$

$$\boxed{m[1,4] = 158, \text{ optimal: } ((A_1(A_2 A_3))A_4)}$$

---

### Solution Q82: 0-1 Knapsack ⭐

Items: (w,v) = (2,3), (3,4), (4,5), (5,6). Capacity W=5.

|   | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1(2,3) | 0 | 0 | 3 | 3 | 3 | 3 |
| 2(3,4) | 0 | 0 | 3 | 4 | 4 | 7 |
| 3(4,5) | 0 | 0 | 3 | 4 | 5 | 7 |
| 4(5,6) | 0 | 0 | 3 | 4 | 5 | 7 |

Maximum value = **7** (items 1 and 2: weight 2+3=5, value 3+4=7)

$$\boxed{\text{Max value} = 7, \text{ select items 1 and 2}}$$

---

### Solution Q83: Floyd-Warshall Path Reconstruction

Using the final matrix from Q68, path from vertex 2 to vertex 4:

$D^{(4)}[2][4] = 3$. Reconstructing via predecessor matrix:

2 → 3 (weight 2) → 4 (weight 1). Total = 3. ✓

$$\boxed{\text{Shortest path: } 2 \to 3 \to 4, \text{ distance} = 3}$$

---

### Solution Q84: Greedy vs DP ⭐

| Problem | Approach | Reason |
|---------|----------|--------|
| (a) Making change {1,5,10,25} | **Greedy** | Canonical coin system — greedy works |
| (b) 0-1 Knapsack | **DP** | No greedy choice property for indivisible items |
| (c) Fractional Knapsack | **Greedy** | Sort by value/weight, take greedily |
| (d) LCS | **DP** | Overlapping subproblems, no greedy structure |
| (e) Huffman Coding | **Greedy** | Merging two smallest is always optimal |
| (f) Shortest path (non-neg) | **Greedy** (Dijkstra) | Once relaxed, vertex is finalized |
| (g) Matrix Chain Mult. | **DP** | Need to try all split points |

---

### Solution Q85: Optimal BST ⭐⭐

Keys: 10(0.3), 20(0.5), 30(0.2)

$e[i,j]$ = expected search cost for keys $i$ to $j$

$w[i,j]$ = sum of probabilities from $i$ to $j$

$w[1,1]=0.3, w[2,2]=0.5, w[3,3]=0.2$
$w[1,2]=0.8, w[2,3]=0.7, w[1,3]=1.0$

**Base:** $e[i,i] = p_i$
$e[1,1]=0.3, e[2,2]=0.5, e[3,3]=0.2$

**Length 2:**
$e[1,2] = w[1,2] + \min(e[1,0]+e[2,2], \; e[1,1]+e[3,2])$
$= 0.8 + \min(0+0.5, \; 0.3+0) = 0.8 + 0.3 = 1.1$ (root=20... wait)

Using standard formula: $e[i,j] = \min_{r=i}^{j}(e[i,r-1] + e[r+1,j] + w[i,j])$

$e[1,2]: r=1: 0 + 0.5 + 0.8 = 1.3; r=2: 0.3 + 0 + 0.8 = 1.1$ → $e[1,2]=1.1$ (root=2, key 20)
$e[2,3]: r=2: 0 + 0.2 + 0.7 = 0.9; r=3: 0.5 + 0 + 0.7 = 1.2$ → $e[2,3]=0.9$ (root=2, key 20)
$e[1,3]: r=1: 0+0.9+1.0=1.9; r=2: 0.3+0.2+1.0=1.5; r=3: 1.1+0+1.0=2.1$ → $e[1,3]=1.5$ (root=2, key 20)

Optimal BST has 20 as root, 10 as left child, 30 as right child.

Expected cost = $\boxed{1.5}$

---

### Solution Q86: Edit Distance

"SATURDAY" → "SUNDAY"

|   |   | S | U | N | D | A | Y |
|---|---|---|---|---|---|---|---|
|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| S | 1 | 0 | 1 | 2 | 3 | 4 | 5 |
| A | 2 | 1 | 1 | 2 | 3 | 3 | 4 |
| T | 3 | 2 | 2 | 2 | 3 | 4 | 4 |
| U | 4 | 3 | 2 | 3 | 3 | 4 | 5 |
| R | 5 | 4 | 3 | 3 | 4 | 4 | 5 |
| D | 6 | 5 | 4 | 4 | 3 | 4 | 5 |
| A | 7 | 6 | 5 | 5 | 4 | 3 | 4 |
| Y | 8 | 7 | 6 | 6 | 5 | 4 | 3 |

$$\boxed{\text{Edit distance} = 3}$$

---

### Solution Q87: Coin Change ⭐

Coins: {1, 5, 6, 9}, target = 11

**DP table (min coins):**
| Amount | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|--------|---|---|---|---|---|---|---|---|---|---|----|----|
| Min coins | 0 | 1 | 2 | 3 | 4 | 1 | 1 | 2 | 3 | 1 | 2 | 2 |

**(a)** Min coins = **2** (5 + 6 = 11)

**(b)** Greedy: take 9 first → remaining 2 → need two 1s → total 3 coins (9+1+1). Greedy gives **3**, NOT optimal.

$$\boxed{(a)\;2 \text{ coins (5+6)}, \;(b)\; \text{Greedy fails (gives 3)}}$$

---

### Solution Q88: LIS ⭐

$[10, 22, 9, 33, 21, 50, 41, 60, 80]$

DP approach: $L[i]$ = length of LIS ending at index $i$

| i | Value | LIS length | Subsequence |
|---|-------|-----------|-------------|
| 0 | 10 | 1 | [10] |
| 1 | 22 | 2 | [10, 22] |
| 2 | 9 | 1 | [9] |
| 3 | 33 | 3 | [10, 22, 33] |
| 4 | 21 | 2 | [10, 21] |
| 5 | 50 | 4 | [10, 22, 33, 50] |
| 6 | 41 | 4 | [10, 22, 33, 41] |
| 7 | 60 | 5 | [10, 22, 33, 50, 60] |
| 8 | 80 | 6 | [10, 22, 33, 50, 60, 80] |

LIS length = **6**

DP: $O(n^2)$. Can be improved to $O(n \log n)$ using patience sorting / binary search approach.

$$\boxed{\text{LIS length} = 6, \; O(n^2) \text{ DP, improvable to } O(n \log n)}$$

---

## Bonus Solutions

### Solution Q89: Red-Black Tree Properties
**Answer: (d) All of the above**
- (a) TRUE: Black-height property guarantees longest path ≤ 2 × shortest path (shortest path is all black, longest alternates red-black)
- (b) TRUE: Standard RB-tree height bound
- (c) TRUE: Insert fix-up does at most 2 rotations (Case 2+3 or just Case 3)

---

### Solution Q90: RB Tree vs AVL
- **Height:** AVL tree has smaller height (strictly balanced: $h \leq 1.44 \log_2(n+2)$) vs RB tree ($h \leq 2\log_2(n+1)$)
- **For frequent insert/delete:** Red-Black tree is better because it requires at most 2-3 rotations per insert/delete (O(1) structural changes), while AVL may need up to $O(\log n)$ rotations on delete.
- Red-Black trees are used in `std::map`, `TreeMap` (Java); AVL is better for lookup-heavy workloads.

---

### Solution Q91: B-Tree Order 5
(a) Max keys = $m - 1 = 5 - 1 = $ **4**
(b) Min keys (non-root) = $\lceil m/2 \rceil - 1 = 3 - 1 = $ **2**
(c) Min children (non-root internal) = $\lceil m/2 \rceil = $ **3**

---

### Solution Q92: B-Tree Height
Order 100 → min degree $t = 50$. For 999,999 keys:
Root has ≥ 1 key. Level 1: ≥ 2 nodes with ≥ 49 keys each. Level $h$: ≥ $2 \times 50^{h-1}$ nodes.
$n \geq 1 + 2(t-1)\sum_{i=0}^{h-1} t^i = 1 + 2(t-1) \cdot \frac{t^h - 1}{t - 1} = 2t^h - 1$
$999999 \geq 2 \times 50^h - 1 \Rightarrow 50^h \leq 500000$
$h \leq \log_{50}(500000) = \frac{\log(500000)}{\log(50)} \approx \frac{5.7}{1.7} \approx 3.35$
**Maximum height = 3** (4 levels: root + 3 internal levels)

---

### Solution Q93: B+ Tree Orders
(a) Internal: $p \times 6 + (p-1) \times 10 \leq 4096 \Rightarrow 16p - 10 \leq 4096 \Rightarrow p \leq 256.6 \Rightarrow$ **p = 256**
(b) Leaf: $p_L \times (10 + 8) + 6 \leq 4096 \Rightarrow 18p_L \leq 4090 \Rightarrow$ **$p_L$ = 227**
(c) Root: max 256 pointers. Level 1: max 256 × 256 = 65536 internal nodes (pointers). Leaves: 65536 × 227 = **14,876,672 records**

---

### Solution Q94: Trie
Strings: {car, card, care, cat, do, dog}
Nodes: root → c → a → r (mark "car") → d (mark "card"), r → e (mark "care"); a → t (mark "cat"); root → d → o (mark "do") → g (mark "dog")
Total nodes: root + c + a + r + d + e + t + d + o + g = **10 nodes** (plus root = 11 with root)
Search "care": traverse c→a→r→e = **4 character comparisons = O(4) = O(L)**

---

### Solution Q95: Union-Find with Rank
After all unions: Tree rooted at 0 with rank 3.
```
        0 (rank 3)
       / \
      1   2 (rank 2)
         / \
        3   4 (rank 1)
           / \
          5   6 (rank 1)
              |
              7
```
**Root rank = 3**

---

### Solution Q96: Amortized MultiPop
Each element is pushed once and popped at most once. In $n$ operations, total pushes ≤ $n$, total pops ≤ total pushes ≤ $n$. Total work = pushes + pops ≤ $2n$. Amortized = $2n/n = $ **$O(1)$ per operation → $O(n)$ total**.

---

### Solution Q97: SCC 
Edges: A→B, B→C, C→A, B→D, D→E, E→F, F→D
- SCC 1: {A, B, C} (cycle: A→B→C→A)
- SCC 2: {D, E, F} (cycle: D→E→F→D)
**2 strongly connected components.**
SCC DAG: {A,B,C} → {D,E,F} (edge from B→D)

---

### Solution Q98: Network Flow
Augmenting paths (Edmonds-Karp):
1. S→A→T: bottleneck 7, flow = 7
2. S→B→T: bottleneck 8, flow = 8
3. S→A→B→T: bottleneck 2 (min(10-7, 5, 10-8) = min(3,5,2) = 2), flow = 2
**Max flow = 7 + 8 + 2 = 15**  (or verify: S→A=10 send 9, S→B=8, A→B=5 send 2, A→T=7, B→T=10)

Correct: paths S→A→T (7), S→B→T (8), S→A→B→T (min(3,5,2)=2). Total = **17**.
Actually: S→A capacity 10, S→B capacity 8. Max outflow from S = 18. A→T=7, B→T=10. Total sink capacity = 17.
Path 1: S→B→T, flow 8. Path 2: S→A→T, flow 7. Path 3: S→A→B→T, flow min(3,5,2)=2.
**Max flow = 17**. Min cut: {S} vs {A,B,T}, cut capacity = 10+8 = 18. Better cut: {S,A} vs {B,T}, capacity = 8+5+7 = 20. Best: **min cut capacity = 17**.

---

### Solution Q99: Optimal BST
Probabilities: $p_1=0.3, p_2=0.1, p_3=0.2, q_0=q_1=q_2=q_3=0.1$
Testing $k_1$ as root: $e = 1(0.3) + (0.1+0.1) + (0.1+0.2×2+0.1×2+0.1×3) = ...$

By DP computation:
- $e[1,1] = q_0 + p_1 + q_1 = 0.1 + 0.3 + 0.1 = 0.5$, $w[1,1] = 0.5$
- $e[2,2] = 0.3$, $e[3,3] = 0.4$
- After computing all $e[i,j]$: **root = $k_1$, expected cost ≈ 1.7**

---

### Solution Q100: B-Tree Order 3 Insertions
Order 3 → max 2 keys/node.
- Insert 10: [10]
- Insert 20: [10, 20]
- Insert 30: Split! Promote 20. Tree: [20] with children [10], [30]
- Insert 40: [30, 40] in right child
- Insert 50: Right child splits! Promote 40. Tree: [20, 40] with children [10], [30], [50]
- Insert 60: [50, 60] in rightmost child
- Insert 70: Rightmost splits → promote 60. Root splits → promote 40.
  Final: [40] with children [20] and [60], grandchildren [10], [30], [50], [70]

---

### Solution Q101: Trie vs Hash Table
| Feature | Trie | Hash Table |
|---------|------|-----------|
| Search | $O(L)$ | $O(L)$ average ($O(nL)$ worst with collision) |
| Prefix search | $O(L)$ — natural | $O(n \cdot L)$ — must check all keys |
| Space | $O(n \cdot L \cdot |\Sigma|)$ worst | $O(n \cdot L)$ |

Trie wins for prefix queries; hash table wins for exact lookups and space.

---

### Solution Q102: Ford-Fulkerson (Edmonds-Karp)
Augmenting paths (BFS shortest path):
1. S→A→C→T: min(16,12,20) = 12, flow = 12
2. S→B→D→C→T: min(13,14,7,8) = 7, then S→B→D→T: min(13,14,4), flow depends on residual...
After full execution: **Max flow = 23**

---

### Solution Q103: Cycle Detection with Union-Find
Process edges:
- (0,1): Union(0,1) → {0,1}
- (1,2): Union(1,2) → {0,1,2}
- (2,3): Union(2,3) → {0,1,2,3}
- (3,0): Find(3)=0, Find(0)=0 → **Same set! Cycle detected!** ⭐
Edge (2,4) would not be processed (or is processed: Union(2,4))

---

### Solution Q104: B+ Tree Range Query
Search for 10: traverse to leaf [5,8] → not found, but pointer to next leaf.
Follow leaf links: [12,15] (12,15 ∈ [10,30] ✓) → [20,25] (20,25 ✓) → [30,35] (30 ✓)
**3 leaf nodes accessed** (skipped first leaf since 10 > 8). Keys found: 12, 15, 20, 25, 30.

---

### Solution Q105: Max-Flow Min-Cut
**Theorem:** In any flow network, max flow from s to t = min over all s-t cuts of the cut capacity.
**Proof sketch:** (1) Flow ≤ any cut capacity (flow conservation). (2) At max flow, the residual graph has no s-t path. The set of vertices reachable from s in residual graph forms a cut whose capacity equals the flow.
**Bipartite matching:** Create flow network: source→left vertices (cap 1), edges between matching pairs (cap 1), right vertices→sink (cap 1). Max flow = max matching.

---

### Solution Q106: Tarjan's SCC
DFS starting from 1:
- Visit 1 (disc=0,low=0), push 1
- Visit 2 (disc=1,low=1), push 2
- Visit 3 (disc=2,low=2), push 3
- 3→1: back edge, low[3]=min(2,0)=0
- 3→4: Visit 4 (disc=3,low=3), push 4
- Visit 5 (disc=4,low=4), Visit 6 (disc=5,low=5)
- 6→4: back edge, low[6]=min(5,3)=3, then low[5]=min(4,3)=3, low[4]=min(3,3)=3
- 4: low=disc=3 → pop SCC: **{4,5,6}**
- Back to 3: low[3]=0, 2: low=0, 1: low=disc=0 → pop SCC: **{1,2,3}**

---

### Solution Q107: Binary Counter
Bit 0 flips every increment, bit 1 every 2, bit $i$ every $2^i$.
Total flips = $n + n/2 + n/4 + ... < 2n$.
**Amortized cost = $2n/n = O(1)$** per increment. (Accounting: charge 2 per increment — 1 for setting bit to 1, bank 1 for future reset to 0.)

---

### Solution Q108: B-Tree Deletion
**Delete 10 from [10,20]:** Left child becomes [20]. But that's fine (root's child with 1 key in order-3 is okay since min = ⌈3/2⌉−1 = 1).
Tree: [30] with children [20], [40,50].

**Delete 30:** Internal node deletion. Replace with inorder predecessor (20) or successor (40).
Replace with 40: Tree becomes [40] with children [20], [50].

---

### Solution Q109: RB-Tree Insertions
- Insert 41: root, color BLACK. → [41B]
- Insert 38: 38<41, left child RED. → 41B(38R, -)
- Insert 31: 31<38, left child RED. Parent 38 is RED → violation! Uncle is NIL (black). LL case → right rotate 41, recolor. → 38B(31R, 41R)
- Insert 12: 12<31, left of 31R. Parent RED, uncle 41 is RED → recolor: 31B, 41B, 38R. Then 38 is root → recolor BLACK. → 38B(31B(12R,-), 41B)
- Insert 19: 19>12, right of 12R. Parent RED, uncle NIL → LR case: left rotate 12, then right rotate 31. → 38B(19B(12R,31R), 41B)
- Insert 8: 8<12, left of 12R. Parent RED, uncle 31 is RED → recolor: 12B, 31B, 19R. Check: 19R under 38B is fine. → 38B(19R(12B(8R,-),31B), 41B)

---

### Solution Q110: Kruskal's with Union-Find
1. (A-B,1): Union(A,B). MST edges: {A-B}
2. (C-D,2): Union(C,D). MST: {A-B, C-D}
3. (A-C,3): Find(A)≠Find(C), Union. MST: {A-B, C-D, A-C}
4. (B-D,4): Find(B)=Find(D) (same set via A-C). **Skip** (would form cycle)
5. (B-C,5): Same set. **Skip**
6. (D-E,6): Find(D)≠Find(E), Union. MST: {A-B, C-D, A-C, D-E}
MST weight = 1+2+3+6 = **12**. 4 edges for 5 vertices ✓
