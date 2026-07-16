# Java SE 21 Developer Professional (1Z0-830)

# Oracle-Style Mock Exam – Streams, Lambdas & Functional Interfaces

This mock exam is designed to resemble Oracle's style:

- ✅ Code-heavy questions
- ✅ Multiple correct answers where appropriate
- ✅ Oracle certification traps
- ✅ Questions testing compilation vs runtime behavior
- ✅ High-probability topics
- ✅ Explanations after every answer

---

# Question 1

What is the output?

```java
Predicate<String> p = s -> s.length() > 3;

System.out.println(p.test("Java"));
System.out.println(p.test("Go"));
```

A)

```
true
false
```

B)

```
false
true
```

C) Compilation error

D) Runtime exception

---

## Answer

✅ **A**

### Explanation

`Predicate<T>`

```java
boolean test(T t)
```

returns a boolean.

```
"Java".length() = 4 -> true
"Go".length() = 2 -> false
```

---

# Question 2

Which functional interface matches the lambda?

```java
x -> x * 2
```

Choose two.

A)

```java
Function<Integer,Integer>
```

B)

```java
UnaryOperator<Integer>
```

C)

```java
Consumer<Integer>
```

D)

```java
Supplier<Integer>
```

---

## Answer

✅ A
✅ B

### Explanation

The lambda

```java
x -> x * 2
```

takes one argument and returns one value.

Both satisfy:

```
Integer -> Integer
```

UnaryOperator is simply

```java
Function<T,T>
```

where input and output are the same type.

---

# Question 3

Which line does **NOT** compile?

A)

```java
Predicate<String> p = s -> s.isEmpty();
```

B)

```java
Consumer<String> c = s -> System.out.println(s);
```

C)

```java
Supplier<String> s = () -> "Java";
```

D)

```java
Function<String> f = s -> s.length();
```

---

## Answer

✅ D

### Explanation

Function requires

```java
Function<T,R>
```

Two generic types.

Correct version:

```java
Function<String,Integer> f = s -> s.length();
```

Very common Oracle trap.

---

# Question 4

What is printed?

```java
List<Integer> list =
    List.of(1,2,3,4);

list.stream()
    .filter(i -> i % 2 == 0)
    .forEach(System.out::print);
```

A)

```
24
```

B)

```
13
```

C)

```
1234
```

D)

Compilation error

---

## Answer

✅ A

---

# Question 5

Which operations are intermediate?

Choose all.

A)

```
map()
```

B)

```
filter()
```

C)

```
collect()
```

D)

```
sorted()
```

E)

```
reduce()
```

---

## Answer

✅ A
✅ B
✅ D

### Explanation

Intermediate:

- map
- filter
- sorted
- distinct
- limit
- skip
- flatMap
- peek

Terminal:

- collect
- reduce
- forEach
- count
- findFirst
- anyMatch

Oracle loves this distinction.

---

# Question 6

What is the output?

```java
Stream.of("a","bb","ccc")
      .map(String::length)
      .forEach(System.out::print);
```

A)

```
123
```

B)

```
abbccc
```

C)

Compilation error

D)

Runtime exception

---

## Answer

✅ A

---

# Question 7

What happens?

```java
Stream<String> stream =
    Stream.of("A","B");

stream.forEach(System.out::print);

stream.forEach(System.out::print);
```

A)

```
ABAB
```

B)

```
AB
```

C)

Runtime exception

D)

Compilation error

---

## Answer

✅ C

### Explanation

A Stream can only be consumed once.

Second terminal operation throws

```
IllegalStateException
```

Very common Oracle favorite.

---

# Question 8

What is printed?

```java
List<String> names =
    List.of("Bob","Kate","Tom");

System.out.println(

names.stream()
     .filter(s -> s.length() > 3)
     .count()

);
```

A)

0

B)

1

C)

2

D)

3

---

## Answer

✅ B

Only

```
Kate
```

has length greater than 3.

---

# Question 9

What is the output?

```java
List<Integer> list =
    List.of(1,2,3);

list.stream()
    .map(i -> i * i)
    .forEach(System.out::print);
```

A)

149

B)

123

C)

369

D)

Compilation error

---

## Answer

✅ A

Squares

```
1
4
9
```

---

# Question 10

Which statement about `peek()` is true?

A)

Terminal operation

B)

Intermediate operation

C)

Always executes immediately

D)

Cannot modify objects

---

## Answer

✅ B

### Explanation

`peek()` is mainly intended for debugging.

It executes only when a terminal operation exists.

---

# Question 11

What is printed?

```java
Stream.of(5,3,2,4)
      .sorted()
      .forEach(System.out::print);
```

A)

5324

B)

2345

C)

5432

D)

Compilation error

---

## Answer

✅ B

---

# Question 12

What is printed?

```java
Stream.of(1,2,3,4,5)
      .limit(3)
      .forEach(System.out::print);
```

A)

123

B)

345

C)

12345

D)

Compilation error

---

## Answer

✅ A

---

# Question 13

What is printed?

```java
Stream.of(1,2,3,4)
      .skip(2)
      .forEach(System.out::print);
```

A)

1234

B)

34

C)

12

D)

Compilation error

---

## Answer

✅ B

---

# Question 14

What is printed?

```java
List<Integer> list =
    List.of(1,2,2,3,3,3);

list.stream()
    .distinct()
    .forEach(System.out::print);
```

A)

122333

B)

123

C)

321

D)

Compilation error

---

