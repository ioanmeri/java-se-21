# Java SE 21 Developer Professional (1Z0-830) – Records

Records are a **very important exam topic** because Oracle likes testing:

- What code is generated automatically
- Constructors
- Immutability
- Accessor methods
- What is allowed and what is not allowed in a record
- Canonical vs Compact constructors

---

# 1. What is a Record?

A **record** is a special kind of class introduced to model immutable data.

Instead of writing boilerplate code:

```java
public class Person {
    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String name() {
        return name;
    }

    public int age() {
        return age;
    }
}
```

you can write:

```java
public record Person(String name, int age) {}
```

The compiler automatically generates much of the code.

---

# 2. Record Declaration Syntax

Basic syntax:

```java
record Person(String name, int age) {}
```

General form:

```java
record RecordName(type1 component1,
                  type2 component2,
                  ...) {
}
```

Example:

```java
record Book(String title,
            String author,
            double price) {}
```

---

# 3. What Does the Compiler Generate?

For:

```java
record Person(String name, int age) {}
```

the compiler automatically generates:

### Private Final Fields

```java
private final String name;
private final int age;
```

---

### Accessor Methods

Not getters!

```java
String name()
int age()
```

Usage:

```java
Person p = new Person("John", 30);

System.out.println(p.name());
System.out.println(p.age());
```

Notice:

```java
p.getName(); // DOES NOT EXIST
```

Exam trap!

---

### Canonical Constructor

Generated automatically:

```java
public Person(String name, int age) {
    this.name = name;
    this.age = age;
}
```

---

### equals()

Generated automatically.

```java
Person p1 = new Person("John", 30);
Person p2 = new Person("John", 30);

System.out.println(p1.equals(p2));
```

Output:

```text
true
```

---

### hashCode()

Generated automatically.

---

### toString()

Generated automatically.

```java
Person p = new Person("John", 30);

System.out.println(p);
```

Output:

```text
Person[name=John, age=30]
```

Oracle loves questions about this output.

---

# 4. Record Components

In:

```java
record Person(String name, int age) {}
```

`name` and `age` are called:

```text
record components
```

Each component automatically creates:

- field
- accessor
- constructor parameter

---

# 5. Records Are Final

Every record is implicitly:

```java
final
```

Example:

```java
record Person(String name) {}
```

Equivalent idea:

```java
final class Person
```

Therefore:

```java
record Employee(String name) extends Person {}
```

❌ Compilation error

Records cannot be extended.

---

# 6. Records Can Implement Interfaces

Allowed:

```java
interface Printable {
    void print();
}

record Person(String name) implements Printable {

    @Override
    public void print() {
        System.out.println(name);
    }
}
```

---

# 7. Records Cannot Extend Classes

Not allowed:

```java
record Person(String name) extends Object {}
```

❌ Compilation error

All records already extend:

```java
java.lang.Record
```

Hierarchy:

```text
Object
   |
 Record
   |
 Person
```

---

# 8. Static Members in Records

Allowed:

```java
record Person(String name) {

    static int count = 0;

    static void hello() {
        System.out.println("Hello");
    }
}
```

---

# 9. Instance Fields

This is a very common exam question.

Not allowed:

```java
record Person(String name) {
    private int age;
}
```

❌ Compilation error

Records cannot declare additional instance fields.

---

Allowed:

```java
record Person(String name) {
    private static int counter;
}
```

✅ Compiles

---

# 10. Instance Initializers

Not allowed:

```java
record Person(String name) {

    {
        System.out.println("Hi");
    }
}
```

❌ Compilation error

Records cannot have instance initializers.

---

Static initializers are allowed:

```java
record Person(String name) {

    static {
        System.out.println("Loaded");
    }
}
```

✅ Compiles

---

# 11. Immutability Behavior

Record fields are:

```java
private final
```

which means references cannot be reassigned.

Example:

```java
record Person(String name) {}

Person p = new Person("John");

p.name = "Mike";
```

❌ Compilation error

---

# Shallow Immutability

Records are immutable only at the field-reference level.

Example:

```java
record Team(List<String> members) {}
```

```java
List<String> list = new ArrayList<>();

Team t = new Team(list);

list.add("John");

System.out.println(t.members());
```

Output:

```text
[John]
```

The reference is final, but the object itself can still change.

Very common certification concept.

---

# 12. Custom Constructors

There are two types Oracle tests heavily:

## A. Canonical Constructor

Full constructor with all components.

Example:

```java
record Person(String name, int age) {

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Must initialize every component.

---

Example validation:

```java
record Person(String name, int age) {

    public Person(String name, int age) {

        if(age < 0)
            throw new IllegalArgumentException();

        this.name = name;
        this.age = age;
    }
}
```

✅ Valid

---

# B. Compact Constructor

Introduced to reduce boilerplate.

Syntax:

```java
record Person(String name, int age) {

    public Person {

        if(age < 0)
            throw new IllegalArgumentException();
    }
}
```

The compiler automatically adds:

```java
this.name = name;
this.age = age;
```

at the end.

---

Equivalent to:

```java
public Person(String name, int age) {

    if(age < 0)
        throw new IllegalArgumentException();

    this.name = name;
    this.age = age;
}
```

---

# Compact Constructor Restrictions

This is exam-favorite material.

Inside:

```java
public Person {
}
```

you cannot assign to fields.

Example:

```java
record Person(String name) {

