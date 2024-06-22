Kotlin allows you to use same name for for different functions as long as the parameter lists differ.

If a class already has a member function with the same signature as an extension function, Kotlin prefers the member function. However you can overload the member function with an extension function.

With overloading you only use what the function does (add) as the name. If you want to know how, then look at parameter list (whether it adds them as int or double). This reduces redundancy as you don't have to use the same information again.

```kt
fun addInt(i: Int, j: Int) = i + j
fun addDouble(i: Double, j: Double) = i + j

fun add(i: Int, j: Int) = i + j
fun add(i: Double, j: Double) = i + j
```