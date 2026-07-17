# Java SE 21 Developer Professional (1Z0-830)

# Module 16 – Java Modules (JPMS Advanced)

These are **exactly** the JPMS topics Oracle expects candidates to understand. Fortunately, the exam **does not require you to be a module expert**. Oracle mostly tests:

- Reading `module-info.java`
- Understanding visibility
- Knowing the difference between `exports`, `opens`, `requires`, and `requires transitive`
- Understanding unnamed and automatic modules
- Identifying compile-time vs runtime accessibility

---

# 1. Quick Review of JPMS

Introduced in **Java 9**.

Every module has a descriptor:

```java
module com.example.app {

}
```

stored in

```
module-info.java
```

A module:

- groups packages
- declares dependencies
- controls accessibility
- improves encapsulation

---

Example

```
com.bank.app
│
├── module-info.java
└── com.bank
        Main.java
```

```java
module com.bank.app {

}
```

---

# 2. exports

This is the most common Oracle question.

Syntax

```java
exports packageName;
```

Example

```java
module bank.module {

    exports com.bank.api;

}
```

Meaning:

Other modules may access

```
com.bank.api
```

but **NOT** any other package.

Example

```
com.bank.api
com.bank.internal
```

Module:

```java
exports com.bank.api;
```

Visible:

```
✓ com.bank.api
```

Hidden:

```
✗ com.bank.internal
```

Even if classes are `public`, they are inaccessible if the package isn't exported.

Oracle LOVES this.

Example

```java
package internal;

public class Secret {}
```

Module

```java
module bank {

}
```

Another module:

```java
import internal.Secret;
```

Compilation:

```
ERROR
```

because package wasn't exported.

---

## exports to

You may export only to specific modules.

```java
exports com.bank.api to reporting.module;
```

Not heavily tested.

Know it exists.

---

# Oracle Favorite

Public class

≠

Accessible class

A public class inside a non-exported package is **still inaccessible**.

---

# 3. opens

Very frequently tested.

Syntax

```java
opens packageName;
```

Example

```java
module bank {

    opens com.bank.model;

}
```

Unlike exports:

```
exports
```

allows

Compile-time access

while

```
opens
```

allows

Reflection.

---

Reflection examples

Frameworks like

- Spring
- Hibernate
- Jackson

need reflection.

Without

```java
opens
```

reflection fails.

---

Important

```
exports
```

↓

Normal Java access

```
opens
```

↓

Reflection only

---

Example

```java
opens com.bank.model;
```

Another module cannot

```java
import com.bank.model.Customer;
```

But reflection can access it.

Oracle loves this distinction.

---

# exports vs opens

| exports                         | opens                    |
| ------------------------------- | ------------------------ |
| Compile-time access             | Reflection               |
| Normal import                   | Reflection only          |
| Allows public types             | Allows reflective access |
| No reflective access by default | No normal imports        |

---

# open module

Different!

```java
open module bank {

}
```

This means

Every package is opened for reflection.

Equivalent to

```
opens package1;
opens package2;
opens package3;
...
```

NOT exported.

Reflection only.

---

# 4. requires

Basic dependency.

```java
module app {

    requires bank.module;

}
```

Meaning

```
app
```

depends on

```
bank.module
```

---

Diagram

```
app
  |
requires
  |
bank
```

---

# 5. requires transitive

This is Oracle's favorite JPMS topic.

Suppose

```
Module C
```

↓

required by

```
Module B
```

↓

required by

```
Module A
```

Without transitive

```
A
 |
requires
 |
B
 |
requires
 |
C
```

A cannot automatically use C.

Need

```java
requires C;
```

also.

---

Example

Module B

```java
module B {

    requires C;

}
```

Module A

```java
module A {

    requires B;

}
```

Trying

```java
import c.SomeClass;
```

Compilation:

```
ERROR
```

Need

```java
requires C;
```

---

Now use

```java
module B {

    requires transitive C;

}
```

Module A

```java
module A {

    requires B;

}
```

Now

```
A
```

automatically reads

```
C
```

No additional

```java
requires C;
```

needed.

---

Think

```
requires
```

↓

Only me.

```
requires transitive
```

↓

Me + everyone depending on me.

---

Memory Trick

```
requires

A → B

Only A gets B.
```

```
requires transitive

A → B → C

A also gets C.
```

---

# 6. unnamed module

Very important.

What if code has

NO

```
module-info.java
```

?

Then it belongs to

```
Unnamed Module
```

This is how old Java projects continue working.

---

Characteristics

- Reads every module
- Exports everything
- Cannot be required by named modules

Oracle loves the last point.

---

Diagram

```
Old project

↓

Unnamed Module
```

---

Properties

| Feature                       | Unnamed Module |
| ----------------------------- | -------------- |
| module-info.java              | No             |
| Exports packages              | All            |
| Reads all modules             | Yes            |
| Can named modules require it? | No             |

---

Example

Old Java 8 project

```
src/
```

No

```
module-info.java
```

↓

Unnamed Module.

---

# 7. automatic modules

Another Oracle favorite.

Suppose you have

```
library.jar
```

without

```
module-info.java
```

Placed on

```
--module-path
```

instead of classpath.

Java creates

Automatic Module.

---

Example

```
mysql.jar
```

↓

Automatic module

↓

Module name derived from JAR.

---

Properties

Automatic module

- Reads every module
- Exports every package
- Gets automatic module name

---

Difference

Unnamed Module

↓

