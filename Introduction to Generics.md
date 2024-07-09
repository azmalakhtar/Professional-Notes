> Generics create parameterized types: components that work across multiple types.

To define a generic type, add angle brackets(`<>`) containing one or more generic placeholders and put this generic specification after the class name. This placeholder now can be used as normal type in the class i.g. return types, parameter types etc.
```kotlin
class Holder<T>(private val data: T) {
    fun getData(): T = data
}
```
 It seems like that having an universal type- a type that is parent of all type types, might be a solution. But it doesn't work because we lose track of the class name & properties.

To define generic function, specify a generic type parameter in angle brackets before the function name.
```kt
fun <T, P> Pair<T, P>.swap(): Pair<P, T> = Pair(this.second, this.first)
```