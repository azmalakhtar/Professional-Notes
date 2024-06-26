Kotlin provides the ability to extend a class or an interface with new functionality without having to inherit from the class or use design patterns such as Decorator. This is done via special declarations called extensions.

## Extension Functions
To declare an extension function, prefix its name with a receiver type, which refers to the type being extended
```kt
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {  
    val tmp = this[index1] // 'this' corresponds to the list  
    this[index1] = this[index2]  
    this[index2] = tmp  
}
```

## Extensions are resolved statically
Extension functions are dispatched statically. So which function is called is already known at compile time based on the receiver type (not on the object type).
```kt
fun main() {  
    open class Shape {  
        open fun getJam() = "ShapeJam"  
    }  
    class Rectangle : Shape() {  
        override fun getJam() = "RectangleJam"  
    }  
  
    fun Shape.getName() = "Shape"  
    fun Rectangle.getName() = "Rectangle"  
    fun printClassName(s: Shape) {  
        println(s.getName())  
        println(s.getJam())  
    }  
    printClassName(Rectangle())  
	/* Output
	Shape => it depends on the receiver type which is Shape not Rectangle
	RectangleJam => it depends on the object type which is Rectangle  
	*/
}
```
If a class has a member function, and an extension function is defined which has the same receiver type, the same name, and is applicable to given arguments, the member always wins. However, it's perfectly OK for extension functions to overload member functions that have the same name but a different signature.

## Nullable receiver
Extensions can be defined with a nullable receiver type. These extensions can be called on the object variable even if its value is null.
```kt
fun String?.getLength(): Int {  
    if (this == null) return -1    
    return this.length
}
```

## Extension properties
Kotlin supports extension properties.
```kt
val <T> List<T>.lastIndex: Int
	get() = size - 1
```

Initializers are not allowed for extension properties as they don't have a backing field. Their behaviour can only be defined by explicitly providing getters/setters.

## Companion Objects
Kotlin allows you to define xtension function and properties for the companion object. Use `.Companion` between receiver type and name of extension.
```kt
class MyClass {  
    companion object { }  
}  
// "Companion" is used  
fun MyClass.Companion.printCompanion() { println("companion") }  
fun main() {  
    MyClass.printCompanion()  
}
```

## Scope of extensions
To use an extension outside its declaring package, import it at the call site

## Declaring extensions as members
You can declare extensions for one class inside another class. An instance of the class in which the extension is declared is called a dispatch receiver, and an instance of the receiver type of the extension method is called an extension receiver.
```kt
class Host(val hostname: String) {  
    fun printHostname() {  
        print(hostname)  
    }  
}  
  
class Connection(val host: Host, val port: Int) {  
    fun printPort() {  
        print(port)  
    }  
  
    fun Host.printConnectionString() {  
        printHostname() // calls Host.printHostname()  
        print(":")  
        printPort() // calls Connection.printPort()  
    }  
  
    fun connect() {  
        host.printConnectionString()  // calls the extension function  
    }  
}
fun main() {
    Connection(Host("kotl.in"), 443).connect()
    //Host("kotl.in").printConnectionString() // error, the extension function is unavailable outside Connection
}
```
In the event of a name conflict between the members of a dispatch receiver and an extension receiver, the extension receiver takes precedence. To refer to the member of the dispatch receiver, you can use the qualified this syntax.
```kt
class Connection {  
    fun Host.getConnectionString() {  
        toString() // calls Host.toString()  
        this@Connection.toString() // calls Connection.toString()  
    }  
}
```

Extensions declared as members can be declared as open and overridden in subclasses. This means that the dispatch of such functions is virtual with regard to the dispatch receiver type, but static with regard to the extension receiver type.
```kt
open class Base { }  
class Derived : Base() { }  
open class BaseCaller {  
    open fun Base.printFunctionInfo() {  
        println("Base extension function in BaseCaller")  
    }  
    open fun Derived.printFunctionInfo() {  
        println("Derived extension function in BaseCaller")  
    }  
    fun call(b: Base) {  
        b.printFunctionInfo() // call the extension function  
    }  
}  
class DerivedCaller: BaseCaller() {  
    override fun Base.printFunctionInfo() {  
        println("Base extension function in DerivedCaller")  
    }  
    override fun Derived.printFunctionInfo() {  
        println("Derived extension function in DerivedCaller")  
    }  
}  
fun main() {  
    BaseCaller().call(Base()) // "Base extension function in BaseCaller"  
    DerivedCaller().call(Base()) // "Base extension function in DerivedCaller" - dispatch receiver is resolved virtually  
    DerivedCaller().call(Derived()) // "Base extension function in DerivedCaller" - extension receiver is resolved statically  
}
```

## Note on visibility
Extensions utilize the same visibility modifiers as regular functions declared in the same scope would.