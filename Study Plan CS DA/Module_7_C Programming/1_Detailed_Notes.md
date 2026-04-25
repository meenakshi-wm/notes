# 💻 C Programming — Detailed Notes for GATE 2027 CS/IT & DA 🎯

> 🌟 **"C is the mother of all modern programming languages — Linux, Windows, Python, MySQL, Git, and even your compiler are ALL written in C!"**

> 💡 **Beginner Analogy:** If programming languages were vehicles 🚗:
> - **C** = A manual transmission race car 🏎️ — gives you TOTAL control, super fast, but you need to handle everything yourself (gears, clutch = memory management)
> - **Python** = An automatic luxury car 🚙 — easy to drive, but you sacrifice control and speed
> - **Java** = A semi-automatic SUV 🚙 — garbage collector handles some things, but you're still more hands-on than Python
>
> C is the **fastest high-level language** and the closest to hardware. When performance matters (operating systems, game engines, embedded systems), C is king! 👑

---

## 🏭 Where is C Used in the Real World?

| 🌍 Project | 🔧 What | 📌 Why C? |
|---|---|---|
| **Linux Kernel** 🐧 | Powers 90% of servers, Android phones | Direct hardware access, speed |
| **Windows OS** 🪟 | Microsoft's operating system | Performance-critical kernel code |
| **Python Interpreter** 🐍 | CPython (the standard Python) is written in C! | Speed of the interpreter itself |
| **MySQL/PostgreSQL** 🗄️ | World's most popular databases | Billions of queries need raw speed |
| **Git** 🐙 | Version control used by 100M+ developers | Fast file operations |
| **Nginx/Apache** 🌐 | Web servers powering the internet | Handle millions of connections |
| **Embedded Systems** 🩸 | Car ECUs, IoT devices, smartwatches | Limited memory, direct hardware control |
| **Game Engines (Unreal)** 🎮 | AAA games (written in C/C++) | Real-time 60fps rendering |
| **NASA Curiosity Rover** 🚀 | Mars exploration | Reliability + hardware control in space |
| **OpenSSL** 🔐 | HTTPS encryption for every website | Cryptographic speed |

---

## 📊 GATE Weightage & Exam Pattern

| Aspect | Details |
|---|---|
| **Marks** | 3–5 marks per paper (1–2 questions) |
| **Question Type** | Output prediction, error finding, tricky expressions |
| **Difficulty** | Medium–High (traps in type promotion, pointers, operators) |
| **Key Areas** | Pointers ⭐, Recursion ⭐, Operators & Precedence ⭐, Storage Classes, Structures |

> **Strategy:** Most C questions are output-prediction. Master operator precedence, pointer arithmetic, and integer promotion rules—these account for 80% of GATE C questions.

---

## Module 1: Fundamentals, Control Flow & Operators

---

### 1.1 First C Program 👶

> 💡 **Beginner Analogy — The Recipe Card 📝:**
> A C program is like a recipe card:
> - `#include <stdio.h>` = "Open the cookbook" (import tools you need) 📖
> - `int main()` = "Start cooking here!" 👨‍🍳 (every program starts from main)
> - `printf("Hello!")` = "Serve the dish!" 🍽️ (output to screen)
> - `return 0` = "Kitchen is clean, all done!" ✅ (tell OS everything went well)
>
> 🏭 **Fun Fact:** Dennis Ritchie created C in 1972 at Bell Labs 🏛️. He also co-created UNIX. Both inventions are still in use 50+ years later — nothing else in tech has lasted this long! He's arguably the most influential programmer who ever lived. 🏆

```c
#include <stdio.h>   // Preprocessor directive — includes Standard I/O header

int main() {         // Entry point; returns int to OS
    printf("Hello, GATE 2027!\n");
    return 0;        // 0 = success
}
```

**Output:**
```
Hello, GATE 2027!
```

**Compilation pipeline (overview):**
```
Source (.c) → Preprocessor → Compiler → Assembler → Linker → Executable
```

---

### 1.2 Number Systems ⭐

| Base | Prefix | Digits | Example |
|------|--------|--------|---------|
| Binary (2) | `0b` (GCC ext) | 0, 1 | `0b1010` = 10 |
| Octal (8) | `0` | 0–7 | `017` = 15 |
| Decimal (10) | none | 0–9 | `15` |
| Hexadecimal (16) | `0x` | 0–9, A–F | `0xF` = 15 |

**Conversions:**

- **Decimal → Binary:** Repeated division by 2, read remainders bottom-up.
- **Binary → Octal:** Group bits in 3s from right. e.g., `101 110` → `56` (octal).
- **Binary → Hex:** Group bits in 4s from right. e.g., `1010 1111` → `0xAF`.

> **GATE Trap:** `int x = 010;` is **octal 10 = decimal 8**, NOT decimal 10!

```c
#include <stdio.h>
int main() {
    int a = 012;   // Octal → 10 in decimal
    int b = 0x1A;  // Hex → 26 in decimal
    printf("%d %d\n", a, b);
    return 0;
}
```
**Output:**
```
10 26
```

---

### 1.3 Data Types — Sizes & Ranges ⭐

Assuming a **32-bit system** (typical GATE assumption unless stated):

| Type | Size (bytes) | Range |
|------|-------------|-------|
| `char` | 1 | $-128$ to $127$ (signed) or $0$ to $255$ (unsigned) |
| `unsigned char` | 1 | $0$ to $255$ |
| `short` | 2 | $-32{,}768$ to $32{,}767$ |
| `int` | 4 | $-2^{31}$ to $2^{31}-1$ |
| `unsigned int` | 4 | $0$ to $2^{32}-1$ |
| `long` | 4 (32-bit) / 8 (64-bit) | varies |
| `long long` | 8 | $-2^{63}$ to $2^{63}-1$ |
| `float` | 4 | ~$\pm 3.4 \times 10^{38}$ (6–7 sig digits) |
| `double` | 8 | ~$\pm 1.7 \times 10^{308}$ (15–16 sig digits) |
| `pointer` | 4 (32-bit) / 8 (64-bit) | — |

