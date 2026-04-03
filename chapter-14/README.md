# Interlude: Memory API

## Types of Memory

In running C program, there are 2 types of memory:

- Stack
- Heap

### Stack

The allocation and deallocation and managed implicitly by the compiler. 

Declaring the stack memory is easy, just create a variable

```
void fun() {
    int x = 10;
}
```

When the function finished, the compiler deallocates the memory automatically. That means, if we want to have a variable that can outlive the function, we better put it not on the stack.

### Heap

The allocation and deallocation are explicitly manages by us.

```
void func() {
    int *x = (int *) malloc(sizeof(int));
    ...
}
```

Actually, that code is having stack & heap memory. 

Stack is the pointer that point the integer value.

And the heap is actually the one that stores the value of the integer.

## The malloc() Call

Malloc function signature is this:

```
void *malloc(size_t size);
```

size parameter means how many bytes you need.

```
double *d = (double *) malloc(sizeof(double));
```

malloc will return pointer address of the data or NULL if it fails allocating the memory.

## The free() Call

To free the heap memory that we already allocated, we can use function free();

```
int *x = malloc(10 * sizeof(int));
...
free(x);
```

## Common Errors

### Forgetting To Allocate Memory

```
char *src = "hello";
char *dst; // oops! unallocated
strcpy(dst, src); // segfault and die
```

It will get segmentation fault.

Segmentation fault means we did something wrong with the memory.

### Not Allocating Enough Memory

```
char *src = "hello";
char *dst = (char *) malloc(strlen(src)); // too small!
strcpy(dst, src); // work properly
```

### Forgetting to Initialize Allocated Memory

If you forget to initialize, usually you will get a garbage value.

### Forgetting To Free Memory

This will cause memory leak.

In long running application like OS, this can be bad, because eventually the application will run out of memory.

In some cases, it's reasonable for short lived application, when process is finished, OS will clean up all of the allocated pages.

While this is works, but this will build a bad habit.

### Freeing Memory Before You Are Done With It

This will cause a dangling pointer, this can crash a program or overwrite a valid memory

### Freeing Memory Repeatedly

After you free the pointer, then you free it again, this will cause undefined behavior.

### Calling free() Incorrectly

Free expect you to pass pointer that you got from malloc(), if you pass another thing, maybe bad thing can happen.

## Underlying OS Support

malloc() and free() is not a system call, it's library call.

## Other call

- calloc(), allocates memory and initialize it as zero
- realloc(), you can reallocate the memory, resize it, and it will copy the value to a new place

## Summary

There's a stack & heap memory

Stack is automatically allocated by compiler, the lifetime is when the function finished.

Heap need to be manually allocated by programmer, when not needed, programmer can use free to release the memory.
