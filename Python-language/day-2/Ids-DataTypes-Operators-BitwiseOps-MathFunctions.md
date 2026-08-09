# Day 2 — Object IDs, Data Types, Operators, Bitwise Operations, Math Functions

**Topics:** Object Identity (`id()`), Constants in Python, `type()`, Data Types, Operators,
Number Base Conversion, Bitwise Operators, Math Module Functions

---

## 1. Object Identity — `id()`

`id()` returns the **memory address** of an object.

```python
prog = [4, 5, 6]

id(prog)      # e.g. 200...  — address of the whole list object
id(prog[1])   # address of the SECOND element in the list ('5')
```

### Determining the Address

```python
name = 'achu'
id(name)              # address of the string object
id('lachu')           # a different address (different object)
```

```python
num = 10
id(num)               # 91234567...

a, b = 10, 10
id(a)                 # 91578...
id(b)                 # SAME as id(a) — small integers are cached/interned by Python

# a and b refer to the SAME object in memory
```

**Key idea:** for small integers (a common CPython optimization), Python reuses the same
object in memory rather than creating a new one each time — so two variables holding the
same small integer value will often share the same `id()`.

---

## 2. Constants

In Python, there is **no `const` keyword** like in C or C++. Python doesn't enforce
true immutability of variable bindings at the language level.

---

## 3. `type()` — Checking a Variable's Type

```python
num = 9
type(num)
# <class 'int'>
```
`type()` returns the type/class of the variable.

---

## 4. Data Types

| # | Type | Notes |
|---|---|---|
| i | **None** | a variable not assigned any value — similar to `NULL`, but in Python it's called `None` |
| ii | **Numeric** | `int`, `float`, `complex`, `bool` |
| iii | **List** | |
| iv | **Tuple** | |
| v | **Set** | |
| vi | **String** | |
| vii | **Dictionary** | |
| viii | **Range** | |

**Important:** Python does **not have a `char` type** — a single character is just a
string of length 1.

### Range
```python
range(10)
# range(0, 10)

list(range(0, 5))
# [0, 1, 2, 3, 4]
```

### Numeric Type Conversions
```python
a = 5 - 6      # -1
b = int(a)     # -1  (already an int, no visible change here)
b              # -1

k = float(b)   # -1.0
k              # -1.0

k = 6
c = complex(b, k)   # combines two numbers into a complex number
c                   # (-1+6j)

bool = b < k
bool           # True
```

---

## 5. Operators

### i. Assignment Operators
```python
x = 2
x += 2     # x = x + 2
x *= 2     # x = x * 2
x /= 2     # x = x / 2

a, b = 5, 6    # multiple assignment in a single line
```

### ii. Unary Operators
```python
m = 7
-m       # -7
+m       # 7   (rarely needed, but valid)
m = -m   # flips the sign of m
```

### iii. Arithmetic Operators
`+`, `-`, `/`, `%`, `*`

### iv. Relational Operators
`<`, `>`, `!=`, `==`, `<=`, `>=`

### v. Logical Operators

**`and`** — both conditions must be true:
```python
a, b = 5, 4
(a < 8) and (b < 5)   # True
```

**`or`** — at least one condition must be true:
```python
a, b = 5, 4
(a > 6) or (b < 5)   # True
```

**`not`** — inverts a boolean:
```python
x = True
not x   # False
```

---

## 6. Number Base Conversion

```python
bin(25)    # '0b11001'   — binary representation (0b prefix)
oct(25)    # '0o31'      — octal representation
hex(10)    # '0xa'       — hexadecimal representation
```

---

## 7. Bitwise Operators

### AND (`&`), OR (`|`), XOR (`^`)
```python
12 & 13    # 12
12 | 13    # 13
12 ^ 13    # 1
```

**Manual binary trace:**
```
AND:  00001100
    & 00001101
    ----------
      00001100  = 12

OR:   00001100
    | 00001101
    ----------
      00001101  = 13

XOR:  00001100
    ^ 00001101
    ----------
      00000001  = 1
```

### Left Shift (`<<`) and Right Shift (`>>`)

**Left shift** = multiply by `2^n`:
```python
12 << 1
# x << n  is equivalent to  x * 2^n
# 12 * 2^1 = 24
```

**Right shift** = divide by `2^n`:
```python
20 >> 1
# x >> n  is equivalent to  x / 2^n
# 20 / 2^1 = 10
```

### Complement (`~`) — Bitwise NOT

```python
~12
# -13
```

**Formula:** `~x = -(x + 1)`
```python
~12 = -(12 + 1) = -13
```

**Why?** The complement operator inverts every bit (all `0`s become `1`s and vice versa).

**Manual trace for `~12`:**
```
12 in binary:        00001100

Take 1's complement (flip every bit):
                      11110011

If the leftmost (sign) bit is 1, the number is negative.
To find its decimal value:
  1. Subtract 1 from the flipped bits:
       11110011
     -        1
     ----------
       11110010

  2. Take the 1's complement again (flip back):
       00001101

  3. Convert to decimal using place values (2^3 2^2 2^1 2^0 = 8 4 2 1):
       8 + 4 + 1 = 13

  4. Since we determined the original was negative, attach the negative sign:
       Result = -13
```

---

## 8. Math Module Functions

To use most math functions, you must **import the `math` module** first.

```python
sqrt(25)          # ERROR — sqrt is not built-in by default
```

### Correct Usage
```python
import math

x = math.sqrt(25)
x
# 5.0
```

### `ceil()` — Rounds UP to the Maximum Value
```python
math.ceil(2.4)
# 3
```

### `floor()` — Rounds DOWN to the Minimum Value
```python
math.floor(2.4)
# 2
```

### `pow()` — Power/Exponent
```python
print(math.pow(3, 2))
# 9.0
```

### Shortening the Module Name with `as`
```python
import math as m

m.sqrt(25)
# 5.0
```
`m` can now be used in place of `math` everywhere — reduces repetitive typing.

### Importing Specific Functions Directly
```python
from math import sqrt, pow

pow(4, 5)
# 1024.0
```
When importing specific functions this way, you no longer need to prefix them with
`math.` (or `m.`) at all — they can be called directly by name.

---


5. Why does `12 << 1` give `24`, and what general formula explains this relationship for
   any left shift by `n` bits?
