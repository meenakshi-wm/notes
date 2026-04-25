# Python Programming — Revision Notes for GATE 2027 DA

---

## 1. Data Types & Mutability

| Type | Mutable? | Ordered? | Duplicates? | Hashable? | Example |
|---|---|---|---|---|---|
| `int` | No | — | — | Yes | `42` |
| `float` | No | — | — | Yes | `3.14` |
| `bool` | No | — | — | Yes | `True` |
| `str` | No | Yes | Yes | Yes | `"GATE"` |
| `tuple` | No | Yes | Yes | Yes* | `(1, 2, 3)` |
| `frozenset` | No | No | No | Yes | `frozenset({1,2})` |
| `list` | Yes | Yes | Yes | No | `[1, 2, 3]` |
| `dict` | Yes | Insertion (3.7+) | Keys: No | No | `{"a": 1}` |
| `set` | Yes | No | No | No | `{1, 2, 3}` |

\* Tuples are hashable only if all elements are hashable.

---

## 2. Operators Quick Reference

| Operator | Notes |
|---|---|
| `/` | **Always float** — `7/2` = `3.5` |
| `//` | Floor toward −∞ — `-7//2` = `-4` |
| `%` | Sign of **divisor** — `-7%3` = `2` |
| `**` | **Right-associative** — `2**3**2` = `512` |
| `~x` | `-(x+1)` |
| `is` | Identity (same object) |
| `==` | Equality (same value) |

**Precedence (high → low):** `**` → `~,+,-` (unary) → `*,/,//,%` → `+,-` → `<<,>>` → `&` → `^` → `|` → comparisons → `not` → `and` → `or`

**Truthy/Falsy:**
- **Falsy:** `0`, `0.0`, `""`, `[]`, `()`, `{}`, `set()`, `None`, `False`
- **Everything else is Truthy** (including `"0"`, `[0]`, `" "`)

---

## 3. Strings

```python
s = "GATE2027"
s[0:4]      # "GATE"       (start:stop — stop exclusive)
s[-4:]      # "2027"       (last 4)
s[::2]      # "GT22"       (every other)
s[::-1]     # "7202ETAG"   (reverse)
```

| Method | Returns | Notes |
|---|---|---|
| `upper()` / `lower()` | New str | Case conversion |
| `strip()` | New str | Remove whitespace |
| `split(sep)` | List of str | Split by separator |
| `join(iterable)` | Str | `",".join(["a","b"])` → `"a,b"` |
| `replace(old, new)` | New str | Replace occurrences |
| `find(sub)` | Int (−1 if not found) | Safe search |
| `index(sub)` | Int (raises ValueError) | Strict search |
| `count(sub)` | Int | Count occurrences |
| `startswith()` / `endswith()` | Bool | Check prefix/suffix |
| `isdigit()` / `isalpha()` | Bool | Content checks |

> **Strings are immutable.** `s[0] = 'x'` → `TypeError`. All methods return new strings.

---

## 4. Lists

```python
lst = [1, 2, 3]
lst[1:3]         # [2, 3]
lst[::-1]        # [3, 2, 1]
lst[1:2] = [9,8] # [1, 9, 8, 3]  (slice assignment)
```

| Method | Returns | In-place? | Notes |
|---|---|---|---|
| `append(x)` | `None` | Yes | Adds x as single element |
| `extend(iter)` | `None` | Yes | Adds each element |
| `insert(i, x)` | `None` | Yes | Insert at index i |
| `remove(x)` | `None` | Yes | Removes first x |
| `pop(i=-1)` | Removed item | Yes | Remove & return |
| `sort()` | `None` | Yes | Sort in-place |
| `reverse()` | `None` | Yes | Reverse in-place |
| `index(x)` | Int | No | First index of x |
| `count(x)` | Int | No | Count x |
| `copy()` | New list | No | Shallow copy |

> `sorted(lst)` returns **new** list. `lst.sort()` returns **None**.

**append vs extend:**
```python
a = [1, 2]; a.append([3, 4])   # [1, 2, [3, 4]]
b = [1, 2]; b.extend([3, 4])   # [1, 2, 3, 4]
```

