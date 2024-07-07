An array is a data structure that holds a fixed number of values of the same type or its subtypes.

## When to use arrays
Use arrays in Kotlin when you have specialized low-level requirements that you need to meet, for example performance requirements beyond what is needed in general. If you don't have these sorts of restrictions, use collections instead.

Collections have the following benefits compared to arrays:
- Collections can be read-only, which gives you more control and allows you to write robust code that has a clear intent.
- It is easy to add or remove elements from collections. In comparison, arrays are fixed in sized. The only way to add or remove elements is to create a new array each time, which is very inefficient:
	```kt
	var riversArray = arrayOf("Nile", "Amazon", "Yangtze")  
  
	// Using the += assignment operation creates a new riversArray,  
	// copies over the original elements and adds "Mississippi"  
	println(riversArray) // [Ljava.lang.String;@28a418fc  
	riversArray += "Mississippi"  
	println(riversArray) // [Ljava.lang.String;@1be6f5c3  
	println(riversArray.joinToString())  
	// Nile, Amazon, Yangtze, Mississippi
	```
- You can use the equality operator (`==`) to check if collections are structurally equal. In case of array, this operator doesn't check structural equality.

## Create arrays
To create arrays in Kotlin, you can use:
- functions, such as `arrayOf()`, `arrayOfNulls()` or `emptyArray()`.
- the `Array` constructor. The `Array` constructor takes the array size and a function that returns values for array elements given its index:
	```kt
	// Creates an Array<Int> that initializes with zeros [0, 0, 0]
	val initArray = Array<Int>(3) { 0 }
	println(initArray.joinToString())
	// 0, 0, 0
	
	// Creates an Array<String> with values ["0", "1", "4", "9", "16"]
	val asc = Array(5) { i -> (i * i).toString() }
	asc.forEach { print(it) }
	// 014916
	```

### Nested arrays
Arrays can be nested within each other to create multidimensional arrays. Nested arrays don't have to be the same type or the same size.

## Access and modify elements
Arrays in Kotlin are *invariant*. This means that Kotlin doesn't allow you to assign an `Array<String>` to an `Array<Any>` to prevent a possible runtime failure.

## Work with arrays
### Pass variable number of arguments to a function
You can do that by `vararg` parameter. To pass an array to an `vararg` parameter, use **spread** operator (`*`).
```kt
fun main() {
    val lettersArray = arrayOf("c", "d")
    printAllStrings("a", "b", *lettersArray)
    // abcd
}

fun printAllStrings(vararg strings: String) {
    for (string in strings) {
        print(string)
    }
}
```

### Compare arrays
To compare whether two arrays have the same elements in the same order, use the `contentEquals()` and `contentDeepEquals()` functions.

### Transform arrays
Kotlin has many useful function to transform arrays like:
- `.sum()` : to return the sum of all elements.
- `.shuffle()` : to randomly shuffle the elements in an array.
- and many more

### Convert arrays to collections
#### Convert to List or Set
Use the `.toList()` and `.toSet()` functions.

#### Convert to Map
Use the `.toMap()` function. Only an array of `Pair<K,V>` can be converted to a `Map`. The first value of a `Pair` instance becomes a key, and the second becomes a value.
```kt
val pairArray = arrayOf("apple" to 120, "banana" to 150, "cherry" to 90, "apple" to 140)

// Converts to a Map
// The keys are fruits and the values are their number of calories
// Note how keys must be unique, so the latest value of "apple"
// overwrites the first
println(pairArray.toMap())
// {apple=140, banana=150, cherry=90}
```

## Primitive-type arrays
If you use the `Array` class with primitive values, these values are boxed into objects, impacting performance. As an alternative, you can use primitive-type arrays, like `BooleanArray`, `ByteArray`, `FloatArray` and so on.

These classes have no inheritance relation to the `Array` class, but they have the same set of functions and properties.

To convert primitive-type arrays to object-type arrays, use the [`.toTypedArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-typed-array.html) function.
To convert object-type arrays to primitive-type arrays, use [`.toBooleanArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-boolean-array.html), [`.toByteArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-byte-array.html), [`.toCharArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-char-array.html), and so on.