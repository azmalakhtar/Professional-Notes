# 4. Types, Variables, and Arithmetic
A **declaration** is a statement that introduces an entity into the program. It specifies a type for the entity.
A **type** defines a set of possible values and a set of operations (for an object).
An **object** is some memory that holds a value of some type.
A **value** is a set of bits interpreted according to a type.
A **variable** is a named object.

## 1. Arithmetic
- Arithmetic
- Comparison
- Logical
- Assignment

In assignments and arithmetic operations, C++ performs all meaningful conversions between the basic types. The conversions used in expressions are called **usual arithmetic conversions** and aim to ensure that expressions are evaluated at the highest precision of its operands. For example, an addition of a double and int is calculated using double-precision floating-point arithmetic.

The order of evaluation of expressions is left to right, except for assignments, which are right-to-left. The order of evaluation of function arguments is unfortunately unspecified.

## 2. Initialization
C++ offers a variety of notations for expressing initialization, such as `=` and `{}`.
```cpp
int a = 1.0;
int b {2};
```

`=` is for backward compatibility with C. If in doubt, use the general `{}`-list form. It saves you from implicit narrowing conversions.

A constant cannot be left uninitialized.

You can use `auto` to automatically deduce the type from the initializer. With `auto`, it's okay to use `=` because there is no potentially type conversion involved. 
```cpp
auto b = 17;
auto c = 3.14;
```

It's common to use `auto` where you don't have a specific reason to mention the type explicitly. Specific reasons include:
- The definition is in a large scope where we want to make the type clearly visible to readers of our code.
- We want to be explicit about a variable's range or precision (e.g., double rather than float).

# 5. Scope and Lifetime
A declaration introduces its name into a scope: local, class, namespace scope. A name not declared inside any other construct is in the global namespace.

An object will be destroyed at the end of its scope, except for the ones created by `new`, which lives until destroyed until by `delete`.

# 6. Constants
C++ supports two notions of immutability:
- `const` : meaning roughly "I promise not to change this value". This is used primarily to specify interfaces so that data can be passed be to functions using pointers and references without fear of being it modified. The compiler enforces the promise made by const. The value of a const can be calculated at **run-time**.
- `constexpr` : meaning roughly "to be evaluated at compile time". This is used to primarily to specify constants, to allow placement of data in read-only memory, and for performance. The value of a constexpr must be calculated at **compile-time**.
```cpp
int sum(const vector<int>& numbers) {
	int result = 0;
	for (int number : numbers) {
		result += number;
	}

	numbers[0] = -1; // error, as numbers is a const, we can't modify it.
	return result;
}
```

```cpp
const int a = 7; 
constexpr int b = 7;
const int c = square(8);
constexpr int d = square(8); // error, as the value can't be evaluated at compile time
```

For a function to be usable in a constant expression, that is, an expression that will be evaluated by the compiler, it must be defined `constexpr`. A `constexpr` function can be used for non-constant arguments, but when that is done the result is not a constant expression.
```cpp
constexpr int square(int num) {
	return num * num;
}
int main() {
	constexpr int d = square(8);	
	int a = -2;
	const int e = square(a);
	constexpr int f = square(a); // error, as the function is constexpr but the expression is not constant expression
	return 0;
}
```

To be `constexpr`, a function must be rather simple and cannot have side effects and can only use information passed to it as arguments. In particular, it cannot modify non-local variables, but it can have loops and use its own local variables.

---
### References
- [C++ In-Depth Tour](C++%20In-Depth%20Tour.md)