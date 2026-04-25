# C Programming — Questions & PYQs for GATE 2027

---

> **Total: 65 Questions** covering Operators, Pointers, Recursion, Storage Classes, Arrays, Strings, Structures, and Type Conversion.  
> Difficulty: ★ Easy | ★★ Medium | ★★★ Hard  
> Questions marked **[GATE YYYY]** are Previous Year Questions or based on GATE style.

---

## Section A: Operators, Expressions & Type Conversion (Q1–Q15)

---

### Q1. ★ — Arithmetic Operators
```c
#include <stdio.h>
int main() {
    int a = 10, b = 3;
    printf("%d %d %d\n", a/b, a%b, -a%b);
    return 0;
}
```
What is the output?

---

### Q2. ★★ — Integer Promotion [GATE Style]
```c
#include <stdio.h>
int main() {
    char c = 125;
    c = c + 10;
    printf("%d\n", c);
    return 0;
}
```
What is the output? (Assume char is 8-bit signed)

---

### Q3. ★★ — Signed vs Unsigned Comparison [GATE 2015 Style]
```c
#include <stdio.h>
int main() {
    unsigned int x = 1;
    int y = -1;
    if (x > y)
        printf("x > y");
    else
        printf("x <= y");
    return 0;
}
```
What is the output?

---

### Q4. ★★ — Bitwise Operators
```c
#include <stdio.h>
int main() {
    int a = 12;  // 1100 in binary
    int b = 10;  // 1010 in binary
    printf("%d %d %d %d\n", a&b, a|b, a^b, ~a);
    return 0;
}
```
What is the output?

---

### Q5. ★★ — Left and Right Shift
```c
#include <stdio.h>
int main() {
    int x = 1;
    printf("%d %d\n", x << 4, x << 10);
    int y = 100;
    printf("%d %d\n", y >> 2, y >> 5);
    return 0;
}
```
What is the output?

---

### Q6. ★★★ — Comma Operator [GATE 2008 Style]
```c
#include <stdio.h>
int main() {
    int a = (1, 2, 3);
    int b;
    b = 1, 2, 3;
    printf("%d %d\n", a, b);
    return 0;
}
```
What is the output?

---

### Q7. ★★ — Short-Circuit Evaluation
```c
#include <stdio.h>
int main() {
    int a = 0, b = 0, c = 0;
    int x = a++ && b++ || c++;
    printf("a=%d b=%d c=%d x=%d\n", a, b, c, x);
    return 0;
}
```
What is the output?

---

### Q8. ★★★ — Pre/Post Increment [GATE Style]
```c
#include <stdio.h>
int main() {
    int x = 5;
    int y = x++ + ++x;
    printf("%d %d\n", x, y);
    return 0;
}
```
What can be said about this code?

---

### Q9. ★★ — Conditional (Ternary) Operator
```c
#include <stdio.h>
int main() {
    int a = 5, b = 10;
    int min = (a < b) ? a : b;
    printf("%d\n", min);
    int c = (a > b) ? a++ : b++;
    printf("%d %d %d\n", a, b, c);
    return 0;
}
```
What is the output?

---

### Q10. ★★★ — Operator Precedence [GATE 2015]
```c
#include <stdio.h>
int main() {
    int i = 1;
    int j = i++ + i++ + i++;
    printf("%d %d\n", i, j);
    return 0;
}
```
What can be said about the output?

---

### Q11. ★★ — Bitwise NOT and Unsigned
```c
#include <stdio.h>
int main() {
    unsigned int x = 0;
    printf("%u\n", ~x);
    printf("%d\n", ~x);
    return 0;
}
```
What is the output? (Assume 32-bit int)

---

### Q12. ★★ — Type Conversion
```c
#include <stdio.h>
int main() {
    int a = 7;
    float b = 7.0;
    printf("%f\n", a/2);
    printf("%f\n", b/2);
    printf("%f\n", (float)a/2);
    return 0;
}
```
What is the output?

---

### Q13. ★★ — Sizeof Operator
```c
#include <stdio.h>
int main() {
    int x = 5;
    printf("%lu\n", sizeof(x++));
    printf("%d\n", x);
    return 0;
}
```
What is the output? (Assume 32-bit int)

