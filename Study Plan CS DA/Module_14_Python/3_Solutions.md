# Python Programming — Solutions for GATE 2027 DA

---

## Section A: Variables, Types & Operators (Q1–Q10)

---

### Q1. Solution — Dynamic Typing

```
<class 'list'>
```

**Explanation:** Python is dynamically typed. `x` is rebound from `int` → `str` → `list`. The last assignment makes `x` a list, so `type(x)` returns `<class 'list'>`.

---

### Q2. Solution — Division Operators

```
3.5 3 1
```

**Explanation:**
- `7 / 2` = `3.5` (true division, always returns float in Python 3)
- `7 // 2` = `3` (floor division)
- `7 % 2` = `1` (modulus)

---

### Q3. Solution — Negative Floor Division

```
-4
1
-4
-3
```

**Explanation:**  
`//` floors toward −∞, and `%` satisfies `a = (a // b) * b + (a % b)`:
- `-7 // 2` = `−4` (floor of −3.5)
- `-7 % 2` = `-7 - (−4 × 2)` = `-7 + 8` = `1`
- `7 // -2` = `−4` (floor of −3.5)
- `7 % -2` = `7 - (−4 × −2)` = `7 - 8` = `−1` → Wait, let's recalculate: `7 = (-4) * (-2) + r` → `7 = 8 + r` → `r = -1`. So `7 % -2 = -1`.

Corrected output:
```
-4
1
-4
-1
```

> **Key Rule:** `a % b` has the same sign as `b` (the divisor).

---

### Q4. Solution — Operator Precedence

```
512
64
```

**Explanation:**  
`**` is right-associative:
- `2 ** 3 ** 2` = `2 ** (3 ** 2)` = `2 ** 9` = `512`
- `(2 ** 3) ** 2` = `8 ** 2` = `64`

---

### Q5. Solution — Boolean & Truthy Values

```
False False False False
True True True
```

**Explanation:**
- `0`, `""`, `[]`, `None` are all falsy → `False`
- `"0"` is a non-empty string → `True`
- `" "` is a non-empty string (contains a space) → `True`
- `[0]` is a non-empty list → `True`

---

### Q6. Solution — Short-Circuit Evaluation

```
0
```

**Explanation:** `y = 0` is falsy. In `y and (x / y)`, since `y` is falsy, Python short-circuits and returns `0` without evaluating `x / y`. No `ZeroDivisionError`.

---

### Q7. Solution — Bitwise NOT

```
-6
-1
0
```

**Explanation:** `~x = -(x + 1)` for any integer:
- `~5` = `-(5 + 1)` = `-6`
- `~0` = `-(0 + 1)` = `-1`
- `~(-1)` = `-(-1 + 1)` = `0`

---

### Q8. Solution — Chained Comparison

```
True
True
True
```

**Explanation:** Chained comparisons are equivalent to `and`-connected pairs:
- `1 < 5 < 10` → `1 < 5 and 5 < 10` → `True and True` = `True`
- `10 > 5 <= 5` → `10 > 5 and 5 <= 5` → `True and True` = `True`
- `1 < 5 > 3 < 7` → `1 < 5 and 5 > 3 and 3 < 7` → `True`

---

### Q9. Solution — `is` vs `==`

```
True
False
True
True
```

**Explanation:**
- `a == b` → `True` (same values)
- `a is b` → `False` (different objects in memory)
- `a is c` → `True` (c is an alias, same object)
- `a == c` → `True` (same values since same object)

---

### Q10. Solution — Integer Caching

```
True
False
```

**Explanation:** CPython caches integers from −5 to 256. For `256`, `a` and `b` point to the same cached object → `is` returns `True`. For `257`, new objects are created → `is` returns `False`.

> **Note:** This is implementation-dependent. In some contexts (e.g., same line in REPL), both may be `True` due to compile-time optimizations.

---

## Section B: Strings (Q11–Q18)

---

### Q11. Solution — String Slicing

```
GATE
2027
GT22
7202ETAG
```

