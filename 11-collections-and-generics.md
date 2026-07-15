# Java SE 21 Developer Professional (1Z0-830)

# Collections & Generics — Complete Exam Guide

## 📚 11. Collections & Generics

- [ ] List, Set, Map basics
- [ ] `ArrayList`, `HashSet`, `HashMap`
- [ ] Iteration methods
- [ ] Generic type rules (`<T>`)
- [ ] Wildcards (`?`, `? extends`, `? super`)
- [ ] Autoboxing/unboxing

This is one of Oracle's favorite exam areas because it combines:

- Collections API knowledge
- Generics syntax
- Type safety
- Polymorphism
- Inheritance
- Method signatures
- Autoboxing behaviors

Many questions are intentionally designed to test **compile-time errors vs runtime behavior**, so pay very close attention to type declarations.

---

# 1. Collections Framework Overview

The Collections Framework provides data structures for storing groups of objects.

Main interfaces:

```java
Collection
 ├── List
 ├── Set
 └── Queue

Map
```

Notice:

```java
Map is NOT a Collection
```

This is a very common exam fact.

---

# 2. List

A List:

✅ Maintains insertion order

✅ Allows duplicates

✅ Allows positional access via index

Examples:

```java
List<String> list = new ArrayList<>();

list.add("A");
list.add("A");
list.add("B");

System.out.println(list);
```

Output:

```java
[A, A, B]
```

Duplicates are allowed.

---

## Common List Methods

```java
add(E e)
add(int index, E e)

remove(int index)
remove(Object o)

get(int index)

set(int index, E e)

contains(Object o)

size()

isEmpty()

clear()
```

Example:

```java
List<String> list = new ArrayList<>();

list.add("Java");
list.add("Oracle");

System.out.println(list.get(1));
```

Output:

```java
Oracle
```

---

# 3. ArrayList

Most important List implementation in the exam.

Characteristics:

✅ Ordered

✅ Resizable array

✅ Fast random access

✅ Allows duplicates

✅ Allows null

```java
ArrayList<String> list = new ArrayList<>();
```

Example:

```java
List<String> list = new ArrayList<>();

list.add("A");
list.add(null);
list.add("A");

System.out.println(list);
```

Output:

```java
[A, null, A]
```

Perfectly legal.

---

# Oracle Favorite Trap #1

```java
List<Integer> list = new ArrayList<>();

list.add(10);

list.remove(10);

System.out.println(list);
```

Result:

```java
IndexOutOfBoundsException
```

Why?

Because:

```java
remove(int index)
```

is chosen.

Compiler sees:

```java
10
```

as int.

So it tries to remove index 10.

---

Correct version

```java
list.remove(Integer.valueOf(10));
```

Now:

```java
remove(Object o)
```

is called.

---

# 4. Set

Characteristics:

✅ No duplicates

✅ No indexes

Example:

```java
Set<String> set = new HashSet<>();

set.add("A");
set.add("A");
set.add("B");

System.out.println(set);
```

Output:

```java
[A, B]
```

(or similar order)

Second "A" is ignored.

---

## Common Set Methods

```java
add()
remove()
contains()
size()
isEmpty()
clear()
```

No:

```java
get(index)
```

because Sets don't have indexes.

---

# 5. HashSet

Most tested Set implementation.

Characteristics:

✅ No duplicates

✅ Fast lookup

✅ Unordered

✅ Allows one null

```java
HashSet<String> set = new HashSet<>();
```

Example:

```java
Set<String> set = new HashSet<>();

set.add("Java");
set.add("Oracle");

System.out.println(set);
```

Order is unpredictable.

---

# Oracle Favorite Trap #2

Never assume order in:

```java
HashSet
HashMap
```

Exam often gives:

```java
Set<Integer> set = new HashSet<>();

set.add(3);
set.add(1);
set.add(2);

System.out.println(set);
```

You must answer:

```java
Order is not guaranteed
```

NOT

```java
[3,1,2]
```

---

# 6. Map

Stores:

```java
key -> value
```

A key maps to a value.

Example:

```java
Map<Integer,String> map = new HashMap<>();
```

---

## Common Methods

```java
put()
get()
remove()
containsKey()
containsValue()
keySet()
values()
entrySet()
size()
```

---

Example

```java
Map<Integer,String> map = new HashMap<>();

map.put(1,"Java");
map.put(2,"Oracle");

System.out.println(map.get(1));
```

Output:

```java
Java
```

---

# 7. HashMap

Most important Map implementation.

Characteristics:

✅ Keys unique

✅ Values may duplicate

