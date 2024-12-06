[Data Structures & Algorithms - Problems](Data%20Structures%20&%20Algorithms%20-%20Problems.md)

## Description
A celebrity is a person who is known to all but does not know anyone at a party. A party is being organized by some people.  A square matrix mat is used to represent people at the party such that if an element of row i and column j is set to 1 it means ith person knows jth person. You need to return the index of the celebrity in the party, if the celebrity does not exist, return -1.

Note: Follow 0-based indexing.

## Algorithm
We use a two pointer approach.
If i knows j then i cannot be a celebrity
If i don't know j then j cannot be a celebrity
Using these conditions we either increment i or decrement j
This way we traverse over the array in O(N) time
The loop terminate when i == j
Then we check if i can be a celebrity
If yes then return i
Else return -1;

## Code
```cpp
    // Function to find if there is a celebrity in the party or not.
    int celebrity(vector<vector<int> >& mat) {
        // code here
        int i, j;
        i = 0;
        j = mat.size() - 1;
        
        while (i < j) {
            if (mat[i][j] == 1) i++;
            else j--;
        }
        
        for (int k = 0; k < mat.size(); k++) {
            // it knows noone
            // everybody knows him
            if (mat[i][k] == 1) return -1;
            else if (mat[k][i] == 0 && k != i) return -1;
        }
        
        return i;
    }
```

## Analysis
Time Complexity = O(N)
Space Complexity = O(1)

---
### Reference
https://www.geeksforgeeks.org/problems/the-celebrity-problem/1