**Formula for n-bit signed (2's complement):**  
$$\text{Range} = [-2^{n-1}, \; 2^{n-1} - 1]$$

**Formula for n-bit unsigned:**  
$$\text{Range} = [0, \; 2^n - 1]$$

---

### 1.4 Signed & Unsigned Numbers in Memory ⭐

**2's Complement Representation:**

For $n$-bit number $N$:  
$$\text{2's complement of } N = 2^n - N$$

Or equivalently: **Flip all bits + Add 1**

**Example:** Represent $-5$ in 8-bit 2's complement:
- $5 = 00000101$
- Flip: $11111010$
- Add 1: $11111011$ → This is $-5$

```c
#include <stdio.h>
int main() {
    unsigned int u = -1;   // All bits set to 1 → max unsigned value
    printf("%u\n", u);     // 4294967295 (2^32 - 1)
    printf("%d\n", u);     // -1 (same bits interpreted as signed)
    return 0;
}
```
**Output:**
```
4294967295
-1
```

> **GATE Trap:** When you assign a negative value to an `unsigned` variable, the bit pattern is reinterpreted — no error, just surprising output.

---

### 1.5 Integer Promotion ⭐⭐

**Rule:** In any expression, `char` and `short` are **promoted to `int`** before any operation.

```c
#include <stdio.h>
int main() {
    char a = 30, b = 40, c = 10;
    char d = (a * b) / c;   // a*b = 1200, computed as int, then truncated to char
    printf("%d\n", d);       // 1200/10 = 120 → fits in char
    return 0;
}
```
**Output:**
```
120
```

**Tricky case:**
```c
#include <stdio.h>
int main() {
    char c = 125;
    c = c + 10;        // 135 overflows signed char (max 127)
    printf("%d\n", c);  // Implementation-defined, typically -121
    return 0;
}
```
**Output (typical):**
```
-121
```

**Explanation:** $125 + 10 = 135$. In 8-bit signed: $135 - 256 = -121$.

**Usual Arithmetic Conversions (Promotion Hierarchy):**
```
char/short → int → unsigned int → long → unsigned long → long long → float → double → long double
```

---

### 1.6 Type Conversion ⭐

**Implicit (Automatic):**
- Narrow → Wide is safe (int → float)
- Wide → Narrow truncates (float → int drops decimal)

**Explicit (Casting):**
```c
int a = 7, b = 2;
float f = (float)a / b;  // 3.5 (a is cast to float before division)
float g = a / b;          // 3.0 (integer division first, then assigned to float)
```

> **GATE Trap:** `7/2` gives `3` (integer division), but `7.0/2` or `(float)7/2` gives `3.5`.

```c
#include <stdio.h>
int main() {
    int i = -1;
    unsigned int u = 1;
    if (i < u)
        printf("i < u\n");
    else
        printf("i >= u\n");
    return 0;
}
```
**Output:**
```
i >= u
```

**Why?** When comparing `int` and `unsigned int`, the `int` is converted to `unsigned`. $-1$ becomes $2^{32} - 1 = 4294967295$, which is greater than $1$.

---

### 1.7 Control Flow — If-Else

```c
#include <stdio.h>
int main() {
    int x = 5;
    if (x > 3)
        printf("A");
    else if (x > 4)
        printf("B");
    else
        printf("C");
    return 0;
}
```
**Output:** `A`  (first true condition wins; "B" is never checked)

**Dangling else problem:**
```c
if (a)
    if (b)
        printf("1");
else               // This else binds to the INNER if (closest unmatched)
    printf("2");
```

---

### 1.8 Switch-Case ⭐

```c
#include <stdio.h>
int main() {
    int x = 2;
    switch (x) {
        case 1: printf("One ");
        case 2: printf("Two ");
        case 3: printf("Three ");
        default: printf("Default ");
    }
    return 0;
}
```
**Output:** `Two Three Default ` (fall-through! No `break` statements)

**Key rules:**
- Case labels must be **integer constant expressions** (no variables, no floats)
- `break` exits the switch; without it, execution **falls through**
- `default` can be placed anywhere (but conventionally at end)
- Duplicate case labels → **compilation error**

---

### 1.9 Loops: While, For, Do-While

**While loop:**
```c
int i = 0;
while (i < 3) {
    printf("%d ", i);
    i++;
}
// Output: 0 1 2
```

**For loop:**
```c
for (int i = 0; i < 3; i++) {
    printf("%d ", i);
}
// Output: 0 1 2
```

**Do-While loop** (executes at least once):
```c
int i = 5;
do {
    printf("%d ", i);
    i++;
} while (i < 3);
// Output: 5   (body executes once even though condition is false)
```

**Infinite loops:**
```c
while (1) { }
for (;;) { }
do { } while (1);
```

---

### 1.10 Break & Continue

```c
for (int i = 0; i < 10; i++) {
    if (i == 3) continue;  // Skip i=3
    if (i == 7) break;     // Stop at i=7
    printf("%d ", i);
}
// Output: 0 1 2 4 5 6
```

- `break` — exits the **innermost** loop or switch
- `continue` — skips to the **next iteration** of the innermost loop

---

### 1.11 Operators in C ⭐⭐⭐

#### Complete Operator Precedence & Associativity Table

| Precedence | Operator | Description | Associativity |
|:---:|---|---|:---:|
| 1 | `()` `[]` `->` `.` `++`(post) `--`(post) | Postfix | L → R |
| 2 | `++`(pre) `--`(pre) `+`(unary) `-`(unary) `!` `~` `*`(deref) `&`(addr) `(type)` `sizeof` | Unary/Prefix | **R → L** |
| 3 | `*` `/` `%` | Multiplicative | L → R |
| 4 | `+` `-` | Additive | L → R |
| 5 | `<<` `>>` | Bitwise Shift | L → R |
| 6 | `<` `<=` `>` `>=` | Relational | L → R |
| 7 | `==` `!=` | Equality | L → R |
| 8 | `&` | Bitwise AND | L → R |
| 9 | `^` | Bitwise XOR | L → R |
| 10 | `\|` | Bitwise OR | L → R |
| 11 | `&&` | Logical AND | L → R |
| 12 | `\|\|` | Logical OR | L → R |
| 13 | `?:` | Ternary/Conditional | **R → L** |
| 14 | `=` `+=` `-=` `*=` `/=` `%=` `<<=` `>>=` `&=` `^=` `\|=` | Assignment | **R → L** |
| 15 | `,` | Comma | L → R |

> **Mnemonic:** "**P**lease **U**se **M**y **A**wesome **S**hift **R**ules **E**very **A**fternoon **X**eroxing **O**ld **L**ogical **L**essons **T**o **A**ssign **C**ommas"
> (Postfix, Unary, Mult, Add, Shift, Relational, Equality, AND, XOR, OR, Logical AND, Logical OR, Ternary, Assignment, Comma)

---

#### 1.11.1 Arithmetic Operators

```c
int a = 17, b = 5;
printf("%d\n", a / b);   // 3  (integer division truncates)
printf("%d\n", a % b);   // 2  (remainder)
printf("%d\n", -17 % 5); // -2 (sign of result = sign of dividend in C99+)
```

> **GATE Trap:** `%` operator cannot be used with `float`/`double`.

---

#### 1.11.2 Relational & Logical Operators

```c
int a = 5, b = 10, c = 5;
printf("%d\n", a == c);       // 1 (true)
printf("%d\n", a > b);        // 0 (false)
printf("%d\n", a && b);       // 1 (both non-zero)
printf("%d\n", !a);           // 0 (a is non-zero, so !a = 0)
printf("%d\n", a || 0);       // 1
```

---

#### 1.11.3 Bitwise Operators ⭐⭐

| Operator | Name | Example ($a=5$, $b=3$) | Binary | Result |
|---|---|---|---|---|
| `&` | AND | `5 & 3` | `0101 & 0011` | `0001` = 1 |
| `\|` | OR | `5 \| 3` | `0101 \| 0011` | `0111` = 7 |
| `^` | XOR | `5 ^ 3` | `0101 ^ 0011` | `0110` = 6 |
| `~` | NOT | `~5` | `~00...0101` | `11...1010` = $-6$ |
| `<<` | Left shift | `5 << 1` | `0101 << 1` | `1010` = 10 |
| `>>` | Right shift | `5 >> 1` | `0101 >> 1` | `0010` = 2 |

**Important formulas:**
- $a \ll n = a \times 2^n$
- $a \gg n = \lfloor a / 2^n \rfloor$ (for non-negative values)
- `~n = -(n+1)` (for 2's complement)

```c
#include <stdio.h>
int main() {
    printf("%d\n", ~0);    // -1
    printf("%d\n", ~1);    // -2
    printf("%d\n", ~(-1)); // 0
    printf("%d\n", 1 << 10); // 1024
    return 0;
}
```
**Output:**
```
-1
-2
0
1024
```

**Useful bit tricks for GATE:**
- Check if $n$-th bit is set: `x & (1 << n)`
- Set $n$-th bit: `x | (1 << n)`
- Clear $n$-th bit: `x & ~(1 << n)`
- Toggle $n$-th bit: `x ^ (1 << n)`
- Check power of 2: `x && !(x & (x-1))`
- Swap without temp: `a ^= b; b ^= a; a ^= b;`

---

#### 1.11.4 Assignment, Conditional & Comma Operators

**Conditional (Ternary):**
```c
int x = 5;
int y = (x > 3) ? 10 : 20;  // y = 10
```

**Comma Operator:**
```c
int a = (3, 5, 7);   // a = 7 (last expression's value)
printf("%d\n", a);    // 7
```

> **GATE Trap:** Comma operator has the **lowest** precedence.

```c
int a;
a = 1, 2, 3;     // a = 1 (assignment binds tighter than comma)
a = (1, 2, 3);   // a = 3 (parentheses force comma evaluation first)
```

---

### 1.12 Sequence Points ⭐⭐

A **sequence point** is a point in execution where all previous side effects are guaranteed to be complete.

**Sequence points occur at:**
1. End of a full expression (`;`)
2. `&&`, `||`, `?:`, `,` operators (between left and right operands)
3. Before a function is called (after arguments are evaluated)

**Undefined Behavior (UB) — modifying a variable twice between sequence points:**
```c
int i = 5;
i = i++ + ++i;   // UNDEFINED BEHAVIOR — don't predict output!
printf("%d %d\n", i++, i++);  // UNDEFINED — order of argument evaluation is unspecified
```

> **GATE Rule:** If a question involves modifying the same variable twice without an intervening sequence point, the answer is **Undefined Behavior**.

---

### 1.13 Short-Circuit Evaluation ⭐⭐

- `&&` — if left operand is **false (0)**, right operand is **NOT evaluated**
- `||` — if left operand is **true (non-zero)**, right operand is **NOT evaluated**

```c
#include <stdio.h>
int main() {
    int a = 0, b = 5;
    int c = a && (b = 10);  // a is 0, so (b=10) is NOT executed
    printf("b = %d, c = %d\n", b, c);

    int x = 1, y = 5;
    int z = x || (y = 10);  // x is 1, so (y=10) is NOT executed
    printf("y = %d, z = %d\n", y, z);
    return 0;
}
```
**Output:**
```
b = 5, c = 0
y = 5, z = 1
```

---

### 1.14 Pre/Post Increment ⭐

```c
int a = 5;
int b = a++;   // b = 5 (use then increment), a becomes 6
int c = ++a;   // a becomes 7 first, c = 7 (increment then use)
printf("a=%d b=%d c=%d\n", a, b, c);
// Output: a=7 b=5 c=7
```

---

## Module 2: Functions, Compilation & Storage Classes

---

### 2.1 Functions in C

```c
#include <stdio.h>

int add(int a, int b) {   // Function definition
    return a + b;
}

int main() {
    int result = add(3, 4);
    printf("%d\n", result);  // 7
    return 0;
}
```

**Key points:**
- C uses **call by value** — a copy of arguments is passed
- Functions return by value
- Default return type is `int` (in older C standards)
- Function declaration (prototype) before use is good practice

**GATE trap — implicit declaration:**
```c
#include <stdio.h>
int main() {
    float f = fun();     // If fun() is not declared, compiler assumes int return
    printf("%f\n", f);
    return 0;
}
float fun() { return 3.14; }
// Behavior depends on standard; C99 requires declaration before use
```

---

### 2.2 Compiler, Linker, Assembler, Loader ⭐

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌────────────┐
│ Source.c  │────→│Preprocessor────→│ Compiler │────→│ Assembler│────→│  Linker    │
│           │     │ (.i file) │     │(.s/.asm) │     │(.o/.obj) │     │(executable)│
└──────────┘     └──────────┘     └──────────┘     └──────────┘     └────────────┘
                                                                          │
                                                                    ┌─────▼─────┐
                                                                    │  Loader   │
                                                                    │(into RAM) │
                                                                    └───────────┘
```

| Stage | Input | Output | Task |
|---|---|---|---|
| **Preprocessor** | `.c` | `.i` | Macro expansion, `#include`, `#ifdef` |
| **Compiler** | `.i` | `.s` (assembly) | Syntax/semantic check, optimize, generate assembly |
| **Assembler** | `.s` | `.o` (object) | Convert assembly to machine code (relocatable) |
| **Linker** | `.o` + libraries | executable | Resolve external symbols, combine object files |
| **Loader** | executable | — | Load into memory, set up segments, start execution |

---

### 2.3 Memory Layout of a C Program ⭐⭐

```
High Address ┌───────────────────┐
             │  Command Line Args│
             │  & Environment    │
             ├───────────────────┤
             │      Stack        │  ← Local variables, function call frames
             │   (grows ↓)       │     LIFO; auto & register vars here
             ├───────────────────┤
             │        ↓          │
             │   Free Space      │
             │        ↑          │
             ├───────────────────┤
             │      Heap         │  ← Dynamic allocation (malloc/calloc)
             │   (grows ↑)       │
             ├───────────────────┤
             │  Uninitialized    │  ← BSS segment (static/global = 0 by default)
             │  Data (BSS)       │
             ├───────────────────┤
             │  Initialized Data │  ← static/global with explicit initial values
             ├───────────────────┤
             │   Text (Code)     │  ← Machine instructions (read-only)
Low Address  └───────────────────┘
```

**Key facts:**
- **Stack** grows downward (high → low address)
- **Heap** grows upward (low → high address)
- Global/static variables initialized to **0** by default (in BSS)
- Local variables are **uninitialized (garbage)** by default
- String literals stored in **read-only** section (modifying → UB/segfault)

---

### 2.4 Storage Classes ⭐⭐⭐

| Property | `auto` | `register` | `static` (local) | `extern` |
|---|---|---|---|---|
| **Storage** | Stack | CPU register (or stack) | Data segment | Data segment |
| **Default value** | Garbage | Garbage | **0** | **0** |
| **Scope** | Block | Block | Block | Global (across files) |
| **Lifetime** | Until block ends | Until block ends | **Entire program** | **Entire program** |
| **Linkage** | None | None | None | External |

#### auto (default for local variables)
```c
void foo() {
    auto int x = 10;  // Same as: int x = 10;
    // x destroyed when foo() returns
}
```

#### register
```c
void foo() {
    register int i;
    // Request to store in CPU register (compiler may ignore)
    // CANNOT take address: &i is ILLEGAL
}
```

#### static (local) ⭐⭐
```c
#include <stdio.h>
void counter() {
    static int count = 0;  // Initialized ONCE, persists across calls
    count++;
    printf("%d ", count);
}
int main() {
    counter();  // 1
    counter();  // 2
    counter();  // 3
    return 0;
}
```
**Output:** `1 2 3`

> **Analogy:** A `static` local variable is like a diary locked in a room — only accessible inside the room, but the writing persists even after you leave and come back.

#### extern
```c
// file1.c
int globalVar = 42;   // Definition

// file2.c
extern int globalVar;  // Declaration — uses the same variable from file1.c
```

**GATE-critical example:**
```c
#include <stdio.h>
int x = 10;                // Global, initialized data segment
void foo() {
    static int y = 20;     // Static local, data segment, initialized once
    y++;
    printf("x=%d y=%d\n", x, y);
}
int main() {
    foo();   // x=10 y=21
    x = 30;
    foo();   // x=30 y=22
    return 0;
}
```
**Output:**
```
x=10 y=21
x=30 y=22
```

---

## Module 3: Recursion, Pointers, Arrays, Strings, Structures

---

### 3.1 Recursion ⭐⭐

**Base case + Recursive case = Complete recursion**

```c
#include <stdio.h>
int factorial(int n) {
    if (n <= 1) return 1;        // Base case
    return n * factorial(n - 1); // Recursive case
}
int main() {
    printf("%d\n", factorial(5)); // 120
    return 0;
}
```

**Call stack trace for `factorial(4)`:**
```
factorial(4) → 4 * factorial(3)
  factorial(3) → 3 * factorial(2)
    factorial(2) → 2 * factorial(1)
      factorial(1) → returns 1
    returns 2*1 = 2
  returns 3*2 = 6
returns 4*6 = 24
```

**Tricky recursion — print order matters:**
```c
#include <stdio.h>
void fun(int n) {
    if (n <= 0) return;
    printf("%d ", n);   // Print BEFORE recursive call
    fun(n - 1);
    printf("%d ", n);   // Print AFTER recursive call (unwinding)
}
int main() {
    fun(3);
    return 0;
}
```
**Output:** `3 2 1 1 2 3`

**Tail recursion:** Recursive call is the **last** operation — can be optimized to a loop by the compiler.

```c
int factTail(int n, int acc) {
    if (n <= 1) return acc;
    return factTail(n - 1, n * acc);  // Tail recursive
}
```

---

### 3.2 Pointers — Basics ⭐⭐⭐

A **pointer** stores the **memory address** of another variable.

```
┌──────────┐     ┌──────────┐
│  p        │────→│  x = 10  │
│ 0x1000    │     │ addr:    │
│ (stores   │     │ 0x2000   │
│  0x2000)  │     │          │
└──────────┘     └──────────┘
  pointer           variable
```

```c
#include <stdio.h>
int main() {
    int x = 10;
    int *p = &x;        // p stores address of x
    printf("%d\n", *p);  // 10 (dereferencing)
    *p = 20;             // Modify x through pointer
    printf("%d\n", x);   // 20
    return 0;
}
```

**Key pointer operations:**
| Expression | Meaning |
|---|---|
| `int *p` | Declare pointer to int |
| `p = &x` | Store address of x in p |
| `*p` | Dereference — access value at address stored in p |
| `&x` | Address-of — get memory address of x |

---

### 3.3 Pointer Arithmetic ⭐⭐⭐

**Rule:** Pointer arithmetic is scaled by `sizeof(pointed type)`.

```c
int arr[] = {10, 20, 30, 40};
int *p = arr;       // p points to arr[0]

// Memory layout (assuming int = 4 bytes, base address 1000):
// Address: 1000  1004  1008  1012
// Value:    10    20    30    40
//           p    p+1   p+2   p+3

printf("%d\n", *p);       // 10
printf("%d\n", *(p+1));   // 20
printf("%d\n", *(p+2));   // 30
printf("%p\n", (void*)p);     // 0x3E8 (1000)
printf("%p\n", (void*)(p+1)); // 0x3EC (1004) — moved by sizeof(int) = 4
```

**Valid pointer arithmetic:**
- `ptr + n` (pointer + integer)
- `ptr - n` (pointer - integer)
- `ptr1 - ptr2` (pointer - pointer → number of elements between them)
- Pointer comparison (`<`, `>`, `==`, `!=`)

**Invalid:**
- `ptr1 + ptr2` ✗
- `ptr * n` ✗
- `ptr / n` ✗

```c
int arr[] = {10, 20, 30, 40, 50};
int *p = &arr[1], *q = &arr[4];
printf("%ld\n", q - p);  // 3 (number of elements, NOT bytes)
```

---

### 3.4 Arrays ⭐⭐

#### 1D Array
```c
int arr[5] = {1, 2, 3, 4, 5};

// Array name decays to pointer to first element
printf("%d\n", *arr);       // 1
printf("%d\n", *(arr+2));   // 3
printf("%d\n", arr[2]);     // 3  (arr[i] ≡ *(arr+i))
printf("%d\n", 2[arr]);     // 3  (i[arr] ≡ *(i+arr) — valid but unusual!)
```

> **GATE Trap:** `arr[i]` is exactly `*(arr + i)`, so `i[arr]` is also valid!

#### 2D Array
```c
int mat[2][3] = {{1,2,3}, {4,5,6}};

// Memory layout (row-major order):
// mat[0][0]=1 mat[0][1]=2 mat[0][2]=3 mat[1][0]=4 mat[1][1]=5 mat[1][2]=6

// mat     → pointer to first row (int (*)[3])
// mat[0]  → pointer to first element of first row (int *)
// mat[1]  → pointer to first element of second row (int *)

printf("%d\n", mat[1][2]);        // 6
printf("%d\n", *(*(mat+1)+2));    // 6 (equivalent)
printf("%d\n", *(mat[0] + 4));    // 5 (treats as flat array: 1,2,3,4,5)
```

**Address formula for `mat[i][j]` in row-major:**
$$\text{Address} = \text{Base} + (i \times \text{cols} + j) \times \text{sizeof(element)}$$

#### 3D Array
```c
int a[2][3][4];
// a[i][j][k] stored at:
// Base + (i*3*4 + j*4 + k) * sizeof(int)
```

---

### 3.5 Pointer-Array Relationship ⭐⭐

```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;   // arr decays to &arr[0]

// All equivalent ways to access arr[2]:
arr[2]       *(arr + 2)      p[2]       *(p + 2)       2[arr]       2[p]
```

**Key difference:** `arr` is NOT a pointer variable (cannot do `arr++`), but `p` is.

```c
sizeof(arr)  // 20 (5 * 4 bytes — total array size)
sizeof(p)    // 4 or 8 (pointer size)
```

---

### 3.6 Sizeof Operator ⭐

```c
#include <stdio.h>
int main() {
    int arr[10];
    int *p = arr;
    printf("%lu\n", sizeof(arr));    // 40 (10 * 4)
    printf("%lu\n", sizeof(p));      // 4 or 8 (pointer size)
    printf("%lu\n", sizeof(*p));     // 4 (size of int)
    printf("%lu\n", sizeof(arr)/sizeof(arr[0]));  // 10 (number of elements)

    char str[] = "Hello";
    printf("%lu\n", sizeof(str));    // 6 (5 chars + '\0')
    printf("%lu\n", strlen(str));    // 5 (doesn't count '\0')
    return 0;
}
```

> **GATE Trap:** `sizeof` is a **compile-time** operator (except for VLAs). It does NOT evaluate its operand.

```c
int i = 5;
printf("%lu\n", sizeof(i++));  // sizeof(int) = 4, but i is STILL 5!
```

---

### 3.7 Strings (Character Arrays) ⭐

```c
char s1[] = "Hello";        // Array of 6 chars: {'H','e','l','l','o','\0'}
char *s2 = "Hello";         // Pointer to string literal (read-only!)
char s3[] = {'H','i','\0'}; // Same as "Hi"

// s1 is modifiable, s2 is NOT (points to read-only memory)
s1[0] = 'J';    // OK → "Jello"
// s2[0] = 'J'; // UNDEFINED BEHAVIOR — segfault likely
```

**Memory diagram:**
```
Stack:          s1: ['H']['e']['l']['l']['o']['\0']   (modifiable)
                s2: [pointer] ──→ Read-only section: "Hello"

Read-only:      "Hello\0"
```

**String functions (from `<string.h>`):**
| Function | Purpose |
|---|---|
| `strlen(s)` | Length (excludes `\0`) |
| `strcpy(dest, src)` | Copy string |
| `strcat(dest, src)` | Concatenate |
| `strcmp(s1, s2)` | Compare (0 if equal) |

---

### 3.8 Pointer to Array vs Array of Pointers ⭐

```c
int (*p)[5];    // p is a POINTER TO an array of 5 ints
int *q[5];      // q is an ARRAY OF 5 pointers to int
```

**Pointer to array:**
```c
int arr[5] = {1, 2, 3, 4, 5};
int (*p)[5] = &arr;
printf("%d\n", (*p)[2]);   // 3
// p+1 moves by sizeof(int[5]) = 20 bytes
```

**Array of pointers:**
```c
int a = 1, b = 2, c = 3;
int *arr[3] = {&a, &b, &c};
printf("%d\n", *arr[1]);   // 2
```

---

### 3.9 Double Pointers (Pointer to Pointer) ⭐

```c
int x = 10;
int *p = &x;
int **pp = &p;

// Memory diagram:
//  pp          p           x
// ┌──────┐   ┌──────┐   ┌──────┐
// │ &p   │──→│ &x   │──→│  10  │
// └──────┘   └──────┘   └──────┘

printf("%d\n", **pp);   // 10
printf("%d\n", *p);     // 10
```

**Use case:** Modifying a pointer inside a function:
```c
void allocate(int **pp) {
    *pp = (int *)malloc(sizeof(int));
    **pp = 42;
}
int main() {
    int *p = NULL;
    allocate(&p);
    printf("%d\n", *p);  // 42
    free(p);
    return 0;
}
```

---

### 3.10 Void Pointer

```c
int x = 10;
float y = 3.14;
void *vp;

vp = &x;
printf("%d\n", *(int *)vp);     // 10 — must cast before dereferencing

vp = &y;
printf("%f\n", *(float *)vp);   // 3.140000
```

**Rules:**
- Cannot dereference `void *` directly (need cast)
- Cannot do pointer arithmetic on `void *` (unknown size) — GCC allows as extension (treats as `char *`)
- `malloc()` returns `void *`

---

### 3.11 Passing Arrays to Functions

```c
#include <stdio.h>

// All three are equivalent for 1D array:
void print(int arr[], int n)   { /* ... */ }
void print(int *arr, int n)    { /* ... */ }
void print(int arr[100], int n){ /* 100 is ignored */ }

// For 2D array — must specify column size:
void print2D(int mat[][3], int rows) {
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < 3; j++)
            printf("%d ", mat[i][j]);
}
```

> **Key:** Arrays always **decay to pointers** when passed to functions. `sizeof(arr)` inside the function gives pointer size, NOT array size.

---

### 3.12 Dynamic Memory Allocation ⭐⭐

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    // malloc — allocates uninitialized memory
    int *p = (int *)malloc(5 * sizeof(int));  // 5 ints
    if (p == NULL) { printf("Allocation failed\n"); return 1; }

    for (int i = 0; i < 5; i++) p[i] = (i + 1) * 10;
    for (int i = 0; i < 5; i++) printf("%d ", p[i]); // 10 20 30 40 50

    // calloc — allocates AND initializes to 0
    int *q = (int *)calloc(5, sizeof(int));

    // realloc — resize previously allocated memory
    p = (int *)realloc(p, 10 * sizeof(int));

    free(p);   // Release memory
    free(q);
    return 0;
}
```

**Memory diagram:**
```
Stack:                Heap:
┌──────┐            ┌────┬────┬────┬────┬────┐
│ p ────────────→   │ 10 │ 20 │ 30 │ 40 │ 50 │
└──────┘            └────┴────┴────┴────┴────┘
```

| Function | Syntax | Zero-init? | Header |
|---|---|---|---|
| `malloc` | `malloc(size)` | No (garbage) | `<stdlib.h>` |
| `calloc` | `calloc(n, size)` | **Yes** | `<stdlib.h>` |
| `realloc` | `realloc(ptr, new_size)` | No | `<stdlib.h>` |
| `free` | `free(ptr)` | — | `<stdlib.h>` |

---

### 3.13 Memory Leak & Dangling Pointer ⭐

**Memory Leak:** Allocated memory not freed → can't be reclaimed.
```c
void leak() {
    int *p = (int *)malloc(100 * sizeof(int));
    // p goes out of scope without free(p) → MEMORY LEAK
}
```

**Dangling Pointer:** Pointer points to freed/invalid memory.
```c
int *p = (int *)malloc(sizeof(int));
*p = 42;
free(p);       // Memory freed
// p is now DANGLING — still holds the old address
// *p = 10;    // UNDEFINED BEHAVIOR
p = NULL;      // Good practice: set to NULL after free
```

**Other dangling pointer cases:**
```c
int *foo() {
    int x = 10;
    return &x;     // DANGLING — x is destroyed when foo returns!
}
```

---

### 3.14 Complex Declarations

**Clockwise/Spiral Rule:** Read declarations starting from the variable name, spiraling outward.

| Declaration | Meaning |
|---|---|
| `int *p` | p is a pointer to int |
| `int **p` | p is a pointer to pointer to int |
| `int (*p)[10]` | p is a pointer to array of 10 ints |
| `int *p[10]` | p is an array of 10 pointers to int |
| `int (*p)(int)` | p is a pointer to function taking int, returning int |
| `int *(*p)(int)` | p is a pointer to function taking int, returning pointer to int |
| `int (*p[5])(int)` | p is an array of 5 pointers to functions taking int, returning int |

---

### 3.15 Structures ⭐

```c
#include <stdio.h>

struct Point {
    int x;
    int y;
};

int main() {
    struct Point p1 = {10, 20};
    struct Point *ptr = &p1;

    printf("%d %d\n", p1.x, p1.y);       // 10 20
    printf("%d %d\n", ptr->x, ptr->y);    // 10 20  (-> operator)
    printf("%d %d\n", (*ptr).x, (*ptr).y); // 10 20  (equivalent)
    return 0;
}
```

**Structure padding & sizeof:**
```c
struct A {
    char c;    // 1 byte + 3 padding
    int i;     // 4 bytes
    char d;    // 1 byte + 3 padding
};
printf("%lu\n", sizeof(struct A));  // 12 (not 6!)

struct B {
    char c;    // 1 byte
    char d;    // 1 byte + 2 padding
    int i;     // 4 bytes
};
printf("%lu\n", sizeof(struct B));  // 8 (reordering saves space)
```

> **Padding Rule:** Each member is aligned to its own size. The struct total is a multiple of the largest member's size.

---

### 3.16 Unions ⭐

A `union` shares memory among all its members — only **one member** is valid at any time.

```c
union Data {
    int i;      // 4 bytes
    float f;    // 4 bytes
    char str[20]; // 20 bytes
};
printf("%lu\n", sizeof(union Data));  // 20 (size of largest member)
```

**Key Differences: struct vs union:**
| Feature | struct | union |
|---------|--------|-------|
| Memory | Sum of all members (+ padding) | Size of largest member |
| Access | All members simultaneously valid | Only last written member valid |
| Use case | Group related data | Save memory, variant types |

**GATE Trap:** Reading a different member than the one last written gives **implementation-defined** behavior (type punning).

```c
union { int i; float f; } u;
u.i = 42;
printf("%f\n", u.f);  // Implementation-defined! Not 42.0
```

---

### 3.17 Typedef ⭐

`typedef` creates an alias (new name) for an existing type.

```c
typedef unsigned long ulong;
typedef struct Node {
    int data;
    struct Node *next;
} Node;   // Now can use "Node" instead of "struct Node"

typedef int (*FuncPtr)(int, int);  // FuncPtr is a pointer-to-function type
```

**typedef vs #define:**
| Feature | typedef | #define |
|---------|---------|---------|
| Scope | Follows C scoping rules | Text replacement (preprocessor) |
| Type safety | Understood by compiler | Simple substitution |
| Pointer types | Handles correctly | Can cause bugs |

```c
typedef int* intptr;
#define INTPTR int*

intptr a, b;  // Both a and b are int*
INTPTR c, d;  // c is int*, d is int (only c gets the pointer!) ⚠️
```

---

### 3.18 Enumerations (enum) ⭐

```c
enum Color { RED, GREEN, BLUE };        // RED=0, GREEN=1, BLUE=2
enum Color { RED=5, GREEN, BLUE=1 };    // RED=5, GREEN=6, BLUE=1

enum Color c = GREEN;
printf("%d\n", c);  // 1 (or 6 in second example)
```

- Enum constants are **integers** (type `int` in C).
- Enum values can be **any integer expression** (duplicates allowed).
- `sizeof(enum Color)` = `sizeof(int)` typically = 4.

---

### 3.19 Function Pointers ⭐⭐

A pointer that stores the address of a function.

```c
int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

int (*fptr)(int, int);  // Declare function pointer
fptr = add;             // Point to 'add'
printf("%d\n", fptr(3, 4));  // 7

fptr = sub;
printf("%d\n", fptr(3, 4));  // -1
```

**Array of function pointers (callback table):**
```c
int (*ops[])(int, int) = {add, sub, mul, divf};
printf("%d\n", ops[0](10, 5));  // 15 (calls add)
printf("%d\n", ops[1](10, 5));  // 5  (calls sub)
```

**Passing function as argument (callback):**
```c
void apply(int (*func)(int, int), int a, int b) {
    printf("Result: %d\n", func(a, b));
}
apply(add, 3, 4);  // Result: 7
```

**qsort example:**
```c
int cmp(const void *a, const void *b) {
    return (*(int*)a - *(int*)b);
}
int arr[] = {5, 2, 8, 1};
qsort(arr, 4, sizeof(int), cmp);
```

---

### 3.20 Preprocessor Directives ⭐⭐

| Directive | Purpose |
|-----------|---------|
| `#define` | Macro definition |
| `#undef` | Undefine macro |
| `#include` | File inclusion |
| `#ifdef` / `#ifndef` | Conditional compilation |
| `#if` / `#elif` / `#else` / `#endif` | Conditional compilation |
| `#pragma` | Compiler-specific directives |
| `#error` | Force compilation error |
| `#line` | Reset line number |

**Object-like macros:**
```c
#define PI 3.14159
#define MAX_SIZE 100
```

**Function-like macros (with parentheses!):**
```c
#define SQUARE(x) ((x) * (x))     // Correct — fully parenthesized
#define BAD_SQ(x) x * x           // WRONG!

int a = SQUARE(3+1);  // ((3+1) * (3+1)) = 16 ✅
int b = BAD_SQ(3+1);  // 3+1 * 3+1 = 3+3+1 = 7 ❌ (operator precedence!)
```

**GATE Trap — Multiple evaluation:**
```c
#define MAX(a,b) ((a) > (b) ? (a) : (b))
int x = 5, y = 3;
int z = MAX(x++, y++);  // x++ evaluated TWICE if x > y!
// z = 6, x = 7, y = 4 (NOT what you'd expect from a function)
```

**Stringification (#) and Token Pasting (##):**
```c
#define STRINGIFY(x) #x          // Converts argument to string
#define CONCAT(a, b) a##b        // Concatenates tokens

printf("%s\n", STRINGIFY(hello)); // "hello"
int CONCAT(var, 1) = 42;         // Creates variable var1 = 42
```

**Conditional compilation:**
```c
#ifdef DEBUG
    printf("Debug: x = %d\n", x);
#endif

#ifndef HEADER_H
#define HEADER_H
// header contents (include guard)
#endif
```

---

### 3.21 Bit Fields in Structures ⭐

```c
struct Flags {
    unsigned int active : 1;   // 1 bit (0 or 1)
    unsigned int ready  : 1;
    unsigned int mode   : 3;   // 3 bits (0–7)
    unsigned int count  : 4;   // 4 bits (0–15)
};
printf("%lu\n", sizeof(struct Flags));  // Usually 4 (fits in one int)
```

- Cannot take address of bit field (`&` not allowed)
- Cannot use with arrays
- Implementation-defined: bit ordering, padding
- Useful for hardware register mapping, protocol headers

---

### 3.22 Volatile Keyword ⭐

`volatile` tells the compiler: **do not optimize** — the variable's value may change at any time (by hardware, ISR, another thread).

```c
volatile int flag = 0;

while (flag == 0) {
    // Without volatile, compiler may optimize this to infinite loop
    // (assuming flag never changes in this thread)
}
```

**Use cases:**
- Memory-mapped I/O registers
- Variables shared between ISR and main program
- Variables modified by another thread (legacy — prefer atomics)

**const volatile:** Variable that shouldn't be modified by code, but may change externally.
```c
const volatile int *status_reg = (const volatile int *)0x40000;
// Cannot write through this pointer, but value may change by hardware
```

---

### 3.23 Const Correctness ⭐⭐

| Declaration | Meaning |
|-------------|---------|
| `const int *p` | Pointer to constant int (can't modify `*p`, can change `p`) |
| `int *const p` | Constant pointer to int (can modify `*p`, can't change `p`) |
| `const int *const p` | Both pointer and pointee are constant |

**Reading rule (right-to-left):** Read declaration from right to left.
- `const int *p` → "p is a pointer to int that is const"
- `int *const p` → "p is a const pointer to int"

```c
const int x = 10;
const int *p = &x;
// *p = 20;   // ERROR — can't modify through p
p = &y;       // OK — can change where p points

int y = 30;
int *const q = &y;
*q = 40;      // OK — can modify the value
// q = &x;    // ERROR — can't change where q points
```

**GATE Trap:**
```c
const int a = 5;
int *p = (int *)&a;  // Cast away const (legal but dangerous)
*p = 10;              // Undefined behavior!
printf("%d %d\n", a, *p);  // May print 5 10 or 10 10 or anything
```

---

### 3.24 Call by Value vs Call by Reference (Simulation) ⭐

C only supports **call by value**. "Call by reference" is simulated using pointers.

```c
#include <stdio.h>

void swapByValue(int a, int b) {
    int temp = a; a = b; b = temp;
    // Changes are LOCAL — do not affect caller
}

void swapByPointer(int *a, int *b) {
    int temp = *a; *a = *b; *b = temp;
    // Modifies original variables through pointers
}

int main() {
    int x = 10, y = 20;
    swapByValue(x, y);
    printf("After value swap: %d %d\n", x, y);    // 10 20 (unchanged)
    swapByPointer(&x, &y);
    printf("After pointer swap: %d %d\n", x, y);  // 20 10 (swapped!)
    return 0;
}
```

---

### 3.17 Static vs Dynamic Scoping ⭐

**Static (Lexical) Scoping** — C uses this:
- Variable binding is determined at **compile time** by the structure of the source code.

**Dynamic Scoping:**
- Variable binding is determined at **runtime** by the calling sequence.

```c
int x = 10;

void f() {
    printf("%d\n", x);
}

void g() {
    int x = 20;
    f();
}

int main() {
    g();
    return 0;
}
```

| Scoping Type | Output | Why |
|---|---|---|
| **Static** (C) | `10` | `f()` sees the global `x` (lexically enclosing) |
| **Dynamic** | `20` | `f()` sees `x` from caller `g()` |

---

### 3.18 Format Specifiers Reference

| Specifier | Type | Example |
|---|---|---|
| `%d` / `%i` | `int` (decimal) | `printf("%d", 42)` → `42` |
| `%u` | `unsigned int` | `printf("%u", 42u)` → `42` |
| `%ld` | `long int` | |
| `%lld` | `long long int` | |
| `%f` | `float` / `double` | `printf("%f", 3.14)` → `3.140000` |
| `%lf` | `double` (scanf) | |
| `%e` | Scientific notation | `printf("%e", 3.14)` → `3.140000e+00` |
| `%c` | `char` | `printf("%c", 65)` → `A` |
| `%s` | `char *` (string) | |
| `%p` | pointer (address) | |
| `%x` / `%X` | Hex (lower/upper) | `printf("%x", 255)` → `ff` |
| `%o` | Octal | `printf("%o", 8)` → `10` |
| `%%` | Literal `%` | |

---

## Common GATE Traps — Summary

| Trap | Example | Pitfall |
|---|---|---|
| Octal literal | `int x = 010;` | x = 8 (not 10) |
| Signed/Unsigned comparison | `if (-1 < 1u)` | False! (-1 becomes large unsigned) |
| Integer promotion | `char c = 200;` | Overflow on signed char |
| `sizeof` doesn't evaluate | `sizeof(i++)` | `i` is unchanged |
| Comma operator | `a = 1,2,3;` | a = 1 (not 3) |
| String literal modification | `char *s = "hi"; s[0]='H';` | Undefined behavior |
| Array decay in function | `void f(int a[]) { sizeof(a); }` | Gives pointer size |
| Dangling pointer | `return &localVar;` | Points to invalid memory |
| Sequence points | `i = i++ + ++i;` | Undefined behavior |
| Short-circuit | `a \|\| (b=5)` | b unchanged if a is true |
| `%d` for unsigned | `printf("%d", UINT_MAX)` | Prints -1 |
| Modifying via void* | `*(void*)p = 5;` | Won't compile — must cast |

---

## Key Takeaways for GATE

1. **Master pointer arithmetic** — it's the #1 topic by frequency
2. **Know operator precedence** cold — especially `*`, `++`, `[]`, `->`
3. **Integer promotion & type conversion** appear almost every year
4. **Trace recursion** step by step — write the call stack
5. **Storage classes** — know default values (static/global = 0, auto = garbage)
6. **`sizeof`** vs `strlen` — classic trap
7. **Short-circuit evaluation** — side effects in conditions
8. Practice **output prediction** — the most common question format

---

## Module 4: Advanced Topics

---

### 4.1 File Handling in C ⭐

```c
FILE *fp = fopen("data.txt", "r");  // Open for reading
if (fp == NULL) {
    perror("Error opening file");
    return 1;
}
char line[256];
while (fgets(line, sizeof(line), fp)) {
    printf("%s", line);
}
fclose(fp);
```

**File Modes:**

| Mode | Meaning | File Exists? | File Not Exists? |
|---|---|---|---|
| `"r"` | Read only | Opens | Returns NULL |
| `"w"` | Write only | Truncates! | Creates |
| `"a"` | Append | Opens at end | Creates |
| `"r+"` | Read + Write | Opens | Returns NULL |
| `"w+"` | Read + Write | Truncates! | Creates |
| `"a+"` | Read + Append | Opens at end | Creates |

**Key Functions:**

| Function | Purpose |
|---|---|
| `fopen()` | Open file, returns FILE* |
| `fclose()` | Close file |
| `fgetc()` / `fputc()` | Read/write single char |
| `fgets()` / `fputs()` | Read/write string |
| `fprintf()` / `fscanf()` | Formatted I/O |
| `fread()` / `fwrite()` | Binary I/O |
| `fseek()` / `ftell()` | Random access |
| `rewind()` | Move to beginning |
| `feof()` | Check end of file |
| `ferror()` | Check error |

⚠️ **GATE Trap:** `fgets()` includes the newline `\n` in the buffer!

---

### 4.2 Command Line Arguments ⭐

```c
int main(int argc, char *argv[]) {
    // argc = argument count (including program name)
    // argv[0] = program name
    // argv[1], argv[2], ... = arguments
    printf("Program: %s\n", argv[0]);
    printf("Arg count: %d\n", argc);
    for (int i = 1; i < argc; i++)
        printf("argv[%d] = %s\n", i, argv[i]);
    return 0;
}
```

Running `./prog hello 42` → `argc = 3`, `argv[0]` = "./prog", `argv[1]` = "hello", `argv[2]` = "42"

⚠️ All arguments are **strings**! Use `atoi()` or `strtol()` to convert to numbers.

---

### 4.3 Variable Length Arrays (VLA) — C99+ ⭐

```c
int n = 10;
int arr[n];  // VLA — size determined at runtime
// sizeof(arr) = n * sizeof(int) — evaluated at runtime!
```

⚠️ VLAs are allocated on **stack** — large VLAs can cause stack overflow!
⚠️ Cannot use `static` or file-scope for VLAs.

---

### 4.4 Variadic Functions ⭐

```c
#include <stdarg.h>
int sum(int count, ...) {
    va_list args;
    va_start(args, count);
    int total = 0;
    for (int i = 0; i < count; i++)
        total += va_arg(args, int);
    va_end(args);
    return total;
}
// sum(3, 10, 20, 30) = 60
```

`printf` itself is variadic: `int printf(const char *format, ...);`

---

### 4.5 Inline Functions ⭐

```c
inline int max(int a, int b) {
    return (a > b) ? a : b;
}
```

- Suggests compiler to insert code at call site (no function call overhead)
- Compiler may ignore; just a hint
- Similar to macros but with type safety

---

### 4.6 Restrict Keyword (C99) ⭐

```c
void copy(int * restrict dest, const int * restrict src, int n) {
    for (int i = 0; i < n; i++)
        dest[i] = src[i];
}
```

Tells compiler: `dest` and `src` don't overlap → enables optimizations.

---

### 4.7 Designated Initializers (C99) ⭐

```c
int arr[5] = {[2] = 10, [4] = 20};  // {0, 0, 10, 0, 20}

struct Point { int x, y, z; };
struct Point p = {.y = 5, .x = 3};   // x=3, y=5, z=0
```

---

### 4.8 Compound Literals (C99) ⭐

```c
struct Point p = (struct Point){3, 4};
int *arr = (int[]){10, 20, 30};  // anonymous array
```

---

### 4.9 `_Generic` Selection (C11) ⭐

```c
#define type_name(x) _Generic((x), \
    int: "int", \
    float: "float", \
    double: "double", \
    default: "other")
printf("%s\n", type_name(42));    // "int"
printf("%s\n", type_name(3.14));  // "double"
```

---

### 4.10 Flexible Array Members ⭐

```c
struct Packet {
    int len;
    char data[];  // FAM — must be last member!
};
struct Packet *p = malloc(sizeof(struct Packet) + 100);
p->len = 100;
```

`sizeof(struct Packet)` does NOT include `data[]`.

---

### 4.11 Multi-dimensional Arrays & Pointers ⭐⭐

```c
int a[3][4];
// a[i][j] is at address: base + (i*4 + j) * sizeof(int)
// a        → pointer to first row: int (*)[4]
// a[0]     → pointer to first element of row 0: int *
// &a       → pointer to entire array: int (*)[3][4]
```

**Passing 2D array to function:**
```c
void f(int a[][4], int rows);     // Column size MUST be specified
void g(int (*a)[4], int rows);    // Equivalent
```

⚠️ `int **p` is NOT the same as `int a[3][4]`!
- `int **p` → pointer to pointer (for array of pointers)
- `int a[3][4]` → contiguous 12-element block

---

### 4.12 Linked List Implementation ⭐⭐

```c
struct Node {
    int data;
    struct Node *next;
};

// Insert at head
struct Node* push(struct Node *head, int val) {
    struct Node *new = malloc(sizeof(struct Node));
    new->data = val;
    new->next = head;
    return new;
}

// Print list
void print(struct Node *head) {
    while (head) {
        printf("%d -> ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}

// Free entire list
void freeList(struct Node *head) {
    struct Node *tmp;
    while (head) {
        tmp = head;
        head = head->next;
        free(tmp);
    }
}
```

---

### 4.13 Self-Referential Structures (Trees) ⭐

```c
struct TreeNode {
    int data;
    struct TreeNode *left;
    struct TreeNode *right;
};
```

---

### 4.14 Bitwise Operations Deep Dive ⭐⭐⭐

| Operation | Symbol | Example | Result |
|---|---|---|---|
| AND | `&` | `0b1100 & 0b1010` | `0b1000` (8) |
| OR | `\|` | `0b1100 \| 0b1010` | `0b1110` (14) |
| XOR | `^` | `0b1100 ^ 0b1010` | `0b0110` (6) |
| NOT | `~` | `~0b1100` | Inverts all bits |
| Left Shift | `<<` | `5 << 2` | `20` (multiply by 4) |
| Right Shift | `>>` | `20 >> 2` | `5` (divide by 4) |

**Common Bit Tricks:**

| Trick | Code | How it works |
|---|---|---|
| Check if even | `(n & 1) == 0` | LSB is 0 for even |
| Check if power of 2 | `n && !(n & (n-1))` | Only one bit set |
| Set bit at position k | `n \|= (1 << k)` | OR with mask |
| Clear bit at position k | `n &= ~(1 << k)` | AND with inverted mask |
| Toggle bit at position k | `n ^= (1 << k)` | XOR with mask |
| Get bit at position k | `(n >> k) & 1` | Shift and mask |
| Count set bits | `__builtin_popcount(n)` | GCC built-in |
| Lowest set bit | `n & (-n)` | Two's complement trick |
| Clear lowest set bit | `n & (n-1)` | Kernighan's method |
| Swap without temp | `a^=b; b^=a; a^=b;` | XOR swap |
| Multiply by $2^k$ | `n << k` | Left shift |
| Divide by $2^k$ | `n >> k` | Right shift (arithmetic for signed) |

⚠️ **GATE Traps:**
- Right shift of negative numbers is **implementation-defined**!
- Left shift of signed negative: **undefined behavior**!
- Left shift that causes overflow: **undefined behavior**!
- `~0` = `-1` on two's complement (all bits set)

---

### 4.15 extern, static, register — Detailed ⭐⭐

**`extern` keyword:**
```c
// file1.c
int global_var = 42;

// file2.c
extern int global_var;  // Declaration only — links to file1.c's definition
```

**`static` keyword — 3 uses:**
1. **Static local variable:** Persists between function calls
2. **Static global variable:** Limits scope to current file (internal linkage)
3. **Static function:** Only visible in current file

**`register` keyword:**
```c
register int i;  // Hint: store in CPU register for speed
// Cannot take address: &i is ILLEGAL!
```

---

# 📝 Practice Set 1: Output Prediction (P1 – P100)

> ⭐ The #1 GATE question type for C! Trace each program step by step.

**P1.** What is the output?
```c
int a = 5, b = 3;
printf("%d", a+++b);
```
→ Maximal munch: `a++ + b` = `5 + 3 = 8`. After: a = 6. **Output: 8**

**P2.** What is the output?
```c
int a = 5;
printf("%d %d", a++, ++a);
```
→ ⚠️ **Undefined behavior!** Order of evaluation of function arguments is unspecified.

**P3.** What is the output?
```c
int x = 5;
printf("%d", sizeof(x++));
```
→ `sizeof` does NOT evaluate its argument. `x` remains 5. **Output: 4** (assuming int = 4 bytes)

**P4.** What is the output?
```c
char *s = "Hello";
printf("%c", *s++);
printf("%c", *s);
```
→ `*s++` = `*(s++)` → dereference s (get 'H'), then increment s. Second: *s = 'e'.
**Output: He**

**P5.** What is the output?
```c
int a = 0;
if (a = 5)
    printf("True");
else
    printf("False");
```
→ `a = 5` is assignment, returns 5 (truthy). **Output: True**

**P6.** What is the output?
```c
int a = 1, b = 1;
int c = a || (b = 0);
printf("%d %d %d", a, b, c);
```
→ Short-circuit: `a` is 1 (true), so `(b=0)` is NOT evaluated. **Output: 1 1 1**

**P7.** What is the output?
```c
int a = 1, b = 1;
int c = a && (b = 0);
printf("%d %d %d", a, b, c);
```
→ `a` is 1, so `(b = 0)` IS evaluated. b becomes 0. `c = 1 && 0 = 0`. **Output: 1 0 0**

**P8.** What is the output?
```c
int i;
for (i = 0; i < 5; i++);
printf("%d", i);
```
→ Note the `;` after for — empty body! Loop runs 5 times, then i = 5. **Output: 5**

**P9.** What is the output?
```c
int a[5] = {1, 2, 3, 4, 5};
int *p = a + 3;
printf("%d", p[-2]);
```
→ `p[-2] = *(p - 2) = *(a + 3 - 2) = *(a + 1) = a[1] = 2`. **Output: 2**

**P10.** What is the output?
```c
char s[] = "GATE";
printf("%lu %lu", sizeof(s), strlen(s));
```
→ `sizeof(s) = 5` (includes '\0'), `strlen(s) = 4`. **Output: 5 4**

**P11.** What is the output?
```c
char *s = "GATE";
printf("%lu %lu", sizeof(s), strlen(s));
```
→ `sizeof(s) = 8` (pointer size on 64-bit), `strlen(s) = 4`. **Output: 8 4**

**P12.** What is the output?
```c
int a = 010;
printf("%d", a);
```
→ `010` is OCTAL! $010_8 = 8_{10}$. **Output: 8**

**P13.** What is the output?
```c
int a = 0x1A;
printf("%d", a);
```
→ $1A_{16} = 26_{10}$. **Output: 26**

**P14.** What is the output?
```c
printf("%d", -1 < 1u);
```
→ `-1` is int, `1u` is unsigned. `-1` gets converted to unsigned → large number. **Output: 0** (False!)

**P15.** What is the output?
```c
unsigned int x = -1;
printf("%u", x);
```
→ `-1` stored as all 1s → $2^{32} - 1 = 4294967295$. **Output: 4294967295**

**P16.** What is the output?
```c
#define SQ(x) x*x
int r = SQ(3+1);
```
→ Expands to `3+1*3+1 = 3+3+1 = 7` (not 16!). ⚠️ Always parenthesize macros: `((x)*(x))`
**Output: 7**

**P17.** What is the output?
```c
#define SQ(x) ((x)*(x))
int r = SQ(3+1);
```
→ `((3+1)*(3+1)) = 4*4 = 16`. **Output: 16**

**P18.** What is the output?
```c
int a = 5, b;
b = a > 3 ? a < 5 ? 100 : 200 : 300;
printf("%d", b);
```
→ `a > 3` → true → `a < 5` → false → `200`. **Output: 200**

**P19.** What is the output?
```c
int x = 1;
switch(x) {
    case 1: printf("A");
    case 2: printf("B");
    case 3: printf("C");
}
```
→ No `break`! Fall-through: prints ABC. **Output: ABC**

**P20.** What is the output?
```c
int x = 2;
switch(x) {
    case 1: printf("A"); break;
    case 2: printf("B"); break;
    case 3: printf("C"); break;
    default: printf("D");
}
```
→ Match case 2, print B, break. **Output: B**

**P21-P50** — Pointer & Array Questions:

**P21.** What is the output?
```c
int a[] = {10, 20, 30, 40, 50};
int *p = a;
printf("%d", *(p + 3));
```
→ `*(p+3)` = `a[3]` = 40. **Output: 40**

**P22.** What is the output?
```c
int a[] = {10, 20, 30};
printf("%d", 2[a]);
```
→ `2[a]` = `*(2+a)` = `*(a+2)` = `a[2]` = 30. **Output: 30**

**P23.** What is the output?
```c
char s[] = "GATE2027";
char *p = s;
printf("%c", *(p+4));
```
→ `*(p+4)` = `s[4]` = `'2'`. **Output: 2**

**P24.** What is the output?
```c
int a[3][2] = {{1,2},{3,4},{5,6}};
printf("%d", *(*(a+1)+1));
```
→ `a+1` = address of row 1. `*(a+1)` = row 1 start. `*(a+1)+1` = address of `a[1][1]`. `*(*(a+1)+1) = a[1][1] = 4`. **Output: 4**

**P25.** What is the output?
```c
int a = 10;
int *p = &a;
int **q = &p;
printf("%d", **q);
```
→ `**q = *(*q) = *(p) = a = 10`. **Output: 10**

**P26.** What is the output?
```c
void f(int *p) { *p = 100; }
int main() {
    int x = 50;
    f(&x);
    printf("%d", x);
}
```
→ Passed by pointer: x modified. **Output: 100**

**P27.** What is the output?
```c
void f(int p) { p = 100; }
int main() {
    int x = 50;
    f(x);
    printf("%d", x);
}
```
→ Passed by value: x unchanged. **Output: 50**

**P28.** What is the output?
```c
int *f() {
    int x = 10;
    return &x;
}
int main() {
    int *p = f();
    printf("%d", *p);
}
```
→ ⚠️ **Dangling pointer!** `x` is local to `f()`. Undefined behavior.

**P29.** What is the output?
```c
static int x = 10;
int *f() {
    return &x;
}
int main() {
    int *p = f();
    printf("%d", *p);
}
```
→ `static x` lives for entire program. **Output: 10** (safe!)

**P30.** What is the output?
```c
char *f() { return "Hello"; }
int main() {
    printf("%s", f());
}
```
→ String literals have static storage. **Output: Hello** (safe!)

**P31.** What is the output?
```c
int a[] = {1,2,3,4,5};
int *p = a;
p++;
printf("%d %d", *p, *(p+3));
```
→ `p` points to `a[1]`. `*p = 2`, `*(p+3) = a[4] = 5`. **Output: 2 5**

**P32.** What is the output?
```c
int a = 5;
printf("%d %d %d", a & 3, a | 3, a ^ 3);
```
→ `5 = 101`, `3 = 011`: AND = `001` = 1, OR = `111` = 7, XOR = `110` = 6. **Output: 1 7 6**

**P33.** What is the output?
```c
printf("%d", printf("GATE"));
```
→ Inner `printf("GATE")` prints "GATE" and returns 4. Outer prints 4. **Output: GATE4**

**P34.** What is the output?
```c
int f(int n) {
    if (n <= 1) return 1;
    return n * f(n-1);
}
printf("%d", f(5));
```
→ `5! = 120`. **Output: 120**

**P35.** What is the output?
```c
int f(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    return f(n-1) + f(n-2);
}
printf("%d", f(6));
```
→ Fibonacci: 0,1,1,2,3,5,8. `f(6) = 8`. **Output: 8**

**P36.** What is the output?
```c
void f(int n) {
    if (n > 0) {
        f(n-1);
        printf("%d ", n);
    }
}
f(4);
```
→ Recursive unwinding prints in order: **Output: 1 2 3 4**

**P37.** What is the output?
```c
void f(int n) {
    if (n > 0) {
        printf("%d ", n);
        f(n-1);
    }
}
f(4);
```
→ Print first, then recurse: **Output: 4 3 2 1**

**P38.** What is the output?
```c
void f(int n) {
    if (n > 0) {
        f(n-1);
        printf("%d ", n);
        f(n-1);
    }
}
f(3);
```
→ Trace: f(3): f(2): f(1): f(0) print 1 f(0) → print 2 → f(1): f(0) print 1 f(0) → print 3 → f(2): f(1): f(0) print 1 f(0) → print 2 → f(1): f(0) print 1 f(0)
→ **Output: 1 2 1 3 1 2 1**

**P39.** What is the output?
```c
int f(int n) {
    static int count = 0;
    count++;
    if (n <= 1) return count;
    f(n/2);
    return count;
}
printf("%d", f(16));
```
→ f(16) → f(8) → f(4) → f(2) → f(1). count incremented 5 times. **Output: 5**

**P40.** What is the output?
```c
int main() {
    int a = 10, b = 20;
    printf("%d", (a, b));
}
```
→ Comma operator: evaluates left, discards, returns right. **Output: 20**

**P41-P50** — Structure & Union Questions:

**P41.** What is the output?
```c
struct S {
    int a;
    char b;
    int c;
};
printf("%lu", sizeof(struct S));
```
→ Padding! Layout: `int a`(4) + `char b`(1) + padding(3) + `int c`(4) = **12**. (May vary by platform.)

**P42.** What is the output?
```c
union U {
    int a;
    char b;
};
union U u;
u.a = 65;
printf("%c", u.b);
```
→ Union shares memory. `u.a = 65`. `u.b` reads first byte = 65 = 'A'. **Output: A**

**P43.** What is the output?
```c
struct S { int a; char b; };
union U { int a; char b; };
printf("%lu %lu", sizeof(struct S), sizeof(union U));
```
→ Struct: 4+1+3(pad) = 8. Union: max(4,1) = 4. **Output: 8 4**

**P44.** What is the output?
```c
struct S { int x; } s = {10};
struct S *p = &s;
printf("%d %d %d", s.x, (*p).x, p->x);
```
→ All access the same member. **Output: 10 10 10**

**P45.** What is the output?
```c
enum Color { RED, GREEN, BLUE };
printf("%d %d %d", RED, GREEN, BLUE);
```
→ Enum starts at 0. **Output: 0 1 2**

**P46.** What is the output?
```c
enum Color { RED = 5, GREEN, BLUE = 20, YELLOW };
printf("%d %d %d %d", RED, GREEN, BLUE, YELLOW);
```
→ GREEN = 6, YELLOW = 21. **Output: 5 6 20 21**

**P47.** What is the output?
```c
typedef int (*FuncPtr)(int, int);
int add(int a, int b) { return a + b; }
int main() {
    FuncPtr fp = add;
    printf("%d", fp(3, 4));
}
```
→ Function pointer call. **Output: 7**

**P48.** What is the output?
```c
struct Node {
    int data;
    struct Node *next;
};
struct Node a = {10, NULL}, b = {20, &a};
printf("%d", b.next->data);
```
→ `b.next` = `&a`, so `b.next->data` = `a.data` = 10. **Output: 10**

**P49.** What is the output?
```c
int a = 5;
int b = ~a;
printf("%d", b);
```
→ `~5` on 32-bit two's complement: `~00...0101 = 11...1010 = -6`. **Output: -6**

**P50.** What is the output?
```c
#define MAX(a,b) ((a)>(b)?(a):(b))
int x = 3, y = 5;
printf("%d", MAX(x++, y++));
```
→ Expands to `((x++)>(y++)?(x++):(y++))`. `3 > 5` → false → `y++` again. 
→ y was incremented once in comparison (now 6), then incremented again → returns 6.
⚠️ Macro with side effects: **y is incremented TWICE!** **Output: 6**

---

# 📝 Practice Set 2: Quick Fire (P51 – P150)

| # | Q | A |
|---|---|---|
| P51 | `sizeof(char)` always? | 1 (by definition) |
| P52 | `sizeof(void)` in GCC? | 1 (extension, not standard) |
| P53 | NULL pointer dereference = ? | Undefined behavior (usually crash/segfault) |
| P54 | `int *p = NULL; p++;` → p value? | `sizeof(int)` (pointer arithmetic on NULL) — UB |
| P55 | `main()` return type? | `int` (standard) |
| P56 | Can `main` call itself? | Yes (recursion), but unusual |
| P57 | `\0` ASCII value? | 0 |
| P58 | `'0'` ASCII value? | 48 |
| P59 | `'A'` ASCII value? | 65 |
| P60 | `'a'` ASCII value? | 97 |
| P61 | `'a' - 'A'`? | 32 |
| P62 | Convert uppercase to lowercase: `ch + 32` or? | `ch \| 0x20` (bitwise trick!) |
| P63 | Convert lowercase to uppercase? | `ch & ~0x20` or `ch - 32` |
| P64 | Toggle case? | `ch ^ 0x20` |
| P65 | `isdigit('5')` equivalent? | `c >= '0' && c <= '9'` |
| P66 | Auto default value? | Garbage (indeterminate) |
| P67 | Static default value? | 0 |
| P68 | Global default value? | 0 |
| P69 | `register` can take address? | NO! |
| P70 | `extern` allocates memory? | NO! (declaration only) |
| P71 | Array name is a pointer? | No — decays to pointer in most expressions, but isn't one |
| P72 | `sizeof(array)` vs `sizeof(pointer)`? | Array: total bytes; Pointer: 4 or 8 bytes |
| P73 | `a[i]` = `i[a]`? | YES! Both = `*(a+i)` |
| P74 | `*p++` precedence? | `*(p++)` — dereference, then increment pointer |
| P75 | `(*p)++` does what? | Increment the VALUE pointed to |
| P76 | `++*p` does what? | Same as `++(*p)` — increment value |
| P77 | `*++p` does what? | Increment pointer, then dereference |
| P78 | `malloc(0)` returns? | Implementation-defined: NULL or unique pointer |
| P79 | `free(NULL)` is safe? | YES — no-op |
| P80 | `free()` twice = ? | Undefined behavior (double free) |
| P81 | `calloc` vs `malloc`? | `calloc` zero-initializes; `malloc` doesn't |
| P82 | `realloc(NULL, n)` = ? | Equivalent to `malloc(n)` |
| P83 | `realloc(p, 0)` = ? | Implementation-defined (may free) |
| P84 | Memory leak: allocated but? | Never freed (lost pointer) |
| P85 | `strlen("abc")` = ? | 3 |
| P86 | `sizeof("abc")` = ? | 4 (includes null terminator) |
| P87 | `strcmp("abc", "abd")` returns? | Negative (comparing 'c' < 'd') |
| P88 | `strcpy` copies including? | Null terminator |
| P89 | `strncpy(dest, src, n)`: null-terminated? | NOT guaranteed if `strlen(src) >= n`! |
| P90 | `strncat` vs `strcat`? | `strncat` limits chars appended |
| P91 | `sprintf` writes to? | String buffer (not stdout) |
| P92 | `snprintf` advantage? | Prevents buffer overflow |
| P93 | Format specifier for `long long`? | `%lld` |
| P94 | `%p` prints? | Pointer address (hex) |
| P95 | `%x` prints? | Unsigned hex |
| P96 | `%o` prints? | Unsigned octal |
| P97 | `%e` or `%E` prints? | Scientific notation |
| P98 | `%g` prints? | Shorter of `%f` and `%e` |
| P99 | `%zu` prints? | `size_t` value |
| P100 | `printf` returns? | Number of characters printed |
| P101 | `scanf` returns? | Number of items successfully read |
| P102 | `getchar()` returns? | `int` (not `char`!) — to handle EOF |
| P103 | `EOF` value? | -1 (typically) |
| P104 | `fflush(stdout)` does? | Forces output buffer to print |
| P105 | `fflush(stdin)` is? | Undefined behavior (don't use!) |
| P106 | `setbuf(stdout, NULL)` does? | Unbuffered output |
| P107 | `volatile int x;` means? | Compiler won't optimize access to x |
| P108 | When volatile needed? | Hardware registers, signal handlers, shared memory |
| P109 | `const int *p` = ? | Pointer to const int (can't modify via p) |
| P110 | `int * const p` = ? | Const pointer to int (can't change p itself) |
| P111 | `const int * const p` = ? | Both const: can't modify data or pointer |
| P112 | `restrict` means? | No aliasing — only this pointer accesses the data |
| P113 | `_Bool` type (C99)? | 0 or 1 |
| P114 | `_Complex` type (C99)? | Complex numbers |
| P115 | `_Atomic` type (C11)? | Atomic operations |
| P116 | `goto` in C: allowed? | Yes, but discouraged (spaghetti code) |
| P117 | `longjmp`/`setjmp` for? | Non-local jumps (like exception handling) |
| P118 | `atexit()` registers? | Functions to call at program exit |
| P119 | `assert()` does? | Aborts if condition false (disabled with NDEBUG) |
| P120 | `#pragma once` vs `#ifndef`? | Both prevent double inclusion; `#pragma once` is non-standard but widely supported |
| P121 | `##` in macros? | Token pasting: `A##B` → `AB` |
| P122 | `#` in macros? | Stringification: `#x` → `"x"` |
| P123 | `__LINE__` macro? | Current line number |
| P124 | `__FILE__` macro? | Current file name |
| P125 | `__func__` (C99)? | Current function name |
| P126 | Trigraphs in C? | `??=` → `#`, `??/` → `\`, etc. (removed in C23) |
| P127 | Digraphs in C? | `<:` → `[`, `:>` → `]`, etc. |
| P128 | Integer division: `7/2` = ? | 3 (truncates toward zero in C99+) |
| P129 | `-7/2` = ? | -3 (C99+: truncation toward zero) |
| P130 | `-7 % 2` = ? | -1 (sign follows dividend in C99+) |
| P131 | Can `sizeof` be used in `#if`? | NO! `sizeof` is not evaluated by preprocessor |
| P132 | Flexible array: `struct { int n; int a[]; }`; sizeof? | `sizeof(int)` — FAM not counted |
| P133 | `int (*fp)(void)` → ? | Pointer to function returning int |
| P134 | `int *fp(void)` → ? | Function returning pointer to int |
| P135 | `int (*a)[10]` → ? | Pointer to array of 10 ints |
| P136 | `int *a[10]` → ? | Array of 10 pointers to int |
| P137 | `void *` can point to any type? | YES (generic pointer) |
| P138 | Can dereference `void *` directly? | NO! Must cast first |
| P139 | `qsort` comparison function signature? | `int (*)(const void *, const void *)` |
| P140 | `bsearch` requires? | Sorted array |
| P141 | Stack overflow from recursion: why? | Too many stack frames |
| P142 | Tail recursion optimization? | Compiler may reuse stack frame (if enabled) |
| P143 | `memcpy` vs `memmove`? | `memmove` handles overlapping regions safely |
| P144 | `memset(a, 0, n)` safe for ints? | Yes (zero bytes = int 0) |
| P145 | `memset(a, 1, n)` for ints? | NO! Sets each byte to 1, NOT each int to 1 |
| P146 | Struct packing: `__attribute__((packed))`? | Remove padding (GCC) |
| P147 | Pragma pack: `#pragma pack(1)`? | Set alignment to 1 byte |
| P148 | Bit field: `int x : 3;` range? | Signed: -4 to 3. Unsigned: 0 to 7. |
| P149 | Can take address of bit field? | NO! |
| P150 | Maximum nesting of `#include`? | Implementation-defined (at least 15) |

---

# 📝 Practice Set 3: Recursion Deep Dive (P151 – P220)

**P151.** Tower of Hanoi: minimum moves for $n$ disks?
→ $T(n) = 2T(n-1) + 1$ → $T(n) = 2^n - 1$. For $n=4$: 15 moves.

**P152.** What does this function compute?
```c
int f(int n) {
    if (n == 0) return 1;
    return n * f(n-1);
}
```
→ Factorial: $n!$

**P153.** What does this print?
```c
void f(int n) {
    if (n == 0) return;
    f(n/10);
    printf("%d", n%10);
}
f(1234);
```
→ Prints digits: **Output: 1234** (recursive extraction)

**P154.** What does this print?
```c
void f(int n) {
    if (n == 0) return;
    printf("%d", n%10);
    f(n/10);
}
f(1234);
```
→ Prints digits reversed: **Output: 4321**

**P155.** Number of recursive calls for `f(5)` where `f(n) = f(n-1) + f(n-2)`, `f(0)=f(1)=1`?
→ Call tree: f(5)→f(4)+f(3). f(4)→f(3)+f(2). f(3)→f(2)+f(1)... Total calls = 15.

**P156.** Time complexity of naive recursive Fibonacci?
→ $O(2^n)$ — exponential! (overlapping subproblems)

**P157.** GCD using recursion (Euclidean):
```c
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
```
`gcd(48, 18)` → `gcd(18, 12)` → `gcd(12, 6)` → `gcd(6, 0)` → **6**

**P158.** Power: `pow(2,10)` recursively in $O(\log n)$?
```c
int power(int base, int exp) {
    if (exp == 0) return 1;
    if (exp % 2 == 0) {
        int half = power(base, exp/2);
        return half * half;
    }
    return base * power(base, exp-1);
}
```
**Output: 1024**

**P159.** Binary search recursive: $T(n) = T(n/2) + O(1)$ → $O(\log n)$

**P160.** What is `f(4)` for:
```c
int f(int n) {
    if (n <= 0) return 0;
    return f(n-1) + n;
}
```
→ `f(4) = 4+3+2+1+0 = 10`. Sum of 1 to n: $n(n+1)/2$.

**P161-P180** — Recursion output tracing:

| # | Function | Input | Output |
|---|---|---|---|
| P161 | `f(n) = f(n-1)*2, f(1)=1` | f(5) | 16 ($2^4$) |
| P162 | `f(n) = f(n-1)+f(n-2), f(0)=0, f(1)=1` | f(7) | 13 (Fibonacci) |
| P163 | `f(n) = 2*f(n/2)+1, f(1)=1` | f(8) | 15 ($2^n - 1$ pattern) |
| P164 | `f(n) = f(n/2) + 1, f(1)=0` | f(16) | 4 ($\log_2 n$) |
| P165 | `f(n) = n + f(n-1), f(0) = 0` | f(100) | 5050 ($n(n+1)/2$) |
| P166 | `f(n,k) = f(n-1,k-1)+f(n-1,k), f(n,0)=f(n,n)=1` | f(5,2) | 10 ($\binom{5}{2}$) |
| P167 | Ackermann: `A(1,1)` | | 3 |
| P168 | `A(2,1)` | | 5 |
| P169 | `A(3,1)` | | 13 |
| P170 | Number of calls for merge sort on 8 elements? | | 24 total merge sort calls |

**P171-P220** — Rapid Output Questions:

| # | Code Snippet | Output |
|---|---|---|
| P171 | `int x=5; printf("%d",x<<1);` | 10 |
| P172 | `int x=20; printf("%d",x>>2);` | 5 |
| P173 | `int x=7; printf("%d",x&3);` | 3 |
| P174 | `int x=5; printf("%d",x\|2);` | 7 |
| P175 | `int x=7; printf("%d",x^7);` | 0 |
| P176 | `int x=0; printf("%d",!x);` | 1 |
| P177 | `int x=5; printf("%d",!!x);` | 1 |
| P178 | `printf("%d",(int)3.9);` | 3 (truncation) |
| P179 | `printf("%d",(int)-3.9);` | -3 (toward zero) |
| P180 | `char c='A'+1; printf("%c",c);` | B |
| P181 | `char c='Z'-'A'; printf("%d",c);` | 25 |
| P182 | `printf("%d",'A'<'a');` | 1 (True: 65 < 97) |
| P183 | `printf("%d",'9'-'0');` | 9 |
| P184 | `int a=5,b=0; printf("%d",a&&b);` | 0 |
| P185 | `int a=5,b=0; printf("%d",a\|\|b);` | 1 |
| P186 | `printf("%d",sizeof(3.14));` | 8 (double) |
| P187 | `printf("%d",sizeof(3.14f));` | 4 (float) |
| P188 | `printf("%d",sizeof('A'));` | 4 (char promoted to int in C!) |
| P189 | `int a[10]; printf("%d",sizeof(a)/sizeof(a[0]));` | 10 |
| P190 | `int *p; printf("%lu",sizeof(p));` | 8 (64-bit) or 4 (32-bit) |
| P191 | `printf("%d", 5 > 3 > 1);` | 0 (5>3=1, 1>1=0) |
| P192 | `printf("%d", 5 > 3 > 0);` | 1 (5>3=1, 1>0=1) |
| P193 | `int x; printf("%d",sizeof(x=5));` | 4 (sizeof doesn't evaluate!) |
| P194 | `char s[]={'G','A','T','E'}; printf("%lu",sizeof(s));` | 4 (no null terminator!) |
| P195 | `char s[]="GATE"; printf("%lu",sizeof(s));` | 5 (includes '\0') |
| P196 | `int a=1; a<<=3; printf("%d",a);` | 8 |
| P197 | `int a=64; a>>=4; printf("%d",a);` | 4 |
| P198 | `printf("%d\n", 2+3*4);` | 14 |
| P199 | `printf("%d\n", (2+3)*4);` | 20 |
| P200 | `int a=1; printf("%d",a+++a);` | UB (modifying and reading same var without sequence point) |
| P201 | `for(int i=0;i<3;i++) for(int j=0;j<3;j++) if(i==j) printf("*"); else printf("."); ` | `*...*...*` |
| P202 | `int s=0; for(int i=1;i<=100;i++) s+=i; printf("%d",s);` | 5050 |
| P203 | `int i=0; while(i++<5); printf("%d",i);` | 6 |
| P204 | `int i=0; do{i++;}while(i<5); printf("%d",i);` | 5 |
| P205 | `for(;;) break; printf("done");` | done |
| P206 | `int i=10; while(i-->0); printf("%d",i);` | -1 |
| P207 | `int a=1,b=2,c=3; a=b=c; printf("%d %d %d",a,b,c);` | 3 3 3 (right-to-left) |
| P208 | `int a=5; a+=a-=a*=a; printf("%d",a);` | UB (multiple modifications) |
| P209 | `int a[]={0,1,2,3}; printf("%d",a[a[a[1]]]);` | a[a[a[1]]] = a[a[1]] = a[1] = 1 → **1** |
| P210 | `int a[]={0,1,2,3}; printf("%d",a[a[a[2]]]);` | a[a[a[2]]] = a[a[2]] = a[2] = 2 → **2** |
| P211 | `char *p="hello"; printf("%c",*p);` | h |
| P212 | `char *p="hello"; printf("%c",*(p+4));` | o |
| P213 | `char *p="hello"; printf("%s",p+2);` | llo |
| P214 | `char *p="hello"; p++; printf("%s",p);` | ello |
| P215 | `char s[]="hello"; s[0]='H'; printf("%s",s);` | Hello (modifiable) |
| P216 | `char *s="hello"; s[0]='H';` | ⚠️ UB! (modifying string literal) |
| P217 | `char s[]="ab"; printf("%d",s[2]);` | 0 (null terminator) |
| P218 | `char s[]=""; printf("%lu %lu",sizeof(s),strlen(s));` | 1 0 |
| P219 | `int n=-5; printf("%d",n%3);` | -2 |
| P220 | `unsigned u=5; int i=-1; printf("%d",u>i);` | 0 (i promoted to large unsigned!) |

---

# 📝 Practice Set 4: GATE PYQ Style (P221 – P320)

**P221.** (GATE style, 2-mark) What is the output?
```c
#include <stdio.h>
int main() {
    int i = 0;
    for ( ; ; ) {
        if (i == 5) break;
        printf("%d ", ++i);
    }
    return 0;
}
```
→ **Output: 1 2 3 4 5**

**P222.** (GATE style, 2-mark) What is the output?
```c
#include <stdio.h>
int main() {
    int a = 3, b = 5;
    int *p = &a, *q = &b;
    *p = *p ^ *q;
    *q = *p ^ *q;
    *p = *p ^ *q;
    printf("%d %d", a, b);
}
```
→ XOR swap: a and b are swapped. **Output: 5 3**

**P223.** (GATE style, 1-mark) What does the following evaluate to?
```c
int a = -5;
unsigned int b = 5;
(a + b == 0)
```
→ `a + b`: `-5` converted to unsigned → very large. `large + 5` wraps around to `0`. **True!** → 1

**P224.** (GATE style, 2-mark) What is the output?
```c
int f(int n) {
    if (n < 2) return n;
    if (n % 2 == 0) return f(n/2);
    return f(n/2) + 1;
}
printf("%d", f(25));
```
→ f(25): odd → f(12)+1. f(12): even → f(6). f(6): even → f(3). f(3): odd → f(1)+1 = 2. f(6) = 2. f(12) = 2. f(25) = 3.
→ **Output: 3** (counts 1-bits in binary: 25 = 11001 → 3 ones!)

**P225.** (GATE style, 2-mark) What is the size of the following struct (assume 64-bit, 4-byte alignment)?
```c
struct X {
    char a;
    int b;
    char c;
    double d;
};
```
→ `a`(1) + pad(3) + `b`(4) + `c`(1) + pad(7) + `d`(8) = **24** (with 8-byte alignment for double)

**P226.** (GATE style, 2-mark) What is the output?
```c
void f(char *t) {
    t = (char*)malloc(20);
    strcpy(t, "Hello");
}
int main() {
    char *s = NULL;
    f(s);
    printf("%s", s);
}
```
→ `t` is local copy of `s`. Modifying `t` doesn't change `s`. `s` is still NULL → **Undefined behavior** (crash likely)

**P227.** Fixed version:
```c
void f(char **t) {
    *t = (char*)malloc(20);
    strcpy(*t, "Hello");
}
int main() {
    char *s = NULL;
    f(&s);
    printf("%s", s);
    free(s);
}
```
→ **Output: Hello** (correct: pass pointer to pointer)

**P228-P320** — Mixed GATE Express:

| # | Question | Answer |
|---|---|---|
| P228 | `int a = 'A'; printf("%d", a);` | 65 |
| P229 | `printf("%d", (unsigned char)-1);` | 255 |
| P230 | Value of `&a[0]` vs `a` for `int a[5]`? | Same address (but different types) |
| P231 | `int a[2][3]; &a[1][0] - &a[0][0]`? | 3 (3 ints apart) |
| P232 | `char *p = "abc"; printf("%c", p[3]);` | `\0` (null terminator) |
| P233 | `int a = 2; printf("%d", a<<-1);` | UB! (negative shift count) |
| P234 | `sizeof(int[5])` = ? | 20 (5 × 4) |
| P235 | Can array size be 0 in C? | Not standard (some compilers allow as extension) |
| P236 | Recursive `strlen`: `return (*s) ? 1+strlen(s+1) : 0;` | Correct! |
| P237 | Number of `malloc` calls for $n \times m$ 2D array? | `1 + n` (1 for row pointers + n for rows) or 1 (contiguous) |
| P238 | `char *s = "hello"; char t[10]; strcpy(t, s);` safe? | Yes (t has enough space) |
| P239 | `char s[3]; strcpy(s, "hello");` safe? | NO! Buffer overflow! |
| P240 | `int *p = (int*)malloc(5 * sizeof(int)); free(p); *p = 10;` | UB: use-after-free |
| P241 | `static` function in file1.c visible in file2.c? | NO (internal linkage) |
| P242 | `extern` variable: defined or declared? | Declared (definition elsewhere) |
| P243 | `int arr[] = {1,2,3}; sizeof(arr)?` | 12 bytes (3 ints) |
| P244 | `int x = (1, 2, 3); printf("%d", x);` | 3 (comma operator returns last) |
| P245 | `printf("%c", 'A' + 2);` | C |
| P246 | Endianness: 0x12345678 in little-endian (first byte)? | 0x78 |
| P247 | Endianness: same in big-endian (first byte)? | 0x12 |
| P248 | How to detect endianness in C? | Store int, read first byte via char pointer |
| P249 | `%n` format specifier? | Writes number of chars printed so far to `int*` arg. ⚠️ Security risk! |
| P250 | `signed char` range? | -128 to 127 |
| P251 | `unsigned char` range? | 0 to 255 |
| P252 | `short` minimum size? | 16 bits |
| P253 | `int` minimum size? | 16 bits (usually 32) |
| P254 | `long` minimum size? | 32 bits |
| P255 | `long long` minimum size? | 64 bits |
| P256 | `float` precision? | ~7 decimal digits |
| P257 | `double` precision? | ~15 decimal digits |
| P258 | `long double` precision? | ~18-19 decimal digits (80-bit extended) |
| P259 | IEEE 754 float: sign + exponent + mantissa? | 1 + 8 + 23 bits |
| P260 | IEEE 754 double? | 1 + 11 + 52 bits |
| P261 | `0.1 + 0.2 == 0.3` in C? | `0` (False!) — floating-point imprecision |
| P262 | How to compare floats? | `fabs(a - b) < epsilon` |
| P263 | `NaN == NaN` in C? | `0` (False!) — NaN is not equal to anything |
| P264 | `isnan()` function? | Tests for NaN |
| P265 | `INFINITY` macro? | Positive infinity (from math.h) |
| P266 | `1.0/0.0` in C? | `INFINITY` (for IEEE 754) |
| P267 | `0.0/0.0` in C? | `NaN` |
| P268 | `INT_MAX + 1` = ? | UB! (signed overflow) |
| P269 | `UINT_MAX + 1` = ? | 0 (unsigned wraps around — defined behavior!) |
| P270 | Two's complement: -1 in binary? | All 1s |
| P271 | Two's complement: negate? | Invert bits + add 1 |
| P272 | `~0` = ? (signed) | -1 |
| P273 | `~0u` = ? (unsigned) | `UINT_MAX` |
| P274 | `int x = 5; x = x;` | No effect (but well-defined) |
| P275 | `int x; x = x;` | UB if x is uninitialized (reading indeterminate value) |
| P276 | `volatile int x; x = x;` | Well-defined: read then write (both must happen) |
| P277 | `const int x = 5; int *p = (int*)&x; *p = 10;` | UB: modifying const object |
| P278 | String concatenation by preprocessor: `"hel" "lo"` | `"hello"` |
| P279 | Multi-line string? | `"hello " "world"` or `"hello \` (backslash) |
| P280 | `#define STR(x) #x` → `STR(hello)`? | `"hello"` |
| P281 | `#define CONCAT(a,b) a##b` → `CONCAT(x,1)`? | `x1` |
| P282 | Predefined macro `__DATE__`? | Compilation date string |
| P283 | `__TIME__`? | Compilation time string |
| P284 | `__STDC__` = 1? | Conforming implementation |
| P285 | `__STDC_VERSION__` for C11? | 201112L |
| P286 | `_Noreturn` specifier (C11)? | Function never returns (e.g., `exit`) |
| P287 | `_Static_assert` (C11)? | Compile-time assertion |
| P288 | `_Alignof(int)` (C11)? | Alignment requirement (usually 4) |
| P289 | `_Alignas(16) int x;`? | Align x to 16-byte boundary |
| P290 | Sequence point at `;`? | YES |
| P291 | Sequence point at `&&`? | YES |
| P292 | Sequence point at `\|\|`? | YES |
| P293 | Sequence point at `,` (comma)? | YES (but NOT in function arg lists!) |
| P294 | Sequence point at `?:`? | YES (between condition and chosen branch) |
| P295 | `i = i++ + 1;` is UB because? | Modifying and reading `i` without intervening sequence point |
| P296 | `a[i] = i++;` is UB because? | Same reason |
| P297 | `f(i++, i++)` is UB because? | Order of argument evaluation unspecified |
| P298 | `a = f() + g();` — which called first? | Unspecified (but NOT UB!) |
| P299 | Tail call optimization: `return f(x);`? | Compiler can reuse stack frame |
| P300 | `alloca()` allocates on? | Stack (freed automatically, non-standard) |
| P301 | Stack grows downward (typical x86)? | YES |
| P302 | Heap grows upward (typical)? | YES |
| P303 | BSS segment stores? | Uninitialized/zero-initialized global & static vars |
| P304 | Data segment stores? | Initialized global & static vars |
| P305 | Code/Text segment is? | Read-only (executable instructions) |
| P306 | `rodata` section stores? | String literals, `const` globals |
| P307 | Stack frame contains? | Return address, local vars, saved registers, params |
| P308 | Buffer overflow: write past array? | Overwrites stack frame → security vulnerability |
| P309 | Stack smashing detected = ? | Canary value overwritten (stack protector) |
| P310 | ASLR protects against? | Return-to-libc attacks (randomizes memory layout) |
| P311 | Format string vulnerability: `printf(user_input)`? | Attacker can read/write memory via %n, %x! |
| P312 | Fix: `printf("%s", user_input)`? | YES — format string is now fixed |
| P313 | `gets()` is dangerous because? | No bounds checking → buffer overflow. Use `fgets()` |
| P314 | `scanf("%s", buf)` danger? | Same: no length limit. Use `scanf("%19s", buf)` |
| P315 | Integer overflow in security? | Can bypass size checks |
| P316 | Use-after-free exploit? | Attacker controls freed memory, then code executes |
| P317 | Null pointer dereference: crash or exploit? | Can be either (on some systems, NULL is mappable) |
| P318 | Defensive: always check `malloc` return? | YES: may return NULL on failure |
| P319 | `calloc(SIZE_MAX, SIZE_MAX)` → ? | Likely overflow in size computation → NULL or UB |
| P320 | Best practices: use `snprintf`, `strncpy`, check bounds | YES — always! |

---

# 📝 Practice Set 5: Memory Layout & Pointer Mastery (P321 – P400)

**P321.** What's stored in each memory segment?
```
┌───────────────┐ High Address
│    Stack      │ ← Local vars, function params, return addresses
├───────────────┤
│    Heap       │ ← malloc, calloc, realloc
├───────────────┤
│    BSS        │ ← Uninitialized global/static (zeroed)
├───────────────┤
│    Data       │ ← Initialized global/static
├───────────────┤
│    Text/Code  │ ← Program instructions (read-only)
└───────────────┘ Low Address
```

**P322.** Where is each variable?
```c
int g = 10;         // Data segment
int h;              // BSS
const char *s = "hi"; // s in Data, "hi" in rodata
void f() {
    int x = 5;      // Stack
    static int y;   // BSS
    int *p = malloc(10); // p on Stack, *p on Heap
}
```

**P323.** What is the output?
```c
int *f() {
    static int arr[] = {1,2,3};
    return arr;
}
int main() {
    int *p = f();
    printf("%d", p[1]);
}
```
→ Static array persists. **Output: 2**

**P324-P400** — Pointer Arithmetic Express:

| # | Expression (given `int a[] = {10,20,30,40,50}; int *p = a;`) | Value |
|---|---|---|
| P324 | `*p` | 10 |
| P325 | `*(p+1)` | 20 |
| P326 | `*(p+4)` | 50 |
| P327 | `*p + 1` | 11 |
| P328 | `p[3]` | 40 |
| P329 | `3[p]` | 40 |
| P330 | `&a[2] - &a[0]` | 2 (pointer subtraction = element count) |
| P331 | `(char*)&a[1] - (char*)&a[0]` | 4 (byte difference) |
| P332 | `sizeof(a)` | 20 (5×4) |
| P333 | `sizeof(p)` | 8 (64-bit pointer) |
| P334 | `sizeof(*p)` | 4 (int) |
| P335 | `sizeof(a)/sizeof(*a)` | 5 (array length idiom!) |
| P336 | After `p += 2`: `*p`? | 30 |
| P337 | `int (*q)[5] = &a; sizeof(*q)` | 20 (pointer to whole array) |
| P338 | `*(p + 5)` | UB (past end of array) |
| P339 | `p + 5` (without deref) | Legal! (one past end is allowed) |
| P340 | `p + 6` | UB (more than one past end) |

**P341-P360** — String Pointer Questions:

| # | Code | Output |
|---|---|---|
| P341 | `char *p = "GATE"; printf("%c", p[0]);` | G |
| P342 | `char *p = "GATE"; printf("%s", p+1);` | ATE |
| P343 | `char *p = "GATE"; printf("%c", *p++);` | G (then p points to A) |
| P344 | `char *p = "GATE"; printf("%c", *(p+3));` | E |
| P345 | `char *p = "GATE"; printf("%d", strlen(p));` | 4 |
| P346 | `char *p = "GATE"; printf("%lu", sizeof(p));` | 8 (pointer size) |
| P347 | `char s[] = "GATE"; printf("%lu", sizeof(s));` | 5 |
| P348 | `char *a[] = {"hi","ok","no"}; printf("%c", *a[1]);` | o |
| P349 | `char *a[] = {"hi","ok","no"}; printf("%s", a[2]);` | no |
| P350 | `char *a[] = {"hi","ok","no"}; printf("%c", *(a[0]+1));` | i |

**P351-P400** — Mixed Advanced:

| # | Q | A |
|---|---|---|
| P351 | Difference: `int *p[10]` vs `int (*p)[10]` | Array of 10 ptrs vs ptr to array of 10 |
| P352 | `void (*fp)(int)` is? | Pointer to function taking int, returning void |
| P353 | `int (*(*fp)(int))[10]` is? | Function ptr: takes int, returns ptr to array of 10 ints |
| P354 | Rule for reading complex declarations? | Clockwise/spiral rule |
| P355 | `typedef int (*comp)(const void*, const void*);` | Type alias for comparator function pointer |
| P356 | `void *p; p++;` is standard? | NO! Void pointer arithmetic is GCC extension |
| P357 | Can cast any pointer to `void*` and back? | YES (guaranteed by standard) |
| P358 | Can cast `int*` to `float*` and dereference? | Violates strict aliasing → UB! |
| P359 | Strict aliasing exception: `char*`? | Can alias any type (used for byte-level access) |
| P360 | `union { int i; float f; } u; u.i = 1; use u.f`? | Type punning via union: defined in C (not C++) |
| P361 | Alignment: why does struct have padding? | CPU accesses aligned data faster |
| P362 | `#pragma pack(1)` removes? | All padding (may slow access) |
| P363 | `offsetof(struct S, member)` gives? | Byte offset of member from struct start |
| P364 | Container_of macro pattern? | Given member ptr, get struct ptr |
| P365 | Sentinel value in array? | Special value marking end (e.g., -1, NULL) |
| P366 | `int x; scanf("%d", x);` bug? | Missing `&`! Should be `&x` |
| P367 | `scanf("%d", &x);` reads? | Integer from stdin |
| P368 | `scanf` return value = 0 means? | No items matched |
| P369 | `scanf` and buffer overflow? | Use field width: `%19s` |
| P370 | Goto: when acceptable? | Error cleanup in C (multi-level break) |
| P371 | Signal handling: `signal(SIGINT, handler)`? | Register signal handler |
| P372 | `raise(SIGTERM)` does? | Sends signal to self |
| P373 | `abort()` sends? | SIGABRT → core dump |
| P374 | `exit(0)` vs `_exit(0)`? | `exit` flushes buffers; `_exit` doesn't |
| P375 | `atexit(cleanup)` → cleanup called when? | At normal exit |
| P376 | Thread-local storage: `_Thread_local int x;`? | Each thread has own copy (C11) |
| P377 | `errno` is? | Integer with last error code |
| P378 | `perror("msg")` does? | Prints "msg: error description" |
| P379 | `strerror(errno)` returns? | Error description string |
| P380 | Macro vs function: macro is? | Text substitution (no type checking, no overhead) |
| P381 | Inline function is? | Like macro but with type safety |
| P382 | Macro can repeat side effects? | YES (`MAX(x++, y++)` evaluates twice!) |
| P383 | Multi-statement macro: use? | `do { ... } while(0)` pattern |
| P384 | Variadic macro: `#define LOG(fmt, ...) printf(fmt, __VA_ARGS__)`? | C99 variadic macros |
| P385 | `#error "msg"` does? | Stops compilation with error message |
| P386 | `#warning "msg"` does? | Emits warning (GCC extension) |
| P387 | `#line 100 "myfile.c"` does? | Changes reported line/file for diagnostics |
| P388 | `#if defined(X)` same as? | `#ifdef X` |
| P389 | Include guard pattern? | `#ifndef H_FILE / #define H_FILE / ... / #endif` |
| P390 | Circular include: how to fix? | Forward declarations + include guards |
| P391 | `sizeof` evaluated at compile time? | YES (for fixed types); for VLA: runtime |
| P392 | `int x[n]` (VLA): `sizeof(x)` at runtime? | YES! |
| P393 | Compound literal lifetime? | Until end of enclosing block |
| P394 | `int *p = &(int){5};` valid? | YES (compound literal, C99) |
| P395 | `int a = {5};` valid? | YES (scalar can use braces) |
| P396 | `int a = {5, 6};` valid? | Warning: excess initializers ignored |
| P397 | Default argument values in C? | NOT supported (C doesn't have them) |
| P398 | Function overloading in C? | NOT supported |
| P399 | `_Generic` is C's closest to? | Overloading (compile-time type dispatch) |
| P400 | Name mangling in C? | None (unlike C++) |

---

# 📝 Practice Set 6: True/False Conceptual (P401 – P480)

| # | Statement | T/F | Why |
|---|---|---|---|
| P401 | Array name is a constant pointer | T (approximately: can't reassign) | `a = p` is illegal for arrays |
| P402 | `int a[5]; a++;` is valid | F | Array name is not modifiable lvalue |
| P403 | `sizeof` is an operator, not function | T | Doesn't evaluate its argument |
| P404 | `sizeof(void)` is 0 | F | GCC says 1; standard says illegal |
| P405 | `printf` always flushes at newline | T for line-buffered (stdout to terminal) | Not guaranteed for buffered |
| P406 | `char c = 256;` causes error | F | Wraps around (impl-defined for signed) |
| P407 | Signed overflow is UB | T | Standard says so! |
| P408 | Unsigned overflow wraps around | T | Defined by standard (modular) |
| P409 | `NULL == 0` in C | T (comparison is true) | NULL is typically defined as `((void*)0)` |
| P410 | `NULL` is guaranteed to be 0 in memory | T | All-bits-zero on common platforms |
| P411 | Pointer to function can be NULL | T | Used as sentinel |
| P412 | `free(p)` sets p to NULL | F | p still holds old address (dangling!) |
| P413 | `const int x = 5;` is a compile-time constant | F in C! | Unlike C++; can't use for array size in C89 |
| P414 | `enum` values are compile-time constants | T | Can use in case labels |
| P415 | `switch` works with strings | F | Only integers/chars |
| P416 | `break` in nested loops exits all loops | F | Only innermost |
| P417 | `continue` skips rest of loop body | T | Jumps to next iteration |
| P418 | `default` must be last in switch | F | Can be anywhere |
| P419 | Fall-through in switch is a bug | F | Sometimes intentional (Duff's device) |
| P420 | C has boolean type | T (C99: `_Bool` or `bool` from stdbool.h) | Stores only 0 or 1 |
| P421 | `int a[0];` is valid C | F (standard) | GCC allows as extension |
| P422 | VLA can be static | F | VLAs are always auto |
| P423 | `malloc` returns aligned memory | T | Suitable for any type |
| P424 | `realloc` may move data | T | Returns new pointer if moved |
| P425 | `calloc(n, size)` = `malloc(n * size)` + `memset(0)`? | Approximately (calloc also checks overflow) | |
| P426 | Memory leak causes immediate crash | F | Wasted memory; crash only when OOM |
| P427 | Recursive function always needs base case | T | Otherwise infinite recursion → stack overflow |
| P428 | Every recursive solution can be iterative | T | Using explicit stack |
| P429 | Iterative is always faster than recursive | F | With tail call optimization, same |
| P430 | C supports pass by reference | F | Only pass by value (simulate with pointers) |
| P431 | `int f(int a[])` receives array by value | F | Array decays to pointer |
| P432 | Struct passed by value in C | T | Entire struct is copied |
| P433 | Struct can contain function pointers | T | Used for OOP-like patterns |
| P434 | Union members share memory | T | Size = largest member |
| P435 | Bit field can cross byte boundary | Implementation-defined | |
| P436 | `register` guarantees register storage | F | Just a hint to compiler |
| P437 | `extern "C"` is valid in C | F | C++ feature (for C linkage) |
| P438 | All pointers have same size | T on most platforms | But not guaranteed by standard |
| P439 | Function pointer and data pointer same size | Not guaranteed | On some embedded: different |
| P440 | `void *` can hold function pointer | Not guaranteed (POSIX: yes) | |
| P441 | String literal is modifiable | F | Stored in read-only section |
| P442 | `char s[] = "hi"` creates modifiable copy | T | On stack |
| P443 | `"hello" + 1` is valid | T | Pointer arithmetic → "ello" |
| P444 | Two identical string literals share storage | Implementation-defined | Compiler may merge |
| P445 | C preprocessor runs before compilation | T | Text substitution, includes |
| P446 | `#include` literally copies file contents | T | |
| P447 | Macro can be recursive | F | Preprocessor prevents self-referential expansion |
| P448 | `#define X X+1` causes infinite recursion | F | Preprocessor stops after one expansion |
| P449 | `__VA_ARGS__` is for variadic macros | T (C99) | |
| P450 | C11 added `_Atomic`, `_Generic`, `_Static_assert` | T | Major new features |

**P451-P480** — Quick Traps:

| # | Trap | Fix |
|---|---|---|
| P451 | `if (x = 5)` (assignment in condition) | Use `if (x == 5)` |
| P452 | Missing `break` in switch | Add `break;` or comment `// fall through` |
| P453 | Off-by-one: `for (i=0; i<=n; i++)` vs `i<n` | `i < n` for n elements |
| P454 | Forgetting null terminator in strings | Leave room: `char s[6] = "hello"` not `s[5]` |
| P455 | `scanf("%d", x)` missing `&` | `scanf("%d", &x)` |
| P456 | Dangling else: `if..if..else` ambiguity | Use braces `{ }` |
| P457 | `sizeof(array)` in function parameter | Gets pointer size, not array size! |
| P458 | `strlen` on non-null-terminated array | Reads past end → UB |
| P459 | Comparing strings with `==` | Compares addresses! Use `strcmp` |
| P460 | `return arr;` for local array | Dangling pointer; arrays don't survive function exit |
| P461 | Using `malloc` without `#include <stdlib.h>` | Implicit declaration → wrong pointer size (especially 64-bit) |
| P462 | Not checking `malloc` return | May be NULL on failure |
| P463 | `free(p); free(p);` double free | Set `p = NULL` after free |
| P464 | Using `gets()` | REMOVED in C11! Use `fgets()` |
| P465 | Signed/unsigned comparison | `-1 > 0u` is TRUE! |
| P466 | Integer overflow: `INT_MAX + 1` | UB (undefined behavior) |
| P467 | Shifting by >= width: `1 << 32` for 32-bit int | UB |
| P468 | Shifting negative numbers left | UB |
| P469 | `printf` wrong format specifier | UB: `%d` for long, `%d` for unsigned, etc. |
| P470 | Uninitialized variable use | UB (except static/global which are 0) |
| P471 | Array index out of bounds | UB (no runtime check in C!) |
| P472 | Modifying const variable via cast | UB |
| P473 | Aliasing violation via wrong pointer type | UB (strict aliasing) |
| P474 | Infinite recursion | Stack overflow |
| P475 | Accessing freed memory | UB (use-after-free) |
| P476 | Thread race condition | UB (use mutex/atomic) |
| P477 | `sprintf` buffer overflow | Use `snprintf` with length |
| P478 | `strncpy` doesn't null-terminate if `n < strlen(src)` | Manually add: `dest[n-1] = '\0'` |
| P479 | `atoi("abc")` returns? | 0 (no error indication!) — use `strtol` |
| P480 | `strtol` advantage? | Sets `endptr` and `errno` for error detection |

---

# 📋 Complete Operator Precedence Table

| Precedence | Operator | Associativity |
|---|---|---|
| 1 (highest) | `()` `[]` `->` `.` `++`(postfix) `--`(postfix) | Left to Right |
| 2 | `++`(prefix) `--`(prefix) `+`(unary) `-`(unary) `!` `~` `*`(deref) `&`(addr) `sizeof` `(type)` | Right to Left |
| 3 | `*` `/` `%` | Left to Right |
| 4 | `+` `-` | Left to Right |
| 5 | `<<` `>>` | Left to Right |
| 6 | `<` `<=` `>` `>=` | Left to Right |
| 7 | `==` `!=` | Left to Right |
| 8 | `&` (bitwise AND) | Left to Right |
| 9 | `^` (bitwise XOR) | Left to Right |
| 10 | `\|` (bitwise OR) | Left to Right |
| 11 | `&&` (logical AND) | Left to Right |
| 12 | `\|\|` (logical OR) | Left to Right |
| 13 | `?:` (ternary) | Right to Left |
| 14 | `=` `+=` `-=` `*=` `/=` `%=` `<<=` `>>=` `&=` `^=` `\|=` | Right to Left |
| 15 (lowest) | `,` (comma) | Left to Right |

🎯 **Mnemonic:** "**P**lease **U**se **M**y **A**wesome **S**hifty **R**ules **E**very **A**fternoon **X**or **O**r **A**nd **O**r **T**ernary **A**ssign **C**omma"
→ Postfix, Unary, Mult/Div/Mod, Add/Sub, Shift, Relational, Equality, Amp(&), Xor(^), Or(|), And(&&), Or(||), Ternary, Assignment, Comma

---

# 🎯 C Programming GATE Score Maximization

## Top 10 Question Patterns:

| Rank | Pattern | Frequency | Example |
|---|---|---|---|
| 1 | Output prediction (pointers) | Very High | `int *p = a; printf("%d", *(p+3));` |
| 2 | Output prediction (operators/precedence) | Very High | `a+++b`, `sizeof(i++)` |
| 3 | Output prediction (recursion) | High | Trace call stack |
| 4 | Type promotion / signed-unsigned | High | `-1 < 1u` |
| 5 | Storage class behavior | Medium | `static` variable persistence |
| 6 | Memory layout questions | Medium | Where is `malloc` stored? |
| 7 | Structure padding/sizeof | Medium | `sizeof(struct)` with alignment |
| 8 | Macro expansion issues | Medium | `SQ(x+1)` without parens |
| 9 | String vs array distinctions | Medium | `sizeof` vs `strlen` |
| 10 | Function pointer / callback | Low-Med | `qsort` comparator |

## Exam Strategy:

1. **Read code character by character** — watch for `=` vs `==`, `;` after for/while
2. **Apply precedence table** — don't guess!
3. **Trace on paper** — write variable values after each line
4. **Check for UB** — any UB means "cannot determine" is the answer
5. **Watch for signed/unsigned mixing** — this is the #1 trap
6. **Time: 2 min for 1-mark, 4 min for 2-mark** — skip and return if stuck

---

---

# 📝 Practice Set 7: Structure Padding Deep Dive (P481 – P540)

**P481.** Rules of Structure Padding:
1. Each member aligned to its own size (or to the alignment of its type)
2. Padding added BEFORE a member to satisfy alignment
3. Total struct size is multiple of the largest member's alignment
4. Packed structs (`__attribute__((packed))`) remove all padding

**P482.** Trace the padding:
```c
struct A {
    char a;    // offset 0, size 1
    // 3 bytes padding (int needs 4-byte alignment)
    int b;     // offset 4, size 4
    char c;    // offset 8, size 1
    // 3 bytes padding (total must be multiple of 4)
};
// sizeof(struct A) = 12
```

**P483.** Same members, different order:
```c
struct B {
    char a;    // offset 0, size 1
    char c;    // offset 1, size 1 (char needs 1-byte alignment)
    // 2 bytes padding
    int b;     // offset 4, size 4
};
// sizeof(struct B) = 8   ← 4 bytes saved by reordering!
```

🎯 **GATE Trick:** Reorder members by decreasing size to minimize padding!

**P484.** With double:
```c
struct C {
    char a;    // offset 0, size 1
    // 7 bytes padding (double needs 8-byte alignment)
    double b;  // offset 8, size 8
    char c;    // offset 16, size 1
    // 7 bytes padding (total must be multiple of 8)
};
// sizeof(struct C) = 24
```

**P485.** Optimized:
```c
struct D {
    double b;  // offset 0, size 8
    char a;    // offset 8, size 1
    char c;    // offset 9, size 1
    // 6 bytes padding
};
// sizeof(struct D) = 16   ← 8 bytes saved!
```

**P486.** Nested struct:
```c
struct Inner { int x; char y; };  // size = 8, align = 4
struct Outer {
    char a;          // offset 0, size 1
    // 3 bytes padding
    struct Inner b;  // offset 4, size 8
    char c;          // offset 12, size 1
    // 3 bytes padding
};
// sizeof(struct Outer) = 16
```

**P487-P510** — sizeof Quiz:

| # | Code | sizeof | Why |
|---|---|---|---|
| P487 | `struct{char a; char b;}` | 2 | No padding needed |
| P488 | `struct{short a; char b;}` | 4 | pad 1 + round to 2 |
| P489 | `struct{int a; int b;}` | 8 | No padding |
| P490 | `struct{char a; short b; int c;}` | 8 | 1+1pad+2+4 |
| P491 | `struct{int a; char b; short c;}` | 8 | 4+1+1pad+2 |
| P492 | `struct{char a; int b; char c; short d;}` | 12 | 1+3pad+4+1+1pad+2 |
| P493 | `struct{double a; char b;}` | 16 | 8+1+7pad |
| P494 | `struct{char a; double b;}` | 16 | 1+7pad+8 |
| P495 | `struct{char a[3]; int b;}` | 8 | 3+1pad+4 |
| P496 | `struct{int a; char b[3];}` | 8 | 4+3+1pad |
| P497 | `union{int a; char b; double c;}` | 8 | max(4,1,8) |
| P498 | `union{char a[20]; int b;}` | 20 | max(20,4), align to 4→20 |
| P499 | `struct{int a:3; int b:5;}` | 4 | bits fit in one int |
| P500 | `struct{int a:30; int b:5;}` | 8 | 30+5=35 > 32, needs 2 ints |
| P501 | Empty struct in C? | UB/undefined | Not standard (GCC: 0 bytes!) |
| P502 | `struct{char a; int b;} __attribute__((packed))` | 5 | No padding! |
| P503 | Array of struct: `sizeof(struct{char a; int b;}) * 3` | 24 | 8 × 3 |
| P504 | Packed array: `sizeof(__attribute__((packed)) struct{char a; int b;}) * 3` | 15 | 5 × 3 |
| P505 | `struct{int a; char b; int c; char d;}` | 16 | 4+1+3pad+4+1+3pad |
| P506 | `struct{char a; char b; char c; char d; int e;}` | 8 | 4 chars + 4 int |
| P507 | `struct{long long a; char b;}` | 16 | 8+1+7pad |
| P508 | `struct{char *a; int b;}` (64-bit) | 16 | 8ptr+4int+4pad |
| P509 | `struct{int b; char *a;}` (64-bit) | 16 | 4+4pad+8ptr |
| P510 | `struct{char a; void *b; char c;}` (64-bit) | 24 | 1+7pad+8+1+7pad |

---

# 📝 Practice Set 8: Preprocessor Mastery (P511 – P580)

**P511.** What is the output?
```c
#define A 2
#define B 3
#define C A*B
printf("%d", C+1);
```
→ `C+1` = `2*3+1` = 7 (not 8!). **Output: 7**

**P512.** Fix:
```c
#define C (A*B)
printf("%d", C+1);  // (2*3)+1 = 7... same here
// But: printf("%d", 10/C); → 10/(2*3) = 1, vs 10/2*3 = 15 without parens!
```

**P513.** Stringification:
```c
#define STR(x) #x
printf("%s", STR(hello world));  // "hello world"
```
**Output: hello world**

**P514.** Token pasting:
```c
#define VAR(n) var_##n
int VAR(1) = 10;  // int var_1 = 10;
int VAR(2) = 20;  // int var_2 = 20;
printf("%d %d", var_1, var_2);
```
**Output: 10 20**

**P515.** Multi-statement macro:
```c
#define SWAP(a,b) do { int t = a; a = b; b = t; } while(0)
int x=5, y=10;
if (1) SWAP(x,y); else printf("no");
// Works correctly because of do-while wrapping!
```

⚠️ Without `do-while(0)`:
```c
#define SWAP(a,b) { int t = a; a = b; b = t; }
if (1) SWAP(x,y); else printf("no");
// Expands to: if(1) {...}; else ... → syntax error! (else left hanging)
```

**P516.** Conditional compilation:
```c
#ifdef DEBUG
    printf("Debug mode\n");
#endif
```
Only compiles if `DEBUG` is defined (via `#define DEBUG` or `-DDEBUG` flag).

**P517.** Macro vs function comparison:

| Feature | Macro | Function |
|---|---|---|
| Type checking | ❌ None | ✅ Yes |
| Side effects | ⚠️ Args evaluated multiple times | ✅ Once |
| Code size | Increases (inline expansion) | Single copy |
| Speed | Faster (no call overhead) | Minor overhead |
| Debugging | Harder (no line info) | Easier |
| Scope | Global after definition | Local scope possible |

**P518-P540** — Preprocessor output quiz:

| # | Code | After preprocessing |
|---|---|---|
| P518 | `#define X 5` → `int a = X;` | `int a = 5;` |
| P519 | `#define F(x) (x+1)` → `F(3)` | `(3+1)` |
| P520 | `#define F(x) (x+1)` → `F(2+3)` | `(2+3+1)` |
| P521 | `#define SQ(x) x*x` → `SQ(2+1)` | `2+1*2+1` = 5 |
| P522 | `#define SQ(x) ((x)*(x))` → `SQ(2+1)` | `((2+1)*(2+1))` = 9 |
| P523 | `#define A B` then `#define B 5` → `A`? | 5 (macros expanded lazily) |
| P524 | Recursive: `#define X X+1` → `X`? | `X+1` (not recursive: self-ref stops) |
| P525 | `#define N 2+3` → `N*N`? | `2+3*2+3` = 11 |
| P526 | `#define N (2+3)` → `N*N`? | `(2+3)*(2+3)` = 25 |
| P527 | `#if 0 ... #endif` effect? | Code block removed (comment-out trick) |
| P528 | `#define MAX(a,b) ((a)>(b)?(a):(b))` → `MAX(3,5)`? | `((3)>(5)?(3):(5))` = 5 |
| P529 | `MAX(x++,y++)` with x=3,y=5? | y++ executed twice → returns 6! |
| P530 | `#undef X` effect? | Undefines X macro |
| P531 | `#define X` (no value)? | Defined as empty string |
| P532 | `#ifdef X` after `#define X`? | TRUE (even if empty) |
| P533 | `defined(X)` vs `#ifdef X`? | Same, but `defined` works inside `#if` expressions |
| P534 | `#define F(x,y) x##y` → `F(1,2)`? | `12` |
| P535 | `#define PRINT(x) printf(#x " = %d\n", x)` → `PRINT(5+3)`? | `printf("5+3" " = %d\n", 5+3)` → "5+3 = 8" |
| P536 | `__LINE__` inside macro expansion? | Line where macro is CALLED |
| P537 | Include order matters for macros? | YES! Macros defined after use aren't expanded |
| P538 | Can macro span multiple lines? | Yes with `\` continuation |
| P539 | `#include <file>` vs `"file"`? | `<>`: system paths; `""`: local first, then system |
| P540 | `#pragma once` is standard? | NO (compiler extension, but widely supported) |

---

# 📝 Practice Set 9: Undefined Behavior Catalog (P541 – P620)

🚨 **UB = Undefined Behavior** — Compiler can do ANYTHING (including working as expected — but not reliably!)

**Category 1: Arithmetic UB**

| # | Code | Type | Notes |
|---|---|---|---|
| P541 | `INT_MAX + 1` | UB | Signed overflow |
| P542 | `1 << 31` (32-bit int) | UB (C99) | Shift into sign bit |
| P543 | `1 << 32` (32-bit int) | UB | Shift ≥ width |
| P544 | `-1 << 1` | UB (C99) | Left shift of negative |
| P545 | `1 >> -1` | UB | Negative shift count |
| P546 | `x / 0` | UB | Division by zero |
| P547 | `INT_MIN / -1` | UB (may overflow) | Result doesn't fit |
| P548 | `INT_MIN % -1` | UB | Same reason |

**Category 2: Pointer UB**

| # | Code | Type | Notes |
|---|---|---|---|
| P549 | `NULL` dereference | UB | Segfault on most systems |
| P550 | Out-of-bounds array | UB | No bounds checking |
| P551 | `p + n` where `n > 1` past end | UB | Only `p + 0` to `p + array_size` allowed |
| P552 | Dereference one-past-end | UB | Only address is valid |
| P553 | `free(p); *p = 5;` | UB | Use-after-free |
| P554 | `free(p); free(p);` | UB | Double free |
| P555 | `int *p; *p = 5;` (uninitialized) | UB | Indeterminate pointer |
| P556 | `int *p = (int*)1; *p = 5;` | UB | Invalid address |
| P557 | `(int*)float_ptr` dereference | UB | Strict aliasing violation |

**Category 3: Sequence Point UB**

| # | Code | Type | Notes |
|---|---|---|---|
| P558 | `i = i++` | UB | Multiple modification without SP |
| P559 | `i = ++i + 1` | UB (C99) / Defined (C11) | C11 clarified |
| P560 | `a[i] = i++` | UB | Read and modify without SP |
| P561 | `f(i++, i++)` | UB | Unspecified order + double modification |
| P562 | `a[i++] = a[i]` | UB | Which `i`? |

**Category 4: Other UB**

| # | Code | Type | Notes |
|---|---|---|---|
| P563 | Modifying string literal | UB | `"hello"[0] = 'H'` |
| P564 | Modifying `const` via pointer cast | UB | `*(int*)&const_var = 5` |
| P565 | Missing `return` in non-void function | UB | Caller uses garbage |
| P566 | `printf("%d", 3.14)` | UB | Wrong format specifier |
| P567 | `printf(user_string)` | UB (security) | Format string attack |
| P568 | Reading uninitialized variable | UB | Indeterminate value |
| P569 | Infinite recursion | UB | Stack overflow |
| P570 | `main` called from signal handler | UB | Only async-signal-safe functions allowed |
| P571 | VLA with negative size | UB | `int n=-1; int a[n];` |
| P572 | Misaligned pointer access | UB | `int *p = (int*)((char*)q+1);` |

**P573-P620** — "Is this UB?" Quick Drill:

| # | Code | UB? | Why |
|---|---|---|---|
| P573 | `int x = 5; x = x + 1;` | No | Single modification |
| P574 | `int x = 5; x = x++;` | YES | Double mod without SP |
| P575 | `int x = 5; x = ++x;` | C99:YES, C11:No | C11 sequenced |
| P576 | `int a=1,b=2; a = (a=3) + b;` | YES (C99) | Modifying `a` twice |
| P577 | `int a=1; if(a++ && a++)` | Not UB | `&&` is sequence point |
| P578 | `int a=0; a++ || a++` | Not UB | `\|\|` is sequence point |
| P579 | `printf("%d%d", i++, i)` | YES | Unsequenced args |
| P580 | `a = b + c;` (all init) | No | Just addition |
| P581 | `int x; if(x==0)` (uninit) | YES | Reading uninit |
| P582 | `static int x; if(x==0)` | No | Static initialized to 0 |
| P583 | `char c = 256;` | Impl-defined | Not UB (defined as wrapping) |
| P584 | `UINT_MAX + 1` | No | Unsigned wraps (defined) |
| P585 | `int *p = malloc(4); free(p); p = NULL;` | No | Safe (p set to NULL) |
| P586 | `void *p = malloc(4); int *q = p;` | No | void* to int* is fine |
| P587 | `memcpy(dest, src, n)` overlapping | YES | Use `memmove` for overlap |
| P588 | `memmove(dest, src, n)` overlapping | No | Handles overlap |
| P589 | `a[i] = b[j];` (all defined, in bounds) | No | Simple copy |
| P590 | `*(int*)((char*)p + 1)` (misaligned) | YES | Alignment violation |
| P591 | `union { int i; float f; } u; u.i=1; printf("%f",u.f);` | No (in C) | Union type punning OK in C |
| P592 | `for(;;);` (infinite loop, no side effects) | UB (C11)! | C11: UB unless observable side effects |
| P593 | `for(;;) { volatile int x = 1; }` | Not UB | Volatile is observable |
| P594 | `int arr[5] = {1}; arr[5] = 10;` | YES | Out of bounds |
| P595 | `int arr[5]; int *p = &arr[5];` | No | One-past-end address is valid |
| P596 | `int arr[5]; int *p = &arr[5]; *p = 10;` | YES | Dereference one-past-end |
| P597 | `signed char c = 127; c++;` | Impl-defined | `char` overflow behavior varies |
| P598 | `x = f() + g();` (both have side effects) | Not UB | Unspecified order but not UB |
| P599 | `printf("%d %d", f(), g());` (side effects) | Not UB | Unspecified order of eval |
| P600 | Order of evaluation of `f() + g() * h()`? | Unspecified | Any order (but * before +) |
| P601-P620 | [See rapid-fire section below] | | |

**P601-P620** — What does this implementation-defined behavior do typically?

| # | Behavior | Typical Result |
|---|---|---|
| P601 | Right shift of negative signed int | Arithmetic shift (sign-extends) |
| P602 | Sign of `char` (signed or unsigned?) | Signed on x86 |
| P603 | Byte order | Little-endian on x86 |
| P604 | `sizeof(int)` | 4 bytes |
| P605 | `sizeof(long)` on 64-bit Linux | 8 bytes |
| P606 | `sizeof(long)` on 64-bit Windows | 4 bytes! (LLP64) |
| P607 | `sizeof(pointer)` on 64-bit | 8 bytes |
| P608 | Padding in structs | Aligns to member size |
| P609 | `malloc(0)` returns | Non-NULL on most (but can be NULL) |
| P610 | Representation of NULL | All-bits-zero |
| P611 | `char` range | -128 to 127 or 0 to 255 |
| P612 | Bit field storage unit | Implementation choice |
| P613 | Bit field order: MSB or LSB first? | Compiler-specific |
| P614 | `#pragma` behavior | Compiler-specific |
| P615 | Function argument eval order | Unspecified (not UB) |
| P616 | Interleaving of `f()+g()` side effects? | Unsequenced but not interleaved in C |
| P617 | Stack direction | Downward on x86/x64 |
| P618 | `main()` with 0 or 2 or 3 args? | Impl-defined (3 args: envp) |
| P619 | Effect of unrecognized `#pragma`? | Ignored (standard says so) |
| P620 | Value of uninitialized `auto` variable | Indeterminate (any value) |

---

# 📝 Practice Set 10: Multi-step GATE Problems (P621 – P700)

**P621.** (GATE 2-mark style) What is the output?
```c
int f(int *p, int *q) {
    *p = *p - *q;
    *q = *p + *q;
    *p = *q - *p;
    return *p + *q;
}
int main() {
    int a = 3, b = 7;
    int r = f(&a, &b);
    printf("%d %d %d", a, b, r);
}
```
→ Step-by-step: `*p = 3-7 = -4`. `*q = -4+7 = 3`. `*p = 3-(-4) = 7`.
→ a = 7, b = 3, r = 7+3 = 10. Swap algorithm! **Output: 7 3 10**

**P622.** What is printed?
```c
void mystery(int n) {
    if (n/2) mystery(n/2);
    printf("%d", n%2);
}
mystery(10);
```
→ Trace: m(10)→m(5)→m(2)→m(1)→m(0: n/2=0, stop). Print 0%2=0... Wait.
→ m(10): n/2=5≠0 → m(5). m(5): n/2=2≠0 → m(2). m(2): n/2=1≠0 → m(1). m(1): n/2=0 → stop.
→ Unwind: print 1%2=1, print 2%2=0, print 5%2=1, print 10%2=0.
→ **Output: 1010** (binary of 10!)

**P623.** What is the output?
```c
int count = 0;
void f(int n) {
    if (n == 0) return;
    count++;
    f(n & (n-1));
}
f(255);
printf("%d", count);
```
→ `n & (n-1)` clears lowest set bit. 255 = 11111111 → 8 set bits. **Output: 8**

**P624.** What is `f(100)`?
```c
int f(int n) {
    if (n <= 1) return 0;
    return 1 + f(n/2);
}
```
→ `f(100) = 1+f(50) = 2+f(25) = 3+f(12) = 4+f(6) = 5+f(3) = 6+f(1) = 6`.
→ Computes $\lfloor \log_2 n \rfloor = 6$. ($2^6 = 64 \leq 100 < 128 = 2^7$) **Output: 6**

**P625.** What is the output?
```c
int main() {
    int a[][3] = {{1,2,3},{4,5,6},{7,8,9}};
    int (*p)[3] = a + 1;
    printf("%d", (*p)[2]);
}
```
→ `a+1` = pointer to row 1. `*p` = row 1. `(*p)[2] = a[1][2] = 6`. **Output: 6**

**P626.** What is the output?
```c
int main() {
    char *s[] = {"GATE", "2027", "CS"};
    char **p = s;
    printf("%s ", *p++);
    printf("%s ", *p++);
    printf("%s", *p);
}
```
→ `*p++`: dereference p (s[0] = "GATE"), then p→s[1]. Then "2027", p→s[2]. Then "CS".
**Output: GATE 2027 CS**

**P627.** How many times is "hello" printed?
```c
int main() {
    fork();
    fork();
    printf("hello\n");
}
```
→ First fork: 2 processes. Second fork: each creates one → 4 processes. **Output: hello printed 4 times**

⚠️ Note: `fork()` is POSIX, not standard C. But appears in GATE!

**P628.** What is the output?
```c
void f(int n, int *sum) {
    if (n == 0) return;
    *sum += n % 10;
    f(n/10, sum);
}
int main() {
    int s = 0;
    f(12345, &s);
    printf("%d", s);
}
```
→ Sum of digits: 1+2+3+4+5 = **15**. **Output: 15**

**P629.** What is the output?
```c
int f(int n) {
    if (n < 10) return n;
    return f(n/10) + n%10;
}
printf("%d", f(9999));
```
→ Sum of digits recursively: 9+9+9+9 = **36**. But wait: f(9999) = f(999)+9 = f(99)+9+9 = f(9)+9+9+9 = 9+27 = 36. **Output: 36**

**P630.** What is the output?
```c
char *f(char *s) {
    if (*s == '\0') return s;
    f(s+1);
    printf("%c", *s);
    return s;
}
f("GATE");
```
→ Recurse to end, then print on unwind: E, T, A, G → **Output: ETAG** (reverse!)

**P631.** What is the output?
```c
int arr[] = {1, 2, 3, 4, 5};
int *p = arr;
int sum = 0;
while (p < arr + 5)
    sum += *p++;
printf("%d", sum);
```
→ 1+2+3+4+5 = **15**. **Output: 15**

**P632.** What is the output?
```c
int x = 5;
int y = x-- - --x;
printf("%d %d %d", x, y, x);
```
→ ⚠️ **Undefined Behavior!** `x` modified twice without sequence point.

**P633.** What does this function do?
```c
int f(unsigned int n) {
    int count = 0;
    while (n) {
        count++;
        n >>= 1;
    }
    return count;
}
```
→ Counts number of bits needed to represent n = $\lfloor\log_2 n\rfloor + 1$.

**P634.** What is the output?
```c
int a = 5;
printf("%d", a >> 1);
printf(" %d", a << 1);
printf(" %d", a & ~1);
```
→ `5>>1 = 2`, `5<<1 = 10`, `5 & ~1 = 5 & 0xFFFFFFFE = 4`.
**Output: 2 10 4**

**P635.** What is the output?
```c
int a = 5;
int b = a | (1 << 3);
printf("%d", b);
```
→ `5 = 0101`. Set bit 3: `0101 | 1000 = 1101 = 13`. **Output: 13**

**P636.** Is bit k set?
```c
int n = 42;  // 101010
int k = 3;
printf("%d", (n >> k) & 1);
```
→ `42 >> 3 = 5 = 101`. `5 & 1 = 1`. Bit 3 IS set. **Output: 1**

**P637.** Function pointer array:
```c
int add(int a, int b) { return a+b; }
int sub(int a, int b) { return a-b; }
int mul(int a, int b) { return a*b; }
int main() {
    int (*ops[])(int,int) = {add, sub, mul};
    printf("%d", ops[2](3,4));
}
```
→ `ops[2]` = `mul`. `mul(3,4) = 12`. **Output: 12**

**P638.** Callback pattern (qsort):
```c
int cmp(const void *a, const void *b) {
    return *(int*)a - *(int*)b;
}
int arr[] = {5,3,1,4,2};
qsort(arr, 5, sizeof(int), cmp);
// arr = {1,2,3,4,5}
```

**P639.** What does this print?
```c
char s[] = "abcde";
char *p = s;
*p++ = *++p;
printf("%s", s);
```
→ ⚠️ **Undefined Behavior!** Multiple modifications and reads of `p` without sequence point.

**P640.** What is the output?
```c
struct { int x; } a = {10}, b = {20};
a = b;
b.x = 30;
printf("%d %d", a.x, b.x);
```
→ Struct assignment copies. `a.x = 20`, `b.x = 30`. **Output: 20 30**

**P641-P700** — Rapid Fire Advanced:

| # | Q | A |
|---|---|---|
| P641 | `int (*a)[5]` -> how many bytes does `a+1` skip? | 20 (5 ints) |
| P642 | `int a[3][4]; a+1` points to? | Row 1 (skips 4 ints = 16 bytes) |
| P643 | `int a[3][4]; *a+1` points to? | `a[0][1]` (skips 1 int) |
| P644 | `int a[3][4]; *(a+1)+2` = ? | `&a[1][2]` |
| P645 | `int a[3][4]; **(a+1)` = ? | `a[1][0]` |
| P646 | `memset(arr, -1, sizeof(arr))` for int arr? | All bytes 0xFF → each int = -1 (two's complement) ✅ |
| P647 | `memset(arr, 1, sizeof(arr))` for int arr? | Each byte 0x01 → each int = 0x01010101 = 16843009 ❌ |
| P648 | Stack frame for `f(a,b)`: pushed in what order? | Right-to-left in cdecl: b then a |
| P649 | Who cleans stack in cdecl? | Caller |
| P650 | Who cleans stack in stdcall? | Callee |
| P651 | `argv[argc]` guaranteed value? | NULL (by standard) |
| P652 | Return value of `main()` meaning: 0? | Success (EXIT_SUCCESS) |
| P653 | Return value of `main()`: non-zero? | Failure (EXIT_FAILURE = 1 typically) |
| P654 | `exit(0)` vs `return 0` in main? | Same effect + atexit handlers |
| P655 | Can local variable be returned by value? | YES (copy is returned) |
| P656 | Can local array be returned? | NO (decays to dangling pointer) |
| P657 | Returning struct by value? | YES (copied; may use NRVO) |
| P658 | `fseek(fp, 0, SEEK_END); ftell(fp)` gives? | File size in bytes |
| P659 | `fseek(fp, -10, SEEK_CUR)` moves? | 10 bytes backward from current |
| P660 | Binary file mode: `"rb"` vs `"r"`? | `"rb"` prevents `\r\n` → `\n` conversion (Windows) |
| P661 | `fwrite(&x, sizeof(x), 1, fp)` writes? | Binary representation of x |
| P662 | `fread(&x, sizeof(x), 1, fp)` reads? | Binary data into x |
| P663 | Portability issue with binary files? | Endianness, padding, type sizes differ |
| P664 | `tmpfile()` creates? | Temp file, auto-deleted on close |
| P665 | `remove("file.txt")` does? | Deletes file |
| P666 | `rename("old", "new")` does? | Renames file |
| P667 | `stderr` vs `stdout`? | `stderr` is unbuffered (immediate output) |
| P668 | `fprintf(stderr, "error\n")` | Writes to error stream |
| P669 | Redirect: `./prog 2> err.txt` | stderr to file |
| P670 | `stdin`, `stdout`, `stderr` are type? | `FILE *` |
| P671 | Enum underlying type in C? | `int` (always) |
| P672 | Can enum have negative values? | YES: `enum { A = -1, B = -2 };` |
| P673 | `typedef struct Node Node;` then? | Can use `Node` instead of `struct Node` |
| P674 | Opaque pointer: `struct Impl;` in header? | Forward declaration → hide implementation |
| P675 | Function-like macro must be called with? | Exactly right number of args |
| P676 | `#define F()` (0 args) → `F` vs `F()`? | `F` doesn't expand (not a call); `F()` expands |
| P677 | `static inline` function in header? | Each TU gets own copy; no link errors |
| P678 | `extern inline` in C99? | Provides external definition |
| P679 | Tentative definition: `int x;` at file scope? | Yes (implicitly `int x = 0;` if no other def) |
| P680 | Multiple `int x;` at file scope? | OK (tentative) — all refer to same `x` |
| P681 | `const` variable in C is NOT a constant expression | TRUE! Can't use in `case` labels (use `enum`) |
| P682 | `sizeof` in preprocessor `#if`? | NOT allowed! `sizeof` not evaluated by preprocessor |
| P683 | Can `switch` have `float` case? | NO! Only integer types |
| P684 | `switch` with `long long`? | YES (C99) |
| P685 | `case 1 ... 5:` (range in switch)? | GCC extension (not standard C) |
| P686 | Duff's device? | Interleaved switch and loop for optimization |
| P687 | Comma in macro arg: `F(a,b)` expands wrong if? | Arg contains comma: `F((a,b))` use extra parens |
| P688 | `__attribute__((constructor))` in GCC? | Runs before main() |
| P689 | `__attribute__((destructor))` in GCC? | Runs after main() |
| P690 | `__attribute__((unused))` suppresses? | Unused variable warning |
| P691 | `__attribute__((noreturn))` tells? | Function never returns (like `exit`) |
| P692 | `__builtin_expect(x, 1)` does? | Branch prediction hint (likely) |
| P693 | `restrict` in `memcpy` sig but not `memmove`? | `memcpy` assumes no overlap |
| P694 | Trick: swap without temp using +/-? | `a=a+b; b=a-b; a=a-b;` ⚠️ overflow risk! |
| P695 | Trick: check if two ints have opposite signs? | `(a ^ b) < 0` |
| P696 | Compute absolute value without branch? | `(n ^ (n>>31)) - (n>>31)` (for 32-bit) |
| P697 | Round up to next power of 2? | `n--; n\|=n>>1; n\|=n>>2;...n\|=n>>16; n++;` |
| P698 | Count trailing zeros? | `__builtin_ctz(n)` (GCC) |
| P699 | Count leading zeros? | `__builtin_clz(n)` (GCC) |
| P700 | Integer log base 2? | `31 - __builtin_clz(n)` |

---

# 📝 Practice Set 11: GATE Previous Year Reproductions (P701 – P780)

**P701.** (GATE CS 2015 style) What is the output?
```c
int main() {
    int i = 0;
    int j = 0;
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            if (i == j) continue;
            if (i == j - 1) break;
        }
        printf("%d ", j);
    }
}
```
→ Trace:
- i=0: j=0 → continue. j=1 → i==j-1? 0==0 → break. j=1. Print 1.
- i=1: j=0 → nothing. j=1 → continue. j=2 → i==j-1? 1==1 → break. j=2. Print 2.
- i=2: j=0,1 → nothing. j=2 → continue. j=3 → break. j=3. Print 3.
- i=3: j=0,1,2 → nothing. j=3 → continue. j=4 → break. j=4. Print 4.
- i=4: j=0,1,2,3 → nothing. j=4 → continue. j=5 → loop ends. j=5. Print 5.
**Output: 1 2 3 4 5**

**P702.** (GATE CS 2016 style) Consider:
```c
int f(int n) {
    int s = 0;
    while (n > 0) {
        s += n & 1;
        n >>= 1;
    }
    return s;
}
```
What does `f(n)` compute? 
→ Counts set bits (1s) in binary representation = **popcount**. `f(7) = 3`, `f(8) = 1`.

**P703.** (GATE CS 2017 style) Struct padding:
```c
struct S { char c; double d; int i; };
```
On 64-bit with 8-byte alignment: `c`(1) + pad(7) + `d`(8) + `i`(4) + pad(4) = **24**.

**P704.** (GATE CS 2018 style)
```c
int f(int a, int b) {
    while (a != b) {
        if (a > b) a -= b;
        else b -= a;
    }
    return a;
}
```
What does `f(a,b)` compute? → **GCD** (subtraction method, equivalent to Euclidean)

**P705.** (GATE DA style) What does this function return for `n = 7`?
```c
int f(int n) {
    if (n <= 0) return 0;
    if (n % 2 == 1) return f(n-1) + 1;
    return f(n-1);
}
```
→ Counts odd numbers from 1 to n. For n=7: 1,3,5,7 → **4** (= $(n+1)/2$)

**P706-P730** — PYQ Express Table:

| # | Topic | Question Pattern | Key Trap |
|---|---|---|---|
| P706 | Pointer arithmetic | `*(p+i)` vs `p[i]` | Same thing! |
| P707 | sizeof vs strlen | `sizeof("abc")` vs `strlen("abc")` | 4 vs 3 |
| P708 | Macro expansion | `SQ(x+1)` without parens | Always parenthesize |
| P709 | Static variable | Count function calls | Persists between calls |
| P710 | Recursion tracing | Binary representation | Print n%2 + recurse n/2 |
| P711 | Switch fall-through | Missing break | All cases after match execute |
| P712 | Comma operator | `(a, b, c)` | Returns last value |
| P713 | Short-circuit | `a \|\| (b=5)` | b may not be modified |
| P714 | Signed vs unsigned | `-1 < 1u` | Promotion makes -1 large |
| P715 | Post/pre increment | `*p++` vs `++*p` | Precedence matters |
| P716 | 2D array type | `a` vs `a[0]` vs `&a` | All different types |
| P717 | String literal type | `char*` (immutable) | Don't modify! |
| P718 | sizeof in function | `sizeof(param_arr)` | Gets pointer size |
| P719 | Struct assignment | `s1 = s2` | Deep copy (shallow for pointers inside) |
| P720 | Union type punning | `u.i = 1; use u.f` | Legal in C |
| P721 | Bit field range | `int x:3` | -4 to 3 signed |
| P722 | Void pointer | `void *p; p++` | Non-standard! |
| P723 | fprintf return | Returns chars written | Check for error |
| P724 | fgetc returns | `int` (not char) | For EOF detection |
| P725 | argc minimum value | 1 (program name) | Even with no args |
| P726 | extern variable | Declaration only | Define elsewhere |
| P727 | Storage duration | auto/static/dynamic/thread | Four kinds |
| P728 | Linkage types | none/internal/external | Three kinds |
| P729 | Sequence points | Between `&&`, `\|\|`, `?:`, `,`, `;` | Not in function args! |
| P730 | Implicit int promotion | `char + char` | Promoted to int first |

**P731-P780** — GATE NAT (Numerical Answer Type) Practice:

| # | Question | Answer |
|---|---|---|
| P731 | How many times is `printf` called? `for(int i=0;i<5;i++) for(int j=i;j<5;j++) printf("*");` | $5+4+3+2+1 = 15$ |
| P732 | Stack depth for `f(100)` where `f(n)=f(n-1), f(0)=return` | 101 frames |
| P733 | Output: `int s=0; for(int i=1;i<=10;i++) s+=i*i; printf("%d",s);` | 385 |
| P734 | `int a=0xFF; printf("%d",a&0x0F);` | 15 |
| P735 | `int a=0xAB; printf("%d",(a>>4)&0x0F);` | 10 (0xA) |
| P736 | Number of digits in `f(n)`: `f(n) = 1 + f(n/10), f(<10)=1`; `f(12345)` | 5 |
| P737 | `sizeof(int(*)[10])` on 64-bit? | 8 (it's a pointer!) |
| P738 | `sizeof(int[10])` | 40 |
| P739 | How many strings in `char *a[5]`? | 5 (array of 5 char pointers) |
| P740 | `int a[5]; &a[4]-&a[0]` | 4 |
| P741 | `int a[5]; (char*)&a[4]-(char*)&a[0]` | 16 |
| P742 | Iterations: `int i=1; while(i<1000) i*=2;` count? | 10 ($2^{10}=1024$) |
| P743 | `int c=0; for(int i=1;i<=n;i++) for(int j=1;j<=i;j++) c++; c` for n=10? | $\frac{10 \times 11}{2} = 55$ |
| P744 | `int c=0; for(int i=n;i>=1;i/=2) c++; c` for n=64? | 7 (64,32,16,8,4,2,1) |
| P745 | `int c=0; for(int i=1;i<=n;i*=2) for(int j=1;j<=n;j++) c++; c` for n=8? | $4 \times 8 = 32$ |
| P746 | `struct{char a;short b;int c;long long d;}` sizeof (64-bit)? | 16: 1+1pad+2+4+8 |
| P747 | `union{char a[10];int b;double c;}` sizeof? | 16 (max=10, align to 8→16) |
| P748 | Number of set bits in 0xFF? | 8 |
| P749 | XOR of all numbers 1 to 4: `1^2^3^4`? | 4 (01^10=11, 11^11=00, 00^100=100) |
| P750 | `int x=0; for(int i=0;i<8;i++) x\|=(1<<i); printf("%d",x);` | 255 ($2^8-1$) |
| P751 | Minimum number of comparisons to find max in array of n? | $n-1$ |
| P752 | Minimum comparisons to find both min and max? | $\lceil 3n/2 \rceil - 2$ |
| P753 | `sizeof(void(*)(void))` on 64-bit? | 8 (function pointer) |
| P754 | `int a[3]={1,2}; a[2]`? | 0 (partially initialized → rest zero) |
| P755 | `static int a[3]; a[0]+a[1]+a[2]`? | 0 (all zero-initialized) |
| P756 | `char s[]=""; sizeof(s)?` | 1 (just null terminator) |
| P757 | `char *p=""; sizeof(p)?` | 8 (pointer) |
| P758 | Max value of `unsigned char`? | 255 |
| P759 | Max value of `signed char`? | 127 |
| P760 | Bits in `int` (typical)? | 32 |
| P761 | `INT_MAX` value? | $2^{31}-1 = 2147483647$ |
| P762 | `INT_MIN` value? | $-2^{31} = -2147483648$ |
| P763 | `UINT_MAX` value? | $2^{32}-1 = 4294967295$ |
| P764 | `float` has how many exponent bits? | 8 |
| P765 | `float` has how many mantissa bits? | 23 |
| P766 | `double` has how many exponent bits? | 11 |
| P767 | `double` has how many mantissa bits? | 52 |
| P768 | Bias for `float` exponent? | 127 |
| P769 | Bias for `double` exponent? | 1023 |
| P770 | Smallest positive normal `float`? | $2^{-126} \approx 1.18 \times 10^{-38}$ |
| P771 | Largest `float`? | $(2 - 2^{-23}) \times 2^{127} \approx 3.4 \times 10^{38}$ |
| P772 | IEEE 754: +0 and -0 equal? | YES: `+0.0 == -0.0` |
| P773 | How many `char` in "Hello\0World"? | 12 (includes both nulls + final null) |
| P774 | `strlen("Hello\0World")`? | 5 |
| P775 | `sizeof("Hello\0World")`? | 12 |
| P776 | `printf("%d", sizeof(printf))`? | Compile error or 1 (GCC: sizeof function = 1) |
| P777 | `int x = 0x10; printf("%d",x);` | 16 |
| P778 | `int x = 010; printf("%d",x);` | 8 (octal!) |
| P779 | `int x = 0b1010; printf("%d",x);` | 10 (binary literal, non-standard but supported) |
| P780 | Number of comparisons in binary search of 1024 elements? | $\lceil\log_2 1024\rceil + 1 = 11$ worst case |

---

# 📝 Practice Set 12: The Last 100 Express (P781 – P880)

**P781.** Scope rules in C:
```
┌─ File scope (global)
│  ├─ Function scope (labels only: goto targets)
│  │  └─ Block scope ({...})
│  │     └─ Nested block scope
│  └─ Function prototype scope (parameter names in declaration)
```

**P782.** Linkage types:
| Type | Meaning | Example |
|---|---|---|
| External | Visible across translation units | `int x;` at file scope |
| Internal | Visible only in this file | `static int x;` at file scope |
| None | Local only | Parameters, local variables |

**P783.** Storage duration:
| Duration | Lifetime | Examples |
|---|---|---|
| Automatic | Block entry to exit | Local vars (auto, register) |
| Static | Entire program | Global, static local |
| Allocated | malloc to free | Heap objects |
| Thread | Thread lifetime | `_Thread_local` (C11) |

**P784-P810** — C Standard Library Quick Reference:

| Function/Header | Purpose |
|---|---|
| `<stdio.h>` | printf, scanf, files |
| `<stdlib.h>` | malloc, free, atoi, qsort, exit |
| `<string.h>` | strlen, strcpy, strcmp, memcpy, memset |
| `<math.h>` | sqrt, pow, sin, cos, log |
| `<ctype.h>` | isalpha, isdigit, toupper, tolower |
| `<limits.h>` | INT_MAX, INT_MIN, CHAR_BIT |
| `<float.h>` | FLT_MAX, DBL_MAX, FLT_EPSILON |
| `<stdint.h>` | int8_t, uint32_t, intptr_t |
| `<stdbool.h>` | bool, true, false |
| `<stddef.h>` | NULL, size_t, ptrdiff_t, offsetof |
| `<stdarg.h>` | va_list, va_start, va_arg, va_end |
| `<assert.h>` | assert() macro |
| `<signal.h>` | signal, raise, SIGINT, SIGTERM |
| `<setjmp.h>` | setjmp, longjmp |
| `<errno.h>` | errno, ERANGE, EDOM |
| `<time.h>` | time, clock, difftime |
| `<complex.h>` | _Complex, cabs, carg (C99) |
| `<tgmath.h>` | Type-generic math macros (C99) |
| `<inttypes.h>` | PRId64, SCNu32 format macros |
| `<wchar.h>` | Wide character functions |

**P811.** Common `string.h` functions with time complexity:

| Function | Complexity | Notes |
|---|---|---|
| `strlen(s)` | $O(n)$ | Scans for `\0` |
| `strcpy(d,s)` | $O(n)$ | Copies until `\0` |
| `strncpy(d,s,n)` | $O(n)$ | May not null-terminate! |
| `strcmp(a,b)` | $O(\min(m,n))$ | Lexicographic |
| `strncmp(a,b,n)` | $O(n)$ | Compare at most n chars |
| `strcat(d,s)` | $O(m+n)$ | Appends s to d |
| `strncat(d,s,n)` | $O(m+n)$ | Appends at most n chars |
| `strchr(s,c)` | $O(n)$ | First occurrence of c |
| `strrchr(s,c)` | $O(n)$ | Last occurrence of c |
| `strstr(h,n)` | $O(m \times n)$ | Find substring |
| `memcpy(d,s,n)` | $O(n)$ | Non-overlapping copy |
| `memmove(d,s,n)` | $O(n)$ | Overlapping-safe copy |
| `memset(s,c,n)` | $O(n)$ | Fill n bytes with c |
| `memcmp(a,b,n)` | $O(n)$ | Compare n bytes |

**P812-P830** — Input/Output Traps:

| # | Code | Issue/Output |
|---|---|---|
| P812 | `int x; scanf("%d", &x); printf("%d", x);` (input: "5") | 5 |
| P813 | `char c; scanf("%c", &c);` after reading int | Reads leftover `\n`! |
| P814 | Fix: `scanf(" %c", &c);` | Space skips whitespace |
| P815 | `int x; scanf("%d", x);` (missing &) | Crash! x value used as address |
| P816 | `char s[10]; scanf("%s", s);` (input: "hello world") | Only reads "hello" (stops at space) |
| P817 | `fgets(s, 10, stdin)` advantage? | Reads whole line, limits length |
| P818 | `printf("100%")` | UB: incomplete format specifier; use `"100%%"` |
| P819 | `printf("%d%n", 42, &count);` | Prints "42", stores 2 in count |
| P820 | `printf("%.3f", 3.14159);` | 3.142 (rounds) |
| P821 | `printf("%10d", 42);` | `        42` (right-aligned, 10 wide) |
| P822 | `printf("%-10d|", 42);` | `42        |` (left-aligned) |
| P823 | `printf("%05d", 42);` | `00042` (zero-padded) |
| P824 | `printf("%+d %+d", 42, -42);` | `+42 -42` |
| P825 | `printf("%.5s", "Hello World");` | `Hello` (precision truncates) |
| P826 | `printf("%*d", 10, 42);` | Same as `%10d` → `        42` |
| P827 | `sprintf` buffer overflow? | YES! Use snprintf |
| P828 | `sscanf("42 hello", "%d %s", &x, s);` returns? | 2 (two items matched) |
| P829 | `sscanf("abc", "%d", &x);` returns? | 0 (no match) |
| P830 | `sscanf("", "%d", &x);` returns? | EOF (-1) |

**P831-P880** — Final Express Round:

| # | Concept | Key Point |
|---|---|---|
| P831 | Dangling pointer | Points to freed/out-of-scope memory |
| P832 | Wild pointer | Uninitialized pointer (any address) |
| P833 | Null pointer | Deliberately points to nothing (0) |
| P834 | Void pointer | Generic: can hold any address |
| P835 | Near/far pointers | Legacy 16-bit segmented memory |
| P836 | Const correctness | Program by contract: promise not to modify |
| P837 | Opaque pointer | Hides implementation (API design) |
| P838 | Intrusive linked list | Node embedded in data struct |
| P839 | Non-intrusive list | List owns separate nodes with data pointers |
| P840 | Circular buffer | Ring buffer with head/tail indices |
| P841 | Sentinel node | Dummy node simplifies edge cases |
| P842 | Preprocessor phases | Trigraphs → line splicing → tokenization → macro expansion |
| P843 | Translation phases (C standard) | 8 phases including preprocessing and compilation |
| P844 | Compilation model | Source → preprocess → compile → assemble → link → executable |
| P845 | Static library (`.a` / `.lib`) | Linked at compile time (copied into executable) |
| P846 | Dynamic library (`.so` / `.dll`) | Linked at runtime (shared) |
| P847 | Position-independent code (PIC) | Required for shared libraries |
| P848 | Symbol table | Maps names to addresses |
| P849 | Relocation | Adjusting addresses during linking |
| P850 | Loading | OS places executable in memory |
| P851 | Stack canary | Detects buffer overflow (security) |
| P852 | ASLR | Randomizes memory layout (security) |
| P853 | DEP / NX bit | Prevents executing data as code |
| P854 | Return-oriented programming (ROP) | Chains existing code fragments |
| P855 | Format string attack | `printf(user_input)` → info leak / code exec |
| P856 | Heap overflow | Writing past heap buffer |
| P857 | Integer overflow attack | `size = n * sizeof(T)` may wrap |
| P858 | Off-by-one in loops | `<=` vs `<` — classic bug |
| P859 | Fencepost error | N posts need N-1 fences |
| P860 | Magic numbers | Use `#define` or `enum` instead |
| P861 | Code smell: global variables | Avoid: use parameters |
| P862 | Code smell: deep nesting | Refactor with early returns |
| P863 | Code smell: long functions | Split into smaller ones |
| P864 | K&R style vs ANSI | `int f(a, b) int a,b;` vs `int f(int a, int b)` |
| P865 | Implicit `int` (C89) | `f() { ... }` = `int f() { ... }` — removed in C99 |
| P866 | `int f()` vs `int f(void)` in C | `f()` = unspecified params; `f(void)` = no params |
| P867 | Designated initializer order | Can be any order; last value wins if repeated |
| P868 | Compound literal scope | Block scope (auto storage) |
| P869 | Statement expression (GCC) | `({ int x = 5; x+1; })` evaluates to 6 |
| P870 | Typeof operator (GCC) | `typeof(expr)` gives type |
| P871 | Zero-length array (GCC) | `int a[0];` at end of struct (pre-C99 FAM) |
| P872 | Computed goto (GCC) | `goto *ptr;` with address labels `&&label` |
| P873 | `__attribute__((cleanup))` (GCC) | RAII-like cleanup for locals |
| P874 | Binary constants `0b1010` (GCC) | C23 will standardize |
| P875 | `_Generic` dispatch table | Compile-time overloading in C11 |
| P876 | `_Atomic int x;` (C11) | Atomic operations without locks |
| P877 | `atomic_fetch_add(&x, 1)` | Thread-safe increment |
| P878 | Memory ordering: `memory_order_relaxed` | No synchronization |
| P879 | `memory_order_seq_cst` | Full sequential consistency |
| P880 | C23 new features | Digit separators (`1'000'000`), `#embed`, `typeof` standard |

---

# 📋 Quick Revision: C Data Types Cheat Sheet

| Type | Size (typical) | Range (signed) | Format |
|---|---|---|---|
| `char` | 1 | -128 to 127 | `%c` or `%d` |
| `unsigned char` | 1 | 0 to 255 | `%u` |
| `short` | 2 | -32768 to 32767 | `%hd` |
| `unsigned short` | 2 | 0 to 65535 | `%hu` |
| `int` | 4 | $-2^{31}$ to $2^{31}-1$ | `%d` |
| `unsigned int` | 4 | 0 to $2^{32}-1$ | `%u` |
| `long` | 4/8 | At least $-2^{31}$ to $2^{31}-1$ | `%ld` |
| `unsigned long` | 4/8 | At least 0 to $2^{32}-1$ | `%lu` |
| `long long` | 8 | $-2^{63}$ to $2^{63}-1$ | `%lld` |
| `unsigned long long` | 8 | 0 to $2^{64}-1$ | `%llu` |
| `float` | 4 | ~$\pm 3.4 \times 10^{38}$ | `%f` or `%e` |
| `double` | 8 | ~$\pm 1.8 \times 10^{308}$ | `%lf` or `%le` |
| `long double` | 8/12/16 | Platform-specific | `%Lf` |
| `size_t` | 4/8 | `0` to `SIZE_MAX` | `%zu` |
| `ptrdiff_t` | 4/8 | Signed pointer diff | `%td` |
| `intptr_t` | 4/8 | Can hold any pointer | `%td` |
| Pointer | 4/8 | Address | `%p` |

---

# 📋 Complete ASCII Quick Reference

| Range | Characters | Notes |
|---|---|---|
| 0-31 | Control chars | `\0`(0), `\t`(9), `\n`(10), `\r`(13) |
| 32 | Space | |
| 48-57 | `'0'`-`'9'` | Digit = `c - '0'` |
| 65-90 | `'A'`-`'Z'` | Upper = `c & ~0x20` |
| 97-122 | `'a'`-`'z'` | Lower = `c \| 0x20` |
| 127 | DEL | |

**Key values:** `'0'=48, 'A'=65, 'a'=97, ' '=32, '\n'=10, '\0'=0`

---

# 📋 Escape Sequences Reference

| Sequence | Meaning |
|---|---|
| `\n` | Newline |
| `\t` | Tab |
| `\r` | Carriage return |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |
| `\0` | Null character |
| `\a` | Alert (bell) |
| `\b` | Backspace |
| `\f` | Form feed |
| `\v` | Vertical tab |
| `\?` | Question mark |
| `\ooo` | Octal value |
| `\xhh` | Hex value |
| `\uhhhh` | Unicode (C99) |

---

# 📋 Format Specifier Complete Reference

| Specifier | Type | Example |
|---|---|---|
| `%d` / `%i` | Signed int | `printf("%d", -42)` → `-42` |
| `%u` | Unsigned int | `printf("%u", 42u)` → `42` |
| `%o` | Unsigned octal | `printf("%o", 8)` → `10` |
| `%x` / `%X` | Unsigned hex | `printf("%x", 255)` → `ff` |
| `%f` | Float/double | `printf("%f", 3.14)` → `3.140000` |
| `%e` / `%E` | Scientific | `printf("%e", 3.14)` → `3.140000e+00` |
| `%g` / `%G` | Shorter of f/e | Auto-selects |
| `%c` | Char | `printf("%c", 65)` → `A` |
| `%s` | String | `printf("%s", "hi")` → `hi` |
| `%p` | Pointer | `printf("%p", p)` → `0x7fff...` |
| `%n` | Write count | ⚠️ Security risk! |
| `%%` | Literal % | `printf("100%%")` → `100%` |
| `%ld` | Long | |
| `%lld` | Long long | |
| `%lu` | Unsigned long | |
| `%llu` | Unsigned long long | |
| `%zu` | size_t | |
| `%hd` | Short | |
| `%hhd` | Signed char | |
| `%Lf` | Long double | |

**Width & Precision:**
- `%10d` → right-align in 10 chars
- `%-10d` → left-align
- `%05d` → zero-pad
- `%.3f` → 3 decimal places
- `%.5s` → max 5 chars from string
- `%*d` → width from argument

---

# 📋 7-Day C Programming Revision Plan

| Day | Topic | Focus |
|---|---|---|
| **Day 1** | Data types, operators, precedence | P51-P100, Precedence table |
| **Day 2** | Control flow, switch, loops | P1-P20, P201-P220 |
| **Day 3** | Pointers & arrays | P21-P50, P321-P340 |
| **Day 4** | Recursion & functions | P151-P180, P621-P640 |
| **Day 5** | Structs, unions, padding | P41-P50, P481-P510 |
| **Day 6** | Preprocessor, memory, UB | P511-P540, P541-P600 |
| **Day 7** | Mixed GATE practice + review | P701-P780, all traps |

---

# 🎯 Top 25 Most Important Rules for GATE C

1. `sizeof` does NOT evaluate its argument
2. Array name decays to pointer (but isn't one)
3. String literals are immutable (stored in rodata)
4. `char s[]` copies string; `char *s` points to literal
5. `-1 > 0u` is TRUE (signed→unsigned promotion)
6. `=` in condition: assignment, not comparison
7. Missing `break` = fall-through in switch
8. `&&` and `||` short-circuit with sequence points
9. Post-increment in expressions: current value used, then incremented
10. `*p++` = `*(p++)` — deref first, increment pointer
11. Struct padding aligns members to their size
12. Union size = largest member (aligned)
13. `#define SQ(x) x*x` → always parenthesize: `((x)*(x))`
14. Macro args with side effects: evaluated multiple times!
15. `static` local persists between calls; `static` global = file scope only
16. `malloc` returns `void*`; must check for NULL
17. `free(p)` doesn't set p to NULL — do it manually
18. Pointer arithmetic: in units of pointed-to type
19. `a[i] = *(a+i) = *(i+a) = i[a]` — all equivalent!
20. Recursive Fibonacci: $O(2^n)$ — always note complexity
21. `printf` returns number of chars printed
22. `main` returns `int`; 0 = success
23. Sequence points: `;`, `&&`, `||`, `?:`, `,` (not in arg lists!)
24. UB = literally anything can happen (including "working")
25. When in doubt, trace on paper — C output prediction is mechanical

---

25. When in doubt, trace on paper — C output prediction is mechanical

---

# 📝 Practice Set 13: Tricky Output Prediction Marathon (P881 – P1000)

**P881.** What is the output?
```c
int main() {
    int a = 1;
    int b = (a++, a++, a++);
    printf("%d %d", a, b);
}
```
→ Comma operator: a goes 1→2→3→4. b gets value of last expression (a++ returns 3, then a becomes 4). **Output: 4 3**

**P882.** What is the output?
```c
int main() {
    int a = 0;
    int b = (a == 0) ? (a += 1) : (a += 2);
    printf("%d %d", a, b);
}
```
→ a==0 is true → a+=1 → a=1, b=1. **Output: 1 1**

**P883.** What is the output?
```c
int main() {
    char *ptr = "GATE";
    printf("%c\n", *ptr++);  // G, ptr now at 'A'
    printf("%c\n", *ptr++);  // A, ptr now at 'T'
    printf("%c\n", *ptr++);  // T, ptr now at 'E'
    printf("%c\n", *ptr);    // E
}
```
**Output: G A T E** (each on separate line)

**P884.** What is the output?
```c
int main() {
    int x = 10;
    int y = (x > 5) && printf("Hello");
    printf(" %d %d", x, y);
}
```
→ x>5 is true, so printf("Hello") runs (prints "Hello", returns 5). y = 1 && 5 = 1.
**Output: Hello 10 1**

**P885.** What is the output?
```c
#include <stdio.h>
int f() { printf("f"); return 1; }
int g() { printf("g"); return 0; }
int main() {
    if (f() || g()) printf("T");
    else printf("F");
}
```
→ f() prints "f", returns 1 (true). Short-circuit: g() NOT called. **Output: fT**

**P886.** What is the output?
```c
int f() { printf("f"); return 0; }
int g() { printf("g"); return 0; }
int main() {
    if (f() || g()) printf("T");
    else printf("F");
}
```
→ f() prints "f" returns 0. g() called, prints "g" returns 0. 0||0 = false. **Output: fgF**

**P887.** What is the output?
```c
int f() { printf("f"); return 1; }
int g() { printf("g"); return 0; }
int main() {
    if (f() && g()) printf("T");
    else printf("F");
}
```
→ f() prints "f" returns 1. g() called, prints "g" returns 0. 1&&0 = false. **Output: fgF**

**P888.** What is the output?
```c
int f() { printf("f"); return 0; }
int g() { printf("g"); return 1; }
int main() {
    if (f() && g()) printf("T");
    else printf("F");
}
```
→ f() prints "f" returns 0. Short-circuit AND: g() NOT called. **Output: fF**

**P889.** What is `sizeof(struct S)`?
```c
struct S {
    int x;
    struct S *next;
};
```
→ On 64-bit: int(4) + pad(4) + pointer(8) = **16**.

**P890.** What is the output?
```c
int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *ptr = (int*)(&arr + 1);
    printf("%d %d", *(arr + 1), *(ptr - 1));
}
```
→ `&arr + 1` = address just past entire array (one-past-end of full array).
→ `ptr - 1` goes back one int → last element. `*(arr+1) = 20`, `*(ptr-1) = 50`.
**Output: 20 50**

**P891.** What is the output?
```c
int main() {
    char arr[] = "GATECS";
    char *ptr = arr;
    while (*ptr != '\0') {
        printf("%c", *ptr);
        ptr += 2;
    }
}
```
→ ptr: G→T→C→'\0' (past 'S' is '\0')? Wait: "GATECS" = G,A,T,E,C,S,\0.
→ ptr at G(0), print G. ptr at T(2), print T. ptr at C(4), print C. ptr at \0(6), stop.
**Output: GTC**

**P892.** What is the output?
```c
int main() {
    int x = 5;
    int *p = &x;
    int **q = &p;
    *p = 10;
    **q = 20;
    printf("%d", x);
}
```
→ `*p = 10` sets x=10. `**q = 20` → `*(*q) = *(p) = x = 20`. **Output: 20**

**P893.** What does this function do?
```c
void f(char *s) {
    if (*s == '\0') return;
    f(s + 1);
    f(s + 1);
    printf("%c", *s);
}
```
→ For "ab": f("ab") → f("b") → f("") return; f("") return; print 'b'. f("b") again → print 'b'. print 'a'.
→ **Output: bba**. In general: each char printed $2^{n-i-1}$ times for position $i$.

**P894.** What is the output?
```c
int main() {
    int a = 1, b = 2, c = 3;
    printf("%d", a += b += c);
}
```
→ Right-to-left: `b += c` → b=5. `a += b` → a=6. **Output: 6**

**P895.** What is the output?
```c
int main() {
    int i;
    for (i = 0; i < 10; i += 3)
        printf("%d ", i);
}
```
→ i = 0, 3, 6, 9. **Output: 0 3 6 9**

**P896.** What is the output?
```c
int main() {
    int n = 5;
    int count = 0;
    while (n) {
        count += n & 1;
        n >>= 1;
    }
    printf("%d", count);
}
```
→ 5 = 101 → 2 set bits. **Output: 2**

**P897.** What is the output?
```c
int main() {
    int x = 0xABCD;
    printf("%x", (x >> 8) & 0xFF);
}
```
→ `0xABCD >> 8 = 0xAB`. `0xAB & 0xFF = 0xAB`. **Output: ab**

**P898.** What is the output?
```c
int main() {
    char s1[] = "GATE";
    char s2[] = "GATE";
    printf("%d %d", s1 == s2, strcmp(s1, s2));
}
```
→ `s1 == s2` compares addresses (different arrays!) → 0. `strcmp` compares content → 0.
**Output: 0 0**

**P899.** What is `sizeof(arr)` inside function?
```c
void f(int arr[100]) {
    printf("%lu", sizeof(arr));
}
```
→ Array parameter decays to pointer. **Output: 8** (64-bit) — NOT 400!

**P900.** What is the output?
```c
int main() {
    int a[5] = {1, 2, 3, 4, 5};
    int sum = 0;
    for (int *p = a; p < a + 5; p++)
        sum += *p;
    printf("%d", sum);
}
```
→ **Output: 15**

**P901-P960** — Rapid-fire C Express:

| # | Code | Output |
|---|---|---|
| P901 | `int a=5; printf("%d",a>3?10:20);` | 10 |
| P902 | `int a=2; printf("%d",a>3?10:20);` | 20 |
| P903 | `printf("%d",sizeof(1?1:1.0));` | 8 (promoted to double) |
| P904 | `int x=3; printf("%d",x&1?"odd":"even"[0]);` | Tricky! `x&1` = 1 → true → `"odd"` → prints address as int → UB. Need parens: `(x&1) ? "odd" : "even"` |
| P905 | `printf("%d",'A'+'a');` | 65+97 = 162 |
| P906 | `printf("%c",'A'+32);` | 'a' (65+32=97) |
| P907 | `printf("%c",'5'-'0'+'A');` | 'F' (5+65=70) |
| P908 | `int x=-1; printf("%u",x);` | 4294967295 |
| P909 | `int x=5; printf("%d",~x+1);` | -5 (two's complement negation!) |
| P910 | `int x=0; printf("%d",x&&(1/x));` | 0 (short-circuit: 1/x never evaluated!) |
| P911 | `int x=1; printf("%d",x\|\|(1/0));` | 1 (short-circuit: 1/0 never evaluated!) |
| P912 | `int x=5; x^=x; printf("%d",x);` | 0 (XOR self = 0) |
| P913 | `int a=5,b=3; a^=b^=a^=b; printf("%d %d",a,b);` | UB in C99 (multiple mod without SP). In practice: swap → 3 5 |
| P914 | `char c=255; printf("%d",c);` | -1 (if char is signed) |
| P915 | `unsigned char c=255; c++; printf("%d",c);` | 0 (wraps around) |
| P916 | `int x=-1; printf("%d",x>>1);` | -1 (arithmetic right shift, typically) |
| P917 | `unsigned x=-1; printf("%d",x>>1);` | 2147483647 (logical shift fills 0) → printed as %d: 2147483647 |
| P918 | `printf("%d",sizeof(sizeof(int)));` | 8 (sizeof returns size_t, typically 8 bytes on 64-bit) |
| P919 | `int a[0]; printf("%lu",sizeof(a));` | 0 (GCC extension) |
| P920 | `printf("%d",++"hello"[0]);` | Compile error! String literal not modifiable lvalue |
| P921 | `char s[]="hello"; printf("%d",++s[0]);` | 105 ('h'+1='i'=105) |
| P922 | `int x=5; int y=x---3; printf("%d %d",x,y);` | Maximal munch: `x-- - 3` = 5-3=2, x→4. **Output: 4 2** |
| P923 | `printf("%d",0["GATE"]);` | 71 ('G' = 0x47 = 71) |
| P924 | `printf("%c",2["GATE"]);` | T |
| P925 | `int i=0; if(i++&&i++) {}; printf("%d",i);` | i=1 (first i++ returns 0→short-circuit, i becomes 1) |
| P926 | `int i=1; if(i++&&i++) {}; printf("%d",i);` | i=3 (first returns 1→true, second evaluated, i goes 1→2→3) |
| P927 | `int a=5; printf("%d",(a%2==0)+(a%3==0));` | 0 (5%2≠0, 5%3≠0) |
| P928 | `int a=6; printf("%d",(a%2==0)+(a%3==0));` | 2 (both true: 1+1) |
| P929 | `printf("%d\n",sizeof(NULL));` | 8 (on 64-bit: NULL is `(void*)0`) |
| P930 | `int *p=NULL; printf("%d",p==0);` | 1 (NULL == 0 is true) |
| P931 | `printf("%d %d",0==0.0, 0=='\0');` | 1 1 |
| P932 | `printf("%d",sizeof(main));` | 1 or error (sizeof function: GCC=1, others=error) |
| P933 | `void f(void); printf("%d",sizeof(f()));` | Error: sizeof void |
| P934 | `int a=10; printf("%o %x",a,a);` | 12 a |
| P935 | `int a=010+10; printf("%d",a);` | 18 (octal 8 + decimal 10) |
| P936 | `printf("%d",'\\');` | 92 (backslash ASCII) |
| P937 | `printf("%d",'\n');` | 10 (newline ASCII) |
| P938 | `printf("%d",'\t');` | 9 (tab ASCII) |
| P939 | `printf("%d %d",EOF,sizeof(EOF));` | -1 4 (EOF is int) |
| P940 | `int x=5; x*=x+=3; printf("%d",x);` | UB (multiple modifications) |
| P941 | `char *p = "abc" "def"; printf("%s",p);` | abcdef (string concatenation) |
| P942 | `printf("%d",(int)(1.5+1.5));` | 3 |
| P943 | `printf("%d",(int)1.5+(int)1.5);` | 2 (1+1) |
| P944 | `printf("%d",1/3);` | 0 |
| P945 | `printf("%f",1/3);` | UB! 1/3=0 (int), but %f expects double |
| P946 | `printf("%f",1.0/3);` | 0.333333 |
| P947 | `printf("%f",(float)1/3);` | 0.333333 (1 cast to float before division) |
| P948 | `int a=-5; printf("%d",a/3);` | -1 (truncation toward zero) |
| P949 | `int a=-5; printf("%d",a%3);` | -2 (sign follows dividend) |
| P950 | `printf("%d",!!42);` | 1 |
| P951 | `printf("%d",!!!42);` | 0 |
| P952 | `int x=0; for(;x;) printf("hi");` | (nothing: x=0=false) |
| P953 | `int x=1; while(x) if(x==5) break; else x++;` | Loop: x goes 1→2→3→4→5 → break |
| P954 | `int arr[]={[4]=5,[0]=1}; printf("%d",arr[2]);` | 0 (not initialized → 0) |
| P955 | `printf("%d",2*3+4*5);` | 26 |
| P956 | `printf("%d",2*(3+4)*5);` | 70 |
| P957 | `int x=0xFF; printf("%d",x&(x-1));` | 0xFE = 254 (clears lowest bit) |
| P958 | `int x=0x80; printf("%d",x&(x-1));` | 0 (single bit set: power of 2!) |
| P959 | `int x=5; printf("%d",x\|(x-1));` | 7 (101 | 100 = 111) |
| P960 | `int x=12; printf("%d",x&(-x));` | 4 (lowest set bit: 1100 & 0100 = 0100) |

---

# 📝 Practice Set 14: GATE Exam Simulation (P961 – P1040)

**Section A: 1-Mark Questions (P961-P990)**

**P961.** The number of elements in the array `int a[3][4][5]` is:
→ $3 \times 4 \times 5 = 60$

**P962.** `strlen(NULL)` causes:
→ Undefined behavior (segfault typically)

**P963.** Which storage class is default for local variables?
→ `auto`

**P964.** Which of these can be used as variable names?
- (a) `int` ❌ (keyword)
- (b) `_var` ✅
- (c) `2name` ❌ (starts with digit)
- (d) `my-var` ❌ (hyphen not allowed)
→ (b)

**P965.** What is `sizeof(struct{})` in GCC?
→ 0 (GCC extension; not standard)

**P966.** What is the value of `~0 & 0xFF`?
→ `~0` = all 1s (0xFFFFFFFF). `& 0xFF` = 0xFF = 255.

**P967.** `int *p = NULL; if(!p) printf("null");` prints?
→ `!NULL` = `!0` = 1 = true. **Prints: null**

**P968.** What does `volatile` prevent?
→ Compiler optimization of variable accesses.

**P969.** Number of `*` printed: `for(int i=0;i<4;i++) for(int j=0;j<i;j++) printf("*");`
→ j iterations: 0+1+2+3 = **6**

**P970.** `int (*p)(int,int)` is a:
→ Pointer to function taking two ints and returning int.

**P971.** `strcat` appends to:
→ First argument (destination). Destination must have enough space!

**P972.** `enum{A=3, B, C, D=10, E}` → value of E?
→ B=4, C=5, E=11.

**P973.** Which operator CANNOT be overloaded in C?
→ Trick question: C has NO operator overloading at all. (That's C++.)

**P974.** `#define NULL 0` vs `#define NULL ((void*)0)`: which is preferred?
→ `((void*)0)` — proper type for pointer null.

**P975.** Maximum value of `unsigned` 16-bit integer?
→ $2^{16} - 1 = 65535$

**P976.** `int f(int n){return n>0 ? n+f(n-2) : 0;}` → `f(7)` = ?
→ 7 + f(5) = 7+5+f(3) = 12+3+f(1) = 15+1+f(-1) = 16+0 = **16**

**P977.** What is `sizeof(3.14)` in C?
→ 8 (default: `double`, not float)

**P978.** `static int x;` at file scope has linkage:
→ Internal (visible only in this file)

**P979.** `float x = 0.1; if(x == 0.1)` → true or false?
→ FALSE! 0.1 is `double`. `float(0.1) ≠ double(0.1)` due to precision.
Fix: `if(x == 0.1f)` or `if(fabs(x - 0.1) < 1e-7)`

**P980.** `int a[5]; float *p = (float*)a;` — is this safe?
→ NO! Strict aliasing violation. UB to dereference.

**Section B: 2-Mark Questions (P981-P1010)**

**P981.** What is the output?
```c
int main() {
    int a[] = {1, 2, 3, 4, 5};
    int *p[] = {a, a+1, a+2, a+3, a+4};
    int **pp = p;
    printf("%d ", **pp);
    pp++;
    printf("%d ", *++*pp);
    printf("%d ", **pp);
}
```
→ `**pp` = `*p[0]` = `a[0]` = 1. Print 1.
→ `pp++`: pp now points to p[1]. `*pp` = `p[1]` = `a+1`. `++*pp` increments `p[1]` to `a+2`. `*` dereferences: `a[2]` = 3. Print 3.
→ `**pp` = `*(p[1])` = `*(a+2)` = `a[2]` = 3. Print 3.
**Output: 1 3 3**

**P982.** What is the output?
```c
void swap(int *a, int *b) {
    int *t = a;
    a = b;
    b = t;
}
int main() {
    int x=1, y=2;
    swap(&x, &y);
    printf("%d %d", x, y);
}
```
→ Only local pointers a, b are swapped. x, y unchanged! **Output: 1 2**

**P983.** Fix:
```c
void swap(int *a, int *b) {
    int t = *a;
    *a = *b;
    *b = t;
}
```
Now **Output: 2 1** ✅

**P984.** What is the output?
```c
int main() {
    int a = 1;
    int b = !a;
    int c = ~a;
    int d = -a;
    printf("%d %d %d %d", a, b, c, d);
}
```
→ `!1 = 0`. `~1 = ~(00...01) = 11...10 = -2`. `-1 = -1`. **Output: 1 0 -2 -1**

**P985.** What is the output?
```c
int* f(int *p) {
    (*p)++;
    return p;
}
int main() {
    int a = 5;
    int *q = f(&a);
    printf("%d %d", a, *q);
}
```
→ `(*p)++` increments a to 6. Returns p (= &a). `*q = a = 6`. **Output: 6 6**

**P986.** What does this program print?
```c
int main() {
    char s[] = "12345";
    int sum = 0;
    for (int i = 0; s[i]; i++)
        sum += s[i] - '0';
    printf("%d", sum);
}
```
→ Sum of digits: 1+2+3+4+5 = 15. **Output: 15**

**P987.** How many times is `f()` called?
```c
int f(int n) {
    if (n <= 1) return 1;
    return f(n-1) + f(n-2);
}
f(5);
```
→ Call tree: f(5)→f(4)+f(3). f(4)→f(3)+f(2). f(3)→f(2)+f(1). f(2)→f(1)+f(0).
→ Total calls: f(5)=1, f(4)=1, f(3)=2, f(2)=3, f(1)=5, f(0)=3 → **15 calls**.

**P988.** What is the output?
```c
int main() {
    int matrix[2][3] = {{1,2,3},{4,5,6}};
    int *p = &matrix[0][0];
    printf("%d ", *(p + 4));
    printf("%d", *(p + 5));
}
```
→ Flat layout: {1,2,3,4,5,6}. `*(p+4)=5`, `*(p+5)=6`. **Output: 5 6**

**P989.** What is the output?
```c
int main() {
    char *names[] = {"Zero", "One", "Two"};
    printf("%c %c %c", *names[0], *(names[1]+1), names[2][2]);
}
```
→ `*names[0]='Z'`, `*(names[1]+1)='n'`, `names[2][2]='o'`. **Output: Z n o**

**P990.** What does this function compute?
```c
unsigned int f(unsigned int n) {
    unsigned int r = 0;
    while (n) {
        r = (r << 1) | (n & 1);
        n >>= 1;
    }
    return r;
}
```
→ Reverses bits! f(0b1010) = 0b0101 = 5. f(0b1100) = 0b0011 = 3.

**P991-P1010** — More 2-mark style:

| # | Question | Answer |
|---|---|---|
| P991 | What does `n & (n-1)` do? | Clears the lowest set bit |
| P992 | When does `n & (n-1) == 0`? | When n is a power of 2 (or 0) |
| P993 | What is `n ^ (n >> 1)`? | Gray code of n |
| P994 | How to isolate rightmost 0 bit? | `~n & (n+1)` |
| P995 | How to turn on rightmost 0 bit? | `n \| (n+1)` |
| P996 | Swap nibbles of byte `x`? | `((x & 0x0F) << 4) \| ((x & 0xF0) >> 4)` |
| P997 | Reverse bits of a byte? | Lookup table or divide-and-conquer |
| P998 | Next power of 2 ≥ n? | See P697 algorithm |
| P999 | Check if exactly k bits set? | `popcount(n) == k` |
| P1000 | XOR 0 to n efficiently? | Pattern: n%4: 0→n, 1→1, 2→n+1, 3→0 |

**Section C: True/False Express (P1001-P1040)**

| # | Statement | T/F |
|---|---|---|
| P1001 | `#include` can appear anywhere in a file | T (but bad practice; conventionally at top) |
| P1002 | A function can return a struct | T |
| P1003 | A function can return an array | F (arrays are not first-class in C) |
| P1004 | The address of a register variable can be taken | F |
| P1005 | `goto` can jump into a block from outside | T (but highly discouraged — can skip initialization) |
| P1006 | Labels in C have function scope | T |
| P1007 | C supports nested functions | F (standard; GCC has extension) |
| P1008 | `sizeof` works on types | T: `sizeof(int)` |
| P1009 | `sizeof` works on expressions | T: `sizeof(x+1)` |
| P1010 | `sizeof` can overflow | F (returns `size_t`, unsigned) |
| P1011 | `a[i]` and `i[a]` always equivalent | T (by definition) |
| P1012 | Pointer + pointer is valid | F (only pointer - pointer and pointer ± integer) |
| P1013 | Pointer - pointer gives `ptrdiff_t` | T |
| P1014 | `void f()` and `void f(void)` are same in C | F! `f()` = unspecified params, `f(void)` = no params |
| P1015 | `int` is at least 16 bits | T (standard guarantee) |
| P1016 | `long` is at least as big as `int` | T |
| P1017 | `long long` is at least 64 bits | T |
| P1018 | `char` is always 8 bits | Not quite — CHAR_BIT ≥ 8 (always 8 on modern platforms) |
| P1019 | `sizeof(char)` is always 1 | T (by definition) |
| P1020 | `CHAR_BIT` is always 8 | Standard only guarantees ≥ 8 |
| P1021 | `NULL` == `(void*)0` | T (on all practical platforms) |
| P1022 | `((void*)0)` and `0` are both valid null pointer constants | T |
| P1023 | Casting int to pointer is portable | F (implementation-defined) |
| P1024 | Casting pointer to int always works | F (`intptr_t` exists for this, but pointer may be larger than int) |
| P1025 | `memcpy(a, a, n)` is defined behavior | T (same pointer = non-overlapping satisfied trivially) |
| P1026 | `memmove(a, a, n)` is defined behavior | T |
| P1027 | `realloc(p, 0)` always frees p | NO — implementation-defined |
| P1028 | `malloc(0)` always returns NULL | NO — implementation-defined |
| P1029 | `sizeof` evaluates its operand for VLAs | T |
| P1030 | Compound literals have block scope | T (auto storage, but static possible at file scope) |
| P1031 | Flexible array member can be only member | F (struct needs at least one named member before FAM) |
| P1032 | Bit fields can use any integer type | Implementation-defined (standard guarantees `_Bool`, `int`, `unsigned`) |
| P1033 | Anonymous struct/union allowed in C11 | T |
| P1034 | `_Alignas` can decrease alignment | F (can only increase or maintain!) |
| P1035 | `restrict` is not in C++ | T (C99 only) |
| P1036 | `inline` semantics differ between C and C++ | T |
| P1037 | Tentative definition is unique to C | T (not in C++) |
| P1038 | `_Generic` is evaluated at compile time | T |
| P1039 | C11 threads are optional | T (feature-test macro) |
| P1040 | C23 removes K&R function definitions | T |

---

# 📝 Practice Set 15: Miscellaneous Advanced C (P1041 – P1100)

**P1041.** Spiral/Clockwise Rule for complex declarations:

"Start at the identifier, go clockwise (right, then up-right, then left, then down-left) reading each element."

Examples:
```
int *p;              → p is a pointer to int
int *p[10];          → p is an array of 10 pointers to int
int (*p)[10];        → p is a pointer to an array of 10 ints
int (*p)(int);       → p is a pointer to a function (taking int) returning int
int *(*p)(int);      → p is a pointer to a function (taking int) returning pointer to int
int (*p[5])(int);    → p is an array of 5 pointers to functions (taking int) returning int
int *(*p[5])(int);   → p is an array of 5 ptrs to funcs (int) returning int*
char *(*(*p)[3])();  → p is a ptr to array of 3 ptrs to functions returning char*
```

**P1042.** `cdecl` tool equivalents:

| C Declaration | English |
|---|---|
| `int **p` | pointer to pointer to int |
| `int (*p)[10]` | pointer to array of 10 int |
| `int *p[10]` | array of 10 pointer to int |
| `void (*signal(int, void(*)(int)))(int)` | signal is a function (int, ptr to func(int)) returning ptr to func(int) returning void |
| `char (*(*x())[])()` | x is a function returning pointer to array of pointers to functions returning char |

**P1043-P1060** — Code quality patterns in C:

| # | Pattern | Example |
|---|---|---|
| P1043 | Error handling with goto | `if(!ptr){goto cleanup;}...cleanup: free(ptr); return -1;` |
| P1044 | Defensive NULL check | `if(p != NULL) { *p = 5; }` |
| P1045 | Bounds checking | `if(i >= 0 && i < n) { arr[i]; }` |
| P1046 | Safe string copy | `snprintf(dest, sizeof(dest), "%s", src);` |
| P1047 | Prevent double free | `free(p); p = NULL;` |
| P1048 | Assert preconditions | `assert(n > 0 && "n must be positive");` |
| P1049 | Encapsulation via opaque ptr | Header: `struct Obj; Obj* create();` Impl: `struct Obj { int x; };` |
| P1050 | vtable pattern (OOP in C) | `struct Shape { int (*area)(struct Shape*); };` |
| P1051 | X-macro technique | `#define COLORS X(RED) X(GREEN) X(BLUE)` |
| P1052 | Stringified enum | `const char *name[] = { [RED]="RED", [GREEN]="GREEN" };` |
| P1053 | Container-of macro | `#define container_of(ptr, type, member) ((type*)((char*)(ptr) - offsetof(type, member)))` |
| P1054 | Static assert message | `_Static_assert(sizeof(int)==4, "int must be 4 bytes");` |
| P1055 | Type-safe min/max (C11) | Use `_Generic` |
| P1056 | Flexible API: varargs | `void log(const char *fmt, ...) { va_list ap; ... }` |
| P1057 | Packed protocol struct | `struct __attribute__((packed)) Header { uint8_t type; uint16_t len; };` |
| P1058 | Memory pool pattern | Pre-allocate array, manage with free list |
| P1059 | Callback + context | `void register(void (*cb)(void *ctx), void *ctx);` |
| P1060 | Compile-time array size | `#define ARRAY_SIZE(arr) (sizeof(arr)/sizeof((arr)[0]))` |

**P1061-P1100** — Connecting C to Other GATE Subjects:

| # | Topic | C Connection | GATE Relevance |
|---|---|---|---|
| P1061 | OS: Process memory | Stack/heap/BSS/data layout | COA + OS questions |
| P1062 | OS: System calls | `fork()`, `exec()`, `wait()` | OS questions use C |
| P1063 | OS: Threads | `pthread_create`, mutex | Concurrency in C |
| P1064 | OS: Signals | `signal()`, `kill()` | IPC mechanism |
| P1065 | COA: Cache | Array traversal (row-major vs col-major) | Locality of reference |
| P1066 | COA: Alignment | Structure padding | Memory alignment |
| P1067 | COA: Endianness | `union { int i; char c[4]; }` to detect | Byte ordering |
| P1068 | COA: IEEE 754 | `float` representation | Numeric computation |
| P1069 | CN: Bit manipulation | Protocol headers, checksums | Network programming |
| P1070 | CN: Socket programming | `socket()`, `bind()`, `listen()` | All in C |
| P1071 | Compiler: Phases | Lexer→Parser→Semantic→CodeGen | `gcc -E`, `-S`, `-c` flags |
| P1072 | Compiler: Symbol table | `extern`, `static`, scope | Linking questions |
| P1073 | Compiler: Activation record | Stack frame layout | Recursion questions |
| P1074 | Compiler: Calling conventions | cdecl, stdcall | Parameter passing |
| P1075 | DSA: Linked list in C | `struct Node {int data; Node *next;}` | DS implementation |
| P1076 | DSA: Stack using array | `arr[top++] = x;` | Stack operations |
| P1077 | DSA: Queue using array | Circular buffer with head/tail | Queue operations |
| P1078 | DSA: BST in C | `struct TreeNode` with left/right | Tree problems |
| P1079 | DSA: Graph adjacency list | Array of linked lists | Graph representation |
| P1080 | DSA: Hash table | Array + chaining (linked list) | Collision handling |
| P1081 | DBMS: File operations | `fopen`, `fread`, `fwrite` | File organization |
| P1082 | ML: Matrix as 2D array | Row-major storage affects cache | Numerical computing |
| P1083 | ML: Random numbers | `rand()`, `srand()` | Simulation |
| P1084 | Python vs C comparison | Memory management, typing | Cross-subject |
| P1085 | Aptitude: Number systems | Binary/octal/hex conversions | All use C notation |
| P1086 | Digital Logic: Bit ops | AND, OR, XOR, NOT | Gate-level operations |
| P1087 | TOC: State machines | `switch(state)` pattern | Lexer implementation |
| P1088 | TOC: Regular expressions | `regex.h` library | Pattern matching |
| P1089 | Stack push = ? | `arr[++top] = val;` or `arr[top++] = val;` (convention!) | DS implementation |
| P1090 | Stack pop = ? | `return arr[top--];` or `return arr[--top];` | Match with push |
| P1091 | Queue enqueue | `arr[rear++] = val; if(rear==n) rear=0;` | Circular buffer |
| P1092 | Queue dequeue | `val = arr[front++]; if(front==n) front=0;` | Circular buffer |
| P1093 | Infix to postfix | Stack-based algorithm in C | Expression evaluation |
| P1094 | Postfix evaluation | Stack: push operands, pop for operators | Classic GATE |
| P1095 | Recursion → Stack | Every recursion = implicit stack | Fundamental connection |
| P1096 | Dynamic programming in C | 1D/2D arrays, bottom-up | Algorithm + C |
| P1097 | Divide & conquer | Merge sort, quicksort in C | Sorting algorithms |
| P1098 | Greedy in C | Activity selection, Huffman | Array sorting + selection |
| P1099 | Backtracking | N-queens, subset sum | Recursion + arrays |
| P1100 | Complexity analysis | Loop counting in C code | Most GATE algo questions! |

---

# 📋 Complete C Keyword Reference (C11)

| Category | Keywords |
|---|---|
| Data Types | `char`, `short`, `int`, `long`, `float`, `double`, `void`, `_Bool`, `_Complex`, `_Imaginary` |
| Type Qualifiers | `const`, `volatile`, `restrict`, `_Atomic` |
| Storage Classes | `auto`, `register`, `static`, `extern`, `_Thread_local` |
| Struct/Union/Enum | `struct`, `union`, `enum`, `typedef` |
| Control Flow | `if`, `else`, `switch`, `case`, `default`, `while`, `do`, `for`, `break`, `continue`, `goto`, `return` |
| Operators | `sizeof`, `_Alignof`, `_Generic` |
| Alignment | `_Alignas` |
| Others | `inline`, `_Noreturn`, `_Static_assert`, `signed`, `unsigned` |
| **Total** | **44 keywords in C11** |

---

# 📋 Index of All Practice Problems

| Set | Range | Topic | Count |
|---|---|---|---|
| Set 1 | P1 – P50 | Output Prediction (mixed) | 50 |
| Set 2 | P51 – P150 | Quick Fire Facts | 100 |
| Set 3 | P151 – P220 | Recursion Deep Dive | 70 |
| Set 4 | P221 – P320 | GATE PYQ Style | 100 |
| Set 5 | P321 – P400 | Memory & Pointer Mastery | 80 |
| Set 6 | P401 – P480 | True/False Conceptual | 80 |
| Set 7 | P481 – P540 | Structure Padding | 60 |
| Set 8 | P511 – P580 | Preprocessor Mastery | 70 |
| Set 9 | P541 – P620 | Undefined Behavior | 80 |
| Set 10 | P621 – P700 | Multi-step GATE Problems | 80 |
| Set 11 | P701 – P780 | GATE PYQ Reproductions | 80 |
| Set 12 | P781 – P880 | The Last 100 Express | 100 |
| Set 13 | P881 – P1000 | Tricky Marathon | 120 |
| Set 14 | P961 – P1040 | GATE Exam Simulation | 80 |
| Set 15 | P1041 – P1100 | Miscellaneous Advanced | 60 |
| **Total** | | | **~1100 problems** |

---

# 🎯 Final Mastery Self-Assessment

| Topic | Can you...? |
|---|---|
| Data Types | Calculate ranges for signed/unsigned, any width? |
| Operators | Apply precedence table without looking? |
| Pointers | Trace `*p++`, `(*p)++`, `++*p`, `*++p` correctly? |
| Arrays | Explain why `sizeof(arr)` differs in/out of function? |
| Strings | Distinguish `char s[]` vs `char *s`? |
| Recursion | Draw full call tree for any recursive function? |
| Memory | Label which segment (stack/heap/BSS/data) for any variable? |
| Structs | Calculate sizeof with padding for any struct? |
| Unions | Explain how type punning works? |
| Preprocessor | Expand macros step-by-step including `#` and `##`? |
| UB | Identify at least 20 common UB sources? |
| Output Prediction | Trace any 20-line C program on paper in 3 minutes? |

If you can answer YES to all: **you're GATE-ready for C!** 🎯

---

# 📝 Practice Set 16: Pointer Gymnastics (P1101 – P1200)

**P1101.** What is the output?
```c
int main() {
    int a[] = {10, 20, 30, 40, 50};
    int *p = a;
    int *q = a + 4;
    printf("%d", (int)(q - p));
}
```
→ Pointer subtraction gives element count: `4 - 0 = 4`. **Output: 4**

**P1102.** What is the output?
```c
int main() {
    int a[2][3] = {{1,2,3},{4,5,6}};
    printf("%d %d", (*a)[2], (*(a+1))[0]);
}
```
→ `(*a)[2] = a[0][2] = 3`. `(*(a+1))[0] = a[1][0] = 4`. **Output: 3 4**

**P1103.** What is the output?
```c
int main() {
    int a[] = {1,2,3,4,5,6};
    int (*p)[3] = (int(*)[3])a;
    printf("%d", p[1][1]);
}
```
→ `p` sees `a` as 2 rows × 3 cols. `p[1][1] = a[3+1] = a[4] = 5`. **Output: 5**

**P1104.** What is the output?
```c
int main() {
    char *p = "Hello";
    char *q = "Hello";
    printf("%d", p == q);
}
```
→ Implementation-defined! Compiler may merge identical string literals → 1, or keep separate → 0. (Typically 1 with optimization.)

**P1105.** What is the output?
```c
void f(int **p) {
    static int x = 10;
    *p = &x;
}
int main() {
    int *ptr;
    f(&ptr);
    printf("%d", *ptr);
}
```
→ Static `x` persists. `ptr` points to it. **Output: 10**

**P1106.** What is the output?
```c
int main() {
    int a = 0x12345678;
    char *p = (char*)&a;
    printf("%x", *p);
}
```
→ Endianness test! Little-endian (x86): `*p = 0x78`. **Output: 78**

**P1107.** What is the output?
```c
int main() {
    void *p;
    int a = 42;
    p = &a;
    printf("%d", *(int*)p);
}
```
→ Cast void pointer to int pointer before deref. **Output: 42**

**P1108.** What is the output?
```c
int main() {
    int x = 5;
    int *const p = &x;
    *p = 10;
    printf("%d", x);
}
```
→ `p` is const (can't change where it points), but can modify `*p`. **Output: 10**

**P1109.** What is the output?
```c
int main() {
    const int x = 5;
    const int *p = &x;
    // *p = 10;  // ERROR: can't modify through const pointer
    printf("%d", *p);
}
```
→ **Output: 5**

**P1110.** const pointer to const:
```c
const int * const p = &x;
// *p = 10;   ERROR
// p = &y;    ERROR
```
→ Most restrictive: can't change pointer or value.

**P1111-P1150** — Pointer type quiz:

| # | Declaration | What is it? |
|---|---|---|
| P1111 | `int *p` | pointer to int |
| P1112 | `int **p` | pointer to pointer to int |
| P1113 | `int ***p` | triple pointer |
| P1114 | `int *p[5]` | array of 5 pointers to int |
| P1115 | `int (*p)[5]` | pointer to array of 5 ints |
| P1116 | `int (*p)()` | pointer to function returning int |
| P1117 | `int *p()` | function returning pointer to int |
| P1118 | `int (*p[3])()` | array of 3 pointers to functions returning int |
| P1119 | `int *(*p)()` | pointer to function returning pointer to int |
| P1120 | `int (*(*p)())[5]` | pointer to function returning pointer to array of 5 ints |
| P1121 | `void (*p)(int)` | pointer to function taking int, returning void |
| P1122 | `int (*p)(void*, void*)` | comparator for qsort! |
| P1123 | `const int *p` | pointer to const int (data is const) |
| P1124 | `int const *p` | same as above! |
| P1125 | `int * const p` | const pointer to int (pointer is const) |
| P1126 | `const int * const p` | both const |
| P1127 | `int (* volatile p)` | volatile pointer to int |
| P1128 | `int * restrict p` | restricted pointer (no aliasing) |
| P1129 | `typedef int (*FP)(int, int)` | type alias for function pointer |
| P1130 | `FP table[4]` | array of 4 function pointers |
| P1131 | `void (*signal(int, void(*)(int)))(int)` | signal function (returns function pointer!) |
| P1132 | Simplified with typedef | `typedef void (*SigHandler)(int); SigHandler signal(int, SigHandler);` |

**P1133-P1200** — Pointer arithmetic precision:

Given:
```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;
```

| # | Expression | Value | Type |
|---|---|---|---|
| P1133 | `p` | `&arr[0]` | `int*` |
| P1134 | `p + 1` | `&arr[1]` | `int*` |
| P1135 | `*p` | 10 | `int` |
| P1136 | `*(p + 4)` | 50 | `int` |
| P1137 | `p[3]` | 40 | `int` |
| P1138 | `3[p]` | 40 | `int` |
| P1139 | `&arr[5]` | valid (one past end) | `int*` |
| P1140 | `*&arr[5]` | UB! | — |
| P1141 | `arr` | `&arr[0]` (decayed) | `int*` |
| P1142 | `&arr` | same address, different type | `int(*)[5]` |
| P1143 | `sizeof(arr)` | 20 | `size_t` |
| P1144 | `sizeof(&arr)` | 8 (pointer) | `size_t` |
| P1145 | `sizeof(*&arr)` | 20 (deref ptr-to-array = array) | `size_t` |
| P1146 | `&arr + 1` | address past entire array | `int(*)[5]` |
| P1147 | `(int*)(&arr + 1) - p` | 5 | `ptrdiff_t` |
| P1148 | `&arr[0] == arr` | always true | `int` (1) |
| P1149 | `&arr == arr` | same address, true (with implicit conversion) | |
| P1150 | `sizeof(&arr[0])` | 8 (pointer) | `size_t` |

For `char *s = "GATE"`:

| # | Expression | Value |
|---|---|---|
| P1151 | `*s` | 'G' (71) |
| P1152 | `*(s+1)` | 'A' (65) |
| P1153 | `s[4]` | '\0' (0) |
| P1154 | `strlen(s)` | 4 |
| P1155 | `sizeof(s)` | 8 (pointer!) |
| P1156 | `s++; *s` | 'A' |
| P1157 | `(*s)++` | UB: modifying string literal! |
| P1158 | `++*s` | UB: same reason |

For `char s[] = "GATE"`:

| # | Expression | Value |
|---|---|---|
| P1159 | `sizeof(s)` | 5 (includes '\0') |
| P1160 | `s++` | ERROR: array can't be incremented! |
| P1161 | `(*s)++` | Legal! s[0] becomes 'H' (71+1=72) |
| P1162 | `s[4]` | '\0' |
| P1163 | `&s[5]` | UB (past null terminator) |
| P1164 | `s == &s[0]` | true |

For `int a[3][4]`:

| # | Expression | Type |
|---|---|---|
| P1165 | `a` | `int(*)[4]` (pointer to first row) |
| P1166 | `a[0]` | `int*` (pointer to first element of row 0) |
| P1167 | `*a` | Same as `a[0]` → `int*` |
| P1168 | `**a` | `a[0][0]` → `int` |
| P1169 | `&a` | `int(*)[3][4]` (pointer to entire 2D array) |
| P1170 | `a + 1` | `int(*)[4]` pointing to row 1 |
| P1171 | `*a + 1` | `int*` pointing to `a[0][1]` |
| P1172 | `*(a+1)` | `int*` pointing to `a[1][0]` |
| P1173 | `*(a+1)+2` | `int*` pointing to `a[1][2]` |
| P1174 | `*(*(a+1)+2)` | `a[1][2]` → `int` |
| P1175 | `&a[0][0]` | `int*` |
| P1176 | `(int*)a` | `int*` (flattened view) |
| P1177 | `*((int*)a + 5)` | `a[1][1]` (5th element in row-major) |
| P1178 | `a[i][j]` equivalent | `*(*(a+i)+j)` |
| P1179 | Address formula | `base + (i*4 + j) * sizeof(int)` |
| P1180 | `sizeof(a)` | `3*4*4 = 48` |
| P1181 | `sizeof(a[0])` | `4*4 = 16` |
| P1182 | `sizeof(a[0][0])` | 4 |
| P1183 | Number of rows | `sizeof(a)/sizeof(a[0]) = 3` |
| P1184 | Number of cols | `sizeof(a[0])/sizeof(a[0][0]) = 4` |

**P1185-P1200** — Dynamic memory patterns:

| # | Pattern | Code | Notes |
|---|---|---|---|
| P1185 | 1D array | `int *a = malloc(n * sizeof(int));` | Single allocation |
| P1186 | 2D array (ptrs) | `int **a = malloc(r*sizeof(int*)); for(...) a[i]=malloc(c*sizeof(int));` | r+1 allocations |
| P1187 | 2D array (flat) | `int *a = malloc(r*c*sizeof(int)); access: a[i*c+j]` | Single allocation ✅ |
| P1188 | 2D contiguous | `int (*a)[c] = malloc(r*sizeof(*a));` | VLA pointer type |
| P1189 | Struct array | `struct S *a = malloc(n*sizeof(struct S));` | |
| P1190 | String duplicate | `char *dup = malloc(strlen(s)+1); strcpy(dup, s);` | +1 for '\0'! |
| P1191 | Resize array | `int *t = realloc(a, new_n*sizeof(int)); if(t) a=t;` | Always use temp! |
| P1192 | Free 2D (ptrs) | `for(...) free(a[i]); free(a);` | Reverse order |
| P1193 | Memory leak | `p = malloc(10); p = malloc(20);` | First allocation leaked! |
| P1194 | Dangling pointer | `free(p); *p = 5;` | Use-after-free |
| P1195 | Double free | `free(p); free(p);` | UB |
| P1196 | Buffer overflow | `int *p = malloc(5*sizeof(int)); p[5] = 10;` | Out of bounds |
| P1197 | Uninitialized read | `int *p = malloc(sizeof(int)); printf("%d",*p);` | Garbage (UB) |
| P1198 | calloc advantage | `int *p = calloc(5, sizeof(int)); // all zeros` | Zero-initialized |
| P1199 | Check malloc | `if (!p) { perror("malloc"); exit(1); }` | Always! |
| P1200 | Use ARRAY_SIZE | `#define N(a) (sizeof(a)/sizeof(*(a)))` | Only for actual arrays |

---

# 📝 Practice Set 17: Complete Code Tracing Workout (P1201 – P1300)

**P1201.** Trace this completely:
```c
int f(int n) {
    printf("%d ", n);
    if (n > 1) f(n/2);
    printf("%d ", n);
}
f(8);
```
→ Going down: print 8, 4, 2, 1. Going up: print 1, 2, 4, 8.
**Output: 8 4 2 1 1 2 4 8**

**P1202.** Trace:
```c
int f(int a, int b) {
    if (b == 0) return 0;
    return a + f(a, b-1);
}
printf("%d", f(5, 3));
```
→ `5 + f(5,2) = 5 + 5 + f(5,1) = 5+5+5+f(5,0) = 15`. Multiplication via addition! **Output: 15**

**P1203.** Trace:
```c
int f(int a, int b) {
    if (b == 0) return 1;
    return a * f(a, b-1);
}
printf("%d", f(2, 10));
```
→ $2^{10} = 1024$. Exponentiation via recursion. **Output: 1024**

**P1204.** What is `f(100)`?
```c
int f(int n) {
    if (n == 0) return 0;
    return n%10 + f(n/10);
}
```
→ Sum of digits: `0 + f(10)` = `0 + 0 + f(1)` = `0 + 0 + 1 + f(0)` = 1. Wait:
→ f(100): 100%10=0 + f(10). f(10): 10%10=0 + f(1). f(1): 1%10=1 + f(0)=0. Total = **1**. **Output: 1**

**P1205.** What is `f(1234)`?
```c
int f(int n) {
    if (n == 0) return 0;
    return (n%10 > f(n/10)) ? n%10 : f(n/10);
}
```
→ Maximum digit: compares last digit with max of remaining. f(1234): max(4, f(123)) = max(4, max(3, f(12))) = max(4, max(3, max(2, f(1)))) = max(4, max(3, max(2, 1))) = max(4, max(3, 2)) = max(4, 3) = **4**. **Output: 4**

**P1206.** What is the output?
```c
int main() {
    int i = 0, j = 0;
    while (i++ < 5) j += i;
    printf("%d %d", i, j);
}
```
→ i: 0→1 (0<5=T, j+=1=1), 1→2 (T, j+=2=3), 2→3 (T, j+=3=6), 3→4 (T, j+=4=10), 4→5 (T, j+=5=15), 5→6 (5<5=F, stop). **Output: 6 15**

**P1207.** What is the output?
```c
int main() {
    int a = 10;
    int b = a + (a = 5);  // UB? In C11: assignment in operand is unsequenced
    printf("%d", b);      // with respect to use of a on left of +
}
```
→ ⚠️ UB: `a` is read and modified in the same expression without a sequence point.

**P1208.** What does this print?
```c
int main() {
    for (int i = 1; i <= 5; i++)
        for (int j = 1; j <= i; j++)
            printf("*");
    printf("\n");
}
```
→ Stars per row: 1+2+3+4+5 = 15. (All on one line — no inner newline!)
**Output: *************** (15 stars)**

**P1209.** What is the value of `*p`?
```c
int a = 10;
int *p = &a;
int *q = p;
*q = 20;
printf("%d", *p);
```
→ `p` and `q` both point to `a`. `*q=20` sets `a=20`. **Output: 20**

**P1210.** What happens?
```c
char s[] = "Hello";
*(s + 5) = '!';
printf("%s", s);
```
→ `s[5]` was '\0'. Now it's '!'. String has no terminator → `printf` reads past end → **UB**

**P1211-P1250** — Code tracing rapid fire:

| # | Code | Output |
|---|---|---|
| P1211 | `int s=1; for(int i=1;i<=5;i++) s*=i; printf("%d",s);` | 120 (5!) |
| P1212 | `int a=7; printf("%d",a%=3);` | 1 (7%3=1) |
| P1213 | `int a=10,b=3; printf("%d",a/b);` | 3 |
| P1214 | `int a=-10,b=3; printf("%d",a/b);` | -3 (toward zero) |
| P1215 | `int a=-10,b=3; printf("%d",a%b);` | -1 |
| P1216 | `int a=10,b=-3; printf("%d",a%b);` | 1 |
| P1217 | `int x=0xF0; printf("%d",x>>4);` | 15 (240>>4=15) |
| P1218 | `int n=15; printf("%d",n&(~(n-1)));` | 1 (lowest set bit of 1111) |
| P1219 | `int n=12; printf("%d",n&(~(n-1)));` | 4 (lowest set bit of 1100) |
| P1220 | `int n=12; printf("%d",n&(-n));` | 4 (same: lowest set bit trick) |
| P1221 | `printf("%d",sizeof(char[10]));` | 10 |
| P1222 | `printf("%d",sizeof(int[10]));` | 40 |
| P1223 | `printf("%d",sizeof("GATE"[0]));` | 1 (char) |
| P1224 | `printf("%d",sizeof(&"GATE"[0]));` | 8 (pointer) |
| P1225 | `int a=-1; printf("%d",a==~0);` | 1 (true: both are all 1s) |
| P1226 | `int a=7; printf("%d",a^a);` | 0 |
| P1227 | `int a=7; printf("%d",a\|0);` | 7 |
| P1228 | `int a=7; printf("%d",a&0);` | 0 |
| P1229 | `int a=7; printf("%d",a^0);` | 7 |
| P1230 | `int a=7; printf("%d",a\|~0);` | -1 (all bits set) |
| P1231 | `int a=5,b=3; printf("%d",(a>b)-(a<b));` | 1 (sign function) |
| P1232 | `int a=3,b=3; printf("%d",(a>b)-(a<b));` | 0 |
| P1233 | `int a=1,b=3; printf("%d",(a>b)-(a<b));` | -1 |
| P1234 | `int x=0; x=x-(-x); printf("%d",x);` | 0 |
| P1235 | `int x=5; x=x-(-x); printf("%d",x);` | 10 (5-(-5)=10) |
| P1236 | `char c='5'; printf("%d",c-'0');` | 5 (char to digit) |
| P1237 | `int d=7; printf("%c",d+'0');` | '7' (digit to char) |
| P1238 | `printf("%d",'Z'-'A'+1);` | 26 |
| P1239 | `printf("%d",'z'-'a'+1);` | 26 |
| P1240 | `char c='G'; printf("%c",c\|0x20);` | 'g' (to lowercase) |
| P1241 | `char c='g'; printf("%c",c&~0x20);` | 'G' (to uppercase) |
| P1242 | `char c='A'; printf("%c",c^0x20);` | 'a' (toggle case) |
| P1243 | `char c='a'; printf("%c",c^0x20);` | 'A' (toggle case) |
| P1244 | `int arr[]={5,3,1,4,2}; printf("%d",*arr);` | 5 |
| P1245 | `int arr[]={5,3,1,4,2}; printf("%d",arr[sizeof(arr)/sizeof(arr[0])-1]);` | 2 (last element) |
| P1246 | `int x=5; int *p=&x; ++*p; printf("%d",x);` | 6 |
| P1247 | `int x=5; int *p=&x; *p+=2; printf("%d",x);` | 7 |
| P1248 | `int x=5; int *p=&x; (*p)--; printf("%d",x);` | 4 |
| P1249 | `int a=3; printf("%d %d %d",a,a<<1,a<<2);` | 3 6 12 |
| P1250 | `int a=100; printf("%d %d %d",a>>1,a>>2,a>>3);` | 50 25 12 |

**P1251-P1300** — Cross-topic Integration Express:

| # | Scenario | C Code Pattern | Answer |
|---|---|---|---|
| P1251 | Check even/odd | `n & 1` | 0 = even, 1 = odd |
| P1252 | Check power of 2 | `n > 0 && !(n & (n-1))` | true if power of 2 |
| P1253 | Next multiple of 8 | `(n + 7) & ~7` | Rounds up to 8 |
| P1254 | Align to 16 | `(n + 15) & ~15` | Alignment formula |
| P1255 | Swap bits at positions i,j | XOR trick | Advanced bit manipulation |
| P1256 | Circular shift right by k | `(x >> k) \| (x << (32-k))` | Rotate bits |
| P1257 | Circular shift left by k | `(x << k) \| (x >> (32-k))` | Rotate bits |
| P1258 | Count digits in n | `while(n) { count++; n/=10; }` | Digit count |
| P1259 | Reverse a number | `while(n) { rev=rev*10+n%10; n/=10; }` | Number reversal |
| P1260 | Check palindrome number | `rev(n) == n` | After reversal |
| P1261 | Check Armstrong number | Sum of d-th powers of digits == n | $153 = 1^3+5^3+3^3$ |
| P1262 | GCD iterative | `while(b) { t=b; b=a%b; a=t; }` | Euclidean |
| P1263 | LCM from GCD | `(a/gcd(a,b)) * b` | Avoid overflow |
| P1264 | Sieve of Eratosthenes | Boolean array, mark composites | Prime generation |
| P1265 | Binary search | `while(lo<=hi) { mid=(lo+hi)/2; ... }` | $O(\log n)$ |
| P1266 | Linear search | `for(i=0;i<n;i++) if(a[i]==x) return i;` | $O(n)$ |
| P1267 | Bubble sort | Nested loops, swap adjacent | $O(n^2)$ |
| P1268 | Selection sort | Find min, swap to front | $O(n^2)$ |
| P1269 | Insertion sort | Shift elements, insert | $O(n^2)$ best $O(n)$ |
| P1270 | Matrix multiply | Triple loop: `c[i][j] += a[i][k]*b[k][j]` | $O(n^3)$ |
| P1271 | Matrix transpose | `swap(a[i][j], a[j][i])` | In-place for square |
| P1272 | String reverse | Two-pointer swap | $O(n)$ |
| P1273 | String palindrome check | Compare s[i] == s[n-1-i] | $O(n)$ |
| P1274 | Count vowels | `strchr("aeiouAEIOU", c) != NULL` | String utility |
| P1275 | Remove duplicates (sorted) | Two-pointer technique | $O(n)$ |
| P1276 | Stack: push | `a[++top] = val` | Check overflow |
| P1277 | Stack: pop | `return a[top--]` | Check underflow |
| P1278 | Stack: peek | `return a[top]` | Don't modify top |
| P1279 | Queue: enqueue | `a[rear++] = val; rear %= n;` | Circular |
| P1280 | Queue: dequeue | `val = a[front++]; front %= n;` | Circular |
| P1281 | Linked list: count nodes | `while(p) { count++; p=p->next; }` | $O(n)$ |
| P1282 | Linked list: search | `while(p && p->data!=x) p=p->next;` | $O(n)$ |
| P1283 | Linked list: insert at end | Traverse to last, `last->next = new` | $O(n)$ |
| P1284 | Linked list: delete node | Find prev, `prev->next = curr->next; free(curr);` | Handle head case |
| P1285 | Linked list: reverse | Three pointers: prev, curr, next | $O(n)$ |
| P1286 | Linked list: detect cycle | Floyd's tortoise & hare | Two pointers |
| P1287 | BST: search | `if(x<root->data) search(left); else search(right);` | $O(h)$ |
| P1288 | BST: insert | Find NULL position, create node | $O(h)$ |
| P1289 | BST: inorder | left → root → right (gives sorted) | DFS |
| P1290 | BST: preorder | root → left → right | DFS |
| P1291 | BST: postorder | left → right → root | DFS |
| P1292 | BST: level order | Queue-based BFS | BFS |
| P1293 | Tree: height | `1 + max(height(left), height(right))` | $O(n)$ |
| P1294 | Tree: count nodes | `1 + count(left) + count(right)` | $O(n)$ |
| P1295 | Tree: count leaves | `if(!left && !right) return 1;` | Base case |
| P1296 | Heap: parent index | `(i-1)/2` for 0-indexed | Array heap |
| P1297 | Heap: left child | `2*i + 1` for 0-indexed | Array heap |
| P1298 | Heap: right child | `2*i + 2` for 0-indexed | Array heap |
| P1299 | Hash: division method | `h(k) = k % m` | Choose m as prime |
| P1300 | Hash: chaining | Array of linked lists | Collision resolution |

---

# 📋 C vs C++ Key Differences (GATE Awareness)

| Feature | C | C++ |
|---|---|---|
| Paradigm | Procedural | Multi-paradigm (OOP, generic, etc.) |
| `void f()` | Unspecified params | No params (same as `void f(void)`) |
| Type checking | Weaker | Stronger |
| `bool` type | C99: `_Bool` (stdbool.h) | Built-in keyword |
| `struct` tag | Need `struct S` everywhere | Can use `S` directly |
| Enum type | Always `int` | Can have any underlying type |
| `const int x = 5;` | Not a constant expression | Is a constant expression |
| Name mangling | None | Yes (for overloading) |
| Function overloading | Not supported | Supported |
| Default arguments | Not supported | Supported |
| References (`&`) | Not supported (use pointers) | Supported |
| `new`/`delete` | N/A (use malloc/free) | Yes |
| Classes | N/A (use structs) | Yes |
| Templates | N/A | Yes |
| Exceptions | N/A (use setjmp/longjmp) | try/catch/throw |
| Namespaces | N/A (use prefixes) | Yes |
| `restrict` | Yes (C99) | Not in standard C++ |
| VLA | Yes (C99, optional in C11) | Not in standard C++ |
| Designated init | Yes (C99) | Partial (C++20) |
| `typeof` | GCC extension (C23 standard) | `decltype` in C++11 |
| `_Generic` | Yes (C11) | Use templates instead |
| Union type punning | Defined behavior | UB (use `memcpy` or `bit_cast`) |
| `malloc` cast | Not needed (`void*` converts) | Required (stricter typing) |
| FAM | Yes | Not standard |

---

# 📋 One-Page Exam Quick Reference Card

```
🔹 SIZES: char=1, short≥2, int≥2(usually 4), long≥4, long long≥8, float=4, double=8, pointer=4/8
🔹 RANGES: char -128..127, unsigned char 0..255, int -2^31..2^31-1, unsigned 0..2^32-1
🔹 ASCII: '0'=48, 'A'=65, 'a'=97, ' '=32, '\n'=10, '\0'=0
🔹 POINTER: *p++ = *(p++), (*p)++, ++*p = ++(*p), *++p = *(++p)
🔹 SIZEOF: sizeof doesn't evaluate! sizeof(array) ≠ sizeof(pointer)
🔹 STRINGS: char s[] copies (modifiable), char *s = literal (read-only)
🔹 sizeof("abc")=4, strlen("abc")=3
🔹 MACROS: Always parenthesize: #define F(x) ((x)*(x))
🔹 STATIC: local→persists, global→file-only, function→file-only
🔹 PADDING: align to member size, total = multiple of largest
🔹 UB: signed overflow, shift≥width, NULL deref, double free, use-after-free
🔹 SHORT-CIRCUIT: && || evaluate left-to-right with sequence points
🔹 COMMA: evaluates all, returns last: (a, b, c) → c
🔹 TERNARY: a>b ? x : y — trick: (a>b)-(a<b) gives sign
🔹 SWITCH: needs break! default can be anywhere
🔹 BIT TRICKS: n&(n-1) clears lowest bit, n&(-n) = lowest bit
🔹 PROMOTION: char→int, -1 < 1u → FALSE!
🔹 RECURSION: f(n) = f(n-1)+f(n-2) is O(2^n), use DP instead
```

---

# 📝 Practice Set 18: Time Complexity from C Code (P1301 – P1380)

> 🎯 One of the most frequent GATE question types: "What is the time complexity of the following C code?"

**P1301.** Simple loop:
```c
for (int i = 0; i < n; i++)
    printf("*");
```
→ $O(n)$

**P1302.** Nested loops:
```c
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        printf("*");
```
→ $n \times n = O(n^2)$

**P1303.** Triangular loop:
```c
for (int i = 0; i < n; i++)
    for (int j = 0; j < i; j++)
        printf("*");
```
→ $0+1+2+...+(n-1) = \frac{n(n-1)}{2} = O(n^2)$

**P1304.** Logarithmic loop:
```c
for (int i = 1; i < n; i *= 2)
    printf("*");
```
→ $i$ takes values $1, 2, 4, ..., 2^k$ where $2^k < n$. So $k = O(\log n)$.

**P1305.** Log-based nested:
```c
for (int i = 1; i < n; i *= 2)
    for (int j = 0; j < n; j++)
        printf("*");
```
→ $O(n \log n)$

**P1306.** Square root loop:
```c
for (int i = 0; i * i < n; i++)
    printf("*");
```
→ Runs until $i^2 \geq n$, i.e., $i \approx \sqrt{n}$. **$O(\sqrt{n})$**

**P1307.** Harmonic-ish:
```c
for (int i = 1; i <= n; i++)
    for (int j = i; j <= n; j += i)
        printf("*");
```
→ Inner loop runs $n/i$ times. Total = $\sum_{i=1}^{n} n/i = n \cdot H_n = O(n \log n)$

**P1308.** Divide in each iteration:
```c
int i = n;
while (i > 0) {
    printf("*");
    i /= 2;
}
```
→ $O(\log n)$

**P1309.** Nested log:
```c
for (int i = n; i > 0; i /= 2)
    for (int j = i; j > 0; j /= 2)
        printf("*");
```
→ Outer: $\log n$ iterations. Inner for each $i$: $\log i$ iterations. Total = $\sum_{k=0}^{\log n} k = O(\log^2 n)$

**P1310.** Cubic:
```c
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        for (int k = 0; k < n; k++)
            printf("*");
```
→ $O(n^3)$

**P1311-P1340** — Complexity Quick Drill:

| # | Loop Pattern | Complexity |
|---|---|---|
| P1311 | `for(i=0;i<n;i+=2)` | $O(n)$ (constant factor) |
| P1312 | `for(i=0;i<n;i++) for(j=0;j<m;j++)` | $O(nm)$ |
| P1313 | `for(i=1;i<n;i*=3)` | $O(\log_3 n) = O(\log n)$ |
| P1314 | `for(i=n;i>1;i=sqrt(i))` | $O(\log \log n)$ |
| P1315 | `for(i=2;i<n;i=i*i)` | $O(\log \log n)$ |
| P1316 | `for(i=0;i<n;i++) for(j=0;j<n;j*=2)` | Infinite! (j starts at 0, j*=2 stays 0) |
| P1317 | `for(i=0;i<n;i++) for(j=1;j<n;j*=2)` | $O(n \log n)$ |
| P1318 | `for(i=1;i<=n;i++) for(j=1;j<=n;j+=i)` | $O(n \log n)$ (harmonic series) |
| P1319 | Recursive: `T(n) = T(n-1) + 1` | $O(n)$ |
| P1320 | `T(n) = T(n-1) + n` | $O(n^2)$ |
| P1321 | `T(n) = T(n/2) + 1` | $O(\log n)$ |
| P1322 | `T(n) = T(n/2) + n` | $O(n)$ (geometric series) |
| P1323 | `T(n) = 2T(n/2) + n` | $O(n \log n)$ (merge sort) |
| P1324 | `T(n) = 2T(n/2) + 1` | $O(n)$ |
| P1325 | `T(n) = 2T(n/2) + n^2` | $O(n^2)$ |
| P1326 | `T(n) = T(n/2) + T(n/2) + 1` | Same as 2T(n/2)+1 → $O(n)$ |
| P1327 | `T(n) = 4T(n/2) + n` | $O(n^2)$ |
| P1328 | `T(n) = T(n-1) + T(n-2)` | $O(2^n)$ (Fibonacci) |
| P1329 | `T(n) = 2T(n-1)` | $O(2^n)$ |
| P1330 | `T(n) = nT(n-1)` | $O(n!)$ (factorial) |
| P1331 | Binary search | $O(\log n)$ |
| P1332 | Merge sort | $O(n \log n)$ |
| P1333 | Quick sort (avg) | $O(n \log n)$ |
| P1334 | Quick sort (worst) | $O(n^2)$ |
| P1335 | Heap sort | $O(n \log n)$ |
| P1336 | Counting sort | $O(n + k)$ |
| P1337 | Radix sort | $O(d(n + k))$ |
| P1338 | Bucket sort (avg) | $O(n + k)$ |
| P1339 | Insertion sort (best) | $O(n)$ (almost sorted) |
| P1340 | Bubble sort (best with flag) | $O(n)$ |

**P1341-P1380** — Master Theorem Quick Reference:

For $T(n) = aT(n/b) + O(n^c)$:

| Case | Condition | Result | Example |
|---|---|---|---|
| 1 | $c < \log_b a$ | $O(n^{\log_b a})$ | $T(n)=4T(n/2)+n$ → $O(n^2)$ |
| 2 | $c = \log_b a$ | $O(n^c \log n)$ | $T(n)=2T(n/2)+n$ → $O(n \log n)$ |
| 3 | $c > \log_b a$ | $O(n^c)$ | $T(n)=2T(n/2)+n^2$ → $O(n^2)$ |

| # | Recurrence | a,b,c | Case | Result |
|---|---|---|---|---|
| P1341 | $T(n) = 2T(n/2) + 1$ | 2,2,0 | Case 1: $0 < 1$ | $O(n)$ |
| P1342 | $T(n) = 2T(n/2) + n$ | 2,2,1 | Case 2: $1 = 1$ | $O(n\log n)$ |
| P1343 | $T(n) = 2T(n/2) + n^2$ | 2,2,2 | Case 3: $2 > 1$ | $O(n^2)$ |
| P1344 | $T(n) = 4T(n/2) + 1$ | 4,2,0 | Case 1: $0 < 2$ | $O(n^2)$ |
| P1345 | $T(n) = 4T(n/2) + n$ | 4,2,1 | Case 1: $1 < 2$ | $O(n^2)$ |
| P1346 | $T(n) = 4T(n/2) + n^2$ | 4,2,2 | Case 2: $2 = 2$ | $O(n^2\log n)$ |
| P1347 | $T(n) = 4T(n/2) + n^3$ | 4,2,3 | Case 3: $3 > 2$ | $O(n^3)$ |
| P1348 | $T(n) = 8T(n/2) + n^2$ | 8,2,2 | Case 1: $2 < 3$ | $O(n^3)$ |
| P1349 | $T(n) = 3T(n/3) + n$ | 3,3,1 | Case 2: $1 = 1$ | $O(n\log n)$ |
| P1350 | $T(n) = 9T(n/3) + n$ | 9,3,1 | Case 1: $1 < 2$ | $O(n^2)$ |
| P1351 | $T(n) = T(n/2) + 1$ | 1,2,0 | Case 2: $0 = 0$ | $O(\log n)$ |
| P1352 | $T(n) = T(n/2) + n$ | 1,2,1 | Case 3: $1 > 0$ | $O(n)$ |
| P1353 | $T(n) = 7T(n/2) + n^2$ | 7,2,2 | Case 1: $2 < 2.81$ | $O(n^{2.81})$ (Strassen) |
| P1354 | $T(n) = T(2n/3) + 1$ | 1,3/2,0 | Case 2 | $O(\log n)$ |
| P1355 | $T(n) = 2T(n/4) + \sqrt{n}$ | 2,4,0.5 | Case 2: $0.5 = \log_4 2$ | $O(\sqrt{n}\log n)$ |
| P1356 | $T(n) = 3T(n/4) + n\log n$ | 3,4,— | $\log_4 3 \approx 0.79 < 1$ | $O(n\log n)$ (Akra-Bazzi) |
| P1357 | $T(n) = T(\sqrt{n}) + 1$ | Substitution: $m=\log n$ | $T'(m) = T'(m/2)+1$ | $O(\log\log n)$ |

**P1358-P1380** — "What is the output AND complexity?" (Combined)

| # | Code | Output | Complexity |
|---|---|---|---|
| P1358 | `int c=0; for(int i=1;i<=n;i++) c++; printf("%d",c);` (n=100) | 100 | $O(n)$ |
| P1359 | `int c=0; for(int i=1;i<=n;i++) for(int j=1;j<=n;j++) c++;` (n=10) | 100 | $O(n^2)$ |
| P1360 | `int c=0; for(int i=1;i<=n;i*=2) c++;` (n=1024) | 11 | $O(\log n)$ |
| P1361 | `int c=0; for(int i=n;i>=1;i/=2) c++;` (n=64) | 7 | $O(\log n)$ |
| P1362 | `int c=0; for(int i=0;i<n;i++) for(int j=i;j<n;j++) c++;` (n=5) | 15 | $O(n^2)$ |
| P1363 | `int c=0; for(int i=1;i*i<=n;i++) c++;` (n=100) | 10 | $O(\sqrt{n})$ |
| P1364 | `int c=0; for(int i=n;i>0;i--) for(int j=1;j<n;j*=2) c++;` (n=8) | 24 | $O(n\log n)$ |
| P1365 | `int c=0; int i=n; while(i>1){c++; i=i/2;}` (n=1000) | 9 | $O(\log n)$ |
| P1366 | `int c=0; for(int i=1;i<=n;i++) for(int j=1;j<=i*i;j++) c++;` (n=3) | 14 | $O(n^3)$ |
| P1367 | `int s=0; for(int i=1;i<=n;i++) s+=i; printf("%d",s);` (n=10) | 55 | $O(n)$ |
| P1368 | `int i=1,s=0; while(s<n){s+=i; i++;}printf("%d",i-1);` (n=10) | 4 | $O(\sqrt{n})$ |
| P1369 | `for(int i=2;i<n;i=i*i) c++;` (n=256) | 3 (i:2,4,16,256) | $O(\log\log n)$ |
| P1370 | `for(int i=n;i>1;i=sqrt(i)) c++;` (n=65536) | 4 | $O(\log\log n)$ |
| P1371 | Recursive: `void f(int n){if(n<=0)return; printf("*"); f(n-1); f(n-1);}` | $2^n-1$ stars | $O(2^n)$ |
| P1372 | `void f(int n){if(n<=0)return; for(int i=0;i<n;i++) printf("*"); f(n-1);}` | $n(n+1)/2$ stars | $O(n^2)$ |
| P1373 | `void f(int n){if(n<=1)return; for(int i=0;i<n;i++) printf("*"); f(n/2);}` | $\approx 2n$ stars | $O(n)$ |
| P1374 | `void f(int n){if(n<=1)return; f(n/2); f(n/2); printf("*");}` | $n-1$ stars | $O(n)$ |
| P1375 | `void f(int n){if(n<=1)return; f(n/2); for(int i=0;i<n;i++) printf("*"); f(n/2);}` | $O(n\log n)$ stars | $O(n\log n)$ |
| P1376 | `void f(int n){if(n<=1)return; f(n-1); f(n-1); f(n-1);}` | $O(3^n)$ calls | $O(3^n)$ |
| P1377 | `void f(int n,int k){if(n<=1)return; f(n/2,k+1); f(n/2,k+1);}` | $O(n)$ calls | $O(n)$ |
| P1378 | Merge sort | Stable, $O(n\log n)$ | Extra space $O(n)$ |
| P1379 | Quick sort | Not stable, avg $O(n\log n)$ | In-place |
| P1380 | Heap sort | Not stable, $O(n\log n)$ | In-place |

---

# 📝 Practice Set 19: Common Interview + GATE MCQ Bank (P1381 – P1440)

| # | Question | Options | Answer |
|---|---|---|---|
| P1381 | Which is NOT a valid C variable name? | a) `_x1` b) `1_x` c) `x_1_` d) `__x` | b) starts with digit |
| P1382 | What is the default return type of `main` in C? | a) void b) int c) float d) char | b) int |
| P1383 | Which loop always executes at least once? | a) while b) for c) do-while d) goto | c) do-while |
| P1384 | `sizeof(char)` is always: | a) 1 b) 2 c) 4 d) platform-dependent | a) 1 |
| P1385 | Which operator has highest precedence? | a) `+` b) `*` c) `()` d) `=` | c) `()` |
| P1386 | `NULL` is defined in which header? | a) stdio.h b) stdlib.h c) stddef.h d) all of these | d) all |
| P1387 | Which is NOT a storage class? | a) auto b) register c) volatile d) extern | c) volatile (type qualifier) |
| P1388 | `const int *p` — what is const? | a) pointer b) data c) both d) neither | b) data |
| P1389 | `int * const p` — what is const? | a) pointer b) data c) both d) neither | a) pointer |
| P1390 | Array is passed to function as: | a) value b) reference c) pointer (decayed) d) copy | c) pointer |
| P1391 | Which function returns `int` not `char`? | a) getchar b) gets c) scanf d) fgetc | a) and d) both return int! |
| P1392 | `printf("%d", printf("GATE"));` output? | a) GATE b) 4GATE c) GATE4 d) 4 | c) GATE4 |
| P1393 | `int x = 5; printf("%d", ~x);` output? | a) -5 b) -6 c) 4 d) -4 | b) -6 |
| P1394 | Correct syntax for function pointer? | a) `int *f(int)` b) `int (*f)(int)` c) `int *(f)(int)` d) `*int f(int)` | b) |
| P1395 | Which can reduce struct size? | a) Add padding b) Reorder members c) Use `static` d) Use `extern` | b) reorder by decreasing size |
| P1396 | `union U {int a; float b;};sizeof(U)=?` | a) 8 b) 4 c) 12 d) depends | b) 4 (max of int/float, both 4) |
| P1397 | Which is NOT UB? | a) INT_MAX+1 b) UINT_MAX+1 c) 1/0 d) NULL deref | b) unsigned wraps |
| P1398 | `assert(0)` does what? | a) nothing b) prints 0 c) aborts d) returns 0 | c) aborts |
| P1399 | Pre-processor `##` does? | a) comment b) stringize c) token paste d) include | c) token paste |
| P1400 | `#define X(a) #a` → `X(hello)` = ? | a) hello b) "hello" c) #hello d) X(hello) | b) "hello" |
| P1401 | `static` variable default value? | a) 0 b) 1 c) garbage d) undefined | a) 0 |
| P1402 | Dangling pointer is: | a) NULL b) wild c) pointing to freed memory d) void | c) |
| P1403 | Which header for `malloc`? | a) stdio.h b) stdlib.h c) string.h d) memory.h | b) stdlib.h |
| P1404 | `calloc(5, sizeof(int))` does what? | a) allocates 5 bytes b) allocates 20 bytes zeroed c) allocates 5 ints uninitialized d) error | b) |
| P1405 | `char *s = "hello"; s[0] = 'H';` is? | a) valid b) UB c) implementation-defined d) error | b) UB |
| P1406 | Which is true about `register`? | a) guarantees register b) can take address c) default for global d) hint only | d) hint |
| P1407 | `extern int x;` does? | a) defines x b) declares x c) initializes x d) allocates x | b) declares |
| P1408 | Little-endian stores first byte as? | a) MSB b) LSB c) middle d) random | b) LSB |
| P1409 | `flexarray[0]` at struct end is? | a) C99 feature b) GCC extension c) error d) zero-length array | b) GCC (FAM is `flexarray[]`) |
| P1410 | Maximum number of arguments to `printf`? | a) 10 b) 127 c) implementation-defined d) unlimited | c) at least 127 (C99) |

