# Day 11 — Multithreading, Generators, Abstract Classes, Iterators, Exception Handling

**Topics:** Multithreading (`threading`, `start()`, `join()`), Generators (`yield`),
Abstract Classes & Methods (`ABC`), Iterators vs Iterables (`iter()`, `next()`),
Exception Handling (`try/except/else/finally`)

---

## 1. Multithreading

**Multithreading is the process in which a program can perform multiple tasks at the
same time.**

**Real-world example:** Chrome running Tab 1, Tab 2, Tab 3 simultaneously.

### Why Use Multithreading?
- Improves performance
- Executes multiple tasks immediately (concurrently)
- Better CPU utilization
- Reduces waiting time

### How to Create a Thread

```python
from threading import *

def display():
    for i in range(5):
        print("Hi")

t = Thread(target=display)
t.start()
```

**Output:**
```
Hi
Hi
Hi
Hi
Hi
```

**Step-by-step breakdown:**
1. `from threading import *` — import all the modules from `threading`
2. Define a normal function (`def display():`)
3. `for i in range(5):`
4. `print("Hi")`
5. `t = Thread(target=display)` — this creates a **Thread object**, wrapping the
   function to be run
6. `t.start()` — this **starts the thread**

---

## 2. `start()` and `join()`

```python
t.start()   # starts the thread
t.join()    # makes the main thread wait until this thread finishes
```

### Example — Without `join()`
```python
from threading import *
from time import sleep

def show():
    for i in range(5):
        print("Hi")

t = Thread(target=show)
t.start()
print("Bye")
```
**Output (order may vary without `join()`):**
```
Hi
Hi
Hi
Hi
Hi
Bye
```

### Example — With `join()`
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
**Output (guaranteed order, "Bye" always LAST):**
```
Hi
Hi
Hi
Hi
Hi
Bye
```

**Key idea:** `join()` forces the main program to **wait** for the thread to completely
finish before continuing — without `join()`, the main thread might print `"Bye"` before
the spawned thread finishes its work, since they run independently.

---

## 3. Two Threads Example

```python
from threading import *
from time import sleep

def show():
    for i in range(5):
        print("Hi")
        sleep(1)

def display():
    for i in range(5):
        print("Hello")
        sleep(1)   # pause the thread for 1 second

t1 = Thread(target=show)
t2 = Thread(target=display)
t1.start()
t2.start()
```

**Output 1 (if the thread is not using `sleep(1)`):**
```
Hi
Hi
Hi
Hi
Hi
```
The first one will execute fastest, before the second one starts — since without a delay,
one thread might race ahead and finish before the other even gets scheduled.

**Possible Output 2 (with `sleep(1)` in both):**
```
Hi
Hello
Hi
Hello
...
```
**Why?** Since both threads run continuously **at the same time**, and both wait for
1 second at each step, the output is **interleaved/mixed** — the exact order can vary
each time the program runs, since the two threads are running concurrently and neither
is guaranteed to "go first" at every step.

---

## 4. Generators

**A generator is a special type of function that produces values one at a time, instead
of returning all values at once.**

**A generator is a function that uses the `yield` keyword to generate values one by one.**

### Normal Function vs Generator Function

```python
# Normal Function
def nums():
    return 1

print(nums())
# Output: 1
```

```python
# Generator Function
def nums():
    yield 1
    yield 2

g = nums()
print(next(g))
print(next(g))
```
**Output:**
```
1
2
```

### Difference Between `return` and `yield`
1. A **function** stops entirely after the first `return`.
2. A **generator** pauses, returns the value, and **continues from where it left off**
   the next time it's called.

```python
def nums():
    for i in range(1, 6):
        yield i

g = nums()
for x in g:
    print(x)
```
**Output:**
```
1
2
3
4
5
```

### Why Do We Use Generators?

```python
def nums():
    for i in range(1000000):
        yield i
```
- **Generates numbers only when needed** — one at a time, on demand.
- **But a list would first generate ALL the numbers** in memory upfront, and only after
  that move to the next step — which is far less memory-efficient for large sequences.

### Iterator vs Generator

| Iterator | Generator |
|---|---|
| Uses `iter()` and `next()` | Uses `yield` |
| More code | Less code |
| Manually coded | Automatically created |
| More complex | Easier |

---

## 5. Abstract Classes and Abstract Methods

### The Error Program — Why We Need Abstract Classes

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def sound(self):
        pass

A = Animal()   # ERROR
```
**Why the error?** Since `Animal` is declared as an **abstract class** (inheriting from
`ABC`) with an **abstract method** (`sound`), Python does **not allow creating an object
of it directly** — it exists purely to be inherited from.

### Why Do We Use Abstract Classes?

- We use them when we want **every child class to follow certain rules**.
- **An abstract class tells the child classes: "You must implement these methods."**

### Full Example

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):        # Abstract class
    @abstractmethod
    def start(self):       # Abstract method
        pass

class Car(Vehicle):
    def start(self):
        print("Car starts")

c = Car()
c.start()
```
**Output:**
```
Car starts
```

