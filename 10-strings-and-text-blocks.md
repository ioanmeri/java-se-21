# Java SE 21 Developer Professional (1Z0-830)

## 🧵 10. Strings & Text Blocks

- [ ] String immutability
- [ ] `StringBuilder` vs `String`
- [ ] `.equals()` vs `==`
- [ ] Text blocks (`"""`) basics

# Topic 10: Strings & Text Blocks — Exam-Focused Guide

This is one of Oracle's **favorite exam areas** because it combines:

- Object immutability
- Memory concepts
- Method behavior
- Operator behavior
- String pool questions
- Compile-time vs runtime values
- Text block syntax

Many candidates lose points here because the questions often look simple but contain subtle traps.

---

# 1. String Immutability

## What is a String?

A `String` is an object representing a sequence of characters.

```java
String s = "Java";
```

Strings are **immutable**.

**Immutable = cannot be changed after creation.**

---

## What actually happens?

```java
String s = "Java";
s.concat(" SE");
```

Many beginners think:

```java
Java SE
```

is stored in `s`.

Wrong.

The original String remains unchanged.

`concat()` creates a NEW String.

```java
String s = "Java";
s.concat(" SE");

System.out.println(s);
```

Output:

```text
Java
```

---

## Correct way

```java
String s = "Java";
s = s.concat(" SE");

System.out.println(s);
```

Output:

```text
Java SE
```

---

# Important Exam Rule

Every String method that appears to modify the String actually returns a new String.

Examples:

```java
trim()
strip()
replace()
concat()
substring()
toUpperCase()
toLowerCase()
repeat()
indent()
```

None modify the original object.

---

## Example

```java
String s = "java";

s.toUpperCase();

System.out.println(s);
```

Output:

```text
java
```

---

# Memory View

```java
String s1 = "Java";
```

Creates an object in the String Pool.

```java
String s2 = s1.concat(" SE");
```

Now:

```text
"Java"
"Java SE"
```

are two different String objects.

---

# Oracle Favorite Question

```java
String s = "cat";
s.replace('c', 'b');

System.out.println(s);
```

Output?

✅

```text
cat
```

Because the result was ignored.

---

# 2. String Pool

You MUST know this.

---

## Literals use the String Pool

```java
String a = "Java";
String b = "Java";
```

Only ONE pooled object exists.

```java
a == b
```

is:

```java
true
```

---

## new String()

```java
String a = "Java";
String b = new String("Java");
```

Now there are TWO objects.

```java
a == b
```

is:

```java
false
```

---

## Example

```java
String a = "Java";
String b = "Ja" + "va";
```

Compile-time concatenation.

Both reference the same pooled object.

```java
a == b
```

✅ true

---

## Runtime Concatenation

```java
String a = "Java";

String x = "Ja";
String b = x + "va";
```

Runtime creation.

```java
a == b
```

✅ false

```java
a.equals(b)
```

✅ true

---

# Exam Trap

```java
final String x = "Ja";

String a = "Java";
String b = x + "va";
```

Because `x` is `final`, the compiler treats it as a constant.

Result:

```java
a == b
```

✅ true

This appears frequently on Oracle exams.

---

# 3. equals() vs ==

This is probably the most tested String concept.

---

## ==

Compares references.

Question:

> Are these variables pointing to the same object?

---

Example:

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

Different objects.

---

## equals()

Compares contents.

Question:

> Do these Strings contain the same characters?

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

---

# Quick Memory Rule

```java
==
```

same object?

```java
equals()
```

same text?

---

# Exam Favorite

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
false
true
```

---

# Null Trap

```java
String s = null;

System.out.println(s.equals("Java"));
```

Runtime:

```text
NullPointerException
```

---

Safer:

```java
"Java".equals(s)
```

Output:

```text
false
```

No exception.

Oracle likes this.

---

# 4. StringBuilder

StringBuilder is mutable.

This is the biggest difference.

---

## String

```java
String s = "A";
s = s + "B";
s = s + "C";
```

Creates multiple String objects.

---

## StringBuilder

```java
StringBuilder sb = new StringBuilder("A");

