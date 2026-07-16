Absolutely. The **Concurrency** section is one of Oracle's favorite areas because it combines API knowledge with subtle behavior. The exam usually focuses on:

- recognizing which code compiles
- understanding thread states
- ExecutorService lifecycle
- synchronization behavior
- Callable/Future differences
- Atomic classes
- concurrent collections
- identifying race conditions
- avoiding deadlocks (basic awareness)

Below is a **mock exam** written in Oracle's style.

---

# Java SE 21 Developer Professional (1Z0-830)

# Mock Exam — Concurrency

**Difficulty:** Oracle-style

**Questions:** 25

---

# Question 1

Which statements about `Thread` and `Runnable` are correct?

```java
class Task implements Runnable {
    public void run() {
        System.out.print("Run ");
    }
}

public class Test {
    public static void main(String[] args) {
        Runnable r = new Task();
        Thread t = new Thread(r);
        t.start();
    }
}
```

Choose TWO.

A. `Runnable` represents the task.

B. `Thread` represents the execution mechanism.

C. Calling `start()` directly invokes `run()` on the current thread.

D. Calling `run()` starts a new thread.

E. `Runnable` cannot be reused.

---

## Answer

✅ A

✅ B

### Explanation

Oracle loves this distinction.

- Runnable = work
- Thread = worker

```
run()   -> normal method
start() -> creates new thread
```

---

# Question 2

Given

```java
Thread t = new Thread(() ->
    System.out.println("Hello"));

t.run();
System.out.println("Done");
```

What is guaranteed?

A.

```
Hello
Done
```

B.

```
Done
Hello
```

C. Order unpredictable

D. Compilation error

---

## Answer

✅ A

### Explanation

`run()` is just a normal method.

No new thread.

---

# Question 3

Given

```java
Thread t = new Thread(() ->
    System.out.print("A"));

t.start();
System.out.print("B");
```

Possible output?

Choose TWO.

A.

```
AB
```

B.

```
BA
```

C.

```
AA
```

D.

```
BB
```

---

## Answer

✅ A

✅ B

### Explanation

Scheduling is nondeterministic.

Oracle loves this question.

---

# Question 4

Which methods belong to `Thread`?

Choose THREE.

A. start()

B. join()

C. run()

D. submit()

E. shutdown()

---

## Answer

✅ A

✅ B

✅ C

---

# Question 5

Which state can a thread enter after calling `sleep()`?

A. NEW

B. WAITING

C. TIMED_WAITING

D. TERMINATED

---

## Answer

✅ C

---

# Question 6

Which state immediately follows

```java
Thread t = new Thread(...);
```

A. RUNNABLE

B. NEW

C. BLOCKED

D. WAITING

---

## Answer

✅ B

---

# Question 7

Which state immediately follows

```java
t.start();
```

A. NEW

B. RUNNABLE

C. WAITING

D. TERMINATED

---

## Answer

✅ B

---

# Question 8

What does synchronization guarantee?

Choose TWO.

A. Mutual exclusion

B. Atomicity of synchronized block

C. Faster execution

D. Prevents all deadlocks

---

## Answer

✅ A

✅ B

---

# Question 9

Given

```java
class Counter {

    int value;

    synchronized void increment() {
        value++;
    }
}
```

Which statement is correct?

A. Only one thread can execute `increment()` on the same object.

B. Multiple threads may execute simultaneously.

C. Synchronization affects all objects.

D. Synchronization makes `value` immutable.

---

## Answer

✅ A

---

# Question 10

Given

```java
ExecutorService service =
        Executors.newSingleThreadExecutor();

service.submit(() ->
    System.out.println("Hello"));
```

What is missing?

A. start()

B. shutdown()

C. run()

D. execute()

---

## Answer

✅ B

### Explanation

Oracle frequently asks this.

Always remember:

```
shutdown()
```

or the application may never terminate.

---

# Question 11

Which method belongs to `ExecutorService`?

Choose TWO.

A. execute()

B. submit()

C. start()

D. join()

---

## Answer

✅ A

✅ B

---

# Question 12

What does `submit()` return?

A. Runnable

B. Future

C. Callable

D. Thread

---

## Answer

✅ B

---

# Question 13

Which interface returns a value?

A. Runnable

B. Callable

C. Thread

D. ExecutorService

---

## Answer

✅ B

---

# Question 14

Which method belongs to `Callable`?

A.

```java
run()
```

B.

```java
call()
```

C.

```java
execute()
```

D.

```java
submit()
```

---

## Answer

✅ B

---

# Question 15

Given

```java
Callable<Integer> c = () -> 10;
```

Which is correct?

A.

```java
Integer x = c.run();
```

B.

```java
Integer x = c.call();
```

C.

```java
Integer x = c.submit();
```

D.

Compilation error

---

## Answer

✅ B

---

# Question 16

Given

