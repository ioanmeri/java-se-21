Here's a study guide focused on what is actually important for the **Oracle Java SE 21 Developer Professional (1Z0-830)** exam.

The Concurrency section of the exam is **much smaller** than Streams, Generics or Collections. Oracle does **not** expect you to become a concurrency expert. Instead, they like testing whether you understand the basic API, object-oriented relationships, and common mistakes.

The biggest Oracle traps are usually:

- `Thread` vs `Runnable`
- calling `run()` vs `start()`
- synchronized methods/blocks
- race conditions
- `ExecutorService`
- shutdown methods
- Future
- thread lifecycle
- daemon threads
- deadlock awareness (basic)

---

# 🧵 13. Concurrency (Java SE 21 Professional)

---

# 1. What is a Thread?

A thread is an independent path of execution.

A Java application always starts with one thread:

```
main thread
```

Example

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName());
    }
}
```

Output

```
main
```

---

## Process vs Thread

Oracle likes this distinction.

| Process             | Thread                     |
| ------------------- | -------------------------- |
| Independent program | Execution inside a process |
| Own memory          | Shares process memory      |
| Heavyweight         | Lightweight                |
| Starts slower       | Starts faster              |

One Java application normally has many threads.

---

# 2. Creating Threads

There are two traditional approaches.

---

## Method 1 — Extend Thread

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Running");
    }
}
```

Run it

```java
MyThread t = new MyThread();
t.start();
```

---

## Method 2 — Implement Runnable (Preferred)

```java
class MyTask implements Runnable {

    @Override
    public void run() {
        System.out.println("Running");
    }
}
```

Run

```java
Thread t = new Thread(new MyTask());
t.start();
```

Oracle loves asking:

> Which approach is preferred?

Answer:

**Implement Runnable**

because Java supports only single inheritance.

---

# Thread vs Runnable

| Thread                   | Runnable                 |
| ------------------------ | ------------------------ |
| class                    | interface                |
| represents thread itself | represents task          |
| can only extend Thread   | can extend another class |
| less flexible            | preferred                |

Oracle favorite.

---

# 3. start() vs run()

One of Oracle's most common questions.

## Correct

```java
Thread t = new Thread(() ->
    System.out.println("Hello"));

t.start();
```

Creates

- new thread

---

## Wrong

```java
t.run();
```

No new thread.

Just an ordinary method call.

Runs on

```
main thread
```

Huge exam trap.

---

# Example

```java
Thread t = new Thread(() ->
    System.out.println(Thread.currentThread().getName()));

t.run();
```

Output

```
main
```

---

Using

```java
t.start();
```

Output

```
Thread-0
```

(or similar)

Oracle loves this.

---

# 4. Runnable as Lambda

Since Runnable has one abstract method:

```java
void run();
```

it is a Functional Interface.

Therefore

```java
Runnable r = () -> System.out.println("Hello");
```

is valid.

---

# 5. Thread States

Oracle only expects awareness.

```
NEW
```

↓

```
RUNNABLE
```

↓

```
BLOCKED
```

↓

```
WAITING
```

↓

```
TIMED_WAITING
```

↓

```
TERMINATED
```

Know the names.

No need to memorize every transition.

---

# Lifecycle

```
Thread t = new Thread();

NEW

↓

t.start()

↓

RUNNABLE

↓

run()

↓

TERMINATED
```

---

# 6. Thread.currentThread()

Returns current thread.

```java
System.out.println(Thread.currentThread().getName());
```

Oracle occasionally asks this.

---

# 7. Sleeping

```java
Thread.sleep(1000);
```

Current thread pauses.

Throws

```
InterruptedException
```

Must

- catch
- or declare

---

Example

```java
try {
    Thread.sleep(1000);
} catch (InterruptedException e) {
}
```

Exam favorite:

Does sleep release locks?

Answer:

**No**

Huge trap.

---

# 8. join()

Waits for another thread.

```java
Thread t = ...

t.start();

t.join();
```

Main thread waits until

```
t
```

finishes.

Throws

```
InterruptedException
```

---

# 9. Yield

```java
Thread.yield();
```

Suggestion to scheduler.

No guarantee.

Oracle rarely asks.

---

# 10. Daemon Threads

Normal thread

```
user thread
```

Background thread

```
daemon thread
```

Example

```java
Thread t = new Thread();

t.setDaemon(true);
```

Must happen

before

```
start()
```

Otherwise

```
IllegalThreadStateException
```

Oracle trap.

---

# 11. Synchronization

Suppose two threads increment