**Explanation:**
- `s[0:4]` → characters at index 0,1,2,3 → `"GATE"`
- `s[-4:]` → last 4 characters → `"2027"`
- `s[::2]` → every other character → `"GT22"` (indices 0,2,4,6 → G,T,2,2)

Wait, let me recount: `s = "GATE2027"`, indices: G(0), A(1), T(2), E(3), 2(4), 0(5), 2(6), 7(7).
- `s[::2]` → indices 0,2,4,6 → `"GT22"`
- `s[::-1]` → `"7202ETAG"`

---

### Q12. Solution — String Immutability

```
TypeError: 'str' object does not support item assignment
```

**Explanation:** Strings are immutable in Python. You cannot change individual characters. Use `s = 'H' + s[1:]` to create a new string.

---

### Q13. Solution — String Methods

```
Hello, World!
['  Hello', ' World!  ']
GATE
2
heLlo
```

**Explanation:**
- `strip()` removes leading/trailing whitespace → `"Hello, World!"`
- `split(",")` splits on comma → `['  Hello', ' World!  ']` (whitespace preserved)
- `"GATE".lower()` → `"gate"`, then `.upper()` → `"GATE"`
- `"abcabc".count("ab")` → finds `"ab"` at index 0 and 3 → `2`
- `"hello".replace("l", "L", 1)` → replaces only 1st occurrence → `"heLlo"`

---

### Q14. Solution — String join

```
GATE-2027-DA
<class 'str'>
```

**Explanation:** `join()` concatenates list elements with the separator `"-"`.

---

### Q15. Solution — String find vs index

```
5
-1
```

**Explanation:**
- `s.find("2027")` → found at index 5
- `s.find("2028")` → not found, returns `-1`
- `s.index("2028")` would raise `ValueError: substring not found`

---

### Q16. Solution — String Multiplication

```
ababab
6
```

**Explanation:** `"ab" * 3` repeats the string 3 times → `"ababab"`, length 6.

---

### Q17. Solution — Slice Out of Range

```

ATE
```

**Explanation:**
- `s[10:20]` → start index beyond length, returns empty string `""` (no error)
- `s[1:100]` → stop index beyond length, returns from index 1 to end → `"ATE"`
- `s[10]` would raise `IndexError: string index out of range`

> **Key:** Slicing is forgiving (no error for out-of-range), indexing is not.

---

### Q18. Solution — String Formatting

```
     Alice
85.7
00001010
```

**Explanation:**
- `f"{name:>10}"` → right-align in 10 characters → `"     Alice"`
- `f"{score:.1f}"` → float with 1 decimal place → `"85.7"`
- `f"{10:08b}"` → binary, zero-padded to 8 digits → `"00001010"`

---

## Section C: Lists (Q19–Q28)

---

### Q19. Solution — `sort()` Return Value

```
None
[1, 1, 3, 4, 5]
```

**Explanation:** `sort()` sorts in place and returns `None`. The list itself is now sorted.

---

### Q20. Solution — `append` vs `extend`

```
[1, 2, [3, 4]] 3
[1, 2, 3, 4] 4
```

**Explanation:**
- `append([3, 4])` adds the entire list as a single element → length 3
- `extend([3, 4])` adds each element individually → length 4

---

### Q21. Solution — List Aliasing

```
[1, 2, 3, 4]
True
```

**Explanation:** `b = a` makes `b` an alias (same object). Modifying `b` also modifies `a`. `a is b` → `True`.

---

### Q22. Solution — Shallow Copy Trap

```
[[1, 2, 99], [3, 4]]
[[1, 2, 99], [3, 4], [5, 6]]
```

**Explanation:**
- `b = a.copy()` is a shallow copy: `b` is a new list, but its elements (inner lists) are shared.
- `b[0].append(99)` modifies the shared inner list, so `a[0]` also gets `99`.
- `b.append([5, 6])` adds to `b`'s outer list only; `a` doesn't see it (outer lists are independent).

