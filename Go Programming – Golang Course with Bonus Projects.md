#### Index
9. Maps 3:39 
10. Advanced Functions 4:06 
11. Pointers 4:31
12. Local development 4:48
13. Channels & Concurrency 5:31
14. Mutexes 6:07
15. Generics 6:30
16. Quiz 6:38
17. P1 RSS aggregator project 6:43
18. P2 Chi router 6:53
19. P3 Postgres database 7:11
20. p4 Authentication w/ API Keys 7:39
21. P5 Many to many relationships 8:18
22. P6 Aggregation Worker
23. P7 Viewing Blog Posts
## 5. Interfaces
### 11. Type Switches
You can use a type switch to switch based on the underlying type of the interface.
```kotlin
func printShapeType(sh shape) {
	switch t := sh.(type) {
	case rect:
		fmt.Printf("It is a rectangle having length %f and width %f.\n", t.length, t.width)
	case circle:
		fmt.Printf("It is a circle with radius %f.\n", t.radius)
	}
}
```

### 12. Clean Interfaces
1. Keep Interfaces as small as can to accurately represent an idea.
2. Interfaces should not have any knowledge of the satisfying type.

## 6. Errors
### 1. The Error Interface
An Error is any type that implements go's builtin `error` interface.
```kotlin
type error interface {
	Error() string
}
```

### 3. The Error Interface
You can create a custom error type by implementing the `error` interface.
```go
type divideError struct {
	dividend float64
}

func (de divideError) Error() string {
	return fmt.Sprintf(
		"can not divide %f by zero",
		de.dividend,
	)
}

```

### 6. The Errors Package
You can use `errors.New()` to create a new error.

## 8. Slices
### 1. Arrays in Go
Arrays have a fixed size.
```go
var myInts [5]int // declare
primes := [3]int{2, 3, 5} // declaring using an initialized literal
```

### 2. Slice
Slice are a particular view of array
```go
primes := [3]int{2, 3, 5} // declaring using an initialized literal
mySlice := primes[1: 2]
```

### 3. Slices Review
Slices wrap arrays to give a more general, convenient and powerful interface to sequences of data.
A slice references the underlying array. Thus changes make to the slice effectively modifies the array.
```go
func square(nums []int) {
	for i := 0; i < len(nums); i++ {
		nums[i] = nums[i] * nums[i]
	}
}

func main() {
	nums := [5]int{1, 2, 3, 4, 5}
	fmt.Println(nums) // [1 2 3 4 5]
	square(nums[1:4])
	fmt.Println(nums) // [1 4 9 16 5]
}

```

### 5. Representation of Slice
A slice is window over an array.
When we append an element to an slice, if the underlying array has space after the last element of the slice, it will write to that space overwriting any value if present, else it will create a new array of twice the length and return a reference to that.

```go
func square(nums []int) {
	nums = append(nums, 6)
	for i := 0; i < len(nums); i++ {
		nums[i] = nums[i] * nums[i]
	}
}

// Case 1
func main() {
	nums := [5]int{1, 2, 3, 4, 5}
	fmt.Println(nums) // [1 2 3 4 5]
	square(nums[1:4])
	fmt.Println(nums) // [1 4 9 16 36] // 5 got overwritten by 6
}
// Case 2
func main() {
	nums := [5]int{1, 2, 3, 4, 5}
	fmt.Println(nums) // [1 2 3 4 5]
	square(nums[1:])
	fmt.Println(nums) // [1 2 3 4 5] // a new array was created, and the slice operated over that, that's why this array is not modified.
}


```

### 6. Make
You can create slices directly in the following ways
```go
/*
the first argument is the type
the second argument is the length of the slice
the third argument is the capacity of the underlying array
*/
n1 := make([]int, 5, 10)
/*
if you don't provide a capacity argument, the capacity defaults to the length of the slice
*/
n2 := make([]int, 5)
/*
You can directly create a slice using slice literal
*/
n3 := []int{1, 2, 3, 4, 5}
```

### 9. `len` & `cap`
`nil` is the zero value of an array/slice
So calling `len` or `cap` on an `nil` returns `0`

### 10. Variadic
```go
func sum(nums ...int) int {
	// inside the function, nums is treated as a slice
	total := 0
	for i := 0; i < len(nums); i++ {
		total += nums[i]
	}
	return total
}

func main() {
	fmt.Println(sum(1, 2, 3, 4, 5)) // 15
	nums := []int{1, 2, 3, 4, 5} // 15
	fmt.Println(sum(nums...)) // ... is the spread operator
}

```