---

### Q14. ★ — Logical Operators
```c
#include <stdio.h>
int main() {
    int a = 5, b = 0, c = -3;
    printf("%d\n", a && b);
    printf("%d\n", a || b);
    printf("%d\n", !c);
    printf("%d\n", !!a);
    return 0;
}
```
What is the output?

---

### Q15. ★★★ — Short-Circuit with Side Effects [GATE Style]
```c
#include <stdio.h>
int main() {
    int a = 1, b = 1, c = 1, d = 1;
    int result = ++a || ++b && ++c;
    printf("a=%d b=%d c=%d d=%d result=%d\n", a, b, c, d, result);
    return 0;
}
```
What is the output?

---

## Section B: Control Flow (Q16–Q20)

---

### Q16. ★ — Switch Fall-Through
```c
#include <stdio.h>
int main() {
    int x = 2;
    switch(x) {
        case 1: printf("A");
        case 2: printf("B");
        case 3: printf("C");
        default: printf("D");
    }
    return 0;
}
```
What is the output?

---

### Q17. ★★ — Loop with Break and Continue
```c
#include <stdio.h>
int main() {
    int i;
    for (i = 0; i < 10; i++) {
        if (i == 3) continue;
        if (i == 7) break;
        printf("%d ", i);
    }
    printf("\ni = %d\n", i);
    return 0;
}
```
What is the output?

---

### Q18. ★★ — While Loop Tricky
```c
#include <stdio.h>
int main() {
    int i = 0;
    while (i++ < 5)
        printf("%d ", i);
    printf("\ni = %d\n", i);
    return 0;
}
```
What is the output?

---

### Q19. ★★ — Do-While vs While
```c
#include <stdio.h>
int main() {
    int i = 10;
    while (i < 5) {
        printf("W%d ", i);
        i++;
    }
    i = 10;
    do {
        printf("D%d ", i);
        i++;
    } while (i < 5);
    return 0;
}
```
What is the output?

---

### Q20. ★★ — Nested Loop Output
```c
#include <stdio.h>
int main() {
    int count = 0;
    for (int i = 0; i < 4; i++)
        for (int j = i; j < 4; j++)
            count++;
    printf("%d\n", count);
    return 0;
}
```
What is the output?

---

## Section C: Functions & Storage Classes (Q21–Q30)

---

### Q21. ★★ — Static Variable [GATE 2019 Style]
```c
#include <stdio.h>
int fun() {
    static int x = 0;
    x++;
    return x;
}
int main() {
    printf("%d ", fun());
    printf("%d ", fun());
    printf("%d ", fun());
    return 0;
}
```
What is the output?

---

### Q22. ★★ — Call by Value
```c
#include <stdio.h>
void foo(int x) {
    x = 100;
}
int main() {
    int a = 5;
    foo(a);
    printf("%d\n", a);
    return 0;
}
```
What is the output?

---

### Q23. ★★★ — Extern Variable [GATE Style]
```c
#include <stdio.h>
int x;
void f1() {
    x = x + 1;
    printf("%d ", x);
}
void f2() {
    int x = 10;
    f1();
    printf("%d ", x);
}
int main() {
    x = 5;
    f2();
    f1();
    return 0;
}
```
What is the output?

---

### Q24. ★★ — Storage Class Default Values
```c
#include <stdio.h>
int g;
int main() {
    int a;
    static int s;
    printf("%d %d\n", g, s);
    return 0;
}
```
What is the output? What is the value of `a`?

---

### Q25. ★★★ — Static in Recursion [GATE PYQ Style]
```c
#include <stdio.h>
int fun(int n) {
    static int count = 0;
    count++;
    if (n > 0)
        fun(n - 1);
    return count;
}
int main() {
    printf("%d\n", fun(4));
    return 0;
}
```
What is the output?

---

### Q26. ★★ — Return Value of printf
```c
#include <stdio.h>
int main() {
    int x = printf("Hello");
    printf(" %d\n", x);
    return 0;
}
```
What is the output?

