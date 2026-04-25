# Python Programming — Questions & PYQs for GATE 2027 DA

---

> **Total: 55 Questions** covering Variables, Operators, Strings, Lists, Loops, Functions, OOP, Recursion, and Data Structures.  
> **+ 30 New Questions (Q66–Q95)** covering File I/O, Exception Handling, Iterators/Generators, Decorators, Regex, Modules.  
> Difficulty: ★ Easy | ★★ Medium | ★★★ Hard  
> Questions marked **[GATE YYYY]** are Previous Year Questions or based on GATE style.

---

## Section A: Variables, Types & Operators (Q1–Q10)

---

### Q1. ★ — Dynamic Typing
```python
x = 10
x = "hello"
x = [1, 2, 3]
print(type(x))
```
What is the output?

---

### Q2. ★★ — Division Operators [GATE Style]
```python
a = 7
b = 2
print(a / b, a // b, a % b)
```
What is the output?

---

### Q3. ★★ — Negative Floor Division [GATE Style]
```python
print(-7 // 2)
print(-7 % 2)
print(7 // -2)
print(7 % -2)
```
What is the output?

---

### Q4. ★★ — Operator Precedence
```python
print(2 ** 3 ** 2)
print((2 ** 3) ** 2)
```
What is the output?

---

### Q5. ★ — Boolean & Truthy Values
```python
print(bool(0), bool(""), bool([]), bool(None))
print(bool("0"), bool(" "), bool([0]))
```
What is the output?

---

### Q6. ★★ — Short-Circuit Evaluation
```python
x = 5
y = 0
result = y and (x / y)
print(result)
```
What is the output?

---

### Q7. ★★ — Bitwise NOT
```python
print(~5)
print(~0)
print(~(-1))
```
What is the output?

---

### Q8. ★★ — Chained Comparison
```python
x = 5
print(1 < x < 10)
print(10 > x <= 5)
print(1 < x > 3 < 7)
```
What is the output?

---

### Q9. ★★ — `is` vs `==` [GATE Style]
```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a
print(a == b)
print(a is b)
print(a is c)
print(a == c)
```
What is the output?

---

### Q10. ★★ — Integer Caching
```python
a = 256
b = 256
print(a is b)

c = 257
d = 257
print(c is d)
```
What is the output? (CPython standard behavior)

---

## Section B: Strings (Q11–Q18)

---

### Q11. ★ — String Slicing
```python
s = "GATE2027"
print(s[0:4])
print(s[-4:])
print(s[::2])
print(s[::-1])
```
What is the output?

---

### Q12. ★★ — String Immutability [GATE Style]
```python
s = "hello"
s[0] = 'H'
print(s)
```
What happens?

---

### Q13. ★★ — String Methods
```python
s = "  Hello, World!  "
print(s.strip())
print(s.split(","))
print("GATE".lower().upper())
print("abcabc".count("ab"))
print("hello".replace("l", "L", 1))
```
What is the output?

---

### Q14. ★ — String join
```python
words = ["GATE", "2027", "DA"]
result = "-".join(words)
print(result)
print(type(result))
```
What is the output?

---

### Q15. ★★ — String find vs index
```python
s = "GATE 2027"
print(s.find("2027"))
print(s.find("2028"))
# print(s.index("2028"))   # What would this do?
```
What is the output? What happens with the commented line?

---

### Q16. ★★ — String Multiplication
```python
s = "ab" * 3
print(s)
print(len(s))
```
What is the output?

---

### Q17. ★★★ — Slice Out of Range
```python
s = "GATE"
print(s[10:20])
print(s[1:100])
# print(s[10])    # What would this do?
```
What is the output? What happens with the commented line?

---

### Q18. ★★ — String Formatting
```python
name = "Alice"
score = 85.678
print(f"{name:>10}")
print(f"{score:.1f}")
print(f"{10:08b}")
```
What is the output?

---

## Section C: Lists (Q19–Q28)

---

### Q19. ★★ — `sort()` Return Value [GATE Style]
```python
lst = [3, 1, 4, 1, 5]
result = lst.sort()
print(result)
print(lst)
```
What is the output?

---

### Q20. ★★ — `append` vs `extend` [GATE Style]
```python
a = [1, 2]
a.append([3, 4])
print(a, len(a))

b = [1, 2]
b.extend([3, 4])
print(b, len(b))
```
What is the output?

---

### Q21. ★★ — List Aliasing [GATE Style]
```python
a = [1, 2, 3]
b = a
b.append(4)
print(a)
print(a is b)
```
What is the output?

---

