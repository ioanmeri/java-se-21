# Java SE 21 Developer Professional (1Z0-830)

# Primitive Types & Numeric Conversions – Complete Exam Guide

This topic is one of the **most heavily tested areas** of the Oracle Java certification exam. Oracle frequently combines primitive types, literals, promotions, casting, overflow, wrappers, and `char` behavior into tricky questions.

---

# 1. All Primitive Types

Java has **8 primitive types**:

| Type      | Size          | Range             |
| --------- | ------------- | ----------------- |
| `byte`    | 8 bits        | -128 to 127       |
| `short`   | 16 bits       | -32,768 to 32,767 |
| `int`     | 32 bits       | -2³¹ to 2³¹-1     |
| `long`    | 64 bits       | -2⁶³ to 2⁶³-1     |
| `float`   | 32 bits       | IEEE 754          |
| `double`  | 64 bits       | IEEE 754          |
| `char`    | 16 bits       | 0 to 65,535       |
| `boolean` | JVM dependent | `true` / `false`  |

---

## Primitive Widening Hierarchy

Memorize this:

```java
byte -> short -> int -> long -> float -> double
               ^
               |
              char
```

Important:

```java
char -> int
```

But:

```java
char != short
```

There is **no automatic conversion between `char` and `short`**.

---

# 2. Default Types of Literals

Oracle frequently asks about the type of a literal.

---

## Integer Literals

```java
10
100
999
```

Default type:

```java
int
```

Example:

```java
long x = 10;
```

Valid because the `int` literal is widened to `long`.

---

## Long Literals

Require suffix:

```java
L
```

or

```java
l
```

Use uppercase `L`.

```java
long x = 10L;
```

---

### Exam Trap

```java
long x = 9999999999;
```

Compile error.

Reason:

The compiler first treats the literal as an `int`.

Correct:

```java
long x = 9999999999L;
```

---

## Floating-Point Literals

Default type:

```java
double
```

Example:

```java
double d = 3.14;
```

---

## Float Literals

Require:

```java
F
```

or

```java
f
```

```java
float f = 3.14F;
```

---

### Exam Trap

```java
float f = 3.14;
```

Compile-time error.

Reason:

```java
3.14
```

is a `double`.

---

## Double Suffix

Optional:

```java
double d1 = 3.14;
double d2 = 3.14D;
double d3 = 3.14d;
```

---

# 3. Numeric Bases

---

## Decimal

```java
int x = 10;
```

---

## Binary

Prefix:

```java
0b
```

or

```java
0B
```

Example:

```java
int x = 0b1010;
```

Value:

```text
10
```

---

## Hexadecimal

Prefix:

```java
0x
```

or

```java
0X
```

Example:

```java
int x = 0xA;
```

Value:

```text
10
```

---

## Octal

Prefix:

```java
0
```

Example:

```java
int x = 010;
```

Value:

```text
8
```

---

### Oracle Favorite

```java
System.out.println(010);
```

Output:

```text
8
```

NOT 10.

---

# 4. Underscores in Literals

Allowed:

```java
int x = 1_000_000;
```

```java
long y = 10_000L;
```

```java
double d = 1_2.3_4;
```

---

## Invalid Uses

### Beginning

```java
int x = _100;
```

Invalid.

---

### End

```java
int x = 100_;
```

Invalid.

---

### Adjacent to Decimal Point

```java
double d = 10_.5;
```

Invalid.

```java
double d = 10._5;
```

Invalid.

---

### Adjacent to Suffix

```java
long x = 100_L;
```

Invalid.

---

### Adjacent to Base Prefix

```java
int x = 0_x10;
```

Invalid.

```java
int x = 0b_1010;
```

Invalid.

---

# 5. Widening Primitive Conversions

A widening conversion is automatic because no information is lost.

```java
byte b = 10;

short s = b;

int i = s;

long l = i;

float f = l;

double d = f;
```

All valid.

---

Example:

```java
int x = 10;
long y = x;
```

No cast required.

---

# 6. Narrowing Primitive Conversions

A narrowing conversion may lose data.

Requires an explicit cast.

```java
long l = 100;

int x = (int) l;
```

