1️⃣ Why is busy-waiting bad compared to BlockingQueue?
------------------------------------------------------

> Busy-waiting continuously checks a condition in a loop without yielding the CPU, consuming CPU cycles while doing no productive work. BlockingQueue, by contrast, blocks threads efficiently using OS-level parking mechanisms, freeing CPU resources until work becomes available.

* * * * *

2️⃣ Why is an unbounded LinkedBlockingQueue dangerous?
------------------------------------------------------

> An unbounded LinkedBlockingQueue never fills up, so ThreadPoolExecutor never expands beyond corePoolSize. Tasks accumulate in memory instead of triggering pool growth or rejection. Under sustained load, this can lead to unbounded memory usage, increased latency, and eventually OutOfMemoryError.

* * * * *

3️⃣ How does BlockingQueue provide backpressure?
------------------------------------------------

> BlockingQueue provides backpressure by blocking producers when the queue reaches its capacity. This prevents the system from accepting more work than it can process, slowing down task submission and stabilizing throughput under load.

* * * * *

4️⃣ In your scheduler, who is producer and who is consumer?
-----------------------------------------------------------

> The producer is the scheduler component that retrieves tasks from the database and submits them to the execution queue. The consumers are the worker threads in the thread pool that take tasks from the queue and execute them.
>
> ### BlockingQueue enables efficient producer–consumer coordination by combining thread-safe exchange with built-in backpressure and blocking semantics.