### Q22. ★★★ — Shallow Copy Trap [GATE Style]
```python
import copy
a = [[1, 2], [3, 4]]
b = a.copy()
b[0].append(99)
b.append([5, 6])
print(a)
print(b)
```
What is the output?

---

### Q23. ★★ — List Comprehension
```python
result = [x**2 for x in range(6) if x % 2 != 0]
print(result)
```
What is the output?

---

### Q24. ★★ — Nested List Comprehension
```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [x for row in matrix for x in row if x % 2 == 0]
print(flat)
```
What is the output?

---

### Q25. ★★ — List Slice Assignment
```python
lst = [1, 2, 3, 4, 5]
lst[1:3] = [20, 30, 40]
print(lst)
print(len(lst))
```
What is the output?

---

### Q26. ★★ — `pop()` Behavior
```python
lst = [10, 20, 30, 40]
x = lst.pop()
y = lst.pop(0)
print(x, y)
print(lst)
```
What is the output?

---

### Q27. ★★★ — List Multiplication Trap [GATE Style]
```python
a = [[0]] * 3
a[0].append(1)
print(a)
```
What is the output?

---

### Q28. ★★ — `sorted()` with key
```python
words = ["banana", "apple", "cherry", "date"]
result = sorted(words, key=len)
print(result)
```
What is the output?

---

## Section D: Loops & Iteration (Q29–Q33)

---

### Q29. ★ — Range Function
```python
print(list(range(5)))
print(list(range(2, 10, 3)))
print(list(range(10, 0, -2)))
print(list(range(5, 2)))
```
What is the output?

---

### Q30. ★★ — Loop with `else` [GATE Style]
```python
for i in range(5):
    if i == 3:
        break
else:
    print("Completed")
print("Done")
```
What is the output?

---

### Q31. ★★ — Loop with `else` (no break)
```python
for i in range(5):
    if i == 10:
        break
else:
    print("Completed")
print("Done")
```
What is the output?

---

### Q32. ★★ — Nested Loop Output
```python
for i in range(3):
    for j in range(i + 1):
        print("*", end="")
    print()
```
What is the output?

---

### Q33. ★★★ — Enumerate & Zip
```python
names = ["Alice", "Bob"]
scores = [85, 92, 78]

for i, (name, score) in enumerate(zip(names, scores)):
    print(f"{i}: {name} - {score}")
```
What is the output?

---

## Section E: Functions & Scope (Q34–Q42)

---

### Q34. ★★ — LEGB Scope [GATE Style]
```python
x = 10
def f():
    x = 20
    def g():
        print(x)
    g()

f()
print(x)
```
What is the output?

---

### Q35. ★★★ — UnboundLocalError [GATE Style]
```python
x = 10
def f():
    print(x)
    x = 20

f()
```
What is the output?

---

### Q36. ★★ — `global` Keyword
```python
x = 1
def f():
    global x
    x = x + 10
    return x

print(f())
print(x)
```
What is the output?

---

### Q37. ★★★ — Mutable Default Argument [GATE Style]
```python
def f(x, lst=[]):
    lst.append(x)
    return lst

print(f(1))
print(f(2))
print(f(3))
```
What is the output?

---

### Q38. ★★ — Pass by Object Reference [GATE Style]
```python
def modify(lst):
    lst.append(4)
    lst = [10, 20, 30]
    lst.append(40)

a = [1, 2, 3]
modify(a)
print(a)
```
What is the output?

---

### Q39. ★★ — `*args` and `**kwargs`
```python
def f(*args, **kwargs):
    print(args)
    print(kwargs)

f(1, 2, 3, x=10, y=20)
```
What is the output?

---

### Q40. ★★ — Lambda with map
```python
nums = [1, 2, 3, 4, 5]
result = list(map(lambda x: x**2 if x % 2 == 0 else x, nums))
print(result)
```
What is the output?

---

### Q41. ★★ — Lambda with filter
```python
nums = list(range(1, 11))
result = list(filter(lambda x: x % 3 == 0, nums))
print(result)
```
What is the output?

---

### Q42. ★★★ — Multiple Return & Unpacking
```python
def f():
    return 1, 2, 3

a = f()
b, c, d = f()
e, *rest = f()

print(a)
print(type(a))
print(b, c, d)
print(e, rest)
```
What is the output?

---

## Section F: Tuples, Dicts & Sets (Q43–Q48)

---

### Q43. ★★ — Tuple Trap [GATE Style]
```python
a = (1)
b = (1,)
c = ()

print(type(a))
print(type(b))
print(type(c))
```
What is the output?

---

