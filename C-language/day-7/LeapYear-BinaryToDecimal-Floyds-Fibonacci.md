Topics: Leap Year Check, Binary-to-Decimal Conversion, Floyd's Triangle, Fibonacci Series

1. Leap Year Check
Rules
A number divisible by 4 is generally a leap year.
Exception: if a year is divisible by 100 but NOT by 400, it is not a leap year (e.g. 1900).
If a year is divisible by both 400 (or by 4 but not caught by the 100 exception), it is a leap year.
c
int year;
scanf("%d", &year);

if (year % 400 == 0)
    printf("Leap year");
else if (year % 100 == 0)
    printf("Not a leap year");
else if (year % 4 == 0)
    printf("Leap year");
else
    printf("Not a leap year");
Trace Examples
Year	%400	%100	%4	Result
2012	≠0	≠0	=0	Leap year
1900	≠0	=0	—	Not a leap year

Why check %400 first? Century years (like 1900, 2000) need the 400-rule to override the simpler 100-rule — checking in this order (400 → 100 → 4) correctly layers the exceptions without extra && conditions.

2. Binary to Decimal Conversion

Logic: each binary digit represents a power of 2, based on its position from the right (the "weight"). Extract each digit from the binary number (as if it were a normal number, using %10 and /10), multiply it by the current weight, and accumulate.

c
int bin, weight, decimal, rem;
scanf("%d", &bin);
decimal = 0;
weight = 1;

while (bin != 0)
{
    rem = bin % 10;
    decimal = decimal + rem * weight;
    bin = bin / 10;
    weight = weight * 2;   // weight doubles each position (2^0, 2^1, 2^2...)
}
printf("%d", decimal);
Trace for bin = 1001 (binary)
Iteration	bin before	rem	decimal after	bin after	weight after
1	1001	1	0+1×1 = 1	100	1×2 = 2
2	100	0	1+0×2 = 1	10	2×2 = 4
3	10	0	1+0×4 = 1	1	4×2 = 8
4	1	1	1+1×8 = 9	0	16

Output: decimal = 9 — correct, since binary 1001 = 2³ + 2⁰ = 8 + 1 = 9.

Key idea: weight tracks the place value (1, 2, 4, 8, ...) and doubles every iteration, matching the fact that each binary position to the left is worth twice as much.

3. Floyd's Triangle

Definition: prints consecutive natural numbers arranged in a right-angle triangle.

1
2  3
4  5  6
7  8  9  10
11 12 13 14 15
Logic

Use a single running counter m (starting at 1) that increments after every number printed — the nested loop structure just controls how many numbers go on each row (row i gets i numbers).

c
#include <stdio.h>
int main()
{
    int m, i, j, rows;
    printf("Enter no. of rows: ");
    scanf("%d", &rows);
    m = 1;

    for (i = 1; i <= rows; i++)
    {
        for (j = 1; j <= i; j++)
        {
            printf("%d ", m);
            m++;
        }
        printf("\n");
    }
    return 0;
}
Trace for rows = 5
Row 1 (i=1): 1 number printed → 1
Row 2 (i=2): 2 numbers → 2 3
Row 3 (i=3): 3 numbers → 4 5 6
Row 4 (i=4): 4 numbers → 7 8 9 10
Row 5 (i=5): 5 numbers → 11 12 13 14 15

Key idea: the outer loop controls the row number, and the inner loop's bound (j <= i) is what makes each row one element wider than the last. m is never reset — it just keeps counting up across the whole triangle.

4. Fibonacci Series

Definition: each term is the sum of the previous two terms. Starts with 0, 1.

Logic

Keep two running variables, a and b, representing the last two terms. Print a, compute the next term as a+b, then shift both variables forward.

c
#include <stdio.h>
int main()
{
    int a, b, result, m, i;
    printf("Enter the no: ");
    scanf("%d", &m);
    a = 0;
    b = 1;

    for (i = 1; i <= m; i++)
    {
        printf("%d ", a);
        result = b + a;
        a = b;
        b = result;
    }
    return 0;
}
Trace for m = 4
Iteration	a (printed)	result = a+b	a after	b after
1	0	0+1=1	1	1
2	1	1+1=2	1	2
3	1	1+2=3	2	3
4	2	2+3=5	3	5

Output: 0 1 1 2

Sample runs:

Enter the no: 5 → Output: 0 1 1 2 3
Enter the no: 6 → Output: 0 1 1 2 3 5

Key idea: a always holds the value about to be printed; b holds the "next" term. After printing a, both slide forward by one position — a becomes the old b, and b becomes the newly computed sum.
