1️⃣ What is the difference between CountDownLatch and join()?
-------------------------------------------------------------

> `join()` is used to wait for the completion of a specific thread.\
> `CountDownLatch` allows one or more threads to wait until a set of independent operations (possibly across multiple threads) completes.\
> Unlike `join()`, CountDownLatch separates coordination logic from thread ownership and can coordinate arbitrary tasks, not just specific Thread objects.

* * * * *

2️⃣ Why is CountDownLatch one-time use?
---------------------------------------

> CountDownLatch is one-time use because once its internal count reaches zero, it cannot be reset. It is designed for one-shot synchronization. If reusable coordination is needed, `CyclicBarrier` or `Phaser` should be used instead.


* * * * *

3️⃣ In your scheduler, where could CountDownLatch be useful?
------------------------------------------------------------

-   Waiting for all worker threads to finish during graceful shutdown.

-   Coordinating completion of parallel downstream calls before marking a task as COMPLETED.

-   Coordinating batch execution where a job is split into multiple parallel subtasks and must wait for all to finish.

* * * * *

4️⃣ What happens if countDown() is never called?
------------------------------------------------

> If `countDown()` is never called enough times to reduce the count to zero, threads calling `await()` will block indefinitely (unless a timeout version of `await()` is used), potentially causing deadlocks or stalled workflows.

### CountDownLatch is a one-time synchronization mechanism for coordinating completion of multiple concurrent tasks, whereas join() only waits for a specific thread.