**P1411-P1440** — Rapid Conceptual:

| # | Concept | One-line Summary |
|---|---|---|
| P1411 | Sequence point | Point where all side effects have completed |
| P1412 | Undefined behavior | Compiler can do anything; program is invalid |
| P1413 | Unspecified behavior | Multiple valid options; compiler chooses one |
| P1414 | Implementation-defined | Like unspecified but compiler must document choice |
| P1415 | Linkage | Whether a name can be referred to from other TUs |
| P1416 | Translation unit | Source file after preprocessing |
| P1417 | Object | Region of storage |
| P1418 | Lvalue | Expression that designates an object (can appear on left of =) |
| P1419 | Rvalue | Expression with a value (can appear on right of =) |
| P1420 | Aggregate type | Array or struct |
| P1421 | Scalar type | Arithmetic or pointer |
| P1422 | Incomplete type | Type whose size is unknown (`struct S;`, `void`, `int a[]`) |
| P1423 | Compatible types | Types that can be used interchangeably |
| P1424 | Implicit conversion | Compiler-inserted type change (e.g., int→double) |
| P1425 | Integer promotion | char/short promoted to int in expressions |
| P1426 | Usual arithmetic conversions | Rules for mixing types in expressions |
| P1427 | Array decay | Array name→pointer in most expressions |
| P1428 | Type qualifier | const, volatile, restrict, _Atomic |
| P1429 | Storage class specifier | auto, register, static, extern, _Thread_local |
| P1430 | Inline function | Hint for compiler to expand at call site |
| P1431 | Flexible array member | Unsized array as last struct member |
| P1432 | Designated initializer | `.field = value` or `[index] = value` |
| P1433 | Compound literal | `(type){initializer}` — creates unnamed object |
| P1434 | Generic selection | `_Generic(expr, type: result, ...)` |
| P1435 | Static assertion | `_Static_assert(expr, "msg")` — compile-time check |
| P1436 | Alignment | Memory address divisibility requirement |
| P1437 | Padding | Extra bytes added for alignment |
| P1438 | Packing | Removing padding (`#pragma pack` or `__attribute__`) |
| P1439 | Endianness detection | `if(*(char*)&x == (x & 0xFF))` → little-endian |
| P1440 | Strict aliasing rule | Access object only through compatible type or char* |

