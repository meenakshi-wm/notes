# 📉 Calculus & Optimization — Comprehensive Notes for GATE 2027 (CS/IT/DA) 🎯

> 🌟 **"Calculus is the mathematics of CHANGE — it's the engine behind every AI that learns, every rocket that flies, and every stock market prediction!"**

> 💡 **Beginner Analogy:** Imagine driving a car 🚗. Your **speedometer** shows the derivative (rate of change of distance). Your **odometer** shows the integral (total distance accumulated). Calculus is literally the math of "how fast is something changing?" and "how much has accumulated?" That's it! Everything else is just fancy versions of these two questions!

---

## 🏭 Where is Calculus Used in the Real World?

| 🌍 Industry | 🔧 Application | 📌 Calculus Concept |
|---|---|---|
| **Machine Learning/AI** 🤖 | Training neural networks (backpropagation) | Gradient descent, Chain rule, Partial derivatives |
| **Tesla Self-Driving Cars** 🚗 | Path planning, motion prediction | Optimization, Differential equations |
| **SpaceX Rockets** 🚀 | Trajectory optimization | Calculus of variations, Constrained optimization |
| **Stock Market** 📈 | Options pricing (Black-Scholes equation) | Partial differential equations |
| **Medical Imaging (MRI)** 🏥 | Image reconstruction from signals | Integration, Fourier transforms |
| **Weather Prediction** ⛈️ | Fluid dynamics simulations | Multivariable calculus, PDEs |
| **Amazon Supply Chain** 📦 | Minimizing delivery costs | Optimization (Lagrange multipliers) |
| **Netflix ML Models** 🎬 | Gradient descent for recommendation engine | Derivatives, Convex optimization |
| **Physics Engines (Games)** 🎮 | Realistic motion simulation | Integration (velocity → position) |
| **Signal Processing (5G)** 📶 | Filter design, modulation | Integration, Taylor series |

---

## 📊 GATE Weightage & Strategy

| Parameter | Details |
|---|---|
| **Expected Marks** | 3–7 marks every year (1–2 questions of 1 or 2 marks each) |
| **Frequency** | Every year — limits, continuity, integration are staples; optimization increasingly important for DA |
| **Difficulty** | Medium — conceptual understanding + careful computation |
| **Time to Master** | 4–5 weeks (first pass) + 1 week revision |
| **ROI** | High — well-defined syllabus, predictable question patterns, overlaps with ML/optimization |

### Recommended Order of Preparation

```
MODULE 1 — Single Variable Calculus
 1. Limits (definition, evaluation techniques, standard forms, sequences)
 2. Indeterminate Forms & L'Hôpital's Rule
 3. Power-form Indeterminates (1^∞, 0^0, ∞^0)
 4. Continuity (EVT, Uniform continuity, Lipschitz)
 5. Differentiability (Implicit, Logarithmic, Parametric, Higher-order, Leibniz)
 6. Mean Value Theorems (Rolle's, LMVT, Cauchy, Taylor Remainder)
 7. Maxima & Minima (Concavity, Inflection Points, Applied Optimization)
 8. Integration (Riemann Sums, FTC, techniques, Improper, Gamma, Beta, Wallis)

MODULE 2 — Multivariate Calculus & Optimization
 9. Taylor Series (single & multivariate, Power Series, Convergence)
10. Partial Differentiation & Chain Rule
11. Total Differential & Euler's Theorem for Homogeneous Functions
12. Jacobian Matrix & Determinant
13. Gradient, Directional Derivatives, Tangent Plane, Normal to Surface
14. Divergence and Curl (DA)
15. Contour Maps & Level Curves
16. Matrix Calculus (derivatives of vector/matrix expressions)
17. Positive Definite Matrices & Quadratic Forms
18. Unconstrained Optimization (multivariate)
19. Convex Functions (first & second order conditions)
20. Gradient Descent Algorithm & Newton's Method
21. Constrained Optimization — Equality (Lagrange Multipliers)
22. Constrained Optimization — Inequality (KKT Conditions)

MODULE 3 — Advanced Topics (DA Extension)
23. Double Integrals (Fubini, switching order, polar coordinates)
24. First-Order ODEs (Separable, Homogeneous, Linear, Exact, Bernoulli)
```

---

# 📐 MODULE 1: SINGLE VARIABLE CALCULUS

---

## Chapter 1: Limits (⭐⭐ FREQUENTLY ASKED) 🔬

### 1.1 What is a Limit? 🤔

> 💡 **Beginner Analogy — Walking Towards a Wall 🧱:**
> Imagine walking towards a wall. You take a step, then half a step, then a quarter step, then an eighth... You get infinitely close to the wall but technically never touch it! The wall's position is the **limit** of your walk.
>
> Mathematically: $\lim_{n\to\infty} (1 - 1/2^n) = 1$ — you approach 1 but never quite reach it!
>
> 🏭 **Production Connection:** Limits are fundamental to:
> - **Machine Learning convergence** 🤖: "Has my neural network's loss function stopped decreasing?" = checking if the limit exists!
> - **Computer precision** 💻: Floating point arithmetic approximates limits (why 0.1 + 0.2 ≠ 0.3 in Python!)
> - **Streaming analytics** 📊: Running averages approach a limit as data flows in (used in Apache Kafka, Flink)

The **limit** of a function $f(x)$ as $x$ approaches $a$ is the value that $f(x)$ gets closer and closer to, **without necessarily reaching it**.

$$\lim_{x \to a} f(x) = L$$

means: for every $\epsilon > 0$, there exists $\delta > 0$ such that $0 < |x - a| < \delta \implies |f(x) - L| < \epsilon$.

**Real-world analogy:** Imagine walking toward a wall. The limit is the wall — you keep getting closer to it. Whether you actually touch the wall (the function value at that point) is a separate question.

### 1.2 Left-Hand and Right-Hand Limits

$$\text{LHL} = \lim_{x \to a^-} f(x) \quad \text{(approach from the left)}$$
$$\text{RHL} = \lim_{x \to a^+} f(x) \quad \text{(approach from the right)}$$

> **The limit exists** iff LHL = RHL, i.e., $\lim_{x \to a^-} f(x) = \lim_{x \to a^+} f(x) = L$.

### 1.3 Algebra of Limits

If $\lim_{x \to a} f(x) = L$ and $\lim_{x \to a} g(x) = M$, then:

| Operation | Result |
|---|---|
| $\lim [f(x) + g(x)]$ | $L + M$ |
| $\lim [f(x) \cdot g(x)]$ | $L \cdot M$ |
| $\lim \frac{f(x)}{g(x)}$ | $\frac{L}{M}$ (if $M \neq 0$) |
| $\lim [f(x)]^n$ | $L^n$ |
| $\lim c \cdot f(x)$ | $c \cdot L$ |

### 1.4 Standard Limits (⭐ MUST MEMORIZE)

| Standard Limit | Value |
|---|---|
| $\lim_{x \to 0} \frac{\sin x}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{\tan x}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{1 - \cos x}{x^2}$ | $\frac{1}{2}$ |
| $\lim_{x \to 0} \frac{e^x - 1}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{\ln(1+x)}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{a^x - 1}{x}$ | $\ln a$ |
| $\lim_{x \to 0} (1+x)^{1/x}$ | $e$ |
| $\lim_{x \to \infty} \left(1+\frac{1}{x}\right)^x$ | $e$ |
| $\lim_{x \to 0} \frac{\sin^{-1} x}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{\tan^{-1} x}{x}$ | $1$ |
| $\lim_{x \to \infty} \frac{x^n}{e^x}$ | $0$ (exponential dominates polynomial) |
| $\lim_{x \to \infty} \frac{\ln x}{x^p}$ ($p > 0$) | $0$ (polynomial dominates log) |

### 1.5 Methods to Evaluate Limits

**Method 1: Direct Substitution**
If $f$ is continuous at $a$, then $\lim_{x \to a} f(x) = f(a)$.

**Method 2: Factoring and Cancellation**
For $\frac{0}{0}$ forms, factor numerator and denominator to cancel common terms.

**Method 3: Rationalization**
Multiply by conjugate when square roots appear.

**Method 4: Standard Limit Substitution**
Rewrite expressions to match standard forms from the table above.

**Method 5: Squeeze Theorem (Sandwich Theorem)**
If $g(x) \leq f(x) \leq h(x)$ near $a$ and $\lim_{x \to a} g(x) = \lim_{x \to a} h(x) = L$, then $\lim_{x \to a} f(x) = L$.

---

**Example 1 (Easy):** Evaluate $\lim_{x \to 2} \frac{x^2 - 4}{x - 2}$.

**Solution:**
$$\frac{x^2 - 4}{x - 2} = \frac{(x-2)(x+2)}{x-2} = x + 2$$
$$\lim_{x \to 2} (x + 2) = 4$$

---

**Example 2 (Medium):** Evaluate $\lim_{x \to 0} \frac{\sin 3x}{\sin 5x}$.

**Solution:**
$$\lim_{x \to 0} \frac{\sin 3x}{\sin 5x} = \lim_{x \to 0} \frac{\sin 3x}{3x} \cdot \frac{5x}{\sin 5x} \cdot \frac{3x}{5x} = 1 \cdot 1 \cdot \frac{3}{5} = \frac{3}{5}$$

---

**Example 3 (Medium):** Evaluate $\lim_{x \to 0} \frac{e^{3x} - 1}{\tan 2x}$.

**Solution:**
$$\lim_{x \to 0} \frac{e^{3x} - 1}{\tan 2x} = \lim_{x \to 0} \frac{e^{3x}-1}{3x} \cdot \frac{2x}{\tan 2x} \cdot \frac{3x}{2x} = 1 \cdot 1 \cdot \frac{3}{2} = \frac{3}{2}$$

---

**Example 4 (Squeeze Theorem):** Evaluate $\lim_{x \to 0} x^2 \sin\frac{1}{x}$.

**Solution:** Since $-1 \leq \sin\frac{1}{x} \leq 1$, we get $-x^2 \leq x^2\sin\frac{1}{x} \leq x^2$. Both $\lim_{x\to 0}(-x^2) = 0$ and $\lim_{x\to 0}(x^2) = 0$. By Squeeze Theorem, $\lim_{x \to 0} x^2\sin\frac{1}{x} = 0$.

---

### 1.6 Indeterminate Forms (⭐)

When direct substitution gives one of these forms, we need special techniques:

| Indeterminate Form | Type |
|---|---|
| $\frac{0}{0}$ | Most common, use L'Hôpital or factoring |
| $\frac{\infty}{\infty}$ | Use L'Hôpital |
| $0 \cdot \infty$ | Convert to $\frac{0}{0}$ or $\frac{\infty}{\infty}$ |
| $\infty - \infty$ | Combine into single fraction, then evaluate |
| $1^{\infty}$ | Use $e^{\lim f \cdot \ln g}$ form |
| $0^0$ | Take logarithm, then evaluate |
| $\infty^0$ | Take logarithm, then evaluate |

### 1.7 L'Hôpital's Rule (⭐⭐ VERY IMPORTANT)

> **Theorem (L'Hôpital's Rule):** If $\lim_{x \to a} \frac{f(x)}{g(x)}$ gives $\frac{0}{0}$ or $\frac{\infty}{\infty}$, and $f'(x)$, $g'(x)$ exist near $a$ with $g'(x) \neq 0$, then:
$$\lim_{x \to a} \frac{f(x)}{g(x)} = \lim_{x \to a} \frac{f'(x)}{g'(x)}$$
> provided the right-hand limit exists (or is $\pm\infty$).

**When to use:** Only when you get $\frac{0}{0}$ or $\frac{\infty}{\infty}$. Can be applied repeatedly.

**Common Mistake:** Applying L'Hôpital when the form is NOT indeterminate. Always verify the form first!

---

**Example 5 (L'Hôpital):** Evaluate $\lim_{x \to 0} \frac{x - \sin x}{x^3}$.

**Solution:** Direct substitution gives $\frac{0}{0}$.

Apply L'Hôpital: $\lim_{x \to 0} \frac{1 - \cos x}{3x^2}$ → still $\frac{0}{0}$.

Apply again: $\lim_{x \to 0} \frac{\sin x}{6x}$ → still $\frac{0}{0}$.

Apply again: $\lim_{x \to 0} \frac{\cos x}{6} = \frac{1}{6}$.

---

**Example 6 (GATE Level):** Evaluate $\lim_{x \to 0} \frac{e^x - 1 - x - \frac{x^2}{2}}{x^3}$.

**Solution:** Form: $\frac{0}{0}$. Apply L'Hôpital repeatedly:

$$\lim_{x \to 0} \frac{e^x - 1 - x}{3x^2} = \lim_{x \to 0} \frac{e^x - 1}{6x} = \lim_{x \to 0} \frac{e^x}{6} = \frac{1}{6}$$

---

### 1.8 Power-Form Indeterminates ($1^\infty$, $0^0$, $\infty^0$) (⭐)

For limits of the form $\lim_{x \to a} [f(x)]^{g(x)}$:

**General technique:** Let $y = [f(x)]^{g(x)}$, then $\ln y = g(x) \cdot \ln f(x)$.

Evaluate $\lim_{x \to a} g(x) \cdot \ln f(x)$, then the answer is $e^{\text{that limit}}$.

**Special formula for $1^\infty$ form:**

$$\lim_{x \to a} [f(x)]^{g(x)} = e^{\lim_{x \to a}\, g(x)\,[f(x) - 1]}$$

when $\lim_{x \to a} f(x) = 1$ and $\lim_{x \to a} g(x) = \infty$.

---

**Example 7 ($1^\infty$):** Evaluate $\lim_{x \to 0} (1 + \sin x)^{\cot x}$.

**Solution:** As $x \to 0$: $\sin x \to 0$ so $(1+\sin x) \to 1$, and $\cot x \to \infty$. Form: $1^\infty$.

Using the formula: $e^{\lim_{x \to 0} \cot x \cdot \sin x} = e^{\lim_{x \to 0} \frac{\sin x \cdot \cos x}{\sin x}} = e^{\lim_{x \to 0} \cos x} = e^1 = e$.

---

**Example 8 (GATE Level — $1^\infty$):** Evaluate $\lim_{x \to 0} \left(\frac{\sin x}{x}\right)^{1/x^2}$.

**Solution:** As $x \to 0$: $\frac{\sin x}{x} \to 1$ and $\frac{1}{x^2} \to \infty$. Form: $1^\infty$.

Let $L = \lim_{x \to 0} \frac{1}{x^2}\left(\frac{\sin x}{x} - 1\right) = \lim_{x \to 0} \frac{\sin x - x}{x^3}$

Using L'Hôpital (as in Example 5): $\frac{\sin x - x}{x^3} \to -\frac{1}{6}$.

$$\text{Answer} = e^{-1/6}$$

---

### 1.9 Limits at Infinity

**Growth rate hierarchy** (from slowest to fastest):
$$\ln x \ll x^p \ll a^x \ll x! \ll x^x \quad \text{for } a > 1, p > 0$$

**Key technique for rational functions:** Divide numerator and denominator by the highest power of $x$.

$$\lim_{x \to \infty} \frac{a_nx^n + \cdots + a_0}{b_mx^m + \cdots + b_0} = \begin{cases} \frac{a_n}{b_m} & \text{if } n = m \\ 0 & \text{if } n < m \\ \pm\infty & \text{if } n > m \end{cases}$$

---

**Example 9:** Evaluate $\lim_{x \to \infty} \frac{3x^2 + 5x - 1}{7x^2 - 2x + 4}$.

**Solution:** Degree of numerator = degree of denominator = 2.

$$\text{Answer} = \frac{3}{7}$$

---

### 1.10 Common Mistakes & Traps in Limits

| Mistake | Correction |
|---|---|
| Applying L'Hôpital without checking $\frac{0}{0}$ or $\frac{\infty}{\infty}$ | Always verify the indeterminate form first |
| Differentiating as quotient rule instead of separately | L'Hôpital: differentiate numerator and denominator **independently** |
| Forgetting $\lim \frac{\sin x}{x} = 1$ only when $x \to 0$ | For $x \to \infty$, $\frac{\sin x}{x} \to 0$ |
| Assuming limit exists when LHL $\neq$ RHL | Check both sides for piecewise/absolute value functions |
| Ignoring the sign of $x$ in $|x|$ near 0 | $|x| = x$ for $x > 0$, $|x| = -x$ for $x < 0$ |

---

### 1.11 🔢 Limits of Sequences (DA Extension)

A **sequence** $\{a_n\}$ converges to $L$ if $|a_n - L| \to 0$ as $n \to \infty$.

**Common Sequence Limits (⭐ Must Memorize):**

| Sequence $a_n$ | $\lim_{n\to\infty} a_n$ |
|---|---|
| $\frac{1}{n^p}$ ($p > 0$) | $0$ |
| $r^n$ (\|r\| < 1) | $0$ |
| $r^n$ ($r > 1$) | $+\infty$ (diverges) |
| $n^{1/n}$ | $1$ |
| $(1 + 1/n)^n$ | $e$ |
| $(1 + c/n)^n$ | $e^c$ |
| $(1 + a/n)^{bn}$ | $e^{ab}$ |
| $\left(\frac{n!}{n^n}\right)^{1/n}$ | $\frac{1}{e}$ (use Stirling) |

> 🧠 **Stirling's Approximation (GATE DA ⭐):** $n! \approx \sqrt{2\pi n}\left(\dfrac{n}{e}\right)^n$ for large $n$.

**Cesàro Mean Theorem:** If $\lim_{n\to\infty} a_n = L$, then $\lim_{n\to\infty} \dfrac{a_1 + a_2 + \cdots + a_n}{n} = L$.

---

### 1.12 ⚠️ Pointwise Limits vs. Uniform Convergence (DA Trap 🔴)

A sequence of continuous functions $f_n(x) \to f(x)$ **pointwise** does NOT guarantee $f$ is continuous!

**Classic Trap Example:** $f_n(x) = x^n$ on $[0, 1]$:
- For $0 \leq x < 1$: $x^n \to 0$
- For $x = 1$: $1^n = 1 \to 1$

So the limit $f(x) = \begin{cases} 0 & 0 \leq x < 1 \\ 1 & x = 1 \end{cases}$ is **discontinuous** even though each $f_n$ is continuous! 😱

⚠️ **GATE DA Trap:** The order of limits matters! In general:
$$\lim_{x \to a}\lim_{n\to\infty} f_n(x) \neq \lim_{n\to\infty}\lim_{x\to a} f_n(x)$$

---

### 1.13 🎯 GATE PYQ — Hard Limit Problems

**📌 PYQ 1 (GATE CS Style 🔴 Hard):** $\lim_{x \to 1} \dfrac{x^7 - 2x^5 + 1}{x^3 - 3x^2 + 2}$

**(A)** $0$ **(B)** $1$ **(C)** $-2$ **(D)** Does not exist

**Solution:** Direct substitution: $\frac{1-2+1}{1-3+2} = \frac{0}{0}$. Apply L'Hôpital:
$$\lim_{x\to 1} \frac{7x^6 - 10x^4}{3x^2 - 6x} = \frac{7 - 10}{3 - 6} = \frac{-3}{-3} = \boxed{1}$$
✅ **Answer: (B)** | Difficulty: 🔴 Hard

⚠️ **Trap:** Confirm $0/0$ before applying L'Hôpital each time.

---

**📌 PYQ 2 (GATE CS Higher-Order 🔴 Hard):** $\lim_{x\to 0} \dfrac{(1+x)^{1/x} - e}{x}$

**Solution:** Using Taylor expansion $\ln(1+x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots$:
$$\frac{\ln(1+x)}{x} = 1 - \frac{x}{2} + \frac{x^2}{3} - \cdots$$
$$(1+x)^{1/x} = e^{1 - x/2 + x^2/3 - \cdots} = e \cdot e^{-x/2 + O(x^2)} = e\left(1 - \frac{x}{2} + O(x^2)\right)$$
$$\therefore \frac{(1+x)^{1/x} - e}{x} \to e \cdot\left(-\frac{1}{2}\right) = \boxed{-\frac{e}{2}}$$
✅ **Answer: $-e/2$** | Difficulty: 🔴 Hard

⚠️ **Trap:** L'Hôpital is messy here. **Taylor expansion is the clean approach.**

---

**📌 PYQ 3 (GATE CS 2019 Style 🟠 Medium-Hard):** $\lim_{x\to 0^+} x^x$

**(A)** $0$ **(B)** $1/e$ **(C)** $1$ **(D)** $\infty$

**Solution:** Form $0^0$. Let $L = \lim_{x\to 0^+} x^x$. Take log:
$$\ln L = \lim_{x\to 0^+} x\ln x = \lim_{x\to 0^+} \frac{\ln x}{1/x} \stackrel{\text{LH}}{=} \lim_{x\to 0^+} \frac{1/x}{-1/x^2} = \lim_{x\to 0^+} (-x) = 0$$
So $L = e^0 = \boxed{1}$. ✅ **Answer: (C)** | Difficulty: 🟠 Medium-Hard

⚠️ **Common Trap:** Assuming $0^0 = 0$. Always use the logarithm technique!

---

**📌 PYQ 4 (GATE CS 2015 Actual 🟡 Easy with Trap):** $\lim_{x\to\infty} x\sin\!\left(\dfrac{1}{x}\right)$

**(A)** $0$ **(B)** $1$ **(C)** $\pi$ **(D)** $\infty$

**Solution:** Let $t = 1/x \to 0^+$:
$$\lim_{t\to 0^+} \frac{\sin t}{t} = \boxed{1}$$
✅ **Answer: (B)** | Difficulty: 🟡 Easy-Medium

⚠️ **Big Trap:** Students confuse $\lim_{x\to\infty} \frac{\sin x}{x} = 0$ (numerator bounded, denominator $\to\infty$) with $\lim_{x\to\infty} x\sin(1/x) = 1$ (argument of $\sin$ goes to $0$). These are completely different!

---

**📌 PYQ 5 (GATE DA 🔴 Hard — Pointwise Limit):** Define $f(x) = \lim_{n\to\infty} \dfrac{x^{2n} - 1}{x^{2n} + 1}$. Then:

**(A)** $f$ has a jump discontinuity at $x = 1$
**(B)** $f$ is continuous everywhere
**(C)** $f$ has a removable discontinuity at $x = 1$
**(D)** $f$ is not defined at $x = 1$

**Solution:**
- $|x| < 1$: $x^{2n} \to 0$, so $f(x) = -1$
- $|x| > 1$: $x^{2n} \to \infty$, so $f(x) = 1$
- $|x| = 1$: $x^{2n} = 1$, so $f(x) = 0$

$\lim_{x\to 1^-} f(x) = -1 \neq 1 = \lim_{x\to 1^+} f(x)$ → **Jump discontinuity at $x = 1$ and $x = -1$**

✅ **Answer: (A)** | Difficulty: 🔴 Hard

⚠️ **Key GATE Trap:** Each $\frac{x^{2n}-1}{x^{2n}+1}$ is continuous for every fixed $n$, but the pointwise limit creates a DISCONTINUITY!

---

**📌 PYQ 6 (GATE CS 2014 Style 🟠 Medium):** $\lim_{n\to\infty} \left(1 + \dfrac{x}{n}\right)^n = ?$

**(A)** $e$ **(B)** $e^x$ **(C)** $1 + x$ **(D)** does not exist for $x \neq 0$

**Solution:**
$$\ln\left[\left(1 + \frac{x}{n}\right)^n\right] = n\ln\!\left(1 + \frac{x}{n}\right) = x \cdot \frac{\ln(1+x/n)}{x/n} \to x \cdot 1 = x$$
So limit $= e^x$. ✅ **Answer: (B)** | Difficulty: 🟠 Medium

⚠️ **Generalization (GATE Trap):** $\lim_{n\to\infty}\left(1 + \dfrac{a}{n}\right)^{bn} = e^{ab}$ — memorize this general form!

---

## Chapter 2: Continuity (⭐)

### 2.1 Definition of Continuity

A function $f$ is **continuous at $x = a$** if ALL three conditions hold:

1. $f(a)$ is defined
2. $\lim_{x \to a} f(x)$ exists (i.e., LHL = RHL)
3. $\lim_{x \to a} f(x) = f(a)$

**Real-world analogy:** A continuous function is like a road with no gaps — you can draw the graph without lifting your pen.

### 2.2 Types of Discontinuity

| Type | Condition | Visual |
|---|---|---|
| **Removable** | Limit exists but $f(a) \neq \lim f(x)$ or $f(a)$ undefined | A hole in the graph |
| **Jump (Irremovable)** | LHL $\neq$ RHL | A sudden jump in the graph |
| **Infinite (Irremovable)** | $\lim_{x \to a} f(x) = \pm\infty$ | Vertical asymptote |
| **Oscillatory** | Limit doesn't exist due to oscillation | e.g., $\sin(1/x)$ at $x=0$ |

### 2.3 Properties of Continuous Functions

| Property | Statement |
|---|---|
| Algebra | Sum, difference, product, quotient (with nonzero denom) of continuous functions are continuous |
| Composition | If $f$ continuous at $a$ and $g$ continuous at $f(a)$, then $g \circ f$ is continuous at $a$ |
| Polynomials | Continuous everywhere |
| $\sin x, \cos x$ | Continuous everywhere |
| $e^x, \ln x$ | Continuous on their domains |

### 2.4 Intermediate Value Theorem (IVT) (⭐)

> **Theorem:** If $f$ is continuous on $[a, b]$ and $k$ is any value between $f(a)$ and $f(b)$, then there exists at least one $c \in (a, b)$ such that $f(c) = k$.

**Key application:** Proving that equations have roots. If $f(a) < 0$ and $f(b) > 0$ (and $f$ is continuous), then $f$ has at least one root in $(a,b)$.

---

**Example 10:** Show that $x^3 - 3x + 1 = 0$ has a root in $[0, 1]$.

**Solution:** Let $f(x) = x^3 - 3x + 1$. $f(0) = 1 > 0$ and $f(1) = 1 - 3 + 1 = -1 < 0$.

Since $f$ is continuous (polynomial) and changes sign, by IVT there exists $c \in (0,1)$ such that $f(c) = 0$.

---

**Example 11 (GATE Level):** Let $f: [0,1] \to [0,1]$ be continuous. Prove $f$ has a fixed point (i.e., $f(c) = c$ for some $c$).

**Solution:** Define $g(x) = f(x) - x$. Then $g(0) = f(0) - 0 = f(0) \geq 0$ (since $f(0) \in [0,1]$) and $g(1) = f(1) - 1 \leq 0$ (since $f(1) \leq 1$). By IVT, $\exists\, c \in [0,1]$ where $g(c) = 0$, i.e., $f(c) = c$.

---

### 2.5 📌 Extreme Value Theorem (EVT) ⭐

> **Theorem (EVT):** If $f$ is **continuous on a closed bounded interval $[a, b]$**, then $f$ attains both its **absolute maximum** and **absolute minimum** on $[a, b]$.

🔑 **Key Points:**
- Interval must be **closed** (includes endpoints) AND **bounded** (finite)
- Function must be **continuous** on the entire interval
- Guarantees **existence** but NOT uniqueness of extrema

**Why conditions are essential:**

| Violation | Counterexample |
|---|---|
| Open interval $(0, 1)$ | $f(x) = 1/x$ — no maximum, blows to $\infty$ |
| Unbounded interval $[0, \infty)$ | $f(x) = x$ — no maximum |
| Discontinuous on $[0, 1]$ | $f(x) = x$ for $x \in [0,1)$, $f(1) = 0$ — no maximum attained |

> 💡 **GATE Connection:** EVT justifies that optimization problems on **compact domains** always have solutions. Crucial for analysis of algorithms that minimize cost functions on bounded parameter sets!

**Example (EVT Application):** Find global extrema of $f(x) = x^3 - 3x$ on $[-2, 2]$.

$f'(x) = 3x^2 - 3 = 0 \Rightarrow x = \pm 1 \in [-2, 2]$

| $x$ | $f(x)$ |
|---|---|
| $-2$ | $-8 + 6 = -2$ |
| $-1$ | $-1 + 3 = 2$ ← **Maximum** |
| $1$ | $1 - 3 = -2$ ← **Minimum** |
| $2$ | $8 - 6 = 2$ ← **Maximum** |

**Global maximum = 2** (at $x = -1$ and $x = 2$). **Global minimum = -2** (at $x = 1$ and $x = -2$).

---

### 2.6 🔗 Uniform Continuity (DA Extension ⭐)

A function $f$ is **uniformly continuous** on $D$ if:
$$\forall\, \epsilon > 0,\; \exists\, \delta > 0 \text{ such that } |x - y| < \delta \Rightarrow |f(x) - f(y)| < \epsilon \quad \text{for ALL } x, y \in D$$

**Key Difference from ordinary continuity:**
- **Ordinary:** $\delta$ can depend on both $\epsilon$ AND the point $x$
- **Uniform:** $\delta$ depends **only on $\epsilon$**, works for ALL points simultaneously

| Function | Domain | Uniformly Continuous? | Reason |
|---|---|---|---|
| $f(x) = x^2$ | $[0, 1]$ | ✅ Yes | Continuous on compact set |
| $f(x) = x^2$ | $[0, \infty)$ | ❌ No | Derivative unbounded ($2x \to \infty$) |
| $f(x) = \sin x$ | $\mathbb{R}$ | ✅ Yes | Lipschitz: $|\sin x - \sin y| \leq |x - y|$ |
| $f(x) = 1/x$ | $(0, 1)$ | ❌ No | Unbounded near $x = 0$ |
| $f(x) = \sqrt{x}$ | $[0, 1]$ | ✅ Yes | Extends to closed set |

> 🏆 **Theorem (Heine-Cantor):** Every continuous function on a **compact set** (closed + bounded in $\mathbb{R}^n$) is **uniformly continuous**.

---

### 2.7 🔒 Lipschitz Continuity (DA Extension ⭐)

$f$ is **Lipschitz continuous** with constant $L \geq 0$ on $D$ if:
$$|f(x) - f(y)| \leq L|x - y| \quad \forall\, x, y \in D$$

**Hierarchy (important for GATE DA!):**
$$\text{Differentiable with bounded derivative} \implies \text{Lipschitz} \implies \text{Uniformly Continuous} \implies \text{Continuous}$$

⚠️ **None of these implications reverse!**

**Application in Machine Learning 🤖:** Lipschitz smoothness of loss function (condition $\|\nabla f(x) - \nabla f(y)\| \leq L\|x-y\|$) guarantees gradient descent converges with step size $\alpha \leq 1/L$. This is the theoretical basis for **learning rate selection**!

---

### 2.8 🎯 GATE PYQ — Hard Continuity Problems

**📌 PYQ 7 (GATE CS 2016 Style 🟠 Medium):**
$$f(x) = \begin{cases} \dfrac{x^2 - 9}{x - 3} & x \neq 3 \\ k & x = 3 \end{cases}$$
For what value of $k$ is $f$ continuous at $x = 3$?

**Solution:** $\lim_{x\to 3} \frac{x^2-9}{x-3} = \lim_{x\to 3}(x+3) = 6$. So $k = 6$. ✅

---

**📌 PYQ 8 (GATE CS 2022 Style 🔴 Hard):**
$$f(x) = \begin{cases} x\sin\dfrac{1}{x} & x \neq 0 \\ 0 & x = 0 \end{cases}$$
Which is TRUE? **(A)** $f$ is discontinuous at $0$ **(B)** Continuous but not differentiable at $0$ **(C)** Differentiable at $0$ **(D)** Continuous and differentiable everywhere except $0$

**Solution:**
- **Continuity:** $\lim_{x\to 0} x\sin(1/x) = 0$ = $f(0)$ ✅ (by Squeeze Theorem: $|x\sin(1/x)| \leq |x|$)
- **Differentiability:** $f'(0) = \lim_{h\to 0} \sin(1/h)$ — **oscillates between $-1$ and $1$, DNE** ✗

✅ **Answer: (B)** | Difficulty: 🔴 Hard

⚠️ **KEY TRAP:** Don't confuse $x\sin(1/x)$ (continuous, NOT differentiable at 0) with $x^2\sin(1/x)$ (continuous AND differentiable at 0, $f'(0)=0$). GATE loves testing this pair!

---

**📌 PYQ 9 (GATE Level 🔴 Hard — Uniform Continuity):**
How many of the following are uniformly continuous on $(0, 1)$?
(I) $\sin x$ (II) $1/x$ (III) $\sqrt{x}$ (IV) $x\sin(1/x)$

**Solution:**
- (I) $\sin x$: Lipschitz (bounded derivative), ✅ uniformly continuous
- (II) $1/x$: $f'(x) = -1/x^2 \to \infty$ as $x\to 0^+$, ❌ NOT uniformly continuous
- (III) $\sqrt{x}$: Extends continuously to $[0,1]$ (compact), ✅ uniformly continuous
- (IV) $x\sin(1/x)$: $|x\sin(1/x) - 0| \leq x \to 0$, extends to $[0,1]$, ✅ uniformly continuous

**Answer: 3 functions** are uniformly continuous. | Difficulty: 🔴 Hard

---

## Chapter 3: Differentiability (⭐)

### 3.1 Definition

$f$ is **differentiable at $x = a$** if the following limit exists:

$$f'(a) = \lim_{h \to 0} \frac{f(a+h) - f(a)}{h}$$

Equivalently, both one-sided derivatives must exist and be equal:

$$\text{LHD} = \lim_{h \to 0^-} \frac{f(a+h) - f(a)}{h} = \text{RHD} = \lim_{h \to 0^+} \frac{f(a+h) - f(a)}{h}$$

**Geometric meaning:** The derivative at a point is the slope of the tangent line at that point.

### 3.2 Relationship Between Continuity and Differentiability (⭐⭐)

> **Theorem:** If $f$ is differentiable at $a$, then $f$ is continuous at $a$.

> **The converse is FALSE:** A function can be continuous but not differentiable.

```
Differentiable ⟹ Continuous    ✓
Continuous ⟹ Differentiable    ✗
```

**Classic counterexample:** $f(x) = |x|$ is continuous at $x = 0$ but not differentiable there.

- LHD at $0$: $\lim_{h \to 0^-} \frac{|h|}{h} = \lim_{h \to 0^-} \frac{-h}{h} = -1$
- RHD at $0$: $\lim_{h \to 0^+} \frac{|h|}{h} = \lim_{h \to 0^+} \frac{h}{h} = 1$

Since LHD $\neq$ RHD, $f$ is not differentiable at $x = 0$.

### 3.3 Where Functions Fail to be Differentiable

| Situation | Example |
|---|---|
| **Corner/Cusp** | $f(x) = |x|$ at $x = 0$ |
| **Vertical tangent** | $f(x) = x^{1/3}$ at $x = 0$ |
| **Discontinuity** | $f(x) = \lfloor x \rfloor$ at integers |

### 3.4 Important Differentiation Rules

| Rule | Formula |
|---|---|
| **Power Rule** | $\frac{d}{dx}x^n = nx^{n-1}$ |
| **Product Rule** | $(fg)' = f'g + fg'$ |
| **Quotient Rule** | $\left(\frac{f}{g}\right)' = \frac{f'g - fg'}{g^2}$ |
| **Chain Rule** | $\frac{d}{dx}f(g(x)) = f'(g(x)) \cdot g'(x)$ |
| **Logarithmic** | $\frac{d}{dx}\ln|f(x)| = \frac{f'(x)}{f(x)}$ |

---

**Example 12 (GATE Level):** Let $f(x) = x^2 \sin\frac{1}{x}$ for $x \neq 0$ and $f(0) = 0$. Is $f$ differentiable at $x = 0$?

**Solution:** We use the definition:

$$f'(0) = \lim_{h \to 0} \frac{f(h) - f(0)}{h} = \lim_{h \to 0} \frac{h^2 \sin(1/h)}{h} = \lim_{h \to 0} h\sin\frac{1}{h} = 0$$

(by Squeeze Theorem, since $|h\sin(1/h)| \leq |h| \to 0$)

Yes, $f$ is differentiable at $x = 0$ and $f'(0) = 0$.

---

**Example 13:** Let $f(x) = x|x|$. Check differentiability at $x = 0$.

**Solution:** $f(x) = x^2$ for $x \geq 0$ and $f(x) = -x^2$ for $x < 0$.

$$f'(0) = \lim_{h \to 0} \frac{f(h) - 0}{h}$$

- RHD: $\lim_{h \to 0^+} \frac{h^2}{h} = 0$
- LHD: $\lim_{h \to 0^-} \frac{-h^2}{h} = 0$

LHD = RHD = 0 → $f$ is differentiable at $x = 0$ with $f'(0) = 0$.

---

### 3.5 🔄 Implicit Differentiation ⭐

When $y$ is defined **implicitly** by $F(x, y) = 0$, differentiating both sides w.r.t. $x$:

$$\frac{dy}{dx} = -\frac{\partial F/\partial x}{\partial F/\partial y} = -\frac{F_x}{F_y}$$

**Step-by-step:**
1. Differentiate both sides of $F(x, y) = 0$ w.r.t. $x$
2. Use chain rule on $y$-terms: $\frac{d}{dx}[y^n] = ny^{n-1}\frac{dy}{dx}$
3. Collect all $\frac{dy}{dx}$ terms on one side and solve

---

**Example (Implicit Diff):** Find $\frac{dy}{dx}$ if $x^3 + y^3 = 6xy$.

**Solution:**
$$3x^2 + 3y^2\frac{dy}{dx} = 6y + 6x\frac{dy}{dx}$$
$$\frac{dy}{dx}(3y^2 - 6x) = 6y - 3x^2 \implies \frac{dy}{dx} = \frac{2y - x^2}{y^2 - 2x}$$

---

**Example (Ellipse slope):** For $\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1$, find $\frac{dy}{dx}$.

$$\frac{2x}{a^2} + \frac{2y}{b^2}\frac{dy}{dx} = 0 \implies \frac{dy}{dx} = -\frac{b^2 x}{a^2 y}$$

---

**Example (GATE Trap — Second Implicit Derivative):** For $x^2 + y^2 = r^2$, find $\frac{d^2y}{dx^2}$.

**Solution:** First: $2x + 2y\frac{dy}{dx} = 0 \implies \frac{dy}{dx} = -\frac{x}{y}$.

$$\frac{d^2y}{dx^2} = -\frac{y - x\frac{dy}{dx}}{y^2} = -\frac{y - x(-x/y)}{y^2} = -\frac{y^2 + x^2}{y^3} = -\frac{r^2}{y^3}$$

---

### 3.6 📝 Logarithmic Differentiation ⭐

Used when:
- Function has **variable in both base and exponent**: $[f(x)]^{g(x)}$
- Function is a **complex product/quotient** of many terms

**Formula for $y = [f(x)]^{g(x)}$:**
$$\ln y = g(x)\ln f(x) \implies \frac{y'}{y} = g'(x)\ln f(x) + g(x)\frac{f'(x)}{f(x)}$$
$$y' = [f(x)]^{g(x)}\left[g'(x)\ln f(x) + \frac{g(x)f'(x)}{f(x)}\right]$$

---

**Example 1:** Differentiate $y = x^x$.
$$\ln y = x\ln x \implies \frac{y'}{y} = \ln x + 1 \implies \boxed{y' = x^x(\ln x + 1)}$$

---

**Example 2:** Differentiate $y = (\sin x)^{\cos x}$.
$$\frac{y'}{y} = -\sin x\ln(\sin x) + \cos x \cdot \frac{\cos x}{\sin x}$$
$$y' = (\sin x)^{\cos x}\left[\cos x \cot x - \sin x\ln(\sin x)\right]$$

---

**Example 3 (GATE Level 🟠):** Differentiate $y = \dfrac{x^2\sqrt{1+x}}{(1-x)^3}$.

**Solution:**
$$\ln y = 2\ln|x| + \tfrac{1}{2}\ln(1+x) - 3\ln|1-x|$$
$$\frac{y'}{y} = \frac{2}{x} + \frac{1}{2(1+x)} + \frac{3}{1-x}$$
$$y' = \frac{x^2\sqrt{1+x}}{(1-x)^3}\left[\frac{2}{x} + \frac{1}{2(1+x)} + \frac{3}{1-x}\right]$$

---

### 3.7 📐 Parametric Differentiation ⭐

If $x = x(t)$ and $y = y(t)$, then:
$$\frac{dy}{dx} = \frac{dy/dt}{dx/dt}$$

$$\frac{d^2y}{dx^2} = \frac{d}{dx}\left(\frac{dy}{dx}\right) = \frac{\frac{d}{dt}\left(\frac{dy}{dx}\right)}{dx/dt} = \frac{\dot{x}\ddot{y} - \dot{y}\ddot{x}}{\dot{x}^3}$$

---

**Example:** For $x = t^2$, $y = t^3$, find $\frac{dy}{dx}$ and $\frac{d^2y}{dx^2}$.
$$\frac{dy}{dx} = \frac{3t^2}{2t} = \frac{3t}{2}$$
$$\frac{d^2y}{dx^2} = \frac{d(3t/2)/dt}{2t} = \frac{3/2}{2t} = \frac{3}{4t}$$

---

**Example (Cycloid):** $x = a(t - \sin t)$, $y = a(1 - \cos t)$:
$$\frac{dy}{dx} = \frac{a\sin t}{a(1 - \cos t)} = \frac{\sin t}{1 - \cos t} = \cot\frac{t}{2}$$

---

### 3.8 🔢 Higher-Order Derivatives & $n$-th Derivative Formulas ⭐

| $f(x)$ | $f^{(n)}(x)$ |
|---|---|
| $e^{ax}$ | $a^n e^{ax}$ |
| $\sin(ax+b)$ | $a^n \sin\!\left(ax + b + \frac{n\pi}{2}\right)$ |
| $\cos(ax+b)$ | $a^n \cos\!\left(ax + b + \frac{n\pi}{2}\right)$ |
| $x^m$ ($m \geq n$, integer) | $\dfrac{m!}{(m-n)!}\,x^{m-n}$ |
| $x^m$ ($m < n$) | $0$ |
| $\ln x$ | $\dfrac{(-1)^{n-1}(n-1)!}{x^n}$ |
| $(ax+b)^m$ | $a^n\dfrac{m!}{(m-n)!}(ax+b)^{m-n}$ |

---

### 3.9 ⚡ Leibniz Formula for the $n$-th Derivative of a Product ⭐⭐

$$[f \cdot g]^{(n)} = \sum_{k=0}^{n} \binom{n}{k} f^{(k)} \cdot g^{(n-k)}$$

This is the **calculus analogue of the binomial theorem**! 🎯

---

**Example:** Find $y^{(n)}$ for $y = x^2 e^x$. (Choose $f = e^x$, $g = x^2$)

$f^{(k)} = e^x$; $g^{(0)}=x^2$, $g^{(1)}=2x$, $g^{(2)}=2$, $g^{(k)}=0$ for $k \geq 3$.
$$y^{(n)} = e^x \cdot x^2 + n \cdot e^x \cdot 2x + \binom{n}{2}e^x \cdot 2 = e^x\left[x^2 + 2nx + n(n-1)\right]$$

---

**Example (GATE Trick):** If $y = xe^x$, find $y^{(100)}$.

**Pattern:** $y' = e^x + xe^x = (x+1)e^x$, $y'' = (x+2)e^x$, $y^{(n)} = (x+n)e^x$.

$$\boxed{y^{(100)} = (x+100)e^x}$$

---

### 3.10 🎯 GATE PYQ — Hard Differentiability Problems

**📌 PYQ 10 (GATE CS 2020 Style 🔴 Hard):**

Define $f(x) = x^2\sin(1/x)$ for $x\neq 0$, $f(0)=0$, and $g(x) = x\sin(1/x)$ for $x\neq 0$, $g(0)=0$. Which is differentiable at $x=0$?

**(A)** Only $f$  **(B)** Only $g$  **(C)** Both  **(D)** Neither

**Solution:**
- **$f'(0)$:** $\lim_{h\to 0} h\sin(1/h) = 0$ (Squeeze: $|h\sin(1/h)| \leq |h|$) ✅ Differentiable
- **$g'(0)$:** $\lim_{h\to 0} \sin(1/h)$ — oscillates, **DNE** ✗ Not differentiable

✅ **Answer: (A)** | Difficulty: 🔴 Hard | ⚠️ **Definitional trap** — must use the limit definition, not the formula!

---

**📌 PYQ 11 (GATE Level 🟠 Medium-Hard):**
For $y = e^{\sin^{-1}x}$, find $y'(0)$ and verify $(1-x^2)y'' - xy' - y = 0$.

**Solution:**
$$y' = e^{\sin^{-1}x} \cdot \frac{1}{\sqrt{1-x^2}} \implies y'(0) = e^0 \cdot 1 = \boxed{1}$$

Differentiating again and multiplying through confirms the ODE $(1-x^2)y'' - xy' - y = 0$. ✅

---

**📌 PYQ 12 (GATE CS 🔴 Hard — Implicit Differentiation):**

The slope of $x^3 + y^3 = 6xy$ at $(3, 3)$ is: **(A)** $-1$ **(B)** $0$ **(C)** $1$ **(D)** DNE

**Solution:** $\frac{dy}{dx} = \frac{2y-x^2}{y^2-2x}$. At $(3,3)$: $\frac{6-9}{9-6} = \frac{-3}{3} = -1$. ✅ **Answer: (A)** | Difficulty: 🔴 Hard

---

**📌 PYQ 13 (GATE CS 🔴 Hard — $n$-th Derivative):**

If $y = (x^2-1)^n$, show that $(x^2-1)y^{(n+2)} + 2xy^{(n+1)} - n(n+1)y^{(n)} = 0$.

**Solution (Rodrigues' Formula approach):** Let $y_1 = (x^2-1)^n$. Then $y_1' = 2nx(x^2-1)^{n-1}$.

$(x^2-1)y_1' = 2nxy_1$. Differentiate $(n+1)$ times using Leibniz:
$$\binom{n+1}{0}(x^2-1)y_1^{(n+2)} + \binom{n+1}{1}(2x)y_1^{(n+1)} + \binom{n+1}{2}(2)y_1^{(n)} = 2n\left[\binom{n+1}{0}xy_1^{(n+1)} + (n+1)y_1^{(n)}\right]$$

Simplifying: $(x^2-1)y_1^{(n+2)} + 2(n+1)xy_1^{(n+1)} + n(n+1)y_1^{(n)} = 2nxy_1^{(n+1)} + 2n(n+1)y_1^{(n)}$
$$(x^2-1)y_1^{(n+2)} + 2xy_1^{(n+1)} - n(n+1)y_1^{(n)} = 0 \quad\checkmark$$

This is **Legendre's differential equation**! | Difficulty: 🔴 Very Hard

---

## Chapter 4: Mean Value Theorems (⭐⭐ IMPORTANT FOR GATE)

### 4.1 Rolle's Theorem

> **Theorem:** If $f$ is continuous on $[a, b]$, differentiable on $(a, b)$, and $f(a) = f(b)$, then there exists at least one $c \in (a, b)$ such that $f'(c) = 0$.

**Intuition:** If you start and end at the same height on a smooth curve, somewhere in between there must be a horizontal tangent (a peak or valley).

**Conditions (all three required):**
1. $f$ continuous on $[a, b]$
2. $f$ differentiable on $(a, b)$
3. $f(a) = f(b)$

---

**Example 14:** Verify Rolle's Theorem for $f(x) = x^2 - 4x + 3$ on $[1, 3]$.

**Solution:** 
- $f(1) = 1 - 4 + 3 = 0$, $f(3) = 9 - 12 + 3 = 0$ ✓ ($f(1) = f(3)$)
- $f$ is a polynomial → continuous on $[1,3]$ and differentiable on $(1,3)$ ✓
- $f'(x) = 2x - 4 = 0 \implies x = 2 \in (1, 3)$ ✓

---

### 4.2 Lagrange's Mean Value Theorem (LMVT) (⭐⭐)

> **Theorem:** If $f$ is continuous on $[a, b]$ and differentiable on $(a, b)$, then there exists at least one $c \in (a, b)$ such that:
$$f'(c) = \frac{f(b) - f(a)}{b - a}$$

**Intuition:** At some interior point, the instantaneous rate of change equals the average rate of change over the interval. Think of driving 100 km in 2 hours — your average speed is 50 km/h, and at some moment you must have been going exactly 50 km/h.

**Note:** Rolle's Theorem is a special case of LMVT with $f(a) = f(b)$.

---

**Example 15:** Find $c$ guaranteed by LMVT for $f(x) = x^3$ on $[1, 3]$.

**Solution:**
$$f'(c) = \frac{f(3) - f(1)}{3 - 1} = \frac{27 - 1}{2} = 13$$

$f'(x) = 3x^2$, so $3c^2 = 13 \implies c = \sqrt{13/3} \approx 2.08 \in (1, 3)$ ✓

---

**Example 16 (GATE Level):** If $f$ is continuous on $[0, 2]$, differentiable on $(0, 2)$, $f(0) = 0$, $f(1) = 1$, $f(2) = 1$. Prove that $f'(c) = 0$ for some $c \in (0, 2)$.

**Solution:** Apply LMVT on $[1, 2]$: $\exists\, c_1 \in (1,2)$ such that $f'(c_1) = \frac{f(2)-f(1)}{2-1} = 0$.

We found $c = c_1 \in (0,2)$ with $f'(c) = 0$.

---

### 4.3 Cauchy's Mean Value Theorem

> **Theorem:** If $f, g$ are continuous on $[a, b]$, differentiable on $(a, b)$, and $g'(x) \neq 0$ for all $x \in (a,b)$, then $\exists\, c \in (a, b)$ such that:
$$\frac{f'(c)}{g'(c)} = \frac{f(b) - f(a)}{g(b) - g(a)}$$

**Note:** Setting $g(x) = x$ recovers LMVT.

---

### 4.4 📐 Applications of Mean Value Theorems ⭐⭐

**Application 1: Proving Inequalities**

LMVT is a powerful tool to prove inequalities!

**Example:** Prove $|\sin a - \sin b| \leq |a - b|$ for all $a, b \in \mathbb{R}$.

**Solution:** Apply LMVT to $f(x) = \sin x$ on $[a, b]$ (WLOG $a < b$):

$\exists\, c \in (a,b)$ such that $\frac{\sin b - \sin a}{b - a} = \cos c$.

Since $|\cos c| \leq 1$:
$$|\sin b - \sin a| = |\cos c||b - a| \leq |b - a|$$
✅ QED

---

**Example (GATE Level):** Prove $e^x \geq 1 + x$ for all $x \in \mathbb{R}$.

**Solution:** Let $f(t) = e^t - 1 - t$.

$f(0) = 0$, $f'(t) = e^t - 1$.

- For $t > 0$: $f'(t) > 0$, so $f$ is increasing → $f(t) > f(0) = 0$
- For $t < 0$: $f'(t) < 0$, so $f$ is decreasing → $f(t) > f(0) = 0$ (approaching from right)

Therefore $e^t - 1 - t \geq 0$ for all $t$, i.e., $e^x \geq 1 + x$. ✅

---

**Application 2: Counting Zeros/Roots**

> **Corollary:** If $f'(x) \neq 0$ on $(a, b)$, then $f$ has **at most one root** in $(a, b)$.

**Proof by contradiction:** If $f(x_1) = f(x_2) = 0$ with $x_1 \neq x_2$, by Rolle's theorem $\exists\, c$ with $f'(c) = 0$, contradiction.

---

### 4.5 🌟 Taylor's Theorem with Remainder (⭐⭐ CRITICAL)

> **Taylor's Theorem:** If $f$ has $(n+1)$ continuous derivatives on $[a, x]$, then:
$$f(x) = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \cdots + \frac{f^{(n)}(a)}{n!}(x-a)^n + R_n(x)$$

**Lagrange Remainder (⭐ Most Important Form):**
$$R_n(x) = \frac{f^{(n+1)}(\xi)}{(n+1)!}(x-a)^{n+1} \quad \text{for some } \xi \in (a, x)$$

**Cauchy Remainder:**
$$R_n(x) = \frac{f^{(n+1)}(\xi)}{n!}(x-\xi)^n(x-a) \quad \text{for some } \xi \in (a, x)$$

> 💡 **Application:** The remainder tells us how much **error** we make when truncating Taylor series after $n$ terms!

**Example (Error Bound):** How many terms of $e^x$ do we need at $x=1$ to get 4 decimal places accuracy?

$e^x = 1 + x + \frac{x^2}{2!} + \cdots + \frac{x^n}{n!} + R_n$

$R_n(1) = \frac{e^\xi}{(n+1)!}$ for some $\xi \in (0,1)$. Since $e^\xi < e < 3$:

$|R_n(1)| < \frac{3}{(n+1)!}$

For 4 decimal places: $\frac{3}{(n+1)!} < 0.00005 \implies (n+1)! > 60000 \implies n \geq 8$.

---

### 4.6 🎯 GATE PYQ — Hard MVT Problems

**📌 PYQ 14 (GATE CS 2019 Style 🟠 Medium):**

For $f(x) = x^2$ on $[1, 3]$, the value of $c$ guaranteed by LMVT is:

**(A)** $1$ **(B)** $2$ **(C)** $2.5$ **(D)** $\sqrt{2}$

**Solution:** $f'(c) = \frac{f(3)-f(1)}{3-1} = \frac{9-1}{2} = 4 \implies 2c = 4 \implies c = 2$. ✅ **Answer: (B)**

---

**📌 PYQ 15 (GATE CS 🔴 Hard):**

Let $f: [0,1] \to \mathbb{R}$ be continuous and differentiable on $(0,1)$ with $f(0) = 0$ and $f(1) = 0$, and $f(1/2) = 1$. Which MUST be true?

**(A)** $f'(c) = 0$ for some $c \in (0,1)$ 
**(B)** $f'(c) = 2$ for some $c \in (0, 1/2)$ 
**(C)** $f'(c) = -2$ for some $c \in (1/2, 1)$
**(D)** All of the above

**Solution:**
- **(A):** By Rolle's theorem on $[0,1]$ (since $f(0)=f(1)=0$): $\exists c$ with $f'(c)=0$. ✅
- **(B):** By LMVT on $[0, 1/2]$: $f'(c_1) = \frac{f(1/2)-f(0)}{1/2-0} = \frac{1}{1/2} = 2$ for some $c_1 \in (0, 1/2)$. ✅
- **(C):** By LMVT on $[1/2, 1]$: $f'(c_2) = \frac{f(1)-f(1/2)}{1-1/2} = \frac{-1}{1/2} = -2$ for some $c_2 \in (1/2,1)$. ✅

✅ **Answer: (D)** | Difficulty: 🔴 Hard

---

**📌 PYQ 16 (GATE DA Style 🔴 Hard):**

If $f$ is twice differentiable on $[a,b]$ and $f(a) = f(b) = 0$, prove $\exists\, \xi \in (a,b)$ such that:
$$f(\xi) = \frac{(a-\xi)(\xi-b)}{2} f''(\xi)$$

**Solution:** Apply Taylor's theorem at $\xi \in (a,b)$ expanding about $a$:
$$f(b) = f(a) + f'(a)(b-a) + \frac{f''(\xi_1)}{2}(b-a)^2$$

Using $f(a)=f(b)=0$: $f'(a) = -\frac{(b-a)}{2}f''(\xi_1)$.

Now expanding $f(\xi)$ about $a$: $f(\xi) = f(a) + f'(a)(\xi-a) + \frac{f''(\xi_2)}{2}(\xi-a)^2$

$= -\frac{(b-a)}{2}f''(\xi_1)(\xi-a) + \frac{f''(\xi_2)}{2}(\xi-a)^2$

The full rigorous proof uses a specially constructed auxiliary function. | Difficulty: 🔴 Very Hard

---

### 5.1 Critical Points

$x = c$ is a **critical point** of $f$ if $f'(c) = 0$ or $f'(c)$ does not exist (but $f(c)$ is defined).

### 5.2 First Derivative Test

For a critical point $x = c$ where $f'(c) = 0$:

| Sign change of $f'$ | Conclusion |
|---|---|
| $f'$ changes from $+$ to $-$ at $c$ | **Local maximum** at $c$ |
| $f'$ changes from $-$ to $+$ at $c$ | **Local minimum** at $c$ |
| $f'$ does not change sign | **Neither** (inflection point) |

### 5.3 Second Derivative Test

For a critical point $x = c$ where $f'(c) = 0$:

| Condition | Conclusion |
|---|---|
| $f''(c) > 0$ | **Local minimum** (concave up) |
| $f''(c) < 0$ | **Local maximum** (concave down) |
| $f''(c) = 0$ | **Test inconclusive** — use first derivative test or higher derivatives |

**Intuition:** Concave up ($f'' > 0$) = "holds water" = minimum. Concave down ($f'' < 0$) = "spills water" = maximum.

### 5.4 Global (Absolute) Extrema on Closed Intervals

For $f$ continuous on $[a, b]$:
1. Find all critical points in $(a, b)$
2. Evaluate $f$ at critical points and endpoints $a, b$
3. The largest value is the **global maximum**, the smallest is the **global minimum**

---

**Example 17 (GATE Level):** Find the maximum and minimum of $f(x) = 2x^3 - 9x^2 + 12x$ on $[0, 3]$.

**Solution:**
$f'(x) = 6x^2 - 18x + 12 = 6(x^2 - 3x + 2) = 6(x-1)(x-2)$

Critical points: $x = 1, 2$ (both in $[0, 3]$).

| $x$ | $f(x)$ |
|---|---|
| $0$ | $0$ |
| $1$ | $2 - 9 + 12 = 5$ |
| $2$ | $16 - 36 + 24 = 4$ |
| $3$ | $54 - 81 + 36 = 9$ |

**Global maximum** = $9$ at $x = 3$. **Global minimum** = $0$ at $x = 0$.

---

**Example 18:** Find local extrema of $f(x) = x^4 - 4x^3 + 6x^2 - 4x + 1$.

**Solution:** $f(x) = (x-1)^4$. Then $f'(x) = 4(x-1)^3$, so $x = 1$ is a critical point.

$f''(x) = 12(x-1)^2$, so $f''(1) = 0$ — **second derivative test is inconclusive**.

Use first derivative test: $f'(x) = 4(x-1)^3$ does not change sign (negative for $x < 1$, positive for $x > 1$... wait: $(x-1)^3 < 0$ for $x < 1$ and $(x-1)^3 > 0$ for $x > 1$). Sign changes $-$ to $+$ → **local minimum** at $x = 1$.

In fact, $f(1) = 0$ is the global minimum and $f(x) \geq 0$ for all $x$.

---

### 5.5 Common Traps in Maxima/Minima

| Trap | Correction |
|---|---|
| Forgetting endpoints for closed intervals | Always check $f(a)$ and $f(b)$ |
| $f'' = 0$ does not mean inflection point | Need $f''$ to change sign |
| Second derivative test inconclusive → stuck | Use first derivative test |
| Critical point where $f'$ DNE | Don't ignore these! e.g., $|x|$ at $x = 0$ |

---

### 5.6 📈 Concavity, Convexity & Points of Inflection ⭐⭐

**Definitions:**
- $f$ is **concave up** (convex) on $(a,b)$ if $f'' > 0$ on $(a,b)$ → graph "holds water" 🥣
- $f$ is **concave down** (concave) on $(a,b)$ if $f'' < 0$ on $(a,b)$ → graph "spills water" 🔻

| Condition | Shape | Memory Aid |
|---|---|---|
| $f'' > 0$ | Concave up ∪ | Happy face 😊 |
| $f'' < 0$ | Concave down ∩ | Sad face 😔 |
| $f'' = 0$ and changes sign | Inflection point | Flex point 💪 |

**Point of Inflection:** A point $x = c$ where the concavity **changes** (from up to down or vice versa).

> ⚠️ **Critical Trap:** $f''(c) = 0$ does NOT guarantee an inflection point — the second derivative must **change sign** at $c$.

**Counter-example:** $f(x) = x^4$. Then $f''(x) = 12x^2$, $f''(0) = 0$ but $f''$ does NOT change sign at $0$ → **no inflection point** at $0$, it's actually a minimum!

---

**Example (Inflection Point):** Find inflection points of $f(x) = x^3 - 6x^2 + 12x - 5$.

$f'(x) = 3x^2 - 12x + 12$, $f''(x) = 6x - 12 = 0 \Rightarrow x = 2$.

Check sign change: $f''(x) < 0$ for $x < 2$, $f''(x) > 0$ for $x > 2$. ✅ **Inflection point at $x = 2$**.

$f(2) = 8 - 24 + 24 - 5 = 3$. Inflection point: $(2, 3)$.

---

**Example (GATE Level 🟠):** For $f(x) = xe^{-x}$, find all inflection points.

$f'(x) = e^{-x} - xe^{-x} = (1-x)e^{-x}$

$f''(x) = -e^{-x} - (1-x)e^{-x} \cdot (-1) = e^{-x}(x-2)$

$f''(x) = 0 \Rightarrow x = 2$. Sign: $f''<0$ for $x<2$, $f''>0$ for $x>2$. ✅ **Inflection at $x=2$**.

---

### 5.7 ✏️ Systematic Curve Sketching Framework ⭐

For GATE problems asking you to analyze a function fully:

1. **Domain & Range:** Where is $f$ defined?
2. **Intercepts:** $f(0)$ (y-intercept), solve $f(x)=0$ (x-intercepts)
3. **Symmetry:** Even ($f(-x)=f(x)$), Odd ($f(-x)=-f(x)$), periodic
4. **Critical Points:** Solve $f'(x) = 0$, note where $f'$ DNE
5. **Increasing/Decreasing:** Sign of $f'$
6. **Local Extrema:** First or second derivative test
7. **Concavity:** Sign of $f''$
8. **Inflection Points:** Where $f''$ changes sign
9. **Asymptotes:** Vertical ($x\to a$ with $f\to\pm\infty$), horizontal ($x\to\pm\infty$), oblique
10. **Global Extrema:** On given interval or as $x\to\pm\infty$

---

### 5.8 📦 Applied Optimization (GATE Favorite 🎯) ⭐⭐

**Recipe:** 
1. Define variables and write the **objective function** $f$
2. Write all **constraints** as equations
3. Use constraints to reduce to a single-variable optimization
4. Find critical points, verify with second derivative test
5. Check endpoints if on a closed interval

---

**Example (Classic 🟠):** A farmer has 100m of fence and wants to enclose a rectangular area against a straight wall (wall needs no fence). Maximize the area.

**Setup:** Let width = $x$ (two sides against wall) and length = $y$. Constraint: $2x + y = 100 \Rightarrow y = 100 - 2x$.

$A = xy = x(100-2x) = 100x - 2x^2$

$A' = 100 - 4x = 0 \Rightarrow x = 25$m, $y = 50$m. $A'' = -4 < 0$ → maximum.

**Max area = 1250 m²**

---

**Example (Cylinder 🟠):** Find the dimensions of the cylinder of maximum volume inscribed in a sphere of radius $R$.

**Setup:** Cylinder has radius $r$ and half-height $h$. Constraint: $r^2 + h^2 = R^2$.

$V = \pi r^2 (2h) = 2\pi h (R^2 - h^2)$

$\frac{dV}{dh} = 2\pi(R^2 - 3h^2) = 0 \Rightarrow h = R/\sqrt{3}$

$r^2 = R^2 - R^2/3 = 2R^2/3 \Rightarrow r = R\sqrt{2/3}$

**Max volume:** $V = 2\pi \cdot \frac{R}{\sqrt{3}} \cdot \frac{2R^2}{3} = \frac{4\pi R^3}{3\sqrt{3}}$

---

### 5.9 🎯 GATE PYQ — Hard Maxima/Minima Problems

**📌 PYQ 17 (GATE CS 2023 Style 🔴 Hard):**

The minimum value of $f(x) = x^x$ for $x > 0$ occurs at $x = ?$

**(A)** $1/e$ **(B)** $e$ **(C)** $1$ **(D)** $1/2$

**Solution:** $\ln y = x\ln x \Rightarrow y'/y = \ln x + 1 \Rightarrow y' = x^x(\ln x + 1) = 0$

$\ln x + 1 = 0 \Rightarrow x = e^{-1} = 1/e$.

Check: $y'' = x^x(\ln x + 1)^2 + x^x \cdot (1/x) > 0$ at $x=1/e$ → minimum!

✅ **Answer: (A)** | Difficulty: 🔴 Hard

---

**📌 PYQ 18 (GATE DA Style 🔴 Hard):**

Find the point closest to the origin on the parabola $y = x^2 - 4$.

**Solution:** Minimize $D^2 = x^2 + y^2 = x^2 + (x^2-4)^2$.

Let $u = x^2$, $f(u) = u + (u-4)^2 = u^2 - 7u + 16$.

$f'(u) = 2u - 7 = 0 \Rightarrow u = 7/2, x = \pm\sqrt{7/2}$, $y = 7/2 - 4 = -1/2$.

Minimum distance $= \sqrt{7/2 + 1/4} = \sqrt{15/4} = \frac{\sqrt{15}}{2}$.

✅ **Answer: $\frac{\sqrt{15}}{2}$** | Difficulty: 🔴 Hard

---

**📌 PYQ 19 (GATE CS 🔴 Hard — Inflection Point Trap):**

For $f(x) = x^4 - 4x^3$, the inflection points are at:

**(A)** Only $x=0$ **(B)** Only $x=2$ **(C)** $x=0$ and $x=2$ **(D)** No inflection points

**Solution:** $f'(x) = 4x^3 - 12x^2$, $f''(x) = 12x^2 - 24x = 12x(x-2)$.

$f''(x) = 0$ at $x=0$ and $x=2$.

Sign of $f''$: $(+)$ for $x<0$, $(-)$ for $0<x<2$, $(+)$ for $x>2$.

- $x=0$: $f''$ changes $(+)\to(-)$ ✅ Inflection point
- $x=2$: $f''$ changes $(-)\to(+)$ ✅ Inflection point

✅ **Answer: (C)** | Difficulty: 🟠 Medium-Hard

---

## Chapter 6: Integration (⭐⭐ FREQUENTLY ASKED)

### 6.1 Indefinite Integration (Antiderivatives)

$\int f(x)\,dx = F(x) + C$ means $F'(x) = f(x)$.

### 6.2 Standard Integrals (MUST MEMORIZE)

| Integral | Result |
|---|---|
| $\int x^n\,dx$ | $\frac{x^{n+1}}{n+1} + C$ (for $n \neq -1$) |
| $\int \frac{1}{x}\,dx$ | $\ln|x| + C$ |
| $\int e^x\,dx$ | $e^x + C$ |
| $\int a^x\,dx$ | $\frac{a^x}{\ln a} + C$ |
| $\int \sin x\,dx$ | $-\cos x + C$ |
| $\int \cos x\,dx$ | $\sin x + C$ |
| $\int \sec^2 x\,dx$ | $\tan x + C$ |
| $\int \csc^2 x\,dx$ | $-\cot x + C$ |
| $\int \sec x \tan x\,dx$ | $\sec x + C$ |
| $\int \frac{1}{1+x^2}\,dx$ | $\tan^{-1}x + C$ |
| $\int \frac{1}{\sqrt{1-x^2}}\,dx$ | $\sin^{-1}x + C$ |

### 6.3 Integration Techniques

**Technique 1: Substitution**

If $\int f(g(x))g'(x)\,dx$, let $u = g(x)$, then $du = g'(x)\,dx$.

**Technique 2: Integration by Parts (⭐)**

$$\int u\,dv = uv - \int v\,du$$

**LIATE Rule** for choosing $u$ (prioritize in this order):
- **L**ogarithmic ($\ln x$)
- **I**nverse trig ($\sin^{-1}x$)
- **A**lgebraic ($x^n$)
- **T**rigonometric ($\sin x$, $\cos x$)
- **E**xponential ($e^x$)

**Technique 3: Partial Fractions**

For rational functions $\frac{P(x)}{Q(x)}$ where $\deg P < \deg Q$, decompose:

| Factor in $Q(x)$ | Partial Fraction Term |
|---|---|
| $(ax + b)$ | $\frac{A}{ax + b}$ |
| $(ax + b)^2$ | $\frac{A}{ax+b} + \frac{B}{(ax+b)^2}$ |
| $(x^2 + bx + c)$ (irreducible) | $\frac{Ax + B}{x^2 + bx + c}$ |

**Technique 4: Trigonometric Substitution**

| Expression | Substitution |
|---|---|
| $\sqrt{a^2 - x^2}$ | $x = a\sin\theta$ |
| $\sqrt{a^2 + x^2}$ | $x = a\tan\theta$ |
| $\sqrt{x^2 - a^2}$ | $x = a\sec\theta$ |

---

### 6.4 Definite Integrals

$$\int_a^b f(x)\,dx = F(b) - F(a)$$

**Properties of Definite Integrals (⭐):**

| Property | Formula |
|---|---|
| Linearity | $\int_a^b [cf(x) + dg(x)]\,dx = c\int_a^b f\,dx + d\int_a^b g\,dx$ |
| Reversal | $\int_a^b f\,dx = -\int_b^a f\,dx$ |
| Splitting | $\int_a^b f\,dx = \int_a^c f\,dx + \int_c^b f\,dx$ |
| Even function ($f(-x) = f(x)$) | $\int_{-a}^{a} f(x)\,dx = 2\int_0^a f(x)\,dx$ |
| Odd function ($f(-x) = -f(x)$) | $\int_{-a}^{a} f(x)\,dx = 0$ |
| $\int_0^a f(x)\,dx$ | $= \int_0^a f(a-x)\,dx$ (King's rule) |
| $\int_0^{2a} f(x)\,dx$ | $= 2\int_0^a f(x)\,dx$ if $f(2a-x) = f(x)$; $= 0$ if $f(2a-x) = -f(x)$ |

### 6.5 Leibniz Rule for Differentiation Under the Integral Sign (⭐)

$$\frac{d}{dx}\int_{a(x)}^{b(x)} f(t)\,dt = f(b(x)) \cdot b'(x) - f(a(x)) \cdot a'(x)$$

---

**Example 19 (Substitution):** Evaluate $\int_0^1 \frac{2x}{1+x^2}\,dx$.

**Solution:** Let $u = 1 + x^2$, $du = 2x\,dx$. When $x=0$, $u=1$; when $x=1$, $u=2$.

$$\int_1^2 \frac{du}{u} = [\ln u]_1^2 = \ln 2 - \ln 1 = \ln 2$$

---

**Example 20 (By Parts):** Evaluate $\int_0^1 x e^x\,dx$.

**Solution:** Let $u = x$, $dv = e^x dx$. Then $du = dx$, $v = e^x$.

$$\int_0^1 xe^x\,dx = [xe^x]_0^1 - \int_0^1 e^x\,dx = (1 \cdot e - 0) - [e^x]_0^1 = e - (e - 1) = 1$$

---

**Example 21 (GATE Level — Odd/Even):** Evaluate $\int_{-1}^{1} x^3 e^{-x^2}\,dx$.

**Solution:** Let $f(x) = x^3 e^{-x^2}$. Check: $f(-x) = (-x)^3 e^{-x^2} = -x^3 e^{-x^2} = -f(x)$.

$f$ is **odd** → $\int_{-1}^{1} f(x)\,dx = 0$.

---

**Example 22 (GATE Level — King's Rule):** Evaluate $\int_0^{\pi/2} \frac{\sin x}{\sin x + \cos x}\,dx$.

**Solution:** Let $I = \int_0^{\pi/2} \frac{\sin x}{\sin x + \cos x}\,dx$.

Using King's rule ($x \to \pi/2 - x$): $I = \int_0^{\pi/2} \frac{\cos x}{\cos x + \sin x}\,dx$.

Adding: $2I = \int_0^{\pi/2} \frac{\sin x + \cos x}{\sin x + \cos x}\,dx = \int_0^{\pi/2} 1\,dx = \frac{\pi}{2}$.

$$I = \frac{\pi}{4}$$

---

**Example 23 (Leibniz Rule):** If $F(x) = \int_0^{x^2} e^{-t^2}\,dt$, find $F'(x)$.

**Solution:** By Leibniz rule:

$$F'(x) = e^{-(x^2)^2} \cdot \frac{d}{dx}(x^2) - e^{-0^2} \cdot \frac{d}{dx}(0) = e^{-x^4} \cdot 2x = 2x e^{-x^4}$$

---

### 6.6 Common Mistakes in Integration

| Mistake | Correction |
|---|---|
| Forgetting the constant $C$ in indefinite integrals | Always write $+ C$ |
| Wrong substitution bounds | Change limits when substituting |
| Applying odd/even without checking | Verify the symmetry condition carefully |
| Forgetting chain rule in Leibniz | Multiply by derivative of the upper/lower limit |

---

### 6.7 📊 Riemann Sums & the Definition of Definite Integral (⭐⭐ GATE DA)

The **Riemann sum** approximates $\int_a^b f(x)\,dx$ by dividing $[a,b]$ into $n$ subintervals of width $\Delta x = \frac{b-a}{n}$:

$$\int_a^b f(x)\,dx = \lim_{n\to\infty} \sum_{i=1}^{n} f(x_i^*)\,\Delta x$$

**Common choices for $x_i^*$:**
- **Right-endpoint:** $x_i^* = a + i\Delta x$ (Right Riemann Sum)
- **Left-endpoint:** $x_i^* = a + (i-1)\Delta x$ (Left Riemann Sum)
- **Midpoint:** $x_i^* = a + (i-\frac{1}{2})\Delta x$

**Standard evaluation (GATE favorite 🎯):**
$$\lim_{n\to\infty} \sum_{i=1}^{n} f\!\left(\frac{i}{n}\right) \cdot \frac{1}{n} = \int_0^1 f(x)\,dx$$

**Key finite summation formulas (⭐ Must Memorize!):**

| Sum | Formula |
|---|---|
| $\sum_{i=1}^n 1$ | $n$ |
| $\sum_{i=1}^n i$ | $\dfrac{n(n+1)}{2}$ |
| $\sum_{i=1}^n i^2$ | $\dfrac{n(n+1)(2n+1)}{6}$ |
| $\sum_{i=1}^n i^3$ | $\left[\dfrac{n(n+1)}{2}\right]^2$ |

---

**Example (GATE Level 🟠):** Evaluate $\lim_{n\to\infty} \dfrac{1}{n}\left[\sqrt{\dfrac{1}{n}} + \sqrt{\dfrac{2}{n}} + \cdots + \sqrt{\dfrac{n}{n}}\right]$.

**Solution:** This is $\lim_{n\to\infty} \frac{1}{n}\sum_{i=1}^n \sqrt{i/n} = \int_0^1 \sqrt{x}\,dx = \left[\frac{2}{3}x^{3/2}\right]_0^1 = \frac{2}{3}$. ✅

---

**Example (GATE Level 🔴 Hard):** Evaluate $\lim_{n\to\infty} \dfrac{1^k + 2^k + \cdots + n^k}{n^{k+1}}$.

**Solution:** $= \lim_{n\to\infty} \dfrac{1}{n}\sum_{i=1}^n \left(\dfrac{i}{n}\right)^k = \int_0^1 x^k\,dx = \dfrac{1}{k+1}$. ✅

---

### 6.8 🏛️ Fundamental Theorem of Calculus (FTC) ⭐⭐⭐

> **FTC Part 1:** If $f$ is continuous on $[a,b]$ and $F(x) = \int_a^x f(t)\,dt$, then $F$ is differentiable on $(a,b)$ and:
$$F'(x) = f(x)$$

> **FTC Part 2 (Evaluation Theorem):** If $f$ is continuous on $[a,b]$ and $G$ is any antiderivative of $f$ (i.e., $G' = f$):
$$\int_a^b f(x)\,dx = G(b) - G(a)$$

**The Big Picture 💡:** FTC is the bridge between differentiation and integration — it says they are **inverse operations**!

$$\frac{d}{dx}\int_a^x f(t)\,dt = f(x) \quad\text{(differentiation undoes integration)}$$

**General version with variable limits (Leibniz Rule):**
$$\frac{d}{dx}\int_{u(x)}^{v(x)} f(t)\,dt = f(v(x))\cdot v'(x) - f(u(x))\cdot u'(x)$$

---

**Example (FTC Part 1):** If $F(x) = \int_0^x \cos(t^2)\,dt$, find $F'(x)$.

**Solution:** $F'(x) = \cos(x^2)$. (Direct application of FTC Part 1.)

---

**Example (GATE Level 🔴 Hard):** If $G(x) = \int_{x^2}^{x^3} \frac{1}{\ln t}\,dt$, find $G'(x)$.

**Solution:** By Leibniz Rule:
$$G'(x) = \frac{1}{\ln(x^3)}\cdot 3x^2 - \frac{1}{\ln(x^2)}\cdot 2x = \frac{3x^2}{3\ln x} - \frac{2x}{2\ln x} = \frac{x^2 - x}{\ln x}$$

---

### 6.9 🚀 Improper Integrals (⭐⭐⭐ GATE CS & DA)

An **improper integral** arises when:
- **Type 1:** Limits of integration are infinite ($a = -\infty$ or $b = \infty$)
- **Type 2:** $f$ has a vertical asymptote (blow-up) inside $[a,b]$

**Type 1 — Infinite Limits:**
$$\int_a^\infty f(x)\,dx = \lim_{b\to\infty} \int_a^b f(x)\,dx$$
$$\int_{-\infty}^\infty f(x)\,dx = \int_{-\infty}^c f(x)\,dx + \int_c^\infty f(x)\,dx$$

**Type 2 — Vertical Asymptote at $c \in [a,b]$:**
$$\int_a^b f(x)\,dx = \lim_{\epsilon\to 0^+} \int_a^{c-\epsilon} f(x)\,dx + \lim_{\epsilon\to 0^+} \int_{c+\epsilon}^b f(x)\,dx$$

**Convergence/Divergence:**

| Integral | Converges? | Condition |
|---|---|---|
| $\int_1^\infty x^{-p}\,dx$ | ✅ Yes iff $p > 1$ | $= \frac{1}{p-1}$ |
| $\int_0^1 x^{-p}\,dx$ | ✅ Yes iff $p < 1$ | $= \frac{1}{1-p}$ |
| $\int_1^\infty e^{-x}\,dx$ | ✅ Yes | $= e^{-1}$ |
| $\int_0^\infty e^{-ax}\,dx$ ($a>0$) | ✅ Yes | $= \frac{1}{a}$ |
| $\int_1^\infty \frac{1}{x}\,dx$ | ❌ No (diverges) | log divergence |
| $\int_0^\infty \sin x\,dx$ | ❌ No | oscillates |

> 🎯 **GATE Trap:** The **$p$-test** for improper integrals changes direction depending on whether the integration goes to $\infty$ or is near a singularity at $0$!
> - Near $0$ ($\int_0^1 x^{-p}$): converges for $p < 1$, diverges for $p \geq 1$
> - At $\infty$ ($\int_1^\infty x^{-p}$): converges for $p > 1$, diverges for $p \leq 1$

---

**Example (Type 1):** Evaluate $\int_0^\infty e^{-x}\,dx$.

$$\lim_{b\to\infty} \int_0^b e^{-x}\,dx = \lim_{b\to\infty} [-e^{-x}]_0^b = \lim_{b\to\infty}(1 - e^{-b}) = 1$$

---

**Example (Type 2 🟠):** Evaluate $\int_0^1 \frac{1}{\sqrt{x}}\,dx$.

$$\lim_{\epsilon\to 0^+} \int_\epsilon^1 x^{-1/2}\,dx = \lim_{\epsilon\to 0^+} [2\sqrt{x}]_\epsilon^1 = 2 - 0 = 2$$ (Converges! $p = 1/2 < 1$)

---

**Example (GATE Level 🔴):** Determine convergence of $\int_1^\infty \frac{\ln x}{x^2}\,dx$.

By parts: $u = \ln x$, $dv = x^{-2}\,dx \Rightarrow v = -x^{-1}$:
$$\left[-\frac{\ln x}{x}\right]_1^\infty + \int_1^\infty \frac{1}{x^2}\,dx = 0 + [-1/x]_1^\infty = 0 + 1 = 1$$

(Note: $\frac{\ln x}{x} \to 0$ as $x\to\infty$.) ✅ **Converges to 1.**

---

**Comparison Test for Improper Integrals (⭐):**

If $0 \leq f(x) \leq g(x)$ for $x \geq a$:
- If $\int_a^\infty g\,dx$ converges → $\int_a^\infty f\,dx$ **converges**
- If $\int_a^\infty f\,dx$ diverges → $\int_a^\infty g\,dx$ **diverges**

---

### 6.10 🔬 Gamma Function (⭐⭐⭐ GATE DA Essential)

$$\Gamma(n) = \int_0^\infty x^{n-1}e^{-x}\,dx \quad \text{for } n > 0$$

**Key Properties (⭐ Must Memorize!):**

| Property | Formula |
|---|---|
| Factorial relation | $\Gamma(n+1) = n\,\Gamma(n)$ |
| Integer values | $\Gamma(n) = (n-1)!$ for positive integer $n$ |
| Half-integer | $\Gamma(1/2) = \sqrt{\pi}$ |
| Extended recursion | $\Gamma(3/2) = \frac{1}{2}\sqrt{\pi}$, $\Gamma(5/2) = \frac{3}{4}\sqrt{\pi}$ |
| Reflection formula | $\Gamma(n)\Gamma(1-n) = \frac{\pi}{\sin(n\pi)}$ |

**Derivation of $\Gamma(1/2) = \sqrt{\pi}$:**
$$\Gamma(1/2) = \int_0^\infty x^{-1/2}e^{-x}\,dx \stackrel{x=t^2}{=} 2\int_0^\infty e^{-t^2}\,dt = \int_{-\infty}^\infty e^{-t^2}\,dt = \sqrt{\pi}$$

(The last integral is the **Gaussian integral** — fundamental in probability/ML!)

---

**Example:** Evaluate $\int_0^\infty x^3 e^{-x}\,dx$.

**Solution:** $= \Gamma(4) = 3! = 6$. ✅

---

**Example (GATE DA 🔴):** Evaluate $\int_0^\infty x^{3/2}e^{-x}\,dx$.

**Solution:** $= \Gamma(5/2) = \frac{3}{2}\Gamma(3/2) = \frac{3}{2} \cdot \frac{1}{2}\Gamma(1/2) = \frac{3}{4}\sqrt{\pi}$. ✅

---

### 6.11 🧪 Beta Function (⭐⭐ GATE DA)

$$B(m, n) = \int_0^1 x^{m-1}(1-x)^{n-1}\,dx \quad \text{for } m, n > 0$$

**Relation to Gamma:**
$$\boxed{B(m, n) = \frac{\Gamma(m)\Gamma(n)}{\Gamma(m+n)}}$$

**Alternative forms:**
$$B(m,n) = 2\int_0^{\pi/2} \sin^{2m-1}\theta\cos^{2n-1}\theta\,d\theta$$

**Symmetry:** $B(m,n) = B(n,m)$

---

**Example:** Evaluate $\int_0^1 x^3(1-x)^4\,dx$.

**Solution:** $= B(4,5) = \dfrac{\Gamma(4)\Gamma(5)}{\Gamma(9)} = \dfrac{3! \cdot 4!}{8!} = \dfrac{6\cdot 24}{40320} = \dfrac{144}{40320} = \dfrac{1}{280}$

---

### 6.12 🌊 Wallis Formula & Reduction Formulae (⭐ DA)

**Wallis Formula:**
$$\int_0^{\pi/2} \sin^n x\,dx = \int_0^{\pi/2} \cos^n x\,dx = \begin{cases} \dfrac{(n-1)!!}{n!!}\cdot\dfrac{\pi}{2} & n \text{ even} \\[10pt] \dfrac{(n-1)!!}{n!!} & n \text{ odd}\end{cases}$$

where $n!! = n(n-2)(n-4)\cdots$

**Key Values:**

| $n$ | $\int_0^{\pi/2}\sin^n x\,dx$ |
|---|---|
| $0$ | $\pi/2$ |
| $1$ | $1$ |
| $2$ | $\pi/4$ |
| $3$ | $2/3$ |
| $4$ | $3\pi/16$ |
| $n$ (even) | $\frac{(n-1)!!\cdot\pi}{n!!\cdot 2}$ |

**Reduction Formula for $I_n = \int_0^{\pi/2}\sin^n x\,dx$:**
$$I_n = \frac{n-1}{n} I_{n-2}$$

---

**Example (GATE Level 🟠):** Evaluate $\int_0^{\pi/2}\sin^5 x\,dx$.

$I_5 = \frac{4}{5}I_3 = \frac{4}{5}\cdot\frac{2}{3}I_1 = \frac{4}{5}\cdot\frac{2}{3}\cdot 1 = \dfrac{8}{15}$

---

### 6.13 🎯 GATE PYQ — Hard Integration Problems

**📌 PYQ 20 (GATE CS 2022 🔴 Hard — Riemann Sum):**

$\lim_{n\to\infty} \left(\dfrac{1}{n+1} + \dfrac{1}{n+2} + \cdots + \dfrac{1}{2n}\right) = ?$

**Solution:**
$$= \lim_{n\to\infty} \sum_{k=1}^n \frac{1}{n+k} = \lim_{n\to\infty} \frac{1}{n}\sum_{k=1}^n \frac{1}{1+k/n} = \int_0^1 \frac{dx}{1+x} = \ln 2$$

✅ **Answer: $\ln 2$** | Difficulty: 🔴 Hard | ⚠️ **Trap:** Don't try to evaluate the telescoping sum — recognize it as a Riemann sum for $\int_0^1 \frac{1}{1+x}\,dx$!

---

**📌 PYQ 21 (GATE CS 2020 Actual 🟠 Medium — King's Rule):**

$\int_0^\pi x\sin x\,dx = ?$

**Solution (Method 1 — Integration by Parts):**
$u=x$, $dv=\sin x\,dx$: $= [-x\cos x]_0^\pi + \int_0^\pi \cos x\,dx = \pi + [\sin x]_0^\pi = \pi + 0 = \pi$

**Solution (Method 2 — King's Rule — Shorter!):**
Let $I = \int_0^\pi x\sin x\,dx$. By King's rule ($x\to\pi-x$): $I = \int_0^\pi (\pi-x)\sin(\pi-x)\,dx = \int_0^\pi (\pi-x)\sin x\,dx$

$2I = \pi\int_0^\pi \sin x\,dx = \pi[-\cos x]_0^\pi = \pi[1+1] = 2\pi \Rightarrow I = \pi$

✅ **Answer: $\pi$** | Difficulty: 🟠 Medium

---

**📌 PYQ 22 (GATE DA 🔴 Hard — Improper Integral):**

For what values of $p$ does $\int_0^\infty \dfrac{x^{p-1}}{1+x}\,dx$ converge?

**Solution:** Near $x=0$: $\frac{x^{p-1}}{1+x} \approx x^{p-1}$, converges if $p-1 > -1$, i.e., $p > 0$.

Near $x=\infty$: $\frac{x^{p-1}}{1+x} \approx x^{p-2}$, converges if $p-2 < -1$, i.e., $p < 1$.

✅ **Converges for $0 < p < 1$**. (In fact $= \frac{\pi}{\sin(\pi p)}$ by the Beta function connection.) | Difficulty: 🔴 Hard

---

**📌 PYQ 23 (GATE CS 🔴 Very Hard — LR + Integration):**

$\int_0^1 \ln x \cdot \ln(1-x)\,dx = ?$

**Solution (Approach via series):**
$\ln(1-x) = -\sum_{n=1}^\infty \frac{x^n}{n}$

$\int_0^1 \ln x \cdot (-x^n)\,dx = -\int_0^1 x^n\ln x\,dx$

By parts: $\int_0^1 x^n \ln x\,dx = \left[\frac{x^{n+1}\ln x}{n+1}\right]_0^1 - \int_0^1 \frac{x^n}{n+1}\,dx = 0 - \frac{1}{(n+1)^2}$

So: $\int_0^1 \ln x\ln(1-x)\,dx = \sum_{n=1}^\infty \frac{1}{n}\cdot\frac{1}{(n+1)^2}$ — advanced; evaluates to $2 - \frac{\pi^2}{6}$.

| Difficulty: 🔴 Very Hard — expected to know setup, not memorize final answer.

---

**📌 PYQ 24 (GATE CS 2016 Actual Style 🟠 Medium):**

Evaluate $\int_0^{\pi/2} \dfrac{\cos x}{\sin x + \cos x}\,dx$.

**Solution (King's Rule):** Let $I = \int_0^{\pi/2}\dfrac{\cos x}{\sin x+\cos x}\,dx$.

By King ($x\to\frac{\pi}{2}-x$): $I = \int_0^{\pi/2}\dfrac{\sin x}{\cos x+\sin x}\,dx$.

Adding: $2I = \int_0^{\pi/2} 1\,dx = \pi/2 \Rightarrow I = \pi/4$.

✅ **Answer: $\pi/4$** | Difficulty: 🟠 Medium

---

---

## Chapter 7: Taylor Series (⭐)

### 7.1 Taylor Series (Single Variable)

The **Taylor series** of $f(x)$ about $x = a$ is:

$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!}(x-a)^n = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \cdots$$

When $a = 0$, this is called the **Maclaurin series**.

### 7.2 Important Maclaurin Series (⭐ MUST MEMORIZE)

| Function | Series | Valid for |
|---|---|---|
| $e^x$ | $1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots$ | All $x$ |
| $\sin x$ | $x - \frac{x^3}{3!} + \frac{x^5}{5!} - \cdots$ | All $x$ |
| $\cos x$ | $1 - \frac{x^2}{2!} + \frac{x^4}{4!} - \cdots$ | All $x$ |
| $\ln(1+x)$ | $x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots$ | $-1 < x \leq 1$ |
| $\frac{1}{1-x}$ | $1 + x + x^2 + x^3 + \cdots$ | $|x| < 1$ |
| $\frac{1}{1+x}$ | $1 - x + x^2 - x^3 + \cdots$ | $|x| < 1$ |
| $(1+x)^n$ | $1 + nx + \frac{n(n-1)}{2!}x^2 + \cdots$ | $|x| < 1$ |
| $\tan^{-1}x$ | $x - \frac{x^3}{3} + \frac{x^5}{5} - \cdots$ | $|x| \leq 1$ |

### 7.3 Applications of Taylor Series

1. **Evaluating limits:** Expand numerator and denominator, then simplify.
2. **Approximation:** Truncate the series for polynomial approximation.
3. **Finding coefficients:** Match terms to identify function behavior.

---

**Example 24:** Evaluate $\lim_{x \to 0} \frac{e^x - 1 - x}{x^2}$ using Taylor series.

**Solution:** $e^x = 1 + x + \frac{x^2}{2} + \frac{x^3}{6} + \cdots$

$$\frac{e^x - 1 - x}{x^2} = \frac{\frac{x^2}{2} + \frac{x^3}{6} + \cdots}{x^2} = \frac{1}{2} + \frac{x}{6} + \cdots \to \frac{1}{2}$$

---

**Example 25 (GATE Level):** Find the coefficient of $x^5$ in the Taylor expansion of $f(x) = e^x \sin x$ about $x = 0$.

**Solution:**
$$e^x = 1 + x + \frac{x^2}{2} + \frac{x^3}{6} + \frac{x^4}{24} + \frac{x^5}{120} + \cdots$$
$$\sin x = x - \frac{x^3}{6} + \frac{x^5}{120} + \cdots$$

Multiply and collect $x^5$ terms:
- $1 \cdot \frac{x^5}{120}$: coefficient $\frac{1}{120}$
- $x \cdot \left(-\frac{x^4}{...}\right)$: no $x^4$ term in $\sin x$... Let's be systematic.

$\sin x = x - \frac{x^3}{6} + \frac{x^5}{120}$

Coefficient of $x^5$: $(1)\left(\frac{1}{120}\right) + (1)\left(-\frac{1}{6}\right)\left(\frac{1}{2}\right) + \left(\frac{1}{6}\right)(1)\left(\frac{1}{...}\right)$

More carefully, using the Cauchy product for $e^x \sin x = \sum c_n x^n$ where $c_n = \sum_{k=0}^{n} a_k b_{n-k}$:

$a_k = \frac{1}{k!}$ (from $e^x$), $b_k$: coefficients of $\sin x$ are $b_1 = 1, b_3 = -\frac{1}{6}, b_5 = \frac{1}{120}$, others zero.

$$c_5 = a_0 b_5 + a_1 b_4 + a_2 b_3 + a_3 b_2 + a_4 b_1 + a_5 b_0$$
$$= (1)\frac{1}{120} + (1)(0) + \frac{1}{2}\left(-\frac{1}{6}\right) + \frac{1}{6}(0) + \frac{1}{24}(1) + \frac{1}{120}(0)$$
$$= \frac{1}{120} - \frac{1}{12} + \frac{1}{24} = \frac{1}{120} - \frac{10}{120} + \frac{5}{120} = -\frac{4}{120} = -\frac{1}{30}$$

Coefficient of $x^5$ is $-\frac{1}{30}$.

---

### 7.4 Multivariate Taylor Series

For $f(x, y)$ about $(a, b)$:

$$f(x,y) = f(a,b) + f_x(a,b)(x-a) + f_y(a,b)(y-b) + \frac{1}{2!}\left[f_{xx}(a,b)(x-a)^2 + 2f_{xy}(a,b)(x-a)(y-b) + f_{yy}(a,b)(y-b)^2\right] + \cdots$$

In compact notation using gradient and Hessian:

$$f(\mathbf{x}) \approx f(\mathbf{a}) + \nabla f(\mathbf{a})^T(\mathbf{x} - \mathbf{a}) + \frac{1}{2}(\mathbf{x} - \mathbf{a})^T H(\mathbf{a})(\mathbf{x} - \mathbf{a})$$

where $H$ is the **Hessian matrix** of second partial derivatives.

---

### 7.5 📡 Power Series (⭐⭐ DA Extension)

A **power series** centered at $a$:
$$\sum_{n=0}^\infty c_n(x-a)^n = c_0 + c_1(x-a) + c_2(x-a)^2 + \cdots$$

**Radius of Convergence $R$:** The series converges absolutely for $|x-a| < R$ and diverges for $|x-a| > R$.

**Finding $R$ via Ratio Test:**
$$R = \lim_{n\to\infty}\left|\frac{c_n}{c_{n+1}}\right| \quad \text{(if limit exists)}$$

Or Root Test:
$$\frac{1}{R} = \limsup_{n\to\infty} |c_n|^{1/n}$$

**Behavior at endpoints $x = a \pm R$:** Must check separately (may converge, diverge, or conditionally converge).

---

**Radii of Convergence for Standard Series:**

| Series | Radius $R$ | Interval of Convergence |
|---|---|---|
| $e^x = \sum \frac{x^n}{n!}$ | $\infty$ | $(-\infty, \infty)$ |
| $\sin x = \sum \frac{(-1)^n x^{2n+1}}{(2n+1)!}$ | $\infty$ | $(-\infty, \infty)$ |
| $\frac{1}{1-x} = \sum x^n$ | $1$ | $(-1, 1)$ |
| $\ln(1+x) = \sum \frac{(-1)^{n+1}x^n}{n}$ | $1$ | $(-1, 1]$ |
| $\tan^{-1}x = \sum \frac{(-1)^n x^{2n+1}}{2n+1}$ | $1$ | $[-1, 1]$ |
| $(1+x)^\alpha = \sum \binom{\alpha}{n}x^n$ | $1$ | $(-1,1)$ (varies at endpoints) |

---

**Example (Radius of Convergence):** Find the radius of convergence of $\sum_{n=1}^\infty \frac{x^n}{n}$.

$$R = \lim_{n\to\infty}\left|\frac{c_n}{c_{n+1}}\right| = \lim_{n\to\infty}\frac{1/n}{1/(n+1)} = \lim_{n\to\infty}\frac{n+1}{n} = 1$$

- At $x=1$: $\sum \frac{1}{n}$ diverges (harmonic series)
- At $x=-1$: $\sum \frac{(-1)^n}{n}$ converges (alternating series test)

**Interval of convergence:** $[-1, 1)$.

---

**Term-by-term Differentiation/Integration (⭐):**

If $\sum c_n(x-a)^n$ has radius $R$, then:
$$\left(\sum_{n=0}^\infty c_n(x-a)^n\right)' = \sum_{n=1}^\infty nc_n(x-a)^{n-1} \quad \text{(same radius } R)$$
$$\int\left(\sum_{n=0}^\infty c_n(x-a)^n\right)dx = \sum_{n=0}^\infty \frac{c_n(x-a)^{n+1}}{n+1} + C \quad \text{(same radius } R)$$

---

### 7.6 🔢 Taylor's Remainder & Error Bounds (⭐⭐)

**Taylor's Theorem with Lagrange Remainder:**
$$f(x) = \sum_{k=0}^n \frac{f^{(k)}(a)}{k!}(x-a)^k + R_n(x)$$

$$R_n(x) = \frac{f^{(n+1)}(\xi)}{(n+1)!}(x-a)^{n+1}$$

for some $\xi$ between $a$ and $x$.

**Using Remainder:**

If $|f^{(n+1)}(x)| \leq M$ on the interval, then:
$$|R_n(x)| \leq \frac{M}{(n+1)!}|x-a|^{n+1}$$

---

**Example (Error Bound 🟠):** How accurately does $\cos x \approx 1 - \frac{x^2}{2}$ approximate $\cos(0.1)$?

$R_2(0.1) = \frac{f^{(3)}(\xi)}{3!}(0.1)^3$ where $f^{(3)} = \sin\xi$.

$|R_2| \leq \frac{1}{6}(0.1)^3 = \frac{0.001}{6} \approx 0.000167$

So $\cos(0.1) \approx 1 - 0.005 = 0.995$ with error $< 0.0002$. ✅

---

### 7.7 🎯 GATE PYQ — Hard Taylor/Series Problems

**📌 PYQ 25 (GATE CS 2022 🔴 Hard):**

The coefficient of $x^4$ in the Maclaurin series of $\dfrac{x}{\sinh x}$ is... 

*(Requires $\sinh x = x + x^3/6 + x^5/120 + \cdots$, so $\frac{x}{\sinh x} = 1 - x^2/6 + 7x^4/360 - \cdots$)*

**Answer:** $\dfrac{7}{360}$ | Difficulty: 🔴 Very Hard — requires polynomial long division of power series.

---

**📌 PYQ 26 (GATE DA 🔴 Hard):**

Find the sum $\sum_{n=1}^\infty \dfrac{n}{2^n}$.

**Solution:** Let $S = \sum_{n=1}^\infty nx^n$. We know $\sum_{n=0}^\infty x^n = \frac{1}{1-x}$ for $|x|<1$.

Differentiating: $\sum_{n=1}^\infty nx^{n-1} = \frac{1}{(1-x)^2}$, so $\sum_{n=1}^\infty nx^n = \frac{x}{(1-x)^2}$.

At $x = 1/2$: $S = \dfrac{1/2}{(1/2)^2} = \dfrac{1/2}{1/4} = 2$. ✅ | Difficulty: 🔴 Hard

---

**📌 PYQ 27 (GATE CS 2021 Style 🟠 Medium):**

Using Taylor series, evaluate $\lim_{x\to 0} \dfrac{\cos x - 1 + x^2/2}{x^4}$.

**Solution:** $\cos x = 1 - \frac{x^2}{2} + \frac{x^4}{24} - \cdots$

$\cos x - 1 + \frac{x^2}{2} = \frac{x^4}{24} - \frac{x^6}{720} + \cdots$

$$\lim_{x\to 0}\frac{x^4/24 + O(x^6)}{x^4} = \frac{1}{24}$$

✅ **Answer: $1/24$** | Difficulty: 🟠 Medium

---

**📌 PYQ 28 (GATE CS 🔴 Hard — Radius of Convergence):**

The interval of convergence of $\sum_{n=0}^\infty \dfrac{(-1)^n x^{2n}}{4^n}$ is:

**(A)** $(-2,2)$ **(B)** $[-2,2]$ **(C)** $(-2,2]$ **(D)** $\{0\}$

**Solution:** Let $u = x^2/4$. Series is $\sum(-u)^n = \frac{1}{1+u}$ for $|u| < 1$.

$|x^2/4| < 1 \Rightarrow |x| < 2$. At $|x|=2$: $|u|=1$, $\sum(-1)^n$ diverges.

✅ **Answer: (A) $(-2,2)$** | Difficulty: 🟠 Medium-Hard | ⚠️ **Trap:** Checking endpoints separately is mandatory!

---

## Chapter 8: Partial Differentiation & Chain Rule (⭐⭐)

### 8.1 Partial Derivatives

For $f(x, y)$, the **partial derivative with respect to $x$** treats $y$ as a constant:

$$f_x = \frac{\partial f}{\partial x} = \lim_{h \to 0} \frac{f(x+h, y) - f(x, y)}{h}$$

Similarly, $f_y = \frac{\partial f}{\partial y}$ treats $x$ as a constant.

**Real-world analogy:** If temperature $T(x, y)$ depends on location: $\frac{\partial T}{\partial x}$ is the rate of temperature change as you walk east (keeping your north-south position fixed).

### 8.2 Higher-Order Partial Derivatives

$$f_{xx} = \frac{\partial^2 f}{\partial x^2}, \quad f_{yy} = \frac{\partial^2 f}{\partial y^2}, \quad f_{xy} = \frac{\partial^2 f}{\partial y \partial x}, \quad f_{yx} = \frac{\partial^2 f}{\partial x \partial y}$$

> **Clairaut's Theorem:** If $f_{xy}$ and $f_{yx}$ are both continuous, then $f_{xy} = f_{yx}$.

### 8.3 Chain Rule for Multivariable Functions (⭐)

**Case 1:** $f(x, y)$ where $x = x(t)$, $y = y(t)$:
$$\frac{df}{dt} = \frac{\partial f}{\partial x}\frac{dx}{dt} + \frac{\partial f}{\partial y}\frac{dy}{dt}$$

**Case 2:** $f(x, y)$ where $x = x(s, t)$, $y = y(s, t)$:
$$\frac{\partial f}{\partial s} = \frac{\partial f}{\partial x}\frac{\partial x}{\partial s} + \frac{\partial f}{\partial y}\frac{\partial y}{\partial s}$$
$$\frac{\partial f}{\partial t} = \frac{\partial f}{\partial x}\frac{\partial x}{\partial t} + \frac{\partial f}{\partial y}\frac{\partial y}{\partial t}$$

---

**Example 26:** Find $\frac{\partial f}{\partial x}$ and $\frac{\partial f}{\partial y}$ for $f(x, y) = x^3 + 3x^2y - y^3$.

**Solution:**
$$f_x = 3x^2 + 6xy, \quad f_y = 3x^2 - 3y^2$$

---

**Example 27 (Chain Rule):** If $f(x, y) = x^2 + y^2$ and $x = r\cos\theta$, $y = r\sin\theta$, find $\frac{\partial f}{\partial r}$.

**Solution:**
$$\frac{\partial f}{\partial r} = \frac{\partial f}{\partial x}\frac{\partial x}{\partial r} + \frac{\partial f}{\partial y}\frac{\partial y}{\partial r} = 2x \cdot \cos\theta + 2y \cdot \sin\theta$$
$$= 2r\cos^2\theta + 2r\sin^2\theta = 2r$$

---

**Example 28 (GATE Level):** If $f(x, y) = e^{xy}$, find $f_{xy}$.

**Solution:**
$$f_x = ye^{xy}$$
$$f_{xy} = \frac{\partial}{\partial y}(ye^{xy}) = e^{xy} + y \cdot xe^{xy} = e^{xy}(1 + xy)$$

---

### 8.4 📏 Total Differential (⭐⭐)

The **total differential** of $f(x, y)$ is:
$$df = \frac{\partial f}{\partial x}dx + \frac{\partial f}{\partial y}dy$$

For $n$ variables: $df = \sum_{i=1}^n \frac{\partial f}{\partial x_i}dx_i$

**Applications:**
1. **Error/Approximation:** If $\Delta x, \Delta y$ are small changes, then $\Delta f \approx df$
2. **Exact Differentials:** $M\,dx + N\,dy$ is exact iff $\frac{\partial M}{\partial y} = \frac{\partial N}{\partial x}$

---

**Example (Approximation 🟠):** A cylinder has radius $r = 4$cm and height $h = 10$cm. $r$ changes by $+0.1$, $h$ by $-0.2$. Approximate change in volume $V = \pi r^2 h$.

$$dV = \frac{\partial V}{\partial r}dr + \frac{\partial V}{\partial h}dh = 2\pi rh\,dr + \pi r^2\,dh$$
$$\approx 2\pi(4)(10)(0.1) + \pi(16)(-0.2) = 8\pi - 3.2\pi = 4.8\pi \approx 15.08\text{ cm}^3$$

---

**Example (Exact Differential):** Show $\frac{y}{x^2+y^2}dx - \frac{x}{x^2+y^2}dy$ is NOT exact.

$M = \frac{y}{x^2+y^2}$, $N = -\frac{x}{x^2+y^2}$

$\frac{\partial M}{\partial y} = \frac{x^2-y^2}{(x^2+y^2)^2}$, $\frac{\partial N}{\partial x} = -\frac{y^2-x^2}{(x^2+y^2)^2} = \frac{x^2-y^2}{(x^2+y^2)^2}$

Wait — they're equal! So it IS exact (but only on a simply-connected domain excluding origin).

---

### 8.5 🌟 Euler's Theorem for Homogeneous Functions (⭐⭐⭐ GATE SUPER IMPORTANT!)

> ⚡ **Most Frequently Tested Topic in Partial Differentiation at GATE!**

**Definition:** $f(x, y)$ is **homogeneous of degree $n$** if:
$$f(\lambda x, \lambda y) = \lambda^n f(x, y) \quad \text{for all } \lambda \neq 0$$

**Examples:**
- $f(x,y) = x^3 + x^2y + y^3$ — homogeneous of degree 3 (each term has degree 3)
- $f(x,y) = \frac{x^3 + y^3}{x^2 + y^2}$ — homogeneous of degree 1 (numerator degree 3, denominator degree 2, difference = 1)
- $f(x,y) = x^2 + y^2 + xy$ — homogeneous of degree 2
- $f(x,y) = x^2 + xy + 1$ — **NOT homogeneous** (constant term has degree 0)

---

> **Euler's Theorem:** If $f(x, y)$ is homogeneous of degree $n$ and differentiable, then:
$$\boxed{x\frac{\partial f}{\partial x} + y\frac{\partial f}{\partial y} = n\,f(x,y)}$$

**For $n$ variables:**
$$\sum_{i=1}^{n} x_i\frac{\partial f}{\partial x_i} = n\,f(x_1, x_2, \ldots, x_n)$$

---

**Corollary (Second-order extension):**

If $f$ is homogeneous of degree $n$, then $\frac{\partial f}{\partial x}$, $\frac{\partial f}{\partial y}$ are homogeneous of degree $(n-1)$.

**Corollary (Higher-order derivatives):**

$$x^2 f_{xx} + 2xy f_{xy} + y^2 f_{yy} = n(n-1)f$$

---

**Example 1 (Direct Application):** If $f(x,y) = x^3 + y^3 - 3xy^2$, verify Euler's theorem.

$f$ is homogeneous of degree 3 ($\lambda^3(x^3+y^3-3xy^2)$). ✅

$f_x = 3x^2 - 3y^2$, $f_y = 3y^2 - 3x\cdot 2y = 3y^2 - 6xy$

Wait: $f_y = 3y^2 - 6xy$

$xf_x + yf_y = x(3x^2-3y^2) + y(3y^2-6xy) = 3x^3 - 3xy^2 + 3y^3 - 6xy^2 = 3(x^3+y^3-3xy^2) = 3f$ ✅

---

**Example 2 (Tricky 🔴):** If $u = \sin^{-1}\!\left(\dfrac{x+y}{\sqrt{x}+\sqrt{y}}\right)$, find $x\frac{\partial u}{\partial x} + y\frac{\partial u}{\partial y}$.

**Solution:** Let $v = \sin u = \dfrac{x+y}{\sqrt{x}+\sqrt{y}}$.

Check degree of $v$: $v(\lambda x, \lambda y) = \dfrac{\lambda x + \lambda y}{\sqrt{\lambda x}+\sqrt{\lambda y}} = \dfrac{\lambda(x+y)}{\sqrt{\lambda}(\sqrt{x}+\sqrt{y})} = \lambda^{1/2} v$.

So $v$ is homogeneous of degree $\frac{1}{2}$. By Euler's theorem:

$x\dfrac{\partial v}{\partial x} + y\dfrac{\partial v}{\partial y} = \dfrac{1}{2}v = \dfrac{1}{2}\sin u$

Now, $\dfrac{\partial v}{\partial x} = \cos u \cdot \dfrac{\partial u}{\partial x}$, similarly for $y$.

$\cos u \left(x\dfrac{\partial u}{\partial x} + y\dfrac{\partial u}{\partial y}\right) = \dfrac{\sin u}{2}$

$$\boxed{x\frac{\partial u}{\partial x} + y\frac{\partial u}{\partial y} = \frac{\tan u}{2}}$$

⚠️ **GATE TRAP:** When $f(u)$ is given and $u$ has a homogeneous inner expression, apply Euler's to the inner function, then chain rule back!

---

**Example 3 (Euler's Theorem — Advanced 🔴):** If $u = f\!\left(\dfrac{y}{x}\right)$, show $x\dfrac{\partial u}{\partial x} + y\dfrac{\partial u}{\partial y} = 0$.

**Solution:** $u$ is a function of $y/x$ only, which is homogeneous of degree 0!

$u(\lambda x, \lambda y) = f\!\left(\dfrac{\lambda y}{\lambda x}\right) = f\!\left(\dfrac{y}{x}\right) = u$ → degree 0. ✅

By Euler: $xu_x + yu_y = 0 \cdot u = 0$. ✅

---

### 8.6 🔭 Jacobian Matrix & Determinant (⭐⭐⭐ GATE CS/DA)

The **Jacobian matrix** of a vector-valued function $\mathbf{F}(x_1,\ldots,x_n) = (f_1,\ldots,f_m)$ is:

$$J = \frac{\partial(f_1,\ldots,f_m)}{\partial(x_1,\ldots,x_n)} = \begin{bmatrix} \frac{\partial f_1}{\partial x_1} & \cdots & \frac{\partial f_1}{\partial x_n} \\ \vdots & \ddots & \vdots \\ \frac{\partial f_m}{\partial x_1} & \cdots & \frac{\partial f_m}{\partial x_n} \end{bmatrix}$$

**Jacobian Determinant:** For $m = n$ (square Jacobian):
$$|J| = \det\left(\frac{\partial(f_1,\ldots,f_n)}{\partial(x_1,\ldots,x_n)}\right)$$

**Key Application — Change of Variables in Integration:**
$$\iint_R f(x,y)\,dA = \iint_S f(x(u,v),\, y(u,v))\,|J|\,du\,dv$$

where $|J| = \left|\dfrac{\partial(x,y)}{\partial(u,v)}\right|$.

---

**Standard Jacobians:**

| Transformation | Jacobian $|J|$ |
|---|---|
| Polar: $x=r\cos\theta$, $y=r\sin\theta$ | $r$ |
| Cylindrical: $x=r\cos\theta$, $y=r\sin\theta$, $z=z$ | $r$ |
| Spherical: general | $r^2\sin\phi$ |

**Proof for Polar:**
$$J = \begin{vmatrix} \frac{\partial x}{\partial r} & \frac{\partial x}{\partial\theta} \\ \frac{\partial y}{\partial r} & \frac{\partial y}{\partial\theta} \end{vmatrix} = \begin{vmatrix} \cos\theta & -r\sin\theta \\ \sin\theta & r\cos\theta \end{vmatrix} = r\cos^2\theta + r\sin^2\theta = r$$

---

**Example (Jacobian):** Find the Jacobian $\frac{\partial(u,v)}{\partial(x,y)}$ where $u = x^2 - y^2$, $v = 2xy$.

$$J = \begin{vmatrix} 2x & -2y \\ 2y & 2x \end{vmatrix} = 4x^2 + 4y^2 = 4(x^2+y^2)$$

---

**Example (GATE DA 🔴 — Change of Variables):** Evaluate $\iint_R e^{-(x^2+y^2)}\,dA$ over the entire plane using polar coordinates.

$$\int_0^{2\pi}\int_0^\infty e^{-r^2}\cdot r\,dr\,d\theta = 2\pi\int_0^\infty re^{-r^2}\,dr = 2\pi\left[-\frac{e^{-r^2}}{2}\right]_0^\infty = 2\pi\cdot\frac{1}{2} = \pi$$

Taking the square root: $\int_{-\infty}^\infty e^{-x^2}\,dx = \sqrt{\pi}$ (the famous Gaussian integral!)

---

**Inverse Function Theorem (⭐):**

If $\mathbf{F}$ has a differentiable inverse $\mathbf{G}$ near a point, then:
$$\frac{\partial(x_1,\ldots,x_n)}{\partial(u_1,\ldots,u_n)} = \left[\frac{\partial(u_1,\ldots,u_n)}{\partial(x_1,\ldots,x_n)}\right]^{-1}$$

i.e., **the Jacobians of inverse transformations are reciprocals!**

---

### 8.7 🎯 GATE PYQ — Hard Partial Diff / Euler / Jacobian Problems

**📌 PYQ 29 (GATE CS 2024 🔴 Hard — Euler's Theorem):**

If $u = x^a y^b$ is homogeneous and $xu_x + yu_y = 5u$, what is $a + b$?

**Solution:** By Euler's theorem, if $u$ is homogeneous of degree $n$: $xu_x + yu_y = nu$.

Given $xu_x + yu_y = 5u \Rightarrow n = 5$, and for $u = x^a y^b$: degree $= a + b = 5$. ✅

**Answer: $a + b = 5$** | Difficulty: 🟡 Easy but requires knowing Euler's theorem

---

**📌 PYQ 30 (GATE CS 2023 Style 🔴 Hard — Chain Rule):**

If $z = f(x, y)$, $x = e^t\cos s$, $y = e^t\sin s$, show that:
$$\frac{\partial^2 z}{\partial s^2} + \frac{\partial^2 z}{\partial t^2} = (x^2 + y^2)\left(\frac{\partial^2 z}{\partial x^2} + \frac{\partial^2 z}{\partial y^2}\right)$$

**Solution (Outline):**
$\frac{\partial z}{\partial s} = f_x(-e^t\sin s) + f_y(e^t\cos s)$, 
$\frac{\partial z}{\partial t} = f_x(e^t\cos s) + f_y(e^t\sin s)$

Computing the second derivatives and adding, noting $x = e^t\cos s$, $y = e^t\sin s$, so $x^2+y^2 = e^{2t}$:

The result follows from careful algebraic manipulation. | Difficulty: 🔴 Hard

---

**📌 PYQ 31 (GATE DA 🔴 Hard — Euler + Composite Function):**

If $f\!\left(\frac{x+y}{\sqrt{xy}}\right)$, find $xu_x + yu_y$.

**Solution:** Let $v = \frac{x+y}{\sqrt{xy}}$. Check: $v(\lambda x,\lambda y) = \frac{\lambda(x+y)}{\sqrt{\lambda^2 xy}} = \frac{\lambda(x+y)}{\lambda\sqrt{xy}} = v$.

So $v$ is **homogeneous of degree 0**. Thus $f(v)$ is also degree 0.

By Euler's theorem: $xu_x + yu_y = \mathbf{0}$. ✅ | Difficulty: 🟠 Medium

---

## Chapter 9: Gradient & Directional Derivatives (⭐⭐ VERY IMPORTANT)

### 9.1 Gradient

The **gradient** of $f(x_1, x_2, \ldots, x_n)$ is the vector of all partial derivatives:

$$\nabla f = \begin{bmatrix} \frac{\partial f}{\partial x_1} \\ \frac{\partial f}{\partial x_2} \\ \vdots \\ \frac{\partial f}{\partial x_n} \end{bmatrix}$$

**Key properties of the gradient:**
1. **Points in the direction of steepest ascent** of $f$
2. $-\nabla f$ points in the direction of **steepest descent**
3. $\nabla f$ is **perpendicular to level curves** (contours) of $f$
4. $\|\nabla f\|$ gives the **rate of maximum increase**

**Real-world analogy:** The gradient on a mountain is like a compass that always points uphill in the steepest direction.

### 9.2 Directional Derivative (⭐)

The rate of change of $f$ in the direction of a **unit vector** $\hat{u}$ is:

$$D_{\hat{u}} f = \nabla f \cdot \hat{u} = \|\nabla f\| \cos\theta$$

where $\theta$ is the angle between $\nabla f$ and $\hat{u}$.

| Direction | Directional Derivative |
|---|---|
| Same as $\nabla f$ ($\theta = 0$) | $\|\nabla f\|$ (maximum rate of increase) |
| Opposite to $\nabla f$ ($\theta = \pi$) | $-\|\nabla f\|$ (maximum rate of decrease) |
| Perpendicular to $\nabla f$ ($\theta = \pi/2$) | $0$ (no change — along level curve) |

---

**Example 29:** Find the gradient of $f(x, y) = x^2y + e^{xy}$ at $(1, 0)$.

**Solution:**
$$f_x = 2xy + ye^{xy}, \quad f_y = x^2 + xe^{xy}$$
$$\nabla f(1, 0) = \begin{bmatrix} 0 + 0 \\ 1 + 1 \end{bmatrix} = \begin{bmatrix} 0 \\ 2 \end{bmatrix}$$

---

**Example 30 (GATE Level):** Find the directional derivative of $f(x,y) = x^2 + y^2$ at $(1, 2)$ in the direction of $\vec{v} = (3, 4)$.

**Solution:**
$$\nabla f = (2x, 2y) \implies \nabla f(1, 2) = (2, 4)$$

Unit vector: $\hat{u} = \frac{(3,4)}{|(3,4)|} = \frac{(3,4)}{5} = \left(\frac{3}{5}, \frac{4}{5}\right)$

$$D_{\hat{u}}f = (2, 4) \cdot \left(\frac{3}{5}, \frac{4}{5}\right) = \frac{6}{5} + \frac{16}{5} = \frac{22}{5}$$

---

**Example 31:** In which direction does $f(x,y) = xy + y^2$ increase most rapidly at $(1, 1)$? What is the maximum rate?

**Solution:**
$$\nabla f = (y, x + 2y) \implies \nabla f(1,1) = (1, 3)$$

Direction of steepest ascent: $(1, 3)$ (or unit vector $\frac{1}{\sqrt{10}}(1, 3)$).

Maximum rate of increase: $\|\nabla f(1,1)\| = \sqrt{1 + 9} = \sqrt{10}$.

---

### 9.3 🏔️ Tangent Plane & Normal to a Surface (⭐⭐ DA/CS)

**Tangent Plane to $z = f(x,y)$ at $(x_0, y_0, z_0)$:**
$$z - z_0 = f_x(x_0,y_0)(x - x_0) + f_y(x_0,y_0)(y - y_0)$$

**Normal Line to $z = f(x,y)$ at $(x_0,y_0,z_0)$:** Direction vector $(f_x, f_y, -1)$:
$$\frac{x-x_0}{f_x} = \frac{y-y_0}{f_y} = \frac{z-z_0}{-1}$$

**For implicit surface $F(x,y,z) = 0$:** The **gradient** $\nabla F = (F_x, F_y, F_z)$ is the **normal** to the surface.

- **Tangent Plane:** $F_x(x-x_0) + F_y(y-y_0) + F_z(z-z_0) = 0$
- **Normal Line:** $\frac{x-x_0}{F_x} = \frac{y-y_0}{F_y} = \frac{z-z_0}{F_z}$

---

**Example (Tangent Plane 🟠):** Find the tangent plane to $z = x^2 + y^2$ at $(1, 2, 5)$.

$f_x = 2x = 2$, $f_y = 2y = 4$ at $(1,2)$.

Tangent plane: $z - 5 = 2(x-1) + 4(y-2) \Rightarrow z = 2x + 4y - 5$

---

**Example (Normal to Implicit Surface 🔴):** Find the tangent plane to $x^2 + y^2 + z^2 = 14$ at $(1, 2, 3)$.

$F = x^2+y^2+z^2-14$; $\nabla F = (2x, 2y, 2z) = (2, 4, 6)$ at $(1,2,3)$.

Tangent plane: $2(x-1) + 4(y-2) + 6(z-3) = 0 \Rightarrow x + 2y + 3z = 14$

---

**Example (GATE DA 🔴 — Angle between surfaces):** Find the angle between surfaces $x^2+y^2+z^2=9$ and $z = x^2+y^2-3$ at $(2, -1, 2)$.

$F_1 = x^2+y^2+z^2-9$: $\nabla F_1 = (4, -2, 4)$
$F_2 = x^2+y^2-z-3$: $\nabla F_2 = (4, -2, -1)$

$\cos\theta = \frac{|\nabla F_1 \cdot \nabla F_2|}{|\nabla F_1||\nabla F_2|} = \frac{|16+4-4|}{\sqrt{36}\cdot\sqrt{21}} = \frac{16}{6\sqrt{21}}$

---

### 9.4 🧭 Divergence and Curl (DA Extension ⭐)

For a **vector field** $\mathbf{F} = (P, Q, R)$:

**Divergence (scalar):** Measures "outward flux" at a point:
$$\text{div}\,\mathbf{F} = \nabla \cdot \mathbf{F} = \frac{\partial P}{\partial x} + \frac{\partial Q}{\partial y} + \frac{\partial R}{\partial z}$$

**Curl (vector):** Measures "rotational tendency":
$$\text{curl}\,\mathbf{F} = \nabla \times \mathbf{F} = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ \partial/\partial x & \partial/\partial y & \partial/\partial z \\ P & Q & R \end{vmatrix}$$

$$= \left(\frac{\partial R}{\partial y} - \frac{\partial Q}{\partial z}\right)\mathbf{i} - \left(\frac{\partial R}{\partial x} - \frac{\partial P}{\partial z}\right)\mathbf{j} + \left(\frac{\partial Q}{\partial x} - \frac{\partial P}{\partial y}\right)\mathbf{k}$$

**Key Identities:**
- $\text{curl}(\nabla f) = \mathbf{0}$ (curl of gradient is zero)
- $\text{div}(\text{curl}\,\mathbf{F}) = 0$ (divergence of curl is zero)
- $\nabla^2 f = \text{div}(\nabla f) = f_{xx}+f_{yy}+f_{zz}$ (Laplacian)

---

**Example:** Find div and curl of $\mathbf{F} = (x^2y, yz^2, zx^2)$.

$\text{div}\,\mathbf{F} = 2xy + z^2 + x^2$

$\text{curl}\,\mathbf{F} = \left(\frac{\partial(zx^2)}{\partial y} - \frac{\partial(yz^2)}{\partial z}\right)\mathbf{i} + \cdots = (0-2yz)\mathbf{i} + (2zx-0)\mathbf{j} + (0-x^2)\mathbf{k}$

$= (-2yz, 2zx, -x^2)$

---

### 9.5 🎯 GATE PYQ — Hard Gradient/Directional Problems

**📌 PYQ 32 (GATE CS 🔴 Hard):**

The maximum directional derivative of $f(x,y) = x^2y + xy^2$ at $(1,1)$ is:

**(A)** $\sqrt{5}$ **(B)** $\sqrt{10}$ **(C)** $\sqrt{13}$ **(D)** $3\sqrt{2}$

**Solution:** $\nabla f = (2xy+y^2, x^2+2xy)$. At $(1,1)$: $\nabla f = (3, 3)$.

Max directional derivative $= \|\nabla f\| = \sqrt{9+9} = 3\sqrt{2}$.

✅ **Answer: (D)** | Difficulty: 🟠 Medium

---

**📌 PYQ 33 (GATE DA 🔴 Hard — Tangent Plane):**

The equation of tangent plane to $xyz = 6$ at $(1, 2, 3)$ is:

**(A)** $6x+3y+2z = 18$ **(B)** $x+y+z=6$ **(C)** $2x+3y+z=11$ **(D)** $6x+3y+2z=6$

**Solution:** $F = xyz - 6$. $\nabla F = (yz, xz, xy) = (6, 3, 2)$ at $(1,2,3)$.

Tangent plane: $6(x-1)+3(y-2)+2(z-3)=0 \Rightarrow 6x+3y+2z = 18$.

✅ **Answer: (A)** | Difficulty: 🟠 Medium-Hard

---

**📌 PYQ 34 (GATE DA 🔴 Very Hard — Directional Derivative + Level Curves):**

At a point $P$, $\nabla f = (4, -3)$. The directional derivative at $P$ in direction $\mathbf{u}$ is maximum. What direction makes the directional derivative zero?

**Solution:** Max directional derivative is in direction $\nabla f = (4,-3)$. 

Zero directional derivative is **perpendicular** to $\nabla f$, i.e., direction $(3, 4)$ or $(-3,-4)$ (since $(4,-3)\cdot(3,4)=12-12=0$).

Unit direction: $\left(\frac{3}{5}, \frac{4}{5}\right)$.

✅ This is along the **level curve** through $P$! | Difficulty: 🟠 Medium

---

## Chapter 10: Contour Maps & Level Curves

### 10.1 Level Curves and Contour Maps

A **level curve** (or **contour line**) of $f(x, y)$ is the set of points where $f$ has a constant value:

$$f(x, y) = c$$

A **contour map** is a collection of level curves for different values of $c$.

**Real-world analogy:** Topographic maps show elevation contours. Closely spaced contours = steep terrain. Widely spaced = flat terrain.

### 10.2 Key Properties

| Property | Explanation |
|---|---|
| Gradient $\perp$ level curves | $\nabla f$ is always perpendicular to contour lines |
| Closely spaced contours | Region of steep change (large $\|\nabla f\|$) |
| Widely spaced contours | Region of slow change (small $\|\nabla f\|$) |
| Closed contours around a point | Indicates local max or min |
| Saddle point | Contours cross or form an "X" pattern |

### 10.3 Identifying Features from Contour Maps

- **Local max:** Contour values increase as you move toward the center of closed curves
- **Local min:** Contour values decrease as you move toward the center
- **Saddle point:** Contour values increase in some directions, decrease in others

---

**Example 32:** For $f(x,y) = x^2 + y^2$, describe the level curves.

**Solution:** $x^2 + y^2 = c$ gives circles centered at the origin with radius $\sqrt{c}$ for $c > 0$. The gradient $\nabla f = (2x, 2y)$ points radially outward, perpendicular to the circular contours.

---

## Chapter 11: Matrix Calculus (⭐ IMPORTANT FOR DA)

### 11.1 Derivatives Involving Vectors and Matrices

Matrix calculus is essential for machine learning optimization. Key results:

**Notation:** $\mathbf{x}$ is a column vector in $\mathbb{R}^n$, $A$ is a matrix.

### 11.2 Key Derivative Formulas (⭐ MUST MEMORIZE)

| Expression $f(\mathbf{x})$ | $\frac{\partial f}{\partial \mathbf{x}}$ (Gradient) |
|---|---|
| $\mathbf{a}^T\mathbf{x}$ | $\mathbf{a}$ |
| $\mathbf{x}^T\mathbf{a}$ | $\mathbf{a}$ |
| $\mathbf{x}^T\mathbf{x}$ | $2\mathbf{x}$ |
| $\mathbf{x}^T A\mathbf{x}$ | $(A + A^T)\mathbf{x}$ |
| $\mathbf{x}^T A\mathbf{x}$ (if $A$ symmetric) | $2A\mathbf{x}$ |
| $\|\mathbf{x}\|^2 = \mathbf{x}^T\mathbf{x}$ | $2\mathbf{x}$ |
| $\|A\mathbf{x} - \mathbf{b}\|^2$ | $2A^T(A\mathbf{x} - \mathbf{b})$ |

### 11.3 Application: Least Squares (⭐)

To minimize $\|A\mathbf{x} - \mathbf{b}\|^2$, set the gradient to zero:

$$\nabla_\mathbf{x} \|A\mathbf{x} - \mathbf{b}\|^2 = 2A^T(A\mathbf{x} - \mathbf{b}) = \mathbf{0}$$

$$A^T A\mathbf{x} = A^T\mathbf{b} \quad \text{(Normal Equations)}$$

$$\hat{\mathbf{x}} = (A^T A)^{-1} A^T \mathbf{b}$$

---

**Example 33:** Find $\nabla_\mathbf{x} (\mathbf{x}^T A\mathbf{x} + \mathbf{b}^T\mathbf{x})$ where $A$ is symmetric.

**Solution:**
$$\nabla_\mathbf{x} (\mathbf{x}^T A\mathbf{x} + \mathbf{b}^T\mathbf{x}) = 2A\mathbf{x} + \mathbf{b}$$

---

**Example 34 (GATE DA Level):** For $f(\mathbf{x}) = \mathbf{x}^T\mathbf{x} - 2\mathbf{c}^T\mathbf{x} + d$ (where $\mathbf{c}$ is a constant vector, $d$ scalar), find the minimizer.

**Solution:**
$$\nabla f = 2\mathbf{x} - 2\mathbf{c} = \mathbf{0} \implies \mathbf{x}^* = \mathbf{c}$$

The Hessian is $H = 2I$ (positive definite), confirming this is a minimum.

---

## Chapter 12: Positive Definite Matrices & Quadratic Forms (⭐)

### 12.1 Quadratic Forms

A **quadratic form** is:

$$Q(\mathbf{x}) = \mathbf{x}^T A\mathbf{x} = \sum_{i}\sum_{j} a_{ij}x_ix_j$$

where $A$ is a symmetric matrix (without loss of generality, since $\mathbf{x}^T A\mathbf{x} = \mathbf{x}^T\left(\frac{A+A^T}{2}\right)\mathbf{x}$).

**Example in 2D:** $Q(x, y) = ax^2 + 2bxy + cy^2 = \begin{bmatrix} x & y \end{bmatrix}\begin{bmatrix} a & b \\ b & c \end{bmatrix}\begin{bmatrix} x \\ y \end{bmatrix}$

### 12.2 Positive Definiteness

| Type | Condition on $\mathbf{x}^TA\mathbf{x}$ | Eigenvalue Condition | Pivot Condition |
|---|---|---|---|
| **Positive Definite (PD)** | $> 0$ for all $\mathbf{x} \neq \mathbf{0}$ | All $\lambda_i > 0$ | All pivots $> 0$ |
| **Positive Semidefinite (PSD)** | $\geq 0$ for all $\mathbf{x}$ | All $\lambda_i \geq 0$ | All pivots $\geq 0$ |
| **Negative Definite (ND)** | $< 0$ for all $\mathbf{x} \neq \mathbf{0}$ | All $\lambda_i < 0$ | - |
| **Negative Semidefinite (NSD)** | $\leq 0$ for all $\mathbf{x}$ | All $\lambda_i \leq 0$ | - |
| **Indefinite** | Takes both $+$ and $-$ values | Has both $+$ and $-$ eigenvalues | - |

### 12.3 Tests for Positive Definiteness

**Test 1: Eigenvalue Test** — Check all eigenvalues are positive.

**Test 2: Leading Principal Minors (Sylvester's Criterion)**
$A$ is PD iff ALL leading principal minors are positive:
$$a_{11} > 0, \quad \begin{vmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{vmatrix} > 0, \quad \det(A) > 0, \quad \ldots$$

**Test 3: Determinant + Trace (for 2×2)**
For $A = \begin{bmatrix} a & b \\ b & c \end{bmatrix}$: PD iff $a > 0$ and $ac - b^2 > 0$ (equivalently, $\text{tr}(A) > 0$ and $\det(A) > 0$).

---

**Example 35:** Is $A = \begin{bmatrix} 2 & 1 \\ 1 & 3 \end{bmatrix}$ positive definite?

**Solution:** Leading principal minors: $a_{11} = 2 > 0$, $\det(A) = 6 - 1 = 5 > 0$. Yes, PD.

Alternatively: eigenvalues are $\frac{5 \pm \sqrt{5}}{2}$, both positive. ✓

---

**Example 36 (GATE Level):** For what values of $k$ is $A = \begin{bmatrix} 4 & k \\ k & 1 \end{bmatrix}$ positive definite?

**Solution:** Conditions: $4 > 0$ ✓, and $\det(A) = 4 - k^2 > 0 \implies k^2 < 4 \implies -2 < k < 2$.

---

### 12.4 Why Positive Definiteness Matters in Optimization

- **PD Hessian at a critical point** → **local minimum**
- **ND Hessian at a critical point** → **local maximum**
- **Indefinite Hessian** → **saddle point**
- In machine learning: PD ensures the loss landscape is convex (bowl-shaped), making optimization well-behaved.

---

## Chapter 13: Unconstrained Optimization for Multivariate Functions (⭐⭐)

### 13.1 Finding Critical Points

For $f(x_1, x_2, \ldots, x_n)$, critical points satisfy:

$$\nabla f = \mathbf{0} \quad \Leftrightarrow \quad \frac{\partial f}{\partial x_i} = 0 \text{ for all } i$$

### 13.2 Second Derivative Test (Multivariate) (⭐⭐)

The **Hessian matrix** is:

$$H = \begin{bmatrix} f_{x_1x_1} & f_{x_1x_2} & \cdots \\ f_{x_2x_1} & f_{x_2x_2} & \cdots \\ \vdots & & \ddots \end{bmatrix}$$

At a critical point $\mathbf{x}^*$ where $\nabla f = \mathbf{0}$:

| Condition on $H(\mathbf{x}^*)$ | Conclusion |
|---|---|
| $H$ is positive definite | **Local minimum** |
| $H$ is negative definite | **Local maximum** |
| $H$ is indefinite | **Saddle point** |
| $H$ is singular (semidefinite) | **Test inconclusive** |

**For 2 variables $f(x, y)$:**

Let $D = f_{xx}f_{yy} - (f_{xy})^2 = \det(H)$.

| Condition | Conclusion |
|---|---|
| $D > 0$ and $f_{xx} > 0$ | Local minimum |
| $D > 0$ and $f_{xx} < 0$ | Local maximum |
| $D < 0$ | Saddle point |
| $D = 0$ | Inconclusive |

---

**Example 37:** Find and classify critical points of $f(x, y) = x^2 + y^2 - 2x - 4y + 5$.

**Solution:**
$$f_x = 2x - 2 = 0 \implies x = 1$$
$$f_y = 2y - 4 = 0 \implies y = 2$$

Critical point: $(1, 2)$.

$f_{xx} = 2$, $f_{yy} = 2$, $f_{xy} = 0$.

$D = (2)(2) - 0 = 4 > 0$ and $f_{xx} = 2 > 0$ → **Local minimum**.

$f(1, 2) = 1 + 4 - 2 - 8 + 5 = 0$.

---

**Example 38 (Saddle Point):** Classify the critical point of $f(x, y) = x^2 - y^2$.

**Solution:** $f_x = 2x = 0$, $f_y = -2y = 0$ → critical point $(0, 0)$.

$f_{xx} = 2$, $f_{yy} = -2$, $f_{xy} = 0$.

$D = (2)(-2) - 0 = -4 < 0$ → **Saddle point**.

---

**Example 39 (GATE Level):** Find and classify all critical points of $f(x, y) = x^3 + y^3 - 3xy$.

**Solution:**
$$f_x = 3x^2 - 3y = 0 \implies y = x^2$$
$$f_y = 3y^2 - 3x = 0 \implies x = y^2$$

Substituting $y = x^2$ into $x = y^2$: $x = x^4 \implies x(x^3 - 1) = 0 \implies x = 0$ or $x = 1$.

Critical points: $(0, 0)$ and $(1, 1)$.

$f_{xx} = 6x$, $f_{yy} = 6y$, $f_{xy} = -3$.

**At $(0, 0)$:** $D = (0)(0) - 9 = -9 < 0$ → **Saddle point**.

**At $(1, 1)$:** $D = (6)(6) - 9 = 27 > 0$ and $f_{xx} = 6 > 0$ → **Local minimum**.

$f(1, 1) = 1 + 1 - 3 = -1$.

---

## Chapter 14: Convex Functions (⭐ IMPORTANT FOR DA/ML)

### 14.1 Definition of Convexity

A function $f: \mathbb{R}^n \to \mathbb{R}$ is **convex** if for all $\mathbf{x}, \mathbf{y} \in \text{dom}(f)$ and $0 \leq \lambda \leq 1$:

$$f(\lambda\mathbf{x} + (1-\lambda)\mathbf{y}) \leq \lambda f(\mathbf{x}) + (1-\lambda)f(\mathbf{y})$$

**Intuition:** The line segment connecting any two points on the graph lies **above** the graph. The function is "bowl-shaped."

**Strictly convex:** Replace $\leq$ with $<$ for $0 < \lambda < 1$ and $\mathbf{x} \neq \mathbf{y}$.

**Concave function:** $f$ is concave iff $-f$ is convex.

### 14.2 Why Convexity Matters

| Property | Implication |
|---|---|
| Every local minimum is a global minimum | No need to search exhaustively |
| Gradient descent converges to global min | Efficient optimization |
| Unique minimum (if strictly convex) | Solution is well-defined |
| Machine learning loss functions | Many designed to be convex |

### 14.3 First-Order Condition for Convexity (⭐)

If $f$ is differentiable, then $f$ is convex iff:

$$f(\mathbf{y}) \geq f(\mathbf{x}) + \nabla f(\mathbf{x})^T(\mathbf{y} - \mathbf{x}) \quad \forall\, \mathbf{x}, \mathbf{y}$$

**Interpretation:** The tangent line (or tangent hyperplane) at any point lies **below** the function. In other words, the linear approximation always **underestimates** a convex function.

### 14.4 Second-Order Condition for Convexity (⭐⭐)

If $f$ is twice differentiable, then:

$$f \text{ is convex} \quad \Leftrightarrow \quad H(\mathbf{x}) = \nabla^2 f(\mathbf{x}) \succeq 0 \text{ (positive semidefinite) for all } \mathbf{x}$$

$$f \text{ is strictly convex} \quad \Leftarrow \quad H(\mathbf{x}) \succ 0 \text{ (positive definite) for all } \mathbf{x}$$

**For single variable:** $f$ convex iff $f''(x) \geq 0$ for all $x$.

### 14.5 Examples of Convex/Concave Functions

| Function | Convex? | Reason |
|---|---|---|
| $f(x) = x^2$ | Yes | $f''(x) = 2 > 0$ |
| $f(x) = e^x$ | Yes | $f''(x) = e^x > 0$ |
| $f(x) = |x|$ | Yes | (by definition check) |
| $f(\mathbf{x}) = \|\mathbf{x}\|^2$ | Yes | $H = 2I \succ 0$ |
| $f(x) = \ln x$ | Concave | $f''(x) = -1/x^2 < 0$ |
| $f(x) = \sqrt{x}$ | Concave | $f''(x) = -\frac{1}{4}x^{-3/2} < 0$ |
| $f(x) = x^3$ | Neither | $f''(x) = 6x$ changes sign |
| $f(\mathbf{x}) = \mathbf{x}^T A\mathbf{x}$ ($A$ PSD) | Yes | $H = 2A \succeq 0$ |

### 14.6 Operations that Preserve Convexity

| Operation | Result |
|---|---|
| Non-negative weighted sum $\sum \alpha_i f_i$ ($\alpha_i \geq 0$) | Convex |
| Pointwise maximum $\max(f_1, f_2, \ldots)$ | Convex |
| Composition with affine: $f(Ax + b)$ | Convex (if $f$ convex) |
| Restriction to a line | Convex |

---

**Example 40:** Show that $f(x) = x^2 + 4x + 5$ is convex.

**Solution:** $f''(x) = 2 > 0$ for all $x$. By second-order condition, $f$ is convex.

---

**Example 41 (GATE DA Level):** Is $f(x, y) = x^2 + xy + y^2$ convex?

**Solution:** Hessian:
$$H = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$$

Eigenvalues: $\lambda = \frac{4 \pm \sqrt{4-12}}{2}$... Let's use Sylvester's criterion: $2 > 0$ and $\det(H) = 4 - 1 = 3 > 0$. So $H$ is PD for all $(x,y)$ → $f$ is **strictly convex**.

---

## Chapter 15: Gradient Descent Algorithm (⭐ IMPORTANT FOR DA/ML)

### 15.1 The Idea

**Goal:** Minimize $f(\mathbf{x})$.

**Strategy:** Start at some point, repeatedly move in the direction of steepest descent ($-\nabla f$).

### 15.2 The Algorithm

**Input:** Starting point $\mathbf{x}_0$, learning rate $\alpha > 0$, tolerance $\epsilon$.

**Update rule:**
$$\mathbf{x}_{k+1} = \mathbf{x}_k - \alpha \nabla f(\mathbf{x}_k)$$

**Repeat** until $\|\nabla f(\mathbf{x}_k)\| < \epsilon$ or max iterations reached.

### 15.3 Learning Rate Selection (⭐)

| Learning Rate $\alpha$ | Effect |
|---|---|
| Too small | Very slow convergence — takes many iterations |
| Too large | Overshoots — may diverge (oscillate or blow up) |
| Just right | Smooth, fast convergence |

**Real-world analogy:** Descending a mountain in fog. The learning rate is your step size. Too small = takes forever. Too large = you overshoot the valley and end up higher.

### 15.4 Convergence Properties

- For **convex** functions, gradient descent converges to the **global minimum**.
- For **strongly convex** functions (eigenvalues of $H$ bounded away from 0), convergence is **linear** (exponential decrease in error).
- Convergence rate depends on the **condition number** $\kappa = \frac{\lambda_{\max}(H)}{\lambda_{\min}(H)}$. Large $\kappa$ = slow convergence (ill-conditioned).

---

**Example 42:** Apply one step of gradient descent to minimize $f(x, y) = x^2 + 4y^2$ starting from $(1, 1)$ with $\alpha = 0.1$.

**Solution:**
$$\nabla f = (2x, 8y) \implies \nabla f(1, 1) = (2, 8)$$

$$\mathbf{x}_1 = (1, 1) - 0.1(2, 8) = (1 - 0.2, 1 - 0.8) = (0.8, 0.2)$$

Check: $f(1,1) = 5$, $f(0.8, 0.2) = 0.64 + 0.16 = 0.8$. Significant decrease! ✓

---

**Example 43 (GATE DA Level):** For $f(x) = (x-3)^2$, starting from $x_0 = 0$ with learning rate $\alpha = 0.4$, find $x_1$ and $x_2$.

**Solution:**
$f'(x) = 2(x - 3)$

$x_1 = x_0 - \alpha f'(x_0) = 0 - 0.4 \cdot 2(0-3) = 0 + 2.4 = 2.4$

$x_2 = x_1 - \alpha f'(x_1) = 2.4 - 0.4 \cdot 2(2.4 - 3) = 2.4 - 0.4(-1.2) = 2.4 + 0.48 = 2.88$

We see convergence toward $x^* = 3$.

---

### 15.5 Variants of Gradient Descent

| Variant | Description |
|---|---|
| **Batch GD** | Uses all data points per update |
| **Stochastic GD (SGD)** | Uses one random data point per update |
| **Mini-batch GD** | Uses a small batch of data points |
| **Momentum** | Adds a fraction of the previous update |
| **Adam** | Adaptive learning rates per parameter |

---

### 15.6 🔬 Newton's Method for Optimization (⭐⭐ DA)

**Gradient Descent limitation:** Convergence can be slow due to curvature (condition number).

**Newton's Method** uses second-order information (Hessian) for **much faster convergence**!

**Iteration:**
$$\mathbf{x}_{k+1} = \mathbf{x}_k - [H(\mathbf{x}_k)]^{-1} \nabla f(\mathbf{x}_k)$$

where $H(\mathbf{x}_k)$ is the Hessian matrix evaluated at $\mathbf{x}_k$.

**Intuition:** Instead of a straight "slide down the hill" (gradient), Newton's method fits a **quadratic (parabola)** at the current point and jumps to the parabola's minimum.

**Comparison:**

| Method | Update | Convergence Rate | Cost per Iteration |
|---|---|---|---|
| Gradient Descent | $\mathbf{x} - \alpha\nabla f$ | Linear | $O(n)$ |
| Newton's Method | $\mathbf{x} - H^{-1}\nabla f$ | **Quadratic** (near optimum) | $O(n^3)$ (matrix inversion) |
| Quasi-Newton (BFGS) | Approximate $H^{-1}$ | Super-linear | $O(n^2)$ |

> 💡 **Quadratic convergence:** Error at step $(k+1)$ is proportional to **square** of error at step $k$ — incredibly fast near the solution!

---

**Example (Newton's for 1D):** Minimize $f(x) = x^4 - 4x^2$ starting from $x_0 = 2$.

$f'(x) = 4x^3 - 8x$, $f''(x) = 12x^2 - 8$

$x_1 = x_0 - \frac{f'(x_0)}{f''(x_0)} = 2 - \frac{32-16}{48-8} = 2 - \frac{16}{40} = 2 - 0.4 = 1.6$

$f'(1.6) = 4(4.096) - 8(1.6) = 16.384 - 12.8 = 3.584$, $f''(1.6) = 12(2.56)-8 = 22.72$

$x_2 = 1.6 - 3.584/22.72 \approx 1.442$

Actual minimum at $x^* = \sqrt{2} \approx 1.414$. Converging quickly! ✅

---

**Newton's Method for Root Finding (related):**

To solve $f(x) = 0$: $x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$

(This is equivalent to finding zeros of the gradient when minimizing.)

---

## Chapter 16: Constrained Optimization — Lagrange Multipliers (⭐⭐ VERY IMPORTANT)

### 16.1 The Problem

Minimize (or maximize) $f(\mathbf{x})$ subject to equality constraints:

$$\text{minimize } f(\mathbf{x}) \quad \text{subject to } g_i(\mathbf{x}) = 0, \quad i = 1, 2, \ldots, m$$

### 16.2 The Method of Lagrange Multipliers

**Step 1:** Form the **Lagrangian**:

$$\mathcal{L}(\mathbf{x}, \boldsymbol{\lambda}) = f(\mathbf{x}) + \sum_{i=1}^{m} \lambda_i g_i(\mathbf{x})$$

**Step 2:** Set partial derivatives to zero:

$$\frac{\partial \mathcal{L}}{\partial x_j} = 0 \quad \text{for all } j$$
$$\frac{\partial \mathcal{L}}{\partial \lambda_i} = 0 \quad \text{for all } i \quad (\text{this recovers the constraints})$$

**Step 3:** Solve the system of equations.

### 16.3 Geometric Intuition (⭐)

At the optimal point of a constrained problem, the gradient of $f$ must be **parallel** to the gradient of $g$:

$$\nabla f = -\lambda \nabla g$$

**Why?** If $\nabla f$ had any component along the constraint surface, we could move along the surface to decrease $f$ further. At the optimum, $\nabla f$ is purely normal to the constraint.

---

**Example 44:** Minimize $f(x, y) = x^2 + y^2$ subject to $x + y = 1$.

**Solution:** Lagrangian:
$$\mathcal{L} = x^2 + y^2 + \lambda(x + y - 1)$$

Setting partial derivatives to zero:
$$\frac{\partial \mathcal{L}}{\partial x} = 2x + \lambda = 0 \implies x = -\frac{\lambda}{2}$$
$$\frac{\partial \mathcal{L}}{\partial y} = 2y + \lambda = 0 \implies y = -\frac{\lambda}{2}$$
$$\frac{\partial \mathcal{L}}{\partial \lambda} = x + y - 1 = 0$$

From the first two: $x = y$. Substituting into constraint: $2x = 1 \implies x = y = \frac{1}{2}$.

Minimum value: $f\left(\frac{1}{2}, \frac{1}{2}\right) = \frac{1}{4} + \frac{1}{4} = \frac{1}{2}$.

---

**Example 45 (Medium):** Find the maximum of $f(x, y) = xy$ subject to $x^2 + y^2 = 1$.

**Solution:** $\mathcal{L} = xy + \lambda(x^2 + y^2 - 1)$

$$\frac{\partial \mathcal{L}}{\partial x} = y + 2\lambda x = 0$$
$$\frac{\partial \mathcal{L}}{\partial y} = x + 2\lambda y = 0$$
$$x^2 + y^2 = 1$$

From first equation: $y = -2\lambda x$. Substitute into second: $x + 2\lambda(-2\lambda x) = 0 \implies x(1 - 4\lambda^2) = 0$.

If $x \neq 0$: $\lambda = \pm\frac{1}{2}$.

**Case $\lambda = -\frac{1}{2}$:** $y = x$, so $2x^2 = 1$, $x = y = \frac{1}{\sqrt{2}}$ or $x = y = -\frac{1}{\sqrt{2}}$. $f = \frac{1}{2}$.

**Case $\lambda = \frac{1}{2}$:** $y = -x$, so $x = \frac{1}{\sqrt{2}}, y = -\frac{1}{\sqrt{2}}$ (or vice versa). $f = -\frac{1}{2}$.

**Maximum** = $\frac{1}{2}$, **Minimum** = $-\frac{1}{2}$.

---

**Example 46 (GATE Level):** Find the minimum distance from the origin to the plane $2x + 3y + z = 14$.

**Solution:** Minimize $f(x,y,z) = x^2 + y^2 + z^2$ subject to $g(x,y,z) = 2x + 3y + z - 14 = 0$.

$$\mathcal{L} = x^2 + y^2 + z^2 + \lambda(2x + 3y + z - 14)$$

$$2x + 2\lambda = 0 \implies x = -\lambda$$
$$2y + 3\lambda = 0 \implies y = -\frac{3\lambda}{2}$$
$$2z + \lambda = 0 \implies z = -\frac{\lambda}{2}$$

Constraint: $2(-\lambda) + 3\left(-\frac{3\lambda}{2}\right) + \left(-\frac{\lambda}{2}\right) = 14$

$$-2\lambda - \frac{9\lambda}{2} - \frac{\lambda}{2} = 14 \implies -2\lambda - 5\lambda = 14 \implies -7\lambda = 14 \implies \lambda = -2$$

So $x = 2$, $y = 3$, $z = 1$.

Minimum distance $= \sqrt{4 + 9 + 1} = \sqrt{14}$.

Check: This matches the formula $\frac{|d|}{\|\mathbf{n}\|} = \frac{14}{\sqrt{14}} = \sqrt{14}$. ✓

---

### 16.4 Multiple Equality Constraints

For $f(\mathbf{x})$ subject to $g_1(\mathbf{x}) = 0, g_2(\mathbf{x}) = 0, \ldots, g_m(\mathbf{x}) = 0$:

$$\mathcal{L} = f + \lambda_1 g_1 + \lambda_2 g_2 + \cdots + \lambda_m g_m$$

Same process: set all partial derivatives to zero and solve.

---

## Chapter 17: Constrained Optimization with Inequality Constraints — KKT Conditions (⭐⭐ CRITICAL FOR DA)

### 17.1 The Problem

$$\text{minimize } f(\mathbf{x})$$
$$\text{subject to: } g_i(\mathbf{x}) \leq 0, \quad i = 1, \ldots, m \quad \text{(inequality constraints)}$$
$$\quad\quad\quad\quad\quad h_j(\mathbf{x}) = 0, \quad j = 1, \ldots, p \quad \text{(equality constraints)}$$

### 17.2 Karush-Kuhn-Tucker (KKT) Conditions (⭐⭐ MUST MEMORIZE)

For $\mathbf{x}^*$ to be optimal, the following **KKT conditions** must hold (necessary conditions for optimality, sufficient if the problem is convex):

**1. Stationarity:**
$$\nabla f(\mathbf{x}^*) + \sum_{i=1}^{m} \mu_i \nabla g_i(\mathbf{x}^*) + \sum_{j=1}^{p} \lambda_j \nabla h_j(\mathbf{x}^*) = \mathbf{0}$$

**2. Primal Feasibility:**
$$g_i(\mathbf{x}^*) \leq 0, \quad h_j(\mathbf{x}^*) = 0$$

**3. Dual Feasibility:**
$$\mu_i \geq 0 \quad \text{for all } i$$

**4. Complementary Slackness (⭐ KEY CONDITION):**
$$\mu_i \cdot g_i(\mathbf{x}^*) = 0 \quad \text{for all } i$$

### 17.3 Interpreting Complementary Slackness

For each inequality constraint $g_i \leq 0$:
- Either $g_i(\mathbf{x}^*) = 0$ (constraint is **active/binding**) and $\mu_i \geq 0$, OR
- $g_i(\mathbf{x}^*) < 0$ (constraint is **inactive**) and $\mu_i = 0$.

**Intuition:** If a constraint isn't tight (you have slack), then that constraint plays no role in the optimization, so its multiplier is zero. Only binding constraints "matter."

### 17.4 KKT for Convex Problems

If $f$ and all $g_i$ are convex and $h_j$ are affine, then:
- KKT conditions are **necessary AND sufficient** for global optimality.
- Any point satisfying KKT is the global minimum.

---

**Example 47:** Minimize $f(x) = (x-2)^2$ subject to $x \leq 1$.

**Solution:** Write constraint as $g(x) = x - 1 \leq 0$.

Lagrangian: $\mathcal{L} = (x-2)^2 + \mu(x - 1)$

**KKT conditions:**
1. **Stationarity:** $2(x-2) + \mu = 0$
2. **Primal:** $x \leq 1$
3. **Dual:** $\mu \geq 0$
4. **Complementary slackness:** $\mu(x-1) = 0$

**Case 1:** $\mu = 0$ → From stationarity: $2(x-2) = 0 \implies x = 2$. But $x = 2 > 1$ violates primal. ✗

**Case 2:** $x = 1$ (constraint active) → From stationarity: $2(1-2) + \mu = 0 \implies \mu = 2 > 0$ ✓

**Solution:** $x^* = 1$, $f(1) = 1$.

---

**Example 48 (GATE DA Level):** Minimize $f(x, y) = x^2 + y^2$ subject to $x + y \geq 4$.

**Solution:** Rewrite: $g(x, y) = -(x + y - 4) = 4 - x - y \leq 0$. (Or equivalently, $g = 4 - x - y \leq 0$.)

$\mathcal{L} = x^2 + y^2 + \mu(4 - x - y)$

**Stationarity:**
$$2x - \mu = 0 \implies x = \frac{\mu}{2}$$
$$2y - \mu = 0 \implies y = \frac{\mu}{2}$$

So $x = y$.

**Complementary slackness:** $\mu(4 - x - y) = 0$.

**Case 1:** $\mu = 0$: $x = y = 0$. Check primal: $4 - 0 - 0 = 4 > 0$. Violates $g \leq 0$! ✗

**Case 2:** $4 - x - y = 0$: $2x = 4 \implies x = y = 2$. From stationarity: $\mu = 4 > 0$ ✓.

**Solution:** $(x^*, y^*) = (2, 2)$, $f(2, 2) = 8$.

---

**Example 49 (Comprehensive KKT):** Minimize $f(x_1, x_2) = x_1^2 + x_2^2$ subject to $x_1 + x_2 \leq 2$ and $x_1 \geq 0$, $x_2 \geq 0$.

**Solution:** Constraints in standard form:
- $g_1: x_1 + x_2 - 2 \leq 0$
- $g_2: -x_1 \leq 0$
- $g_3: -x_2 \leq 0$

The unconstrained minimum is at $(0, 0)$, which satisfies all constraints. Let's verify KKT.

**Stationarity:** $\nabla f + \mu_1 \nabla g_1 + \mu_2 \nabla g_2 + \mu_3 \nabla g_3 = 0$

$$\begin{bmatrix} 0 \\ 0 \end{bmatrix} + \mu_1 \begin{bmatrix} 1 \\ 1 \end{bmatrix} + \mu_2 \begin{bmatrix} -1 \\ 0 \end{bmatrix} + \mu_3 \begin{bmatrix} 0 \\ -1 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$$

This gives $\mu_1 - \mu_2 = 0$ and $\mu_1 - \mu_3 = 0$, so $\mu_1 = \mu_2 = \mu_3$.

**Complementary slackness:**
- $\mu_1(0 + 0 - 2) = -2\mu_1 = 0 \implies \mu_1 = 0$
- $\mu_2(-0) = 0$ ✓ (satisfied for any $\mu_2$)
- $\mu_3(-0) = 0$ ✓ (satisfied for any $\mu_3$)

So $\mu_1 = \mu_2 = \mu_3 = 0$. All KKT conditions satisfied. $(0,0)$ is optimal.

$f(0, 0) = 0$.

---

### 17.5 Summary: Constrained Optimization Decision Table

| Constraint Type | Method | Key Formula |
|---|---|---|
| No constraints | Set $\nabla f = 0$, check Hessian | Second derivative test |
| Equality only ($h = 0$) | Lagrange multipliers | $\nabla f + \lambda \nabla h = 0$ |
| Inequality only ($g \leq 0$) | KKT conditions | Stationarity + Complementary Slackness |
| Mixed | KKT conditions | Full KKT with $\mu_i, \lambda_j$ |

### 17.6 Common Mistakes in KKT

| Mistake | Correction |
|---|---|
| Forgetting to check $\mu_i \geq 0$ | Dual feasibility is essential |
| Not checking all cases from complementary slackness | Systematically try $\mu_i = 0$ and $g_i = 0$ for each constraint |
| Wrong sign convention for constraints | Ensure constraints are in $g_i \leq 0$ form |
| Mixing up Lagrange (equality) and KKT (inequality) | KKT adds $\mu \geq 0$ and complementary slackness |
| Applying KKT without constraint qualification | Check that gradients of active constraints are linearly independent |

---

# 📐 MODULE 3: ADVANCED TOPICS (DA / CS Extended)

---

## Chapter 18: 🧩 Double Integrals (⭐⭐⭐ GATE DA Essential)

### 18.1 Double Integral — Definition

$$\iint_R f(x,y)\,dA = \lim_{m,n\to\infty} \sum_{i=1}^m\sum_{j=1}^n f(x_{ij}^*, y_{ij}^*)\,\Delta A$$

**Geometric meaning:** Volume under surface $z = f(x,y)$ above region $R$ in the $xy$-plane.

### 18.2 Iterated Integrals (Fubini's Theorem) ⭐⭐

> **Fubini's Theorem:** If $f$ is continuous on $R = [a,b]\times[c,d]$:
$$\iint_R f(x,y)\,dA = \int_a^b\int_c^d f(x,y)\,dy\,dx = \int_c^d\int_a^b f(x,y)\,dx\,dy$$

The order of integration can be swapped for rectangular regions.

**For general regions:**

**Type I** (vertical slices): $c \leq y \leq d$, $g_1(y) \leq x \leq g_2(y)$:
$$\iint_R f\,dA = \int_c^d\int_{g_1(y)}^{g_2(y)} f(x,y)\,dx\,dy$$

**Type II** (horizontal slices): $a \leq x \leq b$, $h_1(x) \leq y \leq h_2(x)$:
$$\iint_R f\,dA = \int_a^b\int_{h_1(x)}^{h_2(x)} f(x,y)\,dy\,dx$$

---

**Example 1 (Rectangular Region):** Evaluate $\iint_R xy\,dA$ where $R = [0,2]\times[0,1]$.

$$\int_0^2\int_0^1 xy\,dy\,dx = \int_0^2 x\left[\frac{y^2}{2}\right]_0^1\,dx = \int_0^2 \frac{x}{2}\,dx = \left[\frac{x^2}{4}\right]_0^2 = 1$$

---

**Example 2 (GATE DA 🔴 Hard — Changing Order):** Evaluate $\int_0^1\int_y^1 e^{x^2}\,dx\,dy$.

**Solution:** $e^{x^2}$ has no elementary antiderivative in $x$ — need to switch order!

Region: $y \leq x \leq 1$, $0 \leq y \leq 1$ ↔ $0 \leq x \leq 1$, $0 \leq y \leq x$.

$$\int_0^1\int_0^x e^{x^2}\,dy\,dx = \int_0^1 xe^{x^2}\,dx = \left[\frac{e^{x^2}}{2}\right]_0^1 = \frac{e-1}{2}$$

✅ **Answer: $(e-1)/2$** | Difficulty: 🔴 Hard | ⚠️ **Key GATE Trick:** Switching order of integration bypasses intractable inner integrals!

---

### 18.3 Double Integrals in Polar Coordinates ⭐⭐

$$\iint_R f(x,y)\,dA = \int_\alpha^\beta\int_{r_1(\theta)}^{r_2(\theta)} f(r\cos\theta, r\sin\theta)\cdot r\,dr\,d\theta$$

> ⚠️ **Don't forget the extra $r$** (from the Jacobian!) — the most common mistake!

**When to use polar:** Regions with circular symmetry, integrands involving $x^2+y^2$.

---

**Example (GATE DA 🔴):** Evaluate $\iint_{x^2+y^2 \leq 1} \sqrt{1-x^2-y^2}\,dA$.

$$\int_0^{2\pi}\int_0^1 \sqrt{1-r^2}\cdot r\,dr\,d\theta = 2\pi\int_0^1 r\sqrt{1-r^2}\,dr$$

Let $u = 1-r^2$: $= 2\pi\int_0^1 \frac{\sqrt{u}}{2}\,du = \pi\left[\frac{2u^{3/2}}{3}\right]_0^1 = \frac{2\pi}{3}$ ✅

(This is the volume of a hemisphere of radius 1.)

---

### 18.4 Applications of Double Integrals ⭐

| Application | Formula |
|---|---|
| **Area of region $R$** | $\iint_R 1\,dA$ |
| **Volume under $z=f(x,y)$** | $\iint_R f(x,y)\,dA$ |
| **Mass** (density $\rho(x,y)$) | $\iint_R \rho(x,y)\,dA$ |
| **Center of mass $\bar{x}$** | $\frac{\iint_R x\rho\,dA}{\iint_R \rho\,dA}$ |
| **Moment of Inertia** | $I_O = \iint_R (x^2+y^2)\rho\,dA$ |

---

### 18.5 🎯 GATE PYQ — Double Integrals

**📌 PYQ 35 (GATE DA 🔴 Hard):**

Evaluate $\int_0^2\int_0^{4-x^2} x^2\,dy\,dx$.

**Solution:** Inner integral: $\int_0^{4-x^2}x^2\,dy = x^2(4-x^2)$

$$\int_0^2 (4x^2 - x^4)\,dx = \left[\frac{4x^3}{3} - \frac{x^5}{5}\right]_0^2 = \frac{32}{3} - \frac{32}{5} = 32\cdot\frac{2}{15} = \frac{64}{15}$$

✅ **Answer: $64/15$** | Difficulty: 🟠 Medium

---

**📌 PYQ 36 (GATE DA 🔴 Hard — Switch Order):**

$\int_0^1\int_{x^2}^x (x+y)\,dy\,dx$

**Solution:** Region: $x^2 \leq y \leq x$, $0 \leq x \leq 1$.

$$\int_0^1\int_{x^2}^x (x+y)\,dy\,dx = \int_0^1\left[xy+\frac{y^2}{2}\right]_{x^2}^x\,dx = \int_0^1\left[\left(x^2+\frac{x^2}{2}\right)-\left(x^3+\frac{x^4}{2}\right)\right]\,dx$$

$$= \int_0^1\left[\frac{3x^2}{2} - x^3 - \frac{x^4}{2}\right]\,dx = \left[\frac{x^3}{2} - \frac{x^4}{4} - \frac{x^5}{10}\right]_0^1 = \frac{1}{2} - \frac{1}{4} - \frac{1}{10} = \frac{3}{20}$$

✅ **Answer: $3/20$** | Difficulty: 🟠 Medium-Hard

---

## Chapter 19: 🌊 First-Order Ordinary Differential Equations (ODE) (⭐⭐⭐ GATE DA)

### 19.1 Introduction to ODEs

An **ordinary differential equation (ODE)** involves derivatives of an unknown function $y(x)$.

**Order** = order of the highest derivative. **Degree** = power of the highest-order derivative (after clearing radicals).

**General first-order ODE:** $\frac{dy}{dx} = f(x, y)$

---

### 19.2 Separable Equations ⭐⭐

Form: $\frac{dy}{dx} = g(x)h(y)$ → Separate variables:
$$\frac{dy}{h(y)} = g(x)\,dx \implies \int\frac{dy}{h(y)} = \int g(x)\,dx$$

---

**Example 1 (Separable):** Solve $\frac{dy}{dx} = xy$.

$$\frac{dy}{y} = x\,dx \implies \ln|y| = \frac{x^2}{2} + C \implies y = Ae^{x^2/2}$$

---

**Example 2 (GATE DA 🟠):** Solve $\frac{dy}{dx} = \frac{x^2 + 1}{y}$ with $y(0) = 2$.

$$y\,dy = (x^2+1)\,dx \implies \frac{y^2}{2} = \frac{x^3}{3} + x + C$$

At $x=0$, $y=2$: $2 = C$. So $y^2 = \frac{2x^3}{3} + 2x + 4$.

---

### 19.3 Homogeneous Equations ⭐

Form: $\frac{dy}{dx} = F\!\left(\frac{y}{x}\right)$ → Substitution $v = y/x$ (so $y = vx$):

$$\frac{dy}{dx} = v + x\frac{dv}{dx}$$

This transforms to a separable equation in $v$ and $x$.

---

**Example:** Solve $\frac{dy}{dx} = \frac{y^2 - x^2}{2xy}$.

Write as $\frac{y^2-x^2}{2xy} = \frac{(y/x)^2-1}{2(y/x)} = F(v)$ where $v = y/x$.

$v + xv' = \frac{v^2-1}{2v} \implies xv' = \frac{v^2-1}{2v} - v = \frac{v^2-1-2v^2}{2v} = \frac{-v^2-1}{2v}$

$\frac{2v\,dv}{v^2+1} = -\frac{dx}{x} \implies \ln(v^2+1) = -\ln x + C \implies x(v^2+1) = A$

$x\left(\frac{y^2}{x^2}+1\right) = A \implies \frac{x^2+y^2}{x} = A$ → i.e., $x^2+y^2 = Ax$. ✅

---

### 19.4 Linear First-Order ODEs (⭐⭐⭐ Most Important!)

**Standard Form:** $\frac{dy}{dx} + P(x)y = Q(x)$

**Solution Method — Integrating Factor:**

Step 1: Find **integrating factor** $\mu(x) = e^{\int P(x)\,dx}$

Step 2: Multiply both sides by $\mu$:
$$\frac{d}{dx}[\mu y] = \mu Q(x)$$

Step 3: Integrate both sides:
$$\mu y = \int \mu Q\,dx + C$$

---

**Example 1 (GATE DA 🟠):** Solve $\frac{dy}{dx} - 2y = e^{3x}$.

$P = -2$, $Q = e^{3x}$. $\mu = e^{\int-2\,dx} = e^{-2x}$.

$$\frac{d}{dx}[e^{-2x}y] = e^{-2x}\cdot e^{3x} = e^x$$
$$e^{-2x}y = e^x + C \implies y = e^{3x} + Ce^{2x}$$

---

**Example 2 (GATE DA 🔴 Hard):** Solve $x\frac{dy}{dx} + y = x^3$ with $y(1) = 0$.

Divide by $x$: $\frac{dy}{dx} + \frac{y}{x} = x^2$.

$\mu = e^{\int 1/x\,dx} = e^{\ln x} = x$.

$\frac{d}{dx}[xy] = x^3 \implies xy = \frac{x^4}{4} + C$

At $y(1) = 0$: $0 = 1/4 + C \Rightarrow C = -1/4$.

$$y = \frac{x^3}{4} - \frac{1}{4x}$$

---

### 19.5 ⚡ Bernoulli's Equation (⭐ DA)

**Form:** $\frac{dy}{dx} + P(x)y = Q(x)y^n$

**Substitution:** $z = y^{1-n}$ → transforms to **linear** ODE:
$$\frac{dz}{dx} + (1-n)P(x)z = (1-n)Q(x)$$

---

**Example:** Solve $\frac{dy}{dx} + \frac{y}{x} = x^2 y^3$.

$n=3$, $z = y^{-2}$, $\frac{dz}{dx} = -2y^{-3}\frac{dy}{dx}$.

$\frac{dz}{dx} - \frac{2z}{x} = -2x^2$ (linear in $z$).

$\mu = e^{-2\ln x} = x^{-2}$. $\frac{d}{dx}[x^{-2}z] = -2$.

$x^{-2}z = -2x + C \implies y^{-2} = Cx^2 - 2x^3$. ✅

---

### 19.6 Exact Differential Equations (⭐⭐)

**Form:** $M(x,y)\,dx + N(x,y)\,dy = 0$

**Exact iff:** $\frac{\partial M}{\partial y} = \frac{\partial N}{\partial x}$

**Solution:** Find $\psi(x,y)$ such that $\frac{\partial\psi}{\partial x} = M$ and $\frac{\partial\psi}{\partial y} = N$.

Then solution is $\psi(x,y) = C$.

**Integrating Factor for Non-Exact Equations:**
- If $\frac{M_y - N_x}{N}$ depends only on $x$: $\mu = e^{\int\frac{M_y-N_x}{N}\,dx}$
- If $\frac{N_x - M_y}{M}$ depends only on $y$: $\mu = e^{\int\frac{N_x-M_y}{M}\,dy}$

---

**Example (Exact ODE 🟠):** Solve $(2xy+y^2)\,dx + (x^2+2xy)\,dy = 0$.

$M = 2xy+y^2$, $N = x^2+2xy$.

$M_y = 2x+2y = N_x$ ✅ (Exact!)

$\psi_x = M \Rightarrow \psi = x^2y + xy^2 + g(y)$

$\psi_y = x^2 + 2xy + g'(y) = N = x^2+2xy \Rightarrow g'(y) = 0 \Rightarrow g = C_1$

**Solution:** $x^2y + xy^2 = C$. ✅

---

### 19.7 🎯 GATE PYQ — Hard ODE Problems

**📌 PYQ 37 (GATE DA 2024 Style 🔴 Hard):**

The solution of $\frac{dy}{dx} + y\tan x = \cos^2 x$ passing through $(0, 0)$ is:

**(A)** $y = x\cos x$ **(B)** $y = \sin x\cos x$ **(C)** $y = \frac{\sin 2x}{4} + C\cos x$ **(D)** $y\sec x = \frac{x}{2} + \frac{\sin 2x}{4}$

**Solution:** Linear ODE. $P = \tan x$, $\mu = e^{\int\tan x\,dx} = e^{\ln|\sec x|} = \sec x$.

$\frac{d}{dx}[y\sec x] = \sec x\cos^2 x = \cos x$

$y\sec x = \sin x + C$. At $(0,0)$: $0 = 0 + C \Rightarrow C = 0$.

$y = \sin x\cos x = \frac{\sin 2x}{2}$.

Checking option (B): $y = \sin x\cos x$. ✅ **Answer: (B)** | Difficulty: 🔴 Hard

---

**📌 PYQ 38 (GATE DA 🔴 Hard — Exact ODE):**

Which of the following is an integrating factor for $(y^2 - 2xy)\,dx + (2x^2 - xy)\,dy = 0$?

**(A)** $\frac{1}{x}$ **(B)** $\frac{1}{y}$ **(C)** $xy$ **(D)** $\frac{1}{xy}$

**Solution:** $M_y = 2y-2x$, $N_x = 4x-y$. Not exact.

Try $\mu = \frac{1}{xy}$: New $M^* = \frac{y^2-2xy}{xy} = \frac{y}{x} - 2$, $N^* = \frac{2x^2-xy}{xy} = \frac{2x}{y} - 1$.

$M^*_y = 1/x$, $N^*_x = 2/y$. Not equal. Try $\mu = x$: $M^* = x(y^2-2xy) = xy^2-2x^2y$...

⚠️ This requires systematic checking of each option. | Difficulty: 🔴 Hard

---

**📌 PYQ 39 (GATE DA 🔴 Hard — Population Model):**

A population grows according to $\frac{dP}{dt} = 0.1P$. If $P(0) = 1000$, find $P(t)$ and determine when the population doubles.

**Solution:** Separable: $\frac{dP}{P} = 0.1\,dt \Rightarrow P = 1000e^{0.1t}$.

Double: $e^{0.1t} = 2 \Rightarrow t = \frac{\ln 2}{0.1} \approx 6.93$ units.

✅ **Doubling time** $= \frac{\ln 2}{r}$ for growth rate $r$. | Difficulty: 🟡 Easy-Medium

---

## Quick Reference: Formula Sheet (Updated 2027 🎯)

### 📐 Limits & Continuity
- $\lim_{x\to 0}\frac{\sin x}{x} = 1$, $\lim_{x\to 0}\frac{e^x-1}{x} = 1$, $\lim_{x\to 0}\frac{\ln(1+x)}{x} = 1$, $\lim_{x\to 0}\frac{1-\cos x}{x^2} = \frac{1}{2}$
- $\lim_{x\to\infty}(1+1/n)^n = e$; $(1+c/n)^n \to e^c$; $(1+a/n)^{bn}\to e^{ab}$
- L'Hôpital: **only for $0/0$ or $\infty/\infty$** forms; differentiate top/bottom independently
- $1^\infty$ form: $e^{\lim\, g(x)\cdot[f(x)-1]}$
- EVT: Continuous on $[a,b]$ → attains max and min
- Lipschitz $\Rightarrow$ Uniformly Continuous $\Rightarrow$ Continuous

### ✏️ Differentiation
- Chain rule: $(f\circ g)' = f'(g)\cdot g'$; Implicit: $dy/dx = -F_x/F_y$
- Log diff: $y = f^g$: $\ln y = g\ln f$, then differentiate
- $n$-th derivative of $e^{ax}$: $a^n e^{ax}$; of $\sin(ax)$: $a^n\sin(ax+n\pi/2)$
- Leibniz: $[fg]^{(n)} = \sum_{k=0}^n\binom{n}{k}f^{(k)}g^{(n-k)}$
- LMVT: $f'(c) = \frac{f(b)-f(a)}{b-a}$; Taylor Remainder: $R_n = \frac{f^{(n+1)}(\xi)}{(n+1)!}(x-a)^{n+1}$

### 🔢 Integration (Key Tricks)
- By parts (LIATE): $\int u\,dv = uv - \int v\,du$
- King's rule: $\int_0^a f(x)\,dx = \int_0^a f(a-x)\,dx$
- Odd function on $[-a,a]$: $\int_{-a}^a f\,dx = 0$; Even: $= 2\int_0^a$
- Leibniz (diff under integral): $\frac{d}{dx}\int_{u(x)}^{v(x)}f(t)\,dt = f(v)v' - f(u)u'$
- Riemann sum → integral: $\lim_{n\to\infty}\frac{1}{n}\sum f(i/n) = \int_0^1 f(x)\,dx$
- Improper $p$-test: $\int_1^\infty x^{-p}\,dx$ converges iff $p>1$; $\int_0^1 x^{-p}\,dx$ converges iff $p<1$
- Gamma: $\Gamma(n) = (n-1)!$; $\Gamma(1/2) = \sqrt{\pi}$; $\Gamma(n+1) = n\Gamma(n)$
- Beta: $B(m,n) = \Gamma(m)\Gamma(n)/\Gamma(m+n) = 2\int_0^{\pi/2}\sin^{2m-1}\cos^{2n-1}\,d\theta$

### 🌐 Multivariate Calculus
- Gradient: $\nabla f = (f_{x_1},\ldots,f_{x_n})^T$; points toward steepest ascent
- Directional derivative: $D_{\hat{u}}f = \nabla f\cdot\hat{u} = \|\nabla f\|\cos\theta$
- $\nabla f \perp$ level curves; max directional derivative $= \|\nabla f\|$
- Clairaut: $f_{xy} = f_{yx}$ if both are continuous
- **Euler's Theorem**: $f$ homogeneous degree $n$ → $xf_x + yf_y = nf$
- Higher-order Euler: $x^2f_{xx} + 2xyf_{xy} + y^2f_{yy} = n(n-1)f$
- Jacobian: $|J| = \det\left(\frac{\partial(f_1,\ldots,f_n)}{\partial(x_1,\ldots,x_n)}\right)$; Polar: $|J| = r$
- Tangent plane to $F(x,y,z)=0$: $\nabla F \cdot (\mathbf{r}-\mathbf{r}_0) = 0$
- Total differential: $df = f_x\,dx + f_y\,dy$

### 🏗️ Matrix Calculus
- $\nabla(\mathbf{a}^T\mathbf{x}) = \mathbf{a}$; $\nabla(\mathbf{x}^TA\mathbf{x}) = (A+A^T)\mathbf{x}$; if $A$ symmetric: $2A\mathbf{x}$
- Normal equations: $A^TA\hat{\mathbf{x}} = A^T\mathbf{b}$
- Hessian $H \succ 0$ → min; $H \prec 0$ → max; $H$ indefinite → saddle

### ⚙️ Optimization
- Unconstrained: $\nabla f = 0$, classify via Hessian determinant $D = f_{xx}f_{yy}-(f_{xy})^2$
- Lagrange: $\mathcal{L} = f + \lambda g$, solve $\nabla\mathcal{L}=0$
- KKT (4 conditions): Stationarity + Primal Feasibility + Dual Feasibility ($\mu\geq 0$) + Complementary Slackness ($\mu g = 0$)
- Convex $f$ + convex $g_i$ → KKT necessary **and sufficient**
- Gradient descent: $\mathbf{x}_{k+1} = \mathbf{x}_k - \alpha\nabla f(\mathbf{x}_k)$; converges for $\alpha \leq 1/L$ (Lipschitz constant)
- Newton's method: $\mathbf{x}_{k+1} = \mathbf{x}_k - H^{-1}\nabla f$; quadratic convergence

### 🌊 ODEs
- Separable: $g(y)\,dy = f(x)\,dx$ → integrate both sides
- Linear: $y' + P(x)y = Q(x)$; IF = $e^{\int P\,dx}$; $(\mu y)' = \mu Q$
- Homogeneous: $y'=F(y/x)$; sub $v=y/x$
- Exact: $M_y = N_x$ → find $\psi$; solution: $\psi = C$
- Bernoulli: $y'+Py = Qy^n$; sub $z=y^{1-n}$

---

## 🎯 GATE Traps Master List — Calculus Edition ⚠️

> 🔑 **These are the most frequently exploited traps in GATE CS and DA exams!**

### 🔬 Limits Traps
| ❌ Common Mistake | ✅ Correct Thinking |
|---|---|
| Applying L'Hôpital without $0/0$ or $\infty/\infty$ | Verify the form first EVERY time |
| $\lim_{x\to\infty}\frac{\sin x}{x} = 1$ | $= 0$ ($\sin x$ bounded, denominator $\to \infty$) |
| $\lim_{x\to\infty} x\sin(1/x) = 0$ | $= 1$ (sub $t=1/x$, standard limit) |
| $0^0 = 0$ or $0^0 = 1$ without work | Use log method: $e^{\lim x\ln x}$ |
| Swapping order of limits freely | Check joint continuity first! |
| $x^n \to f(x)$ pointwise → $f$ continuous | FALSE: e.g., $x^n \to \begin{cases}0 & x<1\\1 & x=1\end{cases}$ |

### ✏️ Differentiability Traps
| ❌ Common Mistake | ✅ Correct Thinking |
|---|---|
| Continuous → Differentiable | FALSE: $|x|$ is continuous but not diff at 0 |
| $f'(c)=0$ → local extremum | It could be inflection: $f(x)=x^3$ at $x=0$ |
| L'Hôpital can find $f'(a)$ directly | Must use limit definition: $f'(a) = \lim\frac{f(a+h)-f(a)}{h}$ |
| $f''=0$ → inflection point | $f''$ must CHANGE SIGN: $x^4$ at 0 has $f''(0)=0$ but is a min |
| Differentiating $[f(x)]^{g(x)}$ using power rule alone | Use logarithmic differentiation! |

### 🔢 Integration Traps
| ❌ Common Mistake | ✅ Correct Thinking |
|---|---|
| $\int_0^1 \frac{1}{x}\,dx$ is finite | Diverges! ($p$-test: $p=1$, not $<1$) |
| Dropping $|J|=r$ in polar coordinates | $\iint f\,dA = \iint f(r,\theta)\cdot r\,dr\,d\theta$ |
| Directly integrating when inner integral is intractable | Switch order of integration! |
| Using Wallis without normalization | $\int_0^{\pi/2}\sin^n x\,dx \neq 1/n$ |
| $\Gamma(0)$ or $\Gamma$ at negative integers | Undefined at $0, -1, -2, \ldots$ |

### 🌐 Partial Derivatives & Euler's Theorem Traps
| ❌ Common Mistake | ✅ Correct Thinking |
|---|---|
| Applying Euler directly to $f(u(x,y))$ | Apply to inner function, chain rule back! |
| Checking homogeneity only term-by-term | Check the whole expression: $\frac{x^3+y^3}{x^2+y^2}$ is degree 1 |
| Forgetting degree-0 result: $xu_x+yu_y=0$ | Functions of $y/x$ are homogeneous of degree 0! |
| Jacobian of inverse = inverse of Jacobian | TRUE: $J_{fg} = (J_{gf})^{-1}$ |
| Forgetting extra $r$ in polar Jacobian | Polar integral: always $r\,dr\,d\theta$ |

### ⚙️ Optimization Traps
| ❌ Common Mistake | ✅ Correct Thinking |
|---|---|
| $D = f_{xx}f_{yy}-(f_{xy})^2 = 0$ → no extremum | Test is inconclusive; need higher-order analysis |
| Ignoring endpoints in closed interval optimization | Always evaluate $f$ at boundary points too! |
| All local minima = global minima | Only true for convex functions |
| KKT multiplier $\mu < 0$ is OK | Dual feasibility requires $\mu \geq 0$ for $\leq$ constraints |
| Gradient descent always finds global min | Only guaranteed for convex functions |
| $\mu = 0$ means constraint doesn't hold | $\mu = 0$ means constraint is inactive (slack), not violated |

### 🌊 ODE Traps
| ❌ Common Mistake | ✅ Correct Thinking |
|---|---|
| Using wrong integrating factor | Show $M_y \neq N_x$ before trying IF |
| Forgetting constant $C$ in solution | Every first-order ODE has a family of solutions |
| $dy/dx = f(y/x)$ → directly solving | Sub $v = y/x$, $y = vx$, $y' = v + xv'$ |

---

## GATE PYQ Pattern Analysis (Updated)

| Topic | GATE CS/DA Weight | Type of Questions | Key Tips |
|---|---|---|---|
| Limits | ⭐⭐⭐ High | Tricky indeterminate forms, L'Hôpital, Taylor method | Know standard limits + growth hierarchy |
| Continuity/Differentiability | ⭐⭐⭐ High | Piecewise functions, $x\sin(1/x)$ family, uniform continuity | Always check LHL=RHL, LHD=RHD |
| LMVT/Rolle's | ⭐⭐ Medium | Find $c$, prove inequalities, count roots | Verify ALL conditions; Taylor remainder |
| Maxima/Minima | ⭐⭐⭐ High | Classify critical points, applied optimization | Second derivative test + Hessian for 2D |
| Inflection/Concavity | ⭐⭐ Medium | Identify inflection points | $f''$ must CHANGE SIGN, not just be zero |
| Integration | ⭐⭐⭐⭐ Very High | Definite integral properties, King's rule, IBP | Practice odd/even, King's rule shortcut |
| Improper Integrals | ⭐⭐ Medium | Convergence/divergence, $p$-test | Remember $p>1$ for $[1,\infty)$, $p<1$ for $[0,1]$ |
| Riemann Sums | ⭐⭐ Medium | Recognize sums as integrals | $\frac{1}{n}\sum f(i/n) = \int_0^1 f(x)\,dx$ |
| Taylor Series | ⭐⭐⭐ High | Coefficients, limits via series, convergence | Memorize first 5 terms of $e^x$, $\sin$, $\cos$, $\ln(1+x)$ |
| Euler's Theorem | ⭐⭐⭐ GATE Favorite! | $xf_x+yf_y=nf$, composite functions | Identify degree, handle $f(u)$ via chain rule |
| Jacobian | ⭐⭐⭐ DA | Variable transformation, change of variables | Polar $\|J\|=r$; verify inverse Jacobian relation |
| Total Differential | ⭐⭐ Medium | Approximations, exact differentials | $\Delta f \approx f_x\Delta x + f_y\Delta y$ |
| Gradient/Directional | ⭐⭐⭐ High | Max rate of change, level curves | Gradient ⊥ contours; max DD = $\|\nabla f\|$ |
| Lagrange Multipliers | ⭐⭐⭐ High | Constrained max/min | Geometric condition: $\nabla f \parallel \nabla g$ |
| KKT Conditions | ⭐⭐⭐ High (DA) | Inequality constraints, SVM dual | Complementary slackness is the KEY condition |
| Convexity | ⭐⭐ High (DA) | Verify convex/concave, PSD Hessian | $H \succeq 0$ for convex; local min = global min |
| Double Integrals | ⭐⭐⭐ DA | Iterated integrals, switching order | Switch order when inner integral is stuck |
| ODEs | ⭐⭐⭐ DA | Linear 1st order, separable, exact | Identify type first; find integrating factor |
| Gamma/Beta | ⭐⭐ DA | Evaluate improper integrals | $\Gamma(n) = (n-1)!$; $\Gamma(1/2)=\sqrt{\pi}$ |

---

## ✅ GATE CS/DA Calculus Micro-Checklist 🎯

> Use this during final revision. If you can solve 2–3 unseen problems on each bullet, you're exam-ready!

### Single Variable Calculus
- [ ] Evaluate limits using all 5 methods (direct, factoring, rationalization, standard forms, L'Hôpital)
- [ ] Handle $0^0$, $1^\infty$, $\infty^0$, $\infty-\infty$ forms using log method
- [ ] Apply Squeeze Theorem for oscillatory limits
- [ ] Check continuity (3 conditions) and identify type of discontinuity
- [ ] EVT: identify when max/min are guaranteed
- [ ] Uniform continuity vs. plain continuity — know the difference
- [ ] Lipschitz condition and its role in machine learning
- [ ] Differentiate implicitly, logarithmically, parametrically
- [ ] Compute $n$-th derivatives using formulas; apply Leibniz formula
- [ ] Apply Rolle's, LMVT, Cauchy MVT and prove inequalities
- [ ] Compute Taylor/Maclaurin series; find radii of convergence
- [ ] Apply Taylor's Remainder Theorem for error bounds
- [ ] Classify critical points: first derivative test vs. second derivative test
- [ ] Find inflection points (sign change of $f''$, not just $f''=0$)
- [ ] Global optimization on closed intervals (check endpoints!)
- [ ] Evaluate definite integrals using all properties (King's, odd/even, splitting)
- [ ] Apply integration by parts (LIATE), substitution, partial fractions
- [ ] Recognize and evaluate Riemann sums as definite integrals
- [ ] Apply FTC Part 1 and Part 2; use Leibniz differentiation rule
- [ ] Determine convergence/divergence of improper integrals (Type I, II; $p$-test)
- [ ] Evaluate integrals using Gamma and Beta functions
- [ ] Apply Wallis formula and reduction formulae

### Multivariate & Optimization
- [ ] Compute partial derivatives, chain rule, higher-order mixed partials (Clairaut)
- [ ] Identify homogeneous functions; apply Euler's theorem correctly (including composite)
- [ ] Use total differential for approximation; identify exact differentials
- [ ] Compute Jacobian matrix/determinant; apply to change of variables
- [ ] Find equations of tangent planes and normal lines to surfaces
- [ ] Compute gradient, divergence, curl; interpret geometrically
- [ ] Find directional derivatives; identify direction of max rate of change
- [ ] Classify critical points of $f(x,y)$ using Hessian ($D$ test)
- [ ] Test functions for convexity/concavity using Hessian PSD/NSD
- [ ] Know first-order and second-order optimality conditions
- [ ] Solve constrained optimization using Lagrange multipliers
- [ ] Apply KKT conditions for inequality-constrained problems
- [ ] Apply gradient descent update rule; understand role of learning rate
- [ ] Newton's method for optimization; compare with gradient descent

### DA Extended Topics
- Evaluate double integrals (rectangular and polar coordinates)
- Switch order of integration in double integrals
- Solve separable, homogeneous, linear, exact, and Bernoulli ODEs
- Apply integrating factor method for linear ODEs
- Evaluate power series sums by differentiation/integration tricks
- Apply Gamma/Beta functions to evaluate complex definite integrals

---

## 📝 Practice Set 1: Limits & Continuity (P1 – P60)

**P1.** (1-mark) $\lim_{x \to 0} \frac{\sin 5x}{3x}$ = ?
→ $= \frac{5}{3} \cdot \lim_{x \to 0} \frac{\sin 5x}{5x} = \frac{5}{3} \cdot 1 = \boxed{\frac{5}{3}}$

**P2.** (1-mark) $\lim_{x \to 0} \frac{\tan x - \sin x}{x^3}$ = ?
→ $\frac{\tan x - \sin x}{x^3} = \frac{\sin x(1 - \cos x)}{x^3 \cos x}$
→ $= \frac{\sin x}{x} \cdot \frac{1 - \cos x}{x^2} \cdot \frac{1}{\cos x} = 1 \cdot \frac{1}{2} \cdot 1 = \boxed{\frac{1}{2}}$

**P3.** (2-mark) $\lim_{x \to 0} \frac{e^x - 1 - x}{x^2}$ = ?
→ L'Hôpital (0/0): $\frac{e^x - 1}{2x}$ → still 0/0 → $\frac{e^x}{2} = \frac{1}{2}$
→ OR Taylor: $e^x = 1 + x + \frac{x^2}{2} + \cdots$ → $\frac{x^2/2 + \cdots}{x^2} = \boxed{\frac{1}{2}}$

**P4.** (2-mark) $\lim_{x \to \infty} \left(1 + \frac{3}{x}\right)^{2x}$ = ?
→ Form $1^\infty$. Let $y = \left(1 + \frac{3}{x}\right)^{2x}$
→ $\ln y = 2x \ln\left(1 + \frac{3}{x}\right) \approx 2x \cdot \frac{3}{x} = 6$
→ $y = e^6 = \boxed{e^6}$

**P5.** (1-mark) $\lim_{x \to 0} \frac{x - \sin x}{x^3}$ = ?
→ Taylor: $\sin x = x - \frac{x^3}{6} + \cdots$ → $\frac{x^3/6}{x^3} = \boxed{\frac{1}{6}}$

**P6.** (2-mark) $\lim_{x \to 0} \left(\frac{\sin x}{x}\right)^{1/x^2}$ = ?
→ Form $1^\infty$. $\ln L = \lim \frac{1}{x^2} \ln\left(\frac{\sin x}{x}\right)$
→ $\frac{\sin x}{x} \approx 1 - \frac{x^2}{6}$ → $\ln(1 - x^2/6) \approx -x^2/6$
→ $\frac{-x^2/6}{x^2} = -\frac{1}{6}$ → $L = e^{-1/6} = \boxed{e^{-1/6}}$

**P7.** (1-mark) $\lim_{n \to \infty} \left(\frac{n!}{n^n}\right)^{1/n}$ = ?
→ By Stirling: $n! \approx \sqrt{2\pi n}\left(\frac{n}{e}\right)^n$ → $\frac{n!}{n^n} \approx \frac{\sqrt{2\pi n}}{e^n}$
→ $\left(\frac{\sqrt{2\pi n}}{e^n}\right)^{1/n} = \frac{(2\pi n)^{1/(2n)}}{e} \to \frac{1}{e}$
→ $= \boxed{\frac{1}{e}}$

**P8.** (2-mark) $f(x) = \begin{cases} \frac{x^2 - 4}{x - 2} & x \neq 2 \\ k & x = 2 \end{cases}$. Find $k$ for continuity at $x = 2$.
→ $\lim_{x \to 2} \frac{x^2 - 4}{x - 2} = \lim_{x \to 2} \frac{(x-2)(x+2)}{x-2} = 4$
→ $k = \boxed{4}$

**P9.** (1-mark) Is $f(x) = |x|$ continuous at $x = 0$? Differentiable?
→ Continuous: YES ($\lim_{x \to 0} |x| = 0 = f(0)$)
→ Differentiable: NO (LHD = -1, RHD = +1)

**P10.** (2-mark) $\lim_{x \to 0} \frac{\ln(1 + x) - x + x^2/2}{x^3}$ = ?
→ Taylor: $\ln(1+x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots$
→ $\frac{x - x^2/2 + x^3/3 - x + x^2/2}{x^3} = \frac{x^3/3}{x^3} = \boxed{\frac{1}{3}}$

**P11.** (1-mark) $\lim_{x \to 0} \frac{a^x - 1}{x}$ = ?
→ $= \boxed{\ln a}$ (standard limit)

**P12.** (2-mark) Find $\lim_{x \to 0^+} x^x$.
→ Form $0^0$. $\ln y = x \ln x \to 0 \cdot (-\infty)$
→ Rewrite: $\frac{\ln x}{1/x}$ → L'Hôpital: $\frac{1/x}{-1/x^2} = -x \to 0$
→ $\ln y \to 0$ → $y = e^0 = \boxed{1}$

**P13.** (2-mark) $\lim_{x \to \infty} x^{1/x}$ = ?
→ Form $\infty^0$. $\ln y = \frac{\ln x}{x} \to 0$ (by L'Hôpital: $\frac{1/x}{1} = 0$)
→ $y = e^0 = \boxed{1}$

**P14.** (1-mark) $\lim_{x \to 0} \frac{1 - \cos x}{x^2}$ = ?
→ $= \boxed{\frac{1}{2}}$ (standard; Taylor: $\cos x = 1 - x^2/2 + \cdots$)

**P15.** (2-mark) $\lim_{x \to 0} \frac{e^x + e^{-x} - 2}{x^2}$ = ?
→ Taylor: $e^x = 1 + x + x^2/2 + \cdots$, $e^{-x} = 1 - x + x^2/2 + \cdots$
→ Sum: $2 + x^2 + \cdots$ → $\frac{x^2}{x^2} = \boxed{1}$

**P16-P30** — Quick Limits:

| # | Limit | Answer |
|---|---|---|
| P16 | $\lim_{x \to 0} \frac{\sin(ax)}{\sin(bx)}$ | $\frac{a}{b}$ |
| P17 | $\lim_{x \to 0} \frac{\tan(ax)}{x}$ | $a$ |
| P18 | $\lim_{x \to 0} (1 + ax)^{b/x}$ | $e^{ab}$ |
| P19 | $\lim_{x \to \infty} \frac{x^n}{e^x}$ for any $n$ | $0$ (exponential dominates) |
| P20 | $\lim_{x \to \infty} \frac{e^x}{x^n}$ | $\infty$ |
| P21 | $\lim_{x \to \infty} \frac{\ln x}{x^a}$ for $a > 0$ | $0$ (polynomial dominates log) |
| P22 | $\lim_{x \to 0^+} x \ln x$ | $0$ |
| P23 | $\lim_{n \to \infty} \left(1 + \frac{1}{n}\right)^n$ | $e$ |
| P24 | $\lim_{n \to \infty} n^{1/n}$ | $1$ |
| P25 | $\lim_{x \to 0} \frac{\sin^{-1} x}{x}$ | $1$ |
| P26 | $\lim_{x \to 0} \frac{\tan^{-1} x}{x}$ | $1$ |
| P27 | $\lim_{x \to \pi/2} (\sec x - \tan x)$ | $0$ |
| P28 | $\lim_{x \to 0} \frac{(1+x)^n - 1}{x}$ | $n$ (binomial) |
| P29 | $\lim_{x \to a} \frac{x^n - a^n}{x - a}$ | $n \cdot a^{n-1}$ |
| P30 | $\lim_{x \to 0} \frac{e^{3x} - 1}{x}$ | $3$ |

**P31-P45** — Continuity & Differentiability:

**P31.** (2-mark) $f(x) = \begin{cases} x^2 \sin(1/x) & x \neq 0 \\ 0 & x = 0 \end{cases}$. Differentiable at $x = 0$?
→ $f'(0) = \lim_{h \to 0} \frac{h^2 \sin(1/h)}{h} = \lim_{h \to 0} h \sin(1/h) = 0$
→ YES, differentiable at $x = 0$ with $f'(0) = 0$. But $f'(x)$ discontinuous at 0 (derivative exists but not continuous).

**P32.** (2-mark) $f(x) = x|x|$. Is $f$ differentiable at $x = 0$?
→ $f(x) = \begin{cases} x^2 & x \geq 0 \\ -x^2 & x < 0 \end{cases}$
→ $f'(0^+) = 0$, $f'(0^-) = 0$ → YES differentiable, $f'(0) = 0$.

**P33.** (1-mark) $f(x) = \lfloor x \rfloor$ (floor function). Points of discontinuity?
→ Discontinuous at every integer $n$ (LHL = $n-1$, RHL = $n$).

**P34.** (2-mark) If $f$ and $g$ are discontinuous at $a$, is $f + g$ necessarily discontinuous?
→ **NO.** Example: $f(x) = \begin{cases} 1 & x = 0 \\ 0 & \text{else} \end{cases}$, $g(x) = \begin{cases} -1 & x = 0 \\ 0 & \text{else} \end{cases}$ → $f+g = 0$ (continuous!)

**P35.** (2-mark) $f(x) = \begin{cases} e^{-1/x^2} & x \neq 0 \\ 0 & x = 0 \end{cases}$. Find $f'(0)$.
→ $f'(0) = \lim_{h \to 0} \frac{e^{-1/h^2}}{h}$. As $h \to 0$, $e^{-1/h^2} \to 0$ faster than $h \to 0$.
→ $f'(0) = 0$. In fact, ALL derivatives at 0 are 0! (Famous: $C^\infty$ but not analytic.)

**P36-P45** — True/False:

| # | Statement | T/F |
|---|---|---|
| P36 | Every differentiable function is continuous | TRUE |
| P37 | Every continuous function is differentiable | FALSE ($|x|$ at 0) |
| P38 | $f(x) = x^{2/3}$ is differentiable at $x = 0$ | FALSE (vertical tangent, $f'(0) = \infty$) |
| P39 | Intermediate Value Theorem requires differentiability | FALSE (only continuity on $[a,b]$) |
| P40 | $\lim_{x \to a} f(x)$ can exist even if $f(a)$ is undefined | TRUE |
| P41 | Removable discontinuity: limit exists but ≠ $f(a)$ | TRUE |
| P42 | Jump discontinuity: LHL ≠ RHL | TRUE |
| P43 | $f(x) = \sin(1/x)$ has removable discontinuity at 0 | FALSE (limit doesn't exist — oscillates) |
| P44 | $f(x) = x \sin(1/x)$ has removable discontinuity at 0 | TRUE (limit = 0, just define $f(0) = 0$) |
| P45 | Weierstrass function: continuous everywhere, differentiable nowhere | TRUE |

**P46-P60** — GATE Style MCQs:

**P46.** (2-mark) $\lim_{x \to 0} \frac{(1+x)^{1/x} - e}{x}$ = ?
→ Let $f(x) = (1+x)^{1/x}$. $\ln f = \frac{\ln(1+x)}{x} = 1 - \frac{x}{2} + \frac{x^2}{3} - \cdots$
→ $f = e^{1 - x/2 + \cdots} = e \cdot e^{-x/2 + \cdots} \approx e(1 - x/2)$
→ $\frac{f - e}{x} = \frac{e(-x/2)}{x} = -\frac{e}{2}$
→ $= \boxed{-\frac{e}{2}}$

**P47.** (2-mark NAT) $\lim_{x \to 1} \frac{x^{10} - 1}{x - 1}$ = ?
→ $= 10 \cdot 1^9 = \boxed{10}$ (by standard limit $\frac{x^n - a^n}{x - a} = na^{n-1}$)

**P48.** (2-mark) $\lim_{x \to 0} \frac{\sqrt{1 + x} - \sqrt{1 - x}}{x}$ = ?
→ Rationalize: $\frac{(1+x) - (1-x)}{x(\sqrt{1+x} + \sqrt{1-x})} = \frac{2x}{x \cdot 2} = \boxed{1}$

**P49.** (2-mark) $f(x) = \frac{|x-2|}{x-2}$. Find $\lim_{x \to 2^+} f(x)$ and $\lim_{x \to 2^-} f(x)$.
→ $x > 2$: $f = +1$. $x < 2$: $f = -1$. LHL = $-1$, RHL = $+1$. **Limit DNE**.

**P50.** (2-mark) Apply Squeeze theorem: $\lim_{x \to \infty} \frac{\sin x}{x}$ = ?
→ $-\frac{1}{x} \leq \frac{\sin x}{x} \leq \frac{1}{x}$ → both bounds → 0 → $\boxed{0}$

**P51-P55** — Sequence Limits:

| # | Sequence | Limit |
|---|---|---|
| P51 | $a_n = \frac{3n^2 + 1}{n^2 - 5}$ | $3$ (divide by $n^2$) |
| P52 | $a_n = \left(\frac{n+1}{n}\right)^n$ | $e$ |
| P53 | $a_n = n \sin(1/n)$ | $1$ (use $\sin \theta / \theta \to 1$) |
| P54 | $a_n = \frac{2^n}{n!}$ | $0$ (factorial grows faster) |
| P55 | $a_n = (1 + 2/n)^{3n}$ | $e^6$ |

**P56-P60** — Challenge:

**P56.** (2-mark) $\lim_{n \to \infty} \sum_{k=1}^{n} \frac{1}{n+k}$ = ?
→ Riemann sum: $\sum \frac{1}{n} \cdot \frac{1}{1 + k/n} \to \int_0^1 \frac{dx}{1+x} = \ln 2$
→ $= \boxed{\ln 2}$

**P57.** (2-mark) $\lim_{n \to \infty} \frac{1}{n} \sum_{k=1}^{n} \sin\left(\frac{k\pi}{n}\right)$ = ?
→ Riemann sum: $\int_0^1 \sin(\pi x) \, dx = \left[-\frac{\cos(\pi x)}{\pi}\right]_0^1 = \frac{2}{\pi}$
→ $= \boxed{\frac{2}{\pi}}$

**P58.** (2-mark) Find where $f(x) = \frac{x}{1 + e^{1/x}}$ is discontinuous.
→ At $x = 0$: $\lim_{x \to 0^+} = \frac{0}{1+e^{+\infty}} = 0$, $\lim_{x \to 0^-} = \frac{0}{1+e^{-\infty}} = \frac{0}{1} = 0$
→ **Continuous at 0!** (Removable if undefined; depends on $f(0)$ definition)

**P59.** (1-mark) Number of points where $f(x) = \lfloor \sin x \rfloor$ is discontinuous on $[0, 2\pi]$?
→ Discontinuous when $\sin x$ crosses integers: at $\sin x = 0$ (i.e., $x = \pi, 2\pi$) and $\sin x = 1$ (nah, just touches). Also at $x$ where $\sin x$ crosses from $<0$ to $\geq 0$: $x = \pi$.
→ Actually: $\lfloor \sin x \rfloor$ changes value at $x = \pi/2$ (sin goes from $<1$ to $1$? No, $\sin(\pi/2) = 1$), $x = \pi$ (sin becomes 0 from positive), $x = 3\pi/2$ (sin reaches $-1$), etc.
→ Discontinuous at $x = \pi, 2\pi, 3\pi/2$... Careful analysis: **3 points** on $[0, 2\pi]$

**P60.** (2-mark) $g(x) = \begin{cases} \frac{\sin(x^2)}{x} & x \neq 0 \\ 0 & x = 0 \end{cases}$. Is $g$ differentiable at 0?
→ $g'(0) = \lim_{h \to 0} \frac{g(h) - g(0)}{h} = \lim_{h \to 0} \frac{\sin(h^2)/h}{h} = \lim_{h \to 0} \frac{\sin(h^2)}{h^2} = 1$
→ **YES,** $g'(0) = 1$.

---

## 📝 Practice Set 2: Differentiation & MVT (P61 – P130)

**P61.** (1-mark) If $f(x) = e^{x \ln x}$, find $f'(1)$.
→ $f(x) = x^x$. $\ln f = x \ln x$ → $\frac{f'}{f} = \ln x + 1$ → $f' = x^x(\ln x + 1)$
→ $f'(1) = 1^1 \cdot (0 + 1) = \boxed{1}$

**P62.** (2-mark) Find the 10th derivative of $x^9$ at any point.
→ $\frac{d^{10}}{dx^{10}}(x^9) = 0$ (degree 9, 10th derivative = 0). $\boxed{0}$

**P63.** (2-mark) Find $f^{(n)}(0)$ where $f(x) = e^x \sin x$.
→ By Leibniz's rule: $f^{(n)}(0) = \sum_{k=0}^{n} \binom{n}{k} (e^x)^{(k)}\Big|_0 (\sin x)^{(n-k)}\Big|_0$
→ $(e^x)^{(k)} = e^x$, at 0 = 1
→ $(\sin x)^{(m)}|_0 = \sin(m\pi/2)$ → only nonzero when $m$ is odd
→ $f^{(n)}(0) = \sum_{k=0}^{n} \binom{n}{k} \sin\left(\frac{(n-k)\pi}{2}\right) = (\sqrt{2})^n \sin\left(\frac{n\pi}{4}\right)$
→ $\boxed{2^{n/2} \sin(n\pi/4)}$

**P64.** (1-mark) $\frac{d}{dx}\left(\sin^{-1} x\right)$ = ?
→ $\boxed{\frac{1}{\sqrt{1-x^2}}}$, $|x| < 1$

**P65.** (2-mark) $y = \left(\ln x\right)^{\ln x}$. Find $y'$.
→ $\ln y = \ln x \cdot \ln(\ln x)$
→ $\frac{y'}{y} = \frac{1}{x} \ln(\ln x) + \frac{\ln x}{x \ln x} = \frac{\ln(\ln x) + 1}{x}$
→ $y' = \frac{(\ln x)^{\ln x} [\ln(\ln x) + 1]}{x}$

**P66.** (2-mark) Rolle's Theorem: $f(x) = x^3 - 3x + 1$ on $[-1, 1]$. Verify.
→ $f(-1) = -1 + 3 + 1 = 3$, $f(1) = 1 - 3 + 1 = -1$. Since $f(-1) \neq f(1)$, **Rolle's does NOT apply!**
→ GATE trap: always check $f(a) = f(b)$ first!

**P67.** (2-mark) Apply LMVT to $f(x) = x^3$ on $[1, 3]$. Find $c$.
→ $\frac{f(3) - f(1)}{3 - 1} = \frac{27 - 1}{2} = 13$
→ $f'(c) = 3c^2 = 13$ → $c = \sqrt{13/3} \approx 2.08$
→ $c \in (1, 3)$ ✅

**P68.** (2-mark) Using MVT, prove: $\ln(1+x) < x$ for $x > 0$.
→ Let $f(t) = \ln(1+t)$ on $[0, x]$. By LMVT: $\frac{\ln(1+x) - 0}{x - 0} = \frac{1}{1+c}$ for some $c \in (0, x)$.
→ $\frac{1}{1+c} < 1$ (since $c > 0$) → $\frac{\ln(1+x)}{x} < 1$ → $\ln(1+x) < x$ ✅

**P69.** (1-mark) If $f'(x) = 0$ for all $x$ in an interval, then $f$ is?
→ **Constant** on that interval (by MVT consequence).

**P70.** (2-mark) $f(x) = x + \frac{1}{x}$. Find absolute minimum for $x > 0$.
→ $f'(x) = 1 - \frac{1}{x^2} = 0$ → $x = 1$
→ $f''(1) = \frac{2}{x^3}\Big|_1 = 2 > 0$ → minimum
→ $f(1) = 2$. $\boxed{\text{Min} = 2}$

**P71.** (2-mark) Find the maximum of $f(x) = x e^{-x}$ for $x \geq 0$.
→ $f'(x) = e^{-x} - xe^{-x} = e^{-x}(1-x) = 0$ → $x = 1$
→ $f''(1) = e^{-1}(-1) < 0$ → maximum. $f(1) = e^{-1} = \boxed{\frac{1}{e}}$

**P72.** (2-mark) Taylor expansion of $e^x$ around $x = 0$ up to $x^4$:
→ $e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \frac{x^4}{4!} + \cdots$

Remainder after $n$ terms: $R_n = \frac{f^{(n+1)}(c)}{(n+1)!} x^{n+1}$ for some $c$ between 0 and $x$.

**P73.** (2-mark) Taylor series of $\frac{1}{1-x}$ centered at 0:
→ $\sum_{n=0}^{\infty} x^n = 1 + x + x^2 + x^3 + \cdots$ for $|x| < 1$

**P74.** (2-mark) Using Taylor: approximate $\sqrt{1.01}$ to 4 decimal places.
→ $\sqrt{1+x} \approx 1 + \frac{x}{2} - \frac{x^2}{8}$, with $x = 0.01$
→ $\approx 1 + 0.005 - 0.0000125 = 1.004988$
→ Actual: **1.004988** (matches to 6 decimal places!)

**P75-P90** — Derivative Computations:

| # | Function | Derivative |
|---|---|---|
| P75 | $\frac{d}{dx}(x^n)$ | $nx^{n-1}$ |
| P76 | $\frac{d}{dx}(a^x)$ | $a^x \ln a$ |
| P77 | $\frac{d}{dx}(\log_a x)$ | $\frac{1}{x \ln a}$ |
| P78 | $\frac{d}{dx}(\sec^{-1} x)$ | $\frac{1}{|x|\sqrt{x^2-1}}$ |
| P79 | $\frac{d}{dx}(\cosh x)$ | $\sinh x$ |
| P80 | $\frac{d}{dx}(\tanh x)$ | $\text{sech}^2 x$ |
| P81 | $f(x) = x^{1/x}$, $f'(e) = ?$ | $f' = x^{1/x} \cdot \frac{1 - \ln x}{x^2}$ → $f'(e) = e^{1/e} \cdot 0 = 0$ |
| P82 | $\frac{d}{dx}\left(\int_0^{x^2} e^{-t^2} dt\right)$ | $2x \cdot e^{-x^4}$ (Leibniz integral rule) |
| P83 | $y = e^{\sin x}$, $y'' = ?$ | $y' = e^{\sin x}\cos x$; $y'' = e^{\sin x}(\cos^2 x - \sin x)$ |
| P84 | Implicit: $x^2 + y^2 = 25$, $y' = ?$ | $-x/y$ |
| P85 | Implicit: $x^y = y^x$, $y' = ?$ at $(2, 4)$? | Take log: $y \ln x = x \ln y$ → differentiate → complex! |

**P86-P90** — MVT Applications:

| # | Question | Answer |
|---|---|---|
| P86 | Cauchy MVT for $f = \sin x$, $g = \cos x$ on $[0, \pi/2]$? | $\frac{\sin(\pi/2) - 0}{0 - 1} = \frac{-\cos c}{\sin c}$ → $c = \pi/4$ |
| P87 | $f(x) = x^4$ on $[0, 1]$: LMVT value of $c$? | $4c^3 = 1$ → $c = (1/4)^{1/3} = \frac{1}{\sqrt[3]{4}}$ |
| P88 | Show $|\sin a - \sin b| \leq |a - b|$ | MVT: $\sin a - \sin b = \cos(c)(a-b)$ → $|\cdot| \leq |a-b|$ since $|\cos c| \leq 1$ |
| P89 | Rolle's theorem for $f(x) = e^x \sin x$ on $[0, \pi]$? | $f(0) = 0, f(\pi) = 0$ ✅. $f'(c) = e^c(\sin c + \cos c) = 0$ → $c = 3\pi/4$ |
| P90 | How many roots of $x^5 + x + 1 = 0$ in $\mathbb{R}$? | $f'(x) = 5x^4 + 1 > 0$ always → strictly increasing → exactly **1 real root** |

**P91-P110** — Integration:

**P91.** (2-mark) $\int_0^1 x e^x \, dx$ = ?
→ By parts: $u = x, dv = e^x dx$ → $[xe^x]_0^1 - \int_0^1 e^x dx = e - (e - 1) = \boxed{1}$

**P92.** (2-mark) $\int_0^{\pi/2} \sin^2 x \, dx$ = ?
→ Wallis / identity: $\sin^2 x = \frac{1 - \cos 2x}{2}$
→ $\int_0^{\pi/2} \frac{1}{2} dx = \frac{\pi}{4}$
→ $= \boxed{\frac{\pi}{4}}$

**P93.** (2-mark) $\int_0^{\infty} e^{-x^2} dx$ = ?
→ Gaussian integral: $= \boxed{\frac{\sqrt{\pi}}{2}}$

**P94.** (2-mark) $\int_0^1 \frac{dx}{\sqrt{1-x^2}}$ = ?
→ $= [\sin^{-1} x]_0^1 = \frac{\pi}{2} - 0 = \boxed{\frac{\pi}{2}}$

**P95.** (2-mark) $\Gamma(5) = ?$
→ $\Gamma(n) = (n-1)!$ → $\Gamma(5) = 4! = \boxed{24}$

**P96.** (1-mark) $\Gamma(1/2) = ?$
→ $= \boxed{\sqrt{\pi}}$ (famous result)

**P97.** (2-mark) $\int_0^{\infty} x^3 e^{-x} dx$ = ?
→ $= \Gamma(4) = 3! = \boxed{6}$

**P98.** (2-mark) $B(m, n) = ?$ in terms of Gamma.
→ $B(m,n) = \frac{\Gamma(m)\Gamma(n)}{\Gamma(m+n)}$

**P99.** (2-mark) $\int_0^1 x^3(1-x)^4 \, dx$ = ?
→ $= B(4, 5) = \frac{\Gamma(4)\Gamma(5)}{\Gamma(9)} = \frac{3! \cdot 4!}{8!} = \frac{6 \cdot 24}{40320} = \frac{144}{40320} = \boxed{\frac{1}{280}}$

**P100.** (2-mark) $\int_0^{\pi/2} \sin^6 x \, dx$ = ?
→ Wallis: $\frac{5 \cdot 3 \cdot 1}{6 \cdot 4 \cdot 2} \cdot \frac{\pi}{2} = \frac{15}{48} \cdot \frac{\pi}{2} = \boxed{\frac{5\pi}{32}}$

**P101-P110** — Integration Techniques:

| # | Integral | Method & Answer |
|---|---|---|
| P101 | $\int \frac{dx}{x^2 + 4}$ | $\frac{1}{2}\tan^{-1}(x/2) + C$ |
| P102 | $\int \frac{dx}{x^2 - 4}$ | $\frac{1}{4}\ln\left|\frac{x-2}{x+2}\right| + C$ (partial fractions) |
| P103 | $\int \frac{x}{x^2+1} dx$ | $\frac{1}{2}\ln(x^2+1) + C$ (substitution $u=x^2+1$) |
| P104 | $\int x^2 e^x dx$ | $e^x(x^2 - 2x + 2) + C$ (by parts twice) |
| P105 | $\int \ln x \, dx$ | $x \ln x - x + C$ |
| P106 | $\int \frac{dx}{\sqrt{x^2-1}}$ | $\ln|x + \sqrt{x^2-1}| + C$ or $\cosh^{-1}x + C$ |
| P107 | $\int \sin^3 x \, dx$ | $-\cos x + \frac{\cos^3 x}{3} + C$ |
| P108 | $\int_0^{\infty} \frac{dx}{1+x^2}$ | $\frac{\pi}{2}$ |
| P109 | $\int_{-\infty}^{\infty} e^{-|x|} dx$ | $2$ (by symmetry: $2\int_0^\infty e^{-x} dx = 2$) |
| P110 | $\int_0^1 \frac{\ln x}{1-x} dx$ | $-\frac{\pi^2}{6}$ (famous result) |

**P111-P130** — Multivariable & Optimization:

**P111.** (2-mark) $f(x,y) = x^2 + xy + y^2 - 3x$. Find critical points.
→ $f_x = 2x + y - 3 = 0$, $f_y = x + 2y = 0$ → from 2nd: $x = -2y$
→ $-4y + y - 3 = 0$ → $y = -1$, $x = 2$
→ Critical point: $(2, -1)$

**P112.** (2-mark) Classify the critical point in P111.
→ $f_{xx} = 2$, $f_{yy} = 2$, $f_{xy} = 1$
→ $D = f_{xx} f_{yy} - (f_{xy})^2 = 4 - 1 = 3 > 0$ and $f_{xx} = 2 > 0$
→ **Local minimum** at $(2, -1)$

**P113.** (2-mark) $f(x,y) = x^3 + y^3 - 3xy$. Find and classify critical points.
→ $f_x = 3x^2 - 3y = 0$ → $y = x^2$
→ $f_y = 3y^2 - 3x = 0$ → $y^2 = x$ → $(x^2)^2 = x$ → $x^4 = x$ → $x(x^3 - 1) = 0$
→ $x = 0$ (then $y = 0$) or $x = 1$ (then $y = 1$)
→ At $(0,0)$: $D = 0 - 9 = -9 < 0$ → **Saddle point**
→ At $(1,1)$: $D = 36 - 9 = 27 > 0$, $f_{xx} = 6 > 0$ → **Local minimum**

**P114.** (2-mark) Gradient of $f(x,y,z) = x^2y + yz^2$ at $(1, 2, 3)$.
→ $\nabla f = (2xy, x^2 + z^2, 2yz) = (4, 10, 12)$

**P115.** (2-mark) Directional derivative of $f$ at $(1,2,3)$ in direction $\hat{u} = (1/3, 2/3, 2/3)$?
→ $D_u f = \nabla f \cdot \hat{u} = 4(1/3) + 10(2/3) + 12(2/3) = 4/3 + 20/3 + 24/3 = 48/3 = \boxed{16}$

**P116.** (2-mark) Maximum rate of change of $f$ at $(1,2,3)$?
→ $|\nabla f| = \sqrt{16 + 100 + 144} = \sqrt{260} = 2\sqrt{65}$

**P117.** (2-mark) Lagrange multiplier: Minimize $f = x^2 + y^2$ subject to $x + y = 4$.
→ $\nabla f = \lambda \nabla g$: $(2x, 2y) = \lambda(1, 1)$ → $x = y = \lambda/2$
→ Constraint: $2x = 4$ → $x = y = 2$
→ Minimum $= 4 + 4 = \boxed{8}$

**P118.** (2-mark) Lagrange: Maximize $f = xy$ subject to $x + y = 10$.
→ $y = \lambda$, $x = \lambda$ → $x = y$ → $2x = 10$ → $x = y = 5$
→ Maximum $= 25$. (AM-GM confirms: $xy \leq (x+y)^2/4 = 25$)

**P119.** (2-mark) $f(x, y) = 3x + 4y$. Maximize subject to $x^2 + y^2 = 1$.
→ $\nabla f = \lambda \nabla g$: $(3, 4) = \lambda(2x, 2y)$ → $x = 3/(2\lambda)$, $y = 4/(2\lambda)$
→ $x^2 + y^2 = \frac{9 + 16}{4\lambda^2} = 1$ → $\lambda^2 = 25/4$ → $\lambda = 5/2$
→ $x = 3/5, y = 4/5$. Max = $3(3/5) + 4(4/5) = 9/5 + 16/5 = \boxed{5}$

**P120.** (2-mark) KKT: Minimize $f = x^2 + y^2$ subject to $x + y \geq 4$.
→ Rewrite constraint: $g(x,y) = 4 - x - y \leq 0$
→ KKT: $\nabla f = \mu \nabla g$: $(2x, 2y) = \mu(-1, -1)$ → $x = y = -\mu/2$
→ $\mu \geq 0$ (complementary slackness), $\mu(4 - x - y) = 0$
→ If $\mu > 0$: active constraint $x + y = 4$ → $-\mu = 4$ → $\mu = -4 < 0$ contradiction!
Wait: $\nabla f + \mu \nabla g = 0$ (KKT for min): $(2x, 2y) + \mu(-1, -1) = (0,0)$
→ $2x = \mu$, $2y = \mu$ → $x = y = \mu/2$
→ Active: $x + y = 4$ → $\mu = 4$ → $x = y = 2$ → Min $= 8$ ✅

**P121-P130** — Double Integrals & ODEs:

**P121.** (2-mark) $\int_0^1 \int_0^x xy \, dy \, dx$ = ?
→ Inner: $\int_0^x xy \, dy = x \cdot \frac{y^2}{2}\Big|_0^x = \frac{x^3}{2}$
→ Outer: $\int_0^1 \frac{x^3}{2} dx = \frac{1}{8}$. $= \boxed{\frac{1}{8}}$

**P122.** (2-mark) Switch order: $\int_0^1 \int_x^1 f(x,y) \, dy \, dx$.
→ Region: $0 \leq x \leq y, 0 \leq y \leq 1$ (a triangle)
→ Switched: $\int_0^1 \int_0^y f(x,y) \, dx \, dy$

**P123.** (2-mark) $\int_0^1 \int_0^1 (x + y) \, dx \, dy$ = ?
→ $= \int_0^1 \left[\frac{x^2}{2} + xy\right]_0^1 dy = \int_0^1 (1/2 + y) dy = 1/2 + 1/2 = \boxed{1}$

**P124.** (2-mark) Polar: $\iint_{x^2+y^2 \leq 1} (x^2 + y^2) \, dA$
→ $= \int_0^{2\pi} \int_0^1 r^2 \cdot r \, dr \, d\theta = 2\pi \cdot \frac{r^4}{4}\Big|_0^1 = \boxed{\frac{\pi}{2}}$

**P125.** (2-mark) Solve: $\frac{dy}{dx} = \frac{y}{x}$.
→ Separable: $\frac{dy}{y} = \frac{dx}{x}$ → $\ln|y| = \ln|x| + C$ → $y = Cx$

**P126.** (2-mark) Solve: $\frac{dy}{dx} + 2y = e^{-x}$.
→ IF: $e^{\int 2 dx} = e^{2x}$. Multiply: $\frac{d}{dx}(ye^{2x}) = e^x$
→ $ye^{2x} = e^x + C$ → $y = e^{-x} + Ce^{-2x}$

**P127.** (2-mark) Solve: $\frac{dy}{dx} = \frac{x+y}{x-y}$.
→ Homogeneous. Let $y = vx$: $v + x\frac{dv}{dx} = \frac{1+v}{1-v}$
→ $x\frac{dv}{dx} = \frac{1+v}{1-v} - v = \frac{1+v^2}{1-v}$
→ $\frac{1-v}{1+v^2} dv = \frac{dx}{x}$
→ $\tan^{-1}v - \frac{1}{2}\ln(1+v^2) = \ln|x| + C$

**P128.** (2-mark) Exact ODE: $(2xy + 3) dx + (x^2 + 4y) dy = 0$. Verify and solve.
→ $M = 2xy + 3$, $N = x^2 + 4y$
→ $M_y = 2x = N_x$ → **Exact!** ✅
→ $F = \int M \, dx = x^2y + 3x + g(y)$
→ $F_y = x^2 + g'(y) = x^2 + 4y$ → $g'(y) = 4y$ → $g(y) = 2y^2$
→ Solution: $x^2y + 3x + 2y^2 = C$

**P129.** (2-mark) Bernoulli: $\frac{dy}{dx} + y = y^2$.
→ Divide by $y^2$: $y^{-2}\frac{dy}{dx} + y^{-1} = 1$
→ Let $v = y^{-1}$ → $\frac{dv}{dx} = -y^{-2}\frac{dy}{dx}$ → $-\frac{dv}{dx} + v = 1$ → $\frac{dv}{dx} - v = -1$
→ IF = $e^{-x}$: $ve^{-x} = \int -e^{-x} dx = e^{-x} + C$
→ $v = 1 + Ce^x$ → $y = \frac{1}{1 + Ce^x}$

**P130.** (1-mark) Is the ODE $(e^y + 1)\cos x \, dx + e^y \sin x \, dy = 0$ exact?
→ $M_y = e^y \cos x$, $N_x = e^y \cos x$ → **YES, exact!** ✅

---

## 📝 Practice Set 3: GATE Simulation (P131 – P200)

**P131.** (2-mark NAT) $\lim_{x \to 0} \frac{e^{2x} - 1 - 2x}{x^2}$ = ?
→ Taylor: $e^{2x} = 1 + 2x + 2x^2 + \cdots$ → $\frac{2x^2}{x^2} = \boxed{2}$

**P132.** (2-mark) Find $c$ in LMVT for $f(x) = \ln x$ on $[1, e]$.
→ $\frac{f(e) - f(1)}{e - 1} = \frac{1 - 0}{e - 1} = \frac{1}{e-1}$
→ $f'(c) = \frac{1}{c} = \frac{1}{e-1}$ → $c = e - 1 \approx 1.718$
→ $c \in (1, e)$ ✅

**P133.** (2-mark) $\int_0^{\pi/4} \tan^2 x \, dx$ = ?
→ $\tan^2 x = \sec^2 x - 1$ → $\int_0^{\pi/4} (\sec^2 x - 1) dx = [\tan x - x]_0^{\pi/4} = 1 - \pi/4$
→ $= \boxed{1 - \frac{\pi}{4}}$

**P134.** (2-mark) Classify critical points of $f(x) = x^4 - 4x^3 + 4x^2$.
→ $f'(x) = 4x^3 - 12x^2 + 8x = 4x(x^2 - 3x + 2) = 4x(x-1)(x-2)$
→ Critical points: $x = 0, 1, 2$
→ $f''(x) = 12x^2 - 24x + 8$
→ $f''(0) = 8 > 0$ → local min, $f(0) = 0$
→ $f''(1) = 12 - 24 + 8 = -4 < 0$ → local max, $f(1) = 1$
→ $f''(2) = 48 - 48 + 8 = 8 > 0$ → local min, $f(2) = 0$

**P135.** (2-mark) $\int_{-1}^{1} |x| \, dx$ = ?
→ $= 2 \int_0^1 x \, dx = 2 \cdot \frac{1}{2} = \boxed{1}$ (using even function symmetry)

**P136.** (2-mark) Is $\int_1^{\infty} \frac{dx}{x^{3/2}}$ convergent?
→ $= \left[\frac{x^{-1/2}}{-1/2}\right]_1^{\infty} = -2[0 - 1] = \boxed{2}$ (convergent, $p = 3/2 > 1$)

**P137.** (2-mark NAT) $f(x,y) = e^{x+2y}$. Value of $\frac{\partial^2 f}{\partial x \partial y}$ at $(0,0)$?
→ $f_x = e^{x+2y}$, $f_{xy} = 2e^{x+2y}$ → at $(0,0)$: $= \boxed{2}$

**P138.** (2-mark) $f(x,y) = x^3 + y^3 - 63(x + y) + 12xy$. Critical points?
→ $f_x = 3x^2 + 12y - 63 = 0$ → $x^2 + 4y = 21$ ... (1)
→ $f_y = 3y^2 + 12x - 63 = 0$ → $y^2 + 4x = 21$ ... (2)
→ (1)-(2): $x^2 - y^2 + 4(y-x) = 0$ → $(x-y)(x+y-4) = 0$
→ Case 1: $x = y$ → $x^2 + 4x = 21$ → $x = 3$ or $x = -7$
→ Case 2: $x + y = 4$ → $x^2 + 4(4-x) = 21$ → $x^2 - 4x - 5 = 0$ → $x = 5, y = -1$ or $x = -1, y = 5$
→ Four critical points: $(3,3), (-7,-7), (5,-1), (-1,5)$

**P139.** (2-mark) $\nabla \cdot \vec{F}$ where $\vec{F} = (x^2y, xyz, z^2)$ at $(1,1,1)$?
→ $\text{div} \vec{F} = 2xy + xz + 2z = 2 + 1 + 2 = \boxed{5}$

**P140.** (2-mark) $\nabla \times \vec{F}$ where $\vec{F} = (yz, xz, xy)$?
→ $\text{curl} \vec{F} = \left(\frac{\partial(xy)}{\partial y} - \frac{\partial(xz)}{\partial z}, \frac{\partial(yz)}{\partial z} - \frac{\partial(xy)}{\partial x}, \frac{\partial(xz)}{\partial x} - \frac{\partial(yz)}{\partial y}\right)$
→ $= (x - x, y - y, z - z) = (0, 0, 0)$
→ **Curl = 0** → $\vec{F}$ is conservative (irrotational)!

**P141-P160** — Mixed GATE Problems:

**P141.** (1-mark) $\frac{d}{dx}\left[\int_0^{x^2} \sin(t^2) \, dt\right] = ?$
→ Leibniz: $\sin((x^2)^2) \cdot 2x = \boxed{2x \sin(x^4)}$

**P142.** (2-mark) $\int_0^1 \int_0^{1-x} e^{x+y} \, dy \, dx$ = ?
→ Inner: $\int_0^{1-x} e^{x+y} dy = e^x[e^y]_0^{1-x} = e^x(e^{1-x} - 1) = e - e^x$
→ Outer: $\int_0^1 (e - e^x) dx = [ex - e^x]_0^1 = (e - e) - (0 - 1) = \boxed{1}$

**P143.** (2-mark) For $f(x,y) = x^2 + 2y^2 - 2xy - 4y$. Is $f$ convex?
→ Hessian: $H = \begin{pmatrix} 2 & -2 \\ -2 & 4 \end{pmatrix}$
→ Leading minors: $2 > 0$, $\det(H) = 8 - 4 = 4 > 0$
→ **Positive definite → strictly convex!** ✅

**P144.** (2-mark) Global minimum of $f$ in P143?
→ $f_x = 2x - 2y = 0$ → $x = y$
→ $f_y = 4y - 2x - 4 = 0$ → $4y - 2y = 4$ → $y = 2, x = 2$
→ $f(2,2) = 4 + 8 - 8 - 8 = \boxed{-4}$

**P145.** (2-mark) Gradient descent for $f(x) = x^2 - 4x + 5$. Starting at $x_0 = 0$, learning rate $\alpha = 0.3$:
→ $f'(x) = 2x - 4$
→ $x_1 = x_0 - \alpha f'(x_0) = 0 - 0.3(-4) = 1.2$
→ $x_2 = 1.2 - 0.3(2.4 - 4) = 1.2 + 0.48 = 1.68$
→ $x_3 = 1.68 - 0.3(3.36 - 4) = 1.68 + 0.192 = 1.872$
→ Converging to $x^* = 2$ (the minimizer). Rate of convergence: $|1 - 2\alpha| = |1 - 0.6| = 0.4$ per step.

**P146.** (2-mark) Newton's method for $f(x) = x^2 - 4x + 5$:
→ $x_{n+1} = x_n - \frac{f'(x_n)}{f''(x_n)} = x_n - \frac{2x_n - 4}{2} = x_n - x_n + 2 = 2$
→ Converges in **1 step** for quadratic! (Newton's is exact for quadratics)

**P147-P160** — True/False GATE:

| # | Statement | T/F |
|---|---|---|
| P147 | $\int_a^a f(x) dx = 0$ always | TRUE |
| P148 | $\int_a^b f(x) dx = -\int_b^a f(x) dx$ | TRUE |
| P149 | If $f$ is odd, $\int_{-a}^a f(x) dx = 0$ | TRUE |
| P150 | If $f$ is even, $\int_{-a}^a f(x) dx = 2\int_0^a f(x) dx$ | TRUE |
| P151 | $\Gamma(n+1) = n!$ for positive integer $n$ | TRUE |
| P152 | $B(m,n) = B(n,m)$ | TRUE (symmetric) |
| P153 | Every continuous function on $[a,b]$ is Riemann integrable | TRUE |
| P154 | Every Riemann integrable function is continuous | FALSE |
| P155 | $\int_0^1 \frac{1}{x} dx$ converges | FALSE ($p$-test: $p = 1$, diverges) |
| P156 | $\int_0^1 \frac{1}{\sqrt{x}} dx$ converges | TRUE ($p = 1/2 < 1$, converges to 2) |
| P157 | Gradient points in direction of steepest ascent | TRUE |
| P158 | Saddle point: $D = f_{xx}f_{yy} - (f_{xy})^2 < 0$ | TRUE |
| P159 | Every local minimum of a convex function is global | TRUE |
| P160 | KKT conditions are sufficient for convex problems | TRUE |

**P161-P200** — Numerical Practice:

**P161.** (NAT) $\lim_{x \to 2} \frac{x^3 - 8}{x^2 - 4}$ = ?
→ Factor: $\frac{(x-2)(x^2+2x+4)}{(x-2)(x+2)} = \frac{12}{4} = \boxed{3}$

**P162.** (NAT) $\int_0^2 |x-1| dx$ = ?
→ $\int_0^1 (1-x) dx + \int_1^2 (x-1) dx = 1/2 + 1/2 = \boxed{1}$

**P163.** (NAT) Area between $y = x^2$ and $y = x$ from 0 to 1?
→ $\int_0^1 (x - x^2) dx = 1/2 - 1/3 = \boxed{1/6}$

**P164.** (NAT) $\frac{\partial}{\partial x}(x^2y^3 + \sin(xy))$ at $(1, \pi)$?
→ $= 2xy^3 + y\cos(xy) = 2\pi^3 + \pi\cos(\pi) = 2\pi^3 - \pi$

**P165.** (NAT) Jacobian $\frac{\partial(u,v)}{\partial(x,y)}$ where $u = x + y, v = x - y$?
→ $J = \begin{vmatrix} 1 & 1 \\ 1 & -1 \end{vmatrix} = -2$ → $|J| = \boxed{2}$

**P166.** (NAT) Chain rule: $z = f(x,y), x = r\cos\theta, y = r\sin\theta$. Express $\frac{\partial z}{\partial r}$.
→ $\frac{\partial z}{\partial r} = \frac{\partial z}{\partial x}\cos\theta + \frac{\partial z}{\partial y}\sin\theta$

**P167-P180** — Optimization:

**P167.** Find max/min of $f(x) = x^3 - 6x^2 + 9x + 1$ on $[0, 4]$.
→ $f'(x) = 3x^2 - 12x + 9 = 3(x-1)(x-3) = 0$ → $x = 1, 3$
→ $f(0) = 1$, $f(1) = 5$, $f(3) = 1$, $f(4) = 5$
→ Max = 5 (at $x = 1$ and $x = 4$), Min = 1 (at $x = 0$ and $x = 3$)

**P168.** A rectangular box with surface area 216. Maximize volume.
→ $V = xyz$, $2(xy + yz + xz) = 216$ → $xy + yz + xz = 108$
→ By AM-GM or Lagrange: optimal is cube: $x = y = z = 6$ → $V = \boxed{216}$

**P169.** (2-mark) Hessian of $f(x,y) = x^4 + y^4 - 2x^2y^2$ at origin?
→ $f_x = 4x^3 - 4xy^2$, $f_y = 4y^3 - 4x^2y$
→ $f_{xx} = 12x^2 - 4y^2$, $f_{yy} = 12y^2 - 4x^2$, $f_{xy} = -8xy$
→ At $(0,0)$: $H = \begin{pmatrix} 0 & 0 \\ 0 & 0 \end{pmatrix}$ → **Inconclusive!** ($D = 0$)
→ Actually: $f(x,y) = (x^2 - y^2)^2 \geq 0$ with $f(0,0) = 0$ → **Global minimum!**

**P170.** (2-mark) Is $f(x) = e^x$ convex?
→ $f''(x) = e^x > 0$ for all $x$ → **YES, strictly convex** ✅

**P171-P180** — Quick calculus:

| # | Q | A |
|---|---|---|
| P171 | Is $f(x) = -\ln x$ convex for $x > 0$? | YES ($f'' = 1/x^2 > 0$) |
| P172 | Is $f(x) = x^3$ convex? | NO (not even on $\mathbb{R}$; $f'' = 6x$ changes sign) |
| P173 | Taylor: $\cos x$ up to $x^4$? | $1 - x^2/2 + x^4/24$ |
| P174 | Taylor: $\ln(1+x)$ up to $x^4$? | $x - x^2/2 + x^3/3 - x^4/4$ |
| P175 | Maclaurin of $\tan^{-1} x$? | $x - x^3/3 + x^5/5 - \cdots$ |
| P176 | Radius of convergence of $\sum x^n/n!$? | $\infty$ (entire function $e^x$) |
| P177 | Radius of convergence of $\sum x^n$? | $1$ (geometric series) |
| P178 | Radius of convergence of $\sum n! x^n$? | $0$ (diverges for all $x \neq 0$) |
| P179 | $\Gamma(3/2) = ?$ | $\frac{1}{2}\Gamma(1/2) = \frac{\sqrt{\pi}}{2}$ |
| P180 | $\int_0^1 x^2(1-x)^3 dx = ?$ | $B(3,4) = \frac{2! \cdot 3!}{6!} = \frac{12}{720} = 1/60$ |

**P181-P200** — GATE PYQ Style:

**P181.** (2-mark GATE 2019 style) $\lim_{x \to 0} \frac{(1+x)^{1/x} - e}{x}$
→ Already solved: $\boxed{-e/2}$

**P182.** (2-mark) Find $a, b$ such that $f(x) = \begin{cases} x^2 & x \leq 1 \\ ax + b & x > 1 \end{cases}$ is differentiable at $x = 1$.
→ Continuity: $1 = a + b$
→ Differentiability: $f'(1^-) = 2 = a = f'(1^+)$
→ $a = 2, b = -1$

**P183.** (2-mark) Evaluate $\int_0^{\infty} x^2 e^{-3x} dx$.
→ Sub $u = 3x$: $= \frac{1}{27} \int_0^{\infty} u^2 e^{-u} du = \frac{\Gamma(3)}{27} = \frac{2}{27}$

**P184.** (2-mark) $\int_0^{\pi} x \sin x \, dx$ = ?
→ By parts: $[-x\cos x]_0^{\pi} + \int_0^{\pi} \cos x \, dx = \pi + [\sin x]_0^{\pi} = \pi + 0 = \boxed{\pi}$

**P185.** (2-mark NAT) Number of real roots of $e^x = 3x$?
→ Let $g(x) = e^x - 3x$. $g'(x) = e^x - 3 = 0$ → $x = \ln 3$
→ $g(\ln 3) = 3 - 3\ln 3 = 3(1 - \ln 3) < 0$ (since $\ln 3 > 1$)
→ $g(0) = 1 > 0$, $g(\ln 3) < 0$, $g(\infty) = \infty$
→ **2 real roots** (one before $\ln 3$, one after)

**P186.** (2-mark) Convergence of $\int_0^{\infty} \frac{\sin x}{x} dx$?
→ **Conditionally convergent** (not absolutely convergent). Value = $\pi/2$ (Dirichlet integral)

**P187-P200** — Express Round:

| # | Q | A |
|---|---|---|
| P187 | $\frac{d}{dx}[\cosh^{-1} x]$ | $\frac{1}{\sqrt{x^2-1}}$ |
| P188 | Euler's theorem: $f$ homogeneous deg $n$ → $xf_x + yf_y = ?$ | $nf$ |
| P189 | Jacobian of polar ($x=r\cos\theta, y=r\sin\theta$)? | $r$ |
| P190 | If $\nabla^2 f = 0$, $f$ is called? | Harmonic function |
| P191 | Laplacian in 2D: $\nabla^2 f = ?$ | $f_{xx} + f_{yy}$ |
| P192 | Is $f(x,y) = x^2 + y^2$ harmonic? | NO ($\nabla^2 f = 4 \neq 0$) |
| P193 | Is $f(x,y) = \ln(x^2+y^2)$ harmonic? | YES ($\nabla^2 f = 0$ for $(x,y) \neq (0,0)$) |
| P194 | $\int_0^{2\pi} \sin^2 \theta \cos^2 \theta \, d\theta$? | $\pi/4$ |
| P195 | $\int_0^1 (-\ln x)^n dx$? | $n!$ (sub $t = -\ln x$) |
| P196 | Integration by parts formula? | $\int u \, dv = uv - \int v \, du$ |
| P197 | L'Hôpital applies to which forms? | $0/0$ and $\infty/\infty$ only! |
| P198 | Number of inflection points of $f(x) = x^4$? | 0 ($f''(0) = 0$ but no sign change) |
| P199 | Concave up means $f'' > 0$ or $f'' < 0$? | $f'' > 0$ (concave UP) |
| P200 | Point of inflection: $f''$ changes? | Sign (from + to - or - to +) |

---

## 🧩 50 GATE Traps Expanded — Calculus

| # | Trap | Correct Understanding |
|---|---|---|
| 1 | Using L'Hôpital on non-indeterminate form | L'Hôpital only for $0/0$ or $\infty/\infty$! |
| 2 | Forgetting to check $f(a) = f(b)$ for Rolle's | Rolle's REQUIRES equal endpoint values |
| 3 | Assuming continuous ⇒ differentiable | FALSE ($|x|$ at 0) |
| 4 | Forgetting absolute value in $\int |f(x)| dx$ | Split at zeros and flip sign for negative parts |
| 5 | $\Gamma(0) = 0$ | WRONG! $\Gamma(0)$ is **undefined** (diverges) |
| 6 | $\frac{d}{dx} \int_0^{g(x)} f(t) dt = f(g(x))$ only | WRONG! Must multiply by $g'(x)$! |
| 7 | Confusing $f_{xy}$ and $f_{yx}$ | Equal for $C^2$ functions (Clairaut's theorem) |
| 8 | Forgetting $|J|$ in change of variables for double integrals | $dA = |J| \, du \, dv$ |
| 9 | Using AM-GM on negative numbers | AM-GM only for non-negative! |
| 10 | Saddle point: $D < 0$ but claiming min/max | $D < 0$ means DEFINITELY saddle! |
| 11 | $D = 0$ means saddle | WRONG! $D = 0$ is inconclusive — could be anything |
| 12 | Gradient = 0 implies global optimum | Only for convex functions! Otherwise just local or saddle |
| 13 | KKT: forgetting $\mu \geq 0$ | Complementary slackness requires $\mu \geq 0$ |
| 14 | Divergence of gradient ≠ Laplacian... wait | $\nabla \cdot (\nabla f) = \nabla^2 f$ — they ARE the same! |
| 15 | Power series: forgetting to check endpoints | Radius of convergence doesn't tell about endpoints |
| 16 | $\int_{-1}^{1} \frac{1}{x} dx = 0$ by symmetry | WRONG! Diverges (improper integral)! |
| 17 | Taylor error: using wrong center point | Match the expansion center to the problem |
| 18 | $\sin^{-1}(\sin x) = x$ always | Only for $x \in [-\pi/2, \pi/2]$! |
| 19 | Mixing up homogeneous ODE with homogeneous function | Different concepts! |
| 20 | Forgetting constant of integration in ODE | General solution must have constant $C$ |
| 21 | $e^{x+y} = e^x + e^y$ | WRONG! $e^{x+y} = e^x \cdot e^y$ |
| 22 | $\ln(x+y) = \ln x + \ln y$ | WRONG! Only $\ln(xy) = \ln x + \ln y$ |
| 23 | Forgetting chain rule for $\frac{d}{dx}[\sin(x^2)]$ | $= 2x\cos(x^2)$, NOT $\cos(x^2)$! |
| 24 | Confusing $\sin^2 x$ with $\sin(x^2)$ | Completely different functions! |
| 25 | Integration: $\int \frac{dx}{x} = \ln x + C$ | Should be $\ln|x| + C$ (absolute value!) |
| 26 | Treating $\infty - \infty$ as 0 | Indeterminate! Must analyze carefully |
| 27 | $0 \times \infty$ = 0 | Indeterminate! Convert to $0/0$ or $\infty/\infty$ |
| 28 | Forgetting $+C$ in indefinite integrals | Always include! |
| 29 | Partial fraction: forgetting repeated roots | $(x-a)^2$ needs $\frac{A}{x-a} + \frac{B}{(x-a)^2}$ |
| 30 | Convergence test: ratio test inconclusive at 1 | Try other tests when ratio = 1 |
| 31 | Writing $\nabla f$ as a scalar | Gradient is a VECTOR! |
| 32 | Directional derivative without unit vector | Must normalize the direction! |
| 33 | Max directional derivative = gradient | Only the VALUE is $|\nabla f|$; DIRECTION is $\nabla f / |\nabla f|$ |
| 34 | Integrating factor for non-linear ODE | IF method works only for LINEAR ODEs! |
| 35 | Bernoulli equation is linear | WRONG! It's non-linear but REDUCIBLE to linear |
| 36 | Assuming all $C^\infty$ functions are analytic | FALSE ($e^{-1/x^2}$ at 0 is $C^\infty$ but not analytic) |
| 37 | Fundamental Theorem of Calculus: $f$ must be continuous | $\frac{d}{dx}\int_a^x f(t) dt = f(x)$ needs $f$ continuous |
| 38 | Double integral limits wrong in polar | $dA = r \, dr \, d\theta$ (don't forget the $r$!) |
| 39 | MVT gives exact value of $c$ always | MVT guarantees existence, not uniqueness! |
| 40 | Confusing open and closed interval for min/max | Closed interval: check endpoints too! Open: endpoints excluded |
| 41 | $\int_0^{\pi} \sin x \, dx = 0$ by "symmetry" | WRONG! $= 2$ (not odd function on $[0, \pi]$) |
| 42 | Forgetting to check convergence before manipulating series | Divergent series cannot be rearranged! |
| 43 | $ \frac{d}{dx}(f \cdot g) = f' \cdot g'$ | WRONG! Product rule: $f'g + fg'$ |
| 44 | Chain rule: $\frac{d}{dx}f(g(x)) = f'(g(x))$ | Missing $g'(x)$! Correct: $f'(g(x)) \cdot g'(x)$ |
| 45 | $\int e^{x^2} dx$ has a closed form | NO! No elementary antiderivative (relates to error function) |
| 46 | Confusing $\frac{\partial f}{\partial x}$ with $\frac{df}{dx}$ | Partial holds $y$ constant; total includes chain rule terms |
| 47 | Gradient descent always converges | Only with small enough learning rate & convex $f$ (generally) |
| 48 | Newton's method always converges | Can diverge if starting point is bad or $f''$ = 0 |
| 49 | Laplacian of vector field | $\nabla^2 \vec{F}$ is defined component-wise (often confused with scalar Laplacian) |
| 50 | Confusing convex function with convex set | Function vs geometric set — different concepts! |

---

## ⚡ Emergency Calculus Formulas — Last Minute Card

| Formula | Expression |
|---|---|
| $\lim \frac{\sin x}{x}$ | 1 |
| $\lim \frac{\tan x}{x}$ | 1 |
| $\lim \frac{e^x - 1}{x}$ | 1 |
| $\lim \frac{\ln(1+x)}{x}$ | 1 |
| $\lim \frac{a^x - 1}{x}$ | $\ln a$ |
| $\lim (1 + x)^{1/x}$ | $e$ |
| $\lim (1 + a/n)^{bn}$ | $e^{ab}$ |
| Gaussian: $\int_{-\infty}^{\infty} e^{-x^2} dx$ | $\sqrt{\pi}$ |
| $\Gamma(n) = (n-1)!$ | for positive integer $n$ |
| $\Gamma(1/2) = \sqrt{\pi}$ | — |
| $B(m,n) = \frac{\Gamma(m)\Gamma(n)}{\Gamma(m+n)}$ | — |
| Wallis: $\int_0^{\pi/2} \sin^n x \, dx$ | $\frac{(n-1)!!}{n!!} \cdot$ ($\pi/2$ if $n$ even, 1 if odd) |
| Taylor: $e^x$ | $\sum \frac{x^n}{n!}$ |
| Taylor: $\sin x$ | $\sum (-1)^n \frac{x^{2n+1}}{(2n+1)!}$ |
| Taylor: $\cos x$ | $\sum (-1)^n \frac{x^{2n}}{(2n)!}$ |
| Taylor: $\ln(1+x)$ | $\sum (-1)^{n+1} \frac{x^n}{n}$, $|x| \leq 1$ |
| Taylor: $(1+x)^{\alpha}$ | $\sum \binom{\alpha}{n} x^n$, $|x| < 1$ |
| D-test: $D = f_{xx}f_{yy} - (f_{xy})^2$ | $D>0, f_{xx}>0$: min; $D>0, f_{xx}<0$: max; $D<0$: saddle |
| Gradient descent | $x_{k+1} = x_k - \alpha \nabla f(x_k)$ |
| Newton's method | $x_{k+1} = x_k - [H(x_k)]^{-1}\nabla f(x_k)$ |
| Lagrange | $\nabla f = \lambda \nabla g$ at constrained optimum |
| KKT | $\nabla f + \sum \mu_i \nabla g_i = 0$, $\mu_i \geq 0$, $\mu_i g_i = 0$ |
| Directional derivative | $D_{\hat{u}}f = \nabla f \cdot \hat{u}$ |
| Euler (homogeneous deg $n$) | $xf_x + yf_y = nf$ |
| Integrating factor (linear ODE) | $e^{\int P(x) dx}$ for $y' + P(x)y = Q(x)$ |

---

## 🎓 Calculus Mastery Checklist

- Can I evaluate limits using Taylor series expansion?
- Can I identify and resolve all indeterminate forms ($0/0, \infty/\infty, 1^\infty, 0^0, \infty^0, 0 \cdot \infty, \infty - \infty$)?
- Can I check continuity and differentiability at a point?
- Can I apply Rolle's, LMVT, and Cauchy MVT?
- Can I find and classify critical points (1D and 2D)?
- Can I compute definite integrals using all techniques (parts, partial fractions, substitution)?
- Can I evaluate improper integrals and test convergence?
- Can I use Gamma and Beta functions?
- Can I compute FIRST and SECOND partial derivatives?
- Can I find gradient, divergence, curl?
- Can I compute directional derivatives?
- Can I apply Lagrange multipliers for constrained optimization?
- Can I apply KKT conditions for inequality constraints?
- Can I perform gradient descent iterations?
- Can I evaluate double integrals and switch order of integration?
- Can I solve first-order ODEs (separable, linear, exact, Bernoulli)?

If YES to all → **GATE Calculus is YOURS!** 🏆

---

## 📝 Practice Set 4: Advanced Integration & Series (P201 – P280)

**P201.** (2-mark) $\int_0^{\pi} x^2 \cos x \, dx$ = ?
→ By parts twice: $[x^2 \sin x]_0^{\pi} - \int_0^{\pi} 2x \sin x \, dx$
→ First term = 0. $\int 2x \sin x \, dx = [-2x\cos x]_0^{\pi} + \int 2\cos x \, dx = 2\pi + [2\sin x]_0^{\pi} = 2\pi$
→ Final: $0 - 2\pi = \boxed{-2\pi}$

**P202.** (2-mark) Test convergence: $\sum_{n=1}^{\infty} \frac{n}{2^n}$
→ Ratio test: $\frac{a_{n+1}}{a_n} = \frac{n+1}{2n} \to \frac{1}{2} < 1$ → **Converges** ✅
→ Value: $\sum \frac{n}{2^n} = 2$ (by differentiation of geometric series)

**P203.** (2-mark) Find the sum: $\sum_{n=0}^{\infty} \frac{x^n}{n!}$ at $x = 1$.
→ $= e^1 = \boxed{e}$

**P204.** (2-mark) Radius of convergence of $\sum_{n=1}^{\infty} \frac{n^2 x^n}{3^n}$?
→ Ratio: $\frac{(n+1)^2}{n^2} \cdot \frac{|x|}{3} \to \frac{|x|}{3} < 1$ → $|x| < 3$ → $R = \boxed{3}$

**P205.** (2-mark) $\int_0^1 \frac{x^4 - 1}{\ln x} dx$ = ?
→ Use Feynman's trick: $I(a) = \int_0^1 \frac{x^a - 1}{\ln x} dx$
→ $I'(a) = \int_0^1 x^a \, dx = \frac{1}{a+1}$
→ $I(a) = \ln(a+1) + C$. $I(0) = 0$ → $C = 0$
→ $I(4) = \ln 5 = \boxed{\ln 5}$

**P206.** (2-mark) $\int_0^1 x^m (1-x)^n dx$ in terms of factorials (for $m, n$ positive integers)?
→ $B(m+1, n+1) = \frac{m! \cdot n!}{(m+n+1)!}$

**P207.** (2-mark) $\int_0^{\pi/2} \sin^5 x \cos^3 x \, dx$ = ?
→ $= \frac{1}{2}B(3, 2) = \frac{1}{2} \cdot \frac{\Gamma(3)\Gamma(2)}{\Gamma(5)} = \frac{1}{2} \cdot \frac{2 \cdot 1}{24} = \frac{1}{24}$
Wait, use formula: $\int_0^{\pi/2} \sin^p \cos^q = \frac{1}{2}B\left(\frac{p+1}{2}, \frac{q+1}{2}\right)$
→ $= \frac{1}{2}B(3, 2) = \frac{1}{2} \cdot \frac{2! \cdot 1!}{4!} = \frac{1}{2} \cdot \frac{2}{24} = \boxed{\frac{1}{24}}$

**P208.** (2-mark) Does $\int_0^{\infty} \frac{\cos x}{1+x^2} dx$ converge?
→ $\left|\frac{\cos x}{1+x^2}\right| \leq \frac{1}{1+x^2}$ and $\int_0^{\infty} \frac{dx}{1+x^2} = \pi/2$ converges.
→ By comparison: **absolutely convergent** ✅

**P209.** (2-mark) $\int_0^1 \sqrt{-\ln x} \, dx$ = ?
→ Sub $t = -\ln x$, $x = e^{-t}$, $dx = -e^{-t} dt$. When $x: 0 \to 1$, $t: \infty \to 0$.
→ $= \int_0^{\infty} \sqrt{t} \, e^{-t} dt = \Gamma(3/2) = \frac{\sqrt{\pi}}{2} = \boxed{\frac{\sqrt{\pi}}{2}}$

**P210.** (2-mark) Evaluate $\sum_{n=1}^{\infty} \frac{1}{n(n+1)}$
→ Partial fractions: $\frac{1}{n(n+1)} = \frac{1}{n} - \frac{1}{n+1}$ (telescoping)
→ $= 1 - \frac{1}{2} + \frac{1}{2} - \frac{1}{3} + \cdots = \boxed{1}$

**P211-P230** — Series & Convergence Tests:

| # | Series | Test & Result |
|---|---|---|
| P211 | $\sum \frac{1}{n^2}$ | $p$-series, $p = 2 > 1$ → **Converges** (to $\pi^2/6$) |
| P212 | $\sum \frac{1}{n}$ | Harmonic, $p = 1$ → **Diverges** |
| P213 | $\sum \frac{(-1)^n}{n}$ | Alternating, $1/n \to 0$ → **Conditionally converges** (to $\ln 2$) |
| P214 | $\sum \frac{n!}{n^n}$ | Ratio: $\frac{n^n}{(n+1)^n} \to 1/e < 1$ → **Converges** |
| P215 | $\sum \frac{1}{n \ln n}$ ($n \geq 2$) | Integral test: $\int \frac{dx}{x \ln x} = \ln(\ln x) \to \infty$ → **Diverges** |
| P216 | $\sum \frac{1}{n(\ln n)^2}$ | Integral test: converges (integral $= 1/\ln 2$) → **Converges** |
| P217 | $\sum \frac{2^n}{n!}$ | Ratio: $\frac{2}{n+1} \to 0$ → **Converges** (to $e^2 - 1$) |
| P218 | $\sum (-1)^n \frac{n}{n+1}$ | $a_n \to 1 \neq 0$ → **Diverges** (fails nth term test) |
| P219 | $\sum \frac{1}{3^n}$ | Geometric, $r = 1/3 < 1$ → **Converges** (to $1/2$) |
| P220 | $\sum \frac{n^2}{e^n}$ | Ratio: $\to 1/e < 1$ → **Converges** |

**P221-P240** — Multivariate Calculus:

**P221.** (2-mark) $f(x,y) = \sin(x^2 + y)$. Find $f_{xy}$.
→ $f_x = 2x\cos(x^2 + y)$
→ $f_{xy} = -2x\sin(x^2 + y)$

**P222.** (2-mark) Total derivative of $f(x,y)$ where $x = e^t, y = t^2$:
→ $\frac{df}{dt} = f_x \cdot e^t + f_y \cdot 2t$

**P223.** (2-mark) $f(x,y) = x^3 y^2$. Is $f$ homogeneous? Of what degree?
→ $f(tx, ty) = t^3 x^3 \cdot t^2 y^2 = t^5 f(x,y)$ → **Homogeneous of degree 5** ✅
→ Euler: $xf_x + yf_y = 5f = 5x^3y^2$. Check: $3x^2y^2 \cdot x + 2x^3y \cdot y = 3x^3y^2 + 2x^3y^2 = 5x^3y^2$ ✅

**P224.** (2-mark) Jacobian of transformation $u = x + y, v = x - y$:
→ $\frac{\partial(u,v)}{\partial(x,y)} = \begin{vmatrix} 1 & 1 \\ 1 & -1 \end{vmatrix} = -2$
→ Inverse Jacobian: $\frac{\partial(x,y)}{\partial(u,v)} = -\frac{1}{2}$

**P225.** (2-mark) Taylor expansion of $f(x,y) = e^{x+y}$ about $(0,0)$ up to 2nd order:
→ $f \approx 1 + (x+y) + \frac{(x+y)^2}{2} = 1 + x + y + \frac{x^2 + 2xy + y^2}{2}$

**P226.** (2-mark) Chain rule: $z = xy^2$, $x = \cos t$, $y = \sin t$. Find $dz/dt$.
→ $\frac{dz}{dt} = y^2(-\sin t) + 2xy(\cos t) = \sin^2 t(-\sin t) + 2\cos t \sin t(\cos t)$
→ $= -\sin^3 t + 2\cos^2 t \sin t = \sin t(2\cos^2 t - \sin^2 t)$

**P227.** (2-mark) Tangent plane to $z = x^2 + y^2$ at $(1, 1, 2)$:
→ $z - 2 = f_x(1,1)(x-1) + f_y(1,1)(y-1) = 2(x-1) + 2(y-1)$
→ $z = 2x + 2y - 2$

**P228.** (2-mark) Normal to surface $x^2 + y^2 + z^2 = 14$ at $(1, 2, 3)$?
→ $\nabla F = (2x, 2y, 2z) = (2, 4, 6)$
→ Normal direction: $(1, 2, 3)$ → Line: $\frac{x-1}{1} = \frac{y-2}{2} = \frac{z-3}{3}$

**P229.** (2-mark) $\nabla \times (\nabla f) = ?$ for any smooth $f$?
→ **Always 0!** (Curl of gradient is zero — identity)

**P230.** (2-mark) $\nabla \cdot (\nabla \times \vec{F}) = ?$ for any smooth $\vec{F}$?
→ **Always 0!** (Divergence of curl is zero — identity)

**P231-P250** — ODE Practice:

**P231.** (2-mark) Solve: $y' + y \tan x = \sec x$.
→ IF: $e^{\int \tan x \, dx} = e^{-\ln|\cos x|} = |\sec x| = \sec x$
→ $y \sec x = \int \sec^2 x \, dx = \tan x + C$
→ $y = \sin x + C \cos x$

**P232.** (2-mark) Solve: $y' = y^2$, $y(0) = 1$.
→ Separable: $\int y^{-2} dy = \int dx$ → $-1/y = x + C$
→ $y(0) = 1$: $C = -1$ → $y = \frac{1}{1-x}$ (defined for $x < 1$; blows up at $x = 1$!)

**P233.** (2-mark) Solve: $(2x + 3y) dx + (3x + 4y) dy = 0$.
→ $M = 2x + 3y$, $N = 3x + 4y$
→ $M_y = 3 = N_x$ → **Exact!**
→ $F = x^2 + 3xy + 2y^2 = C$

**P234.** (2-mark) Linear ODE: $y' - 2xy = x$.
→ IF: $e^{\int -2x \, dx} = e^{-x^2}$
→ $ye^{-x^2} = \int x e^{-x^2} dx = -\frac{1}{2}e^{-x^2} + C$
→ $y = -\frac{1}{2} + Ce^{x^2}$

**P235.** (2-mark) Solve: $x \frac{dy}{dx} + y = x^2$. ($y(1) = 0$)
→ Standard form: $y' + \frac{y}{x} = x$. IF = $e^{\int 1/x \, dx} = x$
→ $xy = \int x^2 dx = \frac{x^3}{3} + C$
→ $y(1) = 0$: $0 = 1/3 + C$ → $C = -1/3$
→ $y = \frac{x^2}{3} - \frac{1}{3x}$

**P236-P250** — Quick ODE:

| # | ODE | Solution |
|---|---|---|
| P236 | $y' = ky$ | $y = Ce^{kx}$ |
| P237 | $y' = -y/x$ | $y = C/x$ |
| P238 | $y' = x/y$ | $y^2 = x^2 + C$ |
| P239 | $y' + y = e^{-x}$ | $y = (x+C)e^{-x}$ (IF method) |
| P240 | $y' = y(1-y)$ (logistic) | $y = \frac{1}{1 + Ce^{-x}}$ |
| P241 | $y'' + y = 0$ | $y = A\cos x + B\sin x$ |
| P242 | $y'' - y = 0$ | $y = Ae^x + Be^{-x}$ |
| P243 | $y'' + 4y = 0$ | $y = A\cos 2x + B\sin 2x$ |
| P244 | $y'' + 2y' + y = 0$ | $y = (A + Bx)e^{-x}$ (repeated root) |
| P245 | $(D^2 + 3D + 2)y = 0$ | $y = Ae^{-x} + Be^{-2x}$ |
| P246 | Wronskian of $e^x, e^{2x}$? | $W = e^{3x}$ (never zero → linearly independent) |
| P247 | Particular solution: $y'' + y = \sin x$? | $y_p = -\frac{x}{2}\cos x$ (resonance!) |
| P248 | $y'' - 4y = e^{2x}$? | $y_p = \frac{x}{4}e^{2x}$ (repeated root in char.eq.) |
| P249 | Euler-Cauchy: $x^2y'' + xy' - y = 0$? | $y = Ax + B/x$ (try $y = x^m$: $m^2 - 1 = 0$) |
| P250 | $y' = \frac{y-x}{y+x}$? | Homogeneous: sub $v = y/x$ |

---

## 📝 Practice Set 5: Optimization GATE Style (P251 – P330)

**P251.** (2-mark) Minimize $f(x,y) = x^2 + y^2 + xy$ subject to $x + y = 1$.
→ Lagrange: $(2x+y, x+2y) = \lambda(1,1)$
→ $2x + y = x + 2y$ → $x = y$
→ $x + y = 1$ → $x = y = 1/2$
→ $f(1/2, 1/2) = 1/4 + 1/4 + 1/4 = \boxed{3/4}$

**P252.** (2-mark) Maximum of $f(x,y,z) = xyz$ subject to $x + y + z = 12$, $x,y,z > 0$.
→ By AM-GM: $xyz \leq \left(\frac{x+y+z}{3}\right)^3 = 64$ with equality iff $x = y = z = 4$
→ OR Lagrange: $(yz, xz, xy) = \lambda(1,1,1)$ → $yz = xz = xy$ → $x = y = z$
→ Max $= \boxed{64}$

**P253.** (2-mark) Gradient descent on $f(x,y) = (x-3)^2 + (y-5)^2$ from $(0,0)$, $\alpha = 0.5$:
→ $\nabla f = (2(x-3), 2(y-5))$. At $(0,0)$: $\nabla f = (-6, -10)$
→ $(x_1, y_1) = (0,0) - 0.5(-6,-10) = (3, 5)$ → **Exact minimum in 1 step!**
→ This works because $\alpha = 1/(2 \cdot \text{max eigenvalue of Hessian}) = 1/(2 \cdot 2) = 1/4$... wait, $H = 2I$, eigenvalue = 2. Optimal $\alpha = 1/2$ for this specific case.

**P254.** (2-mark) Is $f(x,y) = \max(x, y)$ convex?
→ YES! Maximum of convex functions (linear) is convex. ✅
→ (But NOT differentiable along $x = y$!)

**P255.** (2-mark) Is $f(x,y) = \min(x, y)$ convex?
→ NO in general. Actually: it IS concave! (Minimum of linear functions is concave.)
→ Wait: $\min(x,y)$ is concave (not convex). Check: $\min(1, 0) = 0$, $\min(0, 1) = 0$, $\min(0.5, 0.5) = 0.5$. $\lambda \cdot 0 + (1-\lambda) \cdot 0 = 0 \leq 0.5$ for $\lambda = 0.5$ → **concave** ✅

**P256.** (2-mark) KKT for: Maximize $f = x + y$ subject to $x^2 + y^2 \leq 4$.
→ Convert to minimize $-x - y$ subject to $x^2 + y^2 - 4 \leq 0$
→ KKT: $(-1, -1) + \mu(2x, 2y) = (0,0)$ → $x = y = \frac{1}{2\mu}$
→ $\mu > 0$: active constraint $x^2 + y^2 = 4$ → $\frac{1}{2\mu^2} = 4$ → $\mu = \frac{1}{2\sqrt{2}}$
→ $x = y = \sqrt{2}$ → Max $= 2\sqrt{2}$

**P257.** (2-mark) Newton's method to find root of $f(x) = x^2 - 2$ starting at $x_0 = 1$:
→ $x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)} = x_n - \frac{x_n^2 - 2}{2x_n} = \frac{x_n + 2/x_n}{2}$
→ $x_1 = (1 + 2)/2 = 1.5$, $x_2 = (1.5 + 4/3)/2 = 1.41\overline{6}$, $x_3 \approx 1.41421569$
→ Rapid convergence to $\sqrt{2} = 1.41421356...$ (quadratic convergence!)

**P258.** (2-mark) Hessian of $f(x,y) = x^4 + y^4$ at origin?
→ $H = \begin{pmatrix} 12x^2 & 0 \\ 0 & 12y^2 \end{pmatrix} \Big|_{(0,0)} = \begin{pmatrix} 0 & 0 \\ 0 & 0 \end{pmatrix}$
→ **Inconclusive by D-test!** But $f(x,y) = x^4 + y^4 \geq 0 = f(0,0)$ → global minimum.

**P259.** (2-mark) Is $f(x) = x \ln x$ convex for $x > 0$?
→ $f''(x) = 1/x > 0$ for $x > 0$ → **YES, strictly convex** ✅
→ This is why KL-divergence (which uses $x \ln x$) is convex!

**P260.** (2-mark) Jensen's inequality: if $f$ is convex, then $f(E[X]) \leq E[f(X)]$. Apply to $f(x) = x^2$:
→ $E[X]^2 \leq E[X^2]$ → this is just $\text{Var}(X) \geq 0$! ✅

**P261-P280** — Speed Drill:

| # | Q | A |
|---|---|---|
| P261 | $\frac{d}{dx}(x^x)$ at $x = 1$? | $1$ |
| P262 | $\int_0^1 x^n \ln x \, dx$? | $-\frac{1}{(n+1)^2}$ (by parts or diff.under integral) |
| P263 | Is $f(x) = |x|$ differentiable on $\mathbb{R}$? | No, not at $x = 0$ |
| P264 | Arc length of $y = x^{3/2}$ from 0 to 4? | $\int_0^4 \sqrt{1 + \frac{9x}{4}} dx$ |
| P265 | Fundamental theorem: $\frac{d}{dx}\int_a^x f(t)dt = ?$ | $f(x)$ |
| P266 | Weierstrass M-test: for uniform convergence | If $|f_n(x)| \leq M_n$ and $\sum M_n < \infty$ → uniform conv. |
| P267 | Cauchy's integral test applies to? | Positive, decreasing functions |
| P268 | Alternating series test (Leibniz): conditions? | $a_n > 0$, $a_n$ decreasing, $a_n \to 0$ |
| P269 | $\sum_{n=0}^{\infty} \frac{(-1)^n}{2n+1}$ = ? | $\pi/4$ (Leibniz formula) |
| P270 | Absolute convergence implies convergence? | YES (always) |
| P271 | Conditional convergence: converges but not absolutely? | Yes, like $\sum (-1)^n/n$ |
| P272 | $\int_{-\infty}^{\infty} \frac{dx}{x^2+a^2}$ = ? | $\pi/a$ |
| P273 | $\int_0^{\infty} e^{-ax} dx$ for $a > 0$? | $1/a$ |
| P274 | If $f$ is bounded and monotonic on $[a,b]$, is it integrable? | YES |
| P275 | Riemann sum with $n$ rectangles for $\int_0^1 x^2 dx$? | $\frac{(n)(n+1)(2n+1)}{6n^3} \to 1/3$ |
| P276 | $\frac{d}{dx}(\sin^{-1}(\cos x))$? | $-1$ (for $0 < x < \pi$) |
| P277 | Implicit diff: $e^{xy} = x + y$, find $y'$? | $\frac{1 - ye^{xy}}{xe^{xy} - 1}$ |
| P278 | Higher-order: $y = \sin(a\sin^{-1}x)$, Chebyshev| $(1-x^2)y'' - xy' + a^2 y = 0$ |
| P279 | Laplacian in polar: $\nabla^2 f$? | $f_{rr} + \frac{1}{r}f_r + \frac{1}{r^2}f_{\theta\theta}$ |
| P280 | Green's theorem relates? | Line integral around closed curve ↔ double integral over enclosed region |

---

## 📝 Practice Set 6: Comprehensive GATE Simulation (P281 – P360)

**P281.** (2-mark NAT) $\lim_{x \to 0} \frac{\sin(x^2)}{x \sin x}$ = ?
→ $\frac{\sin(x^2)}{x^2} \cdot \frac{x}{\sin x} \cdot x$ → wait: $\frac{\sin(x^2)}{x \sin x} = \frac{\sin(x^2)}{x^2} \cdot \frac{x}{\sin x}$
→ $= 1 \cdot 1 = \boxed{1}$

**P282.** (2-mark) $\int_0^1 \frac{x - 1}{\ln x} dx$ = ?
→ Feynman: $I(\alpha) = \int_0^1 \frac{x^\alpha - 1}{\ln x} dx$ → $I'(\alpha) = \int_0^1 x^\alpha dx = \frac{1}{\alpha + 1}$
→ $I(\alpha) = \ln(\alpha + 1)$ (since $I(0) = 0$)
→ $I(1) = \boxed{\ln 2}$

**P283.** (2-mark) Minimize distance from origin to plane $2x + 3y + 6z = 49$.
→ Use Lagrange or formula: $d = \frac{|49|}{\sqrt{4+9+36}} = \frac{49}{7} = \boxed{7}$

**P284.** (2-mark) Closest point on $2x + 3y + 6z = 49$ to origin?
→ Normal direction: $(2,3,6)$. Point $= t(2,3,6)$ on plane: $4t + 9t + 36t = 49$ → $t = 1$
→ Point: $(2, 3, 6)$

**P285.** (2-mark NAT) $f(x) = x - x^3/3$. Number of local extrema?
→ $f'(x) = 1 - x^2 = 0$ → $x = \pm 1$
→ $f''(1) = -2 < 0$ → local max. $f''(-1) = 2 > 0$ → local min.
→ **2 local extrema**

**P286.** (2-mark) Minimum of $f(x) = e^x + e^{-x}$?
→ $f'(x) = e^x - e^{-x} = 0$ → $e^{2x} = 1$ → $x = 0$
→ $f(0) = 2$. $f'' = e^x + e^{-x} > 0$ → **Global minimum = 2**

**P287.** (2-mark) $\int_{-\pi}^{\pi} x^3 \cos x \, dx$ = ?
→ $x^3$ is odd, $\cos x$ is even → $x^3 \cos x$ is **odd**
→ $\int_{-\pi}^{\pi} (\text{odd}) \, dx = \boxed{0}$

**P288.** (2-mark) Dirichlet function: $f(x) = \begin{cases} 1 & x \in \mathbb{Q} \\ 0 & x \notin \mathbb{Q} \end{cases}$. Continuous?
→ Discontinuous **everywhere!** (Every neighborhood of any point contains both rationals and irrationals.)

**P289.** (2-mark) Is Dirichlet function Riemann integrable on $[0,1]$?
→ **NO.** Upper Riemann sum = 1, Lower = 0. They never agree.

**P290.** (2-mark) Thomae's function (modified Dirichlet): $f(x) = \begin{cases} 1/q & x = p/q \text{ reduced} \\ 0 & x \text{ irrational} \end{cases}$.
→ Continuous at **every irrational**, discontinuous at **every rational**.
→ **Riemann integrable** (discontinuities form a set of measure zero).

**P291-P310** — Mixed Quick:

| # | Q | A |
|---|---|---|
| P291 | $\int_0^2 \lfloor x \rfloor \, dx$ | $0 \cdot 1 + 1 \cdot 1 = 1$ |
| P292 | $\int_0^3 \{x\} \, dx$ ($\{x\}$ = fractional part) | $3 \times 1/2 = 3/2$ (each unit interval contributes 1/2) |
| P293 | $\frac{d}{dx}\int_x^{x^2} e^{-t^2} dt$ | $2xe^{-x^4} - e^{-x^2}$ |
| P294 | Is $x^2 + y^2 \leq 1$ a convex set? | YES (unit disk is convex) |
| P295 | Is $\{(x,y) : xy \geq 1, x > 0\}$ convex? | YES |
| P296 | Epigraph of convex function is? | A convex set |
| P297 | Sublevel set of convex function is? | A convex set |
| P298 | Sum of convex functions is? | Convex |
| P299 | Maximum of convex functions is? | Convex |
| P300 | Composition $f(g(x))$: $f$ convex-nondecreasing, $g$ convex → ? | Convex |
| P301 | $\int_0^{\pi/2} \frac{\sin x}{\sin x + \cos x} dx$ | $\pi/4$ (by King's property: $I + I' = \pi/2$) |
| P302 | $\int_0^{\pi/2} \ln(\sin x) dx$ | $-\frac{\pi}{2}\ln 2$ |
| P303 | If $f$ has relative max at $c$, $f'(c) = ?$ | 0 (if $f'$ exists) — Fermat's theorem |
| P304 | Second derivative test: $f'(c) = 0, f''(c) = 0$? | Inconclusive! |
| P305 | $\int_0^a f(x) dx = \int_0^a f(a-x) dx$? | TRUE (King's property) |
| P306 | $\int_0^{2a} f(x) dx = 2\int_0^a f(x) dx$ when? | When $f(2a-x) = f(x)$ (symmetry about $x = a$) |
| P307 | Newton-Leibniz: $\int_a^b f'(x) dx = ?$ | $f(b) - f(a)$ |
| P308 | Convex combination: $\lambda x + (1-\lambda)y$ for? | $0 \leq \lambda \leq 1$ |
| P309 | Strong convexity: $f(y) \geq f(x) + \nabla f^T(y-x) + \frac{m}{2}\|y-x\|^2$ | Guarantees unique global minimum |
| P310 | Lipschitz continuous gradient: $\|\nabla f(x) - \nabla f(y)\| \leq L\|x-y\|$ | Guarantees GD converges with $\alpha < 2/L$ |

**P311-P330** — Integration Championships:

**P311.** (2-mark) $\int_0^{\infty} \frac{x}{e^x - 1} dx$ = ?
→ $= \sum_{n=1}^{\infty} \int_0^{\infty} x e^{-nx} dx = \sum_{n=1}^{\infty} \frac{1}{n^2} = \boxed{\frac{\pi^2}{6}}$

**P312.** (2-mark) $\int_0^1 \frac{\ln(1+x)}{x} dx$ = ?
→ $= \sum_{n=1}^{\infty} \frac{(-1)^{n+1}}{n} \int_0^1 x^{n-1} dx = \sum_{n=1}^{\infty} \frac{(-1)^{n+1}}{n^2} = \boxed{\frac{\pi^2}{12}}$

**P313.** (2-mark) $\int_0^1 x^a dx$ where $a > -1$?
→ $= \frac{1}{a+1}$ (valid for $a > -1$ to ensure convergence at 0)

**P314.** (2-mark) Area enclosed by $r = 2\cos\theta$ (polar)?
→ $A = \frac{1}{2}\int_0^{\pi} (2\cos\theta)^2 d\theta = 2\int_0^{\pi} \cos^2\theta \, d\theta = 2 \cdot \frac{\pi}{2} = \boxed{\pi}$
→ (It's a circle of radius 1 centered at $(1, 0)$, area = $\pi$!)

**P315-P330** — Rapid Answers:

| # | Q | A |
|---|---|---|
| P315 | $\int \frac{dx}{x^2+2x+5}$ | $\frac{1}{2}\tan^{-1}\frac{x+1}{2} + C$ (complete square: $(x+1)^2+4$) |
| P316 | $\int e^x \sin x \, dx$ | $\frac{e^x(\sin x - \cos x)}{2} + C$ |
| P317 | $\int \frac{x}{(x+1)(x+2)} dx$ | $-\ln|x+1| + 2\ln|x+2| + C$ (partial fractions) |
| P318 | $\int \sec^3 x \, dx$ | $\frac{1}{2}[\sec x \tan x + \ln|\sec x + \tan x|] + C$ |
| P319 | $\int_0^1 \frac{dx}{1+x^4}$ | $\frac{\pi}{4\sqrt{2}} + \frac{\ln(\sqrt{2}+1)}{2\sqrt{2}}$ (partial fractions with complex roots) |
| P320 | $\int_0^{\pi} \frac{x \sin x}{1+\cos^2 x} dx$ | $\frac{\pi^2}{4}$ (using $I = \int_0^{\pi} \frac{(\pi-x)\sin x}{1+\cos^2 x} dx$, add) |
| P321 | Is $\sum \frac{1}{n^3}$ convergent? | YES ($p = 3 > 1$; value = Apéry's constant $\zeta(3) \approx 1.202$) |
| P322 | $\sum_{n=0}^{\infty} \frac{(-1)^n}{(2n)!}$ = ? | $\cos 1 \approx 0.5403$ |
| P323 | $\sum_{n=0}^{\infty} \frac{1}{(2n+1)!}$ = ? | $\sinh 1 \approx 1.1752$ |
| P324 | Cauchy condensation test: $\sum a_n$ behaves like? | $\sum 2^n a_{2^n}$ |
| P325 | Abel's test for convergence? | $\sum a_n b_n$: if $\sum a_n$ converges and $b_n$ bounded monotone |
| P326 | Does $\sum \frac{\sin n}{n}$ converge? | YES (Dirichlet test: partial sums of $\sin n$ bounded, $1/n \to 0$) |
| P327 | Leibniz formula: $\frac{\pi}{4} = ?$ | $1 - 1/3 + 1/5 - 1/7 + \cdots$ |
| P328 | Basel problem: $\sum \frac{1}{n^2} = ?$ | $\frac{\pi^2}{6}$ |
| P329 | $e = \sum_{n=0}^{\infty} ?$ | $\frac{1}{n!}$ |
| P330 | Is $e$ rational? | NO (proved by Euler/Fourier) |

---

## 📝 Practice Set 7: Final GATE Blitz (P331 – P400)

**P331.** (2-mark) Find the envelope of the family $y = mx + a/m$ (varying $m$).
→ $F(x,y,m) = y - mx - a/m = 0$
→ $F_m = -x + a/m^2 = 0$ → $m^2 = a/x$ → $m = \sqrt{a/x}$
→ Substitute back: $y = \sqrt{ax} + \sqrt{ax} = 2\sqrt{ax}$
→ Envelope: $y^2 = 4ax$ — **a parabola!**

**P332.** (2-mark NAT) Minimum of $f(x) = x + 1/x$ for $x > 0$?
→ AM-GM: $x + 1/x \geq 2\sqrt{x \cdot 1/x} = 2$
→ Minimum = $\boxed{2}$ at $x = 1$

**P333.** (2-mark) $\int_0^1 \int_0^{\sqrt{1-x^2}} (x^2+y^2) \, dy \, dx$
→ Switch to polar: $\int_0^{\pi/2} \int_0^1 r^2 \cdot r \, dr \, d\theta = \frac{\pi}{2} \cdot \frac{1}{4} = \boxed{\frac{\pi}{8}}$

**P334.** (2-mark) Jacobian of spherical coordinates $(r, \theta, \phi) \to (x,y,z)$?
→ $|J| = r^2 \sin\phi$ (volume element: $r^2 \sin\phi \, dr \, d\theta \, d\phi$)

**P335.** (2-mark) $\iiint_{x^2+y^2+z^2 \leq 1} dV$ = ?
→ Volume of unit sphere: $\frac{4\pi}{3}$
→ In spherical: $\int_0^{2\pi} \int_0^{\pi} \int_0^1 r^2 \sin\phi \, dr \, d\phi \, d\theta = 2\pi \cdot 2 \cdot \frac{1}{3} = \boxed{\frac{4\pi}{3}}$ ✅

**P336-P360** — Mix of Everything:

| # | Q | A |
|---|---|---|
| P336 | $\lim_{n \to \infty} (1 + 1/n^2)^n$ | $1$ (not $e$! Need $1/n$ not $1/n^2$) |
| P337 | $\lim_{x \to 0^+} (\sin x)^x$ | Form $0^0$: $\ln L = x \ln(\sin x) \approx x \ln x \to 0$ → $L = 1$ |
| P338 | Is $f(x,y) = xy$ convex? | NO ($H$ has eigenvalues $\pm 1$, indefinite) |
| P339 | Hessian eigenvalues of $f = x^2 + 3y^2$? | $H = \text{diag}(2, 6)$, eigenvalues 2 and 6 |
| P340 | Condition number of $H$ in P339? | $6/2 = 3$ (affects GD convergence) |
| P341 | GD convergence rate for quadratic with condition $\kappa$? | $O\left(\left(\frac{\kappa-1}{\kappa+1}\right)^k\right)$ |
| P342 | Optimal learning rate for quadratic $f$? | $\alpha^* = \frac{2}{\lambda_{\min} + \lambda_{\max}}$ |
| P343 | Is norm $\|x\|$ convex? | YES (triangle inequality proves it) |
| P344 | Is log-sum-exp $\ln(e^{x_1} + \cdots + e^{x_n})$ convex? | YES (composition rule) |
| P345 | Softmax is gradient of? | Log-sum-exp |
| P346 | Cross-entropy loss convex in predictions? | YES |
| P347 | If $f$ strongly convex with param $m$: GD rate? | Linear convergence: $O((1-m\alpha)^k)$ |
| P348 | Proximal gradient descent used for? | Non-smooth optimization (L1 regularization) |
| P349 | Subgradient of $f(x) = |x|$ at $x = 0$? | Any value in $[-1, 1]$ (subdifferential) |
| P350 | $\frac{\partial}{\partial x}\|Ax - b\|^2 = ?$ | $2A^T(Ax - b)$ |
| P351 | Setting gradient to 0: $A^TAx = A^Tb$ → ? | Normal equation! $x = (A^TA)^{-1}A^Tb$ |
| P352 | Hessian of $\|Ax - b\|^2$? | $2A^TA$ (positive semidefinite) |
| P353 | Is least squares problem convex? | YES ($f = \|Ax-b\|^2$ has PSD Hessian) |
| P354 | Ridge regression adds what to loss? | $\lambda\|x\|^2$ (L2 regularization) |
| P355 | LASSO uses? | L1 penalty $\lambda\|x\|_1$ (promotes sparsity) |
| P356 | Is $\|x\|_1$ differentiable? | NO (at any $x_i = 0$) |
| P357 | Dual of maximize subject to $Ax \leq b$? | Minimize $b^T y$ subject to $A^Ty = c$, $y \geq 0$ |
| P358 | Slater's condition ensures? | Strong duality (zero duality gap) |
| P359 | Complementary slackness: $\mu_i g_i(x^*) = ?$ | 0 for each constraint |
| P360 | Second-order sufficient condition for min? | $\nabla f = 0$ AND $H$ positive definite |

---

## 📝 Practice Set 8: Speed Round (P361 – P400)

| # | Q | A |
|---|---|---|
| P361 | $\frac{d}{dx}[x^3 e^x]$ | $e^x(x^3 + 3x^2)$ |
| P362 | $\int x^2 \ln x \, dx$ | $\frac{x^3}{3}\ln x - \frac{x^3}{9} + C$ |
| P363 | $\int_0^1 \frac{1}{1+x^2} dx$ | $\tan^{-1}(1) = \pi/4$ |
| P364 | $\int_{-\infty}^{\infty} e^{-x^2/2} dx$ | $\sqrt{2\pi}$ |
| P365 | Partial: $\frac{\partial}{\partial y}(x^2 y + \sin(xy))$? | $x^2 + x\cos(xy)$ |
| P366 | Gradient of $f = x^2y + z^3$ at $(1,1,1)$? | $(2, 1, 3)$ |
| P367 | Divergence of $\vec{F} = (x, y, z)$? | $3$ (constant!) |
| P368 | Curl of $\vec{F} = (x, y, z)$? | $(0, 0, 0)$ (irrotational) |
| P369 | Is $\vec{F} = (y, x, 0)$ conservative? | Check: $\nabla \times \vec{F} = (0, 0, 0)$ → YES |
| P370 | Potential of $\vec{F} = (y, x, 0)$? | $\phi = xy$ (since $\nabla(xy) = (y, x, 0)$) |
| P371 | $\int_0^{2\pi} \cos^2 x \, dx$ | $\pi$ |
| P372 | $\int_0^{\pi} \sin^2 x \, dx$ | $\pi/2$ |
| P373 | Is $f(x) = 1/x$ uniformly continuous on $(0, 1)$? | NO (not uniformly continuous near 0) |
| P374 | Is $f(x) = \sin x$ uniformly continuous on $\mathbb{R}$? | YES (Lipschitz: $|\sin a - \sin b| \leq |a - b|$) |
| P375 | Rolle's for $f(x) = x(x-1)(x-2)$ on $[0, 2]$? | $f(0)=f(2)=0$ ✅, $c = 1 \pm 1/\sqrt{3}$ |
| P376 | $\int_0^{\pi/2} \frac{dx}{1 + \tan^n x}$ for any $n$? | $\pi/4$ (by King's!) |
| P377 | $\int_0^1 \frac{x^b - x^a}{\ln x} dx$ ($b > a > -1$)? | $\ln\frac{b+1}{a+1}$ (Frullani/Feynman) |
| P378 | If $f'(x) > 0$ on $(a,b)$? | $f$ is strictly increasing |
| P379 | Inflection: $f''(x)$ changes sign means? | Concavity changes → inflection point |
| P380 | $\int \frac{dx}{x \ln x}$ | $\ln|\ln x| + C$ |
| P381 | Euler's formula: $e^{i\theta}$ = ? | $\cos\theta + i\sin\theta$ |
| P382 | $\int_0^{2\pi} e^{\cos\theta}\cos(\sin\theta) d\theta$ | $2\pi$ (Re part of $\oint e^{e^{i\theta}} d\theta$) |
| P383 | Comparison test: $0 \leq a_n \leq b_n$ | If $\sum b_n$ conv → $\sum a_n$ conv |
| P384 | Limit comparison: $\lim a_n/b_n = L > 0$ | Both converge or both diverge |
| P385 | Root test: $\limsup \sqrt[n]{|a_n|} < 1$ | Converges |
| P386 | $\int_0^1 x^{-x} dx$ = ? | $\sum_{n=1}^{\infty} n^{-n} \approx 1.2913$ (Sophomore's dream!) |
| P387 | $\int_0^{\infty} \frac{\sin x}{x} dx$ | $\pi/2$ (Dirichlet integral) |
| P388 | Level curve of $f(x,y) = x^2 + y^2$ at value $c$? | Circle of radius $\sqrt{c}$ |
| P389 | Steepest descent direction? | $-\nabla f / |\nabla f|$ |
| P390 | Momentum in GD: $v_t = \beta v_{t-1} + \nabla f(x_t)$ | Accelerates convergence, escapes local minima |
| P391 | Adam optimizer combines? | Momentum + adaptive learning rate (RMSProp) |
| P392 | Learning rate schedule: step decay? | Reduce $\alpha$ by factor every $k$ epochs |
| P393 | Batch GD vs SGD? | Batch: all data per step; SGD: one sample per step |
| P394 | Mini-batch typical size? | 32, 64, 128 |
| P395 | Convex function on compact set has? | Global minimum (Weierstrass theorem) |
| P396 | Number of saddle points typically in high-D? | Exponentially many! (Curse of dimensionality) |
| P397 | Hessian-free optimization? | Use conjugate gradient instead of computing full Hessian |
| P398 | Line search in optimization? | Find optimal step size along search direction |
| P399 | Wolfe conditions? | Sufficient decrease + curvature condition for step size |
| P400 | Duality gap at optimum for convex? | Zero (strong duality) |

---

## 📝 Practice Set 9: Mega GATE Drill (P401 – P500)

**P401.** (2-mark) $\lim_{x \to 0} \frac{\sin^{-1} x - \tan^{-1} x}{x^3}$ = ?
→ Taylor: $\sin^{-1}x = x + \frac{x^3}{6} + \cdots$, $\tan^{-1}x = x - \frac{x^3}{3} + \cdots$
→ $\frac{x^3/6 + x^3/3}{x^3} = \frac{1}{6} + \frac{1}{3} = \boxed{\frac{1}{2}}$

**P402.** (2-mark NAT) $\lim_{n \to \infty} \left(\frac{1}{n+1} + \frac{1}{n+2} + \cdots + \frac{1}{2n}\right)$ = ?
→ $= \sum_{k=1}^{n} \frac{1}{n+k} = \frac{1}{n}\sum_{k=1}^{n} \frac{1}{1+k/n} \to \int_0^1 \frac{dx}{1+x} = \ln 2$
→ $= \boxed{\ln 2 \approx 0.693}$

**P403.** (2-mark) $f(x) = \int_0^x e^{-t^2} dt$. Is $f$ concave up or concave down at $x = 1$?
→ $f'(x) = e^{-x^2}$, $f''(x) = -2xe^{-x^2}$
→ $f''(1) = -2e^{-1} < 0$ → **Concave down** at $x = 1$

**P404.** (2-mark) Prove $e^x > 1 + x$ for $x \neq 0$.
→ Let $g(x) = e^x - 1 - x$. $g(0) = 0$, $g'(x) = e^x - 1$.
→ $g'(x) < 0$ for $x < 0$, $g'(x) > 0$ for $x > 0$ → $x = 0$ is global min.
→ $g(x) \geq g(0) = 0$ with equality only at $x = 0$ → $e^x \geq 1 + x$ ✅

**P405.** (2-mark) Taylor remainder bound: Approximate $\sin(0.1)$ using degree 3 Taylor. Error bound?
→ $\sin x \approx x - x^3/6$ → $\sin(0.1) \approx 0.1 - 0.001/6 = 0.09983\overline{3}$
→ Error $\leq \frac{|x|^5}{5!} = \frac{(0.1)^5}{120} = \frac{10^{-5}}{120} < 10^{-7}$

**P406.** (2-mark) Evaluate $\int_0^1 \int_0^1 \frac{1}{1-xy} dx \, dy$ (if convergent).
→ $\frac{1}{1-xy} = \sum_{n=0}^{\infty} (xy)^n$ for $|xy| < 1$
→ $\int_0^1 \int_0^1 \sum (xy)^n dx\, dy = \sum \frac{1}{(n+1)^2} = \frac{\pi^2}{6}$
→ $= \boxed{\frac{\pi^2}{6}}$ (another proof of Basel problem!)

**P407.** (2-mark) $f(x,y) = x^2 - y^2$. Classify $(0,0)$.
→ $D = f_{xx}f_{yy} - f_{xy}^2 = 2 \cdot (-2) - 0 = -4 < 0$ → **Saddle point!**
→ This is a hyperbolic paraboloid (like a Pringles chip 🥔)

**P408.** (2-mark) $\int_0^{\infty} x^{n-1} e^{-x} dx$ for general $n > 0$?
→ $= \Gamma(n)$ (definition of the Gamma function!)

**P409.** (2-mark) Relationship: $\Gamma(n) \cdot \Gamma(1-n) = ?$ for $0 < n < 1$?
→ $= \frac{\pi}{\sin(n\pi)}$ (Euler's reflection formula)

**P410.** (2-mark) $\Gamma(1/2) \cdot \Gamma(1/2) = ?$
→ By reflection: $\frac{\pi}{\sin(\pi/2)} = \pi$ → $\Gamma(1/2)^2 = \pi$ → $\Gamma(1/2) = \sqrt{\pi}$ ✅

**P411-P430** — Multivariable Deep:

**P411.** (2-mark) Change of variables: $\iint_R xy \, dA$ where $R$ is bounded by $xy = 1, xy = 4, y = x, y = 4x$.
→ Let $u = xy, v = y/x$. Compute Jacobian of inverse and transform.
→ $x = \sqrt{u/v}$, $y = \sqrt{uv}$ → $|J| = \frac{1}{2v}$
→ $\int_1^4 \int_1^4 u \cdot \frac{1}{2v} du \, dv = \frac{1}{2}\int_1^4 \frac{dv}{v} \int_1^4 u \, du = \frac{1}{2} \cdot \ln 4 \cdot \frac{15}{2} = \frac{15 \ln 4}{4}$

**P412.** (2-mark) $\vec{F} = (2xy, x^2 + z, y)$. Is $\vec{F}$ conservative?
→ $\nabla \times \vec{F} = \left(\frac{\partial y}{\partial y} - \frac{\partial(x^2+z)}{\partial z}, \frac{\partial(2xy)}{\partial z} - \frac{\partial y}{\partial x}, \frac{\partial(x^2+z)}{\partial x} - \frac{\partial(2xy)}{\partial y}\right)$
→ $= (1 - 1, 0 - 0, 2x - 2x) = (0, 0, 0)$ → **YES, conservative!**
→ Potential: $\phi = x^2y + yz + C$ (verify: $\nabla\phi = (2xy, x^2+z, y)$ ✅)

**P413.** (2-mark) Line integral $\int_C \vec{F} \cdot d\vec{r}$ from $(0,0,0)$ to $(1,1,1)$ for conservative $\vec{F}$ in P412?
→ Path independent! $\phi(1,1,1) - \phi(0,0,0) = (1 + 1) - 0 = \boxed{2}$

**P414.** (2-mark) Green's theorem: $\oint_C (y^2 dx + 3xy \, dy)$ where $C$ is unit circle.
→ $\iint_R \left(\frac{\partial(3xy)}{\partial x} - \frac{\partial(y^2)}{\partial y}\right) dA = \iint_R (3y - 2y) dA = \iint_R y \, dA$
→ By symmetry of unit disk: $\iint y \, dA = 0$ → $\boxed{0}$

**P415.** (2-mark) Stokes' theorem relates?
→ $\oint_C \vec{F} \cdot d\vec{r} = \iint_S (\nabla \times \vec{F}) \cdot d\vec{S}$
→ Line integral around boundary = surface integral of curl

**P416.** (2-mark) Gauss divergence theorem relates?
→ $\oiint_S \vec{F} \cdot d\vec{S} = \iiint_V (\nabla \cdot \vec{F}) \, dV$
→ Surface integral = volume integral of divergence

**P417.** (2-mark) Flux of $\vec{F} = (x, y, z)$ through unit sphere?
→ Divergence: $\nabla \cdot \vec{F} = 3$
→ $\iiint \nabla \cdot \vec{F} \, dV = 3 \cdot \frac{4\pi}{3} = \boxed{4\pi}$

**P418.** (2-mark) Is $f(x,y) = e^{x+y}$ convex?
→ $H = \begin{pmatrix} e^{x+y} & e^{x+y} \\ e^{x+y} & e^{x+y} \end{pmatrix}$
→ Eigenvalues: 0 and $2e^{x+y}$ → positive SEMIDEFINITE → **convex** (not strictly)

**P419.** (2-mark) Convex conjugate of $f(x) = x^2/2$?
→ $f^*(y) = \sup_x (xy - x^2/2) = y^2/2$ (at $x = y$)
→ **Self-conjugate!** $f^* = f$

**P420.** (2-mark) Moreau envelope? Used for?
→ $M_f(x) = \inf_y \left(f(y) + \frac{1}{2\lambda}\|x-y\|^2\right)$
→ Smooth approximation of non-smooth function. Used in proximal algorithms.

**P421-P440** — ODE Advanced:

**P421.** (2-mark) Solve: $y'' + y = \sin x$ (resonance case!).
→ Characteristic: $r^2 + 1 = 0$ → $r = \pm i$ → $y_c = A\cos x + B\sin x$
→ $\sin x$ is part of complementary solution! Use variation of parameters or undetermined × $x$:
→ $y_p = \frac{-x\cos x}{2}$
→ General: $y = A\cos x + B\sin x - \frac{x\cos x}{2}$

**P422.** (2-mark) Solve: $y'' + 4y' + 4y = 0$.
→ Char: $r^2 + 4r + 4 = (r+2)^2 = 0$ → $r = -2$ (repeated)
→ $y = (A + Bx)e^{-2x}$

**P423.** (2-mark) Solve: $y'' + y' - 6y = 0$.
→ Char: $r^2 + r - 6 = (r+3)(r-2) = 0$ → $r = -3, 2$
→ $y = Ae^{2x} + Be^{-3x}$

**P424.** (2-mark) Particular solution of $y'' - y = e^x$?
→ Char: $r^2 - 1 = 0$ → $r = \pm 1$. Since $e^x$ is part of $y_c$:
→ Try $y_p = Cxe^x$: $y_p'' - y_p = C(xe^x + 2e^x) - Cxe^x = 2Ce^x = e^x$ → $C = 1/2$
→ $y_p = \frac{x}{2}e^x$

**P425.** (2-mark) Variation of parameters for $y'' + y = \sec x$:
→ $y_c = c_1\cos x + c_2\sin x$. $W = \cos x \cdot \cos x + \sin x \cdot \sin x = 1$
→ $y_p = -\cos x \int \frac{\sec x \sin x}{1} dx + \sin x \int \frac{\sec x \cos x}{1} dx$
→ $= -\cos x \int \tan x \, dx + \sin x \int dx$
→ $= -\cos x(-\ln|\cos x|) + x\sin x = \cos x \ln|\cos x| + x\sin x$

**P426-P440** — Quick ODE Answers:

| # | ODE | Characteristic / Type | Solution |
|---|---|---|---|
| P426 | $y'' + 9y = 0$ | $r = \pm 3i$ | $A\cos 3x + B\sin 3x$ |
| P427 | $y'' - 9y = 0$ | $r = \pm 3$ | $Ae^{3x} + Be^{-3x}$ |
| P428 | $y'' + 6y' + 9y = 0$ | $r = -3$ (double) | $(A + Bx)e^{-3x}$ |
| P429 | $y'' + 2y' + 5y = 0$ | $r = -1 \pm 2i$ | $e^{-x}(A\cos 2x + B\sin 2x)$ |
| P430 | $y^{(4)} - y = 0$ | $r = \pm 1, \pm i$ | $Ae^x + Be^{-x} + C\cos x + D\sin x$ |
| P431 | $xy'' + y' = 0$ (Euler) | $r = 0$ (double) | $A + B\ln x$ |
| P432 | $x^2y'' - 2y = 0$ | $m(m-1) - 2 = 0$, $m=2,-1$ | $Ax^2 + B/x$ |
| P433 | $(D^2 + 1)y = \cos x$ (resonance) | Particular | $y_p = \frac{x}{2}\sin x$ |
| P434 | $y' = x + y$, $y(0) = 0$ | Linear 1st order | $y = e^x - x - 1$ |
| P435 | $y' = y^2 + 1$ | Separable: $\tan^{-1}y = x + C$ | $y = \tan(x + C)$ |
| P436 | Clairaut's: $y = px + f(p)$, $p = y'$ | Singular + general | General: family of lines; Singular: envelope |
| P437 | Riccati: $y' = y^2 + x$ | Non-linear, no general method | Need particular solution first |
| P438 | $y' + y\cos x = \sin(2x)/2$ | IF $= e^{\sin x}$ | $ye^{\sin x} = \int \sin x \cos x \cdot e^{\sin x} dx$ |
| P439 | Orthogonal trajectories of $y = cx^2$? | $y' = 2y/x$ → ortho: $y' = -x/(2y)$ | $x^2 + 2y^2 = C$ (ellipses) |
| P440 | Is $y'' + py' + qy = 0$ stable when? | All roots of char.eq have Re < 0 | $p > 0, q > 0$ (Routh-Hurwitz) |

**P441-P470** — Optimization Marathon:

**P441.** (2-mark) Minimize $f(x,y) = 2x^2 + 3y^2 - 4x - 12y + 20$.
→ $f_x = 4x - 4 = 0$ → $x = 1$. $f_y = 6y - 12 = 0$ → $y = 2$
→ $H = \text{diag}(4, 6)$ → PD → minimum. $f(1,2) = 2 + 12 - 4 - 24 + 20 = \boxed{6}$

**P442.** (2-mark) Maximize $f(x,y) = x^2 y$ on the ellipse $x^2 + 4y^2 = 12$.
→ Lagrange: $(2xy, x^2) = \lambda(2x, 8y)$
→ From 1st eqn: $2xy = 2\lambda x$ → if $x \neq 0$: $\lambda = y$
→ From 2nd: $x^2 = 8y \cdot y = 8y^2$ → $x^2 = 8y^2$
→ Constraint: $8y^2 + 4y^2 = 12$ → $y^2 = 1$ → $y = 1$ (take positive)
→ $x^2 = 8$ → $f = 8 \cdot 1 = \boxed{8}$

**P443.** (2-mark) Proximal operator of $f(x) = \lambda|x|$?
→ $\text{prox}_f(v) = \text{sign}(v) \max(|v| - \lambda, 0)$ — **soft thresholding!**
→ Fundamental to LASSO/L1 regularization. Values $< \lambda$ shrink to zero (sparsity!).

**P444.** (2-mark) Conjugate gradient method: after $n$ steps on $n$-dimensional quadratic?
→ **Exact solution!** (CG converges in at most $n$ steps for $n \times n$ quadratic)

**P445.** (2-mark) Learning rate too large in GD? What happens?
→ **Divergence!** Oscillations grow wider, loss increases. If $\alpha > 2/\lambda_{\max}$, GD diverges.

**P446.** (2-mark) Learning rate too small?
→ Very slow convergence. Many iterations needed. May get stuck in flat regions.

**P447.** (2-mark) Batch normalization helps GD because?
→ Reduces internal covariate shift, smooths loss landscape, allows higher learning rate.

**P448.** (2-mark) Is $f(x) = -\log(x)$ convex for $x > 0$?
→ $f''(x) = 1/x^2 > 0$ → **YES, strictly convex** ✅
→ Key function in log-barrier methods for interior point optimization.

**P449-P470** — Final MCQ Bank:

| # | Q | A |
|---|---|---|
| P449 | Extreme Value Theorem requires? | Continuous $f$ on **closed, bounded** interval |
| P450 | IVT: if $f(a) < k < f(b)$, then? | $\exists c \in (a,b): f(c) = k$ (continuous $f$ on $[a,b]$) |
| P451 | Fixed point theorem (Brouwer)? | Continuous map from compact convex set to itself has fixed point |
| P452 | Contraction mapping theorem? | Unique fixed point; iterates converge to it |
| P453 | $\frac{d}{dx}\Gamma(x)$ at $x = 1$? | $\Gamma'(1) = \psi(1) = -\gamma \approx -0.5772$ (Euler-Mascheroni) |
| P454 | Stirling approximation: $n! \approx ?$ | $\sqrt{2\pi n} (n/e)^n$ |
| P455 | Is $\sin(1/x)$ uniformly continuous on $(0,1)$? | NO (oscillates infinitely near 0) |
| P456 | Is $x \sin(1/x)$ uniformly continuous on $(0,1)$? | YES (bounded derivative) |
| P457 | Taylor: $\frac{1}{1+x^2}$ centered at 0? | $\sum_{n=0}^{\infty} (-1)^n x^{2n}$, $|x| < 1$ |
| P458 | Integrating term by term: $\int_0^x \frac{dt}{1+t^2}$? | $\sum (-1)^n \frac{x^{2n+1}}{2n+1} = \tan^{-1}x$ ✅ |
| P459 | Differentiating power series: same radius of convergence? | YES! |
| P460 | Integrating power series: same radius of convergence? | YES! |
| P461 | $\int_0^{\infty} e^{-ax^2} dx$ for $a > 0$? | $\frac{1}{2}\sqrt{\pi/a}$ |
| P462 | $\int_0^{\infty} x^2 e^{-ax^2} dx$? | $\frac{\sqrt{\pi}}{4a^{3/2}}$ |
| P463 | Laplace transform: $\mathcal{L}\{e^{at}\}$? | $\frac{1}{s-a}$ for $s > a$ |
| P464 | Fourier series of square wave: harmonics? | $\frac{4}{\pi}\sum \frac{\sin((2n+1)x)}{2n+1}$ (only odd harmonics) |
| P465 | Parseval's theorem for Fourier? | $\frac{1}{2\pi}\int |f|^2 = \sum |c_n|^2$ |
| P466 | Is $\log(\det X)$ concave for $X \succ 0$? | YES (important for SDP, statistics) |
| P467 | Schur complement positive definite iff? | Block matrix PD iff both diagonal blocks and Schur complement PD |
| P468 | Boyd & Vandenberghe: convex optimization textbook? | Standard reference for GATE DA optimization theory |
| P469 | SVM margin maximization is equivalent to? | Minimizing $\|w\|^2$ subject to $y_i(w^Tx_i + b) \geq 1$ |
| P470 | Dual of SVM? | Maximize $\sum \alpha_i - \frac{1}{2}\sum\alpha_i\alpha_j y_i y_j x_i^Tx_j$ s.t. $\alpha_i \geq 0$ |

**P471-P500** — Express Round — 30 Seconds Each:

| # | Q | A |
|---|---|---|
| P471 | $\lim_{x \to 0} \frac{x}{|x|}$ | DNE (LHL = -1, RHL = 1) |
| P472 | Leibniz formula: $(uv)^{(n)}$ | $\sum_{k=0}^n \binom{n}{k} u^{(k)}v^{(n-k)}$ |
| P473 | $\int_a^b f(x) dx \approx ?$ (Trapezoidal) | $\frac{h}{2}[f(a) + f(b)]$ |
| P474 | Simpson's rule? | $\frac{h}{3}[f(a) + 4f(m) + f(b)]$ where $m = (a+b)/2$ |
| P475 | Error in trapezoidal: $O(h^?)$ | $O(h^2)$ per interval, $O(h^2)$ globally |
| P476 | Error in Simpson's: $O(h^?)$ | $O(h^4)$ per interval |
| P477 | Newton-Cotes: what type of numerical integration? | Polynomial interpolation based |
| P478 | Gauss quadrature advantage? | Optimal node placement → higher accuracy |
| P479 | Is $\|x\|_2^2 = x^Tx$ convex? | YES (Hessian = $2I$, PD) |
| P480 | Gradient of $x^TAx$? | $(A + A^T)x$ |
| P481 | If $A$ symmetric: gradient of $x^TAx$? | $2Ax$ |
| P482 | Gradient of $a^Tx$? | $a$ |
| P483 | Gradient of $\|Ax-b\|_2^2$? | $2A^T(Ax-b)$ |
| P484 | Trace: $\text{tr}(AB) = ?$ | $\text{tr}(BA)$ (cyclic property) |
| P485 | $\frac{d}{dX}\text{tr}(AX)$? | $A^T$ |
| P486 | $\frac{d}{dX}\text{tr}(X^TAX)$? | $(A+A^T)X$ |
| P487 | Positive definite $\iff$ all eigenvalues? | $> 0$ |
| P488 | Convex function: sublevel sets are? | Convex |
| P489 | Epigraph of $f$: $\{(x,t) : f(x) \leq t\}$ is convex iff? | $f$ is convex |
| P490 | Perspective function preserves? | Convexity |
| P491 | Affine transformation preserves? | Convexity |
| P492 | Pointwise max of convex functions is? | Convex |
| P493 | Pointwise infimum of affine functions is? | Concave |
| P494 | Indicator function of convex set is? | Convex |
| P495 | Support function of convex set? | $\sigma_C(y) = \sup_{x \in C} y^Tx$ — always convex |
| P496 | Fenchel-Young inequality? | $f(x) + f^*(y) \geq x^Ty$ |
| P497 | Moreau decomposition? | $x = \text{prox}_f(x) + \text{prox}_{f^*}(x)$ |
| P498 | ADMM stands for? | Alternating Direction Method of Multipliers |
| P499 | Interior point method: barrier function? | $-\sum \log(-g_i(x))$ |
| P500 | Central path parameter $t \to \infty$? | Solution approaches optimal |

---

## 🔮 GATE 2025-2027 Predicted Hot Topics — Calculus

1. **Limits involving Taylor expansion** — almost guaranteed every year
2. **Definite integrals with Gamma/Beta** — high probability for DA
3. **LMVT/Rolle's theorem application** — classic GATE favorite
4. **Critical point classification (2D Hessian test)** — regular appearance
5. **Lagrange multiplier optimization** — very high for DA
6. **Gradient descent iteration** — emerging topic for DA
7. **Convexity verification** — new trend for DA
8. **Double integrals (polar + Cartesian)** — DA staple
9. **ODE: linear first-order with IF** — regular
10. **KKT conditions** — DA papers increasingly feature this

---

## 📊 Complete Topic Difficulty & Frequency

| Topic | Conceptual | Computational | GATE CS | GATE DA |
|---|---|---|---|---|
| Limits & L'Hôpital | ★★☆☆☆ | ★★★☆☆ | ★★★★☆ | ★★★☆☆ |
| Continuity & Differentiability | ★★★☆☆ | ★★☆☆☆ | ★★★☆☆ | ★★☆☆☆ |
| Mean Value Theorems | ★★★☆☆ | ★★★☆☆ | ★★★★☆ | ★★☆☆☆ |
| Integration Techniques | ★★☆☆☆ | ★★★★☆ | ★★★★☆ | ★★★★☆ |
| Improper Integrals | ★★★☆☆ | ★★★☆☆ | ★★☆☆☆ | ★★★☆☆ |
| Gamma/Beta Functions | ★★☆☆☆ | ★★★☆☆ | ★☆☆☆☆ | ★★★★☆ |
| Taylor Series | ★★★☆☆ | ★★★★☆ | ★★★☆☆ | ★★★☆☆ |
| Partial Derivatives | ★★☆☆☆ | ★★★☆☆ | ★★★☆☆ | ★★★★☆ |
| Gradient/Directional Deriv | ★★☆☆☆ | ★★★☆☆ | ★★☆☆☆ | ★★★★★ |
| Divergence & Curl | ★★★☆☆ | ★★☆☆☆ | ★☆☆☆☆ | ★★★☆☆ |
| Unconstrained Optimization | ★★★☆☆ | ★★★☆☆ | ★★☆☆☆ | ★★★★★ |
| Convexity | ★★★★☆ | ★★☆☆☆ | ★☆☆☆☆ | ★★★★★ |
| Gradient Descent | ★★★☆☆ | ★★★☆☆ | ★☆☆☆☆ | ★★★★★ |
| Lagrange Multipliers | ★★★☆☆ | ★★★★☆ | ★★★☆☆ | ★★★★★ |
| KKT Conditions | ★★★★☆ | ★★★★☆ | ★☆☆☆☆ | ★★★★★ |
| Double Integrals | ★★☆☆☆ | ★★★★☆ | ★☆☆☆☆ | ★★★★☆ |
| ODEs | ★★★☆☆ | ★★★★☆ | ★☆☆☆☆ | ★★★★☆ |
| Series Convergence | ★★★☆☆ | ★★★☆☆ | ★★☆☆☆ | ★★★☆☆ |

---

## 🧠 30 One-Liner Rapid Recall — Calculus

1. $\lim \frac{\sin x}{x} = 1$. $\lim \frac{\tan x}{x} = 1$. $\lim \frac{e^x - 1}{x} = 1$. $\lim \frac{\ln(1+x)}{x} = 1$.
2. $1^\infty$ form: $e^{\lim f(x) \cdot g(x)}$ where $f \to 0, g \to \infty$.
3. L'Hôpital ONLY for $0/0$ or $\infty/\infty$. No other form!
4. Continuous on $[a,b]$ → IVT + EVT. Differentiable on $(a,b)$ → MVT.
5. $f$ differentiable at $a$ ⇒ continuous. Converse FALSE.
6. Rolle's: $f(a) = f(b)$ + continuous + differentiable → $\exists c: f'(c) = 0$.
7. LMVT: $\frac{f(b) - f(a)}{b - a} = f'(c)$ for some $c \in (a,b)$.
8. Critical point: $f'(c) = 0$ or $f'(c)$ DNE. Second test: $f'' > 0$ min, $f'' < 0$ max.
9. $\int_a^b f = F(b) - F(a)$ where $F' = f$ (FTC).
10. Integration by parts: $\int u\,dv = uv - \int v\,du$. LIATE rule for choosing $u$.
11. $\Gamma(n) = (n-1)!$. $\Gamma(1/2) = \sqrt{\pi}$. $B(m,n) = \Gamma(m)\Gamma(n)/\Gamma(m+n)$.
12. Gaussian: $\int_{-\infty}^{\infty} e^{-x^2} = \sqrt{\pi}$. Half: $\int_0^{\infty} e^{-x^2} = \sqrt{\pi}/2$.
13. Taylor: $e^x = \sum x^n/n!$. $\sin x = \sum (-1)^n x^{2n+1}/(2n+1)!$.
14. $f_{xy} = f_{yx}$ (Clairaut's) when both mixed partials are continuous.
15. Gradient $\nabla f$ points in steepest ascent. Max rate = $|\nabla f|$.
16. Directional deriv: $D_{\hat{u}}f = \nabla f \cdot \hat{u}$. Must use UNIT vector!
17. $D = f_{xx}f_{yy} - f_{xy}^2$. $D > 0$: extremum. $D < 0$: saddle. $D = 0$: inconclusive.
18. Convex: $f(\lambda x + (1-\lambda)y) \leq \lambda f(x) + (1-\lambda)f(y)$. Equiv: $H \succeq 0$.
19. Every local min of convex function is global. $\nabla f = 0$ ↔ global min.
20. GD: $x_{k+1} = x_k - \alpha \nabla f(x_k)$. $\alpha$ too big → diverge. Too small → slow.
21. Newton: $x_{k+1} = x_k - H^{-1}\nabla f$. Quadratic convergence near optimum. Expensive per step.
22. Lagrange: $\nabla f = \lambda \nabla g$ at constrained optimum. Solve system simultaneously.
23. KKT: $\nabla f + \mu \nabla g = 0$, $\mu \geq 0$, $\mu g = 0$ (complementary slackness).
24. Separable ODE: $\frac{dy}{dx} = h(x)g(y)$ → separate and integrate both sides.
25. Linear ODE: $y' + P(x)y = Q(x)$. IF = $e^{\int P\,dx}$. Multiply through and integrate.
26. Exact: $M\,dx + N\,dy = 0$ is exact iff $M_y = N_x$.
27. Double integral: $\iint f\,dA = \int\int f\,dx\,dy$ or $\int\int f \cdot r\,dr\,d\theta$ (polar).
28. Jacobian for polar: $r$. Spherical: $r^2\sin\phi$. Always take $|J|$!
29. Series: $p$-series $\sum 1/n^p$ converges for $p > 1$. Geometric $\sum r^n$ for $|r| < 1$.
30. Jensen: $f$ convex → $f(E[X]) \leq E[f(X)]$. Fundamental inequality in ML/statistics.

---

## 📝 Practice Set 10: Ultimate Marathon (P501 – P620)

**P501.** (2-mark) $\lim_{x \to 0} \left(\frac{1}{\sin^2 x} - \frac{1}{x^2}\right)$ = ?
→ $= \lim \frac{x^2 - \sin^2 x}{x^2 \sin^2 x}$. Taylor: $\sin x = x - x^3/6 + \cdots$
→ $\sin^2 x = x^2 - x^4/3 + \cdots$ → $x^2 - \sin^2 x = x^4/3 + \cdots$
→ $\frac{x^4/3}{x^4} = \boxed{\frac{1}{3}}$

**P502.** (2-mark NAT) $\int_0^2 x^3 \sqrt{4-x^2} \, dx$ = ?
→ Sub $x = 2\sin\theta$: $x^3 = 8\sin^3\theta$, $\sqrt{4-x^2} = 2\cos\theta$, $dx = 2\cos\theta \, d\theta$
→ $= \int_0^{\pi/2} 8\sin^3\theta \cdot 2\cos\theta \cdot 2\cos\theta \, d\theta = 32\int_0^{\pi/2} \sin^3\theta\cos^2\theta \, d\theta$
→ $= 32 \cdot \frac{1}{2}B(2, 3/2) = 32 \cdot \frac{\Gamma(2)\Gamma(3/2)}{2\Gamma(7/2)}$
→ $\Gamma(2) = 1$, $\Gamma(3/2) = \sqrt{\pi}/2$, $\Gamma(7/2) = 15\sqrt{\pi}/8$
→ $= 32 \cdot \frac{\sqrt{\pi}/2}{2 \cdot 15\sqrt{\pi}/8} = 32 \cdot \frac{4}{60} = \boxed{\frac{128}{60} = \frac{32}{15}}$

**P503.** (2-mark) Show $\int_0^{\pi/2} \sin^{2n} x \, dx = \frac{(2n)!}{4^n (n!)^2} \cdot \frac{\pi}{2}$
→ Wallis: $= \frac{(2n-1)!!}{(2n)!!} \cdot \frac{\pi}{2} = \frac{1 \cdot 3 \cdots (2n-1)}{2 \cdot 4 \cdots 2n} \cdot \frac{\pi}{2}$
→ $= \frac{(2n)!}{2^n n! \cdot 2^n n!} \cdot \frac{\pi}{2} = \frac{(2n)!}{4^n (n!)^2} \cdot \frac{\pi}{2}$ ✅

**P504.** (2-mark) $\sum_{n=1}^{\infty} \frac{(-1)^{n+1}}{n} = ?$ Prove using integral.
→ $\ln 2 = \int_0^1 \frac{1}{1+t} dt = \int_0^1 \sum_{n=0}^{\infty} (-t)^n dt = \sum_{n=0}^{\infty} \frac{(-1)^n}{n+1} = \sum_{n=1}^{\infty} \frac{(-1)^{n+1}}{n}$
→ $= \boxed{\ln 2}$ ✅

**P505.** (2-mark) Evaluate $\lim_{n \to \infty} \prod_{k=1}^{n} \left(1 + \frac{k}{n^2}\right)$
→ $\ln P_n = \sum_{k=1}^n \ln\left(1 + \frac{k}{n^2}\right) \approx \sum \frac{k}{n^2} = \frac{n(n+1)}{2n^2} \to \frac{1}{2}$
→ $P_n \to e^{1/2} = \boxed{\sqrt{e}}$

**P506.** (2-mark) $f(x,y) = \begin{cases} \frac{xy}{x^2+y^2} & (x,y) \neq (0,0) \\ 0 & (x,y) = (0,0) \end{cases}$
Is $f$ continuous at origin?
→ Along $y = x$: $f = x^2/(2x^2) = 1/2$. Along $y = 0$: $f = 0$. Different limits!
→ **NOT continuous** at origin.

**P507.** (2-mark) Same $f$: do partial derivatives exist at origin?
→ $f_x(0,0) = \lim_{h \to 0} \frac{f(h,0) - f(0,0)}{h} = \lim \frac{0}{h} = 0$
→ $f_y(0,0) = 0$ similarly
→ **YES, both partials exist and equal 0!** But $f$ is not continuous → partial derivatives existing does NOT imply continuity in multivariable!

**P508.** (2-mark) $f(x,y) = \begin{cases} \frac{x^2y}{x^4+y^2} & (x,y) \neq (0,0) \\ 0 & (0,0) \end{cases}$
Along $y = mx$: $\frac{mx^3}{x^4+m^2x^2} = \frac{mx}{x^2+m^2} \to 0$
Along $y = x^2$: $\frac{x^4}{x^4+x^4} = \frac{1}{2}$
→ Different limits along different paths → **NOT continuous, limit DNE**.

**P509.** (2-mark) Prove: If $f$ is differentiable at $(a,b)$, then $f$ is continuous at $(a,b)$.
→ Differentiable means $f(a+h, b+k) - f(a,b) = f_x h + f_y k + \epsilon_1 h + \epsilon_2 k$ where $\epsilon_i \to 0$.
→ As $(h,k) \to (0,0)$: RHS → 0, so $f(a+h,b+k) \to f(a,b)$ → continuous ✅

**P510-P530** — Series Power Pack:

| # | Series / Sum | Value |
|---|---|---|
| P510 | $\sum_{n=1}^{\infty} \frac{1}{4^n}$ | $\frac{1}{3}$ |
| P511 | $\sum_{n=0}^{\infty} \frac{(-1)^n}{(2n+1)!}$ | $\sin 1 \approx 0.8415$ |
| P512 | $\sum_{n=0}^{\infty} \frac{1}{(2n)!}$ | $\cosh 1 \approx 1.5431$ |
| P513 | $\sum_{n=1}^{\infty} \frac{n}{3^n}$ | $\frac{3}{4}$ (differentiate geometric) |
| P514 | $\sum_{n=1}^{\infty} \frac{n^2}{2^n}$ | $6$ |
| P515 | $\sum_{n=2}^{\infty} \frac{1}{n(n-1)}$ | $1$ (telescoping: $\frac{1}{n-1} - \frac{1}{n}$) |
| P516 | $\sum_{n=1}^{\infty} \frac{1}{n \cdot 2^n}$ | $\ln 2$ (from $-\ln(1-x)$ at $x = 1/2$) |
| P517 | $\sum_{n=0}^{\infty} (-1)^n \frac{x^{2n+1}}{2n+1}$ | $\tan^{-1}x$, $|x| \leq 1$ |
| P518 | Catalan's constant $G = \sum \frac{(-1)^n}{(2n+1)^2}$ | $\approx 0.9159$ |
| P519 | $\sum_{n=1}^{\infty} \frac{1}{n^4}$ | $\frac{\pi^4}{90}$ |
| P520 | Alternating harmonic at $x = -1$: $\ln(1+x)$? | $\ln(1+(-1)) = \ln 0 = -\infty$ → series diverges at $x = -1$ |
| P521 | Binomial series $(1+x)^{1/2}$ coeff of $x^2$? | $\binom{1/2}{2} = \frac{(1/2)(-1/2)}{2} = -1/8$ |
| P522 | $(1+x)^{-1}$ series? | $1 - x + x^2 - x^3 + \cdots$ for $|x| < 1$ |
| P523 | Root test for $\sum (n/(n+1))^{n^2}$? | $\sqrt[n]{a_n} = (n/(n+1))^n \to 1/e < 1$ → converges |
| P524 | Dirichlet test conditions? | $a_n$ monotone → 0, partial sums of $b_n$ bounded → $\sum a_nb_n$ converges |
| P525 | Absolute convergence of $\sum \frac{\cos n}{n^2}$? | $|\cos n/n^2| \leq 1/n^2$ → converges absolutely |
| P526 | Conditional convergence example? | $\sum (-1)^n/n$ (converges, not absolutely) |
| P527 | Rearrangement of conditionally convergent? | Riemann: can rearrange to converge to ANY value! |
| P528 | $\sum 1/n!$ convergence speed? | Exponential convergence ($n!$ grows very fast) |
| P529 | $e \approx ?$ to 4 decimal places? | $2.7183$ |
| P530 | $\pi \approx ?$ to 4 decimal places? | $3.1416$ |

**P531-P560** — Computational Practice:

**P531.** (2-mark) $\int_0^{\pi/4} \sec^4 x \, dx$?
→ $\sec^4 x = \sec^2 x (1 + \tan^2 x) = \sec^2 x + \sec^2 x \tan^2 x$
→ $= [\tan x]_0^{\pi/4} + [\tan^3 x/3]_0^{\pi/4} = 1 + 1/3 = \boxed{4/3}$

**P532.** (2-mark) $\int_0^1 \frac{dx}{(1+x^2)^{3/2}}$?
→ Sub $x = \tan\theta$: $\int_0^{\pi/4} \cos\theta \, d\theta = \sin(\pi/4) = \frac{1}{\sqrt{2}}$

**P533.** (2-mark) Reduction formula: $I_n = \int_0^{\pi/2} \cos^n x \, dx$.
→ $I_n = \frac{n-1}{n} I_{n-2}$, with $I_0 = \pi/2$, $I_1 = 1$
→ $I_4 = \frac{3}{4} \cdot \frac{1}{2} \cdot \frac{\pi}{2} = \frac{3\pi}{16}$

**P534.** (2-mark) $\int_0^1 x^3 e^{x^2} dx$?
→ Sub $u = x^2$: $\frac{1}{2}\int_0^1 u e^u du = \frac{1}{2}[ue^u - e^u]_0^1 = \frac{1}{2}(e - e + 1) = \frac{1}{2}$

**P535.** (2-mark) $\int_0^{\pi} \frac{x}{1 + \sin x} dx$?
→ King's: $I = \int_0^{\pi} \frac{\pi - x}{1 + \sin(\pi-x)} dx = \int_0^{\pi} \frac{\pi - x}{1 + \sin x} dx$
→ $2I = \pi \int_0^{\pi} \frac{dx}{1 + \sin x}$
→ $\int \frac{dx}{1+\sin x} = \int \frac{1 - \sin x}{\cos^2 x} dx = \tan x - \sec x + C$
→ Limits: $[\tan x - \sec x]_0^{\pi} = (0-(-1)) - (0-1) = 1+1 = 2$
→ $2I = 2\pi$ → $I = \boxed{\pi}$

**P536-P550** — Derivative Applications:

**P536.** (2-mark) Find linear approximation of $f(x) = \sqrt{x}$ near $x = 100$.
→ $f(100) = 10$, $f'(x) = \frac{1}{2\sqrt{x}}$, $f'(100) = 1/20$
→ $L(x) = 10 + \frac{x-100}{20}$
→ $\sqrt{102} \approx 10 + 2/20 = 10.1$ (actual: 10.0995 — excellent!)

**P537.** (2-mark) Related rates: sphere expanding at $dV/dt = 10$ cm³/s. Find $dr/dt$ when $r = 5$.
→ $V = \frac{4}{3}\pi r^3$ → $\frac{dV}{dt} = 4\pi r^2 \frac{dr}{dt}$
→ $10 = 4\pi(25) \frac{dr}{dt}$ → $\frac{dr}{dt} = \frac{10}{100\pi} = \frac{1}{10\pi}$ cm/s

**P538.** (2-mark) Concavity of $f(x) = x^3 - 3x^2 + 4$. Inflection point?
→ $f''(x) = 6x - 6 = 0$ → $x = 1$
→ $f''(x) < 0$ for $x < 1$ (concave down), $f''(x) > 0$ for $x > 1$ (concave up)
→ Inflection at $(1, 2)$

**P539.** (2-mark) L'Hôpital: $\lim_{x \to 0} \frac{e^x - e^{-x} - 2x}{x - \sin x}$
→ $\frac{0}{0}$ → differentiate: $\frac{e^x + e^{-x} - 2}{1 - \cos x}$ → $\frac{0}{0}$ → again: $\frac{e^x - e^{-x}}{\sin x}$ → $\frac{0}{0}$ → $\frac{e^x + e^{-x}}{\cos x} = \frac{2}{1} = \boxed{2}$

**P540.** (2-mark) Asymptotes of $f(x) = \frac{x^2 + 1}{x - 1}$?
→ Vertical: $x = 1$. Oblique: divide: $f = x + 1 + \frac{2}{x-1}$
→ Oblique asymptote: $y = x + 1$

**P541-P550** — Applied Problems:

| # | Problem | Answer |
|---|---|---|
| P541 | Largest rectangle inscribed in semicircle $r$? | Width $r\sqrt{2}$, height $r/\sqrt{2}$, area $r^2$ |
| P542 | Shortest ladder over wall of height $h$, distance $d$ from building? | $L = (h^{2/3} + d^{2/3})^{3/2}$ |
| P543 | Maximum area of triangle inscribed in circle radius $R$? | $\frac{3\sqrt{3}}{4}R^2$ (equilateral triangle) |
| P544 | Minimum surface area of cylinder with volume $V$? | $r = (V/(2\pi))^{1/3}$, $h = 2r$, $A = 3(2\pi V^2)^{1/3}/2^{1/3}$ |
| P545 | Rate of change of area of circle w.r.t. radius? | $dA/dr = 2\pi r$ |
| P546 | Elastic band problem: minimize $f(x) = ax^2 + b/x$? | $x = (b/(2a))^{1/3}$ |
| P547 | Curvature of $y = f(x)$? | $\kappa = \frac{|f''|}{(1+f'^2)^{3/2}}$ |
| P548 | Radius of curvature? | $R = 1/\kappa$ |
| P549 | $\kappa$ of $y = x^2$ at $x = 0$? | $\kappa = 2$ (since $f'' = 2$, $f' = 0$: $R = 1/2$) |
| P550 | Arc length of $y = \cosh x$ from $0$ to $a$? | $\int_0^a \sqrt{1+\sinh^2 x} dx = \int_0^a \cosh x \, dx = \sinh a$ |

**P551-P570** — Multivariable Deep Dive:

**P551.** (2-mark) Implicit function: $F(x,y) = x^2 + y^2 - 2xy = 0$. $dy/dx$?
→ $F_x = 2x - 2y$, $F_y = 2y - 2x$ → $dy/dx = -F_x/F_y = -\frac{2x-2y}{2y-2x} = 1$
→ $y' = 1$ (makes sense: $x^2 + y^2 - 2xy = (x-y)^2 = 0$ → $y = x$!)

**P552.** (2-mark) $f(x,y) = x^2e^y$. Find $f_{xxy}$.
→ $f_x = 2xe^y$, $f_{xx} = 2e^y$, $f_{xxy} = 2e^y$

**P553.** (2-mark) Laplacian of $f = e^x \sin y$?
→ $f_{xx} = e^x \sin y$, $f_{yy} = -e^x \sin y$
→ $\nabla^2 f = e^x \sin y - e^x \sin y = 0$ → **Harmonic!** ✅

**P554.** (2-mark) Constrained optimization with 2 constraints:
Minimize $f = x + y + z$ subject to $x^2 + y^2 = 2$ and $y + z = 0$.
→ From $z = -y$: $f = x + y - y = x$. Minimize $x$ s.t. $x^2 + y^2 = 2$.
→ Min $x$ on circle: $x = -\sqrt{2}$ → Min $f = -\sqrt{2}$

**P555.** (2-mark) Envelope of family of circles $(x - a)^2 + y^2 = 1$ (parameterized by $a$)?
→ $F = (x-a)^2 + y^2 - 1$, $F_a = -2(x-a) = 0$ → $x = a$
→ Substitute: $y^2 = 1$ → $y = \pm 1$
→ Envelope: lines $y = 1$ and $y = -1$

**P556-P570** — Optimization for ML:

| # | Q | A |
|---|---|---|
| P556 | Loss function $L = (y - \hat{y})^2$: gradient w.r.t. $\hat{y}$? | $-2(y - \hat{y})$ |
| P557 | Cross-entropy loss: $L = -y\log p - (1-y)\log(1-p)$, $\partial L/\partial p$? | $\frac{-y}{p} + \frac{1-y}{1-p} = \frac{p-y}{p(1-p)}$ |
| P558 | Sigmoid: $\sigma(x) = 1/(1+e^{-x})$. $\sigma'(x) = ?$ | $\sigma(x)(1-\sigma(x))$ |
| P559 | Softmax gradient? | $\frac{\partial p_i}{\partial z_j} = p_i(\delta_{ij} - p_j)$ |
| P560 | Hessian of logistic loss? | $X^T \text{diag}(p(1-p)) X$ → PSD → convex! |
| P561 | L2 regularized least squares: closed form? | $(X^TX + \lambda I)^{-1}X^Ty$ |
| P562 | Does L1 have closed form? | NO (non-smooth; use iterative methods) |
| P563 | Elastic net combines? | L1 + L2: $\lambda_1\|w\|_1 + \lambda_2\|w\|_2^2$ |
| P564 | Dual of ridge regression? | $\alpha = (XX^T + \lambda I)^{-1}y$, $w = X^T\alpha$ |
| P565 | Gradient of $\text{tr}(W^TW)$ w.r.t. $W$? | $2W$ |
| P566 | Matrix calculus: $\frac{d}{dw}(w^TXw)$? | $(X + X^T)w$ |
| P567 | Convergence rate: GD on strongly convex, L-smooth? | $O\left(\left(\frac{L-m}{L+m}\right)^k\right)$ where $m$ = strong convexity |
| P568 | Nesterov accelerated GD rate? | $O\left(\left(\frac{\sqrt{L}-\sqrt{m}}{\sqrt{L}+\sqrt{m}}\right)^k\right)$ — optimal! |
| P569 | SGD convergence rate? | $O(1/\sqrt{T})$ for convex, $O(1/T)$ for strongly convex |
| P570 | Mini-batch reduces variance by factor? | $1/B$ where $B$ = batch size |

**P571-P600** — Final Express:

| # | Q | A |
|---|---|---|
| P571 | $\frac{d}{dx}[\Gamma(x)]$ for general $x$? | $\Gamma(x)\psi(x)$ where $\psi$ = digamma function |
| P572 | Duplication formula: $\Gamma(s)\Gamma(s+1/2) = ?$ | $\frac{\sqrt{\pi}}{2^{2s-1}}\Gamma(2s)$ |
| P573 | $B(1/2, 1/2) = ?$ | $\frac{\Gamma(1/2)^2}{\Gamma(1)} = \pi$ |
| P574 | $\int_0^{\infty} \frac{x^{s-1}}{e^x - 1} dx$ = ? | $\Gamma(s)\zeta(s)$ (Riemann zeta) |
| P575 | Fourier: $\hat{f}(\omega) = \int f(x)e^{-i\omega x} dx$? | Fourier transform (used in signal processing) |
| P576 | Convolution theorem? | $\widehat{f * g} = \hat{f} \cdot \hat{g}$ |
| P577 | Heat equation: $u_t = k u_{xx}$? | Parabolic PDE, solved via separation of variables |
| P578 | Wave equation: $u_{tt} = c^2 u_{xx}$? | Hyperbolic PDE, D'Alembert's solution |
| P579 | Laplace equation: $u_{xx} + u_{yy} = 0$? | Elliptic PDE, harmonic functions |
| P580 | Method of characteristics for? | First-order PDEs |
| P581 | Green's function? | Impulse response of linear differential operator |
| P582 | Cauchy-Schwarz for integrals? | $\left(\int fg\right)^2 \leq \int f^2 \int g^2$ |
| P583 | Hölder's inequality? | $\int |fg| \leq \|f\|_p \|g\|_q$ where $1/p + 1/q = 1$ |
| P584 | Minkowski inequality? | $\|f+g\|_p \leq \|f\|_p + \|g\|_p$ (triangle ineq for $L^p$) |
| P585 | Dominated convergence theorem? | Swap limit and integral if dominated by integrable function |
| P586 | Monotone convergence theorem? | $f_n \uparrow f$ → $\int f_n \uparrow \int f$ |
| P587 | Fatou's lemma? | $\int \liminf f_n \leq \liminf \int f_n$ |
| P588 | Fubini's theorem? | Swap order of double integral if $\int |f| < \infty$ |
| P589 | Leibniz integral rule? | $\frac{d}{dx}\int_{a(x)}^{b(x)} f(t,x) dt = f(b,x)b' - f(a,x)a' + \int_a^b f_x dt$ |
| P590 | Is $f(x) = x\log x$ convex on $(0,\infty)$? | YES ($f'' = 1/x > 0$) — used in KL divergence |
| P591 | Bregman divergence for $\phi(x) = x^2$? | $D_\phi(x,y) = (x-y)^2$ — squared loss |
| P592 | Bregman for $\phi(x) = x\log x$? | $D_\phi(x,y) = x\log(x/y) - x + y$ — KL divergence! |
| P593 | Mirror descent generalizes GD using? | Bregman divergence instead of Euclidean |
| P594 | Frank-Wolfe algorithm for? | Constrained optimization (projection-free) |
| P595 | Coordinate descent: update rule? | Minimize one variable at a time, holding others fixed |
| P596 | Expectation-Maximization relates to? | Variational lower bound optimization |
| P597 | Is log-barrier $-\log(x)$ self-concordant? | YES (key property for interior point) |
| P598 | Central path: $x^*(t)$ minimizes $tf(x) - \sum \log(-g_i(x))$? | Traces path to optimal as $t \to \infty$ |
| P599 | Duality gap bound for barrier method? | $m/t$ where $m$ = number of constraints |
| P600 | Total iterations for barrier to reach $\epsilon$-optimal? | $O(\sqrt{m}\log(m/\epsilon))$ |

**P601-P620** — Last Mile Questions:

| # | Q | A |
|---|---|---|
| P601 | $\int_0^1 \frac{1-x}{-\ln x} dx$? | $\ln 2$ (Frullani integral) |
| P602 | Harmonic mean of $a, b$? | $\frac{2ab}{a+b}$ |
| P603 | AM-GM-HM ordering? | $\text{AM} \geq \text{GM} \geq \text{HM}$ |
| P604 | Power mean with $p \to 0$? | Geometric mean |
| P605 | Power mean with $p = -1$? | Harmonic mean |
| P606 | Young's inequality: $ab \leq ?$ | $\frac{a^p}{p} + \frac{b^q}{q}$ for $1/p + 1/q = 1$ |
| P607 | Is $f(x) = -\sqrt{x}$ convex on $x > 0$? | YES ($f'' = \frac{1}{4x^{3/2}} > 0$)... wait: $f'=-1/(2\sqrt{x})$, $f''=1/(4x^{3/2})>0$ → YES convex |
| P608 | Is $f(x) = \sqrt{x}$ concave? | YES ($f'' = -1/(4x^{3/2}) < 0$) |
| P609 | Perspective: $g(x,t) = tf(x/t)$ convex if $f$ convex? | YES |
| P610 | Loewner order: $A \succeq B$ means? | $A - B$ positive semidefinite |
| P611 | Schur complement: $M = \begin{pmatrix}A & B\\B^T & C\end{pmatrix} \succeq 0$ iff? | $A \succeq 0$, $C - B^TA^{-1}B \succeq 0$ (or equiv for $C$) |
| P612 | Semidefinite programming (SDP)? | Optimize over positive semidefinite matrices |
| P613 | Second-order cone program (SOCP)? | Optimize with $\|Ax+b\| \leq c^Tx+d$ constraints |
| P614 | Danskin's theorem? | $\frac{d}{dx}\max_z f(x,z) = \frac{\partial f}{\partial x}(x, z^*(x))$ |
| P615 | When is max of linear functions differentiable? | At points where unique maximizer (almost everywhere) |
| P616 | Subdifferential of $\|x\|_1$ at $x = 0$? | $[-1, 1]^n$ (unit cube) |
| P617 | Proximal operator of indicator function $I_C$? | Projection onto $C$! |
| P618 | ADMM convergence rate? | $O(1/k)$ for convex |
| P619 | Stochastic variance reduction (SVRG)? | Periodically compute full gradient, reduces variance |
| P620 | Adam: $\beta_1, \beta_2$ typical values? | $\beta_1 = 0.9$, $\beta_2 = 0.999$ |

---

## 📝 Practice Set 11: Convergence & Series Championship (P621 – P700)

**P621.** (2-mark) Test convergence: $\sum_{n=1}^{\infty} \frac{n!}{n^n}$
→ Ratio test: $\frac{a_{n+1}}{a_n} = \frac{(n+1)!}{(n+1)^{n+1}} \cdot \frac{n^n}{n!} = \frac{n^n}{(n+1)^n} = \left(\frac{n}{n+1}\right)^n \to 1/e < 1$
→ **Converges** ✅

**P622.** (2-mark) Test: $\sum_{n=2}^{\infty} \frac{1}{n(\ln n)^2}$
→ Cauchy condensation: $\sum 2^n \cdot \frac{1}{2^n (n\ln 2)^2} = \frac{1}{(\ln 2)^2}\sum \frac{1}{n^2}$ → converges.
→ Or integral test: $\int_2^{\infty} \frac{dx}{x(\ln x)^2} = [-1/\ln x]_2^{\infty} = 1/\ln 2$ → **Converges** ✅

**P623.** (2-mark) $\sum_{n=1}^{\infty} \frac{1}{n(\ln n)(\ln\ln n)^2}$ for $n \geq 3$? 
→ Integral test: let $u = \ln\ln x$, $du = 1/(x\ln x) dx$ → $\int \frac{du}{u^2}$ converges.
→ **Converges** ✅

**P624.** (2-mark) Radius of convergence: $\sum_{n=0}^{\infty} \frac{n!}{n^n} x^n$
→ From P621: ratio → $1/e \cdot |x|$. Converges when $|x|/e < 1$ → $R = e$

**P625.** (2-mark) ROC: $\sum \frac{x^n}{n^n}$
→ Root test: $\sqrt[n]{1/n^n} = 1/n \to 0$ → $R = \infty$ (converges for all $x$!)

**P626.** (2-mark) ROC: $\sum n! x^n$
→ $|a_{n+1}/a_n| = (n+1)|x| \to \infty$ for any $x \neq 0$ → $R = 0$

**P627.** (2-mark) Abel's theorem: If $\sum a_n$ converges, then $\lim_{x \to 1^-} \sum a_n x^n = ?$
→ $= \sum a_n$ (continuity at boundary of convergence)
→ Example: $\sum (-1)^{n+1}/n = \ln 2$ → $\lim_{x \to 1^-} \sum (-1)^{n+1}x^n/n = -\ln(1-x) \to \ln 2$ ✅

**P628.** (2-mark) Weierstrass M-test: $\sum f_n(x)$ converges uniformly if $|f_n(x)| \leq M_n$ and $\sum M_n < \infty$.
→ Example: $\sum \frac{\sin(nx)}{n^2}$: $|\sin(nx)/n^2| \leq 1/n^2$, $\sum 1/n^2 < \infty$ → uniformly convergent ✅

**P629-P640** — Integration Technique Mastery:

| # | Integral | Technique & Answer |
|---|---|---|
| P629 | $\int \frac{dx}{x^4+1}$ | Partial fractions after factoring $x^4+1 = (x^2+\sqrt{2}x+1)(x^2-\sqrt{2}x+1)$ |
| P630 | $\int_0^1 \frac{\ln x}{1-x} dx$ | $= -\sum \int_0^1 x^{n-1}\ln x \, dx = -\sum \frac{-1}{n^2} = -\pi^2/6$ |
| P631 | $\int_0^{\infty} \frac{\sin x}{x} dx$ | $= \pi/2$ (Dirichlet integral, via Laplace/Feynman) |
| P632 | $\int_0^1 \frac{\ln(1+x)}{x} dx$ | $= \sum_{n=1}^{\infty} \frac{(-1)^{n+1}}{n^2} = \frac{\pi^2}{12}$ |
| P633 | $\int_0^{\pi/2} \ln(\sin x) dx$ | $= -\frac{\pi}{2}\ln 2$ |
| P634 | $\int_0^{\infty} e^{-x^2} x^{2n} dx$ | $= \frac{(2n)!\sqrt{\pi}}{n! \cdot 4^n \cdot 2}$ |
| P635 | $\int_0^1 (-\ln x)^n dx$ | $= n!$ (sub $x = e^{-t}$) |
| P636 | $\int_0^{\infty} \frac{dx}{(1+x^2)^n}$ | $= \frac{\pi(2n-2)!}{4^{n-1}((n-1)!)^2}$ |
| P637 | $\int_0^{\pi} \frac{d\theta}{a + b\cos\theta}$ for $a > b > 0$? | $= \frac{\pi}{\sqrt{a^2-b^2}}$ |
| P638 | $\int_{-\infty}^{\infty} \frac{dx}{(x^2+a^2)(x^2+b^2)}$ | $= \frac{\pi}{ab(a+b)}$ |
| P639 | $\int_0^1 x^a(1-x)^b dx$? | $= B(a+1, b+1) = \frac{\Gamma(a+1)\Gamma(b+1)}{\Gamma(a+b+2)}$ |
| P640 | $\int_0^{\pi/2} \sin^p x \cos^q x \, dx$? | $= \frac{1}{2}B\left(\frac{p+1}{2}, \frac{q+1}{2}\right)$ |

**P641-P660** — ODE Advanced Problems:

**P641.** (2-mark) Solve $y'' + y = \sec x$ (variation of parameters)
→ Complementary: $y_c = c_1\cos x + c_2\sin x$
→ Wronskian: $W = \cos x \cdot \cos x - \sin x(-\sin x) = 1$
→ $u_1 = -\int \frac{\sin x \sec x}{1} dx = -\int \tan x \, dx = \ln|\cos x|$
→ $u_2 = \int \frac{\cos x \sec x}{1} dx = \int dx = x$
→ $y_p = \cos x \ln|\cos x| + x\sin x$ ✅

**P642.** (2-mark) Solve $y'' - 2y' + y = \frac{e^x}{x}$
→ $y_c = (c_1 + c_2 x)e^x$. $W = e^{2x}$.
→ $u_1 = -\int \frac{xe^x \cdot e^x/x}{e^{2x}} dx = -\int dx = -x$
→ $u_2 = \int \frac{e^x \cdot e^x/x}{e^{2x}} dx = \int \frac{1}{x} dx = \ln|x|$
→ $y_p = -xe^x + xe^x\ln|x| = xe^x(\ln|x| - 1)$

**P643.** (2-mark) Classify ODE: $M(x,y)dx + N(x,y)dy = 0$
→ Exact if $\frac{\partial M}{\partial y} = \frac{\partial N}{\partial x}$
→ If not exact, check: $\frac{M_y - N_x}{N}$ depends only on $x$ → IF $\mu(x) = e^{\int \frac{M_y-N_x}{N}dx}$
→ $\frac{N_x - M_y}{M}$ depends only on $y$ → IF $\mu(y) = e^{\int \frac{N_x-M_y}{M}dy}$

**P644.** (2-mark) Solve Euler-Cauchy: $x^2y'' - 2xy' + 2y = 0$
→ Try $y = x^m$: $m(m-1) - 2m + 2 = 0$ → $m^2 - 3m + 2 = 0$ → $m = 1, 2$
→ $y = c_1 x + c_2 x^2$

**P645.** (2-mark) Repeated roots in Euler-Cauchy: $x^2y'' - xy' + y = 0$?
→ $m^2 - 2m + 1 = 0$ → $m = 1$ (double)
→ $y = c_1 x + c_2 x \ln x$

**P646.** (2-mark) Complex roots: $x^2y'' + xy' + y = 0$?
→ $m^2 + 0m + 1 = 0$ → $m = \pm i$
→ $y = c_1 \cos(\ln x) + c_2 \sin(\ln x)$

**P647.** (2-mark) Solve $y' + y\tan x = \cos x$, $y(0) = 1$.
→ IF: $e^{\int \tan x \, dx} = e^{-\ln\cos x} = \sec x$
→ $(y\sec x)' = \sec x \cos x = 1$ → $y\sec x = x + C$
→ $y = (x+C)\cos x$. IC: $1 = C \cdot 1$ → $C = 1$
→ $y = (x+1)\cos x$

**P648-P660** — Systems & Stability:

| # | Problem | Answer |
|---|---|---|
| P648 | Equilibrium of $\dot{x}=x(1-x)$? | $x^*=0$ (unstable), $x^*=1$ (stable) |
| P649 | Logistic with harvesting: $\dot{x}=x(1-x)-h$, bifurcation at? | $h = 1/4$ (saddle-node bifurcation) |
| P650 | Predator-prey: Lotka-Volterra fixed points? | $(0,0)$ saddle; $(K/\alpha, r/\beta)$ center |
| P651 | Phase portrait of $\dot{x}=y, \dot{y}=-x$? | Circle (center) — $x^2+y^2 = C$ |
| P652 | $\dot{x}=y, \dot{y}=-x-\epsilon y$ ($\epsilon > 0$)? | Stable spiral — damped oscillation |
| P653 | Jacobian eigenvalues: $\lambda = a \pm bi$, $a<0$? | Stable spiral |
| P654 | $a > 0$? | Unstable spiral |
| P655 | $a = 0, b \neq 0$? | Center (neutrally stable, Lyapunov stable) |
| P656 | Real eigenvalues both negative? | Stable node |
| P657 | Eigenvalues opposite sign? | Saddle point (unstable) |
| P658 | Lyapunov function for $\dot{x}=-x^3$? | $V(x) = x^2/2$: $\dot{V} = x(-x^3) = -x^4 \leq 0$ ✅ |
| P659 | LaSalle's invariance principle? | $\dot{V} \leq 0$ + largest invariant set where $\dot{V}=0$ determines $\omega$-limit |
| P660 | Bendixson's criterion: no limit cycle if? | $\text{div}(f,g) = f_x + g_y$ doesn't change sign |

---

## 📝 Practice Set 12: Numerical Methods & Approximation (P661 – P740)

**P661.** (2-mark) Newton-Raphson: Find $\sqrt{5}$ starting from $x_0 = 2$.
→ $f(x) = x^2 - 5$, $f'(x) = 2x$. $x_{n+1} = x_n - \frac{x_n^2-5}{2x_n} = \frac{x_n + 5/x_n}{2}$
→ $x_1 = (2 + 2.5)/2 = 2.25$, $x_2 = (2.25 + 5/2.25)/2 = (2.25 + 2.222)/2 = 2.236$
→ $\sqrt{5} \approx 2.2361$ — converged in 2 iterations! (Quadratic convergence)

**P662.** (2-mark) Bisection method: convergence rate?
→ Linear: error halves each step → after $n$ steps: $|e_n| \leq \frac{b-a}{2^n}$
→ For $\epsilon = 10^{-6}$, $[a,b] = [0,1]$: $n \geq 20$ iterations

**P663.** (2-mark) Secant method: convergence order?
→ Superlinear: order $\phi = (1+\sqrt{5})/2 \approx 1.618$ (golden ratio!)

**P664.** (2-mark) Fixed point iteration $x_{n+1} = g(x_n)$ converges if?
→ $|g'(x^*)| < 1$ at fixed point $x^* = g(x^*)$ (contraction mapping)

**P665.** (2-mark) Trapezoidal rule: $\int_a^b f \approx \frac{h}{2}(f_0 + 2f_1 + \cdots + 2f_{n-1} + f_n)$
→ Error: $O(h^2)$ — second order. Exact for linear functions.

**P666.** (2-mark) Simpson's 1/3 rule: $\int_a^b f \approx \frac{h}{3}(f_0 + 4f_1 + 2f_2 + 4f_3 + \cdots + f_n)$
→ Error: $O(h^4)$ — fourth order. Exact for cubics!

**P667.** (2-mark) Simpson's 3/8 rule?
→ Uses 3 subintervals: $\frac{3h}{8}(f_0 + 3f_1 + 3f_2 + f_3)$
→ Error: $O(h^4)$ same as 1/3 rule

**P668-P680** — Numerical ODE:

| # | Method | Order | Formula | Stability |
|---|---|---|---|---|
| P668 | Euler forward | 1 | $y_{n+1} = y_n + hf_n$ | Conditionally stable |
| P669 | Euler backward | 1 | $y_{n+1} = y_n + hf_{n+1}$ (implicit) | Unconditionally stable |
| P670 | Heun's (improved Euler) | 2 | Predictor-corrector | Better stability |
| P671 | RK2 (midpoint) | 2 | $k_1=hf_n$, $k_2=hf(t+h/2, y+k_1/2)$, $y_{n+1}=y_n+k_2$ | |
| P672 | RK4 (classical) | 4 | $y_{n+1} = y_n + (k_1+2k_2+2k_3+k_4)/6$ | Most popular |
| P673 | $k_1$ in RK4? | | $k_1 = hf(t_n, y_n)$ | |
| P674 | $k_2$ in RK4? | | $k_2 = hf(t_n+h/2, y_n+k_1/2)$ | |
| P675 | $k_3$ in RK4? | | $k_3 = hf(t_n+h/2, y_n+k_2/2)$ | |
| P676 | $k_4$ in RK4? | | $k_4 = hf(t_n+h, y_n+k_3)$ | |
| P677 | Adams-Bashforth 2-step? | 2 | $y_{n+1} = y_n + h(3f_n - f_{n-1})/2$ | Multi-step, explicit |
| P678 | Adams-Moulton 2-step? | 3 | $y_{n+1} = y_n + h(5f_{n+1}+8f_n-f_{n-1})/12$ | Implicit |
| P679 | Stiff equations need? | | Implicit methods (backward Euler, BDF) | |
| P680 | Local truncation error of Euler? | | $O(h^2)$ per step, $O(h)$ global | |

**P681-P700** — Interpolation & Approximation:

**P681.** (2-mark) Lagrange interpolation for $(0,1), (1,0), (2,1)$:
→ $L_0 = \frac{(x-1)(x-2)}{(0-1)(0-2)} = \frac{(x-1)(x-2)}{2}$
→ $L_1 = \frac{(x-0)(x-2)}{(1-0)(1-2)} = \frac{-x(x-2)}{1} = -x(x-2)$
→ $L_2 = \frac{(x-0)(x-1)}{(2-0)(2-1)} = \frac{x(x-1)}{2}$
→ $P(x) = 1 \cdot L_0 + 0 \cdot L_1 + 1 \cdot L_2 = \frac{(x-1)(x-2)}{2} + \frac{x(x-1)}{2} = \frac{(x-1)(x-2+x)}{2} = \frac{(x-1)(2x-2)}{2} = (x-1)^2$

**P682.** (2-mark) Newton's divided difference for same data:
→ $f[0] = 1$, $f[1] = 0$, $f[2] = 1$
→ $f[0,1] = (0-1)/(1-0) = -1$, $f[1,2] = (1-0)/(2-1) = 1$
→ $f[0,1,2] = (1-(-1))/(2-0) = 1$
→ $P(x) = 1 + (-1)(x-0) + 1(x-0)(x-1) = 1 - x + x^2 - x = x^2 - 2x + 1 = (x-1)^2$ ✅

**P683-P700** — Quick Numerical Problems:

| # | Q | A |
|---|---|---|
| P683 | Runge phenomenon occurs with? | High-degree equispaced polynomial interpolation |
| P684 | Cure for Runge? | Chebyshev nodes: $x_k = \cos\frac{(2k+1)\pi}{2(n+1)}$ |
| P685 | Spline: cubic spline uses? | Piecewise cubics with $C^2$ continuity at knots |
| P686 | Natural cubic spline boundary? | $S''(a) = S''(b) = 0$ |
| P687 | Clamped cubic spline boundary? | $S'(a) = f'(a)$, $S'(b) = f'(b)$ |
| P688 | Hermite interpolation matches? | Function values AND derivative values |
| P689 | Gaussian quadrature: $n$-point integrates exactly degree? | $\leq 2n-1$ polynomials |
| P690 | 2-point Gauss: nodes? | $\pm 1/\sqrt{3}$ on $[-1,1]$, weights $= 1$ each |
| P691 | 3-point Gauss: nodes? | $0, \pm\sqrt{3/5}$; weights $8/9, 5/9, 5/9$ |
| P692 | Richardson extrapolation? | Combine low-order estimates to get higher order |
| P693 | Romberg integration uses? | Repeated Richardson on trapezoidal rule |
| P694 | Finite difference: $f'(x) \approx ?$ (central) | $\frac{f(x+h)-f(x-h)}{2h}$, error $O(h^2)$ |
| P695 | $f''(x) \approx ?$ (central) | $\frac{f(x+h)-2f(x)+f(x-h)}{h^2}$, error $O(h^2)$ |
| P696 | Condition number of matrix? | $\kappa(A) = \|A\| \cdot \|A^{-1}\|$; large = ill-conditioned |
| P697 | LU decomposition: $A = LU$? | $L$ lower triangular, $U$ upper triangular; $O(n^3/3)$ |
| P698 | Cholesky: $A = LL^T$ requires? | $A$ symmetric positive definite; $O(n^3/6)$ |
| P699 | Jacobi iteration convergence? | Sufficient: $A$ strictly diag dominant |
| P700 | Gauss-Seidel vs Jacobi? | GS uses updated values immediately → faster convergence |

---

## 🎯 Commonly Confused Pairs — Calculus Edition

| Concept A | Concept B | Key Difference |
|---|---|---|
| Continuous | Differentiable | Differentiable ⊂ Continuous (not reverse) |
| Differentiable | Partial derivatives exist | In multivar: partials can exist without differentiability |
| Pointwise convergence | Uniform convergence | Uniform: $\sup_x |f_n - f| \to 0$ |
| Absolute convergence | Conditional convergence | Absolute implies convergence; conditional = converges but not absolutely |
| Improper integral converges | Integral exists | Converges = finite limit; "exists" sometimes ambiguous |
| Local minimum | Global minimum | Local: best in neighborhood; Global: best overall |
| Saddle point (optimization) | Saddle point (ODE) | Optimization: $\nabla f = 0$, indefinite Hessian. ODE: eigenvalues opposite sign |
| Convex function | Convex set | Function: $f(\lambda x + (1-\lambda)y) \leq \lambda f(x) + (1-\lambda)f(y)$; Set: $\lambda x + (1-\lambda)y \in S$ |
| Strong convexity | Strict convexity | Strong: $f - \frac{m}{2}\|x\|^2$ convex; Strict: strict inequality (no guarantee on rate) |
| $O(h^2)$ local error | $O(h)$ global error | Global = local accumulated over $n = (b-a)/h$ steps |
| Explicit method | Implicit method | Explicit: $y_{n+1}$ from known; Implicit: need to solve equation |
| Rolle's theorem | MVT | Rolle: $f(a)=f(b)$ required; MVT: general |
| Taylor polynomial | Taylor series | Polynomial: finite $n$ terms; Series: infinite terms |
| Maclaurin | Taylor | Maclaurin = Taylor centered at $a = 0$ |
| Line integral | Surface integral | Line: 1D curve; Surface: 2D surface |
| Green's theorem | Stokes' theorem | Green's: 2D special case of Stokes' in 3D |
| Stokes' | Gauss/Divergence | Stokes': curl over surface = line integral; Gauss: div over volume = surface integral |

---

## 📊 GATE Score Maximization Strategy for Calculus

### Mark Distribution Analysis (GATE CS + DA)
| Topic | Expected Marks | Difficulty | Time (min) |
|---|---|---|---|
| Limits & Continuity | 1-2 | Easy-Medium | 2-3 |
| Derivatives & MVT | 1-2 | Medium | 2-4 |
| Integration (definite/indefinite) | 2-4 | Medium-Hard | 3-5 |
| Taylor/Maclaurin Series | 1-2 | Medium | 2-3 |
| Multivariable Calculus | 2-4 | Medium-Hard | 3-5 |
| Optimization (Lagrange/KKT) | 2-4 | Hard | 4-6 |
| ODEs | 1-2 | Medium | 2-4 |
| **Total Expected** | **10-20** | — | — |

### Strategy Tips:
1. **Always attempt 1-mark calculus questions** — usually straightforward limits/derivatives
2. **Taylor series questions** are high-reward: memorize top 10 expansions
3. **Lagrange multipliers** appear every year (CS + DA both) — practice the mechanical steps
4. **Integration**: King's property + Beta/Gamma are the two biggest time-savers
5. **ODEs**: Linear first-order (IF method) is guaranteed marks — never leave it
6. **DA-specific**: KKT, convexity, gradient descent show up heavily

### 🔥 Last 30 Minutes Revision Order:
1. Emergency formula card (Section above)
2. Taylor expansions of 10 common functions
3. Lagrange multiplier 3-step recipe
4. Integration tricks: King's, Wallis, Beta-Gamma connection
5. Convergence test decision tree
6. ODE solution flowchart

---

## 📋 Complete Convergence Test Decision Tree

```
Given ∑ aₙ:
│
├─ aₙ → 0? ──NO──→ DIVERGES (nth term test)
│    │
│    YES
│    │
├─ Geometric series? (aₙ = arⁿ) ──YES──→ |r|<1: S=a/(1-r); |r|≥1: diverges
│    │
│    NO
│    │
├─ p-series? (1/nᵖ) ──YES──→ p>1: converges; p≤1: diverges
│    │
│    NO
│    │
├─ Alternating? ──YES──→ |aₙ| decreasing → 0? YES: converges (Leibniz)
│    │
│    NO (or want absolute)
│    │
├─ Ratio test: L = lim |aₙ₊₁/aₙ|
│    ├─ L < 1: converges absolutely
│    ├─ L > 1: diverges
│    └─ L = 1: inconclusive → try others
│    │
├─ Root test: L = lim |aₙ|^(1/n)  [same thresholds as ratio]
│    │
├─ Comparison: find bₙ where aₙ ≤ bₙ (conv) or aₙ ≥ bₙ (div)
│    │
├─ Limit comparison: lim aₙ/bₙ = c > 0 → same behavior
│    │
├─ Integral test: ∫₁^∞ f(x) dx and ∑ f(n) same convergence
│    │
└─ Condensation test: ∑ 2ⁿ a₂ₙ same convergence as ∑ aₙ (for decreasing aₙ)
```

---

## 📋 ODE Solution Flowchart

```
Given ODE:
│
├─ First order?
│    ├─ Separable: g(y)dy = f(x)dx → integrate both sides
│    ├─ Linear: y' + P(x)y = Q(x) → IF = e^{∫P dx}, y·IF = ∫Q·IF dx
│    ├─ Exact: M dx + N dy = 0, M_y = N_x → ∫M dx + ∫(terms of N not in ∂F/∂y) dy = C
│    ├─ Bernoulli: y' + Py = Qyⁿ → sub v = y^{1-n} → linear
│    ├─ Homogeneous: y' = f(y/x) → sub v = y/x → separable
│    └─ Riccati: y' = P + Qy + Ry² → need one particular solution
│
├─ Second order linear constant coefficients: ay'' + by' + cy = g(x)?
│    ├─ Homogeneous (g=0):
│    │    ├─ Char eq: am² + bm + c = 0
│    │    ├─ Distinct real roots m₁, m₂ → y = c₁e^{m₁x} + c₂e^{m₂x}
│    │    ├─ Repeated root m → y = (c₁ + c₂x)e^{mx}
│    │    └─ Complex α ± βi → y = e^{αx}(c₁cos βx + c₂sin βx)
│    └─ Non-homogeneous (g≠0):
│         ├─ Undetermined coefficients (for poly, exp, trig, products)
│         └─ Variation of parameters (general method, always works)
│
└─ Euler-Cauchy: x²y'' + bxy' + cy = 0 → try y = xᵐ → quadratic in m
```

---

## 🏅 Final Mastery Checklist (Self-Assessment)

Rate yourself 1-5 on each topic:

| Topic | Rating | Action if < 3 |
|---|---|---|
| Limits (L'Hôpital, Taylor, squeeze) | __ /5 | Review Ch 1, Practice P1-P30 |
| Continuity & Differentiability | __ /5 | Review Ch 2-3, P506-P509 |
| Mean Value Theorems | __ /5 | Review Ch 4, GATE PYQ section |
| Integration techniques | __ /5 | Review Ch 5, Practice P529-P550, P629-P640 |
| Taylor & Maclaurin series | __ /5 | Review Ch 6, Formula sheet |
| Partial differentiation | __ /5 | Review Ch 7-8, P551-P555 |
| Gradient, directional derivative | __ /5 | Review Ch 8-9 |
| Matrix calculus | __ /5 | Review Ch 10-11 |
| Unconstrained optimization | __ /5 | Review Ch 12-13 |
| Gradient descent & variants | __ /5 | Review Ch 14, P556-P570 |
| Lagrange multipliers | __ /5 | Review Ch 15, Practice manually |
| KKT conditions | __ /5 | Review Ch 16, P554 |
| Double/multiple integrals | __ /5 | Review Ch 17 |
| ODEs (1st & 2nd order) | __ /5 | Review Ch 18, P641-P660, Flowchart |
| Series convergence tests | __ /5 | Decision tree + P621-P628 |
| Numerical methods | __ /5 | P661-P700 |
| Vector calculus (Green/Stokes/Gauss) | __ /5 | Practice P281-P360 |
| Convex optimization | __ /5 | Ch 13-16, P590-P620 |

---

## 🎪 Revision Roadmap — 4-Week Plan

### Week 1: Foundations (Days 1-7)
- Day 1-2: Limits, continuity, differentiability (Ch 1-3) + 50 problems
- Day 3-4: MVT, integration (Ch 4-5) + 50 problems  
- Day 5-6: Taylor series, partial derivatives (Ch 6-8) + 40 problems
- Day 7: Practice Set 1-2 (P1-P130), review mistakes

### Week 2: Multivariable & Optimization (Days 8-14)
- Day 8-9: Gradient, directional derivatives, matrix calculus (Ch 8-11)
- Day 10-11: Optimization — unconstrained, convexity (Ch 12-14)
- Day 12-13: Lagrange, KKT (Ch 15-16) + 60 problems
- Day 14: Practice Set 3-4 (P131-P280), timed GATE simulation

### Week 3: ODEs, Integrals & Advanced (Days 15-21)
- Day 15-16: Double integrals, change of order, polar (Ch 17)
- Day 17-18: ODEs — all types (Ch 18) + flowchart practice
- Day 19-20: Numerical methods overview, series convergence
- Day 21: Practice Sets 5-8 (P251-P400), full mock test

### Week 4: Mastery & Speed (Days 22-28)
- Day 22-23: Practice Sets 9-10 (P401-P620), identify weak areas
- Day 24-25: GATE PYQs (all years), timed practice
- Day 26-27: Emergency formulas + rapid recall review, speed drills
- Day 28: Full simulation exam, final review of traps & confused pairs

---

## 📝 Practice Set 13: GATE Predicted Questions 2025-2027 (P701 – P800)

**P701.** (1-mark) $\lim_{x \to 0} \frac{\tan x - \sin x}{x^3} = ?$
→ $\tan x - \sin x = \sin x(\sec x - 1) = \sin x \cdot \frac{1 - \cos x}{\cos x}$
→ $\approx x \cdot \frac{x^2/2}{1} = x^3/2$ → Answer: $\boxed{1/2}$

**P702.** (1-mark) $f(x) = |x^3|$ at $x = 0$: differentiable?
→ $f(x) = x^3$ for $x \geq 0$, $-x^3$ for $x < 0$
→ $f'(0^+) = 0$, $f'(0^-) = 0$ → YES, differentiable. $f'(0) = 0$

**P703.** (2-mark NAT) $\int_0^{\pi/2} \frac{\sin^3 x}{\sin^3 x + \cos^3 x} dx = ?$
→ King's: $I = \int_0^{\pi/2} \frac{\cos^3 x}{\cos^3 x + \sin^3 x} dx$
→ $2I = \int_0^{\pi/2} 1 \, dx = \pi/2$ → $I = \boxed{\pi/4}$

**P704.** (2-mark) Find min of $f(x,y) = x^2 + y^2$ subject to $x + y = 4$.
→ Lagrange: $\nabla f = \lambda \nabla g$: $2x = \lambda$, $2y = \lambda$ → $x = y$
→ $x + y = 4 \Rightarrow x = y = 2$. $f = 4 + 4 = \boxed{8}$

**P705.** (2-mark) Solve: $y' + 2xy = x$, $y(0) = 0$.
→ IF: $e^{x^2}$. $(ye^{x^2})' = xe^{x^2}$
→ $ye^{x^2} = \frac{1}{2}e^{x^2} + C$ → $y = \frac{1}{2} + Ce^{-x^2}$
→ IC: $0 = 1/2 + C$ → $C = -1/2$ → $y = \frac{1}{2}(1 - e^{-x^2})$

**P706.** (2-mark) Jacobian of transformation $u = x + y$, $v = x - y$?
→ $\frac{\partial(u,v)}{\partial(x,y)} = \begin{vmatrix} 1 & 1 \\ 1 & -1 \end{vmatrix} = -2$
→ $|J| = 2$, so $dx\,dy = \frac{1}{2}du\,dv$

**P707.** (2-mark) Area using double integral: region bounded by $y = x^2$ and $y = x$.
→ Intersection: $x^2 = x$ → $x = 0, 1$
→ $A = \int_0^1 \int_{x^2}^{x} dy\,dx = \int_0^1 (x - x^2) dx = [x^2/2 - x^3/3]_0^1 = 1/2 - 1/3 = \boxed{1/6}$

**P708.** (2-mark) Gradient descent on $f(x) = x^4 - 4x^2 + 4$ from $x_0 = 3$, $\eta = 0.01$.
→ $f'(x) = 4x^3 - 8x$. $f'(3) = 108 - 24 = 84$
→ $x_1 = 3 - 0.01 \times 84 = 3 - 0.84 = 2.16$
→ $f'(2.16) = 4(10.077) - 17.28 = 40.31 - 17.28 = 23.03$
→ $x_2 = 2.16 - 0.23 = 1.93$ → converging toward $x = \sqrt{2} \approx 1.414$

**P709.** (2-mark MCQ) Which function is NOT convex on $\mathbb{R}$?
(A) $e^x$ (B) $x^2$ (C) $|x|$ (D) $\sin x$
→ $\sin''(x) = -\sin x$ which changes sign → **Answer: (D)**

**P710.** (2-mark) $f(x,y) = x^3 - 3xy^2$: classify critical point at origin.
→ $f_x = 3x^2 - 3y^2 = 0$, $f_y = -6xy = 0$ → $(0,0)$ is critical
→ $f_{xx} = 6x = 0$, $f_{yy} = -6x = 0$, $f_{xy} = -6y = 0$ at origin
→ $D = 0 \cdot 0 - 0 = 0$ → **Inconclusive!** (Second derivative test fails)
→ Check: $f(x,0) = x^3$ changes sign → **saddle point** (monkey saddle!)

**P711-P730** — Rapid GATE Simulation (1-mark each):

| # | Question | Answer |
|---|---|---|
| P711 | $\frac{d}{dx}\int_0^{x^2} e^{t^2} dt$ = ? | $2xe^{x^4}$ (Leibniz rule) |
| P712 | $\int_0^1 \frac{1}{\sqrt{-\ln x}} dx$ = ? | Sub $t = -\ln x$: $\int_0^{\infty} \frac{e^{-t}}{\sqrt{t}} dt = \Gamma(1/2) = \sqrt{\pi}$ |
| P713 | Is $x^2 + y^2 \leq 1$ a convex set? | YES (unit disk is convex) |
| P714 | Hessian of $f = xy$? | $H = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$, eigenvalues $\pm 1$ → indefinite → saddle |
| P715 | $\nabla \cdot (x\hat{i} + y\hat{j} + z\hat{k})$ = ? | $1 + 1 + 1 = 3$ |
| P716 | $\nabla \times (x\hat{i} + y\hat{j} + z\hat{k})$ = ? | $\vec{0}$ (irrotational, it's $\nabla(r^2/2)$) |
| P717 | Euler's formula: $e^{i\theta}$ = ? | $\cos\theta + i\sin\theta$ |
| P718 | $\sum_{n=0}^{\infty} \frac{(-1)^n x^{2n}}{(2n)!}$ = ? | $\cos x$ |
| P719 | Is $\max(f,g)$ convex if $f,g$ convex? | YES (pointwise max preserves convexity) |
| P720 | Bernoulli ODE: $y' + y = y^2$. Sub? | $v = y^{-1}$: $-v' + v = 1$ → $v' - v = -1$ → solve linear |
| P721 | Wronskian of $e^x, xe^x$? | $W = e^x \cdot e^x(1+x) - xe^x \cdot e^x = e^{2x}$ |
| P722 | $\Gamma(1/2)$ = ? | $\sqrt{\pi}$ |
| P723 | $B(m,n)$ in terms of $\Gamma$? | $\frac{\Gamma(m)\Gamma(n)}{\Gamma(m+n)}$ |
| P724 | $\int_0^1 x^{m-1}(1-x)^{n-1} dx$ = ? | $B(m,n)$ |
| P725 | Radius of convergence of $\sum x^n/n$? | $R = 1$ (ratio test: $L = |x|$) |
| P726 | If $f''(c) > 0$, critical point $c$ is? | Local minimum |
| P727 | Cauchy's MVT: $\frac{f(b)-f(a)}{g(b)-g(a)}$ = ? | $\frac{f'(c)}{g'(c)}$ for some $c \in (a,b)$ |
| P728 | Taylor remainder: $R_n(x)$ = ? | $\frac{f^{(n+1)}(\xi)}{(n+1)!}(x-a)^{n+1}$, $\xi$ between $a$ and $x$ |
| P729 | Is $f(x) = e^{-x^2}$ integrable on $\mathbb{R}$? | YES: $\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}$ (Gaussian integral) |
| P730 | Jacobian for polar coords? | $r$ (since $dx\,dy = r\,dr\,d\theta$) |

**P731-P760** — Cross-Topic Synthesis:

**P731.** (2-mark) In logistic regression, show the loss is convex.
→ $L(w) = -\sum [y_i \log \sigma(w^Tx_i) + (1-y_i)\log(1-\sigma(w^Tx_i))]$
→ $\nabla^2 L = X^T\text{diag}(\sigma_i(1-\sigma_i))X$. Since $\sigma_i(1-\sigma_i) > 0$, diagonal matrix is PD.
→ $X^TDX$ is PSD for any $D$ PSD → $\nabla^2 L \succeq 0$ → **convex** ✅

**P732.** (2-mark) SVM dual involves maximizing $\sum \alpha_i - \frac{1}{2}\alpha^TQ\alpha$ where $Q_{ij} = y_iy_jx_i^Tx_j$.
Subject to $\alpha_i \geq 0$, $\sum \alpha_i y_i = 0$. This is a **quadratic program** (QP).
→ Since $Q$ is PSD (gram matrix), the neg of objective is convex → unique solution exists.

**P733.** (2-mark) Principal component direction: first PC maximizes $\text{Var}(w^TX) = w^T\Sigma w$ s.t. $\|w\| = 1$.
→ Lagrange: $\nabla(w^T\Sigma w - \lambda(w^Tw - 1)) = 0$
→ $2\Sigma w = 2\lambda w$ → $\Sigma w = \lambda w$ → eigenvector of $\Sigma$!
→ Max variance = largest eigenvalue. PC1 = corresponding eigenvector.

**P734.** (2-mark) MLE for normal: $\hat{\mu} = \bar{x}$ is the minimizer of $\sum(x_i - \mu)^2$.
→ Derivative: $-2\sum(x_i - \mu) = 0$ → $n\mu = \sum x_i$ → $\mu = \bar{x}$
→ Second derivative: $2n > 0$ → minimum confirmed ✅

**P735.** (2-mark) Bias-variance tradeoff via calculus:
→ $\text{MSE} = \text{Bias}^2 + \text{Variance}$
→ As model complexity ↑: bias ↓ (better fit), variance ↑ (overfitting)
→ Optimal complexity: minimize total MSE → $\frac{d}{d\text{complexity}}[\text{Bias}^2 + \text{Var}] = 0$

**P736-P760** — Speed Drill (30 seconds each):

| # | Q | A |
|---|---|---|
| P736 | $\lim_{n\to\infty} (1 + 1/n)^n$ | $e$ |
| P737 | $\lim_{n\to\infty} n^{1/n}$ | $1$ |
| P738 | $\lim_{x\to 0^+} x\ln x$ | $0$ ($\ln x / (1/x) \to 0$ by L'Hôp) |
| P739 | $\lim_{x\to\infty} x/e^x$ | $0$ (exp dominates polynomial) |
| P740 | $\frac{d}{dx}(x^x)$ | $x^x(\ln x + 1)$ |
| P741 | Is $\Gamma$ log-convex? | YES (Bohr-Mollerup theorem) |
| P742 | $\int_0^1 x^n \ln x \, dx$ | $\frac{-1}{(n+1)^2}$ |
| P743 | Laplacian in polar: $\nabla^2 f$ | $f_{rr} + f_r/r + f_{\theta\theta}/r^2$ |
| P744 | Divergence theorem: $\oint_S \vec{F}\cdot d\vec{S}$ = | $\int_V \nabla\cdot\vec{F}\, dV$ |
| P745 | Green's first identity? | $\int_V (\nabla u \cdot \nabla v + u\nabla^2 v)\,dV = \oint_S u\frac{\partial v}{\partial n}\,dS$ |
| P746 | Harmonic function satisfies? | $\nabla^2 u = 0$ (Laplace equation) |
| P747 | Max principle for harmonic? | Max/min on boundary, not interior |
| P748 | Is $e^{-\|x\|^2/2}$ log-concave? | YES ($\log = -\|x\|^2/2$ is concave) |
| P749 | Proximal operator of $\lambda\|x\|_1$? | Soft thresholding: $\text{sign}(x)\max(|x|-\lambda, 0)$ |
| P750 | Moreau envelope smooths? | Non-smooth function → smooth approximation |
| P751 | $\int\int_R 1\, dA$ where $R$: unit circle? | $\pi$ (area of unit circle) |
| P752 | $\int\int\int_B 1\, dV$ where $B$: unit ball? | $4\pi/3$ (volume) |
| P753 | Surface area of unit sphere? | $4\pi$ |
| P754 | Arc length of $y = \cosh x$, $[0,1]$? | $\sinh 1 \approx 1.1752$ |
| P755 | Curvature of circle radius $R$? | $\kappa = 1/R$ everywhere |
| P756 | Evolute of parabola $y = x^2$? | $(x - \frac{f'(1+f'^2)}{f''}, y + \frac{1+f'^2}{f''})$ |
| P757 | Is $f(x) = x\log x - x$ convex for $x > 0$? | $f'' = 1/x > 0$ → YES (negative entropy function) |
| P758 | KL divergence: $D_{KL}(p\|q) \geq ?$ | $0$ (Gibbs' inequality), $= 0$ iff $p = q$ |
| P759 | Fisher information: $I(\theta) = ?$ | $E[(\frac{\partial}{\partial\theta}\log f)^2] = -E[\frac{\partial^2}{\partial\theta^2}\log f]$ |
| P760 | Cramér-Rao bound? | $\text{Var}(\hat\theta) \geq 1/I(\theta)$ |

---

## 📝 Practice Set 14: Express Final Round (P761 – P840)

| # | Problem | Answer |
|---|---|---|
| P761 | $\int_0^2 |x - 1| dx$ | $= \int_0^1(1-x)dx + \int_1^2(x-1)dx = 1/2 + 1/2 = 1$ |
| P762 | $\frac{d}{dx}\int_x^{x^2} \sin(t^2)dt$ | $= 2x\sin(x^4) - \sin(x^2)$ (Leibniz) |
| P763 | $\int_0^{\infty} xe^{-2x} dx$ | $= \Gamma(2)/2^2 = 1/4$ |
| P764 | $\sum_{n=0}^{\infty} \frac{2^n}{n!}$ | $= e^2 \approx 7.389$ |
| P765 | Taylor: $\ln(1+x) = ?$ | $x - x^2/2 + x^3/3 - \cdots$, $-1 < x \leq 1$ |
| P766 | Error bound: $|\sin x - x| \leq ?$ for $|x| < 0.1$ | $|x^3|/6 < 10^{-3}/6 \approx 1.67 \times 10^{-4}$ |
| P767 | $f(x,y) = x^2+y^2-2x-4y+5$: min? | $\nabla f = (2x-2, 2y-4) = 0 → (1,2)$. $f(1,2) = 0$ |
| P768 | Implicit diff: $x^3+y^3=6xy$, $dy/dx$? | $\frac{3x^2}{6y-3y^2} = \frac{6y-3x^2}{3y^2-6x} = \frac{2y-x^2}{y^2-2x}$ |
| P769 | Method of Lagrange: max $xy$ on $x+y=10$? | $y=\lambda$, $x=\lambda$, $x+y=10$ → $x=y=5$, max $=25$ |
| P770 | Is $\begin{pmatrix}2&1\\1&2\end{pmatrix}$ PD? | Eigenvalues $3,1 > 0$ → YES |
| P771 | Condition number of above? | $\kappa = 3/1 = 3$ |
| P772 | $\nabla f = (2x+y, x+4y)$ at $(1,1)$: direction of steepest ascent? | $(3, 5)$, normalized: $(3,5)/\sqrt{34}$ |
| P773 | Directional derivative in direction $(1,1)/\sqrt{2}$? | $(3+5)/\sqrt{2} = 4\sqrt{2}$ |
| P774 | Level curve through $(1,1)$ for $f = x^2+xy+2y^2$? | $f(1,1) = 1+1+2 = 4$; curve: $x^2+xy+2y^2 = 4$ |
| P775 | Change of variable: $\int\int_R xy \, dA$, $R$: parallelogram | Use appropriate affine transformation |
| P776 | Green's: $\oint_C (y\,dx - x\,dy)$ for unit circle? | $= -\int\int(-1-1)dA = 2\pi$ |
| P777 | Work done by $\vec{F} = (y, x)$ around unit circle? | $\oint y\,dx + x\,dy$. $\frac{\partial x}{\partial x} - \frac{\partial y}{\partial y} = 1-1 = 0$ → $W = 0$ |
| P778 | $y'' + 4y = 0$: general solution? | $y = c_1\cos 2x + c_2\sin 2x$ |
| P779 | $y'' + 4y = \sin 2x$: resonance? | YES! $\sin 2x$ is solution of homogeneous → multiply by $x$ |
| P780 | Particular solution for P779? | $y_p = -\frac{x\cos 2x}{4}$ |
| P781 | Order of RK4 method? | 4th order (error $O(h^5)$ per step) |
| P782 | Trapezoidal error for $\int_0^1 x^2 dx$, $h = 0.5$? | Trap: $(0.5/2)(0+2(0.25)+1) = 0.375$. Exact: $1/3$. Error: $0.042$ |
| P783 | Simpson's for same? (2 intervals) | $(0.5/3)(0+4(0.25)+1) = (1/6)(2) = 1/3$ → **Exact!** (cubic exact) |
| P784 | $\int_0^1 e^{-x^2} dx \approx ?$ | $\approx 0.7468$ (no closed form; use series/numerical) |
| P785 | Error function: $\text{erf}(x) = ?$ | $\frac{2}{\sqrt{\pi}}\int_0^x e^{-t^2} dt$ |
| P786 | $\text{erf}(\infty) = ?$ | $1$ |
| P787 | Complementary: $\text{erfc}(x) = ?$ | $1 - \text{erf}(x) = \frac{2}{\sqrt{\pi}}\int_x^{\infty} e^{-t^2}dt$ |
| P788 | Normal CDF in terms of erf? | $\Phi(x) = \frac{1}{2}[1 + \text{erf}(x/\sqrt{2})]$ |
| P789 | $\frac{d}{dx}\text{erf}(x)$ = ? | $\frac{2}{\sqrt{\pi}}e^{-x^2}$ |
| P790 | Is $-\log\det(X)$ convex on PD matrices? | YES (standard result in convex optimization) |
| P791 | Trace: $\frac{\partial}{\partial X}\text{tr}(AX)$ = ? | $A^T$ |
| P792 | $\frac{\partial}{\partial X}\text{tr}(X^TAX)$ = ? | $(A+A^T)X$ |
| P793 | $\frac{\partial}{\partial X}\log\det(X)$ = ? | $X^{-T}$ (= $X^{-1}$ if symmetric) |
| P794 | Chain rule for matrices: $\frac{\partial L}{\partial W_1}$ in neural net? | $= \frac{\partial L}{\partial a_2}\frac{\partial a_2}{\partial z_2}\frac{\partial z_2}{\partial a_1}\frac{\partial a_1}{\partial z_1}\frac{\partial z_1}{\partial W_1}$ |
| P795 | Backpropagation is just? | Reverse-mode automatic differentiation (chain rule!) |
| P796 | Total derivative vs partial? | Total: accounts for ALL dependencies; Partial: varies ONE variable |
| P797 | $\frac{dz}{dt}$ where $z = f(x(t), y(t))$? | $\frac{\partial f}{\partial x}\frac{dx}{dt} + \frac{\partial f}{\partial y}\frac{dy}{dt}$ (chain rule) |
| P798 | Envelope theorem in economics? | $\frac{dV}{d\alpha} = \frac{\partial L}{\partial \alpha}\bigg|_{x=x^*}$ — only direct effect! |
| P799 | Is $f(x) = \|x\|_2$ differentiable at $x = 0$? | NO (not differentiable at origin in $\mathbb{R}^n$, $n > 1$) |
| P800 | Subdifferential of $\|x\|_2$ at $0$? | Unit ball: $\{g : \|g\|_2 \leq 1\}$ |

---

## 📝 Practice Set 15: Ultimate Speed Drill (P801 – P840)

*Target: 30 seconds per problem. Pure recall.*

| # | Q | A |
|---|---|---|
| P801 | $e^0$ | $1$ |
| P802 | $\ln 1$ | $0$ |
| P803 | $\sin 0 + \cos 0$ | $1$ |
| P804 | $(d/dx)\ln|x|$ | $1/x$ |
| P805 | $\int e^{3x}dx$ | $e^{3x}/3 + C$ |
| P806 | $\int_0^{\pi} \sin x \, dx$ | $2$ |
| P807 | Taylor: $e^x$ first 4 terms | $1 + x + x^2/2 + x^3/6$ |
| P808 | $\lim_{n \to \infty} (1+2/n)^n$ | $e^2$ |
| P809 | $\frac{d}{dx}\sin^{-1}x$ | $1/\sqrt{1-x^2}$ |
| P810 | $\frac{d}{dx}\tan^{-1}x$ | $1/(1+x^2)$ |
| P811 | $\int \frac{dx}{1+x^2}$ | $\tan^{-1}x + C$ |
| P812 | $\int \frac{dx}{\sqrt{1-x^2}}$ | $\sin^{-1}x + C$ |
| P813 | Rolle: $f'(c) = ?$ | $0$ (for some $c \in (a,b)$) |
| P814 | MVT: $f'(c) = ?$ | $\frac{f(b)-f(a)}{b-a}$ |
| P815 | $B(1,1) = ?$ | $\Gamma(1)\Gamma(1)/\Gamma(2) = 1$ |
| P816 | $\Gamma(5) = ?$ | $4! = 24$ |
| P817 | $\int_0^1 x(1-x)dx$ | $= B(2,2) = 1/6$ |
| P818 | Hessian positive definite →? | Local minimum |
| P819 | Hessian negative definite →? | Local maximum |
| P820 | Hessian indefinite →? | Saddle point |
| P821 | $D = f_{xx}f_{yy} - f_{xy}^2 < 0$ →? | Saddle |
| P822 | GD update: $x_{t+1} = ?$ | $x_t - \eta \nabla f(x_t)$ |
| P823 | Momentum update? | $v_t = \beta v_{t-1} + \nabla f$; $x_t = x_{t-1} - \eta v_t$ |
| P824 | Adam combines? | Momentum + RMSProp (1st + 2nd moment) |
| P825 | Learning rate too large? | Diverges / oscillates |
| P826 | Learning rate too small? | Very slow convergence |
| P827 | Optimal step for quadratic? | $\eta = 1/L$ where $L$ = Lipschitz constant of gradient |
| P828 | $f$ convex + differentiable: $f(y) \geq ?$ | $f(x) + \nabla f(x)^T(y-x)$ |
| P829 | Strong convexity: $f(y) \geq$ | $f(x) + \nabla f(x)^T(y-x) + \frac{m}{2}\|y-x\|^2$ |
| P830 | Lipschitz gradient: $\|\nabla f(x) - \nabla f(y)\| \leq ?$ | $L\|x-y\|$ |
| P831 | Separable ODE pattern? | $g(y)dy = f(x)dx$ |
| P832 | Linear ODE solution: $y = ?$ | $\frac{1}{\mu}\int \mu Q \, dx$ where $\mu = e^{\int P\,dx}$ |
| P833 | Exact ODE condition? | $M_y = N_x$ |
| P834 | $y' = y/x$ solution? | $y = Cx$ (homogeneous, $dy/y = dx/x$) |
| P835 | $y' = -y$ solution? | $y = Ce^{-x}$ |
| P836 | $y'' + y = 0$ solution? | $y = c_1\cos x + c_2\sin x$ |
| P837 | $y'' - y = 0$ solution? | $y = c_1e^x + c_2e^{-x}$ (or $\cosh, \sinh$) |
| P838 | Wronskian nonzero ↔? | Solutions linearly independent |
| P839 | Undetermined coefficients: RHS $= e^{2x}$, try? | $y_p = Ae^{2x}$ (unless $2$ is char root) |
| P840 | Variation of parameters: always works for? | Any non-homogeneous linear ODE |

---

## ⚡ 50 Must-Know Formulas — Exam Day Card

| # | Formula | Domain |
|---|---|---|
| 1 | $\lim_{x\to 0}\frac{\sin x}{x} = 1$ | Limits |
| 2 | $(1+1/n)^n \to e$ | Limits |
| 3 | L'Hôpital: $\lim f/g = \lim f'/g'$ | Limits |
| 4 | $(fg)' = f'g + fg'$ | Derivatives |
| 5 | $(f/g)' = (f'g-fg')/g^2$ | Derivatives |
| 6 | Chain: $(f \circ g)' = f'(g) \cdot g'$ | Derivatives |
| 7 | MVT: $f'(c) = (f(b)-f(a))/(b-a)$ | Theorems |
| 8 | $\int u\,dv = uv - \int v\,du$ | Integration |
| 9 | $\int_0^{\pi/2}\sin^n x\,dx$: Wallis | Integration |
| 10 | $B(m,n) = \Gamma(m)\Gamma(n)/\Gamma(m+n)$ | Special |
| 11 | $\Gamma(n+1) = n!$, $\Gamma(1/2) = \sqrt{\pi}$ | Special |
| 12 | King's: $\int_0^a f(x)dx = \int_0^a f(a-x)dx$ | Integration tricks |
| 13 | Taylor: $f(x) = \sum f^{(n)}(a)(x-a)^n/n!$ | Series |
| 14 | $e^x = 1+x+x^2/2!+\cdots$ | Series |
| 15 | $\sin x = x - x^3/3! + x^5/5! - \cdots$ | Series |
| 16 | $\cos x = 1 - x^2/2! + x^4/4! - \cdots$ | Series |
| 17 | $\ln(1+x) = x - x^2/2 + x^3/3 - \cdots$ | Series |
| 18 | Ratio test: $L < 1$ converges | Convergence |
| 19 | $p$-series: $\sum 1/n^p$, $p>1$ converges | Convergence |
| 20 | $\nabla f = (f_x, f_y)$ | Multivar |
| 21 | $D_uf = \nabla f \cdot \hat{u}$ | Multivar |
| 22 | Hessian: $D = f_{xx}f_{yy} - f_{xy}^2$ | Optimization |
| 23 | $D > 0, f_{xx} > 0$: min | Optimization |
| 24 | $D > 0, f_{xx} < 0$: max | Optimization |
| 25 | $D < 0$: saddle | Optimization |
| 26 | Lagrange: $\nabla f = \lambda \nabla g$ | Optimization |
| 27 | KKT: $\nabla f = \sum \lambda_i \nabla g_i$, $\lambda_i g_i = 0$ | Optimization |
| 28 | Convex: $f(\lambda x+(1-\lambda)y) \leq \lambda f(x)+(1-\lambda)f(y)$ | Convexity |
| 29 | GD: $x_{t+1} = x_t - \eta\nabla f$ | Optimization |
| 30 | IF for ODE: $\mu = e^{\int P\,dx}$ | ODEs |
| 31 | Char eq: $am^2+bm+c=0$ | ODEs |
| 32 | $\int_0^{\infty}e^{-x^2}dx = \sqrt{\pi}/2$ | Gaussian |
| 33 | Jacobian for polar: $r$ | Coordinate change |
| 34 | Jacobian for spherical: $r^2\sin\phi$ | Coordinate change |
| 35 | Green's: $\oint \vec{F}\cdot d\vec{r} = \iint(\partial Q/\partial x - \partial P/\partial y)dA$ | Vector calc |
| 36 | Divergence thm: $\oint\vec{F}\cdot d\vec{S} = \iiint\nabla\cdot\vec{F}\,dV$ | Vector calc |
| 37 | $\sigma(x) = 1/(1+e^{-x})$, $\sigma' = \sigma(1-\sigma)$ | ML |
| 38 | Cross-entropy: $-y\log p-(1-y)\log(1-p)$ | ML |
| 39 | Ridge closed form: $(X^TX+\lambda I)^{-1}X^Ty$ | ML |
| 40 | Newton: $x_{n+1} = x_n - f(x_n)/f'(x_n)$ | Numerical |
| 41 | Trapezoidal: $O(h^2)$ | Numerical |
| 42 | Simpson: $O(h^4)$ | Numerical |
| 43 | RK4: $O(h^4)$ global | Numerical |
| 44 | Leibniz: $\frac{d}{dx}\int_{a(x)}^{b(x)}f\,dt = fb'-fa'+\int f_x\,dt$ | Integration |
| 45 | Fubini: swap integral order if $\int|f|<\infty$ | Integration |
| 46 | AM-GM: $(a+b)/2 \geq \sqrt{ab}$ | Inequalities |
| 47 | Cauchy-Schwarz: $(\sum a_ib_i)^2 \leq \sum a_i^2 \sum b_i^2$ | Inequalities |
| 48 | Jensen: $f(E[X]) \leq E[f(X)]$ (convex) | Inequalities |
| 49 | $\text{erf}(x) = \frac{2}{\sqrt{\pi}}\int_0^x e^{-t^2}dt$ | Special |
| 50 | Euler: $e^{i\theta} = \cos\theta+i\sin\theta$ | Complex |

---

## 📝 Practice Set 16: Last 60 Problems (P841 – P900)

**P841.** (2-mark) $\int_0^1 \int_0^{\sqrt{x}} e^{y/x} \, dy\,dx$. Change order.
→ Region: $0 \leq y \leq \sqrt{x}$, $0 \leq x \leq 1$ → $y^2 \leq x \leq 1$, $0 \leq y \leq 1$
→ $= \int_0^1 \int_{y^2}^1 e^{y/x} dx\,dy$

**P842.** (2-mark) $\int\int_D r^3 \, dr\,d\theta$ where $D$: unit disk.
→ $= \int_0^{2\pi}\int_0^1 r^3 \, dr\,d\theta = 2\pi \cdot [r^4/4]_0^1 = 2\pi/4 = \boxed{\pi/2}$

**P843.** (2-mark) Moment of inertia of unit disk about z-axis: $I_z = \int\int (x^2+y^2) dA$
→ Polar: $\int_0^{2\pi}\int_0^1 r^2 \cdot r\,dr\,d\theta = 2\pi \cdot 1/4 = \pi/2$

**P844.** (2-mark) Center of mass of triangular region with vertices $(0,0), (1,0), (0,1)$, uniform density.
→ $\bar{x} = \frac{\int\int x\,dA}{\int\int dA}$. Area $= 1/2$.
→ $\int_0^1\int_0^{1-x} x\,dy\,dx = \int_0^1 x(1-x)dx = [x^2/2-x^3/3]_0^1 = 1/6$
→ $\bar{x} = (1/6)/(1/2) = 1/3$. By symmetry $\bar{y} = 1/3$.
→ Centroid: $(1/3, 1/3)$ ✅

**P845.** (2-mark) Triple integral: volume of region $x^2+y^2 \leq z \leq 1$.
→ Cylindrical: $\int_0^{2\pi}\int_0^1\int_{r^2}^1 r\,dz\,dr\,d\theta = 2\pi\int_0^1 r(1-r^2)dr$
→ $= 2\pi[r^2/2 - r^4/4]_0^1 = 2\pi(1/2-1/4) = \boxed{\pi/2}$

**P846.** (2-mark) Divergence of $\vec{F} = (x^2, y^2, z^2)$?
→ $\nabla \cdot \vec{F} = 2x + 2y + 2z$

**P847.** (2-mark) Flux of $\vec{F} = (x,y,z)$ through unit sphere (outward)?
→ Gauss: $\int\int\int \nabla\cdot\vec{F}\,dV = \int\int\int 3\,dV = 3 \cdot 4\pi/3 = 4\pi$

**P848.** (2-mark) Stokes': $\oint_C \vec{F}\cdot d\vec{r}$ where $\vec{F} = (-y, x, 0)$, $C$: unit circle in $xy$-plane.
→ $\nabla \times \vec{F} = (0, 0, 2)$. $\int\int (\nabla\times\vec{F})\cdot\hat{n}\,dS = 2\cdot\pi(1)^2 = 2\pi$

**P849.** (2-mark) Potential function: $\vec{F} = (2xy + z, x^2, x)$. Is it conservative?
→ $\nabla\times\vec{F}$: $(\partial_y x - \partial_z x^2, \partial_z(2xy+z) - \partial_x x, \partial_x x^2 - \partial_y(2xy+z))$
→ $= (1-0, 1-1, 2x-2x) = (1, 0, 0) \neq \vec{0}$
→ **NOT conservative**

**P850.** (2-mark) $\vec{F} = (2xy, x^2+z, y)$. Conservative?
→ $\nabla\times\vec{F} = (\partial_y y - \partial_z (x^2+z), \partial_z(2xy) - \partial_x y, \partial_x(x^2+z) - \partial_y(2xy))$
→ $= (1-1, 0-0, 2x-2x) = \vec{0}$ → **YES, conservative!**
→ $\phi = x^2y + yz + C$ (verify: $\nabla\phi = (2xy, x^2+z, y)$ ✅)

**P851-P870** — Tricky Conceptual Questions:

| # | Statement | True/False | Explanation |
|---|---|---|---|
| P851 | Every continuous function on $[a,b]$ is integrable | TRUE | Riemann integrable on closed bounded interval |
| P852 | Every integrable function is continuous | FALSE | Step function: integrable, not continuous |
| P853 | $f$ differentiable at $a$ → $f$ continuous at $a$ | TRUE | Definition of differentiability |
| P854 | $f$ continuous at $a$ → $f$ differentiable at $a$ | FALSE | $|x|$ at $x=0$ |
| P855 | $f_x, f_y$ exist → $f$ differentiable | FALSE | Need partials continuous (sufficient, not necessary) |
| P856 | $f_x, f_y$ continuous → $f$ differentiable | TRUE | Sufficient condition |
| P857 | Absolutely convergent → convergent | TRUE | Always |
| P858 | Convergent → absolutely convergent | FALSE | $\sum(-1)^n/n$ |
| P859 | Uniformly convergent → pointwise convergent | TRUE | Uniform is stronger |
| P860 | Pointwise convergent → uniformly convergent | FALSE | $f_n(x)=x^n$ on $[0,1]$ |
| P861 | Convex + differentiable → gradient monotone | TRUE | $(\nabla f(x)-\nabla f(y))^T(x-y)\geq 0$ |
| P862 | Sum of convex is convex | TRUE | Preserves convexity |
| P863 | Product of convex is convex | FALSE | $f=x, g=-x$ both convex, $fg=-x^2$ concave |
| P864 | Composition $f(g(x))$: $f$ convex, $g$ convex → convex? | Only if $f$ non-decreasing! |
| P865 | $f$ convex, $g$ affine → $f(g(x))$ convex | TRUE | Always |
| P866 | Maximum of convex functions is convex | TRUE | Pointwise max |
| P867 | Minimum of convex functions is convex | FALSE | In general, no |
| P868 | $f$ strictly convex → unique minimum | TRUE | If minimum exists |
| P869 | $f$ convex on $\mathbb{R}^n$ → every local min is global | TRUE | Fundamental property! |
| P870 | $f$ convex + $\nabla f(x^*)=0$ → $x^*$ global min | TRUE | First-order sufficiency |

**P871-P890** — Computation Speed Round:

| # | Compute | Answer |
|---|---|---|
| P871 | $\frac{d}{dx}(x^2\sin x)$ | $2x\sin x + x^2\cos x$ |
| P872 | $\frac{d}{dx}\frac{e^x}{x}$ | $\frac{e^x(x-1)}{x^2}$ |
| P873 | $\int x\cos x\,dx$ | $x\sin x + \cos x + C$ |
| P874 | $\int \ln x \, dx$ | $x\ln x - x + C$ |
| P875 | $\int \frac{x}{x^2+1}dx$ | $\frac{1}{2}\ln(x^2+1)+C$ |
| P876 | $\int_0^{\pi/2} \cos^2 x\,dx$ | $\pi/4$ |
| P877 | $\int_0^1 xe^x dx$ | $[xe^x-e^x]_0^1 = 1$ |
| P878 | $\int_1^e \frac{(\ln x)^2}{x}dx$ | $[(\ln x)^3/3]_1^e = 1/3$ |
| P879 | $\lim_{x\to 0}\frac{1-\cos x}{x^2}$ | $1/2$ |
| P880 | $\lim_{x\to\infty}\frac{x}{e^x}$ | $0$ |
| P881 | $\lim_{x\to 0}\frac{e^x-1}{x}$ | $1$ |
| P882 | $\lim_{x\to 0}\frac{\ln(1+x)}{x}$ | $1$ |
| P883 | $\frac{d}{dx}a^x$ | $a^x \ln a$ |
| P884 | $\int a^x dx$ | $\frac{a^x}{\ln a}+C$ |
| P885 | $\frac{d}{dx}\cosh x$ | $\sinh x$ |
| P886 | $\frac{d}{dx}\tanh x$ | $\text{sech}^2 x = 1 - \tanh^2 x$ |
| P887 | $\frac{\partial}{\partial x}(x^2y^3)$ | $2xy^3$ |
| P888 | $\frac{\partial^2}{\partial x\partial y}(x^2y^3)$ | $6xy^2$ |
| P889 | Gradient of $f = x^2+y^2+z^2$ | $(2x, 2y, 2z)$ |
| P890 | Laplacian of same $f$ | $2+2+2 = 6$ |

**P891-P900** — Final 10 (Hard, 2-mark each):

| # | Q | A |
|---|---|---|
| P891 | $\int_0^{\infty}\frac{\sin^2 x}{x^2}dx$ | $= \pi/2$ (via Parseval's or integration by parts + Dirichlet) |
| P892 | Minimize $\|Ax-b\|^2 + \lambda\|x\|^2$: solution? | $x = (A^TA+\lambda I)^{-1}A^Tb$ (ridge regression) |
| P893 | Newton's method convergence: $f(x)=x^3$, simple root at $0$? | $f'(0)=0$: Newton converges linearly, not quadratically (degenerate) |
| P894 | Prove: $\int_0^1\int_0^1\frac{1}{1-xy}dx\,dy = \sum_{n=1}^{\infty}\frac{1}{n^2} = \frac{\pi^2}{6}$ | Expand $1/(1-xy) = \sum(xy)^n$, integrate term by term |
| P895 | $\int_0^{\pi/2}\ln(\tan x)dx$ | $= 0$ (by symmetry: $\int\ln\sin x + \int\ln\cos x$ cancel) |
| P896 | Catalan constant from integration? | $G = \int_0^1 \frac{\tan^{-1}x}{x}dx \approx 0.9159$ |
| P897 | Euler-Maclaurin formula relates? | $\sum f(k) \approx \int f + \frac{f(a)+f(b)}{2} + \sum\frac{B_{2k}}{(2k)!}(f^{(2k-1)}(b)-f^{(2k-1)}(a))$ |
| P898 | Laplace method: $\int_a^b e^{Mf(x)}dx$ as $M\to\infty$? | $\approx e^{Mf(x_0)}\sqrt{2\pi/(M|f''(x_0)|)}$ where $x_0$ = max of $f$ |
| P899 | Saddle-point approximation? | Same as Laplace but for complex integrals (steepest descent) |
| P900 | Final wisdom: what's the #1 calculus mistake in GATE? | **Forgetting chain rule** in multivariable / composite functions — always check! |

---

## 🔑 Index of All Practice Problems

| Set | Problems | Topics | Count |
|---|---|---|---|
| Set 1 | P1 – P60 | Limits, Continuity | 60 |
| Set 2 | P61 – P130 | Derivatives, MVT, Integration, Multivariable, ODEs | 70 |
| Set 3 | P131 – P200 | GATE Simulation | 70 |
| Set 4 | P201 – P280 | Integration, Series, Multivariate, ODEs | 80 |
| Set 5 | P251 – P330 | Optimization GATE Style | 80 |
| Set 6 | P281 – P360 | Green's/Stokes/Gauss, Convex Analysis | 80 |
| Set 7 | P331 – P400 | Final GATE Blitz | 70 |
| Set 8 | P361 – P400 | Speed Round | 40 |
| Set 9 | P401 – P500 | Mega GATE Drill | 100 |
| Set 10 | P501 – P620 | Ultimate Marathon | 120 |
| Set 11 | P621 – P700 | Convergence, Numerical Methods | 80 |
| Set 12 | P661 – P740 | Numerical Methods & Approximation | 80 |
| Set 13 | P701 – P800 | GATE Predicted 2025-2027 | 100 |
| Set 14 | P801 – P840 | Express Final | 40 |
| Set 15 | P841 – P870 | Vector Calculus, True/False | 30 |
| Set 16 | P871 – P900 | Computation + Hard Problems | 30 |
| **TOTAL** | | | **~900 problems** |

---

## 🧠 50 Conceptual One-Liners for Last-Minute Revision

1. Differentiable ⊂ Continuous ⊂ Integrable (on $[a,b]$) — but none of the reverses hold in general.
2. Rolle's theorem is MVT with $f(a) = f(b)$. MVT is Rolle's applied to $g(x) = f(x) - \frac{f(b)-f(a)}{b-a}(x-a)$.
3. Taylor's theorem = MVT on steroids: higher-order approximation with remainder.
4. L'Hôpital works ONLY for $0/0$ or $\infty/\infty$ forms. Convert other indeterminate forms first.
5. $\int_a^b f(x)dx = F(b) - F(a)$ — Fundamental Theorem of Calculus connects derivatives and integrals.
6. Integration by parts: choose $u$ using LIATE (Log, Inverse trig, Algebraic, Trig, Exponential).
7. For $\int \frac{P(x)}{Q(x)}dx$: if $\deg P \geq \deg Q$, do polynomial division FIRST.
8. Partial fractions: distinct linear → $A/(x-a)$; repeated → $A/(x-a) + B/(x-a)^2$; irreducible quadratic → $(Ax+B)/(x^2+bx+c)$.
9. Wallis formula connects $\int_0^{\pi/2}\sin^n x\,dx$ to factorials/double factorials.
10. King's property is the #1 time-saver for definite integrals on symmetric intervals.
11. Geometric series: $|r|<1 \Rightarrow \sum ar^n = a/(1-r)$. Most fundamental series!
12. Ratio test is best for factorials/exponentials. Root test is best for $n$th powers.
13. Comparison test: find a known convergent/divergent series to bound yours.
14. Alternating series test (Leibniz): decreasing absolute terms → 0 is enough.
15. Power series: always find ROC first, then check endpoints separately.
16. Partial derivatives: differentiate treating other variables as constants. Simple!
17. Gradient points in the direction of steepest ASCENT. Negative gradient → steepest descent.
18. Directional derivative: $\nabla f \cdot \hat{u}$. Maximum when $\hat{u} \parallel \nabla f$, giving $\|\nabla f\|$.
19. Level curves are perpendicular to the gradient at every point.
20. Chain rule in multivar: draw the dependency tree, multiply along paths, add across paths.
21. Jacobian = determinant of the matrix of all first-order partial derivatives. Changes variables in integrals.
22. Hessian test: compute $D = f_{xx}f_{yy} - f_{xy}^2$. Three cases: $D>0$ (min/max), $D<0$ (saddle), $D=0$ (inconclusive).
23. Lagrange multipliers: $\nabla f = \lambda \nabla g$ geometrically means level curves of $f$ and $g$ are tangent.
24. KKT = Lagrange + inequality constraints. Complementary slackness: $\lambda_i g_i(x) = 0$.
25. Convex function: lies below any chord. Equivalently: $f'' \geq 0$ (1D) or Hessian PSD (nD).
26. Every local minimum of a convex function is a global minimum. This is WHY convexity matters!
27. Strong convexity → unique global minimum. Guarantees linear convergence of GD.
28. GD converges in $O(1/\epsilon)$ for convex, $O(\log(1/\epsilon))$ for strongly convex.
29. Learning rate $\eta$ should be $\leq 1/L$ where $L$ = Lipschitz constant of the gradient.
30. Momentum accelerates GD through "inertia" — helps escape shallow local minima and flat regions.
31. Adam = adaptive learning rate per parameter. Best default optimizer in deep learning.
32. Separable ODE: separate variables, integrate both sides. Simplest type!
33. Linear first-order ODE: IF method ALWAYS works. $\mu = e^{\int P\,dx}$, multiply through, integrate.
34. Exact ODE: check $M_y = N_x$. If exact, $F = \int M\,dx + \phi(y)$, find $\phi$ from $F_y = N$.
35. Bernoulli ODE: $y' + Py = Qy^n$ → substitute $v = y^{1-n}$ → becomes linear.
36. Euler-Cauchy: $x^ny^{(n)} + \cdots = 0$ → try $y = x^m$ → polynomial in $m$.
37. Second-order constant coeff: characteristic equation $am^2+bm+c=0$ gives three cases (real distinct, repeated, complex).
38. Variation of parameters works for ANY forcing function. Undetermined coefficients only for special RHS.
39. Wronskian $\neq 0 \Leftrightarrow$ solutions linearly independent. Essential for variation of parameters.
40. Double integrals: sketch the region FIRST, then choose order of integration wisely.
41. Change of order of integration: re-describe the same region with opposite bounds.
42. Polar coordinates: use when region is circular/radial. $dA = r\,dr\,d\theta$ (don't forget the $r$!).
43. Green's theorem converts line integral ↔ double integral. 2D version of Stokes'.
44. Divergence theorem: flux through closed surface = integral of divergence over enclosed volume.
45. Conservative field: $\nabla \times \vec{F} = \vec{0}$, path-independent, has potential function $\phi$.
46. Trapezoidal rule: $O(h^2)$. Simpson's: $O(h^4)$. Always prefer Simpson's for smooth functions.
47. Newton-Raphson: quadratic convergence near simple roots. But needs $f'(x_0) \neq 0$.
48. RK4 is the workhorse of numerical ODEs: 4th order, explicit, easy to implement.
49. Gaussian quadrature with $n$ points exactly integrates polynomials up to degree $2n-1$.
50. In GATE: read the question carefully, check units, verify answer by substituting back, manage time!

---

## 🎯 Topic-Wise GATE Question Frequency (2015-2025)

| Topic | 2015 | 2016 | 2017 | 2018 | 2019 | 2020 | 2021 | 2022 | 2023 | 2024 | 2025 | Total |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Limits | 1 | 0 | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 1 | 1 | 8 |
| Continuity/Diff | 0 | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 0 | 5 |
| MVT/Rolle's | 0 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 1 | 4 |
| Integration | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 11 |
| Taylor/Maclaurin | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 6 |
| Partial Diff | 1 | 0 | 1 | 0 | 1 | 1 | 1 | 0 | 0 | 1 | 1 | 7 |
| Optimization | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 11 |
| Double Integrals | 0 | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 1 | 0 | 0 | 5 |
| ODEs | 1 | 0 | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 1 | 1 | 7 |
| Series/Convergence | 0 | 1 | 1 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 1 | 5 |
| **Total/Year** | **5** | **6** | **7** | **6** | **6** | **6** | **7** | **6** | **6** | **6** | **8** | **69** |

**Key Insight:** Integration and Optimization appear EVERY year. Limits, Partial Diff, and ODEs appear ~70% of years. These 5 topics account for ~65% of all calculus questions.

---

## 📌 Parser for Your Weak Spots

After solving all 900 problems, tabulate your accuracy:

| Topic | Attempted | Correct | Accuracy | Priority |
|---|---|---|---|---|
| Limits & Continuity | __ | __ | __% | |
| Derivatives & MVT | __ | __ | __% | |
| Integration | __ | __ | __% | |
| Taylor Series | __ | __ | __% | |
| Multivariable | __ | __ | __% | |
| Optimization | __ | __ | __% | |
| ODEs | __ | __ | __% | |
| Series & Convergence | __ | __ | __% | |
| Vector Calculus | __ | __ | __% | |
| Numerical Methods | __ | __ | __% | |

- **< 60%:** RED — Restudy chapter notes + redo all problems
- **60-80%:** YELLOW — Review traps + redo mistakes
- **> 80%:** GREEN — Quick formula review is enough

---

## 🏁 Final Words of Wisdom

> "Calculus is the language that God uses to describe the universe." — Richard Feynman

**Top 5 GATE Calculus Commandments:**

1. **Thou shalt not forget the chain rule.** 90% of errors come from missing chain rule applications.
2. **Thou shalt always check the domain.** $\ln(x)$ needs $x > 0$; $\sqrt{x}$ needs $x \geq 0$.
3. **Thou shalt draw before thou integrate.** Sketch the region for double integrals — saves 5 minutes.
4. **Thou shalt verify with substitution.** After solving an ODE, plug back in. 30-second insurance.
5. **Thou shalt memorize thy formulas.** No formula sheet in GATE. The 50-formula card above is your lifeline.

**Remember:**
- Calculus questions in GATE are 95% mechanical — they test speed and accuracy, not creativity
- Practice 10 problems daily for 30 days = 300 problems = enough to master calculus for GATE
- Time yourself: 1-mark in 2 min, 2-mark in 4 min — anything longer means you're on the wrong track

**YOU'VE GOT THIS. NOW GO ACE GATE 2027!** 🎯🔥

---

*🏆 End of Calculus & Optimization — Comprehensive Notes for GATE 2027 (CS/IT/DA)*

*📅 Last Updated: April 2026 | Covers: GATE CS + GATE DA Syllabus | 100% Exam Coverage*

