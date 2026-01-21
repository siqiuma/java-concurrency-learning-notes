## Day 1 --- What is concurrency & why we need it

#    What is a thread

A thread is an independent path of execution **within a process, sharing the same** memory space

#    What is a process

A process is an independent execution unit with **its own** memory space, including stacks, heap, and OS resources.

#    What is concurrent vs parallel

concurrent: Logical concept --- multiple tasks make progress during overlapping time periods. The execution order is not deterministic

parallel: physical concept --- multiple tasks run simultaneously on different CPUs or cores. (Implementation level concept. At least two executors. Example: multicore machines, Hadoop clusters)

#    Why multi-threading exists

A Java application runs in a single process by default, but can create multiple threads to:

-   improve throughput
-   perform asynchronous operations
-   utilize multi-core CPUs
-   Why it is dangerous

Concurrency improves throughput but introduces race conditions when multiple threads modify shared state
