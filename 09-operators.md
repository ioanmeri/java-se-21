# Java SE 21 Developer Professional (1Z0-830) – Operators Exam Guide

## 🧮 9. Operators

- [ ] Arithmetic & precedence
- [ ] `==` vs `.equals()`
- [ ] Logical operators (`&&`, `||`, `!`)
- [ ] Short-circuit behavior
- [ ] Increment/decrement tricky cases

This is one of Oracle's favorite areas because operators combine **precedence, evaluation order, type promotion, object comparison, and side effects**. Many certification questions are deliberately designed to trick candidates.

---

# 1. Arithmetic Operators & Precedence

## Arithmetic Operators

```java
+   addition
-   subtraction
*   multiplication
/   division
%   remainder (modulus)
```

Example:

```java
int a = 10;
int b = 3;

System.out.println(a + b); // 13
System.out.println(a - b); // 7
System.out.println(a * b); // 30
System.out.println(a / b); // 3
System.out.println(a % b); // 1
```

---

## Integer Division

Oracle loves this topic.

```java
System.out.println(5 / 2);
```

Output:

```text
2
```

NOT:

```text
2.5
```

Why?

Both operands are `int`, so Java performs integer division.

To get decimal results:

```java
System.out.println(5.0 / 2);  // 2.5
System.out.println((double)5 / 2); // 2.5
```

---

## Division by Zero

### Integer

```java
int x = 5 / 0;
```

Result:

```text
ArithmeticException
```

---

### Floating Point

```java
System.out.println(5.0 / 0);
```

Output:

```text
Infinity
```

```java
System.out.println(-5.0 / 0);
```

Output:

```text
-Infinity
```

---

## Modulus (%) with Negatives

Oracle occasionally asks this.

```java
System.out.println(10 % 3);   // 1
System.out.println(-10 % 3);  // -1
System.out.println(10 % -3);  // 1
```

Rule:

**The sign follows the left operand.**

---

# Operator Precedence

Memorize the important order:

```text
1. postfix      x++, x--
2. unary        ++x, --x, !, +, -
3. *, /, %
4. +, -
5. relational   < > <= >=
6. equality     == !=
7. &&
8. ||
9. ?:
10. =
```

---

## Example

```java
int result = 2 + 3 * 4;
```

Equivalent:

```java
2 + (3 * 4)
```

Output:

```text
14
```

---

## Parentheses Win

```java
int result = (2 + 3) * 4;
```

Output:

```text
20
```

---

# Oracle Trap: Evaluation Order

Java evaluates operands **left to right**.

```java
int x = 10;

int y = x + ++x;

System.out.println(y);
```

Steps:

```java
x + ++x
10 + 11
```

Output:

```text
21
```

Final:

```java
x = 11
```

---

# 2. == vs .equals()

One of the most tested exam topics.

---

## Primitive Comparison

For primitives:

```java
int a = 5;
int b = 5;

System.out.println(a == b);
```

Output:

```text
true
```

`==` compares values.

---

## Reference Comparison

For objects:

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

Because:

```text
Different objects
```

---

## equals()

```java
System.out.println(s1.equals(s2));
```

Output:

```text
true
```

because string contents are equal.

---

## Memory Picture

```java
String a = new String("Java");
String b = new String("Java");
```

```text
a ---> Object1
b ---> Object2
```

`==`

```text
Object1 == Object2
false
```

`.equals()`

```text
"Java" == "Java"
true
```

---

# String Pool Questions

Oracle absolutely loves these.

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

Both point to same pooled String.

---

## New Object

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

But:

```java
System.out.println(s1.equals(s2));
```

Output:

```text
true
```

---

## String Concatenation Trap

### Compile-time constant

```java
String a = "Ja";
String b = "va";

String c = "Java";
String d = "Ja" + "va";

System.out.println(c == d);
```

Output:

```text
true
```

Compiler optimizes.

---

### Runtime Concatenation

```java
String a = "Ja";
String b = "va";

String c = "Java";
String d = a + b;

System.out.println(c == d);
```

Output:

```text
false
```

New String created at runtime.

---

# equals() and null

Safe:

```java
"Java".equals(str);
```

Unsafe:

```java
str.equals("Java");
```

If:

```java
str == null
```

you get:

```text
NullPointerException
```

---

# 3. Logical Operators

## AND

```java
&&
```

Both operands must be true.

Truth table:

```text
true  && true  -> true
true  && false -> false
false && true  -> false
false && false -> false
```

---

## OR

```java
||
```

One true is enough.

```text
true  || true  -> true
true  || false -> true
false || true  -> true
false || false -> false
```

---

## NOT

```java
!
```

```java
!true
```

Output:

```text
false
```

---

# 4. Short-Circuit Behavior

Extremely important.

---

## &&

If first operand is false:

```java
false && anything
```

Java skips the rest.

Example:

```java
int x = 0;

if(x != 0 && 10 / x > 1) {
}
```

No exception.

Because:

```java
x != 0
```

is false, therefore second part is skipped.

---

## ||

If first operand is true:

```java
true || anything
```

