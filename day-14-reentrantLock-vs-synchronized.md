### 1️⃣ Is ReentrantLock always faster than synchronized? Why or why not?

No, ReentrantLock is not always faster than synchronized.

Both mechanisms can suffer from contention. Modern JVM implementations heavily optimize synchronized, especially under low contention.

ReentrantLock provides more flexibility and control (e.g., tryLock, interruptible locking, fairness policies), but it does not inherently guarantee better performance.

Performance differences depend on workload and contention level.

* * * * *

### 2️⃣ When would synchronized be preferable?

synchronized is preferable when:

-   the critical section is small

-   advanced lock features are unnecessary

-   simpler, safer code is desired

-   interruptible locking or timed acquisition is not required

It is less error-prone because lock release is automatic.

* * * * *

### 3️⃣ Give one real scenario where tryLock prevents a production issue.

A real scenario:

Suppose multiple worker threads periodically update a shared cache or statistics object.

If a thread attempts to acquire the lock but the lock is heavily contended, blocking indefinitely could increase latency and thread backlog.

Using `tryLock()` allows the thread to:

-   fail fast if the lock is unavailable

-   skip the update

-   or execute alternative logic

This prevents excessive blocking and reduces contention under high load.

* * * * *

### 4️⃣ What is the biggest danger of using ReentrantLock?

The biggest danger is forgetting to call `unlock()` in a `finally` block.

Because lock release is manual, failure to release the lock can cause permanent lock retention, leading to blocked threads and potential system deadlock.

## tryLock is useful when blocking indefinitely would harm system responsiveness.

__Interruptible lock waiting__ allows a blocked thread to respond to interruption signals instead of waiting indefinitely for a lock, improving system responsiveness and shutdown behavior.

__Thread backlog__ occurs when many threads are blocked waiting for resources such as locks, IO, or database connections, leading to increased latency and reduced throughput.
