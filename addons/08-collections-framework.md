Absolutely. For the **Java SE 21 Developer Professional (1Z0-830)** exam, the **Collections Framework** is one of Oracle's favorite topics. They rarely ask "What does Queue do?" Instead, they focus on subtle behaviors, interface hierarchies, ordering, mutability, exceptions, comparators, and implementation differences.

Below is essentially everything you should know for the exam.

---

# Java SE 21 (1Z0-830)

# Collections Framework (Exam Guide)

## 8. Collections Framework

Expand to include:

- [ ] Queue
- [ ] Deque
- [ ] LinkedList
- [ ] TreeSet
- [ ] TreeMap
- [ ] Comparator
- [ ] Comparable
- [ ] Immutable collections
- [ ] Collection factory methods (`List.of()`, `Set.of()`, `Map.of()`)

---

# 1. Collections Framework Overview

The Collections Framework is a hierarchy of interfaces and implementations for storing groups of objects.

```
                    Iterable
                        │
                  Collection
         ┌──────────┼──────────┐
         │          │          │
       List        Set      Queue
         │          │          │
 ArrayList     HashSet    PriorityQueue
 LinkedList    TreeSet    ArrayDeque
 Vector

Map is NOT a Collection

          Map
      ┌────┴─────┐
  HashMap     TreeMap
```

## Very Important

Oracle loves asking:

> Which interfaces extend Collection?

Answer:

- List
- Set
- Queue

NOT Map.

Map is completely separate.

---

# 2. Queue

A Queue represents FIFO (First In First Out).

```
Front → [A][B][C] → Back

Remove → A
```

Common implementations

- LinkedList
- ArrayDeque
- PriorityQueue

---

## Queue methods

| Method    | Throws Exception? | Returns Special Value? |
| --------- | ----------------- | ---------------------- |
| add()     | Yes               | No                     |
| offer()   | No                | Yes                    |
| remove()  | Yes               | No                     |
| poll()    | No                | Yes                    |
| element() | Yes               | No                     |
| peek()    | No                | Yes                    |

---

Example

```java
Queue<String> q = new LinkedList<>();

q.offer("A");
q.offer("B");

System.out.println(q.poll());
```

Output

```
A
```

---

Difference

```
remove()

throws exception
```

```
poll()

returns null
```

if queue empty.

---

Likewise

```
element()

throws exception
```

```
peek()

returns null
```

---

## Oracle favorite

Know which methods throw exceptions.

---

# 3. Deque

Deque means

Double Ended Queue.

```
Front
 ↓
[A][B][C]
        ↑
      Back
```

You may insert/remove from both ends.

---

Common implementation

```
ArrayDeque
```

or

```
LinkedList
```

---

Methods

```
addFirst()
addLast()

offerFirst()
offerLast()

removeFirst()
removeLast()

pollFirst()
pollLast()

peekFirst()
peekLast()
```

---

Example

```java
Deque<Integer> d = new ArrayDeque<>();

d.addFirst(10);
d.addLast(20);

System.out.println(d);
```

Output

```
[10,20]
```

---

# ArrayDeque

Fast implementation of Queue and Deque.

Oracle loves asking:

Can it contain null?

Answer

```
NO
```

```java
Deque<String> d = new ArrayDeque<>();

d.add(null);
```

Throws

```
NullPointerException
```

---

# 4. LinkedList

LinkedList implements

```
List
Queue
Deque
```

All three.

This is a favorite exam question.

```
LinkedList<String> list = new LinkedList<>();
```

Can be treated as

```
List
Queue
Deque
```

---

Supports

```
addFirst()
addLast()

removeFirst()

poll()

peek()

get()

set()

add(index)
```

because it implements multiple interfaces.

---

Performance

Random access

```
Slow
```

Insert/remove at ends

```
Fast
```

---

Oracle does NOT expect Big-O complexity.

Just know

ArrayList

- fast get()

LinkedList

- fast add/remove at ends

---

# 5. TreeSet

TreeSet stores

```
unique
sorted
```

elements.

Internally

```
Red-Black Tree
```

---

Example

```java
TreeSet<Integer> set = new TreeSet<>();

set.add(10);
set.add(5);
set.add(20);

System.out.println(set);
```

Output

```
[5,10,20]
```

Automatically sorted.

---

Duplicates?

Ignored.

```java
set.add(5);
```

No effect.

---

Can TreeSet store null?

Java 21

```
No
```

Adding null causes

```
NullPointerException
```

---

Ordering

Default ordering uses

```
Comparable
```