    public Person {
        this.name = name;
    }
}
```

❌ Compilation error

Compiler already does it automatically.

---

Instead:

```java
record Person(String name) {

    public Person {
        name = name.trim();
    }
}
```

✅ Compiles

Compiler later executes:

```java
this.name = name;
```

---

Example:

```java
record Person(String name) {

    public Person {
        name = name.toUpperCase();
    }
}
```

Result:

```java
new Person("john")
```

stores:

```text
JOHN
```

---

# Additional Constructors

Records may have overloaded constructors.

Example:

```java
record Person(String name, int age) {

    Person(String name) {
        this(name, 0);
    }
}
```

✅ Valid

Must ultimately invoke the canonical constructor.

---

Invalid:

```java
record Person(String name, int age) {

    Person(String name) {
    }
}
```

❌ Compilation error

Because record components remain uninitialized.

---

# Accessor Methods Can Be Overridden

Allowed:

```java
record Person(String name) {

    @Override
    public String name() {
        return name.toUpperCase();
    }
}
```

Usage:

```java
Person p = new Person("john");

System.out.println(p.name());
```

Output:

```text
JOHN
```

---

# Exam Traps Summary

### Trap #1

```java
record Person(String name){}
```

Accessor:

```java
p.name()
```

NOT:

```java
p.getName()
```

---

### Trap #2

```java
record Person(String name){}
```

Fields are:

```java
private final
```

---

### Trap #3

```java
record Person(String name){}
```

Record is:

```java
final
```

---

### Trap #4

```java
record Person(String name){}
```

Cannot extend another record/class.

---

### Trap #5

```java
record Person(String name){}
```

Cannot declare additional instance fields.

---

### Trap #6

Compact constructor:

```java
public Person {
}
```

Compiler automatically assigns the fields.

---

### Trap #7

Records are shallowly immutable, not deeply immutable.

---

# Realistic Exam Questions

---

## Question 1

What is the output?

```java
record Person(String name, int age) {}

public class Test {
    public static void main(String[] args) {
        Person p = new Person("John", 30);
        System.out.println(p);
    }
}
```

### Answer

```text
Person[name=John, age=30]
```

Because records automatically generate `toString()`.

---

## Question 2

What is the output?

```java
record Person(String name) {}

public class Test {
    public static void main(String[] args) {
        var p1 = new Person("John");
        var p2 = new Person("John");

        System.out.println(p1.equals(p2));
    }
}
```

### Answer

```text
true
```

Records automatically generate `equals()` based on record components.

---

## Question 3

Will it compile?

```java
record Person(String name) {
    private int age;
}
```

### Answer

❌ No.

Records cannot declare additional instance fields.

---

## Question 4

Will it compile?

```java
record Person(String name) {

    public Person {
        name = name.toUpperCase();
    }
}
```

### Answer

✅ Yes.

Output field value becomes uppercase.

---

## Question 5

What is stored?

```java
record Person(String name) {

    public Person {
        name = name.trim();
    }
}

Person p = new Person(" John ");
```

### Answer

Stored value:

```text
John
```

The canonical assignment happens automatically after the compact constructor body.

---

## Question 6

Will it compile?

```java
record Person(String name) {

    public Person {
        this.name = name;
    }
}
```

### Answer

❌ No.

Fields cannot be explicitly assigned inside a compact constructor.

---

## Question 7

What is the output?

```java
record Person(String name) {}

public class Test {

    public static void main(String[] args) {
        Person p = new Person("John");

        System.out.println(p.name());
    }
}
```

### Answer

```text
John
```

Records generate accessors with the same name as the component.

---

## Question 8

Will it compile?

```java
record Person(String name) {}

record Employee(String name)
        extends Person {}
```

### Answer

❌ No.

Records are implicitly final.

---

## Question 9

What happens?

```java
record Team(List<String> members) {}

var list = new ArrayList<String>();

Team t = new Team(list);

list.add("A");

System.out.println(t.members());
```

### Answer

```text
[A]
```

Records are shallowly immutable.

---

## Question 10

Will it compile?

```java
record Person(String name, int age) {

    Person(String name) {
        this(name, 0);
    }
}
```

### Answer

✅ Yes.

Non-canonical constructors must delegate to another constructor.

---

# Must-Memorize for 1Z0-830

✅ Record components create fields, accessors, and constructor parameters automatically

✅ Records extend `java.lang.Record`

✅ Records are implicitly `final`

✅ Accessors are `componentName()`, not getters

✅ `equals()`, `hashCode()`, and `toString()` are generated automatically

✅ No extra instance fields

✅ No instance initializers

✅ Static members are allowed

✅ Compact constructors automatically assign components

✅ Canonical constructors must initialize every component

✅ Records are shallowly immutable

If you master everything above and can answer the 10 questions without looking at the answers, you'll be very well prepared for almost every Records question that appears on the Java SE 21 Developer Professional (1Z0-830) exam.
