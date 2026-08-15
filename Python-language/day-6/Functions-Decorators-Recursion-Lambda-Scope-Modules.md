# Day 6 — Decorators, Recursion, Lambda/Filter, Variable Scope, Modules, *args/**kwargs

**Topics:** Functions as Objects, Decorators, Passing a List to a Function, Fibonacci
(Iterative), Anonymous/Lambda Functions, `filter()`, Recursion, Factorial (Recursive),
Variable Scope (local/global), Modules, `__name__` Special Variable, `*args`, `**kwargs`

---

## 1. Functions Are Objects in Python

A function can be assigned to a variable and called through that variable — because in
Python, functions themselves are objects that can be referenced.

```python
def greet():
    print("Hi")

x = greet     # NOTE: no parentheses — this assigns the function itself, not its result
x()
# Output: Hi
```
**Key idea:** `greet` (without `()`) refers to the function object itself; `greet()` calls
it. Since functions are objects, they can be assigned to variables, just like an int or a
string can be.

### Passing a Function as an Argument
```python
def greet():
    print("Hi")

def display(func):
    func()

display(greet)
# Output: Hi
```
Here, `greet` serves as an **argument** to `display()` — this only works because
functions are treated as first-class objects in Python.

---

## 2. Decorators

**A decorator is a function that adds extra functionality to another function, without
changing its original code.**

**Analogy:** Ice cream is the original function; chocolate topping is the decorator — it
adds something extra on top, without changing what the ice cream itself is.

### Basic Syntax
```python
def decorator(func):
    def wrapper():
        # before
        func()
        # after
        return
    return wrapper
```
A decorator is a function that:
1. Takes another function (`func`) as its argument
2. Defines an inner `wrapper()` function that does something **before** and/or **after**
   calling `func()`
3. Returns that `wrapper` function

### Example
```python
def decorator(func):
    def wrapper():
        print("Hi")
        func()
        print("Hello")
    return wrapper

@decorator
def view():
    print("How are you?")

view()
```
**Output:**
```
Hi
How are you?
Hello
```
**How it works:** the `@decorator` syntax above `def view():` is shorthand for
`view = decorator(view)`. So calling `view()` now actually calls the `wrapper()` function
created by `decorator`, which prints `"Hi"`, then runs the original `view()` logic
(`"How are you?"`), then prints `"Hello"`.

---

## 3. Passing a List to a Function

```python
def count(list):
    even = 0
    odd = 0
    for i in list:
        if i % 2 == 0:
            even += 1
        else:
            odd += 1
    return even, odd

list = [10, 20, 13, 17, 18, 19, 18, 27]
even, odd = count(list)
print(even)   # 4
print(odd)    # 4
```
**Key idea:** a list can be passed into a function just like any other object, and the
function can freely iterate through it. Since a list is mutable, passing it also means the
function is working with a reference to the original list (see Day 5's call-by-reference
discussion).

---

## 4. Fibonacci Series (Iterative, Inside a Function)

```python
def fib(m):
    a = 0
    b = 1
    if m == 1:
        print(a)
    else:
        print(a)
        print(b)
        for i in range(2, m):
            c = a + b
            a = b
            b = c
            print(c)

fib(8)
# Output: 0 1 1 2 3 5 8 13
```
**Logic:** the first two Fibonacci numbers (`0` and `1`) are printed directly, then each
subsequent term is generated as the sum of the previous two, following the same
slide-forward pattern from Day 3's Fibonacci program — just wrapped inside a reusable
function now.

---

## 5. Anonymous / Lambda Functions

**Normal functions require multiple lines to declare.** A **lambda function** is a
function **without a name**, written in a single line.

### Syntax
```python
variable = lambda arguments: expression
```

### Example — Add Two Numbers
```python
f = lambda a, b: a + b
res = f(5, 6)
print(res)
# Output: 11
```

### Example — Square a Number
```python
f = lambda a: a * a
res = f(5)
print(res)
# Output: 25
```

---

## 6. `filter()` — Using a Normal Function vs a Lambda

`filter()` picks out only the elements of an iterable for which a given function returns
`True`.

### Syntax
```python
filter(function_name, iterable)
```

### Example — Using a Normal Function
```python
def is_num(m):
    if m % 2 == 0:
        return True

nums = [3, 2, 6, 9, 4, 8, 6, 2]
evens = list(filter(is_num, nums))
print(evens)
# Output: [2, 6, 4, 8, 6, 2]
```

### Example — Using a Lambda Instead
```python
nums = [3, 2, 6, 9, 4, 8, 6, 2]
evens = list(filter(lambda n: n % 2 == 0, nums))
print(evens)
# Output: [2, 6, 4, 8, 6, 2]
```
**Key idea:** both versions produce the exact same result — the lambda version just
avoids having to separately define and name a small one-off function (`is_num`) purely to
use it once inside `filter()`.

---

## 7. Recursion

**A function which repeats/calls itself is called recursion.**

### Example 1 — A Function That Doesn't Call Itself (Not Recursion)
```python
def greet():
    print("Hi")

greet()
# Output: Hi
```

### Example 2 — A Function That Calls Itself (Infinite Recursion — No Base Case)
```python
def greet():
    print("Hi")
    greet()
    greet()

greet()
# Output: infinite times (this will crash / hit a recursion limit)
```
**Important:** without a stopping condition (a "base case"), a recursive function calls
itself forever. **There is a limit to how deep recursion can go** in Python (a maximum
recursion depth), after which the program errors out.

---

## 8. Factorial Using Recursion

### Iterative Version (for comparison)
```python
def fact(m):
    fac = 1
    for i in range(1, m + 1):
        fac = fac * i
        i += 1
    print(fac)

fact(5)
# Output: 120
```

