Interfaces in Kotlin can contain declarations of abstract methods, as well as method implementations. What makes them different from abstract classes is that interfaces cannot store state. They can have properties, but these need to be abstract or provide accessor implementations.
```kt
interface Person {  
    val firstName: String  
    val lastName: String get() = ""  
    // val age: Int = 0  // don't compile
    fun tellReligion(): String  
    fun greet() = println("Hi, I'm $firstName $lastName")  
}
```

A class or object can implement one or more interfaces.

## Interfaces Inheritance
An interface can derive from other interfaces, meaning it can both provide implementations for their members and declare new functions and properties.
```kt
interface Student : Person {  
    val facultyNumber: String  
    override fun greet() = println("Good morning sir")  
}
```

## Resolving overriding conflicts
If you derive from two interfaces which have same method, then you need to implement that method in the derived class, even if only one of the superclass implemented it:
```kt
interface A {  
    fun foo() = println("A")  
    fun bar() = println("A bar")  
    fun book() = println("book")  
}  
interface B {  
    fun foo() = println("B")  
    fun bar(): Unit  
}  
  
class C : A , B {  
    override fun foo() {  // Both superclass provide implementations
        TODO("Not yet implemented")  
    }  
  
    override fun bar() {  // Only A provides implementation
        TODO("Not yet implemented")  
    }  
}
```