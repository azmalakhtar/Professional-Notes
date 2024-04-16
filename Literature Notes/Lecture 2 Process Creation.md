domain: 
course: [[Operating System Mindmatrix]]
teacher:
date: 2024-03-19
time: 09:32
status: #unprocessed

# Lecture 2 Process Creation
## What happen when you switch on the Computer?
- OS is stored on secondary memory (hard disk). It is stored in executable format.
- Computers have a **ROM**.
- ROM contains a boot loader. Boot loader knows/stores the address of OS. Then it loads the OS onto main memory.

## Can you make changes in OS?
- High Level Steps
	1. Download open source OS code.
	2. Make all the changes.
	3. Compile it -> Executable file.
	4. Store the executable file is secondary memory at a very special place.
	5. Point bootloader to that address.

## Process Management
- **Process** -> A program in execution
- Process Virtual Memory ![Process In memory](https://imgs.search.brave.com/UqwV5iukZsgr3O6RMLkNFl-Z0kDBXJhxdaxVbPzaBhM/rs:fit:860:0:0/g:ce/aHR0cHM6Ly93d3cu/dHV0b3JpYWxzcG9p/bnQuY29tL2ludGVy/X3Byb2Nlc3NfY29t/bXVuaWNhdGlvbi9p/bWFnZXMvcHJvY2Vz/c19pbWFnZS5qcGc)
- In physical main memory, these sections can be stored at any place.The above diagram is the virtual memory of process.

## Process Control Block PCB
> [!note]
> A pcb, also sometimes called a process descriptor is a data structure used by a computer os to store all the information about a process.

- For each process, there exists a PCB.
- Data structure is used is similar to struct
- Why PCB?
	- As the os pause one process and starts another, the register being used by the paused process gets overriden/used by the current process and all the data stored by the first process on the registers is lost. Without PCB the process will have to execute from the start again each time it is paused. By using PCB, we can store the state of process and when it is resumed , use that state.
- Process Attributes
	1. Identifier: Unique id for process.
	2. State
	3. Priority
	4. Program Counter
	5. Memory Printers
	6. Context Data
	7. I/O Status Information
	8. Accounting Information

## Context Switching
> [!note]
> Process of saving context of one process & loading the context of another process

- Context switching happens when -
	1. A high priority process comes to ready state
	2. An interrupt occurs
	3. User & Kernel mode switch
	4. Preemptive CPU scheduling used

## Process Table
> [!note]
> Data structure maintained by the OS to facilitate context switching & scheduling. It contains all the current processes information or PCBs in the system.

## How program becomes process?
- Steps
	1. Create space for the program on main memory.
	2. Load the program on main memory.
- These steps are done using two systems calls, `fork` & `exec`.

## `fork`
- Creates exact replica of the parent process.
- Parent and child process both will **start executing after the line where `fork` is called**.
- `fork()` will return integer value to both processes.
- fork returns 
	- 0 -> to child process
	- non-zero -> to parent process, in this case it returns the process id of the child.
