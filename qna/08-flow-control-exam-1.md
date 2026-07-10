# Java SE 21 Developer Professional (1Z0-830)

# Flow Control Master Mock Exam

These questions are intentionally written in **Oracle style**:

- "Does it compile?"
- "What is the output?"
- Several answers look plausible.
- Focus on traps and edge cases.

---

# Question 1 — Dangling Else

```java
int x = 1;
int y = 2;

if (x > 0)
    if (y > 0)
        System.out.println("A");
else
    System.out.println("B");
```

What is the output?

### Answer

✅ Output:

```text
A
```

### Explanation

The `else` always belongs to the nearest unmatched `if`.

Equivalent:

```java
if (x > 0) {
    if (y > 0)
        System.out.println("A");
    else
        System.out.println("B");
}
```

Since both conditions are true, `"A"` prints.

---

# Question 2 — Assignment inside if

```java
boolean flag = false;

if (flag = true)
    System.out.println("A");
else
    System.out.println("B");
```

### Answer

✅ Output

```text
A
```

### Explanation

This is assignment:

```java
flag = true
```

not comparison.

The assignment returns:

```java
true
```

Therefore the condition succeeds.

Oracle loves this one.

---

# Question 3 — Invalid Condition

```java
int x = 5;

if (x = 10)
    System.out.println("A");
```

### Answer

❌ Does not compile.

### Explanation

`x = 10` returns an `int`.

`if` requires a `boolean`.

Java does not allow:

```java
if (5)
```

unlike C/C++.

---

# Question 4 — Loop Variable Scope

```java
for(int i = 0; i < 3; i++) {
}

System.out.println(i);
```

### Answer

❌ Does not compile.

### Explanation

`i` exists only inside the `for` loop.

---

# Question 5 — Enhanced For Variable Scope

```java
int[] arr = {1,2,3};

for(int x : arr) {
}

System.out.println(x);
```

### Answer

❌ Does not compile.

`x` is scoped to the enhanced-for loop.

---

# Question 6 — Enhanced For Assignment

```java
int[] arr = {1,2,3};

for(int x : arr)
    x++;

System.out.println(arr[0]);
```

### Answer

✅ Output

```text
1
```

### Explanation

`x` is merely a copy of each element.

The array itself isn't modified.

---

# Question 7 — Classic Fall-through

```java
int x = 1;

switch(x) {
    case 1:
        System.out.print("A");
    case 2:
        System.out.print("B");
    default:
        System.out.print("C");
}
```

### Answer

✅ Output

```text
ABC
```

### Explanation

Missing `break`.

Execution falls through all subsequent cases.

---

# Question 8 — Break Stops Fall-through

```java
int x = 1;

switch(x) {
    case 1:
        System.out.print("A");
        break;
    case 2:
        System.out.print("B");
    default:
        System.out.print("C");
}
```

### Answer

✅ Output

```text
A
```

---

# Question 9 — Modern Switch

```java
int x = 1;

switch(x) {
    case 1 -> System.out.print("A");
    case 2 -> System.out.print("B");
    default -> System.out.print("C");
}
```

### Answer

✅ Output

```text
A
```

### Explanation

No fall-through with `->`.

No `break` needed.

---

# Question 10 — Switch Expression

```java
String s =
    switch(2) {
        case 1 -> "A";
        case 2 -> "B";
        default -> "C";
    };

System.out.println(s);
```

### Answer

✅ Output

```text
B
```

---

# Question 11 — Missing Yield

```java
String s =
    switch(1) {
        case 1: {
            "A";
        }
        default: {
            yield "B";
        }
    };
```

### Answer

❌ Does not compile.

### Explanation

Inside a block switch expression:

```java
{
   ...
}
```

you must use:

```java
yield value;
```

to produce a result.

---

# Question 12 — Correct Yield

```java
String s =
    switch(1) {
        case 1: {
            yield "A";
        }
        default: {
            yield "B";
        }
    };

System.out.println(s);
```

### Answer

✅ Output

```text
A
```

---

# Question 13 — Exhaustive Switch Expression

```java
int x = 1;

String result =
    switch(x) {
        case 1 -> "A";
    };
```

### Answer

❌ Does not compile.

### Explanation

A switch expression must be exhaustive.

`x` can be many values.

Need:

```java
default -> ...
```

---

# Question 14 — Constant Selector

```java
String result =
    switch(1) {
        case 1 -> "A";
    };
```

### Answer

✅ Compiles.

### Explanation

Compiler knows selector is always `1`.

Very sneaky exam trap.

---

# Question 15 — Null in Switch

```java
Object obj = null;

switch(obj) {
    case null -> System.out.println("Null");
    default -> System.out.println("Other");
}
```

### Answer

✅ Output

```text
Null
```

---

# Question 16 — Switch without Null Case

```java
Object obj = null;

switch(obj) {
    case String s -> System.out.println("String");
    default -> System.out.println("Other");
}
```

### Answer

❌ Throws NullPointerException.