### Q44. ★★ — Tuple with Mutable Element [GATE Style]
```python
t = ([1, 2], [3, 4])
t[0].append(5)
print(t)
# t[0] = [10, 20]   # What happens?
```
What is the output? What happens with the commented line?

---

### Q45. ★★ — Dictionary Operations
```python
d = {"a": 1, "b": 2, "c": 3}
d["d"] = 4
d["a"] = 10
del d["b"]
print(d)
print("c" in d)
print(3 in d)
```
What is the output?

---

### Q46. ★★ — Set Operations [GATE Style]
```python
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}
print(A | B)
print(A & B)
print(A - B)
print(A ^ B)
```
What is the output?

---

### Q47. ★★ — Empty Set vs Dict
```python
a = {}
b = set()
print(type(a))
print(type(b))
```
What is the output?

---

### Q48. ★★★ — Dict Comprehension & Zip
```python
keys = ["a", "b", "c"]
values = [1, 2, 3]
d = {k: v**2 for k, v in zip(keys, values)}
print(d)
```
What is the output?

---

## Section G: OOP (Q49–Q53)

---

### Q49. ★★ — Class vs Instance Variables [GATE Style]
```python
class MyClass:
    data = []

a = MyClass()
b = MyClass()
a.data.append(1)
b.data.append(2)
print(a.data)
print(b.data)
print(a.data is b.data)
```
What is the output?

---

### Q50. ★★★ — Class Variable Shadowing
```python
class MyClass:
    x = 10

a = MyClass()
b = MyClass()
a.x = 20           # Creates instance variable for a
MyClass.x = 30     # Changes class variable

print(a.x)
print(b.x)
```
What is the output?

---

### Q51. ★★ — Inheritance & Method Overriding
```python
class Parent:
    def show(self):
        print("Parent")

class Child(Parent):
    def show(self):
        print("Child")

class GrandChild(Child):
    pass

g = GrandChild()
g.show()
print(isinstance(g, Parent))
```
What is the output?

---

### Q52. ★★★ — Multiple Inheritance MRO [GATE Style]
```python
class A:
    def f(self):
        print("A", end=" ")

class B(A):
    def f(self):
        print("B", end=" ")
        super().f()

class C(A):
    def f(self):
        print("C", end=" ")
        super().f()

class D(B, C):
    def f(self):
        print("D", end=" ")
        super().f()

d = D()
d.f()
```
What is the output?

---

### Q53. ★★ — Operator Overloading
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    def __str__(self):
        return f"({self.x}, {self.y})"

p1 = Point(1, 2)
p2 = Point(3, 4)
p3 = p1 + p2
print(p3)
print(type(p3))
```
What is the output?

---

## Section H: Recursion (Q54–Q58)

---

### Q54. ★★ — Factorial Trace
```python
def fact(n):
    if n == 0:
        return 1
    return n * fact(n - 1)

print(fact(5))
```
What is the output? Trace the recursion calls.

---

### Q55. ★★ — Fibonacci
```python
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)

print(fib(7))
```
What is the output?

---

### Q56. ★★★ — Tricky Recursion [GATE Style]
```python
def f(n):
    if n <= 0:
        return 0
    return f(n - 1) + n * n

print(f(4))
```
What is the output?

---

### Q57. ★★★ — Recursion with List [GATE Style]
```python
def mystery(lst):
    if len(lst) == 0:
        return 0
    if len(lst) == 1:
        return lst[0]
    return lst[0] + mystery(lst[2:])

print(mystery([1, 2, 3, 4, 5, 6, 7]))
```
What is the output?

---

### Q58. ★★★ — Recursive String [GATE Style]
```python
def f(s):
    if len(s) <= 1:
        return s
    return f(s[1:]) + s[0]

print(f("GATE"))
```
What is the output?

---

## Section I: Mixed / Comprehensive (Q59–Q65)

---

### Q59. ★★★ — Global + Mutable [GATE Style]
```python
data = [1, 2, 3]

def f():
    data.append(4)

def g():
    data = [10, 20]

f()
g()
print(data)
```
What is the output?

---

### Q60. ★★★ — Nested Function & Closure
```python
def outer():
    x = 10
    def inner():
        nonlocal x
        x += 5
        return x
    return inner

fn = outer()
print(fn())
print(fn())
```
What is the output?

---

### Q61. ★★★ — List as Default + Aliasing [GATE Style]
```python
def f(n, lst=[]):
    for i in range(n):
        lst.append(i * i)
    return lst

