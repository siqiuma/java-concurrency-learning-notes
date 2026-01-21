## Day 2 --- Race condition & thread safety

#   What is race condition

A race condition occurs when multiple threads access **shared** data **concurrently**, and at least one access is a **write**, without proper synchronization.

This can lead to lost updates and unpredictable results.

A race condition occurs when:

    -   multiple threads access the same shared data
    -   at least one access is a write
    -   accesses happen concurrently without proper synchronization
#   What does "thread-safe" mean

Thread-safe means multiple threads can access shared resources without causing incorrect behavior or inconsistent state.

#   Why "it works on my machine" is meaningless

Because thread scheduling is nondeterministic.

A program working in one environment does not guarantee correctness under different timing or concurrency conditions.
