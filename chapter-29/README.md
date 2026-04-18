# Lock-based Concurrent Data Structures

## Concurrent Counters

One of the simplest data structure, counter.

### Simple But Not Scalable

![A counter without locks](img/chapter-29-img-1.png)

![A counter with locks](img/chapter-29-img-2.png)

This concurrent counter is working as intended, just add simple lock.

The problem with this is performance.

To understand why this is so slow, let's look at the benchmark with each thread trying to update the single shared counter in fixed number of times.

![Performance of traditional vs sloppy counters](img/chapter-29-img-3.png)

As you can see, the one that using lock really slow as the thread increase.

### Scalable Counting

![Tracing the sloppy counters](img/chapter-29-img-4.png)

There's an idea to solve this, it's called sloppy counter.

Basically each thread increment it's own local counter. After some time, the global counter got updated with locking.

![Sloppy counter implementation](img/chapter-29-img-5.png)

## Concurrent Linked Lists

![Concurrent linked list](img/chapter-29-img-6.png)

As you can see in the code, every time doing insert and lookup, it will do locking.

### Scaling Linked Lists

![Concurrent linked list: rewritten](img/chapter-29-img-7.png)

As you can see, we actually can improve the code. We don't need to lock when malloc happen, because malloc is already thread safe.
