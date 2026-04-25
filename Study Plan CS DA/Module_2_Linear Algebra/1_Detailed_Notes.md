# 📊 Linear Algebra — Comprehensive Notes for GATE 2027 (CS/IT/DA) 🎯

> 🌟 **"Linear Algebra is the language of data, AI, graphics, and the modern digital world. If you understand matrices, you understand everything from Google Search to self-driving cars!"**

> 💡 **Beginner Analogy:** Imagine you're arranging a music equalizer 🎶 — each slider controls a different frequency (bass, mid, treble). Vectors are like slider positions, matrices are like effects that transform the entire sound. Linear Algebra is the math behind mixing, transforming, and understanding these combinations!

---

## 📑 Table of Contents

| Module | Chapter | Key Topics |
|---|---|---|
| **MODULE 1: Basics** 🧱 | Ch 1: Vectors | Linear independence/dependence, Dot product, Norms, Cauchy-Schwarz, Orthogonality |
| | Ch 2: Spanning Sets | Span, Basis, Dimension |
| | Ch 3: Matrix Operations | Multiplication, Transpose, Permutation matrices, Rank-1 matrices |
| | Ch 4: Systems of Equations | Ax = b, Homogeneous/Non-homogeneous, Solution structure |
| | Ch 5: Gaussian Elimination | REF/RREF, Rank, Rank-Nullity, Rank inequalities |
| | Ch 6: Solutions by Rank | Complete framework, Square matrix case |
| | Ch 7: Determinants | Properties, Cofactors, Vandermonde, Block determinants |
| | Ch 8: Inverse | Adjoint method, Gauss-Jordan, Invertible Matrix Theorem |
| | Ch 9: Cramer's Rule | Solving systems using determinants |
| **MODULE 2: Core** 🌟 | Ch 10: Eigenvalues & Eigenvectors | Characteristic equation, Properties, Gershgorin, Spectral theorem |
| | Ch 11: Cayley-Hamilton | Applications, Computing Aⁿ, Minimal polynomial |
| | Ch 12: Rank and Eigenvalues | Connections, Special matrices |
| | Ch 13: LU Decomposition | Factoring, PA = LU, Cholesky |
| | Ch 14: Types of Matrices | Symmetric, Orthogonal, Idempotent, Nilpotent, Stochastic, etc. |
| **MODULE 3: Advanced** 🏗️ | Ch 15: Vector Spaces | Axioms, Inner product spaces, Dimension formula |
| | Ch 16: Subspaces | Tests, Examples, Direct sum |
| | Ch 17: Basis & Dimension | Key results |
| | Ch 18: Four Fundamental Subspaces | Column/Row/Null/Left-null spaces |
| | Ch 19: Change of Basis | Transition matrices |
| | Ch 20: Linear Transformations | Kernel/Image, Standard matrices, Rotation/Reflection/Shear |
| | Ch 21: Diagonalization | Similar matrices, Orthogonal diagonalization, Jordan form |
| | Ch 22: Projection & Gram-Schmidt | QR decomposition, Least squares |
| | Ch 23: SVD | Computation, Pseudoinverse, Matrix norms |
| | Ch 24: Positive Definite Matrices | Tests, Quadratic forms, Cholesky |
| | Ch 25: Block Matrices | Schur complement |
| | Ch 26: Projection Matrices | Orthogonal decomposition, Least squares |
| **Appendix** 🏆 | | GATE PYQ Map, Formula Reference, GATE Traps, Quick Solver Strategies |

---

## 🏭 Where is Linear Algebra Used in the Real World?

| 🌍 Industry | 🔧 Application | 📌 LA Concept Used |
|---|---|---|
| **Google Search** 🔍 | PageRank (ranks 130 trillion pages) | Eigenvalues, Matrix-vector multiplication |
| **Netflix/Spotify** 🎬🎵 | Recommendation systems | SVD (Singular Value Decomposition) |
| **Self-Driving Cars** 🚗 | Sensor fusion, SLAM | Matrix transformations, Least squares |
| **Instagram Filters** 📸 | Image transformations (rotate, blur, sharpen) | Matrix operations on pixel arrays |
| **Machine Learning/AI** 🤖 | Neural network training (backpropagation) | Matrix calculus, Gradient descent |
| **3D Games & Movies** 🎮 | Character animation, camera movement | Transformation matrices (rotate, scale, translate) |
| **Quantum Computing** ⚗️ | Qubit operations | Unitary matrices, Eigenvalues |
| **Medical Imaging (MRI/CT)** 🏥 | Image reconstruction | Matrix inversion, Fourier transforms |
| **GPS & Navigation** 📍 | Triangulating your position | System of linear equations |
| **Stock Market** 📈 | Portfolio optimization, risk analysis | Covariance matrices, PCA |
| **Data Compression (JPEG)** 🖼️ | Image compression | SVD, Eigendecomposition |
| **Natural Language Processing** 💬 | Word embeddings (Word2Vec, BERT) | Vector spaces, Matrix factorization |

---

## 📊 GATE Weightage & Strategy

| Parameter | Details |
|---|---|
| **Expected Marks** | 5–10 marks every year (1–2 questions of 1 or 2 marks each) |
| **Frequency** | At least 1 question every single year since 2003 |
| **Difficulty** | Medium to Hard — conceptual + computational |
| **Time to Master** | 3–4 weeks (first pass) + 1 week revision |
| **ROI** | Very High — limited syllabus, high scoring if prepared well |

---

# 🧱 MODULE 1: BASICS OF LINEAR ALGEBRA

---

## Chapter 1: Vectors — Linear Independence and Dependence 🧭

### 1.1 What is a Vector? 🧭

> 💡 **Beginner Analogy — GPS Coordinates 📍:**
> A vector is like a GPS location! Your position on Earth needs 2 numbers: latitude & longitude. That's a vector in $\mathbb{R}^2$! Add altitude, and you have $\mathbb{R}^3$. In machine learning, a single data point might have 1000+ features — that's a vector in $\mathbb{R}^{1000}$!
>
> 🏭 **Production Example:** When Spotify represents a song, it uses a vector of ~100 features (tempo, energy, danceability, etc.). Each song = one vector. Similar songs = vectors pointing in similar directions! 🎵

A vector is an ordered list of numbers. In $\mathbb{R}^n$, a vector has $n$ components.

$$\vec{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix}$$

**Real-world analogy:** Think of a vector as a recipe — each component tells you how much of a certain ingredient to use.

### 1.2 Linear Combination

A vector $\vec{w}$ is a **linear combination** of vectors $\vec{v}_1, \vec{v}_2, \ldots, \vec{v}_k$ if there exist scalars $c_1, c_2, \ldots, c_k$ such that:

$$\vec{w} = c_1\vec{v}_1 + c_2\vec{v}_2 + \cdots + c_k\vec{v}_k$$

**Example:**
$$\begin{bmatrix} 5 \\ 7 \end{bmatrix} = 2\begin{bmatrix} 1 \\ 2 \end{bmatrix} + 3\begin{bmatrix} 1 \\ 1 \end{bmatrix}$$

### 1.2A Affine Combination vs Linear Combination (Extension) 🌈

- Linear combination: coefficients can be anything.
- Affine combination: coefficients sum to 1.

$$\vec{w} = \sum_i c_i\vec{v}_i, \quad \sum_i c_i = 1$$

Why useful? In ML and graphics, interpolation between points uses affine combinations (convex combinations are affine combinations with $c_i \ge 0$). This connects vectors to geometry and optimization.

### 1.3 Linear Independence (⭐ FREQUENTLY ASKED) 🔗

> 💡 **Beginner Analogy — The Music Band 🎸:**
> Imagine a band where each musician plays a DIFFERENT instrument:
> - Guitar 🎸, Drums 🥁, Keyboard 🎹 = **Linearly Independent** (each brings something unique!)
> - But if you add a second guitarist playing the EXACT same notes as the first = **Linearly Dependent** (redundant! 😴)
> - Even worse: if the second guitarist plays exactly TWICE as loud as the first = still dependent (just a scaled copy!)
>
> In data science, linearly dependent features in your dataset = **redundant features** that waste computation without adding information! That's why PCA (Principal Component Analysis) removes them. 🧩

> 🌍 **Real-World:** In Amazon's recommendation engine, if "purchase history" and "browsing history" always move together (one is a scaled version of the other), they are linearly dependent — you only need one!

**Definition:** Vectors $\vec{v}_1, \vec{v}_2, \ldots, \vec{v}_k$ are **linearly independent (LI)** if the only solution to:

$$c_1\vec{v}_1 + c_2\vec{v}_2 + \cdots + c_k\vec{v}_k = \vec{0}$$

is $c_1 = c_2 = \cdots = c_k = 0$ (the trivial solution).

**Definition:** Vectors are **linearly dependent (LD)** if there exist scalars $c_1, c_2, \ldots, c_k$, **not all zero**, such that:

$$c_1\vec{v}_1 + c_2\vec{v}_2 + \cdots + c_k\vec{v}_k = \vec{0}$$

**Intuition:** Linearly dependent means at least one vector is "redundant" — it can be written as a combination of the others. Linearly independent means every vector brings "new information."

### Key Results on Linear Independence

| Result | Explanation |
|---|---|
| $k$ vectors in $\mathbb{R}^n$ with $k > n$ | Always **linearly dependent** |
| $n$ vectors in $\mathbb{R}^n$ are LI | iff $\det(A) \neq 0$ where $A$ has vectors as columns |
| If one vector is $\vec{0}$ | The set is always **linearly dependent** |
| Two vectors are LD | iff one is a scalar multiple of the other |
| A single non-zero vector | is always **linearly independent** |
| Linear combination of LI vectors | is **unique** |

### 1.4 How to Check Linear Independence

**Method 1: Determinant (for n vectors in $\mathbb{R}^n$)**
- Form matrix $A$ with vectors as columns
- If $\det(A) \neq 0$ → LI
- If $\det(A) = 0$ → LD

**Method 2: Row Reduction (general method)**
- Form matrix with vectors as columns (or rows)
- Row reduce to echelon form
- If every column has a pivot → LI
- If any column lacks a pivot → LD (that column's vector is dependent)

---

**Example 1 (Easy):** Are $\vec{v}_1 = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$ and $\vec{v}_2 = \begin{bmatrix} 3 \\ 6 \end{bmatrix}$ linearly independent?

**Solution:** $\vec{v}_2 = 3\vec{v}_1$, so they are **linearly dependent**.

Alternatively: $\det\begin{bmatrix} 1 & 3 \\ 2 & 6 \end{bmatrix} = 6 - 6 = 0$ → LD ✓

---

**Example 2 (Medium):** Are $\vec{v}_1 = \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}$, $\vec{v}_2 = \begin{bmatrix} 1 \\ 1 \\ 0 \end{bmatrix}$, $\vec{v}_3 = \begin{bmatrix} 1 \\ 1 \\ 1 \end{bmatrix}$ LI?

**Solution:**
$$\det\begin{bmatrix} 1 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 1 \end{bmatrix} = 1(1 \cdot 1 - 1 \cdot 0) - 1(0 \cdot 1 - 1 \cdot 0) + 1(0 \cdot 0 - 1 \cdot 0) = 1 \neq 0$$

→ **Linearly Independent** ✓

---

**Example 3 (GATE Level):** For what value of $k$ are $\begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix}$, $\begin{bmatrix} 2 \\ 1 \\ 0 \end{bmatrix}$, $\begin{bmatrix} 1 \\ k \\ 3 \end{bmatrix}$ linearly dependent?

**Solution:** Set determinant = 0:
$$\det\begin{bmatrix} 1 & 2 & 1 \\ 2 & 1 & k \\ 3 & 0 & 3 \end{bmatrix} = 0$$

Expanding along R3: $3(2k - 1) - 0 + 3(k - 2) = 0$

$6k - 3 + 3k - 6 = 0 \implies 9k = 9 \implies k = 1$

---

### 1.5 Uniqueness of Linear Combination of LI Vectors

> **Theorem:** If vectors $\vec{v}_1, \vec{v}_2, \ldots, \vec{v}_k$ are linearly independent, then any linear combination that produces a given vector $\vec{w}$ is **unique**.

**Proof sketch:** Suppose $\vec{w} = c_1\vec{v}_1 + \cdots + c_k\vec{v}_k = d_1\vec{v}_1 + \cdots + d_k\vec{v}_k$. Then $(c_1-d_1)\vec{v}_1 + \cdots + (c_k-d_k)\vec{v}_k = \vec{0}$. Since vectors are LI, all $(c_i - d_i) = 0$, so $c_i = d_i$ for all $i$.

### 1.6 Dot Product (Inner Product) (⭐⭐ FUNDAMENTAL) 🎯

> 💡 **Beginner Analogy — The Similarity Meter 📊:**
> The dot product measures how much two vectors "agree" with each other:
> - **Large positive** → pointing in similar directions (vectors "agree") 🤝
> - **Zero** → perpendicular/orthogonal (completely independent!) ⊥
> - **Negative** → pointing in opposite directions (vectors "disagree") 🔄
>
> 🏭 **Production Use:**
> - **Google Search** 🔍: Cosine similarity (dot product ÷ norms) ranks how relevant a document is to your query
> - **Spotify** 🎵: Song similarity = dot product of feature vectors
> - **Neural Networks** 🧠: Every neuron computes a dot product of inputs and weights!
> - **Recommendation engines** 🛒: Amazon's "customers who bought this also bought..." uses dot products

**Definition:** For $\vec{u}, \vec{v} \in \mathbb{R}^n$:
$$\vec{u} \cdot \vec{v} = \vec{u}^T\vec{v} = u_1v_1 + u_2v_2 + \cdots + u_nv_n = \sum_{i=1}^n u_i v_i$$

**Properties of Dot Product:**

| Property | Formula |
|---|---|
| **Commutative** | $\vec{u} \cdot \vec{v} = \vec{v} \cdot \vec{u}$ |
| **Distributive** | $\vec{u} \cdot (\vec{v} + \vec{w}) = \vec{u} \cdot \vec{v} + \vec{u} \cdot \vec{w}$ |
| **Scalar** | $(c\vec{u}) \cdot \vec{v} = c(\vec{u} \cdot \vec{v})$ |
| **Non-negative** | $\vec{u} \cdot \vec{u} \geq 0$, with equality iff $\vec{u} = \vec{0}$ |
| **Geometric** | $\vec{u} \cdot \vec{v} = \|\vec{u}\| \|\vec{v}\| \cos\theta$ |

### 1.7 Vector Norms (Length) 📏

**Euclidean norm (2-norm):**
$$\|\vec{v}\| = \|\vec{v}\|_2 = \sqrt{v_1^2 + v_2^2 + \cdots + v_n^2} = \sqrt{\vec{v} \cdot \vec{v}} = \sqrt{\vec{v}^T\vec{v}}$$

**Other norms (⭐ GATE DA):**

| Norm | Formula | Geometric Meaning |
|---|---|---|
| **1-norm** (Manhattan) | $\|\vec{v}\|_1 = \sum_i |v_i|$ | Taxicab distance 🚕 |
| **2-norm** (Euclidean) | $\|\vec{v}\|_2 = \sqrt{\sum_i v_i^2}$ | Straight-line distance 📏 |
| **p-norm** | $\|\vec{v}\|_p = (\sum_i |v_i|^p)^{1/p}$ | General Lp distance |
| **∞-norm** (Max) | $\|\vec{v}\|_\infty = \max_i |v_i|$ | Largest component |

**Unit vector:** $\hat{v} = \frac{\vec{v}}{\|\vec{v}\|}$ (normalization — direction without magnitude)

> 🏭 **ML Connection:** L1 norm → Lasso regularization (sparse solutions). L2 norm → Ridge regularization (small weights). The choice of norm determines the behaviour of your ML model!

### 1.8 Angle Between Vectors & Cosine Similarity 📐

$$\cos\theta = \frac{\vec{u} \cdot \vec{v}}{\|\vec{u}\| \|\vec{v}\|}$$

| $\cos\theta$ | Relationship |
|---|---|
| $= 1$ | Same direction (parallel) |
| $= 0$ | **Orthogonal** (perpendicular) ⊥ |
| $= -1$ | Opposite direction (anti-parallel) |

> 🏭 **Cosine similarity** is the #1 similarity metric in NLP, search engines, and recommendation systems!

### 1.9 Cauchy-Schwarz Inequality (⭐ IMPORTANT) ⚡

$$|\vec{u} \cdot \vec{v}| \leq \|\vec{u}\| \cdot \|\vec{v}\|$$

**Equality holds iff $\vec{u}$ and $\vec{v}$ are linearly dependent** (one is a scalar multiple of the other).

> 🎯 **GATE Application:** Used to prove many other inequalities. If asked "prove that $|\sum a_ib_i| \leq \sqrt{\sum a_i^2} \cdot \sqrt{\sum b_i^2}$" — that's Cauchy-Schwarz!

### 1.10 Triangle Inequality 📐

$$\|\vec{u} + \vec{v}\| \leq \|\vec{u}\| + \|\vec{v}\|$$

**Equality holds iff $\vec{u}$ and $\vec{v}$ point in the same direction** ($\vec{u} = c\vec{v}$ for $c \geq 0$).

### 1.11 Orthogonality (⭐⭐ VERY IMPORTANT) ⊥

> Two vectors $\vec{u}, \vec{v}$ are **orthogonal** iff $\vec{u} \cdot \vec{v} = 0$.

**Orthogonal Set:** A set of vectors where every pair is orthogonal.

**Orthonormal Set:** An orthogonal set where every vector has norm 1.

**Key Results:**

| Result | Detail |
|---|---|
| Orthogonal vectors are LI | Any orthogonal set of non-zero vectors is linearly independent |
| **Pythagorean theorem** | If $\vec{u} \perp \vec{v}$: $\|\vec{u} + \vec{v}\|^2 = \|\vec{u}\|^2 + \|\vec{v}\|^2$ |
| **Orthogonal complement** | $W^\perp = \{\vec{v} : \vec{v} \cdot \vec{w} = 0 \text{ for all } \vec{w} \in W\}$ |
| $\dim(W) + \dim(W^\perp) = n$ | Dimension splits exactly |
| $(W^\perp)^\perp = W$ | Double complement = original |
| $\mathbb{R}^n = W \oplus W^\perp$ | Every vector decomposes uniquely |

### 1.11A Orthogonal Projection Distance Formula (Micro-topic) 🎯

If $\hat{b}$ is projection of $b$ onto subspace $W$, then $b = \hat{b} + e$ with $\hat{b} \in W$ and $e \in W^\perp$.

$$\|b\|^2 = \|\hat{b}\|^2 + \|e\|^2$$

This is the geometric backbone of least squares, linear regression, and bias-variance style interpretation of errors.

### 1.12 Cross Product (in $\mathbb{R}^3$ only) ✖️

$$\vec{u} \times \vec{v} = \det\begin{bmatrix} \vec{i} & \vec{j} & \vec{k} \\ u_1 & u_2 & u_3 \\ v_1 & v_2 & v_3 \end{bmatrix} = \begin{bmatrix} u_2v_3 - u_3v_2 \\ u_3v_1 - u_1v_3 \\ u_1v_2 - u_2v_1 \end{bmatrix}$$

**Properties:**

| Property | Formula |
|---|---|
| **Anti-commutative** | $\vec{u} \times \vec{v} = -(\vec{v} \times \vec{u})$ |
| **Result orthogonal** | $(\vec{u} \times \vec{v}) \perp \vec{u}$ and $(\vec{u} \times \vec{v}) \perp \vec{v}$ |
| **Magnitude** | $\|\vec{u} \times \vec{v}\| = \|\vec{u}\| \|\vec{v}\| \sin\theta$ = area of parallelogram |
| **Parallel detection** | $\vec{u} \times \vec{v} = \vec{0} \iff \vec{u} \parallel \vec{v}$ |
| **NOT associative** | $\vec{u} \times (\vec{v} \times \vec{w}) \neq (\vec{u} \times \vec{v}) \times \vec{w}$ in general |

**Scalar Triple Product:** $\vec{u} \cdot (\vec{v} \times \vec{w}) = \det[\vec{u} | \vec{v} | \vec{w}]$ = signed volume of parallelepiped

---

### 1.13 Micro-Topic Coverage Checklist for Vectors ✅

- 🔹 Linear combination, affine combination, convex combination, conic combination
- 🔹 Linear dependence certificate: find one non-trivial relation explicitly
- 🔹 Independence by determinant, row reduction, and geometric reasoning
- 🔹 Dot product identities, bilinearity, symmetry, positivity
- 🔹 Norms and norm-equivalence intuition in finite-dimensional spaces
- 🔹 Orthogonality, orthonormality, orthogonal complement, decomposition theorem
- 🔹 Angle formula, cosine similarity, sign interpretation, zero-angle traps
- 🔹 Cauchy-Schwarz, triangle inequality, equality conditions
- 🔹 Cross product, scalar triple product, signed area/volume
- 🔹 GATE traps: zero vector in a set, too many vectors in too small a space, duplicate direction vectors

### 1.14 Advanced Observations You Must Not Miss 🧠

1. If a set has more vectors than the ambient dimension, dependence is automatic. No computation needed.
2. Orthogonal non-zero vectors are always linearly independent, even if their coordinates look messy.
3. In $\mathbb{R}^n$, pairwise orthogonality is stronger than independence and makes coordinates easy to extract.
4. If vectors are linearly independent, every coordinate representation in that set is unique.
5. If vectors are merely spanning, representation may not be unique.
6. The zero vector instantly makes a set dependent, regardless of all other vectors.
7. In DA problems, cosine similarity compares direction, not scale; normalization matters.
8. Equality in Cauchy-Schwarz means one vector is a scalar multiple of the other, not just “similar looking”.

### 1.15 Worked Example Bank: Vectors 🎯

**Example 1 (Medium):** Determine whether $
\left\{\begin{bmatrix}1\\2\\1\end{bmatrix},\begin{bmatrix}2\\4\\2\end{bmatrix},\begin{bmatrix}0\\1\\1\end{bmatrix}\right\}$ is LI.

**Solution:** The second vector is $2$ times the first. Hence the set is linearly dependent without any determinant calculation. ✅

**Example 2 (Hard):** Find the projection of $
\vec{b}=\begin{bmatrix}3\\1\\2\end{bmatrix}$ on $
\vec{a}=\begin{bmatrix}1\\-1\\2\end{bmatrix}$.

**Solution:**
$$\text{proj}_{a}(b)=\frac{a^Tb}{a^Ta}a=\frac{3-1+4}{1+1+4}a=a$$
So the projection is $
\begin{bmatrix}1\\-1\\2\end{bmatrix}$. ✅

**Example 3 (Hard):** Find a unit vector orthogonal to both $
\begin{bmatrix}1\\1\\0\end{bmatrix}$ and $
\begin{bmatrix}1\\0\\1\end{bmatrix}$.

**Solution:** Use cross product:
$$\begin{bmatrix}1\\1\\0\end{bmatrix}\times\begin{bmatrix}1\\0\\1\end{bmatrix}=\begin{bmatrix}1\\-1\\-1\end{bmatrix}$$
Norm $=\sqrt{3}$. Unit vector $=\frac{1}{\sqrt{3}}\begin{bmatrix}1\\-1\\-1\end{bmatrix}$. ✅

### 1.16 PYQ-Style Hard Traps: Vectors 🔥

**Q1. Difficulty: Hard** 😵
If $u,v,w \in \mathbb{R}^3$ satisfy $u+v+w=0$ and $u,v$ are LI, must $\{u,v,w\}$ be dependent?

**Answer:** Yes. Since $w=-(u+v)$, one vector is a linear combination of the others. Dependent. ✅

**Q2. Difficulty: Hard** 😵
Let $u,v$ be non-zero with $u\cdot v=\|u\|\,\|v\|$. Then which statement is true?

**Answer:** By equality in Cauchy-Schwarz, $u=cv$ with $c>0$. Same direction, not opposite. ✅

**Q3. Difficulty: Hard** 😵
Can three non-zero vectors in $\mathbb{R}^2$ be pairwise orthogonal?

**Answer:** No. At most two non-zero mutually orthogonal vectors can exist in $\mathbb{R}^2$. ✅

**Q4. Difficulty: Hard** 😵
If $u\cdot v=0$ and $u\cdot w=0$, does it follow that $u\cdot(v+w)=0$?

**Answer:** Yes, by distributivity of dot product. ✅

**Q5. Difficulty: Hard** 😵
If $\|u+v\|=\|u-v\|$, what can you conclude?

**Answer:** Expanding both squares gives $4u\cdot v=0$, so $u\perp v$. ✅

**Q6. Difficulty: Hard** 😵
Suppose $\|u+v\|=\|u\|+\|v\|$. What must hold?

**Answer:** Equality in triangle inequality implies $u=cv$ for some $c\ge 0$. Same direction. ✅

---

## Chapter 2: Spanning Sets and Filling the Space

### 2.1 Span

The **span** of vectors $\vec{v}_1, \vec{v}_2, \ldots, \vec{v}_k$ is the set of **all possible linear combinations**:

$$\text{Span}(\vec{v}_1, \ldots, \vec{v}_k) = \{c_1\vec{v}_1 + c_2\vec{v}_2 + \cdots + c_k\vec{v}_k \mid c_i \in \mathbb{R}\}$$

**Intuition:** Span is the "reach" of your vectors — all the places you can get to by combining them.

### 2.2 Geometric Interpretation

| Vectors | Span |
|---|---|
| 1 non-zero vector in $\mathbb{R}^2$ | A line through origin |
| 2 LI vectors in $\mathbb{R}^2$ | All of $\mathbb{R}^2$ (the entire plane) |
| 2 LD vectors in $\mathbb{R}^2$ | Just a line |
| 1 non-zero vector in $\mathbb{R}^3$ | A line through origin |
| 2 LI vectors in $\mathbb{R}^3$ | A plane through origin |
| 3 LI vectors in $\mathbb{R}^3$ | All of $\mathbb{R}^3$ |

### 2.3 Basis

A **basis** of a vector space $V$ is a set of vectors that is:
1. **Linearly Independent**
2. **Spans** $V$

> The number of vectors in any basis of $V$ is called the **dimension** of $V$, denoted $\dim(V)$.

**Standard basis of $\mathbb{R}^3$:**
$$\vec{e}_1 = \begin{bmatrix} 1\\0\\0 \end{bmatrix}, \quad \vec{e}_2 = \begin{bmatrix} 0\\1\\0 \end{bmatrix}, \quad \vec{e}_3 = \begin{bmatrix} 0\\0\\1 \end{bmatrix}$$

---

### 2.4 Micro-Topic Coverage Checklist for Span, Basis, Dimension ✅

- 🔹 Span as set of all linear combinations
- 🔹 Span as smallest subspace containing a set
- 🔹 Redundant vectors inside spanning sets
- 🔹 Basis extraction from spanning set using pivot columns
- 🔹 Basis extension from LI set to a basis of the whole space
- 🔹 Coordinates of a vector relative to a basis
- 🔹 Dimension of polynomial space, matrix space, solution spaces
- 🔹 Row-space basis vs column-space basis distinction
- 🔹 Same space can have many bases but same dimension
- 🔹 GATE trap: spanning does not imply independence; independence does not imply spanning

### 2.5 Missing Concept: Span as the Smallest Subspace 🌱

For any set $S=\{v_1,\dots,v_k\}$,
$$\text{Span}(S)=\bigcap_{W\supseteq S,\;W\text{ subspace}}W$$

This matters because when GATE asks for “the subspace generated by vectors”, the correct object is the span, not just one lucky combination.

### 2.6 Coordinates Relative to a Basis 🎛️

If $B=\{b_1,\dots,b_n\}$ is a basis of $V$, then every $v\in V$ can be written uniquely as
$$v=c_1b_1+\cdots+c_nb_n$$
The coordinate vector is
$$[v]_B=\begin{bmatrix}c_1\\ \vdots \\ c_n\end{bmatrix}$$

Coordinate problems often hide a system-solving question under basis language.

### 2.7 Worked Example Bank: Span and Basis 🎯

**Example 1 (Medium):** Find a basis for the span of
$$\left\{\begin{bmatrix}1\\2\\3\end{bmatrix},\begin{bmatrix}2\\4\\6\end{bmatrix},\begin{bmatrix}1\\1\\0\end{bmatrix}\right\}$$

**Solution:** The second vector is redundant. A basis is
$$\left\{\begin{bmatrix}1\\2\\3\end{bmatrix},\begin{bmatrix}1\\1\\0\end{bmatrix}\right\}$$
Dimension $=2$. ✅

**Example 2 (Hard):** Does $\{1+x,1-x,x^2\}$ form a basis of $P_2$?

**Solution:** Write in standard basis $\{1,x,x^2\}$:
$$1+x=(1,1,0),\quad 1-x=(1,-1,0),\quad x^2=(0,0,1)$$
Determinant of coefficient matrix is $-2\ne 0$. Hence basis of $P_2$. ✅

**Example 3 (Hard):** Find coordinates of $v=(5,1)$ in basis $B=\{(1,1),(2,-1)\}$.

**Solution:** Solve
$$c_1(1,1)+c_2(2,-1)=(5,1)$$
So $c_1+2c_2=5$, $c_1-c_2=1$. Hence $c_2=\tfrac{4}{3}$, $c_1=\tfrac{7}{3}$. ✅

### 2.8 PYQ-Style Hard Traps: Span and Basis 🔥

**Q1. Difficulty: Hard** 😵
Can a linearly independent set in $\mathbb{R}^4$ with 3 vectors span $\mathbb{R}^4$?

**Answer:** No. Need 4 vectors in any basis of $\mathbb{R}^4$. ✅

**Q2. Difficulty: Hard** 😵
Can a spanning set of $\mathbb{R}^3$ with 4 vectors be a basis?

**Answer:** No. It spans, but 4 vectors in $\mathbb{R}^3$ must be dependent. ✅

**Q3. Difficulty: Hard** 😵
If a set spans $V$, can one remove vectors and still span $V$?

**Answer:** Yes, remove redundant vectors until a basis remains. ✅

**Q4. Difficulty: Hard** 😵
If a set is LI, can more vectors be added while preserving LI?

**Answer:** Possibly, until a basis is reached. But not always if already full dimension. ✅

**Q5. Difficulty: Hard** 😵
What is the dimension of the solution space of a homogeneous system with 5 variables and rank 3?

**Answer:** By rank-nullity, dimension $=5-3=2$. ✅

**Q6. Difficulty: Hard** 😵
Do two different bases of the same finite-dimensional vector space always have the same number of vectors?

**Answer:** Yes. That common number is the dimension. ✅

---

## Chapter 3: Matrix-Vector and Matrix-Matrix Multiplication

### 3.1 Matrix-Vector Multiplication

