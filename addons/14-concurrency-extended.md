# Java SE 21 Developer Professional (1Z0-830)

# Chapter 14 — Concurrency (Exam Guide)

- [ ] Callable
- [ ] Future
- [ ] ScheduledExecutorService
- [ ] Atomic classes
- [ ] Locks (basic awareness)
- [ ] Thread-safe collections

This chapter covers **the remaining concurrency topics** that Oracle expects candidates to understand after learning Threads, Runnable, synchronization, and ExecutorService.

For the exam, Oracle is **far more interested in understanding APIs and predicting code output** than writing complex multithreaded programs.

---

# Exam Weight

Concurrency questions are usually **medium difficulty** because they combine several topics:

- Executors
- Functional Interfaces
- Exceptions
- Generics
- Lambdas
- Collections

Oracle loves questions where **everything compiles but behaves differently than expected.**

---

# 1. Callable

## What is Callable?

`Callable` is similar to `Runnable`, except that it

- returns a value
- may throw checked exceptions

```java
public interface Callable<V> {
    V call() throws Exception;
}
```

Notice:

- generic return type
- method is `call()`
- not `run()`

---

## Runnable vs Callable

| Runnable                        | Callable                     |
| ------------------------------- | ---------------------------- |
| run()                           | call()                       |
| returns void                    | returns value                |
| cannot throw checked exceptions | can throw checked exceptions |
| Thread can execute              | ExecutorService executes     |
| older API                       | newer API                    |

Oracle loves this table.

---

## Runnable example

```java
Runnable r = () -> System.out.println("Hello");
```

No return value.

---

## Callable example

```java
Callable<Integer> c = () -> 42;

ExecutorService service =
    Executors.newSingleThreadExecutor();

Future<Integer> result = service.submit(c);

System.out.println(result.get());

service.shutdown();
```

Output

```
42
```

---

## submit() overloads

Oracle asks these constantly.

```java
submit(Runnable task)
```

returns

```
Future<?>
```

---

```java
submit(Runnable task, T result)
```

returns

```
Future<T>
```

---

```java
submit(Callable<T>)
```

returns

```
Future<T>
```

Know all three.

---

# Oracle Favorite

```java
Runnable r = () -> {};
Future<?> f = executor.submit(r);
```

Question:

What is the type of `f`?

Answer

```
Future<?>
```

NOT

```
Future<Void>
```

---

# 2. Future

A Future represents

> "a result that may not be available yet."

Think:

```
Task running...
        ↓
Future object
        ↓
Eventually result
```

---

## Important methods

### get()

Blocks until finished.

```java
Integer x = future.get();
```

---

### get(timeout)

```java
future.get(5, TimeUnit.SECONDS);
```

Throws

```
TimeoutException
```

if time expires.

---

### cancel()

```java
future.cancel(true);
```

Attempts cancellation.

---

### isDone()

Returns

```
true
```

after completion.

---

### isCancelled()

Returns whether cancellation succeeded.

---

# Oracle Favorite

```java
Future<Integer> f =
    executor.submit(() -> 100);

System.out.println(f.isDone());
```

Possible output?

Either

```
true
```

or

```
false
```

depending on timing.

Oracle loves nondeterministic answers.

---

# Future.get()

This is one of Oracle's favorite questions.

```java
Future<Integer> f =
    executor.submit(() -> 10);

System.out.println("A");
System.out.println(f.get());
System.out.println("B");
```

Output

```
A
10
B
```

`get()` blocks.

---

# Checked exceptions

`Future.get()` throws

```
InterruptedException
ExecutionException
```

Must be handled.

---

# ExecutionException

Suppose

```java
Callable<Integer> c = () -> {
    throw new RuntimeException();
};
```

Then

```java
future.get();
```

throws

```
ExecutionException
```

NOT RuntimeException.

Oracle asks this often.

---

# 3. ScheduledExecutorService

Creates executors that execute later.

```java
ScheduledExecutorService service =
    Executors.newScheduledThreadPool(2);
```

---

## schedule()

```java
service.schedule(
    () -> System.out.println("Hi"),
    5,
    TimeUnit.SECONDS
);
```

Runs once.

---

## scheduleAtFixedRate()

```java
service.scheduleAtFixedRate(
    task,
    1,
    2,
    TimeUnit.SECONDS
);
```

Timeline

```
1 sec
3 sec
5 sec
7 sec
...
```

Starts every fixed period.

---

## scheduleWithFixedDelay()

Runs

```
task finishes
      ↓
wait delay
      ↓
run again
```

Difference:

Fixed Rate

```
start every X seconds
```

Fixed Delay

