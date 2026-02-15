### 1️⃣ When is Atomic the correct choice?

Atomic is the correct choice when performing **simple atomic updates on a single independent piece of shared state**, such as:

-   counters

-   flags

-   reference swaps\
    and when the operation does not need to maintain invariants across multiple variables.

* * * * *

### 2️⃣ When is synchronized preferable to ReentrantLock?

synchronized is preferable when:

-   the critical section is small and straightforward

-   you only need mutual exclusion

-   you don't need timed lock attempts, interruptible waits, fairness, or condition variables\
    It's simpler and less error-prone because lock release is automatic.

* * * * *

### 3️⃣ When would ReentrantLock be justified?

ReentrantLock is justified when you need advanced control such as:

-   `tryLock()` or timed lock acquisition

-   `lockInterruptibly()` for responsive cancellation

-   fairness policies

-   multiple `Condition` variables for complex coordination

* * * * *

### 4️⃣ In your scheduler, name one place atomic is appropriate and one place it is not.

**Appropriate:** in-memory counters/metrics, e.g.:

-   `tasksProcessed++`

-   `failedTasks++`\
    using `AtomicInteger/LongAdder`

**Not appropriate:** task claiming / status transitions that update multiple DB fields (status, workerId, timestamps, attemptCount, etc.).\
Those require DB-level atomicity (transaction/locking/optimistic version checks), not in-memory atomics.

### Atomics are great for single-variable in-memory state, but database-backed state transitions require transactional guarantees, not CAS in the application.
