domain: 
course: [[Atlas/Operating System Mindmatrix]]
teacher:
date: 2024-05-01
time: 14:29
status: #unprocessed

# Lecture 12 LRTF HRRN Priority Scheduling Algorithms
### Longest Remaining Time First
- Schedule the process with the longest remaining time first. Check after each unit of time.

### Highest Response Ratio Next 
- Response Ratio = (WT + BT) / BT
	- WT here is the waiting time at that particular moment.
- CPU is assigned to the process having highest response ratio. In case of tie, it's broken by FCFS.
- Operated only in non-preemptive mode.

#### Advantages
- It performs better than SJF. It is not just favors the shorter jobs but also limits the waiting time of longer jobs.
#### Disadvantages
- Can't be implemented practically as burst time can't be known in advance.

### Priority Scheduling
- A priority number(integer) is associated with each process.
- The CPU is allocated to the process with the highest priority.
- Priorities can be static or dynamic.
- Can be preemptive or nonpreemptive.
- SJF is priority scheduling where priority is the inverse of predicted next CPU burst time
- Problem - **Starvation** - Low priority processes may never execute.
	- Starvation - Indefinite Waiting
	- Deadlock - Infinite Waiting
- Solution - **Aging** - As the progresses increase the priority of the process (HRRN is one of the way to do it)

### Gate 1998 Question
- Consider n processes sharing the CPU in a round-robin fashion. Assuming that each process switch takes s seconds, what must be the quantum size q such that the overhead resulting from process switching is minimized but at the same time each process is guranteed to get its turn at the CPU at least every t seconds?
	1. q <= (t - ns) / (n - 1)
	2. q >= (t - ns) / (n - 1)
	3. q <= (t - ns) / (n + 1)
	4. q >= (t - ns) / (n + 1)
- Solution : Option (1)
	- Time taken before the process gets the CPU again = Time taken by the processes in between + context switching overhead
	- Process Queue - P1 s P2 s P3 s P4 s P5 s P6 s P1
	- As we can see total n-1 process executes before P1 gets the CPU again
	- It took total n context switches before the process gets the CPU again
	- Time taken by the Processes = (n - 1)q
	- Context Switching Overhead = ns
	- time taken before the process gets the cpu again <= t
	- (n - 1)q + ns <= t
	- q <= (t - ns)/(n -1)