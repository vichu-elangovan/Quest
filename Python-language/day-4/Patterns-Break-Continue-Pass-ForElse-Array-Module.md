# Day 4 — Patterns, Loop Control (break/continue/pass), for-else, Array Module

**Topics:** Nested Loop Patterns, `break`, `continue`, `pass`, `for...else`, Prime Number
Check, The `array` Module, Multi-Dimensional Arrays (NumPy)

---

## 1. Patterns with Nested Loops

### Pattern 1 — Simple 3x3 Grid
```python
for i in range(3):
    for j in range(3):
        print("#", end=" ")
    print()   # move to a new line after each row
```
**Output:**
```
# # #
# # #
# # #
```

### Pattern 2 — Increasing Triangle
```python
for i in range(3):
    for j in range(i + 1):
        print("#", end=" ")
    print()
```
**Trace:**
| `i` | `range(i+1)` | Output row |
|---|---|---|
| 0 | `range(1)` → `j=0` | `#` |
| 1 | `range(2)` → `j=0,1` | `# #` |
| 2 | `range(3)` → `j=0,1,2` | `# # #` |

**Output:**
```
#
# #
# # #
```
**Key idea:** the inner loop's range depends on `i`, so each row prints one more `#` than
the row before it.

### Pattern 3 — Decreasing Row Width
```python
for i in range(3):
    for j in range(3 - i):
        print("#", end=" ")
    print()
```
**Output:**
```
# # #
# #
#
```
Here the inner range **shrinks** as `i` increases (`3-i`), producing a downward-narrowing
triangle instead of a widening one.

---

## 2. Loop Control Statements

### `break` — Terminates the Loop Entirely
```python
i = 0
while i <= 5:
    if i == 3:
        break
    print(i)
```
**Output:**
```
0
1
2
```
Once `i == 3`, the loop stops immediately — `3, 4, 5` are never printed.

### `continue` — Skips the Current Iteration
```python
for i in range(1, 30, 1):
    if i % 3 == 0:
        continue
    print(i)
```
Every number divisible by 3 is **skipped**, but the loop keeps running for all other
numbers — unlike `break`, the loop doesn't stop.

### `pass` — Does Nothing (Placeholder)
```python
for i in range(1, 10):
    if i % 2 == 0:
        pass
    else:
        print(i)
```
**Output:**
```
1
3
5
7
9
```
**Why use `pass`?** Sometimes you know a condition needs to exist in your code, but you
haven't decided what to do inside it yet. Instead of leaving that block syntactically
empty (which causes an error in Python), `pass` acts as a placeholder that does nothing —
letting you fill it in later without breaking the code now.

### `break` vs `continue` vs `pass` — Side-by-Side Comparison

**`pass`** — simply passes over that particular condition, but the **full loop keeps
running normally**:
```python
for i in range(1, 10):
    if i % 2 == 0:
        pass
    else:
        print(i)
# Output: 1 3 5 7 9
```

**`continue`** — skips only the current iteration and moves to the next one:
```python
for i in range(1, 10):
    if i == 3:
        continue
    print(i)
# Output: 1 2 4 5 6 7 8 9  (3 is skipped, loop continues)
```

**When would you actually use `pass`?** Suppose there's a situation where you don't yet
know what to write inside a condition or function body — you can simply write `pass` for
now, and fill in the real logic later, without the code throwing a syntax error in the
meantime.

---

## 3. `for...else` — A Python-Specific Construct

The `else` block after a `for` loop runs **only if the loop completes without hitting a
`break`**.

### Example 1 — Match Found (else is SKIPPED)
```python
nums = [12, 16, 20, 21, 17]
for i in nums:
    if i % 5 == 0:
        print(i)
# Output: 20
```

### Example 2 — No Match Found (else RUNS)
```python
nums = [12, 16, 21, 17]
for i in nums:
    if i % 5 == 0:
        print(i)
else:
    print("Not found")
# Output: Not found
```
**Why does "Not found" print only once?** Because the `else` block is **outside the loop**
— it's tied to the loop's completion, not to each iteration. It only executes if the loop
finishes normally (i.e., never hit a `break`), which is why it doesn't print repeatedly.

**Key idea:** `for...else` is a combination of `for` and `else` — think of it as "if the
loop never found what it was looking for (never broke out early), then do this."

