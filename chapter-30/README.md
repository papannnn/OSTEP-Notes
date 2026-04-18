# Condition Variables

Lock is not the only thing that are needed to build concurrent program.

There are many cases where thread need to check, such as `thread_join()`

![A parent waiting for its child](img/chapter-30-img-1.png)

We can do it using spin based approach, but it's still wasting the CPU cycle.

![A parent waiting for its child: spin based approach](img/chapter-30-img-2.png)

## Definition and Routines

Thread can use such as condition variable.

It's basically waiting a state, if the condition is satisfied, it will notify the waiting thread and wake them up.

A condition variable has 2 operation, `wait()` and `signal()`

`wait()` is called by waiting thread.

`signal()` is called by the changer of the state.

![A parent waiting for its child: use condition variable](img/chapter-30-img-3.png)

## The Producer/Consumer (Bounded Buffer) Problem

Imagine 1 or more producer thread and 1 or more consumer thread.

Producer insert data, consumer popping data.

![Producer / Consumer threads code](img/chapter-30-img-4.png)

With this current code, there's still a chance for race condition.

Let's improve it with condition variable.

![Producer / Consumer single cv and if statement](img/chapter-30-img-5.png)

But this can cause a problem when having a multiple consumer.

![Thread trace, broken solution](img/chapter-30-img-6.png)

We can improve this by using while

![Thread trace, broken solution](img/chapter-30-img-7.png)

But now we have a chance to make all thread sleep because we wake up the wrong thread

![Thread trace broken solution v2](img/chapter-30-img-8.png)


We can fix it by using multiple CV

![Producer / Consumer: Two CV and while](img/chapter-30-img-9.png)