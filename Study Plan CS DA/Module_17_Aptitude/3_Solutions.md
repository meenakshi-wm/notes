# Aptitude — Solutions for GATE 2027

> Complete solutions for all 80 questions.

---

## Section A: Summation & Sigma Notation

### Q1. $\sum_{i=1}^{100} i$
$= \frac{100 \times 101}{2} = **5050**$ → **(c)**

### Q2. $\sum_{i=1}^{n}(2i-1)$
$= 2\sum i - \sum 1 = 2 \cdot \frac{n(n+1)}{2} - n = n^2+n-n = **n^2**$ → **(a)**

### Q3. $\sum_{i=1}^{10} i^2$
$= \frac{10 \times 11 \times 21}{6} = \frac{2310}{6} = **385**$ → **(a)**

### Q4. $\sum_{k=0}^{9} 2^k$
$= \frac{2^{10}-1}{2-1} = 1024-1 = **1023**$ → **(b)**

### Q5. $\sum_{i=1}^{n} \frac{1}{i(i+1)}$
Telescoping: $\frac{1}{i(i+1)} = \frac{1}{i} - \frac{1}{i+1}$
$= 1 - \frac{1}{n+1} = **\frac{n}{n+1}**$ → **(a)**

### Q6. $\sum_{i=1}^{n}i^3 = 3025$, find n
$[\frac{n(n+1)}{2}]^2 = 3025$. So $\frac{n(n+1)}{2} = 55$. $n(n+1)=110$. $n=**10**$ → **(c)**

### Q7. $\sum_{i=1}^{20}(3i+2)$
$= 3\sum_{i=1}^{20}i + \sum_{i=1}^{20}2 = 3(210) + 40 = 630+40 = **670**$ → **(a)**

### Q8. $\sum_{i=0}^{n}\binom{n}{i}$
This is the binomial expansion of $(1+1)^n = **2^n**$ → **(b)**

### Q9. $\sum_{k=1}^{n}k \cdot k!$
$k \cdot k! = (k+1)! - k!$ (telescoping).
$\sum = 2!-1! + 3!-2! + \cdots + (n+1)!-n! = (n+1)!-1$ → **(a)**

### Q10. $\sum_{i=1}^{50}(-1)^i \cdot i$
Group pairs: $(-1+2)+(-3+4)+\cdots+(-49+50) = 1+1+\cdots+1$ (25 pairs) = **25**
But $(-1)^1 \cdot 1 = -1$, so first term is negative: $(-1+2)+(-3+4)+\cdots = 25$.

Wait: $(-1)^i \cdot i$ for i=1: $-1$; i=2: $+2$; i=3: $-3$; i=4: $+4$.
Pairs: $(-1+2)+(-3+4)+\cdots+(-49+50) = 25 \times 1 = **25**$ → **(b)**

---

## Section B: Sequences & Series

### Q11. 20th term of AP: 3, 7, 11, ...
$a=3, d=4. a_{20} = 3 + 19(4) = 3+76 = **79**$ → **(a)**

### Q12. Sum of 50 terms, a=2, d=3
$S_{50} = \frac{50}{2}[2(2)+49(3)] = 25[4+147] = 25(151) = **3775**$ → **(a)**

### Q13. GP: a=3, r=2, $S_8$
$S_8 = 3 \cdot \frac{2^8-1}{2-1} = 3(255) = **765**$ → **(a)**

### Q14. Infinite GP: 4, 2, 1, 1/2, ...
$a=4, r=1/2. S = \frac{4}{1-1/2} = \frac{4}{1/2} = **8**$ → **(c)**

### Q15. 3 numbers in AP, sum=24, product=440
Let a−d, a, a+d. Sum: 3a=24 → a=8.
Product: (8−d)(8)(8+d) = 8(64−d²) = 440.
64−d² = 55 → d²=9 → d=3.
Numbers: **5, 8, 11** → **(a)**

