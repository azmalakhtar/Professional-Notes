# 6. Routing
To use routing, add the `react-router-dom` dependency.

```jsx
export default function TodoApp() {
	return (
		<div className="TodoApp">
			<BrowserRouter>
				<Routes>
					<Route path='/' element={<LoginComponent />}></Route>
					<Route path='/login' element={<LoginComponent />}></Route>
					<Route path='/welcome' element={<WelcomeComponent />}></Route>
				</Routes>
			</BrowserRouter>
		</div>
	);
}
function LoginComponent() {
	// code...
	const navigate = useNavigate()

	function handleSubmit() {
		setIsChecked(true)
		const un = "test"
		const pwd = "pwd"

		if (username === un && password === pwd) {
			navigate('/welcome')
		}
		else {
			setIsAuthenticated(false)
		}
	}
	// more code...
```

> To specify an error route(for undefined routes), add a route with `*` as the path.

# 8. Params
You can pass parameters from one route to another.

1. Define the path to include a param, specified by `:`.
```jsx
<Route path='/welcome/:username' element={<WelcomeComponent />}></Route>
```
2. Navigate to the route, by passing the specified param.
```jsx
navigate(`/welcome/${username}`)
```
3. Access the passed parameters using `useParams` hook.
```jsx
const { username } = useParams();
```

# 10. Linking
If you use an anchor tag `<a />` to go from one page to another, it will perform a full page reload. To instead, only refresh the page, use `<Link />` tag provided by `react-router-dom`.

```jsx
<Link to='/todos'>Manage Todos</Link>
```

# 12. Bootstrap
To add bootstrap as a dependency, run `npm install bootstrap`.

Then import it in the place where you want to use, preferably on `index.js`.
```jsx
import 'bootstrap/dist/css/bootstrap.min.css'
```


# 15. Context
To share some state between multiple components, you can use the `useContext` hook.

## Step 1. Create A Context Provider
Create a context using the `createContext` hook. Then create a functional component that will wrap around other components, as using this component you will share the state.
```js
import { createContext, useState } from "react";

// create a context
export const AuthContext = createContext()


// share the created context with other components
export default function AuthProvider({ children }) {
	const [number, setNumber] = useState(0)

	return (
		// put some state in the context
		<AuthContext.Provider value={{ number }}>
			{children}
		</AuthContext.Provider>
	);
}
```

## Step 2. Wrap the App within the Provider
```jsx
export default function TodoApp() {
	// code ...
	return (
		<div className="TodoApp">
			<AuthProvider>
					<HeaderComponent isLoggedIn={isLoggedIn} />
			</AuthProvider>
		</div>
	);
}
```

## Step 3. Use Context
To get context values, you will need to use `useContext(<context>)` hook. It will take in the `AuthContext`, the one using which you created `AuthProvider`. To access the value, access it like an object.
```jsx
export default function HeaderComponent({ isLoggedIn }) {
	const authContext = useContext(AuthContext)
	console.log(authContext.number)
	return (
		// some jsx
	);
}
```

## Additional
Instead of writing the same code to use context in different components, you can instead create your own hook.
```js
// create a hook
export const useAuth = () => useContext(AuthContext)
```


# 19. Secure Routes
To protect some routes from being accessed, you can create a `AuthenticatedRoute` and validate it inside this component.
```jsx
function AuthenticatedRoute({ children }) {
	const { isAuthenticated } = useAuth()
	if (isAuthenticated) {
		return children
	}
	return <Navigate to="/" />
}
```

Then wrap the routes you want to secure inside the `AuthenticatedRoute`.
```jsx
export default function TodoApp() {

	return (
		<div className="TodoApp">
			<AuthProvider>
				<BrowserRouter>
					<HeaderComponent />

					<Routes>
						<Route path='/' element={<LoginComponent />} />
						<Route path='/login' element={<LoginComponent />} />
						<Route path='/welcome/:username' element={
							<AuthenticatedRoute>
								<WelcomeComponent />
							</AuthenticatedRoute>
						} />
						<Route path='/todos' element={
							<AuthenticatedRoute>
								<ListTodosComponent />
							</AuthenticatedRoute>
						} />
						<Route path='/logout' element={
							<AuthenticatedRoute>
								<LogoutComponent />
							</AuthenticatedRoute>
						} />
						<Route path='*' element={<ErrorComponent />} />
					</Routes>

					<FooterComponent />
				</BrowserRouter>
			</AuthProvider>
		</div>
	);
}

```

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)