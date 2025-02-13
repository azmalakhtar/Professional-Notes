# 4. What's Happening in the Background?
- How are our requests handled?
	- DispatcherServlet - Front Controller Pattern

- How does object get converted to JSON?
	- `JacksonHttpMessageConverters`

# 5. Path Parameter
```java
@GetMapping(path = "hello-world/{name}")
public HelloWorldBean sayHelloBeanPath(@PathVariable String name) {
	return new HelloWorldBean("Bean Hello, " + name);
}
```

# 6. Social Media App REST API
| Method | Function                       |
| ------ | ------------------------------ |
| GET    | Retrieve details of a resource |
| POST   | Create a new resource          |
| PUT    | Update an existing resource    |
| PATCH  | Update part of a resource      |
| DELETE | Delete a resource              |

# 9. POST
You can get the date of a request body by marking one of the parameters as `@RequestBody`.
```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
	return service.save(user);
}
```

# 10. Return correct Status Codes
| Status Code | When?              |
| ----------- | ------------------ |
| 404         | Resource not found |
| 500         | Server Exception   |
| 400         | Validation Error   |
| 200         | Success            |
| 201         | Created            |
| 204         | No content         |
| 401         | Unauthorized       |
| 400         | Bad Request        |
|             |                    |

To return a specific status code, you need to make the method's return type to `ResponseEntity<T>`. `ResponseEntity` provides a lot of methods that correspond to individual status codes. Use any of them.
```java
@PostMapping("/users")
public ResponseEntity<Object> createUser(@RequestBody User user) {
	User savedUser = service.save(user);
	URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}")
		.buildAndExpand(savedUser.getId())
		.toUri();
	return ResponseEntity.created(location).build();
}
```

# 11. Implementing Exception Handling
```java
@GetMapping("/users/{id}")
public User retrieveUser(@PathVariable int id) {
	User user = service.findOne(id);
	if (user == null) {
		throw new UserNotFoundException("id:" + id);
	}
	return user;
}
```

If an exception is thrown during the execution of your endpoint, then it is returned with a status code of `500`. To return a different status on a particular exception, add the `@ResponseStatus(code = <code-value>)` on the exception class definition.
```java
@ResponseStatus(code = HttpStatus.NOT_FOUND)
public class UserNotFoundException extends RuntimeException {
	public UserNotFoundException(String message) {
		super(message);
	}
}
```

# 12. Implementing Generic Exception Handling
You can create a generic exception handler, in which you can customize what should be the response on a particular error. Create a class and mark it with `@ControllerAdvice`. Inside it declare method of signature `ResponseEntity<T> name(Exception, WebRequest)` and annotate it with `@ExceptionHandler` passing in the class of the exception you want to handle.
```java
@ControllerAdvice
public class CustomizedResponseEntityExceptionHandler extends ResponseEntityExceptionHandler {

	@ExceptionHandler(exception = Exception.class)
	public final ResponseEntity<ErrorDetails> handleAllExceptions(Exception ex, WebRequest request) {
		ErrorDetails errorDetails = new ErrorDetails(
			LocalDateTime.now(), ex.getMessage(), request.getDescription(false));

		return new ResponseEntity<ErrorDetails>(errorDetails, HttpStatus.INTERNAL_SERVER_ERROR);
	}

	@ExceptionHandler(exception = UserNotFoundException.class)
	public final ResponseEntity<ErrorDetails> handleUserNotFoundException(Exception ex, WebRequest request) {
		ErrorDetails errorDetails = new ErrorDetails(
			LocalDateTime.now(), ex.getMessage(), request.getDescription(false));

		return new ResponseEntity<ErrorDetails>(errorDetails, HttpStatus.NOT_FOUND);
	}

}
```

#doubt if we have to specify the response code on each exception handler, then what's the point of adding `@ResponseStatus` on the exception class.

# 14. Implementing Validations for REST API
## Step 0. Add the dependency
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

## Step 1. Add Validations on the Type
```java
@Size(min = 2, message = "Name should be atleast 2 characters long.")
private String name;
@Past(message = "Birth Date should be in the past")
private LocalDate birthDate;
```

## Step 2. Make an endpoint valid
To validate the input add the `@Valid` annotation on it.
```java
@PostMapping("/users")
public ResponseEntity<Object> createUser(@Valid @RequestBody User user) {
	User savedUser = service.save(user);
	URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}")
		.buildAndExpand(savedUser.getId())
		.toUri();
	return ResponseEntity.created(location).build();
}
```

## Custom Exception Handling for Validation Fails
Override the `handleMethodArgumentNotValid()` method inside your custom exception handling class.
`getFieldError` -> returns a single error
`getFieldErrors` -> returns a list of all errors
```java
@Override
protected ResponseEntity<Object> handleMethodArgumentNotValid(
		MethodArgumentNotValidException ex,
		HttpHeaders headers,
		HttpStatusCode status,
		WebRequest request) {
	StringBuilder sb = new StringBuilder();
	ex.getFieldErrors().stream()
			.forEach(fieldError -> sb.append(fieldError.getDefaultMessage()));
	ErrorDetails errorDetails = new ErrorDetails(LocalDateTime.now(),
			sb.toString(), request.getDescription(false));
	return new ResponseEntity(errorDetails, HttpStatus.BAD_REQUEST);
}
```
 


---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)