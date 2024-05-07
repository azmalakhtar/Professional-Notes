domain: 
course: [[Atlas/Operating System Mindmatrix]]
teacher:
date: 2024-04-30
time: 08:04
status: #unprocessed

# Lecture 11 Round Robin LJF LRTF HRRN Scheduling Algorithms
### Round Robin 
- CPU assigned to the process on the basis of FCFS for a fixed amount of time(time quantum). After the time quantum expires, the process is preempted & sent to the ready queue. It's always preemptive in nature.
- RR scheduling is FCFS with preemptive mode.
- Can be implemented using **circular linked list**.
- Efficiency - (time quantum) / (time quantum + context switching overhead)
	- For maximum efficiency the context switching overhead should tend to 0 or time quantum >>> cs overhead
- If tq is large => RR acts as FCFS, cs was increased
- If tq is very small => cs will increase
- In general the tq value is between 10 to 100 ms. and cs < 10 micro second.
- Typically, RR have higher average turn around time than SJF, but better response time.
#### Advantages
- Low response time, good interactivity.
- Fair allocation of CPU across processes.
- Low average waiting time when job lengths vary widely.
#### Disadvantages
- Poor average waiting time when jobs have similar length because identical jobs will all finish at nearly the same time(at the very end of the workload time)
	- Average waiting time is even worse than FCFS.
- Performance depends on length of time slice
	- Too high - degenerate to FCFS
	- Too low - too many context switches, costly
#### When to use?
- Round Robin is preferable for 
	- Interactive applications
	- User needs quick responses from system
- FIFO/STF preferable for Batch applications
	- User submits jobs, goes away, comes back to get result.
### Longest Job First
- CPU is assigned to the processes having largest burst time. In case of a tie, it is broken by FCFS.
- Preemptive LJF is LRTF.
- Time Quantum
	- Hardware timer will be triggered.
	- It is one interrupt
	- Interrupt service routine will be checked
	- This in term will lead to short term scheduler code.
#### Advantages
- No process can complete until the longest jobs also reaches its completion
- All the processes approxiametly finishes at same times.
#### Disadvantages
- The average waiting time is high
- Processes with smaller BT may starve for CPU.
### Shortest Job First

### HRRN