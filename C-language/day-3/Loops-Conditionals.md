# Day 3 — Loops & Conditional Statements

**Topics:** Loops (while, for, do-while), Loop Control Statements, Conditional Statements, Switch Statement, Short-Circuit Evaluation

---

## 1. Why Loops?
Loops are used to **reduce the complexity and number of lines** in a program by repeating a block of code without rewriting it manually.

---

## 2. Types of Loops

### `while` loop
```c
while (expression) {
    statement;
    update;
}
```

### `for` loop
```c
for (initialization; condition; update) {
    statement;
}
```

### `do-while` loop
```c
int i;
do {
    printf("%d", i);
    i--;
} while (i > 0);
```

### `while` vs `do-while` — Key Difference

| Feature | `while` | `do-while` |
|---|---|---|
| Runs the loop body | Only if condition is true first | At least **once**, even if condition is false |
| Semicolon after condition | Not required | **Required** after `while(condition);` |

---

## 3. Loop Control Statements

| Statement | Purpose |
|---|---|
| `break` | Terminates the loop immediately, exits out of it entirely |
| `continue` | Skips the current iteration and moves on to the next one |

**Example — Program to print odd numbers (1 to 20) using `continue`:**
```c
int main() {
    int i, m = 2;
    for (i = 1; i <= 20; i++) {
        if (i == m) {
            m = m + 2;
            continue;   // skip printing even numbers
        }
        printf("%d ", i);
    }
    return 0;
}
```
**Output:** `1 3 5 7 9 11 13 15 17 19`

---

## 4. Conditional Statements

### if-else
```c
if (condition) {
    statement_1;
} else {
    statement_2;
}
```

### else-if ladder
```c
if (condition_1) {
    // ...
} else if (condition_2) {
    // ...
} else {
    // ...
}
```

### Nested if
```c
if (condition_1) {
    if (condition_2) {
        // ...
    }
} else {
    // ...
}
```

---

## 5. Switch Statement

```c
switch (condition) {
    case 1:
        printf("...");
        break;
    default:
        printf("...");
        break;
}
```

### Switch Statement Rules
- ❌ Duplicate case labels are not allowed.
- ✅ Only **integer or character constants** are allowed as case labels.
- ❌ Floating-point values are not allowed as case labels.
- ❌ Variables cannot be used as case labels — must be compile-time constants.
- ✅ Macros ARE allowed, since they expand to constant values before compilation.

---

## 6. Short-Circuit Evaluation — Worked Example

```c
int main() {
    int a = 1, b = 1;
    int c = ++a || b++;
    int d = b-- && --a;
    printf("%d %d %d %d", d, c, b, a);
    return 0;
}
```

### Step-by-step trace:

**Line 1: `c = ++a || b++`**
- `++a` executes first → `a` becomes `2`
- Since `||` and first operand is true, `b++` is **never evaluated**
- `b` remains `1`, `c = 1`

**Line 2: `d = b-- && --a`**
- `b--` uses `b=1` (true), then decrements → `b` becomes `0`
- Since `&&` and first operand is true, `--a` **is** evaluated → `a` becomes `1`
- `d = 1`

**Final values:** a=1, b=0, c=1, d=1
**Output (`d c b a` order):** `1 1 0 1`

### Short-Circuit Rule Summary
| Operator | Short-circuits when... | Second operand evaluated? |
|---|---|---|
| `\|\|` (OR) | First operand is **true** | ❌ Skipped |
| `&&` (AND) | First operand is **false** | ❌ Skipped |

---

## Self-Check Questions
1. What's the key difference in execution guarantee between `while` and `do-while`?
2. Why is `continue` more efficient than an if-else check for filtering the odd-numbers program above?
3. Why can't a float value be used as a switch case label?
4. Trace through: if `a=1, b=1` and the code were `c = ++a && b++` (AND instead of OR), what would `c` and `b` equal?
