# Java SE 21 Developer Professional (1Z0-830) Mock Exam

## Topic 9: Operators

This mock exam is intentionally designed in an **Oracle-style** manner:

- Multiple correct answers where appropriate.
- Tricky operator precedence.
- Side effects in expressions.
- Common certification traps.
- Focus on what Oracle exam writers historically love to test.

---

# Question 1 — Arithmetic Precedence

What is the output of the following code?

```java
public class Test {
    public static void main(String[] args) {
        int x = 2 + 3 * 4;
        int y = (2 + 3) * 4;

        System.out.println(x + " " + y);
    }
}
```

### Answer

```text
14 20
```

### Explanation

Multiplication has higher precedence than addition.

```java
x = 2 + (3 * 4); // 14
y = (2 + 3) * 4; // 20
```

**Certification Trap:** Oracle frequently tests operator precedence before introducing more complicated expressions.

---

# Question 2 — Integer Division

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(5 / 2);
        System.out.println(5.0 / 2);
    }
}
```

### Answer

```text
2
2.5
```

### Explanation

When both operands are integers:

```java
5 / 2 == 2
```

Fractional parts are discarded.

When one operand is floating-point:

```java
5.0 / 2 == 2.5
```

**Oracle Favorite:** Mixing integer and floating-point arithmetic.

---

# Question 3 — Modulus Operator

What is the output?

```java
public class Test {
    public static void.main(String[] args) {
        System.out.println(10 % 3);
        System.out.println(11 % 3);
    }
}
```

### Answer

Compilation fails.

### Explanation

There is a typo:

```java
public static void.main
```

should be

```java
public static void main
```

**Certification Trap:** Oracle often hides syntax errors inside operator questions.

---

# Question 4 — Increment Madness

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;

        System.out.println(x++);
        System.out.println(++x);
    }
}
```

### Answer

```text
5
7
```

### Explanation

Post-increment:

```java
x++
```

returns old value, then increments.

```java
print 5
x becomes 6
```

Pre-increment:

```java
++x
```

increments first.

```java
x becomes 7
print 7
```

---

# Question 5 — Oracle Classic Increment Trap

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 1;

        x = x++;

        System.out.println(x);
    }
}
```

### Answer

```text
1
```

### Explanation

Step-by-step:

```java
x++
```

returns 1

then x becomes 2

afterward:

```java
x = 1;
```

So final value is:

```java
1
```

This is one of Oracle's all-time favorite questions.

---

# Question 6 — Multiple Increments

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;

        int y = x++ + ++x;

        System.out.println(y);
        System.out.println(x);
    }
}
```

### Answer

```text
12
7
```

### Explanation

Start:

```java
x = 5
```

First term:

```java
x++
```

returns 5

x becomes 6

Second term:

```java
++x
```

x becomes 7

returns 7

Therefore:

```java
y = 5 + 7 = 12
```

Final x:

```java
7
```

---

# Question 7 — Equality with Primitives

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int a = 10;
        int b = 10;

        System.out.println(a == b);
    }
}
```

### Answer

```text
true
```

### Explanation

For primitives:

```java
==
```

compares values.

Both are 10.

---

# Question 8 — == versus equals()

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String a = new String("Java");
        String b = new String("Java");

        System.out.println(a == b);
        System.out.println(a.equals(b));
    }
}
```

### Answer

```text
false
true
```

### Explanation

```java
==
```

compares references.

Two separate objects:

```java
a != b
```

so:

```java
false
```

```java
equals()
```

compares String contents:

```java
true
```

**Oracle Favorite:** Object identity vs object equality.

---

