# Quick Overview of Basics

### Definition:

A react app is completely made up of components. 

> A component is a piece of the UI that has its own logic and appearance

A component can have other components as well. Thus it can either a simple button OR an entire page. The only rule is that it should have its own logic and appearance.

This was the concept of a component, but in technical terms: "A component is just a JS function that returns markup / HTML". This function tells react what to render. 

The only caveat is that a components name should always start with a 'capital letter'.

```jsx
export function TestComponent() {
	return (
		<div>
			Something to render here!
			<Some other Component/>
		</div>
	)
}
```

A file can export multiple components if it wants to and there can even be 1 default component that a file exports. But as a good practice, it is better for 1 file to have 1 component. Also a component can have multiple components in it.

## JSX
The markup that we return from the component is called JSX. It is a stricter version of HTML that needs all tags to be closed and each component can return only a single JSX tag. In things cases where we want to have multiple tags to be returned, we wrap them in a dummy tag like : `<>...</>`

To style the components, we can give the components a class and then use those classes to add styles in a separate `scss` or `css` file. 
### Making the JSX Dynamic
In JSX, we can insert variables or logic using `{}`. Anything inside them is considered and evaluated like normal javascript.
These curly braces can be used to display variables, return a list of elements, conditionally render elements etc. 

### Conditional Rendering
There can be multiple ways to conditionally render:
1. Using a variable and then having multiple returns:
```jsx
let content;  

if (isLoggedIn) {  
content = <AdminPanel />;  
} else {  
content = <LoginForm />;  
}  

return (  
<div>  
{content}  
</div>  
);
```

2. Or we can use a conditional operator for this and directly render within one return statement
```jsx
<div>  
	{isLoggedIn ? (  
	<AdminPanel />  
	) : (  
	<LoginForm />  
	)}  
</div>

OR

<div>  
{isLoggedIn && <AdminPanel />}  
</div>
```

> We can use a similar process handle on-click functionalities etc. 