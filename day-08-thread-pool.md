### Why is `new Thread()` dangerous?

Creating a new thread per task is dangerous for the following reasons:

1.  **High resource cost**\
    Each thread consumes OS-level resources, including stack memory, scheduling overhead, and context switching cost.

2.  **Unbounded thread creation leads to resource exhaustion**\
    Without proper limits, creating threads in response to load spikes can exhaust memory and CPU resources, causing the system to become unstable or crash.

3.  **No thread reuse**\
    Threads are created and destroyed repeatedly, which is inefficient and increases overhead compared to reusing existing worker threads.

* * * * *

### What problem does a thread pool solve?

A thread pool **limits the number of active threads and reuses them**, which:

-   controls concurrency

-   applies backpressure under load

-   prevents resource exhaustion

-   improves system stability

* * * * *

### In my scheduler, what would happen without a thread pool?

Without a thread pool, each task execution would create a new thread.\
When many tasks become ready at the same time, a large number of threads would be created concurrently.

This can lead to:

-   resource exhaustion (memory, CPU, DB connections)

-   excessive context switching

-   system instability or crashes

Additionally, since threads are not reused, thread creation becomes unnecessarily expensive and inefficient.

#### Creating a new thread per task is dangerous because it provides no concurrency control or backpressure, which can lead to resource exhaustion under load spikes.

__Context switching__ occurs when the CPU switches between threads. It requires saving and restoring thread state, causes cache misses, and consumes CPU time without doing useful work. Excessive context switching reduces throughput and system stability.
