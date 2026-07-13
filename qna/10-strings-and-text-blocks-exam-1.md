# Java SE 21 Developer Professional (1Z0-830)

# Mock Exam – Strings & Text Blocks

This mock exam is designed to imitate Oracle's style:

- Small code snippets
- Multiple plausible answers
- String Pool traps
- `==` vs `.equals()` traps
- String immutability traps
- `StringBuilder` mutability traps
- Text Block newline/indentation traps
- Compile-time vs runtime concatenation traps

---

# Question 1

What is the output?

```java
String s = "Java";
s.concat("21");

System.out.println(s);
```

### A)

```text
Java
```

### B)

```text
Java21
```

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ A

```text
Java
```

### Explanation

`String` is immutable.

`concat()` returns a new String.

Since the result is ignored, `s` remains unchanged.

---

# Question 2

What is the output?

```java
String s = "Java";

s = s.concat("21");

System.out.println(s);
```

### A)

```text
Java
```

### B)

```text
Java21
```

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ B

```text
Java21
```

### Explanation

A new String is created and assigned back to `s`.

---

# Question 3

What is the output?

```java
String s = "java";

s.toUpperCase();

System.out.println(s);
```

### A)

```text
JAVA
```

### B)

```text
java
```

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ B

```text
java
```

### Oracle Trap

Students often assume `toUpperCase()` modifies the String.

It does not.

---

# Question 4

What is the output?

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Depends on JVM

---

## Answer

✅ A

```text
true
```

### Explanation

Both literals point to the same pooled String.

---

# Question 5

What is the output?

```java
String a = "Java";
String b = new String("Java");

System.out.println(a == b);
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ B

```text
false
```

### Explanation

`new String()` creates a new object.

Different references.

---

# Question 6

What is the output?

```java
String a = "Java";
String b = new String("Java");

System.out.println(a.equals(b));
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ A

```text
true
```

### Explanation

`.equals()` compares characters, not references.

---

# Question 7

What is the output?

```java
String a = "Java";
String b = "Ja" + "va";

System.out.println(a == b);
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ A

```text
true
```

### Oracle Favorite

The compiler evaluates:

```java
"Ja" + "va"
```

into:

```java
"Java"
```

at compile time.

Only one pooled object exists.

---

# Question 8

What is the output?

```java
String x = "Ja";

String a = "Java";
String b = x + "va";

System.out.println(a == b);
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Depends on JVM

---

## Answer

✅ B

```text
false
```

### Explanation

This concatenation happens at runtime.

A new String object is created.

---

# Question 9

What is the output?

```java
final String x = "Ja";

String a = "Java";
String b = x + "va";

System.out.println(a == b);
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ A

```text
true
```

### Oracle Favorite

Because `x` is `final`, it is treated as a compile-time constant.

The compiler folds:

```java
x + "va"
```

into:

```java
"Java"
```

---

# Question 10

What is the output?

```java
String s = null;

System.out.println("Java".equals(s));
```

### A)

true

### B)

false

### C)

NullPointerException

### D)

Compilation error

---

## Answer

✅ B

```text
false
```

### Explanation

The literal `"Java"` is never `null`, so `.equals()` executes safely.

---

# Question 11

What happens?

```java
String s = null;

System.out.println(s.equals("Java"));
```

### A)

false

### B)

true

### C)

Compilation error

### D)

NullPointerException

---

## Answer

✅ D

```text
NullPointerException
```

### Oracle Trap

This appears frequently.

---

# Question 12

What is the output?

```java
StringBuilder sb =
    new StringBuilder("Java");

sb.append("21");

System.out.println(sb);
```

### A)

Java

### B)

Java21

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ B

```text
Java21
```

### Explanation

`StringBuilder` is mutable.

---

# Question 13

What is the output?

```java
StringBuilder sb1 =
    new StringBuilder("Java");

StringBuilder sb2 = sb1;

sb1.append("21");

System.out.println(sb2);
```

### A)

Java

### B)

Java21

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ B

```text
Java21
```

### Explanation

Both references point to the same builder.

---

# Question 14

What is the output?

```java
StringBuilder sb =
    new StringBuilder("abc");

sb.delete(1, 3);

System.out.println(sb);
```

### A)

ab

### B)

bc

### C)

a

### D)

abc

---

## Answer

✅ C

```text
a
```

### Oracle Trap

`delete(start,end)`

removes:

```java
1 and 2
```

End index is excluded.

---

# Question 15

What is the output?

```java
StringBuilder sb =
    new StringBuilder("abc");

sb.reverse();

System.out.println(sb);
```

### A)

abc

### B)

cba

### C)

cab

### D)

Compilation error

---

## Answer

✅ B

```text
cba
```

---

# Question 16

What is the output?

```java
StringBuilder sb1 =
    new StringBuilder("abc");

