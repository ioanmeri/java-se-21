Absolutely. Based on Oracle's **Java SE 21 Developer Professional (1Z0-830)** exam objectives and Oracle's historical question style, **Generics** is one of the most important chapters. Oracle loves asking questions that compile (or don't compile) because of subtle generic rules.

The exam is **not** interested in advanced generic library design. Instead, it focuses on:

- compilation errors
- wildcard rules
- type inference
- bounds
- raw types
- PECS
- generic methods
- type erasure

---

# Java SE 21 (1Z0-830)

## 7. Generics (expand)

Current list is too short.

Add:

- [ ] Type inference
- [ ] Generic methods
- [ ] Raw types
- [ ] Generic bounds
- [ ] PECS (`Producer Extends Consumer Super`)
- [ ] Type erasure

# Generics — Complete Theory

---

# 1. Why Generics Exist

Without generics:

```java
List list = new ArrayList();

list.add("Java");
list.add(5);

String s = (String) list.get(1); // Runtime ClassCastException
```

Generics provide:

- compile-time type safety
- eliminate casts
- clearer code

Using generics:

```java
List<String> list = new ArrayList<>();

list.add("Java");
list.add(5);          // Compile error

String s = list.get(0);
```

The compiler prevents incorrect types.

---

# 2. Generic Type Parameters

```java
class Box<T> {

    private T value;

    void set(T value){
        this.value = value;
    }

    T get(){
        return value;
    }
}
```

Usage:

```java
Box<String> box = new Box<>();

box.set("Hello");

String s = box.get();
```

Another example:

```java
Box<Integer> numbers = new Box<>();

numbers.set(100);

Integer i = numbers.get();
```

Same class.

Different types.

---

# 3. Multiple Type Parameters

```java
class Pair<K,V>{

    K key;
    V value;

}
```

Usage

```java
Pair<String,Integer> p =
        new Pair<>();

p.key = "Age";
p.value = 30;
```

Common names

```
T  Type

E  Element

K  Key

V  Value

N  Number
```

Oracle likes these.

---

# 4. Diamond Operator

Before Java 7

```java
List<String> list =
    new ArrayList<String>();
```

Today

```java
List<String> list =
    new ArrayList<>();
```

Compiler infers the type.

---

# 5. Type Inference ⭐⭐⭐⭐

The compiler often determines the generic type automatically.

Example

```java
var list = new ArrayList<String>();
```

Compiler knows

```
ArrayList<String>
```

Another example

```java
List<Integer> list =
    new ArrayList<>();
```

Compiler infers

```
ArrayList<Integer>
```

Generic methods also use inference.

Example

```java
static <T> T identity(T value){
    return value;
}

String s = identity("Java");
```

Compiler infers

```
T = String
```

No need

```java
<String>identity(...)
```

although this is legal

```java
String s =
    Test.<String>identity("Java");
```

---

## Oracle Trap

```java
var list = new ArrayList<>();
```

What is the type?

It becomes

```java
ArrayList<Object>
```

NOT

```
ArrayList<String>
```

because there is no information available.

---

# 6. Generic Methods ⭐⭐⭐⭐⭐

Generic methods declare their own type parameter.

Example

```java
class Utils {

    static <T> void print(T value){

        System.out.println(value);

    }

}
```

Usage

```java
Utils.print("Java");

Utils.print(10);

Utils.print(true);
```

Same method.

Many types.

---

Example returning values

```java
static <T> T first(T value){

    return value;

}
```

Usage

```java
String s =
    first("Oracle");

Integer i =
    first(10);
```

---

Important syntax

Correct

```java
public static <T> T get(T value)
```

Wrong

```java
public static T <T> get(T value)
```

The `<T>` must appear before the return type.

Oracle loves this syntax question.

---

# 7. Raw Types ⭐⭐⭐⭐⭐

Raw type = generic without type argument.

Example

```java
List list = new ArrayList();
```

instead of

```java
List<String> list =
    new ArrayList<>();
```

Raw types disable generic checking.

Example

```java
List list =
    new ArrayList();

list.add("Java");

list.add(10);

String s =
    (String) list.get(1);
```

Runtime failure.

Compiler only gives warnings.

Not errors.

---

Mixing raw and generic

```java
List<String> list =
    new ArrayList<>();

List raw = list;

raw.add(5);

String s =
    list.get(0);
```

Compiles.

Dangerous.

---

Oracle likes asking

Compile?

Runtime?

Warning?

Raw types usually produce

```
Unchecked warning
```

NOT compile error.

---

# 8. Generic Bounds ⭐⭐⭐⭐⭐

Upper bounds

```
extends
```

Lower bounds

```
super
```

---

## Upper Bound

```java
class Box<T extends Number>{
}
```

Allowed

```java
Box<Integer>

Box<Double>

Box<Long>
```

Not allowed

```java
Box<String>
```

Compile error.

---

Upper bound also works with interfaces

```java
<T extends Comparable<T>>
```

---

Multiple bounds

```java
<T extends Number & Comparable<T>>
```

Class first.

Interfaces after.

Correct

```java
<T extends Animal
        & Runnable
        & Serializable>
```

Wrong

```java
<T extends Runnable
      & Animal>
```

Class cannot come after interfaces.

Oracle likes this.

---

# 9. Wildcards

Unknown type

```
?
```

Example

```java
List<?> list;
```

Means

```
List of something.
```

Unknown.

---

Allowed

```java
List<String>

List<Integer>

List<Double>
```

All fit.

---

Cannot add

```java
list.add("Java");
```

Compile error.

Only

```java
list.add(null);
```

works.

---

# 10. Upper Wildcards

```
? extends Number
```

Example

```java
List<? extends Number> list;
```

Can reference

```java
List<Integer>

List<Double>

List<Float>
```

---

Reading

```java
Number n =
    list.get(0);
```

Allowed.

Writing

```java
list.add(5);
```

Compile error.

Compiler doesn't know whether it's

```
Integer

Double

Long
```

---

Remember

```
extends

Read only
```

---

# 11. Lower Wildcards

```
? super Integer
```

Example

```java
List<? super Integer>
```

Can reference

```java
List<Integer>

List<Number>

List<Object>
```

Writing

```java
list.add(5);
```

Allowed.

Reading

```java
Object o =
    list.get(0);
```

Only Object guaranteed.

---

Remember

```
super

Write only
```

---

# 12. PECS ⭐⭐⭐⭐⭐

Oracle LOVES PECS.

Producer

```
extends
```

Consumer

```
super
```

---

Producer

Produces values.

Read.

```java
List<? extends Animal>
```

Can safely read Animals.

Cannot write.

---

Consumer

Consumes values.

Write.

```java
List<? super Dog>
```

Can safely insert Dogs.

Cannot safely read beyond Object.

---

Easy memory trick

```
Producer

Produces data

Read

extends
```

```
Consumer

Consumes data

Write

super
```

---

# 13. Type Erasure ⭐⭐⭐⭐⭐

Generics exist only during compilation.

After compilation

```
List<String>

List<Integer>
```

both become

```
List
```

at runtime.

Example

```java
List<String>

List<Integer>
```

Same runtime class.

```java
System.out.println(
list1.getClass()==list2.getClass());
```

prints

```
true
```

---

Because of erasure

Illegal

```java
if(obj instanceof List<String>)
```

Compile error.

Allowed

```java
obj instanceof List
```

---

Cannot create

```java
new T()
```

Compile error.

Cannot

```java
new T[5]
```

Compile error.

---

Cannot overload

```java
void test(List<String>)

void test(List<Integer>)
```

Compile error.

After erasure both become

```
test(List)
```

---

# 14. Common Oracle Traps

## Trap 1

```java
List<Object> list =
        new ArrayList<String>();
```

Compile?

❌ No.

Generics are invariant.

---

## Trap 2

```java
List<?> list =
        new ArrayList<String>();

list.add("Java");
```

Compile?

❌ No.

---

## Trap 3

```java
List<? extends Number> list =
    new ArrayList<Integer>();

list.add(5);
```

Compile?

❌ No.

---

## Trap 4

```java
List<? super Integer> list =
    new ArrayList<Number>();

list.add(10);
```

✔ Compiles.

---

## Trap 5

```java
List raw =
    new ArrayList<String>();

raw.add(100);
```

Compiles?

✔ Yes.

Warning only.

---

## Trap 6

```java
List<String> list =
    new ArrayList();

```

Compiles?

✔ Yes.

Unchecked warning.

---

## Trap 7

```java
var list =
    new ArrayList<>();
```

Type?

```
ArrayList<Object>
```

---

# Oracle-Style Questions

---

## Question 1

```java
List<? extends Number> list =
    new ArrayList<Integer>();

list.add(5);

System.out.println(list.size());
```

A. Prints 1

B. Prints 0

C. Runtime exception

D. Does not compile

## Answer

✅ D

Explanation:

`? extends` is read-only. You cannot safely add elements.

---

## Question 2

```java
List<? super Integer> list =
    new ArrayList<Number>();

list.add(5);

Object obj =
    list.get(0);

System.out.println(obj);
```

A. Compile error

B. Runtime exception

C. Prints 5

D. Unknown

## Answer

✅ C

Reading from `? super` only guarantees `Object`.

---

## Question 3

```java
static <T> T id(T value){
    return value;
}

public static void main(String[] args){

    var x = id(5);

    System.out.println(x.getClass());
}
```

A. int

B. Integer

C. Object

D. Number

## Answer

✅ B

Autoboxing infers `T` as `Integer`.

---

## Question 4

```java
List<String> a =
        new ArrayList<>();

List<Integer> b =
        new ArrayList<>();

System.out.println(
a.getClass()==b.getClass());
```

A. false

B. true

C. Compile error

D. Runtime error

## Answer

✅ B

Type erasure removes generic information.

---

## Question 5

Which declaration is legal?

A.

```java
<T extends Runnable & Animal>
```

B.

```java
<T extends Animal & Runnable>
```

C.

```java
<T super Number>
```

D.

```java
<T implements Runnable>
```

## Answer

✅ B

Class first.

Interfaces after.

---

## Question 6

```java
List<?> list =
    new ArrayList<String>();

list.add(null);

System.out.println(list.size());
```

A. Compile error

B. Prints 0

C. Prints 1

D. Runtime exception

## Answer

✅ C

`null` is the only value you can add to a `List<?>`.

---

# High-Probability Exam Facts (Memorize)

| Rule                                                                                          | Must Know |
| --------------------------------------------------------------------------------------------- | --------- |
| Generics provide compile-time type safety                                                     | ✅        |
| Diamond operator (`<>`) enables type inference                                                | ✅        |
| Generic methods declare `<T>` before the return type                                          | ✅        |
| Raw types compile with unchecked warnings                                                     | ✅        |
| `List<Object>` is **not** a supertype of `List<String>`                                       | ✅        |
| `List<?>` accepts any generic type but is effectively read-only (except `null`)               | ✅        |
| `? extends` = producer (read)                                                                 | ✅        |
| `? super` = consumer (write)                                                                  | ✅        |
| Type erasure removes generic information at runtime                                           | ✅        |
| `instanceof List<String>` is illegal                                                          | ✅        |
| `new T()` and `new T[]` are illegal                                                           | ✅        |
| Generic classes are invariant (`List<Dog>` is **not** a `List<Animal>`)                       | ✅        |
| Multiple bounds use `Class & Interface1 & Interface2`; the class, if present, must come first | ✅        |

---

# What Oracle Tests Most Often

From Oracle's past Java certification patterns (OCA/OCP through Java 21), these are the generics topics that appear most frequently:

| Topic                                                         | Probability |
| ------------------------------------------------------------- | ----------: |
| `extends` vs `super` wildcards (PECS)                         |  ⭐⭐⭐⭐⭐ |
| `List<?>` behavior                                            |  ⭐⭐⭐⭐⭐ |
| Raw types and unchecked warnings                              |  ⭐⭐⭐⭐⭐ |
| Generic bounds (`extends Number`, multiple bounds)            |   ⭐⭐⭐⭐☆ |
| Generic method syntax and type inference                      |   ⭐⭐⭐⭐☆ |
| Invariance (`List<Dog>` ≠ `List<Animal>`)                     |  ⭐⭐⭐⭐⭐ |
| Type erasure effects (`instanceof`, overloads, runtime class) |  ⭐⭐⭐⭐⭐ |
| Diamond operator and `var` inference                          |   ⭐⭐⭐⭐☆ |

If you can confidently answer questions involving those topics—especially identifying whether code **compiles**, produces an **unchecked warning**, or fails at **runtime**—you will be well prepared for the Generics portion of the 1Z0-830 exam.