unless Comparator supplied.

---

Useful methods

```
first()

last()

higher()

lower()

ceiling()

floor()
```

Oracle occasionally asks these.

---

# 6. TreeMap

Stores

```
key/value
```

pairs

sorted by key.

---

Example

```java
TreeMap<Integer,String> map =
        new TreeMap<>();

map.put(3,"C");
map.put(1,"A");
map.put(2,"B");

System.out.println(map);
```

Output

```
{1=A,2=B,3=C}
```

---

Important

Only

```
keys
```

are sorted.

Values are not.

---

Can keys be null?

No.

```
NullPointerException
```

---

Values?

May be null.

---

# 7. Comparable

Comparable defines

Natural Ordering.

Example

```java
class Student
implements Comparable<Student>{

    int age;

    @Override
    public int compareTo(Student s){
        return age - s.age;
    }
}
```

Now

```
TreeSet<Student>
```

knows how to sort.

---

Method

```java
compareTo(T o)
```

returns

negative

zero

positive

---

Meaning

```
<0

this smaller
```

```
0

equal
```

```
>0

this larger
```

---

Oracle favorite

Comparable

```
inside class
```

Comparator

```
outside class
```

---

# 8. Comparator

Provides custom ordering.

Example

```java
Comparator<String> byLength =
    (a,b) ->
        a.length()-b.length();
```

---

Use

```java
TreeSet<String> set =
    new TreeSet<>(byLength);
```

---

Can have many Comparators.

Natural ordering remains unchanged.

---

Comparator methods

```
compare()

reversed()

thenComparing()

comparing()
```

Need basic familiarity.

---

Example

```java
Comparator<Person> c =
    Comparator
      .comparing(Person::getAge)
      .thenComparing(Person::getName);
```

Oracle occasionally asks this syntax.

---

# Comparable vs Comparator

| Comparable        | Comparator         |
| ----------------- | ------------------ |
| compareTo()       | compare()          |
| Inside class      | Outside class      |
| One natural order | Many custom orders |

---

# Certification Trap

Suppose

```java
TreeSet<Person> set =
        new TreeSet<>();
```

Person

does NOT implement Comparable.

First insertion

```
OK
```

Second insertion

```
ClassCastException
```

because comparison becomes necessary.

Oracle loves this.

---

# 9. Immutable Collections

Java 9+

```
List.of()

Set.of()

Map.of()
```

create immutable collections.

---

Example

```java
List<String> list =
    List.of("A","B","C");
```

Attempt

```java
list.add("D");
```

Throws

```
UnsupportedOperationException
```

---

Very important

Immutable ≠ Unmodifiable Wrapper.

Oracle sometimes distinguishes them.

---

# 10. Collection Factory Methods

## List.of()

```java
List<Integer> nums =
    List.of(1,2,3);
```

Characteristics

- immutable
- preserves order
- duplicates allowed
- null forbidden

---

Null?

```java
List.of(1,null);
```

Throws

```
NullPointerException
```

---

## Set.of()

Characteristics

- immutable
- duplicates forbidden
- null forbidden

---

Duplicate

```java
Set.of("A","A");
```

Throws

```
IllegalArgumentException
```

Not ignored.

Oracle loves this.

HashSet ignores duplicates.

Set.of()

throws exception.

Huge exam favorite.

---

Null

```java
Set.of(null);
```

Throws

```
NullPointerException
```

---

## Map.of()

```java
Map.of(
   1,"A",
   2,"B"
);
```

Immutable.

---

Null key?

```
NPE
```

Null value?

```
NPE
```

Duplicate key?

```
IllegalArgumentException
```

---

# Factory Method Summary

| Factory   | Immutable | Null              | Duplicates                             |
| --------- | --------- | ----------------- | -------------------------------------- |
| List.of() | Yes       | Not allowed       | Allowed                                |
| Set.of()  | Yes       | Not allowed       | IllegalArgumentException               |
| Map.of()  | Yes       | No keys or values | Duplicate key IllegalArgumentException |

---

# Common Certification Traps

## Trap 1

```java
Queue<Integer> q = new LinkedList<>();

q.remove();
```

Empty queue.

Result

```
NoSuchElementException
```

---

## Trap 2

```java
Queue<Integer> q = new LinkedList<>();

System.out.println(q.poll());
```

Output

```
null
```

---

## Trap 3

```java
Deque<Integer> d = new ArrayDeque<>();

d.add(null);
```

Throws

```
NullPointerException
```

---

## Trap 4

