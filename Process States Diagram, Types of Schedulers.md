domain: 
course: [[Atlas/Operating System Mindmatrix]]
teacher:
date: 2024-04-20
time: 09:24
status: #unprocessed

# Lecture 8 Process States Diagram

## Process State Diagram
![](https://imgs.search.brave.com/KhZaSna9iTsIuNISItOku8V6BA8BmzSUHuUnm35McBk/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9zY2Fs/ZXIuY29tL3RvcGlj/cy9pbWFnZXMvcHJv/Y2Vzcy1zdGF0ZS1k/aWFncmFtLndlYnA)

- States in main memory
	- Ready
	- Run
	- Wait/Block
- States in secondary memory
	- New
	- Suspend Wait
	- Suspend Ready
- Even in wait/block & suspend wait state, the process can do I/O operations due to DMA(direct memory access)

##### Multiprogramming
- Multiple processes in ready state
	1. Non-preemption - Not removed until execution completes
	2. With preemption - Forcefully removed process from CPU (Time sharing, multitasking)

##### CPU-bound & I/O-bound process
- CPU-bound processes require more CPU time or spend more time in running state(intensive in CPU operation). I/O-bound processes require more I/O time, more time in waiting state(intensive in I/O operations)

## Types of Schedulers
### 1. Long Term Scheduler (or Job Scheduler)
- It selects a balanced mix of I/O bound & CPU bound processes from the secondary memory (new) then it loads the selected processes onto the MM (ready) fro execution.
- Primary objective of LTS is to maintain a good degree of multiprogramming.
- Degree of multiprogramming is the maximum number of processes that can be present in the ready state.
- Optimal degree of multiprogramming means average rate of process creation is equal to the average departure rate of processes from main memory.
### 2. Short Term Scheduler (or CPU Scheduler)
- It decides which process to execute next from the ready queue. After STS decides the process, dispatcher assign the decided process to the CPU for execution.
- Primary objective of STS is to increase the system performance.
- Dispatcher: Software that moves processes from ready to run state & vice versa.

### 3. Medium Term Scheduler
- Swaps out the processes from MM to secondary memory to free up the MM when required. It reduces degree of multiprogramming. After some time when MM becomes available, MTS swaps in the swapped out processes to the MM & its execution is resumed from where it left off.
- Primary objective is swapping.
- Swapping may be required to improve the process mix.

#### Dispatcher
- Involved in CPU-scheduling function.
- The dispatcher is the module that gives control of the CPU to the process selected by the short-term scheduler. This function involves the following
	- Switching context
	- Switching to user mode
	- Jumping to the proper location in the user program to restart that program
- The dispatcher should be as fast as possible, since it is invoked during every process switch. The time is takes for the dispatcher to stop one process and start another running is known as the **dispatch latency**.