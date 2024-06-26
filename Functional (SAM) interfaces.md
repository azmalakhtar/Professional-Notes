An interface with only one abstract method is called a functional interface, or a Single Abstract Method (SAM) interface. The functional interfaces can have several non-abstract memebers but only one abstract method (it can't have abstract properties).
```kt
fun interface Predicate {  
    fun accept(value: Int): Boolean  
}
```

## SAM conversions
Instead of creating a class that implements a functional interface manually, you can use a lambda expression whose signature is same as that of abstract method. This is know as SAM conversion.
```kt
// Creating an instance using lambda
val isEven = IntPredicate { it % 2 == 0 }
```

## Migration from an interface with constructor function to a functional interface
#notunderstood

## Functional interfaces vs. type aliases
You can also simply rewrite the above using a [type alias](https://kotlinlang.org/docs/type-aliases.html) for a functional type:
```kt
typealias IntPredicate = (i: Int) -> Boolean 
val isEven: IntPredicate = { it % 2 == 0 }
```

However, functional interfaces and [type aliases](https://kotlinlang.org/docs/type-aliases.html) serve different purposes. Type aliases are just names for existing types – they don't create a new type, while functional interfaces do.
#notcomplete