Yes. **JPMS (Java Platform Module System)** is part of the **Oracle Java SE 21 Developer Professional (1Z0-830)** exam, although it is a relatively **small objective** compared to Streams, Generics, Collections, Concurrency, and I/O.

Oracle usually asks **simple but tricky** questions rather than deep module architecture questions. Most questions revolve around remembering the syntax and understanding what is visible outside a module.

---

# 🧭 Java SE 21 Developer Professional (1Z0-830)

# Module System (JPMS)

---

# What is JPMS?

JPMS stands for

> **Java Platform Module System**

It was introduced in **Java 9**.

Before Java 9, applications consisted only of packages and JAR files.

Example:

```
project
 ├── package com.app
 ├── package com.util
 ├── package com.db
```

Nothing explicitly declared what could be accessed.

With JPMS:

```
module
    ↓
packages
    ↓
classes
```

Modules provide:

- stronger encapsulation
- explicit dependencies
- reliable configuration
- smaller runtime images (jlink—not on the exam)

---

# Hierarchy

```
Application

 ├── Module A
 │      ├── package a
 │      └── package b
 │
 ├── Module B
 │      ├── package c
 │      └── package d
 │
 └── Module C
```

Think of modules as a level above packages.

---

# Why Modules?

Before Java 9

```
JAR
```

could access nearly everything.

Large applications became difficult to manage.

Modules solve this by declaring

- what they need
- what they expose

---

# Every module contains

```
module-info.java
```

This file is called

> **Module Descriptor**

It defines

- module name
- dependencies
- exported packages
- services (not on exam)

---

Example

```
module zoo.animal {
}
```

This is the smallest possible module.

---

# Module Structure

```
src

 └── zoo.animal
       │
       ├── module-info.java
       │
       └── com
            └── zoo
                 └── Animal.java
```

---

# module-info.java

Example

```java
module zoo.animal {

}
```

Module name

```
zoo.animal
```

does NOT need to match a package name.

Oracle likes this.

---

# Module Naming Convention

Common style

```
com.company.application
```

Example

```
module com.oracle.payment {

}
```

or

```
module com.example.store {

}
```

---

# First Important Rule

Every module has **exactly one**

```
module-info.java
```

located in the root of the module.

---

# requires

A module depends on another module.

Example

```
module bookstore {

    requires inventory;

}
```

Meaning

```
bookstore
        ↓
   inventory
```

---

Diagram

```
Bookstore
     |
 requires
     |
Inventory
```

---

Multiple dependencies

```java
module bookstore {

    requires inventory;
    requires payment;
    requires logging;

}
```

Perfectly valid.

---

# exports

Packages are NOT visible outside a module by default.

Example

```
module inventory {

    exports com.store.product;

}
```

Only this package becomes public.

---

Without exports

```
module inventory {

}
```

Nothing is accessible outside.

This is Oracle's favorite concept.

---

Example

```
inventory

package com.store.product
package com.store.internal
```

```
module inventory {

    exports com.store.product;

}
```

Result

Accessible

```
com.store.product
```

Hidden

```
com.store.internal
```

---

Diagram

```
Inventory Module

+---------------------------+

product      → exported ✔

internal     → hidden ✘

+---------------------------+
```

---

# requires + exports together

Module A

```java
module inventory {

    exports com.store.product;

}
```

Module B

```java
module bookstore {

    requires inventory;

}
```

Now

```
Bookstore
```

may import

```java
import com.store.product.Product;
```

---

If inventory does NOT export

```
com.store.product
```

Compilation fails.

---

# Access Rules

For one module to use another

Both must be true

✔ requires

AND

✔ exports

Missing either one

↓

Compilation error.

Oracle LOVES this.

---

Example

Inventory

```java
module inventory {

}
```

Bookstore

```java
module bookstore {

    requires inventory;

}
```

Can Bookstore access classes?

No.

Nothing was exported.

---

Reverse Example

Inventory

```java
module inventory {

    exports com.store.product;

}
```

Bookstore

```java
module bookstore {

}
```

Still impossible.

Bookstore forgot

```
requires inventory;
```

Need BOTH.

---

# Public does NOT mean accessible

Inside a module

```java
public class Product {

}
```

Still hidden if package isn't exported.

Huge exam trap.

---

Example

```java
module inventory {

}
```

```
public class Product
```

Outside module?

Still inaccessible.

Because package wasn't exported.

---

# Module Dependency Graph

```
App

requires

Inventory

exports

com.store.product
```

Flow

```
Bookstore
     |
requires
     |
Inventory
     |
exports
     |
Product Package
```

---

# Package Visibility

Suppose

```
inventory

com.store.api
com.store.impl
com.store.internal
```

Module

```java
module inventory {

    exports com.store.api;

}
```

Outside module

Accessible

```
api
```

Not accessible

```
impl
internal
```

---

# Packages remain packages

Modules do NOT replace packages.

Hierarchy

```
Module

↓

Package

↓

Class
```

---

# Module Name vs Package Name

Legal

```
module store {

}
```

Inside

```
package com.oracle.payment;
```

They are unrelated.

Oracle likes this.

---

# Java Base Module

Every module automatically depends on

```
java.base
```

