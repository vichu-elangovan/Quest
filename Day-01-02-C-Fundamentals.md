# C Programming — Fundamentals Log

**Days Covered:** Day 1 & Day 2
**Topics:** Data Types, Variables, Scope, Storage Classes, Operators, Bitwise Operations

---

## Day 1 — Variables, Data Types & Scope

### 1. What is a C Program?
A C program is a sequence of instructions organized into functions, executed step by step. C is a **procedural language** — execution follows a defined order through function calls, unlike object-oriented languages that organize code around objects.

### 2. Compiler & Header Files
A **compiler** translates C source code into machine-executable code.
```
Source Code (.c) → Compiler → Object Code → Executable
```

**Header files** (`.h`) provide access to pre-built functions via the `#include` directive:
```c
#include <stdio.h>   // enables printf(), scanf()
#include <stdlib.h>  // enables malloc(), free()
```

### 3. Data Types & Format Specifiers

| Type | Format Specifier |
|---|---|
| signed integer | `%d` |
| unsigned integer | `%u` |
| long integer | `%ld` |
| unsigned long integer | `%lu` |
| long long integer | `%lld` |
| unsigned long long integer | `%llu` |

**Key rule:** `sizeof(short) ≤ sizeof(int) ≤ sizeof(long)`

`signed int x;` is equivalent to `int x;` — `int` is signed by default.

### 4. Storage Class Specifiers
Four storage classes control a variable's **scope, lifetime, and default value**:

| Specifier | Behavior |
|---|---|
| `auto` | Default for local variables; garbage value if uninitialized |
| `register` | Suggests storing the variable in CPU register memory (fast access, low capacity) — compiler decides whether to honor this |
| `extern` | Declares a variable defined elsewhere (another file); does NOT allocate memory |
| `static` | Retains value between function calls (not covered in depth yet) |

**Two types of memory:**
- **Register memory** — fast access, limited capacity
- **Secondary memory** — slower access, large capacity

### 5. The `extern` Keyword — Cross-File Variables

```c
// File 1
int a = 5;

// File 2
#include <stdio.h>
extern int a;

int main() {
    printf("%d", a);   // accesses 'a' from File 1 via extern
    return 0;
}
```
**Note:** `extern` only *declares* a variable — it does not define or allocate memory for it. This is useful for sharing variables across multiple files in a project without duplicating memory.

### 6. Scope of a Variable
Scope = the region of code where a variable is accessible and "alive."

```c
int var = 3;   // global variable

int main() {
    int var = 4;        // local variable shadows global
    printf("%d", var);  // prints 4 (local variable wins)
    func();
}

void func() {
    printf("%d", var);  // prints 3 (accesses global, since no local 'var' here)
}
```

**Nested block shadowing:**
```c
int main() {
    int var = 3;
    {
        int var = 4;
        printf("%d", var);  // prints 4 (innermost scope)
    }
    printf("%d", var);  // prints 3 (back to outer scope)
}
```

**Key fact:** An uninitialized **global** variable automatically defaults to `0`. An uninitialized **local** variable holds a garbage/indeterminate value.

### 7. Macros (`#define`)

```c
#define PI 3.14159
#define add(x, y) ((x) + (y))

int main() {
    printf("%f", PI);        // Output: 3.14159
    printf("%d", add(3, 4)); // Output: 7
    return 0;
}
```

Macros can also implement simple conditional logic:
```c
#define greater(x, y) if (x > y) printf("%d is greater than %d", x, y); \
                      else printf("%d is lesser than %d", x, y);

int main() {
    greater(5, 6);   // Output: 5 is lesser than 6
    return 0;
}
```

### 8. Predefined Compiler Macros
```c
printf("Date: %s, Time: %s", __DATE__, __TIME__);
```
`__DATE__` and `__TIME__` are predefined by the compiler and expand to the current compilation date/time.

### 9. Number Base Quirks

**Octal trap:** A leading `0` before a number makes it **octal**, not decimal.
```c
int var = 052;
printf("%d", var);   // Output: 42 (NOT 52)
```
Conversion: `052` in octal = 5×8¹ + 2×8⁰ = 40 + 2 = **42**

**Hexadecimal:** Prefixed with `0x`.
```c
int var = 0x43FF;   // interpreted as a hexadecimal value
```

---

## Day 2 — Operators

### 1. Operator Categories