---

# 📋 Updated Problem Index

| Set | Range | Topic | Count |
|---|---|---|---|
| 1 | P1-P50 | Output Prediction (mixed) | 50 |
| 2 | P51-P150 | Quick Fire Facts | 100 |
| 3 | P151-P220 | Recursion Deep Dive | 70 |
| 4 | P221-P320 | GATE PYQ Style | 100 |
| 5 | P321-P400 | Memory & Pointer Mastery | 80 |
| 6 | P401-P480 | True/False Conceptual | 80 |
| 7 | P481-P540 | Structure Padding | 60 |
| 8 | P511-P580 | Preprocessor Mastery | 70 |
| 9 | P541-P620 | Undefined Behavior Catalog | 80 |
| 10 | P621-P700 | Multi-step GATE Problems | 80 |
| 11 | P701-P780 | GATE PYQ Reproductions | 80 |
| 12 | P781-P880 | The Last 100 Express | 100 |
| 13 | P881-P1000 | Tricky Marathon | 120 |
| 14 | P961-P1040 | GATE Exam Simulation | 80 |
| 15 | P1041-P1100 | Miscellaneous Advanced | 60 |
| 16 | P1101-P1200 | Pointer Gymnastics | 100 |
| 17 | P1201-P1300 | Complete Code Tracing | 100 |
| 18 | P1301-P1380 | Time Complexity from C | 80 |
| 19 | P1381-P1440 | GATE MCQ Bank | 60 |
| **TOTAL** | | | **~1440 problems** |

