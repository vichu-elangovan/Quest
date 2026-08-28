# Day 10 — Polymorphism: Duck Typing, Overloading, Overriding

**Topics:** Polymorphism Overview, Duck Typing, Method Overloading (via default
arguments), Method Overriding, Operator Overloading (dunder methods)

---

## 1. What is Polymorphism?

**Polymorphism → "one thing in many forms."**

Four main flavors covered here:
- Duck Typing
- Method Overloading
- Operator Overloading
- Method Overriding

---

## 2. Duck Typing

**Duck Typing is a concept where Python doesn't care about the object's class. It only
cares whether the object has the required method or behavior.**

**Simple definition:**
> If it walks like a duck and quacks like a duck, then treat it as a duck.

**If an object has the required methods, Python will use it — regardless of the class.**

```python
class Duck:
    def sound(self):
        print("Quack Quack")

class Dog:
    def sound(self):
        print("Bow Bow")

def make_sound(animal):
    animal.sound()

d1 = Duck()
d2 = Dog()
make_sound(d1)
make_sound(d2)
```

**What happened?** When the function `make_sound()` is called, it doesn't care whether
the `animal` is a `Duck` or a `Dog`. It only checks: does `animal.sound()` exist? If the
method exists, it just executes it — regardless of the object's actual class.

### Example 2 — Duck Typing with Unrelated Classes

```python
class Pycharm:
    def execute(self):
        print("Compiling")
        print("Running")

class MyEditor:
    def execute(self):
        print("Spell check")
        print("Compiling")

class Laptop:
    def code(self, ide):
        ide.execute()

lap = Laptop()
ide = Pycharm()
lap.code(ide)
ide = MyEditor()
lap.code(ide)
```

**Output:**
```
Compiling
Running
Spell check
Compiling
```

**Key idea:** `Pycharm` and `MyEditor` are **completely unrelated classes** — no shared
parent, no inheritance. But `lap.code(ide)` works with either one, purely because both
classes happen to define an `execute()` method. This is the essence of duck typing:
Python trusts behavior over class identity.

---

## 3. Method Overriding

**When a child class has the same method name as the parent class, it is known as method
overriding.**

**When the method is called using a child class object, the child class method is
executed instead of the parent class method.**

```python
class A:
    def show(self):
        print("Hi")

class B(A):
    def show(self):
        print("Hello")

b = B()
b.show()
```
**Output:**
```
Hello
```
**Why?** When the child creates its own version, it overwrites the parent's version. So
`b.show()` uses `B`'s `show()`, not `A`'s.

**Why is method overriding used?** It's used when a child class wants to **change or
customize the behavior of a method inherited from the parent class**. Python fully
supports method overriding.

---

## 4. Method Overloading

**Method overloading (in most languages) means: same method name, different number of
arguments.**

```python
def add(a, b):
    ...

def add(a, b, c):
    ...
```

**But Python does NOT support method overloading directly.** In this case, the **first
method is overwritten** by the second one — so when you call `add(10, 20)`, it will cause
an error, because only the *last* defined version of `add` actually exists.

### How Python Achieves Overloading — Default Arguments

Since Python doesn't support true overloading, it can be **simulated** using default
argument values:

```python
class Student:
    def add(self, a=None, b=None, c=None):
        s = 0
        if a is not None:
            s += a
        if b is not None:
            s += b
        if c is not None:
            s += c
        return s

s = Student()
print(s.add(10, 20))        # -> "Overloading"
print(s.add(10, 20, 30))    # -> "Overloading"
```

**Output:**
```
30
60
```

**Key idea:** by giving every parameter a default value of `None` and checking
`is not None` before using it, the **same single method** can gracefully handle being
called with 2 arguments or 3 arguments — mimicking what true overloading would look like
in other languages.

---

## 5. Operator Overloading

**Operator overloading means giving a new meaning to an operator (`+`, `>`, `<`, etc.)
when it is used on objects.**

### Why Objects Can't Be Added by Default

```python
a = 5
b = 6
print(a + b)   # Here, + knows how to add two numbers
```

**But what if we create our own class?**
```python
class Student:
    pass

s1 = Student()
s2 = Student()
print(s1 + s2)
# ERROR — because Python doesn't know how to add two Student objects
```

### The Secret Behind Operators — Dunder Methods

**When we write `a + b`, Python internally calls `a.__add__(b)`.**

```python
a + b   ->   a.__add__(b)
a - b   ->   a.__sub__(b)
```

### Operator-to-Method Mapping

| Operator | Dunder Method |
|---|---|
| `+` | `__add__()` |
| `-` | `__sub__()` |
| `*` | `__mul__()` |
| `/` | `__truediv__()` |
| `>` | `__gt__()` |
| `<` | `__lt__()` |
| `==` | `__eq__()` |
| (string repr) | `__str__()` |

```python
print(5 + 6)
print(int.__add__(5, 6))   # both are the same
```

---

## 6. Operator Overloading — Program 1: Addition

**Here we define our own `__add__()` method:**

```python
class Student:
    def __init__(self, marks):
        self.marks = marks

    def __add__(self, other):
        return self.marks + other.marks

s1 = Student(50)
s2 = Student(70)
print(s1 + s2)
```

**Output:**
```
120
```

**Key idea:** `s1 + s2` internally becomes `s1.__add__(s2)`. Instead of writing
`s1.marks + s2.marks` manually, operator overloading lets us just write `s1 + s2`.

---

## 7. Operator Overloading — Program 2: Comparison

```python
class Student:
    def __init__(self, marks):
        self.marks = marks

    def __gt__(self, other):
        return self.marks > other.marks

s1 = Student(50)
s2 = Student(70)
print(s1 > s2)
```

**Output:**
```
False
```
**Key idea:** overriding `__gt__()` lets the `>` operator directly compare two custom
`Student` objects based on their `marks`, instead of comparing the objects themselves
(which wouldn't make sense by default).

---

## 8. Operator Overloading — Program 3: String Representation with `__str__`

### Without Operator Overloading
```python
class Student:
    def __init__(self, name):
        self.name = name

s1 = Student('Vichu')
print(s1)
```
**Output:**
```
<__main__.Student object at 0x...>
```
**Why?** By default, `print()` on an object just shows its class and memory address,
because Python doesn't know how you'd like the object to be displayed as text.

### Using `__str__` to Customize the Output
```python
class Student:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return self.name

s1 = Student('Vichu')
print(s1)
```
**Output:**
```
Vichu
```
**Key idea:** `__str__()` defines what should be returned when the object is converted to
a string (e.g., via `print()`). Without it, Python falls back to its default
representation, showing the object's memory address instead of anything meaningful.

---
