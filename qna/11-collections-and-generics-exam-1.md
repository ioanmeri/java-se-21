Absolutely. Based on Oracle's question style, the **1Z0-830** exam heavily favors:

- Reading code rather than writing it.
- "Choose two/three" questions.
- Compilation vs runtime behavior.
- Generics invariance (`List<String>` is **not** `List<Object>`).
- PECS.
- Type inference.
- TreeSet/TreeMap ordering.
- Comparable vs Comparator.
- Queue/Deque methods.
- Immutable collection factory methods.
- Autoboxing traps (`==`, `remove()`, null unboxing).
- Type erasure.
- Raw types warnings.
- Iterator behavior.
- Oracle loves asking about **which line causes compilation failure**.

---

# Java SE 21 Developer Professional (1Z0-830)

# Mock Exam — Collections & Generics

**30 Oracle-style questions**

---

# Question 1

What is the output?

```java
List<String> list = new ArrayList<>();

list.add("A");
list.add("B");
list.add(1, "C");

System.out.println(list);
```

A.

```
[A, B, C]
```

B.

```
[A, C, B]
```

C. Compilation fails

D. Exception at runtime

---

## Answer

✅ **B**

### Explanation

`add(index, element)` inserts.

```
[A, B]

↓

[A, C, B]
```

---

# Question 2

Which collections allow duplicate elements?

Choose two.

A. HashSet

B. ArrayList

C. TreeSet

D. LinkedList

---

## Answer

✅ **B and D**

### Explanation

Lists allow duplicates.

Sets never do.

---

# Question 3

What is printed?

```java
Set<Integer> set = new HashSet<>();

set.add(3);
set.add(3);
set.add(2);

System.out.println(set.size());
```

A. 1

B. 2

C. 3

D. Compilation fails

---

## Answer

✅ **B**

Duplicate values are ignored.

---

# Question 4

Which statement is true?

```java
List<String> a = new ArrayList<>();
List<Object> b = a;
```

A. Compiles

B. Runtime exception

C. Compilation fails

D. Depends on JVM

---

## Answer

✅ **C**

Generics are invariant.

```
List<String>

≠

List<Object>
```

Huge Oracle favorite.

---

# Question 5

What is printed?

```java
List<Integer> list = new ArrayList<>();

list.add(10);
list.add(20);

for (Integer i : list)
    System.out.print(i);
```

A.

```
1020
```

B.

```
10 20
```

C. Compilation fails

D. Runtime exception

---

## Answer

✅ **A**

No spaces.

---

# Question 6

Which statement about enhanced for-loops is true?

A. They work only for arrays.

B. They require Iterator explicitly.

C. They work for arrays and Iterable.

D. They work only for Lists.

---

## Answer

✅ **C**

---

# Question 7

What happens?

```java
List<Integer> list = new ArrayList<>();

list.add(1);
list.add(2);

list.remove(1);

System.out.println(list);
```

A.

```
[1]
```

B.

```
[2]
```

C.

```
[1,2]
```

D. Compilation fails

---

## Answer

✅ **A**

Oracle favorite.

`remove(int)` removes **index**.

Not object.

---

# Question 8

What removes the Integer value 1?

Choose one.

A.

```java
list.remove(1);
```

B.

```java
list.remove(Integer.valueOf(1));
```

C.

```java
list.delete(1);
```

D.

```java
list.removeObject(1);
```

---

## Answer

✅ **B**

---

# Question 9

What happens?

```java
Integer x = null;

int y = x;
```

A. 0

B. null

C. Compilation fails

D. NullPointerException

---

## Answer

✅ **D**

Automatic unboxing.

---

# Question 10

What is printed?

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

A. true

B. false

C. Compilation fails

D. Runtime exception

---

## Answer

✅ **A**

Integer cache.

Oracle likes this.

---

# Question 11

And now?

```java
Integer a = 200;
Integer b = 200;

System.out.println(a == b);
```

A. true

B. false

C. Compilation fails

D. Depends

---

## Answer

✅ **B**

Outside Integer cache.

---

# Question 12

Which declarations compile?

Choose two.

A.

```java
List<?> list;
```

B.

```java
List<Object> list = new ArrayList<String>();
```

C.

```java
List<? extends Number> nums;
```

D.

```java
List<int> list;
```

---

## Answer

✅ **A and C**

---

# Question 13

Given

```java
List<? extends Number> nums;
```

Which operation is allowed?

A.

```java
nums.add(5);
```

B.

```java
Number n = nums.get(0);
```

C.

```java
nums.add(null);
```

D. Both B and C

---

## Answer

✅ **D**

PECS.

Producer → read only.

Only `null` may be added.

---

# Question 14

Given

```java
List<? super Integer> list;
```

Which statement is true?

A. Reading returns Integer.

B. Can safely add Integer.

C. Cannot add Integer.

D. Reading returns Number.

---

## Answer

✅ **B**

Consumer Super.

---

# Question 15

PECS stands for

A.

Producer Extends Consumer Super

B.

Producer Super Consumer Extends

C.

Private Extends Collection Super

D.

Producer Equals Consumer Same

---

## Answer

✅ **A**

---

# Question 16

Which generic method is correct?

A.

```java
<T> void test(T t){}
```

B.

```java
void <T> test(T t){}
```

