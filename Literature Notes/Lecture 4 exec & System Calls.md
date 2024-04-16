domain: 
course: [[Operating System Mindmatrix]]
teacher:
date: 2024-03-19
time: 10:20
status: #unprocessed

# Lecture 4 exec & System Calls
```c
int main() {
	int i = 0;
	while (i < 2) {
		int pid = fork();
		if (pid)
			waitpid(pid, NULL, 0);
		printf("%d", i);
		i = i + 1;
	}
}
```
- What will be the output of this program
	- A: 0 1 0 1
	- B: 0 0 1 1 1 1
	- C: 0 1 0 1 1 1
	- D: 0 1 1 0 1 1
	- E: 0 1 1 1 0 1
- Answer
	- D

## `exec` system call
> `exec` system call when encountered in the program replaces the current process with the new process.

## Process Creation
- We are going to consider the case of unix based system.
- Operating system loads the terminal/shell on the main memory.
- Whenever we try to run a process using shell, it runs that program in two steps
	- First, the shell will call `fork` to create a child shell, which will occupy space in main memory.
	- Then, the child shell will call `exec` to replace itself with whatever process you gave it to run. 

The rough shell code template looks something like this
```c
while (true) {
	scanf("%s", );
	int pid = fork();
	if (pid == 0) {
		execv(<filename>);
	}
	else {
		wait();
	}
}
```

## System Calls
- so far we have seen four system calls
	- `fork`
	- `exec`
	- `wait` - waits till the child process gets completed
	- `exit` - exit the program (delete the pcb)
> [!note]
> A system call is the programmatic way in which a computer program requests a service from the operating system on which it is executed.
> System calls provide an essential interface between a process and the operating system.

- All system calls are part of kernel ( a kernel is a part of os that contains most important code of os).

- System call vs function call
	- A system call executes the code present in o.s. code while a function call executes the code present in the program.

- Types of system calls
	- process control
	- file management
	- device management
	- information maintenance
	- communications

### Dual mode operations in os
1. User mode
	- When the computer system runs the user app then the system is in user mode.
	- It has limited access to resources.
	- When user app requests a service from OS or an interrupt occurs on system or system call, then there will be transition from user mode(1) to kernel mode(0).
2. Kernel Mode
	- When system boots then hardware starts in kernel mode & when OS is loaded then it starts user app in user mode. 
	- We have privileged instructions (handling interrupts, I/O management) that execute in kernel mode.
![dual mode](https://imgs.search.brave.com/FKIvOihzDpJpbPstSHMDOenIcL8z4bA1qJV3QNNvv1k/rs:fit:500:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS5nZWVrc2Zvcmdl/ZWtzLm9yZy93cC1j/b250ZW50L3VwbG9h/ZHMvZHVhbF9tb2Rl/LmpwZWc)

- Privileged Instructions
	- can run only in kernel mode
	- any attempts to execute p-instruction in user mode, is treated as an illegal instructions. H/W traps it to the O/S.
	- Any instruction that can modify the contents of the timer is a privileged instructions.
	- P instruction are used by the OS in order to achieve correct operation.
	- e.g. I/O instruction & halt instruction, turn off all interrupts, setting timer, context switching, clearing memory. modifying entries on device-status table.
- Non-privileged Instructions
	- can run only in user mode
	- e.g. reading status of processor, reading system time, generating any trap instruction, sending final printout to printer.