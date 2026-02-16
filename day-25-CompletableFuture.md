1️⃣ What problem does `thenCompose` solve?
------------------------------------------

> `thenCompose` solves the problem of nested asynchronous results. If an async operation returns another `CompletableFuture`, `thenCompose` flattens `CompletableFuture<CompletableFuture<T>>` into `CompletableFuture<T>`, enabling proper async chaining without manual blocking.

Important nuance:

-   It's about flattening async pipelines.

-   It enables true async composition.

* * * * *

2️⃣ Why is using the default common pool dangerous in backend systems?
----------------------------------------------------------------------

> By default, `CompletableFuture.supplyAsync()` uses `ForkJoinPool.commonPool`, which is optimized for CPU-bound tasks. Using it for IO-bound operations can cause thread starvation because blocked IO threads consume limited CPU worker threads, reducing throughput and potentially stalling unrelated tasks.

Key ideas:

-   ForkJoinPool is CPU-optimized.

-   Blocking inside it is dangerous.

-   Shared pool → interference across system components.


* * * * *

3️⃣ What is the difference between `thenApply` and `thenCompose`?
-----------------------------------------------------------------

> `thenApply` is used to transform the result of a completed stage synchronously and returns a new `CompletableFuture` containing the transformed value.\
> `thenCompose` is used when the transformation itself returns another `CompletableFuture`; it flattens the nested futures to maintain a single async pipeline.

Even stronger version:

-   `thenApply` → map

-   `thenCompose` → flatMap

* * * * *

4️⃣ When would you still prefer Future over CompletableFuture?
--------------------------------------------------------------


> I would prefer `Future` in simple scenarios where only blocking result retrieval is needed and async composition is unnecessary, such as straightforward task submission with controlled lifecycle management. It is simpler and sufficient when chaining or non-blocking behavior is not required.

Key nuance:

-   Simpler API.

-   Less overhead if async chaining isn't needed.

* * * * *

CompletableFuture (Async Composition Done Right)
------------------------------------------------

1️⃣ Why CompletableFuture Exists
================================

Problem with Future:

-   `get()` blocks

-   No chaining

-   No composition

-   Manual coordination

CompletableFuture solves this by:

> Allowing asynchronous tasks to be chained, combined, transformed, and handled without blocking.

* * * * *

2️⃣ Basic Async Example
=======================

`CompletableFuture<String> future =
    CompletableFuture.supplyAsync(() -> callService());`

Unlike Future:

You don't have to immediately call get().

You can attach actions.

* * * * *

3️⃣ Async Chaining (Core Feature)
=================================

Instead of:

`A a = callA();
B b = callB(a);`

You write:

`CompletableFuture
    .supplyAsync(() -> callA())
    .thenApply(a -> callB(a));`

No blocking.

Each step runs when previous completes.

This is composition.

* * * * *

4️⃣ thenApply vs thenCompose (Very Important)
=============================================

* * * * *

🔹 thenApply
------------

Transforms result.

`.thenApply(result -> transform(result))`

Input: T\
Output: U

T = original type

U = transformed type

Synchronous transformation.

* * * * *

🔹 thenCompose
--------------

Flattens nested async calls.

If callB returns another CompletableFuture:

`.thenCompose(a -> callBAsync(a))`

Without thenCompose, you'd get:

`CompletableFuture<CompletableFuture<B>>`

thenCompose flattens it.

* * * * *

5️⃣ Running Tasks in Parallel
=============================

`CompletableFuture<A> f1 = supplyAsync(callA);
CompletableFuture<B> f2 = supplyAsync(callB);

CompletableFuture<Void> combined =
    CompletableFuture.allOf(f1, f2);`

Wait for both.

Then:

`A a = f1.join();
B b = f2.join();`

Parallel execution made easy.

* * * * *

6️⃣ Exception Handling
======================

`future.exceptionally(ex -> fallbackValue);`

Or:

`future.handle((result, ex) -> { ... });`

No need to unwrap ExecutionException.

Cleaner error handling than Future.

* * * * *

7️⃣ Why CompletableFuture Is Safer
==================================

-   No forced blocking

-   Encourages async flow

-   Built-in composition

-   Built-in exception handling

-   Easier parallel coordination

* * * * *

8️⃣ Common Production Mistake
=============================

Forgetting executor:

`CompletableFuture.supplyAsync(...)`

Uses ForkJoinPool.commonPool by default.

Danger:

-   CPU pool used for IO tasks

-   Starvation

Better:

`CompletableFuture.supplyAsync(task, customExecutor);`

Very important in backend systems.

* * * * *

9️⃣ How This Applies to Your Scheduler
======================================

You could use CompletableFuture to:

-   Call multiple downstream services in parallel

-   Apply retry logic asynchronously

-   Combine results before marking task complete

Instead of blocking on Future.get().