---

# 📊 GATE C Programming: Topic-wise Weightage

| Topic | Weight (%) | Typical Q | Difficulty |
|---|---|---|---|
| Output prediction (pointers, operators) | 30-40% | 1-2 per paper | Medium-Hard |
| Type promotion & conversions | 10-15% | 1 per paper | Medium |
| Recursion tracing | 10-15% | 1 per paper | Medium |
| Memory layout & storage classes | 5-10% | 0-1 per paper | Easy-Medium |
| Structures, unions, padding | 5-10% | 0-1 per paper | Medium |
| Preprocessor (macro expansion) | 5-10% | 0-1 per paper | Medium |
| Time complexity of C code | 10-15% | 1 per paper (overlap with DSA) | Medium |
| String handling & arrays | 5-10% | 0-1 per paper | Easy-Medium |

---

# 🏆 10 Golden Rules for GATE C Programming

> 📌 **Rule 1:** Always trace on paper. Never solve C output in your head.

> 📌 **Rule 2:** Check for undefined behavior first. If it's UB, the answer is "cannot be determined."

> 📌 **Rule 3:** Apply operator precedence table, don't guess. "Maximal munch" for `a+++b`.

> 📌 **Rule 4:** `sizeof` never evaluates. `sizeof(x++)` doesn't change `x`.

