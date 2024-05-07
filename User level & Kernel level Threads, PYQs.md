domain: 
course: [[Atlas/Operating System Mindmatrix]]
teacher:
date: 2024-03-22
time: 19:20
status: #unprocessed

# Lecture 7 User Level & Kernel Level Threads
> Which operations requires CPU and which don't?

## Kernel-Level Threads
- OS now manages threads and processes/address spaces
	- all thread operations are implemented in the kernel
	- OS schedules all of the threads in a system
		- if one thread in a process blocks(e.g., on I/O), the OS knows about it, and can run other threads from that process
		- possible to overlap I/O and computation inside a process
- are cheaper than processes(but costlier than user threads), the reason being
	- different klt of a process share the same logical address, while different processes have different logical address
	- in case of klt, context-switching overhead is very low compared to processes
	- less state to allocate and initialize
- but they are still pretty expensive for fine-grained use
	- orders of magnitude more expensive than a procedure call
	- thread operations are all system calls
		- context switch
		- argument checks
	- must maintain kernel state for each thread

- In case of ult, thread library acts as the scheduler
- **Question**: Can user level threads run kernel code
	- **Answer**: Yes, we can invoke system call

## Multithreading Models
- Various ways have been investigated to try to combine the advantages of user-level threads with kernel-level threads. One way is to use kernel-level threads and then multiplex user-level threads onto some or all of them
### Many-to-One Model
![Many-to-one model](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_05_ManyToOne.jpg)
- If a blocking call is made, then the entire process blocks

### One-to-One Model
![One-to-one model](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_06_OneToOne.jpg)
- It has the overhead of creating so many threads.

### Many-to-Many Model
![Many-to-many model](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_07_ManyToMany.jpg)
- multiplexes any number of user threads onto an equal or smaller number of kernel threads