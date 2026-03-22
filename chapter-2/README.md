# Chapter 2 (Introduction to Operating Systems)

## What happen when program runs?
### Short answer
It runs instruction

### More detailed answer (Von Neumann version)
For million - billion times / sec, computer will do this:
- Processor **fetches** an instruction from memory
- **Decodes** the instruction (to figure out which instruction this is)
- **Execute** the decoded instruction, ex: Add two number, access memory, check condiition, jump to function, etc.
- After it's done, processor will **move to next** instruction, **until** the program is completed.

## What is Operating System
**Operating system** is a *Software* that can help us easily to run another program (or more programs), allowing programs to share memory, making program can interact with other devices, etc. 

It's in charge for making sure the system running the program correctly and effective as possible.

## Virtualization
To make sure the system running correctly, **Operating System** do what it called virtualization. Basically what it does is taking the physical resources of the computer hardware (Processor, Memory, Disk), and transform it into something more general that can be *understandable* by a program.

## OS APIs
The way a program can access the hardware that already got *virtualized* by an OS ex: (Accessing file, Allocating memory, etc) is through API. We can say that OS provides the **Standard Library** to the program.

## Resources Manager
Because Operating System's **Virtualization** making us to be able to run multiple programs (sharing the CPU), and many program concurrently accessing their data (sharing the memory), and many program concurrently accessing the computer's devices (sharing disk, devices, etc). Operating System also known as **Resources Manager**. CPU, memory, devices (disk, network cable, etc) is actually a resources. And Operating System need to manage them so every program can **fairly** get their chances to access those resources.

## Virtualizing CPU
### Scrolling Reddit while my Youtube is playing a video
![two browser (1st opening Reddit, 2nd opening Youtube video)](img/chapter-2-img-1.png)

Even though my computer only having 1 processor, but my computer still can run 2 programs (Browser Reddit & Browser Youtube) running at "the same time".

### Illusion
Turns out, Operating System with the help from the hardware actually do some kind of illusion, giving us the user of the computer a perception of our computer having multiple CPU. Allowing many program to run at "the same time".

## Virtualizing Memory
Looking at the previous image, it's basically 2 separate running program. But those 2 actually not sharing the same state, even though they using the same memory (RAM). If I change my 2nd browser (Youtube) to watch another video, the first browser (Reddit) doesn't got affected. Vice versa, if I do something with first browser (Reddit), for example moving to another website. The 2nd browser doesn't get affected.

## Persistence
Saving data into memory (RAM) is not reliable, when the computer is turned off, all of the data saved in RAM will be lost. 

Because of that, we need hardware that can allow us to store a data **persistently**, such as SSD (Solid-state drives), Hard Drives, etc.

Operating System usually manage this disk through **file system**, it allow the user to store any files in their computer.

Operating System also provides an APIs to program, so the program can access some file through Operating System's APIs.

## Summary
Operating System help's the user of the computer to run the program by managing the computer's resources such as (CPU, Memory, Disk).