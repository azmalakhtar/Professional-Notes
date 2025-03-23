# 2. Why Full-Stack Architecture?
Because they give you flexibility and allow **reuse of REST API**.

# 3. Understanding JavaScript and EcmaScript History?
ES - EcmaScript 
EcmaScript is **standard**. JavaScript is **implementation** of EcmaScript.

# 5. npm
`npm init` - create new project
`package.json` - contains metadata like dependencies, name, author etc.
`npm install <package-name>` - installs the current packages
`npm package` - installs all the dependencies
`node_modules` - downloaded dependencies

# 6. Creating React App with `create-react-app`
**React** is one of the most popular JavaScript libraries to build **SPA**(Single Page Applications). It is an open-source project created by Facebook.

NPM - package manager -> Install, update and delete JS packages.
NPX - package executor -> Execute JS packages directly, without installing

`npx create-react-app <app-name>` => to create a react app
`npm start` (from inside the project)=> to run react app

# 7. Important Node Commands
`npm start`: Runs the app in development mode.
`npm test`: Run unit tests
`npm run build`: Build a production deployable unit (minified & optimized for performance).
`npm install --save <dep-name>`: Install the dependecy.

# 9. React App Folder Structure
`public/index.html`: Contains the root div, in which the components are placed.
`src/index.js`: Initializes React App. Loads App Component.
`src/index.css`: Styling for entire application.
`src/App.js`: Code for app component.
`src/App.css`: Styling for App Component
`src/App.test.js`: Unit tests for App Component

# 10. React Component
React component names must always start with a capital letter. Lowercase is reserved for html.
## Parts of a Component
- View (JSX or JavaScript)
- Logic (JavaScript)
- Styling (CSS)
- State (Internal Data Store)
- Props (Pass Data)


# 11. Creating Component
```js
function App() {
  return (
    <div className="App">
      My Todo Application
      <FirstComponent></FirstComponent>
      <SecondComponent />
    </div>
  );
}

// function component
function FirstComponent() {
  return (
    <div className="FirstComponent">First Component</div>
  );
}

// class component
class SecondComponent extends Component {
  render() {
    return (
      <div className="SecondComponent">Second Component</div>
    );
  }
}
```

# 12. Function vs Class Based Component
Initially only class based component were able to have state. But after the introduction of **hooks** in React 16.8, function component can also have state.

# 13. JSX
React projects use JSX for presentation.

JSX is stricter than HTML
- Close tags are mandatory.
- Only one top-level tag is allowed.

How is JSX enabled in a React project? Different browsers have different support levels modern JS features. To ensure backward compatibility, we will use **Babel**. It can convert JSX into JS.

# 14. Best Practices
> Each component in its own file.

```jsx
export default function FirstComponent() {
	return (
		<div className="FirstComponent">First Component</div>
	);
}
```

# 15. Exploring JavaScript
Use `{}` to write js inside ...
```jsx
const person = {
	name: 'Azmal Akhatr',
	address: {
		line1: 'Baker Street',
		city: 'London',
		country: 'UK',
	},
	profiles: ['twitter', 'linkedin', 'instagram'],
	printProfile: () => {
		person.profiles.map(
			profile => console.log(profile)
		)
	}
}

export default function LearningJavaScript() {
	return (
		<>
			<div>{person.name}</div>
			<div>{person.address.line1}</div>
			<div>{person.address.city}</div>
			<div>{person.profiles[0]}</div>
			<div>{person.printProfile()}</div>
		</>
	);
}
```

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)