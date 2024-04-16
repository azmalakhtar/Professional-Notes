domain: [[Computer Science]]
course: [[Data Structures & Algorithms Love Babbar]]
teacher:
date: 2024-03-19
time: 15:35
status: #unprocessed

# Lecture30 Macros, Global Variables, Inline Functions & Default Args
## Macros
- A macro is a preprocessor directive. It is a piece of code in a program that is replaced by the value of tha macro.
- Syntax - `#define <macro-name> <macro-value>`.
	```cpp
	#define PI 3.14 
	#define AREA(l, b) (l * b)
	```
- macros are useful when we need to use the same piece of code in different places. As it is a preprocessor directive, it also **doesn't occupy any space** and is **constant**.

## Global Variables
- Don't use them. Instead prefer reference variables over them.

## Inline Functions
- An inline function is a function that is expanded in line when it is called.
- To request to the compiler to make a function inline, add `inline` keyword before function definition. As inlining is a request, it is upto the compiler whether it makes the function inline or not.
- Function call have a certain overhead, like creating new variables on stack. But if a function is inline, there is no such overhead.

