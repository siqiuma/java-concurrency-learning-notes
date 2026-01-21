# Day 5 — happens-before

happens-before defines visibility guarantees between threads, not execution order

Example:

- A write to a volatile variable happens-before every subsequent read of that variable.

This means:

> All memory writes visible to thread A before the volatile write become visible to thread B after the volatile read.
> 

A volatile write publishes all previous writes to other threads.

## Why does volatile work?

**Because a volatile write establishes a happens-before relationship with subsequent volatile reads, guaranteeing visibility of all previous memory writes.**


> If thread A writes some data,
>
> then writes to a volatile variable,
>
> and thread B later reads that same volatile variable,
>
> then thread B is guaranteed to see all the earlier writes done by thread A.

The volatile variable acts as a **visibility bridge**.

* * * * *

🧪 Example (very important)
---------------------------

```
intdata=0;
volatilebooleanready=false;

```

### Thread A

```
data =42;// (1)
ready =true;// (2) volatile write

```

### Thread B

```
if (ready) {// (3) volatile read
    System.out.println(data);
}

```

### What Java guarantees

If thread B sees:

```
ready ==true

```

Then Java guarantees:

```
data ==42

```

Even though `data` is NOT volatile.

That's the magic.

* * * * *

Why?
----

Because Java enforces this rule:

> volatile write → happens-before → volatile read

Which means:

-   all writes before the volatile write
-   become visible after the volatile read
