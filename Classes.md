## Constructors
A class in kotlin has a primary constructor and possibly one or more secondary constructors. If the primary constructor does not have any annotations or visibility modifiers, the constructor keyword can be omitted:
```kt
class Person constructor(firstName: String) { /*...*/ }
class Person(firstName: String) { /*...*/ }
```

To execute code during object creation, use initializer blocks inside the class body. The initializer blocks are executed in the same order as they appear in the class body, interlevaed with property initializers:
```kt
//sampleStart
class InitOrderDemo(name: String) {
	val firstProperty = "First property: $name".also(::println)
	init {
		println("First initializer block that prints $name")
	}
	val secondProperty = "Second property: ${name.length}".also(::println)
	init {
		println("Second initializer block that prints ${name.length}")
	}
}
//sampleEnd
fun main() {
InitOrderDemo("hello")
}

/* Output
First property: hello
First initializer block that prints hello
Second property: 5
Second initializer block that prints 5
*/
```

Primary constructor parameters can be used in the initializer blocks and property initializers declared in the class body.
### Secondary Constructors
A class can also declare secondary constructors, which are prefixed with constructor
```kt
class Person(val pets: MutableList<Pet> = mutableListOf())
class Pet {
	constructor(owner: Person) {
		owner.pets.add(this) // adds this pet to the list of its owner's pets
	}
}
```

If the class has a primary constructor, each secondary constructor needs to delegate to the primary constructor, either directly or indirectly through another secondary constructor(s). Delegation to another constructor of the same class is done using the this keyword:
```kt
class Person(val name: String) {
	val children: MutableList<Person> = mutableListOf()
	constructor(name: String, parent: Person) : this(name) {
		parent.children.add(this)
	}
}
```

The code in initializer block & property initializers executes before the body of the secondary constructor. Even if the class has no primary constructor, the delegation still happens implicitly, and the initializer blocks & property initializers are still executed.

If a non-abstract class does not declare any constructors (primary or secondary), it will have a generated primary constructor with no arguments. The visibility of the
constructor will be public
## Class Memebers
Classes can contain:
- [Constructors and Initializer blocks](Classes.md#Constructors)
- Functions
- Properties
- Nested and inner classes
- Object declarations
## Abstract Classes
A class may be declared `abstract`, along with some or all of its members. An abstract member does not have an implementation in its class.
```kt
abstract class Polygon {
	abstract fun draw()
}
class Rectangle : Polygon() {
	override fun draw() {
		// draw the rectangle
	}
}
```
You can override a non-abstract `open` member with an abstract one.
```kt
open class Polygon {
	open fun draw() {
	// some default polygon drawing method
	}
}
abstract class WildShape : Polygon() {
	// Classes that inherit WildShape need to provide their own
	// draw method instead of using the default on Polygon
	abstract override fun draw()
}
```
## Companion Objects
If you need to write a function that can be called without having a class instance but that needs access to the internals of a class (such as a factory method), you can write it as a member of an [object declaration](https://kotlinlang.org/docs/object-declarations.html) inside that class.

Even more specifically, if you declare a [companion object](https://kotlinlang.org/docs/object-declarations.html#companion-objects) inside your class, you can access its members using only the class name as a qualifier.