```java
counter++
```

Counter

```
0
```

Thread A

reads 0

Thread B

reads 0

Both write

```
1
```

Expected

```
2
```

Actual

```
1
```

This is called

```
Race Condition
```

---

# synchronized

Protects shared data.

Example

```java
public synchronized void increment() {
    counter++;
}
```

Only one thread executes it at once.

---

# Synchronized Block

```java
synchronized(this) {
    counter++;
}
```

Only code inside block is locked.

Preferred.

---

Oracle favorite

What object is locked?

```
this
```

---

Static synchronized

```java
public static synchronized void method()
```

Locks

```
Class object
```

NOT

```
this
```

Huge Oracle favorite.

---

# Lock Objects

Better practice

```java
private final Object lock = new Object();

synchronized(lock) {

}
```

instead of

```
this
```

---

# 12. Race Condition

Occurs when

multiple threads

modify shared data

without synchronization.

---

Example

```java
counter++;
```

Not atomic.

Actually

```
read

increment

write
```

Three operations.

Oracle loves asking this.

---

# 13. Deadlock (Awareness)

Very basic.

Example

Thread A

locks A

waits B

Thread B

locks B

waits A

Nobody continues.

Deadlock.

Oracle only expects recognition.

---

# 14. ExecutorService

Modern Java rarely creates Thread directly.

Instead

```java
ExecutorService executor
```

Oracle likes this API.

---

Import

```java
import java.util.concurrent.*;
```

---

Creating

```java
ExecutorService service =
    Executors.newSingleThreadExecutor();
```

or

```java
Executors.newFixedThreadPool(4);
```

Know these names.

---

Submitting Runnable

```java
service.submit(() ->
    System.out.println("Hello"));
```

---

Submitting Callable

Runnable

returns nothing

Callable

returns value

Example

```java
Callable<Integer> task =
    () -> 10;
```

Submit

```java
Future<Integer> future =
    service.submit(task);
```

---

# Future

Represents future result.

```java
Integer x =
    future.get();
```

May block.

Throws checked exceptions.

Oracle likes asking

Which interface returns value?

Answer

```
Callable
```

---

# Runnable vs Callable

| Runnable              | Callable            |
| --------------------- | ------------------- |
| run()                 | call()              |
| void                  | returns value       |
| no checked exceptions | may throw Exception |
| older                 | newer               |

Oracle favorite.

---

# shutdown()

Very important.

```java
service.shutdown();
```

Stops accepting

new tasks.

Already submitted tasks finish.

Oracle asks this frequently.

---

shutdownNow()

```java
service.shutdownNow();
```

Attempts immediate stop.

No guarantee.

Returns tasks

that never started.

Huge exam favorite.

---

# awaitTermination()

```java
service.shutdown();

service.awaitTermination(
        1,
        TimeUnit.MINUTES);
```

Waits until executor finishes.

---

# invokeAll()

Executes

multiple Callable tasks.

Returns

```
List<Future<T>>
```

---

# invokeAny()

Runs many tasks.

Returns

first completed result.

Oracle favorite.

---

# Scheduled Executor

```java
Executors.newScheduledThreadPool(2);
```

Can schedule future execution.

Know it exists.

---

# ExecutorService Lifecycle

```
create

↓

submit

↓

shutdown

↓

terminated
```

Oracle loves asking

What happens if submit is called after shutdown?

```
RejectedExecutionException
```

Very common exam trap.

---

# Thread Safety

Immutable objects

are naturally thread-safe.

Example

```
String
LocalDate
Integer
```

Mutable collections

```
ArrayList

HashMap
```

are NOT thread-safe.

---

# Atomic Classes

Example

```java
AtomicInteger
```

Provides atomic operations.

Oracle only expects awareness.

---

# Common Checked Exceptions

| Method       | Exception                                |
| ------------ | ---------------------------------------- |
| sleep()      | InterruptedException                     |
| join()       | InterruptedException                     |
| Future.get() | InterruptedException, ExecutionException |

Very common.

---

# Oracle Certification Traps

## Trap 1

```java
t.run();
```

NOT a new thread.

---

## Trap 2

```java
t.start();
t.start();
```

Illegal.

Throws

```
IllegalThreadStateException
```

A thread can only be started once.

---

## Trap 3

```java
Thread.sleep()
```

does NOT release monitor lock.

---

## Trap 4

```java
shutdown()
```

does NOT stop running tasks.

---

## Trap 5

```java
shutdownNow()
```

No guarantee.

---

## Trap 6

Submitting after shutdown

```
RejectedExecutionException
```

