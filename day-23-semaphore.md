1️⃣ How is Semaphore different from a lock?
-------------------------------------------

> A lock (like synchronized or ReentrantLock) allows only one thread to enter a critical section at a time.\
> A Semaphore allows a configurable number of threads (permits) to access a resource concurrently, enabling controlled parallelism instead of full mutual exclusion.

Important nuance:

-   Lock protects a critical section.

-   Semaphore controls access count to a resource.

* * * * *

2️⃣ Why is Semaphore useful in IO-bound systems?
------------------------------------------------


> In IO-bound systems, threads spend significant time waiting on external resources such as databases or APIs. A Semaphore allows you to limit the number of concurrent IO operations to match downstream capacity, preventing overload while still allowing parallelism.

Important idea:

-   Align concurrency with resource capacity.

-   Protect downstream systems.

* * * * *

3️⃣ In your scheduler, give one concrete place Semaphore could prevent production issues.
-----------------------------------------------------------------------------------------

> In the scheduler, Semaphore could be used to limit concurrent database or downstream API calls. For example, if the DB connection pool allows 20 connections, a Semaphore with 20 permits can ensure no more than 20 concurrent task executions attempt DB access at the same time, preventing connection exhaustion and timeout cascades.

* * * * *

4️⃣ What happens if release() is forgotten?
-------------------------------------------

> If release() is forgotten, the permit is never returned to the Semaphore. Over time, all permits may be consumed, causing all future acquire() calls to block indefinitely, effectively creating a system stall.

Key distinction:

-   This is not a lock. It is a permit leak.

### Semaphore is useful for limiting concurrent access to scarce IO resources to prevent overload and stabilize system throughput.

```java
Semaphore dbLimit = new Semaphore(20);

dbLimit.acquire();
try {
    callDatabase();
} finally {
    dbLimit.release();
}
```
This prevents overload.
