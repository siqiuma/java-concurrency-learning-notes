### 1️⃣ Are your scheduler tasks CPU-bound or IO-bound? Why?

My scheduler tasks are **IO-bound**.

They primarily involve:

-   database reads and updates

-   network calls to downstream systems

Most of the task execution time is spent **waiting on external systems**, not performing CPU-intensive computation. Therefore, threads are frequently blocked on IO rather than consuming CPU cycles.

* * * * *

### 2️⃣ What resource likely becomes the bottleneck first?

The first bottleneck is likely:

-   **database connection pool capacity**

-   downstream service latency

Since each task interacts with the database, the DB connection pool becomes the primary concurrency limiter. If downstream services slow down, they can also introduce backpressure and increase task execution time.

* * * * *

### 3️⃣ If your DB pool has 20 connections, what thread pool size would you choose and why?

I would choose a thread pool size between **10 and 20**, depending on workload characteristics.

The pool size should be bounded by the database connection pool capacity because the database is the true bottleneck. If each task requires a DB connection, the number of concurrent threads should not exceed the number of available DB connections.

Thread pool sizing must align with the capacity of the slowest downstream resource to prevent overload.

* * * * *

### 4️⃣ What happens if you choose 100 threads?

If I configure 100 threads while the DB pool only allows 20 connections:

-   Only 20 threads can actively use DB connections.

-   The remaining threads block waiting for connections.

-   This increases context switching overhead.

-   It can cause connection timeouts.

-   Retry storms may occur.

-   Latency becomes unstable.

Instead of improving throughput, excessive threads increase contention and reduce overall system stability.

## __Thread pool size should align with downstream resource capacity; otherwise, excessive threads only increase contention without improving throughput.__

Contention occurs when multiple threads compete for the same limited resource, causing blocking, context switching, and reduced throughput.
