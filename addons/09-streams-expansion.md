Yes. For the **Java SE 21 Developer Professional (1Z0-830)** exam, I would absolutely expand the Streams section. Oracle likes to test **code reading**, not memorization, so you need to understand what each operation does, whether it is **intermediate or terminal**, what it returns, and common pitfalls.

I would recommend studying the following methods. This list covers essentially everything you're likely to need for the exam without getting into obscure APIs.

---

# 🌊 Complete Stream Study Guide for Java SE 21 (1Z0-830)

---

# 1. Creating Streams ⭐⭐⭐⭐⭐

Know all common ways to create a stream.

## From a Collection

```java
List<String> list = List.of("A", "B", "C");

Stream<String> stream = list.stream();
```

---

## Stream.of()

```java
Stream<Integer> s = Stream.of(1, 2, 3);
```

---

## Arrays.stream()

```java
int[] numbers = {1,2,3};

IntStream stream = Arrays.stream(numbers);
```

---

## Empty Stream

```java
Stream<String> s = Stream.empty();
```

---

## Infinite Streams

```java
Stream.generate(Math::random)
```

```java
Stream.iterate(1, n -> n + 1)
```

Oracle may combine these with `limit()`.

---

# 2. Intermediate Operations ⭐⭐⭐⭐⭐

Intermediate operations return **another Stream**.

Nothing executes until a terminal operation appears.

---

## filter()

Signature

```java
Stream<T> filter(Predicate<? super T>)
```

Keeps elements that satisfy the predicate.

Example

```java
List.of(1,2,3,4)
    .stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

Output

```
2
4
```

### Oracle Trap

`filter()` **does not change the type**.

---

## map()

Transforms each element.

```java
Stream<R> map(Function<? super T, ? extends R>)
```

Example

```java
Stream.of("Java","Oracle")
      .map(String::length)
      .forEach(System.out::println);
```

Output

```
4
6
```

### Oracle Favorite

`map()` changes the type.

```
Stream<String>

↓

map(String::length)

↓

Stream<Integer>
```

---

## flatMap()

One of Oracle's favorites.

Flattens nested streams.

Example

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

Output

```
ABCD
```

Without `flatMap()`

```
Stream<List<String>>
```

With `flatMap()`

```
Stream<String>
```

---

## distinct()

Removes duplicates.

```java
Stream.of(1,2,2,3,3)
      .distinct()
      .forEach(System.out::print);
```

Output

```
123
```

---

## sorted()

Natural ordering.

```java
Stream.of(5,3,1)
      .sorted()
      .forEach(System.out::print);
```

Output

```
135
```

Custom comparator

```java
.sorted(Comparator.reverseOrder())
```

---

## peek()

Mostly debugging.

```java
Stream.of(1,2,3)
      .peek(System.out::println)
      .count();
```

Output

```
1
2
3
```

### Oracle Trap

`peek()` is **intermediate**, not terminal.

---

## limit()

Returns first n elements.

```java
Stream.of(1,2,3,4)
      .limit(2)
```

Result

```
1 2
```

---

## skip()

Skips first n elements.

```java
Stream.of(1,2,3,4)
      .skip(2)
```

Result

```
3 4
```

---

# 3. Terminal Operations ⭐⭐⭐⭐⭐

Terminal operations trigger execution.

---

## forEach()

```java
stream.forEach(System.out::println);
```

Returns

```
void
```

Cannot continue the pipeline.

---

## collect()

Most common collector.

```java
List<String> list =
Stream.of("A","B")
      .collect(Collectors.toList());
```

Also know

```java
Collectors.toSet()

Collectors.toMap()

Collectors.joining(",")

Collectors.counting()

Collectors.groupingBy(...)
```

`groupingBy()` is less common but worth recognizing.

---

## toList()

Since Java 16.

```java
List<String> list =
Stream.of("A","B")
      .toList();
```

### Oracle Favorite

Returns an **unmodifiable** list.

---

## count()

```java
long total =
Stream.of(1,2,3)
      .count();
```

Returns

```
3
```

---

## reduce()

Combines elements into one value.

Example

```java
int sum =
Stream.of(1,2,3,4)
      .reduce(0, Integer::sum);
```

Result

```
10
```

Another example

```java
String text =
Stream.of("A","B","C")
      .reduce("", String::concat);
```

Result

```
ABC
```

Oracle loves this method.

---

## findFirst()

Returns

```java
Optional<T>
```

Example

```java
Optional<String> first =
Stream.of("A","B")
      .findFirst();
```

---

## findAny()

Similar to `findFirst()`.

Mostly important for parallel streams.

Returns

```
Optional<T>
```

---

## anyMatch()

Returns boolean.

```java
boolean b =
Stream.of(1,2,3)
      .anyMatch(x -> x > 2);
