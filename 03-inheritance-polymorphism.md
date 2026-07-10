## 🧱 3. Inheritance & Polymorphism

- [ ] Method overriding rules
  - Same signature
  - Cannot reduce visibility
- [ ] `@Override` behavior
- [ ] Polymorphic calls (parent reference, child object)
- [ ] Casting (upcasting vs downcasting)
- [ ] `instanceof` (pattern matching too ✅)
- [ ] Constructors in inheritance (`super()` rules)
- [ ] `final` class / method / variable

---

Perfect. For **Oracle Java SE 21 Developer Professional (1Z0-830)**, you should focus on **exam-relevant behavior**, not academic theory. Oracle questions are designed to test:

- Compilation vs Runtime behavior
- What prints?
- What compiles?
- Which line causes an error?
- Inheritance hierarchies
- Reference type vs Object type
- Constructor execution order

I'll give you exactly what you need for today's topics and then realistic exam-style questions.

---

# 1. Method Overriding

## Definition

Overriding happens when a subclass provides a new implementation of a method inherited from a parent.

```java
class Animal {
    void speak() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("Dog");
    }
}
```

---

## Overriding Rules (Exam Critical)

### Rule 1: Same Signature

Must have:

- Same method name
- Same parameter list

✅ Valid

```java
class A {
    void test(int x) {}
}

class B extends A {
    void test(int x) {}
}
```

❌ Not overriding (overloading instead)

```java
class B extends A {
    void test(double x) {}
}
```

---

### Rule 2: Cannot Reduce Visibility

Parent:

```java
protected void run() {}
```

Child:

✅

```java
public void run() {}
```

✅

```java
protected void run() {}
```

❌

```java
private void run() {}
```

Compilation error.

Visibility can stay same or become more accessible.

---

### Rule 3: Return Type

Must be:

- Same type
- OR covariant (subtype)

Example:

```java
class Animal {}

class Dog extends Animal {}

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

✅ Legal

---

### Rule 4: Checked Exceptions

Child cannot throw broader checked exceptions.

```java
class Parent{
    void work() throws IOException {}
}

class Child extends Parent{
    void work() throws Exception {}
}
```

❌ Compilation error

Because `Exception` is broader than `IOException`.

---

### Rule 5: Static Methods

Static methods are hidden, not overridden.

```java
class A{
    static void show(){
        System.out.println("A");
    }
}

class B extends A{
    static void show(){
        System.out.println("B");
    }
}
```

Oracle loves this topic.

---

# 2. @Override Annotation

Used to let compiler verify actual overriding.

```java
@Override
void speak() {}
```

If no method exists in parent:

```java
@Override
void talk() {}
```

Compilation error.

---

## Why Oracle Tests It

Detects accidental overloads.

```java
class Parent{
    void test(int x){}
}

class Child extends Parent{
    @Override
    void test(double x){}
}
```

Compiler error because method isn't overriding.

---

# 3. Polymorphism

One of the MOST IMPORTANT EXAM TOPICS.

---

## Parent Reference + Child Object

```java
Animal a = new Dog();
```

Reference type:

```java
Animal
```

Actual object:

```java
Dog
```

---

## Method Calls

```java
class Animal{
    void speak(){
        System.out.println("Animal");
    }
}

class Dog extends Animal{
    void speak(){
        System.out.println("Dog");
    }
}

Animal a = new Dog();
a.speak();
```

Output:

```text
Dog
```

Runtime uses object type.

---

## Variable Access

Variables are NOT polymorphic.

```java
class Animal{
    String name="Animal";
}

class Dog extends Animal{
    String name="Dog";
}

Animal a = new Dog();

System.out.println(a.name);
```

Output:

```text
Animal
```

Uses reference type.

Exam favorite.

---

# 4. Upcasting and Downcasting

---

## Upcasting

Child → Parent

```java
Dog d = new Dog();
Animal a = d;
```

or

```java
Animal a = new Dog();
```

Implicit.

Always safe.

---

## Downcasting

Parent → Child

```java
Animal a = new Dog();

Dog d = (Dog)a;
```

Requires explicit cast.

---

## Runtime Danger

```java
Animal a = new Animal();

Dog d = (Dog)a;
```

Compiles.

Throws

```text
ClassCastException
```

at runtime.

Oracle LOVES this distinction.

---

## Compilation Error Example

```java
String s = "ABC";

Integer i = (Integer)s;
```

Compiler knows impossible.

Compilation error.

---

# 5. instanceof

Checks actual runtime object type.

```java
Animal a = new Dog();

System.out.println(a instanceof Dog);
```

Output

```java
true
```

---

## Null Case

```java
Animal a = null;

System.out.println(a instanceof Animal);
```

Output

```java
false
```

No exception.

Exam favorite.

---

# 6. Pattern Matching for instanceof

Java 21 feature.

---

Old style:

```java
if(obj instanceof String){
    String s = (String)obj;
    System.out.println(s.length());
}
```

---

Pattern Matching:

```java
if(obj instanceof String s){
    System.out.println(s.length());
}
```

Compiler creates variable automatically.

---

Example

```java
Object obj = "Java";

if(obj instanceof String s){
    System.out.println(s.toUpperCase());
}
```

Output:

```text
JAVA
```

---

Scope Rule

```java
if(obj instanceof String s){
    System.out.println(s);
}

