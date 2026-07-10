Absolutely. For a second mock exam, I'd focus less on basics and more on the kinds of questions Oracle likes when they want to separate a **70% candidate from a 90% candidate**.

This set contains:

- Pattern matching traps
- Dominance traps
- Exhaustiveness traps
- Fall-through traps
- Labelled loop traps
- Scope traps
- Unreachable code traps
- `yield` traps
- `null` traps
- Questions that require compiler-thinking rather than runtime-thinking

---

# Flow Control Mock Exam #2 (Advanced)

---

# Question 1

What happens?

```java
int x = 1;

String result =
    switch(x) {
        case 1:
            yield "A";
        default:
            yield "B";
    };

System.out.println(result);
```

### Answer

✅ Output

```text
A
```

### Explanation

A switch expression may use the traditional `:` syntax.

When using `:` inside a switch expression, values must be produced with `yield`.

---

# Question 2

What happens?

```java
int x = 1;

String result =
    switch(x) {
        case 1:
            break;
        default:
            yield "B";
    };
```

### Answer

❌ Does not compile.

### Explanation

Inside a switch expression:

```java
yield
```

produces a value.

```java
break;
```

does not.

Very common Oracle trap.

---

# Question 3

Compile or not?

```java
boolean b = false;

while(b) {
    System.out.println("A");
}
```

### Answer

✅ Compiles.

### Explanation

`b` is a variable.

The compiler does not treat it as `while(false)`.

---

# Question 4

Compile or not?

```java
final boolean b = false;

while(b) {
    System.out.println("A");
}
```

### Answer

❌ Does not compile.

### Explanation

The compiler sees:

```java
while(false)
```

Body is unreachable.

---

# Question 5

Output?

```java
for(int i = 0; i < 3; i++) {
    System.out.print(i);
    i++;
}
```

### Answer

✅ Output

```text
02
```

### Explanation

Iteration 1:

```java
i=0
print 0
i++ -> 1
for update -> 2
```

Iteration 2:

```java
i=2
print 2
i++ -> 3
for update -> 4
```

Loop ends.

---

# Question 6

Output?

```java
int x = 0;

do {
    System.out.print(x++);
} while(false);
```

### Answer

✅ Output

```text
0
```

### Explanation

`do-while` always executes at least once.

---

# Question 7

Output?

```java
int x = 2;

switch(x) {
    case 1:
        System.out.print("A");
    default:
        System.out.print("B");
    case 2:
        System.out.print("C");
}
```

### Answer

✅ Output

```text
C
```

### Explanation

Execution starts directly at:

```java
case 2
```

No fall-through occurs before it.

---

# Question 8

Output?

```java
int x = 1;

switch(x) {
    default:
        System.out.print("A");
    case 1:
        System.out.print("B");
    case 2:
        System.out.print("C");
}
```

### Answer

✅ Output

```text
BC
```

### Explanation

Matching starts at `case 1`.

`default` is not automatically executed.

Another favorite Oracle trick.

---

# Question 9

Compile or not?

```java
Object obj = "Java";

switch(obj) {
    case CharSequence cs ->
        System.out.println("CS");

    case String s ->
        System.out.println("STRING");

    default ->
        System.out.println("OTHER");
}
```

### Answer

❌ Does not compile.

### Explanation

Every String is a CharSequence.

Therefore:

```java
case String s
```

is dominated.

---

# Question 10

Compile or not?

```java
Object obj = "Java";

switch(obj) {
    case String s ->
        System.out.println("STRING");

    case CharSequence cs ->
        System.out.println("CS");

    default ->
        System.out.println("OTHER");
}
```

### Answer

✅ Compiles.

---

# Question 11

Output?

```java
Object obj = "";

String result =
    switch(obj) {

        case String s when s.isEmpty() ->
            "EMPTY";

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
EMPTY
```

---

# Question 12

Compile or not?

```java
Object obj = "";

switch(obj) {

    case String s ->
        System.out.println("STRING");

    case String s when s.isEmpty() ->
        System.out.println("EMPTY");

    default ->
        System.out.println("OTHER");
}
```

### Answer

❌ Does not compile.

### Explanation

First pattern already handles every String.

Guarded pattern is unreachable.

---

# Question 13

Compile or not?

```java
String result =
    switch("A") {
        case "A" -> "X";
    };
```

### Answer

✅ Compiles.

### Explanation

Compiler knows only `"A"` can occur.

