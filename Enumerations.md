An enumerations is a collection of names.

An enumeration is a special kind of class with a fixed number of instances, all listed within the class body. You can define member properties and functions. If you include additional elements, you must add a semicolon after the last enumeration value:
```kt
enum class Direction(val notation: String) {
  North("N"), South("S"),
  East("E"), West("W");  // Semicolon required
  val opposite: Direction
    get() = when (this) {
      North -> South
      South -> North
      West -> East
      East -> West
    }
}
```

You must qualify each reference to an enumeration name, as with `Direction.North`. You can eliminate this qualification using an import to bring all names from the enumeration into the current namespace `import Direction.*`.

The first declared constant of an enum has an ordinal value of zero. Each subsequent constant receives the next integer value.