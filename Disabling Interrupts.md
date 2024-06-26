The critical section problem could be solved simply in a single-core environment if we could prevent interrupts from occuring while a shared variable was being modified. In this way, we could be sure that current sequence of instructions would be allowed to execute in order without preemption. Thus avoiding critical section problem.
```cpp
while (true) {
	/* disable interrupts */
	/* critical section */
	/* enable interrupts */
	/* remainder */
}
```

## Why is this not a good solution?
Firstly, this solution will not work on multiprocessor environment, disabling interrupts affects only the CPU that executed the disable instruction, the other will continue running and can access the shared memory.

The efficiency of execution will be noticeably degraded because the processor can not interleave processes when the interrupts are disabled.

It is unwise to give the user processes the power to turn off interrupts. What if one of them did it, and never turned them on again? That could be the end of system.