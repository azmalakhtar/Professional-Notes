domain: 
course: [[Atlas/Operating System Mindmatrix]]
teacher:
date: 2024-05-03
time: 14:50
status: #unprocessed

# Lecture 14 Multilevel Queue Scheduling
### Multilevel Queue Scheduling
- It is used when the processes are easily classified into different groups.
- A multilevel queue scheduling algorithm partitions the ready queue into several separate queues.
- The processes are permanently assigned to one queue, generally based on some property of the process, such as memory size, process priority, or process type.
- Each queue has its own scheduling algorithm.
- Priority is also assigned to each queue. #cross-check
#### Disadvantages
- Lower priority queue processes might face *starvation*.

### Multilevel Feedback Queue Scheduling
- Allows a process to move between queues. The idea is to separate processes according to the characteristics of their CPU bursts.
- If a process uses too much CPU time, it will be moved to a lower-priority queue. This scheme leaves I/O bound and interactive processes in the higher-priority queues.
- A process that waits too long in a lower-priority queue may be moved to a higher-priority queue. This form of aging prevents starvation.
