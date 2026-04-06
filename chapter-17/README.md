# Free-Space Management

![Placing segments in physical memory](img/chapter-17-img-1.png)

Let's try to remember what is external fragmentation is.

We have memory with size 30KB. Then in the middle, we use that memory, we use it from 10KB to 20KB. Taking 10KB of size.

Now we have 20KB of free space, but because we use the middle of free space. The free space is 2 10KB. We can't put a program that has >10KB of memory space needed.

## Assumptions

We assume in this OS
- OS allow us to do malloc & free
- When calling free, OS know how many size of memory will be deleted
- Once memory is handed to client, it cannot be relocated to another location in memory. That means, no compaction can happen.
- Allocator manages contiguous region

The space of this data will be stored in heap. And all of those pointer will be stored on generic data structure called **free list**.

When we're concerning about internal fragmentation, we also need to be aware about **Internal fragmentation**.

**Internal fragmentation** happens when allocator of memory is giving a chunk of memory larger than we requested. Any unused / unasked memory we consider this as internal fragmentation.

## Low Level Mechanism