Exhaustive.

Exam-level trap.

---

# Question 14

Compile or not?

```java
String x = "A";

String result =
    switch(x) {
        case "A" -> "X";
    };
```

### Answer

❌ Does not compile.

### Explanation

Variable may contain many String values.

Not exhaustive.

---

# Question 15

Output?

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
012
```

### Explanation

For each outer iteration:

```java
j=0 -> print i
j=1 -> continue outer
```

Only one print per outer cycle.

---

# Question 16

Output?

```java
outer:
for(int i=0;i<3;i++) {

    for(int j=0;j<3;j++) {

        if(j==1)
            break outer;

        System.out.print(i);
    }
}
```

### Answer

✅ Output

```text
0
```

---

# Question 17

Compile or not?

```java
for(int i = 0; i < 10; i++)
    break;
```

### Answer

✅ Compiles.

Although pointless, `break` is inside a loop.

---

# Question 18

Compile or not?

```java
break;
```

### Answer

❌ Does not compile.

Must be inside:

- loop
- switch

---

# Question 19

Compile or not?

```java
continue;
```

### Answer

❌ Does not compile.

Must be inside a loop.

---

# Question 20

Compile or not?

```java
while(true) {
}
System.out.println("A");
```

### Answer

❌ Does not compile.

### Explanation

Compiler knows loop never exits.

Unreachable statement.

---

# Question 21

Compile or not?

```java
while(true) {
    break;
}

System.out.println("A");
```

### Answer

✅ Compiles.

Compiler can see a path out.

---

# Question 22

Output?

```java
for(int i=1;i<=3;i++) {

    if(i==2)
        continue;

    System.out.print(i);
}
```

### Answer

✅ Output

```text
13
```

---

# Question 23

Output?

```java
int x = 0;

while(x++ < 3) {
}

System.out.println(x);
```

### Answer

✅ Output

```text
4
```

### Explanation

Comparisons:

```java
0 < 3
1 < 3
2 < 3
3 < 3
```

The final failed comparison still increments.

---

# Question 24

Output?

```java
for(int i=0,j=0; i<3; i++,j++) {
    System.out.print(i + "" + j + " ");
}
```

### Answer

✅ Output

```text
00 11 22
```

---

# Question 25

Compile or not?

```java
int x = 10;

switch(x) {
    case 5 + 5:
        System.out.println("A");
}
```

### Answer

✅ Compiles.

Case labels may be compile-time constants.

---

# Question 26

Compile or not?

```java
int y = 10;

switch(y) {
    case y:
        System.out.println("A");
}
```

### Answer

❌ Does not compile.

Case labels must be compile-time constants.

---

# Question 27

Output?

```java
Object obj = null;

String result =
    switch(obj) {
        case null -> "N";
        default -> "D";
    };

System.out.println(result);
```

### Answer

✅ Output

```text
N
```

---

# Question 28

Compile or not?

```java
Object obj = "Java";

switch(obj) {

    case null ->
        System.out.println("NULL");

    case Object o ->
        System.out.println("OBJECT");

    case String s ->
        System.out.println("STRING");
}
```

### Answer

❌ Does not compile.

### Explanation

`Object` dominates `String`.

---

# Question 29

Output?

```java
int x = 0;

if(x++ == 0)
    if(x++ == 1)
        System.out.print("A");
    else
        System.out.print("B");
```

### Answer

✅ Output

```text
A
```

### Explanation

A dangerous dangling-else question.

---

# Question 30 (Very Oracle-Like)

Compile or not?

```java
String result =
    switch(1) {

        case 1 -> {
            System.out.println("A");
            yield "X";
        }
    };
```

### Answer

✅ Compiles.

✅ Prints

```text
A
```

✅ `result` becomes

```text
X
```

---

# High-Value Exam Questions

If I were betting what Oracle is most likely to test, I'd focus heavily on:

1. Questions 9–14 (pattern matching + dominance + exhaustive switches)
2. Questions 20–21 (unreachable code)
3. Questions 25–26 (compile-time constants)
4. Questions 7–8 (fall-through)
5. Questions 15–16 (labelled break/continue)
6. Questions 2 and 30 (`yield`)
7. Questions 13–14 (the exact trap we discussed regarding `switch(1)` vs `switch(variable)`)

If you can answer all 30 of these quickly and correctly, Flow Control is unlikely to be the section that prevents you from passing 1Z0-830.