System.out.println(s);
```

Compilation error.

`s` only exists where pattern matches.

---

# 7. Constructors & Inheritance

Huge exam topic.

---

## Constructor Execution Order

```java
class Parent{
    Parent(){
        System.out.println("Parent");
    }
}

class Child extends Parent{
    Child(){
        System.out.println("Child");
    }
}
```

```java
new Child();
```

Output

```text
Parent
Child
```

Parent constructor always runs first.

---

## Implicit super()

Compiler inserts:

```java
super();
```

as first statement.

```java
class Child extends Parent{
    Child(){
        super();
    }
}
```

Equivalent.

---

## Rule: super() Must Be First

❌

```java
Child(){
    System.out.println("X");
    super();
}
```

Compilation error.

---

## Calling Specific Parent Constructor

```java
class Parent{
    Parent(String s){}
}

class Child extends Parent{
    Child(){
        super("Java");
    }
}
```

---

## When Parent Has No No-Arg Constructor

```java
class Parent{
    Parent(String s){}
}

class Child extends Parent{
    Child(){}
}
```

Compiler inserts:

```java
super();
```

but doesn't exist.

Compilation error.

EXTREMELY common Oracle question.

---

# 8. final Class

Cannot be extended.

```java
final class Animal{}
```

❌

```java
class Dog extends Animal{}
```

Compilation error.

---

Examples from JDK

```java
String
```

is final.

---

# 9. final Method

Cannot be overridden.

```java
class Parent{
    final void run(){}
}

class Child extends Parent{
    void run(){}
}
```

Compilation error.

---

# 10. final Variable

Assigned once.

---

## Local Variable

```java
final int x = 5;

x = 8;
```

Compilation error.

---

## Blank Final

```java
final int x;

x = 5;
```

Allowed.

Must be assigned exactly once.

---

## Final Reference

```java
final StringBuilder sb =
        new StringBuilder("A");

sb.append("B");
```

Allowed.

Object changes.

Only reference cannot change.

❌

```java
sb = new StringBuilder();
```

Compilation error.

---

# Oracle Exam Traps Summary

### Dynamic Method Dispatch

Methods → object type

```java
Animal a = new Dog();
a.speak();
```

Calls Dog version.

---

### Variables

Fields → reference type.

```java
a.name
```

Uses Animal field.

---

### Casts

```java
Animal a = new Dog();
Dog d = (Dog)a;
```

Safe.

```java
Animal a = new Animal();
Dog d = (Dog)a;
```

Runtime exception.

---

### Constructors

Always parent first.

```java
super();
```

must be first statement.

---

### final

| Feature        | Effect         |
| -------------- | -------------- |
| final class    | No inheritance |
| final method   | No overriding  |
| final variable | Assign once    |

---

# Realistic 1Z0-830 Practice Questions

## Question 1

```java
class A {
    void print() {
        System.out.println("A");
    }
}

class B extends A {
    void print() {
        System.out.println("B");
    }
}

public class Test {
    public static void main(String[] args) {
        A a = new B();
        a.print();
    }
}
```

### Answer

✅ B

Runtime dispatch uses object type.

---

## Question 2

```java
class A {
    String value = "A";
}

class B extends A {
    String value = "B";
}

public class Test {
    public static void main(String[] args) {
        A a = new B();
        System.out.println(a.value);
    }
}
```

### Answer

✅ A

Fields use reference type.

---

## Question 3

```java
class Parent {}

class Child extends Parent {}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();

        if(p instanceof Child c){
            System.out.println("YES");
        }
    }
}
```

### Answer

✅ YES

---

## Question 4

```java
class Animal {}

class Dog extends Animal {}

public class Test {
    public static void main(String[] args) {
        Animal a = new Animal();
        Dog d = (Dog)a;
    }
}
```

Choose:

A. Prints nothing

B. Compilation error

C. Runtime exception

D. Prints Dog

### Answer

✅ C

ClassCastException

---

## Question 5

```java
class Parent{
    Parent(String s){}
}

class Child extends Parent{
    Child(){}
}
```

Choose:

A. Compiles

B. Runtime exception

C. Compilation error

D. Prints null

### Answer

✅ C

Compiler inserts `super()` but parent has no no-arg constructor.

---

## Question 6

```java
class Parent{
    final void run(){}
}

class Child extends Parent{
    void run(){}
}
```

Choose:

A. Compiles

B. Runtime exception

C. Compilation error

D. Prints run

### Answer

✅ C

Cannot override final method.

---

## Question 7

```java
Object obj = "Java";

if(obj instanceof String s){
    System.out.println(s.length());
}
```

Output?

A. Java

B. 4

C. true

D. Compilation error

### Answer

✅ B

---

## Question 8

```java
class A{
    static void show(){
        System.out.println("A");
    }
}

class B extends A{
    static void show(){
        System.out.println("B");
    }
}

public class Test{
    public static void main(String[] args){
        A a = new B();
        a.show();
    }
}
```

Output?

### Answer

✅ A

Static methods are hidden, not polymorphic.

---

If you master everything above and can consistently answer these questions correctly, you'll have covered essentially all inheritance and polymorphism concepts commonly tested in the 1Z0-830 exam.
