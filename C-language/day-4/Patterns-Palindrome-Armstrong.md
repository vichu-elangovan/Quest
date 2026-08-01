Commit message: Add C Day 4: Patterns, Palindrome, Armstrong

markdown
# Day 4 — Pattern Printing, Palindrome & Armstrong Numbers

**Topics:** Nested Loops for Patterns, Palindrome Check, Digit Count, Armstrong Numbers

---

## 1. Pyramid Pattern (Star/X Triangle)

**Goal:** print a widening triangle of `x`s, e.g. for 5 rows:
x

xxx
xxxxx
xxxxxxx
xxxxxxxxx


### Logic
For row `i` (1-indexed) out of `m` total rows, the printed characters should span from
column `m - (i-1)` to `m + (i-1)` — everything else in that row is blank. This window
is centered on column `m` and expands by 1 on each side as the row number increases.

- Row 1: print from `m` to `m` → 1 char
- Row 2: print from `m-1` to `m+1` → 3 chars
- Row 3: print from `m-2` to `m+2` → 5 chars
- ...and so on, up to `2m - 1` chars on the last row.

### Code
```c
#include <stdio.h>
int main()
{
    int m, i, j;
    printf("No. of rows: ");
    scanf("%d", &m);

    for (i = 1; i <= m; i++)
    {
        for (j = 1; j <= m; j++)
        {
            if (j >= m - (i - 1) && j <= m + (i - 1))
                printf("*");
            else
                printf(" ");
        }
        printf("\n");
    }
    return 0;
}
```

**Key idea:** the outer loop controls the row, the inner loop scans every possible column position and decides whether to print `*` or a blank, based on the row-dependent window `[m-(i-1), m+(i-1)]`.

---

## 2. Palindrome Check

**Definition:** A number is a palindrome if reversing its digits gives back the same number (e.g. `121`, `12321`, `1001`).

### Logic
Reverse the number digit-by-digit using `%` and `/`, then compare the reversed value to the original.

```c
#include <stdio.h>
int main()
{
    int m, q, res = 0, rem;
    scanf("%d", &m);
    q = m;

    while (q != 0)
    {
        rem = q % 10;           // extract last digit
        res = res * 10 + rem;   // build reversed number
        q = q / 10;             // strip last digit
    }

    if (m == res)
        printf("Palindrome");
    else
        printf("Not a Palindrome");

    return 0;
}
```

### Trace for `m = 121`
| Step | `q` | `rem` | `res` |
|---|---|---|---|
| start | 121 | — | 0 |
| 1 | 12 | 1 | 0×10+1 = 1 |
| 2 | 1 | 2 | 1×10+2 = 12 |
| 3 | 0 | 1 | 12×10+1 = 121 |

`res == m` → **121 is a Palindrome.**

### Trace for `m = 120` (trailing-zero case)
| Step | `q` | `rem` | `res` |
|---|---|---|---|
| start | 120 | — | 0 |
| 1 | 12 | 0 | 0×10+0 = 0 |
| 2 | 1 | 2 | 0×10+2 = 2 |
| 3 | 0 | 1 | 2×10+1 = 21 |

`res = 21 ≠ 120` → **Not a Palindrome.** Trailing zeros in the original number silently "lose" a digit's worth of place value once reversed, which is exactly why they break palindrome-ness.

---

## 3. Counting the Number of Digits

Needed as a building block for the Armstrong check below.

```c
int m = 191, q = m, count = 0;
while (q != 0)
{
    q = q / 10;
    count++;
}
// count = 3
```

Trace: `191 → 19 (count=1) → 1 (count=2) → 0 (count=3)`.

---

## 4. Armstrong Number

**Definition:** A number equals the sum of each of its digits raised to the power = total digit count (e.g. `153 = 1³ + 5³ + 3³`).

### Logic
1. Count the digits (`count`, from section 3) — this tells us the exponent to use.
2. For each digit, raise it to the power `count` (done manually via a multiplication loop, since there's no `pow()` used here).
3. Sum all the powered digits into `result`.
4. Compare `result` to the original number.
5. **Reset `mul` to `1` after each digit** — otherwise the leftover value from the previous digit's power calculation carries into the next digit's multiplication, corrupting the result (since `mul = mul * rem` accumulates across iterations by design).

```c
int cnt = count, result = 0, mul = 1, rem;
q = m;

while (q != 0)
{
    rem = q % 10;
    cnt = count;
    while (cnt != 0)
    {
        mul = mul * rem;
        cnt--;
    }
    result = result + mul;
    q = q / 10;
    mul = 1;   // reset for next digit
}

if (result == m)
    printf("Armstrong number");
else
    printf("Not an Armstrong number");
```

### Trace for `m = 153` (count = 3)
| Digit | Power calc | Contribution | Running `result` |
|---|---|---|---|
| 3 | 3×3×3 | 27 | 27 |
| 5 | 5×5×5 | 125 | 152 |
| 1 | 1×1×1 | 1 | 153 |

`result == m` → **153 is an Armstrong number.**
