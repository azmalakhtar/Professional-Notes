# 3. First Spring MVC Controller
If you add a `@Controller` aannotation on a controller, then the method inside it will not return string directly. Instead they will look for a file named as that string and return it. If they don't find a file, the endpoint will return `404`.
To instead return the string as it is, add the `@ResponseBody` annotation on the endpoint.
```java
@Controller
public class SayHelloController {
	@RequestMapping("say-hello")
	@ResponseBody
	public String hello() {
		return """
		<h1 style="color:blue;">Hello! What are you learning today?</h1>
		""";
	}
}
```

# 4. 
You can use `StringBuffer` to build strings.
```java
		StringBuffer buffer = new StringBuffer();

		buffer.append("<html>");
		buffer.append("<head>");
		buffer.append("<title>First Web Page</title>");
		buffer.append("</head>");
		buffer.append("<body>");
		buffer.append("<h1>Hello! What are you learning?</h1>");
		buffer.append("</body>");
		buffer.append("</html>");

		return buffer.toString();
```

# 5. Redirect to a JSP using Spring Boot
## Step 1. Add the dependency
```xml
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>
```

## Step 2. Create a JSP File
JSP files should be created under the `src/main/resources/META-INF/resources/WEB-INF` directory.

## Step 3. Configure the prefix and suffix
In `application.properties`, define the prefix and suffix for the jsp request. The request will become as `<prefix><filename><suffix>`. In our case, the jsp file is under the `/src/main/resources/META-INF/resources/WEB-INF/jsp` directory.
You can omit the `/src/main/resources/META-INF/resources`, as spring already adds it.
```properties
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
```

## Step 4. Create the endpoint
Return the jsp file name from the endpoint.
```java
	@RequestMapping("say-hello-jsp")
	public String sayHelloJsp() {
		return "sayHello";
	}
```

# 8. QueryParams and Model
To capture query parameters, add a parameter with the same name as query parameter and add the `@RequestParam`.
```java
	// login?name=something
	@RequestMapping("login")
	public String goToLoginPage(@RequestParam String name) {
		System.out.println("REQ PARAM: " + name);
		return "login";
	}
```

To dynamically pass values to a jsp, use the `ModelMap`. Add a parameter of `ModelMap` on the endpoint method and then put the values inside this map.
```java
	@RequestMapping("login")
	public String goToLoginPage(@RequestParam String name, ModelMap map) {
		map.put("name", name.toUpperCase());
		return "login";
	}
```

To access the provided value in a `jsp` file, use the `${<key>}`.
```html
<body>
    Welcome ${name}!
</body>
```

# 9. Logging
You can specify what level of logs to print in the `application.properties` file. To declare logging level for a package, add the `logging.level.<package-name>=<log-level>`.
```properties
logging.level.org.springframework=info
logging.level.com.azmalakhtar.springboot.myfirstwebapp.login=debug
```

To log something inside your custom package, use the `Logger`.
```java
@Controller
public class LoginController {
	private Logger logger = LoggerFactory.getLogger(getClass());

	@RequestMapping("char")
	public String getCharacter(@RequestParam String name, @RequestParam String weapon, @RequestParam String color, ModelMap map) {
		logger.debug("Request Parameter are {}, {}, {}", name, weapon, color);
		return "character";
	}
}
```

# 10. Understanding DispatcherServlet
## History
### Model 1 Architecture
All the code was in views (JSPs), like view logic, flow logic, queries to database.
It was very complex as it had no separation of concerns, and thus it was harder to maintain.

