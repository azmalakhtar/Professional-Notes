Extension functions allows you to effectively add member functions to existing classes. The type you extend is called the receiver. To define an extension function you precede the function name with receiver type

```kt
fun String.singleQuote() = "'$this'"
fun String.doubleQuote() = "\"$this\""
```

Extension functions can only access public elements of the type being extended.