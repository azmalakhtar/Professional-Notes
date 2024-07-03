## Integer types
|Type|Size (bits)|Min value|Max value|
|---|---|---|---|
|`Byte`|8|-128|127|
|`Short`|16|-32768|32767|
|`Int`|32|-2,147,483,648 (-2^31)|2,147,483,647 (2^31 - 1)|
|`Long`|64|-9,223,372,036,854,775,808 (-2^63)|9,223,372,036,854,775,807 (2^63 - 1)|

If you initialize a variable with no explicit type, `Int` is inferred if the value is in range of `Int`, else `Long` is inferred.
```kt
val one = 1 // Int 
val threeBillion = 3000000000 // Long 
val oneLong = 1L // Long 
val oneByte: Byte = 1
```

## Floating-point types
`Float` reflects the IEEE 754 single precision, while `Double` reflects double precision.

| Type     | Size (bits) | Significant bits | Exponent bits | Decimal digits |
| -------- | ----------- | ---------------- | ------------- | -------------- |
| `Float`  | 32          | 24               | 8             | 6-7            |
| `Double` | 64          | 53               | 11            | 15-16          |
You can initialize floating-point varibles with numbers having a fractional part. The default inferred type is `Double`, to make it a `Float`, add `f` or `F` as suffix to the fractional value.
```kt
val pi = 3.14 // Double 
// val one: Double = 1 // Error: type mismatch 
val oneDouble = 1.0 // Double

val e = 2.7182818284 // Double 
val eFloat = 2.7182818284f // Float, actual value is 2.7182817
```

## Literal constants for numbers
For integer values
- Decimals: `123`
- Longs are tagged by a capital `L`: `123L`
- Hexadecimals: `0x0F`
- Binaries: `0b00001011`

For floating-point numbers
- Doubles by default: `123.5`, `123.5e10`
- Floats are tagged by `f` or `F`: `123.5f`

You can use underscores to make number constants more readable:
```kt
val oneMillion = 1_000_000 
val creditCardNumber = 1234_5678_9012_3456L 
val socialSecurityNumber = 999_99_9999L 
val hexBytes = 0xFF_EC_DE_5E 
val bytes = 0b11010010_01101001_10010100_10010010
```

## Number representation on the JVM
On JVM platform, numbers are stored as primitive type: `int`, `double`, and so on. Exceptions are cases when you create a nullable number reference such as `Int?` or use generics. In these cases numbers are boxed in Java classes `Integer`, `Double`, and so on.
```kt
val a: Int = 1  
val boxedA: Int? = a
println(a::class.java)  // int
println(if (boxedA != null) boxedA::class.java else "NULL")  // class java.lang.Integer
```

Nullable references to the same number can refer to different objects:
```kt
val a: Int = 100  
val boxedA: Int? = a  
val anotherBoxedA: Int? = a  
  
val b: Int = 10000  
val boxedB: Int? = b  
val anotherBoxedB: Int? = b  
  
println(boxedA === anotherBoxedA) // true  
println(boxedB === anotherBoxedB) // false
```
All nullable references to `a` are actually the same object because of the memory optimization that JVM applies to `Integer`s between `-128` and `127`. It doesn't apply to the `b` references, so they are different objects.

## Explicit Number conversions
Due to different representation, smaller types are not subtypes of bigger ones. That's why we need to explicitly convert.
```kt
val b: Byte = 1 // OK, literals are checked statically 
// val i: Int = b // ERROR 
val i1: Int = b.toInt()
```

All number types support conversions to other types: `toByte()`, `toShort()`, and so on.

## Operations on numbers
Kotlin supports the standard set of arithmetical operations over numbers: `+`, `-`, `*`, `/`, `%`.

Kotlin provides a set of bitwise operators on integer numbers.

### Floating-point numbers comparison
When the operands `a` and `b` are statically known to be `Float` or `Double` or their nullable counterparts, the operations on the numbers and the range that they form follow the IEEE 754 Standard for Floating-Point Arithmetic.

However, the behaviour is different for operands that are not statically typed as floating-point numbers. In this case, the operations use the `equals` and `compareTo` implementations for `Float` and `Double`. As a result:
- `NaN` is considered equal to itself
- `NaN` is considered greater than any other element including `POSITIVE_INFINITY`
- `-0.0` is considered less than `0.0`

```kt
// Operand statically typed as floating-point number  
println(Double.NaN == Double.NaN)                 // false  
// Operand NOT statically typed as floating-point number  
// So NaN is equal to itself  
println(listOf(Double.NaN) == listOf(Double.NaN)) // true  
  
// Operand statically typed as floating-point number  
println(0.0 == -0.0)                              // true  
// Operand NOT statically typed as floating-point number  
// So -0.0 is less than 0.0  
println(listOf(0.0) == listOf(-0.0))              // false  
  
println(listOf(Double.NaN, Double.POSITIVE_INFINITY, 0.0, -0.0).sorted())  
// [-0.0, 0.0, Infinity, NaN]
```