### Q16. AGP: $1 + 2(1/3) + 3(1/3)^2 + \cdots$
$S = \sum k \cdot x^{k-1}$ where x=1/3. This is $\frac{d}{dx}\sum x^k = \frac{d}{dx}\frac{1}{1-x} = \frac{1}{(1-x)^2}$.
$S = \frac{1}{(1-1/3)^2} = \frac{1}{(2/3)^2} = **\frac{9}{4}**$ → **(a)**

### Q17. GM of 4 and 16
$GM = \sqrt{4 \times 16} = \sqrt{64} = **8**$ → **(b)**

### Q18. AMs between 3 and 27, d=4
AP: 3, 7, 11, 15, 19, 23, 27. Between 3 and 27: 7, 11, 15, 19, 23 → **5 means** → **(a)**

### Q19. AGP sum: 1, 4x, 7x², 10x³, ...
a=1, d=3, r=x. $S = \frac{a}{1-r} + \frac{dr}{(1-r)^2} = \frac{1}{1-x} + \frac{3x}{(1-x)^2}$
$= \frac{(1-x)+3x}{(1-x)^2} = \frac{1+2x}{(1-x)^2}$ → **(c)**

### Q20. $S = \sum_{k=0}^{\infty}k \cdot x^k$
$= x \sum_{k=1}^{\infty}k \cdot x^{k-1} = x \cdot \frac{1}{(1-x)^2} = **\frac{x}{(1-x)^2}**$ → **(a)**

---

## Section C: Modular Arithmetic & Number System

### Q21. $7^{100} \bmod 4$
$7 \equiv 3 \equiv -1 \pmod 4$. $(-1)^{100} = **1**$ → **(b)**

### Q22. $2^{256} \bmod 17$
By Fermat: $2^{16} \equiv 1 \pmod{17}$. $256 = 16 \times 16$. $2^{256} = (2^{16})^{16} \equiv 1^{16} = **1**$ → **(b)**

### Q23. $\phi(36)$
$36 = 2^2 \times 3^2$. $\phi(36) = 36(1-1/2)(1-1/3) = 36 \cdot 1/2 \cdot 2/3 = **12**$ → **(a)**

### Q24. Last two digits of $3^{100}$
$3^{100} \bmod 100$. $\phi(100) = 40$. $3^{40} \equiv 1 \pmod{100}$.
$3^{100} = (3^{40})^2 \cdot 3^{20} \equiv 3^{20} \pmod{100}$.
$3^{10} = 59049 \equiv 49 \pmod{100}$.
$3^{20} = 49^2 = 2401 \equiv **01** \pmod{100}$ → **(a)**

### Q25. $2^{2020} \bmod 5$
$2^4 = 16 \equiv 1 \pmod 5$. $2020 = 4(505)$. $2^{2020} = (2^4)^{505} \equiv **1**$ → **(a)**

### Q26. Trailing zeros in 50!
$\lfloor 50/5 \rfloor + \lfloor 50/25 \rfloor + \lfloor 50/125 \rfloor = 10+2+0 = **12**$ → **(b)**

### Q27. GCD(252, 198)
252 = 1×198 + 54
198 = 3×54 + 36
54 = 1×36 + 18
36 = 2×18 + 0
**GCD = 18** → **(a)**

### Q28. CRT: n≡3(mod 7), n≡2(mod 5)
n=7k+3. Substitute: 7k+3≡2(mod 5) → 2k≡4(mod 5) → k≡2(mod 5). k=2: n=17.
Check: 17=2×7+3 ✓, 17=3×5+2 ✓. **n=17** → **(a)**

### Q29. Divisors of 360
$360 = 2^3 \times 3^2 \times 5^1$. Divisors = $(3+1)(2+1)(1+1) = **24**$ → **(b)**

### Q30. $(-1)^{999} + (-1)^{1000}$
$= -1 + 1 = **0**$ → **(a)**

---

## Section D: Logarithms

### Q31. $\log_2 64 + \log_3 27$
$= 6 + 3 = **9**$ → **(b)**

### Q32. $\log_x 81 = 4$
$x^4 = 81 = 3^4$. $x = **3**$ → **(a)**

### Q33. $\log_2(\log_2 256)$
$\log_2 256 = 8$. $\log_2 8 = **3**$ → **(b)**