```java
ExecutorService service =
    Executors.newSingleThreadExecutor();

Future<Integer> f =
    service.submit(() -> 42);

System.out.println(f.get());

service.shutdown();
```

Output?

A. 42

B. Future object

C. null

D. Compilation error

---

## Answer

✅ A

---

# Question 17

What does `Future.get()` do?

Choose TWO.

A. Returns result.

B. Waits if necessary.

C. Starts task.

D. Cancels task.

---

## Answer

✅ A

✅ B

---

# Question 18

Which method schedules future execution?

A.

```
ExecutorService
```

B.

```
ScheduledExecutorService
```

C.

```
Future
```

D.

```
Callable
```

---

## Answer

✅ B

---

# Question 19

Which method schedules repeated execution?

A.

```
schedule()
```

B.

```
scheduleAtFixedRate()
```

C.

```
submit()
```

D.

```
runLater()
```

---

## Answer

✅ B

---

# Question 20

Given

```java
AtomicInteger ai =
        new AtomicInteger(5);

ai.incrementAndGet();

System.out.println(ai.get());
```

Output?

A.

```
5
```

B.

```
6
```

C.

Compilation error

D.

Runtime exception

---

## Answer

✅ B

---

# Question 21

Why use `AtomicInteger` instead of `int`?

Choose TWO.

A. Thread-safe updates

B. Lock-free operations

C. Immutable

D. Faster than every operation

---

## Answer

✅ A

✅ B

---

# Question 22

Which class provides explicit locking?

A.

```
Lock
```

B.

```
ReentrantLock
```

C.

```
AtomicLock
```

D.

```
ExecutorLock
```

---

## Answer

✅ B

---

# Question 23

Which collection is thread-safe?

Choose TWO.

A.

```
ConcurrentHashMap
```

B.

```
CopyOnWriteArrayList
```

C.

```
HashMap
```

D.

```
ArrayList
```

---

## Answer

✅ A

✅ B

---

# Question 24

Which statement about concurrent collections is correct?

A. They eliminate all synchronization.

B. They are designed for concurrent access.

C. They cannot be modified.

D. They always lock the whole collection.

---

## Answer

✅ B

---

# Question 25 (Oracle Favorite)

Given

```java
ExecutorService service =
        Executors.newSingleThreadExecutor();

Future<String> future =
        service.submit(() -> "Java");

System.out.println("A");

System.out.println(future.get());

System.out.println("B");

service.shutdown();
```

Which output is guaranteed?

A.

```
A
Java
B
```

B.

```
Java
A
B
```

C.

```
A
B
Java
```

D. Order unpredictable

---

## Answer

✅ A

### Explanation

`future.get()` blocks until the task completes. Therefore:

1. `"A"` is printed.
2. `future.get()` waits (if necessary) and then returns `"Java"`.
3. `"Java"` is printed.
4. `"B"` is printed.

The order is guaranteed.

---

# Oracle Certification Traps & Gotchas

These are patterns Oracle repeatedly tests:

| Trap                      | Correct Answer                                                                                                                 |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `run()` vs `start()`      | `run()` executes on the current thread; `start()` creates a new thread.                                                        |
| `Runnable` vs `Callable`  | `Runnable` returns no result and cannot throw checked exceptions; `Callable` returns a value and may throw checked exceptions. |
| `submit()` vs `execute()` | `submit()` returns a `Future`; `execute()` returns `void`.                                                                     |
| Forgetting `shutdown()`   | An `ExecutorService` should be shut down to allow orderly termination.                                                         |
| `Future.get()`            | Blocks until the computation completes (unless already done).                                                                  |
| Thread scheduling         | The execution order of concurrently running threads is generally unpredictable.                                                |
| `sleep()`                 | Puts the current thread into the `TIMED_WAITING` state; it does **not** release a monitor lock.                                |
| `join()`                  | Causes the calling thread to wait for another thread to finish.                                                                |
| `synchronized`            | Locks the monitor of the target object (or the `Class` object for static synchronized methods).                                |
| `AtomicInteger`           | Provides atomic, thread-safe operations without explicit locking.                                                              |
| `ConcurrentHashMap`       | Designed for concurrent access and preferred over synchronizing a `HashMap` for many use cases.                                |
| `CopyOnWriteArrayList`    | Safe for concurrent iteration; writes create a new underlying array and are relatively expensive.                              |
| `ReentrantLock`           | Offers explicit locking with features such as `lock()`, `unlock()`, and `tryLock()`.                                           |
| Thread states             | `NEW → RUNNABLE → (BLOCKED / WAITING / TIMED_WAITING) → TERMINATED`; transitions depend on scheduling and synchronization.     |

## Exam Readiness

If you can confidently answer these 25 questions and explain **why** each answer is correct, you'll have covered the core concurrency concepts that Oracle typically emphasizes for the Java SE 21 Developer Professional (1Z0-830) exam.
