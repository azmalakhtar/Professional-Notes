# 3. H2 Console
To enable H2 console add the following configuration. Spring generates a different url everytime that you run the project. If you want, you can add a specific url to be used.
```properties
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:testdb
```

To access the console, visit `h2-console` endpoint.

JDBC automatically picks up the `schema.sql` file created under the `resources` folder.
```sql
create table course (
	id bigint not null,
	name varchar(255) not null,
	author varchar(255) not null,
	primary key (id)
);
```

# 5. Spring JDBC - Insert Hardcoded Data
Create a repository class. Autowire the `JdbcTemplate` and use it to execute queries on the database.
```java
@Repository
public class CourseJdbcRepository {
	@Autowired
	private JdbcTemplate springJdbcTemplate;

	private static String INSERT_QUERY = 
	"""
		insert into course (id, name, author)
		values (1, 'Learn Spring', 'in28minutes');
	""";

	public void insert() {
		springJdbcTemplate.update(INSERT_QUERY);
	}
}
```

To run a piece of code on the application startup, create a class that implements `CommandLineRunner` interface and put the code to be run in the `run` method.
```java
@Component
public class CourseJdbcCommandLineRunner implements CommandLineRunner {
	@Autowired
	private CourseJdbcRepository repository;

	@Override
	public void run(String... args) throws Exception {
		repository.insert();
	}
}
```

# 6. Inserting and Deleting Using Spring JDBC
```java
@Repository
public class CourseJdbcRepository {
	@Autowired
	private JdbcTemplate springJdbcTemplate;

	private static String INSERT_QUERY = 
	"""
		insert into course (id, name, author)
		values (?, ?, ?);
	""";

	private static String DELETE_QUERY = 
	"""
		delete from course 
		where id = ?
	""";

	public void insert(Course course) {
		springJdbcTemplate.update(INSERT_QUERY, course.getId(), course.getName(), course.getAuthor());
	}

	public void deleteById(long id) {
		springJdbcTemplate.update(DELETE_QUERY, id);
	}
}
```

# 7. Querying Data using Spring JDBC
To query, we have many `query` methods. To query a single object, we can call the `queryForObject` method. It takes in the query, a mapper, and the arguments if any.
The responsibility of a mapper is to convert the `ResultSet` to the `Bean`. In this case we have used a `BeanPropertyRowMapper()`, which maps the object based on the column name and property name of the object.
```java
@Repository
public class CourseJdbcRepository {
	@Autowired
	private JdbcTemplate springJdbcTemplate;

	private static String SELECT_QUERY = 
	"""
		select * from course 
		where id = ?
	""";

	public Course findById(long id) {
		return springJdbcTemplate.queryForObject(SELECT_QUERY, 
			new BeanPropertyRowMapper<>(Course.class), id);
	}
}
```

# 8. JPA and Entity Manager
## Step 1. Create a Entity Class
Create a class whose data you want to persist on the database.

Add the `@Entity(name = <table-name>)` annotation on the class. If the table name and class name are same, you can omit the name variable.

Add the `@Column(name = <col-name>)` annotation on the properties. If the column name and property name is same, you can omit the whole `@Column` annotation.

Add the `@Id` on the primary key.

```java
@Entity
public class Course {
	@Id
	private long id;
	private String name;
	private String author;

	/* code */
}
```

## Step 2. Create A Repository
Create a repository class which makes use of the `EntityManager` to do the database operation.
Add the `@Transactional` annotation on the class.

```java
@Repository
@Transactional
public class CourseJpaRepository {
	@Autowired
	private EntityManager entityManager;

	public void insert(Course course) {
		entityManager.merge(course);
	}

	public Course findById(long id) {
		return entityManager.find(Course.class, id);
	}

	public void deleteById(long id) {
		Course course = entityManager.find(Course.class, id);
		entityManager.remove(course);
	}
}
```

You can also use the `@PersistenceContext` annotation on the `EntityManager` to autowire it.

To see the the queries being executed by JPA on the output, add the following to config.
```properties
spring.jpa.show-sql=true
```


# 10. Spring Data JPA
To manage an entity, you can use Spring Data JPA instead of JPA.

Create a repository interface which extends the `JpaRepository<T, D>` interface, where `T` is the type of class to be managed and `D` is the type of primary key. 
```java
public interface CourseSpringDataJpaRepository extends JpaRepository<Course, Long> {}
```
A bean of this interface will be created. You can autowire this bean wherever you need. The `JpaRepository` provides a lot of methods to interact with the database.

# e. JDBC -> Spring JDBC -> JPA -> Spring JPA
## JDBC
- Write a lot of SQL queries.
- And write a lot of Java code.

## Spring JDBC
- Write a lot of SQL queries.
- But lesser Java Code.

## JPA
- No queries.
- Just Map entities to Table.

## Spring Data JPA
- No queries
- No Java Code


# 11. Exploring Features of JPA
Spring data JPA allows you to add methods to the Repository interface. For example to find objects by one of their property, add a method `findBy<property-name>` to the repository.
```java
public interface CourseSpringDataJpaRepository extends JpaRepository<Course, Long> {
	public List<Course> findByAuthor(String author);
	public List<Course> findByName(String name);
}
```

# 12. Hibernate vs JPA
JPA defines the specification. It is an API. Hibernate is one of the popular implementations of JPA.

> [!note]
> Prefer not to use hibernate annotation directly in your code. Because it would result in a lock to hibernate. Instead use the jakarta annotations. This way if you want to swap one implementation of JPA for another, you wouldn't have to worry about changing the code.


---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)