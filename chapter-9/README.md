# Scheduling: Proportional Share (Fair Share)

## Why

Instead of optimizing turnaround or response time. Scheduler will try to guarantee each job obtain percentage of CPU time.

## Basic Concept: Tickets Represent Your Share

Each process will have **tickets**, tickets is just a representative of share it owns to a CPU.

### Example

Imagine there's only 2 process:

**Process A**: 75 tickets

**Process B**: 25 tickets

That means process A is having 75% share of total CPU, and B only have 25%.

Holding ticket is straightforward, scheduler just need to keep track how many ticket for each process have.

Then scheduler just need to pick a random number from 1-100.

If number 1-75 picked. Process A got executed by CPU.

If number 76-100 picked. Process B got executed by CPU.

## Ticket Mechanisms