---

### Q23. Solution — List Comprehension

```
[1, 9, 25]
```

**Explanation:** Squares of odd numbers in range(6): 1²=1, 3²=9, 5²=25.

---

### Q24. Solution — Nested List Comprehension

```
[2, 4, 6, 8]
```

**Explanation:** Flattens the matrix and filters even numbers: 2, 4, 6, 8.

---

### Q25. Solution — List Slice Assignment

```
[1, 20, 30, 40, 4, 5]
6
```

**Explanation:** `lst[1:3]` (elements at index 1 and 2, i.e., `[2, 3]`) is replaced by `[20, 30, 40]`. The list grows from 5 to 6 elements because we replaced 2 elements with 3.

---

### Q26. Solution — `pop()` Behavior

```
40 10
[20, 30]
```

**Explanation:**
- `lst.pop()` removes and returns last element → `40`
- `lst.pop(0)` removes and returns first element → `10`
- Remaining: `[20, 30]`

---

### Q27. Solution — List Multiplication Trap

```
[[0, 1], [0, 1], [0, 1]]
```

**Explanation:** `[[0]] * 3` creates 3 references to the **same** inner list. Appending to `a[0]` affects all three because they're the same object.

---

### Q28. Solution — `sorted()` with key

```
['date', 'apple', 'banana', 'cherry']
```

**Explanation:** Sorted by length: `date(4)`, `apple(5)`, `banana(6)`, `cherry(6)`. `sorted()` is stable, so `banana` comes before `cherry` (original order preserved for equal lengths).

---

## Section D: Loops & Iteration (Q29–Q33)

---

### Q29. Solution — Range Function

```
[0, 1, 2, 3, 4]
[2, 5, 8]
[10, 8, 6, 4, 2]
[]
```

**Explanation:**
- `range(5)` → 0 to 4
- `range(2, 10, 3)` → 2, 5, 8
- `range(10, 0, -2)` → 10, 8, 6, 4, 2
- `range(5, 2)` → empty (default step is +1, and 5 > 2)

---

### Q30. Solution — Loop with `else` (break triggered)

```
Done
```

**Explanation:** The loop breaks when `i == 3`. The `else` block does **not** execute because the loop exited via `break`. Only `"Done"` is printed.

---

### Q31. Solution — Loop with `else` (no break)

```
Completed
Done
```

**Explanation:** The condition `i == 10` is never true, so `break` is never executed. The `else` block runs after the loop completes normally.

---

### Q32. Solution — Nested Loop Output

```
*
**
***
```

**Explanation:**
- `i=0`: inner loop runs 1 time → `*`
- `i=1`: inner loop runs 2 times → `**`
- `i=2`: inner loop runs 3 times → `***`

---

### Q33. Solution — Enumerate & Zip

```
0: Alice - 85
1: Bob - 92
```

**Explanation:** `zip` truncates to the shorter list (2 elements). `enumerate` adds index starting from 0. Only 2 pairs are formed.

---

## Section E: Functions & Scope (Q34–Q42)

---

### Q34. Solution — LEGB Scope

```
20
10
```

**Explanation:**
- Inside `g()`, Python finds `x = 20` in the enclosing scope (`f`) → prints `20`
- The global `x` remains `10` → prints `10`

---

### Q35. Solution — UnboundLocalError

```
UnboundLocalError: local variable 'x' referenced before assignment
```

**Explanation:** The assignment `x = 20` at the bottom of `f()` makes `x` local for the **entire** function. When `print(x)` executes first, `x` hasn't been assigned yet locally → `UnboundLocalError`.

---

### Q36. Solution — `global` Keyword

```
11
11
```

**Explanation:** `global x` makes `x` refer to the global variable. `x = x + 10` → `x = 1 + 10 = 11`. The function returns `11`, and the global `x` is now `11`.

---

### Q37. Solution — Mutable Default Argument

```
[1]
[1, 2]
[1, 2, 3]
```

