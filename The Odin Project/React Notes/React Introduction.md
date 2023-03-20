# Why React? 
- Reusability of components
- Well supported due to its popularity
- React is not opinionated, which means that it won't force you to follow any specific design patterns, project organizational structure, or logic. It's all up to you 
- Smaller learning curve, especially when you already have a good grasp of JavaScript and HTML 

# Components
Applications built with React are made with (reusable) components. Components are your "building blocks". 

In React, each component is defined in an ES6 module. ES6 introduced the import statment which allows you to import code from one module to another. This allows us to write each component in a separate file and import all components into the parent component. 

```js
import React, { Component } from 'react'

class App extends Component {
	constructor() {
		super()
	}

	render() {
		return (
			<div className="App">
				Hello World!
			</div>
		)
	}
}
export default App
```

JSX is an HTML-lik syntax that is transpiled (or converted) into JavaScript so a browser is able to process it. One of the primary characteristics and features of React is the ability to combine JavaScript and JSX. One thing to know about JSX is that you can't use some JavaScript protected words as html attributes anymore, such as `class`, `onchange`, or `for`. All attributes are written in camelCase and some have their names changed completely to avoid the transpiler getting too confused about whether you're assigning a label `for` an input. 

Using class components is one of two ways of defining components in React. The other, more modern, approach is to define the component as a function (like a factory function). 

A basic functional component looks something like this: 

```js
import React from 'react';

function App() {
	return <div className="App">Hello World!</div>;
}

const App = () => {
	return <div className="App">Hello World!</div>;
};

const App = () => <div className="App">Hello World!</div>;

export default App; 
```

As you can see, there are a few differences between functional and class components.  

With functional components: 
1. We don't have to import and extend "Component" from React.
2. We don't need a constructor
3. We don't need the render funciton, instead we put the return statement right at the end of the  function body

A component is a reusable chunk of code. Components make it possibel to divide any UI into dependent, reusable pieces and think about these pieces in isolation. Just like a pure function, a component should ideally do just one thing

## Quick Start (React Docs) 

### Creating and nesting components
React apps are made out of *components*. A component is a piece of the UI that has its own logic and appearance. A component can be as small as a button, or as large as an entire page. 

```js
function MyButton() {
	return (
		<button>I'm a button</button>
	)
}
```

Now that you've declared `MyButton`, you can nest it into another component:
```js
export default function MyApp() {
	return (
		<div>
			<h1>Welcome to my app</h1>
			<MyButton />
		</div>
	)
}
```

React component names must always start with a capital letter, while HTML tags must be lowercase. 

The `export default` keywords specify the main component in the file.

### Writing markup with JSX
The markup syntax is called *JSX*. 

JSX is stricter than HTML. You have to close tags. Your component also can't return multiple JSX tags. You have you wrap them into a shared parent, like a `<div>...</div>` or an empty `<>...</>` wrapper

### Adding styles 
In React, you specify a CSS class with `className`. It works the same way as HTML `class` attribute

Then you write the CSS rules for it in a separate CSS file.

React does not prescribe how you add CSS files. In the simplest case, you'll ad a `<link>` tag to your HTML. 

### Displaying data
JSX lets you put markup into JavaScript. Curly braces let you "escape back" into JavaScript so you can embed some variable from your code and display it to the user. 

```js 
return (
	<h1>
		{user.name}
	</h1>
)
```

### Responding to events
You can respond to events by declaring *event handler* funcitons inside your components:
```js
function MyButton() {
	function handleClick(){
		alert("You clicked me!")
	}
	return (
	<button onClick={handleClick}>
		Click me
	</button>
	)
}
```
Notice how `onClick={handleClick}` has no parentheses at the end! Do not *call* the event handler function: you only need to pass it down. React will call your event handler when the user clicks the button.

## Passing Props to a Component 
React components use *props* to communicate with each other. Every parent component can pass some information to its child components by giving them props. Props might remind you of HTML attributes, but you can pass any JavaScript value through them, including objects, arrays and functions.

### Familiar props
Props are the information that you pass to a JSX tag. 

The props you can pass to an `<img>` tag are predefined. But you can pass any props to your own components, to customize them. 

### Passing props to a component
In this code, the `Profile` component isn't passing any props to its child component, `Avatar`:
```js
export default function Profile() {
	return (
		<Avatar />
	);
}
```
You can give `Avatar` some props in two steps.
#### Step 1: Pass props to the child component
First, pass some props to `Avatar`. 
```js
export default function Profile() {
	return (
		<Avatar
			person={{name: 'Lin Lanying', imageId: '123456'}}
			size={100}
		/>
	);
}
```
Now you can read these props inside the `Avatar` component

#### Step 2: Read props inside the child component
You can read these props by listing their names `person, size` separated by the commas inside ({}) directly after `function Avatar`. This lets you use them inside the `Avatar` code, like you would with a variable.

```js
function Avatar({person, size}) {
	// person and size are available here
}
```

Add some logic to `Avatar` that uses the `person` and `size` props for rendering, and you're done. 

Props let you think about parent and child components independently. You can change `person` or the `size` props inside `Profile` without having to think about how `Avatar` uses them. Similarly, you can change how the `Avatar` uses these props, without looking at the `Profile`. 

You can think of props like "knobs" that you can adjust. They serve the same role as arguments serve for functions -- in fact, props *are* the only argument to your component! React component functions accept a single argument, a `props` object: 
```js
function Avatar(props) {
	let person = props.person;
	let size = props.size;
}
// but this is better... this is destructuring 
function Avatar({person, size}) {
	//... 
}
```

#### Specifying a default value for a prop
If you want to give a prop a default value to fall back on when no value is specified, you can do it with the destructuring by putting `=` and the default value right after the parameter:
```js 
function Avatar({person, size = 100}) {
	//...
}
```

#### Forwarding props with the JSX spread syntax
Sometimes, passing props gets very repetitive:
```js
function Profile(props) {
	return (
		<div className="card">
			<Avatar {...props} />
		</div>
	);
}
```
This forwards all of `Profile`'s props to the `Avatar` without listing each of their names.

**Use spread syntax with restraint**. If you're using it in every other component, something is wrong. Often, it indicates that you should split your components and pass children as JSX.

#### Passing JSX as children
```js
<Card>
	<Avatar />
</Card>
```

When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children`. 

The parent tag doesn't need to know what's being rendered inside of it. You can think of a component with a `children` prop as having a "hole" that can be "filled in" by its parent components with arbitrary JSX. You will often use the `children` prop for visual wrappers: panels, grids, etc.