---

## Trap 7

Runnable

cannot return value.

Callable can.

---

## Trap 8

```
synchronized static
```

locks

```
Class
```

not object.

---

## Trap 9

```
counter++
```

Not atomic.

---

## Trap 10

```
ExecutorService
```

must usually be shut down.

Oracle likes asking which code leaks threads.

---

# Things to Memorize

✓ Thread implements Runnable.

✓ Runnable is a functional interface.

✓ start() creates a new thread.

✓ run() is a normal method.

✓ Thread cannot be restarted.

✓ sleep() throws InterruptedException.

✓ join() waits.

✓ synchronized prevents race conditions.

✓ synchronized static locks Class.

✓ ExecutorService is preferred over Thread.

✓ Callable returns values.

✓ Future.get() waits.

✓ shutdown() allows submitted tasks to finish.

✓ shutdownNow() attempts immediate shutdown.

✓ submit after shutdown → RejectedExecutionException.

---

# Oracle-Style Practice Questions

### Question 1

What is the output?

```java
Thread t = new Thread(() ->
    System.out.print("A"));

t.run();
System.out.print("B");
```

**Answer**

```
AB
```

### Explanation

`run()` is an ordinary method call. It executes on the **main thread**, so `"A"` is printed before `"B"` with no concurrency involved.

---

### Question 2

Which interface can return a value?

A. Runnable

B. Thread

C. Callable

D. Executor

**Answer**

✅ C

---

### Question 3

Which statement about `Thread.start()` is correct?

A. It invokes `run()` on the current thread.

B. It creates a new thread and eventually executes `run()`.

C. It may be called multiple times on the same thread.

D. It always blocks until the thread completes.

**Answer**

✅ B

---

### Question 4

What happens?

```java
Thread t = new Thread(() -> { });

t.start();
t.start();
```

A. Compiles and runs.

B. The second call creates another thread.

C. Throws `IllegalThreadStateException`.

D. Throws `InterruptedException`.

**Answer**

✅ C

---

### Question 5

What is locked by a `static synchronized` method?

A. `this`

B. The current thread

C. The class object (`Class<?>`)

D. The method itself

**Answer**

✅ C

---

### Question 6

Which method should normally be called when you are finished with an `ExecutorService`?

A. `close()`

B. `stop()`

C. `shutdown()`

D. `terminate()`

**Answer**

✅ C

---

### Question 7

What exception is thrown when submitting a task after calling `shutdown()`?

A. `IllegalStateException`

B. `RejectedExecutionException`

C. `ExecutionException`

D. `InterruptedException`

**Answer**

✅ B

---

### Question 8

Which statements are true? (Choose **two**.)

```java
ExecutorService service =
        Executors.newSingleThreadExecutor();

Future<Integer> future =
        service.submit(() -> 42);
```

A. `submit()` returns a `Future<Integer>`.

B. The lambda is a `Runnable`.

C. `future.get()` may block.

D. `shutdown()` is called automatically.

**Answer**

✅ A and C

---

### Question 9

Which statement about `Thread.sleep()` is correct?

A. It releases any monitor locks held by the thread.

B. It always terminates the thread.

C. It pauses the current thread and may throw `InterruptedException`.

D. It can only be called from the `main` thread.

**Answer**

✅ C

---

### Question 10

What is the main reason to synchronize access to shared mutable state?

A. To improve performance.

B. To prevent race conditions and ensure memory visibility.

C. To guarantee thread creation order.

D. To avoid checked exceptions.

**Answer**

✅ B

---

# Final 1Z0-830 Exam Tips

For the Professional exam, Oracle focuses on understanding rather than advanced concurrent programming. Make sure you can confidently answer questions about:

- The difference between `Thread`, `Runnable`, and `Callable`.
- The behavior of `start()` versus `run()`.
- Basic thread lifecycle states (`NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, `TERMINATED`).
- What `synchronized` locks (`this` for instance methods/blocks, the `Class` object for `static synchronized` methods).
- Common causes of race conditions.
- The basic `ExecutorService` workflow (`create → submit → shutdown`).
- `Future.get()` blocking behavior and its checked exceptions.
- The effects of `shutdown()` versus `shutdownNow()`.
- The exceptions most commonly associated with concurrency APIs (`InterruptedException`, `ExecutionException`, `RejectedExecutionException`, `IllegalThreadStateException`).

If you are comfortable with the material above and the certification traps, you will have covered essentially everything Oracle typically expects from the Concurrency section of the Java SE 21 Professional (1Z0-830) exam.
