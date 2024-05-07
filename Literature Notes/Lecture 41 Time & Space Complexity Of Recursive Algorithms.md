domain: 
course: [[Data Structures & Algorithms Love Babbar]]
teacher:
date: 2024-05-02
time: 11:43
status: #unprocessed

# Lecture 41 Time & Space Complexity Of Recursive Algorithms
> Find the time complexity of factorial function?
> ```cpp
> int factorial(int n) {
> 	if (n == 0) return 1;
> 	return n * factorial(n - 1);
> }
> ```

- First we will find the recurrence relation.
- F(n) = n * F(n - 1)
- Let the time taken by if block be k1, and by multipliction of n & factorial be k2.
- T(n) = k1 + k2 + T(n - 1)
- T(n) = k + T(n - 1), where k = k1 + k2
- T(n - 1) = k + T(n - 2)
- T(n - 2) = k + T(n - 3)
- ......
- T(1) = k + T(0)
- T(0) = k1
- Adding all of the above, the terms T(n - 1), T(n - 2), .., T(0) cancels each other out.
- T(n) = k + k + ... (n times) + k1
- T(n) = n.k + k1
- So the time complexity will be O(n)

- You can also find the complexities by drawing recursive tree.
#todo/practice Find complexities of all the questiosn that you attempted in this recursion series.
#todo/practice https://www.naukri.com/code360/guided-paths/competitive-programming/content/126222/offering/1476042 Read & Solve

-  Find the space complexity of merge sort?
	- Answer 
		- O(N) - https://www.youtube.com/watch?v=aThGJFgk59E