Strings in Kotlin are represemted by the type `String`. It is a sequence of characters in double quotes (`"`).

> [!note]
> On the JVM, an object of String type in UTF-18 encoding uses approximately 2 bytes per character.

Strings are immutable.

To concatenate strings, use the `+` operator. This also works for concatenating strings with values of other types, as long as the first element in the expression is a string. Although in most cases string templates or multiline strings is preferred.

## String literals
Kotlin has two types of string literals
- Escaped strings
- Multiline strings

### Escaped strings
Escaped strings can contain escaped characters.

### Multiline strings
Multiline strings can contain newlines and arbitrary text. It is delimited by a triple quote ("""), contains no escaping.
```kt
val text = """
	for (c in "foo")
		print(c)
"""
```

Use `trimIndent()` or `trimMargin()` function to remove leading whitespaces.

## String templates
String literals may contain template expressions – pieces of code that are evaluated and whose results are concatenated into a string. You can use templates both in multiline and escaped strings.
```kt
val price = 10  
val receipt = """  
    Receipt        
    Price: ${price}  
""".trimIndent()  
println(receipt)
```

## String formatting
> [!Note]
> String formatting with the String.format() function is only available in Kotlin/JVM.

The `String.format()` function accepts a format string and one or more arguments. The format string contains one placeholder (indicated by %) for a given argument,
followed by format specifiers. Format specifiers are formatting instructions for the respective argument, consisting of flags, width, precision, and conversion type. You can also use the argument_index$ syntax to reference the same argument multiple times within the format string in different formats.
```kt
fun main() {  
//sampleStart  
// Formats an integer, adding leading zeroes to reach a length of seven characters  
    val integerNumber = String.format("%07d", 31416)  
    println(integerNumber)  
// 0031416  

// Formats a floating-point number to display with a + sign and four decimal places  
    val floatNumber = String.format("%+.4f", 3.141592)  
    println(floatNumber)  
// +3.1416  

// Formats two strings to uppercase, each taking one placeholder  
    val helloString = String.format("%S %S", "hello", "world")  
    println(helloString)  
// HELLO WORLD  

// Formats a negative number to be enclosed in parentheses, then repeats the same number in a different format (without parentheses) using `argument_index$`.  
    val negativeNumberInParentheses = String.format("%(d means %1\$d", -31416)  
    println(negativeNumberInParentheses)  
//(31416) means -31416  
//sampleEnd  
}
```