# Day 5 — NumPy Arrays, Matrices, Functions, Call by Value/Reference

**Topics:** NumPy Copy vs View, Multi-Dimensional Arrays, `linspace()`/`logspace()`,
`zeros()`/`ones()`, Array Arithmetic, Matrix Operations, Functions, Multiple Return
Values, Call by Value vs Call by Reference, Variable-Length Arguments (`*args`)

---

## 1. NumPy — Copying Arrays Correctly (`copy()` vs `view()`)

### The Problem — Sharing Memory Unintentionally
```python
from numpy import *

arr1 = array([1, 2, 3])
arr2 = arr1.view()   # view() does NOT create an independent copy

arr1[0] = 100
print(arr1)   # [100, 2, 3]
print(arr2)   # [100, 2, 3]  <- changed too!
```
**Why?** `view()` creates a new array object, but it still points to the **same underlying
memory/data** as the original. Changing one changes the other, since they're really just
two "windows" onto the same data.

### The Fix — `copy()` Creates a Truly Independent Array
```python
arr1 = array([2, 3, 7])
arr2 = arr1.copy()   # creates a completely separate copy in memory

print(id(arr1))
print(id(arr2))
# Different memory addresses — arr1 and arr2 are now independent
```
**Key idea:** use `.copy()` whenever you need a genuinely separate array; use `.view()`
only when you deliberately want two references to the same underlying data.

---

## 2. Multi-Dimensional Arrays with NumPy

```python
from numpy import *

arr1 = array([[1, 2, 3],
              [4, 5, 6]])
```

### Useful Array Properties

```python
arr1.dtype
# int32   -> the data type NumPy is storing internally

arr1.ndim
# 2       -> number of dimensions (this is a 2D array)

arr1.shape
# (2, 3)  -> 2 rows, 3 columns

arr1.size
# 6       -> total number of elements
```

### Flattening — Converting to 1D
```python
arr2 = arr1.flatten()
print(arr2)
# [1 2 3 4 5 6]
```
`.flatten()` collapses a multi-dimensional array down into a single 1D array.

### Reshaping — Changing Dimensions
```python
arr2 = arr1.reshape(3, 2)   # reshape into 3 rows, 2 columns
arr3 = arr2.reshape(2, 3, 1)  # reshape into a 3D array
```
**Key idea:** `reshape()` requires the total number of elements to stay the same — you're
only rearranging how they're grouped into dimensions, not adding or removing data.

---

## 3. `linspace()` and `logspace()`

### `linspace(start, stop, num)` — Evenly Spaced Values
- `start` → first value
- `stop` → last value (inclusive)
- `num` → total number of values to generate

```python
linspace(1, 10, 10)
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
Generates `num` values, evenly spaced between `start` and `stop`.

```python
linspace(1, 20, 15)
# 15 equally spaced values between 1 and 20
# (the spacing between consecutive values is (20-1)/(15-1), not a whole number)
```

### `logspace(start, stop, num)` — Logarithmically Spaced Values
```python
logspace(1, 10, 2)
# values spaced using powers of 10, not a linear difference
```
**Key difference:** in `linspace`, the difference between consecutive numbers is
**constant** (linear scale). In `logspace`, the values are spaced so the **ratio**
between consecutive numbers is constant (logarithmic scale) — useful when values span
several orders of magnitude.

---

## 4. `zeros()` and `ones()`

```python
zeros(5)
# [0. 0. 0. 0. 0.]

zeros(5, int)
# [0 0 0 0 0]   -> integer type instead of float

ones(5)
# [1. 1. 1. 1. 1.]

