Absolutely. Based on the Oracle Java SE 21 Developer Professional (1Z0-830) exam objectives and years of Oracle certification patterns, **Streams & Lambdas** is one of the most heavily tested sections. Oracle loves to combine lambdas with collections, generics, method references, `Optional`, and functional interfaces in tricky code-reading questions.

The good news is that **the exam only tests a subset of the Stream API**. You do **not** need to memorize every stream operation. The topics you've listed are exactly the important ones.

---

# 📚 Java SE 21 Developer Professional (1Z0-830)

# Chapter 12 — Streams & Lambdas

---

# Part 1 — Why Lambdas Exist

Before Java 8, if we wanted to pass behavior (code) to a method we usually created an anonymous class.

Example:

```java
button.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Clicked");
    }
});
```

With lambdas:

```java
button.addActionListener(e -> System.out.println("Clicked"));
```

A lambda is simply **an implementation of a functional interface.**

---

# Functional Interfaces

A functional interface contains **exactly ONE abstract method.**

It may contain:

- static methods ✅
- default methods ✅
- private methods ✅
- Object methods (equals/hashCode/toString) ✅

Only one abstract method is allowed.

Example

```java
interface Printer {
    void print();
}
```

Lambda:

```java
Printer p = () -> System.out.println("Hello");
```

---

## Oracle Favorite

This is legal:

```java
interface Test {

    void run();

    default void stop(){}

    static void hello(){}

    private void x(){}

}
```

Still functional.

Only

```java
void run();
```

counts.

---

## Oracle Trap

This is NOT functional.

```java
interface Test{

    void run();

    void stop();

}
```

Two abstract methods.

Compilation error when used as lambda.

---

# Built-in Functional Interfaces

These are extremely important.

Know them by heart.

---

## Predicate<T>

Represents

```
T → boolean
```

Method

```java
boolean test(T t)
```

Example

```java
Predicate<String> p =
    s -> s.length() > 3;

System.out.println(
    p.test("Java")
);
```

Output

```
true
```

---

## Function<T,R>

Represents

```
T → R
```

Method

```java
R apply(T t)
```

Example

```java
Function<String,Integer> f =
    s -> s.length();

System.out.println(
    f.apply("Oracle")
);
```

Output

```
6
```

---

## Consumer<T>

Represents

```
T → void
```

Method

```java
void accept(T t)
```

Example

```java
Consumer<String> c =
    System.out::println;

c.accept("Java");
```

Output

```
Java
```

---

## Supplier<T>

Represents

```
() → T
```

Method

```java
T get()
```

Example

```java
Supplier<Double> s =
    Math::random;

System.out.println(
    s.get()
);
```

---

# Oracle Memory Table

| Interface | Input | Output  |
| --------- | ----- | ------- |
| Predicate | T     | boolean |
| Function  | T     | R       |
| Consumer  | T     | void    |
| Supplier  | none  | T       |

Memorize this table.

---

# Lambda Syntax

General syntax

```java
(parameters) -> expression
```

or

```java
(parameters) -> {
    statements;
}
```

---

Example

```java
x -> x * 2
```

Same as

```java
(int x) -> {
    return x * 2;
}
```

---

## Parentheses Rules

One parameter

```java
x -> x + 1
```

works.

Also

```java
(x) -> x + 1
```

works.

---

Zero parameters

Must use

```java
() -> 5
```

---

Multiple parameters

Must use

```java
(a,b) -> a+b
```

---

# Curly Braces Rule

Without braces

```java
x -> x + 1
```

return is implicit.

With braces

```java
x -> {
    return x+1;
}
```

Must explicitly write return.

---

Oracle Favorite

Wrong

```java
x -> {
    x + 1;
}
```

Compilation error.

---

# Variable Capture

Lambdas may access

- static variables
- instance variables
- effectively final local variables

Example

```java
int x = 10;

Predicate<Integer> p =
    n -> n > x;
```

Works.

---

Now

```java
x++;

Predicate<Integer> p =
    n -> n > x;
```

Compilation error.

Local variables must be effectively final.

---

# Effectively Final

Legal

```java
int x = 10;

Runnable r =
    () -> System.out.println(x);
```

---

Illegal

```java
int x = 10;

x++;

Runnable r =
    () -> System.out.println(x);
```

---

Oracle asks this constantly.

---

# Method References

Alternative syntax.

Instead of

```java
x -> System.out.println(x)
```

write

```java
System.out::println
```

---

Instead of

```java
s -> s.length()
```

write

```java
String::length
```

---

# Streams

A stream is **NOT** a collection.

A stream processes data.

Think

```
Collection

↓

Stream

↓

Operations

↓

Result
```

---

Create stream

```java
List.of(1,2,3)
    .stream();
```

---

# Stream Pipeline

Pipeline consists of

```
Source

↓

Intermediate operations

↓

Terminal operation
```

