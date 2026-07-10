# Java SE 21 Developer Professional (1Z0-830)

# Day 7: Exceptions – Complete Exam Guide

Exceptions are a **very important exam topic**. Oracle loves testing:

- Checked vs unchecked exceptions
- Exception hierarchy
- Exception propagation
- Catch block ordering
- Multi-catch syntax
- `throw` vs `throws`
- `finally`
- Try-with-resources
- Compilation errors involving exceptions

The concepts below are exactly the kind of things that appear in certification questions. [\[docs.oracle.com\]](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html), [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Exception.html)

---

# 1. Exception Hierarchy

All exceptions ultimately inherit from `Throwable`.

```java
Object
 └─ Throwable
     ├─ Error
     └─ Exception
          ├─ RuntimeException
          └─ Other checked exceptions
```

Examples:

```java
Throwable
├── Error
│   ├── StackOverflowError
│   └── OutOfMemoryError
│
└── Exception
    ├── IOException
    ├── SQLException
    ├── FileNotFoundException
    └── RuntimeException
         ├── NullPointerException
         ├── ArithmeticException
         ├── IllegalArgumentException
         └── ArrayIndexOutOfBoundsException
```

`Error` usually should not be handled by application code. `Exception` represents conditions an application may reasonably handle. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Exception.html), [\[docs.oracle.com\]](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)

---

# 2. Checked vs Unchecked Exceptions

## Checked Exceptions

Compiler forces you to:

- Catch them
- OR declare them using `throws`

Examples:

```java
IOException
SQLException
FileNotFoundException
ClassNotFoundException
```

```java
import java.io.*;

public class Test {
    public static void main(String[] args)
            throws FileNotFoundException {

        FileInputStream fis =
                new FileInputStream("a.txt");
    }
}
```

Valid because exception is declared.

---

## Unchecked Exceptions

Subclasses of `RuntimeException`.

Compiler does NOT force handling.

Examples:

```java
NullPointerException
ArithmeticException
IllegalArgumentException
ArrayIndexOutOfBoundsException
```

```java
public class Test {
    public static void main(String[] args) {
        int x = 10 / 0;
    }
}
```

Compiles successfully but fails at runtime with:

```text
ArithmeticException
```