✅ Allows one null key

✅ Allows multiple null values

✅ Unordered

---

Example

```java
Map<Integer,String> map = new HashMap<>();

map.put(1,"A");
map.put(1,"B");

System.out.println(map);
```

Output:

```java
{1=B}
```

Old value replaced.

---

# Oracle Favorite Trap #3

```java
Map<Integer,String> map = new HashMap<>();

map.put(1,"A");
map.put(1,"B");

System.out.println(map.size());
```

Output:

```java
1
```

Not 2.

Keys must be unique.

---

# 8. Iteration Methods

## Enhanced for loop

```java
for(String s : list){
    System.out.println(s);
}
```

Very common.

---

## Iterator

```java
Iterator<String> it = list.iterator();

while(it.hasNext()){
    System.out.println(it.next());
}
```

---

# Very Important Exam Topic

Removing elements while iterating.

Bad:

```java
for(String s : list){
    list.remove(s);
}
```

Runtime:

```java
ConcurrentModificationException
```

---

Correct:

```java
Iterator<String> it = list.iterator();

while(it.hasNext()){
    if(it.next().equals("A"))
        it.remove();
}
```

---

# Iterating a Map

## keySet()

```java
for(Integer k : map.keySet()){
    System.out.println(k);
}
```

---

## values()

```java
for(String v : map.values()){
    System.out.println(v);
}
```

---

## entrySet()

Most common Oracle example.

```java
for(Map.Entry<Integer,String> e : map.entrySet()){
    System.out.println(e.getKey());
    System.out.println(e.getValue());
}
```

---

# 9. Generics Basics

Generics provide compile-time type safety.

Without Generics:

```java
ArrayList list = new ArrayList();

list.add("Java");
list.add(10);
```

Allowed.

Dangerous.

---

With Generics:

```java
ArrayList<String> list = new ArrayList<>();

list.add("Java");
```

Compiler ensures only Strings.

---

# Generic Syntax

```java
<T>
```

represents a type parameter.

Example:

```java
class Box<T> {

    private T value;

    public T getValue() {
        return value;
    }

    public void setValue(T value) {
        this.value = value;
    }
}
```

---

Usage

```java
Box<String> box = new Box<>();
```

Now:

```java
T = String
```

---

# Oracle Favorite Trap #4

```java
List<String> list = new ArrayList<>();

list.add(100);
```

Compilation error.

Why?

```java
Integer
```

is not

```java
String
```

---

# Generic Methods

```java
public static <T> void print(T obj){
    System.out.println(obj);
}
```

Usage:

```java
print("Java");
print(100);
```

---

# 10. Wildcards

One of Oracle's favorite subjects.

---

## Unbounded Wildcard

```java
?
```

Means:

```java
unknown type
```

Example:

```java
List<?> list;
```

Can reference:

```java
List<String>
List<Integer>
List<Double>
```

---

Example

```java
List<?> list = new ArrayList<String>();
```

Legal.

---

Cannot add elements:

```java
list.add("Java");
```

Compile error.

Because type unknown.

---

Only allowed:

```java
list.add(null);
```

Oracle loves this fact.

---

# 11. Upper Bound Wildcard

## ? extends

Syntax:

```java
List<? extends Number>
```

Means:

```java
List of Number or subclass
```

Examples:

```java
List<Integer>
List<Double>
List<Float>
```

all valid.

---

Read-only rule

```java
List<? extends Number> nums =
        new ArrayList<Integer>();
```

Reading:

```java
Number n = nums.get(0);
```

Allowed.

---

Adding:

```java
nums.add(10);
```

Compile error.

---

Rule:

> Producer Extends

PECS

```java
Producer = extends
```

You safely read data.

---

# 12. Lower Bound Wildcard

## ? super

Syntax:

```java
List<? super Integer>
```

Means:

```java
Integer or parent class
```

Examples:

```java
List<Integer>
List<Number>
List<Object>
```

---

Adding allowed:

```java
nums.add(10);
```

Legal.

---

Reading:

```java
Integer n = nums.get(0);
```

Compile error.

Because actual type unknown.

Only:

```java
Object obj = nums.get(0);
```

guaranteed.

---

Rule:

> Consumer Super

PECS

```java
Consumer = super
```

---

# Oracle PECS Shortcut

Remember:

## PECS

Producer Extends

```java
? extends T
```

Read

Can't safely write

---

Consumer Super

```java
? super T
```

Write

Can't safely read

---

# 13. Invariance (BIG EXAM TOPIC)

Many developers get this wrong.

