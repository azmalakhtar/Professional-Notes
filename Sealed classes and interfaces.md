Sealed classes and interfaces provide controlled inheritance of your class hierarchies. All *direct* subclasses of a sealed class are known at compile time. No other subclasses may appear outside the module and the package within which the sealed class is defined. The same is true for sealed interfaces.

Sealed classes are best used for scenarios when:
- **Limited class inheritance is desired:** You have a predefined, finite set of subclasses that extend a class, all of which are known at compile time.
    
- **Type-safe design is required:** Safety and pattern matching are crucial in your project. Particularly for state management or handling complex conditional logic. For an example, check out [Use sealed classes with when expressions](https://kotlinlang.org/docs/sealed-classes.html#use-sealed-classes-with-when-expression).
    
- **Working with closed APIs:** You want robust and maintainable public APIs for libraries that ensure that third-party clients use the APIs as intended.

## Declare a sealed class or interface
To declare a sealed class or interface, use the `sealed` modifier:
```kt
// Create a sealed interface  
sealed interface Error  
  
// Create a sealed class that implements sealed interface Error  
sealed class IOError(): Error  
  
// Define subclasses that extend sealed class 'IOError'  
class FileReadError(val file: File): IOError()  
class DatabaseError(val source: DataSource): IOError()  
  
// Create a singleton object implementing the 'Error' sealed interface  
object RuntimeError : Error
```

### Constructors
A sealed class itself is always an abstract class, and as a result, can't be instantiated directly. However it may contain or inherit constructors. These constructors aren't for creating instances of the sealed class itself but for its subclasses.
```kt
sealed class Error(val message: String) {  
    class NetworkError : Error("Network failure")  
    class DatabaseError : Error("Database cannot be reached")  
    class UnknownError : Error("An unknown error has occurred")  
}
```

Constructors of sealed classes can have one of two visibilities: `protected` (by default) or `private`:

## Inheritance
Direct subclasses of sealed classes and interfaces must be declared in the same package. Subclasses of sealed classes must have a properly qualified name. They can't be local or anonymous objects.

These restrictions don't apply to indirect subclasses. If a direct subclass of a sealed class is not marked as sealed, it can be extended in any way that its modifiers allow:
```kt
// Sealed interface 'Error' has implementations only in the same package and module
sealed interface Error

// Sealed class 'IOError' extends 'Error' and is extendable only within the same package
sealed class IOError(): Error

// Open class 'CustomError' extends 'Error' and can be extended anywhere it's visible
open class CustomError(): Error
```

## Use sealed classes with when expressions
Kotlin compiler can at compile time check exhaustively that all possibilites are covered and thus you don't need to use an `else` clause:
```kt
// Function to log errors
fun log(e: Error) = when(e) {
    is Error.FileReadError -> println("Error while reading file ${e.file}")
    is Error.DatabaseError -> println("Error while reading from database ${e.source}")
    Error.RuntimeError -> println("Runtime error")
    // No `else` clause is required because all the cases are covered
}
```

## Use case scenarious
Let's explore some practical scenarios where sealed classes and interfaces can be particularly useful. For explanation refer to the [documentation](https://kotlinlang.org/docs/sealed-classes.html#use-case-scenarios)

### State mangement in UI applications
```kt
sealed class UIState {
    data object Loading : UIState()
    data class Success(val data: String) : UIState()
    data class Error(val exception: Exception) : UIState()
}

fun updateUI(state: UIState) {
    when (state) {
        is UIState.Loading -> showLoadingIndicator()
        is UIState.Success -> showData(state.data)
        is UIState.Error -> showError(state.exception)
    }
}
```

### Payment method handling
```kt
sealed class Payment {  
    data class CreditCard(val number: String, val expiryDate: String) : Payment()  
    data class PayPal(val email: String) : Payment()  
    data object Cash : Payment()  
}  
  
fun processPayment(payment: Payment) {  
    when (payment) {  
        is Payment.CreditCard -> processCreditCardPayment(payment.number, payment.expiryDate)  
        is Payment.PayPal -> processPayPalPayment(payment.email)  
        is Payment.Cash -> processCashPayment()  
    }  
}
```

### API request-response handling
```kt
// Import necessary modules
import io.ktor.server.application.*
import io.ktor.server.resources.*

import kotlinx.serialization.*

// Define the sealed interface for API requests using Ktor resources
@Resource("api")
sealed interface ApiRequest

@Serializable
@Resource("login")
data class LoginRequest(val username: String, val password: String) : ApiRequest


@Serializable
@Resource("logout")
object LogoutRequest : ApiRequest

// Define the ApiResponse sealed class with detailed response types
sealed class ApiResponse {
    data class UserSuccess(val user: UserData) : ApiResponse()
    data object UserNotFound : ApiResponse()
    data class Error(val message: String) : ApiResponse()
}

// User data class to be used in the success response
data class UserData(val userId: String, val name: String, val email: String)

// Function to validate user credentials (for demonstration purposes)
fun isValidUser(username: String, password: String): Boolean {
    // Some validation logic (this is just a placeholder)
    return username == "validUser" && password == "validPass"
}

// Function to handle API requests with detailed responses
fun handleRequest(request: ApiRequest): ApiResponse {
    return when (request) {
        is LoginRequest -> {
            if (isValidUser(request.username, request.password)) {
                ApiResponse.UserSuccess(UserData("userId", "userName", "userEmail"))
            } else {
                ApiResponse.Error("Invalid username or password")
            }
        }
        is LogoutRequest -> {
            // Assuming logout operation always succeeds for this example
            ApiResponse.UserSuccess(UserData("userId", "userName", "userEmail")) // For demonstration
        }
    }
}

// Function to simulate a getUserById call
fun getUserById(userId: String): ApiResponse {
    return if (userId == "validUserId") {
        ApiResponse.UserSuccess(UserData("validUserId", "John Doe", "john@example.com"))
    } else {
        ApiResponse.UserNotFound
    }
    // Error handling would also result in an Error response.
}

// Main function to demonstrate the usage
fun main() {
    val loginResponse = handleRequest(LoginRequest("user", "pass"))
    println(loginResponse)

    val logoutResponse = handleRequest(LogoutRequest)
    println(logoutResponse)

    val userResponse = getUserById("validUserId")
    println(userResponse)

    val userNotFoundResponse = getUserById("invalidId")
    println(userNotFoundResponse)
}
```