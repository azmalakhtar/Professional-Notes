Many modern computer systems provide special hardware instructions that allow us either to test and modify the content of a word or to swap the contents of two words **atomically**- that is, as one uninterruptible unit. There are roughly two types - `test_and_set()` and `compare_and_swap()`.
```cpp
boolean test_and_set(boolean *target) {
	boolean rv = *target;
	*target = true;
	return rv;
}
```


Mutual exclusion with `test_and_set()`. `lock` is initialized to `false`.
```cpp
do {
	while (test_and_set(&lock))
		; /* do nothing */
		/* critical section */
	lock = false;
	/* remainder section */
} while (true);
```

The definition of compare & swap.
```cpp
int compare_and_swap(int *value, int expected, int new value) {
	int temp = *value;
	if (*value == expected)
		*value = new value;
	return temp;
}
```
Mutual exclusion with compare and swap.
```cpp
while (true) {
	while (compare and swap(&lock, 0, 1) != 0)
		; /* do nothing */
		/* critical section */
	lock = 0;
	/* remainder section */
}
```

In both of the above solutions, bounded waiting is not satisfied. For that we have a modified version using `compare_and_swap()`.
```cpp
while (true) {
	waiting[i] = true;
	key = 1;
	while (waiting[i] && key == 1)
		key = compare and swap(&lock,0,1);
	waiting[i] = false;
		/* critical section */
	j = (i + 1) % n;
	while ((j != i) && !waiting[j])
		j = (j + 1) % n;
	if (j == i)
		lock = 0;
	else
		waiting[j] = false;
	/* remainder section */
}
```