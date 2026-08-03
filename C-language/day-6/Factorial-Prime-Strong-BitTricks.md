# Day 6 — Factorial, Prime Numbers, Strong Numbers & Bit Tricks

**Topics:** Factorial, Optimized Prime Check (via √n), Strong Numbers, Addition without `+`, Half Adder using XOR/AND

---

## 1. Factorial of a Number

```c
int m, fact = 1, i;
scanf("%d", &m);
for (i = 1; i <= m; i++)
{
    fact = fact * i;
}
printf("%d", fact);
```
Accumulator pattern: multiply `fact` by every integer from `1` to `m`. This becomes the building block for the Strong Number check below.

---

## 2. Prime Number — Optimized Check Using Square Root

**Definition:** A number divisible only by itself and `1`.

**The trick:** only check divisibility up to **√n**, not all the way to `n`. Any factor larger than √n would pair with a factor smaller than √n, which would already have been caught.

```c
#include <math.h>
int x, vd1, vd2, count = 0, i;
scanf("%d", &x);

vd1 = ceil(sqrt(x));   // sqrt = square root, ceil = round UP
vd2 = x;

for (i = 2; i <= vd1; i++)
{
    if (vd2 % i == 0)
        count = 1;
}

if ((count == 0 && vd2 != 1) || vd2 == 2 || vd2 == 3)
    printf("%d is Prime no", vd2);
else
    printf("not a prime number");
```

**Trace for `x = 23`:** `√23 ≈ 4.79 → ceil → 5`, so check `i = 2` to `5`:

| i | 23 % i |
|---|---|
| 2 | 1 |
| 3 | 2 |
| 4 | 3 |
| 5 | 3 |

None are `0` → `count` stays `0` → **23 is Prime**.

**Why the special cases?** `vd2 != 1` excludes 1 (not prime by definition). `vd2 == 2 \|\| vd2 == 3` explicitly handles the two smallest primes, since the general loop bounds can behave oddly for very small numbers.

---

## 3. Strong Number

**Definition:** A number where the sum of the factorials of its digits equals the number itself.

```c
int m, q, rem, res = 0, fact = 1, i;
scanf("%d", &m);
q = m;

while (q != 0)
{
    rem = q % 10;
    for (i = 1; i <= rem; i++)
    {
        fact = fact * i;
    }
    res = res + fact;
    q = q / 10;
    fact = 1;   // reset for next digit
}

if (res == m)
    printf("Strong");
else
    printf("Not strong");
```

**Trace for `m = 145`** (`145 = 1!+4!+5! = 1+24+120`):

| Step | `rem` | Factorial | `fact` | `res` after | `q` after |
|---|---|---|---|---|---|
| 1 | 5 | 1×2×3×4×5 | 120 | 120 | 14 |
| 2 | 4 | 1×2×3×4 | 24 | 144 | 1 |
| 3 | 1 | 1 | 1 | **145** | 0 |

`res == m` → **145 is a Strong number.**

**Critical detail:** `fact` must reset to `1` after each digit, or the factorial carries over incorrectly into the next digit — same principle as the `mul` reset in the Day 4 Armstrong program.

---

## 4. Addition Without Using the `+` Operator

```c
int x, y;
scanf("%d %d", &x, &y);

while (y != 0)
{
    x++;
    y--;
}
printf("%d", x);
```
Repeatedly moves one unit from `y` to `x` until `y` runs out. Example: `x=5, y=7` → output `12`.

---

## 5. Half Adder Using Bitwise Operators (No `+`)

- **Sum bit** = `A XOR B`
- **Carry bit** = `(A AND B) << 1`

**Why the left shift?** A carry out of one bit position must be added into the *next* bit position — one place to the left.

```c
int sum, carry, a, b;
scanf("%d %d", &a, &b);

while (b != 0)
{
    sum = a ^ b;
    carry = (a & b) << 1;
    a = sum;
    b = carry;
}
printf("%d", sum);
```
This repeats until there's no carry left (`b == 0`), at which point `sum` holds the final result — a ripple-carry adder, simulated with bitwise operators.

---

## Self-Check Questions
1. Why is checking divisibility only up to `√n` sufficient to determine primality?
2. In the Strong Number program, why doesn't digit-processing order affect the final result?
3. Trace the half-adder for `a = 5, b = 3` step by step until `b` becomes `0`. What's the final sum?
4. Why does the carry need to be shifted left by 1 before being reused?
5. What happens in the increment/decrement addition program if `y` is negative? Would the loop terminate correctly?
