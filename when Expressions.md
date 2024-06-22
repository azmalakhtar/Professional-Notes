A when expression compares a value against a selection of possibilities. Each match (possibility) is an expression followed by a right arrow(->). The expression is evaluated and compared to the target value. If it matches, the expression to the right of arrow produces the result of the when expression.

If you use when as an expression, then it must be exhaustive. Else if you use it as statement it doesn't need to be exhaustive.

2. You can list several values in one branch using commas. Here, if the user enters either “up” or “u” we interpret it as a move up.
3. Multiple actions within a branch must be in a block body.
4. “Doing nothing” is expressed with an empty block.
5. Returning from the outer function is a valid action within a branch.
```kt
    when (input) {                   // [1]
      "up", "u" -> coordinates.y--   // [2]
      "down", "d" -> coordinates.y++
      "left", "l" -> coordinates.x--
      "right", "r" -> {              // [3]
        trace("Moving right")
        coordinates.x++
      }
      "nowhere" -> {}                // [4]
      "exit" -> return               // [5]
      else -> trace("bad input: $input")
    }
```
  




when has a special form that takes no argument. Omitting the argument means
the branches can check different Boolean conditions. You can use any Boolean
expression as a branch condition.