---

### Q27. ★★★ — Multiple Calls with Static
```c
#include <stdio.h>
void f() {
    static int i = 0;
    printf("%d ", ++i);
}
int main() {
    f(); f(); f(); f(); f();
    return 0;
}
```
What is the output?

---

### Q28. ★★ — register Keyword
```c
#include <stdio.h>
int main() {
    register int x = 10;
    int *p = &x;
    printf("%d\n", *p);
    return 0;
}
```
What happens when this code is compiled?

---

### Q29. ★★ — Function Returning Pointer to Static
```c
#include <stdio.h>
int *fun() {
    static int x = 10;
    return &x;
}
int main() {
    int *p = fun();
    *p = 20;
    int *q = fun();
    printf("%d\n", *q);
    return 0;
}
```
What is the output?

---

### Q30. ★★★ — Static vs Non-Static Counter
```c
#include <stdio.h>
int countA() {
    static int c = 0;
    return ++c;
}
int countB() {
    int c = 0;
    return ++c;
}
int main() {
    printf("%d %d %d\n", countA(), countA(), countA());
    printf("%d %d %d\n", countB(), countB(), countB());
    return 0;
}
```
What is the output?

---

## Section D: Recursion (Q31–Q38)

---

### Q31. ★★ — Simple Recursion
```c
#include <stdio.h>
void fun(int n) {
    if (n <= 0) return;
    printf("%d ", n);
    fun(n - 2);
    printf("%d ", n);
}
int main() {
    fun(5);
    return 0;
}
```
What is the output?

---

### Q32. ★★★ — Fibonacci Recursion [GATE Style]
```c
#include <stdio.h>
int fib(int n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
int main() {
    printf("%d\n", fib(6));
    return 0;
}
```
What is the output? How many total function calls are made for `fib(6)`?

---

### Q33. ★★ — Print Order in Recursion
```c
#include <stdio.h>
void fun(int n) {
    if (n == 0) return;
    fun(n / 2);
    printf("%d ", n % 2);
}
int main() {
    fun(13);
    return 0;
}
```
What is the output?

---

### Q34. ★★★ — Recursion with Return Value [GATE 2017 Style]
```c
#include <stdio.h>
int fun(int n) {
    if (n == 0) return 0;
    return n + fun(n - 1);
}
int main() {
    printf("%d\n", fun(10));
    return 0;
}
```
What is the output? Write the general formula for `fun(n)`.

---

### Q35. ★★★ — Mutual Recursion
```c
#include <stdio.h>
int isEven(int n);
int isOdd(int n);

int isEven(int n) {
    if (n == 0) return 1;
    return isOdd(n - 1);
}
int isOdd(int n) {
    if (n == 0) return 0;
    return isEven(n - 1);
}
int main() {
    printf("%d %d %d %d\n", isEven(4), isOdd(4), isEven(7), isOdd(7));
    return 0;
}
```
What is the output?

---

### Q36. ★★ — Recursion Count
```c
#include <stdio.h>
int count = 0;
void fun(int n) {
    count++;
    if (n <= 1) return;
    fun(n / 2);
    fun(n / 2);
}
int main() {
    fun(8);
    printf("%d\n", count);
    return 0;
}
```
What is the output?

---

### Q37. ★★★ — Tricky Recursion Output [GATE Style]
```c
#include <stdio.h>
int f(int n) {
    if (n <= 1) return 1;
    if (n % 2 == 0)
        return f(n/2);
    return f(n/2) + f(n/2 + 1);
}
int main() {
    printf("%d\n", f(11));
    return 0;
}
```
What is the output?

---

### Q38. ★★ — Recursion with Static Variable
```c
#include <stdio.h>
void fun(int n) {
    static int x = 0;
    x++;
    if (n > 0) {
        fun(n - 1);
    }
    printf("%d ", x);
}
int main() {
    fun(3);
    return 0;
}
```
What is the output?

---

## Section E: Pointers (Q39–Q52)

---

### Q39. ★ — Basic Pointer
```c
#include <stdio.h>
int main() {
    int x = 10;
    int *p = &x;
    *p = 20;
    printf("%d %d\n", x, *p);
    return 0;
}
```
What is the output?

