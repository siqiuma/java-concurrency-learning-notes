### What is corePoolSize?

`corePoolSize` is the **baseline number of worker threads** kept in the thread pool to execute tasks.\
If fewer than `corePoolSize` threads are running, the pool creates new threads instead of queuing tasks.

* * * * *

### What role does the queue play?

The queue acts as a **buffer** for tasks waiting to be executed.

It determines whether:

-   tasks should wait for existing threads, or

-   the pool should grow beyond `corePoolSize`

The queue is a key mechanism for **smoothing load spikes** and applying backpressure.

* * * * *

### When is maxPoolSize used?

`maxPoolSize` defines the **upper limit** on the number of threads in the pool.

It is used **only when**:

-   all core threads are busy, and

-   the queue is full

If the queue is unbounded, the pool will typically never grow beyond `corePoolSize`.

* * * * *

### Task submission logic (step by step)

When a task is submitted to a `ThreadPoolExecutor`, the following logic is applied:

1.  **If active threads < corePoolSize**\
    → create a new thread to execute the task immediately

2.  **Else if the queue is not full**\
    → enqueue the task to wait for an available thread

3.  **Else if active threads < maxPoolSize**\
    → create a new non-core thread to execute the task

4.  **Else**\
    → reject the task according to the rejection policy

__ThreadPoolExecutor decides whether to execute, queue, or reject tasks based on corePoolSize, queue capacity, and maxPoolSize.__