### Recursive Version
```python
def fact(m):
    if m == 0:
        return 1
    return m * fact(m - 1)

result = fact(5)
print(result)
# Output: 120
```
**Logic:** the **base case** is `m == 0`, which returns `1` (since `0! = 1`) and stops
the recursion. For any other `m`, the function returns `m * fact(m-1)` — calling itself
with a smaller value each time, until it eventually hits the base case.

**Trace for `fact(5)`:**
```
fact(5) = 5 * fact(4)
        = 5 * (4 * fact(3))
        = 5 * (4 * (3 * fact(2)))
        = 5 * (4 * (3 * (2 * fact(1))))
        = 5 * (4 * (3 * (2 * (1 * fact(0)))))
        = 5 * 4 * 3 * 2 * 1 * 1
        = 120
```

---

## 9. Scope of a Variable

### Global vs Local Variables
```python
a = 10   # global variable

def something():
    a = 15   # this creates a NEW local variable, scoped only to this function
    print("inside", a)

something()
print("outside", a)
```
**Output:**
```
inside 15
outside 10
```
**Why?** Assigning `a = 15` **inside** the function creates a separate **local variable**
that only exists within `something()`. It does not affect the `a` defined outside — the
global `a` remains `10`.

### Modifying the Global Variable — the `global` Keyword

To actually change a global variable from inside a function, you must explicitly declare
it with `global`:

```python
a = 10

def some():
    global a
    a = 15
    print("inside", a)

some()
print("outside", a)
```
**Output:**
```
inside 15
outside 15
```
**Key idea:** without the `global` keyword, `a = 15` inside a function creates a *new*
local variable that shadows the outer one. With `global a` declared first, Python instead
modifies the actual global variable — so the change persists outside the function too.

### Accessing (Not Modifying) a Global Variable — `globals()`
```python
a = 10

def some():
    x = globals()['a']   # 'a' is mentioned; there can be many local variables,
                          # and we are only accessing the global one specifically
    print("inside", x)

some()
print("outside", a)
```
**Output:**
```
inside 10
outside 10
```
`globals()` returns a dictionary of all global variables, letting you explicitly look up
a specific one by name — useful when you want to **read** a global value without risking
accidentally modifying it.

---

## 10. Modules

**A module is simply a Python file that contains functions, variables, or classes that
can be reused in other programs.**

### `main.py`
```python
from vichu import *

a = 9
b = 7
c = add(a, b)
print(c)
```

### `vichu.py`
```python
def add(a, b):
    return a + b

def sub(a, b):
    return a - b
```
By importing everything from `vichu.py` into `main.py`, the `add()` function defined in
one file becomes directly usable in the other.

---

## 11. The Special Variable `__name__`

### First (Main) Module
```python
print(__name__)
```
**Output:**
```
__main__
```
**Why?** The name of the file that is run **directly** (as the entry point) is always
assigned the special value `__main__` — regardless of the file's actual filename.

### Second Module (Imported by Another File)
```python
# vichu.py
print("Hello")
print(__name__)
```
When `vichu.py` is **imported** by another file (rather than run directly), `__name__`
inside `vichu.py` is set to `"vichu"` (the actual module name) — **not** `"__main__"`.

### Practical Use — Guarding Code That Should Only Run Directly

**`vichu.py`:**
```python
import main   # if vichu.py imports main.py

print("Hello", __name__)
```

**`main.py`:**
```python
def main():
    print("Hello")
    print("Hi")

if __name__ == "__main__":
    main()
```
**Output when running `main.py` directly:**
```
Hello
Hi
```

**Why this pattern matters:** wrapping code inside `if __name__ == "__main__":` ensures
that block **only runs when the file is executed directly** — not when it's imported by
another file. This is why the `main()` function call works in `main.py` (since its
`__name__` really is `"__main__"` when run directly), but the same guard would **not**
trigger if `vichu.py` tried to use it, since `vichu.py`'s `__name__` would be `"vichu"`,
not `"__main__"`, whenever it's imported.

---

## 12. `*args` — Variable-Length Positional Arguments

```python
def add(*nums):
    s = 0
    for i in nums:
        s += i
    print(s)

add(10, 20)
# Output: 30

add(10, 20, 30)
# Output: 60
```
`*nums` captures **any number** of positional arguments into a tuple, letting the same
function handle calls with different argument counts.

---

## 13. `**kwargs` — Keyword Variable-Length Arguments

**Used when you don't know how many keyword arguments will be passed to a function.**

### Syntax
```python
def function_name(**kwargs):
    pass
```

### Example
```python
def person(name, **data):
    print(name)
    print(data)

person('navin', age=20, city='Mumbai', mob=9865580342)
```
**Output:**
```
navin
{'age': 20, 'city': 'Mumbai', 'mob': 9865580342}
```

### Another Example — Just `**data`, No Named Parameter
```python
def person(**data):
    print(data)

person(name="Vichu", age=21)
```
**Output:**
```
{'name': 'Vichu', 'age': 21}
```

### `*args` vs `**kwargs`
| | `*args` | `**kwargs` |
|---|---|---|
| Stores values as | a **tuple** | a **dictionary** |

### Combined Example
```python
def students(name, **data):
    print(name)
    for i, j in data.items():
        print(i, j)

students("Vichu", age=21, dept="ECE", gpa=9.13)
```
**Output:**
```
Vichu
age 21
dept ECE
gpa 9.13
```
`data.items()` returns each key-value pair from the `**kwargs` dictionary, which can then
be unpacked into `i, j` and printed individually.

---

