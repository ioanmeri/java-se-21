## 🧠 1. Java Basics & Syntax

- [ ] `var` usage rules (local only, cannot be null without type)
- [ ] Scope of variables (local vs instance vs static)
- [ ] Default values (fields vs local variables)
- [ ] Order of initialization (static → instance → constructor)
- [ ] `main` method valid signatures
- [ ] Compilation vs runtime errors

---

## ✅ Day 1 – Java Basics & Syntax (1Z0-830)

---

# 1) 🔑 Key Concepts You Must Know

### 📌 Java Structure

- Every Java program has at least one **class**
- Execution starts at:

```java
public static void main(String[] args) {}
```

### 📌 Packages & Imports

```java
package com.example;
import java.util.*;
```

- `java.lang` is imported automatically
- Wildcard `*` does **not include subpackages**

---

### 📌 Variables & Types

- **Primitive types**
  - `byte, short, int, long`
  - `float, double`
  - `char`
  - `boolean`

- **Default values (instance vars only)**:
  - int → `0`
  - boolean → `false`
  - object → `null`

- **Var keyword (Java 10+)**

```java
var x = 10;     // int
```

⚠️ Must initialize immediately

---

### 📌 Literals

```java
int x = 0b1010;   // binary
int y = 0xFF;     // hex
long z = 10L;
double d = 10.5;
```

---

### 📌 Scope

- Local variables must be **initialized**
- Instance/class variables get default values

---

### 📌 Operators

- Arithmetic: `+ - * / %`
- Unary: `++ --`
- Logical: `&& || !`
- Relational: `== != > <`

---

### 📌 Strings

```java
String s = "Hello";
```

- Immutable
- `==` compares references
- `.equals()` compares values ✅

---

### 📌 Output

```java
System.out.println("Hello");
System.out.print("Hi");
```

---

# 2) ⚠️ Common Traps & Mistakes

### ❌ Using `==` with Strings

```java
String a = "Hi";
String b = new String("Hi");
System.out.println(a == b);        // false
System.out.println(a.equals(b));   // true ✅
```

---

### ❌ Uninitialized Local Variables

```java
int x;
System.out.println(x); // ❌ compile error
```

---

### ❌ `var` Misuse

```java
var x; // ❌ must be initialized
```

---

### ❌ Numeric Literals

```java
long x = 10;   // OK
long y = 10L;  // preferred
```

```java
float f = 10.0;  // ❌ double by default
float f = 10.0f; // ✅
```

---

### ❌ Increment Confusion

```java
int x = 5;
System.out.println(x++); // prints 5
System.out.println(++x); // prints 7
```

---

### ❌ Scope Issues

```java
if(true){
   int x = 10;
}
System.out.println(x); // ❌ out of scope
```

---

# 3) 🧠 Tricky Exam-Style Questions

---

## ✅ Q1

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;
        System.out.println(x++);
        System.out.println(++x);
    }
}
```

**Options:**
A) 10, 11  
B) 10, 12 ✅  
C) 11, 12  
D) 11, 11

**✅ Answer: B**

**Explanation:**

- `x++` → print first (10), then increment → 11
- `++x` → increment first → 12, then print

---

## ✅ Q2

```java
public class Test {
    public static void main(String[] args) {
        String a = "Java";
        String b = new String("Java");
        System.out.println(a == b);
        System.out.println(a.equals(b));
    }
}
```

**Answer:**

```
false
true
```

**Explanation:**

- `==` → compares references
- `.equals()` → compares content ✅

---

## ✅ Q3

```java
public class Test {
    public static void main(String[] args) {
        var x = 10;
        x = 10.5;
        System.out.println(x);
    }
}
```

**✅ Answer: Compile-time error**

**Explanation:**

- `var x = 10` → `x` is `int`
- Cannot assign `double` later

---

## ✅ Q4

```java
public class Test {
    int x;
    public static void main(String[] args) {
        Test t = new Test();
        System.out.println(t.x);
    }
}
```

**✅ Answer: 0**

**Explanation:**

- Instance variables get default values

---

## ✅ Q5

```java
public class Test {
    public static void main(String[] args) {
        int x;
        if (true) {
            x = 10;
        }
        System.out.println(x);
    }
}
```

**✅ Answer: 10**

**Explanation:**

- Compiler sees `x` is always initialized (since `if(true)`)

---

## ✅ Q6

```java
public class Test {
    public static void main(String[] args) {
        double d = 10 / 4;
        System.out.println(d);
    }
}
```

**✅ Answer: 2.0**

**Explanation:**

- `10/4` = integer division → `2`
- Then assigned → `2.0`

---

## ✅ Q7

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(10 + 20 + "Java");
        System.out.println("Java" + 10 + 20);
    }
}
```

