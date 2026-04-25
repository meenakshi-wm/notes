# Probability & Statistics — Questions and PYQs for GATE 2027

---

> **70+ Questions** | Easy → Medium → GATE PYQ Level  
> Organized by topic | All answers in `3_Solutions.md`

---

## Section A: Probability Basics, Inclusion-Exclusion, De Morgan (Q1–Q8)

---

**Q1. [Easy]** A bag contains 5 red, 4 blue, and 3 green balls. One ball is drawn at random. Find $P(\text{Red or Green})$.

---

**Q2. [Easy]** In a group of 100 people, 60 like tea, 40 like coffee, 20 like both. How many like neither?

---

**Q3. [Medium]** In a class of 200 students: 120 passed Maths, 90 passed Physics, 60 passed Chemistry, 50 passed both Maths and Physics, 40 passed both Maths and Chemistry, 30 passed both Physics and Chemistry, and 20 passed all three. How many passed at least one subject?

---

**Q4. [Medium]** Events $A$ and $B$ satisfy $P(A) = 0.6$, $P(B) = 0.5$, $P(A \cup B) = 0.8$. Find $P(A \cap B)$, $P(A^c \cap B^c)$, $P(A \cap B^c)$.

---

**Q5. [GATE Style]** Two dice are rolled. Let $A$ = "sum is 7", $B$ = "at least one die shows 4". Find $P(A \cup B)$.

---

**Q6. [GATE Style]** If $P(A) = 0.4$, $P(B) = 0.3$, and $A$ and $B$ are mutually exclusive, find $P(A^c \cap B^c)$.

---

**Q7. [Medium]** Using De Morgan's law, express $P(\overline{A \cup B \cup C})$ in terms of intersections.

---

**Q8. [Easy]** Three events $A$, $B$, $C$ have $P(A) = P(B) = P(C) = 1/4$. All pairs have intersection probability $1/16$ and $P(A \cap B \cap C) = 1/32$. Find $P(A \cup B \cup C)$.

---

## Section B: Conditional Probability, Total Probability, Bayes' Theorem (Q9–Q22)

---

**Q9. [Easy]** A bag has 4 white and 6 black balls. Two balls are drawn without replacement. Find $P(\text{both white})$.

---

**Q10. [Easy]** A card is drawn from a standard deck. Given that it's a face card, what is the probability it's a King?

---

**Q11. [Medium]** Box 1: 3 red, 2 blue. Box 2: 4 red, 6 blue. A box is chosen at random, then a ball is drawn. Find $P(\text{Red})$.

---

