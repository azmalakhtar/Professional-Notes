Classes, objects, interfaces, constructors, and functions, as well as properties and their setters, can have visibility modifiers. Getters always have the same visibility as their properties.
There are four visibility modifiers in Kotlin: `private`, `protected`, `internal`, and `public`. The default visibility is `public`.

## Packages
Functions, properties, classes, objects, and interfaces can be declared at the "top-level" directly inside a package:
- If you don't use a visibility modifier, `public` is used by default, which means that your declarations will be visible everywhere.
    
- If you mark a declaration as `private`, it will only be visible inside the file that contains the declaration.
    
- If you mark it as `internal`, it will be visible everywhere in the same [module](https://kotlinlang.org/docs/visibility-modifiers.html#modules).
    
- The `protected` modifier is not available for top-level declarations.
```kt
// file name: example.kt 
package foo 

private fun foo() { ... } // visible inside example.kt 

public var bar: Int = 5 // property is visible everywhere 
private set // setter is visible only in example.kt 

internal val baz = 6 // visible inside the same module
```

## Class members
For members declared inside a class:

- `private` means that the member is visible inside this class only (including all its members).
    
- `protected` means that the member has the same visibility as one marked as `private`, but that it is also visible in subclasses.
    
- `internal` means that any client _inside this module_ who sees the declaring class sees its `internal` members.
    
- `public` means that any client who sees the declaring class sees its `public` members.

## Modules
A module is a set of Kotlin files compiled together