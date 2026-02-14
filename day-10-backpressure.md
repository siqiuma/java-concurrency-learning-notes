### What happens when using an unbounded queue under load?

With an unbounded queue, the queue never fills up, so the thread pool typically never grows beyond `corePoolSize`.\
Only core threads execute tasks, even under heavy load.

As a result:

-   tasks continuously accumulate in the queue

-   task latency grows without bound

-   tasks are never rejected

-   memory usage increases indefinitely

This leads to **silent overload**, where the system appears responsive but eventually crashes due to memory exhaustion.

* * * * *

### Why does maxPoolSize often become meaningless with unbounded queues?

New threads beyond `corePoolSize` are created **only when the queue is full** and active threads are below `maxPoolSize`.

With an unbounded queue:

-   the queue never becomes full

-   new threads are never created

-   `maxPoolSize` is effectively ignored

Tasks are always queued instead of triggering pool expansion.

* * * * *

### How does a bounded queue apply backpressure?

A bounded queue has a fixed capacity.\
When the queue is full and active threads have reached `maxPoolSize`, new task submissions are rejected.

This rejection:

-   forces the caller to slow down, retry, or fail fast

-   prevents the system from accepting more work than it can handle

-   propagates backpressure upstream

* * * * *

### In my scheduler, why is a bounded queue safer?

A bounded queue ensures that:

-   the number of in-memory tasks is limited

-   overload is detected early

-   the system fails fast instead of degrading silently

This protects system stability and allows controlled recovery strategies such as retrying or rescheduling tasks.

__A bounded queue applies backpressure by limiting in-memory tasks and forcing the system to fail fast under overload instead of crashing later.__