### 11. Append
Append function appends a number of elements in slice.
It take in the slice as an input and a variadic number of elements to append.
```go
func append(slice []Type, elems ...Type) []Type
```

### 12. Slice of Slices
```go
rows := [][]int{}
```

### 13. Tricky slices
Never append to a different slice than the one you are assigning the result of append to
> Always assign the result of the `append()` function back to the same slice.
```go
// don't do this
someSlice = append(otherSlice, element)
```

Because this code will result in modifying the otherSlice which can lead to unintended results. someSlice in some case can be holding a reference to otherSlice. Someone else might modify the otherSlice, which can overwrite our changes.

### 16. Range
GO provides syntactic sugar to iterate over a slice
```go
for INDEX, ELEMENT := range SLICE {
	// code
}
```

## 9. Maps
### 1. Maps
```go
// using make
	ages := make(map[string]int)
	ages["steve rogers"] = 90
	ages["tony stark"] = 40
	ages["thor"] = 120
// using map literal
	ages := map[string]int{
		"steve rogers": 90,
		"tony stark":   40,
		"thor":         120,
	}

```

### 2. Mutations
```go
ages[key] = value // Insert or Update
value := ages[key] // Get a value
delete(ages, key) // delete an value
value, ok := ages[key] // check if the key exists
```

### 3. Key Types
Any type that can be compared using the `==` operator is valid key. This means slices, maps & functions can't be keys of a map.

It is simpler to use struct as directly as a key than to nest maps. Because in using nested maps, to modify an inner key-value pair you have to check that outer keys exists which will result in a very verbose code.

## 10. Advanced Functions
### 1. First Class Functions
Functions can be passed as an input and return from a function.
A first class function is a function that can be treated like other values.
```go
func add(x, y int) int {
	return x + y
}

func mul(x, y int) int {
	return x * y
}

func aggregate(a, b, c int, arithmetic func(int, int) int) int {
	return arithmetic(arithmetic(a, b), c)
}
func main() {
	fmt.Println(aggregate(2, 3, 4, add)) //9
	fmt.Println(aggregate(2, 3, 4, mul)) //24
}

```

### 4. Currying
Takes function as an input and returns a function.
```go
func add(x, y int) int {
	return x + y
}

func mul(x, y int) int {
	return x * y
}

func selfMath(mathFunc func(int, int) int) func(int) int {
	return func(x int) int {
		return mathFunc(x, x)
	}
}

func main() {
	squareFunc := selfMath(mul)
	doubleFunc := selfMath(add)

	fmt.Println(squareFunc(5)) // 25
	fmt.Println(doubleFunc(5)) // 10
}

```

### 5. Defer
You execute a function just before the enclosing function returns by using the `defer` keyword.

### 6. Closures
Closures are function that can access the variables of outside scope.
```go
func counter() func(int) int {
	count := 0
	return func(x int) int {
		count += x
		return count
	}
}

func main() {
	count := counter()
	fmt.Println(count(2)) // 2
	fmt.Println(count(10)) // 12
}

```

### 9. Anonymous Functions
A function without a name.

## 11. Pointers
### 1. Introduction
```go
var p *int // default pointer value is nil
myStr := "hello"
myStrPtr := &myStr
```

#### Why Pointers?
1. When you want to modify the original variable inside a function instead of modifying a duplicate.
2. When you want to optimise performance.

### 5. Nil Pointers
Dereferencing a `nil` pointer will cause a runtime error. Thus before using a pointer make sure it is not null.

### 6. Pointer Receiver
A receiver type on a method can be a pointer.
```go
/* Non-Pointer Receiver */
type car struct {
	color string
}

func (c car) paint(newColor string) {
	c.color = newColor
}

func main() {
	c := car{color: "red"}
	c.paint("blue")
	fmt.Println(c.color)
	// prints "red"
}
/* Pointer Receiver */
type car struct {
	color string
}

func (c *car) paint(newColor string) {
	/* you don't have to dereference c, you can just use it like a varible */
	c.color = newColor
}

func main() {
	c := car{color: "red"}
	/* Calling pointer receiver function doesn't require you to explicitly convert into pointer */
	c.paint("blue")
	fmt.Println(c.color)
	// prints "blue"
}

```

## 12. Local Development
### 1. Packages
Every GO program is made up of packages.
A package named `main` has en entry point at the `main()` function. A `main` package is compiled into an executable program.

