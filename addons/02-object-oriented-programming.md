# Java SE 21 Developer Professional (1Z0-830)

# Object-Oriented Programming – Exam-Focused Deep Dive

The topics you listed are **absolutely exam-worthy**. Oracle loves testing subtle differences between **overloading vs overriding**, **inheritance initialization**, **field hiding**, and **static method hiding** because they generate many compiler/runtime surprises.

---

# 1. Constructor Overloading

## Definition

A class may have multiple constructors with different parameter lists.

```java
class Employee {
    Employee() {
        System.out.println("Default");
    }

    Employee(String name) {
        System.out.println(name);
    }

    Employee(String name, int age) {
        System.out.println(name + " " + age);
    }
}
```

This is valid because parameter lists differ.

---

## Rules

### Rule 1: Constructors are NEVER inherited

```java
class Parent {
    Parent(){}
}

class Child extends Parent {}
```

Child does not inherit Parent constructors.

Oracle likes this.

---

### Rule 2: Constructor name must match class name

```java
class Test {
    Test() {}
}
```

Valid.

---

### Rule 3: No return type

```java
class Test {
    void Test() {}
}
```

This is NOT a constructor.

It's a method.

Exam favorite.

---

### Rule 4: First statement must be super() or this()

```java
class A {
    A() {}
}

class B extends A {
    B() {
        super(); // first statement
    }
}
```

Invalid:

```java
B() {
    System.out.println("Hello");
    super();     // compile error
}
```

---

## Constructor Chaining

### this()

Calls another constructor of same class.

```java
class Person {
    Person() {
        this("Unknown");
    }

    Person(String name) {}
}
```

---

### super()

Calls parent constructor.

```java
class Animal {
    Animal(String type){}
}

class Dog extends Animal {
    Dog() {
        super("Dog");
    }
}
```

---

## Certification Traps

### Trap #1

```java
class A {
    A(int x){}
}

class B extends A {
}
```

Compilation error.

Compiler inserts:

```java
super();
```

But parent doesn't have no-arg constructor.

---

### Trap #2

```java
class A {
    A() {
        this(10);
    }

    A(int x) {}
}
```

Valid.

---

### Trap #3

```java
class A {
    A() {
        super();
    }
}
```

Valid.

All classes ultimately call Object().

---

# Oracle Question

What happens?

```java
class A {
    A(int x){}
}

class B extends A {
    B(){}
}
```

### Answer

Compilation fails.

Reason:

```java
B() {
    super(); // inserted automatically
}
```

No matching constructor exists.

---

# 2. Method Overloading Rules

## Definition

Same method name, different parameter list.

```java
void print(int x){}
void print(String s){}
```

---

## What CAN differ?

### Number of parameters

```java
m(int);
m(int,int);
```

---

### Type of parameters

```java
m(int);
m(long);
```

---

### Parameter order

```java
m(int,String);
m(String,int);
```

---

## What CANNOT differ?

### Return type only

```java
int get(){ return 1; }

String get(){ return "A"; }
```

Compilation error.

---

### Exception list only

```java
void read() throws IOException {}

void read() throws SQLException {}
```

Compilation error.

---

# Overload Resolution Order

Oracle LOVES this.

Java chooses:

### 1 Exact Match

```java
m(10);
```

chooses:

```java
m(int)
```

---

### 2 Primitive Widening

```java
int → long
int → float
int → double
```

Example:

```java
void m(long x){}
m(10);
```

Calls m(long).

---

### 3 Autoboxing

```java
void m(Integer x){}
m(10);
```

int → Integer

---

### 4 Varargs

```java
void m(int... x){}
m(10);
```

Varargs last priority.

---

# Priority Rule

```text
Exact Match
   ↓
Primitive Widening
   ↓
Autoboxing
   ↓
Varargs
```

Memorize this.

Huge exam favorite.

---

## Example

```java
void m(long x) {
    System.out.println("long");
}

void m(Integer x) {
    System.out.println("Integer");
}

m(10);
```

Output:

```text
long
```

Because widening beats boxing.

---

## Ambiguous Overload

```java
void m(Integer i){}
void m(Long l){}

m(null);
```

Compilation error.

null matches both equally.

---

# Oracle Question

Output?

```java
void test(int x){}
void test(Integer x){}
void test(int... x){}

test(5);
```

Answer:

```text
test(int)
```

Exact match wins.

---

# Certification Traps

### Trap 1

```java
test(null);
```

```java
test(String)
test(Integer)
```

Ambiguous.

---

### Trap 2

```java
void test(long x){}
void test(Integer x){}

test(10);
```

Chooses:

```java
test(long)
```

Widening beats boxing.

---

# 3. Covariant Return Types

## Definition

When overriding, return type may be a subtype of original return type.

---

### Parent

```java
class Animal {}

class Dog extends Animal {}
```

---

### Overriding

```java
class Parent {
    Animal get() {
        return new Animal();
    }
}

class Child extends Parent {
    @Override
    Dog get() {
        return new Dog();
    }
}
```

Valid.

---

## Invalid Example

```java
class Parent {
    Animal get() {
        return new Animal();
    }
}

class Child extends Parent {
    Object get() {
        return new Object();
    }
}
```

Compile error.

Object is broader, not narrower.

---

# Certification Trap

Only applies to:

### Non-primitive reference types

Valid:

```java
Animal → Dog
```

Invalid:

```java
int → long
```

Primitive returns must be identical.

---

## Oracle Question

```java
class A {}
class B extends A {}

class Parent {
    A get() { return new A(); }
}

class Child extends Parent {
    B get() { return new B(); }
}
```

Result?

Answer:

Compiles.

Covariant return.

---

# 4. Hiding Static Methods

Very popular.

---

## Overriding vs Hiding

