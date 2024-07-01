A class `Derived` can implement an interface `Base` by delegating all of its public members to a specified object:
```kt
interface Talkable {  
    fun talk(): String  
}  
interface Walkable {  
    fun walk(): String  
}  
  
class Human : Talkable, Walkable {  
    override fun talk(): String = "Hello"  
    override fun walk(): String = "Taking a walk"  
}  
  
class Student(human: Human) : Talkable by human, Walkable by human // by-clause

fun main() {  
    val student = Student(Human())  
    println(student.talk())  // Hello
    println(student.walk())  // Taking a walk
}
```
The `by`-clause in the supertype list for `Derived` indicates that `b` will be stored internally in objects of `Derived` and the compiler will generate all the methods of `Base` that forward to `b`.

## Overriding a member of an interface implemented by delegation
Use the `override` keyword. Note, however, that members overridden in this way do not get called from the members of the delegate object, which can only access its own implementations of the interface members:
```kt
interface Base {
    val message: String
    fun print()
}

class BaseImpl(x: Int) : Base {
    override val message = "BaseImpl: x = $x"
    override fun print() { println(message) }
}

class Derived(b: Base) : Base by b {
    // This property is not accessed from b's implementation of `print`
    override val message = "Message of Derived"
}

fun main() {
    val b = BaseImpl(10)
    val derived = Derived(b)
    derived.print()  // BaseImpl: x = 10 -> print() is using the message of BaseImpl not of Derived, because it can only access BaseImpl methods
    println(derived.message)  // Message of derived -> This comes from the Derived class
}
```