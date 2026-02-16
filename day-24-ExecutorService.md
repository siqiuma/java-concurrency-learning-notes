1️⃣ What is ExecutorService?
============================

ExecutorService is:

> A higher-level API for managing asynchronous task execution.

Instead of:

`new Thread(...)`

You use:

`ExecutorService executor = Executors.newFixedThreadPool(10);`

It manages:

-   Thread creation

-   Reuse

-   Task queue

-   Shutdown

* * * * *

2️⃣ Common Executor Types 
===============================================

* * * * *

🔹 FixedThreadPool
------------------

`Executors.newFixedThreadPool(n);`

-   Fixed number of threads

-   Uses unbounded LinkedBlockingQueue (⚠ important)

-   Good for controlled concurrency

Risk:

-   Unbounded queue can cause memory growth

Interview note:

> Good default, but consider bounded queue in production.

* * * * *

🔹 CachedThreadPool
-------------------

`Executors.newCachedThreadPool();`

-   Creates threads as needed

-   Reuses idle threads

-   No upper bound on threads

Risk:

-   Thread explosion

-   Resource exhaustion

Use only for:

-   Short-lived lightweight tasks

* * * * *

🔹 SingleThreadExecutor
-----------------------

`Executors.newSingleThreadExecutor();`

-   Only one worker thread

-   Tasks executed sequentially

-   Useful for ordered processing

* * * * *

🔹 ScheduledThreadPool
----------------------

`Executors.newScheduledThreadPool(n);`

-   For delayed or periodic tasks

-   Used in schedulers

* * * * *

3️⃣ Runnable vs Callable
========================

* * * * *

Runnable
--------

`executor.execute(() -> doWork());`

-   No return value

-   No checked exception

* * * * *

Callable
--------

`Future<String> future = executor.submit(() -> "result");`

-   Returns value

-   Can throw checked exception

Use Callable when:

-   You need result

-   You need error propagation

* * * * *

4️⃣ Understanding Future 
====================================

`Future<T> future = executor.submit(task);
T result = future.get();`

Important facts:

-   `get()` blocks

-   Can throw ExecutionException

-   Can use timeout

Example:

`future.get(2, TimeUnit.SECONDS);`

This prevents infinite blocking.

* * * * *

5️⃣ The Biggest Production Mistake
==================================

Forgetting to shut down executor.

Always:

`executor.shutdown();
executor.awaitTermination(timeout, unit);`

Or in shutdown hook.

Otherwise:

-   Thread leak

-   JVM won't exit

* * * * *

6️⃣ Deadlock / Thread Starvation Pattern 
=========================================================

Example:

Thread pool size = 2

Task A submits Task B and waits:

```java
Future f = executor.submit(taskB);
f.get();
```

If both threads are running Task A,\
no thread available to run Task B.

Deadlock.

This is very commonly tested.

* * * * *

7️⃣ Best Practice Pattern
=========================

Prefer:

```java
ExecutorService executor = new ThreadPoolExecutor(
    corePoolSize,
    maxPoolSize,
    keepAliveTime,
    unit,
    new ArrayBlockingQueue<>(capacity)
);
```

Instead of blindly using `Executors` factory methods.

Why?

Because factory methods often hide unbounded queues.

* * * * *

8️⃣ How This Applies to Your Scheduler
======================================

Your scheduler likely uses:

-   Fixed thread pool

-   Possibly scheduled executor

Important considerations:

-   Bounded queue

-   Proper shutdown

-   Avoid blocking inside pool unnecessarily

-   Align pool size with DB capacity

1️⃣ Why is `newCachedThreadPool` dangerous?
-------------------------------------------

> `newCachedThreadPool` has no upper bound on thread creation. Under high load, it may create an unbounded number of threads, leading to excessive context switching, memory pressure, and potential resource exhaustion.

Important nuance:

-   Not just memory --- also context switching overhead.

-   Thread creation is expensive.

* * * * *

2️⃣ Why is `newFixedThreadPool` potentially risky?
--------------------------------------------------

> `newFixedThreadPool` uses an unbounded `LinkedBlockingQueue` internally. If task submission rate exceeds processing rate, tasks accumulate indefinitely in memory, potentially causing OutOfMemoryError and increased latency.

Key insight:

-   The danger is silent accumulation.

-   No rejection occurs.

-   `maxPoolSize` is effectively ignored.

* * * * *

3️⃣ What happens if you call `future.get()` inside a task running in the same pool?
-----------------------------------------------------------------------------------

> If a task running in a thread pool submits another task to the same pool and calls `future.get()`, it may cause thread starvation or deadlock. If all threads are occupied waiting for subtasks that cannot start because the pool is exhausted, the system becomes blocked.

* * * * *

4️⃣ What is the correct shutdown pattern?
-----------------------------------------

```java
executor.shutdown();  // stop accepting new tasks

if (!executor.awaitTermination(timeout, unit)) {
    executor.shutdownNow();  // force interrupt
}
```

Important nuance:

-   shutdown() → graceful

-   awaitTermination() → wait

-   shutdownNow() → force interrupt if needed
