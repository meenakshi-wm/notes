# Calculus & Optimization — Detailed Solutions & Shortcuts

## Solutions for All Questions from File 2

---

# SECTION A: LIMITS

---

### Q1. Solution
$$\frac{x^2 - 9}{x - 3} = \frac{(x-3)(x+3)}{x-3} = x + 3$$
$$\lim_{x \to 3}(x + 3) = 6$$

**Shortcut:** For $\lim_{x \to a}\frac{x^n - a^n}{x - a}$, answer is $na^{n-1}$. Here $n = 2, a = 3$: $2(3) = 6$. ✓

**Answer: 6**

---

### Q2. Solution
$$\lim_{x \to 0}\frac{\sin 5x}{3x} = \lim_{x \to 0}\frac{\sin 5x}{5x}\cdot\frac{5x}{3x} = 1 \cdot \frac{5}{3} = \frac{5}{3}$$

**Shortcut:** $\lim_{x\to 0}\frac{\sin(ax)}{bx} = \frac{a}{b}$ always.

**Answer: $\frac{5}{3}$**

---

### Q3. Solution
$$\lim_{x \to 0}\frac{e^{2x}-1}{\sin 3x} = \lim_{x \to 0}\frac{e^{2x}-1}{2x}\cdot\frac{3x}{\sin 3x}\cdot\frac{2x}{3x} = 1\cdot 1\cdot\frac{2}{3} = \frac{2}{3}$$

**Shortcut:** $\lim_{x\to 0}\frac{e^{ax}-1}{\sin bx} = \frac{a}{b}$.

**Answer: $\frac{2}{3}$**

---

### Q4. Solution
$$\tan x - \sin x = \sin x\left(\frac{1}{\cos x} - 1\right) = \sin x\cdot\frac{1 - \cos x}{\cos x}$$

$$\frac{\tan x - \sin x}{x^3} = \frac{\sin x}{x}\cdot\frac{1 - \cos x}{x^2}\cdot\frac{1}{\cos x}$$

As $x \to 0$: $\frac{\sin x}{x} \to 1$, $\frac{1-\cos x}{x^2} \to \frac{1}{2}$, $\frac{1}{\cos x} \to 1$.

$$\text{Answer} = 1 \cdot \frac{1}{2} \cdot 1 = \frac{1}{2}$$

**Answer: $\frac{1}{2}$**

---

### Q5. Solution [GATE 2019]
Use Taylor series: $e^x = 1 + x + \frac{x^2}{2} + \cdots$, $\cos x = 1 - \frac{x^2}{2} + \cdots$

Numerator: $x(e^x - 1) + 2(\cos x - 1) = x\left(x + \frac{x^2}{2} + \cdots\right) + 2\left(-\frac{x^2}{2} + \frac{x^4}{24} - \cdots\right)$
$= x^2 + \frac{x^3}{2} + \cdots - x^2 + \frac{x^4}{12} - \cdots = \frac{x^3}{2} + \cdots$

Denominator: $x(\cos x - 1) = x\left(-\frac{x^2}{2} + \cdots\right) = -\frac{x^3}{2} + \cdots$

$$\lim_{x \to 0}\frac{x^3/2 + \cdots}{-x^3/2 + \cdots} = \frac{1/2}{-1/2} = -1$$

**Answer: $-1$**

---

### Q6. Solution
$$\frac{1}{x^2} - \frac{1}{\sin^2 x} = \frac{\sin^2 x - x^2}{x^2 \sin^2 x}$$

Using Taylor: $\sin x = x - \frac{x^3}{6} + \cdots$, so $\sin^2 x = x^2 - \frac{x^4}{3} + \cdots$

Numerator: $\sin^2 x - x^2 = -\frac{x^4}{3} + \cdots$

Denominator: $x^2\sin^2 x = x^2(x^2 - \frac{x^4}{3} + \cdots) = x^4 - \frac{x^6}{3} + \cdots$

$$\lim_{x \to 0}\frac{-x^4/3 + \cdots}{x^4 + \cdots} = -\frac{1}{3}$$

**Answer: $-\frac{1}{3}$**

---

### Q7. Solution [GATE 2017]
$$\lim_{x \to 4}\frac{\sin(x-4)}{x^2-16} = \lim_{x \to 4}\frac{\sin(x-4)}{(x-4)(x+4)}$$

Let $h = x - 4 \to 0$:
$$= \lim_{h \to 0}\frac{\sin h}{h}\cdot\frac{1}{h+4+4} = 1 \cdot \frac{1}{8} = \frac{1}{8}$$

**Answer: $\frac{1}{8}$**

---

### Q8. Solution
Form: $(1 + 3x)^{2/x}$ as $x \to 0$: base $\to 1$, exponent $\to \infty$ → $1^\infty$ form.

Using the formula $e^{\lim g(x)[f(x)-1]}$:

$$e^{\lim_{x \to 0}\frac{2}{x}\cdot 3x} = e^{\lim_{x \to 0} 6} = e^6$$

**Shortcut:** For $\lim_{x\to 0}(1+ax)^{b/x}$, answer is $e^{ab}$.

**Answer: $e^6$**

---

### Q9. Solution
$$\lim_{n \to \infty}\left(1+\frac{2}{n}\right)^{3n} = \lim_{n \to \infty}\left[\left(1+\frac{2}{n}\right)^n\right]^3 = (e^2)^3 = e^6$$

**Shortcut:** $\lim_{n\to\infty}(1+a/n)^{bn} = e^{ab}$.

**Answer: $e^6$**

---

### Q10. Solution [GATE 2014]
Form: $\infty^0$. Let $y = x^{1/x}$, then $\ln y = \frac{\ln x}{x}$.

$$\lim_{x \to \infty}\frac{\ln x}{x} = 0 \quad \text{(log grows slower than polynomial)}$$

So $\ln y \to 0$, hence $y \to e^0 = 1$.

**Answer: 1**

---

