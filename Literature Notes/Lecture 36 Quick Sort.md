domain: 
course: [[Data Structures & Algorithms Love Babbar]]
teacher:
date: 2024-04-27
time: 12:11
status: #unprocessed

# Lecture 36 Quick Sort
## Algorithm
1. Choose a pivot element.
2. Put it at the index in which it should be if the array was sorted.
3. Put all the element smaller than pivot before the pivot element.
4. Recursively sort the right and left part.

## Code
```cpp
int partition(int *arr, int start, int end) {
  int pivot = (start + end) / 2;
  int index = start;
  for (int i = start; i <= end; i++) {
       if (arr[i] < arr[pivot]) index++;
  }
  swap(arr[pivot], arr[index]); // send pivot to its sorted index

  while (start < end) {
    while (arr[start] < arr[index]) start++;
    while (end >= index && arr[end] >= arr[index]) end--;
    if (start < end) swap(arr[start], arr[end]);
  }
  return index;
}

void quickSort(int *arr, int start, int end) {
  if (start >= end) return;
  int pivotIndex = partition(arr, start, end);
  quickSort(arr, start, pivotIndex - 1); // sort the left part
  quickSort(arr, pivotIndex + 1, end); // sort the right part
}
```

## Complexities
- Time Complexity
	- Best Case - O(N.log(N))
	- Worst Case - O(N.N)
- Space Complexity - O(N)