**✅ Answer:**

```
30Java
Java1020
```

**Explanation:**

- Left-to-right evaluation

---

## ✅ Q8

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;
        if (x > 2)
            int y = 10;
        System.out.println("Done");
    }
}
```

**✅ Answer: Compile-time error**

**Explanation:**

- Can't declare variable without `{}` in single-line `if`

---

# 4) 🚀 Quick Exam Tips

- ✅ Always check:
  - **data types**
  - **initialization**
  - **scope**
  - **operator precedence**

- ✅ Watch for:
  - `== vs equals()`
  - `++x vs x++`
  - `String + int` order
  - `var` inference rules

---

# ✅ Want Next Step?

I can continue with:

👉 **Day 2 – Control Flow (if/switch/loops)**  
👉 or give you a **mini mock test (20 questions)** JUST on Day 1 to lock it in.

Just tell me 👍

---

Great checklist ✅ — you already covered the **core high-yield topics**. I’ll enhance it with **missing theory + exam traps + extra tricky questions**.

---

# ✅ 🔥 Missing / Extra Theory You Should Add

## ➕ 1) Identifier Rules (VERY common exam trap)

```java
int _x = 10;   // ✅
int $x = 20;   // ✅
int 1x = 10;   // ❌
int class = 10; // ❌ keyword
```

✅ Rules:

- Can start with: letter, `_`, `$`
- Cannot start with number
- Cannot be a **Java keyword**
- Case-sensitive (`age` ≠ `Age`)

---

## ➕ 2) Class & File Structure Rules

```java
public class A {}
class B {}
```

✅ Key rules:

- Only **ONE public class per file**
- Filename must match public class name
- You can have multiple non-public classes

---

## ➕ 3) Import Behavior (VERY tested)

```java
import java.util.*;
```

⚠️ Trap:

- Imports only the package (not subpackages)

```java
import java.util.*; // does NOT include java.util.concurrent
```

---

## ➕ 4) Numeric Promotion Rules

```java
byte a = 10;
byte b = 20;
byte c = (byte)(a + b); // ❗ required cast
```

✅ Important:

- Arithmetic promotes to **int**
- Even if both operands are `byte`/`short`

---

## ➕ 5) char + int behavior

```java
char c = 'A';
int x = c + 1; // 66
```

✅ char behaves like an **integer (Unicode)**

---

## ➕ 6) Underscores in Numbers

```java
int x = 1_000_000; // ✅
int y = _1000;     // ❌
int z = 1000_;     // ❌
```

---

## ➕ 7) Boolean Restrictions

```java
if (1) {}  // ❌ Java (NOT like C)
```

✅ Only `boolean` allowed in conditions

---

## ➕ 8) main Method Valid Forms ✅ (your checklist item expanded)

✅ All valid:

```java
public static void main(String[] args)
public static void main(String args[])
public static void main(String... args)
static public void main(String[] args)
```

❌ Invalid:

```java
public void main(String[] args)      // ❌ no static
public static int main(String[] args) // ❌ wrong return
```

---

## ➕ 9) Order of Initialization (expand yours)

✅ Exact order:

1. Static variables
2. Static blocks
3. Instance variables
4. Instance initialization blocks
5. Constructor

---

## ➕ 10) Compilation vs Runtime Errors (expand)

✅ Compile-time:

- Syntax issues
- Type mismatch
- Uninitialized variables

✅ Runtime:

- `NullPointerException`
- `ArithmeticException`
- Array index out of bounds

---

# ✅ ⚠️ Additional High-Yield Traps

- Integer overflow (rare but testable)
- `final` variables must be assigned once
- `null` assignment:

```java
var x = null; // ❌ cannot infer type
```

---

# 🧠 ✅ New Exam-Style Questions (Important Level)

---

## ✅ Q1 – Identifier Trap

```java
public class Test {
    public static void main(String[] args) {
        int $ = 10;
        int _ = 20;
        System.out.println($ + _);
    }
}
```

✅ Answer: `30`

👉 `$` and `_` are valid identifiers

---

## ✅ Q2 – File/Class Rule

```java
public class A {}
public class B {}
```

✅ Answer: ❌ Compile-time error

👉 Only one public class per file

---

## ✅ Q3 – Import Trap

```java
import java.util.*;