---

### Q40. ★★ — Pointer Arithmetic [GATE Style]
```c
#include <stdio.h>
int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *p = arr;
    printf("%d\n", *(p + 3));
    printf("%d\n", *(arr + 4));
    printf("%d\n", *p + 3);
    return 0;
}
```
What is the output?

---

### Q41. ★★★ — Pointer and Array Name [GATE 2008 Style]
```c
#include <stdio.h>
int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int *p = arr;
    printf("%d %d\n", sizeof(arr), sizeof(p));
    return 0;
}
```
What is the output? (Assume 32-bit system)

---

### Q42. ★★ — Pointer Increment
```c
#include <stdio.h>
int main() {
    int arr[] = {10, 20, 30, 40};
    int *p = arr;
    p++;
    printf("%d\n", *p);
    *p++ = 100;
    printf("%d %d\n", arr[1], *p);
    return 0;
}
```
What is the output?

---

### Q43. ★★★ — Double Pointer [GATE 2008]
```c
#include <stdio.h>
int main() {
    int x = 10;
    int *p = &x;
    int **pp = &p;
    printf("%d %d %d\n", x, *p, **pp);
    **pp = 20;
    printf("%d\n", x);
    return 0;
}
```
What is the output?

---

### Q44. ★★★ — Pointer to Array
```c
#include <stdio.h>
int main() {
    int arr[5] = {1, 2, 3, 4, 5};
    int (*p)[5] = &arr;
    printf("%d\n", *(*p + 2));
    printf("%d\n", (*p)[3]);
    return 0;
}
```
What is the output?

---

### Q45. ★★★ — Array of Pointers
```c
#include <stdio.h>
int main() {
    int a = 1, b = 2, c = 3, d = 4;
    int *arr[] = {&a, &b, &c, &d};
    printf("%d %d\n", *arr[0], *arr[2]);
    printf("%d\n", **(arr + 1));
    return 0;
}
```
What is the output?

---

### Q46. ★★ — Pointer and String
```c
#include <stdio.h>
int main() {
    char *s = "Hello";
    printf("%c\n", *s);
    printf("%c\n", *(s+1));
    printf("%s\n", s+2);
    return 0;
}
```
What is the output?

---

### Q47. ★★★ — 2D Array and Pointers [GATE Style]
```c
#include <stdio.h>
int main() {
    int a[2][3] = {{1,2,3},{4,5,6}};
    printf("%d\n", *(a[0] + 4));
    printf("%d\n", *(*(a+1) + 1));
    printf("%d\n", a[1][2]);
    return 0;
}
```
What is the output?

---

### Q48. ★★★ — Pointer Subtraction
```c
#include <stdio.h>
int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *p = arr + 1;
    int *q = arr + 4;
    printf("%ld\n", q - p);
    printf("%ld\n", (char*)q - (char*)p);
    return 0;
}
```
What is the output? (Assume sizeof(int) = 4)

---

### Q49. ★★ — Swap Using Pointers
```c
#include <stdio.h>
void swap(int *a, int *b) {
    int t = *a;
    *a = *b;
    *b = t;
}
int main() {
    int x = 10, y = 20;
    swap(&x, &y);
    printf("%d %d\n", x, y);
    return 0;
}
```
What is the output?

---

### Q50. ★★★ — Pointer Passed to Function [GATE Style]
```c
#include <stdio.h>
void fun(int *p) {
    int q = 20;
    p = &q;
}
int main() {
    int x = 10;
    int *p = &x;
    fun(p);
    printf("%d\n", *p);
    return 0;
}
```
What is the output?

---

### Q51. ★★★ — Dangling Pointer
```c
#include <stdio.h>
int *fun() {
    int x = 10;
    return &x;
}
int main() {
    int *p = fun();
    printf("%d\n", *p);
    return 0;
}
```
What is the issue with this code?

---

### Q52. ★★★ — Void Pointer [GATE Style]
```c
#include <stdio.h>
int main() {
    int x = 10;
    void *vp = &x;
    printf("%d\n", *(int*)vp);
    // printf("%d\n", *vp);  // Line A
    return 0;
}
```
What is the output? What happens if Line A is uncommented?

