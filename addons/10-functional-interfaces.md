Absolutely. **Functional Interfaces** are one of Oracle's favorite exam topics because they combine several Java concepts:

- interfaces
- lambda expressions
- method references
- generics
- type inference
- Stream API (which is the next chapter)

Oracle loves asking questions where the code _looks_ correct but fails because of a subtle functional interface rule.

---

# Java SE 21 (1Z0-830)

# Chapter 10 — Functional Interfaces (Complete Exam Guide)

---

# 1. What is a Functional Interface?

A functional interface is an interface that contains **exactly one abstract method**.

It may also contain:

- default methods
- static methods
- private methods (Java 9+)
- methods inherited from Object

Only **one abstract method** is allowed.

Example

```java
interface Printer {
    void print();
}
```

Valid.

---

Also valid

```java
interface Printer {

    void print();

    default void stop() {}

    static void reset() {}

    private void helper() {}
}
```

Still only one abstract method.

---

Not valid

```java
interface Printer {

    void print();

    void stop();
}
```

Two abstract methods.

Not functional.

---

# @FunctionalInterface

This annotation is optional.

```java
@FunctionalInterface
interface Printer {
    void print();
}
```

If someone later adds another abstract method

```java
void stop();
```

Compiler error.

Oracle likes this.

---

# Object methods DO NOT count

Example

```java
interface A {

    void run();

    String toString();
}
```

Still functional.

Why?

Because `toString()` already exists in `Object`.

Likewise

```
equals()

hashCode()

toString()
```

do not increase the abstract method count.

---

# Oracle Favorite

Is this functional?

```java
interface Test {

    void a();

    default void b(){}

    static void c(){}

    private void d(){}

    String toString();
}
```

Answer:

Yes.

Only one abstract method (`a()`).

---

# Built-in Functional Interfaces

The exam heavily focuses on these.

Know them by heart.

| Interface         | Input | Output  |
| ----------------- | ----- | ------- |
| Predicate<T>      | T     | boolean |
| Function<T,R>     | T     | R       |
| Consumer<T>       | T     | void    |
| Supplier<T>       | none  | T       |
| UnaryOperator<T>  | T     | T       |
| BinaryOperator<T> | T,T   | T       |

Memorize this table.

---

# Predicate

Represents a boolean test.

Method

```java
boolean test(T t)
```

Example

```java
Predicate<Integer> even =
        n -> n % 2 == 0;

System.out.println(even.test(4));
```

Output

```
true
```

---

Useful methods

```
and()

or()

negate()

isEqual()
```

Example

```java
Predicate<Integer> positive =
        n -> n > 0;

Predicate<Integer> even =
        n -> n % 2 == 0;

Predicate<Integer> both =
        positive.and(even);

System.out.println(both.test(8));
```

Output

```
true
```

---

Another

```java
Predicate<String> p =
        s -> s.startsWith("A");

System.out.println(
        p.negate().test("Bob"));
```

Output

```
true
```

---

# Oracle Favorite

What prints?

```java
Predicate<String> p =
        s -> s.length() > 3;

System.out.println(
        p.test("Cat"));
```

Answer

```
false
```

---

# Function

Transforms one value into another.

Method

```java
R apply(T t)
```

Example

```java
Function<String,Integer> f =
        s -> s.length();

System.out.println(
        f.apply("Java"));
```

Output

```
4
```

---

Useful methods

```
compose()

andThen()

identity()
```

---

compose()

Runs second first.

```java
Function<Integer,Integer> times2 =
        x -> x * 2;

Function<Integer,Integer> plus1 =
        x -> x + 1;

System.out.println(
        times2.compose(plus1).apply(5));
```

Execution

```
plus1(5)=6

times2(6)=12
```

Output

```
12
```

---

andThen()

Runs current first.

```java
times2.andThen(plus1)
```

Execution

```
times2(5)=10

plus1(10)=11
```

Output

```
11
```

Oracle asks this often.

---

identity()

Returns the input unchanged.

```java
Function<String,String> id =
        Function.identity();

System.out.println(id.apply("Java"));
```

Output

```
Java
```

---

# Consumer

Consumes data.

Returns nothing.

Method

```java
void accept(T t)
```

Example

```java
Consumer<String> c =
        s -> System.out.println(s);

c.accept("Hello");
```

Output

```
Hello
```

---

Useful method

```
andThen()
```

Example

```java
Consumer<String> c1 =
        System.out::print;

Consumer<String> c2 =
        s -> System.out.println("!");

c1.andThen(c2).accept("Hi");
```

