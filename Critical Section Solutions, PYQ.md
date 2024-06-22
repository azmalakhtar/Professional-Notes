### How to Prove Correctness/Solve Questions of Critical Section?
1. **Mutual Exclusion**
	1. One process in critical section, another process tries to enter. Show that second process will block in entry code
	2. Two (or more) processes are in the entry code. Show that at most one will enter critical section
2. **Progress**(absence of deadlock)
	1. No process in critical section, P1 arrives, P1 enters
	2. Two (or more) processes are in the entry code. Show that at least one will enter critical section.
3. **Bounded Waiting**(fairness)
	1. One process in critical section, another process is waiting to enter, show that if first process exits the critical section and attempts to re-enter, show that waiting process will be able to get in.