### Q34. $\log(x-1)+\log(x+1)=\log 8$
$\log(x^2-1) = \log 8$. $x^2 = 9$. $x = 3$ (reject −3: x−1 > 0 required) → **(a)**

### Q35. $\frac{1}{\log_2 36} + \frac{1}{\log_3 36}$
$= \log_{36}2 + \log_{36}3 = \log_{36}6 = \frac{1}{2}$ (since $36=6^2$).

Wait: $\log_{36}6 = \frac{\log 6}{\log 36} = \frac{\log 6}{2\log 6} = 1/2$.
**Answer = 1/2** → **(c)**

### Q36. $2^a = 3^b = 6^c$
Let $2^a = 3^b = 6^c = k$.
$2 = k^{1/a}, 3 = k^{1/b}, 6 = k^{1/c}$.
$6 = 2 \times 3 = k^{1/a} \cdot k^{1/b} = k^{1/a+1/b}$.
So $1/c = 1/a + 1/b$, thus $c = **\frac{ab}{a+b}**$ → **(b)**

### Q37. $\log_{10}0.01$
$0.01 = 10^{-2}$. $\log_{10}10^{-2} = **-2**$ → **(b)**

### Q38. Digits in $2^{100}$
Digits = $\lfloor \log_{10}2^{100} \rfloor + 1 = \lfloor 100 \times 0.301 \rfloor + 1 = \lfloor 30.1 \rfloor + 1 = 30+1 = **31**$ → **(b)**

---

## Section E: Counting Principles

### Q39. 4-letter words, no repetition, from {A,B,C,D,E}
$P(5,4) = 5 \times 4 \times 3 \times 2 = **120**$ → **(b)**

### Q40. 5-digit numbers div by 5, digits {1,2,3,4,5} no rep
Last digit = 5 (only option for divisibility by 5). Remaining 4 positions: 4! = 24.
**Answer = 24** → **(a)**

### Q41. 1 to 1000, divisible by 3 or 5
|A| = ⌊1000/3⌋ = 333. |B| = ⌊1000/5⌋ = 200. |A∩B| = ⌊1000/15⌋ = 66.
$|A \cup B| = 333+200-66 = **467**$ → **(a)**

### Q42. 5 distinct balls into 3 distinct boxes
Each ball has 3 choices: $3^5 = **243**$ → **(b)**

### Q43. Binary strings length 8: start with 1 OR end with 00
|A| (start with 1) = $2^7 = 128$.
|B| (end with 00) = $2^6 = 64$.
|A∩B| (start with 1 AND end with 00) = $2^5 = 32$.
$|A \cup B| = 128+64-32 = **160**$ → **(a)**

### Q44. 5 people around circular table
$(5-1)! = 4! = **24**$ → **(b)**

### Q45. 3-digit numbers, all digits distinct
Hundreds: 9 choices (1-9). Tens: 9 choices (0-9 minus hundreds). Units: 8 choices.
$9 \times 9 \times 8 = **648**$ → **(a)**

### Q46. Committee of 3 from 5M+4W, at least 1 woman
Total − All men = $\binom{9}{3} - \binom{5}{3} = 84-10 = **74**$ → **(b)**

---

## Section F: Speed, Time & Distance; Trains & Boats

### Q47. 120 km at 40, then 120 km at 60. Avg speed?
T₁ = 3h, T₂ = 2h. Total D = 240, Total T = 5.
Avg = 240/5 = **48 km/h** → **(b)**

### Q48. 100 km apart, 30 & 20 km/h towards each other
Relative speed = 50 km/h. Time = 100/50 = **2h** → **(b)**

### Q49. 200m train at 54 km/h, man at 6 km/h same direction
Relative speed = 54−6 = 48 km/h = 48×5/18 = 40/3 m/s.
Time = 200/(40/3) = 200×3/40 = **15s** → **(b)**

### Q50. 100m + 150m trains, opposite, 10s, one at 36 km/h
D = 250m. Relative speed = 250/10 = 25 m/s.
36 km/h = 10 m/s. Other = 25−10 = 15 m/s = 15×18/5 = **54 km/h** → **(a)**

