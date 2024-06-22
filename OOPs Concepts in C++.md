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

`this` is a pointer that points to the current object of the class.

Each class has its own default copy constructor, which does what the name implies, make the object copy of another object. You can also implement your own copy constructor, by making a constructor which takes as input of the same class.

Default copy constructor makes a shallow copy of the object. To make a deep copy we need to implement our own copy constructor.

Copy assignment operator (`object1 = object2`) copies the value of one object to another.

Destructors are called when the object is being deleted. Every class has a default destructor and you can also code destructors, in which case the custom destructor will be called instead of default destructor. A destructor is a function that has no parameter, no return type and its name is same as class name prefixed with a ~ .
For statically allocated objects, the destructor is called automatically when the scope of it ends, but in the case of dynamically allocated objects, we have to explicitly delete the object, which will call the destructor.

Static members belongs to the class, unlike non static members which belong to the objects. Static functions can only access static data members of the class.
