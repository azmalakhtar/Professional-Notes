# 3. Enable CORS Requests
By default, CORS requests are prohibited. To enable CORS, you need to create a bean returning `WebMvcConfigurer`. `WebMvcConfigurer` is an interface, to enable CORS requests, you need to implement the `addCorsMapping(CorsRegistry)` method.

```java
@Bean
public WebMvcConfigurer corsConfigurer() {
	return new WebMvcConfigurer() {
		public void addCorsMappings(CorsRegistry registry) {
			registry.addMapping("/**")
					.allowedMethods("*")
					.allowedOrigins("http://localhost:3000");
		}
	};
}
```

# 6. Specify a Base URL for Axios
```js
const apiClient = axios.create(
	{
		baseURL: "http://localhost:8080"
	}
)
export const retrieveHelloWorldBean =
	() => apiClient.get("/hello-world-bean")
```

# 8. `useEffect`
```jsx
useEffect(() => refreshTodos(), [])
```

# 13. Formik and Moment
Formik for form management and Moment for date formatting.
```jsx
function onSubmit(values) {
	console.log(values)
}

// code....
<Formik initialValues={{ description, targetDate }}
	enableReinitialize={true}
	onSubmit={onSubmit}
>
	{
		(props) => (
			<Form>
				<fieldset className="form-group">
					<label>Description</label>
					<Field type="text" className="form-control" name="description" />
				</fieldset>
				<fieldset className="form-group">
					<label>Target Date</label>
					<Field type="date" className="form-control" name="targetDate" />
				</fieldset>
				<div>
					<button className="btn btn-success m-5" type="submit">Save</button>
				</div>
			</Form>
		)
	}
</Formik>

```

# 14. Validation Using Formik
## Step 1. Create A Validate Function
Create a function which will receive the values. If the values are not valid, return an error object with error otherwise return an empty object.
```jsx
function validate(values) {
	let errors = {}

	if (values.description.length < 5) {
		errors.description = 'Enter atleast 5 characters'
	}
	if (values.targetDate == null) {
		errors.description = 'Enter a target date'
	}
	console.log(errors)
	return errors
}
 // code...
```
## Step 2. Specify the validate function
```jsx
<Formik initialValues={{ description, targetDate }}
	enableReinitialize={true}
	onSubmit={onSubmit}
	validate={validate}
	validateOnChange={false}
	validateOnBlur={false}
>
```

## Step 3. Add `ErrorMessage`
Inside the `Form` tag, add the `ErrorMessage` tag specifying the `name`, which is the key inside the error object whose value should be printed inside.
```jsx
<Form>
	<ErrorMessage
		name="description"
		component="div"
		className="alert alert-warning"
	/>
	<ErrorMessage
		name="targetDate"
		component="div"
		className="alert alert-warning"
	/>
	{
		// code...
	}
</Form>
```

# 16. Update Todo
To pass something as request body, add it to the api call in the form of `apiClient.<METHOD>(<URL>, <REQUEST-BODY>)`.
```js
export const updateTodoApi =
	(username, id, todo) => apiClient.put(`/users/${username}/todos/${id}`, todo)
```

Spring fills the `@RequestBody Todo` based on the `Todo`'s attribute names. Thus pass an object as request body whose key matches with the `Todo`'s attribute names.
```jsx
function onSubmit(values) {
	const todo = {
		id: id,
		username: username,
		description: values.description,
		targetDate: values.targetDate,
		done: false,
	}
	updateTodoApi(username, id, todo)
		.then((response) => {
			navigate('/todos')
		})
		.catch(console.log)
}
```

# 17. Adding Todo
You can reuse the `TodoComponent` to add a todo. Navigate to `/todo/-1`. And inside the `TodoComponent` only retrieve the todo if the id is not -1.

# 20. Authentication For Logging In
Code a function to make request to the `/basicuth` endpoint, passing in the `Authorization` header.
```js
export const executeBasicAuthenticationService =
	(token) => apiClient.get("/basicauth", {
		headers: {
			Authorization: token
		}
	})
```

Make request to the `basicauth` endpoint passing in the token, created using the following pattern:
```js
const basicAuthToken = "Basic " + window.btoa(username + ":" + password)
```

# 22. Authentication Logic
Inside the `LoginComponent`
```js
async function handleSubmit() {
	if (await login(username, password)) {
		navigate(`/welcome/${username}`)
		setIsError(false)
	}
	else {
		setIsError(true)
	}
}
```
Inside `AuthContext`
```js
async function login(username, password) {
	const basicAuthToken = "Basic " + window.btoa(username + ":" + password)
	try {
		const response = await executeBasicAuthenticationService(basicAuthToken)
		setIsAuthenticated(false)
		if (response.status == 200) {
			setIsAuthenticated(true)
			setUsername(username)
			setToken(basicAuthToken)
			return true
		}
		else {
			logout()
			return false
		}
	} catch (error) {
		logout()
		return false
	}
}

function logout() {
	setIsAuthenticated(false)
	setUsername(null)
	setToken(null)
}
```

# 24. Adding Interceptors
To intercept a request and add some logic to it, you can use `apiClient.interceptors`.
```js
apiClient.interceptors.request.use(
	config => {
		config.headers.Authorization = basicAuthToken
		return config
	}
)
```

# 25. JWT and Spring Security
- Basic Authentication
	- No expiration date
	- No user details
	- Easily decoded
- How about a custom token system?
	- Custom structure
	- Possible security flaws
	- Service Provider and Service Consumer should understand
- JWT (Json Web Token)
	- Open, industry standard for representing claims securely between two parties
	- Can contain User Details and Authorizations

A Jwt contains:
- Header
	- Type: JWT
	- Hashing Algorithm: HS512
- Payload
	- Standard Attributes
		- iss: The issuer
		- sub: The subject
		- aud: The audience
		- exp: When does the token expire
		- iat: When was the token issued?
	- Custom Attributes
		- youratt1: Your custom attribute 1
- Signature
	- Includes a secret

Authorization Header Format:
```
Bearer TOKEN_VALUE
```

# 26. Jwt and React
```js
export const executeJwtAuthenticationService =
	(username, password) => apiClient.post("/authenticate", { username, password })
```

```js
const response = await executeJwtAuthenticationService(username, password)
if (response.status == 200) {
	const jwtToken = "Bearer " + response.data.token
	setIsAuthenticated(true)
	setUsername(username)
	setToken(jwtToken)
	apiClient.interceptors.request.use(
		config => {
			config.headers.Authorization = jwtToken
			return config
		}
	)
	return true
}

```

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)