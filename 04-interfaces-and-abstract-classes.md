# Day 4 – Interfaces & Abstract Classes (1Z0-830 Exam Focus)

## 📦 4. Interfaces & Abstract Classes

- [ ] Interface default methods
- [ ] Static methods in interfaces
- [ ] Functional interfaces
- [ ] Abstract class rules
- [ ] Multiple inheritance via interfaces

---

For the **Java SE 21 Developer Professional (1Z0-830)** exam, Oracle heavily tests:

- Compile-time vs runtime behavior
- What compiles and what doesn't
- Interface inheritance rules
- Default/static/private methods in interfaces
- Functional interfaces and lambda compatibility
- Abstract class restrictions
- Multiple inheritance conflicts

The exam is much more about **reading code and identifying outcomes/errors** than writing large programs. [\[education.oracle.com\]](https://education.oracle.com/java-se-21-developer-professional/pexam_1Z0-830), [\[enthuware.com\]](https://enthuware.com/oca-ocp-java-certification-resources/290-ocp-java-21-exam-syllabus)

---

# 1. Interface Default Methods

Introduced in Java 8.

A default method:

```java
interface Animal {
    default void eat() {
        System.out.println("Eating");
    }
}
```

Allows interfaces to provide an implementation.

---

## Important Exam Rules

### Rule 1: Default methods MUST have a body

✅ Valid

```java
default void run() {
    System.out.println("Running");
}
```

❌ Invalid

```java
default void run();
```

Compilation error.

---

### Rule 2: Default methods are inherited

```java
interface Animal {
    default void eat() {
        System.out.println("Eat");
    }
}

class Dog implements Animal {
}

public class Test {
    public static void main(String[] args) {
        new Dog().eat();
    }
}
```

Output:

```text
Eat
```

---

### Rule 3: Class implementation wins over default method

```java
interface Animal {
    default void eat() {
        System.out.println("Interface");
    }
}

class Dog implements Animal {

    public void eat() {
        System.out.println("Class");
    }
}
```

Output:

```text
Class
```

**Exam favorite:** Class methods always override interface default methods.

---

### Rule 4: Object methods cannot become default methods

❌ Invalid:

```java
interface Animal {

    default boolean equals(Object o) {
        return true;
    }
}
```

Compilation error.

Interfaces cannot provide default implementations for:

```java
equals()
hashCode()
toString()
```

because they originate from `Object`.

---

# 2. Static Methods in Interfaces

Example:

```java
interface Animal {

    static void info() {
        System.out.println("Animal");
    }
}
```

Call using:

```java
Animal.info();
```

Output:

```text
Animal
```

---

## Exam Rule: NOT inherited

```java
interface Animal {

    static void info() {
        System.out.println("Animal");
    }
}

class Dog implements Animal {
}
```

❌ Invalid

```java
Dog.info();
```

Compilation error.

Must use:

```java
Animal.info();
```

---

## Static methods are hidden, not overridden

```java
interface A {
    static void print() {
        System.out.println("A");
    }
}

interface B extends A {
    static void print() {
        System.out.println("B");
    }
}
```

Perfectly valid.

These are separate methods.

---

# 3. Functional Interfaces

A functional interface contains exactly **one abstract method**.

Examples:

```java
@FunctionalInterface
interface Flyable {
    void fly();
}
```

---

## Valid Functional Interfaces

### Default methods don't count

```java
@FunctionalInterface
interface Test {

    void run();

    default void walk() {}
}
```

Still functional.

---

### Static methods don't count

```java
@FunctionalInterface
interface Test {

    void run();

    static void helper() {}
}
```

Still functional.

---

### Private methods don't count

```java
@FunctionalInterface
interface Test {

    void run();

    private void helper() {}
}
```

Still functional.

---

### Methods inherited from Object don't count

```java
@FunctionalInterface
interface Test {

    void run();

    String toString();
}
```

Still functional.

The `toString()` declaration matches `Object`.

---

## Invalid Functional Interface

```java
@FunctionalInterface
interface Test {

    void run();

    void walk();
}
```

Compilation error.

Two abstract methods.

---

## Lambda Compatibility

```java
@FunctionalInterface
interface Flyable {
    void fly();
}
```

Lambda:

```java
Flyable f = () -> System.out.println("Flying");
```

Valid.

---

## Common Exam Trap

```java
interface A {
    void test();
}

interface B {
    void test();
}

@FunctionalInterface
interface C extends A, B {
}
```

✅ Valid

Why?

Only ONE abstract method exists after inheritance.

---

## Another Trap

```java
interface A {
    void test();
}

interface B {
    int test();
}

interface C extends A, B {
}
```

❌ Invalid

Return types conflict.

---

# 4. Abstract Class Rules

Example:

```java
abstract class Animal {

    abstract void eat();
}
```

Cannot be instantiated.

```java
new Animal();
```

Compilation error.

---

## Abstract Method Rules

### No body

```java
abstract void eat();
```

✅ Valid

---

```java
abstract void eat() {
}
```

❌ Invalid

Abstract methods cannot have a body.

---

## Concrete subclasses must implement

```java
abstract class Animal {
    abstract void eat();
}

class Dog extends Animal {

    void eat() {
        System.out.println("Dog");
    }
}
```

Valid.

---

## Otherwise subclass must also be abstract

```java
abstract class Animal {
    abstract void eat();
}

abstract class Dog extends Animal {
}
```

Valid.

---

# Abstract Classes Can Have

### Constructors

```java
abstract class Animal {

    Animal() {
        System.out.println("Constructor");
    }
}
```

Very common exam topic.

---

### Fields

```java
abstract class Animal {
    int age;
}
```

---

### Static methods

```java
abstract class Animal {

    static void info() {}
}
```

---

### Final methods

```java
abstract class Animal {

    final void sleep() {}
}
```

---

### Concrete methods

```java
abstract class Animal {

    void walk() {
        System.out.println("Walk");
    }
}
```

---

# Abstract + Final

❌ Illegal

```java
final abstract class Animal {
}
```

Why?

- abstract ⇒ must be extended
- final ⇒ cannot be extended

Contradiction.

---

# Abstract Method Cannot Be

### final

```java
abstract final void eat();
```

❌ Invalid

---

### private

```java
private abstract void eat();
```

❌ Invalid

---

### static

```java
abstract static void eat();
```

❌ Invalid

---

# Constructors & Exam Trap

```java
abstract class Animal {

    Animal() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    Dog() {
        System.out.println("Dog");
    }
}
```

```java
new Dog();
```

Output:

```text
Animal
Dog
```

Superclass constructor executes first.

---

# 5. Multiple Inheritance via Interfaces

Java doesn't support:

```java
class C extends A, B
```

❌ Illegal

---

Java DOES support:

```java
interface A {}
interface B {}

class C implements A, B {}
```

✅ Legal

---

# Default Method Conflict (VERY IMPORTANT)

```java
interface A {
    default void test() {
        System.out.println("A");
    }
}

interface B {
    default void test() {
        System.out.println("B");
    }
}

class C implements A, B {
}
```

❌ Compilation error

The compiler doesn't know which implementation to use.

---

## Resolution

Override method

```java
class C implements A, B {

    @Override
    public void test() {
        System.out.println("Resolved");
    }
}
```

✅ Valid

---

## Calling Specific Parent Default Method

Exam favorite:

```java
class C implements A, B {

    @Override
    public void test() {
        A.super.test();
    }
}
```

Output:

```text
A
```

---

# Interface vs Abstract Class (Exam Summary)

| Feature                | Interface           | Abstract Class |
| ---------------------- | ------------------- | -------------- |
| Multiple inheritance   | Yes                 | No             |
| Constructors           | No                  | Yes            |
| Instance fields        | Constants only      | Yes            |
| Default methods        | Yes                 | N/A            |
| Static methods         | Yes                 | Yes            |
| Abstract methods       | Yes                 | Yes            |
| Can be instantiated    | No                  | No             |
| Extends multiple types | Multiple interfaces | One class      |

---

# High-Probability Mock Questions

## Question 1

```java
interface A {
    default void test() {
        System.out.println("A");
    }
}

class B implements A {
    public void test() {
        System.out.println("B");
    }
}

public class Main {
    public static void main(String[] args) {
        A a = new B();
        a.test();
    }
}
```

A. A  
B. B  
C. Compilation error  
D. Runtime exception

✅ Answer: **B**

---

## Question 2

```java
interface A {
    static void test() {
        System.out.println("A");
    }
}

class B implements A { }

public class Main {
    public static void main(String[] args) {
        B.test();
    }
}
```

A. A  
B. Runtime exception  
C. Compilation error  
D. NullPointerException

✅ Answer: **C**

---

## Question 3

```java
@FunctionalInterface
interface Test {

    void run();

    default void walk() {}

    static void help() {}
}
```

A. Compiles  
B. Not functional  
C. Two abstract methods  
D. Runtime exception

✅ Answer: **A**

---

## Question 4

```java
abstract class A {

    abstract void test();
}

class B extends A {
}
```

A. Compiles  
B. Runtime exception  
C. Compilation error  
D. B becomes abstract automatically

✅ Answer: **C**

---

## Question 5

```java
interface A {
    default void test() {
        System.out.println("A");
    }
}

interface B {
    default void test() {
        System.out.println("B");
    }
}

class C implements A, B {

    @Override
    public void test() {
        B.super.test();
    }
}
```

```java
new C().test();
```

A. A  
B. B  
C. Compilation error  
D. Runtime exception

✅ Answer: **B**

---

## Question 6

```java
@FunctionalInterface
interface A {
    void test();
}

interface B {
    void test();
}

@FunctionalInterface
interface C extends A, B {
}
```

A. Compiles  
B. Runtime exception  
C. Not functional  
D. Two abstract methods

✅ Answer: **A**

---

# What Oracle Loves Testing

Memorize these:

✅ Class method beats interface default method  
✅ Static interface methods are NOT inherited  
✅ Functional interface = exactly one abstract method  
✅ Default/static/private methods don't count toward functional interface count  
✅ Abstract classes may have constructors  
✅ Abstract class cannot be final  
✅ Default method conflict must be resolved explicitly  
✅ Use `InterfaceName.super.method()` to call a specific default implementation  
✅ Interfaces support multiple inheritance; classes do not

If you master the above rules, you will answer roughly **90–95% of all Interface & Abstract Class questions** that typically appear on the 1Z0-830 exam. [\[enthuware.com\]](https://enthuware.com/oca-ocp-java-certification-resources/290-ocp-java-21-exam-syllabus), [\[masteryexamprep.com\]](https://masteryexamprep.com/exams/oracle/1z0-830/exam-blueprint/)
