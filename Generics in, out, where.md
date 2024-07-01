## Variance
### Declaration-site variance
If a class only uses a type parameter T, as return types in its function. It can be annotated as `out`. Using this it allows that `C<Base>` can safely be a supertype of `C<Derived>`. This relation is known as covariance, type parameter and class both become specific together.
```kt
class Holder<out T>(private var value: T) {
	// fun update(t: T) {value = t} // Type parameter T is declared as 'out' but occurs in 'in' position in type Tkotlin(TYPE_VARIANCE_CONFLICT_ERROR
	fun get(): T = value
}

fun main() {
	var intholder = Holder<Int>(10)
	var numberholder = Holder<Number>(10)
	numberholder = intholder // Holder<Int> is a subtype of Holder<Number>
}
```

If a class only uses a type parameter T, as parameter types in its function. It can be annotated as `in`. Using this it allows that `C<Derived>` can safely be a supertype of `C<Base>`. This relation is known as contravarience, type parameter and class become specific in opposite direction.
```kt
interface Comparator<in T> {
	fun compare(first: T, second: T): Int
	// fun show(): T // Type parameter T is declared as 'in' but occurs in 'out' position in type Tkotlin(TYPE_VARIANCE_CONFLICT_ERROR)
}

open class Human(val age: Int)
open class Student(age: Int, val cgpi: Double) : Human(age)

class HumanComparator : Comparator<Human> {
	override fun compare(first: Human, second: Human): Int {
		if (first.age > second.age) return 1
		else if (first.age < second.age) return -1
		else return 0
	}
}
class StudentComparator : Comparator<Student> {
	override fun compare(first: Student, second: Student): Int {
		if (first.cgpi> second.cgpi) return 1
		else if (first.cgpi < second.cgpi) return -1
		else return 0
	}
}

fun main() {
	var humanComparator: Comparator<Human> = HumanComparator()
	var studentComparator: Comparator<Student> = StudentComparator()
	studentComparator = humanComparator; // Comparator<Student> is a supertype of Comparator<Human>
}
```

## Type projections
You can create a more