sb.append("B");
sb.append("C");
```

Same object modified.

---

# Why StringBuilder Exists

Efficiency.

Instead of creating:

```text
A
AB
ABC
```

as separate Strings,

StringBuilder changes its internal character buffer.

---

# Common Methods

## append()

```java
StringBuilder sb =
    new StringBuilder("Java");

sb.append(" SE");
```

Result:

```text
Java SE
```

---

## insert()

```java
StringBuilder sb =
    new StringBuilder("Java");

sb.insert(4, " SE");
```

Result:

```text
Java SE
```

---

## delete()

```java
StringBuilder sb =
    new StringBuilder("Java");

sb.delete(1,3);
```

Removes indexes:

```text
1 and 2
```

Result:

```text
Ja
```

---

## deleteCharAt()

```java
sb.deleteCharAt(0);
```

---

## reverse()

```java
sb.reverse();
```

---

## replace()

```java
sb.replace(0,4,"Code");
```

---

# Oracle Trap: Chaining

```java
StringBuilder sb =
     new StringBuilder("Java");

sb.append(" SE")
  .append(" 21")
  .reverse();
```

Legal.

Most methods return the same builder.

---

# StringBuilder Equality Trap

```java
StringBuilder sb1 =
    new StringBuilder("Java");

StringBuilder sb2 =
    new StringBuilder("Java");

System.out.println(sb1.equals(sb2));
```

Output:

```text
false
```

Because StringBuilder does NOT override `equals()`.

It inherits Object's implementation.

Equivalent to:

```java
sb1 == sb2
```

---

# Oracle Loves This

```java
StringBuilder sb1 =
    new StringBuilder("abc");

StringBuilder sb2 =
    sb1;

sb1.append("d");

System.out.println(sb2);
```

Output:

```text
abcd
```

Both variables reference the same object.

---

# StringBuilder vs String

| Feature                       | String         | StringBuilder |
| ----------------------------- | -------------- | ------------- |
| Mutable                       | No             | Yes           |
| Thread-safe                   | N/A            | No            |
| Stored in pool                | Yes (literals) | No            |
| Concatenation creates objects | Yes            | No            |
| equals() compares content     | Yes            | No            |

---

# Exam Trap: StringBuilder + String

```java
StringBuilder sb =
    new StringBuilder("Java");

String s = sb.toString();
```

Now:

```java
s.equals("Java")
```

✅ true

---

But:

```java
sb.equals(new StringBuilder("Java"))
```

✅ false

---

# StringBuilder Capacity

Oracle occasionally asks this.

```java
StringBuilder sb =
    new StringBuilder();
```

Default capacity:

```text
16
```

---

```java
new StringBuilder("Java")
```

Capacity:

```text
16 + 4 = 20
```

Length:

```text
4
```

---

Example:

```java
StringBuilder sb =
       new StringBuilder("Java");

System.out.println(sb.length());
System.out.println(sb.capacity());
```

Output:

```text
4
20
```

---

# 5. Text Blocks

Introduced to simplify multiline Strings.

---

## Basic Syntax

```java
String html = """
        <html>
            <body>
                Hello
            </body>
        </html>
        """;
