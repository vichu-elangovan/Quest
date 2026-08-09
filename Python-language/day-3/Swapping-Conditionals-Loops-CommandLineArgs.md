# Day 3 — Swapping, Conditional Statements, Loops, Command Line Arguments

**Topics:** Swapping Two Numbers, `if-elif-else`, `while` Loop, `for` Loop, `for...in range()`,
Type Conversion with `input()`, Command Line Arguments (`sys.argv`), `eval()`

---

## 1. Swapping Two Numbers

### Method 1 — Using a Temporary Variable
```python
a = 5
b = 6

temp = a
a = b
b = temp

print(a)   # 6
print(b)   # 5
```
Straightforward: store `a`'s value in `temp` before overwriting it, so it isn't lost.

### Method 2 — Without a Temporary Variable
```python
a = 5
b = 6

a = a + b   # a = 11
b = a - b   # b = 11 - 6 = 5
a = a - b   # a = 11 - 5 = 6

print(a)    # 6
print(b)    # 5
```

**Trace:**
| Step | Expression | Value |
|---|---|---|
| Start | `a=5, b=6` | |
| `a = a+b` | `5+6` | `a=11` |
| `b = a-b` | `11-6` | `b=5` |
| `a = a-b` | `11-5` | `a=6` |

**Careful with this method:** it works for numbers, but relies on arithmetic — using it
with very large numbers can risk overflow in other languages (less of a concern in Python,
but worth knowing for languages like C).

### Method 3 — Pythonic One-Liner
```python
a = 5
b = 6

a, b = b, a

print(a)   # 6
print(b)   # 5
```
Python allows direct **tuple-style swapping** — the right-hand side `(b, a)` is evaluated
first as a pair, then unpacked into `a, b` simultaneously. This is the cleanest and most
"Pythonic" way to swap two values, with no temp variable and no arithmetic involved.

---

## 2. Conditional Statements

### Basic `if-else`
```python
x = 3

if (x == x):
    print("Even")
else:
    print("Odd")
```

### `if-elif-else`
```python
x = 2

if (x == 3):
    print("Even")
elif (condition):
    print("statement")
else:
    print("statement")
```
**Structure:**
```
if condition:
    statement
elif condition:
    statement
else:
    statement
```

### Nested `if`
```python
if condition:
    if condition:
        statement
    else:
        statement
```

**Key syntax reminder:** Python uses **indentation** (not braces `{}`) to define code
blocks — the indented lines under `if`/`elif`/`else` are what belong to that branch.

---

## 3. Loops

### `while` Loop
```python
while condition:
    statement
    i++   # increment (conceptually — Python uses i += 1, no ++ operator)
```

**Example — printing 1 to 5:**
```python
i = 1
while i <= 5:
    print("Hi")
    i += 1
```
**Output:**
```
Hi
Hi
Hi
Hi
Hi
```

### `for` Loop with `range()`
```python
for i in range(1, 6):
    print(i)
```
**Output:** `1 2 3 4 5`

**Key idea:** `range(1, 6)` generates numbers starting at `1` up to (but not including) `6`
— this is the standard Pythonic replacement for a C-style counting loop.

---

## 4. `input()` and Type Conversion

By default, `input()` always returns a **string**, even if the person types a number.

```python
x = input("Enter string 1: ")
y = input("Enter string 2: ")
z = x + y
print(z)
```
**Output for inputs `"Navin"` and `"65"`:**
```
Navin65
```
This is **string concatenation**, not addition — because both `x` and `y` are strings.

### Fixing It — Convert to `int` First
```python
x = int(input("Enter string 1: "))
y = int(input("Enter string 2: "))
z = x + y
print(z)
```
**Output for inputs `5` and `72`:**
```
77
```
Now `x` and `y` are proper integers, so `+` performs actual addition.

**Key lesson:** always wrap `input()` with `int()`, `float()`, etc. when you need numeric
behavior instead of string concatenation — this is one of the most common early Python bugs.

---

## 5. Command Line Arguments — `sys.argv`

**Use case:** passing values to a Python script directly from the terminal, without using
`input()` interactively.

```python
import sys

x = int(sys.argv[1])
y = int(sys.argv[2])
z = x + y
print(z)
```

**Running from the terminal:**
```
python main.py 6 2
```
**Output:**
```
8
```

**How it works:**
- `sys.argv` is a **list** containing the command-line arguments used to invoke the script.
- `sys.argv[0]` is always the script name itself (e.g. `main.py`).
- `sys.argv[1]`, `sys.argv[2]`, etc. are the actual arguments passed in, **as strings** —
  hence the need to wrap them in `int()` when doing arithmetic.

---

## 6. `eval()` — Evaluating an Expression from a String

`eval()` takes a string and evaluates it as if it were actual Python code.

```python
ses = eval(input("Enter an expression: "))
print(ses)
```

**Example run:**
```
Enter an expression: 6+7-2
11
```
Instead of just reading the input as a plain string, `eval()` actually **computes** the
expression written inside it.

**Caution (good practice to know, even if not explicitly noted in class):** `eval()` runs
arbitrary code, so it should never be used on untrusted input in real applications — it's
fine for learning exercises like this, but risky in production code.

---

## 7. `for...in range()` — More Examples

### Iterating Over a String
```python
x = 'NAVIN'
for i in x:
    print(i)
```
**Output:**
```
N
A
V
I
N
```

### Iterating Over a List of Mixed Types
```python
for i in [2, 6, 'paul']:
    print(i)
```
**Output:**
```
2
6
paul
```

### Basic `range()` Forms
```python
for i in range(10):     # 0 to 9
    print(i)

for i in range(1, 21, 2):   # start=1, stop=21 (exclusive), step=2
    print(i)
# 1, 3, 5, 7, ... 19 (odd numbers)

for i in range(20, 11, -1):  # start=20, stop=11 (exclusive), step=-1
    print(i)
# 20, 19, 18, ... 12 (reverse order / countdown)
```

**Key idea:** `range(start, stop, step)` — the `step` value can be **negative**, which is
how you count *downward* instead of upward. Omitting `step` defaults to `1`.

### Printing Perfect Square Numbers
```python
for i in range(1, 501):
    if (i ** 0.5) % 1 == 0:
        print(i)
```
**Logic:** `i ** 0.5` computes the square root of `i`. If the square root has **no
fractional part** (i.e. `% 1 == 0`), then `i` is a perfect square.

---