---

## 5. Tuples

```python
t = (1, 2, 3)
t2 = (1,)      # Single element — comma required!
t3 = (1)       # This is int 1, NOT a tuple

# Methods: only count() and index()
# Immutable — can be dict key (if elements are hashable)
# Unpacking: a, *b, c = (1, 2, 3, 4, 5)  → a=1, b=[2,3,4], c=5
```

---

## 6. Dictionaries

```python
d = {"a": 1, "b": 2}
d["c"] = 3          # Insert
d["a"] = 10         # Update
del d["b"]          # Delete
d.get("z", 0)       # 0 (default if missing)
```

| Method | Returns |
|---|---|
| `keys()` | dict_keys view |
| `values()` | dict_values view |
| `items()` | dict_items view |
| `get(k, default)` | Value or default |
| `pop(k)` | Value (removes key) |
| `update(d2)` | None (merges) |
| `setdefault(k, v)` | Value (sets if missing) |

> `in` checks **keys**, not values. Keys must be **hashable** (immutable).

---

## 7. Sets

```python
s = {1, 2, 3}
empty = set()     # NOT {} (that's a dict!)
```

| Operation | Operator | Method |
|---|---|---|
| Union | `A \| B` | `A.union(B)` |
| Intersection | `A & B` | `A.intersection(B)` |
| Difference | `A - B` | `A.difference(B)` |
| Symmetric Diff | `A ^ B` | `A.symmetric_difference(B)` |

| Method | Notes |
|---|---|
| `add(x)` | Add element |
| `remove(x)` | Remove (KeyError if missing) |
| `discard(x)` | Remove (no error if missing) |
| `pop()` | Remove arbitrary element |

---

## 8. Loops

```python
for i in range(5):        # 0, 1, 2, 3, 4
for i in range(2, 8, 2):  # 2, 4, 6
```

**`range(start, stop, step)`** — stop is **exclusive**.

**`else` with loop:** Runs if loop finishes **without** `break`.

**List Comprehension:** `[expr for x in iter if cond]`
**Dict Comprehension:** `{k: v for k, v in iter}`
**Set Comprehension:** `{expr for x in iter}`

---

## 9. Functions & Scope

**LEGB Rule:** Local → Enclosing → Global → Built-in

```python
# global x      — access/modify global variable
# nonlocal x    — access/modify enclosing variable
```

**Pass by Object Reference:**
- Mutable arg + mutation (`.append()`) → caller sees change
- Mutable arg + reassignment (`lst = [...]`) → caller does NOT see change
- Immutable arg → caller never sees change

**Mutable Default Argument Trap:**
```python
def f(lst=[]):  # DEFAULT LIST IS SHARED ACROSS CALLS!
    lst.append(1)
    return lst
# Fix: use lst=None, then lst = [] if lst is None
```

**`*args`** → tuple of positional args  
**`**kwargs`** → dict of keyword args

---

## 10. Lambda & Functional Tools

```python
lambda x: x ** 2                    # Anonymous function
list(map(func, iterable))           # Apply func to each element
list(filter(func, iterable))        # Keep where func returns True
reduce(func, iterable)              # Cumulative operation (from functools)
sorted(lst, key=lambda x: x[1])    # Sort by custom key
```

---

## 11. Copying

| Method | Type | Nested objects shared? |
|---|---|---|
| `b = a` | Alias | Yes (same object!) |
| `a[:]`, `list(a)`, `a.copy()`, `copy.copy(a)` | Shallow | Yes |
| `copy.deepcopy(a)` | Deep | No (fully independent) |

---

## 12. OOP Quick Reference

```python
class MyClass:
    class_var = 0              # Shared by all instances
    
    def __init__(self, x):     # Constructor
        self.x = x             # Instance variable
    
    def method(self):          # Instance method
        return self.x

class Child(Parent):           # Inheritance
    def __init__(self, x, y):
        super().__init__(x)    # Call parent constructor
        self.y = y
```

**Key Dunder Methods:**