### Explanation

Pattern switch does not automatically handle null.

Need:

```java
case null
```

---

# Question 17 — Pattern Matching Switch

```java
Object obj = "Java";

switch(obj) {
    case String s ->
        System.out.println(s.length());

    default ->
        System.out.println("Other");
}
```

### Answer

✅ Output

```text
4
```

---

# Question 18 — Guarded Pattern

```java
Object obj = "Java";

switch(obj) {
    case String s when s.length() > 3 ->
        System.out.println("Long");

    case String s ->
        System.out.println("Short");

    default ->
        System.out.println("Other");
}
```

### Answer

✅ Output

```text
Long
```

---

# Question 19 — Dominance Rule

```java
Object obj = "Java";

switch(obj) {
    case String s ->
        System.out.println("String");

    case String s when s.length() > 3 ->
        System.out.println("Long");

    default ->
        System.out.println("Other");
}
```

### Answer

❌ Does not compile.

### Explanation

First pattern already matches every String.

Second pattern can never be reached.

Oracle loves dominance questions.

---

# Question 20 — Another Dominance Trap

```java
Object obj = "Java";

switch(obj) {
    case Object o ->
        System.out.println("Object");

    case String s ->
        System.out.println("String");
}
```

### Answer

❌ Does not compile.

### Explanation

Everything is an Object.

`String` case can never be reached.

---

# Question 21 — Continue

```java
for(int i=0;i<5;i++) {

    if(i==2)
        continue;

    System.out.print(i);
}
```

### Answer

✅ Output

```text
0134
```

---

# Question 22 — Break

```java
for(int i=0;i<5;i++) {

    if(i==2)
        break;

    System.out.print(i);
}
```

### Answer

✅ Output

```text
01
```

---

# Question 23 — Labelled Break

```java
outer:
for(int i=0;i<3;i++) {
    for(int j=0;j<3;j++) {

        if(i==1 && j==1)
            break outer;

        System.out.print("*");
    }
}
```

### Answer

✅ Output

```text
****
```

### Explanation

Stars printed:

```text
i=0,j=0 *
i=0,j=1 *
i=0,j=2 *
i=1,j=0 *
```

Then:

```java
break outer;
```

terminates both loops.

---

# Question 24 — Labelled Continue

```java
outer:
for(int i=0;i<3;i++) {

    for(int j=0;j<3;j++) {

        if(j==1)
            continue outer;

        System.out.print(i);
    }
}
```

### Answer

✅ Output

```text
000
```

### Explanation

For each outer iteration:

```java
j=0 -> print i
j=1 -> continue outer
```

Inner loop is abandoned.

---

# Question 25 — Infinite Loop

```java
for(;;) {
    System.out.print("A");
}
```

### Answer

✅ Compiles.

✅ Infinite loop.

Oracle occasionally asks this.

---

# Question 26 — Unreachable Code

```java
while(false) {
    System.out.println("A");
}
```

### Answer

❌ Does not compile.

### Explanation

Compiler knows body is unreachable.

---

# Question 27 — Special Exception

```java
if(false) {
    System.out.println("A");
}
```

### Answer

✅ Compiles.

### Explanation

Many candidates answer incorrectly.

Java allows this.

---

# Question 28 — Code After Return

```java
void test() {
    return;
    System.out.println("A");
}
```

### Answer

❌ Does not compile.

Unreachable statement.

---

# Question 29 — do-while Semicolon

```java
int x = 0;

do {
    x++;
}
while(x < 3);

System.out.println(x);
```

### Answer

✅ Output

```text
3
```

---

# Question 30 — Missing do-while Semicolon

```java
int x = 0;

do {
    x++;
}
while(x < 3)

System.out.println(x);
```

### Answer

❌ Does not compile.

### Explanation

Required:

```java
while(condition);
                ^
```

The semicolon is mandatory.

---

# Question 31 — Oracle Favorite Combination

```java
Object obj = null;

String result =
    switch(obj) {
        case null -> "NULL";
        case String s when s.length() > 3 -> "LONG";
        case String s -> "SHORT";
        default -> "OTHER";
    };

System.out.println(result);
```

### Answer

✅ Output

```text
NULL
```

---

# Question 32 — Hard Oracle Style

```java
Object obj = "Java";

String result =
    switch(obj) {

        case String s when s.length() > 5 ->
            "LONG";

        case String s ->
            "STRING";

        default ->
            "OTHER";
    };

System.out.println(result);
```

### Answer

✅ Output

```text
STRING
```

---

# Score Guide

- **28–32 correct** → Exam-ready for Flow Control.
- **24–27 correct** → Strong chance to answer Flow Control questions correctly.
- **20–23 correct** → Review switch expressions, patterns, and labelled loops.
- **Below 20** → Revisit this domain before moving on.

If you can solve Questions **13, 14, 16, 18, 19, 20, 23, 24, 26, 27, and 30** without hesitation, you're close to Oracle-certification-level mastery of Flow Control.
