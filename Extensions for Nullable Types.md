You can code extension function directly for the nullable types. To make the receiver nullable, add `?` to the type being extended.
```kt
fun String?.isNullOrEmpty() =
  s == null || s.isEmpty()

fun main() {
  null.isNullOrEmpty() eq true
  "".isNullOrEmpty() eq true
}
```

Take care when using extension functions for nullable types. They are great for simple cases like  `isNullOrEmpty()` and `isNullOrBlank()`, especially with self-explanatory names that imply the receiver might be null. In general, it's better to declare regular (non-nullable) extensions. Safe calls and explicit checks clarify the receiver's nullability, while extension for nullable types may conceal nullability and confuse the reader of your code (probably, "future you").
