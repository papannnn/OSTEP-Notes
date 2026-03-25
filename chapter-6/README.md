# Mechanism: Limited Direct Execution

## How to virtualize CPU?
By doing time sharing

## What's the drawback of virtualizing CPU?

### Performance
There's an overhead when CPU moving to another process

### Control
There's a chance process can run forever, it could take over the CPU time and not letting other process getting a chance to run.

## That means
We need to balance out the overhead vs control, 

Too many moving to another process = overhead = performance degrade

Less moving to another process = another process can't get a chance to run

## Basic Technique: Limited Direct Execution

Simple technique, just run the process **directly on the CPU**. When OS want to run the process, OS will:
- Create entry for process list
- Allocate memory for program
- Load program into memory
- Set up stack with argc / argv
- Clear registers
- Exec call main()
- Run main()
- Exec return from main()
- Free memory of process
- Remove from process list

### Problem 1
The execution is actually fast, because it runs natively on CPU, but because it's running directly on CPU, what if the process want have read / update access to some I/O devices? Or maybe want to gain access on CPU / memory?

#### Let the process to do whatever they want related to I/O
But if we do this, that means we can't build a file system that have a permission checker feature.

#### Introduce User Mode
Code that runs on user mode is restricted to do something specific, ex: User mode can't modify something related to memory. The opposite of User Mode is **Kernel Mode**, it can do anything and usually Kernel / OS have this mode.

#### If User mode can't do what Kernel do, what should User mode do?
We provide a proxy to Kernel mode through system call. System call provides a temporary access to Kernel mode, such as creating file, accessing file, destroying process, etc.

````
User Program (User Mode)
    |
    | 1. call read()
    v
C Library wrapper
    |
    | 2. executes TRAP instruction (e.g. syscall / int 0x80)
    v
=============================
CPU switches to Kernel Mode
=============================
    |
    | 3. OS system call handler runs
    |    - checks arguments
    |    - performs the operation (e.g. disk read)
    v
Kernel finishes
    |
    | 4. return-from-trap
    v
=============================
Back to User Mode
=============================
    |
    | 5. result returned to program
````

### Problem 2
The next problem is related to switching another process when process A is running.

Assuming this:
- OS is currently running on CPU
- OS schedule process A to run
- OS get out from the CPU
- Process A is running

How does OS handle the switching process A on CPU? OS is not in CPU anymore.

### A Cooperative Approach: Wait For System Calls
When a running process call a system calls, it will transfer the control back to OS, and OS can run again.

Process can transfer control by doing something bad, example: divide by 0 can trigger a trap to the OS.

### A Non-Cooperative Approach: The OS Takes Control
But the issue is, we depend on process giving the control back explicitly. If the process got stuck in infinite loop, OS can't get the control.

#### Timer Interrupt
When OS give control to another process, OS will set a timer interrupt to that process, if the timer is up, the process will give control back to OS, and OS can do something about it.

### Saving and Restoring Context
Moving process to OS whether using Non-Cooperative / Timer Interrupt, the one who did it is a part of OS called **Scheduler**.

If process is moved, that means OS need to do **Context Switch**, it's basically saving the last state of the process, so when the process is getting picked up again, OS know the last state of the process and can continue on that last state.

## Summary
OS need to balance out the performance vs control for virtualizing CPU.

Some technique is to just run the process directly in CPU.

But sometimes process need access for other devices like Disk, Memory, I/O devices, etc.

We can give them full access for all of those.

But if we want to build an apps related to permission access, we can't do this.

The solution is to separate the mode into 2:
- User Mode
- Kernel Mode

User mode has restrictive access
Kernel mode can do everything

But how does User mode want to access something related to kernel?

We give them System call

It's literally a proxy to access a temporary kernel mode from user mode.

How about moving process A to process B?

We do Scheduling, and to retain the state of the process, we do Context Switching