### Q11. Solution
Using Taylor: $\ln(1+x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots$

$$\frac{x - \ln(1+x)}{x^2} = \frac{x - x + \frac{x^2}{2} - \frac{x^3}{3} + \cdots}{x^2} = \frac{1}{2} - \frac{x}{3} + \cdots \to \frac{1}{2}$$

**Shortcut:** This is a standard result worth memorizing: $\lim_{x\to 0}\frac{x - \ln(1+x)}{x^2} = \frac{1}{2}$.

**Answer: $\frac{1}{2}$**

---

### Q12. Solution
Form: $0^0$. Let $y = x^x$, then $\ln y = x\ln x$.

$$\lim_{x \to 0^+} x\ln x = \lim_{x \to 0^+}\frac{\ln x}{1/x} \quad \left(\frac{-\infty}{\infty}\right)$$

By L'Hôpital: $= \lim_{x \to 0^+}\frac{1/x}{-1/x^2} = \lim_{x \to 0^+}(-x) = 0$

So $\ln y \to 0$, hence $y \to e^0 = 1$.

**Answer: 1**

---

# SECTION B: CONTINUITY & DIFFERENTIABILITY

---

### Q13. Solution
For continuity at $x = 0$: $\lim_{x \to 0} f(x) = f(0) = k$.

$$\lim_{x \to 0}\frac{\sin x}{x} = 1$$

So $k = 1$.

**Answer: $k = 1$**

---

### Q14. Solution
$f(x) = |x-1| + |x-2|$.

For $x$ near 1: $f(x) = |x-1| + (2-x)$ (since $x < 2$ near $x = 1$).

- RHD at $x = 1$: $f(x) = (x-1) + (2-x) = 1$ for $1 \leq x < 2 \implies f'(1^+) = 0$
- LHD at $x = 1$: $f(x) = (1-x) + (2-x) = 3-2x$ for $x < 1 \implies f'(1^-) = -2$

LHD $\neq$ RHD → **Not differentiable** at $x = 1$.

**Answer: Not differentiable at $x = 1$**

---

### Q15. Solution [GATE 2016]
$f(x) = x^{1/3}$ is continuous everywhere (including $x = 0$).

Check differentiability at $x = 0$:
$$f'(0) = \lim_{h \to 0}\frac{h^{1/3} - 0}{h} = \lim_{h \to 0}\frac{1}{h^{2/3}} = \infty$$

Derivative is infinite → not differentiable (vertical tangent at origin).

**Answer: (B) Continuous but not differentiable at $x = 0$**

---

### Q16. Solution
**Differentiability at $x = 0$:**
$$f'(0) = \lim_{h \to 0}\frac{h^2\sin(1/h)}{h} = \lim_{h \to 0} h\sin(1/h) = 0 \quad \text{(Squeeze Theorem)}$$

Yes, $f$ is differentiable at $x = 0$ with $f'(0) = 0$.

**Is $f'$ continuous at $x = 0$?**

For $x \neq 0$: $f'(x) = 2x\sin(1/x) + x^2 \cdot (-1/x^2)\cos(1/x) = 2x\sin(1/x) - \cos(1/x)$

$\lim_{x\to 0} f'(x) = 0 - \lim_{x \to 0}\cos(1/x)$, which does **not exist** (oscillates between $-1$ and $1$).

Since $\lim_{x\to 0} f'(x) \neq f'(0)$, $f'$ is **not continuous** at $x = 0$.

**Answer: Differentiable at $x = 0$, but $f'$ is not continuous there.**

---

### Q17. Solution
Apply LMVT on $[0, 1]$: $\exists\, c_1 \in (0,1)$ with $f'(c_1) = \frac{f(1)-f(0)}{1-0} = 3$.

Apply LMVT on $[1, 2]$: $\exists\, c_2 \in (1,2)$ with $f'(c_2) = \frac{f(2)-f(1)}{2-1} = 5$.

By LMVT on $f'$ over $[c_1, c_2]$ (if $f'$ is differentiable, which it is since $f$ is twice differentiable):
$\exists\, c \in (c_1, c_2)$ with $f''(c) = \frac{f'(c_2)-f'(c_1)}{c_2-c_1} = \frac{5-3}{c_2-c_1} = \frac{2}{c_2-c_1} > 0$.

So $f''(c) > 0$ for some $c$, not necessarily $= 0$. Counterexample: $f(x) = x^2$ satisfies all conditions but $f''(x) = 2 \neq 0$ everywhere.

**Answer: LMVT gives $f'(c_1) = 3$ for some $c_1 \in (0,1)$ and $f'(c_2) = 5$ for some $c_2 \in (1,2)$.**

---

### Q18. Solution [GATE 2015]
$f(x) = \max(x, x^2)$.

- For $x \leq 0$: $x^2 \geq x$ (since $x^2 \geq 0 \geq x$), so $f(x) = x^2$.
- For $0 \leq x \leq 1$: $x \geq x^2$, so $f(x) = x$.
- For $x \geq 1$: $x^2 \geq x$, so $f(x) = x^2$.

Switch points: $x = 0$ and $x = 1$.

At $x = 0$: LHD = $f'(0^-) = 2(0) = 0$, RHD = $f'(0^+) = 1$. LHD $\neq$ RHD → **not differentiable**.

At $x = 1$: LHD = $f'(1^-) = 1$, RHD = $f'(1^+) = 2(1) = 2$. LHD $\neq$ RHD → **not differentiable**.

**Answer: (C) 2 points**

---

# SECTION C: MEAN VALUE THEOREMS

---

### Q19. Solution
$f(x) = x^2 - 5x + 6$. Check: $f(2) = 4 - 10 + 6 = 0$, $f(3) = 9 - 15 + 6 = 0$. ✓ $f(2) = f(3)$.

$f$ is a polynomial → continuous on $[2,3]$, differentiable on $(2,3)$.

$f'(x) = 2x - 5 = 0 \implies x = 2.5 \in (2, 3)$ ✓