---

## Section F: Dynamic Memory & Strings (Q53–Q58)

---

### Q53. ★★ — malloc and free
```c
#include <stdio.h>
#include <stdlib.h>
int main() {
    int *p = (int *)malloc(5 * sizeof(int));
    for (int i = 0; i < 5; i++)
        p[i] = i * 10;
    printf("%d %d %d\n", p[0], p[2], p[4]);
    free(p);
    return 0;
}
```
What is the output?

---

### Q54. ★★ — calloc vs malloc
```c
#include <stdio.h>
#include <stdlib.h>
int main() {
    int *a = (int *)malloc(3 * sizeof(int));
    int *b = (int *)calloc(3, sizeof(int));
    printf("%d %d %d\n", b[0], b[1], b[2]);
    free(a);
    free(b);
    return 0;
}
```
What is the output?

---

### Q55. ★★★ — Memory Leak Identification
```c
#include <stdio.h>
#include <stdlib.h>
void fun() {
    int *p = (int *)malloc(10 * sizeof(int));
    p[0] = 42;
    printf("%d\n", p[0]);
}
int main() {
    fun();
    fun();
    fun();
    return 0;
}
```
What is the output? What is the problem with this code?

---

### Q56. ★★ — String Operations
```c
#include <stdio.h>
#include <string.h>
int main() {
    char s1[] = "Hello";
    char s2[] = "World";
    printf("%lu %lu\n", strlen(s1), sizeof(s1));
    printf("%d\n", strcmp(s1, s2));
    return 0;
}
```
What is the output?

---

### Q57. ★★★ — String and Pointer Trap
```c
#include <stdio.h>
int main() {
    char s[] = "GATE2027";
    char *p = s;
    printf("%c %c\n", *p, *(p + 4));
    p += 2;
    printf("%s\n", p);
    return 0;
}
```
What is the output?

---

### Q58. ★★★ — Sizeof with Strings
```c
#include <stdio.h>
#include <string.h>
int main() {
    char *s1 = "Hello";
    char s2[] = "Hello";
    char s3[20] = "Hello";
    printf("%lu %lu %lu\n", sizeof(s1), sizeof(s2), sizeof(s3));
    printf("%lu %lu %lu\n", strlen(s1), strlen(s2), strlen(s3));
    return 0;
}
```
What is the output? (Assume 32-bit system)

---

## Section G: Structures (Q59–Q65)

---

### Q59. ★★ — Structure Basics
```c
#include <stdio.h>
struct Point {
    int x, y;
};
int main() {
    struct Point p = {10, 20};
    struct Point *ptr = &p;
    printf("%d %d\n", p.x, ptr->y);
    ptr->x = 30;
    printf("%d\n", p.x);
    return 0;
}
```
What is the output?

---

### Q60. ★★★ — Structure Padding [GATE Style]
```c
#include <stdio.h>
struct A {
    char c;
    int i;
    char d;
};
struct B {
    char c;
    char d;
    int i;
};
int main() {
    printf("%lu %lu\n", sizeof(struct A), sizeof(struct B));
    return 0;
}
```
What is the output?

---

### Q61. ★★ — Array of Structures
```c
#include <stdio.h>
struct Student {
    int id;
    float marks;
};
int main() {
    struct Student s[3] = {{1, 85.5}, {2, 90.0}, {3, 78.5}};
    struct Student *p = s;
    printf("%.1f\n", (p+1)->marks);
    printf("%d\n", (*(p+2)).id);
    return 0;
}
```
What is the output?

---

### Q62. ★★★ — Structure and Function
```c
#include <stdio.h>
struct Point {
    int x, y;
};
struct Point move(struct Point p, int dx, int dy) {
    p.x += dx;
    p.y += dy;
    return p;
}
int main() {
    struct Point a = {1, 2};
    struct Point b = move(a, 3, 4);
    printf("a=(%d,%d) b=(%d,%d)\n", a.x, a.y, b.x, b.y);
    return 0;
}
```
What is the output?

