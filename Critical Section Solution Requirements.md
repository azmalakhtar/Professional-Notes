## The Critical-Section Problem
### The Problem

Each process has a segment of code, called a critical section, in which the process may be changing common variables, updating a table, writing a file, and son on.

The important feature of the system is that when one process is executing in its critical section, no other process is allowed to execute in its critical section. That is, no two processes are executing in their critical sections at the same time.

The critical-section problem is to design a protocol that the processes can use to cooperate.
- What can be the final value of count.
	```cpp
	// Thread 1
	for (int i = 0; i < 10; i++) count++;
	// Thread 2
	for (int i = 0; i < 10; i++) count++;
	```

	- We will find what can be the minimum & maximum value.
	
	- The value of count will be maximum when the thread executes sequentially. The maximum value will be 20.
	
	- The minimum value will occur
		1. Thread 1 1st Iteration starts, but it get preempted before the saving of the count. So count = 0 & register = 1
		2. Thread 2 9 Iterations Completed. count = 9
		3. Thread 1 1st Iteration Completes. count = 1
		4. Thread 2 10th Iteration starts, but gets preempted before saving the count. So count = 1 & register = 2.
		5. Thread 1 Remaining 9 Iterations Completes. count = 10
		6. Thread 2 10th Iteration completes. count = 2
	- The minimum value will be 2.

- *General Structure of a Typical Process P*
	```cpp
	do {
		// entry section code
			// critical section code
		// exit section code
			// remaining code
	} while(true);
	```



### The Solution

A solution to the critical-section problem must satisfy the following three requirements:
1. **Mutual Exclusion**: If process Pi is executing in its critical section, then no other processes can be executing in their critical sections.
2. **Progress**: If no process is executing in its critical section and some processes wish to enter their critical sections. then only those processes that are not executing in their remainder sections can participate in deciding which will enter its critical section next, and this selection can't be postponed indefinitely.
3. **Waiting**: There exists a bound, or limit, on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted.
- Another condition which is not required but good to have is **architectural neutrality**, which basically means that the solution should be independent of the architecture.

Implement Mutual Exclusion via
- Software: entirely by user code
- OS support: primitives via system call
- Hardware: Special Machine Instructions