If $A$ is an $m \times n$ matrix and $\vec{x}$ is an $n \times 1$ vector, then:

$$A\vec{x} = x_1 \vec{a}_1 + x_2 \vec{a}_2 + \cdots + x_n \vec{a}_n$$

where $\vec{a}_i$ are the **columns** of $A$.

> **Key Insight:** $A\vec{x}$ is a **linear combination of the columns of A** with weights from $\vec{x}$.

**Example:**
$$\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}\begin{bmatrix} 2 \\ 1 \end{bmatrix} = 2\begin{bmatrix} 1\\3 \end{bmatrix} + 1\begin{bmatrix} 2\\4 \end{bmatrix} = \begin{bmatrix} 4\\10 \end{bmatrix}$$

### 3.2 Matrix-Matrix Multiplication

For $A$ ($m \times n$) and $B$ ($n \times p$): $C = AB$ is $m \times p$ where:

$$C_{ij} = \sum_{k=1}^{n} A_{ik} B_{kj} = (\text{Row } i \text{ of } A) \cdot (\text{Column } j \text{ of } B)$$

**Key Properties:**
| Property | True/False |
|---|---|
| $AB = BA$ (commutativity) | **FALSE** in general |
| $A(BC) = (AB)C$ (associativity) | TRUE |
| $A(B+C) = AB + AC$ (distributivity) | TRUE |
| $(AB)^T = B^T A^T$ | TRUE |
| $AB = 0 \implies A = 0$ or $B = 0$ | **FALSE** |
| $AB = AC \implies B = C$ | **FALSE** (unless $A$ is invertible) |

### 3.3 Ways to Multiply Matrices

1. **Row × Column (standard):** Each entry $C_{ij}$ = dot product of row $i$ of $A$ and column $j$ of $B$
2. **Column picture:** Each column of $C$ = $A$ × (corresponding column of $B$)
3. **Row picture:** Each row of $C$ = (corresponding row of $A$) × $B$
4. **Column × Row (outer product):** $AB = \sum_k (\text{col}_k \text{ of } A)(\text{row}_k \text{ of } B)$ — sum of rank-1 matrices

### 3.4 Transpose — Properties (⭐ MUST KNOW) 🔄

| Property | Formula |
|---|---|
| $(A^T)^T = A$ | Double transpose = original |
| $(A + B)^T = A^T + B^T$ | Transpose distributes over addition |
| $(cA)^T = cA^T$ | Scalar commutes |
| $(AB)^T = B^T A^T$ | ⚠️ **Order reverses!** |
| $(ABC)^T = C^T B^T A^T$ | ⚠️ Reverse order extends |
| $\text{rank}(A) = \text{rank}(A^T)$ | Row rank = Column rank |
| $\det(A^T) = \det(A)$ | Transpose preserves determinant |
| $(A^{-1})^T = (A^T)^{-1}$ | Inverse and transpose commute |

> 🎯 **GATE Trap:** $(AB)^T \neq A^T B^T$! The order REVERSES. This is like taking off shoes and socks — you put on socks first then shoes, but take off shoes first then socks! 🧦👟

### 3.5 Permutation Matrices 🔀

A **permutation matrix** $P$ is a square matrix obtained by permuting the rows (or columns) of the identity matrix.

**Properties:**

| Property | Detail |
|---|---|
| Each row/column has exactly one 1 | Rest are zeros |
| $P^T = P^{-1}$ | Permutation matrices are orthogonal! |
| $\det(P) = \pm 1$ | +1 for even permutation, −1 for odd |
| $P^T P = PP^T = I$ | |
| Product of permutation matrices is a permutation matrix | Closed under multiplication |
| $PA$ = rows of $A$ permuted | Left multiplication permutes rows |
| $AP$ = columns of $A$ permuted | Right multiplication permutes columns |
| Total $n \times n$ permutation matrices | $n!$ |

> 🏭 **Application:** Used in LU decomposition with partial pivoting: $PA = LU$

### 3.6 Rank-1 Matrices & Outer Product 🧱

The **outer product** of $\vec{u} \in \mathbb{R}^m$ and $\vec{v} \in \mathbb{R}^n$ is:
$$\vec{u}\vec{v}^T = \text{an } m \times n \text{ matrix of rank 1 (if both non-zero)}$$

**Properties of rank-1 matrices $A = \vec{u}\vec{v}^T$:**

| Property | Formula |
|---|---|
| Rank | 1 (if $\vec{u}, \vec{v} \neq \vec{0}$) |
| Eigenvalues | $\vec{v}^T\vec{u}$ (one non-zero) and 0 (multiplicity $n-1$) |
| Trace | $\text{tr}(\vec{u}\vec{v}^T) = \vec{v}^T\vec{u} = \vec{u} \cdot \vec{v}$ |
| Determinant | 0 (for $n \geq 2$) |
| $(\vec{u}\vec{v}^T)^k$ | $(\vec{v}^T\vec{u})^{k-1} \vec{u}\vec{v}^T$ |

> 🎯 **GATE Insight:** SVD decomposes ANY matrix as a sum of rank-1 matrices: $A = \sum_i \sigma_i \vec{u}_i \vec{v}_i^T$

### 3.7 Special Matrix Products (⭐ GATE DA Awareness) 🔧

**Hadamard (Element-wise) Product:** $A \circ B$ where $(A \circ B)_{ij} = A_{ij} B_{ij}$
- Same-sized matrices, multiply element by element
- Used in attention mechanisms in Transformers (BERT, GPT)! 🤖

**Kronecker Product:** $A \otimes B$ = block matrix where each $a_{ij}$ is replaced by $a_{ij}B$
- If $A$ is $m \times n$ and $B$ is $p \times q$, then $A \otimes B$ is $mp \times nq$
- $(A \otimes B)(C \otimes D) = (AC) \otimes (BD)$ (mixed product property)
- Used in quantum computing and signal processing

### 3.8 Elementary Matrices & Elementary Operations (Exam Micro-topic) 🧩

Each elementary row operation corresponds to left-multiplication by an elementary matrix $E$:

- Row swap: permutation elementary matrix
- Row scaling: diagonal elementary matrix with one changed diagonal entry
- Row replacement: identity with one off-diagonal non-zero entry

If $A \xrightarrow{\text{row ops}} U$, then:

$$E_kE_{k-1}\cdots E_1A = U$$

If reduced to identity, then:

$$E_k\cdots E_1A = I \Rightarrow A^{-1} = E_k\cdots E_1$$

This gives the theorem: every invertible matrix is a product of elementary matrices.

---

### 3.9 Micro-Topic Coverage Checklist for Matrix Operations ✅

- 🔹 Row picture, column picture, outer-product picture
- 🔹 When products are defined and what dimensions result
- 🔹 Non-commutativity examples and cancellation caveats
- 🔹 Elementary matrices and row operations
- 🔹 Permutation matrices and row/column shuffling
- 🔹 Rank-1 matrices and outer products
- 🔹 Trace properties: linearity and cyclic property
- 🔹 Hadamard vs Kronecker product differences
- 🔹 Block multiplication intuition
- 🔹 GATE trap: product zero does not force a zero factor

### 3.10 Trace: The Often-Tested Small Tool 🧮

For square matrices:
$$\operatorname{tr}(A)=\sum_i a_{ii}$$

Key facts:

- $\operatorname{tr}(A+B)=\operatorname{tr}(A)+\operatorname{tr}(B)$
- $\operatorname{tr}(cA)=c\operatorname{tr}(A)$
- $\operatorname{tr}(AB)=\operatorname{tr}(BA)$ whenever both products are defined
- Similar matrices have same trace

### 3.11 Cancellation Law: Exact Condition ⚠️

If $AB=AC$, then from the left you may cancel $A$ only if $A$ is invertible.

If $BA=CA$, then from the right you may cancel $A$ only if $A$ is invertible.

Many GATE traps are built by giving singular $A$ where cancellation fails.

### 3.12 Worked Example Bank: Matrix Multiplication 🎯

**Example 1 (Medium):** Show that $AB=0$ does not imply $A=0$ or $B=0$.

**Solution:** Take
$$A=\begin{bmatrix}1&-1\\1&-1\end{bmatrix},\quad B=\begin{bmatrix}1&1\\1&1\end{bmatrix}$$
Then $AB=0$ but neither matrix is zero. ✅

**Example 2 (Hard):** Write
$$A=\begin{bmatrix}1&2\\3&4\end{bmatrix},\quad B=\begin{bmatrix}5&6\\7&8\end{bmatrix}$$
as sum of outer products.

**Solution:**
$$AB=\begin{bmatrix}1\\3\end{bmatrix}\begin{bmatrix}5&6\end{bmatrix}+\begin{bmatrix}2\\4\end{bmatrix}\begin{bmatrix}7&8\end{bmatrix}$$
This gives the same result as standard multiplication. ✅

**Example 3 (Hard):** If $P$ is a permutation matrix, what is $P^{-1}$?

**Solution:** $P^{-1}=P^T$ because permutation matrices are orthogonal. ✅

### 3.13 PYQ-Style Hard Traps: Matrix Operations 🔥

**Q1. Difficulty: Hard** 😵
If $A$ is $m\times n$ and $B$ is $n\times p$, what is the size of $AB$?

**Answer:** $m\times p$. Inner dimensions must match. ✅

**Q2. Difficulty: Hard** 😵
Can $AB$ be defined while $BA$ is undefined?

**Answer:** Yes. Example: $A$ of size $2\times 3$, $B$ of size $3\times 2$. Then $AB$ exists, $BA$ also exists. But with $A$ $2\times 3$ and $B$ $3\times 4$, only $AB$ exists. ✅

**Q3. Difficulty: Hard** 😵
If $A$ is invertible and $AB=AC$, what follows?

**Answer:** Multiply by $A^{-1}$ on the left, so $B=C$. ✅

**Q4. Difficulty: Hard** 😵
What is $(ABC)^T$?

**Answer:** $C^TB^TA^T$. Reverse the order. ✅

**Q5. Difficulty: Hard** 😵
If $A=u v^T$ with non-zero $u,v$, what is rank$(A)$?

**Answer:** $1$. It is a rank-1 outer product. ✅

**Q6. Difficulty: Hard** 😵
If $P$ is a permutation matrix, what is $\det(P)$?

**Answer:** $\pm 1$, depending on parity of the permutation. ✅

---

## Chapter 4: System of Linear Equations — Ax = b (⭐⭐ MOST FREQUENTLY ASKED) 🔢

> 💡 **Beginner Analogy — The Recipe Problem 🍳:**
> Imagine you're in a kitchen:
> - "2 eggs + 1 cup flour = pancakes" (equation 1)
> - "4 eggs + 3 cups flour = waffles" (equation 2)
> - Question: "How many eggs and flour cups do I need to make BOTH?" → That's solving Ax = b!
>
> 🏭 **Production Applications:**
> - **GPS Triangulation** 📱: Your phone solves a system of equations from 3+ satellite signals to find your exact location!
> - **Electrical Circuit Analysis** ⚡: Kirchhoff's laws produce systems of linear equations (every circuit simulator like SPICE does this)
> - **Machine Learning (Linear Regression)** 📈: Finding the best-fit line through data = solving a system of equations!
> - **Economics (Leontief Model)** 💰: Nobel Prize-winning input-output model uses Ax = b to model entire economies!
> - **Computer Graphics** 🎮: Ray tracing in video games solves millions of Ax = b systems per frame to render realistic lighting!

### 4.1 Formulation

A system of $m$ linear equations in $n$ unknowns:
$$a_{11}x_1 + a_{12}x_2 + \cdots + a_{1n}x_n = b_1$$
$$a_{21}x_1 + a_{22}x_2 + \cdots + a_{2n}x_n = b_2$$
$$\vdots$$
$$a_{m1}x_1 + a_{m2}x_2 + \cdots + a_{mn}x_n = b_m$$

Written compactly as $A\vec{x} = \vec{b}$ where $A$ is $m \times n$.

### 4.2 Geometric Interpretation

- Each equation represents a **hyperplane** (line in 2D, plane in 3D)
- Solution = **intersection** of all hyperplanes
- **No solution:** Hyperplanes don't all intersect at a common point
- **Unique solution:** Hyperplanes meet at exactly one point
- **Infinite solutions:** Hyperplanes meet along a line/plane/etc.

### 4.3 Column Picture of Ax = b

$$A\vec{x} = \vec{b} \quad\Leftrightarrow\quad x_1\vec{a}_1 + x_2\vec{a}_2 + \cdots + x_n\vec{a}_n = \vec{b}$$

**Question becomes:** Can $\vec{b}$ be written as a linear combination of columns of $A$?

### 4.4 Homogeneous System: Ax = 0

- Always has the **trivial solution** $\vec{x} = \vec{0}$
- Has non-trivial solutions iff columns of $A$ are **linearly dependent** iff $\text{rank}(A) < n$
- If $m < n$ (more unknowns than equations), **always** has non-trivial solutions

### 4.5 Non-Homogeneous System: Ax = b

| Condition | Number of Solutions |
|---|---|
| $\text{rank}(A) = \text{rank}([A|b]) = n$ | **Unique solution** |
| $\text{rank}(A) = \text{rank}([A|b]) < n$ | **Infinitely many solutions** |
| $\text{rank}(A) < \text{rank}([A|b])$ | **No solution** |

> This is the **Rouche-Capelli theorem** (a name that is sometimes asked directly). 📌

> **Key Rule:** $\text{rank}([A|b]) - \text{rank}(A)$ can only be 0 or 1, never more.

### 4.6 Structure of Solution

For $A\vec{x} = \vec{b}$:

$$\vec{x}_{\text{general}} = \vec{x}_{\text{particular}} + \vec{x}_{\text{homogeneous}}$$

where $\vec{x}_{\text{particular}}$ is any one solution to $A\vec{x} = \vec{b}$ and $\vec{x}_{\text{homogeneous}}$ is the general solution to $A\vec{x} = \vec{0}$.

**Number of free variables** = $n - \text{rank}(A)$

### 4.7 Parametric Vector Form (Micro-topic) 🧾

Infinite-solution systems should be written in vector-parametric form:

$$x = x_p + t_1v_1 + t_2v_2 + \cdots + t_kv_k$$

where $v_i$ are basis vectors of $N(A)$ and $k = \text{nullity}(A)$.

This form is cleaner than solving variables one by one and directly reveals geometry of the solution set.

---

**Example 4 (GATE Level):** For the system:
$$x + 2y + 3z = 4$$
$$2x + 4y + 7z = 9$$
$$3x + 6y + 10z = 13$$

Find the solution.

**Solution:**
$$[A|b] = \begin{bmatrix} 1 & 2 & 3 & | & 4 \\ 2 & 4 & 7 & | & 9 \\ 3 & 6 & 10 & | & 13 \end{bmatrix}$$

$R_2 \leftarrow R_2 - 2R_1$, $R_3 \leftarrow R_3 - 3R_1$:

$$\begin{bmatrix} 1 & 2 & 3 & | & 4 \\ 0 & 0 & 1 & | & 1 \\ 0 & 0 & 1 & | & 1 \end{bmatrix}$$

$R_3 \leftarrow R_3 - R_2$:

$$\begin{bmatrix} 1 & 2 & 3 & | & 4 \\ 0 & 0 & 1 & | & 1 \\ 0 & 0 & 0 & | & 0 \end{bmatrix}$$

$\text{rank}(A) = \text{rank}([A|b]) = 2 < n = 3$ → **Infinitely many solutions**

Free variable: $y = t$ (parameter)

From Row 2: $z = 1$

From Row 1: $x + 2t + 3(1) = 4 \implies x = 1 - 2t$

**Solution:** $(x, y, z) = (1 - 2t, t, 1)$ for any $t \in \mathbb{R}$

---

### 4.8 Micro-Topic Coverage Checklist for Systems of Equations ✅

- 🔹 Column picture and row picture of $Ax=b$
- 🔹 Homogeneous vs non-homogeneous systems
- 🔹 Consistent, inconsistent, underdetermined, overdetermined systems
- 🔹 Particular solution plus null-space solution structure
- 🔹 Free variables and parametric vector form
- 🔹 Geometric interpretation in 2D and 3D
- 🔹 Solution set of homogeneous system is a subspace
- 🔹 Solution set of non-homogeneous system is an affine translate
- 🔹 Full column rank vs full row rank meaning
- 🔹 GATE trap: “more equations than unknowns” does not automatically mean no solution

### 4.9 Affine Geometry of Solutions 🌌

If $Ax=b$ is consistent and $x_p$ is one solution, then every solution is
$$x=x_p+x_h,\quad x_h\in N(A)$$

So the solution set is not usually a subspace. It is an affine subspace, a shifted copy of $N(A)$.

### 4.10 Worked Example Bank: Systems 🎯

**Example 1 (Hard):** Solve
$$x+y+z=2,\quad 2x+2y+2z=4,\quad x-y+z=0$$

**Solution:** Second equation is redundant. From first and third:
$$x+z=1,\quad y=1$$
Let $z=t$. Then $x=1-t$, $y=1$. Infinite solutions. ✅

**Example 2 (Hard):** Determine consistency of
$$x+y=1,\quad 2x+2y=3$$

**Solution:** Second equation contradicts the first after scaling. Inconsistent. No solution. ✅

**Example 3 (Hard):** For homogeneous $Ax=0$ with $A$ of size $3\times 5$, must a non-trivial solution exist?

**Solution:** Yes. Since rank $\le 3<5$, nullity $\ge 2$. Non-trivial solutions exist. ✅

### 4.11 PYQ-Style Hard Traps: Systems 🔥

**Q1. Difficulty: Hard** 😵
If $A$ is $m\times n$ with rank $n$, what can you say about $Ax=0$?

**Answer:** Only trivial solution. Nullity $=0$. ✅

**Q2. Difficulty: Hard** 😵
If rank$(A)$ equals rank$([A|b])$ but is less than $n$, how many solutions does $Ax=b$ have?

**Answer:** Infinitely many. There are free variables. ✅

**Q3. Difficulty: Hard** 😵
Can an overdetermined system be consistent?

**Answer:** Yes. More equations than unknowns does not imply inconsistency. ✅

**Q4. Difficulty: Hard** 😵
Can an underdetermined system have a unique solution?

**Answer:** Not for a consistent linear system over $\mathbb{R}$ with fewer equations than unknowns and full row rank only. There will be free variables. ✅

**Q5. Difficulty: Hard** 😵
What is the dimension of the solution set of a consistent system with 6 unknowns and rank 4?

**Answer:** $6-4=2$. ✅

**Q6. Difficulty: Hard** 😵
Is the solution set of $Ax=b$ always a subspace?

**Answer:** No. Only if $b=0$. Otherwise it is an affine set. ✅

---

## Chapter 5: Gaussian Elimination, Echelon Form & Rank (⭐⭐ CRITICAL)

### 5.1 Row Echelon Form (REF)

A matrix is in **Row Echelon Form** if:
1. All zero rows are at the bottom
2. The leading entry (pivot) of each non-zero row is to the right of the pivot above it
3. All entries below a pivot are zero

**Example of REF:**
$$\begin{bmatrix} 2 & 1 & 3 \\ 0 & 4 & 1 \\ 0 & 0 & 5 \end{bmatrix}$$

### 5.2 Reduced Row Echelon Form (RREF)

Additional conditions beyond REF:
4. Each pivot is 1
5. Each pivot is the **only** non-zero entry in its column

**Example of RREF:**
$$\begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

> 🔑 **Key Fact:** The RREF of a matrix is **UNIQUE** (unlike REF which depends on operations chosen). This makes RREF the canonical form for determining rank, pivot columns, and solution spaces. ⭐

### 5.3 Gaussian Elimination Algorithm

1. Find the leftmost non-zero column
2. Select a pivot (swap rows if needed to get non-zero entry at top)
3. Eliminate all entries below the pivot using row operations
4. Repeat for the sub-matrix below and to the right
5. (For RREF) Back-substitute to eliminate entries above pivots too

**Allowed Row Operations:**
- $R_i \leftrightarrow R_j$ (swap two rows)
- $R_i \leftarrow kR_i$ where $k \neq 0$ (scale a row)
- $R_i \leftarrow R_i + kR_j$ (add multiple of one row to another)

### 5.3A Numerical Stability: Pivoting (Extension + DA relevance) 🛡️

- **Partial pivoting:** swap current row with a lower row having largest absolute pivot in current column.
- **Complete pivoting:** also swaps columns (stronger, costlier).

Why this matters: tiny pivots amplify rounding error. In practical solvers (NumPy, LAPACK), pivoting is standard.

### 5.3B Time Complexity Snapshot

- Gaussian elimination: $O(n^3)$ for $n \times n$
- Back substitution: $O(n^2)$
- Total solve after elimination: $O(n^3)$

GATE can ask asymptotic comparisons between elimination, inversion, and decomposition-based solves.

### 5.4 Pivot Columns and Free Columns

- **Pivot columns:** Columns containing pivots in REF → correspond to **basic/dependent variables**
- **Free columns:** Columns without pivots → correspond to **free variables**

### 5.5 Rank of a Matrix (⭐ EXTREMELY IMPORTANT)

> **$\text{rank}(A)$** = number of pivots in the row echelon form = number of non-zero rows in REF

**Equivalent definitions:**
- Maximum number of linearly independent rows
- Maximum number of linearly independent columns
- Dimension of column space = Dimension of row space
- Order of the largest non-zero minor

**Key Properties of Rank:**

| Property | Formula |
|---|---|
| $\text{rank}(A) = \text{rank}(A^T)$ | Row rank = Column rank |
| $\text{rank}(A) \leq \min(m, n)$ for $m \times n$ matrix | Upper bound |
| $\text{rank}(AB) \leq \min(\text{rank}(A), \text{rank}(B))$ | Rank of product |
| $\text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)$ | Subadditivity |
| $\text{rank}(A) = n$ iff $A\vec{x} = \vec{0}$ has only trivial solution | Full column rank |
| $\text{rank}(A) = m$ iff $A\vec{x} = \vec{b}$ has a solution for every $\vec{b}$ | Full row rank |
| $\text{rank}(kA) = \text{rank}(A)$ for $k \neq 0$ | Scalar invariance |
| $\text{rank}(A^T A) = \text{rank}(A A^T) = \text{rank}(A)$ | Key identity |

### 5.6 Advanced Rank Inequalities (⭐ GATE TESTED) 📊

| Inequality | Name |
|---|---|
| $\text{rank}(A) + \text{rank}(B) - n \leq \text{rank}(AB)$ | **Sylvester's Rank Inequality** 🔥 |
| $\text{rank}(AB) \leq \min(\text{rank}(A), \text{rank}(B))$ | Upper bound on product |
| $\text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)$ | Subadditivity |
| $\text{rank}(A) = \text{rank}(A^T) = \text{rank}(A^TA) = \text{rank}(AA^T)$ | Fundamental identity |
| If $AB = 0$, then $\text{rank}(B) \leq n - \text{rank}(A)$ | Consequence of Sylvester |
| $\text{rank}(A) = n \Rightarrow \text{rank}(AB) = \text{rank}(B)$ | Left invertible preserves rank |
| $\text{rank}(B) = n \Rightarrow \text{rank}(AB) = \text{rank}(A)$ | Right invertible preserves rank |

> 🎯 **Sylvester's Inequality — GATE Application:**
> If $A$ is $m \times n$ with rank $r_1$ and $B$ is $n \times p$ with rank $r_2$, then:
> $$r_1 + r_2 - n \leq \text{rank}(AB) \leq \min(r_1, r_2)$$
>
> **Example:** $A$ is $4 \times 3$, rank 3. $B$ is $3 \times 5$, rank 2. Then $3 + 2 - 3 \leq \text{rank}(AB) \leq \min(3,2)$ → $2 \leq \text{rank}(AB) \leq 2$ → $\text{rank}(AB) = 2$ exactly! ✅

### 5.6 Rank-Nullity Theorem (⭐ VERY IMPORTANT)

> For an $m \times n$ matrix $A$:
> $$\text{rank}(A) + \text{nullity}(A) = n$$

where $\text{nullity}(A)$ = dimension of null space = number of free variables.

**Intuition:** The $n$ columns either contribute to rank (pivots) or to the null space (free variables). They partition exactly.

---

**Example 5 (GATE Level):** Find the rank of:
$$A = \begin{bmatrix} 1 & 2 & 3 & 4 \\ 2 & 4 & 7 & 9 \\ 3 & 6 & 10 & 13 \end{bmatrix}$$

**Solution:** Apply row operations:

$R_2 \leftarrow R_2 - 2R_1$, $R_3 \leftarrow R_3 - 3R_1$:
$$\begin{bmatrix} 1 & 2 & 3 & 4 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 1 & 1 \end{bmatrix}$$

$R_3 \leftarrow R_3 - R_2$:
$$\begin{bmatrix} 1 & 2 & 3 & 4 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 0 & 0 \end{bmatrix}$$

**rank(A) = 2** (2 pivots)

Nullity = $n - \text{rank} = 4 - 2 = 2$ (free variables: $x_2, x_4$)

---

### 5.7 Micro-Topic Coverage Checklist for Elimination and Rank ✅

- 🔹 REF vs RREF and uniqueness of RREF
- 🔹 Pivot positions, pivot columns, basic and free variables
- 🔹 Elementary row operations and elementary matrices
- 🔹 Rank as pivot count, row-space dimension, column-space dimension
- 🔹 Rank invariance under elementary row operations
- 🔹 Rank inequalities and product bounds
- 🔹 Rank-nullity theorem and null-space dimension
- 🔹 Numerical pivoting intuition
- 🔹 Why column space basis must come from original matrix
- 🔹 GATE trap: non-zero rows must be counted after reduction, not before

### 5.8 Rank Factorization (Useful Extension) 🏗️

If rank$(A)=r$, then $A$ can be written as
$$A=BC$$
where $B$ is $m\times r$ with independent columns and $C$ is $r\times n$ with independent rows.

This explains why rank measures the true information content of a matrix.

### 5.9 Worked Example Bank: Elimination and Rank 🎯

**Example 1 (Hard):** Find rank of
$$A=\begin{bmatrix}1&2&3\\2&4&6\\1&1&1\end{bmatrix}$$

**Solution:** Row 2 is $2$ times Row 1. Replace Row 3 by Row 3 minus Row 1:
$$\begin{bmatrix}1&2&3\\0&0&0\\0&-1&-2\end{bmatrix}$$
Two non-zero rows remain. Rank $=2$. ✅

**Example 2 (Hard):** If $A$ is $4\times 6$ with rank $3$, find nullity.

**Solution:** Nullity $=6-3=3$. ✅

**Example 3 (Hard):** Why must rank$(A)=\text{rank}(A^TA)$?

**Solution:** Both have the same null space, so by rank-nullity they have the same rank. ✅

### 5.10 PYQ-Style Hard Traps: Elimination and Rank 🔥

**Q1. Difficulty: Hard** 😵
Does a row swap change rank?

**Answer:** No. Elementary row operations preserve rank. ✅

**Q2. Difficulty: Hard** 😵
Can row operations change the column space itself?

**Answer:** Yes. They preserve rank but not the actual column space as a subset of $\mathbb{R}^m$. ✅

**Q3. Difficulty: Hard** 😵
What is the maximum possible rank of a $5\times 3$ matrix?

**Answer:** $3$. Rank cannot exceed the smaller dimension. ✅

**Q4. Difficulty: Hard** 😵
If rank$(AB)=2$, can rank$(A)$ be $1$?

**Answer:** No. Rank$(AB)\le \min(\text{rank}(A),\text{rank}(B))$. So rank$(A)\ge 2$. ✅

**Q5. Difficulty: Hard** 😵
If $A$ is $n\times n$ and rank$(A)=n$, what is the RREF of $A$?

**Answer:** $I_n$. ✅

**Q6. Difficulty: Hard** 😵
If rank$(A)=r$, how many pivot columns does RREF have?

**Answer:** Exactly $r$. ✅

---

## Chapter 6: Solutions Based on Rank — The Complete Framework

### 6.1 Summary Table for $A\vec{x} = \vec{b}$ where $A$ is $m \times n$

| $\text{rank}(A)$ vs $\text{rank}([A|b])$ | $\text{rank}(A)$ vs $n$ | Solutions |
|---|---|---|
| $\text{rank}(A) = \text{rank}([A|b])$ | $= n$ | **Unique** |
| $\text{rank}(A) = \text{rank}([A|b])$ | $< n$ | **Infinite** ($n - r$ free vars) |
| $\text{rank}(A) < \text{rank}([A|b])$ | — | **No solution** |

### 6.2 Special Case: Square Matrix ($m = n$)

For $n \times n$ matrix $A$:

| Condition | Equivalent Statements |
|---|---|
| $A$ is invertible | $\det(A) \neq 0$, $\text{rank}(A) = n$, $A\vec{x}=\vec{b}$ has unique solution for every $\vec{b}$, all eigenvalues $\neq 0$, $A\vec{x} = \vec{0}$ has only trivial solution, columns are LI, $A^{-1}$ exists |
| $A$ is singular | $\det(A) = 0$, $\text{rank}(A) < n$, at least one eigenvalue = 0, columns are LD |

### 6.3 Finding LI Columns Using Gaussian Elimination

After row reduction, the **pivot columns** of the **original matrix** form a linearly independent set that is a basis for the column space.

> **Warning:** Use the pivot column positions from the echelon form, but take the actual columns from the **original** matrix.

---

### 6.4 Micro-Topic Coverage Checklist for Rank-Based Solution Decisions ✅

- 🔹 Rank comparison between coefficient and augmented matrix
- 🔹 Unique, infinite, and no-solution classification
- 🔹 Square invertible case vs rectangular case
- 🔹 Full column rank and injectivity
- 🔹 Full row rank and surjectivity
- 🔹 Number of free variables from rank
- 🔹 Homogeneous and non-homogeneous relation
- 🔹 Invertible Matrix Theorem link
- 🔹 Column-space interpretation of consistency
- 🔹 GATE trap: rank conditions must be matched with number of unknowns, not equations

### 6.5 Worked Example Bank: Decision Framework 🎯

**Example 1 (Hard):** A system has 4 unknowns and rank$(A)=\text{rank}([A|b])=4$. Number of solutions?

**Solution:** Unique solution. Rank equals number of unknowns. ✅

**Example 2 (Hard):** A system has 5 unknowns and rank$(A)=\text{rank}([A|b])=3$. Number of free variables?

**Solution:** $5-3=2$ free variables, so infinitely many solutions. ✅

**Example 3 (Hard):** If rank$(A)=2$ and rank$([A|b])=3$, what happens?

**Solution:** Inconsistent system. No solution. ✅

### 6.6 PYQ-Style Hard Traps: Rank-Based Solutions 🔥

