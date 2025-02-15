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

# 16. REST API Documentation
Your REST API consumer needs to understand your API, like resources, actions, request & response structure.

Challenges:
- **Accuracy**: How do you ensure that your documentation is upto date and correct (basically in-sync with code)?
- **Consistency**: You might have 100s of REST API in an enterprise. How do you ensure consistency.

There are two options:
- Manually Maintain Documentation
- Generate Documentation from Code

## Swagger & Open API
In *2011*, Swagger Specification and Swagger Tools were introduced. Then, in *2016*, Open API specification was created based on swagger specification.

Open API specification: Standard, language-agnostic interface
Swagger UI: Visualize and interact with your REST API.

### Setup
Add the following dependency:
```xml
<dependency>
	<groupId>org.springdoc</groupId>
	<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
	<version>2.8.4</version>
</dependency>

```

 Go to the `/swagger-ui.html` to see the swagger ui. From there you can also visit the open-api page.


# 18. Content Negotiation
For the same resource request on the same uri, we can return different respresentation. This is known as content negotiation.
`Accept` header represent the `MIME` types- like `application/json`, `application/xml` etc.
There is also a `Accept-Language` header for specifying the content language.

## In Spring Boot
If you want to return a xml if an xml type is asked for by the client in the request, then include the following dependecy
```xml
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

# 19. Internationalization
To return different messages based on the `Accept-Langauge` header.

## Create Different Messages File
Under the `resources` folder, create `messages.properties` file. To add another language, add the `messages_<language-code>.properties` file. Inside these files, define the messages.

```properties
# inside messages.properties file
good.morning.message=Good Morning
# inside messages_nl.properties file
good.morning.message=Goedemorgen
```

## Retrieve Message 
To get messages from the properties file, you need an instance of the `MessageSource` class. So, autowire the `MessageSource`.

To retrieve the message, call `getMessage` on the `MessageSource` object. The last argument to the `getMessage` call is the `Locale`. `LocaleContextHolder.getLocale()` returns the requested locale, if there is one or else returns the default locale.
```java
@GetMapping(path = "hello-world/in")
public String sayHelloInternational() {
	Locale locale = LocaleContextHolder.getLocale();
	return messageSource.getMessage("good.morning.message", null, "Default Message", locale);
}
```

# 20. Versioning REST API
Versioning comes in handy when you need to implement a breaking change. If you change the API without versioning, then all of the code that depends on your API will crash if the consumer doesn't change its code immediately. By providing versioning, you continue to support the old API specification so that your consumer can switch to the new version on his will.

You can version a REST API based on
- URI
- Request Parameter
- Header
- Media Type

Let's just say that we have a person API. It returns a person's full name as response.
```json
{
	"name": "Steve Rogers"
}
```
We want to improve our api by returning the name into two parts, first name and last name.
```json
{
	"name": {
		"firstName": "Steve",
		"lastName": "Rogers"
	}
}
```

## Ways of Versioning
### URI
You provide two enpoints, `v1/person` & `v2/person` etc.
```java
@GetMapping("v1/person")
public PersonV1 getPerson1() {
	return new PersonV1("Steve Rogers");
}

@GetMapping("v2/person")
public PersonV2 getPerson2() {
	return new PersonV2(new Name("Steve", "Rogers"));
}
```

Used by twitter.

### Request Parameter
`params` option of the `GetMapping` ensures that this endpoint is accesed only when the request parameter matches the specified params.
```java
@GetMapping(path = "/person", params = "version=1")
public PersonV1 getPerson1Param() {
	return new PersonV1("Steve Rogers");
}

@GetMapping(path = "/person", params = "version=2")
public PersonV2 getPerson2Param() {
	return new PersonV2(new Name("Steve", "Rogers"));
}
```
Used by Amazon.

### Headers
```java
@GetMapping(path = "/person/headers", headers = "X-API-VERSION=1")
public PersonV1 getPerson1Header() {
	return new PersonV1("Steve Rogers");
}

@GetMapping(path = "/person/headers", headers = "X-API-VERSION=2")
public PersonV2 getPerson2Header() {
	return new PersonV2(new Name("Steve", "Rogers"));
}
```
Used by Microsoft.

### Media Type Versioning 
By passing different `Accept` header to the request.

```java
@GetMapping(path = "/person/accept", produces = "application/vnd.company.app-v1+json")
public PersonV1 getPerson1AcceptHeader() {
	return new PersonV1("Steve Rogers");
}

@GetMapping(path = "/person/accept", produces =  "application/vnd.company.app-v2+json")
public PersonV2 getPerson2AcceptHeader() {
	return new PersonV2(new Name("Steve", "Rogers"));
}
```

Used by GitHub

## Which Versioning Method to Choose?
You should consider the following factors:
- URI Pollution
	- URI & Request Parameter versioning lead to URI pollution.
- Misuse of HTTP Headers
	- Headers were not meant for versioning, so Header and Media Type versioning misuses the Headers.
- Caching
	- By default, caching is done for the same URI. Thus if you are using Header or Media Type versioning, you will need to handle caching differently.
- Can we execute the request on the browser?
	- URI and Request Parameter versioned API can be executed on the browser easily, however for Header and Media Type versioned API you need a REST client.
- API Documentation
	- Generating API docs for URI and Request Parameter versioning is more convenient.

> [!note]
>Author's Recommendations 
> - Thinks about versioning even before you need it! 
> - One Enterprise - One Versioning Approach

# 22. Implementing HATEOAS for REST API
**Hypermedia As The Engine Of Application State**

Websites allow you to: See Data and Perform Actions

HATEOAS is about enhancing your REST API to tell consumers how to perform subsequent actions.

Implementation Options:
- Custom Format and Implementation
	- Difficult to maintain
- Use Standard Implementation
	- HAL (JSON Hypertext Application Language): Simple format that gives a consistent and easy way to hyperlink between resources in your API.
	- Spring HATEOAS: Generate HAL responses with hyperlinks to resources.

## Spring HATEOAS
Add the dependency
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```