Output

```
HiHi!
```

Notice that `c1` prints `Hi` without a newline, then `c2` prints `Hi!`.

---

# Supplier

Produces values.

No parameters.

Method

```java
T get()
```

Example

```java
Supplier<Double> s =
        Math::random;

System.out.println(s.get());
```

---

Another

```java
Supplier<List<String>> list =
        ArrayList::new;
```

Very common.

---

# UnaryOperator

Special Function

Input and output are the same type.

```
T -> T
```

Equivalent to

```java
Function<T,T>
```

Method

```java
apply()
```

Example

```java
UnaryOperator<Integer> square =
        x -> x * x;

System.out.println(square.apply(5));
```

Output

```
25
```

---

# BinaryOperator

Takes two parameters.

Returns same type.

```
(T,T) -> T
```

Equivalent to

```
BiFunction<T,T,T>
```

Example

```java
BinaryOperator<Integer> add =
        (a,b) -> a+b;

System.out.println(add.apply(2,3));
```

Output

```
5
```

---

Useful methods

```
minBy()

maxBy()
```

Example

```java
BinaryOperator<Integer> max =
    BinaryOperator.maxBy(Integer::compareTo);
```

---

# Method References

Oracle asks these constantly.

---

There are four kinds.

---

## 1. Static Method

Syntax

```java
Class::staticMethod
```

Example

```java
Function<String,Integer> f =
        Integer::parseInt;
```

Equivalent

```java
s -> Integer.parseInt(s)
```

---

## 2. Instance Method of Particular Object

```java
object::method
```

Example

```java
String name="Java";

Supplier<String> s =
        name::toUpperCase;
```

Equivalent

```java
()->name.toUpperCase()
```

---

## 3. Instance Method of Arbitrary Object

This is Oracle's favorite.

Syntax

```java
Class::instanceMethod
```

Example

```java
Function<String,Integer> f =
        String::length;
```

Equivalent

```java
s -> s.length()
```

The object becomes the first parameter.

Another

```java
Predicate<String> p =
        String::isEmpty;
```

Equivalent

```java
s -> s.isEmpty()
```

---

## 4. Constructor Reference

Syntax

```java
Class::new
```

Example

```java
Supplier<ArrayList<String>> s =
        ArrayList::new;
```

Equivalent

```java
()->new ArrayList<>();
```

---

Parameterized constructor

```java
Function<Integer,ArrayList<String>> f =
        ArrayList::new;
```

Equivalent

```java
size -> new ArrayList<>(size)
```

---

# Oracle Favorite

```java
Supplier<StringBuilder> sb =
        StringBuilder::new;
```

Valid.

---

# Lambda Syntax

Single parameter

```java
x -> x+1
```

---

Multiple

```java
(x,y)->x+y
```

---

Block

```java
x -> {

    System.out.println(x);

    return x+1;

}
```

---

No parameter

```java
()->5
```

---

# Type Inference

Compiler infers parameter types.

```java
Function<String,Integer> f =
        s -> s.length();
```

Equivalent

```java
(String s)->s.length()
```

---

Cannot mix

Invalid

```java
(var x, int y)->x+y
```

Either use `var` for all parameters or omit it for all.

---

# Variable Capture

Lambdas can access local variables only if they are **final or effectively final**.

```java
int x=10;

Supplier<Integer> s =
        ()->x;
```

Valid.

---

Invalid

```java
int x=10;

x++;

Supplier<Integer> s =
        ()->x;
```

Compilation error.

Not effectively final.

Oracle loves this.

---

Instance fields

No restriction.

```java
class Test{

    int x=10;

    void run(){

        Supplier<Integer> s =
                ()->x;

        x++;

    }

}
```

Valid.

---

# Functional Interface Assignment

Valid

```java
Predicate<String> p =
        String::isEmpty;
```

---

Valid

```java
Function<String,Integer> f =
        String::length;
```

---

Invalid

```java
Predicate<String> p =
        String::length;
```

Why?

Predicate expects boolean.

length returns int.

---

# Common Oracle Traps

---

## Trap 1

```java
Consumer<String> c =
        String::length;
```

Compilation error.

Consumer returns void.

---

## Trap 2

```java
Supplier<Integer> s =
        Integer::parseInt;
```

Compilation error.

parseInt needs an argument.

Supplier provides none.

---

## Trap 3

```java
Function<String,Integer> f =
        Integer::parseInt;
```