**Answer: $c = 2.5$ satisfies Rolle's Theorem.**

---

### Q20. Solution
$f(x) = \sqrt{x}$. LMVT: $f'(c) = \frac{f(4)-f(1)}{4-1} = \frac{2-1}{3} = \frac{1}{3}$.

$f'(x) = \frac{1}{2\sqrt{x}}$, so $\frac{1}{2\sqrt{c}} = \frac{1}{3} \implies \sqrt{c} = \frac{3}{2} \implies c = \frac{9}{4} = 2.25$.

Check: $c = 2.25 \in (1, 4)$ ✓

**Answer: $c = \frac{9}{4}$**

---

### Q21. Solution [GATE 2013]
Since $f$ is a polynomial of degree 3 → continuous on $[1,3]$, differentiable on $(1,3)$, and $f(1) = f(3) = 0$.

All conditions of **Rolle's Theorem** are satisfied → $\exists$ at least one $c \in (1,3)$ with $f'(c) = 0$.

**Answer: (C) There is at least one $c \in (1, 3)$ such that $f'(c) = 0$.**

---

### Q22. Solution
Let $g(x) = \sin x$. Apply LMVT on $[a, b]$ (WLOG $a < b$):

$$g'(c) = \frac{\sin b - \sin a}{b - a}$$ for some $c \in (a, b)$.

Since $g'(c) = \cos c$ and $|\cos c| \leq 1$:

$$\left|\frac{\sin b - \sin a}{b - a}\right| \leq 1 \implies |\sin a - \sin b| \leq |a - b|$$

**This is a standard LMVT application — memorize the pattern!**

**Answer: Proven using LMVT and $|\cos c| \leq 1$.**

---

### Q23. Solution
By LMVT on $[0, 4]$: $f'(c) = \frac{f(4) - f(0)}{4 - 0} = \frac{f(4) - 2}{4}$ for some $c \in (0, 4)$.

Since $|f'(c)| \leq 3$: $\left|\frac{f(4) - 2}{4}\right| \leq 3 \implies |f(4) - 2| \leq 12$.

$$-10 \leq f(4) \leq 14$$

**Answer: $f(4) \in [-10, 14]$**

---

# SECTION D: MAXIMA & MINIMA

---

### Q24. Solution
$f'(x) = 3x^2 - 12x + 9 = 3(x^2 - 4x + 3) = 3(x-1)(x-3)$

Critical points: $x = 1, 3$.

$f''(x) = 6x - 12$.
- $f''(1) = -6 < 0$ → **local maximum** at $x = 1$, $f(1) = 1 - 6 + 9 + 2 = 6$.
- $f''(3) = 6 > 0$ → **local minimum** at $x = 3$, $f(3) = 27 - 54 + 27 + 2 = 2$.

**Answer: Local max at $x = 1$ ($f = 6$), Local min at $x = 3$ ($f = 2$).**

---

### Q25. Solution
$f'(x) = 4x^3 - 16x = 4x(x^2 - 4) = 4x(x-2)(x+2)$

Critical points: $x = 0, \pm 2$ (all in $[-3, 3]$).

| $x$ | $f(x)$ |
|---|---|
| $-3$ | $81 - 72 + 3 = 12$ |
| $-2$ | $16 - 32 + 3 = -13$ |
| $0$ | $3$ |
| $2$ | $16 - 32 + 3 = -13$ |
| $3$ | $81 - 72 + 3 = 12$ |

**Answer: Absolute max = 12 (at $x = \pm 3$), Absolute min = $-13$ (at $x = \pm 2$).**

---

### Q26. Solution [GATE 2018]
$f(x) = x^3$, $f'(x) = 3x^2$, $f'(0) = 0$ (critical point).

$f''(x) = 6x$, $f''(0) = 0$ — second derivative test inconclusive.

First derivative test: $f'(x) = 3x^2 \geq 0$ for all $x$. Sign does NOT change (positive on both sides). So $x = 0$ is **neither** a max nor a min — it is a point of inflection.

**Answer: (C) Neither max nor min**

---

### Q27. Solution
$f(x) = xe^{-x}$, $f'(x) = e^{-x} - xe^{-x} = e^{-x}(1 - x)$.

$f'(x) = 0 \implies x = 1$ (since $e^{-x} \neq 0$).

$f'$ changes from $+$ (for $x < 1$) to $-$ (for $x > 1$) → **local maximum** at $x = 1$.

Check boundary: $f(0) = 0$ and $\lim_{x \to \infty} xe^{-x} = 0$. So $x = 1$ is the **global maximum** on $[0, \infty)$.

$f(1) = e^{-1} = \frac{1}{e}$.

**Answer: $x = 1$ (maximum value $= 1/e$)**

---

# SECTION E: INTEGRATION

---

### Q28. Solution
$$\int_0^1 (3x^2 + 2x + 1)\,dx = \left[x^3 + x^2 + x\right]_0^1 = (1 + 1 + 1) - 0 = 3$$

**Answer: 3**

---

### Q29. Solution
$$\int_0^{\pi/2}\cos x\,dx = [\sin x]_0^{\pi/2} = 1 - 0 = 1$$

**Answer: 1**

---

### Q30. Solution
Use integration by parts twice. Let $u = x^2$, $dv = e^x dx$.

$$\int_0^1 x^2 e^x\,dx = [x^2 e^x]_0^1 - 2\int_0^1 xe^x\,dx = e - 2\left([xe^x]_0^1 - \int_0^1 e^x\,dx\right)$$
$$= e - 2\left(e - [e^x]_0^1\right) = e - 2(e - (e - 1)) = e - 2(e - e + 1) = e - 2$$

**Answer: $e - 2$**

---

### Q31. Solution [GATE 2012]
Use integration by parts: $u = x$, $dv = \sin x\,dx$.

