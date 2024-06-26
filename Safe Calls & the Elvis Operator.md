Safe calls `?.` access members of nullable type only if it is not null, if the receiver is null it does not perform the access, but produces null for the expression.

Elvis operator `?:`. If the expression on the left of `?:` is not null, that expression becomes the result. If the left-hand expression is null, then the expression on the right of the `?:` becomes the result.