public class Test {
    java.util.concurrent.atomic.AtomicInteger x;
}
```

✅ Answer: ✅ Compiles

👉 Fully qualified name is used, import not required

---

## ✅ Q4 – Numeric Promotion

```java
public class Test {
    public static void main(String[] args) {
        byte a = 10;
        byte b = 20;
        byte c = a + b;
    }
}
```

✅ Answer: ❌ Compile-time error

👉 Result is `int`, needs cast

---

## ✅ Q5 – char Arithmetic

```java
public class Test {
    public static void main(String[] args) {
        char c = 'A';
        System.out.println(c + 1);
    }
}
```

✅ Answer: `66`

---

## ✅ Q6 – Underscore Trap

```java
int x = 1_0_0_0;
int y = 1000_;
```

✅ Answer:

- `x` ✅ valid
- `y` ❌ compile error

---

## ✅ Q7 – main Method

```java
class Test {
    static public void main(String... args) {
        System.out.println("Hi");
    }
}
```

✅ Answer: ✅ Runs

---

## ✅ Q8 – Order of Initialization

```java
public class Test {
    static int a = print("Static var");
    int b = print("Instance var");

    public Test() {
        print("Constructor");
    }

    static int print(String s) {
        System.out.println(s);
        return 1;
    }

    public static void main(String[] args) {
        new Test();
    }
}
```

✅ Answer:

```
Static var
Instance var
Constructor
```

---

## ✅ Q9 – Boolean Trap

```java
public class Test {
    public static void main(String[] args) {
        if(1 == 1)
            System.out.println("Yes");
    }
}
```

✅ Answer: ✅ Works

👉 Because expression is boolean

BUT:

```java
if(1)
```

❌ invalid

---

## ✅ Q10 – var + null

```java
var x = null;
```

✅ Answer: ❌ Compile-time error

---

# ✅ ✅ Final Improved Checklist

Here’s your **exam-ready Day 1 checklist**:

✅ var rules  
✅ Scope (local / instance / static)  
✅ Default values  
✅ Initialization order  
✅ main method signatures  
✅ Compile vs runtime errors

➕ ADD THESE:
✅ Identifier rules  
✅ Class/file rules  
✅ Import behavior  
✅ Numeric promotion  
✅ char arithmetic  
✅ Underscore literals  
✅ Boolean restrictions  
✅ String comparison (`==` vs `.equals`)

---

# 🚀 If you want

I can now give you:

✅ **Day 1 Mini Mock Exam (25 HARD questions like real 1Z0-830)**  
✅ or move to **Day 2 (Control Flow Mastery)**

Just tell me 👍
