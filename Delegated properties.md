Delegating properties allows you to use the same property implementation for as many times as you would like. It makes the code concise and clear.
```kt
class Example {
    var p: String by Delegate()
}
class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name}' in $thisRef.")
    }
}

fun main() {
	println(e.p) // Example@33a17727, thank you for delegating 'p' to me!
	e.p = "NEW" // NEW has been assigned to 'p' in Example@33a17727.
}
```
## Standard delegates
The Kotlin standard library provides factory methods for several useful kinds of delegates.
### Lazy properties
The first call to `get()` executes the lambda passed to `lazy()` and remembers the result. Subsequent call to `get()` simply returns the remembered result.
```kt
val lazyValue: String by lazy {
    println("computed!")
    "Hello"
}

fun main() {
    println(lazyValue)
    println(lazyValue)
    /* 
    computed // Only called once, at the time of first call to get()
    Hello
    Hello
    */
}
```
By default, the evaluation of lazy properties is **synchronized**: the value is computed only in one thread, but all threads will see the same value. You can change this behaviour.
### Observable properties
The handler is called every time you assign to the property (after the assignment has been performed).
```kt
class User {
    var name: String by Delegates.observable("<no name>") {
        prop, old, new ->
        println("$old -> $new")
    }
}
```
## Delegating to another property
A top level or class property can delegate its getter and setter to 
- A top-level property
- A member or an extension property of the same class
- A member or an extension property of the another class

To delegate a property to another property, use the `::` qualifier in the delagate name.
```kt
var topLevelInt: Int = 0
class ClassWithDelegate(val anotherClassInt: Int)

class MyClass(var memberInt: Int, val anotherClassInstance: ClassWithDelegate) {
    var delegatedToMember: Int by this::memberInt
    var delegatedToTopLevel: Int by ::topLevelInt

    val delegatedToAnotherClass: Int by anotherClassInstance::anotherClassInt
}
var MyClass.extDelegated: Int by ::topLevelInt
```

This may be useful, for example, when you want to rename a property in a backward-compatible way.
```kt
class MyClass {
   var newName: Int = 0
   @Deprecated("Use 'newName' instead", ReplaceWith("newName"))
   var oldName: Int by this::newName
}
fun main() {
   val myClass = MyClass()
   // Notification: 'oldName: Int' is deprecated.
   // Use 'newName' instead
   myClass.oldName = 42
   println(myClass.newName) // 42
}
```
## Storing properties in a map
You can use map instance as the delegate for a delegated property.
```kt
class User(val map: Map<String, Any?>) {
    val name: String by map
    val age: Int     by map
}
fun main() {
    val user = User(mapOf(
        "name" to "John Doe",
        "age"  to 25
    ))
    println(user.name) // Prints "John Doe"
    println(user.age)  // Prints 25
}
```
This also works for `var`'s properties if you use a `MutableMap`.

## Local delegated properties
You can declare local variables as delegated properties.

## Property delegate requirements
For a  `val`, a delegate should provide an operator function `getValue()` with the following parameters:
- `thisRef` must be the same type as, or a supertype of, the **property owner** (for extension properties, it should be the type being extended).
- `property` must be of type `KProperty<*>` or its supertype.
`getValue()` must return the same type as the property (or its subtype)

For a `var`, a delegate has to additionally provide an operator function `setValue()` with the following parameters:
- Rules for `thisRef` & `property` is same as for `val`
- `value` must be of the same type as the property (or its supertype)

```kt
class Resource  
  
class Owner {  
    var varResource: Resource by ResourceDelegate()  
}  
  
class ResourceDelegate(private var resource: Resource = Resource()) {  
    operator fun getValue(thisRef: Owner, property: KProperty<*>): Resource {  
        return resource  
    }  
    operator fun setValue(thisRef: Owner, property: KProperty<*>, value: Any?) {  
        if (value is Resource) {  
            resource = value  
        }  
    }  
}
```
`getValue()` and/or `setValue()` functions can be provided either as member functions of the delegate class or as extension functions.