```java
Integer extends Number
```

TRUE

But:

```java
List<Integer>
```

is NOT a subtype of

```java
List<Number>
```

---

Compilation error

```java
List<Number> nums =
        new ArrayList<Integer>();
```

---

Correct:

```java
List<? extends Number> nums =
        new ArrayList<Integer>();
```

---

# Oracle Favorite Trap #5

Which compiles?

```java
List<Object> a = new ArrayList<String>();
```

❌ No

```java
List<?> a = new ArrayList<String>();
```

✅ Yes

---

# 14. Autoboxing

Automatic conversion:

```java
int -> Integer
```

Example:

```java
Integer x = 10;
```

Compiler changes it to:

```java
Integer x = Integer.valueOf(10);
```

---

# Unboxing

```java
Integer -> int
```

Example:

```java
Integer x = 10;

int y = x;
```

Compiler inserts:

```java
x.intValue()
```

---

# Oracle Favorite Trap #6

```java
Integer x = null;

int y = x;
```

Runtime:

```java
NullPointerException
```

because unboxing null.

---

# Oracle Favorite Trap #7

```java
List<Integer> list = new ArrayList<>();

list.add(1);
list.add(2);

int sum = 0;

for(Integer i : list)
    sum += i;
```

Compiler performs repeated unboxing.

Perfectly legal.

---

# Most Important Collection Interfaces To Memorize

| Interface | Duplicates | Ordered | Index |
| --------- | ---------- | ------- | ----- |
| List      | Yes        | Yes     | Yes   |
| Set       | No         | No\*    | No    |
| Map       | Keys No    | No\*    | No    |

(\*) HashSet and HashMap do not guarantee order.

---

# Top 10 Exam Gotchas

## 1

```java
Map is not a Collection
```

---

## 2

```java
HashSet order not guaranteed
```

---

## 3

```java
HashMap order not guaranteed
```

---

## 4

```java
remove(10)
```

calls

```java
remove(int index)
```

for List<Integer>

---

## 5

Cannot safely add to

```java
List<?>
List<? extends T>
```

---

## 6

Can only add

```java
null
```

to

```java
List<?>
```

---

## 7

```java
List<Integer>
```

NOT subtype of

```java
List<Number>
```

---

## 8

Unboxing null causes

```java
NullPointerException
```

---

## 9

Modifying a collection during enhanced-for iteration causes

```java
ConcurrentModificationException
```

---

## 10

Duplicate key in HashMap replaces old value.

---

# Oracle-Style Practice Questions

---

## Question 1

```java
List<Integer> list = new ArrayList<>();

list.add(1);
list.add(2);

list.remove(1);

System.out.println(list);
```

Answer:

```java
[1]
```

Explanation:

```java
remove(int index)
```

removes element at index 1.

---

## Question 2

```java
Set<String> set = new HashSet<>();

set.add("A");
set.add("A");
set.add("B");

System.out.println(set.size());
```

Answer:

```java
2
```

---

## Question 3

```java
Map<Integer,String> map = new HashMap<>();

map.put(1,"A");
map.put(1,"B");

System.out.println(map.get(1));
```

Answer:

```java
B
```

---

## Question 4

```java
List<? extends Number> list =
        new ArrayList<Integer>();

list.add(1);
```

Answer:

```java
Compilation Error
```

---

## Question 5

```java
List<? super Integer> list =
        new ArrayList<Number>();

list.add(1);

Object obj = list.get(0);
```

Answer:

Compiles successfully.

---

## Question 6

```java
Integer n = null;

int x = n;

System.out.println(x);
```

Answer:

```java
NullPointerException
```

---

## Question 7

```java
List<?> list = new ArrayList<String>();

list.add(null);
```

Answer:

Compiles successfully.

---

# Final Exam-Day Memory Sheet

Memorize these:

```text
Map is NOT a Collection
```

```text
HashSet -> no duplicates
```

```text
HashMap -> unique keys
```

```text
HashMap/HashSet -> no ordering guarantee
```

```text
List<Integer> ≠ List<Number>
```

```text
<?> -> read mostly
```

```text
<? extends T> -> Producer
```

```text
<? super T> -> Consumer
```

```text
PECS = Producer Extends Consumer Super
```

```text
remove(10) = remove index 10
```

```text
Unboxing null => NullPointerException
```

```text
Duplicate HashMap key replaces old value
```

If you master everything above, you'll cover roughly **90–95% of the Collections & Generics questions typically seen on Oracle Java certification exams**, including most of the common trick questions and compiler-error scenarios Oracle likes to test.
