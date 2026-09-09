# Day 12 — Linear Search, Binary Search, File Handling

**Topics:** Linear Search, Binary Search, Threading Recap (`start()`/`join()`), File
Handling (open/read/write/append modes, `with` statement), Compiled vs Interpreted
Python

---

## 1. Linear Search

**Logic:** check every element one by one, from the start, until a match is found (or the
list ends).

### Version 1 — Inline
```python
arr = [10, 20, 30, 40]
m = int(input("Enter element to be searched: "))

for i in range(len(arr)):
    if arr[i] == m:
        print("Element found at pos:", i + 1)
        break
else:
    print("Not found")
```

### Version 2 — As a Reusable Function
```python
def linear(arr, m):
    for i in range(len(arr)):
        if arr[i] == m:
            return i
    return -1

m = int(input())
arr = [10, 20, 30, 40, 50]
result = linear(arr, m)

if result != -1:
    print("Element found at position:", result + 1)
else:
    print("Element not found")
```

**Key idea:** the function version returns `-1` as a **sentinel value** when nothing is
found, which the caller checks against — this is a common pattern for search functions
across nearly every language, not just Python.

**Time complexity:** O(n) — in the worst case, every element must be checked.

---

## 2. Binary Search

**Requires the array to be sorted.** Instead of checking every element, it repeatedly
splits the search range in half, eliminating half the remaining elements each step.

```python
def binary(arr, m):
    low = 0
    high = len(arr) - 1

    while low <= high:
        mid = (low + high) // 2

        if arr[mid] == m:
            return mid
        elif m < arr[mid]:
            high = mid - 1
        else:
            low = mid + 1

    return -1

arr = [10, 20, 30, 40]
m = int(input(""))
result = binary(arr, m)

if result != -1:
    print("Found at pos:", result + 1)
else:
    print("Not found")
```

### How It Works, Step by Step
1. Start with `low = 0` and `high = len(arr) - 1` — the full range of valid indices.
2. While `low <= high`, compute the middle index: `mid = (low + high) // 2`.
3. **If `arr[mid] == m`** → found it, return `mid`.
4. **If `m < arr[mid]`** → the target must be in the **left half**, so narrow the range:
   `high = mid - 1`.
5. **Otherwise** → the target must be in the **right half**, so narrow the range:
   `low = mid + 1`.
6. If the loop ends without finding a match (`low > high`), return `-1`.

**Time complexity:** O(log n) — each step eliminates half of the remaining search space,
which is dramatically faster than linear search for large sorted arrays.

**Key requirement:** binary search **only works correctly on a sorted array** — if the
array isn't sorted, the "go left or right" logic breaks down completely.

---

## 3. Threading Recap — `start()` vs `join()`

```python
from threading import *
from time import sleep

def show():
    for i in range(5):
        print("Hi")
        sleep(1)

t = Thread(target=show)
t.start()
t.join()
print("Bye")
```

**Output WITHOUT `join()`:**
```
Hi
Bye
Hi
Hi
Hi
Hi
```
(interleaved — main thread continues immediately, printing "Bye" before the spawned
thread finishes all 5 iterations)

**Output WITH `join()`:**
```
Hi
Hi
Hi
Hi
Hi
Bye
```
(guaranteed order — main thread waits for the spawned thread to completely finish before
printing "Bye")

---

## 4. File Handling

**Used to create files, and read, write, and append data to files.**

### Opening a File
```python
f = open("sample.txt", "r")
```
- `"sample.txt"` → file name
- `"r"` → read mode
- `f` → file object

### File Modes

| Mode | Meaning |
|---|---|
| `"r"` | Read |
| `"w"` | Write |
| `"a"` | Append |
| `"r+"` | Read & Write |

---

## 5. Reading a File

Suppose the file contains: `Hello`

```python
f = open("sample.txt", "r")
print(f.read())
f.close()
```
**Output:**
```
Hello
```

---

## 6. Writing to a File

```python
f = open("sample.txt", "w")
f.write("Hello Python")
f.close()
```
**File content after this:** `Hello Python`

**Important:** **write mode (`"w"`) erases the old content** and writes the new content
in its place — it does **not** append to what was already there.

---

## 7. Appending Data

Suppose the file currently contains: `Hi`

```python
f = open("sample.txt", "a")
f.write("\nWelcome")
f.close()
```
**File content after this:**
```
Hi
Welcome
```
**Key idea:** unlike write mode, **append mode (`"a"`) preserves the existing content**
and adds the new content after it.

---

## 8. Reading Line by Line

**File contains:**
```
Apple
Banana
```

```python
f = open("sample.txt", "r")
for line in f:
    print(line)
f.close()
```
**Output:**
```
Apple
Banana
```
**Key idea:** iterating directly over the file object (`for line in f:`) reads it one
line at a time, without needing to load the entire file into memory at once.

---

## 9. Using `with` Statements — The Better Way to Handle Files

```python
with open("sample.txt", "r") as f:
    print(f.read())
```

**Why use `with`?** It **automatically closes the file** once the block finishes — even
if an error occurs inside the block. This removes the risk of forgetting to call
`f.close()` manually, which is a common bug source when handling files the old way.

### File Reading Methods

| Method | Behavior |
|---|---|
| `read()` | Reads the **entire file** as one string |
| `readline()` | Reads **one line** at a time |
| `readlines()` | Reads **all lines as a list** |

```python
with open("sample.txt", "r") as f:
    print(f.readlines())
```
**Output:**
```
['Hello\n', 'Python\n', 'Java']
```
**Key idea:** `readlines()` gives you each line as a **separate list element**, keeping
the newline character (`\n`) at the end of each line except possibly the last — useful
when you need to process the file line by line as discrete items, rather than one giant
string.

---

## 10. Is Python Compiled or Interpreted?

- Python is often called an **interpreted language** — its bytecode is executed by the
  **Python Virtual Machine**.
- **However**, Python **first compiles the source code into bytecode** before execution.
  So technically, the **compiled bytecode is used by the interpreter**.
- **Python uses both a compiler AND an interpreter.**

**Key idea:** this two-step process (compile to bytecode, then interpret the bytecode)
is why Python is sometimes described as neither purely compiled nor purely interpreted —
it's a hybrid approach, which is common among modern high-level languages.

