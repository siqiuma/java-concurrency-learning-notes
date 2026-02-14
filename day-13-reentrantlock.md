### 1️⃣ What problem does ReentrantLock solve that synchronized does not?

ReentrantLock provides additional control over locking behavior compared to synchronized, including:

-   `tryLock()` --- allows non-blocking lock attempts

-   `lockInterruptibly()` --- allows waiting threads to be interrupted

-   optional fairness policy (FIFO ordering)

-   explicit lock management for more complex coordination

Unlike synchronized, it allows finer-grained control over how threads acquire and release locks.

* * * * *

### 2️⃣ Why must unlock be in a finally block?

ReentrantLock is an explicit lock, meaning the programmer is responsible for both acquiring and releasing it.

If an exception occurs inside the critical section and `unlock()` is not called, the lock will remain held, causing other threads to block indefinitely.

Placing `unlock()` inside a `finally` block guarantees that the lock is always released, preventing deadlocks caused by accidental lock retention.

* * * * *

### 3️⃣ When would tryLock be useful?

`tryLock()` is useful when:

-   you want to avoid blocking indefinitely

-   you want to implement fail-fast behavior

-   you want to perform alternative logic if the lock is unavailable

It is especially helpful in preventing deadlocks or reducing contention in high-contention scenarios.

* * * * *

### 4️⃣ Would you use fair or non-fair lock in a high-throughput backend system, and why?

In a high-throughput backend system, I would typically use a non-fair lock.

Non-fair locks allow higher throughput because threads can "barge" and acquire the lock immediately if it becomes available, reducing context switching and scheduling overhead.

Fair locks improve predictability but reduce overall performance due to strict ordering.

In latency-sensitive systems, throughput and efficiency are usually prioritized over strict fairness.

## ReentrantLock is not necessarily better than synchronized, but it provides more control over locking behavior in complex concurrent scenarios.
Lock retention occurs when a thread holds a lock longer than necessary, increasing contention and reducing system throughput.
