# Day 1 — Python Basics: Strings, Lists, Tuples, Sets, Dictionaries

**Topics:** Python Overview, Interpreter vs Compiler, Strings, Indexing/Slicing, Lists,
Tuples, Sets, Dictionaries

---

## 1. About Python

- Invented by **Guido van Rossum**
- Python is:
  - **Procedural**
  - **Object Oriented**
  - **High level language**
  - **Interpreted**

**IDE** — Integrated Development Environment

**Interpreter:** a program that reads your Python code **line by line** and executes it
immediately.

**Compiler:** the whole code is executed at once (translated fully before running), unlike
an interpreter which goes line by line.

---

## 2. Trying Things in the Interpreter

```python
>>> 5 + 7
12

>>> 8 - 9
-1

>>> 12 * 3
36

>>> 4 / 2
2.0
```

**Why does `4 / 2` return `2.0` (a float) instead of `2`?**
Because `/` is **true division** in Python — it always returns a float, since dividing two
numbers doesn't always produce a whole number (e.g. `5 / 2 = 2.5`). Python keeps the return
type consistent regardless of the specific numbers involved.

**If you don't want the decimal (float) result**, use **floor division** with `//`:
```python
>>> 5 // 2
2   # Integer division / Floor division — drops the decimal part
```

---

## 3. Strings

### Single vs Double Quotes
```python
>>> 'navin'
'navin'
>>> print('navin')
navin
```
Both single (`'...'`) and double (`"..."`) quotes work the same way for plain strings —
there's no functional difference for simple text.

### The Apostrophe Problem
```python
print('navin's laptop')     # ERROR — the apostrophe closes the string early
print('navin's "laptop"')   # ERROR — same issue
```

**Fix 1 — switch outer quotes:**
```python
print("navin's laptop")
# Output: navin's laptop
```

