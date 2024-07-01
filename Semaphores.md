[Operating System Concepts Galvin](Atlas/Operating%20System%20Concepts%20Galvin.md)

A **semaphore** S is an integer variable that, apart form initialization, is accessed only through two standard **atomic** operations: `wait()` and `signal()`. 
```cpp
wait(S) {
	while (S <= 0)
		; // busy wait
	S--;
}
signal(S) {
	S++;
}
```

All modifications to the **integer value** of the semaphore in the `wait()` and `signal()` operations must be executed atomically. In addition, in the case of wait(S), the testing of the integer value of S (S ≤ 0), as well as its possible modification (S--), must be executed without interruption.

## Semaphore Usage
There are two types of semaphores:
1. Counting semaphores : their value can range over an unrestricted domain.
2. Binary semaphores: their value can range only between 0 & 1.

Counting semaphores can be used to control access to a given resource consisting of a finite number of instances. The semaphore is initialized to number of resources available. Each process performs `wait()` operation before accessing the resource & `signal()` after it releases the resource.
```cpp
wait(S);
/* Resource Acquisition & Use */
// Release the resource
signal(S);
```

We can also use semaphores to solve various synchronization problems. Suppose we have two process P1 & P2. We want that the statement S2 (from P2) executes after the statement S1 (from P1). We can implement this scheme by sharing a common semaphore `synch` between P1 & P2, initialized to 0;
```cpp
// In P1
S1;
signal(synch);

// In P2
wait(synch);
S2;
```

## Semaphore Implementation
In the above implementation of the wait and signal suffers from busy waiting. To get rid of that we do following modifications. In `wait()` operation, rather than engaging in busy waiting , a process can suspend itself, if the value of semaphore is negative. This process will be restrated when some other process executes a `signal` operation.
```cpp
typedef struct {
	int value;
	struct process *list;
} semaphore;

wait(semaphore *S) {
	S -> value--;
	if (S->value < 0) {
		// Add this process P, to list
		sleep(P);
	}
}

signal(semaphore *S) {
	S -> value++;
	if (S->value <= 0) {
		// Remove a process P from the list
		wakeup(P);
	}
}
```

If a semaphore value is negative, its magnitude is the number of processes waiting on that semaphore.

To ensure that `wait()` and `signal()` are executed atomically
- On single-processer system : we can do this by disabling interrupts
- On multi-processor system: SMP systems must provide alternative techniques - such as `compare_and_swap()` or spinlocks 

