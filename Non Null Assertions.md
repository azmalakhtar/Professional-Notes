Non null assertions `!!` means that you are guranteeing that the receiver is not null, if it is a null, then an exception will be throwed (`NullPointerException`).

```kt
fun main() {
  var x: String? = "abc"
  x!! eq "abc"
  x = null
  capture {
    x!!
  } eq "NullPointerException"
}
```

Non null asserted calls consists of two operators: the non-null assertion (`!!`) and a dereference (`.`). Avoid non-null assertions and prefer safe calls or explicit checks.

If you frequently use non-null assertions in your code for the same operation,
it’s better to use a separate function with a specific assertion describing the
problem.  As an example, suppose your program logic requires a particular key to be present in a Map, and you prefer getting an exception instead of silently doing nothing if the key is absent. Instead of extracting the value with the usual approach (square brackets), `getValue()` throws `NoSuchElementException` if a key is missing.
```kt
fun main() {
  val map = mapOf(1 to "one")
  map[1]!!.toUpperCase() eq "ONE"
  map.getValue(1).toUpperCase() eq "ONE"
  capture {
    map[2]!!.toUpperCase()
  } eq "NullPointerException"
  capture {
    map.getValue(2).toUpperCase()
  } eq "NoSuchElementException: " +
    "Key 2 is missing in the map."
}
```

Optimal code uses only safe calls and special functions that throw detailed exceptions. Only use non-null asserted calls when you absolutely must.