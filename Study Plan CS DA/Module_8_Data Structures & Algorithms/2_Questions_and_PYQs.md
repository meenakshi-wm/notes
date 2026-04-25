# Data Structures & Algorithms — Questions & Previous Year Questions (GATE 2027)

> 80+ questions | Easy → Medium → GATE Level | All topics covered
> ⭐ = GATE PYQ style | Topics tagged for targeted practice

---

## Table of Contents

1. [Asymptotic Analysis & Recurrences (Q1–Q12)](#section-1-asymptotic-analysis--recurrences)
2. [Arrays, Linked Lists, Stacks & Queues (Q13–Q24)](#section-2-arrays-linked-lists-stacks--queues)
3. [Trees — Binary Trees, BST, AVL (Q25–Q38)](#section-3-trees--binary-trees-bst-avl)
4. [Heaps & Hashing (Q39–Q50)](#section-4-heaps--hashing)
5. [Sorting Algorithms (Q51–Q60)](#section-5-sorting-algorithms)
6. [Graph Algorithms — BFS, DFS, Shortest Path, MST (Q61–Q74)](#section-6-graph-algorithms)
7. [Greedy & Dynamic Programming (Q75–Q88)](#section-7-greedy--dynamic-programming)

---

## Section 1: Asymptotic Analysis & Recurrences

### Q1. [Easy] Asymptotic Comparison
Arrange the following in increasing order of asymptotic growth rate:
$$n^2, \; 2^n, \; n \log n, \; n!, \; \log n, \; n^{1.5}, \; n^n$$

---

### Q2. [Easy] Big-O Determination
What is the time complexity of the following code?
```c
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= i*i; j++)
        x++;
```

---

### Q3. [Medium] ⭐ Nested Loop Analysis
What is the time complexity?
```c
for (int i = 1; i <= n; i = i * 2)
    for (int j = 1; j <= i; j++)
        x++;
```

---

### Q4. [Medium] Asymptotic Notation
Which of the following is/are TRUE?
(a) $n^2 + n = O(n^2)$
(b) $2^{n+1} = O(2^n)$
(c) $2^{2n} = O(2^n)$
(d) $n! = O(n^n)$
(e) $\log(n!) = \Theta(n \log n)$

---

### Q5. [GATE Level] ⭐ Master Theorem Application
Solve the following recurrence relations:
(a) $T(n) = 4T(n/2) + n^2$
(b) $T(n) = 3T(n/4) + n \log n$
(c) $T(n) = 2T(n/2) + n / \log n$
(d) $T(n) = T(n/2) + T(n/4) + T(n/8) + n$

---

### Q6. [GATE Level] Iteration Method
Solve using iteration: $T(n) = 2T(n-1) + 1$, $T(0) = 1$.

---

### Q7. [GATE Level] ⭐ Change of Variable
Solve: $T(n) = T(\sqrt{n}) + 1$ with $T(2) = 1$.

---

### Q8. [Medium] Loop Complexity
What is the time complexity?
```c
int i = n;
while (i > 1) {
    i = sqrt(i);
}
```

---

### Q9. [GATE Level] ⭐ Summation Complexity
What is the value of $\sum_{i=1}^{n} \lfloor \log_2 i \rfloor$ in terms of $\Theta$?

---

### Q10. [GATE Level] Recurrence Trees
Draw the recursion tree and determine the complexity:
$T(n) = T(n/3) + T(2n/3) + n$

---

### Q11. [Medium] Tricky Notation
Is the following statement TRUE or FALSE?
"If $f(n) = O(g(n))$ and $g(n) = O(h(n))$, then $f(n) = O(h(n))$"

---

### Q12. [GATE Level] ⭐ While Loop
What is the time complexity?
```c
int i = 1, s = 1;
while (s <= n) {
    i++;
    s = s + i;
}
```

---

## Section 2: Arrays, Linked Lists, Stacks & Queues

### Q13. [Easy] Array Address Calculation
An array $A[5 \ldots 10][2 \ldots 8]$ is stored in row-major order with base address 100 and each element takes 4 bytes. Find the address of $A[7][5]$.

---

### Q14. [Medium] ⭐ Column Major
A 2D array $A[1 \ldots 20][1 \ldots 30]$ is stored in column-major order. Base address = 500, element size = 2 bytes. Find address of $A[11][15]$.

---

### Q15. [Easy] Linked List Operations
What is the minimum number of pointer changes required to insert a node after a given node in:
(a) Singly linked list
(b) Doubly linked list

---

### Q16. [Medium] ⭐ Linked List Cycle Detection
A singly linked list has $n$ nodes with a cycle. The cycle begins at the $k$-th node from the head. A slow pointer moves 1 step and a fast pointer moves 2 steps per iteration. After how many iterations will they first meet?

---

### Q17. [GATE Level] ⭐ Stack Permutations
Which of the following permutations can be obtained by pushing the sequence $1, 2, 3, 4, 5$ onto a stack and popping?
(a) $3, 2, 5, 4, 1$
(b) $3, 4, 5, 1, 2$
(c) $2, 5, 4, 1, 3$
(d) $5, 4, 3, 2, 1$
(e) $1, 3, 2, 5, 4$

---

### Q18. [Medium] Infix to Postfix
Convert the following infix expression to postfix:
$$A + B * C - (D / E + F) * G$$

---

### Q19. [GATE Level] ⭐ Postfix Evaluation
Evaluate the postfix expression: $8 \; 2 \; 3 \; \hat{} \; / \; 2 \; 3 \; * + 7 \; -$
(Here $\hat{}$ denotes power)

---

### Q20. [Medium] Queue using Stacks
Implement a queue using two stacks such that both Enqueue and Dequeue have amortized $O(1)$ time complexity. Show the sequence of operations for: Enqueue(1), Enqueue(2), Dequeue(), Enqueue(3), Dequeue(), Dequeue().

---

### Q21. [GATE Level] ⭐ Circular Queue
A circular queue is implemented using an array of size 8 (indices 0–7). After a series of operations, front = 3 and rear = 7. How many elements are in the queue? If we perform two more enqueue and one dequeue operations, what are the new values of front and rear?

---

### Q22. [Medium] Multiple Stacks
Two stacks S1 and S2 are implemented in a single array $A[0 \ldots n-1]$. S1 grows from index 0 upward and S2 grows from index $n-1$ downward. What is the condition for overflow?

---

### Q23. [GATE Level] ⭐ Infix to Prefix
Convert to prefix: $(A + B) * (C - D) / (E + F)$

---

### Q24. [Medium] Catalan Numbers
How many distinct valid stack permutations exist for $n = 5$ elements?

---

## Section 3: Trees — Binary Trees, BST, AVL

### Q25. [Easy] Binary Tree Properties
A binary tree has 10 internal nodes, each with exactly 2 children. How many leaf nodes does it have?

---

### Q26. [Medium] ⭐ Tree Construction
Given:
- Inorder: D B H E A F C G
- Preorder: A B D E H C F G

Construct the binary tree and write its postorder traversal.

---

### Q27. [GATE Level] ⭐ Tree Construction from Postorder
Given:
- Inorder: 4 8 2 5 1 6 3 7
- Postorder: 8 4 5 2 6 7 3 1

Construct the binary tree and give its preorder and level-order traversals.

---

### Q28. [Medium] Binary Tree — Number of Trees
How many structurally different binary trees can be formed with 4 nodes? How many if the nodes are labeled 1, 2, 3, 4?

---

### Q29. [GATE Level] ⭐ BST Search Sequence
Elements 15, 25, 5, 18, 20, 3, 10, 8 are inserted (in that order) into an initially empty BST. Draw the BST and find:
(a) The height of the tree
(b) The number of comparisons to search for 8
(c) The inorder traversal

---

### Q30. [Medium] BST Delete
In the BST from Q29, delete the node with key 15 (root). Show the resulting BST using:
(a) Inorder successor replacement
(b) Inorder predecessor replacement

---

### Q31. [GATE Level] ⭐ AVL Insertion
Insert the following keys one by one into an initially empty AVL tree: 10, 20, 30, 25, 27, 7, 4, 15.

Show the tree after each rotation, identifying the type of rotation (LL, RR, LR, RL).

---

### Q32. [GATE Level] ⭐ AVL Tree Height
What is the minimum number of nodes in an AVL tree of height 6?

---

### Q33. [Medium] AVL Balance Factor
After inserting keys 1 through 7 in order into an AVL tree, what is the height of the resulting tree? How many rotations are performed?

---

### Q34. [GATE Level] ⭐ Tree Traversal — Output
What does the following function compute on a binary tree?
```c
int func(Node* root) {
    if (root == NULL) return 0;
    int l = func(root->left);
    int r = func(root->right);
    return (l > r) ? l + 1 : r + 1;
}
```

---

### Q35. [Medium] Threaded Binary Tree
What is the advantage of an inorder threaded binary tree? How many NULL pointers are converted to threads in a tree with $n$ nodes?

---

### Q36. [GATE Level] ⭐ BST — Preorder to BST
Given the preorder traversal of a BST: 30, 20, 10, 15, 25, 40, 35, 50. Reconstruct the BST and give its inorder traversal.

---

### Q37. [GATE Level] Number of BSTs
How many distinct BSTs can be formed with keys $\{1, 2, 3, 4, 5\}$?

---

### Q38. [Medium] Tree Properties
A complete binary tree has 1023 nodes. What is:
(a) Its height?
(b) Number of leaf nodes?
(c) Number of internal nodes with exactly one child?

---

## Section 4: Heaps & Hashing

### Q39. [Easy] Heap Construction
Build a max-heap from the array: [3, 9, 2, 1, 4, 5, 7, 6, 8].

---

### Q40. [Medium] ⭐ Build Heap Complexity
Prove that building a heap using the bottom-up method takes $O(n)$ time. How many comparisons does building a max-heap from an array of 15 elements require in the worst case?

---

### Q41. [GATE Level] ⭐ Heap Operations
In a max-heap of 10 elements stored in array $A$, after calling ExtractMax twice and then inserting the value 50, how many elements are at the correct position relative to the original heap?

---

### Q42. [Medium] Min-Heap Property
Is the following array a valid min-heap? $[2, 5, 4, 7, 8, 6, 9, 10, 12, 11]$

---

### Q43. [GATE Level] ⭐ k-th Smallest in Heap
Given a min-heap of $n$ elements, what is the time complexity to find the $k$-th smallest element?

---

### Q44. [Medium] Heap Sort Trace
Perform heap sort on: [4, 10, 3, 5, 1]. Show each step.

---

### Q45. [GATE Level] ⭐ Hashing — Linear Probing
Hash table of size 11 with hash function $h(k) = k \mod 11$. Insert the following keys using **linear probing**: 10, 22, 31, 4, 15, 28, 17, 88, 59.

Show the final table and the number of probes for each insertion.

---

### Q46. [GATE Level] ⭐ Hashing — Double Hashing
A hash table of size 13 uses double hashing with:
- $h_1(k) = k \mod 13$
- $h_2(k) = 7 - (k \mod 7)$

Insert keys: 18, 41, 22, 44, 59, 32, 31, 73. Show the final table.

---

### Q47. [Medium] Chaining
A hash table of size 10 uses chaining with $h(k) = k \mod 10$. Insert: 12, 44, 13, 88, 23, 94, 11, 39, 20, 16, 5.
(a) Show the hash table
(b) What is the longest chain?
(c) What is the average search time for a successful search?

---

### Q48. [GATE Level] ⭐ Expected Probes
A hash table of size 100 has 80 entries. Using open addressing, what is the expected number of probes for:
(a) An unsuccessful search?
(b) A successful search?

---

### Q49. [GATE Level] Quadratic Probing
Hash table of size 7, $h(k) = k \mod 7$, quadratic probing with $h(k, i) = (h(k) + i^2) \mod 7$. Insert 76, 93, 40, 47, 10, 55. Can all keys be inserted? If not, which key causes failure?

---

### Q50. [Medium] Rehashing
When should a hash table be rehashed? If a table of size 7 with load factor 0.7 is rehashed to the next prime size, what is the new table size?

---

## Section 5: Sorting Algorithms

### Q51. [Easy] Sorting Comparison
Fill in the blanks:

| Algorithm | Best Case | Worst Case | Stable? | In-place? |
|-----------|-----------|-----------|---------|-----------|
| Merge Sort | ___ | ___ | ___ | ___ |
| Quick Sort | ___ | ___ | ___ | ___ |
| Heap Sort | ___ | ___ | ___ | ___ |
| Counting Sort | ___ | ___ | ___ | ___ |

---

### Q52. [Medium] ⭐ Bubble Sort Passes
After 3 passes of bubble sort on $[5, 3, 8, 1, 9, 2, 7]$, what is the array?

---

### Q53. [GATE Level] ⭐ Quick Sort Partition
Apply Lomuto partition (pivot = last element) to: $[7, 2, 1, 6, 8, 5, 3, 4]$. Show each step and the final position of the pivot.

---

### Q54. [GATE Level] Quick Sort Worst Case
For which of the following inputs does quick sort (with first element as pivot) have worst-case $O(n^2)$ behavior?
(a) $[1, 2, 3, 4, 5]$
(b) $[5, 4, 3, 2, 1]$
(c) $[3, 1, 4, 1, 5]$
(d) Both (a) and (b)

---

### Q55. [GATE Level] ⭐ Lower Bound on Sorting
Prove that any comparison-based sorting algorithm must make at least $\lceil \log_2(n!) \rceil$ comparisons in the worst case. For $n = 8$, what is the minimum number of comparisons?

---

### Q56. [Medium] Counting Sort
Sort the array $[4, 2, 2, 8, 3, 3, 1]$ using counting sort. Show the count array and the sorted output.

---

### Q57. [GATE Level] ⭐ Inversions
How many inversions does the array $[5, 4, 3, 2, 1]$ have? What algorithm can count inversions in $O(n \log n)$?

---

### Q58. [GATE Level] ⭐ Minimum Swaps
What is the minimum number of swaps needed to sort $[4, 3, 2, 1]$?

---

### Q59. [Medium] Radix Sort
Sort the following using radix sort (LSD): $[329, 457, 657, 839, 436, 720, 355]$.

---

### Q60. [GATE Level] Selection Sort Analysis
Selection sort is applied to an array of $n$ elements. What is the total number of comparisons, regardless of input? Is selection sort adaptive?

---

## Section 6: Graph Algorithms

### Q61. [Medium] BFS Traversal
Perform BFS starting from vertex A on the following graph (break ties alphabetically):
```
A -- B -- E
|    |    |
C -- D -- F
```
Give the BFS tree and the discovery order.

---

### Q62. [Medium] ⭐ DFS Edge Classification
Perform DFS on the following directed graph (starting from vertex 1, visiting neighbors in numerical order):
```
1 → 2, 1 → 4
2 → 3
3 → 1
4 → 3, 4 → 5
5 → 6
6 → 4
```
Classify each edge as tree, back, forward, or cross edge.

---

### Q63. [GATE Level] ⭐ Topological Sort
Find ALL valid topological orderings for the following DAG:
```
Edges: A→C, A→D, B→D, B→E, C→F, D→F, E→F
```

---

### Q64. [GATE Level] ⭐ Articulation Points
Find all articulation points in the following undirected graph:
```
1 -- 2 -- 3
|         |
4 -- 5 -- 6
     |
     7
```

---

### Q65. [Medium] Connected Components
How many connected components does the following undirected graph have?
Vertices: {1, 2, 3, 4, 5, 6, 7, 8}
Edges: {(1,2), (2,3), (4,5), (6,7), (7,8), (6,8)}

---

### Q66. [GATE Level] ⭐ Dijkstra's Algorithm
Apply Dijkstra's algorithm starting from vertex S on:
```
     1       6
S ------→ A ------→ D
|         ↑ \       ↑
| 5       |2  \3    |1
↓         |    ↓    |
B ------→ C ------→ E
     2         4
```
Show the order of vertex selection and final shortest distances.

---

### Q67. [GATE Level] ⭐ Bellman-Ford
Apply Bellman-Ford from vertex A on:
```
A --(-1)-→ B
A --(4)--→ C
B --(3)--→ C
B --(2)--→ D
D --(2)--→ B
C --(5)--→ D
C --(-3)-→ E
D --(-1)-→ E
```
Show the distance table after each iteration. Does a negative cycle exist?

---

### Q68. [GATE Level] ⭐ Floyd-Warshall
Apply Floyd-Warshall to the following directed graph (adjacency matrix):

|   | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| 1 | 0 | 3 | ∞ | 7 |
| 2 | 8 | 0 | 2 | ∞ |
| 3 | 5 | ∞ | 0 | 1 |
| 4 | 2 | ∞ | ∞ | 0 |

Show the distance matrix after each intermediate vertex $k = 1, 2, 3, 4$.

---

### Q69. [GATE Level] ⭐ Kruskal's MST
Find the MST using Kruskal's algorithm:
```
Vertices: A, B, C, D, E, F
Edges: A-B(1), A-C(5), B-C(4), B-D(2), C-D(6), C-E(3), D-E(7), D-F(8), E-F(4)
```
Show the order of edge selection and the total MST weight.

---

### Q70. [GATE Level] ⭐ Prim's MST
Apply Prim's algorithm starting from vertex A on the graph in Q69. Show the frontier at each step.

---

### Q71. [GATE Level] ⭐ MST — Cut Property
In a graph with unique edge weights, is the MST always unique? Prove or disprove. If two edges have the same weight, can there be multiple MSTs?

---

### Q72. [GATE Level] DAG Shortest Path
Find the shortest path from S to all vertices in the following DAG:
```
S →(5)→ A →(3)→ D
S →(3)→ B →(2)→ D
B →(6)→ A →(-1)→ C
C →(4)→ D
```
Use topological sort + relaxation.

---

### Q73. [Medium] BFS Shortest Path
In an unweighted undirected graph, BFS from source $s$ computes shortest path distances. True or False: If vertex $v$ is at distance $d$ from $s$, then ALL paths from $s$ to $v$ have length $\geq d$?

---

### Q74. [GATE Level] ⭐ Strongly Connected Components
Find all strongly connected components in:
```
1 → 2
2 → 3
3 → 1
2 → 4
4 → 5
5 → 6
6 → 4
```

---

## Section 7: Greedy & Dynamic Programming

### Q75. [Medium] ⭐ Huffman Coding
Construct a Huffman tree for characters with frequencies: A:5, B:9, C:12, D:13, E:16, F:45.
(a) Draw the Huffman tree
(b) Assign codes to each character
(c) What is the total number of bits for encoding a message containing these frequencies?

---

### Q76. [GATE Level] ⭐ Huffman — Minimum Total Cost
The frequencies of 6 characters are 1, 3, 5, 7, 9, 11. Find the minimum weighted external path length using Huffman coding.

---

### Q77. [Medium] Activity Selection
Given activities with (start, finish): (1,4), (3,5), (0,6), (5,7), (3,8), (5,9), (6,10), (8,11), (8,12), (2,13), (12,14).
Find the maximum number of non-overlapping activities using the greedy approach.

---

### Q78. [GATE Level] ⭐ Job Scheduling
Jobs with (deadline, profit): J1(2,100), J2(1,19), J3(2,27), J4(1,25), J5(3,15).
Find the maximum profit job sequence using the greedy approach.

---

### Q79. [Medium] Fractional Knapsack
Items with (weight, value): (10, 60), (20, 100), (30, 120). Knapsack capacity = 50.
Find the maximum value using fractional knapsack.

---

### Q80. [GATE Level] ⭐ LCS
Find the length and one LCS of:
$X = $ "AGGTAB" and $Y = $ "GXTXAYB"
Show the complete DP table.

---

### Q81. [GATE Level] ⭐⭐ Matrix Chain Multiplication
Find the minimum number of scalar multiplications for:
$A_1(5 \times 4), A_2(4 \times 6), A_3(6 \times 2), A_4(2 \times 7)$

Show the DP table and the optimal parenthesization.

---

### Q82. [GATE Level] ⭐ 0-1 Knapsack
Items: (weight, value) = (2, 3), (3, 4), (4, 5), (5, 6). Capacity = 5.
Solve using DP. Show the table and identify which items are selected.

---

### Q83. [GATE Level] ⭐ Floyd-Warshall with Path
For the graph in Q68, use Floyd-Warshall to not only find shortest distances but also reconstruct the shortest path from vertex 2 to vertex 4.

---

### Q84. [GATE Level] ⭐ Greedy vs DP
For each problem, state whether Greedy or DP is appropriate and WHY:
(a) Making change with coin denominations {1, 5, 10, 25}
(b) 0-1 Knapsack
(c) Fractional Knapsack
(d) Longest Common Subsequence
(e) Huffman Coding
(f) Shortest path with non-negative weights
(g) Matrix Chain Multiplication

---

### Q85. [GATE Level] ⭐⭐ DP — Optimal BST
Given keys $\{10, 20, 30\}$ with search probabilities $\{0.3, 0.5, 0.2\}$. Construct the optimal BST that minimizes the expected search cost. What is the expected cost?

---

### Q86. [GATE Level] DP — Edit Distance
Find the edit distance (minimum operations: insert, delete, replace) between "SATURDAY" and "SUNDAY". Show the DP table.

---

### Q87. [GATE Level] ⭐ Coin Change
Given coins $\{1, 5, 6, 9\}$ and target sum = 11:
(a) Find the minimum number of coins using DP
(b) Does the greedy approach give the optimal answer?

---

### Q88. [GATE Level] ⭐ Longest Increasing Subsequence
Find the length of the longest increasing subsequence of: $[10, 22, 9, 33, 21, 50, 41, 60, 80]$.
What is the time complexity of the DP solution? Can it be improved?

---

## Section 8: Advanced Data Structures & Algorithms (Q89–Q110)

### Q89. ★★ — Red-Black Tree Properties
Which of the following is/are TRUE about Red-Black trees?
(a) The longest path from root to leaf is at most twice the shortest path
(b) Every Red-Black tree with $n$ nodes has height $\leq 2\log_2(n+1)$
(c) Insertion requires at most 2 rotations
(d) All of the above

---

### Q90. ★★★ — RB Tree vs AVL [GATE Style]
A Red-Black tree and an AVL tree both contain the same $n$ keys. Which tree has smaller height? Which is better for frequent insertions/deletions? Justify.

---

### Q91. ★★ — B-Tree Order
A B-tree of order 5 (max 5 children) has the following constraints. What are:
(a) Maximum keys per node?
(b) Minimum keys in a non-root node?
(c) Minimum children of a non-root internal node?

---

### Q92. ★★★ — B-Tree Height [GATE Style]
A B-tree of order 100 stores 999,999 keys. What is the maximum possible height?

---

### Q93. ★★★ — B+ Tree vs B-Tree [GATE Style]
In a B+ tree of order $p$ with block size 4096 bytes, key size 10 bytes, data pointer 8 bytes, block pointer 6 bytes:
(a) What is the internal node order $p$?
(b) What is the leaf node order $p_L$?
(c) If the tree has 3 levels (root + 1 internal + leaves), what is the maximum number of records?

---

### Q94. ★★ — Trie 
How many nodes does a trie contain for the set of strings: {"car", "card", "care", "cat", "do", "dog"}? What is the time to search for "care" in this trie?

---

### Q95. ★★★ — Union-Find [GATE Style]
Starting from 8 singleton sets {0}, {1}, ..., {7}, perform the following union operations using Union by Rank: Union(0,1), Union(2,3), Union(4,5), Union(6,7), Union(0,2), Union(4,6), Union(0,4). Draw the final tree. What is the rank of the root?

---

### Q96. ★★★ — Amortized Analysis [GATE Style]
A stack supports Push ($O(1)$) and MultiPop(k) (pops min(k, size) elements). Using the aggregate method, show that any sequence of $n$ Push and MultiPop operations takes $O(n)$ total time.

---

### Q97. ★★★ — SCC Kosaraju [GATE Style]  
Consider a directed graph with vertices {A, B, C, D, E, F} and edges: A→B, B→C, C→A, B→D, D→E, E→F, F→D. 
(a) How many strongly connected components are there?
(b) List the vertices in each SCC.
(c) Draw the SCC DAG (condensation graph).

---

### Q98. ★★ — Network Flow
In a flow network with source S and sink T:
```
S→A: 10, S→B: 8, A→B: 5, A→T: 7, B→T: 10
```
Find the maximum flow from S to T. What is the minimum cut?

---

### Q99. ★★★ — Optimal BST [GATE Style]
Three keys $k_1 < k_2 < k_3$ with search probabilities $p_1 = 0.3, p_2 = 0.1, p_3 = 0.2$ and dummy key probabilities $q_0 = 0.1, q_1 = 0.1, q_2 = 0.1, q_3 = 0.1$. What is the expected cost of the optimal BST? Which key is at the root?

---

### Q100. ★★★ — B-Tree Insertion [GATE Style]
Insert keys 10, 20, 30, 40, 50, 60, 70 (in order) into an initially empty B-tree of order 3 (max 2 keys per node). Show the tree after each split.

---

### Q101. ★★ — Trie vs Hash Table
Compare Trie and Hash Table for storing a dictionary of $n$ strings, each of length up to $L$:
(a) Search time complexity
(b) Prefix search support  
(c) Space usage

---

### Q102. ★★★ — Ford-Fulkerson [GATE Style]
Given the flow network:
```
S→A:16, S→B:13, A→B:10, A→C:12, B→A:4, B→D:14, C→B:9, C→T:20, D→C:7, D→T:4
```
Find the max flow using Edmonds-Karp (BFS augmentation). Show each augmenting path.

---

### Q103. ★★★ — Cycle Detection for Union-Find [GATE Style]
Use Union-Find to detect if the following undirected graph has a cycle: Edges = {(0,1), (1,2), (2,3), (3,0), (2,4)}. Show the Union-Find forest after processing each edge.

---

### Q104. ★★ — B+ Tree Range Query
In a B+ tree with leaf nodes linked as: [5,8] → [12,15] → [20,25] → [30,35], how would you find all keys in the range [10, 30]? How many leaf nodes are accessed?

---

### Q105. ★★★ — Max Flow Min Cut [GATE 2023 Style]
State and prove the Max-Flow Min-Cut theorem. What is the relationship between maximum bipartite matching and maximum flow?

---

### Q106. ★★★ — Tarjan's SCC [GATE Style]
Run Tarjan's algorithm on: 1→2, 2→3, 3→1, 3→4, 4→5, 5→6, 6→4. Show disc[], low[], and stack at each step. Identify SCCs.

---

### Q107. ★★ — Amortized: Binary Counter
A $k$-bit binary counter is incremented $n$ times (starting from 0). What is the total number of bit flips? What is the amortized cost per increment?

---

### Q108. ★★★ — B-Tree Deletion [GATE Style]
Given a B-tree of order 3 with structure:
```
        [30]
       /    \
    [10,20]  [40,50]
```
Delete key 10. Then delete key 30. Show the tree after each deletion.

---

### Q109. ★★★ — Red-Black Tree Insertion [GATE Style]
Insert keys 41, 38, 31, 12, 19, 8 into an initially empty Red-Black tree. Show the tree after each insertion, noting rotations and recolorings.

---

### Q110. ★★★ — Kruskal's with Union-Find [GATE Style]
Use Kruskal's algorithm with Union-Find (union by rank + path compression) on the graph:
Edges (sorted): (A-B,1), (C-D,2), (A-C,3), (B-D,4), (B-C,5), (D-E,6), (C-E,7)
Show the Union-Find forest after adding each MST edge.

---

> **Study tip:** Time yourself — aim for 2-3 minutes per MCQ/NAT at GATE level. For numerical answers, verify by tracing on paper.