```

Result

```
true
```

---

## allMatch()

```java
allMatch(x -> x > 0)
```

---

## noneMatch()

```java
noneMatch(x -> x < 0)
```

---

# 4. Collectors ⭐⭐⭐⭐

Know these.

---

## toList()

```java
.collect(Collectors.toList())
```

---

## toSet()

```java
.collect(Collectors.toSet())
```

Duplicates removed.

---

## joining()

```java
.collect(Collectors.joining(","))
```

Produces

```
A,B,C
```

---

## counting()

```java
.collect(Collectors.counting())
```

Returns

```
Long
```

---

## groupingBy()

Example

```java
.collect(Collectors.groupingBy(String::length))
```

Produces

```
Map<Integer,List<String>>
```

Recognize it even if you don't memorize every variation.

---

# 5. Optional ⭐⭐⭐⭐⭐

Know all of these.

---

## of()

```java
Optional.of(value)
```

Throws

```
NullPointerException
```

if value is null.

---

## ofNullable()

Accepts null.

```java
Optional.ofNullable(value)
```

---

## empty()

```java
Optional.empty()
```

---

## get()

Returns value.

Throws

```
NoSuchElementException
```

if empty.

---

## orElse()

```java
optional.orElse(defaultValue)
```

---

## orElseGet()

```java
optional.orElseGet(() -> create())
```

Oracle likes the difference from `orElse()`.

---

## orElseThrow()

```java
optional.orElseThrow()
```

Throws if empty.

---

## isPresent()

```java
optional.isPresent()
```

---

## isEmpty()

Added in Java 11.

```java
optional.isEmpty()
```

---

## ifPresent()

```java
optional.ifPresent(System.out::println);
```

---

# 6. Primitive Streams ⭐⭐⭐⭐

Know the three primitive stream types.

```
IntStream

LongStream

DoubleStream
```

---

## range()

```java
IntStream.range(1,5)
```

Produces

```
1 2 3 4
```

---

## rangeClosed()

```java
IntStream.rangeClosed(1,5)
```

Produces

```
1 2 3 4 5
```

Oracle asks this difference surprisingly often.

---

## sum()

```java
IntStream.of(1,2,3).sum();
```

---

## average()

Returns

```
OptionalDouble
```

---

## max()

Returns

```
OptionalInt
```

---

## min()

Returns

```
OptionalInt
```

---

# 7. Lazy Evaluation ⭐⭐⭐⭐⭐

One of Oracle's favorite concepts.

```java
Stream.of(1,2,3)
      .filter(x -> {
          System.out.println(x);
          return true;
      });
```

Output

```
nothing
```

Only after

```java
.count();
```

does execution begin.

---

# 8. Stream Pipeline ⭐⭐⭐⭐⭐

Every pipeline has this structure:

```
Source

↓

Intermediate

↓

Intermediate

↓

Intermediate

↓

Terminal
```

Example

```java
List.of("java","oracle","exam")
    .stream()
    .filter(s -> s.length() > 4)
    .map(String::toUpperCase)
    .sorted()
    .forEach(System.out::println);
```

Execution order is **element by element**, not operation by operation. For example, for each element the stream applies `filter`, then `map`, then moves on to the next element, rather than filtering the entire stream first and then mapping the results.

---

# 9. Method References ⭐⭐⭐⭐

Know all four forms.

### Static method

```java
Integer::parseInt
```

---

### Instance method of a particular object

```java
System.out::println
```

---

### Instance method of an arbitrary object of a particular type

```java
String::length
```

---

### Constructor reference

```java
ArrayList::new
```

---

# 🚨 Oracle's Favorite Stream Traps

| Trap                                        | Correct Answer                      |
| ------------------------------------------- | ----------------------------------- |
| Streams are reusable                        | ❌ No                               |
| `peek()` is terminal                        | ❌ No                               |
| `map()` filters                             | ❌ No                               |
| `filter()` changes type                     | ❌ No                               |
| Intermediate operations execute immediately | ❌ No (they're lazy)                |
| `Stream.toList()` returns a mutable list    | ❌ No                               |
| `Optional.of(null)` is valid                | ❌ Throws `NullPointerException`    |
| `Optional.get()` is always safe             | ❌ Throws if empty                  |
| `range(1,5)` includes `5`                   | ❌ No                               |
| `rangeClosed(1,5)` excludes `5`             | ❌ No, it includes `5`              |
| `forEach()` returns a stream                | ❌ It returns `void`                |
| `sorted()` modifies the original collection | ❌ Streams do not modify the source |

---

# 🎯 High-Priority Methods to Memorize

If you memorize one list for the exam, make it this one:

### Stream creation

- `stream()`
- `Stream.of()`
- `Arrays.stream()`
- `Stream.empty()`
- `Stream.generate()`
- `Stream.iterate()`

### Intermediate operations

- `filter()`
- `map()`
- `flatMap()`
- `distinct()`
- `sorted()`
- `peek()`
- `limit()`
- `skip()`

### Terminal operations

- `forEach()`
- `collect()`
- `toList()`
- `count()`
- `reduce()`
- `findFirst()`
- `findAny()`
- `anyMatch()`
- `allMatch()`
- `noneMatch()`

### Collectors

- `Collectors.toList()`
- `Collectors.toSet()`
- `Collectors.toMap()`
- `Collectors.joining()`
- `Collectors.counting()`
- `Collectors.groupingBy()`

### Optional

- `of()`
- `ofNullable()`
- `empty()`
- `get()`
- `orElse()`
- `orElseGet()`
- `orElseThrow()`
- `ifPresent()`
- `isPresent()`
- `isEmpty()`

### Primitive streams

- `IntStream.range()`
- `IntStream.rangeClosed()`
- `sum()`
- `average()`
- `min()`
- `max()`

This collection of APIs covers virtually all of the Stream-related functionality that Oracle commonly targets on the 1Z0-830 exam, and it's enough to confidently solve the vast majority of Stream and Optional questions you'll encounter.
