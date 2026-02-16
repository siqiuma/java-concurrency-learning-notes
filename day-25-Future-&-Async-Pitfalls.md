1️⃣ Why can Future lead to thread starvation?
---------------------------------------------
> Future can lead to thread starvation when tasks running in a thread pool submit other tasks to the same pool and block on `future.get()`. If all pool threads are occupied waiting for subtasks that cannot start due to lack of available threads, the system becomes deadlocked due to thread starvation.

Important nuance:

-   It's not just dependency.

-   It's blocking + limited thread pool capacity.

-   This is called "thread starvation deadlock."

* * * * *

2️⃣ Why is ignoring timeout dangerous in IO systems?
----------------------------------------------------

> In IO-bound systems, threads often wait on external resources such as databases or APIs. If `future.get()` is called without a timeout and the downstream system hangs, threads may block indefinitely. This can exhaust thread pools and connection pools, causing cascading system stalls.

Key improvement:

-   Mention cascading failure.

-   Mention pool exhaustion.

* * * * *

3️⃣ Why is Future considered non-composable?
--------------------------------------------

> Future is considered non-composable because it does not provide built-in mechanisms for chaining asynchronous operations. You must manually call `get()` to retrieve results and then submit subsequent tasks, which leads to blocking and awkward control flow.

Important nuance:

-   Composition means chaining async without blocking.

-   Future forces manual coordination.

* * * * *

4️⃣ What is the biggest design limitation of Future?
----------------------------------------------------

> The biggest design limitation of Future is that it represents a one-time result retrieval mechanism with blocking semantics and no built-in support for asynchronous composition, transformation, or combination of results.
