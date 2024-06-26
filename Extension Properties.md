> Just as functions can be extension functions, properties can be extension properties.

The extended type comes right before the property name just like in the case of functions. An extension property requires a custom getter.
```kt
val ReceiverType.extensionProperty: PropType
  get() { ... }
```

When the generic argument type isn't used, you may replace it with `*`. This is called a star projection. When you use `*`, you lose all specific information about the generic type. An element of type `*` can only be assigned to `Any?`. 