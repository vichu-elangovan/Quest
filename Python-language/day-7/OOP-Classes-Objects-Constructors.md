# Day 7 — OOP Basics: Classes, Objects, Constructors, Class vs Instance Variables

**Topics:** OOP Introduction, Classes and Objects, Calling Methods via Objects,
`__init__` Constructor, `self`, Class Variables vs Instance Variables, Comparing
Objects with a Custom Method

---

## 1. What is OOP?

**OOP (Object-Oriented Programming)** is a programming approach where we organize code
using **classes** and **objects**.

- **Class** → a **blueprint** (a template describing what something should look like)
- **Object** → the **real thing made from that blueprint**

An object has two parts:
- **Attributes** — information (variables)
- **Behaviour** — actions (functions/methods)

---

## 2. Defining a Class and Creating an Object

### Syntax
```python
class ClassName:
    def method_name(self):
        statement
```

### Example
```python
class Vichu:
    def lachu(self):
        print('Abi Achu')

tata = Vichu()   # creating an object
tata.lachu()     # calling the method through the object
```
**Output:**
```
Abi Achu
```

**Key idea:** `tata = Vichu()` creates an **object** (an instance) of the class `Vichu`.
`tata.lachu()` then calls the `lachu` method **on that object**.

**Important note about `self`:** when calling a method **through an object**
(`tata.lachu()`), you don't need to manually pass the object — Python does this
automatically. `self` inside the method definition is just a placeholder that
automatically refers to whichever object called the method.

---

## 3. `__init__(self)` — The Constructor

**`__init__` is called a constructor in Python. It runs automatically whenever an object
is created.**

```python
class Vichu:
    def __init__(self):
        print("Hi")
        self.a = a
        self.b = b

    def lachu(self):
        print("We are", self.a, self.b)

c1 = Vichu.lachu('Abi', 'Achu')
c2 = Vichu.lachu('Naina', 'Amma')

c1.lachu()
c2.lachu()
```

**Output:**
```
Hi
Hi
We are Abi Achu
We are Naina Amma
```

**Key idea:** `self.a` and `self.b` store values **on the object itself**, so each object
(`c1`, `c2`) keeps its own independent copy of `a` and `b` — this is what allows
`c1.lachu()` and `c2.lachu()` to print different results even though they're calling the
exact same method.

---

## 4. Why `__init__` Matters — Memory Allocation

**Every time we create an object, it allocates a new memory. So the size of the object is
created by the constructor.**

`__init__` is where you typically set up all the initial data an object needs — since it
runs the moment the object is created, it's the natural place to initialize attributes.

---

## 5. Basic Constructor Example

```python
class Vichu:
    def __init__(self):
        self.name = 'Abi'
        self.age = 36

    def lachu(self):
        self.age = 30

    def compare(self, other):
        if self.age == other.age:
            return True
        else:
            return False

c1 = Vichu()
c2 = Vichu()

print(c2.age)      # 36
c1.name = 'abi'
c1.lachu()

if c1.compare(c2):
    print("They are equal")
else:
    print("Not equal")

print(c1.age)       # 30
```

**Output:**
```
36
Not equal
30
```

**Trace:**
- `c1` and `c2` both start with `age = 36` (set in `__init__`)
- `c1.lachu()` changes `c1.age` to `30` — but `c2.age` is untouched, since each object
  has its **own independent** copy of `age`
- `c1.compare(c2)` checks `self.age == other.age` → `30 == 36` → `False` → prints
  "Not equal"

**Key idea:** because each object stores its own `self.age`, modifying one object's
attribute never affects another object's attribute, even though they came from the same
class blueprint.

---

## 6. Types of Variables — Class Variables vs Instance Variables

### Class Variable
A variable defined **directly inside the class**, shared across **all objects** of that
class (unless individually overridden).

```python
class Car:
    wheels = 4   # class variable — shared by ALL Car objects

    def __init__(self):
        self.mil = 10          # instance variable
        self.com = "BMW"       # instance variable

c1 = Car()
c2 = Car()

print(c1.mil, c1.com, c1.wheels)
print(c2.mil, c2.com, c2.wheels)
```

### Instance Variable
A variable defined **inside `__init__`** using `self.`, unique to **each individual
object**.

```python
c2.mil = 8
print(c1.mil, c1.com, c1.wheels)   # 10 BMW 4
print(c2.mil, c2.com, c2.wheels)   # 8 BMW 4
```

**Key difference:** changing `c2.mil` only affects `c2` — it does **not** affect `c1.mil`,
because `mil` is an **instance variable** (each object gets its own copy). But `wheels`
stays the same for both, because it's a **class variable** shared across every object,
unless a specific object explicitly overrides it.

---

## 7. Another Full Example — `Student` Class

```python
class Student:
    def __init__(self):
        pass   # placeholder constructor

    def show(self):
        print("student")

s1 = Student()
s2 = Student()

s1.student
s2.student

s1.show()
s2.show()
```

**Key idea:** even with minimal logic inside `__init__`, every object created from
`Student` still goes through the constructor when it's instantiated — this is the pattern
that makes OOP predictable: every object of a class is guaranteed to be initialized the
same consistent way.

---
