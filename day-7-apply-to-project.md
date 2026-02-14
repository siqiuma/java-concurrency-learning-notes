# Day 7 --- Apply to project

##   Where is shared state?

The shared mutable state in my task scheduler is the **task record stored in the database**.

Each task contains fields such as:

-   status
-   attemptCount
-   nextRunAt
-   scheduledFor
-   workerId
-   lastError
-   createdAt / updatedAt

Since multiple worker threads may access and modify the same task row, the database becomes the shared resource.

##   Which variable could be accessed by multiple threads?

The following fields may be accessed concurrently by multiple workers:

-   status
-   workerId
-   attemptCount
-   lastError
-   nextRunAt
-   updatedAt
-   scheduledFor

This happens because multiple worker threads may attempt to **claim or update the same task at the same time**, especially under high concurrency.

##   Where would race condition occur?

Race conditions can occur during **task claiming**, when multiple threads attempt to claim the same task concurrently.

This typically follows a read--modify--write pattern:

1.  Multiple threads read the same task with status = PENDING
2.  Each thread believes it can claim the task
3.  Multiple threads attempt to update the same task row

Without proper synchronization (e.g., atomic updates, locking, or version checks), this can lead to:

-   duplicate task execution
-   incorrect attempt counts
-   inconsistent task state

Concurrency problems are not limited to in-memory variables --- they frequently occur at the database level when multiple workers operate on the same records.
