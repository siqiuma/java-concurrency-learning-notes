### 1️⃣ Why is volatile not sufficient for count++?

`volatile` guarantees visibility but not atomicity.

The operation `count++` consists of three steps:

1.  Read current value

2.  Increment

3.  Write new value

If multiple threads execute this concurrently, their read--modify--write operations can interleave, causing lost updates.

Therefore, `volatile` alone cannot make compound operations like increment atomic.

* * * * *

### 2️⃣ What does CAS guarantee? (CAS = Compare-And-Swap.)

CAS guarantees that a value is updated atomically only if it has not been modified since it was last read.

It ensures atomic compare-and-update without locking.

If the value has changed, the update fails and must be retried.

CAS provides lock-free atomicity.

* * * * *

### 3️⃣ When would AtomicInteger be preferable to synchronized?

AtomicInteger is preferable when:

-   the operation involves simple state updates (e.g., incrementing a counter)

-   no complex multi-step logic is required

-   contention is low to moderate

-   high scalability and non-blocking behavior are desired

It avoids blocking and reduces context switching compared to synchronized.

* * * * *

### 4️⃣ What happens under very high contention with CAS?

Under very high contention:

-   Many threads repeatedly attempt CAS

-   Failed attempts trigger retries

-   CPU cycles are consumed by spinning (A thread repeatedly retries an operation in a loop instead of sleeping or blocking.)

-   Throughput may degrade

In extreme contention scenarios, a lock-based approach may sometimes perform more predictably.

## Atomic classes provide lock-free atomicity using CAS, but under heavy contention the retry cost can reduce performance.
Atomic operations avoid blocking and therefore scale better under moderate contention, but excessive spinning under high contention can reduce performance.

__Scalability__ refers to how well a system maintains or improves throughput as concurrency increases.

Threads:  1   2   4   8   16   32   64

----------------------------------------

volatile: incorrect results

synchronized: throughput rises → flattens → drops

atomic: throughput rises → flattens → slightly drops
