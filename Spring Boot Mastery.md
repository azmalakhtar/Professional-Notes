## 1. What is Spring Boot?
Spring boot makes it easy to create stand-alone, production-grade Spring applications that  you can just run.
It simplifies a lot of things that you have to do in Spring manually.

## 3. Basic
You can convert any class into a rest controller by annotating it with `@RestController`.
To set a get endpoint use `@GetMapping`.
By default spring boot uses tomcat server on port `8080`.
```java
@RestController
public class MyClass {

    @GetMapping("abc")
    public String sayHi() {
        return "Hello";
    }
}
```

## 4. Maven
Maven is a build tool used for managing dependencies and building the application. The dependencies are in the `pom.xml` file.

Maven has a lifecycle.
You can many command using the `mvn` command line tool. Pass it extra argument to tell what lifecycle stage to use like `validate`, `test`, `package` etc.

You can run a jar file using `java -jar <jar-filename>`.

## 5. Project Structure
`mvn package` produces two jar file. One of them is the fat jar. which includes the you code as well as the dependency code.

## 6. `@SpringBootApplication` Internal Working
In spring we don't create the objects ourselves, instead we ask the spring for required objects. This is called *inversion of control*(IOC). 
IOC container is a container which contains the required objects. Application context is a way to achieve IOC container.

To achieve IOC, spring scans your code and creates the object of components inside the container. A component is nothing but a class annotated with the `@Component` annotation. A component is also called a bean.

```java
@Component
public class MyComponent {
	// class is automatically registered as a Spring bean.
}
```

```java
package com.azmalakhtar.First;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class FirstApplication {
	public static void main(String[] args) {
		SpringApplication.run(FirstApplication.class, args);
	}
}
```

`@SpringBootApplication` serves as a combination of three annotations:
- `@ComponentScan`
- `@EnableAutoConfiguration`
- `@Configuration`

`@ComponentScan` tells the Spring to scan for components in the package in which this annotation has been used. Any component outside this package will not be scanned(it will not be in the IOC container).

`@Autowired` annotation is used to achieve dependency injection. Using it tells the Spring to provide the bean of the type.
```java
@RestController
public class MyClass {

	// A Person object is automatically provided by the Spring
    @Autowired
    private Person person;

    @GetMapping("abc")
    public String sayHi() {
        return person.getName();
    }
}
```

`@EnableAutoConfiguration` is used to achieve some automatic configuration.
`@Configuration` tells the Spring that the following class provides some configuration.

## 7. REST API using Spring Boot
Methods inside a controller class should be public so that they can be accessed and invoked by the spring framework or external http requests.

Use the `@RestController` to make the class a controller.
`@RequestMapping` maps the provided URL path to the controller on which it is used.

```java
package com.azmalakhtar.journal.controller;

import com.azmalakhtar.journal.entity.JournalEntry;
import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@RestController
@RequestMapping("/journal")
public class JournalEntryController {

	// mocking a database
    private Map<Long, JournalEntry> journalEntries = new HashMap<>();

    @GetMapping
    public List<JournalEntry> getAll() {
        return new ArrayList<>(journalEntries.values());
    }

	// the request body is converted into a JournalEntry as we the RequestBody annotation 
    @PostMapping
    public boolean createEntry(@RequestBody JournalEntry journalEntry) {
        journalEntries.put(journalEntry.getId(), journalEntry);
        return true;
    }

	// the name inside the {} are dynamically known when the request is made
    @GetMapping("/id/{id}")
    public JournalEntry getJournalEntryById(@PathVariable long id) {
        return journalEntries.get(id);
    }

    @DeleteMapping("/id/{id}")
    public JournalEntry deleteJournalEntryById(@PathVariable long id) {
        return journalEntries.remove(id);
    }

    @PutMapping("/id/{id}")
    public JournalEntry updateJournalById(@PathVariable long id, @RequestBody JournalEntry journalEntry) {
        return journalEntries.put(id, journalEntry);
    }
}
```

## 9. MongoDB: Key Concepts
Collections are like tables. Fields are like columns. Documents are like rows.

## 10. ORM, JPA, and Spring Data JPA
ORM is a technique used to map Java objects to database tables. It allows developer to work with databases using object-oriented programming concepts, making it easier to interact with databases.

Java Persistence API(JPA) is a way to achieve ORM, includes interfaces and annotations that you use in your java classes. It requires a persistence provider(ORM tools) for implementation.
A persistence provider is a specific implementation of the JPA specification. Examples of JPA persistence providers include Hibernate, EclipseLink, and OpenJPA. These providers implement the JPA interfaces and provide the underlying functionality to interact with databases.
Spring Data JPA is built on top of the JPA specification, but it is not a JPA implementation itself. Instead, it simplifies wokring with JPA by providing higher-level abstractions and utilities. However, to use Spring Data JPA effectively, you still need a JPA.

JPA is primarily designed for working with relational databased, where data is stored in tables with a predefined schema.
MongoDB, on the pther hand, is a NoSQL database that uses a different data model, typically based on collections of documents, which are schema-less or have flexible scheme. This fundamental difference in data models and storage structures is why *JPA is not used with MongoDB*.

In case of MongoDB, you can use Spring Data MongoDB as the persistence provider.

Query Method DSL and Criteria API are two different ways to interact with a database when using Spring Data JPA for relational databases and Spring Data MongoDB for MongoDB databases.

Query Method DSL is a simple and convenient way to create queries based on method naming conventions, while the Criteria API offers a more dynamic and programmatic approach for building complex and custom queries.

## 11. Integrate MongoDB in Your Spring Boot Application
The recommended way to structure your code is
```
controller --> service ---> repository
```
Controller maps the API endpoints to methods.
Repository interacts with the database.
Service exposes methods does something.
### Step 1. Add the required dependency and configuration
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-mongodb</artifactId>
		</dependency>

```
Inside the `application.properties` file add the following 
```
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.database=journaldb
```

### Step 2. Make A Entity Class
Create a Java class which will represent a single document. Add the `@Document("collection_name")` annotation on the class. Make one field as the id by annotating it with the `@Id`.

```java
@Document("journal_entries")
public class JournalEntry {
    @Id
    private ObjectId id;
    private String title;
    private String content;
    private LocalDateTime dateTime;
	/* 
	...
	getters and setters 
	...
	*/
}
```

### 3. Create A Repository Interface
Create a repository interface by extending the `MongoRepository<T, V>` interface.
```java
public interface JournalEntryRepository extends MongoRepository<JournalEntry, ObjectId> { }
```

### 4. Create A Service Class
Inject the repository inside your service class and use the methods provided by the repository.
```java
@Component
public class JournalEntryService {

    @Autowired
    private JournalEntryRepository journalEntryRepository;

    public void saveEntry(JournalEntry journalEntry) {
        journalEntryRepository.save(journalEntry);
    }

    public List<JournalEntry> getAll() {
        return journalEntryRepository.findAll();
    }

    public Optional<JournalEntry> getEntryById(ObjectId id) {
        return journalEntryRepository.findById(id);
    }

    public void deleteEntry(ObjectId id) {
        journalEntryRepository.deleteById(id);
    }
}
```

### Step 5. Use the Service class inside controller.
Then use the service class bean inside of your controller to handle the request.

---
### References
[Spring Boot Mastery: From Basics to Advanced](https://www.youtube.com/playlist?list=PLA3GkZPtsafacdBLdd3p1DyRd5FGfr3Ue)