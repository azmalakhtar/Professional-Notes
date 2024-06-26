Data classes in Kotlin are primarily used to hold data. For each data class, the compiler automatically generates additional member functions that provides useful functionalities.
```kt
data class User(val name: String, val age: Int)
```

The compiler automatically derives the following members from all properties declared in the primary constructor:
- `equals()` / `hashCode()` pair
- `toString()` of the form "User(name=John, age=42)"
- `componentN()` functions corresponding to the properties in their order of declaration
- `copy()` function

Data classes have to fulfill the following requirements:
- The primary constructor must have at least one parameter.
- All primary constructor parameters must be marked as `val` or `var`.
- Data classes can't be abstract, open, sealed, or inner.

The generation of data classes members follow these rules with regard to the member's inheritance:
- If there are explicit implementation of `equals()`, `hashCode()`, or `toString()` in the data class body or `final` implementation in a superclass, then these functions are not generated, and the existing implementations are used.
- If a supertype has `componentN()` functions that are `open` and return compatible types, the corresponding functions are generated for the data class and override those of the supertype. If the functions of the supertype cannot be overriden due to incompatible signature or due to their being final, an error is reported.
- Providing explicit implementations for the `componentN()` and `copy()` functions is not allowed.

## Properties declared in the class body
The compiler only uses the properties defined inside the primary constructor for the automatically generated functions. The properties declared inside the class body are not considered.

## Data classes and destructring declarations
Component functions generated for data classes make it possible to use them in destructring declarations:
```kt
val jane = User("Jane", 35) 
val (name, age) = jane 
println("$name, $age years of age") // Jane, 35 years of age
```

## Standard data classes
The standard library provides the `Pair` and `Triple` classes. In most cases though named data classes are better design choice because they make code easier to understand.
