🔥 Perfect—here’s your **Day 1 Mini Mock Exam (25 HARD Questions)** for **1Z0-830**.  
These are designed to mimic real exam traps.

---

# 🧠 ✅ Instructions

- Try answering first
- Then check solutions below

---

# 💥 ✅ QUESTIONS

---

## Q1

```java
public class Test {
    public static void main(String[] args) {
        var x = 10;
        var y = 10.0;
        var z = x + y;
        System.out.println(z);
    }
}
```

---

## Q2

```java
public class Test {
    static int x;
    public static void main(String[] args) {
        System.out.println(x);
    }
}
```

---

## Q3

```java
public class Test {
    public static void main(String[] args) {
        int x;
        System.out.println(x);
    }
}
```

---

## Q4

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(10 + 20 + "Java" + 10 + 20);
    }
}
```

---

## Q5

```java
public class Test {
    public static void main(String[] args) {
        byte a = 1;
        byte b = 2;
        byte c = a + b;
    }
}
```

---

## Q6

```java
public class Test {
    public static void main(String[] args) {
        char c = 'A';
        c = c + 1;
        System.out.println(c);
    }
}
```

---

## Q7

```java
public class Test {
    public static void main(String[] args) {
        var x = null;
    }
}
```

---

## Q8

```java
class Test {
    static public void main(String... args) {
        System.out.println("OK");
    }
}
```

---

## Q9

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;
        if (x > 5)
            int y = 20;
        System.out.println("Done");
    }
}
```

---

## Q10

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;
        System.out.println(++x + x++);
    }
}
```

---

## Q11

```java
public class Test {
    static int x = print("A");
    static int y = print("B");

    static int print(String s) {
        System.out.print(s);
        return 1;
    }

    public static void main(String[] args) {}
}
```

---

## Q12

```java
public class Test {
    public static void main(String[] args) {
        String s = "Java";
        String t = "Java";
        System.out.println(s == t);
    }
}
```

---

## Q13

```java
public class Test {
    public static void main(String[] args) {
        String s = new String("Java");
        String t = new String("Java");
        System.out.println(s == t);
    }
}
```

---

## Q14

```java
public class Test {
    public static void main(String[] args) {
        int x = 1_0_0_0;
        int y = 1000_;
    }
}
```

---

## Q15

```java
public class Test {
    int x;
    static int y;
    public static void main(String[] args) {
        Test t = new Test();
        System.out.println(t.x + " " + y);
    }
}
```

---

## Q16

```java
public class Test {
    public static void main(String[] args) {
        double d = 10 / 4;
        System.out.println(d);
    }
}
```

---

## Q17

```java
public class Test {
    public static void main(String[] args) {
        if(true);
        System.out.println("Hello");
    }
}
```

---

## Q18

```java
public class Test {
    public static void main(String[] args) {
        boolean b = false;
        if(b = true)
            System.out.println("Yes");
    }
}
```

---

## Q19

```java
public class Test {
    public static void main(String[] args) {
        long x = 10;
        float y = x;
        System.out.println(y);
    }
}
```

---

## Q20

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;
        System.out.println(x++ + ++x);
    }
}
```

---

## Q21

```java
public class Test {
    static {
        System.out.println("Static");
    }
    public static void main(String[] args) {
        System.out.println("Main");
    }
}
```

---

## Q22

```java
public class Test {
    public static void main(String[] args) {
        int x = (int)10.5;
        System.out.println(x);
    }
}
```

---

## Q23

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(true && false || true);
    }
}
```

---

## Q24

```java
public class Test {
    public static void main(String[] args) {
        int x = 0;
        if(x)
            System.out.println("Hi");
    }
}
```

---

## Q25

```java
public class Test {
    public static void main(String[] args) {
        System.out.println('A' + 'B');
    }
}
```

---

# ✅ ✅ ANSWERS + QUICK EXPLANATIONS

---

### ✅ Q1 → `20.0`

- int + double → double

---

### ✅ Q2 → `0`

- static default value

---

### ✅ Q3 → ❌ Compile Error

- local variable not initialized

---

### ✅ Q4 → `30Java1020`

- left-to-right evaluation

---

### ✅ Q5 → ❌ Compile Error

- byte + byte → int

---

### ✅ Q6 → ❌ Compile Error

- needs cast:

```java
c = (char)(c + 1);
```

---

### ✅ Q7 → ❌ Compile Error

- var cannot infer null

---

### ✅ Q8 → ✅ OK

- valid main signature

---

### ✅ Q9 → ❌ Compile Error

- cannot declare variable in single-line if

---

### ✅ Q10 → `12`

Steps:

- ++x → 6
- x++ → 6  
  → 6 + 6

---

### ✅ Q11 → `AB`

- static executes top to bottom

---

### ✅ Q12 → `true`

- string pool

---

### ✅ Q13 → `false`

- different objects

---

### ✅ Q14 → ❌ Compile Error

- trailing underscore not allowed

---

### ✅ Q15 → `0 0`

- default values

---

### ✅ Q16 → `2.0`

- integer division first

---

### ✅ Q17 → `Hello`

- `if(true);` is empty statement

---

### ✅ Q18 → `Yes`

- assignment inside if → true

---

### ✅ Q19 → `10.0`

- implicit widening

---

### ✅ Q20 → `12`

Steps:

- x++ → 5 (x=6)
- ++x → 7
  → 5 + 7

---

### ✅ Q21 →

```
Static
Main
```

---

### ✅ Q22 → `10`

- casting truncates

---

### ✅ Q23 → `true`

- precedence: `&&` then `||`

---

### ✅ Q24 → ❌ Compile Error

- Java requires boolean

---

### ✅ Q25 → `131`

- ASCII: 65 + 66

---

# 🔥 Final Tip

If you scored:

- ✅ **22–25** → Ready for exam ✅
- ✅ **18–21** → Almost there (revise traps)
- ✅ **<18** → Repeat Day 1 + redo test

---

# 🚀 Next

Say:

👉 **“Day 2”** → Control Flow (if/switch/loops deeper traps)  
👉 OR **“Hard Mock 50Q”** → Full exam simulation

Let's get you certified 💪