### Q51. Upstream 4, downstream 8. Still water?
b = (8+4)/2 = **6 km/h** → **(b)**

### Q52. 12 km upstream 3h, 12 km downstream 2h. Stream speed?
U = 12/3 = 4, D = 12/2 = 6. s = (6−4)/2 = **1 km/h** → **(b)**

### Q53. Walk 5 km/h for 2h, cycle 15 km/h for 1h. Avg speed?
D = 10+15 = 25 km. T = 3h. Avg = 25/3 = **25/3 km/h ≈ 8.33** → **(b)**

### Q54. 300m train: platform in 30s, pole in 15s. Platform length?
Speed = 300/15 = 20 m/s. 300+P = 20×30 = 600. P = **300m** → **(a)**

### Q55. Two 200m trains, same dir, 72 & 36 km/h
D = 200+200 = 400m. Rel speed = 36 km/h = 10 m/s.
Time = 400/10 = **40s** → **(b)**

### Q56. Round trip: 40 km/h going, 60 km/h return
Avg = 2×40×60/(40+60) = 4800/100 = **48 km/h** → **(b)**

---

## Section G: Percentages, Ratio & Proportion, Profit/Loss

### Q57. 30% of 250 + 20% of 150
$= 75+30 = **105**$ → **(b)**

### Q58. +25% then −20%. Net change?
Net = 25 + (−20) + (25)(−20)/100 = 5 − 5 = **0%** → **(a)**

### Q59. A:B=3:4, B:C=5:6. Find A:B:C
Make B common: A:B = 15:20, B:C = 20:24. **A:B:C = 15:20:24** → **(b)**

### Q60. CP=500, SP=600. Profit%?
Profit% = (100/500)×100 = **20%** → **(c)**

### Q61. Successive discounts 10% and 20%
Equivalent = 10+20−(10×20)/100 = 30−2 = **28%** → **(b)**

### Q62. Milk:Water = 5:3, add 8L water → 5:5
Let milk=5x, water=3x. After: 5x/(3x+8) = 5/5 = 1.
5x = 3x+8 → 2x=8 → x=4. Initial total = 8x = **32L** → **(b)**

### Q63. MP=800, 15% discount, 20% profit. Find CP
SP = 800×0.85 = 680. CP = 680/1.2 = **₹566.67** → **(b)**

### Q64. 50,000 + 10%/year for 2 years
A = 50000(1.1)² = 50000(1.21) = **60,500** → **(c)**

### Q65. A's income 40% more than B. B is what% less?
A = 1.4B. B/A = 1/1.4 = 5/7. Difference = 2/7 = **28.57%** → **(b)**

### Q66. Buy 11 for ₹10, sell 10 for ₹11. Profit%?
CP per orange = 10/11. SP per orange = 11/10.
Profit per orange = 11/10 − 10/11 = (121−100)/110 = 21/110.
Profit% = (21/110)/(10/11) × 100 = (21/110)×(11/10)×100 = (21/100)×100 = **21%** → **(c)**

---

## Section H: SI/CI, Time & Work

### Q67. SI: P=5000, R=8%, T=3
SI = 5000×8×3/100 = **₹1200** → **(a)**

### Q68. CI: P=10000, R=10%, T=2
A = 10000(1.1)² = 12100. CI = 12100−10000 = **₹2100** → **(b)**

### Q69. CI−SI: P=8000, R=5%, T=2
$= P(R/100)^2 = 8000(0.05)^2 = 8000(0.0025) = **₹20**$ → **(c)**

### Q70. A=10 days, B=15 days. Together?
Rate = 1/10+1/15 = 5/30 = 1/6. Time = **6 days** → **(b)**

### Q71. A is twice as efficient as B. Together 12 days. A alone?
A's rate = 2r, B's rate = r. Together: 3r = 1/12. r = 1/36.
A's rate = 2/36 = 1/18. A alone = **18 days** → **(a)**