---

## 4. Prime Number Check

```python
num = 10
for i in range(2, num):
    if num % i == 0:
        print("Not Prime")
        break
else:
    print("Prime")
```
**Logic:** loop through every number from `2` to `num-1`, checking if any of them evenly
divides `num`. If one does, it's not prime — print immediately and `break`. If the loop
finishes without ever breaking (no divisor found), the `else` block runs and confirms it's
prime.

**This is a textbook use case for `for...else`** — the `else` naturally represents "the
loop never found a reason to say otherwise."

---

## 5. The `array` Module

Python's plain `list` works differently from a strictly-typed array. The `array` module
gives you a more C-like, single-type array.

### Three Ways to Import and Use It
```python
# Method 1
import array
arr = array.array([])

# Method 2 — import with an alias
import array as a
vd = a.array([])

# Method 3 — import everything directly (no prefix needed)
from array import *
vds = array([])
```

### Declaring a Typed Array
```python
from array import *
vals = array('i', [5, 6, 7, 8, 9])
print(vals)
# Output: array('i', [5, 6, 7, 8, 9])
```
The `'i'` is a **type code** — it tells Python what kind of data the array will hold.

### Type Codes Table

| Type | Code | Python Type | Min Size (bytes) |
|---|---|---|---|
| Signed char | `'b'` | int | 1 |
| Unsigned char | `'B'` | int | 1 |
| Unicode char | `'u'` | unicode character | 2 |
| Signed short | `'h'` | int | 2 |
| Unsigned short | `'H'` | int | 2 |
| Signed int | `'i'` | int | 2 |
| Unsigned int | `'I'` | int | 2 |
| Signed long | `'l'` | int | 4 |
| Unsigned long | `'L'` | int | 4 |
| Float | `'f'` | float | 4 |
| Double | `'d'` | float | 8 |

### Important — Arrays Are Strictly Typed
```python
vals = array('i', [5.6, 7, 8, 9])
print(vals)
# ERROR — due to the float value, but 'i' only stores integers
```
Unlike a plain Python `list` (which can freely mix types), an `array` with type code `'i'`
will **reject** a float value — the type code is enforced.

---

## 6. Common Array Operations

```python
vals.buffer_info()
# (2780..., 5)   -> (memory address, size)

vals.append(2)        # adds to the end
vals.reverse()         # reverses the array in place
vals.insert(1, 15)     # inserts value 15 at index 1
vals.remove(7)         # removes the first occurrence of value 7
vals.pop()             # removes and returns the last element

print(vals.count(1))   # counts occurrences of the value 1
```

### `del` — Deleting a Slice
```python
del vals[1:3]
```
**Important note:** Python's `array` module has **no dedicated "delete" function** —
`del` is a general Python keyword (not an array method) used to remove elements by index
or slice, or even to delete the entire array object.

---

## 7. Getting User Input for an Array

```python
from array import *

arr = array('i', [])
n = int(input("no. of elements: "))

for i in range(n):
    x = int(input("Enter the value: "))
    arr.append(x)

print(arr)
```
**Logic:** start with an empty typed array, ask the user how many elements they want to
enter, then loop that many times, prompting for and appending each value.

### Searching for a Value
```python
y = int(input("Enter the number to be found: "))
print(arr.index(y))
```
`.index(value)` returns the **position** of the first occurrence of that value in the
array (raises an error if the value isn't found).

---

## 8. Multi-Dimensional Arrays

Python's built-in `array` module does **not support multi-dimensional arrays** directly.

```python
from array import *
arr = array('i', [1,2,3,4,5], [6,7,8,9,10])
print(arr)
# ERROR
```

**Why the error?** We're using an external package's basic single-dimension array type,
which doesn't support nested/multi-dimensional structures — for that, we need an external
package that supports multi-dimensional arrays, which is **NumPy**.

### Using NumPy Instead
```python
from numpy import *
arr = array([1, 2, 3, 4, 5])
print(arr)
# Output: [1 2 3 4 5]
```
**Key difference:** in NumPy, you do **not need to specify a type code** — it infers or
handles the data type automatically, and importantly, it **does support** true
multi-dimensional arrays (2D, 3D, etc.), which is exactly what the built-in `array` module
lacks.

---