StringBuilder sb2 =
    new StringBuilder("abc");

System.out.println(sb1.equals(sb2));
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ B

```text
false
```

### Oracle Favorite

Many candidates incorrectly expect content comparison.

`StringBuilder` does NOT override `equals()`.

---

# Question 17

What is the output?

```java
StringBuilder sb =
    new StringBuilder("abc");

String s = sb.toString();

System.out.println(s.equals("abc"));
```

### A)

true

### B)

false

### C)

Compilation error

### D)

Runtime exception

---

## Answer

✅ A

```text
true
```

### Explanation

`toString()` returns a String object.

String's `equals()` compares content.

---

# Question 18

What is the output?

```java
StringBuilder sb =
    new StringBuilder("Java");

System.out.println(sb.length());
System.out.println(sb.capacity());
```

### A)

4 and 16

### B)

4 and 20

### C)

20 and 4

### D)

16 and 4

---

## Answer

✅ B

```text
4
20
```

### Explanation

Length:

```text
4
```

Capacity:

```text
16 + 4 = 20
```

---

# Question 19

Will this compile?

```java
String s = """Hello""";
```

### A)

Yes

### B)

No

---

## Answer

✅ B

### Explanation

A text block opening delimiter must be followed by a newline.

---

# Question 20

What is the value of `s`?

```java
String s = """
Hello
""";
```

### A)

```text
Hello
```

### B)

```text
Hello\n
```

### C)

Empty String

### D)

Compilation Error

---

## Answer

✅ B

```text
Hello\n
```

### Oracle Favorite

The trailing newline is part of the text block.

---

# Question 21

What is the output?

```java
String s = """
Hi
""";

System.out.print(s.length());
```

### A)

2

### B)

3

### C)

4

### D)

5

---

## Answer

✅ B

```text
3
```

### Explanation

Characters:

```text
H
i
\n
```

---

# Question 22

What is the output?

```java
String s = """
Java\
""";

System.out.print(s.length());
```

### A)

4

### B)

5

### C)

6

### D)

Compilation error

---

## Answer

✅ A

```text
4
```

### Explanation

The trailing `\` removes the final newline.

---

# Question 23

What is the output?

```java
String s = """
        A
        B
        C
        """;

System.out.print(s);
```

### A)

Spaces appear before every letter

### B)

A B C on one line

### C)

Common indentation is removed

### D)

Compilation error

---

## Answer

✅ C

### Explanation

Text blocks automatically remove common incidental indentation.

---

# Question 24

Choose ALL correct statements.

### A)

Strings are immutable.

### B)

StringBuilder is immutable.

### C)

`==` compares object references.

### D)

`StringBuilder.equals()` compares text content.

### E)

Text blocks may contain multiple lines.

---

## Answer

✅ A

✅ C

✅ E

### Explanation

`StringBuilder` is mutable.

`StringBuilder.equals()` behaves like `Object.equals()`.

---

# Question 25 (Very Oracle-Like)

What is the output?

```java
String a = "Java";

StringBuilder sb =
    new StringBuilder("Ja");

sb.append("va");

System.out.println(a.equals(sb));
System.out.println(a == sb.toString());
```

### A)

true true

### B)

true false

### C)

false true

### D)

false false

---

## Answer

✅ D

```text
false
false
```

### Explanation

First line:

```java
a.equals(sb)
```

A String is never equal to a StringBuilder.

Result:

```text
false
```

Second line:

```java
a == sb.toString()
```

`sb.toString()` creates a String object at runtime.

Different references.

Result:

```text
false
```

---

# Last-Minute Exam Gotchas to Memorize

### Gotcha #1

```java
s.toUpperCase();
```

does NOT change `s`.

---

### Gotcha #2

```java
==
```

compares references.

---

### Gotcha #3

```java
.equals()
```

compares String contents.

---

### Gotcha #4

```java
new String(...)
```

almost always makes `==` become `false`.

---

### Gotcha #5

```java
"Ja" + "va"
```

is compile-time.

```java
x + "va"
```

is runtime unless `x` is `final`.

---

### Gotcha #6

```java
StringBuilder.equals()
```

does NOT compare contents.

---

### Gotcha #7

```java
delete(start,end)
```

excludes `end`.

---

### Gotcha #8

Most text blocks contain a hidden trailing newline.

---

### Gotcha #9

```java
"""
Hello
"""
```

Length is usually one more than you expect because of `\n`.

---

### Gotcha #10

The opening text block delimiter must be followed by a newline:

```java
String s = """
Hello
""";
```

✅ Valid

```java
String s = """Hello""";
```

❌ Invalid

If you can score **22+/25** consistently on questions like these, you are generally in very good shape for the Strings & Text Blocks objectives of the 1Z0-830 exam.