> 📌 **Rule 5:** Array ≠ pointer, but array decays to pointer. `sizeof` tells the difference.

> 📌 **Rule 6:** Signed + unsigned comparison → unsigned wins. `-1 > 0u` is TRUE.

> 📌 **Rule 7:** Short-circuit `&&` and `||` are sequence points. Function args are NOT.

> 📌 **Rule 8:** Structure padding: align each member to its size, total to largest.

> 📌 **Rule 9:** Macros: always parenthesize everything. `#define F(x) ((x)*(x))`.

> 📌 **Rule 10:** `static` local = persists. `static` global = hidden from other files.

---

> 💡 **Pro Tip:** In GATE, C questions are worth 1-2 marks each but can be solved in under a minute if you've practiced enough output prediction. Don't waste time — trace mechanically and move on.

> 🧠 **Mindset:** C is the language of systems. Every GATE algorithm question, every OS question, every compiler question uses C. Master it once, benefit everywhere.

---

# 📝 Practice Set 20: Extreme Output Challenge (P1441 – P1550)

**P1441.** What is the output?
```c
int main() {
    int a = 2;
    int b = 5;
    int c = a > b ? a > b ? a : b : b > a ? b : a;
    printf("%d", c);
}
```
→ Outer: `a > b` → false → else branch: `b > a ? b : a` → `5 > 2` → true → `b = 5`. **Output: 5**

