## 🔒 2. Encapsulation & Classes

- [ ] Access modifiers (`private`, `protected`, `public`, package-private)
- [ ] Getters/setters basics
- [ ] Immutable class basics
- [ ] Static vs instance members
- [ ] `this` keyword usage

Great—this is **exactly the right level of focus** for 1Z0‑830 prep. I’ll give you:

1. ✅ **Exam-focused theory (only what you need)**
2. ✅ **Key code examples**
3. ✅ **Oracle-style traps (very important)**
4. ✅ **Practice questions like the real exam**

---

# ✅ 2. Encapsulation & Classes — Full Exam Theory

---

## 🔐 1. Access Modifiers

### ✅ The 4 Types (MEMORIZE THIS TABLE)

| Modifier    | Same Class | Same Package | Subclass (diff pkg) | Other Packages |
| ----------- | ---------- | ------------ | ------------------- | -------------- |
| `private`   | ✅         | ❌           | ❌                  | ❌             |
| _(default)_ | ✅         | ✅           | ❌                  | ❌             |
| `protected` | ✅         | ✅           | ✅ (important rule) | ❌             |
| `public`    | ✅         | ✅           | ✅                  | ✅             |

---

### ✅ Key Exam Rules

#### 1. `private`

- Only inside the **same class**
- Even subclasses **cannot access it**

---

#### 2. package-private (no modifier)

```java
class A {}  // NOT public
```

- Visible **only inside same package**
- Most exam questions test this implicitly

---

#### 3. `protected` (EXAM TRAP HEAVY ⚠️)

✔ Same package → behaves like default  
✔ Different package → only via **inheritance**

🚨 **Critical rule:**

> In a different package, you can access `protected` ONLY through inheritance AND via the reference of the subclass.

```java
// package a
package a;
public class Parent {
    protected int x = 10;
}
```

```java
// package b
package b;
import a.Parent;

public class Child extends Parent {
    void test() {
        System.out.println(x);      // ✅ OK
        Parent p = new Parent();
        System.out.println(p.x);    // ❌ DOES NOT COMPILE
    }
}
```

---

#### 4. `public`

- Accessible everywhere

---

✅ **Class-level rule**

- Top-level class → only `public` or _default_
- ❌ NOT allowed: `private`, `protected`

---

## 🔄 2. Getters & Setters (Encapsulation)

### ✅ Concept

Encapsulation = **hide fields + expose controlled access**

```java
class Person {
    private String name;   // hidden

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

---

### ✅ Why exam cares

- Data protection
- Validation logic
- Immutability foundation

---

### ✅ Common Patterns

```java
public int getAge() { return age; }

public void setAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException();
    }
    this.age = age;
}
```

---

### ⚠️ Exam Traps

1. Getter doesn’t have to start with `get`
2. Setter doesn’t have to be `void`
3. You **can** skip setter → immutable field

---

## 🧊 3. Immutable Class Basics

### ✅ Requirements (MEMORIZE THESE)

To make a class immutable:

1. `final class`
2. `private final fields`
3. No setters
4. Initialize via constructor
5. Defensive copies for mutable objects

---

### ✅ Example

```java
final class Student {
    private final String name;
    private final int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }
}
```

---

### ✅ With mutable field (⚠️ trap)

```java
final class Example {
    private final List<String> list;

    public Example(List<String> list) {
        this.list = new ArrayList<>(list); // defensive copy
    }

    public List<String> getList() {
        return new ArrayList<>(list); // NOT original
    }
}
```

---

### ⚠️ Exam Traps

1. `final` reference ≠ immutable object

```java
final List<String> list = new ArrayList<>();
list.add("X");  // ✅ allowed
```

2. Missing defensive copy → NOT immutable

---

## ⚙️ 4. Static vs Instance Members

---

### ✅ Static Members

```java
class Counter {
    static int count = 0;

