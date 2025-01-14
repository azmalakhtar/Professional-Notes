## Step 1. Understanding the Need For Spring Framework
Using Spring & Spring Boot you can build a variety of applications like
- Web App
- REST API
- Full Stack
- Microservices
- and more...

## 5. Understanding Loose Coupling and Tight Coupling
Coupling is a measure of how much work is required to change something?
In a tightly coupled system, changing a thing requires you to change a lot of other things. On the other hand, loosely coupled system allows you to make change in a thing without requiring much change n other things.

Example:
- A engine is tightly coupled to a car.
- A wheel is loosely coupled to a car.

> [!note]
> In software, we want loose coupling as much as possible. We want to make functional changes with as less code changes as possible.

## 6. Using Interfaces for Loose Coupling
Instead of requiring a specific class in your code, require a interface. In this way if you need to swap one implementation for another, you won't need to change the code where the implementation is used.

## 8. Spring Bean and Configuration
Spring can manage our objects for us.
For using Spring we need to 
1. Create a Spring Context object.
2. Create a Spring Configuration class which configures what beans it have among other things.
3. Use the spring context wherever needed to retrieve the beans.
```java
public class App02HelloWorldSpring {
	public static void main(String[] args) {
		//1: Launch a spring context
		var context = new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);
		//2: Configure the things that we want spring to manage
		// Creating the HelloWorldConfiguration class
		//3: Retrieve Beans managed by Spring
		System.out.println(context.getBean("name"));
	}
}
```

```java
@Configuration
public class HelloWorldConfiguration {
	@Bean
	public String name() {
		return "Steve Rogers";
	}
}
```

Trying to call `getBean()` with a name whose bean is not defined results in an `NoSuchBeanDefinitionException`.

## 10. Implementing Autowiring
By default the name of the bean is same as the method name, but if you provide a custom name to the `@Bean` annotation, provided name is used.
```java
	@Bean(name = "random")
	public Address address() {
		return new Address("SOME", "Far from home");
	}
```

You can also retrieve beans based on the type. But if you have two beans of the same type, and try to retrieve them based on type, it will result in an exception.
```java
System.out.println(context.getBean(Address.class));
```

You can create beans which requires other beans. You can either directly call the method which declare the required bean or in your bean declare a parameter of the type of the required bean and name the parameter same as the required bean name.
```java
@Configuration
public class HelloWorldConfiguration {

	@Bean
	public String name() {
		System.err.println("name called");
		return "I am STEVE ROGERS";
	}

	@Bean
	public int age() {
		System.err.println("age called");
		return 100;
	}

	@Bean
	public Person person() {
		return new Person("Peter Parker", 20, address());
	}
	@Bean
	public Person person2() {
		return new Person(name(), age(), sherlockAddress());
	}
	@Bean
	public Person person3(String name, int age, Address random) {
		return new Person(name, age, random);
	}


	@Bean
	public Address sherlockAddress() {
		return new Address("Baker Street", "London");
	}

	@Bean(name = "random")
	public Address address() {
		System.err.println("address called");
		return new Address("SOME", "Far from home");
	}
}

```

## 12. Spring IOC Container
Spring Container/ Spring Context / IOC Container : It manages spring beans and their lifecycle.
There are two types:
1. Bean Factory: Basic Spring Container (rarely directly used)
2. Application Context: Advanced Spring Container with enterprise-specific features. It is easy to use in web applications, with spring AOP (mostly used).


## 13. Java Bean vs POJO vs Spring Bean
### Java Bean
Any class that adhering to 3 constarints
1. Have public non-args constructor
2. Allow access to their properties using getters and setters methods
3. Implement `java.io.Serializable`

### POJO
- No constraints.
- Any java object is a POJO.

### Spring Bean
Any java object that is managed by Spring

## 14. 
### How can I list all the beans managed by the Spring Framework?
Call `getBeanDefinitionNames()` on the spring context to get the names of all beans name as `String[]`.

### What if you have multiple matching beans?
#notunderstood 
It will throw an error.
To resolve this issue, you can make one of the bean primary by adding the `@Primary` annotation on the bean.
You can also add the `@Qualifier(<name>)`, to make a qualifier name.

## 17. Review
Topics we covered in this section:
1. Tight Coupling
2. Loose Coupling
3. Java Interfaces
4. Spring Container
5. Application Context
6. Basic Annotations
7. `@Configuration`
8. `@Bean`
9. Auto-wiring
10. Java Bean vs Spring Bean

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)