---

### Q63. ★★★ — Self-Referential Structure
```c
#include <stdio.h>
#include <stdlib.h>
struct Node {
    int data;
    struct Node *next;
};
int main() {
    struct Node n1 = {10, NULL};
    struct Node n2 = {20, NULL};
    struct Node n3 = {30, NULL};
    n1.next = &n2;
    n2.next = &n3;
    
    struct Node *p = &n1;
    while (p != NULL) {
        printf("%d ", p->data);
        p = p->next;
    }
    return 0;
}
```
What is the output?

---

### Q64. ★★★ — Structure with Pointer Member
```c
#include <stdio.h>
struct Data {
    int *ptr;
    int val;
};
int main() {
    int x = 100;
    struct Data d = {&x, 200};
    printf("%d %d\n", *(d.ptr), d.val);
    *(d.ptr) = 300;
    printf("%d\n", x);
    return 0;
}
```
What is the output?

---

### Q65. ★★★ — Scoping: Static vs Dynamic [GATE Style]
```c
#include <stdio.h>
int x = 10;
int f() {
    return x;
}
int g() {
    int x = 20;
    return f();
}
int main() {
    printf("%d\n", g());
    return 0;
}
```
What is the output under (a) static scoping (C's rule), and (b) dynamic scoping?

---

## Bonus: Mixed Concept Questions

---

### Q66. ★★★ — Comprehensive [GATE Style]
```c
#include <stdio.h>
int main() {
    int a[] = {1, 2, 3, 4, 5};
    int *p = a;
    ++*p;
    printf("%d ", *p);
    *p++;
    printf("%d ", *p);
    (*p)++;
    printf("%d ", *p);
    printf("\n");
    for (int i = 0; i < 5; i++)
        printf("%d ", a[i]);
    return 0;
}
```
What is the output?

---

### Q67. ★★★ — Pointer Maze [GATE Style]
```c
#include <stdio.h>
int main() {
    int a[3][3] = {{1,2,3},{4,5,6},{7,8,9}};
    int *p = &a[0][0];
    printf("%d %d %d\n", *(p+1), *(p+4), *(p+7));
    printf("%d\n", a[2][1] - a[0][2]);
    return 0;
}
```
What is the output?

---

### Q68. ★★★ — General Concepts MCQ [GATE Style]

Which of the following statements is/are TRUE?

(a) `sizeof` is a runtime operator in C  
(b) `register` variables cannot have their address taken  
(c) `static` local variables are initialized to 0 by default  
(d) `extern` variables are allocated on the stack  

---

## Section H: Unions, Typedef, Enum, Function Pointers, Preprocessor (Q69–Q85)

### Q69. ★ — Union Basics
```c
#include <stdio.h>
union Data {
    int i;
    float f;
    char c;
};
int main() {
    printf("%lu\n", sizeof(union Data));
    union Data d;
    d.i = 42;
    d.f = 3.14;
    printf("%d\n", d.i);
    return 0;
}
```
What is the output? Is the value of `d.i` predictable after assigning to `d.f`?

---

### Q70. ★★ — Struct vs Union Size
```c
struct S { char c; int i; double d; };
union U { char c; int i; double d; };
```
What is `sizeof(struct S)` and `sizeof(union U)` on a 64-bit system with natural alignment?

---

### Q71. ★★ — Typedef Trap
```c
typedef int* intptr;
#define INTPTR int*

intptr a, b;
INTPTR c, d;
```
What are the types of a, b, c, d?

---

### Q72. ★ — Enum Values
```c
enum Fruit { APPLE = 3, BANANA, CHERRY = 1, DATE };
printf("%d %d %d %d\n", APPLE, BANANA, CHERRY, DATE);
```
What is the output?

---

### Q73. ★★ — Function Pointer Basics
```c
#include <stdio.h>
int add(int a, int b) { return a + b; }
int mul(int a, int b) { return a * b; }
int main() {
    int (*fp)(int, int);
    fp = add;
    printf("%d ", fp(3, 4));
    fp = mul;
    printf("%d\n", fp(3, 4));
    return 0;
}
```
What is the output?

---

### Q74. ★★★ — Array of Function Pointers [GATE Style]
```c
int f1(int x) { return x + 1; }
int f2(int x) { return x * 2; }
int f3(int x) { return x * x; }

int main() {
    int (*arr[])(int) = {f1, f2, f3};
    int val = 2;
    for (int i = 0; i < 3; i++)
        val = arr[i](val);
    printf("%d\n", val);
    return 0;
}
```
What is the output?

---

### Q75. ★★ — Macro Side Effects
```c
#define SQUARE(x) ((x) * (x))
int main() {
    int a = 3;
    int b = SQUARE(a++);
    printf("a=%d b=%d\n", a, b);
    return 0;
}
```
What is the output? Why is this problematic?

---

### Q76. ★★ — Macro vs Function
```c
#define MAX(a,b) ((a) > (b) ? (a) : (b))
int max(int a, int b) { return a > b ? a : b; }

int main() {
    int x = 5, y = 3;
    printf("%d\n", MAX(x++, y++));
    printf("x=%d y=%d\n", x, y);
    x = 5; y = 3;
    printf("%d\n", max(x++, y++));
    printf("x=%d y=%d\n", x, y);
    return 0;
}
```
What is the output? Explain the difference.

---

### Q77. ★★ — Stringification and Token Pasting
```c
#define STR(x) #x
#define CONCAT(a, b) a##b

int main() {
    printf("%s\n", STR(Hello World));
    int CONCAT(var, 1) = 100;
    printf("%d\n", var1);
    return 0;
}
```
What is the output?

---

### Q78. ★★★ — Conditional Compilation [GATE Style]
```c
#define X 10
#if X > 5
    int a = X * 2;
#else
    int a = X + 2;
#endif
#undef X
// Is X still defined here?
printf("%d\n", a);
```
What is the output? What happens if you try to use `X` after `#undef`?

---

### Q79. ★★ — Bit Fields
```c
struct Packet {
    unsigned int version : 4;
    unsigned int type    : 4;
    unsigned int length  : 8;
};
int main() {
    struct Packet p = {15, 3, 200};
    printf("v=%u t=%u l=%u\n", p.version, p.type, p.length);
    printf("size=%lu\n", sizeof(struct Packet));
    return 0;
}
```
What is the output? Can you take the address of `p.version`?

---

### Q80. ★★★ — Volatile [GATE Style]
Which of the following is/are TRUE about the `volatile` keyword in C?
(a) It prevents the compiler from optimizing accesses to the variable
(b) It makes the variable thread-safe
(c) It is useful for memory-mapped I/O
(d) A variable can be both `const` and `volatile`

---

### Q81. ★★★ — Const Correctness [GATE Style]
What is the difference between the following declarations?
```c
const int *p1;
int *const p2;
const int *const p3;
```
Which pointer(s) can be reassigned? Through which pointer(s) can the pointed value be modified?

---

### Q82. ★★★ — Complex Declaration [GATE Style]
What does the following declaration mean?
```c
int (*(*fp)(int, int))[10];
```
Use the clockwise/spiral rule to decode.

---

### Q83. ★★★ — Union and Endianness [GATE Style]
```c
union { int i; char c[4]; } u;
u.i = 0x01020304;
printf("%x %x %x %x\n", u.c[0], u.c[1], u.c[2], u.c[3]);
```
What is the output on (a) Little-endian (b) Big-endian machine?

---

### Q84. ★★★ — Function Pointer with typedef [GATE Style]
```c
typedef int (*Operation)(int, int);
int apply(Operation op, int a, int b) { return op(a, b); }
int sub(int a, int b) { return a - b; }
int main() {
    printf("%d\n", apply(sub, 10, 3));
    return 0;
}
```
What is the output?

---

### Q85. ★★★ — Preprocessor Pitfall [GATE 2018 Style]
```c
#define PRINT(x, y) printf("x=%d, y=%d\n", x, y)
int main() {
    int a = 1, b = 2;
    PRINT(a + b, a - b);
    return 0;
}
```
What is the output? Does the `#x` stringification apply here?

---