Runtime exceptions and Errors are unchecked. [\[docs.oracle.com\]](https://docs.oracle.com/javase/tutorial/essential/exceptions/runtime.html), [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Exception.html)

---

# Exam Trick

Which are checked?

```java
IOException
SQLException
Exception
```

Which are unchecked?

```java
RuntimeException
NullPointerException
ArithmeticException
Error
```

Remember:

```text
RuntimeException -> Unchecked
Everything else under Exception -> Checked
```

(except RuntimeException subclasses)

---

# 3. try-catch-finally

## Basic Syntax

```java
try {
    // risky code
}
catch(Exception e) {
    // handle
}
finally {
    // always executes
}
```

---

## Example

```java
public class Test {
    public static void main(String[] args) {

        try {
            System.out.println("A");
            int x = 10 / 0;
        }
        catch (ArithmeticException e) {
            System.out.println("B");
        }
        finally {
            System.out.println("C");
        }
    }
}
```

Output:

```text
A
B
C
```

---

# finally Block

The `finally` block runs whether an exception occurs or not.

```java
try {
    System.out.println("try");
}
finally {
    System.out.println("finally");
}
```

Output:

```text
try
finally
```

---

# Exam Rule

A `finally` block executes even when:

```java
return;
```

is encountered.

Example:

```java
public static int test() {
    try {
        return 10;
    } finally {
        System.out.println("finally");
    }
}
```

Output:

```text
finally
```

Returned value:

```text
10
```

---

# 4. Catch Block Ordering

This is heavily tested.

You must catch:

```text
Child first
Parent later
```

---

## Correct

```java
try {
}
catch(FileNotFoundException e) {
}
catch(IOException e) {
}
```

because:

```java
FileNotFoundException extends IOException
```

---

## Incorrect

```java
try {
}
catch(IOException e) {
}
catch(FileNotFoundException e) {
}
```

Compiler error:

```text
Unreachable catch block
```

because first catch already handles the subclass.

---

# 5. Multi-Catch

Java allows multiple exception types in one catch.

```java
catch (IOException | SQLException e)
```

Example:

```java
try {
}
catch (IOException | SQLException e) {
    System.out.println("handled");
}
```

---

## Exam Rule

The exception variable is effectively final.

This is illegal:

```java
catch(IOException | SQLException e) {
    e = new IOException();
}
```

Compiler error.

---

## Another Rule

Exceptions in multi-catch cannot be in parent-child relationship.

Illegal:

```java
catch(IOException | FileNotFoundException e)
```

because:

```java
FileNotFoundException
    extends IOException
```

Compiler error.

---

# 6. throw vs throws

Oracle asks this a lot.

## throw

Used to create and throw an exception object.

```java
throw new RuntimeException();
```

Example:

```java
public static void test() {
    throw new RuntimeException();
}
```

---

## throws

Used in method declaration.

```java
void test() throws IOException
```

Example:

```java
public static void test() throws IOException {
}
```

---

## Easy Memory Trick

### throw

Inside method body.

```java
throw new IOException();
```

### throws

Inside method signature.

```java
void read() throws IOException
```

---

# Common Exam Question

```java
void test() {
    throw new IOException();
}
```

Result?

Compilation error.

Because checked exception must be:

```java
caught
or
declared
```

Correct:

```java
void test() throws IOException {
    throw new IOException();
}
```

---

# 7. Try-With-Resources

Introduced to automatically close resources. Resources implementing `AutoCloseable` can be declared in a try-with-resources statement and are automatically closed. [\[docs.oracle.com\]](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html), [\[baeldung.com\]](https://www.baeldung.com/java-try-with-resources)

---

## Syntax

```java
try(Resource r = new Resource()) {

}
```

Example:

```java
try (BufferedReader br =
        new BufferedReader(
            new FileReader("a.txt"))) {

    System.out.println(br.readLine());
}
```

No explicit close needed.

---

# AutoCloseable

Resource must implement:

```java
AutoCloseable
```

Example:

```java
class MyResource implements AutoCloseable {

    @Override
    public void close() {
        System.out.println("closed");
    }
}
```

---

# Multiple Resources

```java
try(
    A a = new A();
    B b = new B()
) {
}
```

Valid.

---

# Closing Order

Resources close in reverse order.

```java
try(
    A a = new A();
    B b = new B()
) {
}
```

Closing sequence:

```text
B
A
```

Resources acquired first are closed last. [\[baeldung.com\]](https://www.baeldung.com/java-try-with-resources), [\[docs.oracle.com\]](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

---

# 8. Exception Flow Order

This topic appears frequently.

Given:

```java
try {
    statement1;
}
catch(Exception e) {
    statement2;
}
finally {
    statement3;
}
statement4;
```

---

## No Exception

Execution:

```text
statement1
statement3
statement4
```

---

## Exception Occurs

Execution:

```text
statement1
statement2
statement3
statement4
```

---

## Exception Not Caught

```java
try {
    throw new RuntimeException();
}
finally {
    System.out.println("finally");
}
```

Output:

```text
finally
```

Then exception propagates.

---

# Exception Propagation

Exception moves up the call stack until found.

```java
static void c() {
    throw new RuntimeException();
}

static void b() {
    c();
}

static void a() {
    b();
}
```

If nobody catches it:

```text
Program terminates
```

---

# Most Important Certification Facts

✅ Checked exceptions must be caught or declared.

✅ RuntimeException and Error are unchecked.

✅ Child catch before parent catch.

✅ `throw` throws an exception object.

✅ `throws` declares possible exceptions.

✅ `finally` executes even with return.

✅ Try-with-resources automatically closes resources.

✅ Resources close in reverse order.

✅ Multi-catch uses `|`.

✅ Multi-catch cannot contain parent-child exception types.

---

# Practice Questions

## Question 1

What is the output?

```java
try {
    System.out.print("A");
}
finally {
    System.out.print("B");
}
```

### Answer

```text
AB
```

`finally` always executes.

---

## Question 2

Which is unchecked?

A. IOException

B. SQLException

C. RuntimeException

D. FileNotFoundException

### Answer

```text
C. RuntimeException
```

---

## Question 3

What happens?

```java
public static void test() {
    throw new IOException();
}
```

### Answer

Compilation error.

Checked exception not caught or declared.

---

## Question 4

What is the output?

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.print("A");
}
finally {
    System.out.print("B");
}
```

### Answer

```text
AB
```

---

## Question 5

Does this compile?

```java
catch(IOException e) {
}
catch(FileNotFoundException e) {
}
```

### Answer

No.

`FileNotFoundException` is already covered by `IOException`.

---

## Question 6

What is the output?

```java
try {
    return;
}
finally {
    System.out.println("F");
}
```

### Answer

```text
F
```

`finally` still executes.

---

## Question 7

Which keyword declares exceptions?

A. throw

B. throws

C. catch

D. final

### Answer

```text
B. throws
```

---

## Question 8

Which keyword actually throws an exception?

A. throw

B. throws

### Answer

```text
A. throw
```

---

## Question 9

Will this compile?

```java
catch(IOException | Exception e) {
}
```

### Answer

No.

Parent-child relationship in multi-catch.

---

## Question 10

What is the output?

```java
class A implements AutoCloseable {
    public void close() {
        System.out.print("A");
    }
}

class B implements AutoCloseable {
    public void close() {
        System.out.print("B");
    }
}

public class Test {
    public static void main(String[] args) {

        try(
            A a = new A();
            B b = new B()
        ) {
            System.out.print("T");
        }
    }
}
```

### Answer

```text
TBA
```

Resources close in reverse order.

---

# Exam-Level Challenge

What is the output?

```java
public class Test {

    static void test() {
        try {
            System.out.print("A");
            throw new RuntimeException();
        }
        catch(RuntimeException e) {
            System.out.print("B");
        }
        finally {
            System.out.print("C");
        }

        System.out.print("D");
    }

    public static void main(String[] args) {
        test();
    }
}
```

### Answer

```text
ABCD
```

Flow:

1. A
2. Exception thrown
3. B
4. C
5. D

---

If you're following a day-by-day study plan for 1Z0-830, Exceptions are one of the sections where Oracle likes to hide **compilation errors and output questions**, so practice identifying:

- compile-time error vs runtime error,
- catch ordering,
- checked vs unchecked,
- try-with-resources closing order,

without running the code. That's typically how the exam tests this chapter.
