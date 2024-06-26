All classes in Kotlin have a common superclass `Any`, which is the default superclass for a class with no supertypes declared. `Any` has three methods: `equals()`, `hashCode()`, and `toString()`.
```kt
class Example // Implicitly inherits from Any
```

By default, Kotlin classes are final - they can't be inherited. To make a class inheritable, mark it with the `open` keyword.
```kt
open class Base // Class is open for inheritance
```
To declare an explicit supertype, place the type after a colon in the class header:
```kt
open class Base(p: Int)
class Derived(p: Int) : Base(p)
```

If the derived class has a primary constructor, the base class can (and must) be initialized in that primary constructor. Else each secondary constructor has to initialize the base type using the `super` keyword or it has to delegate to another constructor which does.
```kt
class MyView : View {
	constructor(ctx: Context) : super(ctx)
	constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

## Overriding methods
Kotlin requires explicit modifiers for overridable members and overrides. The `override` modifier is required to override. 
```kt
open class Shape {
	open fun draw() { /*...*/ }
	fun fill() { /*...*/ }
}
class Circle() : Shape() {
	override fun draw() { /*...*/ }
}
```

A member marked `override` is itself open, so it may be overridden in subclasses. If you want to prohibit re-overriding, use `final`:
```kt
open class Rectangle() : Shape() {
	final override fun draw() { /*...*/ }
}
```

## Overriding properties
The overriding mechanism works on properties in the same way that it does on methods. Each declared property can be overridden by a property with an initializer or by a property with a `get` method:
```kt
open class Shape {  
    open val vertexCount: Int = 0  
}  
  
class Rectangle : Shape() {  
    override val vertexCount = 4  
}
```

You can also override a `val` property with a `var` property, but not vice-versa. This is allowed because a `val` property essentially declares a `get` method, and overriding it as a `var` additionally declares a `set` method in the derived class.

## Derived class initialization order
During the construction of a new instance of a derived class, the base class initialization is done as the first step (preceded only by evaluation of the arguments for the base class constructor), which means that it happens before the initialization logic of the derived class is run.
```kt
open class Base(val name: String) {  
  
    init { println("Initializing a base class") }  
  
    open val size: Int =  
        name.length.also { println("Initializing size in the base class: $it") }  
}  
  
class Derived(  
    name: String,  
    val lastName: String,  
) : Base(name.replaceFirstChar { it.uppercase() }.also { println("Argument for the base class: $it") }) {  
  
    init { println("Initializing a derived class") }  
  
    override val size: Int =  
        (super.size + lastName.length).also { println("Initializing size in the derived class: $it") }  
}
/* Output
Argument for the base class: Steve
Initializing a base class
Initializing size in the base class: 5
Initializing a derived class
Initializing size in the derived class: 11
*/
```

This means that when the base class constructor is executed, the properties declared or overridden in the derived class have not yet been initialized.

## Calling the superclass implementation
Code in a derived class can call its superclass functions and property accessor implementations using the `super` keyword:
```kt
open class Rectangle {  
    open fun draw() { println("Drawing a rectangle") }  
    val borderColor: String get() = "black"  
}  
  
class FilledRectangle : Rectangle() {  
    override fun draw() {  
        super.draw()  
        println("Filling the rectangle")  
    }  
  
    val fillColor: String get() = super.borderColor  
}
```

Inside an inner class, accessing the superclass of the outer class is done using the `super` keyword qualified with the outer class name: `super@Outer`:
```kt
class FilledRectangle: Rectangle() {  
    override fun draw() {  
        val filler = Filler()  
        filler.drawAndFill()  
    }  
  
    inner class Filler {  
        fun fill() { println("Filling") }  
        fun drawAndFill() {  
            super@FilledRectangle.draw() // Calls Rectangle's implementation of draw()  
            fill()  
            println("Drawn a filled rectangle with color ${super@FilledRectangle.borderColor}") // Uses Rectangle's implementation of borderColor's get()  
        }  
    }  
}
```

## Overriding rules
if a class inherits multiple implementations of the same member from its immediate superclasses, it must override this member and provide its own implementation (perhaps, using one of the inherited ones). To denote the supertype from which the inherited implementation is taken, use `super` qualified by the supertype name in angle brackets, such as `super<Base>`:
```kt
open class Rectangle {  
    open fun draw() { /* ... */ }  
}  
  
interface Polygon {  
    fun draw() { /* ... */ } // interface members are 'open' by default  
}  
  
class Square() : Rectangle(), Polygon {  
    // The compiler requires draw() to be overridden:  
    override fun draw() {  
        super<Rectangle>.draw() // call to Rectangle.draw()  
        super<Polygon>.draw() // call to Polygon.draw()  
    }  
}
```