$$\int_0^{\pi} x\sin x\,dx = [-x\cos x]_0^{\pi} + \int_0^{\pi}\cos x\,dx$$
$$= [-\pi\cos\pi + 0] + [\sin x]_0^{\pi} = [-\pi(-1)] + [0 - 0] = \pi$$

**Answer: $\pi$**

---

### Q32. Solution [GATE 2016]
$f(x) = \cos(x)\sin^2(x)$.

Check: $f(-x) = \cos(-x)\sin^2(-x) = \cos(x)\sin^2(x) = f(x)$... Wait, let's check more carefully.

Actually the integrand is $\cos x \cdot \sin^2 x$. Let $g(x) = \cos x \cdot \sin^2 x$.

$g(-x) = \cos(-x)\sin^2(-x) = \cos x \cdot \sin^2 x = g(x)$ → even function.

So $\int_{-\pi}^{\pi} g(x)\,dx = 2\int_0^{\pi} \cos x\cdot\sin^2 x\,dx$.

Let $u = \sin x$, $du = \cos x\,dx$:

$$2\int_0^{0} u^2\,du = 0$$

(since $\sin 0 = 0$ and $\sin\pi = 0$, both limits give $u = 0$).

**Answer: 0**

**Shortcut:** $\int_0^{\pi}\cos x\cdot\sin^2 x\,dx = \left[\frac{\sin^3 x}{3}\right]_0^{\pi} = 0 - 0 = 0$.

---

### Q33. Solution
Let $I = \int_0^{\pi/2}\frac{\cos x}{\sin x + \cos x}\,dx$.

By **King's rule** ($x \to \pi/2 - x$):

$$I = \int_0^{\pi/2}\frac{\sin x}{\cos x + \sin x}\,dx$$

Add both: $2I = \int_0^{\pi/2}\frac{\sin x + \cos x}{\sin x + \cos x}\,dx = \int_0^{\pi/2} 1\,dx = \frac{\pi}{2}$

$$I = \frac{\pi}{4}$$

**Shortcut:** Any integral of the form $\int_0^{\pi/2}\frac{f(\cos x)}{f(\sin x)+f(\cos x)}\,dx = \frac{\pi}{4}$, provided $f$ is well-behaved.

**Answer: $\frac{\pi}{4}$**

---

### Q34. Solution
Use integration by parts: $u = x$, $dv = e^{-x}dx$.

$$\int_0^{\infty} xe^{-x}\,dx = [-xe^{-x}]_0^{\infty} + \int_0^{\infty} e^{-x}\,dx = 0 + [-e^{-x}]_0^{\infty} = 0 - (-1) = 1$$

**Shortcut:** $\int_0^{\infty} x^n e^{-x}\,dx = n!$ (Gamma function). For $n = 1$: $1! = 1$.

**Answer: 1**

---

### Q35. Solution [GATE 2020]
By Leibniz rule:

$$F'(x) = \sin(x^2)\cdot\frac{d}{dx}(x^2) - \sin(0)\cdot\frac{d}{dx}(0) = \sin(x^2)\cdot 2x = 2x\sin(x^2)$$

**Answer: $2x\sin(x^2)$**

---

### Q36. Solution
$|x^3| = x^3$ for $x \geq 0$ and $|x^3| = -x^3$ for $x < 0$. So $|x^3|$ is an **even function**.

$$\int_{-2}^{2}|x^3|\,dx = 2\int_0^2 x^3\,dx = 2\left[\frac{x^4}{4}\right]_0^2 = 2\cdot\frac{16}{4} = 8$$

**Answer: 8**

---

### Q37. Solution
$$\frac{1}{x^2-1} = \frac{1}{(x-1)(x+1)} = \frac{A}{x-1} + \frac{B}{x+1}$$

$1 = A(x+1) + B(x-1)$. Set $x = 1$: $A = 1/2$. Set $x = -1$: $B = -1/2$.

$$\int\frac{1}{x^2-1}\,dx = \frac{1}{2}\ln|x-1| - \frac{1}{2}\ln|x+1| + C = \frac{1}{2}\ln\left|\frac{x-1}{x+1}\right| + C$$

**Answer: $\frac{1}{2}\ln\left|\frac{x-1}{x+1}\right| + C$**

---

# SECTION F: TAYLOR SERIES

---

### Q38. Solution
$e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots$

Replace $x$ with $-x$:

$$e^{-x} = 1 - x + \frac{x^2}{2} - \frac{x^3}{6} + \cdots$$

**Answer: $1 - x + \frac{x^2}{2} - \frac{x^3}{6}$**

---

### Q39. Solution
$\sin x = x - \frac{x^3}{6} + \frac{x^5}{120} - \frac{x^7}{5040} + \cdots$

$$\sin x - x + \frac{x^3}{6} = \frac{x^5}{120} - \frac{x^7}{5040} + \cdots$$

$$\frac{\sin x - x + x^3/6}{x^5} = \frac{1}{120} - \frac{x^2}{5040} + \cdots \to \frac{1}{120}$$

**Answer: $\frac{1}{120}$**

---

### Q40. Solution [GATE 2022]
Let $g(x) = \ln(1 + x + x^2)$.

Use $\ln(1+u) = u - \frac{u^2}{2} + \frac{u^3}{3} - \cdots$ where $u = x + x^2$.

$$g(x) = (x + x^2) - \frac{(x+x^2)^2}{2} + \frac{(x+x^2)^3}{3} - \cdots$$

Collect $x^3$ terms:
- From $(x + x^2)$: no $x^3$ term.
- From $-\frac{(x+x^2)^2}{2} = -\frac{x^2 + 2x^3 + x^4}{2}$: coefficient of $x^3$ is $-\frac{2}{2} = -1$.
- From $\frac{(x+x^2)^3}{3}$: leading term is $\frac{x^3}{3}$, so coefficient of $x^3$ is $\frac{1}{3}$.

Total coefficient of $x^3$: $-1 + \frac{1}{3} = -\frac{2}{3}$.

**Answer: $-\frac{2}{3}$**

---

