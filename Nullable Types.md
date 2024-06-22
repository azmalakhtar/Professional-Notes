> Consider a function that sometimes produces “no result.” When this happens, the function doesn’t produce an error per se. Nothing went wrong, there’s just “no answer.”

If you treat null the same way you treat a meaningful value, you get a dramatic failure. That's why its creatore Tony Hoare, refer to it as "my billion dollar mistake".

Kotlin to avoid this "billion dollar mistake" implemented the following system: types default to non-nullable, however if something can be nullable, you must append a question mark to the type name to explicitly tag that results as nullable. The nullable type is not just a modification, but is actually different from the original one.

You can't simply dereference (call a memeber function or access a member property) a value of a nullable type. To dereference from a nullable type, you need to explicitly make sure that the value is not null. One way to do this is to check it using if statement.
```kt
fun main() {
  val s: String? = "abc"
  // s.length - doesn't compile
  if (s != null)
    s.length eq 3
}
```

You can't assign an identifier of a nullable type to an identifier of a non-null type. To do this, you need to first explicitly make sure that nullable type is not null.