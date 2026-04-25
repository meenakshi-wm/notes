# C Programming — Revision Notes for GATE 2027

---

> Quick-reference sheet. Review this before mocks and on exam day.

---

## 1. Data Type Sizes & Ranges (32-bit)

| Type | Size | Range |
|------|------|-------|
| `char` | 1 B | $-128$ to $127$ |
| `unsigned char` | 1 B | $0$ to $255$ |
| `short` | 2 B | $-32768$ to $32767$ |
| `int` | 4 B | $-2^{31}$ to $2^{31}-1$ |
| `unsigned int` | 4 B | $0$ to $2^{32}-1$ |
| `long long` | 8 B | $-2^{63}$ to $2^{63}-1$ |
| `float` | 4 B | ~$\pm3.4\times10^{38}$ |
| `double` | 8 B | ~$\pm1.7\times10^{308}$ |
| `pointer` | 4 B (32-bit) / 8 B (64-bit) | — |

**Signed n-bit:** $[-2^{n-1},\; 2^{n-1}-1]$ &emsp; **Unsigned n-bit:** $[0,\; 2^n - 1]$

---

## 2. Operator Precedence & Associativity (Complete)

| # | Operator | Assoc |
|---|---------|:---:|
| 1 | `()  []  ->  .  ++post  --post` | L→R |
| 2 | `++pre  --pre  +  -  !  ~  *deref  &addr  (cast)  sizeof` | **R→L** |
| 3 | `*  /  %` | L→R |
| 4 | `+  -` | L→R |
| 5 | `<<  >>` | L→R |
| 6 | `<  <=  >  >=` | L→R |
| 7 | `==  !=` | L→R |
| 8 | `&` (bitwise AND) | L→R |
| 9 | `^` (XOR) | L→R |
| 10 | `\|` (OR) | L→R |
| 11 | `&&` | L→R |
| 12 | `\|\|` | L→R |
| 13 | `?:` | **R→L** |
| 14 | `=  +=  -=  *=  /=  %=  <<=  >>=  &=  ^=  \|=` | **R→L** |
| 15 | `,` | L→R |

**Mnemonic:** **P-U-M-A-S-R-E-A-X-O-L-L-T-A-C**

---

## 3. Storage Classes

| | `auto` | `register` | `static` | `extern` |
|---|---|---|---|---|
| **Storage** | Stack | Register/Stack | Data seg | Data seg |
| **Default** | Garbage | Garbage | **0** | **0** |
| **Scope** | Block | Block | Block | Global |
| **Lifetime** | Block | Block | **Program** | **Program** |
| **`&` allowed?** | Yes | **No** | Yes | Yes |

---

## 4. Format Specifiers

| Specifier | Type | Notes |
|---|---|---|
| `%d` / `%i` | `int` | Signed decimal |
| `%u` | `unsigned int` | |
| `%ld` / `%lld` | `long` / `long long` | |
| `%f` | `float`/`double` | 6 decimal places |
| `%e` | Scientific | |
| `%c` | `char` | |
| `%s` | `char *` | Until `\0` |
| `%p` | `void *` | Address in hex |
| `%x` / `%o` | Hex / Octal | |
| `%%` | Literal `%` | |

---

## 5. Pointer Rules — Cheat Sheet

| Expression | Meaning |
|---|---|
| `int *p = &x` | p stores address of x |
| `*p` | Value at address p |
| `p + n` | Moves n × sizeof(*p) bytes forward |
| `p1 - p2` | Number of elements between pointers |
| `arr[i]` ≡ `*(arr+i)` ≡ `i[arr]` | Array-pointer equivalence |
| `sizeof(arr)` | Total array bytes |
| `sizeof(ptr)` | Pointer size (4/8) |

**Pointer arithmetic validity:**
- ✅ `ptr ± int`, `ptr - ptr`, `ptr compare ptr`
- ❌ `ptr + ptr`, `ptr * int`, `ptr / int`

---

## 6. Bitwise Tricks

| Operation | Code |
|---|---|
| Check bit n | `x & (1 << n)` |
| Set bit n | `x \| (1 << n)` |
| Clear bit n | `x & ~(1 << n)` |
| Toggle bit n | `x ^ (1 << n)` |
| Is power of 2 | `x && !(x & (x-1))` |
| Left shift n | $x \times 2^n$ |
| Right shift n | $\lfloor x / 2^n \rfloor$ |
| `~n` | $-(n+1)$ |
| Swap a, b | `a^=b; b^=a; a^=b;` |

---

## 7. Type Conversion Rules

```
char/short → int → unsigned int → long → unsigned long → long long → float → double → long double
```

**Key traps:**
- `signed` compared with `unsigned` → signed is converted to unsigned
- `int / int` = `int` (truncates). Cast to float for decimal result.
- $-1$ as unsigned = $2^{32}-1$

---

## 8. Integer Promotion

- `char` and `short` → promoted to `int` in all expressions
- Overflow in `char`: value wraps ($135$ in 8-bit signed = $135 - 256 = -121$)

---

## 9. Short-Circuit Rules

| Operator | Short-circuit when |
|---|---|
| `&&` | Left = 0 → skip right |
| `\|\|` | Left ≠ 0 → skip right |

Side effects in skipped operands are **NOT executed**.

---

## 10. Sequence Points

**Defined at:** `;` `&&` `||` `?:` `,` and before function call.

**Rule:** Modifying a variable more than once between sequence points = **Undefined Behavior**.

Examples of UB: `i = i++`, `i++ + i++`, `printf("%d %d", i++, i++)`.