a = f(3)
b = f(2)
print(a)
print(b)
print(a is b)
```
What is the output?

---

### Q62. ★★ — Reduce
```python
from functools import reduce

result = reduce(lambda a, b: a * b, [1, 2, 3, 4, 5])
print(result)
```
What is the output?

---

### Q63. ★★★ — Deep Copy vs Shallow [GATE Style]
```python
import copy

a = [[1, 2], [3, 4]]
b = copy.copy(a)
c = copy.deepcopy(a)

a[0][0] = 99
print(b[0][0])
print(c[0][0])
```
What is the output?

---

### Q64. ★★ — Extended Unpacking
```python
a, *b, c = [1, 2, 3, 4, 5]
print(a)
print(b)
print(c)
print(type(b))
```
What is the output?

---

### Q65. ★★★ — Linked List Traversal [GATE Style]
```python
class Node:
    def __init__(self, data, next=None):
        self.data = data
        self.next = next

head = Node(1, Node(2, Node(3, Node(4))))

current = head
total = 0
while current:
    total += current.data
    current = current.next

print(total)
```
What is the output?

---

*End of Python Questions & PYQs for GATE 2027 DA — 65 Questions*

---

## Section J: File I/O & Exception Handling (Q66–Q75)

---

### Q66. ★★ — File Write & Read [GATE Style]
```python
with open("test.txt", "w") as f:
    f.write("Hello")
    f.write("World")

with open("test.txt", "r") as f:
    print(f.read())
```
What is the output?

---

### Q67. ★★ — finally Overrides return [GATE Style]
```python
def f():
    try:
        return 1
    except:
        return 2
    finally:
        return 3

print(f())
```
What is the output?

---

### Q68. ★★★ — Exception Handling Flow [GATE Style]
```python
def g(x):
    try:
        if x == 0:
            raise ValueError("zero")
        return 10 // x
    except ValueError as e:
        print("VE:", e)
        return -1
    except ZeroDivisionError:
        print("ZDE")
        return -2
    else:
        print("OK")
    finally:
        print("DONE")

print(g(0))
```
What is the output?

---

### Q69. ★★ — Exception Hierarchy
```python
try:
    d = {"a": 1}
    print(d["b"])
except LookupError:
    print("Caught!")
```
What is the output? (Hint: Is `KeyError` a subclass of `LookupError`?)

---

### Q70. ★★★ — Custom Exception
```python
class MyError(ValueError):
    def __init__(self, val):
        self.val = val
        super().__init__(f"Bad: {val}")

try:
    raise MyError(42)
except ValueError as e:
    print(type(e).__name__, e)
```
What is the output?

---

### Q71. ★★ — File Modes [GATE Style]
```python
with open("test.txt", "w") as f:
    f.write("ABC")

with open("test.txt", "a") as f:
    f.write("DEF")

with open("test.txt", "r") as f:
    print(f.read())
```
What is the output?

---

### Q72. ★★★ — readlines vs readline
```python
with open("test.txt", "w") as f:
    f.write("Line1\nLine2\nLine3")

with open("test.txt", "r") as f:
    a = f.readline()
    b = f.readlines()
    print(repr(a))
    print(b)
```
What is the output?

---

### Q73. ★★ — Nested try/except [GATE Style]
```python
try:
    try:
        1 / 0
    except ZeroDivisionError:
        print("inner")
        raise ValueError("converted")
except ValueError as e:
    print("outer:", e)
finally:
    print("final")
```
What is the output?

---

### Q74. ★★★ — except Order Matters
```python
try:
    x = int("abc")
except Exception:
    print("A")
except ValueError:
    print("B")
```
What gets printed?

---

### Q75. ★★ — writelines Behavior
```python
with open("test.txt", "w") as f:
    f.writelines(["Hello", "World", "!"])

with open("test.txt", "r") as f:
    print(f.read())
```
What is the output?

---

## Section K: Iterators, Generators & Decorators (Q76–Q85)

---

### Q76. ★★ — Iterator Exhaustion [GATE Style]
```python
nums = [1, 2, 3]
it = iter(nums)
print(sum(it))
print(sum(it))
```
What is the output?

---

### Q77. ★★★ — Generator Execution [GATE Style]
```python
def gen():
    print("A")
    yield 1
    print("B")
    yield 2
    print("C")

g = gen()
print(next(g))
print(next(g))
```
What is the output?

---

### Q78. ★★ — Generator Expression vs List Comprehension
```python
x = [1, 2, 3, 4, 5]
a = [i**2 for i in x if i > 3]
b = sum(i**2 for i in x if i > 3)
print(a, b)
```
What is the output?

---

### Q79. ★★★ — Generator with return
```python
def gen():
    yield 1
    return "done"
    yield 2