# Question 9 — String Pool Trap

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String a = "Java";
        String b = "Java";

        System.out.println(a == b);
    }
}
```

### Answer

```text
true
```

### Explanation

String literals are interned in the string pool.

Both references point to the same object.

```java
a == b
```

is true.

**Very High Probability Exam Topic.**

---

# Question 10 — equals() Null Trap

What happens?

```java
public class Test {
    public static void main(String[] args) {
        String s = null;

        System.out.println(s.equals("Java"));
    }
}
```

### Answer

Runtime exception.

```text
NullPointerException
```

### Explanation

You cannot invoke a method on a null reference.

---

# Question 11 — Short-Circuit AND

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 0;

        if (x != 0 && 10 / x > 1) {
            System.out.println("A");
        }

        System.out.println("Done");
    }
}
```

### Answer

```text
Done
```

### Explanation

First expression:

```java
x != 0
```

is false.

Since `&&` short-circuits:

```java
10 / x
```

is never evaluated.

No exception occurs.

---

# Question 12 — Non-Short-Circuit AND

What happens?

```java
public class Test {
    public static void main(String[] args) {
        int x = 0;

        if (x != 0 & 10 / x > 1) {
            System.out.println("A");
        }
    }
}
```

### Answer

Runtime exception.

```text
ArithmeticException
```

### Explanation

Single ampersand:

```java
&
```

evaluates BOTH operands.

Therefore:

```java
10 / 0
```

is executed.

---

# Question 13 — Short-Circuit OR

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 0;

        if (true || ++x > 0) {
        }

        System.out.println(x);
    }
}
```

### Answer

```text
0
```

### Explanation

For:

```java
true || anything
```

the second operand is never evaluated.

Therefore:

```java
++x
```

never runs.

---

# Question 14 — Logical NOT

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        boolean b = false;

        System.out.println(!b);
    }
}
```

### Answer

```text
true
```

---

# Question 15 — Oracle Precedence Nightmare

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 1;
        int y = 2;

        boolean result = x + y * 2 > 4 && y++ > 1;

        System.out.println(result);
        System.out.println(y);
    }
}
```

### Answer

```text
true
3
```

### Explanation

First expression:

```java
1 + (2 * 2)
```

equals

```java
5
```

Then:

```java
5 > 4
```

true

Second operand evaluated:

```java
y++ > 1
```

returns:

```java
2 > 1
```

true

Then:

```java
y becomes 3
```

Result:

```java
true
```

---

# Question 16 — Side Effects with Short-Circuit

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 1;

        boolean result = false && (++x > 0);

        System.out.println(x);
    }
}
```

### Answer

```text
1
```

### Explanation

Because the left side is false:

```java
false && anything
```

right side never executes.

No increment occurs.

---

# Question 17 — Exam-Level Gotcha

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 4;

        System.out.println(x++ + x++);
    }
}
```

### Answer

```text
9
```

### Explanation

First:

```java
x++ -> 4
```

x becomes 5

Second:

```java
x++ -> 5
```

x becomes 6

Result:

```java
4 + 5 = 9
```

---

# Question 18 — High Probability Certification Style

How many lines compile?

```java
String a = "java";
String b = "java";
String c = new String("java");

System.out.println(a == b);       // Line 1
System.out.println(a == c);       // Line 2
System.out.println(a.equals(c));  // Line 3
```

### Answer

```text
3 lines compile
```

Outputs:

```text
true
false
true
```

### Explanation

- Line 1 → same pooled literal
- Line 2 → different objects
- Line 3 → same contents

---

# Quick Oracle Exam Memory Sheet

### `==`

- Primitive → compare values
- Object → compare references

### `.equals()`

- Compare contents (if overridden)

### `&&`

- Short-circuit AND

### `||`

- Short-circuit OR

### `&`

- Always evaluates both operands

### `|`

- Always evaluates both operands

### `x++`

- Return value first
- Increment second

### `++x`

- Increment first
- Return value second

### Top 5 Operator Traps Oracle Loves

1. `x = x++;`
2. `x++ + ++x`
3. `&&` vs `&`
4. `==` vs `.equals()`
5. String pool (`"Java" == "Java"` vs `new String("Java")`)

If you can answer all 18 of these without making a mistake, you're covering roughly **80–90% of the operator-related trick questions that historically appear in Oracle Java certification exams.**