ones(5, int)
# [1 1 1 1 1]
```
Both create arrays pre-filled with a single repeated value — useful as a starting point
before filling in real data (e.g. initializing counters or placeholders).

---

## 5. Array Arithmetic (Broadcasting)

### Adding a Scalar to Every Element
```python
arr = array([1, 2, 3, 4, 5])
arr = arr + 5
print(arr)
# [6, 7, 8, 9, 10]
```
NumPy automatically applies `+5` to **every element** — this is called **broadcasting**,
and it's a major reason NumPy arrays are more convenient than plain Python lists for
numeric work (a plain list would need a loop to do this).

### Adding Two Arrays Together
```python
arr1 = array([1, 2, 3, 4, 5])
arr2 = array([6, 7, 8, 9, 10])
arr3 = arr1 + arr2
print(arr3)
# [7, 9, 11, 13, 15]
```
Element-wise addition — each position in `arr1` is added to the corresponding position
in `arr2`.

---

## 6. Matrix Operations

### Creating a Matrix
```python
m = matrix([[1, 2, 3], [4, 5, 6]])
print(m)
```

### Using String Notation (semicolons separate rows)
```python
m = matrix('1,2,3; 4,5,6')
print(m)
# equivalent to matrix([[1,2,3],[4,5,6]])
```
**Key idea:** the semicolon (`;`) in string notation marks where one row ends and the
next begins.

### Matrix Addition
```python
m3 = m1 + m2
print(m3)
```

### Matrix Multiplication
```python
m3 = m1 * m2
print(m3)
```
**Important:** for `matrix` objects, `*` performs actual **matrix multiplication**
(following linear algebra rules), unlike plain NumPy `array` objects where `*` would do
element-wise multiplication instead.

---

## 7. Functions

**Purpose:** dividing a bigger problem into smaller, manageable sub-problems.

### Basic Syntax
```python
def function_name():
    statement
```

### Simple Example
```python
def vishu():
    print('Achu Lachu Abi')

vishu()
# Output: Achu Lachu Abi
```

### Function with Parameters
```python
def add(x, y):
    c = x + y
    print(c)

add(5, 4)
# Output: 9
```

---

## 8. Returning Multiple Values

```python
def add_sub(x, y):
    c = x + y
    d = x - y
    return c, d

res1, res2 = add_sub(5, 4)
print(res1, res2)
# Output: 9 1
```
**Key idea:** Python lets a function return multiple values at once as a tuple, which can
then be unpacked directly into separate variables on the calling side.

---

## 9. Call by Value vs Call by Reference

### i. Call by Value — for Immutable Objects (int, float, string, tuple)

```python
def update(x):
    x = 8
    print(x)

a = 10
update(a)
print('a =', a)
```
**Output:**
```
8
a = 10
```
Even though `x` was changed to `8` *inside* the function, `a` outside remains `10`.
**Why?** Immutable objects (int, float, string, tuple) behave like call by value — the
function receives its own independent copy of the reference, so reassigning `x` inside
the function has no effect on `a` outside.

### ii. Call by Reference — for Mutable Objects (list, dict, set)

```python
def update(x):
    x[1] = 8
    print(id(x))

my_list = [10, 20, 30]
update(my_list)
print('list =', my_list)
```
**Output:**
```
[10, 8, 30]
```
This time, the change made **inside** the function is reflected **outside** it too.

**Why the difference?** In Python, what's actually passed to a function is a **reference**
to the object — not a full independent copy. For an immutable object, "changing" it (like
`x = 8`) actually creates a brand-new object and rebinds `x` to it, leaving the original
object (and the caller's variable) untouched. But for a mutable object like a list,
**modifying its contents in place** (like `x[1] = 8`) changes the actual underlying data
that both the function's `x` and the caller's `my_list` are pointing to — so the change is
visible outside the function as well.

**Summary rule:** Python doesn't have separate "call by value" and "call by reference"
modes the way C does — it always passes references. What changes is whether the object
itself is mutable or immutable, which determines whether in-place modifications are
visible outside the function.

---

## 10. Variable-Length Arguments — `*args`

Sometimes you don't know in advance how many arguments a function will need to accept.
`*args` lets a function capture **any number** of positional arguments into a single tuple.

```python
def odd(*nums):
    for i in nums:
        if i % 2 != 0:
            print(i)

odd(10, 20, 30)
# (no output — none are odd)

odd(1, 2, 3, 4, 5)
# Output:
# 1
# 3
# 5
```
**Key idea:** inside the function, `nums` behaves like a regular tuple containing
whatever values were passed in — however many there are. This is what allows the same
`odd()` function to be called with 3 arguments in one place and 5 arguments in another,
without needing to define multiple versions of the function.