A package by any other name is a library package. Libraries have no entry point. Libraries simply export functionality that can be used by other packages.

### 2. Package Naming
#### Naming Convention
By convention a package's name is the same as the last element of its import path.
Package name can be different than import path, but this practice is discouraged.

#### One Package/Directory
A directory of go can have at most one package. All `.go` files is a single directory must all belong to the same package. If they don't an error will be thrown by the compiler.

### 5. Modules
A module is a collection of GO packages that are released together.
A repository contains one or more modules.

### Go Mod
Use `go mod init {repository}/{username}/{package_name}` to create a `go.mod` file in the directory.

- `go run {filename}` -> runs the file
- `go build` -> build the executable

### `go install` command
Compiles the program and installs the executable globally so that you can execute it from anywhere

### Requiring an package
To require an external package add it in the `go.mod` file
```go
module example.com/username/hellogo

go 1.21.5

/* 
- This is just so that go uses the library on the machine instead of trying to download it from the internet 
- Don't use this in production code.
*/
replace example.com/username/mystrings v0.0.0 => ../mystrings

/* this declares the dependencies */
require (
	example.com/username/mystrings v0.0.0
)

```

`go get {repository}/{username}/{module}` => downloads the package from the internet and adds it to the `go.mod` file as a dependency.

### 26. Clean Packages
#### 1. Don't export code from the main package
#### 2. Export only the required code from a library
- To reduce maintanenace burden
#### 3. Don't change the API
The unexported function within a package can and should change often for testing, refactoring, and bug fixes.
But the API should be stable.

## 13. Channels & Concurrency

### 1. Concurrency
You can spawn a new goroutine using the `go` keyword.
```go
go doSomething()
```

### 2. Channels
Channels are a typed, thread-safe queue. They allow different goroutines to communicate with each other.
```go
ch := make(chan int) // creating a channel
ch <- 7 // send data to a channel
v := <-ch // receive data from a channel
```
Sending & receiving data is a blocking operation. A send operation is blocked until there is another goroutine ready to receive and vice versa.

### 4. Buffered Channels
Channels can optionally be buffered.
```go
ch := make(chan int, 100)
```
Sending on a buffered channel only blocks when the buffer is full.
Receiving blocks only when the buffer is empty.

### 5. Closing Channels
```go
close(ch) // closing the channel
```
It is advisable that only the sender goroutine should close the channel.
```go
v, ok := <-ch
```
`ok` is false if the channel is empty and closed.
Sending on closed channel will cause a panic.

### 6. Range
Similar to slices and maps, channels can be ranged over
```go
for item := range ch {
	// item is the next value received from the channel
}
```
This for loop will exit when the channel is closed.

### 7. Select
A `select` statement is used to listen to multiple channels as the same time
```go
select {
	case i, ok := <-chInts:
		fmt.Println(i)
	case s, ok := <-chStrings:
		fmt.Println(s)
}
```

### 8. Select Default Case
The `default` case executes immediately if no other channels have a value ready. It stops `select` statement from blocking.

#### Tickers
```go
time.Tick() // returns a channel that sends a value on a given interval
time.After() // sends a value once after the duration has passed
time.Sleep() // blocks the current goroutine for a specified amount of time
```

#### Read-Only Channels
```go
func readCh(ch <-chan int) {
	// ch can only be read from in this function
}
```

#### Write-Only Channels
```go
func writeCh(ch chan<- int) {
	// ch can only be written to in this function
}
```

### 9. Channels Review
- A send to a nil channel blocks forever
- A receive from a nil channel blocks forever
- A send to a closed channel panics
- A receive from a closed channel return the zero value immediately

## 14. Mutexes
### 1. Mutexes in GO
```go
var mut sync.Mutex
mut.Lock() 
mut.Unlock()
```

### 5. RW Mutexes
It allows multiple reader to read at the same time.
```go
var mut sync.RWMutex
mut.RLock()
mut.RUnlock()
```

## 15. Generics
```go
func splitSlice[T any](s []T) ([]T, []T) {
	mid := len(s) / 2
	return s[:mid], s[mid:]
}
```

### 5. Constraints
Instead of passing `any` you can specify a subtype interface.

### 6. Parametric Constraints
#todo/read 

```go
type store[P product] interface {
	Sell(P)
}
```


---
### Reference
- [Go Programming – Golang Course with Bonus Projects](https://www.youtube.com/watch?v=un6ZyFkqFKo)