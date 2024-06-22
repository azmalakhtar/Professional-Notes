Kotlin has two classes for holding data `Pair` & `Triple`.

Destructring means getting values from a structure(class). Data classes, Pair & Triple class supports destructring.
```kt
data class Computation(
  val data: Int,
  val info: String
)

fun evaluate(input: Int) =
  if (input > 5)
    Computation(input * 2, "High")
  else
    Computation(input * 2, "Low")

fun main() {
  val (value, description) = evaluate(7) // Destructring
  value eq 14
  description eq "High"
}
```

The values are assigned in the order you define in the properties in the class.

withIndex() is a standard library extension function for List. It returns a collection of IndexedValues, which can be destructured:
```kt
fun main() {
  val list = listOf('a', 'b', 'c')
  for ((index, value) in list.withIndex()) {
    trace("$index:$value")
  }
  trace eq "0:a 1:b 2:c"
}
```