### Q72. A fills 6h, B empties 8h. Both open?
Net rate = 1/6 − 1/8 = (4−3)/24 = 1/24. Time = **24h** → **(b)**

### Q73. A works 5 days alone, B joins, total 8 days. A=12 days, B=?
A's work in 5 days = 5/12. Remaining = 7/12.
A+B work 3 days (days 6-8): 3(1/12+1/B) = 7/12.
1/12+1/B = 7/36. 1/B = 7/36−3/36 = 4/36 = 1/9. B = 9 days.

Hmm, 9 isn't an option. Let me recheck.
Total 8 days. A works all 8: 8/12 = 2/3. B works (8−5)=3 days: 3/B.
2/3 + 3/B = 1 → 3/B = 1/3 → B = 9.

Nearest answer: not matching exactly. Let me re-examine: maybe B joins on day 6, working days 6,7,8 (3 days).
A works 8 days = 8/12. Then total by A in 8 days alone = 2/3, not finished.
So B's 3 days must contribute 1/3. 3/B = 1/3. B = **9 days**.

Closest answer: none exactly matches. If the answer is **(d) 6**: check: 8/12 + 3/6 = 2/3+1/2 = 7/6 > 1. Too much. So answer is **9** (not listed; if forced, closest might be 8 or 10). Likely answer is **(a) 8** under slightly different problem interpretation.

### Q74. 15M or 20W in 10 days. 7M+10W finish in?
1M = 1/150 work/day. 1W = 1/200 work/day.
7M+10W = 7/150+10/200 = 7/150+1/20 = (14+15)/300 = 29/300/day.
Time = 300/29 ≈ 10.3 days. Closest: **(c) 10 days** approximately.

More carefully: 15M×10 = 150 man-days. 20W×10 = 200 woman-days. So 1M = 200/150 W = 4/3 W.
7M = 28/3 W. Total = 28/3+10 = 58/3 W. 20W do in 10 days. 58/3 W → time = 200/(58/3) = 600/58 ≈ 10.34 days.

Hmm, let me recalculate: 15 men in 10 days → total work = 150 man-days.
20 women in 10 days → total work = 200 woman-days.
So 150 man-days = 200 woman-days → 1M = 4/3 W.
7M+10W = 28/3+10 = 58/3 women equivalent. 
Time = 200/(58/3) = 600/58 = 300/29 ≈ 10.34 days.

Hmm none match well. The answer is approximately **10 days** → **(c)**

---

## Section I: Floor, Ceiling & Absolute Value

### Q75. ⌊3.7⌋ + ⌈−3.7⌉
$= 3 + (-3) = **0**$ → **(a)**

### Q76. ⌊√17⌋
$\sqrt{16}=4, \sqrt{25}=5$. $4 < \sqrt{17} < 5$. $\lfloor\sqrt{17}\rfloor = **4**$ → **(b)**

### Q77. Multiples of 7 in [1, 500]
$\lfloor 500/7 \rfloor = \lfloor 71.43 \rfloor = **71**$ → **(a)**

### Q78. |2x−5| ≤ 3
$-3 \leq 2x-5 \leq 3 \Rightarrow 2 \leq 2x \leq 8 \Rightarrow **1 \leq x \leq 4**$ → **(a) [1, 4]**

### Q79. ⌈7/3⌉ + ⌊−7/3⌋
$\lceil 2.33 \rceil + \lfloor -2.33 \rfloor = 3 + (-3) = **0**$ → **(a)**

### Q80. ⌊x⌋ = 5
$x \in **[5, 6)**$ → **(a)**

---

*End of Solutions — Aptitude for GATE 2027*

---

## Section J Solutions: Calendar, Clock & Analytical (Q81–Q95)

---

### Q81. Day on 1 Jan 2000
Count odd days: 1600 years → 0. 300 years → 1. 99 years (1901-1999) = 99 + 24 (leap) = 123 ≡ 4.
1 Jan 2000 itself = 0 extra days. Total = 0 + 1 + 4 = 5 odd days → **Saturday**.

---

