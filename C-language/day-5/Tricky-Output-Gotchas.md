# Day 5 — Tricky C Output Gotchas

**Topics:** XOR Swap, Comma Operator, Switch Fallthrough, Break/Continue Interaction,
Short-Circuit with Side Effects, Unsigned Integer Wraparound, Dangling Semicolons

---

## 1. XOR Swap (Swap without a temp variable)

```c
int a = 4, b = 3;
a = a ^ b;
b = a ^ b;
a = a ^ b;
printf("After XOR, a = %d and b = %d", a, b);
```

**Output:** `a = 3 and b = 4`

**Why it works:** XOR-ing a value with itself cancels out (`x ^ x = 0`), and XOR-ing with `0`
returns the original value (`x ^ 0 = x`). This lets you swap two variables without needing a
third temporary variable.

---

## 2. Comma Operator — Value is Always the Rightmost Expression

```c
int var, num;
num = (var = 15, var + 35);
printf("%d", num);
```

**Output:** `50` (not `15`)

**Why:** The comma operator evaluates each expression **left to right**, but the value of the
*entire comma expression* is always the value of the **last (rightmost)** sub-expression.
- `var = 15` runs first → `var` becomes `15`
- `var + 35` evaluates to `50` → this is what gets assigned to `num`

**Common mistake:** assuming the comma expression takes the value of the first assignment.

---

## 3. Switch Fallthrough — Missing `break` Runs Every Case Below the Match

```c
int i;
for (i = 0; i < 20; i++)
{
    switch (i)
    {
        case 0: i += 5;
        case 1: i += 2;
        case 5: i += 5;
        default: i += 4;
    }
    printf("%d ", i);
}
```

**Output:** `16 21`

**Trace:**
- `i=0` matches `case 0` → no `break`, so it **falls through** every case below:
  `i=0+5=5` → `case 1: i=5+2=7` → `case 5: i=7+5=12` → `default: i=12+4=16` → prints `16`
- Loop's `i++` → `i=17`
- `i=17` matches no case → goes to `default: i=17+4=21` → prints `21`
- Loop's `i++` → `i=22`, loop condition `22<20` fails → loop ends

**Key idea:** without `break`, a `switch` doesn't stop at the matching case — it keeps
executing every subsequent case (including `default`) until a `break` or the end of the block.

---

## 4. `break` vs `continue` Inside a Loop

```c
int i = -5;
while (i <= 5)
{
    if (i >= 0)
        break;
    else
    {
        i++;
        continue;
    }
    printf("Neso");
}
```

**Output:** `Neso` is printed **0 times**

**Why:** While `i < 0`, the `else` branch runs (`i++; continue;`) — `continue` sends control
back to the `while` condition, **skipping `printf` entirely** every single time. By the time
`i` finally reaches `0`, the `if (i >= 0)` condition becomes true and `break` exits the loop
**before** `printf("Neso")` is ever reached. The `printf` line is unreachable in this logic.

---

## 5. Short-Circuit Evaluation with Side-Effecting Functions in a `for` Header

```c
int i = 0;
for (printf("one\n"); i < 3 && printf(""); i++)
{
    printf("Hi!\n");
}
```

**Output:**

one


**Why:** `printf("one\n")` runs once as the `for` loop's **initialization** step, printing
`one`. Then the condition is checked: `i < 3 && printf("")`.
- `i < 3` is true
- But `printf("")` **returns the number of characters printed**, which is `0` — and `0` is
  falsy in C
- Since `&&` short-circuits on a false operand, the overall condition is **false**

The loop body (`printf("Hi!\n")`) never executes, because the condition fails on the very
first check. `printf("")` looks harmless, but its return value silently kills the loop.

---

## 6. Unsigned Integer Wraparound with Post-Increment

```c
unsigned int i = 500;
while (i++ != 0);
printf("%d", i);
```

**Range of unsigned int (4 bytes):** `0` to `4294967295`

**Output:** `1`

**Why:** `i++` is **post-increment** — the comparison uses the value *before* incrementing,
but `i` is incremented regardless of the comparison result. The loop keeps incrementing `i`
(silently, with no body) until `i` wraps all the way around from `4294967295` back to `0`.
At the exact moment the *compared* value is `0`, the loop condition (`i++ != 0`) becomes
false and the loop stops — but the increment for that final check has already happened,
leaving `i = 1`.

**Key insight:** this lands on `1` regardless of the starting value (as long as it's a
positive value less than the max), because post-increment always fires one more time than
the comparisons that read the "pre-increment" value.

---

## 7. The Dangling Semicolon Bug

```c
int x = 3;
if (x == 2); x = 0;
if (x == 3) x++;
else x += 2;
printf("x = %d", x);
```

**Output:** `x = 2`

**Why:** The stray semicolon right after `if (x == 2)` terminates the `if` statement with an
**empty statement** as its body. This means `x = 0;` is **not** inside the `if` — it is a
separate statement that runs **unconditionally**, every time, regardless of `x`'s value.

- `x` becomes `0` (unconditionally)
- `if (x == 3)` is now false (since `x` is `0`)
- `else` branch fires: `x += 2` → `x = 2`

**Key lesson:** a semicolon directly after an `if(...)` condition (before any real statement)
silently creates an empty if-body, and the next line runs regardless of the condition. This
is one of the most common silent bugs in C.