### Q41. Solution
$e^{0.1} \approx 1 + 0.1 + \frac{(0.1)^2}{2} + \frac{(0.1)^3}{6} = 1 + 0.1 + 0.005 + 0.000167 = 1.105167$

True value: $e^{0.1} = 1.10517...$

Absolute error $\approx 0.000004$ (extremely small for just 4 terms).

**Shortcut:** For small $x$, even 3 terms of the Maclaurin series of $e^x$ give excellent accuracy.

**Answer: $\approx 1.10517$, error $\approx 4 \times 10^{-6}$**

---

### Q42. Solution
$f(x, y) = e^{x+y}$ at $(0, 0)$: $f = 1$, $f_x = f_y = 1$, $f_{xx} = f_{yy} = f_{xy} = 1$.

$$f(x,y) \approx 1 + x + y + \frac{1}{2}(x^2 + 2xy + y^2) = 1 + (x+y) + \frac{(x+y)^2}{2}$$

**Answer: $1 + (x+y) + \frac{(x+y)^2}{2}$**

---

# SECTION G: PARTIAL DERIVATIVES, GRADIENT & DIRECTIONAL DERIVATIVES

---

### Q43. Solution
$f(x, y) = x^3 y + 2xy^2 - y^3$

$$f_x = 3x^2 y + 2y^2, \quad f_y = x^3 + 4xy - 3y^2$$

**Answer: $f_x = 3x^2 y + 2y^2$, $f_y = x^3 + 4xy - 3y^2$**

---

### Q44. Solution
$f(x, y) = e^x \sin y$.

$$f_x = e^x \sin y, \quad f_{xy} = e^x \cos y$$
$$f_y = e^x \cos y, \quad f_{yx} = e^x \cos y$$

$f_{xy} = f_{yx} = e^x \cos y$ ✓ (Clairaut's theorem verified).

**Answer: $f_{xy} = f_{yx} = e^x\cos y$**

---

### Q45. Solution [GATE 2021]
$f(x, y) = x^2 + 2xy + y^2 = (x+y)^2$.

$$f_x = 2x + 2y, \quad f_y = 2x + 2y$$

$$\nabla f(1, 1) = (2+2,\; 2+2) = (4, 4)$$

**Answer: $(4, 4)$**

---

### Q46. Solution [GATE 2019]
$f(x,y) = x^2 + 3y^2$.

$$\nabla f = (2x, 6y) \implies \nabla f(1, 1) = (2, 6)$$

Direction $(1,1)$: unit vector $\hat{u} = \frac{1}{\sqrt{2}}(1, 1)$.

$$D_{\hat{u}}f = (2, 6)\cdot\frac{1}{\sqrt{2}}(1, 1) = \frac{2+6}{\sqrt{2}} = \frac{8}{\sqrt{2}} = 4\sqrt{2}$$

**Answer: $4\sqrt{2}$**

---

### Q47. Solution
$f(x,y,z) = x^2 + yz$.

$$\nabla f = (2x, z, y) \implies \nabla f(1, 2, 3) = (2, 3, 2)$$

Maximum rate of change = $\|\nabla f\| = \sqrt{4 + 9 + 4} = \sqrt{17}$.

**Answer: $\sqrt{17}$**

---

### Q48. Solution [GATE 2023]
$f(x,y) = x^2y + xy^3$.

$$f_x = 2xy + y^3$$
$$f_{xy} = \frac{\partial}{\partial y}(2xy + y^3) = 2x + 3y^2$$

At $(1,1)$: $f_{xy} = 2(1) + 3(1)^2 = 5$.

**Answer: 5**

---

### Q49. Solution
$f(x,y) = \ln(x^2 + y^2)$.

$$f_x = \frac{2x}{x^2+y^2}, \quad f_y = \frac{2y}{x^2+y^2}$$

$$\nabla f(1, 0) = \left(\frac{2}{1}, \frac{0}{1}\right) = (2, 0)$$

Direction $\hat{j} = (0,1)$ is already a unit vector.

$$D_{\hat{j}}f = (2, 0)\cdot(0, 1) = 0$$

**Interpretation:** The function doesn't change in the $y$-direction at $(1,0)$ — this makes sense since $(1,0)$ lies on the $x$-axis and the function is symmetric in $y$ around it.

**Answer: $\nabla f(1,0) = (2, 0)$; directional derivative in $\hat{j}$ direction $= 0$.**

---

# SECTION H: UNCONSTRAINED OPTIMIZATION (HESSIAN TEST)

---

### Q50. Solution
$f_x = 2x - 2 = 0 \implies x = 1$, $f_y = 2y - 4 = 0 \implies y = 2$.

Critical point: $(1, 2)$.

Hessian: $H = \begin{bmatrix} f_{xx} & f_{xy} \\ f_{xy} & f_{yy} \end{bmatrix} = \begin{bmatrix} 2 & 0 \\ 0 & 2 \end{bmatrix}$

$\det(H) = 4 > 0$ and $f_{xx} = 2 > 0$ → **local minimum**.

$f(1, 2) = 1 + 4 - 2 - 8 + 5 = 0$.

**Answer: Local minimum at $(1, 2)$ with $f = 0$.**

---

### Q51. Solution [GATE 2023]
$f_x = 2x + y - 3 = 0$, $f_y = 2y + x = 0$.

From second equation: $x = -2y$. Substitute: $2(-2y) + y - 3 = 0 \implies -3y = 3 \implies y = -1$, $x = 2$.

Critical point: $(2, -1)$.

Hessian: $H = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$, $\det(H) = 4 - 1 = 3 > 0$ and $f_{xx} = 2 > 0$ → **local minimum**.

$f(2, -1) = 4 + 1 + (-2) - 6 = -3$.

**Answer: Minimum value $= -3$ at $(2, -1)$.**

---

### Q52. Solution
$f(x,y) = x^3 + y^3 - 3xy$.

$f_x = 3x^2 - 3y = 0 \implies y = x^2$
$f_y = 3y^2 - 3x = 0 \implies x = y^2$

Substitute $y = x^2$ into $x = y^2$: $x = x^4 \implies x(x^3 - 1) = 0 \implies x = 0$ or $x = 1$.

Critical points: $(0, 0)$ and $(1, 1)$.

At $(1, 1)$: $H = \begin{bmatrix} 6x & -3 \\ -3 & 6y \end{bmatrix}_{(1,1)} = \begin{bmatrix} 6 & -3 \\ -3 & 6 \end{bmatrix}$

$\det(H) = 36 - 9 = 27 > 0$, $f_{xx} = 6 > 0$ → **local minimum**.

$f(1,1) = 1 + 1 - 3 = -1$.

**At $(0, 0)$:** $H = \begin{bmatrix} 0 & -3 \\ -3 & 0 \end{bmatrix}$, $\det(H) = -9 < 0$ → **saddle point**.

**Answer: $(1,1)$ is a local minimum ($f = -1$); $(0,0)$ is a saddle point.**

---

### Q53. Solution [GATE 2022]
$f_x = 4x^3 - 4y = 0 \implies y = x^3$
$f_y = 4y^3 - 4x = 0 \implies x = y^3$

Substitute: $x = (x^3)^3 = x^9 \implies x^9 - x = 0 \implies x(x^8 - 1) = 0$.

Real solutions: $x = 0, 1, -1$.

Critical points: $(0, 0)$, $(1, 1)$, $(-1, -1)$.

Hessian: $H = \begin{bmatrix} 12x^2 & -4 \\ -4 & 12y^2 \end{bmatrix}$

**At $(0,0)$:** $\det(H) = 0 - 16 = -16 < 0$ → **saddle point**.

**At $(1,1)$:** $\det(H) = 144 - 16 = 128 > 0$, $f_{xx} = 12 > 0$ → **local minimum**. $f(1,1) = 1 + 1 - 4 + 1 = -1$.

**At $(-1,-1)$:** $\det(H) = 144 - 16 = 128 > 0$, $f_{xx} = 12 > 0$ → **local minimum**. $f(-1,-1) = 1 + 1 + 4 + 1 = 7$. Wait: $f(-1,-1) = 1 + 1 - 4(-1)(-1) + 1 = 1 + 1 - 4 + 1 = -1$.

**Answer: $(0,0)$ is saddle; $(1,1)$ and $(-1,-1)$ are local minima with $f = -1$.**

---

# SECTION I: CONVEX FUNCTIONS

---

### Q54. Solution
Need to show: $(\lambda x_1 + (1-\lambda)x_2)^2 \leq \lambda x_1^2 + (1-\lambda)x_2^2$ for $\lambda \in [0,1]$.

Expand LHS: $\lambda^2 x_1^2 + 2\lambda(1-\lambda)x_1 x_2 + (1-\lambda)^2 x_2^2$.

$\text{RHS} - \text{LHS} = \lambda(1-\lambda)x_1^2 - 2\lambda(1-\lambda)x_1 x_2 + \lambda(1-\lambda)x_2^2 = \lambda(1-\lambda)(x_1 - x_2)^2 \geq 0$.

Since $\lambda \in [0,1]$, both $\lambda \geq 0$ and $(1-\lambda) \geq 0$, so $\text{RHS} - \text{LHS} \geq 0$. ✓

**Answer: Proven. The key identity is $\text{RHS} - \text{LHS} = \lambda(1-\lambda)(x_1-x_2)^2 \geq 0$.**

---

### Q55. Solution
$f(x,y) = x^2 + y^2 + xy$.

Hessian: $H = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$

Eigenvalues: $(2 - \lambda)^2 - 1 = 0 \implies \lambda = 1, 3$.

Both eigenvalues are positive → $H$ is **positive definite** everywhere → $f$ is **strictly convex**.

**Shortcut:** For a quadratic $f(\mathbf{x}) = \mathbf{x}^T A\mathbf{x} + \cdots$, convexity depends only on $A$. Check if $A$ is PD: $\det \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix} = 3 > 0$ and leading entry $2 > 0$ → PD → convex.

