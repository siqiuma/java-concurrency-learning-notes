### 1️⃣ What problem does AtomicReference solve?

> AtomicReference enables atomic updates to object references using CAS, allowing lock-free state transitions and reference swaps without using synchronized or explicit locks.

2️⃣ Explain the ABA problem in your own words

The ABA problem occurs when a thread reads a value A, then another thread changes it from A to B and back to A. When the first thread performs CAS, it sees the value is still A and assumes nothing changed, even though intermediate modifications occurred. CAS cannot detect that the value was temporarily modified.

3️⃣ When would atomic be insufficient?

Atomic is insufficient when:

-   Multiple related variables must be updated consistently.

-   Complex multi-step logic must be protected.

-   Maintaining invariants across multiple fields is required.

-   Coordination between threads involves more than a single variable.

Atomic guarantees atomicity for one variable, not for multiple dependent state changes.

4️⃣ Why is versioning needed in CAS-based algorithms?

Versioning allows CAS to detect intermediate state changes by associating a version number with the value. Even if the value returns to its original state (A → B → A), the version changes, preventing false-positive CAS success.

__Atomic operations are ideal for simple state transitions, but complex invariants across multiple fields require locking or transactional guarantees.__
