# 2. World Before Spring Boot
Setting up Spring Projects before Spring Boot was not easy as you have to manually take care of 
- Dependency Management (`pom.xml`)
- Define Web App Configuration (`web.xml`)
- Manage Spring Beans (`context.xml`)
- Implement Non Functional Requirements (like Error Handling, Logging etc) (`web.xml`)

This will typically take a few days to setup for each project (and countless hours to maintain)

# 4. Hello World API with Spring Boot
Create a controller class and annotate it with `@RestController`.
Inside the controller declare methods which will act as endpoints and annotate them with `@RequestMapping(<endpoint>)`.
```java
@RestController
public class CourseController {
	@RequestMapping("/courses")
	public List<Course> retrieveAllCourses() {
		var courses = Arrays.asList(
			new Course(1, "Learn spring boot", "in28minutes"),
			new Course(2, "Learn DevOps", "in28minutes")
		);
		return courses;
	}
}
```

# 5. Goal of Spring Boot
> To help you build **PRODUCTION-READY** apps **QUICKLY**.

Build Quickly
- Spring Initializer
- Spring Boot Starter Projects
- Spring Boot Auto Configuration
- Spring Boot DevTools

Be Production-Ready
- Logging
- Different Configuration for Different Environments
- Monitoring (Spring Actuator)

# 6. Spring Boot Starter Projects
I need a lot of framework to build application features:
- Build a REST API: I need Spring, Spring MVC, Tomcat, JSON Conversion...
- Write Unit Tests: I need Spring Test, JUnit, Mockito

How can I group them and make it easy to build applications? Using starters, which are convenient dependency descriptor for different features.

Spring Boot provides variety of starter projects:

| Feature                                 | Starter Project                                                                                           |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| Web Application & REST API              | Spring Boot Starter Web (spring-webmvc, spring-web, spring-boot-starter-tomcat, spring-boot-starter-json) |
| Unit Tests                              | Spring Boot Starter Test                                                                                  |
| Talk to a database using JPA            | Spring Boot Starter Data JPA                                                                              |
| Talk to a database using JDBC           | Spring Boot Starter Data JDBC                                                                             |
| Secure your web application or REST API | Spring Boot Starter Security                                                                              |

# 7. Auto Configuration
I need a lot of configuration to build Spring app: component scan, DispatcherServlet, Data Sources, JSON Conversion

To simplify this, Spring Boot provides Auto Configuration
Auto Configuration is automated configuration for your app. It is decided based on:
- Which frameworks are in the class path?
- What is the existing configuration (Annotations etc)?

To change the logging level, add this line to the `application.properties`
```properties
logging.level.org.springframework=debug
```

Example for Spring Boot Starter Web:
- Dispatcher Servlet (`DispatcherServletAutoConfiguration`)
- Default Error Page (`ErrorMvcAutoConfiguration`)
- Bean - JSON conversion (`JacksonHttpMessageConvertersConfiguration`)

# 8. Spring Boot DevTools
It automatically restarts the server whenever the code changes. This saves us time as we don't have to manually restart the server.
To use it in your project, add the following dependency:
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
```

> [!warning]
It can detect code changes in most of the file type. But for `pom.xml` dependency changes, it won't be able to detect it, you will need to restart server manually.

# 9. Profiles
Applications have different environments: Dev, QA, Stage, Prod, etc. They all need different configuration(different databases, web services, etc).
Profiles are environment specific configuration.

## Step 1. Make Different Profiles
To make a profile, add the file as `application-<profile-name>.properties` in the `resources` folder. Inside the file add the environment specific configuration.
`application-prod.properties`
```properties
logging.level.org.springframework=debug
```

`application-dev.properties`
```properties
logging.level.org.springframework=trace
```

## Step 2. Specify a Profile to Use
To use a profile add the `spring.profiles.active` property with the value of the profile name.
```properties
spring.application.name=learn-spring-boot
logging.level.org.springframework=off
spring.profiles.active=prod
```
Environment specific configuration (profiles) has higher priority than the ones declared in `application.properties`.

## Logging Levels
The logging level specified logs all the messages from that level and the levels below it.
- Trace
- Debug
- Info
- Warning
- Error 
- Off

# 10. ConfigurationProperties
You can access properties defined in the `application.properties` by using the `@ConfigurationProperties(prefix = <prefix>)`.
```properties
currency-service.url=http://main.com
currency-service.username=mainUsername
currency-service.key=mainKey
```

To access properties with the prefix `currency-service`, 
1. Create a class having the class field name same as the properties. 
2. Create setters and getters for the fields.
3. Add the `@ConfigurationProperties`, passing the prefix("currency-service") to the annotation. Also add the `@Component` annotation.

```java
@ConfigurationProperties(prefix = "currency-service")
@Component
public class CurrencyServiceConfiguration {
	private String url;
	private String username;
	private String key;

	public String getUrl() {
		return url;
	}
	public void setUrl(String url) {
		this.url = url;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getKey() {
		return key;
	}
	public void setKey(String key) {
		this.key = key;
	}
}
```

# 11. Embedded Servers
## In the OLD times
We have to perform following steps in order to deploy your application
1. Install Java
2. Install Web/Application Server (like tomcat, websphere, etc)
3. Deploy the application WAR (Web ARchive)

This was complex to setup, also known as the **OLD WAR Approach**.

## But now
Spring boot provide embedded server, which makes the deployment a breeze. The steps are:
1. Install Java
2. Run JAR file

Embedded Servers Examples:
- spring-boot-starter-tomcat
- spring-boot-starter-jetty
- spring-boot-starter-undertow

# 12. Actuators
To monitor and manage your application in production, Spring provides actuators. It provides endpoints that you can call to get information about your running application.
Add the following dependecy.
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
```

To use actuators call the `actuator` endpoint.
To specify which endpoints to expose, configure it as a property in the `.properties` file. To expose all the endpoints, pass `*` as the value.
```properties
management.endpoints.web.exposure.include=health,metrics
```

# 13. Spring vs Spring MVC vs Spring Boot
Spring Framework is all about dependency injection. But just dependency injection is not enough (you need other frameworks to build apps).
This is where Spring Modules and Spring Projects comes in. They extend the Spring Eco System.
Spring MVC is a Spring Module which simplify building web apps & REST API.
Spring Boot is a Spring Project which helps in building PRODUCTION-READY apps QUICKLY. 


---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)