g = gen()
print(next(g))
try:
    print(next(g))
except StopIteration as e:
    print(e.value)
```
What is the output?

---

### Q80. ★★★ — Decorator Execution Order [GATE Style]
```python
def dec1(func):
    def wrapper():
        print("D1")
        func()
    return wrapper

def dec2(func):
    def wrapper():
        print("D2")
        func()
    return wrapper

@dec1
@dec2
def hello():
    print("Hello")

hello()
```
What is the output?

---

### Q81. ★★ — @staticmethod vs @classmethod
```python
class A:
    x = 10
    
    @staticmethod
    def f():
        return A.x
    
    @classmethod
    def g(cls):
        return cls.x

class B(A):
    x = 20

print(A.f(), A.g())
print(B.f(), B.g())
```
What is the output?

---

### Q82. ★★★ — Closure Variable Binding Trap [GATE Style]
```python
funcs = []
for i in range(3):
    funcs.append(lambda: i)

print([f() for f in funcs])
```
What is the output?

---

### Q83. ★★ — @property
```python
class Temp:
    def __init__(self, celsius):
        self._c = celsius
    
    @property
    def fahrenheit(self):
        return self._c * 9/5 + 32

t = Temp(100)
print(t.fahrenheit)
try:
    t.fahrenheit = 212
except AttributeError:
    print("Read-only!")
```
What is the output?

---

### Q84. ★★★ — functools.wraps Effect
```python
from functools import wraps

def dec(func):
    @wraps(func)
    def wrapper(*args):
        return func(*args)
    return wrapper

@dec
def add(a, b):
    """Adds two numbers"""
    return a + b

print(add.__name__)
print(add.__doc__)
```
What is the output?

---

### Q85. ★★★ — Generator send() [GATE Style]
```python
def accumulator():
    total = 0
    while True:
        x = yield total
        total += x

gen = accumulator()
print(next(gen))
print(gen.send(10))
print(gen.send(20))
print(gen.send(5))
```
What is the output?

---

## Section L: Regex, Modules & Advanced (Q86–Q95)

---

### Q86. ★★ — re.findall [GATE Style]
```python
import re
text = "abc123def456ghi"
result = re.findall(r"\d+", text)
print(result)
```
What is the output?

---

### Q87. ★★★ — re.match vs re.search
```python
import re
text = "Hello World 42"
m1 = re.match(r"\d+", text)
m2 = re.search(r"\d+", text)
print(m1)
print(m2.group())
```
What is the output?

---

### Q88. ★★★ — Regex Groups [GATE Style]
```python
import re
m = re.search(r"(\d{2})/(\d{2})/(\d{4})", "DOB: 15/08/1947")
print(m.group(0))
print(m.group(1), m.group(2), m.group(3))
print(m.groups())
```
What is the output?

---

### Q89. ★★ — re.sub
```python
import re
result = re.sub(r"[aeiou]", "*", "Hello World", flags=re.I)
print(result)
```
What is the output?

---

### Q90. ★★ — Counter [GATE Style]
```python
from collections import Counter
c = Counter("mississippi")
print(c.most_common(2))
print(c['s'])
print(c['z'])
```
What is the output?

---

### Q91. ★★ — defaultdict
```python
from collections import defaultdict
d = defaultdict(list)
d['a'].append(1)
d['a'].append(2)
d['b'].append(3)
print(dict(d))
print(d['c'])
```
What is the output?

---

### Q92. ★★★ — __name__ == "__main__" [GATE Style]
Consider these two files:

**helper.py:**
```python
print("Helper loaded")

def greet():
    return "Hi"

if __name__ == "__main__":
    print("Running helper directly")
```

**main.py:**
```python
import helper
print(helper.greet())
```

When you run `python main.py`, what is the output?

---

### Q93. ★★★ — itertools.product [GATE Style]
```python
from itertools import product
result = list(product([0, 1], repeat=3))
print(len(result))
print(result[0], result[-1])
```
What is the output?

---

### Q94. ★★ — Walrus Operator
```python
data = [3, 7, 2, 8, 1, 9]
result = [y for x in data if (y := x * 3) > 10]
print(result)
```
What is the output?

---

### Q95. ★★★ — Type Hints Runtime Behavior [GATE Style]
```python
def add(a: int, b: int) -> int:
    return a + b

result = add("hello", " world")
print(result)
print(type(result))
```
What is the output?

---

*End of Python Questions & PYQs for GATE 2027 DA — 95 Questions*