Example

```java
List.of(1,2,3)

.stream()

.filter(...)

.map(...)

.collect(...)
```

---

# Intermediate Operations

Produce another stream.

Lazy.

Nothing executes yet.

Examples

```
filter()

map()

sorted()

distinct()

peek()
```

---

# Terminal Operations

Produce result.

Trigger execution.

Examples

```
forEach()

collect()

count()

reduce()

toList()

findFirst()

anyMatch()
```

---

Oracle loves asking

"Which operation triggers stream execution?"

Answer:

Only terminal operations.

---

# Lazy Evaluation

Nothing happens until terminal operation.

Example

```java
Stream.of(1,2,3)

.filter(x->{
    System.out.println(x);
    return true;
});
```

Output

Nothing.

---

Now

```java
Stream.of(1,2,3)

.filter(x->{
    System.out.println(x);
    return true;
})

.count();
```

Output

```
1
2
3
```

---

# filter()

Keeps elements matching Predicate.

Example

```java
List.of(1,2,3,4)

.stream()

.filter(x -> x%2==0)

.forEach(System.out::println);
```

Output

```
2
4
```

---

# map()

Transforms elements.

Example

```java
List.of("a","bb","ccc")

.stream()

.map(String::length)

.forEach(System.out::println);
```

Output

```
1
2
3
```

---

Oracle Favorite

`filter()` returns fewer elements.

`map()` changes elements.

---

# forEach()

Terminal.

Consumes every element.

```java
stream.forEach(System.out::println);
```

Returns

```
void
```

Cannot continue pipeline afterward.

---

Wrong

```java
stream.forEach(...)

.filter(...)
```

Compilation error.

---

# collect()

Collects stream into collection.

Example

```java
List<String> list =

Stream.of("A","B")

.collect(Collectors.toList());
```

Oracle also expects you to know that since Java 16:

```java
List<String> list =

Stream.of("A","B")

.toList();
```

is valid.

---

# Oracle Favorite

Difference

```
collect(Collectors.toList())
```

vs

```
toList()
```

`toList()` returns an **unmodifiable** list.

```java
List<String> list =
    Stream.of("A").toList();

list.add("B");
```

Runtime exception:

```
UnsupportedOperationException
```

Oracle likes this trap.

---

# Stream Cannot Be Reused

```java
Stream<Integer> s =
    Stream.of(1,2,3);

s.count();

s.count();
```

Runtime

```
IllegalStateException
```

Very common exam question.

---

# Optional

Represents

```
Value

or

No Value
```

Instead of returning null.

---

Create

```java
Optional<String> o =
Optional.of("Java");
```

---

Empty

```java
Optional<String> o =
Optional.empty();
```

---

Nullable

```java
Optional.ofNullable(value);
```

Works with null.

---

Oracle Favorite

Difference

```java
Optional.of(null);
```

Throws

```
NullPointerException
```

---

But

```java
Optional.ofNullable(null);
```

Produces

```
Optional.empty
```

---

# get()

```java
Optional<String> o =
Optional.of("Java");

o.get();
```

Returns

```
Java
```

---

But

```java
Optional.empty().get();
```

Throws

```
NoSuchElementException
```

---

Oracle asks this frequently.

---

# orElse()

```java
Optional<String> o =
Optional.empty();

System.out.println(

o.orElse("Default")

);
```

Output

```
Default
```

---

# ifPresent()

```java
Optional.of("Java")

.ifPresent(System.out::println);
```

Prints

```
Java
```

---

# isPresent()

```java
if(optional.isPresent())
```

Legal.

Although modern Java often prefers methods like `ifPresent()`, the exam still includes `isPresent()`.

---

# Oracle Favorites Summary

Know these perfectly.

| Topic                     | Frequently Tested |
| ------------------------- | ----------------- |
| Functional interfaces     | ⭐⭐⭐⭐⭐        |
| Predicate vs Function     | ⭐⭐⭐⭐⭐        |
| Lambda syntax             | ⭐⭐⭐⭐⭐        |
| Effective final variables | ⭐⭐⭐⭐⭐        |
| filter()                  | ⭐⭐⭐⭐⭐        |
| map()                     | ⭐⭐⭐⭐⭐        |
| Intermediate vs Terminal  | ⭐⭐⭐⭐⭐        |
| Stream reuse              | ⭐⭐⭐⭐⭐        |
| Optional.of vs ofNullable | ⭐⭐⭐⭐⭐        |
| Optional.get()            | ⭐⭐⭐⭐          |
| toList() vs collect()     | ⭐⭐⭐⭐          |
| Method references         | ⭐⭐⭐⭐          |

---

# 🚨 Certification Traps & Gotchas

These are the mistakes Oracle loves to test.

