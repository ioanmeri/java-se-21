# Java SE 21 (1Z0-830) – Sealed Classes Exam Guide

Sealed classes were finalized in Java 17 (JEP 409) and are part of Java SE 21. They allow you to **control inheritance** by explicitly specifying which classes or interfaces may extend or implement a type. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

---

# 1. Purpose of Sealed Classes

Before sealed classes:

```java
class Shape { }
```

Any class could extend `Shape`.

With sealed classes:

```java
public sealed class Shape
    permits Circle, Rectangle, Square {
}
```

Only the classes listed in the `permits` clause may extend `Shape`. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

Benefits:

- Better control of APIs
- More maintainable hierarchies
- Better support for pattern matching and switch expressions
- Compiler knows all permitted subclasses [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html), [\[dev.to\]](https://dev.to/adrian_nowosielski_7bd282/sealed-classes-in-java-21-controlling-class-hierarchies-1moo)

---

# 2. The `sealed` Modifier

A sealed type restricts inheritance.

Syntax:

```java
public sealed class Animal
    permits Dog, Cat {
}
```

or

```java
public sealed interface Vehicle
    permits Car, Truck {
}
```

A sealed type must explicitly specify its permitted subclasses unless all of them are declared in the same source file. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

---

# 3. The `permits` Clause

The `permits` clause lists all direct subclasses.

Example:

```java
public sealed class Shape
    permits Circle, Square, Rectangle {
}
```

Only:

```java
Circle
Square
Rectangle
```

may directly extend `Shape`. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

Attempting:

```java
class Triangle extends Shape { }
```

causes a compilation error.

---

# 4. Omission of `permits`

Exam favorite!

If all permitted subclasses are declared in the same file:

```java
sealed class Shape {

}

final class Circle extends Shape {

}

final class Square extends Shape {

}
```

The compiler can infer the permitted subclasses.

Therefore the `permits` clause is optional in this case. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

---

# 5. The Three Choices for a Permitted Subclass

A direct subclass of a sealed type MUST be exactly one of:

- `final`
- `sealed`
- `non-sealed`

Anything else causes a compilation error. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

---

## A. `final`

Stops inheritance completely.

```java
public final class Circle extends Shape {
}
```

No class may extend `Circle`.

Think:

> final = end of hierarchy

---

## B. `sealed`

Continues the restriction.

```java
public sealed class Rectangle
    extends Shape
    permits FilledRectangle {
}
```

Only `FilledRectangle` may extend `Rectangle`. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

Think:

> sealed = continue controlling inheritance

---

## C. `non-sealed`

Reopens inheritance.

```java
public non-sealed class Square
    extends Shape {
}
```

Now any class may extend `Square`.

```java
class ColoredSquare extends Square {
}
```

This is legal. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html), [\[stackoverflow.com\]](https://stackoverflow.com/questions/63972130/what-is-the-difference-between-a-final-and-a-non-sealed-class-in-java-15s-seale)

Think:

> non-sealed = break the seal

---

# 6. Restrictions on Inheritance

For the exam you must know all restrictions.

## Restriction #1

A sealed class must have permitted subclasses.

```java
sealed class A permits B {
}

final class B extends A {
}
```

Valid.

---

## Restriction #2

Every direct subclass must be:

```java
final
sealed
non-sealed
```

This is invalid:

```java
sealed class A permits B {
}

class B extends A {
}
```

Compilation error. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

---

## Restriction #3

Permitted subclasses must directly extend the sealed class.

Valid:

```java
sealed class A permits B {
}

final class B extends A {
}
```

Invalid:

```java
sealed class A permits C {
}

class B extends A {
}

class C extends B {
}
```

Because `C` is not a direct subclass.

---

## Restriction #4

Permitted subclasses must be in:

- the same module, OR
- the same package (if unnamed module)

as the sealed class. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

Oracle likes testing this.

---

## Restriction #5

Classes not listed in `permits` cannot extend the sealed class.

```java
sealed class A permits B {
}

final class B extends A {
}

final class C extends A {
}
```

Compilation error.

`C` is not permitted.

---

# 7. Sealed Interfaces

Interfaces can also be sealed.

```java
public sealed interface Vehicle
    permits Car, Truck {
}
```

Subtypes:

```java
final class Car implements Vehicle {
}

non-sealed class Truck implements Vehicle {
}
```

Perfectly valid. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html)