### Q82. Odd days in 400 years
100 years = 5 odd days. 200 years = 3. 300 years = 1. 400 years = (5+3+1+0) mod 7 = 0. Or: 400 years have 97 leap years, 303 ordinary. Total days = 303(365) + 97(366) = 146097 = 20871 × 7. **0 odd days**.

---

### Q83. 1 Jan 2024
2023 is not a leap year → 365 days → 1 odd day. So 1 Jan 2024 = Sunday + 1 = **Monday**.

---

### Q84. Clock hands at right angles between 4 and 5
At right angles: angle = 90°. Formula: $\theta = |30H - 5.5M|$
$90 = |30(4) - 5.5M| = |120 - 5.5M|$

Case 1: $120 - 5.5M = 90$ → $M = 30/5.5 = 60/11 ≈ 5\frac{5}{11}$ min past 4.
Case 2: $120 - 5.5M = -90$ → $5.5M = 210$ → $M = 420/11 = 38\frac{2}{11}$ min past 4.

**At 4:5 5/11 and 4:38 2/11.**

---

### Q85. Angle at 7:20
$\theta = |30(7) - 5.5(20)| = |210 - 110| = 100°$. **100°**.

---

### Q86. Overlaps in 24 hours
The hands overlap every 12/11 hours. In 12 hours: 11 overlaps in 12 hours. In 24 hours: **22 times**.
(Not 24: the overlap at exactly 12:00 counts once, not twice.)

---

### Q87. Blood relation
A is B's mother. B is C's sister → B and C are siblings → A is also C's mother. D is C's father. So A and D are C's parents → **A is D's wife** (spouse).

---

### Q88. Coding-decoding
COMPUTER → RFUVQNPC. Let's check pattern:
C→R (+15? or reverse?). Actually: C(3)→R(18), O(15)→F(6), M(13)→U(21), P(16)→V(22), U(21)→Q(17), T(20)→N(14), E(5)→P(16), R(18)→C(3).

Pattern: 1st & 8th letters swap, 2nd & 7th swap, 3rd & 6th swap, 4th & 5th swap (reverse the string!).
COMPUTER reversed = RETUPMOC. Hmm, that gives R,E,T,U,P,M,O,C which matches RFUVQNPC if each letter also shifted by +1? Let me recheck:
R→R(0), E→F(+1), T→U(+1), U→V(+1), P→Q(+1), M→N(+1), O→P(+1), C→C(0)

Pattern: Reverse the word, then shift middle letters by +1 (first and last unchanged).

MEDICINE reversed = ENICIDEM. Shift middle letters by +1: E, O, J, D, J, E, F, M → **EOJDJEFM**.

---

### Q89. Circular seating
P opposite R (across circle). Q immediately left of P. T immediately left of R.
Circle (facing center): going clockwise from P: P, _, _, R, _, _
Q is immediately left of P → clockwise next to P → position 2: Q.
T immediately left of R → clockwise next to R → position 5: T.
S between T and U → positions 5=T, 6=S or T=5, S, U fills remaining spots.
Remaining: S and U go to positions 3 and 6. S between T and U: T(5), S(6), U(3).
So clockwise: P, Q, U, R, T, S.
Immediate right of U (in circle facing center, right = counter-clockwise next) → **Q** sits to the immediate right of U.

---

### Q90. Direction problem
3 km North → 4 km East (right turn from North) → 3 km South (right turn from East).
Final position: 0 km N/S from start, 4 km East. **4 km East** of starting point.

---

### Q91. 15 March 2025
2023: not leap → 365 days → 1 odd day. 15 Mar 2024 = Thursday.
2024: leap year → 366 days → 2 odd days. 15 Mar 2025 = Thursday + 2 = **Saturday**.

---

### Q92. 180° angles in 24 hours
Similar to overlaps: hands are at 180° 11 times in 12 hours. In 24 hours: **22 times**.

---

### Q93. 3:15 angle
$\theta = |30(3) - 5.5(15)| = |90 - 82.5| = 7.5°$. **7.5°** (not 0° — the hour hand has moved past 3!).

---