| Trap                                             | Result                      |
| ------------------------------------------------ | --------------------------- |
| Functional interface with 2 abstract methods     | ❌ Not functional           |
| Lambda modifies local variable                   | ❌ Compilation error        |
| Lambda uses non-effectively-final local variable | ❌ Compilation error        |
| `Optional.of(null)`                              | ❌ `NullPointerException`   |
| `Optional.empty().get()`                         | ❌ `NoSuchElementException` |
| Reuse stream after terminal operation            | ❌ `IllegalStateException`  |
| `forEach()` is terminal                          | Cannot continue pipeline    |
| `filter()` changes type                          | ❌ No, it only filters      |
| `map()` filters elements                         | ❌ No, it transforms        |
| Intermediate operations execute immediately      | ❌ They are lazy            |
| `toList()` returns mutable list                  | ❌ It is unmodifiable       |

---

# Oracle-Style Practice Questions

## Question 1

What is the output?

```java
Predicate<String> p = s -> s.length() > 3;

System.out.println(p.test("Java"));
```

A. Java

B. 4

C. true

D. false

### ✅ Answer

**C**

Explanation:

`"Java"` has length 4, so `4 > 3` is `true`.

---

## Question 2 (Oracle Favorite)

Which functional interface represents **T → R**?

A. Predicate

B. Consumer

C. Supplier

D. Function

### ✅ Answer

**D**

Explanation:

- `Predicate<T>` → `T → boolean`
- `Function<T,R>` → `T → R`
- `Consumer<T>` → `T → void`
- `Supplier<T>` → `() → T`

---

## Question 3 (Certification Trap)

What happens?

```java
int x = 5;

Consumer<Integer> c = n -> System.out.println(n + x);

x++;

c.accept(1);
```

A. Prints 6

B. Prints 7

C. Runtime exception

D. Does not compile

### ✅ Answer

**D**

Explanation:

`x` is modified after being captured by the lambda, so it is no longer effectively final.

---

## Question 4 (Oracle Favorite)

What is the output?

```java
Stream.of("a", "bb", "ccc")
      .map(String::length)
      .filter(n -> n > 1)
      .forEach(System.out::print);
```

A. `123`

B. `23`

C. `12`

D. Compilation error

### ✅ Answer

**B**

Explanation:

Lengths are `1, 2, 3`. The `filter` keeps values greater than 1, so the output is `23`.

---

## Question 5 (Certification Trap)

What happens?

```java
Optional<String> opt = Optional.of(null);
```

A. `Optional.empty`

B. `null`

C. `NoSuchElementException`

D. `NullPointerException`

### ✅ Answer

**D**

Explanation:

`Optional.of()` does not allow `null`. Use `Optional.ofNullable()` when the value may be `null`.

---

## Question 6 (Oracle Favorite)

What happens?

```java
Stream<Integer> s = Stream.of(1, 2, 3);

System.out.println(s.count());

System.out.println(s.count());
```

A. `3 3`

B. `3 0`

C. Compilation error

D. `IllegalStateException`

### ✅ Answer

**D**

Explanation:

A stream can only be consumed once. After a terminal operation such as `count()`, the stream is closed.

---

## Question 7

Which of the following are **intermediate** operations? _(Choose all that apply.)_

A. `filter()`

B. `map()`

C. `collect()`

D. `forEach()`

### ✅ Answer

**A and B**

Explanation:

`filter()` and `map()` return a new stream. `collect()` and `forEach()` are terminal operations that trigger execution.

---

## Question 8 (Oracle Favorite)

What is the output?

```java
List<String> list =
    Stream.of("A", "B").toList();

list.add("C");

System.out.println(list);
```

A. `[A, B, C]`

B. `[A, B]`

C. `UnsupportedOperationException`

D. Compilation error

### ✅ Answer

**C**

Explanation:

`Stream.toList()` returns an **unmodifiable** list. Calling `add()` throws an `UnsupportedOperationException`.

---

# 🎯 Final Exam Checklist

Before taking the 1Z0-830 exam, make sure you can answer these questions without hesitation:

- ✅ What is a functional interface?
- ✅ Which built-in functional interface should you use (`Predicate`, `Function`, `Consumer`, `Supplier`)?
- ✅ Can you recognize valid and invalid lambda syntax?
- ✅ What does "effectively final" mean?
- ✅ What is the difference between `filter()` and `map()`?
- ✅ Which operations are intermediate and which are terminal?
- ✅ Why are streams considered lazy?
- ✅ Why can't a stream be reused?
- ✅ What is the difference between `Optional.of()` and `Optional.ofNullable()`?
- ✅ What happens when calling `Optional.get()` on an empty `Optional`?
- ✅ What is the difference between `Stream.toList()` and `Collectors.toList()`?

If you're comfortable with all of the above, you'll have covered the Stream & Lambda material that Oracle most commonly targets on the Java SE 21 Developer Professional (1Z0-830) exam.
