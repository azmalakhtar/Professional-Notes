Kotlin provides the following types for unsigned integers:
#todo/add Unsigned Integer Table

Unsigned numbers are implemented as inline class with a single storage property that contains the corresponding signed counterpart type of the same width.

## Unsigned arrays and ranges
Same as for primitives, each unsigned type has a corresponding type that represents arrays of that type:
- `UByteArray`: an array of unsigned bytes.
- `UShortArray`: an array of unsigned shorts.
- `UIntArray`: an array of unsigned ints.
- `ULongArray`: an array of unsigned longs.

## Unsigned integers literals
Use `u` or `U` as a suffix to make a integer literal unsigned. The exact type is determined based on the expected type. If no expected type is provided, compiler will use `UInt` or `ULong` depending on the size of literal:

## Use cases
The main use case of unsigned numbers is utilizing the full bit range of an integer to represent positive values.