**Fix 2 — use a backslash to escape the apostrophe:**
```python
print('navin\'s "laptop"')
# Output: navin's "laptop"
```
**Key idea:** when your string itself contains a quote character, either use the *other*
type of quote as the outer delimiter, or escape the inner quote with a backslash (`\`).

### String Concatenation and Repetition
```python
'navin' + 'navin'
# navinnavin

2 * 'navin'
# navinnavin
```
`+` joins strings together; `*` repeats a string that many times.

### Backslashes and Raw Strings
```python
print('C:\docs\navin')
# C:\docs
# avin
# due to \n
```
The backslash in `\n` is interpreted as a **newline escape character**, not a literal
backslash — this breaks file paths written normally.

**Fix — use a raw string** by prefixing with `r`:
```python
print(r'C:\docs\navin')
# C:\docs\navin
```
A raw string tells Python to treat every backslash literally, ignoring escape sequences.

### Indexing and Slicing
In Python, indexing starts from **0** (forward) and **-1** (reverse, from the end).

```python
name = 'youtube'

name[0]     # 'y'
name[-2]    # 'b'

name[0:2]   # 'yo'   (index 0 up to, not including, index 2)
name[0:4]   # 'yout'
name[1:]    # 'outube'  (from index 1 to the end)
name[:4]    # 'yout'    (from the start up to index 4)
```

**Strings are immutable** — once created, individual characters cannot be changed.
```python
len(name)   # 7 — length of the string
```

---

## 4. Lists — `[ ]`

Similar to arrays in C, but far more flexible.

```python
nums = [25, 12, 36, 95, 14]

nums[0]     # 25
nums[2:]    # [36, 95, 14]

names = ['achu', 'vichu', 'abi', 'lachu']
names       # ['achu', 'vichu', 'abi', 'lachu']

values = [9.5, 'volvo', 25]   # a list can hold DIFFERENT types at once
```

### Lists Within Lists
```python
av = [nums, names]
# [[25, 12, 36, 95, 14], ['achu', 'vichu', 'abi', 'lachu']]
```

**Lists are mutable** — unlike strings, their contents can be changed after creation.

### Adding Elements

**`append()`** — adds an element to the **end**:
```python
nums.append(45)
# [25, 12, 36, 95, 14, 45]
```

**`insert(position, value)`** — adds an element at a **specific position**:
```python
nums.insert(2, 77)
# [25, 12, 77, 36, 95, 14, 45]
```

### Removing Elements

**`remove(value)`** — removes by **value**, not position:
```python
nums.remove(14)
# 14 is no longer in the list
```
**Note:** `remove()` can't access by index position — it searches for the value itself.

**`pop(index)`** — removes and **returns** the element at a given index:
```python
nums.pop(1)     # returns 12 (removes the element at index 1)
nums.pop()      # returns 45 (with no argument, pops the LAST element)
```
`pop()` follows a **stack-like (LIFO)** behavior — if no index is given, the last element
is popped.

**`del`** — deletes a slice or index directly (no return value):
```python
del nums[2:]    # deletes everything from index 2 onward
nums            # [25, 77]
```

### Combining Lists

**`extend()`** — adds multiple values from another list:
```python
nums.extend([29, 12, 14, 36])
# [25, 77, 29, 12, 14, 36]
```

### Useful Built-in Functions
```python
min(nums)        # 12  — smallest value
max(nums)        # 77  — largest value
sum(nums)        # 193 — total of all values
nums.count(25)   # 1   — counts occurrences of a value in the list
```

### Sorting
```python
nums.sort()
# [12, 14, 25, 29, 36, 77]
```

---

## 5. Tuples — `( )`

**Tuples are immutable** — once created, their contents cannot be changed.

```python
tup = (21, 36, 14, 25)
tup[1]      # 36
```
Indexing works the same as lists, but you cannot append, insert, or remove elements.

---

## 6. Sets — `{ }`

```python
s = {22, 25, 14, 22, 5}
s
# {22, 25, 5, 14}   — duplicate 22 is automatically removed
```

**Key facts about sets:**
- **Sets don't maintain a fixed order/sequence**, so indexing is **not possible**:
  ```python
  s[1]   # ERROR
  ```
- **Sets are mutable**, but they **do not support duplicate values** — adding a duplicate
  is simply ignored.

---

## 7. Dictionaries — `{ key: value }`

A dictionary uses **keys** to fetch corresponding **values**.

```python
data = {1: 'Harsh', 2: 'Abi', 3: 'Achu'}

data[3]         # 'Achu'   — access by key
data.get(3)     # 'Achu'   — safer access method
```

### Handling Missing Keys
```python
data.get(4)              # None   (key 4 doesn't exist, no error)
print(data.get(4))       # None

data.get(4, 'Not Found') # 'Not Found'  — custom default value if key is missing
```
**Key idea:** `dict[key]` throws an error for a missing key, but `dict.get(key)` safely
returns `None` (or a custom default) instead — much safer for lookups where the key might
not exist.

### Combining Two Lists Into One Dictionary
```python
keys = [1, 2, 3, 4]
values = ['Hi', 'Hello', 'Bye', 'Zoo']

data = dict(zip(keys, values))
# zip() pairs up the two lists element-by-element
# dict() converts those pairs into a dictionary

data
# {1: 'Hi', 2: 'Hello', 3: 'Bye', 4: 'Zoo'}

data[1]      # 'Hi'
```

### Adding and Deleting Keys
```python
data[5] = 'Como'
# adds a new key-value pair
# {1: 'Hi', 2: 'Hello', 3: 'Bye', 4: 'Zoo', 5: 'Como'}

del data[2]
# removes the key-value pair for key 2
# {1: 'Hi', 3: 'Bye', 4: 'Zoo', 5: 'Como'}
```

### Nested Structures — Lists and Dictionaries Inside a Dictionary
```python
prog = {
    1: 'Hi',
    2: 'Bye',
    3: ['Vichu', 'Achu'],
    4: {5: 'Abi', 6: 'Lachu'}
}

prog[1]         # 'Hi'
prog[3]         # ['Vichu', 'Achu']
prog[3][1]      # 'Achu'      — index into the nested list
prog[4]         # {5: 'Abi', 6: 'Lachu'}
prog[4][5]      # 'Abi'       — access a key inside the nested dictionary
prog[4][6]      # 'Lachu'
```
**Key idea:** dictionary values can themselves be lists or other dictionaries. Chain the
access operators (`[]`) to drill down into nested structures, one level at a time.

---