Instance methods → override

Static methods → hide

---

Example:

```java
class Parent {
    static void speak() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    static void speak() {
        System.out.println("Child");
    }
}
```

---

## Which method executes?

```java
Parent p = new Child();
p.speak();
```

Output:

```text
Parent
```

NOT Child.

Because static method resolution uses reference type.

---

# Key Rule

Static methods are determined at compile time.

Instance methods are determined at runtime.

---

# Exam Favorite

```java
class Parent {
    static void m() {
        System.out.println("P");
    }
}

class Child extends Parent {
    static void m() {
        System.out.println("C");
    }
}

Parent p = new Child();

p.m();
```

Output:

```text
P
```

---

## Certification Traps

### Trap 1

People assume polymorphism:

```java
Parent p = new Child();
```

No polymorphism for static methods.

---

### Trap 2

Cannot override static with instance

```java
class Parent {
    static void test(){}
}

class Child extends Parent {
    void test(){}
}
```

Compile error.

---

### Trap 3

Cannot override instance with static

```java
class Parent {
    void test(){}
}

class Child extends Parent {
    static void test(){}
}
```

Compile error.

---

# 5. Hiding Fields

Another classic Oracle trap.

Fields are NEVER polymorphic.

---

Example

```java
class Parent {
    String value = "Parent";
}

class Child extends Parent {
    String value = "Child";
}
```

---

### Access

```java
Parent p = new Child();

System.out.println(p.value);
```

Output:

```text
Parent
```

Field lookup uses reference type.

---

### Method Example

```java
System.out.println(p.getValue());
```

If overridden:

```java
Child
```

Methods use runtime object.

Fields use reference type.

---

# Exam Favorite

```java
class Parent {
    int x = 10;
}

class Child extends Parent {
    int x = 20;
}

Parent p = new Child();

System.out.println(p.x);
```

Output:

```text
10
```

---

# Memory Trick

```text
Fields -> Reference type
Static methods -> Reference type

Instance methods -> Actual object type
```

Memorize this.

---

# 6. Initialization Order Across Inheritance

This is probably the MOST IMPORTANT topic in this list.

Oracle frequently builds huge questions around it.

---

## Complete Initialization Order

Creating:

```java
new Child();
```

Java executes:

### Step 1

Parent static variables

---

### Step 2

Parent static blocks

---

### Step 3

Child static variables

---

### Step 4

Child static blocks

---

### Step 5

Parent instance variables

---

### Step 6

Parent instance initializers

---

### Step 7

Parent constructor

---

### Step 8

Child instance variables

---

### Step 9

Child instance initializers

---

### Step 10

Child constructor

---

# Example

```java
class Parent {

    static {
        System.out.println("Parent static");
    }

    {
        System.out.println("Parent instance");
    }

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    static {
        System.out.println("Child static");
    }

    {
        System.out.println("Child instance");
    }

    Child() {
        System.out.println("Child constructor");
    }
}
```

```java
new Child();
```

Output:

```text
Parent static
Child static
Parent instance
Parent constructor
Child instance
Child constructor
```

---

# Important Rule

### Static initialization occurs once per class load

```java
new Child();
new Child();
```

Statics execute only first time.

---

# Dangerous Exam Trap

```java
class Parent {

    Parent() {
        print();
    }

    void print() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int value = 5;

    @Override
    void print() {
        System.out.println(value);
    }
}
```

```java
new Child();
```

Output:

```text
0
```

NOT 5.

Why?

Parent constructor executes before Child instance fields initialize.

This is one of Oracle's favorite inheritance questions.

---

# High-Probability Exam Questions

## Q1

```java
class Parent {
    static void m() {
        System.out.print("P");
    }
}

class Child extends Parent {
    static void m() {
        System.out.print("C");
    }
}

Parent p = new Child();
p.m();
```

### Answer

```text
P
```

---

## Q2

```java
class A {
    int x = 10;
}

class B extends A {
    int x = 20;
}

A a = new B();

System.out.println(a.x);
```

### Answer

```text
10
```

---

## Q3

```java
void m(long x){}
void m(Integer x){}

m(5);
```

### Answer

Calls:

```java
m(long)
```

Widening beats boxing.

---

## Q4

```java
class Parent {
    Parent(int x){}
}

class Child extends Parent {
    Child(){}
}
```

### Answer

Compilation error.

No implicit `super()` available.

---

## Q5

```java
class A {}
class B extends A {}

class Parent {
    A get(){ return new A(); }
}

class Child extends Parent {
    B get(){ return new B(); }
}
```

### Answer

Compiles successfully.

Covariant return type.

---

# Final Exam Cheat Sheet

### Overloading

✅ Different parameters

❌ Different return type only

Priority:

```text
Exact Match
↓
Widening
↓
Boxing
↓
Varargs
```

---

### Constructors

✅ Not inherited

✅ this() or super() first statement

✅ Compiler inserts super() if omitted

❌ Constructor has no return type

---

### Covariant Returns

✅ Child may return subtype

```java
Animal -> Dog
```

❌ Cannot return broader type

---

### Static Methods

✅ Hidden, not overridden

✅ Use reference type

```java
Parent p = new Child();
p.m(); // Parent
```

---

### Fields

✅ Hidden, not overridden

✅ Use reference type

```java
Parent p = new Child();
p.x; // Parent field
```

---

### Initialization Order

```text
Parent static fields
Parent static blocks
Child static fields
Child static blocks
Parent instance fields
Parent instance blocks
Parent constructor
Child instance fields
Child instance blocks
Child constructor
```

### Ultimate Oracle Trap

If a parent constructor calls an overridden method, the child fields may still have default values (`0`, `false`, `null`) because child initialization has not yet happened. This appears on certification exams very frequently.