**P1442.** What is the output?
```c
int main() {
    char c = 125;
    c += 10;
    printf("%d", c);
}
```
→ `125 + 10 = 135`. But signed char range is -128 to 127! Overflow → impl-defined. Typically wraps: `135 - 256 = -121`. **Output: -121**

**P1443.** What is the output?
```c
int main() {
    int x = 10;
    if (x == 10)
        if (x < 20)
            printf("A");
    else
        printf("B");
}
```
→ Dangling else! The `else` binds to the nearest `if` (inner one). Since `x < 20` is true, prints "A". `else` never reached. **Output: A**

**P1444.** Change condition: What if `x = 10` and inner `if (x > 20)`?
→ Inner false → else → **Output: B** (even though outer if is true!)

**P1445.** What is the output?
```c
int main() {
    int a = 1;
    switch(a) {
        printf("NEVER");
        case 1: printf("ONE"); break;
        case 2: printf("TWO"); break;
    }
}
```
→ `printf("NEVER")` is BEFORE any case → never executed! **Output: ONE**

**P1446.** What is the output?
```c
int main() {
    int a = 0;
    switch(a) {
        case 0: a = 1;
        case 1: a = 2;
        case 2: a = 3;
    }
    printf("%d", a);
}
```
→ Match case 0: a=1. Fall-through to case 1: a=2. Fall-through to case 2: a=3. **Output: 3**