```
finish
wait
start again
```

Oracle occasionally asks this distinction.

---

# 4. Atomic Classes

Synchronization is expensive.

Sometimes you only need atomic updates.

Java provides

```
AtomicInteger
AtomicLong
AtomicBoolean
AtomicReference
```

Package

```
java.util.concurrent.atomic
```

---

## Example

Instead of

```java
int counter;
counter++;
```

Use

```java
AtomicInteger counter =
    new AtomicInteger();

counter.incrementAndGet();
```

Thread-safe.

---

## Common methods

### get()

```java
counter.get();
```

---

### set()

```java
counter.set(10);
```

---

### incrementAndGet()

```java
counter.incrementAndGet();
```

Returns incremented value.

---

### getAndIncrement()

Returns previous value.

Huge Oracle favorite.

Example

```java
AtomicInteger a =
    new AtomicInteger(5);

System.out.println(
    a.getAndIncrement());

System.out.println(
    a.get());
```

Output

```
5
6
```

---

### incrementAndGet()

```java
AtomicInteger a =
    new AtomicInteger(5);

System.out.println(
    a.incrementAndGet());

System.out.println(a.get());
```

Output

```
6
6
```

Know the difference.

---

# compareAndSet()

Very common interview question.

```java
AtomicInteger a =
    new AtomicInteger(5);

boolean b =
a.compareAndSet(5,10);
```

Now

```
a = 10
```

Returns

```
true
```

---

# 5. Locks

Package

```
java.util.concurrent.locks
```

Main interface

```
Lock
```

Implementation

```
ReentrantLock
```

---

Instead of

```java
synchronized
```

you can write

```java
Lock lock =
    new ReentrantLock();

lock.lock();

try{

}
finally{
    lock.unlock();
}
```

Always unlock inside finally.

Oracle likes this.

---

## tryLock()

```java
if(lock.tryLock()){

}
```

Doesn't block forever.

---

## lockInterruptibly()

Allows interruption while waiting.

Exam only expects awareness.

---

# synchronized vs Lock

| synchronized     | Lock          |
| ---------------- | ------------- |
| keyword          | class         |
| automatic unlock | manual unlock |
| less flexible    | more flexible |
| simpler          | advanced      |

Know this comparison.

---

# 6. Thread-safe Collections

Most collections are **NOT thread-safe**.

For example

```
ArrayList
HashMap
HashSet
LinkedList
TreeMap
TreeSet
```

None are synchronized.

---

## Legacy synchronized collections

Examples

```
Vector
Hashtable
```

Still thread-safe.

Oracle occasionally asks.

---

## Collections.synchronizedX()

```java
List<String> list =
Collections.synchronizedList(
    new ArrayList<>());
```

Thread-safe wrapper.

---

## Concurrent collections

Most important ones

```
ConcurrentHashMap
ConcurrentLinkedQueue
ConcurrentLinkedDeque
CopyOnWriteArrayList
CopyOnWriteArraySet
BlockingQueue
```

Know these names.

---

# ConcurrentHashMap

Replacement for Hashtable.

Allows concurrent access.

```java
ConcurrentHashMap<Integer,String> map =
    new ConcurrentHashMap<>();
```

---

# CopyOnWriteArrayList

Oracle favorite.

Every modification copies the array.

Excellent for

```
many readers
few writers
```

Bad for frequent modifications.

---

# BlockingQueue

Used for producer-consumer.

Methods

```
put()
take()
offer()
poll()
```

`take()` blocks until an element becomes available.

---

# Oracle Favorites

## Future.get()

Always blocks.

---

## submit(Runnable)

Returns

```
Future<?>
```

---

## submit(Callable)

Returns

```
Future<T>
```

---

## AtomicInteger

Know

```
incrementAndGet()

vs

getAndIncrement()
```

---

## schedule()

Runs once.

---

## scheduleAtFixedRate()

Repeated execution.

---

## scheduleWithFixedDelay()

Repeated after previous execution completes.

---

## Lock

Always

```
unlock()
```

inside finally.

---

## ConcurrentHashMap

Preferred over Hashtable.

---

## CopyOnWriteArrayList

Great for

```
many reads
few writes
```

---

# Certification Traps

## Trap 1

```java
Thread t =
new Thread(() -> 5);
```

Illegal.

Runnable returns void.

---

## Trap 2

```java
Callable<Integer> c = () -> {};
```

Illegal.

Must return Integer.

---

## Trap 3

```java
Callable<Integer> c =
() -> {
    throw new IOException();
};
```

Legal.

Callable may throw checked exceptions.

---

## Trap 4