C.

```java
void test<T>(T t){}
```

D.

```java
<T extends> void test(){}
```

---

## Answer

✅ **A**

---

# Question 17

What is inferred?

```java
var list = List.of(1,2,3);
```

Type?

A.

```
List<Object>
```

B.

```
List<Integer>
```

C.

```
ArrayList<Integer>
```

D.

```
Collection<Integer>
```

---

## Answer

✅ **B**

---

# Question 18

What happens?

```java
var list = List.of(1,2,3);

list.add(4);
```

A. Works

B. UnsupportedOperationException

C. Compilation fails

D. IllegalArgumentException

---

## Answer

✅ **B**

Immutable collection.

---

# Question 19

Which collections are immutable?

Choose three.

A.

```java
List.of(...)
```

B.

```java
Set.of(...)
```

C.

```java
Map.of(...)
```

D.

```java
new ArrayList<>()
```

---

## Answer

✅ **A B C**

---

# Question 20

Which statement about HashMap is true?

A. Ordered by insertion

B. Sorted

C. No ordering guarantee

D. Sorted descending

---

## Answer

✅ **C**

---

# Question 21

What is printed?

```java
Queue<Integer> q = new LinkedList<>();

q.offer(1);
q.offer(2);

System.out.println(q.poll());
System.out.println(q.peek());
```

A.

```
1
2
```

B.

```
2
2
```

C.

```
1
1
```

D. Exception

---

## Answer

✅ **A**

---

# Question 22

Which Queue method removes and returns the head?

A. peek()

B. poll()

C. element()

D. offer()

---

## Answer

✅ **B**

---

# Question 23

Deque supports

Choose three.

A. Stack

B. Queue

C. Double-ended operations

D. Sorting

---

## Answer

✅ **A B C**

---

# Question 24

Which collection keeps elements sorted?

A. HashSet

B. ArrayList

C. TreeSet

D. LinkedList

---

## Answer

✅ **C**

---

# Question 25

Given

```java
TreeSet<String> set = new TreeSet<>();

set.add("B");
set.add("A");
set.add("C");

System.out.println(set);
```

Output?

A.

```
[B, A, C]
```

B.

```
[A, B, C]
```

C.

```
[C, B, A]
```

D. Random order

---

## Answer

✅ **B**

---

# Question 26

Which interface determines natural ordering?

A. Comparator

B. Comparable

C. Iterable

D. Queue

---

## Answer

✅ **B**

---

# Question 27

Comparator is used

A. Inside the class

B. Outside the class

C. Only by TreeMap

D. Only by HashSet

---

## Answer

✅ **B**

---

# Question 28

Which statement about raw types is true?

A. Compilation error

B. Allowed with warning

C. Faster

D. Removed in Java 21

---

## Answer

✅ **B**

Oracle loves raw type warnings.

---

# Question 29

Which statement about type erasure is true?

A. Generic information exists at runtime.

B. Generic parameters are removed during compilation.

C. JVM creates separate classes.

D. Type erasure occurs at runtime.

---

## Answer

✅ **B**

---

# Question 30 (Oracle-style)

What happens?

```java
class Box<T> {

    T value;

    Box(T value){
        this.value = value;
    }

    public T get(){
        return value;
    }
}

var box = new Box<>("Java");

System.out.println(box.get());
```

A.

```
Java
```

B. Compilation fails

C. Runtime exception

D.

```
Object
```

---

## Answer

✅ **A**

Type inference determines:

```java
Box<String>
```

No explicit type argument is needed.

---

# ⭐ High-Probability Oracle Traps (Know These Cold)

These are among the most frequently tested concepts in Oracle-style exams:

| Topic                                       | What Oracle likes to test                                                  |
| ------------------------------------------- | -------------------------------------------------------------------------- |
| `remove(1)` vs `remove(Integer.valueOf(1))` | Index vs object overload                                                   |
| `Integer == Integer`                        | Integer cache (`-128` to `127`)                                            |
| Unboxing `null`                             | `NullPointerException`                                                     |
| `List<String>` vs `List<Object>`            | Generics are invariant                                                     |
| `? extends`                                 | Read safely, cannot add (except `null`)                                    |
| `? super`                                   | Safe to add values, reads as `Object`                                      |
| `List.of()` / `Set.of()` / `Map.of()`       | Immutable collections; modification throws `UnsupportedOperationException` |
| `HashSet`                                   | No duplicates, no guaranteed order                                         |
| `TreeSet` / `TreeMap`                       | Natural or comparator-defined sorted order                                 |
| `HashMap`                                   | No guaranteed iteration order                                              |
| `Queue.poll()` vs `peek()`                  | `poll()` removes, `peek()` only inspects                                   |
| `Comparable` vs `Comparator`                | Natural ordering vs external ordering                                      |
| Raw types                                   | Compile with warnings, reduce type safety                                  |
| Type erasure                                | Generic type information is removed during compilation                     |
| `var` with generics                         | Infers the most specific generic type (e.g., `List<Integer>`)              |
| Generic methods                             | Type parameter (`<T>`) appears before the return type                      |

This set is very close in style and difficulty to the actual **Oracle Java SE 21 Developer Professional (1Z0-830)** exam and emphasizes the certification's common pitfalls and favorite patterns.
