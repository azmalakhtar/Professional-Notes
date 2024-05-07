domain: [[Computer Science]]
course: [[Atlas/Operating System Mindmatrix]]
teacher:
date: 2024-03-20
time: 10:12
status: #unprocessed

# Lecture 5 System Call Execution
- How many times will 4 be printed?
	```c
	#include <stdio.h>
	#include <unistd.h>


	int main() {
	    if (fork() && (!fork())) {
	        if (fork() || fork()) {
	            fork();
	        }
	    }

	    printf("4\n");
	    return 0;
	}
	```
	- Answer 
		- 7
- User & Kernel Space
	- **User space** -> less privileged memory space where user processes execute
	- **Kernel space** -> Privileged memory space where the OS main process resides. No user application should be able to change.
	```mermaid
		flowchart
		UserSpace --> KernelSpace
		KernelSpace --> Hardware
	```
- What is a System Call
	- different definition based on different perspectives
	- Answer 1: Request to kernel to perform a service
		- Open a file
		- Execute a new program
		- Create a new process
		- Send a message to another process
	- Answer 2: Call A function (from programmer's perspective)
		- system call looks like any other function call
	- Answer 3: Entry point providing controlled mechanism to execute kernel code
		- syscall = one of few mechanism by which program can ask to execute kernel code
		- System call are:
			- OS specific
			- Limited/Strictly defined by OS
				- Linux kernel provides 400+ syscall
- System call Execution
	-  Each system call has a unique numeric identifier
	- OS has a system call table that maps numbers to functionality requested (address of the kernel code corresponding to the syscall).
	- When invoking a system call, user places system call number and associated parameters in a specific register, then executes the syscall instruction.
	```assembly
		// assembly instructions for a fork() call
		mov rax, 57;
		syscall;
	```
- Steps in system call execution
	- Program calls wrapper function in C library (`printf`, `fork` etc)
	- Wrapper Function
		- Packages syscall arguments into registers
		- Puts (unique) syscall number into a register
	- Wrapper flips CPU to kernel mode
		- Execute special machine instruction ( e.g. syscall)
		- Main effect: CPU can now touch memory marked as accessible only in kernel mode
	- Kernel executes syscall handler
		- Invokes service routine corresponding to syscall number. Do the real work, generate result status
		- Places return value from service routine in a register
		- Switches back to user mode, passing control back to wrapper
- Inside the syscall Instruction
	1. Saves the old (user) SP (stack pointer) value.
	2. Switches the SP to kernel stack.
	3. Saves the old (user) PC (program counter) value (= return addr)
	4. Saves the old privilege mode
	5. Sets the new privilege mode to 0
	6. Sets the new PC to the kernel syscall handler.
- System call Table:
	- protected entry points into the kernel for each system call. We don't want application to randomly jump into any part of the OS Code
	- Syscall table is usually implemented as an array of function pointers, where each function implements one system call
	- Syscall table is indexed via system call number
- Overhead
	- System call are very slow
	- They can take tens to hundreds of thounsands of clock cycles.
	- This is due to:
		1. Changing protection domains
		2. Validating arguments
		3. Adjusting memory mappings
		4. Cache Effects
	- Programs should make fewer system calls when practical.