Therefore

You NEVER write

```java
requires java.base;
```

Although it is legal, it is **redundant**.

This module contains

- Object
- String
- Math
- Exception
- Collections
- Streams
- etc.

Everything fundamental.

---

# Example

```java
module app {

}
```

Still has access to

```java
String
Object
List
Map
```

because

```
java.base
```

is automatically required.

---

# Example

```java
module app {

    requires java.sql;

}
```

Now

```
java.base
```

-

```
java.sql
```

are available.

---

# Module Descriptor Summary

```java
module bookstore {

    requires inventory;

    requires java.sql;

    exports com.store.api;

}
```

Read as

Module

```
bookstore
```

depends on

```
inventory

java.sql
```

and exposes

```
com.store.api
```

---

# Oracle Favorite Questions

## Question 1

```java
module inventory {

}
```

Product.java

```java
package com.store;

public class Product {

}
```

Can another module use Product?

Answer

❌ No.

Nothing is exported.

---

## Question 2

```java
module inventory {

    exports com.store;

}
```

Bookstore

```java
module bookstore {

}
```

Can Bookstore use Product?

Answer

❌ No.

Missing

```
requires inventory;
```

---

## Question 3

```java
module inventory {

    exports com.store;
}
```

```java
module bookstore {

    requires inventory;
}
```

Can Product be imported?

Answer

✅ Yes.

Both conditions satisfied.

---

## Question 4

Which file defines a module?

A

```
package-info.java
```

B

```
module-info.java
```

C

```
manifest.mf
```

D

```
module.java
```

Answer

✅ B

---

## Question 5

How many module descriptors may a module contain?

A

0

B

1

C

Many

D

Unlimited

Answer

✅ B

Exactly one.

---

## Question 6

Which keyword exposes packages?

A

```
public
```

B

```
visible
```

C

```
exports
```

D

```
requires
```

Answer

✅ C

---

## Question 7

Which keyword declares module dependencies?

A

```
imports
```

B

```
requires
```

C

```
extends
```

D

```
opens
```

Answer

✅ B

---

## Question 8

Which statement is true?

A

Every public class is visible outside the module.

B

Every package is exported automatically.

C

Packages must be exported explicitly.

D

requires exports packages.

Answer

✅ C

---

## Question 9

Which module is automatically required?

A

java.lang

B

java.util

C

java.base

D

java.root

Answer

✅ C

---

## Question 10

Which is valid?

```java
module app {

    requires java.sql;

    exports com.app.api;

}
```

Answer

✅ Valid.

---

# Certification Traps

## Trap 1

Public class ≠ Visible

Wrong thinking

```
public class Product
```

means

Everyone can use it.

Wrong.

Need

```
exports
```

---

## Trap 2

requires alone is enough

Wrong.

Need BOTH

```
requires

exports
```

---

## Trap 3

exports alone is enough

Wrong.

Consumer module still needs

```
requires
```

---

## Trap 4

Packages exported automatically

False.

Default

```
Nothing exported.
```

---

## Trap 5

Module name must equal package

False.

They are independent.

---

## Trap 6

Need

```
requires java.base;
```

False.

Automatic.

---

## Trap 7

One package = one module

False.

A module may contain many packages.

---

## Trap 8

Modules replace packages

False.

Modules contain packages.

---

## Trap 9

module-info.java may appear anywhere

False.

It belongs at the root of the module source.

---

## Trap 10

A module can have multiple module-info.java files

False.

Exactly one per module.

---

# High-Probability Exam Facts

| Fact                                  | Must Know                 |
| ------------------------------------- | ------------------------- |
| Module descriptor file                | `module-info.java`        |
| Declares dependencies                 | `requires`                |
| Makes package visible                 | `exports`                 |
| Default package visibility            | Hidden outside the module |
| Public class without exported package | Still inaccessible        |
| Consumer needs                        | `requires`                |
| Producer needs                        | `exports`                 |
| Module contains                       | Packages                  |
| Automatic dependency                  | `java.base`               |
| One descriptor per module             | Yes                       |

# Final Oracle Exam Cheat Sheet (Memorize)

```
module-info.java
    ↓
Declares module

requires
    ↓
Depends on another module

exports
    ↓
Makes package visible outside

No exports
    ↓
Nothing visible

No requires
    ↓
Cannot use another module

public class
    ↓
Still hidden if package isn't exported

java.base
    ↓
Automatically required

One module
    ↓
One module-info.java

Hierarchy

Module
   ↓
Package
   ↓
Class
```

## What to expect on the 1Z0-830 exam

For JPMS, Oracle typically focuses on **syntax and visibility rules**, not advanced modular architecture. The highest-probability topics are:

- Correct `module-info.java` syntax.
- Understanding the roles of `requires` and `exports`.
- Recognizing that **both** `requires` (consumer) and `exports` (provider) are required for access.
- Knowing that `public` alone does **not** make a class accessible outside its module.
- Remembering that `java.base` is implicitly required.
- Identifying valid and invalid module declarations.

If you can answer the practice questions above without hesitation and remember the cheat sheet, you'll have covered essentially all of the JPMS knowledge typically expected for the Java SE 21 Developer Professional (1Z0-830) exam.