---

# 8. Common Exam Traps

## Trap 1

```java
sealed class A permits B {
}

class B extends A {
}
```

❌ Compilation error

Reason:

```java
B
```

must be:

```java
final
sealed
non-sealed
```

---

## Trap 2

```java
sealed class A permits B {
}

final class C extends A {
}
```

❌ Compilation error

`C` isn't permitted.

---

## Trap 3

```java
sealed interface X permits Y {
}

interface Y extends X {
}
```

❌ Compilation error

`Y` must be:

```java
sealed
non-sealed
```

or

```java
final
```

(interfaces can't be final, so only sealed/non-sealed are possible).

---

## Trap 4

```java
sealed class A permits B {
}

non-sealed class B extends A {
}

final class C extends B {
}
```

✅ Compiles

Because `non-sealed` reopens inheritance.

---

# Oracle-Style Practice Questions

---

## Question 1

What is the output?

```java
sealed class Animal permits Dog {
}

final class Dog extends Animal {
}

public class Test {
    public static void main(String[] args) {
        System.out.println("Compiled");
    }
}
```

### Answer

✅ Compiles

Output:

```text
Compiled
```

Reason:

- `Animal` is sealed.
- `Dog` is permitted.
- `Dog` is declared `final`.

All rules are satisfied.

---

## Question 2

Will this compile?

```java
sealed class Vehicle permits Car {
}

class Car extends Vehicle {
}
```

### Answer

❌ Does not compile

Reason:

A direct subclass of a sealed class must be:

```java
final
sealed
non-sealed
```

`Car` has none of them.

---

## Question 3

Will this compile?

```java
sealed class Shape permits Circle {
}

final class Circle extends Shape {
}

final class Square extends Shape {
}
```

### Answer

❌ Does not compile

Reason:

`Square` is not listed in the `permits` clause.

---

## Question 4

How many compilation errors?

```java
sealed class A permits B {
}

sealed class B extends A {
}
```

### Answer

✅ One compilation error

`B` is sealed but doesn't specify its own permitted subclasses.

A sealed class must have permitted subclasses.

---

## Question 5

Will this compile?

```java
sealed class A permits B {
}

non-sealed class B extends A {
}

class C extends B {
}
```

### Answer

✅ Compiles

Reason:

`non-sealed` reopens inheritance.

`C` may freely extend `B`.

---

## Question 6

What happens?

```java
sealed interface Vehicle permits Car {
}

non-sealed class Car implements Vehicle {
}

class SportsCar extends Car {
}
```

### Answer

✅ Compiles

Reason:

`Car` is `non-sealed`.

Therefore `SportsCar` may extend it.

---

## Question 7

Will this compile?

```java
sealed interface Flyable permits Bird {
}

sealed class Bird implements Flyable {
}
```

### Answer

❌ Does not compile

Reason:

`Bird` is sealed but lacks its own `permits` clause (or inferred permitted subclasses).

---

## Question 8

Choose all valid modifiers for a direct subclass of a sealed class.

A. abstract

B. final

C. sealed

D. non-sealed

### Answer

✅ B

✅ C

✅ D

❌ A

A direct subclass must be one of:

```java
final
sealed
non-sealed
```

---

## Question 9

Will this compile?

```java
sealed class Animal permits Dog {
}

final class Dog extends Animal {
}

class Poodle extends Dog {
}
```

### Answer

❌ Does not compile

Reason:

`Dog` is `final`.

Final classes cannot be extended.

---

## Question 10

What is true about `non-sealed`?

A. Prevents inheritance

B. Allows only permitted subclasses

C. Reopens inheritance

D. Equivalent to final

### Answer

✅ C

`non-sealed` breaks the inheritance restriction and allows unrestricted extension. [\[docs.oracle.com\]](https://docs.oracle.com/en/java/javase/21/language/sealed-classes-and-interfaces.html), [\[stackoverflow.com\]](https://stackoverflow.com/questions/63972130/what-is-the-difference-between-a-final-and-a-non-sealed-class-in-java-15s-seale)

---

# Exam Memorization Summary

Remember this rule:

```text
sealed  -> restricted inheritance
final   -> stop inheritance
non-sealed -> reopen inheritance
```

And the most tested fact:

```text
Every direct subclass of a sealed class/interface
must be either:

final
sealed
non-sealed
```

If you remember only one rule for 1Z0-830, make it that one.
