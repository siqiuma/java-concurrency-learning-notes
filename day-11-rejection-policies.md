### 1️⃣ What happens when AbortPolicy is used?

When the thread pool cannot accept a task, it throws a `RejectedExecutionException`.

The task is not executed, and it is the caller's responsibility to:

-   catch the exception

-   retry later

-   reschedule

-   or fail gracefully

AbortPolicy enforces **fail-fast behavior**.
The caller is the thread or component that submitted the task to the thread pool. When rejection occurs, it is responsible for handling the exception or applying retry logic.
* * * * *

### 2️⃣ Why does CallerRunsPolicy apply backpressure?

With `CallerRunsPolicy`, the submitting thread executes the task itself.

This slows down task submission because:

-   the producer thread is now busy doing work

-   it cannot continue submitting tasks at full speed

This naturally reduces load and propagates backpressure upstream.

* * * * *

### 3️⃣ Which rejection policy would you choose for your scheduler, and why?

For my scheduler, I would likely choose **CallerRunsPolicy** or **AbortPolicy**, depending on system goals.

-   CallerRunsPolicy is useful because it slows down submission without losing tasks, applying natural backpressure.

-   AbortPolicy is useful when I want to fail fast and explicitly handle retries or rescheduling logic.

For a task scheduler that values correctness and controlled retries, either policy can work, but silent discarding would not be appropriate.

* * * * *

### 4️⃣ Why is silent discard dangerous?

Silent discard (DiscardPolicy) is dangerous because tasks are dropped without error or visibility.

In systems that care about correctness, this can lead to:

-   lost work

-   inconsistent state

-   hard-to-debug failures

It hides overload instead of signaling it.

__AbortPolicy makes overload explicit, while CallerRunsPolicy makes overload self-regulating.__