```java
TreeSet<String> set =
        new TreeSet<>();

set.add("B");
set.add("A");
set.add("C");

System.out.println(set);
```

Output

```
[A, B, C]
```

---

## Trap 5

```java
Set<String> s =
    Set.of("A","A");
```

Throws

```
IllegalArgumentException
```

---

## Trap 6

```java
HashSet<String> s =
        new HashSet<>();

s.add("A");
s.add("A");

System.out.println(s.size());
```

Output

```
1
```

No exception.

---

## Trap 7

```java
List<String> list =
    List.of("A","B");

list.remove(0);
```

Throws

```
UnsupportedOperationException
```

---

# Oracle Favorite Practice Questions

---

## Question 1

```java
Queue<Integer> q = new LinkedList<>();

System.out.println(q.poll());
```

A)

Exception

B)

0

C)

null

D)

Compilation error

### Answer

✅ C

`poll()` returns `null` when the queue is empty.

---

## Question 2

```java
Queue<Integer> q = new LinkedList<>();

q.remove();
```

A)

null

B)

NoSuchElementException

C)

IllegalStateException

D)

UnsupportedOperationException

### Answer

✅ B

---

## Question 3

```java
Set<String> set =
    Set.of("A","B","A");
```

A)

prints A B

B)

duplicates ignored

C)

IllegalArgumentException

D)

Compilation error

### Answer

✅ C

---

## Question 4

```java
List<String> list =
        List.of("A","B");

list.add("C");
```

What happens?

### Answer

✅ UnsupportedOperationException

---

## Question 5

```java
TreeMap<Integer,String> map =
        new TreeMap<>();

map.put(3,"C");
map.put(1,"A");
map.put(2,"B");

System.out.println(map);
```

Output?

A)

```
{3=C,1=A,2=B}
```

B)

```
{1=A,2=B,3=C}
```

C)

Random order

D)

Compilation error

### Answer

✅ B

---

## Question 6

```java
class Person {}

TreeSet<Person> set =
        new TreeSet<>();

set.add(new Person());
set.add(new Person());
```

Result?

### Answer

✅ `ClassCastException`

`Person` does not implement `Comparable`, and no `Comparator` was provided. The first insertion succeeds because no comparison is needed; the second insertion triggers a comparison and fails.

---

## Question 7

```java
Deque<Integer> d =
        new ArrayDeque<>();

d.addFirst(1);
d.addLast(2);

System.out.println(d.removeFirst());
System.out.println(d.removeLast());
```

Output?

### Answer

```
1
2
```

---

# Final Exam Cheat Sheet (Memorize)

| Collection    | Ordered                              | Sorted      | Duplicates                 | Null                                  |
| ------------- | ------------------------------------ | ----------- | -------------------------- | ------------------------------------- |
| ArrayList     | ✅ Yes (insertion order)             | ❌          | ✅                         | ✅                                    |
| LinkedList    | ✅                                   | ❌          | ✅                         | ✅                                    |
| HashSet       | ❌                                   | ❌          | ❌                         | One `null` allowed                    |
| TreeSet       | ✅ (sorted)                          | ✅          | ❌                         | ❌                                    |
| HashMap       | ❌                                   | ❌          | Duplicate keys not allowed | One `null` key, many `null` values    |
| TreeMap       | ✅ (sorted by key)                   | ✅          | Duplicate keys not allowed | ❌ `null` keys, `null` values allowed |
| ArrayDeque    | Queue/Deque order                    | ❌          | ✅                         | ❌                                    |
| PriorityQueue | Priority order (not insertion order) | By priority | ✅                         | ❌                                    |

---

## High-Priority Topics for 1Z0-830

If Oracle asks about collections, these are the concepts most likely to appear:

1. ⭐ Difference between `Comparable` and `Comparator`
2. ⭐ `TreeSet` and `TreeMap` ordering behavior
3. ⭐ `Queue` methods (`poll()` vs `remove()`, `peek()` vs `element()`)
4. ⭐ `LinkedList` implementing `List`, `Queue`, and `Deque`
5. ⭐ Immutable collections (`List.of()`, `Set.of()`, `Map.of()`)
6. ⭐ Factory method restrictions (`null`, duplicates, immutability)
7. ⭐ `UnsupportedOperationException`, `IllegalArgumentException`, `NullPointerException`, and `ClassCastException` scenarios
8. ⭐ Which collections allow `null`, maintain insertion order, or keep elements sorted

Mastering these behaviors and edge cases will prepare you for the majority of collection-related questions on the Java SE 21 Developer Professional exam.
