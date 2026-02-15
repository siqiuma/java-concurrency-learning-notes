### 1️⃣ In your scheduler, what guarantees that two workers cannot process the same task simultaneously?

Two workers cannot process the same task simultaneously because task claiming is implemented using **conditional database updates (optimistic locking)**.

For example:

`UPDATE task
SET status = 'PROCESSING', worker_id = ?
WHERE id = ?
  AND status = 'PENDING';`

Only one worker will successfully update the row.\
The other will see `0 rows updated` and know it failed to claim the task.

The database enforces concurrency correctness, not Java locks.

* * * * *

### 2️⃣ Why is using synchronized to protect task claiming incorrect?

`synchronized` only controls concurrency **within a single JVM instance**.

In a real scheduler system:

-   There may be multiple worker threads.

-   There may be multiple application instances.

-   Task claiming happens in the database.

Using `synchronized` would not prevent another JVM instance from claiming the same task.\
Therefore, concurrency must be enforced at the database level, not the application memory level.

* * * * *

### 3️⃣ What is the real bottleneck of your scheduler?

The real bottlenecks are:

-   Database connection pool capacity

-   Database query/update latency

-   Downstream service response time

Since tasks are IO-bound, performance is limited by external systems, not CPU.

Thread pool sizing and backpressure must align with these bottlenecks.

* * * * *

### 4️⃣ What concurrency mistake would most likely cause a production incident?

Several realistic risks:

-   Incorrect task-claiming condition leading to double execution

-   Retry storms caused by poor backoff strategy

-   Thread pool misconfiguration causing overload and latency spikes

-   Holding locks during IO operations (lock retention)

-   Ignoring DB transaction isolation and causing inconsistent state

The most critical risk is double execution due to improper claim logic.

### Concurrency is primarily controlled at the database level using conditional updates for task claiming. The application layer manages execution concurrency via a bounded thread pool aligned with database connection capacity, while in-memory synchronization is limited to simple metrics and flags.
