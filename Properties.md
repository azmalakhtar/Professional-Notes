## Getters and setters
The full syntax for declaring a property is as follows:
```kt
var <propertyName>[: <PropertyType>] [= <property_initializer>]  
[<getter>]  
[<setter>]
```

Read-only properties `val` does not allow a setter.

You can define custom accessors (`get()` & `set()`) for a property. 

If you need to annotate an accessor or change its visibility, but you don't want to change the default implementation, you can define the accessor without defining its body:
```kt
var setterVisibility: String = "abc"  
    private set // the setter is private and has the default implementation  
  
var setterWithAnnotation: Any? = null  
    @Inject set // annotate the setter with Inject
```

### Backing fields
In Kotlin, a field is only used as a part of a property to hold its value in memory. They are automatically provided by Kotlin if the property uses default implementation of at least one accessors, or if a custom accessor references it through the `field` identifier.
```kt
var counter = 0 // the initializer assigns the backing field directly  
    set(value) {  
        if (value >= 0)  
            field = value  
        // counter = value // ERROR StackOverflow: Using actual name 'counter' would make setter recursive  
    }
val isEmpty: Boolean // this will not have any backing field
	get() = this.size == 0
```
Backing fields are not available for properties in the following cases:
- Properties declared in interfaces.
- Properties annotated with `inline` modifier.
- Extension properties

### Backing properties
If you want to do something that does not fit into this implicit backing field scheme, you can always fall back to having a backing property:
```kt
private var _table: Map<String, Int>? = null  // This is the backing property
public val table: Map<String, Int>  
    get() {  
        if (_table == null) {  
            _table = HashMap() // Type parameters are inferred  
        }  
        return _table ?: throw AssertionError("Set to null by another thread")  
    }
```


## Compile-time constants
If the value of a read-only property is known at compile time, mark it as a compile time constant using the `const` modifier. Such a property needs to fulfil the following requirements:
- It must be a top-level property, or a member of an [`object` declaration](https://kotlinlang.org/docs/object-declarations.html#object-declarations-overview) or a _[companion object](https://kotlinlang.org/docs/object-declarations.html#companion-objects)_.
- It must be initialized with a value of type `String` or a primitive type
- It cannot be a custom getter

## Late-initialized properties and variables
mark the property with the `lateinit` modifier

For a property to be lateinit, it must be:
- `var` property
- inside the body of the class (only when it doesn't have a custom accessor and is not in primary consructor), top-level properties or local variable
- type must be non-nullable and non-primitive

Accessing a `lateinit` property before it has been initialized throws a `UninitializedPropertyAccessException`.

To check whether a `lateinit var` has already been initialized, use `.isInitialized` on the [reference to that property](https://kotlinlang.org/docs/reflection.html#property-references):
```kt
if (foo::bar.isInitialized) {  
    println(foo.bar)  
}
```