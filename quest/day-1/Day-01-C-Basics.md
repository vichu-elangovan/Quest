Day 1 — Variables, Data Types & Scope

Topics: What is C, Compilers, Header Files, Data Types, Storage Classes, Scope, Macros

1. What is a C Program?

A C program is a sequence of instructions organized into functions, executed step by step. C is a procedural language — execution follows a defined order through function calls, unlike object-oriented languages that organize code around objects.

2. Compiler & Header Files

A compiler translates C source code into machine-executable code.

Source Code (.c) → Compiler → Object Code → Executable

Header files (.h) provide access to pre-built functions via the #include directive:

c
#include <stdio.h>   // enables printf(), scanf()
#include <stdlib.h>  // enables malloc(), free()
3. Data Types & Format Specifiers
Type	Format Specifier
signed integer	%d
unsigned integer	%u
long integer	%ld
unsigned long integer	%lu
long long integer	%lld
unsigned long long integer	%llu

Key rule: sizeof(short) ≤ sizeof(int) ≤ sizeof(long)

signed int x; is equivalent to int x; — int is signed by default.

4. Storage Class Specifiers

Four storage classes control a variable's scope, lifetime, and default value:

Specifier	Behavior
auto	Default for local variables; garbage value if uninitialized
register	Suggests storing the variable in CPU register memory (fast access, low capacity) — compiler decides whether to honor this
extern	Declares a variable defined elsewhere (another file); does NOT allocate memory
static	Retains value between function calls

Two types of memory:

Register memory — fast access, limited capacity
Secondary memory — slower access, large capacity
5. The extern Keyword — Cross-File Variables
c
// File 1
int a = 5;

// File 2
#include <stdio.h>
extern int a;

int main() {
    printf("%d", a);   // accesses 'a' from File 1 via extern
    return 0;
}

Note: extern only declares a variable — it does not define or allocate memory for it. This is useful for sharing variables across multiple files in a project without duplicating memory.

6. Scope of a Variable

Scope = the region of code where a variable is accessible and "alive."

c
int var = 3;   // global variable

int main() {
    int var = 4;        // local variable shadows global
    printf("%d", var);  // prints 4 (local variable wins)
    func();
}

void func() {
    printf("%d", var);  // prints 3 (accesses global, since no local 'var' here)
}

Nested block shadowing:

c
int main() {
    int var = 3;
    {
        int var = 4;
        printf("%d", var);  // prints 4 (innermost scope)
    }
    printf("%d", var);  // prints 3 (back to outer scope)
}

Key fact: An uninitialized global variable automatically defaults to 0. An uninitialized local variable holds a garbage/indeterminate value.

7. Macros (#define)
c
#define PI 3.14159
#define add(x, y) ((x) + (y))

int main() {
    printf("%f", PI);        // Output: 3.14159
    printf("%d", add(3, 4)); // Output: 7
    return 0;
}

Macros can also implement simple conditional logic:

c
#define greater(x, y) if (x > y) printf("%d is greater than %d", x, y); \
                      else printf("%d is lesser than %d", x, y);

int main() {
    greater(5, 6);   // Output: 5 is lesser than 6
    return 0;
}
8. Predefined Compiler Macros
c
printf("Date: %s, Time: %s", __DATE__, __TIME__);

__DATE__ and __TIME__ are predefined by the compiler and expand to the current compilation date/time.

9. Number Base Quirks

Octal trap: A leading 0 before a number makes it octal, not decimal.

c
int var = 052;
printf("%d", var);   // Output: 42 (NOT 52)

Conversion: 052 in octal = 5×8¹ + 2×8⁰ = 40 + 2 = 42

Hexadecimal: Prefixed with 0x.

c
int var = 0x43FF;   // interpreted as a hexadecimal value