**Answer: Strictly convex (Hessian is positive definite).**

---

### Q56. Solution [GATE 2024]
Check $f''(x)$ for each:

(A) $f(x) = x^3$: $f''(x) = 6x$, changes sign → **not convex**.
(B) $f(x) = e^{-x}$: $f''(x) = e^{-x} > 0$ for all $x$ → **convex** ✓
(C) $f(x) = \ln x$: $f''(x) = -1/x^2 < 0$ → **concave**, not convex.
(D) $f(x) = -x^2$: $f''(x) = -2 < 0$ → **concave**, not convex.

**Answer: (B) $e^{-x}$**

---

# SECTION J: LAGRANGE MULTIPLIERS

---

### Q57. Solution
Minimize $f = x^2 + y^2$ subject to $g: x + y - 1 = 0$.

Lagrangian: $\mathcal{L} = x^2 + y^2 - \lambda(x + y - 1)$.

$$\frac{\partial\mathcal{L}}{\partial x} = 2x - \lambda = 0 \implies x = \lambda/2$$
$$\frac{\partial\mathcal{L}}{\partial y} = 2y - \lambda = 0 \implies y = \lambda/2$$

Constraint: $x + y = 1 \implies \lambda/2 + \lambda/2 = 1 \implies \lambda = 1$.

$(x, y) = (1/2, 1/2)$, $f = 1/4 + 1/4 = 1/2$.

**Answer: Minimum $= \frac{1}{2}$ at $(x, y) = (\frac{1}{2}, \frac{1}{2})$.**

---

### Q58. Solution [GATE 2020]
Maximize $f = xy$ subject to $g: x + y - 10 = 0$.

Lagrangian: $\mathcal{L} = xy - \lambda(x + y - 10)$.