**Q1. Difficulty: Hard** 😵
If $A$ is $3\times 3$ singular, can $Ax=b$ still have a solution?

**Answer:** Yes. It may have none or infinitely many, depending on $b$. ✅

**Q2. Difficulty: Hard** 😵
If a square system has a unique solution for one particular $b$, must $A$ be invertible?

**Answer:** Yes. Unique solution for some $b$ in a square system implies nullity $0$, hence invertible. ✅

**Q3. Difficulty: Hard** 😵
If $A$ has full row rank, is $Ax=b$ solvable for every $b$?

**Answer:** Yes. Column space equals the whole codomain. ✅

**Q4. Difficulty: Hard** 😵
If $A$ has full column rank, is $Ax=b$ solvable for every $b$?

**Answer:** Not necessarily. Full column rank gives uniqueness when solvable, not universal solvability. ✅

**Q5. Difficulty: Hard** 😵
For a homogeneous system, when do infinitely many solutions occur?

**Answer:** When rank$(A)<n$, equivalently nullity $>0$. ✅

**Q6. Difficulty: Hard** 😵
If rank$(A)=n$ in an $m\times n$ matrix, what property does the column set satisfy?

**Answer:** Columns are linearly independent. ✅

---

## Chapter 7: Determinants (⭐⭐ FREQUENTLY ASKED) 🎲

> 💡 **Beginner Analogy — The Area/Volume Detector 📏:**
> Imagine you have two arrows (vectors) starting from the same point. The determinant tells you the **area of the parallelogram** they form!
> - If the area = 0, the arrows are pointing in the SAME direction (dependent!) — you can't form a proper shape 🚧
> - If the area ≠ 0, they point in different directions (independent!) — you get a real parallelogram ✅
>
> In 3D, it's the **volume** of the box formed by 3 vectors! A zero determinant = flat pancake (no volume) = linearly dependent vectors.
>
> 🏭 **Production Use:** Unity and Unreal Engine use determinants to check if 3D objects are flipped/mirrored (negative determinant = mirror image!) 🎮

### 7.1 Definition and Computation

**For $2 \times 2$:**
$$\det\begin{bmatrix} a & b \\ c & d \end{bmatrix} = ad - bc$$

