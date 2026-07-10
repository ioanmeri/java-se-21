# Java SE 21 Developer Professional (1Z0-830) — Day 8: Flow Control

## 🔁 8. Flow Control

- [ ] `if`, `switch` (including new switch expressions ✅)
- [ ] Pattern matching in `switch`
- [ ] Loops (`for`, enhanced for, `while`, `do-while`)
- [ ] `break` / `continue`
- [ ] Labelled loops

---

This is one of Oracle's favorite exam areas because it combines:

- Syntax rules
- Scope rules
- Definite assignment
- Type compatibility
- Reachability
- Pattern matching
- Switch expressions

Many questions look simple but are actually testing one tiny language rule.

---

# 1. if Statements

## Basic Syntax

```java
if (condition)
    statement;

if (condition) {
    statement1;
    statement2;
}

if (condition) {
    ...
} else {
    ...
}

if (condition1) {
    ...
} else if (condition2) {
    ...
} else {
    ...
}
```

---

## Condition Must Be boolean

✅ Valid

```java
boolean b = true;

if (b) {
    System.out.println("yes");
}
```

❌ Invalid

```java
int x = 1;

if (x) { }      // DOES NOT COMPILE
```

Unlike C/C++, Java does not convert numbers into booleans.

---

## Common Oracle Trap

```java
boolean b = false;

if (b = true)
    System.out.println("A");
```

Output:

```text
A
```

Explanation:

```java
b = true
```

is an assignment expression.

The expression evaluates to:

```java
true
```

Therefore the block executes.

---

## Dangling Else

Oracle loves this.

```java
if (true)
    if (false)
        System.out.println("A");
    else
        System.out.println("B");
```

Output:

```text
B
```

The `else` attaches to the nearest unmatched `if`.

Equivalent:

```java
if (true) {
    if (false)
        System.out.println("A");
    else
        System.out.println("B");
}
```

---

# 2. Traditional switch Statement

Works with:

```java
byte
short
char
int
Integer
Byte
Short
Character
String
enum
```

Also pattern matching support exists in modern versions.

---

## Example

```java
int x = 2;

switch (x) {
    case 1:
        System.out.println("One");
        break;
    case 2:
        System.out.println("Two");
        break;
    default:
        System.out.println("Other");
}
```

Output:

```text
Two
```

---

# Switch Fall-Through

Without break, execution continues.

```java
int x = 1;

switch (x) {
    case 1:
        System.out.println("A");
    case 2:
        System.out.println("B");
    case 3:
        System.out.println("C");
}
```

Output:

```text
ABC
```

---

## Oracle Trap

```java
final int x = 1;

switch (x) {
    case 1:
    case 1:
}
```

❌ Does not compile

Duplicate case label.

---

# Case Labels Must Be Compile-Time Constants

✅

```java
final int A = 10;

switch (x) {
    case A:
}
```

❌

```java
int A = 10;

switch (x) {
    case A:
}
```

Not a compile-time constant.

---

# String Switch

```java
String s = "Java";

switch (s) {
    case "Java":
        System.out.println("OK");
        break;
}
```

---

## Null Trap

Traditional switch:

```java
String s = null;

switch (s) {
    case "A":
}
```

Runtime:

```text
NullPointerException
```

Exception:

Pattern matching switch can handle null explicitly.

---

# 3. Switch Expressions (VERY IMPORTANT)

Huge exam topic.

Introduced to eliminate fall-through problems.

---

## Arrow Syntax

```java
int day = 1;

String result =
    switch(day) {
        case 1 -> "Mon";
        case 2 -> "Tue";
        default -> "Unknown";
    };
```

Output:

```text
Mon
```

---

## Multiple Labels

```java
String result =
    switch(day) {
        case 1, 2, 3, 4, 5 -> "Weekday";
        case 6, 7 -> "Weekend";
        default -> "Invalid";
    };
```

---

## Yield

Used when block returns a value.

```java
String result =
    switch(day) {
        case 1 -> {
            System.out.println("A");
            yield "Mon";
        }
        default -> "Other";
    };
```

---

## Oracle Trap

```java
String result =
    switch(day) {
        case 1 -> {
            return "Mon";
        }
    };
```

❌ Does not compile

Need:

```java
yield "Mon";
```

inside switch expression blocks.

---

## Exhaustiveness

Switch expressions must produce a value for every possibility.

✅

```java
String result =
    switch(day) {
        case 1 -> "Mon";
        default -> "Other";
    };
```

❌

```java
String result =
    switch(day) {
        case 1 -> "Mon";
    };
```

Not exhaustive.

---