---

## 11. Memory Layout

```
High  │ Stack  (↓ grows down)     — local, auto, register
      │ Free Space
      │ Heap   (↑ grows up)       — malloc, calloc
      │ BSS                        — uninitialized static/global (zeroed)
      │ Data                       — initialized static/global
Low   │ Text                       — code (read-only)
```

---

## 12. Dynamic Memory

| Function | Zero-init? | Syntax |
|---|---|---|
| `malloc(size)` | No | `(int*)malloc(n * sizeof(int))` |
| `calloc(n, size)` | **Yes** | `(int*)calloc(n, sizeof(int))` |
| `realloc(ptr, size)` | No | Resize existing allocation |
| `free(ptr)` | — | Release memory |

**Memory leak:** Losing reference to allocated block without calling `free`.  
**Dangling pointer:** Using `free(p)` then accessing `*p`. Fix: set `p = NULL` after free.

---

## 13. Strings

| | `char s[] = "Hi"` | `char *s = "Hi"` |
|---|---|---|
| Storage | Stack (array) | Read-only segment |
| Modifiable? | **Yes** | **No** (UB) |
| `sizeof(s)` | 3 (incl. `\0`) | 4/8 (ptr size) |
| `strlen(s)` | 2 | 2 |

---

## 14. Structures

- Access: `obj.member` or `ptr->member`
- **Padding:** Members aligned to their own size. Total = multiple of largest member.
- Passed by **value** (copy) to functions.
- Self-referential: `struct Node { int data; struct Node *next; };`

## 14a. Unions
- **sizeof(union)** = size of **largest** member
- Only **one member valid** at a time (last written)
- Type punning: reading different member = implementation-defined

## 14b. Typedef & Enum
- `typedef` = compiler-understood alias (type-safe)
- `#define` = text substitution (preprocessor)
- **Trap:** `typedef int* P; P a,b;` → both pointers. `#define P int*; P a,b;` → only a is pointer!
- `enum` values are `int`. Auto-increment after explicit value.

## 14c. Function Pointers
- `int (*fp)(int, int)` = pointer to function returning int
- Can create arrays: `int (*ops[])(int,int) = {add, sub};`
- Used as callbacks (e.g., `qsort` comparator)

## 14d. Preprocessor Quick Reference
| Directive | Use |
|-----------|-----|
| `#define F(x) ((x)*(x))` | Function-like macro (PARENTHESIZE!) |
| `#x` | Stringification |
| `a##b` | Token pasting |
| `#ifdef` / `#ifndef` | Conditional compilation |
| `#pragma once` | Include guard alternative |

**Traps:** Macros evaluate arguments multiple times → side effects with `++/--`

## 14e. Volatile & Const
- `volatile`: don't optimize away reads (MMIO, ISR). NOT thread-safe.
- `const int *p` → can't modify `*p`, can change `p`
- `int *const p` → can't change `p`, can modify `*p`
- `const volatile` → legal (read-only hardware register)

## 14f. Bit Fields
- `unsigned int field : n;` → n bits
- Cannot take address (`&`) of bit fields
- Ordering is implementation-defined

---

## 15. Recursion Checklist

1. Identify the **base case** (termination condition)
2. Identify the **recursive case** (problem reduction)
3. Trace the **call stack** step-by-step
4. Watch for **static variables** in recursion (shared across all calls)
5. Count total calls when asked (draw the recursion tree)

---

## 16. Complex Declarations

| Declaration | Read as |
|---|---|
| `int *p` | pointer to int |
| `int **p` | pointer to pointer to int |
| `int (*p)[N]` | pointer to array of N ints |
| `int *p[N]` | array of N pointers to int |
| `int (*p)(int)` | pointer to function(int) → int |
| `int (*p[N])(int)` | array of N function pointers |

**Rule:** `[]` and `()` bind tighter than `*`.

---

## 17. Top 12 GATE Traps

| # | Trap | Remember |
|---|---|---|
| 1 | `010` is octal (= 8) | Leading zero = octal |
| 2 | `-1 < 1u` is FALSE | Signed → unsigned conversion |
| 3 | `char c = 200` overflows | Signed char max = 127 |
| 4 | `sizeof(i++)` — i unchanged | sizeof doesn't evaluate |
| 5 | `a = 1,2,3` → a = 1 | `=` binds tighter than `,` |
| 6 | `char *s = "hi"; s[0]='H'` is UB | String literal is read-only |
| 7 | Array decays in function param | `sizeof(a)` = pointer size |
| 8 | `return &local` → dangling | Local destroyed on return |
| 9 | `i = i++ + ++i` → UB | Two modifications, no sequence point |
| 10 | `a \|\| (b=5)` → b maybe unchanged | Short-circuit skips side effects |
| 11 | `7/2` = 3, not 3.5 | Integer division truncates |
| 12 | `register int x; &x` → error | Can't take address of register |

---

## 18. Compilation Stages (Quick)

```
.c → Preprocessor(.i) → Compiler(.s) → Assembler(.o) → Linker(exe) → Loader(RAM)
```

---

## 19. Call by Value vs Pointer Simulation

- C is **always call by value**
- "Call by reference" = passing address (pointer)
- To modify a pointer itself in a function → pass `int **`

---

## 20. Scoping

| Type | Binding Time | C uses? |
|---|---|---|
| Static (Lexical) | Compile time | **Yes** |
| Dynamic | Runtime | No |

In static scoping, a function sees variables from where it's **defined**, not where it's **called**.

---