**P1447.** What is the output?
```c
int main() {
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            if(j == 1) break;
            printf("%d%d ", i, j);
        }
    }
}
```
→ For each i, j runs 0 then breaks at 1. **Output: 00 10 20 **

**P1448.** What is the output?
```c
int main() {
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            if(j == 1) continue;
            printf("%d%d ", i, j);
        }
    }
}
```
→ Skip j=1 for each i. **Output: 00 02 10 12 20 22 **

**P1449.** What is the output?
```c
int main() {
    int i = 0;
    while (i < 5) {
        i++;
        if (i == 3) continue;
        printf("%d ", i);
    }
}
```
→ i: 1(print), 2(print), 3(skip), 4(print), 5(print). **Output: 1 2 4 5**

**P1450.** What is the output?
```c
int main() {
    int x = 1, y = 2, z = 3;
    printf("%d", x < y < z);
}
```
→ `x < y` = `1 < 2` = 1. Then `1 < z` = `1 < 3` = 1. **Output: 1**

**P1451.** What about `3 > 2 > 1`?
→ `3 > 2` = 1. `1 > 1` = 0. **Output: 0** (trap!)

**P1452.** What is the output?
```c
int main() {
    int a = 5;
    switch(a) {
        default: printf("D");
        case 1: printf("1"); break;
        case 5: printf("5");
        case 6: printf("6"); break;
    }
}
```
→ Match case 5: print "5". Fall-through to case 6: print "6". Break. **Output: 56**

**P1453.** What if `a = 7` in above?
→ No match → default: print "D". Fall-through to case 1: print "1". Break. **Output: D1**

**P1454.** What is the output?
```c
int main() {
    int arr[5] = {};  // C99: zero-initialized!
    printf("%d %d %d", arr[0], arr[2], arr[4]);
}
```
→ **Output: 0 0 0**

**P1455.** What is the output?
```c
int main() {
    char s1[] = {'H','i','\0'};
    char s2[] = "Hi";
    printf("%d %d", sizeof(s1), sizeof(s2));
    printf(" %d", strcmp(s1, s2));
}
```
→ Both: size 3 (including null). `strcmp` returns 0 (equal). **Output: 3 3 0**

**P1456.** What is the output?
```c
int main() {
    char s[] = {'H','i'};  // NO null terminator!
    printf("%lu", sizeof(s));
}
```
→ sizeof = 2 (just the chars, no '\0'). **Output: 2**.
But `printf("%s", s)` would be UB — reads past end!

**P1457.** What value is stored?
```c
int main() {
    float f = 1.0 / 3.0;
    printf("%.20f", f);
}
```
→ Float has ~7 digits precision. **Output: ~0.33333334326744079590** (imprecise after 7 digits)

**P1458.** What is the output?
```c
int main() {
    double d = 1.0 / 3.0;
    printf("%.20f", d);
}
```
→ Double has ~15 digits precision. **Output: ~0.33333333333333331483**

**P1459.** What is the output?
```c
int main() {
    printf("%d", 'A' + 'B');
}
```
→ 65 + 66 = 131. **Output: 131**

**P1460.** What is the output?
```c
int main() {
    int a = 0;
    for( ; ; ) {
        if(a++ >= 5) break;
    }
    printf("%d", a);
}
```
→ a goes 0(pass)→1→2→3→4→5(5>=5 true, BUT a++ makes a=6 before break). **Output: 6**

**P1461-P1510** — Type Conversion Matrix:

| # | Expression | Result Type | Value | Why |
|---|---|---|---|---|
| P1461 | `5 + 3.0` | double | 8.0 | int→double |
| P1462 | `5 / 3` | int | 1 | int/int=int |
| P1463 | `5.0 / 3` | double | 1.6667 | 3→double |
| P1464 | `5 / 3.0` | double | 1.6667 | 5→double |
| P1465 | `(float)5 / 3` | float | 1.6667f | cast first |
| P1466 | `5 / (float)3` | float | 1.6667f | cast divisor |
| P1467 | `(int)3.7` | int | 3 | truncation |
| P1468 | `(int)-3.7` | int | -3 | toward zero |
| P1469 | `(char)300` | char | 44 (impl-defined) | 300-256=44 |
| P1470 | `(unsigned)-1` | unsigned | 4294967295 | reinterpret bits |
| P1471 | `(int)4294967295u` | int | -1 (impl-defined) | reinterpret |
| P1472 | `'A' + 1` | int | 66 | char promoted to int |
| P1473 | `(char)('A' + 1)` | char | 'B' | explicit cast back |
| P1474 | `sizeof('A')` in C | int | 4 | char constant is int! |
| P1475 | `sizeof('A')` in C++ | char | 1 | Different in C++! |
| P1476 | `1 + 1.0f` | float | 2.0f | int→float |
| P1477 | `1.0f + 1.0` | double | 2.0 | float→double |
| P1478 | `1L + 1` | long | 2L | int→long |
| P1479 | `1LL + 1` | long long | 2LL | int→long long |
| P1480 | `1u + (-2)` | unsigned | UINT_MAX | -2→large unsigned |
| P1481 | `(int)(1u + (-2))` | int | -1 | cast back |
| P1482 | `(int)1e10` | int | UB (overflow) | 10B > INT_MAX |
| P1483 | `(long long)1e10` | long long | 10000000000 | fits in 64 bits |
| P1484 | `(float)INT_MAX` | float | ~2.147484e9 | precision loss! |
| P1485 | `(double)INT_MAX` | double | 2147483647.0 | exact |
| P1486 | `(int)(1.0/0.0)` | UB | — | infinity→int |
| P1487 | `(int)NAN` | UB | — | NaN→int |
| P1488 | `(int)INFINITY` | UB | — | infinity→int |
| P1489 | `sizeof(1 + 1.0)` | 8 | — | double size |
| P1490 | `sizeof(1 + 1.0f)` | 4 | — | float size |
| P1491 | `sizeof(1 + 1L)` | 4 or 8 | — | long size |
| P1492 | `sizeof(1 + 1LL)` | 8 | — | long long size |
| P1493 | `sizeof(1u)` | 4 | — | unsigned int |
| P1494 | `sizeof(1ULL)` | 8 | — | unsigned long long |
| P1495 | `1 ? 1 : 1.0` type | double | 1.0 | ternary: common type |
| P1496 | `0 ? 1 : 1.0` type | double | 1.0 | same rule |
| P1497 | `sizeof(0 ? 1 : 1.0)` | 8 | — | double |
| P1498 | `sizeof(1 ? 'a' : 1)` | 4 | — | char promoted to int |
| P1499 | `sizeof(void*)` vs `sizeof(int*)` | same | 8 | all pointers same size (typically) |
| P1500 | `sizeof(char*)` vs `sizeof(double*)` | same | 8 | all data pointers same |

**P1501-P1510** — Conversion edge cases:

| # | Question | Answer |
|---|---|---|
| P1501 | `(unsigned char)(char)-1` | 255 |
| P1502 | `(signed char)(unsigned char)200` | -56 (200-256) |
| P1503 | `(int)(unsigned int)3000000000u` | -1294967296 |
| P1504 | `(short)65536` | 0 (wraps) |
| P1505 | `(short)65537` | 1 |
| P1506 | `(unsigned short)-1` | 65535 |
| P1507 | `(char)(-129)` | impl-defined |
| P1508 | `(unsigned)(-1LL)` | 4294967295 (lower 32 bits) |
| P1509 | `(long long)(float)LLONG_MAX` | UB (precision loss in float) |
| P1510 | `(int)((unsigned)INT_MAX + 1u)` | INT_MIN (typically) |

---

# 📝 Practice Set 21: Final Comprehensive Quiz (P1511 – P1560)

**P1511.** Match the C standard feature:

| Feature | Standard |
|---|---|
| P1511a | VLAs | C99 |
| P1511b | `_Bool` | C99 |
| P1511c | `long long` | C99 |
| P1511d | `restrict` | C99 |
| P1511e | Designated initializers | C99 |
| P1511f | `_Generic` | C11 |
| P1511g | `_Atomic` | C11 |
| P1511h | `_Static_assert` | C11 |
| P1511i | `_Thread_local` | C11 |
| P1511j | `_Alignas` / `_Alignof` | C11 |
| P1511k | `typeof` (standard) | C23 |
| P1511l | `#embed` | C23 |
| P1511m | Digit separators | C23 |
| P1511n | `nullptr` keyword | C23 |
| P1511o | Anonymous struct/union | C11 |

**P1512-P1530** — Fill in the blanks:

| # | Statement | Answer |
|---|---|---|
| P1512 | The `___ ` segment stores string literals | rodata (read-only data) |
| P1513 | `malloc` returns a `___ ` pointer | `void *` |
| P1514 | The `___ ` operator has lazy evaluation | `&&` and `\|\|` (short-circuit) |
| P1515 | In two's complement, `-x = ___ + 1` | `~x` |
| P1516 | `a[i]` is syntactic sugar for `___ ` | `*(a + i)` |
| P1517 | EOF is typically defined as `___ ` | `-1` |
| P1518 | `sizeof` returns type `___ ` | `size_t` |
| P1519 | Pointer subtraction returns type `___ ` | `ptrdiff_t` |
| P1520 | `___ ` keyword prevents compiler optimization of reads | `volatile` |
| P1521 | `___ ` segment contains uninitialized globals | BSS |
| P1522 | Parameters are pushed `___ ` in cdecl | right-to-left |
| P1523 | `union` size equals `___ ` member's size | largest |
| P1524 | Enum values are of type `___ ` in C | `int` |
| P1525 | `fgets` stops at `___ ` or buffer limit | `\n` |
| P1526 | `strtol` is safer than `___ ` | `atoi` |
| P1527 | `___ ` functions run at program exit | `atexit`-registered functions |
| P1528 | Double free is `___ ` behavior | undefined |
| P1529 | `const` in C does NOT create `___ ` expression | constant (compile-time) |
| P1530 | Array of function pointers enables `___ ` pattern | dispatch table / vtable |

**P1531-P1560** — Error Correction (find and fix the bug):

| # | Buggy Code | Bug | Fix |
|---|---|---|---|
| P1531 | `int a[5]; for(int i=0;i<=5;i++) a[i]=i;` | Off-by-one: a[5] OOB | `i < 5` |
| P1532 | `char *s = malloc(strlen(src)); strcpy(s, src);` | Missing +1 for '\0' | `strlen(src)+1` |
| P1533 | `int *p = malloc(sizeof(int)); *p = 5; free(p); *p = 10;` | Use-after-free | Remove last `*p=10` |
| P1534 | `char *s = "hello"; s[0] = 'H';` | Modifying string literal | Use `char s[] = "hello";` |
| P1535 | `int x; printf("%d", x);` | Uninitialized variable | `int x = 0;` |
| P1536 | `int *p; *p = 5;` | Wild pointer | `int x; int *p = &x;` |
| P1537 | `if (x = 5) doSomething();` | Assignment in condition | `if (x == 5)` |
| P1538 | `scanf("%d", x);` | Missing & | `scanf("%d", &x);` |
| P1539 | `char s[5]; gets(s);` | Buffer overflow + deprecated | `fgets(s, 5, stdin);` |
| P1540 | `free(p); free(p);` | Double free | Add `p = NULL;` after first free |
| P1541 | `int (*cmp)(void*,void*); qsort(a,n,sizeof(int),cmp);` | cmp is NULL/uninitialized | Assign valid comparator |
| P1542 | `int *p = malloc(10); free(p); p = malloc(20);` | Leak if sizeof=10 bytes but needed ints | `malloc(10*sizeof(int))` |
| P1543 | `for(int i=0;i<n;i++); printf("%d",i);` | `i` out of scope after for (C99 block scope) | Remove `;` or declare `i` outside |
| P1544 | `char s[3] = "abc"; printf("%s",s);` | No null terminator (exactly 3 chars fill array) | `char s[4] = "abc";` |
| P1545 | `float f = 0.1; if(f == 0.1) ...` | float != double precision | `if(f == 0.1f)` |
| P1546 | `printf("%d\n", sizeof(int));` | `%d` for `size_t` → UB | `%zu` or cast `(int)sizeof(int)` |
| P1547 | `int a[5] = {1,2,3,4,5}; int *p = a+5; printf("%d",*p);` | Dereference one-past-end | Remove dereference |
| P1548 | `char *f(){char s[]="hello"; return s;}` | Returning local array | Use `static char s[]` or `malloc` |
| P1549 | `int n=5; int a[n] = {0};` | VLA can't have initializer in standard C | Use constant or `memset` |
| P1550 | `memcpy(a, a+2, 3*sizeof(int));` | Overlapping memory | Use `memmove` |

---

# 📝 Practice Set 22: Speed Round — 60-Second Questions (P1551 – P1600)

| # | Question (answer in < 10 seconds) | Answer |
|---|---|---|
| P1551 | `sizeof(int)` on most modern systems? | 4 |
| P1552 | `sizeof(void*)` on 64-bit? | 8 |
| P1553 | `'A' + 25`? | 'Z' (90) |
| P1554 | `'0' + 9`? | '9' (57) |
| P1555 | `7 & 3`? | 3 |
| P1556 | `7 \| 3`? | 7 |
| P1557 | `7 ^ 3`? | 4 |
| P1558 | `~0`? | -1 |
| P1559 | `1 << 10`? | 1024 |
| P1560 | `1024 >> 5`? | 32 |
| P1561 | `!0`? | 1 |
| P1562 | `!!5`? | 1 |
| P1563 | `sizeof(char[100])`? | 100 |
| P1564 | `sizeof("hello")`? | 6 |
| P1565 | `strlen("hello")`? | 5 |
| P1566 | `10 % 3`? | 1 |
| P1567 | `-10 % 3`? | -1 |
| P1568 | `(int)3.99`? | 3 |
| P1569 | `(int)-3.99`? | -3 |
| P1570 | `5 > 3 ? 10 : 20`? | 10 |
| P1571 | `5 < 3 ? 10 : 20`? | 20 |
| P1572 | `(1, 2, 3)`? | 3 |
| P1573 | `sizeof(struct{int a;})`? | 4 |
| P1574 | `sizeof(union{int a; double b;})`? | 8 |
| P1575 | `0xFF`? | 255 |
| P1576 | `0x100`? | 256 |
| P1577 | `077`? | 63 ($7 \times 8 + 7$) |
| P1578 | `'A' ^ 0x20`? | 'a' (97) |
| P1579 | `'z' & ~0x20`? | 'Z' (90) |
| P1580 | Is `void` a complete type? | No (incomplete) |
| P1581 | Default return of `main` if no return? | 0 (C99+) |
| P1582 | `INT_MAX` value? | 2147483647 |
| P1583 | $2^{10}$? | 1024 |
| P1584 | $2^{16}$? | 65536 |
| P1585 | $2^{20}$? | 1048576 (~1M) |
| P1586 | $2^{32}$? | 4294967296 (~4G) |
| P1587 | Number of values in 8 bits unsigned? | 256 |
| P1588 | Number of values in 16 bits unsigned? | 65536 |
| P1589 | Number of values in 32 bits unsigned? | $2^{32}$ = ~4.3 billion |
| P1590 | `n & 1` for n=6? | 0 (even) |
| P1591 | `n & 1` for n=7? | 1 (odd) |
| P1592 | `n & (n-1)` for n=8? | 0 (power of 2) |
| P1593 | `n & (n-1)` for n=7? | 6 (clear lowest bit: 111→110) |
| P1594 | `n & (-n)` for n=12? | 4 (lowest set bit: 1100→0100) |
| P1595 | Fibonacci(10)? | 55 |
| P1596 | 10!? | 3628800 |
| P1597 | $\log_2 1024$? | 10 |
| P1598 | $\log_2 1048576$? | 20 |
| P1599 | Sum 1 to 100? | 5050 |
| P1600 | Sum of $1^2$ to $10^2$? | 385 |

---

# 📋 Final Summary: Complete Problem Count

| Category | Problems |
|---|---|
| Output Prediction | ~500 |
| Quick Fire / Facts | ~250 |
| Recursion | ~100 |
| GATE PYQ Style | ~200 |
| Memory / Pointers | ~250 |
| True/False / Conceptual | ~150 |
| Structure Padding | ~60 |
| Preprocessor | ~70 |
| Undefined Behavior | ~100 |
| Time Complexity | ~80 |
| Type Conversions | ~60 |
| Error Correction | ~30 |
| Speed Round | ~50 |
| **Grand Total** | **~1600 problems** |

---

*🏆 End of C Programming — Comprehensive Notes for GATE 2027 (CS/IT/DA)*

*📅 Last Updated: April 2026 | Covers: GATE CS + GATE DA Syllabus | 100% Exam Coverage*