# 4. Pattern Matching for switch (Java 21)

One of the biggest Java 21 exam topics.

---

## Basic Example

```java
Object obj = "Java";

switch (obj) {
    case String s ->
        System.out.println(s.length());

    default ->
        System.out.println("Other");
}
```

Output:

```text
4
```

---

## Pattern Variable Scope

```java
case String s -> System.out.println(s);
```

`s` exists only in that case.

---

## Multiple Types

```java
switch(obj) {
    case Integer i ->
        System.out.println(i);

    case String s ->
        System.out.println(s);

    default ->
        System.out.println("Other");
}
```

---

## Guarded Patterns

Java 21 uses:

```java
when
```

Example:

```java
switch(obj) {
    case String s when s.length() > 3 ->
        System.out.println("Long");

    case String s ->
        System.out.println("Short");

    default ->
        System.out.println("Other");
}
```

---

# Null Handling in Pattern Switch

Very important.

```java
switch(obj) {
    case null ->
        System.out.println("Null");

    case String s ->
        System.out.println(s);

    default ->
        System.out.println("Other");
}
```

This safely handles null.

---

# Dominance Rules (Oracle Favorite)

More specific patterns must come first.

❌

```java
switch(obj) {
    case Object o -> System.out.println("Object");
    case String s -> System.out.println("String");
}
```

Compile error.

Reason:

```java
Object
```

already matches everything.

String case can never be reached.

---

✅ Correct

```java
switch(obj) {
    case String s -> System.out.println("String");
    case Object o -> System.out.println("Object");
}
```

---

# 5. for Loop

## Basic

```java
for(int i=0; i<5; i++) {
    System.out.println(i);
}
```

---

## Anatomy

```java
for(initialization;
    condition;
    update)
```

---

## Infinite Loop

```java
for(;;) {
}
```

Perfectly legal.

Equivalent to:

```java
while(true)
```

---

## Multiple Variables

```java
for(int i=0, j=10; i<j; i++, j--) {
}
```

---

## Oracle Trap

```java
for(long x = 0, int y = 0; ; )
```

❌ Does not compile

Same type required when declaring variables.

---

✅

```java
long x = 0;
int y = 0;

for(; ; ) {
}
```

---

# Variable Scope in for

```java
for(int i=0; i<5; i++) {
}

System.out.println(i);
```

❌ Does not compile

`i` is out of scope.

---

# 6. Enhanced for Loop

## Syntax

```java
for(type variable : collection)
```

---

## Arrays

```java
int[] nums = {1,2,3};

for(int n : nums) {
    System.out.println(n);
}
```

---

## Collections

```java
List<String> list = List.of("A","B");

for(String s : list) {
    System.out.println(s);
}
```

---

## Oracle Trap

```java
int[] nums = {1,2,3};

for(int n : nums) {
    n++;
}
```

Output?

Array unchanged.

Reason:

```java
n
```

is a copy.

---

# 7. while Loop

```java
while(condition) {
}
```

Example:

```java
int x = 0;

while(x < 3) {
    System.out.println(x++);
}
```

Output:

```text
0
1
2
```

---

# 8. do-while Loop

Runs at least once.

```java
do {
    statements;
}
while(condition);
```

---

Example

```java
int x = 10;

do {
    System.out.println(x);
} while(x < 5);
```

Output:

```text
10
```

---

## Semicolon Trap

Required:

```java
do {
}
while(x > 0);
```

Missing semicolon:

```java
while(x > 0)
```

❌ Does not compile.

---

# 9. break

Terminates nearest loop or switch.

```java
for(int i=0;i<10;i++) {
    if(i==5)
        break;
}
```

Ends loop.

---

# break with switch

```java
switch(x) {
    case 1:
        break;
}
```

Stops switch execution.

---

# 10. continue

Skips current iteration.

```java
for(int i=0;i<5;i++) {

    if(i==2)
        continue;

    System.out.print(i);
}
```

Output:

```text
0134
```

---

# Oracle Trap

```java
while(true) {
    continue;
}
```

Legal.

Infinite loop.

---

# 11. Labelled Statements

Extremely common certification topic.

---

## Label Syntax

```java
OUTER:
for(int i=0;i<3;i++) {
}
```

---

# Labelled break

```java
OUTER:
for(int i=0;i<3;i++) {

    for(int j=0;j<3;j++) {

        if(j==1)
            break OUTER;
    }
}
```

Terminates the OUTER loop.

---

# Labelled continue

```java
OUTER:
for(int i=0;i<3;i++) {

    for(int j=0;j<3;j++) {

        if(j==1)
            continue OUTER;

        System.out.println(i + ":" + j);
    }
}
```