    static void increment() {
        count++;
    }
}
```

✔ Belong to **class**
✔ Shared across all objects

---

### ✅ Instance Members

```java
class Dog {
    String name;   // instance field
}
```

✔ Belong to **object**

---

### ✅ Key Differences

| Feature    | static    | instance   |
| ---------- | --------- | ---------- |
| Belongs to | class     | object     |
| Access via | ClassName | object     |
| Memory     | one copy  | per object |

---

### ✅ Static Block

```java
static {
    System.out.println("Loaded");
}
```

- Runs **once when class loads**

---

### ⚠️ Exam Traps

1. Static method cannot directly access instance fields

```java
static void test() {
    // System.out.println(name); ❌
}
```

2. Static can call static directly

3. You CAN call static via object (bad practice but legal)

```java
obj.staticMethod(); // ✅ compiles
```

---

## 🔁 5. `this` Keyword

---

### ✅ Usage 1: reference current object

```java
this.name = name;
```

---

### ✅ Usage 2: constructor chaining

```java
class A {
    A() {
        this(10); // must be FIRST
    }

    A(int x) {}
}
```

---

### ✅ Usage 3: pass current object

```java
void add(A obj) {}

void test() {
    add(this);
}
```

---

### ✅ Usage 4: return current object

```java
A get() {
    return this;
}
```

---

### ⚠️ Exam Traps

1. `this()` must be **first line** in constructor
2. Cannot use `this` in static context
3. Variable shadowing confusion

```java
class Test {
    int x = 10;

    void setX(int x) {
        x = x;        // ❌ useless
        this.x = x;   // ✅ correct
    }
}
```

---

# 🧠 REAL EXAM TRAPS SUMMARY

- `protected` across packages
- default access confusion
- `final` != immutable
- static accessing instance
- `this()` placement
- missing defensive copies
- calling static via instance
- shadowing variable bug

---

# 🎯 Practice Questions (Exam Style)

---

## ✅ Q1

```java
class Test {
    private int x = 5;
}

class Main {
    public static void main(String[] args) {
        Test t = new Test();
        System.out.println(t.x);
    }
}
```

👉 What happens?

✅ **Answer:** Compilation error (private access)

---

## ✅ Q2

```java
class A {
    static int x = 10;
}

class B {
    public static void main(String[] args) {
        A a = new A();
        System.out.println(a.x);
    }
}
```

👉 Output?

✅ **Answer:** `10` (legal but bad practice)

---

## ✅ Q3 (TRICK)

```java
class Test {
    int x;

    Test() {
        this(10);
    }

    Test(int x) {
        x = x;
    }

    public static void main(String[] args) {
        Test t = new Test();
        System.out.println(t.x);
    }
}
```

👉 Output?

✅ **Answer:** `0`

➡️ `x = x` does nothing → instance variable untouched

---

## ✅ Q4

```java
final class A {
    private final int x;

    public A(int x) {
        this.x = x;
    }

    public void setX(int x) {
        this.x = x;
    }
}
```

👉 Result?

✅ **Answer:** Compilation error (`final field` reassignment)

---

## ✅ Q5 (IMPORTANT)

```java
class A {
    protected int x = 10;
}

// different package
class B extends A {
    void test() {
        A a = new A();
        System.out.println(a.x);
    }
}
```

👉 Result?

✅ **Answer:** Compilation error

---

## ✅ Q6

```java
class Test {
    static int x = 5;

    void method() {
        System.out.println(x);
    }
}
```

✅ Valid?

✅ **Answer:** YES

---

## ✅ Q7 (Constructor trap)

```java
class Test {
    Test() {
        System.out.println("A");
        this(10);
    }

    Test(int x) {}
}
```

👉 Result?

✅ **Answer:** Compilation error (`this()` not first line)

---

# ✅ Final Advice for Exam

👉 Focus especially on:

- `protected` access across packages (TOP 3 trap)
- `this()` vs constructor order
- static vs instance
- immutability rules

---

If you want, I can next:
✅ Turn this into a **1-hour revision sheet**
✅ Or give you a **mini mock exam (30 real-style questions)**