**Explanation:** The default list `[]` is created **once** at function definition. Each call appends to the **same** list. This is one of the most common Python traps in GATE.

---

### Q38. Solution — Pass by Object Reference

```
[1, 2, 3, 4]
```

**Explanation:**
- `lst.append(4)` → modifies the original list → `a` becomes `[1, 2, 3, 4]`
- `lst = [10, 20, 30]` → rebinds the local parameter `lst` to a new list; does NOT affect `a`
- `lst.append(40)` → modifies the new local list, not `a`

The caller only sees the `append(4)` change.

---

### Q39. Solution — `*args` and `**kwargs`

```
(1, 2, 3)
{'x': 10, 'y': 20}
```

**Explanation:** Positional arguments go into `args` as a tuple. Keyword arguments go into `kwargs` as a dict.

---

### Q40. Solution — Lambda with map

```
[1, 4, 3, 16, 5]
```

**Explanation:** The lambda squares even numbers and leaves odd numbers unchanged:
- 1 (odd) → 1
- 2 (even) → 4
- 3 (odd) → 3
- 4 (even) → 16
- 5 (odd) → 5

---

### Q41. Solution — Lambda with filter

```
[3, 6, 9]
```

**Explanation:** `filter` keeps elements where `x % 3 == 0`: 3, 6, 9.

---

### Q42. Solution — Multiple Return & Unpacking

```
(1, 2, 3)
<class 'tuple'>
1 2 3
1 [2, 3]
```

**Explanation:**
- `return 1, 2, 3` returns a tuple `(1, 2, 3)`
- `a = f()` → `a` is the tuple `(1, 2, 3)`
- `b, c, d = f()` → unpacks into three variables
- `e, *rest = f()` → `e = 1`, `rest = [2, 3]` (rest is a **list**, not tuple)

---

## Section F: Tuples, Dicts & Sets (Q43–Q48)

---

### Q43. Solution — Tuple Trap

```
<class 'int'>
<class 'tuple'>
<class 'tuple'>
```

**Explanation:**
- `(1)` is just `1` with parentheses → `int`
- `(1,)` has a trailing comma → `tuple`
- `()` is an empty tuple → `tuple`

---

### Q44. Solution — Tuple with Mutable Element

```
([1, 2, 5], [3, 4])
```