```

---

# Rules

Opening delimiter:

```java
"""
```

must be followed by a newline.

This is illegal:

```java
String s = """Hello""";
```

❌ Compile error

---

Correct:

```java
String s = """
Hello
""";
```

✅

---

# Line Breaks Preserved

```java
String s = """
A
B
C
""";
```

Contains:

```text
A\n
B\n
C\n
```

---

# Trailing Newline

```java
String s = """
Java
""";
```

Actually contains:

```text
Java\n
```

Oracle loves testing this.

---

## Avoid Final Newline

Use:

```java
String s = """
Java\
""";
```

Result:

```text
Java
```

with no terminating newline.

---

# Escapes Still Work

```java
String s = """
Java\t21
""";
```

Contains tab.

---

```java
String s = """
Java \"SE\"
""";
```

Legal.

---

# Text Block and Quotes

Normal String:

```java
String s =
"<name=\"John\">";
```

Text block:

```java
String s = """
<name="John">
""";
```

Much easier.

---

# Indentation

Java automatically removes common indentation.

```java
String s = """
        A
        B
        C
        """;
```

Result:

```text
A
B
C
```

not:

```text
        A
        B
        C
```

---

# Oracle Favorite Text Block Question

```java
String s = """
           Java
           """;

System.out.print(s.length());
```

The hidden newline is included.

Answer:

```text
5
```

for "Java\n":

```text
4 + 1 = 5
```

---

# Most Common Certification Traps

## Trap #1

```java
String s = "Java";

s.toUpperCase();

System.out.println(s);
```

Output:

```text
Java
```

NOT JAVA

---

## Trap #2

```java
String a = "Java";
String b = "Ja" + "va";

System.out.println(a == b);
```

✅ true

Compile-time constant.

---

## Trap #3

```java
String x = "Ja";
String b = x + "va";
```

✅ Runtime creation.

```java
a == b
```

false

---

## Trap #4

```java
StringBuilder sb =
    new StringBuilder("Java");

String s =
     sb.append("SE").toString();

System.out.println(s);
```

Output:

```text
JavaSE
```

---

## Trap #5

```java
StringBuilder sb1 =
      new StringBuilder("abc");

StringBuilder sb2 =
      new StringBuilder("abc");

System.out.println(sb1.equals(sb2));
```

Output:

```text
false
```

---

## Trap #6

```java
StringBuilder sb =
      new StringBuilder("abc");

sb.delete(1,3);

System.out.println(sb);
```

Output:

```text
a
```

End index excluded.

---

## Trap #7

```java
String s = """
Java
""";
```

Contains newline.

Length:

```text
5
```

not 4.

---

# Oracle-Style Practice Questions

## Question 1

```java
String s = "Java";
s.concat("21");

System.out.println(s);
```

### Answer

```text
Java
```

Reason: String immutable.

---

## Question 2

```java
String a = "Java";
String b = "Ja" + "va";

System.out.println(a == b);
```

### Answer

```text
true
```

Compile-time constant folding.

---

## Question 3

```java
String a = "Java";
String x = "Ja";
String b = x + "va";

System.out.println(a == b);
```

### Answer

```text
false
```

Runtime concatenation.

---

## Question 4

```java
StringBuilder sb =
       new StringBuilder("abc");

sb.reverse();

System.out.println(sb);
```

### Answer

```text
cba
```

---

## Question 5

```java
StringBuilder sb =
      new StringBuilder("abc");

sb.append("d")
  .insert(1, "X");

System.out.println(sb);
```

### Answer

```text
aXbcd
```

---

## Question 6

```java
StringBuilder sb1 =
      new StringBuilder("Java");

StringBuilder sb2 = sb1;

sb1.append("21");

System.out.println(sb2);
```

### Answer

```text
Java21
```

Same object.

---

## Question 7

```java
String s = """
Hi
""";

System.out.print(s.length());
```

### Answer

```text
3
```

Characters:

```text
H i \n
```

---

# Final Exam Cheat Sheet

Memorize these:

✅ Strings are immutable

✅ String methods return new Strings

✅ `==` compares references

✅ `.equals()` compares String contents

✅ String literals are stored in the String Pool

✅ Compile-time concatenation uses the pool

✅ Runtime concatenation creates new objects

✅ `final` constants may allow compile-time concatenation

✅ StringBuilder is mutable

✅ StringBuilder `.equals()` compares references, not content

✅ `delete(start,end)` excludes end

✅ Default StringBuilder capacity = 16

✅ Text blocks preserve line breaks

✅ Text blocks usually include a trailing newline

✅ Opening `"""` must be followed by a newline

If you master everything above, you'll be able to answer **90–95% of the String/Text Block questions typically seen on the 1Z0-830 exam**.
