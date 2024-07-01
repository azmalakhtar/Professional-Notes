```kt
enum class Direction { NORTH, SOUTH, WEST, EAST }
```
Each enum constant is an object. Since each enum is an instance of the enum class, it can be initialized as:
```kt
enum class Color(val rgb: Int) { 
	RED(0xFF0000), 
	GREEN(0x00FF00), 
	BLUE(0x0000FF) 
}
```

## Anonymous classes
Enum constants can declare their own anonymous classes with their corresponding methods, as well as with overriding base methods.
```kt
enum class UiState {  
    LOADING {  
        override fun displaySymbol() {  
            println("<LOADING_SYMBOL>")  
        }  
    },  
    SUCCESS {  
        var data: Int = 0  
            get() = field  
            private set  
        fun increment() {  
            data++  
        }  
        override fun displaySymbol() {  
            println("<SUCCESS_SYMBOL>")  
        }  
    },  
    ERROR {  
        val error: String = "Unknown"  
        override fun displaySymbol() {  
            println("<ERROR_SYMBOL>")  
        }  
    };  
    abstract fun displaySymbol(): Unit  
}
```

## Implementing interfaces in enum classes
An enum class can implement an interface (but it cannot derive from a class), providing either:
- a common implementation of interface members for all the entries, or
- a separate implementations for each entry within its anonymous class.
```kt
interface Walkable {  
    fun walk(): Unit  
}  
interface Talkable {  
    fun talk(): Unit  
}  
  
enum class Animals: Talkable, Walkable {  
    DOG {  
        override fun talk() = println("bhau..bhau")  
    },  
    CAT {  
        override fun talk() = println("meuw")  
    },  
    COW {  
        override fun talk() = println("moo")  
    };  
  
    override fun walk() = println("walking on four legs")  
}
```

All enum classes implement the Comparable interface by default.

## Working with enum constants
```kt
EnumClass.valueOf(value: String): EnumClass
EnumClass.entries: EnumEntries<EnumClass> // specialized List<EnumClass>
```

```kt
enum class RGB { RED, GREEN, BLUE }

fun main() {
    for (color in RGB.entries) println(color.toString()) // prints RED, GREEN, BLUE
    println("The first color is: ${RGB.valueOf("RED")}") // prints "The first color is: RED"
}
```
`valueOf()` method can throw an `IllegalArgumentException`.

Every enum constant has `name` and `ordinal` properties.

To access the constants in an class in a generic way, use `enumValue<T>()`. In Kotlin 2.0.0, the `enumEntries<T>` function is introduced serving the same purpose. `enumEntries<T>()` is more efficient because it return the same list each time, while `enumValues<T>` creates a new array every time.
```kt
fun main() {  
    val enumValues = enumValues<Animals>()  
    enumValues.forEach { animal ->  
        println(animal.name)  
        animal.talk()  
        animal.walk()  
    }  
}
```