$$\frac{\partial\mathcal{L}}{\partial x} = y - \lambda = 0 \implies \lambda = y$$
$$\frac{\partial\mathcal{L}}{\partial y} = x - \lambda = 0 \implies \lambda = x$$

So $x = y$. Constraint: $2x = 10 \implies x = y = 5$.

$f(5, 5) = 25$.

**Shortcut (AM-GM):** $xy \leq \left(\frac{x+y}{2}\right)^2 = 25$, equality when $x = y$.

**Answer: Maximum value $= 25$ at $(5, 5)$.**

---

### Q59. Solution
Box with no top: dimensions $x, y, z$ (base $x \times y$, height $z$).

Volume: $V = xyz$. Surface area (no top): $g = xy + 2xz + 2yz = 12$.

Lagrangian: $\mathcal{L} = xyz - \lambda(xy + 2xz + 2yz - 12)$.

$$\frac{\partial\mathcal{L}}{\partial x} = yz - \lambda(y + 2z) = 0 \quad \cdots (1)$$
$$\frac{\partial\mathcal{L}}{\partial y} = xz - \lambda(x + 2z) = 0 \quad \cdots (2)$$
$$\frac{\partial\mathcal{L}}{\partial z} = xy - \lambda(2x + 2y) = 0 \quad \cdots (3)$$

From (1): $\lambda = \frac{yz}{y + 2z}$. From (2): $\lambda = \frac{xz}{x + 2z}$.

Setting equal: $\frac{yz}{y+2z} = \frac{xz}{x+2z}$. Cancel $z$: $\frac{y}{y+2z} = \frac{x}{x+2z}$.

Cross-multiply: $y(x+2z) = x(y+2z) \implies 2yz = 2xz \implies x = y$.

From (1) and (3) with $x = y$: $\frac{xz}{x+2z} = \frac{x^2}{4x} = \frac{x}{4}$.

So $\frac{xz}{x+2z} = \frac{x}{4} \implies 4z = x + 2z \implies x = 2z$.

Constraint: $x^2 + 2x(x/2) + 2x(x/2) = x^2 + x^2 + x^2 = 3x^2 = 12 \implies x = 2$.

$(x, y, z) = (2, 2, 1)$, $V = 4$.

**Answer: Dimensions $2 \times 2 \times 1$, maximum volume $= 4$ m³.**

---

### Q60. Solution [GATE 2023]
Minimize $f = x^2 + y^2 + z^2$ subject to $g: x + y + z = 3$.

By symmetry and Lagrange multipliers:

$\nabla f = \lambda \nabla g \implies (2x, 2y, 2z) = \lambda(1, 1, 1) \implies x = y = z = \lambda/2$.

Constraint: $3(\lambda/2) = 3 \implies \lambda = 2$, so $x = y = z = 1$.

$f(1,1,1) = 3$.

**Shortcut (Cauchy-Schwarz):** By QM-AM inequality: $\frac{x^2+y^2+z^2}{3} \geq \left(\frac{x+y+z}{3}\right)^2 = 1$, so $x^2+y^2+z^2 \geq 3$, equality when $x=y=z$.

**Answer: Minimum value $= 3$ at $(1, 1, 1)$.**

---

# SECTION K: KKT CONDITIONS

---

### Q61. Solution
Minimize $f(x) = (x-3)^2$ subject to $g(x) = x - 1 \leq 0$ (i.e., $x \leq 1$).

**KKT Conditions:**
1. **Stationarity:** $f'(x) + \mu g'(x) = 0 \implies 2(x-3) + \mu = 0$
2. **Primal feasibility:** $x \leq 1$
3. **Dual feasibility:** $\mu \geq 0$
4. **Complementary slackness:** $\mu(x - 1) = 0$

**Case 1: $\mu = 0$** (constraint inactive). Then $2(x-3) = 0 \implies x = 3$. But $x = 3 > 1$ violates primal feasibility. ✗

**Case 2: $x = 1$** (constraint active). Then $2(1-3) + \mu = 0 \implies \mu = 4 > 0$ ✓

All KKT conditions satisfied at $x^* = 1$, $\mu^* = 4$.

$f(1) = (1-3)^2 = 4$.

**Answer: $x^* = 1$, minimum value $= 4$.**

---

### Q62. Solution
Minimize $f = x^2 + y^2$ subject to $g_1: -(x+y-4) \leq 0$ (i.e., $x+y \geq 4$), $g_2: -x \leq 0$, $g_3: -y \leq 0$.

Rewrite: $g_1 = 4 - x - y \leq 0$, $g_2 = -x \leq 0$, $g_3 = -y \leq 0$.

**KKT Conditions:**

Stationarity: $\nabla f = \mu_1 \nabla g_1 + \mu_2 \nabla g_2 + \mu_3 \nabla g_3$

$(2x, 2y) = \mu_1(-1, -1) + \mu_2(-1, 0) + \mu_3(0, -1)$

$2x = -\mu_1 - \mu_2$, $2y = -\mu_1 - \mu_3$

By symmetry of the objective and constraint $g_1$, try $x = y$:

$x + y = 4 \implies x = y = 2$ (assume $g_1$ active, $g_2, g_3$ inactive so $\mu_2 = \mu_3 = 0$).

$2(2) = -\mu_1 \implies \mu_1 = -4$.

But $\mu_1 \geq 0$ is required! The issue is the sign convention. Let's use standard form: minimize $f$ subject to $h(x,y) = x + y - 4 \geq 0$.

The Lagrangian: $\mathcal{L} = x^2 + y^2 - \lambda(x + y - 4)$ with $\lambda \geq 0$.

$2x - \lambda = 0 \implies x = \lambda/2$
$2y - \lambda = 0 \implies y = \lambda/2$

$x + y = 4 \implies \lambda = 4$, so $x = y = 2$, $\lambda = 4 > 0$ ✓

$f(2, 2) = 8$.

**Answer: Minimum value $= 8$ at $(2, 2)$.**

---

### Q63. Solution [GATE 2024]
Minimize $f(x) = x^2 + 1$ subject to $x \leq 2$.