**Explanation:** The tuple itself is immutable (can't reassign elements), but the list inside is mutable. `t[0].append(5)` modifies the list in-place — allowed! `t[0] = [10, 20]` would raise `TypeError` because you can't reassign a tuple element.

---

### Q45. Solution — Dictionary Operations

```
{'a': 10, 'c': 3, 'd': 4}
True
False
```

**Explanation:**
- `d["d"] = 4` → adds key `"d"`
- `d["a"] = 10` → updates value for key `"a"`
- `del d["b"]` → removes key `"b"`
- `"c" in d` → checks keys → `True`
- `3 in d` → checks keys (3 is not a key) → `False`

---

### Q46. Solution — Set Operations

```
{1, 2, 3, 4, 5, 6}
{3, 4}
{1, 2}
{1, 2, 5, 6}
```

**Explanation:**
- `|` → union
- `&` → intersection
- `-` → difference (in A but not B)
- `^` → symmetric difference (in A or B but not both)

> **Note:** Set output order may vary as sets are unordered, but the elements will be the same.

---

### Q47. Solution — Empty Set vs Dict

```
<class 'dict'>
<class 'set'>
```

**Explanation:** `{}` creates an empty **dict**, not a set. Use `set()` for an empty set.

---

### Q48. Solution — Dict Comprehension & Zip

```
{'a': 1, 'b': 4, 'c': 9}
```

**Explanation:** `zip(keys, values)` pairs up `('a',1), ('b',2), ('c',3)`. The comprehension squares each value: `{'a': 1, 'b': 4, 'c': 9}`.

---

## Section G: OOP (Q49–Q53)

---

### Q49. Solution — Class vs Instance Variables

```
[1, 2]
[1, 2]
True
```

**Explanation:** `data` is a class variable (mutable list). Both `a.data` and `b.data` point to the same list. Calling `.append()` on either modifies the shared list. `a.data is b.data` → `True`.

---

### Q50. Solution — Class Variable Shadowing

```
20
30
```

**Explanation:**
- `a.x = 20` creates a new **instance variable** for `a` that shadows the class variable
- `MyClass.x = 30` changes the class variable
- `a.x` → `20` (instance variable takes priority)
- `b.x` → `30` (no instance variable, falls back to class variable)

---

### Q51. Solution — Inheritance & Method Overriding

```
Child
True
```

**Explanation:** `GrandChild` doesn't define `show()`, so Python looks up the MRO: `GrandChild → Child → Parent`. Finds `show()` in `Child` → prints `"Child"`. `isinstance(g, Parent)` → `True` because `GrandChild` inherits from `Child` which inherits from `Parent`.

---

### Q52. Solution — Multiple Inheritance MRO

```
D B C A
```

**Explanation:** MRO for `D` is: `D → B → C → A → object`.
- `D.f()` prints `"D "`, then calls `super().f()` → `B.f()`
- `B.f()` prints `"B "`, then calls `super().f()` → `C.f()` (not A! follows MRO)
- `C.f()` prints `"C "`, then calls `super().f()` → `A.f()`
- `A.f()` prints `"A "`

Result: `D B C A`

> **Key:** `super()` follows the MRO, not just the direct parent.

---

### Q53. Solution — Operator Overloading

```
(4, 6)
<class '__main__.Point'>
```

**Explanation:** `p1 + p2` invokes `p1.__add__(p2)`, which returns `Point(4, 6)`. `print(p3)` calls `__str__` → `"(4, 6)"`. The type is `Point`.

---

## Section H: Recursion (Q54–Q58)

---

### Q54. Solution — Factorial Trace

```
120
```

**Trace:**
```
fact(5) = 5 * fact(4)
       = 5 * 4 * fact(3)
       = 5 * 4 * 3 * fact(2)
       = 5 * 4 * 3 * 2 * fact(1)
       = 5 * 4 * 3 * 2 * 1 * fact(0)
       = 5 * 4 * 3 * 2 * 1 * 1
       = 120
```

---

### Q55. Solution — Fibonacci

```
13
```

**Explanation:** `fib(7)`:
```
fib(0)=0, fib(1)=1, fib(2)=1, fib(3)=2, fib(4)=3, fib(5)=5, fib(6)=8, fib(7)=13
```

---

### Q56. Solution — Tricky Recursion

```
30
```

**Trace:**
```
f(4) = f(3) + 16
f(3) = f(2) + 9
f(2) = f(1) + 4
f(1) = f(0) + 1
f(0) = 0

f(1) = 0 + 1 = 1
f(2) = 1 + 4 = 5
f(3) = 5 + 9 = 14
f(4) = 14 + 16 = 30
```

This computes 1² + 2² + 3² + 4² = 1 + 4 + 9 + 16 = 30.

---

### Q57. Solution — Recursion with List

```
16
```

**Trace:**
```
mystery([1,2,3,4,5,6,7])
= 1 + mystery([3,4,5,6,7])
= 1 + 3 + mystery([5,6,7])
= 1 + 3 + 5 + mystery([7])
= 1 + 3 + 5 + 7
= 16
```

The function picks every other element starting from index 0 (skips `lst[1]` by using `lst[2:]`): 1, 3, 5, 7.

---

### Q58. Solution — Recursive String

```
ETAG
```

**Trace:**
```
f("GATE") = f("ATE") + "G"
f("ATE")  = f("TE") + "A"
f("TE")   = f("E") + "T"
f("E")    = "E"  (base case)

Unwinding:
f("TE")   = "E" + "T" = "ET"
f("ATE")  = "ET" + "A" = "ETA"
f("GATE") = "ETA" + "G" = "ETAG"
```

The function reverses the string.

---

## Section I: Mixed / Comprehensive (Q59–Q65)

---

### Q59. Solution — Global + Mutable

```
[1, 2, 3, 4]
```

**Explanation:**
- `f()` calls `data.append(4)` → modifies the global list in-place (no `global` keyword needed for mutation)
- `g()` does `data = [10, 20]` → creates a **local** variable `data` (shadows global, doesn't affect it)
- Global `data` is `[1, 2, 3, 4]`

> **Key:** You need `global` keyword only for **reassignment**, not for **mutation** of global mutable objects.

---

### Q60. Solution — Nested Function & Closure

```
15
20
```

**Explanation:**
- `outer()` returns the `inner` function, which closes over `x = 10`
- First `fn()` call: `x` goes from 10 to 15, returns `15`
- Second `fn()` call: `x` goes from 15 to 20, returns `20`
- `nonlocal x` allows `inner` to modify `x` in the enclosing scope, and the value persists across calls.

---

### Q61. Solution — List as Default + Aliasing

```
[0, 1, 4, 0, 1]
[0, 1, 4, 0, 1]
True
```

**Explanation:**
- First call `f(3)`: `lst = []` (default), appends 0, 1, 4 → `lst = [0, 1, 4]`
- Second call `f(2)`: same default list (now `[0, 1, 4]`), appends 0, 1 → `lst = [0, 1, 4, 0, 1]`
- Both `a` and `b` point to the same default list object → `a is b` → `True`

---

### Q62. Solution — Reduce

```
120
```

**Explanation:** `reduce(lambda a, b: a * b, [1, 2, 3, 4, 5])`:
```
1 * 2 = 2
2 * 3 = 6
6 * 4 = 24
24 * 5 = 120
```

---

### Q63. Solution — Deep Copy vs Shallow

```
99
1
```

**Explanation:**
- `b = copy.copy(a)` → shallow copy. Inner lists are shared. When `a[0][0] = 99`, `b[0][0]` also becomes `99`.
- `c = copy.deepcopy(a)` → deep copy. Fully independent. `c[0][0]` remains `1`.

---

### Q64. Solution — Extended Unpacking

```
1
[2, 3, 4]
5
<class 'list'>
```

**Explanation:** `a, *b, c = [1, 2, 3, 4, 5]`:
- `a` gets the first element → `1`
- `c` gets the last element → `5`
- `*b` gets everything in between → `[2, 3, 4]` (always a **list**)

---

### Q65. Solution — Linked List Traversal

```
10
```

**Explanation:** The linked list is `1 → 2 → 3 → 4 → None`. The while loop traverses all nodes, summing: `1 + 2 + 3 + 4 = 10`.

---

*End of Python Solutions for GATE 2027 DA*

---

## Section J Solutions: File I/O & Exception Handling (Q66–Q75)

---

### Q66. Solution — File Write & Read

```
HelloWorld
```

**Explanation:** `write()` does NOT add newlines. Two `write()` calls produce `"HelloWorld"` concatenated.

---

### Q67. Solution — finally Overrides return

```
3
```

**Explanation:** `finally` block ALWAYS executes. When `finally` has a `return`, it overrides whatever `try` or `except` would have returned. The `return 1` in `try` is discarded.

---

### Q68. Solution — Exception Handling Flow

```
VE: zero
DONE
-1
```

**Explanation:** `x == 0` → `raise ValueError("zero")` → caught by `except ValueError` → prints `"VE: zero"` → `return -1` is queued. But `finally` runs FIRST → prints `"DONE"`. Then `-1` is returned. The `else` block does NOT run because an exception occurred.

---

### Q69. Solution — Exception Hierarchy

```
Caught!
```

**Explanation:** `KeyError` is a subclass of `LookupError` (which is a subclass of `Exception`). Python's `except` catches the specified class and all its subclasses.

---

### Q70. Solution — Custom Exception

```
MyError Bad: 42
```

**Explanation:** `MyError` inherits from `ValueError`. `except ValueError` catches `MyError` because it's a subclass. `type(e).__name__` gives `"MyError"`, and `str(e)` gives `"Bad: 42"`.

---

### Q71. Solution — File Modes

```
ABCDEF
```

**Explanation:** `"w"` mode creates/truncates → writes `"ABC"`. `"a"` mode appends → file becomes `"ABCDEF"`. Reading gives `"ABCDEF"`.

---

### Q72. Solution — readlines vs readline

```
'Line1\n'
['Line2\n', 'Line3']
```

**Explanation:** `readline()` reads ONE line including `\n` → `'Line1\n'`. The file cursor is now at `Line2`. `readlines()` reads remaining lines → `['Line2\n', 'Line3']`. Note: last line has no `\n`.

---

### Q73. Solution — Nested try/except

```
inner
outer: converted
final
```

**Explanation:** Inner try catches `ZeroDivisionError`, prints `"inner"`, then raises `ValueError`. This propagates to outer try, caught by `except ValueError`, prints `"outer: converted"`. The outermost `finally` prints `"final"`.

---

### Q74. Solution — except Order Matters

```
A
```

**Explanation:** `Exception` is the parent of `ValueError`. Python checks `except` clauses top-to-bottom, and `except Exception` matches first. The `except ValueError` is unreachable. (Python 3 actually raises a `SyntaxError` warning about this in some cases, but it prints `"A"`).

---

### Q75. Solution — writelines Behavior

```
HelloWorld!
```

**Explanation:** `writelines()` writes each string WITHOUT adding separators or newlines. The three strings are concatenated: `"HelloWorld!"`.

---

## Section K Solutions: Iterators, Generators & Decorators (Q76–Q85)

---

### Q76. Solution — Iterator Exhaustion

```
6
0
```

**Explanation:** `iter(nums)` creates an iterator. First `sum(it)` consumes all elements: `1+2+3 = 6`. Second `sum(it)` finds an exhausted iterator → `sum` of empty → `0`.

---

### Q77. Solution — Generator Execution

```
A
1
B
2
```

**Explanation:** `gen()` returns generator but doesn't execute body. `next(g)` runs until first `yield` → prints `"A"`, yields `1`. Second `next(g)` resumes after first `yield` → prints `"B"`, yields `2`.

---

### Q78. Solution — Generator Expression vs List Comprehension

```
[16, 25] 41
```

**Explanation:** `a` is a list `[4**2, 5**2] = [16, 25]`. `b` uses generator expression passed directly to `sum()`: `16 + 25 = 41`.

---

### Q79. Solution — Generator with return

```
1
done
```

**Explanation:** `next(g)` yields `1`. Second `next(g)` hits `return "done"` which raises `StopIteration` with `value="done"`. The `yield 2` after `return` is unreachable. `e.value` captures the return value.

---

### Q80. Solution — Decorator Execution Order

```
D1
D2
Hello
```

**Explanation:** `@dec1 @dec2 def hello` means `hello = dec1(dec2(hello))`. Calling `hello()` calls `dec1`'s wrapper → prints `"D1"` → calls `dec2`'s wrapper → prints `"D2"` → calls original `hello()` → prints `"Hello"`. Decorators apply bottom-up, but execute top-down.

---

### Q81. Solution — @staticmethod vs @classmethod

```
10 10
10 20
```

**Explanation:** `A.f()` → `A.x = 10`. `A.g()` → `cls=A`, `cls.x = 10`. `B.f()` → still `A.x = 10` (hardcoded). `B.g()` → `cls=B`, `cls.x = 20`. Key difference: `@classmethod` respects inheritance through `cls`.

---

### Q82. Solution — Closure Variable Binding Trap

```
[2, 2, 2]
```

**Explanation:** Classic closure trap! All three lambdas capture the **variable** `i`, not its value. After the loop, `i = 2`, so all lambdas return `2`. Fix: `lambda i=i: i` (default argument captures value at creation time).

---

### Q83. Solution — @property

```
212.0
Read-only!
```

**Explanation:** `Temp(100).fahrenheit` calls the getter: `100 * 9/5 + 32 = 212.0`. Attempting to set `fahrenheit` raises `AttributeError` because no `@fahrenheit.setter` is defined.

---

### Q84. Solution — functools.wraps Effect

```
add
Adds two numbers
```

**Explanation:** `@wraps(func)` copies `__name__`, `__doc__`, and other attributes from the original function to the wrapper. Without `@wraps`, output would be `wrapper` and `None`.

---

### Q85. Solution — Generator send()

```
0
10
30
35
```

**Explanation:** `next(gen)` → runs to first `yield total`, `total=0`, yields `0`. `send(10)` → `x = 10`, `total = 0+10=10`, yields `10`. `send(20)` → `x = 20`, `total = 10+20=30`, yields `30`. `send(5)` → `x = 5`, `total = 30+5=35`, yields `35`.

---

## Section L Solutions: Regex, Modules & Advanced (Q86–Q95)

---

### Q86. Solution — re.findall

```
['123', '456']
```

**Explanation:** `\d+` matches one or more digits. `findall` returns all non-overlapping matches as a list of strings.

---

### Q87. Solution — re.match vs re.search

```
None
42
```

**Explanation:** `re.match` checks only at the START of the string — `"Hello"` doesn't start with digits → `None`. `re.search` finds first match ANYWHERE → finds `42`.

---

### Q88. Solution — Regex Groups

```
15/08/1947
15 08 1947
('15', '08', '1947')
```

**Explanation:** `group(0)` = entire match. `group(1)`, `group(2)`, `group(3)` = individual capture groups. `groups()` returns tuple of all groups.

---

### Q89. Solution — re.sub

```
H*ll* W*rld
```

**Explanation:** `[aeiou]` with `re.I` flag matches vowels case-insensitively. Each vowel (e, o, o) is replaced by `*`. `H*ll* W*rld`.

---

### Q90. Solution — Counter

```
[('i', 4), ('s', 4)]
4
0
```

**Explanation:** `Counter("mississippi")` → `{'i':4, 's':4, 'p':2, 'm':1}`. `most_common(2)` returns 2 most frequent. `c['s'] = 4`. `c['z'] = 0` (Counter returns 0 for missing keys, not KeyError).

---

### Q91. Solution — defaultdict

```
{'a': [1, 2], 'b': [3]}
[]
```

**Explanation:** `defaultdict(list)` creates empty list for missing keys. `d['a']` gets `[1, 2]` after two appends. `d['c']` doesn't exist → default factory creates `[]` and returns it.

---

### Q92. Solution — __name__ == "__main__"

```
Helper loaded
Hi
```

**Explanation:** When `main.py` imports `helper`, Python executes `helper.py`. The top-level `print("Helper loaded")` runs. But `__name__` in `helper` is `"helper"` (not `"__main__"`), so the `if` block is skipped. Then `helper.greet()` returns `"Hi"`.

---

### Q93. Solution — itertools.product

```
8
(0, 0, 0) (1, 1, 1)
```

**Explanation:** `product([0,1], repeat=3)` generates all 3-element tuples from `{0,1}` = $2^3 = 8$ tuples. First is `(0,0,0)`, last is `(1,1,1)`.

---

### Q94. Solution — Walrus Operator

```
[21, 24, 27]
```

**Explanation:** For each `x`: `y := x*3`, then keep if `y > 10`. Values: 9(no), 21(yes), 6(no), 24(yes), 3(no), 27(yes). Result: `[21, 24, 27]`.

---

### Q95. Solution — Type Hints Runtime Behavior

```
hello world
<class 'str'>
```

**Explanation:** Type hints are NOT enforced at runtime. Despite `a: int, b: int`, Python happily concatenates two strings. `"hello" + " world" = "hello world"`, type is `str`.

---

*End of Python Solutions for GATE 2027 DA — 95 Solutions*