### Model 2 Architecture
It was better than [model 1 architecture](#Model%201%20Architecture) as it has separation of controls.
What it lacked was that there was no way to implement common features to all controllers.

### Model 2 Architecture - Front Controller
This solved the problem with model 2 architecture by providing a central controller (aka front controller), so that all the common features could be implemented inside it.

## Spring MVC Front Controller - Dispatcher Servlet
Spring way of implementing the Model 2 Architecture - Front Controller.
![Spring-MVC-Flow-Diagram](Attachments/Spring-MVC-Flow-Diagram.png)

# 11. Create a Login Form
```html
<html>
<head>
    <title>Login Page</title>
<style>
    * {
        background-color: black;
        color: white;
        }
</style>
</head>
<body>
    Welcome to the login page!
    <form method="post">
        Name: <input type="text" name="name"/>
        Password: <input type="password" name="password"/>
        <input type="submit"/>
    </form>
</body>
</html>
```

If you don't specify a method on the form, then the default method used is `GET`. When you submit form using `GET`, the form data is sent as query parameters. This is not secure as the query parameters will be visible to all routers.
On the other hand if you use `POST` method, form data is not sent as the query parameters. Instead, it is sent as payload.  

You can access the form data the same way you access query parameters, using `@RequestParam` annotation.
```java
	@RequestMapping(value = "login", method = RequestMethod.POST)
	public String gotoWelcomePage(@RequestParam String name, @RequestParam String password, ModelMap model) {
		model.put("name", name);
		model.put("password", password);
		return "welcome";
	}
```

# 16. Session vs Request Scopes
By default the values put in the `ModelMap` are in request scopes. The values of type request scope are available only for that request. While the values inside the session scope survives the whole session.
To make a attribute session scoped, add the `@SessionAttributes(<attribute-name>)` annotation on all the controllers.

# 17. Adding JSTL to Spring Boot
## Step 1. Add the dependency
```xml
		<dependency>
			<groupId>jakarta.servlet.jsp.jstl</groupId>
			<artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
		</dependency>
		<dependency>
			<groupId>org.eclipse.jetty</groupId>
			<artifactId>glassfish-jstl</artifactId>
			<version>11.0.24</version>
		</dependency>
```

## Step 2. Add the tag
Before using jstl, you need to add a tag inside your jsp files indicating which functunalities you want to import
```jsp
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

## Step 3. Use the JSTL
```jsp
        <tbody>
            <c:forEach items="${todos}" var="todo">
                <tr>
                    <td>${todo.id}</td>
                    <td>${todo.description}</td>
                    <td>${todo.targetDate}</td>
                    <td>${todo.done}</td>
                </tr>
            </c:forEach>
        </tbody>
```

# 18. Adding Bootstrap
## Step 1. Add the Dependency
Bootstrap has a dependency on jquery, so you also need to include that.
```xml
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap</artifactId>
			<version>5.1.3</version>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery</artifactId>
			<version>3.6.0</version>
		</dependency>
```

## Step 2. Refer to the files.
```html
    <link href="webjars/bootstrap/5.1.3/css/bootstrap.min.css" rel="stylesheet">
    <script src="webjars/bootstrap/5.1.3/js/bootstrap.min.js"></script>
    <script src="webjars/jquery/3.6.0/jquery.min.js"></script>
```

# 21. 
You can also use `ModelMap` to retrieve values. To redirect to a different endpoint, return the `redirect:/<end-point>` from the method.
```java
	@RequestMapping(value = "add-todo", method = RequestMethod.POST)
	public String addTodo(@RequestParam String description, @RequestParam String targetDate, ModelMap model) {
		todoService.addTodo((String) model.get("name"), description, targetDate);
		return "redirect:/list-todos";
	}
```

# 22. Spring Boot Validation
## Step 1. Add Dependency
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
```

## Step 2. Create Two Way Binding
Create a two form binding. This means binding form to an object. Binding object properties to the input fields. 
It serves two purposes:
1. When the page (and form) is loaded, the `input` values are populated based on the object passed to the `ModelMap`.
2. When the form is submitted, the modified `input` values are then used to populate the object used in another endpoint as a parameter.

To create two way binding you need to import form tag libraray.
```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```


`modelAttribute` - the model attribute(key) whose properties should be accessed.
`path` - the property name
```jsp
<form:form method="post" modelAttribute="todo">
	Description: <form:input type="text" path="description" required="required"/>
	<form:input type="hidden" path="id"/>
	<form:input type="hidden" path="done"/>
	<input type="submit" class="btn btn-success"/>
</form:form>
```

In the endpoint, which first loads the `jsp`, you need to pass the object, which will be used inside the `jsp` to populate the `input`.
```java
@RequestMapping(value = "add-todo", method = RequestMethod.GET)
public String getAddTodoPage(ModelMap model) {
	String username = (String) model.get("name");
	Todo todo = new Todo(0, username, "", LocalDate.now().plusYears(1), false);
	model.put("todo", todo);
	return "add-todo";
}
```

In the endpoint in which you will use the `input` value, add a parameter on the method.
```java
@RequestMapping(value = "add-todo", method = RequestMethod.POST)
public String addTodo(ModelMap model, Todo todo) {
	System.out.println(todo);
	todoService.addTodo((String) model.get("name"), todo.getDescription());
	return "redirect:/list-todos";
}
```

## Step 3. Add Validation
You add validation directly on the properties of the class which you use in two-way-binding. The `message` is used to display errors on the `jsp`.
```java
@Size(min = 10, message = "Enter atleast 10 characters.")
private String description;
```

If the user fails this validation, then an error page is shown as result. If you don't want to show the error page and instead want to do some other logic, declare a `BindingResult` parameter on the method (at the end).
```java
@RequestMapping(value = "add-todo", method = RequestMethod.POST)
public String addTodo(ModelMap model, @Valid Todo todo, BindingResult result) {
	if (result.hasErrors()) {
		return "add-todo";
	}
	todoService.addTodo((String) model.get("name"), todo.getDescription());
	return "redirect:/list-todos";
}
```

If you want to show the `message` to the user, add the `form:errors` tag inside the `jsp`. Inside the form-tags, to specify a class, you need to use `cssClass` instead of the `class`.
```jsp
Description: <form:input type="text" path="description" required="required"/>
	<form:errors path="description" cssClass="text-warning"/>
```

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)