```java
Future<Integer> f =
executor.submit(() -> 5);

System.out.println(f);
```

Prints

```
FutureTask...
```

NOT

```
5
```

Need

```
get()
```

---

## Trap 5

```java
future.get();
```

May throw

```
ExecutionException
InterruptedException
```

---

## Trap 6

```java
AtomicInteger a =
new AtomicInteger(1);

a++;
```

Illegal.

Use

```
incrementAndGet()
```

---

## Trap 7

```java
lock.lock();
```

Forgot

```
unlock();
```

Deadlock risk.

---

## Trap 8

```java
ArrayList
```

is NOT thread-safe.

Many students incorrectly assume it is.

---

# Oracle-Style Practice Questions

---

## Question 1

```java
Callable<Integer> c = () -> 10;

ExecutorService service =
Executors.newSingleThreadExecutor();

Future<Integer> f =
service.submit(c);

System.out.println(f.get());

service.shutdown();
```

### Answer

```
10
```

---

## Question 2

```java
Runnable r = () -> {};

Future<?> f =
executor.submit(r);
```

What is the type?

### Answer

```
Future<?>
```

---

## Question 3

```java
AtomicInteger a =
new AtomicInteger(7);

System.out.println(
a.getAndIncrement());

System.out.println(
a.incrementAndGet());
```

### Answer

```
7
9
```

Explanation

Initial value = 7

```
getAndIncrement()
returns 7
value becomes 8

incrementAndGet()
becomes 9
returns 9
```

---

## Question 4

```java
Callable<Integer> c =
() -> {
    throw new RuntimeException();
};

Future<Integer> f =
executor.submit(c);

System.out.println(
f.get());
```

### Answer

Throws

```
ExecutionException
```

---

## Question 5

Which collection is designed for many reads and few writes?

A)

```
ArrayList
```

B)

```
LinkedList
```

C)

```
CopyOnWriteArrayList
```

D)

```
HashSet
```

### Answer

C

---

## Question 6

Which method blocks until a result becomes available?

A

```
isDone()
```

B

```
cancel()
```

C

```
get()
```

D

```
schedule()
```

### Answer

C

---

## Question 7

Which interface allows checked exceptions?

A

```
Runnable
```

B

```
Callable
```

C

```
Thread
```

D

```
ExecutorService
```

### Answer

B

---

# Final Exam Cheat Sheet

## Callable

- `call()`
- Returns value
- Can throw checked exceptions
- Submitted to `ExecutorService`

---

## Future

- Represents asynchronous result
- `get()` blocks
- `cancel()`
- `isDone()`
- `isCancelled()`
- `get()` throws `InterruptedException` and `ExecutionException`

---

## ScheduledExecutorService

- `schedule()` → once
- `scheduleAtFixedRate()` → repeated at fixed intervals
- `scheduleWithFixedDelay()` → repeated after the previous run finishes plus a delay

---

## Atomic Classes

- `AtomicInteger`
- `AtomicLong`
- `AtomicBoolean`
- `AtomicReference`
- `incrementAndGet()` vs `getAndIncrement()`
- `compareAndSet()`

---

## Locks

- `Lock` / `ReentrantLock`
- `lock()`
- `unlock()` in `finally`
- `tryLock()`
- More flexible than `synchronized`

---

## Thread-safe Collections

- `ConcurrentHashMap`
- `ConcurrentLinkedQueue`
- `ConcurrentLinkedDeque`
- `CopyOnWriteArrayList`
- `CopyOnWriteArraySet`
- `BlockingQueue`
- `Collections.synchronizedList()`
- `Vector` and `Hashtable` are legacy synchronized collections

## High-Probability Oracle Exam Topics (★★★★★)

If Oracle asks 5–7 concurrency questions, these are the ones most likely to appear:

1. Difference between `Runnable` and `Callable`.
2. `submit(Runnable)` vs `submit(Callable)` return types.
3. `Future.get()` blocking behavior and checked exceptions.
4. `AtomicInteger`: `getAndIncrement()` vs `incrementAndGet()`.
5. `ScheduledExecutorService`: `schedule()`, `scheduleAtFixedRate()`, and `scheduleWithFixedDelay()`.
6. `ConcurrentHashMap` and `CopyOnWriteArrayList` use cases.
7. Proper `ReentrantLock` usage (`unlock()` in a `finally` block).
8. Identifying which standard collections are **not** thread-safe (`ArrayList`, `HashMap`, `HashSet`, etc.).

Mastering these topics and the associated API signatures, exception behavior, and common traps will put you in a very strong position for the concurrency portion of the Java SE 21 Developer Professional (1Z0-830) exam.