Second side skipped.

Example:

```java
int x = 5;

if(x > 0 || ++x > 10) {
}
```

`++x` never executes.

Final:

```java
x = 5
```

---

# Non-Short-Circuit Operators

Many candidates forget these.

```java
&
|
```

When used with booleans:

```java
boolean a = false;
boolean b = true;

System.out.println(a & b);
```

Output:

```text
false
```

BUT both sides are evaluated.

---

## Oracle Favorite

```java
int x = 0;

boolean result =
    (x > 1) & (++x > 2);

System.out.println(x);
```

Output:

```text
1
```

because `&` evaluates both operands.

---

Compare:

```java
int x = 0;

boolean result =
    (x > 1) && (++x > 2);

System.out.println(x);
```

Output:

```text
0
```

---

# 5. Increment / Decrement Operators

Most dangerous exam topic.

---

## Prefix Increment

```java
++x
```

Increment first.

Return new value.

Example:

```java
int x = 5;

System.out.println(++x);
```

Output:

```text
6
```

Final:

```text
x = 6
```

---

## Postfix Increment

```java
x++
```

Return current value first.

Increment afterward.

```java
int x = 5;

System.out.println(x++);
```

Output:

```text
5
```

Final:

```text
x = 6
```

---

# Prefix vs Postfix Example

```java
int x = 5;

int y = ++x;
```

Result:

```text
x = 6
y = 6
```

---

```java
int x = 5;

int y = x++;
```

Result:

```text
x = 6
y = 5
```

---

# Oracle Favorite Trap

```java
int x = 5;

int y = x++ + ++x;
```

Evaluation:

```java
5 + 7
```

Output:

```text
12
```

Final:

```java
x = 7
```

---

# Another Oracle Trap

```java
int x = 1;

x = x++;
```

Steps:

```java
temp = 1
x becomes 2
assignment x = temp
```

Result:

```java
x == 1
```

Many candidates answer 2.

---

## Similar

```java
int x = 1;

x = ++x;
```

Result:

```java
2
```

---

# Exam Trick: Final Variables

```java
final int x = 5;

x++;
```

Compile error.

---

# Numeric Promotion

Frequently tested together with operators.

---

## byte, short, char Become int

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

becomes `int`.

Correct:

```java
int c = a + b;
```

or

```java
byte c = (byte)(a + b);
```

---

# Compound Assignment Exception

Oracle loves this.

```java
byte x = 10;

x += 5;
```

Compiles.

Equivalent (approximately):

```java
x = (byte)(x + 5);
```

---

But:

```java
x = x + 5;
```

Compile error.

---

# Must-Know Exam Gotchas

## Gotcha #1

```java
System.out.println(1 + 2 + "3");
```

Output:

```text
33
```

Because:

```java
1 + 2 = 3
3 + "3" = "33"
```

---

## Gotcha #2

```java
System.out.println("1" + 2 + 3);
```

Output:

```text
123
```

String concatenation starts immediately.

---

## Gotcha #3

```java
System.out.println(null == null);
```

Output:

```text
true
```

---

## Gotcha #4

```java
String s = null;

System.out.println(s == null);
```

Output:

```text
true
```

No exception.

---

## Gotcha #5

```java
String s = null;

System.out.println(s.equals("Java"));
```

Throws:

```text
NullPointerException
```

---

# High-Probability Exam Questions

---

### Question 1

```java
int x = 2;
int y = x++ * ++x;

System.out.println(y);
```

Answer:

```text
8
```

Steps:

```java
2 * 4 = 8
```

Final:

```java
x = 4
```

---

### Question 2

```java
String s1 = "Java";
String s2 = "Ja" + "va";

System.out.println(s1 == s2);
```

Answer:

```text
true
```

Compile-time constant.

---

### Question 3

```java
String a = "Ja";
String b = "va";

String c = a + b;

System.out.println(c == "Java");
```

Answer:

```text
false
```

Runtime concatenation.

---

### Question 4

```java
int x = 0;

boolean r = false && (++x > 0);

System.out.println(x);
```

Answer:

```text
0
```

Short-circuit.

---

### Question 5

```java
int x = 0;

boolean r = false & (++x > 0);

System.out.println(x);
```

Answer:

```text
1
```

No short-circuit.

---

### Question 6

```java
byte b = 10;

b += 5;
System.out.println(b);
```

Answer:

```text
15
```

Compiles due to compound assignment.

---

# Last-Minute Exam Cheatsheet

Memorize:

✅ `==` → primitive values OR reference identity

✅ `.equals()` → object content equality

✅ `&&` and `||` short-circuit

✅ `&` and `|` always evaluate both sides

✅ Prefix → modify first (`++x`)

✅ Postfix → use first (`x++`)

✅ Integer division truncates decimals

✅ `byte + byte` → `int`

✅ `x = x++` leaves x unchanged

✅ String pool can make `==` return `true`

✅ `"Java".equals(str)` is null-safe

If you master every example above, you'll be in excellent shape for nearly all operator-related questions on the Java SE 21 Developer Professional (1Z0-830) exam.
