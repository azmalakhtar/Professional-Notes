domain: [[Computer Science]]
course: [[Data Structures & Algorithms Love Babbar]]
teacher: 
date: 2024-03-19
time: 14:46
status: #unprocessed

# Lecture29 Dynamic Memory Allocation of 2D Arrays
- Creating a dynamic 2d array
	```cpp
	int** arr = new int*[rows];
	for (int i = 0; i < rows; i++) {
		arr[i] = new int[cols];
	}
	```
- Deleting a dynamically created 2d array
	```cpp
	for (int i = 0; i < rows; i++) {
		delete [] arr[i];
	}
	delete [] arr;
	```