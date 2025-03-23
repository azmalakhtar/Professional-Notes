# 3. Styling Your Components
There are two ways of styling your components:
1. By passing in a style object to the `style` attribute.
2. Specifying a css class on `className`.

# 5. What's happening in the background?
When we update the state, React updates the view.

React keeps a virtual representation of a UI in memory known as **virtual DOM**.

On initial page load, react creates virtual DOM v1.
When you update the state, react creates virtual DOM v2 using the updated state. It compares between v1 and v2. Then it synchronizes changes to the HTML page.

# 6. Props
Use props for values that remains constant throughout the lifetime of the component.
```jsx
function App() {
  return (
    <div className="App">
      <Counter by={10} />
    </div>
  );
}

function Counter({ by }) {
	const [count, setCount] = useState(0);

	return (
		<div className="Counter">
			<span className="count">{count}</span>
			<div>
				<button className="counterButton" onClick={() => setCount(count + by)}>+{by}</button>
				<button className="counterButton" onClick={() => setCount(count - by)}>-{by}</button>
			</div>
		</div>
	);
}
```

You can specify the type of properties using the `propTypes` key on the component name.
You can specify the default value of properties using the `defaultProps` key on the component name.

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)