| Method | Triggered by |
|---|---|
| `__init__` | `ClassName()` |
| `__str__` | `print()`, `str()` |
| `__repr__` | `repr()`, shell |
| `__add__` | `+` |
| `__eq__` | `==` |
| `__len__` | `len()` |
| `__getitem__` | `obj[i]` |

**MRO:** `ClassName.__mro__` — C3 Linearization for multiple inheritance.

**Class vs Instance Variable Trap:**
- `.append()` on class var → mutates shared object (all instances affected)
- `self.x = ...` → creates new instance variable (shadows class var)

---

## 13. Recursion Patterns

```python
# Factorial: n! = n × (n-1)!,  base: 0! = 1
# Fibonacci: fib(n) = fib(n-1) + fib(n-2),  base: fib(0)=0, fib(1)=1
# Sum: sum(lst) = lst[0] + sum(lst[1:]),  base: empty list → 0
# GCD: gcd(a, b) = gcd(b, a%b),  base: b=0 → a
# Power: pow(b, e) = b × pow(b, e-1),  base: e=0 → 1
# Hanoi: 2^n - 1 moves for n disks
```

**Recursion depth:** Default 1000 (`sys.getrecursionlimit()`).

---

## 14. Linked List Template

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Traversal: while current: ... current = current.next
# Insert at head: new.next = head; head = new
# Delete: prev.next = current.next
```

---

## 15. Top GATE Traps — Quick Checklist

| # | Trap | Remember |
|---|---|---|
| 1 | `/` always float | `7/2` → `3.5` |
| 2 | `//` floors to −∞ | `-7//2` → `-4` |
| 3 | `**` right-assoc | `2**3**2` → `512` |
| 4 | `"0"` is Truthy | Non-empty string! |
| 5 | `(1)` is int | `(1,)` is tuple |
| 6 | `{}` is dict | `set()` for empty set |
| 7 | `sort()` → `None` | Use `sorted()` for new list |
| 8 | `append` vs `extend` | append nests, extend flattens |
| 9 | Shallow copy danger | Nested mutables are shared |
| 10 | Mutable default args | Evaluated once at def time |
| 11 | `is` vs `==` | Identity vs equality |
| 12 | `UnboundLocalError` | Any assignment → local for entire function |
| 13 | `[[0]]*3` trap | All elements are same object |
| 14 | `in` on dict | Checks keys, not values |
| 15 | Slice vs index OOB | Slicing: no error; Indexing: error |
| 16 | Integer cache | `−5` to `256` cached (CPython) |
| 17 | Class var mutation | `.append()` affects all instances |
| 18 | Tuple inner mutation | `([1,2],)` — inner list is mutable |
| 19 | Global mutation | No `global` needed for `.append()` on global list |
| 20 | `range` stop excl | `range(5)` → 0,1,2,3,4 |

---

## 16. Built-in Functions Cheat Sheet

| Function | Purpose | Example |
|---|---|---|
| `len(x)` | Length | `len([1,2,3])` → `3` |
| `type(x)` | Type | `type(42)` → `<class 'int'>` |
| `id(x)` | Memory address | Unique integer |
| `print()` | Output | `print("hi")` |
| `input()` | Input (returns str) | `s = input()` |
| `int()` / `float()` / `str()` | Type cast | `int("42")` → `42` |
| `bool(x)` | Boolean cast | `bool(0)` → `False` |
| `abs(x)` | Absolute value | `abs(-5)` → `5` |
| `max()` / `min()` | Max / Min | `max(1,2,3)` → `3` |
| `sum(iter)` | Sum | `sum([1,2,3])` → `6` |
| `sorted(iter)` | New sorted list | `sorted([3,1,2])` → `[1,2,3]` |
| `reversed(iter)` | Reverse iterator | `list(reversed([1,2,3]))` → `[3,2,1]` |
| `enumerate(iter)` | Index + value | `list(enumerate("ab"))` → `[(0,'a'),(1,'b')]` |
| `zip(a, b)` | Pair up | Truncates to shorter |
| `map(f, iter)` | Apply f | `list(map(str, [1,2]))` → `['1','2']` |
| `filter(f, iter)` | Keep where True | `list(filter(bool, [0,1,2]))` → `[1,2]` |
| `isinstance(obj, cls)` | Type check | `isinstance(5, int)` → `True` |
| `range(start,stop,step)` | Integer sequence | Stop is exclusive |
| `hash(x)` | Hash value | Only for hashable types |

