# C Programming — Solutions for GATE 2027

---

## Section A: Operators, Expressions & Type Conversion

---

### S1. Arithmetic Operators

**Output:**
```
3 1 -2
```

**Explanation:**
- `10/3 = 3` (integer division truncates toward zero)
- `10%3 = 1` (remainder)
- `-10%3 = -2` (in C99+, sign of result = sign of dividend)

---

### S2. Integer Promotion

**Output:**
```
-121
```

**Trace:**
- `c = 125`. Type: `signed char` (range: $-128$ to $127$).
- `c + 10`: `char` is promoted to `int` → $125 + 10 = 135$.
- Stored back into `char`: $135$ doesn't fit in signed char.
- $135 - 256 = -121$ (wraps around in 2's complement).
- `%d` prints the `int`-promoted value: $-121$.

---

### S3. Signed vs Unsigned Comparison

**Output:**
```
x <= y
```

**Trace:**
- Comparing `unsigned int x = 1` with `int y = -1`.
- Mixed comparison: `y` is converted to `unsigned int`.
- $-1$ as `unsigned` = $2^{32} - 1 = 4294967295$.
- So comparison becomes: `1 > 4294967295` → **false**.
- Output: `x <= y`.

---

### S4. Bitwise Operators

**Output:**
```
8 14 6 -13
```

**Trace (in binary, lower 4 bits shown):**
- $a = 12 = 1100_2$, $b = 10 = 1010_2$
- `a & b = 1000` = $8$
- `a | b = 1110` = $14$
- `a ^ b = 0110` = $6$
- `~a = ~(00...01100)` → `11...10011`. In 2's complement: $-(12+1) = -13$.

---

### S5. Left and Right Shift

**Output:**
```
16 1024
25 3
```

**Trace:**
- `1 << 4` = $1 \times 2^4 = 16$
- `1 << 10` = $1 \times 2^{10} = 1024$
- `100 >> 2` = $\lfloor 100 / 4 \rfloor = 25$
- `100 >> 5` = $\lfloor 100 / 32 \rfloor = 3$

---

### S6. Comma Operator

**Output:**
```
3 1
```

**Trace:**
- `a = (1, 2, 3)`: Parentheses group the comma expression. Comma evaluates left-to-right, returns last value. `a = 3`.
- `b = 1, 2, 3`: Assignment (`=`) has higher precedence than comma. So `b = 1` is evaluated first, then `2` and `3` are discarded. `b = 1`.

---

### S7. Short-Circuit Evaluation

**Output:**
```
a=1 b=0 c=1 x=1
```

**Trace (step by step):**
1. `a++`: post-increment. Value used in expression = $0$ (original), then `a` becomes $1$.
2. `0 && b++`: Left side is $0$ → short-circuit. `b++` is **NOT evaluated**. Result = $0$.
3. `0 || c++`: Left side is $0$ → right side IS evaluated. `c++` uses value $1$, then `c` becomes $1$ (was already 1? No: original `c=0`, post-increment: value $0$ is used... wait).

Let me retrace:
- Initial: `a=0, b=0, c=0`
- `a++`: value = 0, then a = 1
- `0 && b++`: since 0 is false, `b++` NOT evaluated. This sub-expression = 0.
- `0 || c++`: since 0 is false, `c++` IS evaluated. value = 0, then c = 1. Sub-expression = 0.
- Wait: `c++` returns 0 (original value), so `0 || 0 = 0`? 

Retrace more carefully:
- `x = a++ && b++ || c++`
- Precedence: `&&` higher than `||`, so: `x = (a++ && b++) || c++`
- Step 1: `a++` = 0 (post-inc, a becomes 1). Since 0 is false, `b++` not evaluated (short-circuit). `(a++ && b++)` = 0.
- Step 2: `0 || c++`. `c++` = 0 (post-inc, c becomes 1). `0 || 0` = 0.
- So: `x = 0`.
- Final: `a=1, b=0, c=1, x=0`

**Corrected Output:**
```
a=1 b=0 c=1 x=0
```

---

### S8. Pre/Post Increment — Undefined Behavior

**Answer:** This is **Undefined Behavior**.

The variable `x` is modified more than once between sequence points (`x++` and `++x` in the same expression). The C standard does not define the result. Different compilers may produce different outputs.

> **GATE Answer:** If this appears in GATE, the answer is "Undefined Behavior" or "Implementation Dependent."

---

### S9. Conditional (Ternary) Operator

**Output:**
```
5
5 11 10
```

**Trace:**
- `min = (5 < 10) ? 5 : 10 = 5`.
- `c = (5 > 10) ? a++ : b++`: condition is false, so `b++` is evaluated.
  - `c = b` (before increment) = $10$. Then `b` becomes $11$.
  - `a` is unchanged = $5$.
- Print: `5 11 10`.

---

### S10. Undefined Behavior [GATE 2015]

**Answer:** **Undefined Behavior.**

`i++ + i++ + i++` modifies `i` three times without an intervening sequence point. The C standard makes this undefined. Any answer the compiler gives is technically valid.

---

### S11. Bitwise NOT and Unsigned

**Output:**
```
4294967295
-1
```

**Trace:**
- `~0u`: flip all bits of `unsigned 0` → all 1s = $2^{32} - 1 = 4294967295$.
- `%u` prints it as unsigned: $4294967295$.
- `%d` interprets the same bit pattern as signed: $-1$ (2's complement of all 1s).

---

### S12. Type Conversion

**Output:**
```
0.000000 (or undefined behavior due to format mismatch)
3.500000
3.500000
```

**Trace:**
- `a/2` = `7/2` = `3` (integer division). But `%f` expects a `double` — passing an `int` with `%f` is **undefined behavior**. On many systems, it prints `0.000000` or garbage.
- `b/2` = `7.0/2` = `3.5`. Printed as `3.500000`.
- `(float)a/2` = `7.0f/2` = `3.5`. Printed as `3.500000`.

---

### S13. Sizeof Doesn't Evaluate

**Output:**
```
4
5
```

**Trace:**
- `sizeof(x++)`: returns `sizeof(int)` = $4$. The expression `x++` is **NOT evaluated** — `sizeof` is a compile-time operator.
- `x` remains $5$.

---

### S14. Logical Operators

**Output:**
```
0
1
0
1
```

**Trace:**
- `5 && 0` = 0 (one side is 0)
- `5 || 0` = 1 (one side is non-zero)
- `!(-3)` = `!non-zero` = 0
- `!!5` = `!0` = 1 (double negation normalizes to 1)

---

### S15. Short-Circuit with Precedence

**Output:**
```
a=2 b=1 c=1 d=1 result=1
```

**Trace:**
- Expression: `++a || ++b && ++c`
- Precedence: `&&` binds tighter, so: `++a || (++b && ++c)`
- `++a` = $2$ (non-zero = true).
- `||` short-circuits: right side `(++b && ++c)` is **NOT evaluated**.
- `b`, `c`, `d` remain unchanged at $1$.
- `result = 1` (true).

---

## Section B: Control Flow

---

### S16. Switch Fall-Through

**Output:** `BCD`

**Trace:** `x = 2` matches `case 2`. No `break`, so execution falls through: `case 3` and `default` also execute.

---

### S17. Loop with Break and Continue

**Output:**
```
0 1 2 4 5 6 
i = 7
```

**Trace:**
- `i=0,1,2`: printed. `i=3`: `continue` (skip print). `i=4,5,6`: printed. `i=7`: `break` (exit loop).
- After `break`, `i = 7` (not incremented further).

---

### S18. While Loop Tricky

**Output:**
```
1 2 3 4 5 
i = 6
```

**Trace:**
- `i=0`: condition `0 < 5` true, then `i++` makes `i=1`. Print $1$.
- `i=1`: condition `1 < 5` true, then `i=2`. Print $2$.
- ... continues until `i=4`: condition `4 < 5` true, `i=5`. Print $5$.
- `i=5`: condition `5 < 5` false, but post-increment makes `i=6`. Loop exits.

---

### S19. Do-While vs While

**Output:** `D10 `

**Trace:**
- `while (10 < 5)` is false → body never executes. Nothing printed.
- `do { } while (10 < 5)` → body executes **once** (prints `D10`), then condition is false → loop exits.

---

### S20. Nested Loop Count

**Output:** `10`

**Trace:**
| i | j values | iterations |
|---|---|---|
| 0 | 0,1,2,3 | 4 |
| 1 | 1,2,3 | 3 |
| 2 | 2,3 | 2 |
| 3 | 3 | 1 |
| **Total** | | **10** |

Formula: $\sum_{i=0}^{3}(4-i) = 4+3+2+1 = 10$

---

## Section C: Functions & Storage Classes

---

### S21. Static Variable

**Output:** `1 2 3 `

**Trace:** `static int x = 0` is initialized **once**. Each call increments and returns: 1, 2, 3.

---

### S22. Call by Value

**Output:** `5`

**Trace:** `foo(a)` passes a copy of `a`. Modifying `x` inside `foo` doesn't affect `a` in `main`.

---

### S23. Extern Variable

**Output:** `6 10 7 `

**Trace:**
- `x = 5` (global).
- `f2()` is called:
  - `f2` declares local `int x = 10`.
  - `f1()` is called: uses **global** `x = 5`, increments to $6$. Prints `6 `.
  - Back in `f2`: prints **local** `x = 10`. Prints `10 `.
- Back in `main`: `f1()` called directly: global `x` is now $6$, increments to $7$. Prints `7 `.

---

### S24. Default Values

**Output:** `0 0`

**Trace:**
- `g` (global) → default $0$.
- `s` (static local) → default $0$.
- `a` (auto/local) → **uninitialized (garbage)**. Not printed, so no issue.

---

### S25. Static in Recursion

**Output:** `5`

**Trace:**
- `static int count = 0` — initialized once, shared across all calls.
- Call chain: `fun(4)` → `fun(3)` → `fun(2)` → `fun(1)` → `fun(0)`.
- Each call increments `count`: after 5 calls, `count = 5`.
- `fun(0)` returns $5$. Each returning call also returns $5$ (same static variable).

---

### S26. Return Value of printf

**Output:** `Hello 5`

**Trace:** `printf` returns the number of characters printed. `"Hello"` has 5 characters, so `x = 5`.

---

### S27. Multiple Calls with Static

**Output:** `1 2 3 4 5 `

**Trace:** `static int i = 0` initialized once. `++i` increments each call: 1, 2, 3, 4, 5.

---

### S28. Register Keyword

**Answer:** **Compilation error.**

You cannot take the address of a `register` variable. `&x` is illegal when `x` is declared `register`.

> Note: Some compilers (like GCC with default settings) may allow it as an extension with a warning, but per the C standard it's a constraint violation.

---

### S29. Pointer to Static Variable

**Output:** `20`

**Trace:**
- `fun()` returns pointer to `static int x = 10`. Static variables persist, so this is safe (not dangling).
- `*p = 20` modifies the static `x` to $20$.
- `q = fun()` returns the same address (same static `x`).
- `*q = 20`.

---

### S30. Static vs Non-Static Counter

**Output:**
```
3 2 1
1 1 1
```

**Trace:**
- `countA()` uses static counter — increments across calls: 1, 2, 3.
- `countB()` uses local counter — resets each call, always returns 1.
- **Note:** The order of evaluation of `printf` arguments is **unspecified** (not undefined). Most compilers evaluate right-to-left, so `countA()` prints `3 2 1`. But technically, any order of `1,2,3` is valid.

---

## Section D: Recursion

---

### S31. Simple Recursion

**Output:** `5 3 1 1 3 5 `

**Call trace:**
```
fun(5): print 5 → fun(3) → print 5
  fun(3): print 3 → fun(1) → print 3
    fun(1): print 1 → fun(-1) [returns] → print 1
```

Pre-recursion prints: 5, 3, 1  
Post-recursion prints (unwinding): 1, 3, 5

---

### S32. Fibonacci

**Output:** `8`

**Trace:**
- $fib(6) = fib(5) + fib(4) = 5 + 3 = 8$
- Fibonacci sequence: 0, 1, 1, 2, 3, 5, **8**

**Total calls for fib(6):** 25 calls.

Call tree:
```
fib(6)
├── fib(5)
│   ├── fib(4)
│   │   ├── fib(3) ... (4 calls)
│   │   └── fib(2) ... (2 calls)
│   └── fib(3) ... (4 calls)
└── fib(4)
    ├── fib(3) ... (4 calls)
    └── fib(2) ... (2 calls)
```

Number of calls = $2 \times fib(n+1) - 1 = 2 \times 13 - 1 = 25$.

---

### S33. Binary Representation via Recursion

**Output:** `1 1 0 1 `

**Trace:** The function prints the binary representation of 13.
```
fun(13) → fun(6) → fun(3) → fun(1) → fun(0) [returns]
Printing on unwind: 1%2=1, 3%2=1, 6%2=0, 13%2=1 → "1 1 0 1"
```
$13_{10} = 1101_2$ ✓

---

### S34. Sum via Recursion

**Output:** `55`

**General formula:** $fun(n) = \sum_{i=1}^{n} i = \frac{n(n+1)}{2}$

$fun(10) = \frac{10 \times 11}{2} = 55$

---

### S35. Mutual Recursion

**Output:** `1 0 0 1`

**Trace:**
- `isEven(4)` → `isOdd(3)` → `isEven(2)` → `isOdd(1)` → `isEven(0)` → returns $1$. So `isEven(4) = 1`.
- `isOdd(4)` → `isEven(3)` → `isOdd(2)` → `isEven(1)` → `isOdd(0)` → returns $0$. So `isOdd(4) = 0`.
- `isEven(7)`: chain of 7 calls → `isOdd(0)` → returns $0$. So `isEven(7) = 0`.
- `isOdd(7)`: chain of 7 calls → `isEven(0)` → returns $1$. So `isOdd(7) = 1`.

---

### S36. Recursion Count

**Output:** `15`

**Trace (tree):**
```
fun(8) [count=1]
├── fun(4) [count=2]
│   ├── fun(2) [count=3]
│   │   ├── fun(1) [count=4] ← base case
│   │   └── fun(1) [count=5] ← base case
│   └── fun(2) [count=6]
│       ├── fun(1) [count=7]
│       └── fun(1) [count=8]
└── fun(4) [count=9]
    ├── fun(2) [count=10]
    │   ├── fun(1) [count=11]
    │   └── fun(1) [count=12]
    └── fun(2) [count=13]
        ├── fun(1) [count=14]
        └── fun(1) [count=15]
```

Total: **15** calls. Pattern: $2^{n} - 1$ for $n = \log_2(8) + 1 = 4$ levels → $2^4 - 1 = 15$.

---

### S37. Tricky Recursion

**Output:** `3`

**Trace:**
```
f(11) → odd: f(5) + f(6)
  f(5) → odd: f(2) + f(3)
    f(2) → even: f(1) → 1
    f(3) → odd: f(1) + f(2) → 1 + f(1) → 1 + 1 = 2
  f(5) = 1 + 2 = 3
  f(6) → even: f(3)
    f(3) = 2 (computed above, but recalculated: f(1) + f(2) = 1 + 1 = 2)
  f(6) = 2
  But wait — let me redo. We have f(11) = f(5) + f(6).
```

Recompute:
- $f(1) = 1$
- $f(2) = f(1) = 1$
- $f(3) = f(1) + f(2) = 1 + 1 = 2$
- $f(4) = f(2) = 1$
- $f(5) = f(2) + f(3) = 1 + 2 = 3$
- $f(6) = f(3) = 2$
- $f(11) = f(5) + f(6) = 3 + 2 = 5$

**Corrected Output:** `5`

---

### S38. Recursion with Static

**Output:** `4 4 4 4 `

**Trace:**
- Static `x` is shared. Each call increments `x`.
- Call chain: `fun(3)` → `fun(2)` → `fun(1)` → `fun(0)`.
- At `fun(0)`: `x = 4` (incremented 4 times). `n > 0` is false, so no recursive call.
- `fun(0)` prints $4$.
- Unwinding: `fun(1)` prints $4$, `fun(2)` prints $4$, `fun(3)` prints $4$.
- All print the same value because `x` is static and equals $4$ by the time printing happens.

---

## Section E: Pointers

---

### S39. Basic Pointer

**Output:**
```
20 20
```

**Trace:** `*p = 20` modifies `x` through the pointer. Both `x` and `*p` now equal $20$.

---

### S40. Pointer Arithmetic

**Output:**
```
40
50
13
```

**Trace:**
- `*(p+3)`: pointer moved 3 elements → `arr[3] = 40`.
- `*(arr+4)`: moved 4 elements → `arr[4] = 50`.
- `*p + 3`: dereference first (`*p = arr[0] = 10`), then add 3 → $13$.

---

### S41. Pointer and Array Size

**Output:**
```
20 4
```

**Trace (32-bit system):**
- `sizeof(arr)` = $5 \times 4 = 20$ bytes (total array size).
- `sizeof(p)` = $4$ bytes (pointer size on 32-bit).

---

### S42. Pointer Increment

**Output:**
```
20
100 30
```

**Trace:**
1. `p = arr` → points to `arr[0]`.
2. `p++` → `p` now points to `arr[1]`. `*p = 20`.
3. `*p++ = 100`:
   - `*p` = `arr[1]`, assigned $100$. Then `p++` moves `p` to `arr[2]`.
   - Post-increment: use `p`, then increment.
   - `arr[1]` is now $100$. `p` points to `arr[2]`.
4. Print: `arr[1] = 100`, `*p = arr[2] = 30`.

---

### S43. Double Pointer

**Output:**
```
10 10 10
20
```

**Trace:**
```
pp ──→ p ──→ x
```
- `x = 10`, `*p = 10`, `**pp = 10`.
- `**pp = 20`: modifies `x` through two levels of indirection. `x` becomes $20$.

---

### S44. Pointer to Array

**Output:**
```
3
4
```

**Trace:**
- `p` is a pointer to the entire array of 5 ints. `*p` gives the array itself (decays to `int *`).
- `*(*p + 2)` = `*(arr + 2)` = `arr[2] = 3`.
- `(*p)[3]` = `arr[3] = 4`.

---

### S45. Array of Pointers

**Output:**
```
1 3
2
```

**Trace:**
- `arr[0] = &a`, `arr[2] = &c`. So `*arr[0] = 1`, `*arr[2] = 3`.
- `**(arr+1)` = `*(arr[1])` = `*(&b)` = $2$.

---

### S46. Pointer and String

**Output:**
```
H
e
llo
```

**Trace:**
- `*s = 'H'` (first character).
- `*(s+1) = 'e'` (second character).
- `s+2` points to `"llo"`. `%s` prints from that point until `\0`.

---

### S47. 2D Array and Pointers

**Output:**
```
5
5
6
```

**Trace:**
- `a[0]` is `{1,2,3}` stored contiguously, followed by `a[1]` = `{4,5,6}`.
- `*(a[0] + 4)`: `a[0]` points to address of `1`. Moving 4 elements: `1,2,3,4,**5**` → $5$.
- `*(*(a+1) + 1)`: `*(a+1)` = `a[1]` (pointer to `{4,5,6}`). `+1` → element at index 1 → $5$.
- `a[1][2] = 6`.

---

### S48. Pointer Subtraction

**Output:**
```
3
12
```

**Trace:**
- `q - p` = `(arr+4) - (arr+1)` = $3$ (elements between).
- `(char*)q - (char*)p` = $3 \times 4 = 12$ bytes (pointer arithmetic in char units).

---

### S49. Swap Using Pointers

**Output:** `20 10`

**Trace:** Pointers allow modifying the original variables. Standard swap via dereferencing.

---

### S50. Pointer Reassignment Inside Function

**Output:** `10`

**Trace:**
- `fun(p)`: pointer `p` is passed **by value**. Inside `fun`, the local copy `p` is reassigned to `&q`.
- The `p` in `main` is unchanged — still points to `x`.
- `*p = 10`.

> **Key Insight:** To modify a pointer itself inside a function, you need a **double pointer** (`int **pp`).

---

### S51. Dangling Pointer

**Answer:** The code exhibits **undefined behavior**.

- `x` is a local variable in `fun()`. When `fun()` returns, `x`'s stack frame is deallocated.
- `p` in `main` now points to invalid memory (dangling pointer).
- Dereferencing `*p` is undefined — may print 10, garbage, or crash.

---

### S52. Void Pointer

**Output:** `10`

**If Line A is uncommented:** Compilation error. You cannot dereference a `void *` directly — the compiler doesn't know the type/size. Must cast first: `*(int *)vp`.

---

## Section F: Dynamic Memory & Strings

---

### S53. malloc and free

**Output:**
```
0 20 40
```

**Trace:** `p[i] = i * 10` → `{0, 10, 20, 30, 40}`. Print indices 0, 2, 4.

---

### S54. calloc vs malloc

**Output:**
```
0 0 0
```

**Trace:** `calloc` zero-initializes all allocated memory. `malloc` would contain garbage. The `calloc` array `b` has all zeros.

---

### S55. Memory Leak

**Output:**
```
42
42
42
```

**Problem:** Each call to `fun()` allocates memory with `malloc` but never calls `free`. After 3 calls, 30 integers worth of memory is leaked. The pointer `p` is lost when `fun()` returns, making the memory unreachable.

---

### S56. String Operations

**Output:**
```
5 6
(negative number)
```

**Trace:**
- `strlen("Hello")` = $5$ (doesn't count `\0`).
- `sizeof("Hello")` = $6$ (includes `\0`).
- `strcmp("Hello", "World")`: 'H' < 'W' in ASCII, so returns a **negative** value.

---

### S57. String and Pointer

**Output:**
```
G 2
TE2027
```

**Trace:**
- `s = "GATE2027"`, `p = s`.
- `*p = 'G'` (first char), `*(p+4) = '2'` (fifth char).
- `p += 2` → `p` now points to `s[2]` = `'T'`.
- `%s` prints from `p`: `"TE2027"`.

---

### S58. Sizeof with Strings

**Output (32-bit system):**
```
4 6 20
5 5 5
```

**Trace:**
- `sizeof(s1)` = $4$ (pointer size on 32-bit).
- `sizeof(s2)` = $6$ (char array `"Hello"` = 5 + 1 for `\0`).
- `sizeof(s3)` = $20$ (declared size of array).
- `strlen` counts characters before `\0`: all three return $5$.

---

## Section G: Structures

---

### S59. Structure Basics

**Output:**
```
10 20
30
```

**Trace:**
- `p.x = 10`, `ptr->y = 20`.
- `ptr->x = 30` modifies `p.x` through the pointer.

---

### S60. Structure Padding

**Output:**
```
12 8
```

**Trace for struct A:**
```
Offset 0: char c  (1 byte) + 3 bytes padding
Offset 4: int i   (4 bytes)
Offset 8: char d  (1 byte) + 3 bytes padding
Total: 12 bytes
```

**Trace for struct B:**
```
Offset 0: char c  (1 byte)
Offset 1: char d  (1 byte) + 2 bytes padding
Offset 4: int i   (4 bytes)
Total: 8 bytes
```

Reordering members can save space!

---

### S61. Array of Structures

**Output:**
```
90.0
3
```

**Trace:**
- `(p+1)->marks`: pointer to `s[1]`, access `marks` = $90.0$.
- `(*(p+2)).id`: dereference pointer to `s[2]`, access `id` = $3$.

---

### S62. Structure and Function

**Output:**
```
a=(1,2) b=(4,6)
```

**Trace:** Structures are passed **by value** in C. `move()` modifies a copy of `a`. The original `a` is unchanged. `b` gets the modified copy.

---

### S63. Self-Referential Structure (Linked List)

**Output:** `10 20 30 `

**Trace:**
```
n1(10) → n2(20) → n3(30) → NULL
```
Traversal prints each node's data.

---

### S64. Structure with Pointer Member

**Output:**
```
100 200
300
```

**Trace:**
- `d.ptr = &x`, so `*(d.ptr) = x = 100`. `d.val = 200`.
- `*(d.ptr) = 300` modifies `x` through the pointer. `x` becomes $300$.

---

### S65. Static vs Dynamic Scoping

**(a) Static Scoping (C's rule):** Output = `10`

`f()` is defined at global scope. Under static scoping, `f()` sees the global `x = 10`, regardless of who called it.

**(b) Dynamic Scoping:** Output = `20`

Under dynamic scoping, `f()` would see the `x` from its caller `g()`, which has local `x = 20`.

---

## Bonus Solutions

---

### S66. Comprehensive Pointer

**Output:**
```
2 2 3 
2 2 3 4 5 
```

**Trace step by step:**
- Initial: `a = {1,2,3,4,5}`, `p = &a[0]`
- `++*p`: Pre-increment the value at `p`. `*p = a[0]` becomes $2$. Print `2 `.
- `*p++`: Post-increment the pointer. Use `*p = a[0] = 2` (print `2 `), then `p` moves to `&a[1]`.
- `(*p)++`: Increment value at current `p` (which is `a[1]`). Print current value $2$, then `a[1]` becomes $3$.
- Array after: `{2, 3, 3, 4, 5}`.

Wait, let me retrace more carefully:
- `++*p`: `*p` is `a[0]=1`. `++` makes it $2$. Print: `2 `.
- `*p++`: Post-increment on `p`. Dereference first: `*p = a[0] = 2`. Print: `2 `. Then `p++` → `p = &a[1]`.
- `(*p)++`: `*p = a[1] = 2`. Post-increment: use $2$ then `a[1]` becomes $3$. Print: `3`?

Actually `(*p)++` is post-increment on the value. Value used = current value = $2$. After: `a[1] = 3`. So print `2`?

Hmm, but `printf("%d ", (*p)++)` — no, the code does `(*p)++` as a statement, then `printf("%d ", *p)`.

Let me re-read the code:
```c
++*p;
printf("%d ", *p);   // a[0] after increment
*p++;
printf("%d ", *p);   // after p moves to a[1]
(*p)++;
printf("%d ", *p);   // a[1] after increment
```

- `++*p`: `a[0]` goes from 1 to 2. `*p = 2`. Print: `2 `.
- `*p++`: dereference then increment pointer. The value `*p = 2` is computed but discarded (expression statement). Then `p` moves to `&a[1]`. Print `*p = a[1] = 2`. Print: `2 `.
- `(*p)++`: `a[1]` goes from 2 to 3. But post-increment: value is 2, then increment happens. But `printf` comes after, reading `*p = 3`. Print: `3 `.
- Array: `{2, 3, 3, 4, 5}`.

**Corrected Output:**
```
2 2 3 
2 3 3 4 5 
```

---

### S67. Pointer Maze

**Output:**
```
2 5 8
5
```

**Trace:**
- `p = &a[0][0]`, flat view: `{1,2,3,4,5,6,7,8,9}`.
- `*(p+1) = 2`, `*(p+4) = 5`, `*(p+7) = 8`.
- `a[2][1] - a[0][2] = 8 - 3 = 5`.

---

### S68. MCQ

**Answer: (b) and (c).**

- (a) FALSE — `sizeof` is a **compile-time** operator (except for VLAs).
- (b) TRUE — `register` variables cannot have their address taken with `&`.
- (c) TRUE — `static` local variables are initialized to $0$ by default.
- (d) FALSE — `extern` variables are in the **data segment**, not the stack.

---

## Section H: Unions, Typedef, Enum, Function Pointers, Preprocessor

### S69. Union Basics
`sizeof(union Data)` = `sizeof(float)` or `sizeof(int)` — both 4, but largest member is `char` at 1... Wait, `int`=4, `float`=4, `char`=1, so sizeof = **4**.
After `d.f = 3.14;`, `d.i` is **not predictable** — it gives the bit pattern of 3.14 reinterpreted as int. This is implementation-defined behavior (type punning).

Output:
```
4
1078523331  (or some garbage int value)
```

---

### S70. Struct vs Union Size
```
struct S: char(1) + 3 padding + int(4) + double(8) = 16 bytes
union U: max(char, int, double) = sizeof(double) = 8 bytes
```
**sizeof(struct S) = 16, sizeof(union U) = 8**

---

### S71. Typedef Trap
- `intptr a, b;` → `typedef int* intptr;` → **a is int\*, b is int\***
- `INTPTR c, d;` → `#define INTPTR int*` → expands to `int* c, d;` → **c is int\*, d is int**

This is a classic GATE trap — `typedef` applies to all variables, `#define` is text substitution.

---

### S72. Enum Values
- APPLE = 3, BANANA = 4 (auto-increment), CHERRY = 1, DATE = 2
**Output: `3 4 1 2`**

---

### S73. Function Pointer Basics
`fp = add` → `fp(3,4)` = 7. Then `fp = mul` → `fp(3,4)` = 12.
**Output: `7 12`**

---

### S74. Array of Function Pointers
- val = 2
- i=0: val = f1(2) = 3
- i=1: val = f2(3) = 6
- i=2: val = f3(6) = 36
**Output: `36`**

---

### S75. Macro Side Effects
`SQUARE(a++)` expands to `((a++) * (a++))`.
a starts at 3. First `a++` gives 3 (then a=4), second `a++` gives 4 (then a=5).
b = 3 * 4 = 12. a = 5.
**Output: `a=5 b=12`**
(Note: Technically undefined behavior due to multiple unsequenced modifications)

---

### S76. Macro vs Function
**With macro `MAX(x++, y++)`:**
Expands to `((x++) > (y++) ? (x++) : (y++))`.
5 > 3 is true, so result = second x++ = 6. x ends at 7, y = 4.
**Output: `6` then `x=7 y=4`**

**With function `max(x++, y++)`:**
Arguments evaluated once: max(5,3) = 5. x=6, y=4.
**Output: `5` then `x=6 y=4`**

---

### S77. Stringification and Token Pasting
- `STR(Hello World)` → `"Hello World"` (literal string)
- `CONCAT(var, 1)` → `var1` → creates variable `var1 = 100`
**Output:**
```
Hello World
100
```

---

### S78. Conditional Compilation
`X > 5` is true (10>5), so `int a = X * 2 = 20`.
After `#undef X`, the macro X is no longer defined — using X would cause a compiler error.
**Output: `20`**

---

### S79. Bit Fields
- version=15 (fits in 4 bits: max 15), type=3, length=200 (fits in 8 bits: max 255)
- Total bits used: 4+4+8 = 16 bits, but struct is padded to `sizeof(unsigned int)` = 4 bytes
- Cannot take address of bit fields.
**Output: `v=15 t=3 l=200` and `size=4`**

---

### S80. Volatile MCQ
**Answer: (a), (c), (d)**
- (a) TRUE — prevents compiler optimization of accesses
- (b) FALSE — `volatile` does NOT provide atomicity or thread safety
- (c) TRUE — MMIO registers must be read every time, not cached
- (d) TRUE — `const volatile` is valid (e.g., read-only hardware register)

---

### S81. Const Correctness
| Declaration | Pointer reassignable? | Value modifiable? |
|---|---|---|
| `const int *p1` | Yes (`p1 = &other`) | **No** (`*p1 = x` is error) |
| `int *const p2` | **No** (`p2 = &other` is error) | Yes (`*p2 = x`) |
| `const int *const p3` | **No** | **No** |

---

### S82. Complex Declaration
`int (*(*fp)(int, int))[10]`
Reading: fp is a **pointer to a function** taking (int, int) and **returning a pointer to an array of 10 ints**.

---

### S83. Union and Endianness
`u.i = 0x01020304`

**(a) Little-endian (e.g., x86):** LSB at lowest address.
`u.c[0]=04, u.c[1]=03, u.c[2]=02, u.c[3]=01`
**Output: `4 3 2 1`**

**(b) Big-endian:** MSB at lowest address.
`u.c[0]=01, u.c[1]=02, u.c[2]=03, u.c[3]=04`
**Output: `1 2 3 4`**

---

### S84. Function Pointer with typedef
`apply(sub, 10, 3)` calls `sub(10, 3)` = 10 − 3 = **7**.
**Output: `7`**

---

### S85. Preprocessor Pitfall
`PRINT(a+b, a-b)` expands to `printf("x=%d, y=%d\n", a+b, a-b)`.
Note: the `"x=%d, y=%d\n"` is a **string literal** — the x and y inside the string are NOT replaced. `#x` stringification only works with `#` operator in macro definition.
**Output: `x=3, y=-1`**

---
