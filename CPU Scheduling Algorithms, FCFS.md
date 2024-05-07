domain: 
course: [[Atlas/Operating System Mindmatrix]]
teacher:
date: 2024-04-22
time: 16:42
status: #unprocessed

# Lecture 9 CPU Scheduling Algortihms
## Scheduling Algorithms
### Some goals of scheduling algorithms
1. Maximum CPU utilizations
2. Fair Allocation of CPU
3. Maximum throughput (number of processes that complete there execution per time unit)
4. Minimum Average Waiting Time
5. Minimum Average Response Time
6. Minimum Average Turnaround Time
- Read the Galvin Excerpt included in the lecture slide #todo/read

### Various Times Related to Process
#### Arrival Time
- Time at which the process enters the ready queue.
#### Waiting Time
- Time spend by the process waiting for getting the CPU.
- Waiting time = turn around time - burst time
#### Response Time
- Time after which a process gets the CPU for the first time after entering the ready queue.
- response time = time at which process 1st gets CPU - arrival time
#### Burst Time
- Time required by a process for executing on CPU. Also called running time.
- It can only be known after the process has been executed.
#### Completion Time
- Time at which a process completes its execution on the CPU & takes exit from the system.
#### Turn Around Time
- Total time spent by a process in the system
- Turn around time = burst time + waiting time
- Turn around time = completion time - arrival time

### First Come First Serve (FCFS) Scheduling Algorithms
- Process arriving first in the ready queue is firstly assigned the CPU. In case of a tie, process with smaller process id is executed first.
- It's always non-preemptive in nature.
#### Advantages
- Simple & Easy To Implement
- Can be implement using Queue, a FIFO Data Structure
#### Disadvantages
- Doesn't consider priority or burst time
- Convoy Effect - If the process with higher burst time arrived before the processes with smaller burst time, then smaller processes have to wait for a long time for longer processes to release the CPU.
- Uneven Average Waiting time
> In case of non-preemptive CPU scheduling algorithms, waiting time is same as response time.