Unconstrained minimum of $f$ is at $x = 0$ (since $f'(x) = 2x = 0$), and $x = 0 \leq 2$ satisfies the constraint.

So the constraint is **inactive** and the solution is $x^* = 0$.

**Verification via KKT:** $\mu = 0$ (constraint inactive), $f'(0) = 0$ ✓, $0 \leq 2$ ✓.

**Answer: (A) $x^* = 0$**

---

# SECTION L: GRADIENT DESCENT

---

### Q64. Solution
$f(x) = (x-3)^2$, $f'(x) = 2(x-3)$.

$x_1 = x_0 - \alpha f'(x_0) = 0 - 0.1 \cdot 2(0-3) = 0 - 0.1(-6) = 0.6$

**Answer: $x_1 = 0.6$** (moving toward the minimum at $x = 3$).

---

### Q65. Solution
$f(x,y) = x^2 + 4y^2$. $\nabla f = (2x, 8y)$.

At $(4, 2)$: $\nabla f = (8, 16)$.

$(x_1, y_1) = (4, 2) - 0.1(8, 16) = (4 - 0.8, 2 - 1.6) = (3.2, 0.4)$

After step 1: $|x_1| = 3.2 > |y_1| = 0.4$. But the $y$-gradient is $4\times$ the $x$-gradient for the same coordinate value, so $y$ shrinks faster.

General pattern: $x_k = 4(1-0.2)^k = 4(0.8)^k$, $y_k = 2(1-0.8)^k = 2(0.2)^k$.

$|x_k| < |y_k|$: $4(0.8)^k < 2(0.2)^k \implies 2 < (0.25)^k \implies k\ln(0.25) < \ln 2 \implies k > \frac{\ln 2}{\ln 4} = 0.5$.

At $k = 1$: $|x| = 3.2 > |y| = 0.4$ → $|x|$ is already greater. Actually $y$ shrinks much faster, so $|x| > |y|$ for all $k \geq 1$. The question "when does $|x| < |y|$" — it never happens with these parameters, since $y$ converges to 0 much faster.

**Answer: After 1 step: $(3.2, 0.4)$. $|x|$ remains greater than $|y|$ for all subsequent steps.**

---

### Q66. Solution [GATE 2024]
$f(x) = x^4 - 4x^2$. $f'(x) = 4x^3 - 8x$.

$f'(3) = 4(27) - 8(3) = 108 - 24 = 84$.

$x_1 = x_0 - \alpha f'(x_0) = 3 - 0.01(84) = 3 - 0.84 = 2.16$

**Answer: $x_1 = 2.16$**

---

### Q67. Solution [Comprehensive]
$f(x,y) = 2x^2 + y^2 - 2xy - 4x$.

**(a) Critical point:**
$f_x = 4x - 2y - 4 = 0$
$f_y = 2y - 2x = 0 \implies y = x$

Substitute: $4x - 2x - 4 = 0 \implies x = 2$, $y = 2$.

Critical point: $(2, 2)$.

**(b) Hessian test:**
$$H = \begin{bmatrix} 4 & -2 \\ -2 & 2 \end{bmatrix}$$

$\det(H) = 8 - 4 = 4 > 0$ and $f_{xx} = 4 > 0$ → **local minimum** ✓

$f(2,2) = 8 + 4 - 8 - 8 = -4$.

**(c) Gradient descent from $(0,0)$ with $\alpha = 0.1$:**

$\nabla f = (4x - 2y - 4,\; 2y - 2x)$.

**Iteration 1:** $\nabla f(0,0) = (-4, 0)$.
$(x_1, y_1) = (0,0) - 0.1(-4, 0) = (0.4, 0)$.

**Iteration 2:** $\nabla f(0.4, 0) = (1.6 - 0 - 4,\; 0 - 0.8) = (-2.4, -0.8)$.
$(x_2, y_2) = (0.4, 0) - 0.1(-2.4, -0.8) = (0.64, 0.08)$.

Both $(0.4, 0)$ and $(0.64, 0.08)$ are moving toward $(2, 2)$ → **yes, converging**.

**Answer: Critical point $(2,2)$ is a local min with $f = -4$. GD converges toward it.**

---

### Q68. Solution [Mixed]
$f(x,y) = x^2 + y^2$, constraint $g(x,y) = x + y - 2 = 0$.

**(a) Lagrange multipliers:**

$\nabla f = \lambda \nabla g$: $(2x, 2y) = \lambda(1, 1) \implies x = y = \lambda/2$.

$x + y = 2 \implies \lambda = 2$, so $x = y = 1$, $f(1,1) = 2$.

**(b) KKT for $g(x,y) = x + y - 2 \leq 0$:**

$\mathcal{L} = x^2 + y^2 + \mu(x + y - 2)$, $\mu \geq 0$.

- $2x + \mu = 0 \implies x = -\mu/2$
- $2y + \mu = 0 \implies y = -\mu/2$
- Complementary slackness: $\mu(x+y-2) = 0$
- Primal: $x + y \leq 2$

If $\mu = 0$: $x = y = 0$, $f = 0$, and $0 + 0 - 2 = -2 \leq 0$ ✓. This is the unconstrained minimum, which is **inside** the feasible region.

So the minimum of $f$ subject to $x + y \leq 2$ is $f = 0$ at $(0, 0)$.

**(c) Verification:**

The minimum of $f$ subject to $g = 0$ (equality) is at $(1,1)$ with $f = 2$. This lies on the **boundary** of $g \leq 0$ (since $g(1,1) = 0$). The minimum of $f$ subject to $g \leq 0$ is at $(0,0)$ with $f = 0$, which is in the **interior** of the feasible region. The constrained equality minimum is necessarily $\geq$ the inequality minimum.

**Answer: (a) Min $= 2$ at $(1,1)$. (b) Unconstrained min $(0,0)$ is feasible, so min $= 0$. (c) Equality minimum lies on boundary.**
