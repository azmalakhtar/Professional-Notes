Type aliases provide alternative names for existing types. 
```kt
// Generic Types
typealias NodeSet = Set<Network.Node> 
typealias FileTable<K> = MutableMap<K, MutableList<File>>

// Function Types
typealias MyHandler = (Int, String, Any) -> Unit 
typealias Predicate<T> = (T) -> Boolean

// Inner and Nested Classes
class A {  
    inner class Inner  
}  
class B {  
    inner class Inner  
}  
  
typealias AInner = A.Inner  
typealias BInner = B.Inner
```

Type aliases do not introduce new types. They are equivalent to the underlying types. Kotlin compiler always expands the type alias to its underlying type. 