Valid.

---

Compile error:

```java
long l = 100;

int x = l;
```

---

# 7. Casting Rules

---

## Double to Integer

```java
double d = 10.9;

int x = (int) d;
```

Result:

```java
10
```

Fractional part is discarded.

There is **no rounding**.

---

Example:

```java
System.out.println((int) 9.99);
```

Output:

```text
9
```

---

# 8. Overflow and Underflow

Oracle loves overflow questions.

---

## Integer Overflow

```java
int x = Integer.MAX_VALUE;

x++;
```

Value becomes:

```text
-2147483648
```

No exception occurs.

---

Example:

```java
System.out.println(Integer.MAX_VALUE + 1);
```

Output:

```text
-2147483648
```

---

## Integer Underflow

```java
int x = Integer.MIN_VALUE;

x--;
```

Output:

```text
2147483647
```

---

## Floating-Point Overflow

```java
double d = Double.MAX_VALUE;

d *= 2;
```

Result:

```text
Infinity
```

---

# 9. Numeric Promotion Rules

This is one of the most tested topics.

---

## Rule 1

If either operand is `double`

Result is `double`.

```java
5 + 5.0
```

Type:

```java
double
```

---

## Rule 2

Otherwise if either operand is `float`

Result is `float`.

```java
5 + 5f
```

Type:

```java
float
```

---

## Rule 3

Otherwise if either operand is `long`

Result is `long`.

```java
5 + 5L
```

Type:

```java
long
```

---

## Rule 4

Everything else becomes `int`.

This includes:

```java
byte
short
char
```

---

Example:

```java
byte a = 1;
byte b = 2;

var result = a + b;
```

Type:

```java
int
```

Not `byte`.

---

# 10. Why Does byte + byte Become int?

Java automatically promotes:

```java
byte
short
char
```

to

```java
int
```

before arithmetic.

---

Example:

```java
byte a = 10;
byte b = 20;

byte c = a + b;
```

Compile error.

Because:

```java
a + b
```

evaluates to an `int`.

---

Correct:

```java
byte c = (byte) (a + b);
```

---

# 11. Compound Assignment Operators

Very common exam trap.

---

This fails:

```java
byte b = 1;

b = b + 1;
```

Compile error.

---

This works:

```java
byte b = 1;

b += 1;
```

Compiles successfully.

The compiler effectively does:

```java
b = (byte) (b + 1);
```

behind the scenes.

---

# 12. Compile-Time Constant Narrowing

Special exception.

This compiles:

```java
byte b = 100;
```

because 100 fits into the `byte` range.

---

This also compiles:

```java
final int x = 100;

byte b = x;
```

because `x` is a constant.

---

This does NOT compile:

```java
int x = 100;

byte b = x;
```

because `x` is not a compile-time constant.

---

# 13. char and Unicode

`char` is a:

```text
16-bit unsigned integer
```

Range:

```text
0 to 65535
```

---

## Character Literal

```java
char c = 'A';
```

---

## Unicode Literal

```java
char c = '\u0041';
```

Output:

```text
A
```

---

### Oracle Favorite

```java
char c = 65;

System.out.println(c);
```

Output:

```text
A
```

---

## char Arithmetic

```java
char c = 'A';

System.out.println(c + 1);
```

Output:

```text
66
```

because `c` is promoted to `int`.

---

To print `B`:

```java
System.out.println((char) (c + 1));
```

Output:

```text
B
```

---

# 14. char vs String

Character:

```java
char c = 'A';
```

Single quotes.

---

String:

```java
String s = "A";
```

Double quotes.

---

Compile error:

```java
char c = "A";
```

---

# 15. Wrapper Classes

| Primitive | Wrapper     |
| --------- | ----------- |
| `byte`    | `Byte`      |
| `short`   | `Short`     |
| `int`     | `Integer`   |
| `long`    | `Long`      |
| `float`   | `Float`     |
| `double`  | `Double`    |
| `char`    | `Character` |
| `boolean` | `Boolean`   |

---

# 16. Autoboxing

Primitive → Wrapper

```java
Integer x = 10;
```

