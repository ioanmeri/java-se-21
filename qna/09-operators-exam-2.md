# Java SE 21 Developer Professional (1Z0-830)

# Mock Test #2 — Operators (Hard Mode)

This set is intentionally **slightly harder than a real exam**, because if you can consistently solve these, the actual certification questions should feel easier.

Focus Areas:

✅ Arithmetic & Precedence  
✅ `==` vs `.equals()`  
✅ Logical Operators  
✅ Short-Circuit Evaluation  
✅ Increment/Decrement Traps  
✅ Oracle-Style Side Effects

---

# Question 1

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 2;

        System.out.println(x + x * x++);
    }
}
```

### Answer

```text
6
```

### Explanation

Java evaluates operands left-to-right.

Initial:

```java
x = 2
```

Expression:

```java
x + x * x++
```

Left operand:

```java
x = 2
```

Then:

```java
x * x++
```

Uses:

```java
2 * 2
```

then x becomes:

```java
3
```

Result:

```java
2 + 4 = 6
```

---

# Question 2

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;

        System.out.println(++x + x++);
    }
}
```

### Answer

```text
12
```

### Explanation

```java
x = 5
```

First:

```java
++x
```

becomes 6

Second:

```java
x++
```

returns 6 then becomes 7

Result:

```java
6 + 6 = 12
```

---

# Question 3

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;

        int y = x++ + x++ + ++x;

        System.out.println(y);
        System.out.println(x);
    }
}
```

### Answer

```text
18
8
```

### Explanation

```java
x++  -> 5   (x=6)
x++  -> 6   (x=7)
++x  -> 8   (x=8)
```

Result:

```java
5 + 6 + 8 = 19
```

Wait...

Let's calculate carefully:

```java
5 + 6 + 8 = 19
```

### Correct Answer

```text
19
8
```

### Oracle Lesson

Always recalculate. These are exactly the kinds of questions Oracle uses because candidates rush.

---

# Question 4

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(10 + 20 / 5 * 2);
    }
}
```

### Answer

```text
18
```

### Explanation

Division and multiplication have equal precedence.

Evaluate left-to-right:

```java
20 / 5 = 4
4 * 2 = 8
10 + 8 = 18
```

---

# Question 5

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(2 + 3 * 4 > 10);
    }
}
```

### Answer

```text
true
```

### Explanation

```java
2 + 12 = 14
14 > 10
```

Result:

```java
true
```

---

# Question 6

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        Integer a = 100;
        Integer b = 100;

        System.out.println(a == b);
    }
}
```

### Answer

```text
true
```

### Explanation

Wrapper cache:

```java
-128 to 127
```

Values in this range may reference the same cached object.

Oracle loves this.

---

# Question 7

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        Integer a = 200;
        Integer b = 200;

        System.out.println(a == b);
    }
}
```

### Answer

```text
false
```

### Explanation

200 is outside the Integer cache.

Different objects.

```java
a == b
```

compares references.

---

# Question 8

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        Integer a = 200;
        Integer b = 200;

        System.out.println(a.equals(b));
    }
}
```

### Answer

```text
true
```

### Explanation

`equals()` compares values.

---

# Question 9

What happens?

```java
public class Test {
    public static void main(String[] args) {
        String s = null;

        System.out.println("java".equals(s));
    }
}
```

### Answer

```text
false
```

### Explanation

Very common null-safe pattern:

```java
constant.equals(variable)
```

No exception occurs.

---

# Question 10

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 1;

        boolean b = true || (++x > 0);

        System.out.println(x);
    }
}
```

### Answer

```text
1
```

### Explanation

Right side never executes.

Short-circuit OR.

---

# Question 11

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 1;

        boolean b = false | (++x > 0);

        System.out.println(x);
    }
}
```

### Answer

```text
2
```

### Explanation

Single pipe:

```java
|
```

Always evaluates both sides.

---

