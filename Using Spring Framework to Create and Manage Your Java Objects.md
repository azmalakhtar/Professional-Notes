## 1. Getting Spring Framework to Create and Manage You Java Objects
To tell Spring to automatically create an object of a class, add the `@Component` annotation on that class.
```java
package com.azmalakhtar.learn_spring_framework.game;

import org.springframework.stereotype.Component;

@Component
public class PacmanGame implements GamingConsole {
 /* code */
}
```
If the class in which you are requiring the component is in different package than the component, add the `@ComponentScan(<component-package-name>)` annotation on the class.
```java
@Configuration
@ComponentScan("com.azmalakhtar.learn_spring_framework.game")
public class GamingAppLauncherApplication {
    public static void main(String[] args) {
        try (var context = new AnnotationConfigApplicationContext(GamingAppLauncherApplication.class)) {
            var gameRunner = context.getBean(GameRunner.class);
            gameRunner.run();
        } catch (Exception exception) {
            throw exception;
        }
    }
}
```

## 2. `@Primary` & `@Qualifier` for Spring Component
Annotating a component with `@Primary` makes the component as the preferred component for auto-wiring.
```java
@Component
@Primary
public class MarioGame implements GamingConsole {
	/* code */
}
```
Annotating a component with `@Qualifier(<name>)` gives the ability to inject this particular component wherever wanted by specifying the `@Qualifier(<name>)` annotation on the auto-wired property.

```java
@Component
@Qualifier("sc-game")
public class SuperContraGame implements GamingConsole {
	/* code */
}
```

```java
@Component
public class GameRunner {
	private GamingConsole game;

	public GameRunner(@Qualifier("sc-game") GamingConsole game) {
		// game will be of type SuperContraGame because of the Qualifier having
		// the name as "sc-game" which is the qualifier name of SuperContraGame
		// component
		this.game = game;
	}
}
```

## 3. Primary and Qualifier
`@Primary` => A bean should be given preference when multiple candidates are qualified.
`@Qualifier` => A specific bean should be auto-wired (name of the bean can be used as qualifier).

`@Qualifier` has higher priority than `@Primary`.

## 4. Different Types of Dependency Injection
### Constructor Based
Dependencies are set by creating the Bean using its constructor.
The `@Autowired` annotation is optional in constructor based injection.
```java
@Component
class YourBusinessClass {
    Dependency1 dependency1;
    Dependency2 dependency2;

    @Autowired
    public YourBusinessClass(Dependency1 dependency1, Dependency2 dependency2) {
        this.dependency1 = dependency1;
        this.dependency2 = dependency2;
    }
```
### Setter Based
Dependencies are set by calling setter method on your beans.
```java
@Component
class YourBusinessClass {
    Dependency1 dependency1;
    Dependency2 dependency2;

    @Autowired
    public void setDependency1(Dependency1 dependency1) {
        this.dependency1 = dependency1;
    }

    @Autowired
    public void setDependency2(Dependency2 dependency2) {
        this.dependency2 = dependency2;
    }
}
```
### Field
Dependency is injected using Reflection.
```java
@Component
class YourBusinessClass {
    @Autowired
    Dependency1 dependency1;
    @Autowired
    Dependency2 dependency2;
}
```

### Spring Team Recommendation

> [!note]
> Spring team recommends Constructor-based injection as dependencies are automatically set when an object is created.

## 5. Spring Framework - Important Terminology
**Dependency Injection**: Identify beans, their dependencies and wire them together (provides *IOC* - Inversion Of Control)
**Spring Beans**: An object managed by Spring Framework
**IOC Container**: Manages the lifecycle of beans and dependencies.
**Autowiring**: Process of wiring in dependencies for a Spring Bean

## 6. Comparing `@Component` and `@Bean`
| Heading           | `@Component`                                      | `@Bean`                                                                  |
| ----------------- | ------------------------------------------------- | ------------------------------------------------------------------------ |
| Where?            | Can be used on any Java class.                    | Typically used on methods in Spring Configuration classes.               |
| Ease of use       | Very easy. Just add an annotation.                | You write all the code.                                                  |
| Autowiring        | Yes - Field, Setter or Constructor Injection      | Yes - method call or method parameters                                   |
| Who creates beans | Spring Framework                                  | You write bean creation code.                                            |
| Recommended for   | Instantiating Beans for Your Own Application Code | 1. Custom Business Logic, 2. Instantiating Beans for 3rd-party libraries |


---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)