Compiler converts it to something equivalent to:

```java
Integer x = Integer.valueOf(10);
```

---

# 17. Unboxing

Wrapper → Primitive

```java
Integer x = 10;

int y = x;
```

Compiler converts it to something equivalent to:

```java
int y = x.intValue();
```

---

# 18. Wrapper Null Trap

Oracle asks this constantly.

```java
Integer x = null;

int y = x;
```

Compiles successfully.

Runtime result:

```text
NullPointerException
```

because unboxing is attempted.

---

# 19. Wrapper Caching

Frequently tested.

---

Example:

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

Output:

```text
true
```

---

But:

```java
Integer a = 200;
Integer b = 200;

System.out.println(a == b);
```

Output:

```text
false
```

typically because values outside the cache range are not reused.

---

Cache range:

```text
-128 to 127
```

---

Always prefer:

```java
a.equals(b)
```

for value comparison.

---

# Top Oracle Certification Traps

## Trap 1

```java
byte b = 1;

b = b + 1;
```

❌ Compile error

---

## Trap 2

```java
byte b = 1;

b += 1;
```

✅ Compiles

---

## Trap 3

```java
char c = 65;
```

✅ Compiles

Prints:

```text
A
```

---

## Trap 4

```java
char c = 'A';

c++;
```

✅ Compiles

`c` becomes `'B'`.

---

## Trap 5

```java
System.out.println(010);
```

Output:

```text
8
```

---

## Trap 6

```java
long x = 10000000000;
```

❌ Compile error

Needs:

```java
long x = 10000000000L;
```

---

## Trap 7

```java
float f = 3.14;
```

❌ Compile error

Needs:

```java
float f = 3.14F;
```

---

## Trap 8

```java
Integer x = null;

int y = x;
```

✅ Compiles

💥 Throws `NullPointerException`

---

# Oracle-Style Practice Questions

## Question 1

```java
byte a = 40;
byte b = 50;

byte c = a + b;

System.out.println(c);
```

### Answer

Compile error.

Reason:

```java
a + b
```

becomes `int`.

---

## Question 2

```java
byte b = 10;

b += 20;

System.out.println(b);
```

### Answer

```text
30
```

---

## Question 3

```java
char c = 'A';

System.out.println(c + 1);
```

### Answer

```text
66
```

---

## Question 4

```java
char c = 'A';

System.out.println((char)(c + 1));
```

### Answer

```text
B
```

---

## Question 5

```java
int x = Integer.MAX_VALUE;

System.out.println(x + 1);
```

### Answer

```text
-2147483648
```

---

## Question 6

```java
Integer a = 127;
Integer b = 127;

System.out.println(a == b);
```

### Answer

```text
true
```

---

## Question 7

```java
Integer a = 128;
Integer b = 128;

System.out.println(a == b);
```

### Answer

```text
false
```

---

# Must-Memorize Exam Sheet

```text
byte/short/char arithmetic => int

double > float > long > int

Widening = automatic

Narrowing = explicit cast

Overflow wraps around

Integer cache = -128..127

float literals require F

long literals require L

char is unsigned 16-bit

char + anything => int

+= performs implicit cast

byte b = 100;   // valid
byte b = 128;   // compile error

Integer x = null;
int y = x;      // NullPointerException

010 = octal 8
0x10 = hexadecimal 16
0b10 = binary 2
```

# Extra "Oracle Loves These" Questions

### What is the output?

```java
char c = 'A';
short s = 1;

System.out.println(c + s);
```

Answer:

```text
66
```

Type is `int`.

---

### What is the output?

```java
long x = 10;
float y = 20;

System.out.println(x + y);
```

Answer:

```text
30.0
```

Type is `float`.

---

### Will this compile?

```java
byte b = 127;
b++;
```

Answer:

✅ Yes

Result:

```text
-128
```

Overflow occurs.

---

### Will this compile?

```java
byte b = 128;
```

Answer:

❌ No

128 is outside the byte range.

---

If you can answer every question in this guide without hesitation, you'll be very well prepared for nearly all primitive-type and numeric-conversion questions that typically appear on the 1Z0-830 exam.
