# Day 3 --- synchronized

##   What synchronized does

synchronized ensures only ONE thread can execute the critical section at a time.

##   What it locks (object monitor)

Every Java object has an intrinsic lock (monitor).

When a thread enters a synchronized block or method:

-   it acquires the monitor lock
-   other threads attempting to acquire the same lock must wait
##   Method-level vs block-level

Method-level: Instance methods synchronize on the current object (`this`).
```java
public synchronized void increment() { count++; }
```
Instance methods are *synchronized* over the instance of the class owning the method, which means only one thread per instance of the class can execute this method.

Block-level: The synchronized block locks on the specified monitor object.
```java
synchronized (this) { setCount(getCount()+1); }
```
we passed a parameter *this* to the *synchronized* block. This is the monitor object. The code inside the block gets synchronized on the monitor object

#   Mutual exclusion

`synchronized` provides mutual exclusion by ensuring that only one thread holds the monitor lock at a time.