| Type | Operators |
|---|---|
| Arithmetic | `+ - * / %` |
| Relational | `< > == !=` |
| Increment/Decrement | `++ --` |
| Logical | `&& \|\| !` |
| Bitwise | `& \| ^ ~ << >>` |
| Assignment | `= += -= *=` |
| Other | `?:` (ternary), `&` (address-of), `*` (pointer), `sizeof()`, `,` (comma) |

### 2. Pre-increment vs Post-increment

```c
int a = 5;
int x = ++a;   // pre-increment: increment FIRST, then assign
// a becomes 6, x = 6

int a = 6;
int x = a++;   // post-increment: assign FIRST, then increment
// x = 6, a becomes 7
```

### 3. Logical Operators & Short-Circuit Evaluation

```c
int a = 5, b = 3, result;

result = (a > b) && (b++);
// a>b is true(1); since && requires BOTH operands, b++ IS evaluated
// b++ returns 3 (pre-increment value, nonzero = true)
// result = 1 && 3 = 1 (true)
// After this line: b = 4
printf("%d", result);  // Output: 1
printf("%d", b);       // Output: 4
```

```c
int a = 5, b = 3, result;

result = (a > b) || (b++);
// a>b is true(1); since || only needs ONE true operand,
// b++ is NEVER evaluated (short-circuit)
// result = 1
printf("%d", result);  // Output: 1
printf("%d", b);       // Output: 3 (unchanged — short-circuited)
```

**Key rule:** In `&&`, if the first operand is false, the second is never evaluated. In `||`, if the first operand is true, the second is never evaluated.

### 4. Bitwise Operators

**AND (`&`)** — 1 only if both bits are 1
**OR (`|`)** — 1 if either bit is 1
**XOR (`^`)** — 1 if bits are different (not both same)
**NOT (`~`)** — flips every bit (one's complement)

```c
7 & 4:
  0111
& 0100
------
  0100  = 4

7 | 4:
  0111
| 0100
------
  0111  = 7

~7 (assuming 8-bit signed):
  00000111  →  flip all bits  →  11111000
  Interpreted as 2's complement signed value = -8
```

### 5. Bit Shift Operators

**Left shift (`<<`)** = multiplication by 2ⁿ
**Right shift (`>>`)** = division by 2ⁿ (right operand)

```c
int var = 3;
var << 1;   // 3 × 2¹ = 6
var << 4;   // 3 × 2⁴ = 48

int var = 32;
var >> 4;   // 32 ÷ 2⁴ = 2
```

**Mechanism:** Left shift removes the leftmost bit and fills the emptied space on the right with 0. Right shift removes the rightmost bit and fills the emptied space on the left with 0.

### 6. XOR Swap — Swapping Without a Temporary Variable

```c
#include <stdio.h>

int main() {
    int a = 5, b = 4;

    a = a ^ b;   // a = 5^4
    b = a ^ b;   // b = (5^4)^4 = 5
    a = a ^ b;   // a = (5^4)^5 = 4

    printf("a=%d and b=%d", a, b);  // Output: a=4 and b=5
    return 0;
}
```
This works because XOR is its own inverse: `(a^b)^b = a` and `(a^b)^a = b`.

### 7. Ternary (Conditional) Operator
The **only ternary operator** in C — an alternate, compact form of if-else.

```c
result = (marks >= 33) ? 'P' : 'F';
// if marks >= 33, result = 'P', else result = 'F'

int result = 0 ? 1 : 2;
// 0 is false, so result = 2
```

### 8. Comma Operator
- Can be used as an operator within an expression.
- Evaluates all expressions left to right, but only the **rightmost value** is retained.
- Has the **lowest precedence** of all operators.

```c
int a;
(a = 3), 5, 4;
printf("%d", a);   // Output: 3 (assignment happened before the comma sequence)

int x = (5, 3, 8, 1);
printf("%d", x);   // Output: 1 (rightmost value in the comma sequence)
```

**Important:** `int a = 3, 5, 4;` is **invalid syntax** — in a declaration, commas separate variable names, not expression values. Parentheses are required to invoke actual comma-operator behavior: `int a = (3, 5, 4);`

---

## Self-Check Questions
1. Why does `052` evaluate to 42 instead of 52?
2. What is the difference between pre-increment and post-increment?
3. Why does `(a > b) && (b++)` increment `b`, but `(a > b) || (b++)` does not?
4. Trace through the XOR swap manually for `a=7, b=3`.
5. What value does `x` hold after `int x = (2, 4, 6, 8);`, and why?