**Q12. [Medium]** In Q11, given that the ball drawn is Red, what is the probability it came from Box 1? (Bayes' theorem)

---

**Q13. [GATE PYQ Style]** A factory has 3 machines producing 50%, 30%, 20% of total output. Defect rates are 3%, 4%, 5% respectively. A randomly selected item is defective. Find $P(\text{Machine 1})$.

---

**Q14. [GATE PYQ Style]** A diagnostic test for a disease has sensitivity 95% and specificity 90%. The disease prevalence is 2%. A person tests positive. What is $P(\text{actually has disease})$?

---

**Q15. [GATE Style]** A coin is selected at random from three coins: fair coin, two-headed coin, and a biased coin with $P(H) = 3/4$. The selected coin is tossed twice and both show Heads. What is the probability the selected coin was the fair one?

---

**Q16. [Hard]** A person has two children. Given that at least one child is a boy born on a Tuesday, what is the probability both children are boys? (Assume 7 days equally likely, boy/girl equally likely, independence.)

---

**Q17. [Medium]** A student is preparing for an exam. If they study (prob 0.7), they pass with prob 0.9. If they don't study, they pass with prob 0.3. Given that the student passed, what is $P(\text{studied})$?

---

**Q18. [GATE Style]** Bag A: 5 white, 3 black. Bag B: 4 white, 6 black. A ball is drawn from A and transferred to B, then a ball is drawn from B. Find $P(\text{ball drawn from B is white})$.

---

**Q19. [Medium]** Three prisoners problem: Among 3 prisoners A, B, C, one will be pardoned. A asks the guard to name someone (other than A) who will NOT be pardoned. The guard says "B". Find $P(\text{A is pardoned} | \text{guard says B})$.

---

**Q20. [Easy]** $P(A) = 0.5$, $P(B|A) = 0.6$, $P(B|A^c) = 0.2$. Find $P(B)$.

---

**Q21. [GATE Style]** In a communication channel, bit 0 is sent with probability 0.6 and bit 1 with probability 0.4. The probability of error (flipping) is 0.1 for bit 0 and 0.2 for bit 1. If the received bit is 1, find $P(\text{sent was 1})$.

---

**Q22. [Hard]** An urn contains 5 red and 5 blue balls. Balls are drawn one at a time without replacement. What is $P(\text{3rd ball drawn is red})$?

---

## Section C: Independence (Q23–Q26)

---

**Q23. [Easy]** Events $A$ and $B$ are independent with $P(A) = 0.3$, $P(B) = 0.5$. Find $P(A \cap B)$, $P(A \cup B)$, $P(A|B)$.

---

**Q24. [Medium]** Two fair dice rolled. $A$: "first die is even", $B$: "sum is even". Are $A$ and $B$ independent?

---

**Q25. [GATE Style]** $A$, $B$, $C$ are events with $P(A) = P(B) = P(C) = 1/2$ and $P(A \cap B) = P(B \cap C) = P(A \cap C) = 1/4$ but $P(A \cap B \cap C) = 1/4$. Are they mutually independent?

---

**Q26. [Medium]** A system has two components in series. Each works independently with probability 0.9. What is $P(\text{system works})$? What if they are in parallel?

---

## Section D: Random Variables, PMF, Expectation (Q27–Q38)

---

**Q27. [Easy]** Roll a fair die. Let $X = |3 - \text{outcome}|$. Find the PMF of $X$.

---

**Q28. [Easy]** Two fair coins tossed. $X$ = number of heads. Find PMF, $E[X]$, $\text{Var}(X)$.

---

**Q29. [Medium]** A random variable $X$ has PMF: $P(X=k) = c/k$ for $k = 1, 2, 3, 4, 5$ and 0 otherwise. Find $c$ and $E[X]$.

---

**Q30. [GATE Style]** A bag has 3 red and 2 blue balls. Draw 2 without replacement. Let $X$ = number of red balls. Find $E[X]$ using the indicator variable method.

---

**Q31. [GATE PYQ Style]** $X$ has PMF $P(X = k) = (1/2)^k$ for $k = 1, 2, 3, \ldots$. Find $E[X]$.

---

**Q32. [Medium]** $E[X] = 5$, $E[X^2] = 30$. Find $\text{Var}(X)$ and $\text{Var}(2X - 3)$.

---

**Q33. [GATE Style]** Expected number of tosses of a fair coin to get 3 consecutive heads. (Recursive method)

---

**Q34. [Hard]** A fair die is rolled repeatedly until a 6 appears. Let $X$ be the number of rolls. Find $E[X]$ and $\text{Var}(X)$.

---

**Q35. [GATE Style]** Using LOTUS: $X$ is uniform on $\{1, 2, 3, 4\}$. Find $E[X^2 + 2X]$.

---

**Q36. [Medium]** CDF of $X$ is: $F(x) = 0$ for $x < 0$; $F(x) = x/4$ for $0 \leq x < 2$; $F(x) = x/4 + 1/4$ for $2 \leq x < 3$; $F(x) = 1$ for $x \geq 3$. Find $P(X = 2)$, $P(1 < X \leq 2.5)$.

---

**Q37. [GATE Style]** $X$ and $Y$ are independent. $X$ takes values 1, 2, 3 equally likely. $Y$ takes values 0, 1 equally likely. Find $E[XY]$ and $\text{Var}(X + Y)$.

---

**Q38. [Medium]** Coupon collector: There are 3 types of coupons, each equally likely. Find the expected number of draws to collect all 3 types.

---

## Section E: Discrete Distributions (Q39–Q47)

---

**Q39. [Easy]** $X \sim \text{Binomial}(10, 0.4)$. Find $E[X]$, $\text{Var}(X)$, and $P(X = 3)$.

---

**Q40. [Medium]** A multiple-choice exam has 20 questions, each with 4 options. A student guesses all. $P(\text{at least 8 correct})$? Set up the expression.

---

**Q41. [GATE PYQ Style]** Typos on a page follow Poisson(2). Find $P(\text{at most 1 typo on a page})$.

---

**Q42. [GATE Style]** If $X \sim \text{Poisson}(\lambda)$ and $P(X = 0) = P(X = 1)$, find $\lambda$ and $P(X = 2)$.

---

**Q43. [Medium]** Calls arrive at a call center at rate 4 per minute (Poisson). Find $P(\text{no calls in 30 seconds})$.

---

**Q44. [Easy]** $X \sim \text{Geometric}(1/3)$. Find $P(X = 4)$, $E[X]$, $P(X > 3)$.

---

**Q45. [GATE Style]** Memoryless property: $X \sim \text{Geometric}(p)$. Show that $P(X > m + n | X > m) = P(X > n)$.

---

**Q46. [GATE Style]** A biased coin ($P(H) = p$) is tossed until first head. Let $N$ = number of tosses. Find $P(N \text{ is even})$.

---

**Q47. [Medium]** Bernoulli trials with $p = 0.6$. Find $P(\text{first success on 4th or 5th trial})$.

---

## Section F: Continuous Distributions (Q48–Q57)

---

**Q48. [Easy]** $X \sim \text{Uniform}(2, 8)$. Find $E[X]$, $\text{Var}(X)$, $P(3 < X < 6)$.

---

**Q49. [Medium]** $f(x) = cx^2$ for $0 \leq x \leq 3$, else 0. Find $c$, $E[X]$, $P(X > 2)$.

---

**Q50. [GATE PYQ Style]** $X \sim N(50, 100)$. Find $P(40 < X < 65)$. (Use $\Phi(1) = 0.8413$, $\Phi(1.5) = 0.9332$)

---

**Q51. [GATE Style]** Lifetimes are exponentially distributed with mean 500 hours. Find $P(\text{lasts between 400 and 600 hours})$.

---

**Q52. [Medium]** $X \sim \text{Exp}(\lambda)$. Show that Median $= \frac{\ln 2}{\lambda}$.

---

**Q53. [GATE PYQ Style]** $X \sim N(\mu, \sigma^2)$. If $P(X > 10) = 0.1587$ and $P(X > 20) = 0.0228$, find $\mu$ and $\sigma$.

---

**Q54. [GATE Style]** $X \sim \text{Exp}(2)$. Find $E[X^2]$ and $P(X > 1 | X > 0.5)$.

---

**Q55. [Medium]** CDF: $F(x) = 1 - e^{-x^2/2}$ for $x \geq 0$. Find the PDF $f(x)$ and check if this is a known distribution.

---

**Q56. [Hard]** If $X \sim N(0,1)$, find $E[X^2]$ and $E[|X|]$.

---

**Q57. [GATE Style]** $X$ has PDF $f(x) = 2e^{-2x}$ for $x > 0$. Find the CDF and $P(1 < X < 3)$.

---

## Section G: Joint Distributions, Covariance, Correlation (Q58–Q67)

---

**Q58. [Easy]** Joint PMF:

| $X \backslash Y$ | 0 | 1 |
|---|---|---|
| 0 | 0.1 | 0.3 |
| 1 | 0.2 | 0.4 |

Find marginal PMFs. Are $X, Y$ independent?

---

**Q59. [Medium]** For Q58, find $E[X]$, $E[Y]$, $E[XY]$, $\text{Cov}(X,Y)$, $\rho(X,Y)$.

---

**Q60. [GATE Style]** Joint PDF: $f(x,y) = 4xy$ for $0 < x < 1, 0 < y < 1$, else 0. Find marginal PDFs, $E[X]$, and check independence.

---

**Q61. [Medium]** $f(x,y) = 6x$ for $0 < x < y < 1$. Find $f_X(x)$, $f_Y(y)$, $E[X]$, $E[Y]$.

---

**Q62. [GATE PYQ Style]** $X, Y$ are i.i.d. $\text{Uniform}(0,1)$. Find $P(X + Y > 1)$ and $E[\max(X,Y)]$.

---

**Q63. [GATE Style]** Conditional expectation: Roll a die to get $N$, then toss a coin $N$ times. Let $X$ = number of heads. Find $\text{Var}(X)$ using the law of total variance.

---

**Q64. [Medium]** $\text{Cov}(X+Y, X-Y)$ where $\text{Var}(X) = 4$, $\text{Var}(Y) = 9$, $\text{Cov}(X,Y) = -2$.

---

**Q65. [GATE Style]** Random vector $(X, Y)$ has covariance matrix $\Sigma = \begin{pmatrix} 4 & 1 \\ 1 & 9 \end{pmatrix}$. Find $\text{Var}(2X - Y)$.

---

**Q66. [Hard]** $X \sim \text{Uniform}(-1, 1)$, $Y = X^2$. Find $\text{Cov}(X,Y)$ and $\rho(X,Y)$. Are they independent?

---

**Q67. [GATE Style]** If $X$ and $Y$ have correlation $\rho = 0.6$, $\sigma_X = 2$, $\sigma_Y = 3$, find $\text{Var}(X - 2Y)$.

---

## Section H: Central Limit Theorem & Confidence Intervals (Q68–Q73)

---

**Q68. [Medium]** $X_1, \ldots, X_{100}$ i.i.d. with $\mu = 10$, $\sigma = 5$. Find $P(\bar{X} > 10.5)$ using CLT.

---

**Q69. [GATE PYQ Style]** A fair die rolled 360 times. Let $S$ = sum of outcomes. Find $P(S > 1300)$.
(Hint: Mean of single die = 3.5, Var = 35/12)

---

**Q70. [GATE Style]** 64 bulbs have mean life 1200 hours, SD 200 hours. Find 95% CI for the population mean.

---

**Q71. [Medium]** A sample of 25 gives $\bar{x} = 82$, $s = 12$. Construct a 99% CI using t-distribution. ($t_{0.005, 24} = 2.797$)

---

**Q72. [GATE Style]** How many samples needed for a 95% CI with margin of error 3, if $\sigma = 15$?

---

**Q73. [Medium]** 100 light bulbs tested: $\bar{x} = 1570$ hours, $\sigma = 120$. Construct a 90% CI.

---

## Section I: Hypothesis Testing & Chi-Squared (Q74–Q82)

---

**Q74. [Medium]** A company claims average battery life is 500 hours. Sample of 50 gives $\bar{x} = 490$, $\sigma = 40$. Test at $\alpha = 0.05$ (two-tailed).

---

**Q75. [GATE PYQ Style]** $H_0: \mu = 30$, $H_1: \mu > 30$. $n = 16$, $\bar{x} = 33$, $S = 8$. Use t-test at $\alpha = 0.05$. ($t_{0.05, 15} = 1.753$)

---

**Q76. [Medium]** Define p-value. If a two-tailed z-test gives $Z = 1.8$, find the p-value and decide at $\alpha = 0.05$. ($\Phi(1.8) = 0.9641$)

---

**Q77. [GATE Style]** In a hypothesis test at $\alpha = 0.05$, we fail to reject $H_0$. Which of the following is correct?
(a) $H_0$ is true  
(b) $H_0$ is false  
(c) There is insufficient evidence to reject $H_0$  
(d) $H_1$ is true

---

**Q78. [Medium]** A study has Type I error rate $\alpha = 0.01$ and Type II error rate $\beta = 0.15$. What is the power of the test? If 500 tests are performed on true null hypotheses, expected number of false positives?

---

**Q79. [GATE PYQ Style]** Roll a die 60 times. Results: {1:8, 2:12, 3:10, 4:14, 5:9, 6:7}. Test at $\alpha = 0.05$ if the die is fair. ($\chi^2_{0.05, 5} = 11.07$)

---

**Q80. [GATE Style]** A coin is tossed 200 times: 115 heads, 85 tails. Test if the coin is fair at $\alpha = 0.05$ using the chi-squared test.

---

**Q81. [GATE Style]** Contingency table:

|  | Pass | Fail | Total |
|---|---|---|---|
| Method A | 60 | 40 | 100 |
| Method B | 80 | 20 | 100 |
| Total | 140 | 60 | 200 |

Test independence at $\alpha = 0.01$. ($\chi^2_{0.01, 1} = 6.635$)

---

**Q82. [Hard]** If $X_1, X_2, \ldots, X_{10}$ are i.i.d. $N(0, 4)$, what is the distribution of $\sum_{i=1}^{10} X_i^2 / 4$? Find $P\left(\sum X_i^2 / 4 > 18.31\right)$ given $\chi^2_{0.05, 10} = 18.31$.

---

## Section J: Mixed/Comprehensive (Q83–Q85)

---

**Q83. [GATE PYQ Style - Comprehensive]** Messages arrive at a server as a Poisson process with rate 10/minute. Each message is "urgent" independently with probability 0.3.
(a) Find $P(\text{no urgent messages in 2 minutes})$.
(b) Find the expected waiting time for the first urgent message.

---

**Q84. [GATE Style - Comprehensive]** $X \sim N(5, 4)$, $Y \sim N(3, 9)$, independent.
(a) $P(X > Y)$  
(b) $E[2X - Y + 1]$  
(c) $\text{Var}(X - 2Y)$

---

**Q85. [Hard - Comprehensive]** In a quality control process, items are accepted if the sample mean of 25 items is within 2 units of the claimed mean $\mu = 50$, using $\sigma = 5$.
(a) Find the probability of accepting a lot with true mean $\mu = 50$ (not committing Type I error).
(b) Find probability of accepting if true mean is $\mu = 52$ (Type II error, $\beta$).

---

> **Total: 85 Questions** covering all modules — use `3_Solutions.md` for detailed step-by-step solutions.
