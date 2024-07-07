In Kotlin, you can perform type checks to check the type of an **object** at **runtime**. Type casts enable you to convert objects to a different type.

## `is` and `!is` operators
To perform a runtime check that identifies whether an object conforms to a given type, use the `is` operator or its negated form `!is`:

```kt
if (obj is String) {
    print(obj.length)
}

if (obj !is String) { // Same as !(obj is String)
    print("Not a String")
} else {
    print(obj.length)
}
```

## Smart casts
In most cases, you don't need to use explicit cast operators because the compiler automatically casts objects for you. This is called smart-casting.
```kt
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x is automatically cast to String
    }
}
```

The compiler is even smart enough to know that a cast is safe if a negative check leads to a return:

```kt
if (x !is String) return

print(x.length) // x is automatically cast to String
```

### Control flow
Smart casts work not only for `if` conditional expressions but also for `when` expressions and  `while` loops:
```kt
when (x) {
    is Int -> print(x + 1)
    is String -> print(x.length + 1)
    is IntArray -> print(x.sum())
}
```

### Logical operators
The compiler can perform smart casts on the right-hand side of `&&` or `||` operators if there is a type check (regular or negative) on the left-hand side:

```kotlin
// x is automatically cast to String on the right-hand side of `||`
if (x !is String || x.length == 0) return

// x is automatically cast to String on the right-hand side of `&&`
if (x is String && x.length > 0) {
    print(x.length) // x is automatically cast to String
}
```

If you combine type checks for objects with an `or` operator (`||`), a smart cast is made to their closest common supertype:

```kotlin
interface Status {
    fun signal() {}
}

interface Ok : Status
interface Postponed : Status
interface Declined : Status

fun signalCheck(signalStatus: Any) {
    if (signalStatus is Postponed || signalStatus is Declined) {
        // signalStatus is smart-cast to a common supertype Status
        signalStatus.signal()
    }
}
```

### Smart cast prerequisites
> [!warning]
> Note that smart casts work only when the compiler can guarantee that the variable won't change between the check and its usage.

Smart casts can be used in the following conditions:

|                       |                                                                                                                                                                                                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `val` local variables | Always, except [local delegated properties](https://kotlinlang.org/docs/delegated-properties.html).                                                                                                                                                                               |
| `val` properties      | If the property is `private`, `internal`, or if the check is performed in the same [module](https://kotlinlang.org/docs/visibility-modifiers.html#modules) where the property is declared. Smart casts can't be used on `open` properties or properties that have custom getters. |
| `var` local variables | If the variable is not modified between the check and its usage, is not captured in a lambda that modifies it, and is not a local delegated property.                                                                                                                             |
| `var` properties      | Never, because the variable can be modified at any time by other code.                                                                                                                                                                                                            |


## "Unsafe" cast operator
To explicitly cast an object to a non-nullable type, use the _unsafe_ cast operator `as`:
```kotlin
val x: String = y as String
```
If the cast isn't possible, the compiler throws an exception. This is why it's called _unsafe_.

## "Safe" (nullable) cast operator
To avoid exceptions, use the _safe_ cast operator `as?`, which returns `null` on failure.
```kotlin
val x: String? = y as? String
```