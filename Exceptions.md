## Exception Classes

All exception classes in Kotlin inherit the Throwable class. Every exception has a message, a stack trace, and an optional cause.

To throw an exception object, use the throw expression:
```kt
fun main() {
	//sampleStart
	throw Exception("Hi There!")
	//sampleEnd
}
```
To catch an exception, use the try...catch expression:
```kt
try {
	// some code
} catch (e: SomeException) {
	// handler
} finally {
	// optional finally block
}
```

There may be zero or more catch blocks, and the finally block may be omitted. However, at least one catch or finally block is required. finally block is always called regradless of whether try or catch is executed. Even if you return from inside try & catch, finally block will still be executed.

try is an expression, which means it can have a return value:
```kt
val a: Int? = try { input.toInt() } catch (e: NumberFormatException) { null }##
```
## Checked Exceptions
Kotlin does not have checked exceptions.
## The Nothing Type
throw is an expression in Kotlin, and has the type Nothing. This type has no values and is used to mark code locations that can never be reached. Nothing can be used to amrk a function that never returns
```kt
val s = person.name ?: throw IllegalArgumentException("Name required")
```

```kt
fun fail(message: String): Nothing {
	throw IllegalArgumentException(message)
}
```

The nullable variant of this type, Nothing?, has exactly one possible value, which is null. If you
use null to initialize a value of an inferred type and there's no other information that can be used to determine a more specific type, the compiler will infer the
Nothing? type: 
```kt
val x = null             // 'x' has type `Nothing?`
val l = listOf(null)     // 'l' has type `List<Nothing?>
```