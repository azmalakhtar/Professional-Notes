domain: [[Computer Science]]
course: [[Operating System Mindmatrix]]
teacher:
date: 2024-03-21
time: 09:51
status: #unprocessed

# Lecture 6 Threads
- Thread is a light weighted process. In certain situations, a single application may be required to perform several similar tasks.
	- e.g. -> a word processor at the same time might need to
		- display graphics
		- spelling & grammar checking
		- responding to keystrokes
- If we create different processes for each different task then
	- Context Switching Overhead
	- Different address space
	- Inter process communication required.
- There are two types of processes
	- single threaded process -> have only one thread
	- multi threaded process -> have more than one thread
- Threads are about concurrency and parallelism
	- Concurrency
			 ![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_03_ConcurrentSingleCore.jpg)
	- Parallel execution
		- ![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_04_ParralelMulticore.jpg)
- Single vs Multi Threaded Process
	-  ![Mem Layout](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_01_ThreadDiagram.jpg)
	- In multi threaded process, code, data, files, & heap section are same for all threads, while each thread have its own registers and stack. The reason being is that having separate register help us in avoiding data loss.
	- The register set and stack of the process get divided for different threads. They each get some of the stack space and some of registers.
	- Each thread will have its own SP(stack pointer) and  PC(program counter).
- Types of Threads
	- User Level
		- When we make threads using function calls.
		- Have its own TCB(thread control block)
	- Kernel Level
		- When we make threads using system call
		- Have its own TCB(thread control block)
		- Since only kernel can create TCB, hence system call is needed.
- Thread Libraries
	- A thread library provides the programmer with an API for creating and managing threads. 
	- There are two primary ways of implementing a thread library.
	- The first approach is to provide a library entirely in user space with no kernel support. All code and data structures for the library exist in user space. This means that invoking a function in the libraray results in a local function call is user space and not a system call. **User Level Thread.**
	- The second approach is to implement a kernel-level library supported directly by the operating system. In this case, code and data structures for the library exist in kernel space. Invoking a function in the API for the library typically results in a system call to the kernel. **Kernel Level Thread**.
- User Level Threads
	- they are small and fast
		- managed entirely by the user-level library - e.g. pthreads
		- each thread is represented simply by a PC, registers, a stack, and a small thread control block(TCB).
		- creating a thread, switching between threads, and synchronizing threads are done via procedure calls, no kerel involvement is necessary.
	- their operations can be 10-100x faster than kernel threads as a result
	- OS doesn't know about user level threads
	- The OS only knows about the process containing the threads,
	- The OS only schedules the process, not the threads within the process.
	- The programmer uses a thread library to manage threads(create and delete them, synchronize them, and schedule them)
- User level threads disadvantages
	- Since the OS does not know about the existence of the user-level threads, it may make make poor scheduling decisions:
		- It might run a process that only has idle threads.
		- If a user-level thread is waiting for I/O, the entire process will wait(all other threads will be blocked too).
		- Solving this problem requires communication between the kernel and the user-level thread manager.
	- Since the OS just knows about the process, it schedules the process the same way as other processes, regardless of the number of user threads.
	- For kernel level threads, the more threads a process creates, the more time slices the OS will dedicate to it.