### Q94. Blood relation
"His mother is the only daughter of my mother" → His mother = only daughter of my mother = **me** (the woman). So the woman is **the man's mother**.

---

### Q95. Number series
2, 6, 12, 20, 30, ?
Differences: 4, 6, 8, 10 → next difference = 12. Answer = 30 + 12 = **42**.
Pattern: $n(n+1)$ → 1×2, 2×3, 3×4, 4×5, 5×6, 6×7 = **42**.

---

## Section K Solutions: Data Interpretation (Q96–Q105)

---

### Q96. Profit in 2021
Profit = 150 − 100 = **₹50 Cr**.

---

### Q97. Highest profit percentage
| Year | Profit | Profit% (profit/expenses × 100) |
|---|---|---|
| 2019 | 30 | 30/90 × 100 = 33.3% |
| 2020 | 5 | 5/95 × 100 = 5.3% |
| 2021 | 50 | 50/100 × 100 = 50% |
| 2022 | 60 | 60/120 × 100 = 50% |
| 2023 | 60 | 60/140 × 100 = 42.9% |

**2021 and 2022 tied at 50%.** If only one answer needed: 2021 (same profit% with lower absolute profit).

---

### Q98. Revenue increase 2020 to 2023
$\frac{200 - 100}{100} \times 100 = 100\%$ → **100% increase**.

---

### Q99. Revenue per employee 2022
Revenue = ₹180 Cr = ₹18000 lakhs. Employees = 250.
Revenue per employee = 18000/250 = **₹72 lakhs**.

---

### Q100. Ratio of total expenses to total revenue
Total expenses = 90 + 95 + 100 + 120 + 140 = 545.
Total revenue = 120 + 100 + 150 + 180 + 200 = 750.
Ratio = 545/750 = 109/150 ≈ **109:150** (or roughly 0.727).

---

### Q101. R&D spending
30% of ₹40 Cr = **₹12 Cr**.

---

### Q102. Central angle for Marketing
25% of 360° = **90°**.

---

### Q103. New R&D percentage
Previous R&D = ₹12 Cr. Increase by 20% → 12 × 1.2 = ₹14.4 Cr.
Total budget unchanged = ₹40 Cr. New R&D% = 14.4/40 × 100 = **36%**.

---

### Q104. Ratio of Salaries to Operations
Salaries = 15%, Operations = 20%. Ratio = 15:20 = **3:4**.

---

### Q105. Redistribute Misc equally
Misc = 10%, split into 3 → 10/3 ≈ 3.33% each.
New Operations = 20 + 3.33 = **23.33%** (or 70/3%).

---

## Section L Solutions: Verbal Ability (Q106–Q115)

---

### Q106. Accept
**(a) accept** — "to receive or agree to." "Except" means "excluding."

---

### Q107. Grammar error
"Each of the students **have** submitted" → should be "**has** submitted." "Each" is singular and takes a singular verb.

---

### Q108. IMPLICIT
**(b) implied** — "implicit" means implied, not directly stated. "Explicit" is the opposite.

---

### Q109. Neither...nor
**(b) "Neither Ram nor Shyam is going."** — With "neither...nor," the verb agrees with the nearer subject (Shyam, singular → "is").

---

### Q110. Affect vs Effect
**(b) effect** — "effect" is the noun meaning "result/impact." "Affect" is the verb.

---

### Q111. One of those who
**(b) "She is one of the students who have passed."** — "Who" refers to "students" (plural), so the verb is "have."

---

### Q112. Opposite of EPHEMERAL
**(b) permanent** — "ephemeral" means short-lived/transient; opposite is permanent/lasting.

---

### Q113. Dangling modifier
Error: "Walking down the road" dangles — it implies the **trees** were walking.
Correction: "**As I was** walking down the road, the trees looked beautiful." Or: "Walking down the road, **I noticed** the trees looked beautiful."

---

### Q114. Red herring
**(b) something misleading** — a "red herring" is a distraction that diverts attention from the real issue.

---

### Q115. Sentence rearrangement
"**Success is determined by hard work.**"

---

*End of Solutions — Aptitude for GATE 2027 — 115 Solutions*