To return a HAL response, use the `EntityModel<T>` class. To build links use `WebMvcLinkBuilder`.
```java
@GetMapping("/users/{id}")
public EntityModel<User> retrieveUser(@PathVariable int id) {
	User user = service.findOne(id);
	if (user == null) {
		throw new UserNotFoundException("id:" + id);
	}
	EntityModel<User> entityModel = EntityModel.of(user);
	WebMvcLinkBuilder link = WebMvcLinkBuilder
			.linkTo(WebMvcLinkBuilder.methodOn(this.getClass()).retrieveAllUsers());
	entityModel.add(link.withRel("all-users"));

	return entityModel;
}
```

# 23. Customizing REST API Responses
**Serialization**: Convert object to stream. Most popular JSON serialization in Java is **Jackson**.

## Customizing Field Names
If you want to customize the field name, add the `@JsonProperty` annotation on the field name.
```java
@JsonProperty("user-name")
@Size(min = 2, message = "Name should be atleast 2 characters long.")
private String name;
```

Now the response will be of this format. Notice that the returned field name is the one specified inside the `@JsonProperty` and not what was the actual name of the property.
```json
{
	"user-name": "Steve Rogers",
	"birthDate": "2023-01-01"
}
```

But customizing field name also has one interesting effect. If you want deserialize the json into the object, you will have to pass the key with the same name as the one specified in the `@JsonProperty`. If you instead pass the actual property name as the key, it will be ignored, and the property will be `null`.

## Static Filtering
Filtering is removing certain fields from the response.
When there is same filtering for a bean across different REST API, it is called static filtering. Implemented using `JsonIgnore` or `@JsonIgnorePropeties`.

The properties marked with `@JsonIgnore` will be ignored and thus not returned in the response.
```java
@JsonIgnore
private String field2;
```

## Dynamic Filtering
When we customize filtering for a bean for a specific REST API, it is called static filtering. 
```java
@GetMapping("filter-list")
public MappingJacksonValue staticFilteringList() {
	List<SomeBean> someBeans = Arrays.asList(
		new SomeBean("value1", "value2", "value3"),
		new SomeBean("value1", "value2", "value3")
	);
	MappingJacksonValue mappingJacksonValue = new MappingJacksonValue(someBeans);

	SimpleBeanPropertyFilter filter = SimpleBeanPropertyFilter.filterOutAllExcept("field2", "field3");
	FilterProvider filters = new SimpleFilterProvider().addFilter("PasswordFilter", filter);
	mappingJacksonValue.setFilters(filters);

	return mappingJacksonValue;
}
```

Annotate the bean with `@JsonFilter()` passing in the same name as the one passed in `addFilter`.
```java
@JsonFilter("PasswordFilter")
class SomeBean {
	// code...
}
```

# 26. Spring Boot HAL Explorer
You can use HAL explorer.
Dependecny
```xml
<dependency>
	<groupId>org.springframework.data</groupId>
	<artifactId>spring-data-rest-hal-explorer</artifactId>
</dependency>
```

# 30. Many-To-One Relationship in JPA
To create a many to one relationship.
Crete the `Entity` which will be "many", in this example a `Post`. Inside it create a field of the type "one", in this case `User` & annotate it with `@ManyToOne`.
```java
@Entity
public class Post {
	@Id
	@GeneratedValue
	private Integer id;
	private String description;
	@ManyToOne(fetch = FetchType.LAZY)
	@JsonIgnore
	private User user;
	// code...
}
```

Inside the one type, create a field of `List` of many type and annotate it with `@OneToMany(mappedBy = <field-name>`, passing in the field name which one has in the many type.
```java
@Entity(name = "user_details")
public class User {
	@Id
	@GeneratedValue
	private Integer id;
	@Size(min = 2, message = "Name should be atleast 2 characters long.")
	@NotBlank
	private String name;
	@Past(message = "Birth Date should be in the past")
	private LocalDate birthDate;

	@OneToMany(mappedBy = "user")
	@JsonIgnore
	private List<Post> posts;
}
```

The `Post` schema in JPA will have a `user_id` field of type `Integer`. This will contain the id of the `User` that this `Post` belongs to.

## Interacting with the data
```java
@GetMapping("/users/{id}/posts")
public List<Post> retrieveAllPostOfUser(@PathVariable Integer id) {
	User user = service.findOne(id);

	if (user == null) {
		throw new UserNotFoundException("id:" + id);
	}

	return user.getPosts();
}

@PostMapping("/users/{id}/posts")
public ResponseEntity<Object> createPostForUser(@PathVariable Integer id, @Valid @RequestBody Post post) {
	User user = service.findOne(id);
	if (user == null) {
		throw new UserNotFoundException("id:" + id);
	}

	post.setUser(user);
	Post savedPost = postRepository.save(post);

	URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}")
			.buildAndExpand(savedPost.getId())
			.toUri();
	return ResponseEntity.created(location).build();
}
```



---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)