Classpath

Automatic Module

↓

Module path

Huge exam trap.

---

Comparison

| Unnamed            | Automatic       |
| ------------------ | --------------- |
| Classpath          | Module path     |
| No module-info     | No module-info  |
| Reads all          | Reads all       |
| Exports all        | Exports all     |
| Cannot be required | Can be required |

---

Oracle asks exactly this.

---

# 8. Reading graph

Example

```
module A {

    requires B;

}
```

```
module B {

    exports pkg;
}
```

A may import

```
pkg.*
```

---

If

```java
module B {

}
```

No exports.

Compilation fails.

---

If

```java
module B {

    opens pkg;
}
```

Still cannot import.

Reflection only.

---

# 9. Visibility Summary

| Keyword             | Meaning                      |
| ------------------- | ---------------------------- |
| exports             | Accessible to other modules  |
| opens               | Reflection                   |
| open module         | Reflection for every package |
| requires            | Dependency                   |
| requires transitive | Dependency inherited         |
| module-info.java    | Module descriptor            |

---

# 10. Oracle Favorite Traps

## Trap 1

Public class

does NOT imply accessible.

Need exported package.

---

## Trap 2

```
opens
```

does NOT allow imports.

Reflection only.

---

## Trap 3

```
exports
```

does NOT automatically allow reflection.

---

## Trap 4

```
requires
```

is NOT inherited.

Need

```
requires transitive
```

for downstream modules.

---

## Trap 5

Automatic module

≠

Unnamed module.

---

## Trap 6

Unnamed module

cannot be required.

---

## Trap 7

Module names

Need not match package names.

---

## Trap 8

Only exported packages are visible.

Not individual classes.

---

## Trap 9

Every package is strongly encapsulated.

Even public classes.

---

## Trap 10

Module descriptor

```
module-info.java
```

contains

- requires
- exports
- opens

NOT

imports.

---

# Oracle-Style Practice Questions

---

## Question 1

```java
module bank {
    exports api;
}
```

Which package is accessible?

A)

```
api
```

B)

```
internal
```

C)

All public classes

D)

Entire module

---

### Answer

✅ A

Explanation:

Only exported packages are visible.

---

## Question 2

```java
module bank {

    opens model;

}
```

Which is true?

A Reflection allowed

B Imports allowed

C Both

D Neither

---

### Answer

✅ A

---

## Question 3

Module B

```java
requires transitive C;
```

Module A

```java
requires B;
```

Can A use exported packages from C?

A Yes

B No

---

### Answer

✅ A

---

## Question 4

Automatic modules originate from

A Source folders

B JARs on module path

C JARs on classpath

D Packages

---

### Answer

✅ B

---

## Question 5

Unnamed module is created when

A Missing package

B Missing exports

C Missing module-info.java

D Missing requires

---

### Answer

✅ C

---

## Question 6

Which keyword allows reflection?

A exports

B requires

C opens

D transitive

---

### Answer

✅ C

---

## Question 7

```java
module app {
    requires bank;
}
```

If

```
bank
```

does not export

```
api
```

What happens?

A Runtime error

B Compile error

C Reflection error

D Warning

---

### Answer

✅ B

---

## Question 8

Which statement about unnamed modules is true?

A Named modules may require them

B They export everything

C They require module-info.java

D They cannot read named modules

---

### Answer

✅ B

---

## Question 9

Which keyword exports every package for reflection?

A exports

B open module

C opens

D requires

---

### Answer

✅ B

---

## Question 10

Which statement is correct?

A `opens` allows normal imports.

B `exports` allows reflection only.

C `exports` exposes packages for compile-time access.

D `requires transitive` hides dependencies.

---

### Answer

✅ C

---

# Exam Cheat Sheet (Memorize)

| Keyword               | Purpose                                                                                                                                                           |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `module-info.java`    | Declares a module and its dependencies                                                                                                                            |
| `requires`            | Depends on another module                                                                                                                                         |
| `requires transitive` | Dependency is re-exported to downstream modules                                                                                                                   |
| `exports`             | Makes a package available for normal (compile-time and runtime) access by other modules                                                                           |
| `exports ... to`      | Exports a package only to specified modules                                                                                                                       |
| `opens`               | Opens a package for reflection only (not normal imports)                                                                                                          |
| `open module`         | Opens **all** packages in the module for reflection                                                                                                               |
| Unnamed module        | Code on the **classpath** with no `module-info.java`; reads all modules, exports all packages, but **cannot be required** by named modules                        |
| Automatic module      | JAR without `module-info.java` placed on the **module path**; receives an automatic module name, exports all packages, reads all modules, and **can be required** |

# High-Probability 1Z0-830 Exam Topics

These are the JPMS concepts that have the highest likelihood of appearing on the exam:

1. ✅ Distinguishing **`exports`** from **`opens`**.
2. ✅ Understanding **`requires`** versus **`requires transitive`**.
3. ✅ Recognizing that a **public class in a non-exported package is not accessible**.
4. ✅ Knowing the difference between the **unnamed module** (classpath) and an **automatic module** (module path).
5. ✅ Reading and interpreting a simple `module-info.java`.
6. ✅ Understanding **`open module`** versus `opens package`.
7. ✅ Identifying compile-time errors caused by missing `requires` or missing `exports`.

If you can answer these types of questions confidently, you'll have covered virtually all of the JPMS material that Oracle typically tests on the Java SE 21 Developer Professional (1Z0-830) exam.
