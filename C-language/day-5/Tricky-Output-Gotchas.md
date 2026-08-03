# Day 5 — Tricky C Output Gotchas

**Topics:** XOR Swap, Comma Operator, Switch Fallthrough, Break/Continue, Short-Circuit Side Effects, Unsigned Wraparound, Dangling Semicolons

---

## 1. XOR Swap (Swap Without a Temp Variable)

```c
int a = 4, b = 3;
a = a ^ b;
b = a ^ b;
a = a ^ b;
printf("a = %d and b = %d", a, b);
// Output: a = 3 and b = 4
```
Works because XOR is its own inverse: `(a^b)^b = a` and `(a^b)^a = b`.

---

## 2. Comma Operator — Value is Always the Rightmost Expression

```c
int var, num;
num = (var = 15, var + 35);
printf("%d", num);
// Output: 50 (NOT 15)
```
The comma operator evaluates left to right, but the **whole expression's value** is always the **last (rightmost)** sub-expression — `var = 15` runs first, then `var + 35 = 50` becomes the value assigned to `num`.

---

## 3. Switch Fallthrough — Missing `break` Runs Every Case Below

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
// Output: 16 21
```

| Iteration | Path taken | Result |
|---|---|---|
| `i=0` | case 0→1→5→default: `0+5+2+5+4` | prints `16`, then `i++`→17 |
| `i=17` | matches nothing → default: `17+4` | prints `21`, then `i++`→22, loop ends |

**Key rule:** without `break`, a `switch` executes every case *below* the match, including `default`.

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
// "Neso" is printed 0 times
```
While `i < 0`, `continue` sends control back to the loop condition, skipping `printf` every time. Once `i` reaches `0`, `break` fires **before** `printf` is ever reached — it's unreachable code in this logic.

---

## 5. Short-Circuit with a Side-Effecting Function in a `for` Header

```c
int i = 0;
for (printf("one\n"); i < 3 && printf(""); i++)
{
    printf("Hi!\n");
}
// Output: one
```
`printf("one\n")` runs once as the init step. Then the condition `i < 3 && printf("")` is checked — `printf("")` **returns 0** (zero characters printed), which is falsy, so `&&` short-circuits to false immediately. The loop body never runs.

---

## 6. Unsigned Integer Wraparound with Post-Increment

```c
unsigned int i = 500;
while (i++ != 0);
printf("%d", i);
// Range of unsigned int (4 bytes): 0 to 4294967295
// Output: 1
```
`i++` compares the **pre-increment** value but always increments. The loop runs until `i` wraps from `4294967295` back to `0` — at that check the comparison is false and the loop stops, but the increment already fired, leaving `i = 1`. This lands on `1` regardless of the starting value (below the max).

---

## 7. The Dangling Semicolon Bug

```c
int x = 3;
if (x == 2); x = 0;
if (x == 3) x++;
else x += 2;
printf("x = %d", x);
// Output: x = 2
```
The stray `;` after `if(x==2)` creates an **empty statement** as the if-body. So `x = 0;` runs unconditionally, every time — not just when `x==2`. `x` becomes `0`, then `if(x==3)` is false, so `else` fires: `x += 2 → x = 2`.

**Key lesson:** a semicolon directly after an `if(...)` condition silently creates an empty if-body, and the next line runs regardless of the condition.
