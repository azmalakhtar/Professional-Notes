Boolean type are represented by the type `Boolean`.

On JVM, booleans stored as the primitive boolean type typically use 8 bits. Nullable references to boolean objects are boxed in Java classes.

The `||` (disjunction) and `&&` (conjunction) operators work lazily. They only evaluate the second operand if needed.