**For $3 \times 3$ (Sarrus' Rule or Cofactor Expansion):**

$$\det(A) = a_{11}(a_{22}a_{33} - a_{23}a_{32}) - a_{12}(a_{21}a_{33} - a_{23}a_{31}) + a_{13}(a_{21}a_{32} - a_{22}a_{31})$$

**General: Cofactor Expansion along row $i$:**
$$\det(A) = \sum_{j=1}^n a_{ij} C_{ij}$$

where $C_{ij} = (-1)^{i+j} M_{ij}$ is the cofactor and $M_{ij}$ is the minor (determinant of submatrix obtained by deleting row $i$, column $j$).

### 7.2 Properties of Determinants (⭐ KNOW ALL OF THESE)

| # | Property | Formula/Detail |
|---|---|---|
| 1 | $\det(I) = 1$ | Identity matrix |
| 2 | Row swap | Changes sign of determinant |
| 3 | Scaling a row by $k$ | Multiplies $\det$ by $k$ |
| 4 | $\det(kA) = k^n \det(A)$ for $n \times n$ | Scale factor is $k^n$ |
| 5 | $\det(A^T) = \det(A)$ | Transpose preserves determinant |
| 6 | $\det(AB) = \det(A)\det(B)$ | Product rule |
| 7 | $\det(A^{-1}) = \frac{1}{\det(A)}$ | Inverse |
| 8 | $\det(A^m) = [\det(A)]^m$ | Power rule |
| 9 | Row of zeros → $\det = 0$ | |
| 10 | Two identical rows → $\det = 0$ | |
| 11 | Triangular matrix | $\det =$ product of diagonal entries |
| 12 | $\det(\text{adj}(A)) = [\det(A)]^{n-1}$ | Adjoint formula |
| 13 | $R_i \leftarrow R_i + kR_j$ | Does **not** change determinant |
| 14 | $\det(A) \neq 0 \iff A$ is invertible | |
| 15 | Block triangular: $\det\begin{bmatrix} A & B \\ 0 & D \end{bmatrix} = \det(A)\det(D)$ | |

### 7.2A Cauchy-Binet Formula (Advanced extension) 🧠

For $A$ ($m \times n$), $B$ ($n \times m$), with $m \le n$:

$$\det(AB) = \sum_{S \subseteq \{1,\dots,n\}, |S|=m} \det(A_{:,S})\det(B_{S,:})$$

This generalizes multiplicativity and appears in advanced rank/determinant proofs.

### 7.3 Shortcuts for Determinant Computation

1. **Triangularize first:** Row reduce to upper triangular form, then multiply diagonal entries. Track sign changes from row swaps.
2. **For $3 \times 3$:** Use Sarrus' rule (faster than cofactor expansion).
3. **Block matrices:** Use $\det = \det(A)\det(D)$ for block triangular.
4. **If a row/column has many zeros:** Expand along that row/column.

---

**Example (Medium):** Find $\det(A)$ where:
$$A = \begin{bmatrix} 2 & 1 & 3 \\ 0 & -1 & 2 \\ 4 & 2 & 1 \end{bmatrix}$$

**Solution:** $R_3 \leftarrow R_3 - 2R_1$:
$$\begin{bmatrix} 2 & 1 & 3 \\ 0 & -1 & 2 \\ 0 & 0 & -5 \end{bmatrix}$$

$\det = 2 \times (-1) \times (-5) = 10$

---

### 7.4 Area/Volume Interpretation

$|\det(A)|$ = volume of the parallelepiped spanned by the column vectors of $A$.

- $2 \times 2$: $|\det|$ = area of parallelogram
- $3 \times 3$: $|\det|$ = volume of parallelepiped
- $\det = 0$ means the vectors are coplanar/collinear (degenerate)

### 7.5 Special Determinants (⭐ GATE SHORTCUTS) ⚡

**Vandermonde Determinant:**
$$\det\begin{bmatrix} 1 & a_1 & a_1^2 \\ 1 & a_2 & a_2^2 \\ 1 & a_3 & a_3^2 \end{bmatrix} = (a_2 - a_1)(a_3 - a_1)(a_3 - a_2) = \prod_{1 \leq i < j \leq 3} (a_j - a_i)$$

**General $n \times n$ Vandermonde:**
$$\det(V) = \prod_{1 \leq i < j \leq n}(a_j - a_i)$$
$\det = 0 \iff$ any two $a_i$ are equal. Non-zero iff all $a_i$ are distinct.

> 🎯 **GATE Use:** If you see a matrix with columns $1, x, x^2, \ldots$ — it's Vandermonde! The determinant is just the product of pairwise differences.

**Circulant Matrix Determinant:** For $C = \text{circ}(c_0, c_1, \ldots, c_{n-1})$:
$$\det(C) = \prod_{j=0}^{n-1} (c_0 + c_1\omega^j + c_2\omega^{2j} + \cdots + c_{n-1}\omega^{(n-1)j})$$
where $\omega = e^{2\pi i/n}$ is the $n$-th root of unity.

**Block Diagonal Determinant:**
$$\det\begin{bmatrix} A_1 & 0 & \cdots & 0 \\ 0 & A_2 & \cdots & 0 \\ \vdots & & \ddots & \vdots \\ 0 & 0 & \cdots & A_k \end{bmatrix} = \det(A_1) \cdot \det(A_2) \cdots \det(A_k)$$

**Determinant of $I + \vec{u}\vec{v}^T$:** (rank-1 update)
$$\det(I + \vec{u}\vec{v}^T) = 1 + \vec{v}^T\vec{u}$$

> 🔥 **Matrix Determinant Lemma (General):** $\det(A + \vec{u}\vec{v}^T) = (1 + \vec{v}^T A^{-1}\vec{u})\det(A)$

### 7.6 Cofactor Matrix & Adjoint — Detailed (⭐ GATE COMPUTATION) 📝

**Minor $M_{ij}$:** Determinant of the $(n-1) \times (n-1)$ submatrix obtained by deleting row $i$ and column $j$.

**Cofactor $C_{ij}$:** $C_{ij} = (-1)^{i+j} M_{ij}$

**Cofactor Matrix:** $\text{cof}(A) = [C_{ij}]$

**Adjoint (Adjugate):** $\text{adj}(A) = [\text{cof}(A)]^T$ (transpose of cofactor matrix!)

**Key Properties of Adjoint:**

| Property | Formula |
|---|---|
| $A \cdot \text{adj}(A) = \text{adj}(A) \cdot A$ | $= \det(A) \cdot I$ |
| $\det(\text{adj}(A))$ | $= [\det(A)]^{n-1}$ |
| $\text{adj}(\text{adj}(A))$ | $= [\det(A)]^{n-2} \cdot A$ (for invertible $A$) |
| $\text{adj}(AB)$ | $= \text{adj}(B) \cdot \text{adj}(A)$ ⚠️ Reverses! |
| $\text{adj}(A^T)$ | $= [\text{adj}(A)]^T$ |
| $\text{adj}(kA)$ | $= k^{n-1} \text{adj}(A)$ |
| $\text{rank}(\text{adj}(A))$ | $n$ if rank$(A)=n$; $1$ if rank$(A)=n-1$; $0$ if rank$(A) < n-1$ |

> 🎯 **GATE Favorite:** $\text{rank}(\text{adj}(A))$ — memorize the three cases above!

**Worked Example — Finding adj(A) for 3×3:**

For $A = \begin{bmatrix} 1 & 2 & 3 \\ 0 & 1 & 4 \\ 5 & 6 & 0 \end{bmatrix}$:

Cofactors (applying $(-1)^{i+j}$ pattern):
$$\text{cof}(A) = \begin{bmatrix} -24 & 20 & -5 \\ 18 & -15 & 4 \\ 5 & -4 & 1 \end{bmatrix}$$

$$\text{adj}(A) = \text{cof}(A)^T = \begin{bmatrix} -24 & 18 & 5 \\ 20 & -15 & -4 \\ -5 & 4 & 1 \end{bmatrix}$$

### 7.7 Characteristic Polynomial — Coefficients (⭐ GATE THEORY) 🔬

For $n \times n$ matrix $A$, the characteristic polynomial is:
$$p(\lambda) = \det(A - \lambda I) = (-1)^n[\lambda^n - s_1\lambda^{n-1} + s_2\lambda^{n-2} - \cdots + (-1)^n s_n]$$

where:
- $s_1 = \text{tr}(A) = \sum \lambda_i$ (sum of eigenvalues)
- $s_2 = \sum_{i<j} \lambda_i\lambda_j$ = sum of all $2 \times 2$ principal minors of $A$
- $s_k$ = sum of all $k \times k$ principal minors of $A$
- $s_n = \det(A) = \prod \lambda_i$

**For $2 \times 2$:** $p(\lambda) = \lambda^2 - \text{tr}(A)\lambda + \det(A)$

**For $3 \times 3$:** $p(\lambda) = -\lambda^3 + s_1\lambda^2 - s_2\lambda + s_3$ where:
- $s_1 = a_{11} + a_{22} + a_{33}$
- $s_2 = M_{11} + M_{22} + M_{33}$ (sum of $2 \times 2$ principal minors)
- $s_3 = \det(A)$

> 🎯 **Quick trick for GATE:** For a $2 \times 2$ matrix, you can find eigenvalues instantly: $\lambda = \frac{\text{tr}(A) \pm \sqrt{\text{tr}(A)^2 - 4\det(A)}}{2}$

---

### 7.8 Micro-Topic Coverage Checklist for Determinants ✅

- 🔹 Determinant as signed area/volume scaling
- 🔹 Cofactor expansion, minors, cofactors, adjoint link
- 🔹 Effect of row swap, row scaling, row addition
- 🔹 Determinant of triangular, block triangular, diagonal matrices
- 🔹 Multiplicativity: $\det(AB)=\det(A)\det(B)$
- 🔹 Determinant and invertibility
- 🔹 Vandermonde determinant shortcut
- 🔹 Rank-1 update determinant lemma awareness
- 🔹 Characteristic polynomial coefficients via trace/minors/determinant
- 🔹 GATE trap: determinant is not linear in the whole matrix, only multilinear in each row/column separately

### 7.9 Determinant as Volume-Scaling Map 📦

If $T(x)=Ax$, then $|\det(A)|$ is the factor by which $n$-dimensional volume scales.

- Positive determinant: orientation preserved
- Negative determinant: orientation flipped
- Zero determinant: space collapses into lower dimension

This geometric lens often makes abstract statements immediate.

### 7.10 Worked Example Bank: Determinants 🎯

**Example 1 (Hard):** Evaluate
$$\det\begin{bmatrix}1&1&1\\1&a&b\\1&a^2&b^2\end{bmatrix}$$

**Solution:** This is Vandermonde after column reordering logic. Value is
$$ (a-1)(b-1)(b-a) $$
✅

**Example 2 (Hard):** If $A$ is upper triangular with diagonal $2,-1,3,4$, find $\det(A)$.

**Solution:** Product of diagonal entries $=2\cdot(-1)\cdot3\cdot4=-24$. ✅

**Example 3 (Hard):** If $\det(A)=5$ and matrix $B$ is formed by swapping two rows of $A$ and multiplying one row by $3$, what is $\det(B)$?

**Solution:** Row swap changes sign, row scaling multiplies by $3$. Hence $\det(B)=-15$. ✅

### 7.11 PYQ-Style Hard Traps: Determinants 🔥

**Q1. Difficulty: Hard** 😵
Does $\det(A+B)=\det(A)+\det(B)$ hold in general?

**Answer:** No. Absolutely false in general. ✅

**Q2. Difficulty: Hard** 😵
If two rows are equal, what is the determinant?

**Answer:** Zero. ✅

**Q3. Difficulty: Hard** 😵
If $A$ is singular, what is $\det(A)$?

**Answer:** Zero. Singular iff determinant zero for square matrices. ✅

**Q4. Difficulty: Hard** 😵
If $\det(A)=0$, are the columns LI?

**Answer:** No. They are dependent. ✅

**Q5. Difficulty: Hard** 😵
If $\det(A)=2$ for a $3\times 3$ matrix, what is $\det(2A)$?

**Answer:** $2^3\det(A)=16$. ✅

**Q6. Difficulty: Hard** 😵
If $\det(A)=3$ and $\det(B)=-2$, what is $\det(AB)$?

**Answer:** $-6$. ✅

---

## Chapter 8: Inverse of a Matrix

### 8.1 Definition

For an $n \times n$ matrix $A$, if there exists a matrix $A^{-1}$ such that:
$$AA^{-1} = A^{-1}A = I$$

then $A$ is **invertible** (non-singular).

**Existence condition:** $\det(A) \neq 0$

### 8.2 Methods to Find Inverse

**Method 1: Adjoint Method**
$$A^{-1} = \frac{1}{\det(A)} \text{adj}(A)$$

where $\text{adj}(A) = [C_{ij}]^T$ (transpose of cofactor matrix).

**For $2 \times 2$:**
$$\begin{bmatrix} a & b \\ c & d \end{bmatrix}^{-1} = \frac{1}{ad-bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$

**Method 2: Gauss-Jordan Method**
Augment $[A|I]$ and row reduce. When left side becomes $I$, right side is $A^{-1}$:
$$[A|I] \xrightarrow{\text{row ops}} [I|A^{-1}]$$

### 8.3 Properties of Inverse

| Property | Formula |
|---|---|
| $(A^{-1})^{-1} = A$ | |
| $(AB)^{-1} = B^{-1}A^{-1}$ | Reverse order! |
| $(A^T)^{-1} = (A^{-1})^T$ | |
| $(kA)^{-1} = \frac{1}{k}A^{-1}$ | |
| $\det(A^{-1}) = \frac{1}{\det(A)}$ | |
| If $A$ is symmetric, $A^{-1}$ is symmetric | |
| If $A$ is orthogonal, $A^{-1} = A^T$ | |

### 8.4 The Invertible Matrix Theorem (⭐⭐⭐ MASTER LIST) 🏆

> 🔥 **THE single most important theorem in Linear Algebra!** All these statements are EQUIVALENT for an $n \times n$ matrix $A$:

For a square $n \times n$ matrix $A$, **ALL of the following are equivalent:**

| # | Statement |
|---|---|
| 1 | $A$ is **invertible** (non-singular) |
| 2 | $A^{-1}$ exists |
| 3 | $\det(A) \neq 0$ |
| 4 | $\text{rank}(A) = n$ (full rank) |
| 5 | The RREF of $A$ is $I_n$ |
| 6 | $A\vec{x} = \vec{0}$ has ONLY the trivial solution |
| 7 | $A\vec{x} = \vec{b}$ has a **unique** solution for every $\vec{b}$ |
| 8 | Columns of $A$ are **linearly independent** |
| 9 | Rows of $A$ are **linearly independent** |
| 10 | Columns of $A$ **span** $\mathbb{R}^n$ |
| 11 | Columns of $A$ form a **basis** for $\mathbb{R}^n$ |
| 12 | $\text{nullity}(A) = 0$ (null space = $\{\vec{0}\}$) |
| 13 | $C(A) = \mathbb{R}^n$ (column space = entire space) |
| 14 | **All** eigenvalues are non-zero |
| 15 | $0$ is NOT an eigenvalue of $A$ |
| 16 | $A^T$ is invertible |
| 17 | $A^TA$ is invertible |
| 18 | $A$ is a product of elementary matrices |
| 19 | The linear transformation $T(\vec{x}) = A\vec{x}$ is bijective |

> ⚠️ **GATE Trap:** If ANY ONE of these fails, ALL of them fail simultaneously! One crack breaks everything.

---

### 8.5 Micro-Topic Coverage Checklist for Inverses ✅

- 🔹 Definition via two-sided inverse
- 🔹 Inverse existence iff determinant non-zero for square matrices
- 🔹 Adjoint formula and Gauss-Jordan method
- 🔹 Inverse of product and transpose
- 🔹 Inverse of orthogonal and diagonal matrices
- 🔹 Invertible Matrix Theorem links
- 🔹 Elementary matrix factorization view
- 🔹 Left inverse vs right inverse for rectangular matrices
- 🔹 When inverse computations are numerically bad in practice
- 🔹 GATE trap: non-square matrices do not have ordinary inverses

### 8.6 Left Inverse and Right Inverse Awareness 🔁

For a rectangular matrix $A$:

- If $A$ has full column rank, a left inverse may exist.
- If $A$ has full row rank, a right inverse may exist.
- A genuine inverse requires $A$ to be square and nonsingular.

### 8.7 Worked Example Bank: Inverses 🎯

**Example 1 (Medium):** Find inverse of
$$A=\begin{bmatrix}2&1\\1&1\end{bmatrix}$$

**Solution:**
$$A^{-1}=\frac{1}{2\cdot1-1\cdot1}\begin{bmatrix}1&-1\\-1&2\end{bmatrix}=\begin{bmatrix}1&-1\\-1&2\end{bmatrix}$$
✅

**Example 2 (Hard):** If $A$ is orthogonal, prove inverse exists.

**Solution:** $A^TA=I$, so $A^{-1}=A^T$. ✅

**Example 3 (Hard):** If $A$ and $B$ are invertible, what is $(AB)^{-1}$?

**Solution:** $B^{-1}A^{-1}$, not $A^{-1}B^{-1}$. Reverse order. ✅

### 8.8 PYQ-Style Hard Traps: Inverses 🔥

**Q1. Difficulty: Hard** 😵
Can a singular matrix have an inverse?

**Answer:** No. Singular means determinant zero. ✅

**Q2. Difficulty: Hard** 😵
If $A^2=I$, what can you say about $A^{-1}$?

**Answer:** $A^{-1}=A$. Such matrices are involutory. ✅

**Q3. Difficulty: Hard** 😵
If $A$ is diagonal with non-zero diagonal entries, how do you invert it?

**Answer:** Invert each diagonal entry independently. ✅

**Q4. Difficulty: Hard** 😵
If $A$ is symmetric invertible, is $A^{-1}$ symmetric?

**Answer:** Yes. $(A^{-1})^T=(A^T)^{-1}=A^{-1}$. ✅

**Q5. Difficulty: Hard** 😵
If $A$ is $m\times n$ with $m\ne n$, can $A^{-1}$ exist in the ordinary sense?

**Answer:** No. Ordinary inverse is defined only for square matrices. ✅

**Q6. Difficulty: Hard** 😵
If $AB=I$, does it always follow that $BA=I$?

**Answer:** For square matrices, yes. For rectangular matrices, not necessarily in the same size context. ✅

---

## Chapter 9: Cramer's Rule

For $A\vec{x} = \vec{b}$ where $A$ is $n \times n$ and $\det(A) \neq 0$:

$$x_i = \frac{\det(A_i)}{\det(A)}$$

where $A_i$ is $A$ with column $i$ replaced by $\vec{b}$.

**Example:** Solve:
$$2x + y = 5$$
$$x + 3y = 7$$

$\det(A) = 6 - 1 = 5$

$x = \frac{\det\begin{bmatrix} 5 & 1 \\ 7 & 3 \end{bmatrix}}{5} = \frac{15 - 7}{5} = \frac{8}{5}$

$y = \frac{\det\begin{bmatrix} 2 & 5 \\ 1 & 7 \end{bmatrix}}{5} = \frac{14 - 5}{5} = \frac{9}{5}$

> **When to use:** Cramer's rule is mainly useful for $2 \times 2$ and $3 \times 3$ systems or when they ask specifically. For larger systems, Gaussian elimination is more efficient.

---

### 9.1A Micro-Topic Coverage Checklist for Cramer's Rule ✅

- 🔹 Formula for each variable using determinant replacement
- 🔹 Only valid when coefficient matrix is square and nonsingular
- 🔹 Fast use for $2\times 2$ and some $3\times 3$ systems
- 🔹 Relation to adjoint/inverse formula
- 🔹 Geometric meaning through determinant ratios
- 🔹 Why it is inefficient for large systems
- 🔹 GATE trap: determinant zero means Cramer's rule breaks, not “answer is zero”

### 9.2 Worked Example Bank: Cramer's Rule 🎯

**Example 1 (Hard):** Solve
$$x+y+z=6,\quad x-y+z=2,\quad x+y-z=2$$
using determinant logic.

**Solution:** Solving the symmetric structure gives $x=2$, $y=2$, $z=2$. ✅

**Example 2 (Hard):** If $\det(A)=7$ and replacing first column by $b$ gives determinant $14$, what is $x_1$?

**Solution:** By Cramer, $x_1=14/7=2$. ✅

### 9.3 PYQ-Style Hard Traps: Cramer's Rule 🔥

**Q1. Difficulty: Hard** 😵
Can Cramer's rule be used for a $3\times 4$ system?

**Answer:** No. Coefficient matrix is not square. ✅

**Q2. Difficulty: Hard** 😵
If $\det(A)=0$, can Cramer's rule be applied?

**Answer:** No. Division by zero issue; matrix is singular. ✅

**Q3. Difficulty: Hard** 😵
Is Cramer's rule computationally efficient for large $n$?

**Answer:** No. It is mainly theoretical or for tiny systems. ✅

**Q4. Difficulty: Hard** 😵
If only one numerator determinant is non-zero and denominator is non-zero, what happens?

**Answer:** Only the corresponding variable is non-zero in that coordinate description. ✅

**Q5. Difficulty: Hard** 😵
Can Cramer's rule prove uniqueness?

**Answer:** Yes. It requires $\det(A)\ne 0$, which already guarantees uniqueness. ✅

**Q6. Difficulty: Hard** 😵
What theorem connects Cramer and inverse formula conceptually?

**Answer:** $A^{-1}=\frac{1}{\det(A)}\operatorname{adj}(A)$. ✅

---

## Chapter 10: Eigenvalues and Eigenvectors (⭐⭐⭐ HIGHEST WEIGHT IN GATE) 🌟

> 💡 **Beginner Analogy — The Stretchy T-Shirt 👕:**
> Imagine stretching a t-shirt:
> - Most points on the fabric move in complicated diagonal ways
> - But along certain special directions (like horizontally or vertically), points just **stretch** or **shrink** along that SAME direction
> - Those special directions = **Eigenvectors** 🧭
> - How much they stretch = **Eigenvalues** (2x means stretched to double!) 📏
>
> If eigenvalue = 1 → no change. If eigenvalue = 0 → collapsed to nothing! If negative → direction flipped!

> 🌍 **Where Eigenvalues Power the Real World:**
> | Application | How Eigenvalues Help |
> |---|---|
> | **Google PageRank** 🔍 | The dominant eigenvector of the web link matrix = page rankings! Larry Page's $1 trillion idea! |
> | **Facial Recognition** 👤 | "Eigenfaces" — Facebook/Apple Face ID uses eigenvectors to represent face features |
> | **Vibration Analysis** 🏛️ | Engineers use eigenvalues to find resonant frequencies of bridges/buildings (prevent collapse!) |
> | **Quantum Mechanics** ⚗️ | Observable quantities ARE eigenvalues! The entire theory is built on linear algebra |
> | **PCA (Principal Component Analysis)** 📊 | The #1 dimensionality reduction technique — finds directions of maximum variance via eigenvectors |
> | **Stability of Systems** 🚀 | Eigenvalues determine if a rocket's autopilot system is stable or will crash |
> | **Netflix Recommendations** 🎬 | Low-rank matrix approximation using eigendecomposition |
>
> 🏗️ **Deep Research Insight:** Google's original PageRank paper (1998) by Larry Page and Sergey Brin was essentially computing the largest eigenvector of a matrix with **billions** of rows and columns. This eigenvalue problem was worth 💰 **$1.9 trillion** (Google's market cap)!

### 10.1 Definition

For a square matrix $A$ of size $n \times n$, a scalar $\lambda$ and a non-zero vector $\vec{v}$ satisfying:

$$A\vec{v} = \lambda\vec{v}$$

are called an **eigenvalue** and **eigenvector** respectively.

**Intuition:** An eigenvector is a direction that doesn't change under the transformation $A$ — it only gets scaled by $\lambda$.

**Real-world analogy:** Think of stretching a rubber sheet. Most points move in complicated ways, but along certain special directions (eigenvectors), points just move along the same line, stretched by a factor $\lambda$.

### 10.2 Characteristic Equation

$$\det(A - \lambda I) = 0$$

This is a polynomial of degree $n$ in $\lambda$ called the **characteristic polynomial**.

For $2 \times 2$: $\lambda^2 - \text{tr}(A)\lambda + \det(A) = 0$

where $\text{tr}(A) = a_{11} + a_{22}$ (sum of diagonal entries).

### 10.3 Finding Eigenvalues and Eigenvectors — Step by Step

**Step 1:** Compute $\det(A - \lambda I) = 0$ to find eigenvalues $\lambda_1, \lambda_2, \ldots$

**Step 2:** For each $\lambda_i$, solve $(A - \lambda_i I)\vec{v} = \vec{0}$ to find eigenvectors.

---

**Example (Complete walkthrough):** Find eigenvalues and eigenvectors of:
$$A = \begin{bmatrix} 4 & 1 \\ 2 & 3 \end{bmatrix}$$

**Step 1:** $\det(A - \lambda I) = 0$:
$$\det\begin{bmatrix} 4-\lambda & 1 \\ 2 & 3-\lambda \end{bmatrix} = (4-\lambda)(3-\lambda) - 2 = \lambda^2 - 7\lambda + 10 = 0$$

$$(\lambda - 5)(\lambda - 2) = 0 \implies \lambda_1 = 5, \lambda_2 = 2$$

**Quick check:** $\lambda_1 + \lambda_2 = 7 = \text{tr}(A)$ ✓, $\lambda_1 \cdot \lambda_2 = 10 = \det(A)$ ✓

**Step 2 for $\lambda_1 = 5$:**
$$(A - 5I)\vec{v} = \begin{bmatrix} -1 & 1 \\ 2 & -2 \end{bmatrix}\vec{v} = \vec{0}$$

$-v_1 + v_2 = 0 \implies v_2 = v_1$

Eigenvector: $\vec{v}_1 = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$ (or any scalar multiple)

**Step 2 for $\lambda_2 = 2$:**
$$(A - 2I)\vec{v} = \begin{bmatrix} 2 & 1 \\ 2 & 1 \end{bmatrix}\vec{v} = \vec{0}$$

$2v_1 + v_2 = 0 \implies v_2 = -2v_1$

Eigenvector: $\vec{v}_2 = \begin{bmatrix} 1 \\ -2 \end{bmatrix}$

---

### 10.4 Key Properties of Eigenvalues (⭐⭐ MUST MEMORIZE)

| Property | Formula |
|---|---|
| Sum of eigenvalues $= \text{trace}(A)$ | $\lambda_1 + \lambda_2 + \cdots + \lambda_n = \text{tr}(A)$ |
| Product of eigenvalues $= \det(A)$ | $\lambda_1 \cdot \lambda_2 \cdots \lambda_n = \det(A)$ |
| Eigenvalues of $A^k$ | $\lambda^k$ |
| Eigenvalues of $A^{-1}$ | $1/\lambda$ |
| Eigenvalues of $A + cI$ | $\lambda + c$ |
| Eigenvalues of $A^T$ | Same as $A$ |
| Eigenvalues of $kA$ | $k\lambda$ |
| Eigenvalues of $\text{adj}(A)$ | $\det(A)/\lambda$ |
| Eigenvalues of $f(A)$ | $f(\lambda)$ for any polynomial $f$ |

### 10.5 Algebraic vs Geometric Multiplicity

- **Algebraic Multiplicity (AM):** The number of times $\lambda$ appears as a root of the characteristic equation
- **Geometric Multiplicity (GM):** The number of linearly independent eigenvectors for $\lambda$ = $\dim(\text{null}(A - \lambda I))$

> **Always:** $1 \leq \text{GM} \leq \text{AM}$

### 10.6 Eigenvectors with Repeating Eigenvalues

**Case 1: GM = AM** — We get enough LI eigenvectors (matrix is diagonalizable)

**Case 2: GM < AM** — We don't get enough LI eigenvectors (matrix is NOT diagonalizable)

**Example (3 cases for $3 \times 3$ with eigenvalue $\lambda = 2$ of AM = 2):**

| Matrix | $\dim(\text{null}(A-2I))$ | GM | LI eigenvectors for $\lambda=2$ |
|---|---|---|---|
| $\begin{bmatrix} 2&0&0\\0&2&0\\0&0&3 \end{bmatrix}$ | 2 | 2 | 2 (diagonalizable) |
| $\begin{bmatrix} 2&1&0\\0&2&0\\0&0&3 \end{bmatrix}$ | 1 | 1 | 1 (not diagonalizable) |

### 10.7 Properties of Symmetric Matrices (⭐ FREQUENTLY TESTED)

For real symmetric $A = A^T$:
1. **All eigenvalues are real**
2. **Eigenvectors corresponding to distinct eigenvalues are orthogonal**
3. **Always has $n$ linearly independent eigenvectors** (always diagonalizable)
4. **GM = AM for every eigenvalue**
5. Can be diagonalized as $A = Q\Lambda Q^T$ where $Q$ is orthogonal

### 10.8 Spectral Theorem / Spectral Decomposition (⭐⭐ CRITICAL) 🌈

> 💡 **The Spectral Theorem is the crown jewel of symmetric matrix theory!**

**Statement:** Every real symmetric matrix $A$ can be decomposed as:
$$A = Q\Lambda Q^T = \lambda_1 \vec{q}_1\vec{q}_1^T + \lambda_2 \vec{q}_2\vec{q}_2^T + \cdots + \lambda_n \vec{q}_n\vec{q}_n^T$$

where:
- $Q = [\vec{q}_1 | \vec{q}_2 | \cdots | \vec{q}_n]$ is an **orthogonal** matrix ($Q^TQ = QQ^T = I$)
- $\Lambda = \text{diag}(\lambda_1, \ldots, \lambda_n)$ is the diagonal matrix of eigenvalues
- Each $\lambda_i \vec{q}_i\vec{q}_i^T$ is a symmetric rank-1 matrix (a "spectral component")

> 🏭 **Production Use:**
> - **PCA** (Principal Component Analysis) is literally the spectral decomposition of the covariance matrix! The eigenvectors = principal components, eigenvalues = variance along each direction. 📊
> - **Google PageRank** uses the dominant eigenvector of the link matrix! 🔍

### 10.9 Gershgorin's Circle Theorem (⭐ GATE — Eigenvalue Localization) 🎯

> 💡 **Quickly locate ALL eigenvalues without computing them!**

**Theorem:** Every eigenvalue of $A$ lies within at least one **Gershgorin disc**:
$$D_i = \{z \in \mathbb{C} : |z - a_{ii}| \leq R_i\}$$

where $R_i = \sum_{j \neq i} |a_{ij}|$ (sum of absolute values of off-diagonal entries in row $i$).

**Center:** $a_{ii}$ (diagonal entry), **Radius:** $R_i$ (sum of |off-diagonal| in that row)

**Example:** $A = \begin{bmatrix} 4 & 1 & 0 \\ 0 & 2 & 1 \\ 1 & 0 & 6 \end{bmatrix}$

| Row | Center | Radius | Disc |
|---|---|---|---|
| 1 | 4 | $|1| + |0| = 1$ | $|z - 4| \leq 1$ → $[3, 5]$ |
| 2 | 2 | $|0| + |1| = 1$ | $|z - 2| \leq 1$ → $[1, 3]$ |
| 3 | 6 | $|1| + |0| = 1$ | $|z - 6| \leq 1$ → $[5, 7]$ |

All eigenvalues lie in $[1, 3] \cup [3, 5] \cup [5, 7]$. Since discs 1 and 3 are disjoint from disc 2, each must contain exactly one eigenvalue! ✅

> 🎯 **GATE Quick Check:** If all Gershgorin discs are in the right half-plane (center - radius > 0), all eigenvalues are positive → matrix is positive definite (for symmetric matrices)!

**Corollary — Diagonally Dominant Matrices:**
If $|a_{ii}| > \sum_{j \neq i} |a_{ij}|$ for all $i$ (strictly diagonally dominant), then $A$ is invertible.

### 10.10 Two Magical Properties

1. **Eigenvalues of $AB$ and $BA$:** If $A$ is $m \times n$ and $B$ is $n \times m$, then $AB$ ($m \times m$) and $BA$ ($n \times n$) have the **same non-zero eigenvalues** (with same multiplicities).

   > The extra eigenvalues (if dimensions differ) are all zero.

2. **$\text{trace}(AB) = \text{trace}(BA)$** — Even if $AB \neq BA$.

### 10.10A Left vs Right Eigenvectors (Micro-topic) 🔁

- Right eigenvector: $A v = \lambda v$
- Left eigenvector: $u^T A = \lambda u^T$ (equivalently $A^T u = \lambda u$)

For non-symmetric matrices, left and right eigenvectors are generally different. This matters in Markov chains, sensitivity analysis, and advanced DA models.

### 10.11 Complex Eigenvalues (⭐ GATE Awareness) 🔮

For a **real** matrix with complex eigenvalues:
- Complex eigenvalues always come in **conjugate pairs**: if $\lambda = a + bi$ is an eigenvalue, so is $\bar{\lambda} = a - bi$
- Corresponding eigenvectors are also conjugate pairs
- A real matrix of **odd** order always has at least one **real** eigenvalue
- $\det(A) = \prod \lambda_i$ is always real (conjugate pairs multiply to give real numbers)

**Example:** $A = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ (90° rotation)
- Characteristic equation: $\lambda^2 + 1 = 0$ → $\lambda = \pm i$ (complex conjugate pair)
- No real eigenvectors! (Rotation doesn't keep any direction fixed in $\mathbb{R}^2$)

### 10.12 Markov Chains & Stochastic Matrices (⭐ GATE DA) 🎲

> 💡 **Stochastic matrices model probability transitions — from weather prediction to Google PageRank!**

A **stochastic matrix** (row stochastic) has:
- All entries $\geq 0$
- Each row sums to 1

**Key eigenvalue properties:**

| Property | Detail |
|---|---|
| $\lambda = 1$ is always an eigenvalue | Eigenvector = $\vec{1}$ (all ones) for column stochastic |
| All eigenvalues satisfy $|\lambda| \leq 1$ | Bounded by 1 |
| **Steady state** $\vec{\pi}$ | The eigenvector for $\lambda = 1$: $A^T\vec{\pi} = \vec{\pi}$ |
| $\lim_{k \to \infty} A^k$ | Converges to steady state matrix (if ergodic) |

> 🏭 **Google PageRank** is literally the steady-state eigenvector of the web's stochastic link matrix! Larry Page's billion-dollar eigenvector! 💰

### 10.13 Perron-Frobenius Insight (Extension with high DA value) 👑

For a positive matrix ($a_{ij} > 0$):

- A unique dominant real eigenvalue $\lambda_{\max} > 0$ exists.
- The associated eigenvector can be chosen strictly positive.
- No other eigenvalue has equal magnitude.

This explains why repeated multiplication/normalization converges to one principal direction in ranking systems and iterative importance models.

### 10.14 Power Method (Computing dominant eigenpair) ⚙️

Iterative scheme:

$$x_{k+1} = \frac{Ax_k}{\|Ax_k\|}$$

If $|\lambda_1| > |\lambda_2|$, then $x_k$ converges to eigenvector of $\lambda_1$ (up to sign/scale). Rayleigh quotient estimates eigenvalue:

$$\rho(x) = \frac{x^TAx}{x^Tx}$$

---

### 10.15 Micro-Topic Coverage Checklist for Eigenvalues and Eigenvectors ✅

- 🔹 Characteristic equation and characteristic polynomial
- 🔹 Eigenspace computation by solving $(A-\lambda I)v=0$
- 🔹 Algebraic multiplicity vs geometric multiplicity
- 🔹 Distinct eigenvalues imply LI eigenvectors
- 🔹 Eigenvalues of triangular, diagonal, symmetric, orthogonal, idempotent, nilpotent matrices
- 🔹 Trace/determinant relations
- 🔹 Left vs right eigenvectors
- 🔹 Gershgorin, power method, Perron-Frobenius intuition
- 🔹 Spectral radius and stability intuition
- 🔹 GATE trap: eigenvectors are basis vectors only in special coordinates, not in general

### 10.16 Spectral Radius and Stability 🌪️

The spectral radius is
$$\rho(A)=\max_i |\lambda_i|$$

Why it matters:

- If $\rho(A)<1$, repeated powers $A^k$ tend to decay in many settings.
- If $\rho(A)>1$, repeated powers typically grow along dominant directions.
- In DA and control, this connects matrix iteration to convergence and stability.

### 10.17 Worked Example Bank: Eigenvalues 🎯

**Example 1 (Hard):** Find eigenvalues of
$$A=\begin{bmatrix}2&1&0\\0&2&0\\0&0&5\end{bmatrix}$$

**Solution:** Triangular matrix, so eigenvalues are diagonal entries: $2,2,5$. ✅

**Example 2 (Hard):** If $A$ is idempotent, what are possible eigenvalues?

**Solution:** From $A^2=A$, for any eigenvalue $\lambda$, $\lambda^2=\lambda$, so $\lambda\in\{0,1\}$. ✅

**Example 3 (Hard):** If $A$ is nilpotent, what are all eigenvalues?

**Solution:** From $A^k=0$, every eigenvalue satisfies $\lambda^k=0$, so $\lambda=0$. ✅

### 10.18 PYQ-Style Hard Traps: Eigenvalues 🔥

**Q1. Difficulty: Hard** 😵
Can a non-zero eigenvector correspond to two distinct eigenvalues?

**Answer:** No. A fixed non-zero vector cannot satisfy $Av=\lambda v=\mu v$ with $\lambda\ne\mu$. ✅

**Q2. Difficulty: Hard** 😵
If $0$ is an eigenvalue, what can you conclude about $A$?

**Answer:** $A$ is singular. ✅

**Q3. Difficulty: Hard** 😵
For symmetric real $A$, can eigenvalues be complex non-real?

**Answer:** No. They are always real. ✅

**Q4. Difficulty: Hard** 😵
If $A$ and $A^T$ have the same eigenvalues, do they have the same eigenvectors?

**Answer:** Not in general. Same eigenvalues only. ✅

**Q5. Difficulty: Hard** 😵
If $A$ is orthogonal, what is true about eigenvalue magnitudes?

**Answer:** All satisfy $|\lambda|=1$. ✅

**Q6. Difficulty: Hard** 😵
If a $3\times 3$ real matrix has two complex eigenvalues, what must be true about the third?

**Answer:** It must be real. Complex eigenvalues occur in conjugate pairs. ✅

---

## Chapter 11: Cayley-Hamilton Theorem (⭐ IMPORTANT) 🏆

> 💡 **Beginner Analogy — The Mirror of Self-Fulfillment 🪞:**
> The Cayley-Hamilton theorem says: "Every matrix obeys its own characteristic equation." It's like saying every person follows their own DNA blueprint! The matrix's "DNA" (characteristic polynomial) determines a rule, and the matrix ITSELF satisfies that rule! 🧬
>
> 🏭 **Production Use:** This theorem is used in **control systems engineering** (designing autopilots, robot controllers) to compute matrix powers efficiently, which is critical for real-time systems! 🤖

### 11.1 Statement

> Every square matrix satisfies its own characteristic equation.

If the characteristic polynomial is:
$$p(\lambda) = \lambda^n + c_{n-1}\lambda^{n-1} + \cdots + c_1\lambda + c_0 = 0$$

then:
$$p(A) = A^n + c_{n-1}A^{n-1} + \cdots + c_1A + c_0I = 0$$

### 11.2 Applications

1. **Finding $A^{-1}$:** From $A^2 - (\text{tr}A)A + \det(A)I = 0$, multiply by $A^{-1}$:
   $$A^{-1} = \frac{1}{\det(A)}[(\text{tr}A)I - A]$$

2. **Finding higher powers of $A$:** Express $A^n$ in terms of lower powers.

3. **Finding $f(A)$:** For any polynomial $f$, divide by characteristic polynomial to reduce degree.

---

**Example (GATE Level):** If $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$, find $A^{-1}$ using Cayley-Hamilton.

Characteristic equation: $\lambda^2 - 5\lambda - 2 = 0$

By Cayley-Hamilton: $A^2 - 5A - 2I = 0$

$\implies A^2 = 5A + 2I$

$\implies A = 5I + 2A^{-1}$

$\implies A^{-1} = \frac{1}{2}(A - 5I) = \frac{1}{2}\begin{bmatrix} -4 & 2 \\ 3 & -1 \end{bmatrix} = \begin{bmatrix} -2 & 1 \\ 3/2 & -1/2 \end{bmatrix}$

### 11.3 Computing $A^n$ for Large $n$ Using Cayley-Hamilton (⭐⭐ GATE FAVOURITE) 💪

> 💡 **This is one of the most frequently asked types in GATE — computing $A^{100}$, $A^{50}$, etc.!**

**Method:** For a $2 \times 2$ matrix with characteristic polynomial $\lambda^2 - a\lambda - b = 0$:

By CH: $A^2 = aA + bI$

Now express higher powers recursively:
- $A^3 = A \cdot A^2 = A(aA + bI) = aA^2 + bA = a(aA + bI) + bA = (a^2 + b)A + abI$
- $A^4 = A \cdot A^3 = \ldots$ (continue the pattern)

**General principle:** Any $A^n$ can be expressed as $\alpha A + \beta I$ (for $2 \times 2$).

**Example:** $A = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$. Find $A^{50}$.

CH: $\lambda^2 - 2\lambda + 1 = 0 \implies A^2 = 2A - I$

$A^n = nA - (n-1)I$ (by induction, since $(\lambda - 1)^2 = 0$ and the eigenvalue is repeated)

$A^{50} = 50A - 49I = 50\begin{bmatrix} 1&1\\0&1 \end{bmatrix} - 49\begin{bmatrix} 1&0\\0&1 \end{bmatrix} = \begin{bmatrix} 1 & 50 \\ 0 & 1 \end{bmatrix}$ ✅

> 🎯 **GATE Shortcut for Diagonalizable:** If $A = PDP^{-1}$, then $A^n = PD^nP^{-1}$. But if $A$ is NOT diagonalizable, use Cayley-Hamilton!

### 11.4 Minimal Polynomial (⭐ GATE Awareness) 📌

The **minimal polynomial** $m(\lambda)$ of $A$ is the **lowest degree monic polynomial** such that $m(A) = 0$.

| Property | Detail |
|---|---|
| Degree | $\leq n$ (degree of characteristic polynomial) |
| Divides $p(\lambda)$ | Minimal polynomial divides the characteristic polynomial |
| Same roots | Both have the **same roots** (but possibly different multiplicities) |
| Diagonalizability test | $A$ is diagonalizable iff minimal polynomial has **all distinct linear factors** (no repeated roots) |

**Examples:**
- $I_{n \times n}$: char poly = $(\lambda - 1)^n$, minimal poly = $(\lambda - 1)$
- $A = \text{diag}(1, 2, 2)$: char poly = $(\lambda - 1)(\lambda - 2)^2$, minimal poly = $(\lambda - 1)(\lambda - 2)$ (since it's diagonal → all GM = AM → distinct linear factors)
- $A = \begin{bmatrix} 2 & 1 \\ 0 & 2 \end{bmatrix}$: char poly = $(\lambda - 2)^2$, minimal poly = $(\lambda - 2)^2$ (NOT diagonalizable — the minimal poly has a repeated root)

> 🎯 **GATE Key:** If the minimal polynomial has a repeated root ↔ matrix is NOT diagonalizable.

---

### 11.5 Micro-Topic Coverage Checklist for Cayley-Hamilton ✅

- 🔹 Statement and substitution of matrix into its own characteristic polynomial
- 🔹 Use for reducing high powers
- 🔹 Use for inverse computation when invertible
- 🔹 Polynomial reduction modulo characteristic polynomial
- 🔹 Minimal polynomial relation
- 🔹 Repeated-root behaviour and non-diagonalizable matrices
- 🔹 GATE trap: characteristic polynomial always annihilates the matrix, minimal polynomial is the smallest such monic polynomial

### 11.6 Worked Example Bank: Cayley-Hamilton 🎯

**Example 1 (Hard):** For a $2\times 2$ matrix with characteristic polynomial $\lambda^2-4\lambda+3$, what matrix identity holds?

**Solution:**
$$A^2-4A+3I=0$$
Hence every higher power reduces to a combination of $A$ and $I$. ✅

**Example 2 (Hard):** If $A^2-5A+6I=0$ and $A$ is invertible, find $A^{-1}$.

**Solution:** Multiply by $A^{-1}$:
$$A-5I+6A^{-1}=0\Rightarrow A^{-1}=\frac{1}{6}(5I-A)$$
✅

### 11.7 PYQ-Style Hard Traps: Cayley-Hamilton 🔥

**Q1. Difficulty: Hard** 😵
Can CH be applied to non-square matrices?

**Answer:** No. Characteristic polynomial is for square matrices. ✅

**Q2. Difficulty: Hard** 😵
Does minimal polynomial always equal characteristic polynomial?

**Answer:** No. It only divides it. ✅

**Q3. Difficulty: Hard** 😵
For a $2\times 2$ matrix, can every power $A^n$ be reduced to $\alpha A+\beta I$?

**Answer:** Yes, via CH. ✅

**Q4. Difficulty: Hard** 😵
If $A$ satisfies $A^2=A$, what does CH/minimal polynomial suggest?

**Answer:** The polynomial $x(x-1)$ annihilates $A$; eigenvalues are $0$ or $1$. ✅

**Q5. Difficulty: Hard** 😵
If $A$ is nilpotent, what type of polynomial annihilates it?

**Answer:** Some power $x^k$. ✅

**Q6. Difficulty: Hard** 😵
Is CH a computational shortcut or just a theorem statement?

**Answer:** Both. In GATE it is frequently used as a shortcut for $A^n$ and $A^{-1}$. ✅

---

## Chapter 12: Rank and Eigenvalues

### 12.1 Connection Between Rank and Eigenvalues

| Result | Explanation |
|---|---|
| $\text{rank}(A) =$ number of non-zero eigenvalues (counting multiplicity) | Only for diagonalizable matrices |
| $A$ is singular $\iff$ 0 is an eigenvalue | $\det(A) = \prod \lambda_i$ |
| $\text{nullity}(A) =$ number of zero eigenvalues (AM) | For diagonalizable matrices |
| Idempotent ($A^2 = A$): eigenvalues ∈ \{0, 1\} | $\text{rank}(A) =$ trace$(A)$ |
| Nilpotent ($A^k = 0$): all eigenvalues = 0 | $\text{rank} < n$ |

### 12.2 Eigenvalues of Special Matrices

| Matrix Type | Eigenvalue Property |
|---|---|
| Symmetric ($A = A^T$) | All real |
| Skew-symmetric ($A = -A^T$) | Pure imaginary or zero |
| Orthogonal ($A^T A = I$) | $|\lambda| = 1$ |
| Idempotent ($A^2 = A$) | $\lambda \in \{0, 1\}$ |
| Nilpotent ($A^k = 0$) | All $\lambda = 0$ |
| Involutory ($A^2 = I$) | $\lambda \in \{-1, 1\}$ |
| Projection | $\lambda \in \{0, 1\}$ |
| Positive definite | All $\lambda > 0$ |

---

### 12.3 Micro-Topic Coverage Checklist for Rank-Eigenvalue Links ✅

- 🔹 Singularity iff zero eigenvalue
- 🔹 Rank deficiency and null space dimensions
- 🔹 Rank of idempotent matrix equals trace
- 🔹 Nilpotent matrix has only zero eigenvalues
- 🔹 Rank and number of non-zero singular values
- 🔹 Distinction between rank counts and eigenvalue multiplicity counts
- 🔹 GATE trap: number of non-zero eigenvalues equals rank only in special settings, not blindly for all defective matrices

### 12.4 Worked Example Bank: Rank and Eigenvalues 🎯

**Example 1 (Hard):** If eigenvalues of a $3\times 3$ diagonalizable matrix are $4,0,0$, what is its rank?

**Solution:** Number of non-zero eigenvalues counting multiplicity is $1$, so rank is $1$. ✅

**Example 2 (Hard):** If $A$ is idempotent with trace $3$, what is rank$(A)$?

**Solution:** For idempotent matrices, rank equals trace. Hence rank $=3$. ✅

### 12.5 PYQ-Style Hard Traps: Rank and Eigenvalues 🔥

**Q1. Difficulty: Hard** 😵
If $\det(A)=0$, is rank$(A)=n-1$ always?

**Answer:** No. Rank can be any value less than $n$. ✅

**Q2. Difficulty: Hard** 😵
If all eigenvalues are non-zero, what follows for a square matrix?

**Answer:** Matrix is invertible, hence full rank. ✅

**Q3. Difficulty: Hard** 😵
Can a rank-1 matrix have trace 0 and still be non-zero?

**Answer:** Yes, for some non-symmetric cases; trace alone does not determine rank. ✅

**Q4. Difficulty: Hard** 😵
For nilpotent $A$, can rank$(A)$ be non-zero?

**Answer:** Yes. Nilpotent does not mean zero matrix. ✅

**Q5. Difficulty: Hard** 😵
If $A$ is invertible, can $0$ be a singular value?

**Answer:** No. Invertible matrices have all singular values positive. ✅

**Q6. Difficulty: Hard** 😵
Does positive determinant imply positive definite?

**Answer:** No. Positive determinant alone is too weak. ✅

---

## Chapter 13: LU Decomposition

### 13.1 Concept

Factor $A = LU$ where:
- $L$ = Lower triangular matrix with 1s on diagonal
- $U$ = Upper triangular (the echelon form)

**When it works:** When Gaussian elimination can be performed **without row swaps**.

### 13.2 How to Find LU

Perform Gaussian elimination on $A$ to get $U$. The multipliers used go into $L$.

**Example:**
$$A = \begin{bmatrix} 2 & 1 \\ 6 & 4 \end{bmatrix}$$

$R_2 \leftarrow R_2 - 3R_1$: multiplier is 3

$$U = \begin{bmatrix} 2 & 1 \\ 0 & 1 \end{bmatrix}, \quad L = \begin{bmatrix} 1 & 0 \\ 3 & 1 \end{bmatrix}$$

Verify: $LU = \begin{bmatrix} 1&0\\3&1 \end{bmatrix}\begin{bmatrix} 2&1\\0&1 \end{bmatrix} = \begin{bmatrix} 2&1\\6&4 \end{bmatrix} = A$ ✓

### 13.3 Solving Ax = b Using LU

1. Factor $A = LU$
2. Solve $L\vec{y} = \vec{b}$ (forward substitution)
3. Solve $U\vec{x} = \vec{y}$ (back substitution)

**Advantage:** If we need to solve $A\vec{x} = \vec{b}$ for many different $\vec{b}$'s, we only factor $A$ once.

### 13.4 With Row Swaps: PA = LU

If row swaps are needed, we have $PA = LU$ where $P$ is a permutation matrix.

### 13.4A Existence and Uniqueness Micro-facts

- LU without pivoting exists if all leading principal minors are non-zero.
- PA=LU always exists (with suitable pivoting) for nonsingular matrices.
- If $L$ is constrained to have unit diagonal, decomposition is unique.

### 13.4B Doolittle vs Crout Forms (Extension) 🏗️

- **Doolittle:** diag($L$)=1, general $U$ diagonal
- **Crout:** diag($U$)=1, general $L$ diagonal

Both are LU families; only normalization convention differs.

### 13.5 Cholesky Decomposition (⭐ GATE — For Positive Definite Matrices) 🔑

> 💡 **Cholesky is like "taking the square root" of a positive definite matrix!**

**Statement:** If $A$ is **symmetric positive definite**, then there exists a **unique** lower triangular matrix $L$ with **positive diagonal entries** such that:
$$A = LL^T$$

**Why it matters:** Cholesky is **twice as fast** as LU decomposition and numerically more stable for SPD matrices! Used extensively in ML, statistics, and optimization.

**Example:** Factor $A = \begin{bmatrix} 4 & 2 \\ 2 & 5 \end{bmatrix}$ using Cholesky.

$A = LL^T$ where $L = \begin{bmatrix} l_{11} & 0 \\ l_{21} & l_{22} \end{bmatrix}$

$\begin{bmatrix} 4 & 2 \\ 2 & 5 \end{bmatrix} = \begin{bmatrix} l_{11}^2 & l_{11}l_{21} \\ l_{11}l_{21} & l_{21}^2 + l_{22}^2 \end{bmatrix}$

- $l_{11}^2 = 4 \implies l_{11} = 2$
- $l_{11}l_{21} = 2 \implies l_{21} = 1$
- $l_{21}^2 + l_{22}^2 = 5 \implies l_{22} = 2$

$$L = \begin{bmatrix} 2 & 0 \\ 1 & 2 \end{bmatrix}, \quad A = LL^T = \begin{bmatrix} 2&0\\1&2 \end{bmatrix}\begin{bmatrix} 2&1\\0&2 \end{bmatrix} = \begin{bmatrix} 4&2\\2&5 \end{bmatrix}$$ ✅

> 🎯 **GATE Key:** Cholesky exists ↔ matrix is symmetric positive definite. If asked "does Cholesky exist?", check SPD conditions!

| Decomposition | Condition | Form |
|---|---|---|
| LU | No row swaps needed | $A = LU$ |
| PA = LU | General (with permutation) | $PA = LU$ |
| Cholesky | Symmetric positive definite | $A = LL^T$ |
| $LDL^T$ | Symmetric (not necessarily PD) | $A = LDL^T$ ($D$ diagonal) |

---

### 13.6 Micro-Topic Coverage Checklist for LU and Cholesky ✅

- 🔹 LU factorization via elimination multipliers
- 🔹 PA=LU with pivoting
- 🔹 Forward and backward substitution
- 🔹 Doolittle vs Crout forms
- 🔹 Cholesky for SPD matrices
- 🔹 $LDL^T$ for symmetric matrices
- 🔹 Existence conditions and uniqueness conventions
- 🔹 Repeated solves with same matrix, different right-hand sides
- 🔹 GATE trap: LU without pivoting may fail even when matrix is invertible

### 13.7 Worked Example Bank: Decompositions 🎯

**Example 1 (Hard):** Why is LU useful for multiple right-hand sides?

**Solution:** Factor once as $A=LU$, then solve $Ly=b$ and $Ux=y$ for each new $b$. Total cost drops dramatically. ✅

**Example 2 (Hard):** When does Cholesky exist?

**Solution:** Exactly when the matrix is symmetric positive definite. ✅

**Example 3 (Hard):** If row swaps are needed during elimination, what decomposition is standard?

**Solution:** $PA=LU$. ✅

### 13.8 PYQ-Style Hard Traps: LU/Cholesky 🔥

**Q1. Difficulty: Hard** 😵
Can every invertible matrix be written as $A=LU$ without pivoting?

**Answer:** No. Sometimes row swaps are required. ✅

**Q2. Difficulty: Hard** 😵
Is every symmetric matrix Cholesky decomposable?

**Answer:** No. Must be symmetric positive definite. ✅

**Q3. Difficulty: Hard** 😵
If $A=LL^T$, what can you conclude about $A$?

**Answer:** $A$ is symmetric positive semidefinite; with positive diagonal and full rank, SPD. ✅

**Q4. Difficulty: Hard** 😵
What is the diagonal of $L$ in the standard Doolittle convention?

**Answer:** All ones. ✅

**Q5. Difficulty: Hard** 😵
Can $P$ in $PA=LU$ be chosen as any invertible matrix?

**Answer:** No. It is specifically a permutation matrix encoding row swaps. ✅

**Q6. Difficulty: Hard** 😵
What solve order is used once $A=LU$ is known?

**Answer:** First forward substitution in $Ly=b$, then backward substitution in $Ux=y$. ✅

---

## Chapter 14: Types of Matrices (⭐ FREQUENTLY ASKED) 📝

> 💡 **Beginner Analogy — Types of Employees 🧑‍💼:**
> Think of special matrices like employees with specific superpowers:
> - **Symmetric** = Diplomat 🤝 (treats everyone the same from both sides: $A = A^T$)
> - **Orthogonal** = Perfectionist 🎯 (preserves everything: lengths, angles, just rotates/reflects)
> - **Idempotent** = Lazy worker 😴 (doing the task twice = doing it once! $A^2 = A$)
> - **Nilpotent** = Self-destructor 💣 (repeated application leads to ZERO! Like repeatedly averaging towards 0)
> - **Positive Definite** = Always Positive ☀️ (energy is always positive — used in ML optimization!)
>
> 🏭 **Production Connections:**
> - **Symmetric matrices** = Covariance matrices in statistics, used in EVERY ML model
> - **Orthogonal matrices** = 3D rotations in games/VR/AR (Unity, Unreal)
> - **Positive definite** = Kernel matrices in SVM (Support Vector Machines)
> - **Sparse matrices** = Google's web link matrix (99.99% zeros!) — stored efficiently using CSR/CSC formats

### 14.1 Complete Reference Table

| Matrix Type | Definition | Properties |
|---|---|---|
| **Symmetric** | $A = A^T$ | Real eigenvalues; orthogonal eigenvectors; always diagonalizable |
| **Skew-Symmetric** | $A = -A^T$ | Diagonal entries = 0; eigenvalues are pure imaginary or 0; $\det(A) \geq 0$ for even $n$; $\det(A) = 0$ for odd $n$ |
| **Orthogonal** | $A^T A = I$ ($A^{-1} = A^T$) | $\|\lambda\| = 1$; columns are orthonormal; $\det = \pm 1$; preserves length and angles |
| **Idempotent** | $A^2 = A$ | Eigenvalues ∈ {0, 1}; $\text{rank}(A) = \text{trace}(A)$; $(I-A)$ is also idempotent |
| **Nilpotent** | $A^k = 0$ for some $k$ | All eigenvalues = 0; $\det = 0$; $\text{trace} = 0$; $(I - A)$ is invertible |
| **Involutory** | $A^2 = I$ | Eigenvalues ∈ {-1, 1}; $A = A^{-1}$ |
| **Diagonal** | $a_{ij} = 0$ for $i \neq j$ | Eigenvalues = diagonal entries; $A^k$ = diagonal with entries raised to $k$ |
| **Upper/Lower Triangular** | Entries below/above diagonal = 0 | Eigenvalues = diagonal entries; product of triangular = triangular |
| **Hermitian** | $A = A^*$ ($\bar{A}^T$) | Real eigenvalues; complex version of symmetric |
| **Unitary** | $A^*A = I$ | Complex version of orthogonal |
| **Normal** | $AA^* = A^*A$ | Unitarily diagonalizable |
| **Positive Definite** | $\vec{x}^T A\vec{x} > 0$ for all $\vec{x} \neq 0$ | All eigenvalues > 0; all pivots > 0; all leading minors > 0 |
| **Positive Semi-Definite** | $\vec{x}^T A\vec{x} \geq 0$ for all $\vec{x}$ | All eigenvalues $\geq 0$ |

### 14.2 Key Results for GATE

1. **Trace Properties:**
   - $\text{tr}(A + B) = \text{tr}(A) + \text{tr}(B)$
   - $\text{tr}(kA) = k \cdot \text{tr}(A)$
   - $\text{tr}(AB) = \text{tr}(BA)$ (even if $AB \neq BA$)
   - $\text{tr}(A) = \sum \lambda_i$

2. **For Idempotent Matrices:**
   - $\text{rank}(A) = \text{tr}(A)$
   - Eigenvalues are only 0 and 1
   - Number of eigenvalue 1 = rank = trace

3. **For Nilpotent Matrices ($A^k = 0$):**
   - $\text{tr}(A) = 0$, $\det(A) = 0$
   - $(I - A)^{-1} = I + A + A^2 + \cdots + A^{k-1}$
   - $(I + A)^{-1} = I - A + A^2 - \cdots + (-1)^{k-1}A^{k-1}$

### 14.3 Stochastic Matrices (⭐ GATE DA) 🎲

A **stochastic matrix** is a square matrix with non-negative entries where:

| Type | Condition | Eigenvalue Property |
|---|---|---|
| **Row stochastic** | Each row sums to 1 | $\lambda = 1$ is an eigenvalue (right eigenvector = $\vec{1}$) |
| **Column stochastic** | Each column sums to 1 | $\lambda = 1$ is an eigenvalue (left eigenvector = $\vec{1}^T$) |
| **Doubly stochastic** | Both rows AND columns sum to 1 | $\lambda = 1$; stationary distribution = uniform |

**Properties:**
- All eigenvalues satisfy $|\lambda| \leq 1$
- Product of stochastic matrices is stochastic
- Powers $A^k$ converge to a rank-1 matrix (for ergodic chains)

> 🏭 **GATE DA Link:** Stochastic matrices model **Markov chains**. The steady-state vector $\vec{\pi}$ satisfies $A^T\vec{\pi} = \vec{\pi}$ (eigenvector for $\lambda = 1$). Google PageRank = dominant eigenvector of a stochastic matrix! 🔍

### 14.4 Matrix Exponential (⭐ Awareness Level) 🧪

The matrix exponential is defined as:
$$e^A = I + A + \frac{A^2}{2!} + \frac{A^3}{3!} + \cdots = \sum_{k=0}^{\infty} \frac{A^k}{k!}$$

**Key properties:**
- $e^0 = I$
- $\det(e^A) = e^{\text{tr}(A)}$
- If $A = PDP^{-1}$, then $e^A = Pe^DP^{-1}$ where $e^D = \text{diag}(e^{\lambda_1}, \ldots, e^{\lambda_n})$
- $(e^A)^{-1} = e^{-A}$
- **WARNING:** $e^{A+B} = e^A e^B$ ONLY if $AB = BA$ (matrices commute)

> 🏭 **Application:** Solves systems of differential equations: $\vec{x}'(t) = A\vec{x}(t)$ has solution $\vec{x}(t) = e^{At}\vec{x}(0)$. Used in control theory, quantum mechanics, and dynamical systems! ⚡

---

### 14.5 Micro-Topic Coverage Checklist for Special Matrices ✅

- 🔹 Symmetric, skew-symmetric, orthogonal, idempotent, nilpotent, involutory
- 🔹 Hermitian, unitary, normal matrices for extension awareness
- 🔹 Positive definite and positive semidefinite
- 🔹 Stochastic and doubly stochastic matrices
- 🔹 Diagonal, triangular, block diagonal, sparse, banded
- 🔹 Eigenvalue restrictions for each special family
- 🔹 Trace, determinant, inverse behaviour
- 🔹 GATE trap: “special” properties combine only when hypotheses really overlap

### 14.6 Missing High-Value Matrix Families 🌟

**Sparse matrix:** mostly zero entries, crucial in graph and web-scale computation.

**Banded matrix:** non-zero entries confined near the diagonal, enabling faster linear solves.

**Diagonal dominant matrix:** useful for invertibility and numerical stability tests.

**Circulant matrix:** each row is a cyclic shift of the previous one; linked to FFT ideas.

### 14.7 Worked Example Bank: Special Matrices 🎯

**Example 1 (Hard):** If $A$ is skew-symmetric of odd order, what is $\det(A)$?

**Solution:** Zero. Since $\det(A)=\det(A^T)=\det(-A)=(-1)^n\det(A)=-\det(A)$ for odd $n$. ✅

**Example 2 (Hard):** If $A$ is orthogonal, show norm preservation.

**Solution:**
$$\|Ax\|^2=x^TA^TAx=x^TIx=\|x\|^2$$
✅

**Example 3 (Hard):** If $A$ is idempotent, what is $I-A$?

**Solution:** Also idempotent, since $(I-A)^2=I-2A+A^2=I-A$. ✅

### 14.8 PYQ-Style Hard Traps: Special Matrices 🔥

**Q1. Difficulty: Hard** 😵
Can an orthogonal matrix have determinant $2$?

**Answer:** No. Determinant must be $\pm 1$. ✅

**Q2. Difficulty: Hard** 😵
If $A$ is nilpotent, can it be invertible?

**Answer:** No. All eigenvalues are zero. ✅

**Q3. Difficulty: Hard** 😵
If $A$ is idempotent, must $A$ be symmetric?

**Answer:** No. Idempotent does not imply symmetric. ✅

**Q4. Difficulty: Hard** 😵
If $A$ is positive definite, can any diagonal entry be non-positive?

**Answer:** No. All diagonal entries are positive. ✅

**Q5. Difficulty: Hard** 😵
If $A$ is stochastic, is $1$ always an eigenvalue?

**Answer:** Yes. ✅

**Q6. Difficulty: Hard** 😵
Can a symmetric matrix fail to be diagonalizable over $\mathbb{R}$?

**Answer:** No. Real symmetric matrices are always orthogonally diagonalizable. ✅

---

# MODULE 2: ADVANCED LINEAR ALGEBRA

---

## Chapter 15: Vector Spaces

### 15.1 Definition

A **vector space** $V$ over a field $\mathbb{F}$ (usually $\mathbb{R}$) is a set of elements (vectors) with two operations:
1. **Vector addition:** $\vec{u} + \vec{v} \in V$
2. **Scalar multiplication:** $c\vec{v} \in V$

satisfying **10 axioms** (closure under addition and scalar multiplication, associativity, commutativity, existence of zero vector, existence of additive inverse, distributivity, etc.)

### 15.2 Common Vector Spaces

| Space | Description |
|---|---|
| $\mathbb{R}^n$ | n-tuples of real numbers |
| $\mathbb{P}_n$ | Polynomials of degree $\leq n$ (dimension = $n+1$) |
| $\mathbb{M}_{m \times n}$ | All $m \times n$ real matrices (dimension = $mn$) |
| $C[a,b]$ | Continuous functions on $[a,b]$ (infinite dimensional) |

### 15.2A Field Awareness (Micro-topic often ignored) 🌍

Vector spaces are defined over a field $\mathbb{F}$ (commonly $\mathbb{R}$ or $\mathbb{C}$). The same set can have different dimensions over different fields.

Example: $\mathbb{C}$ is 1-dimensional over $\mathbb{C}$ but 2-dimensional over $\mathbb{R}$.

### 15.3 How to Verify a Vector Space

Check:
1. **Zero vector** belongs to $V$
2. **Closed under addition:** If $\vec{u}, \vec{v} \in V$, then $\vec{u} + \vec{v} \in V$
3. **Closed under scalar multiplication:** If $\vec{v} \in V$ and $c \in \mathbb{F}$, then $c\vec{v} \in V$

> **Shortcut:** If $V$ is a non-empty subset of a known vector space and is closed under addition and scalar multiplication, then $V$ is a vector space (subspace).

### 15.4 Common "Traps" — Sets That Are NOT Vector Spaces

- $\{(x, y) : x + y = 1\}$ — doesn't contain zero vector
- $\{(x, y) : x \geq 0\}$ — not closed under scalar multiplication (multiply by -1)
- $\{(x, y) : xy \geq 0\}$ — not closed under addition

### 15.5 Inner Product Spaces (⭐ GATE DA) 🎵

> 💡 **An inner product generalizes the dot product to abstract vector spaces!**

An **inner product** on a real vector space $V$ is a function $\langle \cdot, \cdot \rangle : V \times V \to \mathbb{R}$ satisfying:

| Axiom | Condition |
|---|---|
| **Symmetry** | $\langle \vec{u}, \vec{v} \rangle = \langle \vec{v}, \vec{u} \rangle$ |
| **Linearity** | $\langle a\vec{u} + b\vec{w}, \vec{v} \rangle = a\langle \vec{u}, \vec{v} \rangle + b\langle \vec{w}, \vec{v} \rangle$ |
| **Positive definiteness** | $\langle \vec{v}, \vec{v} \rangle > 0$ for $\vec{v} \neq \vec{0}$; $\langle \vec{0}, \vec{0} \rangle = 0$ |

**Standard inner product** on $\mathbb{R}^n$: $\langle \vec{u}, \vec{v} \rangle = \vec{u}^T \vec{v} = \sum u_i v_i$ (the dot product)

**Induced norm:** $\|\vec{v}\| = \sqrt{\langle \vec{v}, \vec{v} \rangle}$

**Important results in inner product spaces:**
- Cauchy-Schwarz: $|\langle \vec{u}, \vec{v} \rangle| \leq \|\vec{u}\| \cdot \|\vec{v}\|$
- Triangle inequality: $\|\vec{u} + \vec{v}\| \leq \|\vec{u}\| + \|\vec{v}\|$
- Parallelogram law: $\|\vec{u} + \vec{v}\|^2 + \|\vec{u} - \vec{v}\|^2 = 2(\|\vec{u}\|^2 + \|\vec{v}\|^2)$
- Polarization identity: $\langle \vec{u}, \vec{v} \rangle = \frac{1}{4}(\|\vec{u}+\vec{v}\|^2 - \|\vec{u}-\vec{v}\|^2)$

> 🏭 **GATE DA Link:** In ML, the kernel trick defines custom inner products: polynomial kernel, RBF kernel, etc. This turns non-linearly separable data into linearly separable in a higher-dimensional inner product space! 🧠

---

### 15.6 Micro-Topic Coverage Checklist for Vector Spaces ✅

- 🔹 Vector space axioms and shortcut subspace test
- 🔹 Standard examples: $\mathbb{R}^n$, polynomial spaces, matrix spaces, function spaces
- 🔹 Finite vs infinite dimensional spaces
- 🔹 Field awareness: $\mathbb{R}$ vs $\mathbb{C}$
- 🔹 Inner products and induced norms
- 🔹 Parallelogram law and polarization identity
- 🔹 GATE trap: satisfying only some closure properties is not enough for a vector space

### 15.7 Worked Example Bank: Vector Spaces 🎯

**Example 1 (Hard):** Is $V=\{(x,y,z):x+y+z=0\}$ a vector space?

**Solution:** Yes. It contains zero and is closed under addition and scalar multiplication because the defining equation is homogeneous. ✅

**Example 2 (Hard):** Why is $\mathbb{P}_n$ of dimension $n+1$?

**Solution:** Basis is $\{1,x,x^2,\dots,x^n\}$, which has $n+1$ vectors. ✅

**Example 3 (Hard):** Why is $C[a,b]$ infinite dimensional?

**Solution:** The set $\{1,x,x^2,\dots\}$ gives infinitely many LI functions. ✅

### 15.8 PYQ-Style Hard Traps: Vector Spaces 🔥

**Q1. Difficulty: Hard** 😵
Does closure under addition alone make a subset a vector space?

**Answer:** No. Need scalar-multiplication closure and zero vector too. ✅

**Q2. Difficulty: Hard** 😵
Is $\{(x,y):x\ge 0\}$ a vector space?

**Answer:** No. Not closed under negative scalar multiplication. ✅

**Q3. Difficulty: Hard** 😵
Is $\{(x,y):x+y=1\}$ a vector space?

**Answer:** No. Does not contain the zero vector. ✅

**Q4. Difficulty: Hard** 😵
What is the dimension of all $2\times 3$ real matrices?

**Answer:** $6$. Basis is standard matrices $E_{ij}$. ✅

**Q5. Difficulty: Hard** 😵
Can the same set have different dimensions over different fields?

**Answer:** Yes. Example: $\mathbb{C}$ over $\mathbb{R}$ vs over $\mathbb{C}$. ✅

**Q6. Difficulty: Hard** 😵
Does every inner product induce a norm?

**Answer:** Yes. $\|v\|=\sqrt{\langle v,v\rangle}$. ✅

---

## Chapter 16: Subspaces

### 16.1 Definition

A subset $W$ of a vector space $V$ is a **subspace** if $W$ is itself a vector space under the same operations.

**Subspace Test (3 conditions):**
1. $\vec{0} \in W$
2. If $\vec{u}, \vec{v} \in W$, then $\vec{u} + \vec{v} \in W$ (closed under addition)
3. If $\vec{v} \in W$ and $c$ scalar, then $c\vec{v} \in W$ (closed under scalar multiplication)

> **Equivalent single condition:** $W \neq \emptyset$ and for all $\vec{u}, \vec{v} \in W$ and scalars $a, b$: $a\vec{u} + b\vec{v} \in W$

### 16.2 Examples of Subspaces

- Any line through the origin in $\mathbb{R}^2$
- Any plane through the origin in $\mathbb{R}^3$
- $\{0\}$ (trivial subspace) and $V$ itself
- Solution set of $A\vec{x} = \vec{0}$ (null space)
- Column space, row space, null space of any matrix

### 16.3 NOT a Subspace

- A line not through the origin
- Solution set of $A\vec{x} = \vec{b}$ where $\vec{b} \neq \vec{0}$
- Union of two subspaces (in general) — but intersection IS always a subspace

> **Important:** Intersection of subspaces is always a subspace. Union of subspaces is a subspace ONLY if one is contained in the other.

### 16.4 Dimension Formula for Sum of Subspaces (⭐ GATE) 📐

If $U$ and $W$ are subspaces of $V$:
$$\dim(U + W) = \dim(U) + \dim(W) - \dim(U \cap W)$$

where $U + W = \{u + w : u \in U, w \in W\}$ (the sum of subspaces)

> 💡 **Analogy:** This is exactly like the inclusion-exclusion principle for sets! $|A \cup B| = |A| + |B| - |A \cap B|$ 🔢

**Example:** In $\mathbb{R}^3$, if $U$ is a plane (dim 2) and $W$ is a line not in $U$ (dim 1), then $U \cap W = \{0\}$ (dim 0), so $\dim(U + W) = 2 + 1 - 0 = 3 = \dim(\mathbb{R}^3)$. Hence $U + W = \mathbb{R}^3$.

### 16.5 Direct Sum of Subspaces ⊕

$V = U \oplus W$ ($V$ is the **direct sum** of $U$ and $W$) means:
1. $V = U + W$ (every vector in $V$ can be written as $u + w$)
2. $U \cap W = \{0\}$ (the decomposition is unique)

**Equivalent:** Every $\vec{v} \in V$ has a **unique** representation $\vec{v} = \vec{u} + \vec{w}$ with $\vec{u} \in U, \vec{w} \in W$.

If $V = U \oplus W$, then $\dim(V) = \dim(U) + \dim(W)$.

> 🎯 **GATE Connection:** The four fundamental subspaces give direct sum decompositions:
> - $\mathbb{R}^n = C(A^T) \oplus N(A)$ (row space ⊕ null space)
> - $\mathbb{R}^m = C(A) \oplus N(A^T)$ (column space ⊕ left null space)

---

### 16.6 Micro-Topic Coverage Checklist for Subspaces ✅

- 🔹 Subspace test and one-shot linear-combination test
- 🔹 Trivial and improper subspaces
- 🔹 Intersections, sums, direct sums
- 🔹 Solution sets of homogeneous systems as subspaces
- 🔹 Non-homogeneous solution sets not being subspaces
- 🔹 Dimension formula for sum of subspaces
- 🔹 Geometric intuition in low dimensions
- 🔹 GATE trap: union of subspaces is usually not a subspace

### 16.7 Worked Example Bank: Subspaces 🎯

**Example 1 (Hard):** Is $W=\{(x,y,z):x=2y\}$ a subspace of $\mathbb{R}^3$?

**Solution:** Yes. The defining relation is homogeneous and preserved under addition and scalar multiplication. ✅

**Example 2 (Hard):** Is the union of the x-axis and y-axis in $\mathbb{R}^2$ a subspace?

**Solution:** No. $(1,0)$ and $(0,1)$ belong, but their sum $(1,1)$ does not. ✅

**Example 3 (Hard):** If $U$ is a plane through origin in $\mathbb{R}^3$ and $W$ is a line in that plane through origin, what is $U\cap W$?

**Solution:** The line itself. ✅

### 16.8 PYQ-Style Hard Traps: Subspaces 🔥

**Q1. Difficulty: Hard** 😵
Is every subset containing zero a subspace?

**Answer:** No. Closure properties are still required. ✅

**Q2. Difficulty: Hard** 😵
Is the solution set of $Ax=b$ with $b\ne 0$ a subspace?

**Answer:** No, in general. ✅

**Q3. Difficulty: Hard** 😵
Is intersection of subspaces always a subspace?

**Answer:** Yes. ✅

**Q4. Difficulty: Hard** 😵
When is union of two subspaces a subspace?

**Answer:** Only when one is contained in the other. ✅

**Q5. Difficulty: Hard** 😵
If $V=U\oplus W$, what is $U\cap W$?

**Answer:** $\{0\}$. ✅

**Q6. Difficulty: Hard** 😵
What is $\dim(U+W)$ if $\dim(U)=3$, $\dim(W)=2$, and $\dim(U\cap W)=1$?

**Answer:** $3+2-1=4$. ✅

---

## Chapter 17: Basis, Span, and Dimension

### 17.1 Definitions Recap

| Concept | Definition |
|---|---|
| **Span** | Set of all linear combinations of given vectors |
| **Basis** | A linearly independent spanning set |
| **Dimension** | Number of vectors in a basis |

### 17.2 Key Results

1. Every basis of a finite-dimensional vector space has the **same number** of vectors
2. If $\dim(V) = n$:
   - Any $n$ LI vectors form a basis
   - Any $n$ vectors that span $V$ form a basis
   - Any LI set can be extended to a basis
   - Any spanning set can be pruned to a basis
3. If $W$ is a subspace of $V$: $\dim(W) \leq \dim(V)$, and equality iff $W = V$

---

### 17.3 Micro-Topic Coverage Checklist for Basis and Dimension ✅

- 🔹 Basis as LI spanning set
- 🔹 Dimension invariance theorem
- 🔹 Extending LI sets to a basis
- 🔹 Shrinking spanning sets to a basis
- 🔹 Coordinates and uniqueness
- 🔹 Basis of null space, column space, row space
- 🔹 Exchange ideas and redundancy removal
- 🔹 GATE trap: “number of vectors equals dimension” is useful only together with LI or spanning condition

### 17.4 Exchange Principle Intuition 🔄

If $B$ is a basis and $v$ is a vector not in span of some partial subset, then one may replace a suitable basis vector by $v$ and still get a basis.

This idea powers many proofs though GATE usually tests it implicitly, not by name.

### 17.5 Worked Example Bank: Basis and Dimension 🎯

**Example 1 (Hard):** In a 4-dimensional vector space, can 5 vectors be LI?

**Solution:** No. Any set larger than the dimension is dependent. ✅

**Example 2 (Hard):** In a 4-dimensional vector space, if 4 vectors span, what can you conclude?

**Solution:** They form a basis. ✅

**Example 3 (Hard):** If 4 vectors in a 4-dimensional vector space are LI, what can you conclude?

**Solution:** They form a basis. ✅

### 17.6 PYQ-Style Hard Traps: Basis and Dimension 🔥

**Q1. Difficulty: Hard** 😵
Can a basis contain the zero vector?

**Answer:** No. That would break linear independence. ✅

**Q2. Difficulty: Hard** 😵
Can a basis be non-unique?

**Answer:** Yes. Many bases can exist, but all have same size. ✅

**Q3. Difficulty: Hard** 😵
If a space has dimension 3, can a spanning set have 5 vectors?

**Answer:** Yes, but it will be redundant. ✅

**Q4. Difficulty: Hard** 😵
If a space has dimension 3, can an LI set have 2 vectors?

**Answer:** Yes, but it is not yet a basis. ✅

**Q5. Difficulty: Hard** 😵
Does every finite-dimensional non-zero vector space have a basis?

**Answer:** Yes. ✅

**Q6. Difficulty: Hard** 😵
If $W\subseteq V$ and $\dim(W)=\dim(V)$ in finite dimensions, what follows?

**Answer:** $W=V$. ✅

---

## Chapter 18: Four Fundamental Subspaces (⭐⭐ KEY CONCEPT) 🌟

> 💡 **Beginner Analogy — The Matrix's DNA 🧬:**
> Every matrix has FOUR hidden "rooms" (subspaces) — like four chambers of a heart ❤️:
> 1. **Column Space** = What outputs can I produce? (the "range of possibilities")
> 2. **Null Space** = What inputs get killed to zero? (the "invisibility cloak" directions)
> 3. **Row Space** = What input directions actually matter? (the "active ingredients")
> 4. **Left Null Space** = What outputs are impossible? (the "rejected" directions)
>
> Gilbert Strang (MIT) called these the "four fundamental subspaces" and built his entire legendary Linear Algebra course around them! 🎓
>
> 🏭 **Production Insight:** In recommendation systems (Netflix, Amazon), the column space represents what ratings are possible, and the null space captures user preferences that don't affect ratings — this decomposition is at the heart of **collaborative filtering**! 🎬

For an $m \times n$ matrix $A$ with $\text{rank}(A) = r$:

### 18.1 Definitions

| Subspace | Symbol | Definition | Dimension | Lives in |
|---|---|---|---|---|
| **Column Space** | $C(A)$ | $\{A\vec{x} : \vec{x} \in \mathbb{R}^n\}$ = span of columns | $r$ | $\mathbb{R}^m$ |
| **Null Space** | $N(A)$ | $\{\vec{x} : A\vec{x} = \vec{0}\}$ | $n - r$ | $\mathbb{R}^n$ |
| **Row Space** | $C(A^T)$ | Span of rows of $A$ = Column space of $A^T$ | $r$ | $\mathbb{R}^n$ |
| **Left Null Space** | $N(A^T)$ | $\{\vec{y} : A^T\vec{y} = \vec{0}\}$ | $m - r$ | $\mathbb{R}^m$ |

### 18.2 The Big Picture

```
                    R^n                              R^m
        ┌───────────────────────┐        ┌───────────────────────┐
        │                       │        │                       │
        │  Row Space   Null     │   A    │  Column    Left Null  │
        │  C(A^T)     Space     │ ────→  │  Space     Space      │
        │  dim = r    N(A)      │        │  C(A)      N(A^T)     │
        │             dim=n-r   │        │  dim = r   dim=m-r    │
        │                       │        │                       │
        └───────────────────────┘        └───────────────────────┘
        
        Row space ⊥ Null space           Column space ⊥ Left null space
        (orthogonal complements)          (orthogonal complements)
```

### 18.3 Key Relationships

1. $C(A^T) \perp N(A)$ in $\mathbb{R}^n$ and $\dim C(A^T) + \dim N(A) = n$
2. $C(A) \perp N(A^T)$ in $\mathbb{R}^m$ and $\dim C(A) + \dim N(A^T) = m$
3. $A\vec{x} = \vec{b}$ has a solution iff $\vec{b} \in C(A)$
4. $A\vec{x} = \vec{b}$ has a solution iff $\vec{b} \perp N(A^T)$ (i.e., $\vec{b}$ is orthogonal to every vector in the left null space)

### 18.4 How to Find Each Space

| Space | Method |
|---|---|
| Column Space $C(A)$ | Row reduce $A$; pivot columns of **original** $A$ form basis |
| Null Space $N(A)$ | Solve $A\vec{x} = \vec{0}$; express in terms of free variables |
| Row Space $C(A^T)$ | Row reduce $A$; non-zero rows of **REF** form basis |
| Left Null Space $N(A^T)$ | Solve $A^T\vec{y} = \vec{0}$ (or use the Gauss-Jordan method on $[A^T|I]$) |

---

**Example:** Find all four fundamental subspaces for:
$$A = \begin{bmatrix} 1 & 3 & 5 \\ 2 & 6 & 10 \end{bmatrix}$$

Row reduce: $R_2 \leftarrow R_2 - 2R_1$:
$$\begin{bmatrix} 1 & 3 & 5 \\ 0 & 0 & 0 \end{bmatrix}$$

$\text{rank} = 1$

- **Column Space:** Basis = $\left\{\begin{bmatrix} 1\\2 \end{bmatrix}\right\}$ (1st column of original $A$, the pivot column), dim = 1
- **Row Space:** Basis = $\left\{\begin{bmatrix} 1\\3\\5 \end{bmatrix}\right\}$ (non-zero row of REF), dim = 1
- **Null Space:** From $x_1 + 3x_2 + 5x_3 = 0$, free vars: $x_2 = s, x_3 = t$

  $x_1 = -3s - 5t$ → Basis = $\left\{\begin{bmatrix} -3\\1\\0 \end{bmatrix}, \begin{bmatrix} -5\\0\\1 \end{bmatrix}\right\}$, dim = 2
  
- **Left Null Space:** Solve $A^T\vec{y} = \vec{0}$: $\begin{bmatrix} 1&2\\3&6\\5&10 \end{bmatrix}\begin{bmatrix} y_1\\y_2 \end{bmatrix} = \vec{0}$

  $y_1 = -2y_2$. Basis = $\left\{\begin{bmatrix} -2\\1 \end{bmatrix}\right\}$, dim = 1

**Check:** $1 + 2 = 3 = n$ ✓ and $1 + 1 = 2 = m$ ✓

---

### 18.5 Micro-Topic Coverage Checklist for Four Fundamental Subspaces ✅

- 🔹 Column space, row space, null space, left null space
- 🔹 Ambient spaces where each one lives
- 🔹 Dimension counts and rank-nullity relations
- 🔹 Orthogonal complement relations
- 🔹 Consistency test via column space and left null space
- 🔹 Basis extraction procedures for each subspace
- 🔹 Decompositions of domain and codomain spaces
- 🔹 GATE trap: row operations preserve row space basis information but not original column vectors themselves

### 18.6 Worked Example Bank: Fundamental Subspaces 🎯

**Example 1 (Hard):** If $A$ is $4\times 6$ with rank $3$, what are dimensions of the four subspaces?

**Solution:**
- dim$\,C(A)=3$
- dim$\,N(A)=6-3=3$
- dim$\,C(A^T)=3$
- dim$\,N(A^T)=4-3=1$
✅

**Example 2 (Hard):** Why is $b\in C(A)$ equivalent to solvability of $Ax=b$?

**Solution:** Because $Ax$ is a linear combination of columns of $A$. ✅

**Example 3 (Hard):** Why is $N(A)$ orthogonal to row space?

**Solution:** If $x\in N(A)$, each row dot-product with $x$ equals zero. Hence $x$ is orthogonal to every row vector. ✅

### 18.7 PYQ-Style Hard Traps: Fundamental Subspaces 🔥

**Q1. Difficulty: Hard** 😵
Do row space and null space live in the same ambient space?

**Answer:** Yes, both live in $\mathbb{R}^n$ for an $m\times n$ matrix. ✅

**Q2. Difficulty: Hard** 😵
Do column space and left null space live in the same ambient space?

**Answer:** Yes, both live in $\mathbb{R}^m$. ✅

**Q3. Difficulty: Hard** 😵
Is the column space orthogonal to the null space?

**Answer:** No. Column space is orthogonal to left null space. ✅

**Q4. Difficulty: Hard** 😵
If $A$ has full column rank, what is $N(A)$?

**Answer:** Only the zero vector. ✅

**Q5. Difficulty: Hard** 😵
If $A$ has full row rank, what is $N(A^T)$?

**Answer:** Only the zero vector. ✅

**Q6. Difficulty: Hard** 😵
Can $Ax=b$ be inconsistent even when $A$ has non-zero columns?

**Answer:** Yes. If $b\notin C(A)$, no solution exists. ✅

---

## Chapter 19: Change of Basis

### 19.1 Concept

Every vector has coordinates that depend on the choice of basis. If we switch from basis $B_1$ to basis $B_2$, we need a **change of basis matrix** $P$.

If $[\vec{v}]_{B_1}$ is the coordinate vector w.r.t. $B_1$ and $[\vec{v}]_{B_2}$ w.r.t. $B_2$:

$$[\vec{v}]_{B_2} = P^{-1}[\vec{v}]_{B_1}$$

where $P$ is the matrix whose columns are the vectors of $B_2$ expressed in the basis $B_1$.

### 19.2 Change of Basis Matrix

If $B_1 = \{\vec{u}_1, \ldots, \vec{u}_n\}$ and $B_2 = \{\vec{w}_1, \ldots, \vec{w}_n\}$, and both are expressed in standard coordinates:

Let $U = [\vec{u}_1 | \cdots | \vec{u}_n]$ and $W = [\vec{w}_1 | \cdots | \vec{w}_n]$.

Then:
$$[\vec{v}]_{B_2} = W^{-1} U \cdot [\vec{v}]_{B_1}$$

The **change of basis matrix** from $B_1$ to $B_2$ is $P_{B_1 \to B_2} = W^{-1}U$.

### 19.3 From Standard Basis

If $B_1$ is the standard basis: $U = I$, so $P = W^{-1}$.

Coordinates in new basis $B_2$: $[\vec{v}]_{B_2} = W^{-1}\vec{v}$

---

**Example:** Let $B = \left\{\begin{bmatrix} 1\\1 \end{bmatrix}, \begin{bmatrix} 1\\-1 \end{bmatrix}\right\}$. Express $\vec{v} = \begin{bmatrix} 3\\1 \end{bmatrix}$ in basis $B$.

$W = \begin{bmatrix} 1&1\\1&-1 \end{bmatrix}$, $W^{-1} = \frac{1}{-2}\begin{bmatrix} -1&-1\\-1&1 \end{bmatrix} = \begin{bmatrix} 1/2 & 1/2\\ 1/2 & -1/2 \end{bmatrix}$

$[\vec{v}]_B = W^{-1}\vec{v} = \begin{bmatrix} 1/2&1/2\\1/2&-1/2 \end{bmatrix}\begin{bmatrix} 3\\1 \end{bmatrix} = \begin{bmatrix} 2\\1 \end{bmatrix}$

So $\vec{v} = 2\begin{bmatrix} 1\\1 \end{bmatrix} + 1\begin{bmatrix} 1\\-1 \end{bmatrix} = \begin{bmatrix} 3\\1 \end{bmatrix}$ ✓

---

### 19.4 Micro-Topic Coverage Checklist for Change of Basis ✅

- 🔹 Coordinates with respect to a basis
- 🔹 Transition matrices between bases
- 🔹 From standard basis to custom basis and back
- 🔹 Similarity under basis change
- 🔹 Basis change for linear transformation matrices
- 🔹 Coordinate vector computation by solving linear systems
- 🔹 GATE trap: vectors themselves do not change, only their coordinate descriptions do

### 19.5 Worked Example Bank: Change of Basis 🎯

**Example 1 (Hard):** If $B=\{(1,0),(1,1)\}$, find coordinates of $(3,2)$ in basis $B$.

**Solution:** Solve
$$c_1(1,0)+c_2(1,1)=(3,2)$$
Then $c_2=2$ and $c_1=1$. Coordinates are $(1,2)$. ✅

**Example 2 (Hard):** Why are similar matrices associated with the same linear map in different bases?

**Solution:** Because only the coordinate system changes; the underlying transformation does not. ✅

### 19.6 PYQ-Style Hard Traps: Change of Basis 🔥

**Q1. Difficulty: Hard** 😵
If basis vectors form columns of $P$, how do standard coordinates map to basis coordinates?

**Answer:** Multiply by $P^{-1}$. ✅

**Q2. Difficulty: Hard** 😵
Does changing basis change eigenvalues?

**Answer:** No. Similar matrices have same eigenvalues. ✅

**Q3. Difficulty: Hard** 😵
Does changing basis change determinant and trace of the matrix representation?

**Answer:** No. Similarity preserves both. ✅

**Q4. Difficulty: Hard** 😵
If $P$ is change-of-basis matrix, what form relates two matrices of same map?

**Answer:** $B=P^{-1}AP$. ✅

**Q5. Difficulty: Hard** 😵
Do coordinates depend on basis choice?

**Answer:** Yes. Same vector, different coordinate column. ✅

**Q6. Difficulty: Hard** 😵
Can a non-basis set be used for coordinate representation uniquely?

**Answer:** No. Need a basis for unique coordinates. ✅

---

## Chapter 20: Linear Transformations (⭐⭐ IMPORTANT FOR GATE DA)

### 20.1 Definition

A function $T: V \to W$ between vector spaces is a **linear transformation** if:

1. $T(\vec{u} + \vec{v}) = T(\vec{u}) + T(\vec{v})$ (preserves addition)
2. $T(c\vec{v}) = cT(\vec{v})$ (preserves scalar multiplication)

**Equivalently:** $T(a\vec{u} + b\vec{v}) = aT(\vec{u}) + bT(\vec{v})$

**Note:** $T(\vec{0}) = \vec{0}$ always for a linear transformation.

### 20.2 Examples of Linear Transformations

| Transformation | Formula | Linear? |
|---|---|---|
| Rotation by $\theta$ | $T\begin{bmatrix} x\\y \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta\\ \sin\theta & \cos\theta \end{bmatrix}\begin{bmatrix} x\\y \end{bmatrix}$ | Yes |
| Reflection about x-axis | $T\begin{bmatrix} x\\y \end{bmatrix} = \begin{bmatrix} x\\-y \end{bmatrix}$ | Yes |
| Projection onto x-axis | $T\begin{bmatrix} x\\y \end{bmatrix} = \begin{bmatrix} x\\0 \end{bmatrix}$ | Yes |
| Translation | $T(\vec{v}) = \vec{v} + \vec{a}$ | **No** (unless $\vec{a} = \vec{0}$) |
| $T(x) = x^2$ | Squares the input | **No** |
| Differentiation $D(f) = f'$ | On polynomial space | Yes |
| Integration $\int_0^x f(t)dt$ | On polynomial space | Yes |

### 20.3 Matrix of a Linear Transformation

> Every linear transformation between finite-dimensional vector spaces can be represented as a matrix.

**Method 1 (Standard Basis):**

If $T: \mathbb{R}^n \to \mathbb{R}^m$, the matrix $A$ of $T$ w.r.t. standard bases is:

$$A = [T(\vec{e}_1) | T(\vec{e}_2) | \cdots | T(\vec{e}_n)]$$

Each column is the image of a standard basis vector.

**Method 2 (General Basis):**

If $T: V \to W$ with basis $B_1 = \{\vec{v}_1, \ldots, \vec{v}_n\}$ for $V$ and $B_2 = \{\vec{w}_1, \ldots, \vec{w}_m\}$ for $W$:

$$[T]_{B_1}^{B_2} = [[T(\vec{v}_1)]_{B_2} | [T(\vec{v}_2)]_{B_2} | \cdots | [T(\vec{v}_n)]_{B_2}]$$

Each column is the coordinate representation of $T(\vec{v}_i)$ in basis $B_2$.

### 20.4 Kernel and Image (Range)

| Concept | Definition | Dimension |
|---|---|---|
| **Kernel** (Null Space) | $\text{ker}(T) = \{\vec{v} : T(\vec{v}) = \vec{0}\}$ | $\text{nullity}(T)$ |
| **Image** (Range) | $\text{Im}(T) = \{T(\vec{v}) : \vec{v} \in V\}$ | $\text{rank}(T)$ |

> **Rank-Nullity for LT:** $\dim(V) = \text{rank}(T) + \text{nullity}(T)$

### 20.5 Types of Linear Transformations

| Type | Condition | Matrix Condition |
|---|---|---|
| **One-to-one (Injective)** | $T(\vec{u}) = T(\vec{v}) \implies \vec{u} = \vec{v}$ | $\text{nullity}(T) = 0$ |
| **Onto (Surjective)** | Every $\vec{w} \in W$ has a pre-image | $\text{rank}(T) = \dim(W)$ |
| **Bijective (Isomorphism)** | Both injective and surjective | $T$ is invertible |

### 20.6 The Second Method — Matrix of LT w.r.t. Custom Basis

If you know $T$ as a matrix $A$ w.r.t. standard basis and want it w.r.t. basis $B = \{v_1, \ldots, v_n\}$:

$$[T]_B = P^{-1}AP$$

where $P = [v_1 | v_2 | \cdots | v_n]$ (columns are basis vectors).

### 20.7 Standard Transformation Matrices in $\mathbb{R}^2$ (⭐ GATE FAVOURITE) 🎨

> 💡 **MEMORIZE these — GATE asks direct questions about them!**

| Transformation | Matrix | Effect |
|---|---|---|
| **Rotation by $\theta$** (CCW) | $\begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$ | Rotates every vector by angle $\theta$ |
| **Reflection about x-axis** | $\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$ | Flips y-coordinate |
| **Reflection about y-axis** | $\begin{bmatrix} -1 & 0 \\ 0 & 1 \end{bmatrix}$ | Flips x-coordinate |
| **Reflection about $y = x$** | $\begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$ | Swaps coordinates |
| **Reflection about origin** | $\begin{bmatrix} -1 & 0 \\ 0 & -1 \end{bmatrix}$ | Negates both coordinates |
| **Reflection about $y = x\tan\theta$** | $\begin{bmatrix} \cos 2\theta & \sin 2\theta \\ \sin 2\theta & -\cos 2\theta \end{bmatrix}$ | General line through origin |
| **Scaling** | $\begin{bmatrix} k_1 & 0 \\ 0 & k_2 \end{bmatrix}$ | Scales x by $k_1$, y by $k_2$ |
| **Horizontal shear** | $\begin{bmatrix} 1 & k \\ 0 & 1 \end{bmatrix}$ | Shears along x-axis |
| **Vertical shear** | $\begin{bmatrix} 1 & 0 \\ k & 1 \end{bmatrix}$ | Shears along y-axis |
| **Projection onto x-axis** | $\begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$ | Drops y-component |
| **Projection onto y-axis** | $\begin{bmatrix} 0 & 0 \\ 0 & 1 \end{bmatrix}$ | Drops x-component |

> 🎯 **GATE Trap:** Rotation matrix is **orthogonal** ($\det = 1$), reflection matrix is also orthogonal ($\det = -1$). Both preserve lengths!

### 20.8 Composition of Transformations 🔗

If $T_1$ has matrix $A$ and $T_2$ has matrix $B$, then:
- $T_2 \circ T_1$ (first apply $T_1$, then $T_2$) has matrix $BA$ (note: **right to left**!)
- $(T_2 \circ T_1)(\vec{v}) = B(A\vec{v}) = (BA)\vec{v}$

**Example:** Rotate by 90° then reflect about x-axis:
$$R = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}, \quad S = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$$

Composition = $SR = \begin{bmatrix} 1&0\\0&-1 \end{bmatrix}\begin{bmatrix} 0&-1\\1&0 \end{bmatrix} = \begin{bmatrix} 0 & -1 \\ -1 & 0 \end{bmatrix}$ (reflection about $y = -x$)

### 20.9 Invariant Subspaces (Advanced but exam-useful) 🧠

A subspace $W$ is invariant under $T$ if $T(W) \subseteq W$.

Why it matters:

- Eigenspaces are invariant subspaces.
- Block and Jordan forms are built by decomposing into invariant subspaces.
- Many iterative methods (Krylov subspaces) rely on this idea.

---

### 20.10 Micro-Topic Coverage Checklist for Linear Transformations ✅

- 🔹 Definition and linearity test
- 🔹 Kernel and image
- 🔹 Rank-nullity theorem for transformations
- 🔹 Matrix representation in standard and custom bases
- 🔹 Injection, surjection, bijection conditions
- 🔹 Composition and inverse transformations
- 🔹 Standard geometric transformations in $\mathbb{R}^2$
- 🔹 Invariant subspaces
- 🔹 GATE trap: translation is not linear because zero is not preserved

### 20.11 Worked Example Bank: Linear Transformations 🎯

**Example 1 (Hard):** Is $T(x,y)=(x+y,y)$ linear?

**Solution:** Yes. It is represented by matrix
$$\begin{bmatrix}1&1\\0&1\end{bmatrix}$$
✅

**Example 2 (Hard):** Is $T(x,y)=(x+1,y)$ linear?

**Solution:** No, since $T(0,0)=(1,0)\ne(0,0)$. ✅

**Example 3 (Hard):** If $T:\mathbb{R}^3\to\mathbb{R}^2$ has rank 2, what is nullity?

**Solution:** $3=2+\text{nullity}$, so nullity $=1$. ✅

### 20.12 PYQ-Style Hard Traps: Linear Transformations 🔥

**Q1. Difficulty: Hard** 😵
Does linearity imply $T(0)=0$?

**Answer:** Yes. Always. ✅

**Q2. Difficulty: Hard** 😵
If ker$(T)=\{0\}$, what property holds?

**Answer:** $T$ is injective. ✅

**Q3. Difficulty: Hard** 😵
If im$(T)=W$, what property holds?

**Answer:** $T$ is onto $W$. ✅

**Q4. Difficulty: Hard** 😵
If $T$ is bijective on finite-dimensional spaces of equal dimension, does $T^{-1}$ exist and is it linear?

**Answer:** Yes. ✅

**Q5. Difficulty: Hard** 😵
If $T_2\circ T_1$ is represented by $BA$, which map acts first?

**Answer:** $T_1$ acts first, then $T_2$. ✅

**Q6. Difficulty: Hard** 😵
Can differentiation on $P_n$ be a linear transformation?

**Answer:** Yes. It preserves addition and scalar multiplication. ✅

---

## Chapter 21: Similar Matrices and Diagonalization (⭐⭐ VERY IMPORTANT)

### 21.1 Similar Matrices

$A$ and $B$ are **similar** if there exists an invertible $P$ such that:
$$B = P^{-1}AP$$

**Properties of similar matrices (they share):**
- Same eigenvalues (same characteristic polynomial)
- Same determinant
- Same trace
- Same rank
- Same nullity

### 21.2 Diagonalization

A matrix $A$ is **diagonalizable** if it is similar to a diagonal matrix:

$$D = P^{-1}AP \quad\text{or equivalently}\quad A = PDP^{-1}$$

where:
- $D$ = diagonal matrix of eigenvalues
- $P$ = matrix whose columns are corresponding eigenvectors

### 21.3 When is a Matrix Diagonalizable?

| Condition | Diagonalizable? |
|---|---|
| All $n$ eigenvalues are distinct | **Always yes** |
| Repeated eigenvalues but GM = AM for each | **Yes** |
| Any eigenvalue has GM < AM | **No** |
| Real symmetric matrix | **Always yes** |

### 21.4 Power of Diagonalization

If $A = PDP^{-1}$:

$$A^k = PD^kP^{-1}$$

where $D^k = \text{diag}(\lambda_1^k, \lambda_2^k, \ldots, \lambda_n^k)$

This is incredibly useful for computing large powers of matrices!

### 21.5 Geometric Interpretation

Diagonalization decomposes a linear transformation into:
1. Change to eigenvector basis ($P^{-1}$)
2. Scale along each eigenvector direction ($D$)
3. Change back to original basis ($P$)

---

**Example (GATE Level):** Diagonalize $A = \begin{bmatrix} 2 & 1 \\ 0 & 3 \end{bmatrix}$.

Eigenvalues: $\lambda_1 = 2, \lambda_2 = 3$ (from triangular matrix — diagonal entries)

For $\lambda_1 = 2$: $(A-2I)\vec{v} = \begin{bmatrix} 0&1\\0&1 \end{bmatrix}\vec{v} = \vec{0}$ → $v_2 = 0$ → $\vec{v}_1 = \begin{bmatrix} 1\\0 \end{bmatrix}$

For $\lambda_2 = 3$: $(A-3I)\vec{v} = \begin{bmatrix} -1&1\\0&0 \end{bmatrix}\vec{v} = \vec{0}$ → $v_1 = v_2$ → $\vec{v}_2 = \begin{bmatrix} 1\\1 \end{bmatrix}$

$$P = \begin{bmatrix} 1&1\\0&1 \end{bmatrix}, \quad D = \begin{bmatrix} 2&0\\0&3 \end{bmatrix}$$

$$A^{100} = P\begin{bmatrix} 2^{100}&0\\0&3^{100} \end{bmatrix}P^{-1}$$

### 21.6 Orthogonal Diagonalization (⭐⭐ CRITICAL for Symmetric Matrices) 🌟

A symmetric matrix can be diagonalized using an **orthogonal** matrix $Q$ (instead of just invertible $P$):

$$A = Q\Lambda Q^T \quad (Q^{-1} = Q^T)$$

**Method:**
1. Find all eigenvalues of $A$
2. Find eigenvectors for each eigenvalue
3. If eigenvalue is repeated → apply **Gram-Schmidt** to eigenvectors of that eigenvalue to make them orthogonal
4. Normalize all eigenvectors to unit length
5. Form $Q$ from these orthonormal eigenvectors

> 🎯 **Key difference from regular diagonalization:** Regular uses $P^{-1}AP = D$ (need to compute $P^{-1}$). Orthogonal diag uses $Q^TAQ = \Lambda$ (just transpose! Much easier!)

**Example:** Orthogonally diagonalize $A = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$

$\lambda_1 = 3$: eigenvector $\vec{v}_1 = \begin{bmatrix} 1\\1 \end{bmatrix}$, normalized: $\vec{q}_1 = \frac{1}{\sqrt{2}}\begin{bmatrix} 1\\1 \end{bmatrix}$

$\lambda_2 = 1$: eigenvector $\vec{v}_2 = \begin{bmatrix} -1\\1 \end{bmatrix}$, normalized: $\vec{q}_2 = \frac{1}{\sqrt{2}}\begin{bmatrix} -1\\1 \end{bmatrix}$

$$Q = \frac{1}{\sqrt{2}}\begin{bmatrix} 1&-1\\1&1 \end{bmatrix}, \quad A = Q\begin{bmatrix} 3&0\\0&1 \end{bmatrix}Q^T$$ ✅

### 21.7 Jordan Normal Form (⭐ Awareness Level) 🧩

> 💡 **When a matrix is NOT diagonalizable, the "next best thing" is Jordan form.**

If $A$ is not diagonalizable (GM < AM for some eigenvalue), it can still be reduced to **Jordan Normal Form**:

$$A = PJP^{-1}$$

where $J$ is block diagonal with **Jordan blocks**:

$$J_k(\lambda) = \begin{bmatrix} \lambda & 1 & 0 & \cdots & 0 \\ 0 & \lambda & 1 & \cdots & 0 \\ \vdots & & \ddots & \ddots & \vdots \\ 0 & 0 & \cdots & \lambda & 1 \\ 0 & 0 & \cdots & 0 & \lambda \end{bmatrix}_{k \times k}$$

**Key facts for GATE:**

| Fact | Detail |
|---|---|
| Number of Jordan blocks for $\lambda$ | = GM($\lambda$) (geometric multiplicity) |
| Sum of sizes of blocks for $\lambda$ | = AM($\lambda$) (algebraic multiplicity) |
| Matrix is diagonalizable | ↔ all Jordan blocks are $1 \times 1$ |
| Minimal polynomial | degree = size of **largest** Jordan block |

> 🎯 **GATE typically doesn't ask you to COMPUTE Jordan form — but may ask: "Is this matrix diagonalizable?" or "What is the minimal polynomial?" where Jordan form understanding helps.**

### 21.8 Schur Decomposition (Advanced Extension) 🔷

For any complex square matrix $A$, there exists unitary $Q$ such that:

$$A = QTQ^*$$

where $T$ is upper triangular with eigenvalues on its diagonal.

For real matrices, real Schur form gives quasi-upper-triangular blocks (1x1 and 2x2). This is the practical numerical route before eigenvalue routines.

---

### 21.9 Micro-Topic Coverage Checklist for Similarity and Diagonalization ✅

- 🔹 Similar matrices and invariant quantities
- 🔹 Diagonalization criterion via eigenvector count
- 🔹 Distinct eigenvalues imply diagonalizability
- 🔹 Repeated eigenvalues need GM = AM
- 🔹 Orthogonal diagonalization for symmetric matrices
- 🔹 Jordan form awareness when diagonalization fails
- 🔹 Schur form as numerical viewpoint
- 🔹 Power computation using $A=PDP^{-1}$
- 🔹 Minimal polynomial link to diagonalizability
- 🔹 GATE trap: repeated eigenvalues do not automatically imply non-diagonalizable matrix

### 21.10 Worked Example Bank: Diagonalization 🎯

**Example 1 (Hard):** Is
$$A=\begin{bmatrix}4&1\\0&4\end{bmatrix}$$
diagonalizable?

**Solution:** Characteristic polynomial is $(\lambda-4)^2$. For $\lambda=4$,
$$A-4I=\begin{bmatrix}0&1\\0&0\end{bmatrix}$$
Eigenspace is 1-dimensional, so GM $<2$. Not diagonalizable. ✅

**Example 2 (Hard):** If a $3\times 3$ matrix has distinct eigenvalues, how many LI eigenvectors exist?

**Solution:** Three. Hence the matrix is diagonalizable. ✅

**Example 3 (Hard):** Why are symmetric matrices orthogonally diagonalizable?

**Solution:** By the spectral theorem, they admit an orthonormal eigenbasis. ✅

### 21.11 PYQ-Style Hard Traps: Diagonalization 🔥

**Q1. Difficulty: Hard** 😵
Do similar matrices have the same eigenvalues?

**Answer:** Yes. They have the same characteristic polynomial. ✅

**Q2. Difficulty: Hard** 😵
Do similar matrices have the same eigenvectors?

**Answer:** Not necessarily in the same coordinates. ✅

**Q3. Difficulty: Hard** 😵
If a matrix has repeated eigenvalue, must it fail to diagonalize?

**Answer:** No. Need to compare GM and AM. ✅

**Q4. Difficulty: Hard** 😵
If a real matrix is symmetric, can it be diagonalized by a non-orthogonal matrix only?

**Answer:** No. It can be diagonalized orthogonally. ✅

**Q5. Difficulty: Hard** 😵
What is the quickest way to compute $A^{50}$ once $A=PDP^{-1}$ is known?

**Answer:** Use $A^{50}=PD^{50}P^{-1}$. ✅

**Q6. Difficulty: Hard** 😵
If the minimal polynomial has repeated linear factor, can the matrix be diagonalizable?

**Answer:** No. ✅

**Q7. Difficulty: Hard** 😵
What do all Jordan blocks become when a matrix is diagonalizable?

**Answer:** All Jordan blocks are $1\times 1$. ✅

**Q8. Difficulty: Hard** 😵
Does orthogonal diagonalization require symmetry over the reals?

**Answer:** Yes, in the real case. ✅

---

## Chapter 22: Projection and Gram-Schmidt Process

### 22.1 Orthogonal Projection onto a Vector

Projection of $\vec{b}$ onto $\vec{a}$:

$$\text{proj}_{\vec{a}}(\vec{b}) = \frac{\vec{a} \cdot \vec{b}}{\vec{a} \cdot \vec{a}} \vec{a} = \frac{\vec{a}^T\vec{b}}{\vec{a}^T\vec{a}} \vec{a}$$

**Projection matrix** (onto line through $\vec{a}$):
$$P = \frac{\vec{a}\vec{a}^T}{\vec{a}^T\vec{a}}$$

### 22.2 Orthogonal Projection onto a Subspace

Projection of $\vec{b}$ onto the column space of $A$:

$$\hat{b} = A(A^TA)^{-1}A^T\vec{b}$$

**Projection matrix:**
$$P = A(A^TA)^{-1}A^T$$

**Error vector:** $\vec{e} = \vec{b} - \hat{b}$ is orthogonal to the column space.

### 22.3 Properties of Projection Matrices

| Property | Detail |
|---|---|
| $P^2 = P$ | Idempotent (projecting twice = projecting once) |
| $P^T = P$ | Symmetric |
| Eigenvalues of $P$ | Only 0 and 1 |
| $\text{rank}(P) = \text{trace}(P)$ | Since idempotent |
| $I - P$ | Projects onto the orthogonal complement |

### 22.4 Gram-Schmidt Process

Converts a set of LI vectors into an **orthogonal** (or orthonormal) set.

Given LI vectors $\vec{v}_1, \vec{v}_2, \ldots, \vec{v}_k$:

**Step 1:** $\vec{u}_1 = \vec{v}_1$

**Step 2:** $\vec{u}_2 = \vec{v}_2 - \frac{\vec{v}_2 \cdot \vec{u}_1}{\vec{u}_1 \cdot \vec{u}_1}\vec{u}_1$

**Step 3:** $\vec{u}_3 = \vec{v}_3 - \frac{\vec{v}_3 \cdot \vec{u}_1}{\vec{u}_1 \cdot \vec{u}_1}\vec{u}_1 - \frac{\vec{v}_3 \cdot \vec{u}_2}{\vec{u}_2 \cdot \vec{u}_2}\vec{u}_2$

**General:** $\vec{u}_k = \vec{v}_k - \sum_{j=1}^{k-1} \frac{\vec{v}_k \cdot \vec{u}_j}{\vec{u}_j \cdot \vec{u}_j}\vec{u}_j$

To get **orthonormal** vectors: $\hat{u}_i = \frac{\vec{u}_i}{\|\vec{u}_i\|}$

### 22.5 QR Decomposition

Gram-Schmidt gives us $A = QR$ where:
- $Q$ = orthogonal matrix (columns are orthonormal)
- $R$ = upper triangular

### 22.5A Modified Gram-Schmidt and Householder QR (Numerical micro-topics) 🧮

- Classical Gram-Schmidt can lose orthogonality in finite precision.
- Modified Gram-Schmidt is more stable.
- Householder QR is the most common stable implementation in scientific libraries.

For GATE DA interviews and practical exams, knowing this stability distinction is a strong differentiator.

### 22.6 Least Squares — Detailed Treatment (⭐⭐ GATE CS & DA) 📈

> 💡 **Least squares is the bridge between Linear Algebra and Machine Learning/Statistics!**

**Problem:** When $A\vec{x} = \vec{b}$ has **no exact solution** (inconsistent system), find $\hat{x}$ that **minimizes** the error:
$$\min_{\vec{x}} \|A\vec{x} - \vec{b}\|^2$$

**Derivation (Normal Equations):**
- The error $\vec{e} = \vec{b} - A\hat{x}$ must be orthogonal to the column space of $A$
- $A^T\vec{e} = \vec{0}$ → $A^T(\vec{b} - A\hat{x}) = \vec{0}$ → $A^TA\hat{x} = A^T\vec{b}$

$$\boxed{\hat{x} = (A^TA)^{-1}A^T\vec{b}}$$ (when $A$ has full column rank)

**The projection:** $\hat{b} = A\hat{x} = A(A^TA)^{-1}A^T\vec{b} = P\vec{b}$

**Least squares error:** $\|e\|^2 = \|\vec{b} - A\hat{x}\|^2 = \|\vec{b}\|^2 - \|\hat{b}\|^2$

**Application — Linear Regression (GATE DA):** 🤖

Fitting $y = c_0 + c_1 x$ to data points $(x_1, y_1), \ldots, (x_m, y_m)$:

$$A = \begin{bmatrix} 1 & x_1 \\ 1 & x_2 \\ \vdots & \vdots \\ 1 & x_m \end{bmatrix}, \quad \vec{b} = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_m \end{bmatrix}, \quad \hat{x} = \begin{bmatrix} c_0 \\ c_1 \end{bmatrix}$$

Solve $A^TA\hat{x} = A^T\vec{b}$ to get the best-fit line coefficients!

**Example:** Fit $y = c_0 + c_1 x$ to points $(0, 1), (1, 3), (2, 4)$.

$A = \begin{bmatrix} 1&0\\1&1\\1&2 \end{bmatrix}, \vec{b} = \begin{bmatrix} 1\\3\\4 \end{bmatrix}$

$A^TA = \begin{bmatrix} 3&3\\3&5 \end{bmatrix}, A^T\vec{b} = \begin{bmatrix} 8\\11 \end{bmatrix}$

$\hat{x} = (A^TA)^{-1}A^T\vec{b} = \frac{1}{6}\begin{bmatrix} 5&-3\\-3&3 \end{bmatrix}\begin{bmatrix} 8\\11 \end{bmatrix} = \frac{1}{6}\begin{bmatrix} 7\\9 \end{bmatrix} = \begin{bmatrix} 7/6 \\ 3/2 \end{bmatrix}$

Best fit line: $y = \frac{7}{6} + \frac{3}{2}x$ ✅

---

**Example:** Apply Gram-Schmidt to $\vec{v}_1 = \begin{bmatrix} 1\\1\\0 \end{bmatrix}, \vec{v}_2 = \begin{bmatrix} 1\\0\\1 \end{bmatrix}$.

$\vec{u}_1 = \begin{bmatrix} 1\\1\\0 \end{bmatrix}$

$\vec{u}_2 = \vec{v}_2 - \frac{\vec{v}_2 \cdot \vec{u}_1}{\vec{u}_1 \cdot \vec{u}_1}\vec{u}_1 = \begin{bmatrix} 1\\0\\1 \end{bmatrix} - \frac{1}{2}\begin{bmatrix} 1\\1\\0 \end{bmatrix} = \begin{bmatrix} 1/2\\-1/2\\1 \end{bmatrix}$

Check: $\vec{u}_1 \cdot \vec{u}_2 = 1/2 - 1/2 + 0 = 0$ ✓ (orthogonal)

---

### 22.7 Micro-Topic Coverage Checklist for Projection, Gram-Schmidt, QR, Least Squares ✅

- 🔹 Projection onto a vector and onto a subspace
- 🔹 Projection matrices and their symmetry/idempotence
- 🔹 Orthogonal decomposition of vectors
- 🔹 Gram-Schmidt orthogonalization and normalization
- 🔹 QR decomposition meaning
- 🔹 Least squares via normal equations and projection view
- 🔹 Modified Gram-Schmidt and Householder awareness
- 🔹 Hat matrix and residual maker matrix
- 🔹 GATE trap: projection matrices are idempotent; orthogonal projections are additionally symmetric

### 22.8 Worked Example Bank: Projection and Gram-Schmidt 🎯

**Example 1 (Hard):** Project $b=(2,1)$ onto the line spanned by $a=(1,1)$.

**Solution:**
$$\text{proj}_a(b)=\frac{a^Tb}{a^Ta}a=\frac{3}{2}(1,1)=\left(\frac{3}{2},\frac{3}{2}\right)$$
✅

**Example 2 (Hard):** What is the error vector in least squares?

**Solution:** $e=b-\hat b$, and it is orthogonal to the column space of $A$. ✅

**Example 3 (Hard):** Why is $P=A(A^TA)^{-1}A^T$ symmetric?

**Solution:**
$$P^T=A(A^TA)^{-1}A^T=P$$
because $(A^TA)^{-1}$ is symmetric. ✅

### 22.9 PYQ-Style Hard Traps: Projection and Least Squares 🔥

**Q1. Difficulty: Hard** 😵
If $P$ is an orthogonal projection matrix, what are its eigenvalues?

**Answer:** Only $0$ and $1$. ✅

**Q2. Difficulty: Hard** 😵
If $P^2=P$ but $P^T\ne P$, is it still a projection?

**Answer:** Yes, but not an orthogonal projection. ✅

**Q3. Difficulty: Hard** 😵
Why is least squares needed?

**Answer:** Because $Ax=b$ may be inconsistent, and we minimize residual norm. ✅

**Q4. Difficulty: Hard** 😵
If columns of $Q$ are orthonormal, what is projection onto their span?

**Answer:** $QQ^T$. ✅

**Q5. Difficulty: Hard** 😵
Is Gram-Schmidt valid for dependent vectors as written?

**Answer:** No. It assumes linear independence for a full orthogonal basis output. ✅

**Q6. Difficulty: Hard** 😵
If residual is orthogonal to column space, what equation follows?

**Answer:** $A^T(A\hat x-b)=0$. ✅

**Q7. Difficulty: Hard** 😵
Is $A^TA$ always invertible?

**Answer:** No. Only when $A$ has full column rank. ✅

**Q8. Difficulty: Hard** 😵
What does the hat matrix do in regression?

**Answer:** Maps $y$ to fitted values $\hat y$. ✅

---

## Chapter 23: Singular Value Decomposition (SVD) (⭐ IMPORTANT FOR DA)

### 23.1 Statement

Every $m \times n$ matrix $A$ can be decomposed as:

$$A = U\Sigma V^T$$

where:
- $U$ ($m \times m$) = orthogonal matrix (left singular vectors) — eigenvectors of $AA^T$
- $\Sigma$ ($m \times n$) = diagonal matrix of **singular values** $\sigma_1 \geq \sigma_2 \geq \cdots \geq 0$
- $V$ ($n \times n$) = orthogonal matrix (right singular vectors) — eigenvectors of $A^TA$

### 23.2 How to Compute SVD — Step by Step

1. Compute $A^TA$ (symmetric $n \times n$ matrix)
2. Find eigenvalues $\lambda_1 \geq \lambda_2 \geq \cdots \geq \lambda_n \geq 0$ and eigenvectors of $A^TA$
3. Singular values: $\sigma_i = \sqrt{\lambda_i}$
4. $V$ = matrix of eigenvectors of $A^TA$ (normalized, orthonormal)
5. $U$: first $r$ columns from $\vec{u}_i = \frac{1}{\sigma_i}A\vec{v}_i$; remaining columns from null space of $A^T$
6. $\Sigma$ = diagonal with $\sigma_i$'s

### 23.3 Key Facts About SVD

| Fact | Detail |
|---|---|
| $\text{rank}(A)$ | = number of non-zero singular values |
| $\|A\|_2$ (spectral norm) | $= \sigma_1$ (largest singular value) |
| $\|A\|_F$ (Frobenius norm) | $= \sqrt{\sigma_1^2 + \sigma_2^2 + \cdots}$ |
| Condition number | $\kappa(A) = \sigma_1 / \sigma_r$ |
| Column space of $A$ | = span of first $r$ columns of $U$ |
| Row space of $A$ | = span of first $r$ columns of $V$ |
| Null space of $A$ | = span of last $n-r$ columns of $V$ |

### 23.4 SVD as Sum of Rank-1 Matrices

$$A = \sigma_1 \vec{u}_1 \vec{v}_1^T + \sigma_2 \vec{u}_2 \vec{v}_2^T + \cdots + \sigma_r \vec{u}_r \vec{v}_r^T$$

Each term $\sigma_i \vec{u}_i \vec{v}_i^T$ is a rank-1 matrix. This is the foundation of **low-rank approximation** (used in data compression, PCA, recommender systems).

### 23.5 Truncated SVD (Best Rank-k Approximation)

$$A_k = \sum_{i=1}^{k} \sigma_i \vec{u}_i \vec{v}_i^T$$

By the Eckart-Young theorem, $A_k$ is the best rank-$k$ approximation to $A$ in both spectral and Frobenius norms.

### 23.6 Geometry of SVD

SVD decomposes any linear transformation into:
1. **Rotation** (by $V^T$) in domain space
2. **Scaling** (by $\Sigma$) along coordinate axes
3. **Rotation** (by $U$) in codomain space

### 23.7 Pseudoinverse (Moore-Penrose Inverse) (⭐ GATE DA) 🔧

> 💡 **The pseudoinverse generalizes the matrix inverse to ANY matrix — even non-square or singular!**

**Definition via SVD:** If $A = U\Sigma V^T$, then:
$$A^+ = V\Sigma^+ U^T$$

where $\Sigma^+$ is formed by taking the reciprocal of each non-zero entry in $\Sigma$ (and transposing).

**For full column rank** ($A$ is $m \times n$, $m \geq n$, $\text{rank} = n$):
$$A^+ = (A^TA)^{-1}A^T$$

This is the **left inverse**! And $\hat{x} = A^+\vec{b}$ gives the least squares solution.

**For full row rank** ($A$ is $m \times n$, $m \leq n$, $\text{rank} = m$):
$$A^+ = A^T(AA^T)^{-1}$$

This is the **right inverse**!

**Key properties:**
- $AA^+A = A$
- $A^+AA^+ = A^+$
- $(AA^+)^T = AA^+$
- $(A^+A)^T = A^+A$

### 23.7A Minimum-Norm and Minimum-Residual Interpretations

- Overdetermined system ($m>n$): $x^* = A^+b$ is the minimum-residual least squares solution.
- Underdetermined system ($m<n$): among infinitely many exact/approx solutions, $x^* = A^+b$ has minimum Euclidean norm.

This distinction is essential in DA contexts with high-dimensional features.

> 🏭 **Production Use:** In ML, when you have more features than data points (underdetermined system), the pseudoinverse gives the **minimum norm** solution — used in linear regression with regularization! 📊

### 23.8 Matrix Norms (⭐⭐⭐ GATE DA — EXPLICITLY IN SYLLABUS) 📏

> ⚠️ **This is explicitly listed in the GATE DA syllabus — do NOT skip this section!**

**Matrix norms measure the "size" of a matrix, just as vector norms measure the "size" of a vector.**

| Norm | Formula | Meaning |
|---|---|---|
| **Frobenius norm** $\|A\|_F$ | $\sqrt{\sum_{i,j} a_{ij}^2} = \sqrt{\text{tr}(A^TA)} = \sqrt{\sigma_1^2 + \cdots + \sigma_r^2}$ | "Entry-wise L2 norm" — squares all entries, sums, takes root |
| **Spectral norm** $\|A\|_2$ | $\sigma_1$ (largest singular value) | Maximum stretch factor of $A$ |
| **1-norm** $\|A\|_1$ | $\max_j \sum_i |a_{ij}|$ | Maximum **column** sum of absolute values |
| **$\infty$-norm** $\|A\|_\infty$ | $\max_i \sum_j |a_{ij}|$ | Maximum **row** sum of absolute values |
| **Nuclear norm** $\|A\|_*$ | $\sigma_1 + \sigma_2 + \cdots + \sigma_r$ | Sum of singular values |

> 🎯 **GATE Memory Aid:** $\|A\|_1$ = max **Column** sum (1 looks like a column!), $\|A\|_\infty$ = max **Row** sum (∞ is wide like a row!)

**Key relationships:**
- $\|A\|_2 \leq \|A\|_F \leq \sqrt{r}\|A\|_2$ (where $r$ = rank)
- $\frac{1}{\sqrt{n}}\|A\|_\infty \leq \|A\|_2 \leq \sqrt{m}\|A\|_\infty$
- **Submultiplicativity:** $\|AB\| \leq \|A\| \cdot \|B\|$ (for any matrix norm)
- $\|A\|_F^2 = \sum \sigma_i^2 = \text{tr}(A^TA)$

**Condition Number:** $\kappa(A) = \|A\| \cdot \|A^{-1}\| = \frac{\sigma_1}{\sigma_n}$ (for spectral norm)
- $\kappa(A)$ near 1 → well-conditioned (small input changes → small output changes)
- $\kappa(A) \gg 1$ → ill-conditioned (numerically unstable!)

**Example:** $A = \begin{bmatrix} 3 & -1 \\ 2 & 4 \end{bmatrix}$

- $\|A\|_1 = \max(|3|+|2|, |-1|+|4|) = \max(5, 5) = 5$ (max column sum)
- $\|A\|_\infty = \max(|3|+|-1|, |2|+|4|) = \max(4, 6) = 6$ (max row sum)
- $\|A\|_F = \sqrt{9 + 1 + 4 + 16} = \sqrt{30}$

---

### 23.9 Micro-Topic Coverage Checklist for SVD and Matrix Norms ✅

- 🔹 Full and reduced SVD
- 🔹 Left/right singular vectors from $AA^T$ and $A^TA$
- 🔹 Singular values as square roots of eigenvalues of $A^TA$
- 🔹 Rank from non-zero singular values
- 🔹 Best rank-$k$ approximation and Eckart-Young theorem
- 🔹 Pseudoinverse and minimum-norm solution
- 🔹 Matrix norms: spectral, Frobenius, 1, infinity, nuclear
- 🔹 Condition number and ill-conditioning
- 🔹 PCA link for DA preparation
- 🔹 GATE trap: singular values are always non-negative, unlike eigenvalues

### 23.10 Worked Example Bank: SVD and Norms 🎯

**Example 1 (Hard):** If singular values are $5,2,0$, what is rank$(A)$?

**Solution:** Rank is number of non-zero singular values, so rank $=2$. ✅

**Example 2 (Hard):** If singular values are $5,2$, what is $\|A\|_2$ and $\|A\|_F$?

**Solution:**
$$\|A\|_2=5,\quad \|A\|_F=\sqrt{25+4}=\sqrt{29}$$
✅

**Example 3 (Hard):** Why is $A^+$ useful for underdetermined systems?

**Solution:** It gives the minimum-norm solution among all solutions. ✅

### 23.11 PYQ-Style Hard Traps: SVD and Norms 🔥

**Q1. Difficulty: Hard** 😵
Can SVD fail for a rectangular matrix?

**Answer:** No. Every real matrix has an SVD. ✅

**Q2. Difficulty: Hard** 😵
Are singular values the same as eigenvalues?

**Answer:** No. Singular values are non-negative square roots of eigenvalues of $A^TA$. ✅

**Q3. Difficulty: Hard** 😵
If $A$ is symmetric positive semidefinite, how do singular values relate to eigenvalues?

**Answer:** They are the same values, all non-negative. ✅

**Q4. Difficulty: Hard** 😵
What is the spectral norm of a matrix?

**Answer:** Largest singular value. ✅

**Q5. Difficulty: Hard** 😵
What is the Frobenius norm squared?

**Answer:** Sum of squares of all entries, also sum of squares of singular values. ✅

**Q6. Difficulty: Hard** 😵
If $A$ is invertible, what is condition number in 2-norm?

**Answer:** $\sigma_{\max}/\sigma_{\min}$. ✅

**Q7. Difficulty: Hard** 😵
If one singular value is zero, can the matrix be invertible?

**Answer:** No. ✅

**Q8. Difficulty: Hard** 😵
What does truncated SVD optimize?

**Answer:** Best low-rank approximation in spectral and Frobenius norms. ✅

---

## Chapter 24: Positive Definite Matrices

### 24.1 Definition

A symmetric matrix $A$ is **positive definite** if:
$$\vec{x}^T A \vec{x} > 0 \quad \text{for all } \vec{x} \neq \vec{0}$$

**Positive semi-definite:** $\vec{x}^T A \vec{x} \geq 0$ for all $\vec{x}$.

### 24.2 Tests for Positive Definiteness

| Test | Condition |
|---|---|
| **Eigenvalue test** | All eigenvalues > 0 |
| **Pivot test** | All pivots (in elimination) > 0 |
| **Leading minor test** | All leading principal minors > 0 |
| **Cholesky decomposition** | $A = LL^T$ exists with positive diagonal entries |

**Leading principal minors:** For $A = \begin{bmatrix} a & b \\ b & c \end{bmatrix}$: $a > 0$ and $ac - b^2 > 0$.

### 24.3 Properties

- $\det(A) > 0$ (product of positive eigenvalues)
- $A + B$ is positive definite if both $A$ and $B$ are
- $A^{-1}$ is positive definite
- $\text{tr}(A) > 0$
- All diagonal entries > 0
- $A = B^TB$ for some invertible $B$

### 24.4 Quadratic Forms (⭐⭐ GATE CS & DA) 📐

> 💡 **Quadratic forms connect matrices to "energy" — positive definite means energy is always positive!**

A **quadratic form** in $n$ variables is:
$$Q(\vec{x}) = \vec{x}^T A \vec{x} = \sum_{i=1}^{n}\sum_{j=1}^{n} a_{ij}x_ix_j$$

where $A$ is a symmetric matrix (we can always symmetrize: use $\frac{A + A^T}{2}$).

**For $2 \times 2$:** $Q(x_1, x_2) = ax_1^2 + 2bx_1x_2 + cx_2^2 = \begin{bmatrix} x_1 & x_2 \end{bmatrix}\begin{bmatrix} a & b \\ b & c \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$

> ⚠️ **GATE Trap:** The off-diagonal entry of the matrix is **half** the coefficient of the cross term $x_1 x_2$! If $Q = 3x_1^2 + 4x_1x_2 + 5x_2^2$, then $A = \begin{bmatrix} 3 & 2 \\ 2 & 5 \end{bmatrix}$ (NOT $\begin{bmatrix} 3 & 4 \\ 4 & 5 \end{bmatrix}$)

**Classification of Quadratic Forms:**

| Type | Condition on $A$ | Eigenvalue Condition |
|---|---|---|
| **Positive definite** | $Q(\vec{x}) > 0$ for all $\vec{x} \neq 0$ | All $\lambda > 0$ |
| **Positive semi-definite** | $Q(\vec{x}) \geq 0$ for all $\vec{x}$ | All $\lambda \geq 0$ |
| **Negative definite** | $Q(\vec{x}) < 0$ for all $\vec{x} \neq 0$ | All $\lambda < 0$ |
| **Negative semi-definite** | $Q(\vec{x}) \leq 0$ for all $\vec{x}$ | All $\lambda \leq 0$ |
| **Indefinite** | $Q$ takes both positive and negative values | Mixed signs of $\lambda$ |

**Reducing to canonical form:** Using $\vec{x} = Q\vec{y}$ (orthogonal diagonalization):
$$Q(\vec{x}) = \vec{x}^TA\vec{x} = \vec{y}^T(Q^TAQ)\vec{y} = \vec{y}^T\Lambda\vec{y} = \lambda_1 y_1^2 + \lambda_2 y_2^2 + \cdots + \lambda_n y_n^2$$

> 🏭 **GATE DA Link:** In ML optimization, the **Hessian matrix** of a loss function is used to check convexity. If Hessian is positive definite at a point → local minimum! If indefinite → saddle point 📉

### 24.5 Sylvester's Criterion (⭐ Definite Means via Leading Minors) 🔍

For an $n \times n$ symmetric matrix $A$, let $D_k$ be the $k$-th **leading principal minor** (determinant of top-left $k \times k$ submatrix):

| Type | Condition |
|---|---|
| **Positive definite** | $D_1 > 0, D_2 > 0, \ldots, D_n > 0$ (ALL positive) |
| **Negative definite** | $D_1 < 0, D_2 > 0, D_3 < 0, \ldots$ (alternating, starting negative) |
| **Positive semi-definite** | All principal minors $\geq 0$ (NOTE: ALL principal minors, not just leading!) |

**Example:** Is $A = \begin{bmatrix} 2 & -1 & 0 \\ -1 & 2 & -1 \\ 0 & -1 & 2 \end{bmatrix}$ positive definite?

$D_1 = 2 > 0$ ✅, $D_2 = \det\begin{bmatrix} 2&-1\\-1&2 \end{bmatrix} = 3 > 0$ ✅, $D_3 = \det(A) = 4 > 0$ ✅

All leading minors positive → $A$ is positive definite! ✅

### 24.6 Hessian Test Link (Optimization Micro-topic) 📉

For a twice-differentiable scalar function $f(x)$, Hessian $H$ at a critical point determines local behavior:

- $H$ positive definite: strict local minimum
- $H$ negative definite: strict local maximum
- $H$ indefinite: saddle point

This is directly used in convex optimization and ML loss-surface analysis.

---

### 24.7 Micro-Topic Coverage Checklist for Positive Definite Matrices and Quadratic Forms ✅

- 🔹 Definition through $x^TAx$
- 🔹 Symmetry requirement in standard tests
- 🔹 Eigenvalue test, pivot test, Sylvester criterion, Cholesky test
- 🔹 Positive definite vs semidefinite vs indefinite
- 🔹 Quadratic form matrix construction from coefficients
- 🔹 Orthogonal reduction to canonical form
- 🔹 Hessian link in optimization
- 🔹 Gram matrices and covariance matrices as PSD examples
- 🔹 GATE trap: positive determinant alone is not enough for positive definiteness

### 24.8 Worked Example Bank: Positive Definiteness 🎯

**Example 1 (Hard):** Is
$$A=\begin{bmatrix}2&1\\1&2\end{bmatrix}$$
positive definite?

**Solution:** Leading minors: $2>0$, determinant $=3>0$. Hence positive definite. ✅

**Example 2 (Hard):** Classify
$$Q(x,y)=x^2-y^2$$

**Solution:** Matrix is diag$(1,-1)$, with mixed-sign eigenvalues. Indefinite. ✅

**Example 3 (Hard):** Why is covariance matrix PSD?

**Solution:** It has form $\frac{1}{n-1}X^TX$, so $z^T\Sigma z=\frac{1}{n-1}\|Xz\|^2\ge 0$. ✅

### 24.9 PYQ-Style Hard Traps: Positive Definite Matrices 🔥

**Q1. Difficulty: Hard** 😵
Can a non-symmetric matrix be positive definite under standard GATE definition?

**Answer:** Positive definiteness is usually tested through its symmetric part; standard theory focuses on symmetric matrices. ✅

**Q2. Difficulty: Hard** 😵
If all eigenvalues are positive, is the matrix positive definite?

**Answer:** Yes, for symmetric real matrices. ✅

**Q3. Difficulty: Hard** 😵
Can a PSD matrix have zero eigenvalue?

**Answer:** Yes. ✅

**Q4. Difficulty: Hard** 😵
Can a PD matrix have determinant zero?

**Answer:** No. Product of positive eigenvalues is positive. ✅

**Q5. Difficulty: Hard** 😵
Does $x^TAx>0$ for one non-zero vector imply PD?

**Answer:** No. Must hold for all non-zero vectors. ✅

**Q6. Difficulty: Hard** 😵
If Hessian is indefinite at a critical point, what type of point is it?

**Answer:** Saddle point. ✅

**Q7. Difficulty: Hard** 😵
Can all diagonal entries be positive while matrix is indefinite?

**Answer:** Yes. Positive diagonal alone is insufficient. ✅

**Q8. Difficulty: Hard** 😵
What decomposition certifies SPD efficiently for computation?

**Answer:** Cholesky decomposition. ✅

---

## Chapter 25: Block (Partition) Matrices

### 25.1 Concept

A matrix can be partitioned into submatrices (blocks):

$$A = \begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{bmatrix}$$

### 25.2 Block Multiplication

$$AB = \begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{bmatrix}\begin{bmatrix} B_{11} & B_{12} \\ B_{21} & B_{22} \end{bmatrix} = \begin{bmatrix} A_{11}B_{11} + A_{12}B_{21} & A_{11}B_{12} + A_{12}B_{22} \\ A_{21}B_{11} + A_{22}B_{21} & A_{21}B_{12} + A_{22}B_{22} \end{bmatrix}$$

**Condition:** Inner dimensions of blocks must match.

### 25.3 Block Determinant

For block triangular:
$$\det\begin{bmatrix} A & B \\ 0 & D \end{bmatrix} = \det(A)\det(D)$$

For general $2 \times 2$ block matrix (when $A$ is invertible):
$$\det\begin{bmatrix} A & B \\ C & D \end{bmatrix} = \det(A)\det(D - CA^{-1}B)$$

where $D - CA^{-1}B$ is the **Schur complement** of $A$.

### 25.4 Block Inverse

$$\begin{bmatrix} A & 0 \\ 0 & D \end{bmatrix}^{-1} = \begin{bmatrix} A^{-1} & 0 \\ 0 & D^{-1} \end{bmatrix}$$

For general block matrix $M = \begin{bmatrix} A & B \\ C & D \end{bmatrix}$ with invertible $A$ and Schur complement $S = D - CA^{-1}B$ invertible:

$$M^{-1} = \begin{bmatrix}
A^{-1} + A^{-1}BS^{-1}CA^{-1} & -A^{-1}BS^{-1} \\
-S^{-1}CA^{-1} & S^{-1}
\end{bmatrix}$$

This formula appears in Bayesian updates, Kalman filtering, and Gaussian graphical models.

---

### 25.5 Micro-Topic Coverage Checklist for Block Matrices and Schur Complement ✅

- 🔹 Block partitioning and compatible dimensions
- 🔹 Block multiplication and transpose
- 🔹 Block diagonal inverse and determinant
- 🔹 Schur complement formula
- 🔹 Block inverse conditions
- 🔹 Applications in Gaussian elimination, covariance updates, Kalman filters
- 🔹 GATE trap: block formulas are valid only when the required inverse exists

### 25.6 Worked Example Bank: Block Matrices 🎯

**Example 1 (Hard):** What is inverse of
$$\begin{bmatrix}A&0\\0&D\end{bmatrix}?$$

**Solution:**
$$\begin{bmatrix}A^{-1}&0\\0&D^{-1}\end{bmatrix}$$
provided $A,D$ are invertible. ✅

**Example 2 (Hard):** What is determinant of a block upper-triangular matrix?

**Solution:** Product of determinants of diagonal blocks. ✅

**Example 3 (Hard):** Why is Schur complement useful?

**Solution:** It reduces determinant and inverse questions on a big block matrix to smaller matrices. ✅

### 25.7 PYQ-Style Hard Traps: Block Matrices 🔥

**Q1. Difficulty: Hard** 😵
Can blocks be multiplied just because the whole matrix sizes look compatible?

**Answer:** No. Internal block dimensions must match too. ✅

**Q2. Difficulty: Hard** 😵
Does $\det\begin{bmatrix}A&B\\C&D\end{bmatrix}=\det(A)\det(D)-\det(B)\det(C)$ hold in general?

**Answer:** No. That shortcut is false in general. ✅

**Q3. Difficulty: Hard** 😵
When is Schur complement of $A$ defined as $D-CA^{-1}B$?

**Answer:** When $A$ is invertible. ✅

**Q4. Difficulty: Hard** 😵
Can block diagonalization simplify repeated solves?

**Answer:** Yes. Independent block systems can be solved separately. ✅

**Q5. Difficulty: Hard** 😵
Do block formulas require commutativity of blocks?

**Answer:** No, but order must be preserved carefully. ✅

**Q6. Difficulty: Hard** 😵
If one diagonal block is singular, is the whole block diagonal matrix invertible?

**Answer:** No. ✅

---

## Chapter 26: Projection Matrices (Advanced)

### 26.1 Projection Matrix onto Column Space

$$P = A(A^TA)^{-1}A^T$$

### 26.2 For Orthonormal Basis

If columns of $Q$ are orthonormal: $Q^TQ = I$, so:

$$P = QQ^T$$

(Much simpler! No inverse needed.)

### 26.3 Orthogonal Decomposition

Any vector $\vec{b} \in \mathbb{R}^m$ can be decomposed as:

$$\vec{b} = \hat{b} + \vec{e}$$

where $\hat{b} = P\vec{b}$ is in the column space and $\vec{e} = (I-P)\vec{b}$ is in the left null space.

### 26.4 Least Squares (Connection)

When $A\vec{x} = \vec{b}$ has no exact solution, the **least squares solution** minimizes $\|A\vec{x} - \vec{b}\|^2$:

$$A^TA\hat{x} = A^T\vec{b}$$

$$\hat{x} = (A^TA)^{-1}A^T\vec{b}$$

The projection $\hat{b} = A\hat{x}$ is the closest point in $C(A)$ to $\vec{b}$.

### 26.5 Hat Matrix in Regression (DA Micro-topic) 🎩

In linear regression, the projection matrix

$$H = X(X^TX)^{-1}X^T$$

is called the **hat matrix** because it maps $y$ to predictions $\hat{y}$:

$$\hat{y} = Hy$$

Micro-facts:

- $H$ is symmetric and idempotent
- diagonal entries $h_{ii}$ are leverage scores
- residual maker matrix: $M = I - H$

### 26.6 Oblique Projections (Extension) 🧭

Not all projections are orthogonal. Oblique projection satisfies $P^2=P$ but may have $P^T \ne P$. Orthogonal projection is the special symmetric case.

This distinction appears in generalized least squares and constrained estimation.

---

### 26.7 Micro-Topic Coverage Checklist for Projection Matrices ✅

- 🔹 Line and subspace projection matrices
- 🔹 Symmetric idempotent characterization of orthogonal projections
- 🔹 Complementary projector $I-P$
- 🔹 Hat matrix and residual maker matrix
- 🔹 Leverage scores and diagnostics awareness
- 🔹 Oblique projections versus orthogonal projections
- 🔹 GATE trap: every idempotent matrix is a projection, but not every projection is orthogonal

### 26.8 Worked Example Bank: Projection Matrices 🎯

**Example 1 (Hard):** If $P$ projects onto span of unit vector $u$, what is $P$?

**Solution:** $P=uu^T$. ✅

**Example 2 (Hard):** If $P$ is projection matrix, what is projection onto orthogonal complement?

**Solution:** $I-P$. ✅

**Example 3 (Hard):** Why is $H=X(X^TX)^{-1}X^T$ idempotent?

**Solution:**
$$H^2=X(X^TX)^{-1}X^TX(X^TX)^{-1}X^T=H$$
✅

### 26.9 PYQ-Style Hard Traps: Projection Matrices 🔥

**Q1. Difficulty: Hard** 😵
If $P^2=P$, must $P$ be symmetric?

**Answer:** No. Symmetry is special to orthogonal projection. ✅

**Q2. Difficulty: Hard** 😵
What are possible eigenvalues of any projection matrix?

**Answer:** Only $0$ and $1$. ✅

**Q3. Difficulty: Hard** 😵
If $P$ is orthogonal projection, what identity links range and null spaces?

**Answer:** Range$(P)$ is orthogonal to Null$(P)$. ✅

**Q4. Difficulty: Hard** 😵
Can projection increase Euclidean norm in the orthogonal case?

**Answer:** No. Orthogonal projection is a contraction or equal on the target subspace. ✅

**Q5. Difficulty: Hard** 😵
If $P$ projects onto a 3-dimensional subspace, what is trace$(P)$?

**Answer:** $3$. ✅

**Q6. Difficulty: Hard** 😵
What does leverage score correspond to in regression?

**Answer:** A diagonal entry of the hat matrix. ✅

---

# GATE PYQ TOPIC MAP (Year-wise Frequency Analysis) 📊

> 💡 **Study smarter, not harder! Focus more time on topics that appear repeatedly.**

| Topic | Frequency | Years |
|---|---|---|
| **Eigenvalues & Eigenvectors** | ⭐⭐⭐⭐⭐ | Almost every year |
| **Systems of Linear Equations (rank-based)** | ⭐⭐⭐⭐⭐ | Almost every year |
| **Determinants** | ⭐⭐⭐⭐ | 2017, 2018, 2019, 2020, 2021, 2023, 2024 |
| **Matrix Rank** | ⭐⭐⭐⭐ | 2016, 2017, 2019, 2020, 2022, 2024 |
| **Cayley-Hamilton Theorem** | ⭐⭐⭐⭐ | 2015, 2016, 2018, 2020, 2023, 2025 |
| **Diagonalization** | ⭐⭐⭐ | 2016, 2018, 2020, 2023, 2024 |
| **Linear Transformations** | ⭐⭐⭐ | 2017, 2019, 2021, 2023, 2025 |
| **Matrix Inverse** | ⭐⭐⭐ | 2015, 2017, 2019, 2022, 2024 |
| **Projection & Least Squares** | ⭐⭐⭐ | 2018, 2020, 2022, 2025 |
| **LU Decomposition** | ⭐⭐ | 2016, 2019, 2022 |
| **SVD** | ⭐⭐ | 2020, 2023, 2024 (DA) |
| **Positive Definite Matrices** | ⭐⭐ | 2018, 2021, 2024 (DA) |
| **Matrix Norms** | ⭐⭐ | 2023 (DA), 2024 (DA), 2025 (DA) |
| **Vector Spaces / Subspaces** | ⭐⭐ | 2015, 2017, 2021 |
| **Gram-Schmidt / QR** | ⭐ | 2019, 2022 |
| **Four Fundamental Subspaces** | ⭐ | 2020, 2024 |
| **Block Matrices** | ⭐ | 2017, 2021 |

> 🎯 **GATE CS Priority:** Eigenvalues > Systems of equations > Determinants > Rank > CH Theorem > Diagonalization
> 🎯 **GATE DA Extra Focus:** Matrix Norms, SVD, Positive Definite, Least Squares, Pseudoinverse

---

# GATE DA — SPECIFIC FOCUS TOPICS 🎯

> ⚠️ **GATE DA syllabus adds "Matrix Norms" beyond the CS syllabus. The following topics have extra weight for DA candidates.**

### DA.1 Covariance Matrix & PCA Connection

The **covariance matrix** $\Sigma = \frac{1}{n-1}X^TX$ (where $X$ is mean-centered data) is:
- Symmetric → real eigenvalues, orthogonal eigenvectors
- Positive semi-definite → all eigenvalues $\geq 0$

**PCA = Spectral decomposition of $\Sigma$:**
- Eigenvectors = principal components (directions of maximum variance)
- Eigenvalues = variance along each principal component
- Keep top $k$ eigenvectors for dimensionality reduction

### DA.2 Matrix Norms in Data Science

| Context | Norm Used | Why |
|---|---|---|
| Regularization (Ridge/L2) | $\|w\|_2^2 = w^Tw$ | Shrinks weights, prevents overfitting |
| Regularization (Lasso/L1) | $\|w\|_1$ | Sparsity — forces some weights to zero |
| Low-rank approximation error | $\|A - A_k\|_F$ or $\|A - A_k\|_2$ | Frobenius or spectral norm error |
| Condition number | $\kappa(A) = \sigma_1/\sigma_r$ | Numerical stability check |
| Nuclear norm minimization | $\|A\|_*$ | Matrix completion (Netflix problem!) |

### DA.3 Kernel Methods Connection

In SVM and kernel methods:
- **Kernel matrix** $K_{ij} = \langle \phi(x_i), \phi(x_j) \rangle$ must be positive semi-definite (Mercer's condition)
- Eigendecomposition of $K$ → kernel PCA
- SVD of data matrix → latent factor analysis

---

# EXPANDED QUICK REFERENCE: MASTER FORMULA CARD 📋

## Vectors & Norms

| Formula | Name |
|---|---|
| $\vec{u} \cdot \vec{v} = \sum u_iv_i = \|\vec{u}\|\|\vec{v}\|\cos\theta$ | Dot product |
| $\|\vec{v}\| = \sqrt{\sum v_i^2}$ | Euclidean norm |
| $\|\vec{v}\|_p = \left(\sum |v_i|^p\right)^{1/p}$ | p-norm |
| $|\vec{u} \cdot \vec{v}| \leq \|\vec{u}\|\|\vec{v}\|$ | Cauchy-Schwarz |
| $\cos\theta = \frac{\vec{u} \cdot \vec{v}}{\|\vec{u}\|\|\vec{v}\|}$ | Cosine similarity |

## Matrices & Determinants

| Formula | Name |
|---|---|
| $\det(AB) = \det(A)\det(B)$ | Product rule |
| $\det(A^{-1}) = 1/\det(A)$ | Inverse rule |
| $\det(kA) = k^n\det(A)$ | Scalar rule ($n \times n$) |
| $\det(A^T) = \det(A)$ | Transpose rule |
| $A^{-1} = \frac{1}{\det(A)}\text{adj}(A)$ | Inverse via adjoint |

## Eigenvalues

| Formula | Name |
|---|---|
| $\sum \lambda_i = \text{tr}(A)$ | Trace = sum of eigenvalues |
| $\prod \lambda_i = \det(A)$ | Det = product of eigenvalues |
| $\lambda^2 - \text{tr}(A)\lambda + \det(A) = 0$ | Char. eq. for $2 \times 2$ |
| $\lambda(A^{-1}) = 1/\lambda(A)$ | Inverse eigenvalues |
| $\lambda(A^k) = \lambda(A)^k$ | Power eigenvalues |
| $\lambda(A + cI) = \lambda(A) + c$ | Shift eigenvalues |

## Decompositions

| Formula | Name |
|---|---|
| $A = PDP^{-1}$ | Diagonalization |
| $A = Q\Lambda Q^T$ ($A$ symmetric) | Spectral decomposition |
| $A = U\Sigma V^T$ | SVD |
| $A = LU$ | LU decomposition |
| $A = LL^T$ ($A$ SPD) | Cholesky |
| $A = QR$ | QR decomposition |

## Systems & Rank

| Formula | Name |
|---|---|
| $\text{rank}(A) + \text{nullity}(A) = n$ | Rank-Nullity |
| $\text{rank}(AB) \leq \min(\text{rank}(A), \text{rank}(B))$ | Rank inequality |
| $\text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)$ | Subadditivity |
| $\text{rank}(A^TA) = \text{rank}(A)$ | Key identity |
| $\hat{x} = (A^TA)^{-1}A^T\vec{b}$ | Least squares |
| $P = A(A^TA)^{-1}A^T$ | Projection matrix |

## Matrix Norms (GATE DA)

| Formula | Name |
|---|---|
| $\|A\|_F = \sqrt{\text{tr}(A^TA)} = \sqrt{\sum \sigma_i^2}$ | Frobenius norm |
| $\|A\|_2 = \sigma_1$ | Spectral norm |
| $\|A\|_1 = \max_j \sum_i |a_{ij}|$ | Max column sum |
| $\|A\|_\infty = \max_i \sum_j |a_{ij}|$ | Max row sum |
| $\kappa(A) = \sigma_1/\sigma_r$ | Condition number |

---

# GATE TRAPS & PITFALLS — MASTER LIST ⚠️

> 🎯 **Review this list 1 week before GATE — these are the most common ways students lose marks!**

| # | Trap | Correct Understanding |
|---|---|---|
| 1 | $AB = BA$ | Matrix multiplication is NOT commutative |
| 2 | $AB = 0 \implies A=0$ or $B=0$ | **Wrong!** Both can be non-zero |
| 3 | $\det(A+B) = \det(A) + \det(B)$ | **Wrong!** No such formula |
| 4 | $\det(kA) = k\det(A)$ | **Wrong!** $\det(kA) = k^n\det(A)$ |
| 5 | Row ops preserve column space | **Wrong!** Row ops preserve row space, NOT column space |
| 6 | Eigenvectors can be $\vec{0}$ | **Wrong!** Eigenvectors are non-zero by definition |
| 7 | $A^2 = A \implies A = I$ or $A = 0$ | **Wrong!** Any idempotent matrix works |
| 8 | Distinct eigenvalues → symmetric | **Wrong!** Distinct eigenvalues → diagonalizable (not necessarily symmetric) |
| 9 | $\text{rank}(AB) = \text{rank}(A) \cdot \text{rank}(B)$ | **Wrong!** $\text{rank}(AB) \leq \min(\text{rank}(A), \text{rank}(B))$ |
| 10 | $A^T A = A A^T$ always | Only for **normal** matrices |
| 11 | $A$ and $A^T$ have same eigenvectors | Same eigenvalues, but generally **different** eigenvectors |
| 12 | Forgetting $\text{rank}([A|b])$ vs $\text{rank}(A)$ check | Must check augmented matrix for consistency |
| 13 | For symmetric $A$, eigenvectors for same $\lambda$ are automatically orthogonal | Only for **distinct** eigenvalues; for same $\lambda$, use Gram-Schmidt |
| 14 | SVD only for square matrices | SVD exists for **ANY** matrix (any size) |
| 15 | $e^{A+B} = e^A e^B$ | Only when $AB = BA$! |
| 16 | $(AB)^T = A^TB^T$ | **Wrong!** $(AB)^T = B^TA^T$ (reverse order!) |
| 17 | $(AB)^{-1} = A^{-1}B^{-1}$ | **Wrong!** $(AB)^{-1} = B^{-1}A^{-1}$ (reverse order!) |
| 18 | Rank of original matrix = number of non-zero rows | Must **row reduce** first! |
| 19 | Confusing AM and GM for eigenvalues | AM $\geq$ GM always; they can differ |
| 20 | $\text{adj}(A) = 0 \implies A = 0$ | **Wrong!** $\text{rank}(\text{adj}(A))$ depends on $\text{rank}(A)$ |
| 21 | Quadratic form $ax^2 + bxy + cy^2$ → matrix has $b$ as off-diagonal | **Wrong!** Off-diagonal is $b/2$ (symmetrize!) |
| 22 | All minors $\geq 0$ → positive semi-definite | Need ALL principal minors $\geq 0$, not just leading |
| 23 | $\|A\|_1$ = max row sum | **Wrong!** $\|A\|_1$ = max **column** sum, $\|A\|_\infty$ = max row sum |
| 24 | Cholesky decomposition exists for any symmetric matrix | Only for symmetric **positive definite** matrices |
| 25 | If $A$ is idempotent, $\text{rank}(A) = n$ | **Wrong!** $\text{rank}(A) = \text{tr}(A)$, can be any value $0$ to $n$ |

---

# QUICK SOLVER STRATEGIES FOR GATE 🏃‍♂️

### Strategy 1: 2×2 Matrix Speed Tricks

For $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$:
- **Eigenvalues:** $\lambda = \frac{(a+d) \pm \sqrt{(a-d)^2 + 4bc}}{2}$ or use $\lambda^2 - \text{tr}(A)\lambda + \det(A) = 0$
- **Inverse:** $A^{-1} = \frac{1}{ad-bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$ (swap diagonal, negate off-diagonal)
- **$A^n$ via CH:** Express $A^n = \alpha A + \beta I$ using Cayley-Hamilton

### Strategy 2: Determinant Shortcuts

- **Triangular/Diagonal:** $\det = $ product of diagonal entries
- **One row/column has mostly zeros:** Expand along that row/column
- **Row operations:** Do $R_i \leftarrow R_i - kR_j$ to create zeros, then expand
- **$2 \times 2$ blocks:** Use block determinant formula if structure permits
- **Eigenvalues known:** $\det = \prod \lambda_i$

### Strategy 3: Rank Determination Speed

- **Look for proportional rows/columns first** (immediate rank reduction)
- **For parametric matrices:** The determinant = 0 gives the threshold
- **Rank 1 check:** Is $A = \vec{u}\vec{v}^T$? If so, rank = 1

### Strategy 4: Eigenvalue Speed Tricks

- **Triangular/Diagonal:** Eigenvalues = diagonal entries
- **$2 \times 2$:** Use trace and det: $\lambda_1 + \lambda_2 = \text{tr}(A)$, $\lambda_1\lambda_2 = \det(A)$
- **Known matrix type:** Idempotent → {0,1}, nilpotent → {0}, involutory → {±1}, orthogonal → $|\lambda|=1$
- **Shift:** $\lambda(A + kI) = \lambda(A) + k$

### Strategy 5: System of Equations Quick Check

1. Compute $\text{rank}(A)$ and $\text{rank}([A|b])$
2. If $\text{rank}(A) \neq \text{rank}([A|b])$ → **inconsistent** (no solution)
3. If equal: unique solution when $\text{rank} = n$ (number of unknowns), infinite when $\text{rank} < n$
4. **Free variables** = $n - \text{rank}(A)$

---

# CAPSTONE 100/100 MASTERY SECTION 🏁

## A. Chapterwise Last-Minute Memory Hooks 🧠

### A1. Vectors
- Independence means no redundancy.
- Orthogonality gives easy coefficients.
- Cauchy-Schwarz and triangle inequality are default hidden tools.

### A2. Span and Basis
- Span = all reachable vectors.
- Basis = minimal spanning = maximal LI.
- Dimension is the number that never changes across bases.

### A3. Matrix Operations
- Shapes first, arithmetic second.
- Multiplication is associative, not commutative.
- Transpose and inverse reverse order.

### A4. Systems
- $Ax=b$ asks whether $b$ lies in column space.
- Homogeneous solutions form a subspace.
- Non-homogeneous solutions form an affine shift.

### A5. Elimination and Rank
- Pivots count rank.
- RREF is unique.
- Free variables count nullity.

### A6. Rank-Based Decision
- Compare rank$(A)$ with rank$([A|b])$.
- Equal ranks mean consistency.
- Equal ranks plus full column rank mean uniqueness.

### A7. Determinants
- Product rule, sign flip on swap, $k^n$ on scalar multiplication.
- Zero determinant means singularity.
- Triangular determinant is diagonal product.

### A8. Inverse
- Inverse exists iff square and nonsingular.
- Orthogonal inverse is transpose.
- Inverse of product reverses order.

### A9. Cramer's Rule
- Only for square nonsingular systems.
- Great for tiny systems, poor for large ones.
- Numerator determinant replaces one column.

### A10. Eigenvalues
- Solve $\det(A-\lambda I)=0$.
- Trace is sum, determinant is product.
- Distinct eigenvalues give LI eigenvectors.

### A11. Cayley-Hamilton
- Every square matrix satisfies its characteristic polynomial.
- Reduce high powers using CH.
- Inverse formulas can be extracted from CH.

### A12. Rank and Eigenvalues
- Zero eigenvalue means singular.
- Rank and eigenvalue counting need care.
- Singular values are often safer than eigenvalues for rank logic.

### A13. LU/Cholesky
- LU encodes elimination.
- PA=LU handles pivoting.
- Cholesky is for SPD matrices.

### A14. Special Matrices
- Symmetric gives real orthogonal eigen-structure.
- Orthogonal preserves length.
- Idempotent and nilpotent have tiny eigenvalue sets.

### A15. Vector Spaces
- Check zero, addition, scalar multiplication.
- Function and polynomial spaces matter too.
- Inner products create geometry.

### A16. Subspaces
- Homogeneous linear constraints usually define subspaces.
- Unions usually fail.
- Intersections always work.

### A17. Basis and Dimension
- Same dimension across all bases.
- Enough LI vectors gives a basis.
- Enough spanning vectors gives a basis.

### A18. Four Fundamental Subspaces
- Column space decides solvability.
- Null space decides freedom.
- Orthogonal complements organize everything.

### A19. Change of Basis
- Vectors do not change, coordinates do.
- Similar matrices are basis-changed versions.
- Transition matrix columns are new basis vectors.

### A20. Linear Transformations
- Preserve addition and scalar multiplication.
- Kernel tests injectivity.
- Image tests surjectivity.

### A21. Diagonalization
- Enough eigenvectors is the whole game.
- Symmetric matrices are the clean case.
- Jordan form explains failure.

### A22. Projection and Gram-Schmidt
- Projection error is orthogonal to target space.
- Gram-Schmidt builds orthogonal bases.
- Least squares is projection in disguise.

### A23. SVD and Norms
- Every matrix has an SVD.
- Singular values measure stretch.
- Truncated SVD powers low-rank approximation.

### A24. Positive Definite Matrices
- Check eigenvalues or leading principal minors.
- Hessian positivity signals local minima.
- Covariance matrices are PSD.

### A25. Block Matrices
- Respect block dimensions.
- Schur complement is the master shortcut.
- Block triangular determinant is easy.

### A26. Projection Matrices
- Orthogonal projector = symmetric idempotent.
- Complement is $I-P$.
- Trace equals projected dimension.

## B. Mixed Hard PYQ-Style Mega Drill 🔥

**M1. Difficulty: Hard** 😵
If $u,v,w\in\mathbb{R}^n$ are such that $u,v$ are LI and $w=u+v$, determine whether $\{u,v,w\}$ is LI.

**Answer:** Dependent, because $w-u-v=0$ is non-trivial. ✅

**M2. Difficulty: Hard** 😵
If $A$ is $3\times 5$ and rank$(A)=3$, what is dim$\,N(A)$?

**Answer:** $5-3=2$. ✅

**M3. Difficulty: Hard** 😵
If $A$ is $4\times 4$ and $\det(A)=0$, what can you conclude about eigenvalues?

**Answer:** At least one eigenvalue is $0$. ✅

**M4. Difficulty: Hard** 😵
For a symmetric matrix, eigenvectors of distinct eigenvalues are what?

**Answer:** Orthogonal. ✅

**M5. Difficulty: Hard** 😵
If $A$ is orthogonal, what is $\|Ax\|$ compared with $\|x\|$?

**Answer:** Equal. Norm preserved. ✅

**M6. Difficulty: Hard** 😵
If $A^2=A$, possible eigenvalues are what?

**Answer:** Only $0$ and $1$. ✅

**M7. Difficulty: Hard** 😵
If $A^k=0$ for some $k$, what is $\det(I-A)$?

**Answer:** $1$, since all eigenvalues of $A$ are $0$, so all eigenvalues of $I-A$ are $1$. ✅

**M8. Difficulty: Hard** 😵
If $P$ is projection matrix onto a 2D subspace of $\mathbb{R}^5$, what is rank$(P)$?

**Answer:** $2$. ✅

**M9. Difficulty: Hard** 😵
If $Q$ is orthogonal, what is $Q^{-1}$?

**Answer:** $Q^T$. ✅

**M10. Difficulty: Hard** 😵
If rank$(A)=\text{rank}([A|b])<n$, how many solutions does $Ax=b$ have?

**Answer:** Infinitely many. ✅

**M11. Difficulty: Hard** 😵
If rank$(A)<\text{rank}([A|b])$, what is the system status?

**Answer:** Inconsistent. No solution. ✅

**M12. Difficulty: Hard** 😵
If all singular values of $A$ are non-zero, what follows for a square matrix?

**Answer:** It is invertible. ✅

**M13. Difficulty: Hard** 😵
If $A$ is SPD, what decomposition exists uniquely with positive diagonal?

**Answer:** Cholesky: $A=LL^T$. ✅

**M14. Difficulty: Hard** 😵
What is the quickest determinant of an upper triangular matrix?

**Answer:** Product of diagonal entries. ✅

**M15. Difficulty: Hard** 😵
If $A$ and $B$ are similar, what are two invariants you can immediately write?

**Answer:** Same determinant and same trace; also same eigenvalues. ✅

**M16. Difficulty: Hard** 😵
If $A$ is $m\times n$ with full row rank, which space is zero: $N(A)$ or $N(A^T)$?

**Answer:** $N(A^T)=\{0\}$. ✅

**M17. Difficulty: Hard** 😵
If $A$ is $m\times n$ with full column rank, which space is zero?

**Answer:** $N(A)=\{0\}$. ✅

**M18. Difficulty: Hard** 😵
If $A=PDP^{-1}$ and $D$ is diagonal, how do you compute $f(A)$ for a polynomial $f$?

**Answer:** $f(A)=Pf(D)P^{-1}$. ✅

**M19. Difficulty: Hard** 😵
If $A^TAx=A^Tb$, what problem are you solving?

**Answer:** Least squares. ✅

**M20. Difficulty: Hard** 😵
If $A$ has singular values $7,3,1$, what is $\|A\|_2$?

**Answer:** $7$. ✅

**M21. Difficulty: Hard** 😵
If $A$ has singular values $7,3,1$, what is condition number in 2-norm?

**Answer:** $7/1=7$. ✅

**M22. Difficulty: Hard** 😵
If $A$ is row stochastic, what special eigenvalue must exist?

**Answer:** $1$. ✅

**M23. Difficulty: Hard** 😵
If $P^2=P$ and $P^T=P$, what kind of matrix is $P$?

**Answer:** Orthogonal projection matrix. ✅

**M24. Difficulty: Hard** 😵
If $A$ is skew-symmetric of odd order, what is determinant?

**Answer:** $0$. ✅

**M25. Difficulty: Hard** 😵
If $A$ is diagonalizable with eigenvalues $2,2,5$, what can you say about minimal polynomial factors?

**Answer:** It has distinct linear factors among present eigenvalues, so divides $(x-2)(x-5)$. ✅

**M26. Difficulty: Hard** 😵
If $A$ is not diagonalizable and has only eigenvalue $3$, what must be true about Jordan form?

**Answer:** At least one Jordan block for $3$ has size greater than $1$. ✅

**M27. Difficulty: Hard** 😵
If $H=X(X^TX)^{-1}X^T$, what is $I-H$ used for?

**Answer:** Residual maker matrix. ✅

**M28. Difficulty: Hard** 😵
What is the ambient space of row space for an $m\times n$ matrix?

**Answer:** $\mathbb{R}^n$. ✅

**M29. Difficulty: Hard** 😵
What is the ambient space of column space for an $m\times n$ matrix?

**Answer:** $\mathbb{R}^m$. ✅

**M30. Difficulty: Hard** 😵
If a matrix has determinant $-1$ and is orthogonal, what type of geometric action can it represent?

**Answer:** Reflection or orientation-reversing orthogonal transformation. ✅

**M31. Difficulty: Hard** 😵
If $A$ is positive semidefinite, can $x^TAx$ ever be negative?

**Answer:** No. ✅

**M32. Difficulty: Hard** 😵
If $A$ is positive definite, is $A^{-1}$ also positive definite?

**Answer:** Yes. ✅

**M33. Difficulty: Hard** 😵
If columns of $A$ are orthonormal, what is $A^TA$?

**Answer:** Identity. ✅

**M34. Difficulty: Hard** 😵
If rows of $A$ are orthonormal and $A$ is square, what is $AA^T$?

**Answer:** Identity. ✅

**M35. Difficulty: Hard** 😵
What does $\det(A)\ne 0$ say about columns of $A$?

**Answer:** They are linearly independent and form a basis for $\mathbb{R}^n$ if $A$ is $n\times n$. ✅

**M36. Difficulty: Hard** 😵
If $B=P^{-1}AP$, what connects $A$ and $B$ conceptually?

**Answer:** Same linear map in different bases. ✅

## C. Final 48-Hour Revision Ritual ⏳

1. Re-solve all six hard drills from Chapters 1, 4, 5, 7, 10, 21, 22, 23, and 24 without seeing answers.
2. Memorize the exact conditions for invertibility, consistency, diagonalizability, orthogonal diagonalizability, and positive definiteness.
3. Practice 2-minute recognition: matrix type, fastest method, likely trap.
4. Train yourself to classify every question first: determinant, rank, eigen, projection, decomposition, or basis change.
5. In the exam, do not compute before checking for structure: triangular, symmetric, rank-1, orthogonal, idempotent, nilpotent, stochastic, block, or SPD.

## D. Extra 40 Hard Booster Drills 🚀

**M37. Difficulty: Hard** 😵
If $A$ is $2\times 2$ with trace $6$ and determinant $8$, find eigenvalues.

**Hint:** Solve $\lambda^2-6\lambda+8=0$.

**Answer:** $\lambda=2,4$. ✅

**M38. Difficulty: Hard** 😵
If a square matrix has exactly one eigenvalue and is diagonalizable, what must it be similar to?

**Hint:** Diagonalizable with one repeated eigenvalue means scalar matrix.

**Answer:** $\lambda I$. ✅

**M39. Difficulty: Hard** 😵
If $A$ is $3\times 3$ and rank$(A)=1$, what are possible dimensions of null space?

**Hint:** Use rank-nullity.

**Answer:** Nullity $=2$. ✅

**M40. Difficulty: Hard** 😵
If all columns of $A$ are multiples of one vector, what is rank$(A)$?

**Hint:** Column space is one-dimensional unless all-zero.

**Answer:** Rank is $1$ if not all zero, else $0$. ✅

**M41. Difficulty: Hard** 😵
If $A$ is symmetric and $x^TAx=0$ for some non-zero $x$, can $A$ be positive definite?

**Hint:** PD requires strict positivity.

**Answer:** No. ✅

**M42. Difficulty: Hard** 😵
If $A$ is orthogonal and symmetric, what are possible eigenvalues?

**Hint:** Combine $|\lambda|=1$ with reality from symmetry.

**Answer:** Only $\pm 1$. ✅

**M43. Difficulty: Hard** 😵
If $A$ is nilpotent, what is trace$(A)$?

**Hint:** Sum of eigenvalues.

**Answer:** $0$. ✅

**M44. Difficulty: Hard** 😵
If $A$ is idempotent and invertible, what is $A$?

**Hint:** $A^2=A$ and multiply by $A^{-1}$.

**Answer:** $A=I$. ✅

**M45. Difficulty: Hard** 😵
If $A$ is involutory and symmetric, what can you say about orthogonal diagonalization?

**Hint:** Symmetric matrices diagonalize orthogonally and eigenvalues solve $\lambda^2=1$.

**Answer:** It is orthogonally diagonalizable with diagonal entries only $\pm1$. ✅

**M46. Difficulty: Hard** 😵
If $A$ is $m\times n$ and $A^TA$ is invertible, what follows about columns of $A$?

**Hint:** Nullity test.

**Answer:** Columns are linearly independent. ✅

**M47. Difficulty: Hard** 😵
If $AA^T$ is invertible, what follows about rows of $A$?

**Hint:** Row rank view.

**Answer:** Rows are linearly independent. ✅

**M48. Difficulty: Hard** 😵
If $A$ is diagonalizable and all eigenvalues are positive, is $A$ positive definite?

**Hint:** Need symmetry for standard PD conclusion in real case.

**Answer:** Not necessarily; positivity of eigenvalues alone is insufficient without symmetry. ✅

**M49. Difficulty: Hard** 😵
If $A$ is real symmetric with eigenvalues $1,2,5$, what is the smallest possible value of $x^TAx$ over unit vectors $x$?

**Hint:** Rayleigh quotient minimum is smallest eigenvalue.

**Answer:** $1$. ✅

**M50. Difficulty: Hard** 😵
If $A$ is real symmetric with eigenvalues $1,2,5$, what is the largest possible value of $x^TAx$ over unit vectors $x$?

**Hint:** Rayleigh quotient maximum.

**Answer:** $5$. ✅

**M51. Difficulty: Hard** 😵
If $A$ is $n\times n$ and $A^2=0$, what can be said about $\det(I+A)$?

**Hint:** Nilpotent eigenvalues.

**Answer:** $1$. ✅

**M52. Difficulty: Hard** 😵
If $A$ is $3\times 3$ with characteristic polynomial $(\lambda-2)^2(\lambda+1)$ and minimal polynomial $(\lambda-2)(\lambda+1)$, is $A$ diagonalizable?

**Hint:** Distinct linear factors in minimal polynomial.

**Answer:** Yes. ✅

**M53. Difficulty: Hard** 😵
If minimal polynomial is $(\lambda-2)^2(\lambda+1)$, is $A$ diagonalizable?

**Hint:** Repeated factor kills diagonalizability.

**Answer:** No. ✅

**M54. Difficulty: Hard** 😵
If $A$ is $n\times n$ and rank$(A)=n-1$, what is nullity?

**Hint:** Rank-nullity.

**Answer:** $1$. ✅

**M55. Difficulty: Hard** 😵
If rank$(A)=n-1$ for square $A$, what can you say about determinant?

**Hint:** Full rank is needed for invertibility.

**Answer:** Determinant is $0$. ✅

**M56. Difficulty: Hard** 😵
If $A$ is $n\times n$ and one row is sum of two other rows, what is $\det(A)$?

**Hint:** Row dependence.

**Answer:** $0$. ✅

**M57. Difficulty: Hard** 😵
If $A$ is $3\times 3$ orthogonal with determinant $1$, what transformation family does it represent geometrically?

**Hint:** Proper orthogonal transformation.

**Answer:** A rotation (or composition equivalent in $\mathbb{R}^3$). ✅

**M58. Difficulty: Hard** 😵
If $A$ is $3\times 3$ orthogonal with determinant $-1$, what kind of action appears?

**Hint:** Orientation reversal.

**Answer:** Reflection-type orientation-reversing orthogonal transformation. ✅

**M59. Difficulty: Hard** 😵
If $A$ is stochastic and regular, what happens to $A^k$ as $k\to\infty$?

**Hint:** Perron-Frobenius and steady state.

**Answer:** It converges to a rank-1 steady-state matrix. ✅

**M60. Difficulty: Hard** 😵
If $A=uv^T$, what is trace$(A)$?

**Hint:** Use cyclic property.

**Answer:** $v^Tu=u\cdot v$. ✅

**M61. Difficulty: Hard** 😵
If $A=uv^T$ is square and $v^Tu=0$, what is $A^2$?

**Hint:** Multiply rank-1 form carefully.

**Answer:** $A^2=0$. ✅

**M62. Difficulty: Hard** 😵
If $A$ has orthonormal columns, what kind of matrix is $P=AA^T$?

**Hint:** Subspace projector.

**Answer:** Orthogonal projection onto column space of $A$. ✅

**M63. Difficulty: Hard** 😵
If $A$ is $m\times n$ with orthonormal columns, what is $A^TA$?

**Hint:** Column orthonormality.

**Answer:** $I_n$. ✅

**M64. Difficulty: Hard** 😵
If $A$ is $m\times n$ with orthonormal rows, what is $AA^T$?

**Hint:** Row orthonormality.

**Answer:** $I_m$. ✅

**M65. Difficulty: Hard** 😵
If $A$ is symmetric and diagonalizable as $Q\Lambda Q^T$, how do you compute $A^{-1}$ when invertible?

**Hint:** Invert diagonal part only.

**Answer:** $A^{-1}=Q\Lambda^{-1}Q^T$. ✅

**M66. Difficulty: Hard** 😵
If $A$ is SPD, what can be said about all principal leading minors?

**Hint:** Sylvester criterion.

**Answer:** They are all positive. ✅

**M67. Difficulty: Hard** 😵
If $A$ is PSD, can Cholesky always be taken with full-rank triangular factor?

**Hint:** Singular case matters.

**Answer:** Not with full-rank positive diagonal form in general; standard Cholesky guarantee is for PD. ✅

**M68. Difficulty: Hard** 😵
If $A$ is diagonal with entries $3,-2,0$, what is rank$(A)$?

**Hint:** Count non-zero diagonal entries.

**Answer:** $2$. ✅

**M69. Difficulty: Hard** 😵
If $A$ is diagonal with entries $3,-2,0$, what is nullity?

**Hint:** Dimension minus rank.

**Answer:** $1$. ✅

**M70. Difficulty: Hard** 😵
If $A$ has eigenvalues $2$ and $3$, what are eigenvalues of $A+4I$?

**Hint:** Shift property.

**Answer:** $6$ and $7$. ✅

**M71. Difficulty: Hard** 😵
If $A$ has eigenvalues $2$ and $3$, what are eigenvalues of $A^{-1}$ if invertible?

**Hint:** Reciprocal rule.

**Answer:** $1/2$ and $1/3$. ✅

**M72. Difficulty: Hard** 😵
If $A$ has eigenvalues $2$ and $3$, what are eigenvalues of $A^2$?

**Hint:** Power rule.

**Answer:** $4$ and $9$. ✅

**M73. Difficulty: Hard** 😵
If a matrix is both upper triangular and orthogonal, what can its diagonal entries be?

**Hint:** Orthogonal eigenvalue magnitude and triangular diagonal-eigenvalue coincidence.

**Answer:** Only $\pm 1$, with severe restrictions on off-diagonals in the real case. ✅

**M74. Difficulty: Hard** 😵
If $A$ is diagonalizable and $A=PDP^{-1}$, what is rank$(A)$ in terms of $D$?

**Hint:** Similar matrices share rank.

**Answer:** Rank$(A)$ equals number of non-zero diagonal entries of $D$. ✅

**M75. Difficulty: Hard** 😵
If $A$ is singular, can $A^TA$ be invertible?

**Hint:** Same null space logic.

**Answer:** No. ✅

**M76. Difficulty: Hard** 😵
If $A$ is non-singular, can $A^TA$ be singular?

**Hint:** Positive definiteness of $A^TA$.

**Answer:** No. $A^TA$ is SPD for invertible real $A$. ✅

## E. Rapid Recognition Glossary ⚡

**Affine set:** A shifted subspace; typical solution form of consistent non-homogeneous systems.

**Algebraic multiplicity:** Number of times an eigenvalue occurs as root of characteristic polynomial.

**Augmented matrix:** Coefficient matrix with right-hand side column attached.

**Basis:** A linearly independent spanning set.

**Bilinear form:** A function linear in each argument separately, like $x^TAy$.

**Block matrix:** Matrix viewed as smaller matrices arranged in a grid.

**Cauchy-Schwarz:** The default inequality behind many dot-product bounds.

**Characteristic polynomial:** $\det(A-\lambda I)$.

**Column space:** All outputs $Ax$ achievable from the matrix.

**Condition number:** Sensitivity measure; larger means more numerical trouble.

**Covariance matrix:** Symmetric PSD matrix encoding variance and pairwise covariance.

**Diagonalizable:** Has a full basis of eigenvectors.

**Direct sum:** Unique decomposition into pieces from subspaces with zero intersection.

**Eigenvector:** Non-zero vector whose direction is preserved by the transformation.

**Elementary matrix:** Encodes one elementary row operation.

**Full column rank:** Columns are linearly independent.

**Full row rank:** Rows are linearly independent.

**Gershgorin disc:** Region guaranteed to contain eigenvalues.

**Gram matrix:** Matrix of pairwise inner products; always PSD.

**Hat matrix:** Regression projector mapping $y$ to fitted values $\hat y$.

**Idempotent matrix:** Satisfies $A^2=A$.

**Image:** Same as range or column space of a transformation.

**Inner product:** Generalized dot product creating length and angle.

**Jordan block:** Near-diagonal block with repeated eigenvalue and ones above diagonal.

**Kernel:** Same as null space.

**Least squares:** Best approximate solve minimizing residual norm.

**Left null space:** Null space of $A^T$.

**LU decomposition:** Triangular factorization tied to elimination.

**Minimal polynomial:** Smallest monic polynomial annihilating the matrix.

**Nilpotent matrix:** Some power becomes zero.

**Normal equations:** $A^TA\hat x=A^Tb$.

**Nullity:** Dimension of null space.

**Orthogonal matrix:** Preserves lengths and angles; inverse equals transpose.

**Orthogonal projection:** Symmetric idempotent projection.

**Perron-Frobenius:** Positive/stochastic matrices have dominant positive eigen-structure.

**Pivot:** Leading non-zero entry used in elimination.

**Positive definite:** $x^TAx>0$ for all non-zero $x$.

**Pseudoinverse:** Generalized inverse from SVD.

**Rank:** Dimension of row space equals dimension of column space.

**Rayleigh quotient:** $\frac{x^TAx}{x^Tx}$, especially useful for symmetric matrices.

**Reduced row echelon form:** Unique canonical row-reduced form.

**Schur complement:** The key smaller block after eliminating one diagonal block.

**Singular value:** Non-negative square root of eigenvalue of $A^TA$.

**Spectral norm:** Largest singular value.

**Spectral theorem:** Real symmetric matrices have orthonormal eigenbasis.

**Span:** All linear combinations of a set.

**Trace:** Sum of diagonal entries, also sum of eigenvalues.

**Vandermonde determinant:** Product of pairwise differences.

*End of Comprehensive Notes — Linear Algebra for GATE 2027* 📚✨
