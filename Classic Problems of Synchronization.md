## The Produce-Consumer (Bounded Buffer) Problem
### Problem
There are multiple producer and consumer threads and they all modify the same buffer (similar to array). 
The requirements are that:
- No production when buffer is full
- No consumption when buffer is empty
- Only one thread should modify the buffer (critical section) at any time.

### Solution
The `mutex` binary semaphore provides mutual exclusion for accesses to the buffer pool. The `empty` and `full` semaphores count the number of empty and full buffers. 
The statement `wait(empty)` in producer process ensures that no production happens when the buffer is full ( no slot is empty ).
The statement `wait(full)` in consumer process ensures that no consumption happens when the buffer is empty ( no slot in full ).
```cpp
/* Shared Data Structures*/
semaphore mutex = 1; 
semaphore empty = BUFFER_SIZE;
semaphore full = 0;
```

```cpp
/* Structure of the producer process*/
while (true) {
	// . . .
	/* produce an item in next produced */
	// . . .
	wait(empty);
	wait(mutex);
	// . . .
	/* add next produced to the buffer */
	// . . .
	signal(mutex);
	signal(full);
}
```

```cpp
/* Structure of the consumer process*/
while (true) {
	wait(full);
	wait(mutex);
	// . . .
	/* remove an item from buffer to next consumed */
	// . . .
	signal(mutex);
	signal(empty);
	// . . .
	/* consume the item in next consumed */
	// . . .
}
```

## The Readers-Writers Problem
### Problem
A reader can only read the data, while a writer can read as well as write the data.
Requirements:
1. When a reader is reading, no writer must be allowed.
2. Multiple readers should not be allowed.
3. When a writer is writing, no reader must be allowed.
4. Multiple writers should not be allowed

### Solution
`rw_mutex` ensures that 
- no writer should write when another writer is writing or a reader is reading
- no reader should read when a writer is writing

`mutex` is used to make sure that 
- mutual exclusion when `read_count` is updated.
- no preemption happens between updation of `read_count` and the `if` statements.

`rw_mutex` is used by the writer and first or last reader that enters the critical section.
If a writer is in the critical section and *n* readers are waiting, then one reader is queued on `rw_mutex`, and *n - 1* readers are queued on `mutex`. 

When a writer executes `signal(rw mutex)`, we may resume the execution of either the waiting readers or a single waiting writer. The selection is made by the scheduler.
```cpp
semaphore rw mutex = 1; // shared between reader and writer processes.
semaphore mutex = 1; // only reader processes
int read count = 0; // only reader processes
```

```cpp
/* writer process */
while (true) {
	wait(rw mutex);
	// . . .
	/* writing is performed */
	// . . .
	signal(rw mutex);
}
```

```cpp
/* reader process */
while (true) {
	wait(mutex);
	read count++;
	if (read count == 1)
		wait(rw mutex);
	signal(mutex);
	// . . .
	/* reading is performed */
	// . . .
	wait(mutex);
	read count--;
	if (read count == 0)
		signal(rw mutex);
	signal(mutex);
}
```

### Extra
Reader-writer locks are most useful in the following situations:
- In applications where it is easy to identify which process only read shared data and which processes only write shared data.
- In applications that have more readers than writers.
## The Dining-Philosophers Problem
### Problem
There are five philosophers who can either think or eat. They are sitting on a circular table surrounded by five chairs. On the table there are five chopsticks, between every philosopher. A philosopher can only pick one chopstick at a time. When a hunger philosopher has both her chopsticks at the same time, she can eat. When a philosopher is finished eating, he puts down both the chopsticks and starts thinking again. 

### Solution
#### Semaphore Solution
One simple solution is to represent each chopstick with a semaphore. A philosopher tries to grab a chopstick by executing a `wait()` operation on that semaphore. She releases her chopsticks by executing the `signal()` operation on the appropriate semaphores.

```cpp 
// Shared Data
semaphore chopstick[5]; // All intitialized to 1.
```

```cpp
// The structuer of philosopher i
while (true) {
	wait(chopstick[i]);
	wait(chopstick[(i+1) % 5]);
	// . . .
	/* eat for a while */
	// . . .
	signal(chopstick[i]);
	signal(chopstick[(i+1) % 5]);
	// . . .
	/* think for awhile */
	// . . .
}
```

Although this solution guarantees that no two neighbors are eating simultaneously, it nevertheless must be rejected because it could create a deadlock.

Several possible remedies to the deadlock problem are the following:
- Allow at most four philosophers to be sitting simultaneously at the table.
	```cpp
	// Shared Data
	semaphore limit = 4;
	
	// The structure of philosopher i
	while (true) {
		wait(limit);
		wait(chopstick[i]);
		wait(chopstick[(i+1) % 5]);
		// . . .
		/* eat for a while */
		// . . .
		signal(chopstick[i]);
		signal(chopstick[(i+1) % 5]);
		signal(limit);
		// . . .
		/* think for awhile */
		// . . .
	}
	```
- Allow a philosopher to pick up her chopsticks only if both chopsticks are available (to do this, she must pick them up in a critical section).
- Use an asymmetric solution—that is, an odd-numbered philosopher picks up first her left chopstick and then her right chopstick, whereas an even- numbered philosopher picks up her right chopstick and then her left chopstick.

#### Monitor Solution
This solution imposes the restriction that a philosopher may pick up her chopsticks only if both of them are available. To code this solution, we need to distinguish among three states in which we may find a philosopher.

```cpp
DiningPhilosophers.pickup(i);
//...
// eat
//...
DiningPhilosophers.putdown(i);
```


```cpp
/* A monitor solution to the dining-philosophers problem. */
monitor DiningPhilosophers
{
	enum {THINKING, HUNGRY, EATING} state[5];
	condition self[5];
	void pickup(int i) {
		state[i] = HUNGRY;
		test(i);
		if (state[i] != EATING)
			self[i].wait();
	}
	void putdown(int i) {
		state[i] = THINKING;
		test((i + 4) % 5);
		test((i + 1) % 5);
	}
	void test(int i) {
		if ((state[(i + 4) % 5] != EATING) &&
			(state[i] == HUNGRY) &&
			(state[(i + 1) % 5] != EATING)) {
			state[i] = EATING;
			self[i].signal();
		}
	}
	initialization code() {
		for (int i = 0; i < 5; i++)
			state[i] = THINKING;
	}
}
```

#### Problem with both these solutions
Any satisfactory solution to the dining-philosopher problem must guard against the possibility that one of the philosophers will starve to death. In both of the above solutions, starvation is a possibility. A deadlock-free solution does not necessarily eliminate the possibility of starvation.