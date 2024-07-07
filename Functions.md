Kotlin functions are declared using the `fun` keyword:

```kotlin
fun double(x: Int): Int {
    return 2 * x
}
```

## Function Usage

### Default arguments
An overriding function is not allowed to specify default values for its parameters.
```kotlin
open class X {  
    open fun double(number: Int = 0): Int = number * 2  
    open fun triple(number: Int): Int = number * 3  
}  
  
class Y : X() {  
    // override fun double(number: Int = 2): Int = number * 2  // Error
    // override fun triple(number: Int = 1): Int = number * 3  // Error
}
```

If a default parameter precedes a parameter with no default value, the default value can only be used by calling the function with [named arguments](https://kotlinlang.org/docs/functions.html#named-arguments):
```kotlin
fun foo(
    bar: Int = 0,
    baz: Int,
) { /*...*/ }

foo(baz = 1) // The default value bar = 0 is used
```

### Named arguments
You can name one or more of a function's arguments when calling it.
You are also able to skip specific arguments with default values, rather than omitting them all. However, after the first skipped argument, you must name all subsequent arguments:
```kotlin
fun reformat(
    str: String,
    normalizeCase: Boolean = true,
    upperCaseFirstLetter: Boolean = true,
    divideByCamelHumps: Boolean = false,
    wordSeparator: Char = ' ',
) { /*...*/ }

reformat("This is a long String!")
reformat("This is a short String!", upperCaseFirstLetter = false, wordSeparator = '_')
```

### Variable number of arguments (varargs)
Only one parameter can be marked as `vararg`. 
If a `vararg` parameter is not the last one in the list, values for the subsequent parameters can be passed using *named argument syntax*, or, if the parameter has a function type, by passing a lambda outside the parentheses.

### Infix notation
Functions marked with the `infix` keyword can also be called using the infix notation (omitting the dot and the parentheses for the call).
Infix functions must meet the following requirements:
- They must be member functions or [extension functions](https://kotlinlang.org/docs/extensions.html).
- They must have a single parameter.
- The parameter must not [accept variable number of arguments](https://kotlinlang.org/docs/functions.html#variable-number-of-arguments-varargs) and must have no [default value](https://kotlinlang.org/docs/functions.html#default-arguments).

```kotlin
infix fun Int.isGreaterThan(other: Int): Boolean = (this > other)

fun main() {
	println(12 isGreaterThan 10)  
	println(12 isGreaterThan 12)  
	println(12 isGreaterThan 15)
}
```
> [!note]
> Infix function calls have lower precedence than arithmetic operators, type casts, and the `rangeTo` operator. 
> On the other hand, an infix function call's precedence is higher than that of the boolean operators `&&` and `||`, `is`- and `in`-checks, and some other operators.


## Function scope
Kotlin functions can be declared at the top level in a file. In addition to top level functions, Kotlin functions can also be declared locally as member functions and extension functions.
### Local functions
Kotlin supports local functions, which are functions inside other functions. A local function can access local variables of outer functions (the closure).
```kotlin
fun Counter() : () -> Int {  
    var count = 0;  
    val increment: () -> Int = { ++count }  
    return increment  
}
fun main() {
	val counter = Counter()  
	println(counter())  // 1
	println(counter())  // 2
	println(counter())  // 3
}
```

### Member functions
A member function is a function that is defined inside a class or object:
```kotlin
class Sample {
    fun foo() { print("Foo") }
}
```

## Generic functions
Functions can have generic parameters, which are specified using angle brackets before the function name:
```kotlin
fun <T> singletonList(item: T): List<T> { /*...*/ }
```


## Tail recursive functions
When a function is marked with the `tailrec` modifier and meets the required formal conditions, the compiler optimizes out the recursion, leaving behind a fast and efficient loop based version instead:
```kotlin
val eps = 1E-10 // "good enough", could be 10^-15

tailrec fun findFixPoint(x: Double = 1.0): Double =
    if (Math.abs(x - Math.cos(x)) < eps) x else findFixPoint(Math.cos(x))
```

To be eligible for the `tailrec` modifier, a function must call itself as the last operation it performs.