## Answer

✅ B

---

# Question 15

What is printed?

```java
Stream.of("a","bb","ccc")
      .map(String::toUpperCase)
      .forEach(System.out::print);
```

A)

ABBCCC

B)

abbccc

C)

ABC

D)

Compilation error

---

## Answer

✅ A

---

# Question 16

What is the result?

```java
Optional<String> opt =
    Optional.of("Java");

System.out.println(opt.get());
```

A)

Java

B)

Optional[Java]

C)

null

D)

Exception

---

## Answer

✅ A

---

# Question 17

What happens?

```java
Optional<String> opt =
    Optional.empty();

System.out.println(opt.get());
```

A)

null

B)

empty

C)

NoSuchElementException

D)

Compilation error

---

## Answer

✅ C

Oracle asks this frequently.

---

# Question 18

What is printed?

```java
Optional<String> opt =
    Optional.empty();

System.out.println(
    opt.orElse("Default")
);
```

A)

Default

B)

null

C)

Exception

D)

empty

---

## Answer

✅ A

---

# Question 19

What is printed?

```java
System.out.println(

Stream.of(1,2,3,4)
      .reduce(0, Integer::sum)

);
```

A)

10

B)

24

C)

1234

D)

Compilation error

---

## Answer

✅ A

---

# Question 20

What is printed?

```java
List<String> list =
    List.of("A","B","C");

System.out.println(

list.stream()
    .collect(Collectors.joining("-"))

);
```

A)

ABC

B)

A-B-C

C)

[A,B,C]

D)

Compilation error

---

## Answer

✅ B

---

# Question 21

What is the output?

```java
List<List<String>> list =
    List.of(
        List.of("A","B"),
        List.of("C","D")
    );

list.stream()
    .flatMap(List::stream)
    .forEach(System.out::print);
```

A)

ABCD

B)

[A, B][C, D]

C)

Compilation error

D)

Runtime exception

---

## Answer

✅ A

### Explanation

`flatMap()` flattens nested streams:

```
[A,B]
[C,D]

↓

A B C D
```

Oracle often asks about the difference between `map()` and `flatMap()`.

---

# Question 22

Which statement about lazy evaluation is true?

A) `filter()` executes immediately.

B) `map()` executes immediately.

C) Intermediate operations execute only when a terminal operation is invoked.

D) Streams evaluate eagerly.

---

## Answer

✅ C

### Explanation

A stream pipeline is **lazy**.

Nothing happens until a terminal operation such as:

```java
collect()
count()
forEach()
reduce()
```

is called.

---

# Question 23

What is the output?

```java
Stream.of(1,2,3)
      .peek(System.out::print);
```

A)

123

B)

Nothing

C)

Compilation error

D)

Runtime exception

---

## Answer

✅ B

### Explanation

There is **no terminal operation**.

`peek()` never executes by itself.

This is a classic Oracle trap.

---

# Question 24

Which method reference is equivalent to this lambda?

```java
s -> s.length()
```

A)

```java
String::length
```

B)

```java
String.length
```

C)

```java
length::String
```

D)

```java
String::getLength
```

---

## Answer

✅ A

---

# Question 25

Which primitive stream is most appropriate for this code?

```java
Stream.of(1,2,3)
```

Choose one.

A)

DoubleStream

B)

LongStream

C)

IntStream

D)

Stream<Integer> is always required

---

## Answer

✅ C

### Explanation

Primitive streams avoid boxing and unboxing:

```
IntStream
LongStream
DoubleStream
```

They provide methods such as:

```java
sum()
average()
max()
min()
```

without wrapper objects.

---

# ⭐ High-Probability Oracle Exam Traps

These are among the most frequently tested concepts on the **1Z0-830** exam:

| Topic                                                                                    | Very Likely |
| ---------------------------------------------------------------------------------------- | ----------- |
| Functional interface method signatures (`Predicate`, `Function`, `Consumer`, `Supplier`) | ⭐⭐⭐⭐⭐  |
| `UnaryOperator` vs `Function`                                                            | ⭐⭐⭐⭐    |
| Method references                                                                        | ⭐⭐⭐⭐⭐  |
| Intermediate vs terminal operations                                                      | ⭐⭐⭐⭐⭐  |
| Stream reuse (`IllegalStateException`)                                                   | ⭐⭐⭐⭐⭐  |
| Lazy evaluation                                                                          | ⭐⭐⭐⭐⭐  |
| `map()` vs `flatMap()`                                                                   | ⭐⭐⭐⭐⭐  |
| `peek()` behavior                                                                        | ⭐⭐⭐⭐    |
| `Optional.get()` on empty                                                                | ⭐⭐⭐⭐⭐  |
| `orElse()` vs `orElseGet()`                                                              | ⭐⭐⭐⭐    |
| `Collectors.joining()`, `toList()`, `toSet()`, `groupingBy()`                            | ⭐⭐⭐⭐⭐  |
| `reduce()` overloads                                                                     | ⭐⭐⭐⭐    |
| Primitive streams (`IntStream`, `LongStream`, `DoubleStream`)                            | ⭐⭐⭐⭐    |
| Compilation errors from incorrect generic parameters                                     | ⭐⭐⭐⭐⭐  |

If you can answer these 25 questions confidently and explain _why_ each answer is correct, you'll have covered the core Streams, Lambdas, Functional Interfaces, and Optional concepts that Oracle emphasizes on the Java SE 21 Developer Professional (1Z0-830) exam.
