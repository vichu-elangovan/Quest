# Day 2 — Operators

**Topics:** Arithmetic, Relational, Logical, Bitwise, Shift, Ternary, Comma Operators

---

## 1. Operator Categories

| Type | Operators |
|---|---|
| Arithmetic | `+ - * / %` |
| Relational | `< > == !=` |
| Increment/Decrement | `++ --` |
| Logical | `&& \|\| !` |
| Bitwise | `& \| ^ ~ << >>` |
| Assignment | `= += -= *=` |
| Other | `?:` (ternary), `&` (address-of), `*` (pointer), `sizeof()`, `,` (comma) |

## 2. Pre-increment vs Post-increment

```c
int a = 5;
int x = ++a;   // pre-increment: increment FIRST, then assign
// a becomes 6, x = 6

int a = 6;
int x = a++;   // post-increment: assign FIRST, then increment
// x = 6, a becomes 7
```

## 3. Logical Operators & Short-Circuit Evaluation

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

## 4. Bitwise Operators

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

## 5. Bit Shift Operators

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

## 6. XOR Swap — Swapping Without a Temporary Variable

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

## 7. Ternary (Conditional) Operator
The **only ternary operator** in C — an alternate, compact form of if-else.

```c
result = (marks >= 33) ? 'P' : 'F';
// if marks >= 33, result = 'P', else result = 'F'

int result = 0 ? 1 : 2;
// 0 is false, so result = 2
```

## 8. Comma Operator
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
1. What is the difference between pre-increment and post-increment?
2. Why does `(a > b) && (b++)` increment `b`, but `(a > b) || (b++)` does not?
3. Trace through the XOR swap manually for `a=7, b=3`.
4. What value does `x` hold after `int x = (2, 4, 6, 8);`, and why?
