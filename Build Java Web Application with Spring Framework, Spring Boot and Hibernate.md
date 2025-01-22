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

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)