**Key rules:**
1. Abstract classes are coded using `ABC`.
2. Abstract methods are coded using the `@abstractmethod` decorator.

---

## 6. Abstract Class — What is it?

**An abstract class is a class that cannot create objects directly. It is used as a
blueprint for other classes.**

**Abstract method:** it is a method declared in the abstract class, but does **not**
have an implementation. **The child class must provide the implementation.**

---

## 7. Iterators

**An iterator is an object that allows us to traverse (access) elements one by one.**

### Example — Python Is Already Using an Iterator Internally

```python
m = [10, 20, 30]
for i in m:
    print(i)
```
**Output:**
```
10
20
30
```
Here, Python is internally using an iterator to move through the list, even though we
didn't write `iter()` or `next()` ourselves — a `for` loop does this automatically behind
the scenes.

### Python Provides Two Functions

**1. `iter()`** — converts an iterable into an iterator:
```python
m = [10, 20, 30]
it = iter(m)
print(it)
```
**Output:**
```
<list_iterator object>
```

**2. `next()`** — retrieves the next value from an iterator:
```python
nums = [10, 20, 30]
it = iter(nums)
print(next(it))
print(next(it))
print(next(it))
```
**Output:**
```
10
20
30
```
Calling `next()` one more time after all elements are exhausted raises a `StopIteration`
error.

---

## 8. Iterable vs Iterator

**1. Iterable:** objects that **can be iterated**. Example: tuple, list.

**2. Iterator:** created using `it = iter(m)`. **An iterator is an object that allows
sequential access to elements of a collection using the `next()` function.**

**Key distinction:** every iterator is built from an iterable, but not every iterable is
itself an iterator — a list is iterable, but you need `iter()` to actually turn it into
an iterator you can call `next()` on.

---

## 9. Exception Handling in Python

### What is an Exception?

**An exception is an error that occurs during program execution (a run-time error).**

```python
print(10 / 0)
```
**Output:**
```
ZeroDivisionError
```

### Why Do We Use Exception Handling?

**To prevent the program from crashing when an error occurs.**

### Without Exception Handling
```python
a = 10
b = 0
print(a / b)
print("Hi")
```
**Output:**
```
ZeroDivisionError
```
The program ends — `"Hi"` is **never executed**, because the error crashes the program
before reaching that line.

### With `try` and `except`
```python
a, b = 10, 0
try:
    print(a / b)
except:
    print("Hi")
print("Hello")
```
**Output:**
```
Hi
Hello
```
**How it works:**
```
try -> error? -> Yes -> except
                -> No  -> skip except
```
If an error occurs inside `try`, control jumps to `except`, and the program **continues
running afterward**, instead of crashing.

### Specific Exceptions
```python
try:
    print(10 / 0)
except ZeroDivisionError:
    print("Hi")
```
**Output:**
```
Hi
```
Catching a **specific** exception type (like `ZeroDivisionError`) is safer than a bare
`except:` — it only catches the error you actually expect, rather than silently
swallowing every possible error.

---

## 10. Multiple Exceptions

```python
try:
    a = int(input("Enter: "))
    print(10 / a)
except ZeroDivisionError:
    print("Hi")
except ValueError:
    print("Hello")
```

### Sample Runs
**1) Enter: 0**
```
Hi
```
(triggers `ZeroDivisionError`)

**2) Enter: 10**
```
1.0
```
(no error — runs normally)

**3) Enter: abc**
```
Hello
```
(triggers `ValueError`, since `"abc"` can't be converted to `int`)

**Key idea:** you can chain multiple `except` blocks, each catching a **different**
specific error type — Python checks them in order and runs whichever one matches the
error that actually occurred.

---

## 11. `finally` Block

```python
try:
    print(10 / 0)
except:
    print("Error Occurred")
finally:
    print("Finished")
```
**Output:**
```
Error Occurred
Finished
```
**`finally` always executes** — regardless of whether an error occurred or not. It's
typically used for cleanup code (like closing a file or a database connection) that must
run no matter what.

---

## 12. `else` Block

```python
try:
    print(10 / 2)
except:
    print("Hi")
else:
    print("Error")   # -> executes if try is TRUE (i.e., executes if NO error)
```
**Output:**
```
5.0
```
**Key idea:** the `else` block only runs if the `try` block completes **successfully**
(no exception was raised) — it's the opposite complement of `except`, which only runs
when there **was** an error.

**Important note:** the function name should not be changed once defined — renaming it
inconsistently across calls will break the program.

---

## Self-Check Questions
1. Why does `t.join()` guarantee "Bye" prints last, while omitting it does not?
2. Why is a generator more memory-efficient than a list for something like
   `range(1000000)`?
3. Why does `Animal()` throw an error when `Animal` is an abstract class with an
   abstract method, even though the class itself has no syntax errors?
4. What is the core difference between an **iterable** and an **iterator**?
5. Why is catching `except ZeroDivisionError:` specifically considered better practice
   than a bare `except:`?
6. In what order do `try`, `except`, `else`, and `finally` execute relative to each
   other, and under what conditions does each one run?
