# Day 8 — Class vs Instance Variables, Instance/Class/Static Methods

**Topics:** Class Variables vs Instance Variables, Instance Methods, Class Methods
(`@classmethod`, `cls`), Static Methods (`@staticmethod`)

---

## 1. Types of Variables

```python
class Car:
    wheels = 4   # class variable

    def __init__(self):
        self.mil = 10          # instance variable
        self.com = "BMW"       # instance variables

c1 = Car()
c2 = Car()

print(c1.mil, c1.com, c1.wheels)
c2.mil = 8
print(c2.mil, c2.com, c2.wheels)
```

**Output:**
```
10 BMW 4
8 BMW 4
```

### Class Variable
Defined **directly inside the class body**, shared by **all objects** unless a specific
object overrides it.
```python
wheels = 4   # -> class variable
```

### Instance Variable
Defined **inside `__init__`** using `self.`, unique to each individual object.
```python
self.mil = 10
self.com = "BMW"   # -> instance variables
```

**Key idea:** changing `c2.mil = 8` only affects `c2` — `c1.mil` stays `10`, because `mil`
is an instance variable and each object gets its own separate copy. But `wheels` stays
`4` for both, since it's a shared class variable.

---

## 2. Types of Methods

There are **three types of methods** in Python classes: instance methods, class methods,
and static methods.

### i. Instance Method — Uses `self`

**Use case:** when each object needs its own unique data.

> Let's say there are two variables — `name` and `age`. But each and every student will
> have a different name and age. So in this case, we use an **instance method** with
> `self` as a common object reference.

```python
class Student:
    def __init__(self, name):
        self.name = name

    def show(self):
        print("Name:", self.name)

s1 = Student('Achu')
s2 = Student('Vichu')

s1.show()
# Name: Achu
s2.show()
# Name: Vichu
```
**Key idea:** `self` refers to whichever specific object called the method, so `s1.show()`
and `s2.show()` correctly print each object's own individual `name`.

---

### ii. Class Method — Uses `cls` and `@classmethod`

**Use case:** when every object shares the **same common data** — no need to repeat it
per-object.

> But every student will have the common school. In that case we are using a **class
> method**.

```python
class Student:
    college = 'SJCE'   # shared by every student

    @classmethod
    def show(cls):
        print(cls.college)   # 'cls' is the keyword for class

student.show()
```
**What is `cls`?** `cls` refers to the **class itself**, not to any individual object.
`cls.college` accesses the class-level `college` value directly.

### What Does `@classmethod` Actually Change?

**In a normal (instance) method**, when you call:
```python
c1.vichu()
```
it calls the method through the **corresponding object** (`c1`) — `self` inside refers to
that specific object.

**But when you use `@classmethod`:**
```python
c1.vichu()
```
this instead calls the method through the **class itself** — regardless of which object
you technically called it through, `cls` always refers to the class, not the individual
object.

---

### iii. Static Method — Uses `@staticmethod`

**Use case:** when you want to write a simple message or perform a task that **doesn't
need to reference `name`, `age`, `college`**, or any object/class-specific data at all.

> Here, when we want to simply print a message that doesn't include name, age, college —
> that static is used. It just does a task.

```python
class Student:
    @staticmethod
    def greet():
        print("Hi")

Student.greet()
# Output: Hi
```

**Why use a static method?** Because it **doesn't need `self` or `cls`** at all — it's
essentially just a regular function that happens to live inside a class for
organizational purposes, with no dependency on object or class-level data.

---

## 3. Full Combined Example — Instance, Class, and Static Methods Together

```python
class Student:
    college = "ARLM"   # class variable

    def __init__(self, name):
        self.name = name   # instance variable

    def show_name(self):   # instance method
        print("Name:", self.name)

    @classmethod
    def show_college(cls):   # class method
        print(cls.college)

    @staticmethod
    def message():   # static method
        print("Hello")


c1 = Student('Alex')
c2 = Student('Bob')
c3 = Student('Jones')

c1.show_name()
c2.show_name()
c3.show_name()

Student.show_college()
Student.message()
```

**Output:**
```
Alex
Bob
Jones
ARLM
Hello
```

### Summary Table

| Method Type | Decorator | First Parameter | Accesses | Use Case |
|---|---|---|---|---|
| Instance method | (none) | `self` | Object's own data | Data unique to each object (e.g. `name`) |
| Class method | `@classmethod` | `cls` | Class-level data | Data shared by all objects (e.g. `college`) |
| Static method | `@staticmethod` | (none) | Neither | A standalone task with no object/class dependency (e.g. a generic message) |

---

