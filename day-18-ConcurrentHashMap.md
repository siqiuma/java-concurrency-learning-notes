### 1️⃣ Why can HashMap break under concurrent modification?

HashMap is not thread-safe because its internal structure (array + linked lists / tree bins) is not protected by synchronization.

When multiple threads modify it concurrently:

-   Lost updates can occur.

-   Internal bucket chains can become corrupted.

-   During resize operations, rehashing may produce inconsistent structure.

-   In older Java versions, this could even cause infinite loops due to cyclic bucket chains.

HashMap assumes single-threaded access unless externally synchronized.

* * * * *

### 2️⃣ Why does synchronizedMap not scale well?

`Collections.synchronizedMap()` wraps the entire map with a single monitor lock.

Every read and write operation must acquire the same lock, which:

-   Serializes all access.

-   Creates high contention under concurrency.

-   Limits throughput in multi-threaded environments.

As concurrency increases, performance degrades due to blocking and context switching.

* * * * *

### 3️⃣ What design idea makes ConcurrentHashMap more scalable?

ConcurrentHashMap improves scalability through:

-   Fine-grained synchronization at the bucket level.

-   Lock-free reads.

-   CAS-based updates where possible.

-   Minimal locking during structural changes.

By avoiding a single global lock, it significantly reduces contention and allows higher parallelism.

* * * * *

### 4️⃣ When would you use computeIfAbsent?

You would use `computeIfAbsent` when:

-   You need to initialize a value lazily.

-   You want to ensure that initialization happens atomically.

-   Multiple threads may attempt to insert the same key concurrently.

With ConcurrentHashMap, `computeIfAbsent` ensures that the mapping function is applied at most once per key, preventing duplicate initialization under concurrency.

In single-threaded scenarios, it is also convenient for clean lazy initialization logic.

### ConcurrentHashMap improves scalability by reducing lock granularity and leveraging lock-free reads and CAS-based updates.
