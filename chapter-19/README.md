# Paging: Faster Translations (TLBs)

Using paging as a core mechanism can be bad, because paging need an extra memory to lookup for every instruction.

To speed up address translation, a hardware need a helper. We're going to add **Translation lookaside buffer (TLB)**. TLB is a part of Memory Management Unit (MMU), it's a hardware cache to store popular virtual to physical address translation.

Each virtual memory reference, hardware will check TLB first if it's already cached, if it's cached, the hardware can just use that cached physical address to fetch the data.

## TLB Basic Algorithm

- Extract the virtual page number from virtual address
- Check the TLB if it's cached the VPN
- If yes, TLB hit, means can get Page Frame Number (PFN)
- Concantenate PFN with Offset to get Physical Address
- Access memory

If CPU doesn't find translation in TLB, it will do a costly action, and put the translation to TLB.

If TLB miss happen often, the application will run slowly.

![TLB control flow algorithm](img/chapter-19-img-1.png)

## Example: Accessing an array

In this example, let's assume there's an array of 10 4 byte integer in memory, starting at virtual address 100.

We have small 8 bit of virtual address space.

With 16 bytes of pages

That means, 4 bit for VPN, and 4 bit for offset.

![Example: an array in tiny address space](img/chapter-19-img-2.png)

As you can see, the array first entry (a[0]) begin on VPN 6, offset 4. That means only 3 integer can fit in this page.

Then on next 4 entries, a[3] - a[6] is on VPN 7. Then last 3 a[7] - a[9] is on VPN 8.

Let's consider this simple loop
```
int sum = 0;
for (i = 0; i < 10; i++) {
    sum += a[i];
}
```

For simplicity, let's pretend only memory access is the array, ignoring code, int i, int sum.

When accessing a[0], CPU will load virtual address 100. The hardware will extract that into VPN 6, and check the TLB for that translation, the result is TLB miss.

Next access is a[1], but this time, it's TLB hit because we already accessed this page, so the translation is inside the TLB.

Accessing a[2] also TLB hit.

Accessing a[3] will TLB miss, but a[4-6] will TLB hit.

Accessing a[7] will TLB miss, but a[8-9] will TLB hit.

The TLB hit rate will be 70%.

The TLB improve the performance because **spatial locality**.

The elements of the array are packed tightly into pages, that means the TLB miss only when first time getting the pages.

If the pages is more bigger, that means the TLB miss will be decreased because there's a high chance it will be on the same pages.

If we do the same loop after we done looping, the TLB miss will most likely zero. Because all of the pages already cached in the TLB. It's called temporal locality.

## Who Handles The TLB Miss?

In olden days, it was Hardware who handles that, that means, hardware need to know where exactly the page tables resides in memory (via page table register).

Now, more modern architecture is using **software managed TLB**. On TLB miss, hardware raise an exception which pauses the instruction stream