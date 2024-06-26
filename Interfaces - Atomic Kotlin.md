> An interface describes the concept of a type. It is a prototype for all classes that implement the interface.

It describes what a class should do, but not how it should do it. One dictionary definition is that an interface is "The place at which independent and often unrelated systems meet and act on or communicate with each other.".

To create an interface use the `interface` keyword instead of a `class` keyword. When defining a class that implements an interface, follow the class name with a `:` (colon) and the name of the interface.
A class that implements the interface must provide bodies for all the declared functions and declared properties making those members concrete (3 & 4). There is no need to override properties who have a getter and functions who are implemented (1 & 2).
```kt
interface Computer {  
	val isComputer: Boolean // [1]
		get() = true 
	fun shutDown(): String = println("Shutting down..") // [2]
    val notation: Char // [3]
    fun prompt(): String // [4]
    fun calculateAnswer(): Int  
}  
  
class Windows : Computer {  
    override val notation: Char = 'W'  
    override fun prompt(): String = "Command Prompt Because Windows"  
    override fun calculateAnswer(): Int = 20  
}  
  
class MacOS : Computer {  
    override val notation: Char  
        get() = 'M'  
    override fun prompt(): String = "Fish Because MacOS"  
    override fun calculateAnswer(): Int = 50  
}  
  
class Linux : Computer {  
    override val notation: Char  
        get() = 'L'  
    override fun prompt(): String = "Bash Because Linux"  
    override fun calculateAnswer(): Int = 100  
}
```

An [enumeration](Enumerations.md) can also implement an interface.

## Functional (SAM) interfaces
An interface with only one abstract method is called a functional interface, or a Single Abstract Method (SAM) interface.
