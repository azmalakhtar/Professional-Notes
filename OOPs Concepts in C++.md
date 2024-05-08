- "private" is the default access modifier.
- Getters & setters are also there.
- #todo/read padding & greedy alignment
- You can create objects on either heap or stack.
- To access a field of a object from its pointer use `->` instead of `.` , 
```cpp
User *user = new User;
cout << user->fullName << endl;
cout << (*user).fullName << endl;
```