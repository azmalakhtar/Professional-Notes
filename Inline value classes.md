Inline class allows you to wrap a value in a class to create more domain specific type.
```kt
value class Password(private val s: String)
```
To declare an inline class for the JVM backend, use the `value` modifier along with the `@JvmInline` annotation before the class declaration:
```kt
// For JVM backends 
@JvmInline 
value class Password(private val s: String)
```

An inline class must have a single property initialized in the primary constructor. At runtime, instances of the inline class will be represented using this single property. _inline_: data of the class is _inlined_ into its usages (similar to how content of [inline functions](https://kotlinlang.org/docs/inline-functions.html) is inlined to call sites).

## Members
They are allowed to declare properties and functions, have an `init` block and secondary constructors. But they can't have backing fields. They can only have simple computable properties (no `lateinit`/delegated properties).

## Inheritance
Inline classes can inherit from interfaces. But they can't extend other classes and are always `final`.

## Representation
#notunderstood 

## Inline classes vs type aliases
Type aliases are assignment-compatible with their underlying type (and with other type aliases with the same underlying type), while inline classes are not because they introduce a truly new type, contrary to type aliases which only introduce an alternative name (alias) for existing type:
```kt
typealias NameString = String  
  
@JvmInline  
value class ValueString(private val s: String)  
  
fun acceptString(s: String) = println(s)  
fun acceptAsName(s: NameString) = println(s)  
fun acceptAsValue(s: ValueString) = println(s)  
  
fun main() {  
    val nameString: NameString = ""  
    val valueString: ValueString = ValueString("")  
    val string: String = ""  
  
    // Can they be passed as their underlying type?  
    acceptString(nameString)  
    // acceptString(valueString) // Kotlin: Type mismatch: inferred type is ValueString but String was expected  
  
    // Can their underlying type be used in place of them?    
    acceptAsName(string)  
    // acceptAsValue(string) // Kotlin: Type mismatch: inferred type is String but ValueString was expected  
}
```

## Inline classes and delegation
#notunderstood 