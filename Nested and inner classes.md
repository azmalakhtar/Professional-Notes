Classes can be nested in other classes:
```kt
class Outer {
	private val bar: Int = 1
	class Nested {
		fun foo() = 2
	}
}

val demo = Outer.Nested().foo() // == 2
```

You can also use interfaces with nesting. All combinations of classes and interfaces are possible
```kt
interface OuterInterface { 
	class InnerClass 
	interface InnerInterface 
} 

class OuterClass { 
	class InnerClass 
	interface InnerInterface 
}
```

## Inner classes
A nested class marked as `inner` can access the members of its outer class. Inner classes carry a reference to an object of an outer class:
```kt
class Outer {
	private val bar: Int = 1
	inner class Inner {
		fun foo() = bar
	}
}

val demo = Outer.Inner().foo() // == 2
```