> `break` and `continue` allow you to jump within a loop.

`continue` can only jump to the beginning of the loop, and `break` can only jump to the end of a loop.

Plain `break` and `continue` can jump no further than the boundaries of their local loop. *Labels* allow `break` and `continue` to jump to the boundaries of enclosing loops, so you aren't limited to the scope of the current loop.
You create a label by using `label@`, where `label` can be any name. In this example the label is outer.
```kt
fun main() {
  val strings = mutableListOf<String>()
  outer@ for (c in 'a'..'e') {
    for (i in 1..9) {
      if (i == 5) continue@outer
      if ("$c$i" == "c3") break@outer
      strings.add("$c$i")
    }
  }
  strings eq listOf("a1", "a2", "a3", "a4",
    "b1", "b2", "b3", "b4", "c1", "c2")
}
```

Both break and continue tend to create complicated and un-maintainable code, because jumps interrupt program flow. Code without jumps is almost easier to understand.
In some cases, you can write the conditions for iteration explicitly instead of using break and continue. In other cases, you can restructure your code and introduce new functions.