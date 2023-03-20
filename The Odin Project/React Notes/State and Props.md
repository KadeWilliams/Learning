# Props
Props are one of the two major pillars of React, the very heart of what the framework was built on. 

In JSX where we implement our component, we also pass down a property. The property is then applied to the component and can be manipulated and in the same way normal HTML is applied. 

*IMPORTANT*: Make sure you pass `props` to the constructor of the child component as well as the `super()` method, otherwise you will not be able to access the prop in the child component.  

```js

import React, { Component } from 'react';

class MyComponent extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div>
        <h1>{this.props.title}</h1>
        <button onClick={this.props.onButtonClicked}>Click Me!</button>
      </div>
    );
  }
}


class App extends Component {
  constructor(props) {
    super(props);

    this.onClickBtn = this.onClickBtn.bind(this);
  }

  onClickBtn() {
    console.log('Button has been clicked!');
  }

  render() {
    return (
      <div>
        <MyComponent title="React" onButtonClicked={this.onClickBtn} />
      </div>
    );
  }
}

export default App;
```

First, there is `MyComponent` which is essentially the same except one key difference: `{this.props.onButtonClicked}` is assigned to the `onClick` event of the component. Essentially, what this means is:
- Our `MyComponent` component is expecting a prop to be passed into it named `onButtonClicked` 
- The value of that prop should be some kind of function
- We want this function to be attached to the click even of our button

**Special note 1**: In React, instead of using `addEventListener` to add event listeners, we assign them right in the JSX. Unlike adding them in HTML, these attributes must be camelCased!

**Special note 2**: Did you notice how the function `this.props.onButtonClicked` was wrapped in curly braces? This is because JSX needs a way of knowing when you are using JavaScript, otherwise it will try to transpile your code into HTML elements, text nodes, or strings (or throw an error in some cases). In this case, we are referring to a JavaScript object property, which technically qualifies as "using JavaScript", so we must wrap it in curly braces. 

The reason we have to bind the `this` keyword when passing a function to another component is that it needs to stay in the same context in which it was declared. Always remember: you **must** bind `this` for all methods in **class components** when passing them to other components. 

As you can see when you are passing many properties or functions to a component, it can get quite exhausting to always refer to them with `this.props.someProperty`. We can alternatively write the above as follows:

```javascript
import React, { Component } from 'react';

class MyComponent extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    const { title, onButtonClicked } = this.props;

    return (
      <div>
        <h1>{title}</h1>
        <button onClick={onButtonClicked}>Click Me!</button>
      </div>
    );
  }
}

export default MyComponent;
```

Here, we are destructuring `title` and `onButtonClicked` from `this.props`, which lets us refer to them with just their names. Make sure to destructure within the render method when using class components. In functional components, you would destructure outside of the return statement or inside the parameter parentheses of the functional component. 

# State 
The other main pillar of React is `state`. State is simply what we use to handle values that can change over time. For example, consider a very simple application that has a button and a counter. When the user clicks the button, the counter is incremented by 1. Since `count` will need to change on every click, we want to hold that value in `state`. 

```javascript
import React, { Component } from 'react';

class App extends Component {
  constructor() {
    super();

    this.state = {
      count: 0,
    };

    this.countUp = this.countUp.bind(this);
  }

  countUp() {
    this.setState({
      count: this.state.count + 1,
    });
  }

  render() {
    return (
      <div>
        <button onClick={this.countUp}>Click Me!</button>
        <p>{this.state.count}</p>
      </div>
    );
  }
}
```

In the above component, we declared our state as an object with a property `count` set to an initial value of `0`. You **always** declare state in the constructor of a class component. 

The `setState` method we call inside the `countUp` method sets the state to a new value. 

**IMPORTANT**: In React, state should be treated as **immutable**. This means you should **never** change state directly (i.e. without using `setState`) because it can lead to unexpected behavior or bugs. 

In other words, you should never do something like: `this.state.count = 3`. Instead, always use the setState method React provides to class components to modify the state. Keep this in mind - it can save you a lot of debugging when you are getting started with React. 

## What about passing state as a prop?
One of the greatest and most powerful features of React is the ability to pass one component's state down to multiple children. When that piece of state is changed, each child component that uses it is automatically re-rendered with the new value!


## State and props in functional components 
React provides the ability to create components as *functions* instead of classes. We call these functional components. In functional components, we don't pass `props` as an argument to the constructor, but instead just pass it as an argument to the component itself. Another major difference between functional and class components concerning props is the way you reference the props. We access props simply with: `props.someFunction`. That's the main difference with `props` between class and functional components. 

Using state in functional components is a bit different. Before the end of 2018, developers were not able to access state in functional components at all. Functional components were therefore just used for returning JSX logic with props. However, with the introduction of **React Hooks**, this changed. Now we can set and access state in functional components, and in the modern React landscape, they are often preferred over class components. 

It is recommended to name props from the component's own point of view rather than the context in which it is being used. 

A good rule of thumb is that if a part of your UI is used several times (Button, Panel, Avatar), or is complex enough on its own (App, FeedStory, Comment), it is a good condidate to be extracted to a separate component. 

### Props are Read-Only
Whether you declare a component as a function or a class, it must never modify its own props. 

```js
function sum(a, b) {
  return a + b;
}
```

The above function is called a "pure" function because it does not attempt to change it's input and always returns the same result for the same inputs. 

**All React components must act like pure functions with respect to their props**.

Components need to "remember" things: the current input value, the current image, the shopping cart. In React, this kind of component-specific memory is called *state*. 

1. Local variables don't persist between renders. When React renders a component a second time, it renders it from scratch - it doesn't consider any changes to the local variables. 
2. Changes to local variables won't trigger renders. React doesn't realize it needs to render the component again with the new data. 

To update a component with new data, two things need to happen:
1. Retain the data between renders. 
2. Trigger React to render the component with new data (re-rendering).

The `useState` Hook provides those two things:
1. A state variable to retain the data between renders. 
2. A state setter function to update the variable and trigger React to render the component again 

### Adding a state variable

To add a state variable, import `useState` from React at the top of the file: 
```js
import { useState } from 'react';

const [index, setIndex] = useState(0);
```

`index` is a state variable and `setIndex` is the setter function. 

This is how they work together in `handleClick`:
```js
function handleClick() {
	setIndex(index + 1);
}
```

### Meet your first Hook
In React, `useState`, as well as any other function starting with "use", is called a Hook.

*Hooks* are special functions that are only available while React is rendering. They let you "hook into" different React features. 

Hooks can only be called at the top leverl of your components or yoru own hooks. You can't call Hooks inside conditions, loops, or other nested funcitons. 

The only argument to `useState` is the initial value of your state variable. In this example, the `index`'s initial value is set to `0` with `useState(0)`. 

Every time your component renders, `useState` gives you an array containing two values:
1. The state variable (`index`) with the value you stored
2. The state setter function (`setIndex`) which can update the state variable and trigger React to render the component again. 

You can have as many state variables of as many types as you like in one component. 


It is a good idea to have multiple state variables if their state is unrelated. But if you find that you often change two state variables together, it might be easier to combine them into one. 

### State is isolated and private
State is local to a component instance on the screen. In other words, if you render the same component twice, each copy will have completely isolated state! Changing one of them will not affect the other. 

Unlike props, state is fully private to the component decalaring it. The parent component can't change it. This lets you add state to any component or remove it without impacting the rest of the components.

