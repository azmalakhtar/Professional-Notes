A mechanism to simplify code and perform common tasks for classes that primarily holds data.

You define a data class using the `data` keyword before `class` keyword. Each constructor parameter must be preceded by `var` or `val`.

```kt
data class Simple(
  val arg1: String,
  var arg2: Int
)

fun main() {
  val s1 = Simple("Hi", 29)
  val s2 = Simple("Hi", 29)
  s1 eq "Simple(arg1=Hi, arg2=29)" // [1]
  s1 eq s2 // [2]
}
```


String representation of a data class is different from the normal class. It includes the parameter names and values of the data held by the object. (1)

If you create two instances of the same data class containing identical data (equal values for properties), you probably want those instances to be equal, TO achieve that behaviour for regualr class, you must define a special function `equals()` to compare instances. In data classes, this function is automatically generated; it compares the values of all properties specified as constructor parameters. (2)

`copy()` is a function that is generated for every data class, which creates a new object containing the data from current object, while also allowing you to change selected values.

```kt
data class DetailedContact(
  val name: String,
  val surname: String,
  val number: String,
  val address: String
)

fun main() {
  val contact = DetailedContact(
    "Miffy",
    "Miller",
    "1-234-567890",
    "1600 Amphitheatre Parkway")
  val newContact = contact.copy(
    number = "098-765-4321",
    address = "Brandschenkestrasse 110")
  newContact eq DetailedContact(
    "Miffy",
    "Miller",
    "098-765-4321",
    "Brandschenkestrasse 110")
}
```

Creating a data class also generates an appropriate *hash function* so that objects can be used as keys in `HashMaps` and `HashSets`.