domain: 
course: [[Operating System Concepts Galvin]]
teacher:
date: 2024-03-22
time: 12:09
status: #unprocessed

# 4 Threads & Concurrency
## 4.1 Overview
- A thread is a basic unit of CPU utilization. It comprises a thread Id, a program counter(PC), a register ser, and a stack.
- It shares with other threads belonging to the same process its code section, data section, and other operating-system resources, such as open files nad signals.
### 4.1.2 Benefits
The benefits of multithreading program
1. **Responsiveness**: Because any blocked part or lengthy operation wouldn't interfere with the running of program.
2. **Resource Sharing**: To share resources amongst processes you need to explicitly arrange that, while thread share the resources by default.
3. **Economy**: Allocating memory ans resources for process creation is costly. Becuase threads share the resources of the process to which thet belong, it is more economical to create and context-switch threads. Context switching is typically faster between threads than between processes.
4. **Scalability**: A single-threaded process can run on only one processor, regradless how many are available, while in multithreading programming you can use all the cores.