Valid.

---

## Trap 4

```java
Predicate<String> p =
        String::isEmpty;
```

Valid.

---

## Trap 5

```java
Supplier<List<String>> s =
        ArrayList::new;
```

Valid.

---

## Trap 6

```java
Function<Integer,List<String>> f =
        ArrayList::new;
```

Valid.

Calls constructor with initial capacity.

---

## Trap 7

```java
Function<String,String> f =
        String::toUpperCase;
```

Valid.

---

## Trap 8

```java
Consumer<String> c =
        System.out::println;
```

Valid.

---

# Exam Tips

Know these instantly:

| Interface      | Method   |
| -------------- | -------- |
| Predicate      | test()   |
| Function       | apply()  |
| Consumer       | accept() |
| Supplier       | get()    |
| UnaryOperator  | apply()  |
| BinaryOperator | apply()  |

---

Know these chains:

```
Predicate

and()

or()

negate()
```

```
Function

compose()

andThen()

identity()
```

```
Consumer

andThen()
```

---

Know every method reference type.

Oracle asks them constantly.

---

# Oracle-Style Practice Questions

### Question 1

What is the output?

```java
Predicate<Integer> p = n -> n > 5;

System.out.println(p.test(5));
```

A. true

B. false

C. Compilation error

D. Runtime exception

<details>
<summary><strong>Answer</strong></summary>

**B. false**

`5 > 5` is `false`.

</details>

---

### Question 2

What is the output?

```java
Function<String, Integer> f = String::length;

System.out.println(f.apply("Oracle"));
```

A. 6

B. 7

C. Compilation error

D. Runtime exception

<details>
<summary><strong>Answer</strong></summary>

**A. 6**

`"Oracle"` has six characters.

</details>

---

### Question 3 (Oracle Favorite)

```java
Function<Integer, Integer> f1 = x -> x * 2;
Function<Integer, Integer> f2 = x -> x + 3;

System.out.println(f1.compose(f2).apply(4));
```

A. 8

B. 11

C. 14

D. Compilation error

<details>
<summary><strong>Answer</strong></summary>

**C. 14**

`compose()` runs the argument first:

- `f2(4) = 7`
- `f1(7) = 14`

</details>

---

### Question 4

Which declaration compiles?

A.

```java
Supplier<Integer> s = Integer::parseInt;
```

B.

```java
Function<String, Integer> f = Integer::parseInt;
```

C.

```java
Consumer<String> c = String::length;
```

D.

```java
Predicate<String> p = String::length;
```

<details>
<summary><strong>Answer</strong></summary>

**B**

- A ❌ `Supplier` takes no arguments.
- B ✅ `parseInt(String)` matches `Function<String, Integer>`.
- C ❌ `Consumer` requires `void`.
- D ❌ `Predicate` requires `boolean`.

</details>

---

### Question 5 (Certification Trap)

What happens?

```java
int x = 10;

Supplier<Integer> s = () -> x;

x++;

System.out.println(s.get());
```

A. 10

B. 11

C. Compilation error

D. Runtime exception

<details>
<summary><strong>Answer</strong></summary>

**C. Compilation error**

`x` is modified after being captured, so it is **not effectively final**.

</details>

---

# High-Probability Exam Checklist

Before taking the exam, make sure you can answer these without hesitation:

- ✅ Define a functional interface and identify valid/invalid ones.
- ✅ Know that `Object` methods (`toString`, `equals`, `hashCode`) do not count toward the single abstract method rule.
- ✅ Memorize the six core functional interfaces and their abstract methods.
- ✅ Distinguish `Predicate`, `Function`, `Consumer`, `Supplier`, `UnaryOperator`, and `BinaryOperator`.
- ✅ Understand `compose()` vs. `andThen()` for `Function`.
- ✅ Use `and()`, `or()`, and `negate()` with `Predicate`.
- ✅ Recognize all four kinds of method references.
- ✅ Match method references to compatible functional interface signatures.
- ✅ Understand constructor references, including parameterized constructors.
- ✅ Remember that lambdas may capture only **final or effectively final** local variables.
- ✅ Recognize when instance fields can be modified inside or after a lambda.
- ✅ Be comfortable reading lambda expressions with omitted parameter types due to type inference.
- ✅ Spot incompatible assignments between functional interfaces and method references.

If you are comfortable with everything above, you'll be very well prepared for the **Functional Interfaces** portion of the Java SE 21 Developer Professional (1Z0-830) exam.