# Question 12

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 0;

        boolean b = true | (++x > 0);

        System.out.println(x);
    }
}
```

### Answer

```text
1
```

### Explanation

Many candidates incorrectly think OR stops here.

Only:

```java
||
```

short-circuits.

```java
|
```

does not.

---

# Question 13

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;

        if (x > 2 && x++ > 4) {
            x++;
        }

        System.out.println(x);
    }
}
```

### Answer

```text
7
```

### Explanation

Start:

```java
x = 5
```

First condition:

```java
5 > 2
```

true

Second:

```java
x++ > 4
```

returns 5

x becomes 6

Body executes:

```java
x++
```

becomes 7

---

# Question 14

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 1;

        boolean b = !(x++ > 1);

        System.out.println(b);
        System.out.println(x);
    }
}
```

### Answer

```text
true
2
```

### Explanation

```java
x++ > 1
```

means

```java
1 > 1
```

false

Then:

```java
!false
```

true

x becomes 2.

---

# Question 15

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String s1 = "Ja";
        String s2 = "va";

        String s3 = "Java";
        String s4 = s1 + s2;

        System.out.println(s3 == s4);
        System.out.println(s3.equals(s4));
    }
}
```

### Answer

```text
false
true
```

### Explanation

Runtime concatenation creates a new String object.

References differ.

Contents match.

This appears constantly in Oracle exams.

---

# Question 16

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String s1 = "Ja" + "va";
        String s2 = "Java";

        System.out.println(s1 == s2);
    }
}
```

### Answer

```text
true
```

### Explanation

Compile-time constant folding.

Both reference the same pooled String.

---

# Question 17

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int a = 1;
        int b = 2;
        int c = 3;

        System.out.println(a + b > c && c++ > 2);
        System.out.println(c);
    }
}
```

### Answer

```text
false
3
```

### Explanation

First condition:

```java
1 + 2 > 3
```

means

```java
3 > 3
```

false

Short-circuit:

```java
c++ > 2
```

never runs.

c remains 3.

---

# Question 18

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 1;

        System.out.println(x++ + ++x + x++);
    }
}
```

### Answer

```text
8
```

### Explanation

```java
x++ -> 1 (x=2)
++x -> 3 (x=3)
x++ -> 3 (x=4)
```

Result:

```java
1 + 3 + 3 = 7
```

### Correct Answer

```text
7
```

Another classic certification trap.

---

# Question 19 (Very Oracle-ish)

What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 3;

        boolean result =
                x > 2 &&
                x++ > 2 &&
                ++x > 4;

        System.out.println(result);
        System.out.println(x);
    }
}
```

### Answer

```text
true
5
```

### Explanation

```java
x > 2       → true
x++ > 2     → true (returns 3, x=4)
++x > 4     → true (x=5)
```

Final:

```java
true
5
```

---

# Question 20 (Exam Difficulty)

Choose all correct statements.

```java
String a = "java";
String b = new String("java");
String c = "ja" + "va";
```

### A

```java
a == b
```

is true

### B

```java
a == c
```

is true

### C

```java
a.equals(b)
```

is true

### D

```java
b == c
```

is true

### Answer

✅ B

✅ C

### Explanation

```java
a == b      false
a == c      true
a.equals(b) true
b == c      false
```

---

# 1Z0-830 High-Risk Operator Traps Checklist

If you see these during the exam, slow down:

✅ `x = x++`

✅ `x++ + ++x`

✅ `&&` vs `&`

✅ `||` vs `|`

✅ Integer cache (`100` vs `200`)

✅ String pool

✅ Compile-time vs runtime concatenation

✅ Side effects inside boolean expressions

✅ Evaluation order (left-to-right)

✅ `null.equals(...)` vs `"text".equals(null)`

If you can solve these 20 questions comfortably, you're operating at roughly **above the typical operator-question difficulty seen on the actual Java certification exams**.
