domain: 
course: [[Operating System Concepts Galvin]]
teacher:
date: 2024-05-04
time: 08:57
status: #unprocessed

# CPU Scheduling
- CPU scheduling is the basis of multiprogrammed operating systems.
- In modern-operating systems, it is kernel-level threads not processes that are being scheduled.

## 1 Basic Concepts
- Process execution consists of cycles of CPU execution & I/O wait.
### 1.1 CPU-I/O Burst Cycle
- Although CPU burst duration vary greatly, they tend to have a frequency curve similar to this.
	- ![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_02_CPU_Histogram.jpg)
	- Large number of short CPU-bursts & small number of long CPU-bursts.

### 1.2 CPU Scheduler
- Ready queue is not necessarily a FIFO.
- The records in the queues are generally process control blocks (PCBs) of the processes.
### 1.3 Preemptive and Nonpreemptive Scheduling
### 1.4 Dispatcher

## 2 Scheduling Criteria
- **CPU utilization**
- **Throughput**
- **Turnaround time**
- **Waiting time**
- **Response time**

## 3 Scheduling Algorithms
### 3.1 First-Come, First-Served Scheduling
### 3.2 Shortest-Job-First Scheduling
### 3.3 Round Robin Scheduling
### 3.4 Priority Scheduling
### 3.5 Multilevel Queue Scheduling
### 3.6 Multilevel Feedback Queue Scheduling

## 4 Thread Scheduling
### 4.1 Contention Scope
### 4.2 Pthread Scheduling

## 5 Multi-Processor Scheduling
### 5.1 Approaches to Multiple-Processor Scheduling
### 5.2 Multicore Processors
### 5.3 Load Balancing
### 5.4 Processor Affinity
### 5.5 Heterogeneous Multiprocessing

## 6 Real-Time CPU Scheduling
### 6.1 Minimizing Latency
### 6.2 Priority-Based Scheduling
### 6.3 Rate-Monotonic Scheduling
### 6.4 Earliest-Deadline-First Scheduling
### 6.5 Proportional Share Scheduling
### 6.6 POSIX Real-Time Scheduling

## 7 Operating-System Examples
### 7.1 Example: Linux Scheduling
### 7.2 Example: Window Scheduling
### 7.3 Example: Solaris Scheduling

## 8 Algorithm Evaluation
### 8.1 Deterministic Modeling
### 8.2 Queueing Models
### 8.3 Simulations
### 8.4 Implementation