Kotlin also provides `ReadOnlyProperty` and `ReadWriteProperty` (extends the `ReadOnlyProperty`) interfaces for creating property delegates. You can use these to create delegates as anonymous objects.
```kt
fun resourceDelegate(resource: Resource = Resource()): ReadWriteProperty<Any?, Resource> =  
    object : ReadWriteProperty<Any?, Resource> {  
        var curValue = resource  
        override fun getValue(thisRef: Any?, property: KProperty<*>): Resource = curValue  
        override fun setValue(thisRef: Any?, property: KProperty<*>, value: Resource) {  
            curValue = value  
        }  
    }  
  
val readOnlyResource: Resource by resourceDelegate()  // ReadWriteProperty as val  
var readWriteResource: Resource by resourceDelegate()
```
## Translation rules for delegated properties
Under the hood, the Kotlin compiler generates auxiliary properties for some kinds of delegated properties and then delegates to them.
For example:
```kt
// User written code
class C {  
    var prop: Type by MyDelegate()  
}  
  
// this code is generated by the compiler instead:  
class C {  
    private val prop$delegate = MyDelegate() // Auxiliary property 
    var prop: Type  
        get() = prop$delegate.getValue(this, this::prop)  
    set(value: Type) = prop$delegate.setValue(this, this::prop, value)  
}
```
### Optimized cases for delegated properties
The `$delegate` field will be omitted if a delegate is:
- A referenced property
- A named objects
- A final `val` property with a backing field and a default getter in the same module.
- A constant expression, enum entry, `this`, `null`.
### Translation rules when delegating to another property
When delegating to another property, the Kotlin compiler generates immediate access to the referenced property. This means that the compiler doesn' generate the field `prop$delegate`.
```kt
// Written Code
class C<Type> {  
    private var impl: Type = ...  
    var prop: Type by ::impl  
}  
// Compiler generated code
class C<Type> {  
    private var impl: Type = ...  
  
    var prop: Type  
        get() = impl  
        set(value) {  
            impl = value  
        }  
  
    fun getProp$delegate(): Type = impl // This method is needed only for reflection  
}
```

## Providing a delegate
By defining the `provideDelegate` operator, you can extend the logic for creating the object to which the property implementation is delegated. If the object used on the right-hand side of `by` defines `provideDelegate` as a member or extension function, that function will be called to create the property delegate instance.
```kt
class ResourceDelegate<T> : ReadOnlyProperty<MyUI, T> {  
    override fun getValue(thisRef: MyUI, property: KProperty<*>): T { ... }  
}  
  
class ResourceLoader<T>(id: ResourceID<T>) {  
    operator fun provideDelegate(  
        thisRef: MyUI,  
        prop: KProperty<*>  
    ): ReadOnlyProperty<MyUI, T> {  
        checkProperty(thisRef, prop.name)  
        // create delegate  
        return ResourceDelegate()  
    }  
  
    private fun checkProperty(thisRef: MyUI, name: String) { ... }  
}  
  
class MyUI {  
    fun <T> bindResource(id: ResourceID<T>): ResourceLoader<T> { ... }  
  
    val image by bindResource(ResourceID.image_id)  
    val text by bindResource(ResourceID.text_id)  
}
```
The parameters of `provideDelegate` are the same as those of `getValue`.

In the generated code, the `provideDelegate` method is called to initialize the auxiliary `prop$delegate` property
```kt
class C {  
    var prop: Type by MyDelegate()  
}  
  
// this code is generated by the compiler  
// when the 'provideDelegate' function is available:  
class C {  
    // calling "provideDelegate" to create the additional "delegate" property  
    private val prop$delegate = MyDelegate().provideDelegate(this, this::prop)  
    var prop: Type  
        get() = prop$delegate.getValue(this, this::prop)  
    set(value: Type) = prop$delegate.setValue(this, this::prop, value)  
}
```

Kotlin provides `PropertyDelegateProvider` interface for providing a delegate.