Jumps to next OUTER iteration.

Output:

```text
0:0
1:0
2:0
```

---

# Label Rules

Labels are identifiers.

✅

```java
myLabel:
while(true) {
}
```

---

❌ Duplicate label in same scope

```java
x:
while(true) {}

x:
while(true) {}
```

Does not compile.

---

# Reachability (Important)

Oracle loves unreachable code.

---

## Example

```java
while(false) {
    System.out.println("A");
}
```

❌ Compile error

Condition known at compile time.

---

## Example

```java
final boolean b = false;

while(b) {
}
```

❌ Compile error

Unreachable.

---

## Example

```java
boolean b = false;

while(b) {
}
```

✅ Compiles

Compiler cannot know actual runtime value.

---

# Exam Favorites Summary

Memorize these:

### 1. Switch expression uses `yield`, not `return`

```java
case 1 -> {
    yield "A";
}
```

---

### 2. Switch expressions must be exhaustive

---

### 3. Pattern matching handles null

```java
case null ->
```

---

### 4. Dominance rule

Specific types before general types.

---

### 5. Traditional switch falls through

Without `break`.

---

### 6. Enhanced for modifies copy, not original value

---

### 7. `do-while` executes at least once

---

### 8. `continue` skips iteration

`break` exits loop.

---

### 9. Labels work with both `break` and `continue`

---

### 10. `if` requires boolean expression

Not int, not String.

---

# High-Probability Exam Questions

---

## Question 1

What is the output?

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

```text
ABC
```

No break statements → fall-through.

---

## Question 2

What is the output?

```java
boolean b = false;

if (b = true)
    System.out.println("YES");
else
    System.out.println("NO");
```

### Answer

```text
YES
```

Assignment returns assigned value.

---

## Question 3

Compile or not?

```java
String result =
    switch(1) {
        case 1 -> "A";
    };
```

### Answer

❌ Does not compile.

Switch expression must be exhaustive.

---

## Question 4

Output?

```java
Object obj = "Java";

switch(obj) {
    case String s ->
        System.out.println(s.length());

    default ->
        System.out.println("X");
}
```

### Answer

```text
4
```

Pattern matching identifies String.

---

## Question 5

Compile or not?

```java
switch("abc") {
    case "abc":
        break;
    case "abc":
        break;
}
```

### Answer

❌ Does not compile.

Duplicate case labels.

---

## Question 6

Output?

```java
for(int i=0;i<3;i++) {
    if(i==1)
        continue;

    System.out.print(i);
}
```

### Answer

```text
02
```

Iteration 1 skipped.

---

## Question 7

Output?

```java
int x = 3;

do {
    System.out.print(x);
} while(x < 2);
```

### Answer

```text
3
```

do-while runs once regardless.

---

## Question 8

Compile or not?

```java
Object o = "Java";

switch(o) {
    case Object obj -> {}
    case String s -> {}
}
```

### Answer

❌ Does not compile.

`Object` dominates `String`.

---

## Question 9

Output?

```java
OUT:
for(int i=0;i<3;i++) {

    for(int j=0;j<3;j++) {

        if(j==1)
            break OUT;

        System.out.print(i);
    }
}
```

### Answer

```text
0
```

Break exits both loops immediately.

---

## Question 10 (Classic Oracle Trap)

Output?

```java
int[] nums = {1,2,3};

for(int x : nums) {
    x++;
}

System.out.println(nums[0]);
```

### Answer

```text
1
```

Enhanced-for variable is a copy.

---

## Question 11

Compile or not?

```java
for(int i=0, j=0; i<10; i++) {
}
```

### Answer

✅ Compiles.

Same type.

---

## Question 12

Compile or not?

```java
for(int i=0, long j=0; i<10; i++) {
}
```

### Answer

❌ Does not compile.

Cannot declare different types in same initialization section.

---

# Last-Minute 1Z0-830 Checklist

Make sure you can instantly answer:

- ✅ switch statement vs switch expression
- ✅ yield usage
- ✅ fall-through behavior
- ✅ exhaustive switch expressions
- ✅ pattern matching switch
- ✅ guarded patterns (`when`)
- ✅ null in switch
- ✅ dominance rules
- ✅ enhanced-for semantics
- ✅ labeled break/continue
- ✅ unreachable code
- ✅ dangling else
- ✅ assignment inside if conditions
- ✅ loop variable scope
- ✅ do-while semicolon

If you master everything above, you will be very well prepared for virtually every Flow Control question Oracle can throw at you in the Java SE 21 Developer Professional exam.