---

## 17. File I/O Quick Reference

| Operation | Code | Notes |
|---|---|---|
| Read entire file | `f.read()` | Returns single string |
| Read one line | `f.readline()` | Includes `\n` |
| Read all lines | `f.readlines()` | List of strings with `\n` |
| Write string | `f.write(s)` | Does NOT add `\n` |
| Write list | `f.writelines(lst)` | Does NOT add separators |
| Context manager | `with open(f) as f:` | Auto-closes file ✅ |
| Modes | `r, w, a, x, r+, w+, a+, b` | `w` truncates, `a` appends |

---

## 18. Exception Handling Quick Reference

| Concept | Key Rule |
|---|---|
| `finally` | ALWAYS runs, even with `return` |
| `finally` + `return` | `finally`'s return overrides `try`'s |
| `else` block | Runs only if NO exception in `try` |
| `except` order | Check top-to-bottom; put specific before general |
| `except Exception` | Catches all except `SystemExit`, `KeyboardInterrupt` |
| Custom exceptions | Inherit from `Exception` (not `BaseException`) |
| `raise` (bare) | Re-raises current exception |

---

## 19. Iterators & Generators Quick Reference

| Concept | Key Point |
|---|---|
| Iterable | Has `__iter__()` — list, tuple, str, dict |
| Iterator | Has `__iter__()` + `__next__()` — one-shot! |
| `yield` | Pauses function, remembers state |
| Generator expression | `(x for x in range(n))` — lazy, not `[]` |
| Iterator exhaustion | Once consumed, `sum(it)` → `0` |
| `send(val)` | Send value INTO generator, resumes execution |
| `itertools` key | `product`, `permutations`, `combinations`, `chain`, `accumulate` |

---

## 20. Decorators Quick Reference

| Concept | Key Point |
|---|---|
| Decorator syntax | `@dec` ≡ `func = dec(func)` |
| Stacked decorators | `@d1 @d2` ≡ `func = d1(d2(func))` — apply bottom-up, execute top-down |
| `functools.wraps` | Preserves `__name__`, `__doc__` of original |
| `@property` | Makes method act as attribute (getter) |
| `@staticmethod` | No `self`/`cls` — utility function |
| `@classmethod` | Gets `cls` — factory methods, respects inheritance |
| Closure trap | `lambda: i` captures variable, not value — use `lambda i=i: i` |

---

## 21. Regex Quick Reference

| Pattern | Meaning |
|---|---|
| `\d+` | One or more digits |
| `\w+` | Word characters |
| `\s+` | Whitespace |
| `\b` | Word boundary |
| `[^...]` | Negated character class |
| `re.match` | Start of string only |
| `re.search` | First match anywhere |
| `re.findall` | All matches as list |
| `re.sub` | Replace matches |
| `group(0)` | Entire match |
| `group(n)` | nth capture group |

---

## 22. Additional GATE Traps

| # | Trap | Key Point |
|---|---|---|
| 21 | `write()` no newline | Must add `\n` explicitly |
| 22 | `writelines()` no separator | Just concatenates strings |
| 23 | `finally` overrides `return` | Return in finally wins |
| 24 | Iterator exhaustion | Can only iterate once |
| 25 | Closure variable binding | `lambda: i` captures variable, not value |
| 26 | `re.match` vs `re.search` | `match` = start only; `search` = anywhere |
| 27 | Type hints no enforcement | `add(a: int, b: int)` accepts strings |
| 28 | `Counter` missing key → 0 | Not KeyError |
| 29 | Generator expression exhaustion | `list(gen)` twice → second is `[]` |
| 30 | `@property` without setter | Assignment raises `AttributeError` |

---

*End of Python Revision Notes for GATE 2027 DA*
