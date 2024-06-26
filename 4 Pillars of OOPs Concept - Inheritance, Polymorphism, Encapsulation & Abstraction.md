## Encapsulation
Is the bundling of data with the mechanism or methods that operate on the data. It may also refer to the limiting of direct access to some of that data, such as an object's components. Essentially, encapsulation prevents external code from being concerned with the internal workings of an object.

## Inheritance
Inheritance is the mechanism of basing an object or class upon another object (prototype-based inheritance) or class (class-based inheritance), retaining similar implementation.
```cpp
class <Class-Name> : <Visibility Modifier> <Super-Class> {};
```

### Access of the members
| Modifier in superclass | Mode of inheritance | Resulting modifier of the member |
--- | -- | --
`public` | `public` | `public`
`public` | `protected` | `protected`
`public` | `private` | `private`
`protected` | `public` | `protected`
`protected` | `protected` | `protected`
`protected` | `private` | `private`
`private` | `public` | Not accessible
`private` | `protected` | Not accessible
`private` | `private` | Not accessible

### Types of Inheritance
- Single inheritance - One class derives from another class
- Multilevel inheritance - One class drives from another class, which in turn derives from another class
- Multiple inheritance - A class drives from more than one class
- Hierarchical inheritance - One class is superclass for many subclasses
- Hybrid inheritance - Any combination of the above four

### Inheritance Ambiguity
If a class inherits multiple implemantation of function with same signature, then it need to override this function, as otherwise there will be an ambiguity as to which superclass implementation is to be called. To access this function of superclass, use scope resolution operator `this-><superclass>::<function-name>`.
```cpp
class A {
	public:
	void foo() {
		std::cout << "A" << std::endl;
	}
};

class B {
	public:
	void foo() {
		std::cout << "B" << std::endl;
	}
};
  
class C : public A, public B {
	public:
	void foo() { // C has to implement its own foo() method
		this->A::foo(); // the foo() implementation of the superclasses are accessed using :: operator
		this->B::foo();
	}
};
```

## Polymorphism
In computer science, polymorphism describes the concept that you can access objects of different types through the same interface. Each type can provide its own independent implementation of this interface.
Multiple forms
 It is of two types : compile-time & run-time polymorphism

### Compile Time Polymorphism
Also known as static polymorphism

It is implemented in one of two ways
1. Function overloading
2. Operator overloading `<return-type> operator<operator-symbol>(<params>) {/* COde */}`

### Run Time Polymorphism
ALso known as dynamic polymorphism
It is implemented as method overriding, thus it is inheritance dependent.

## Abstraction
Its main goal is to handle complexity by hiding unnecessary details from the user. That enables the user to implement more complex logic on top of the provided abstraction without understanding or even thinking about all the hidden complexity.