# Day 4 --- volatile

##   What volatile guarantees

visibility

Writes to a volatile variable happen-before subsequent reads of the same variable.

##   What it does NOT guarantee

atomicity - compound operations like count++ are still not thread safe, even if count is volatile

Volatile is suitable for simple state flags, not compound updates.

(atomicity means when a thread modifies the state of some objects, another thread can't see any intermediary state. Either it sees the state as it was before the operation, or it sees the state as it is after the operation)
