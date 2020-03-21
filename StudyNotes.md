## Introduction 

This project was created using the help of the tutorial provided by `reactjs`, and was meant as a starter project introducing me to many of the functionalities provided by React. 


## Comparison of Angular vs React 

This is a topic that comes up a lot so I thought I would make some docs regarding the two javascript frameworks. 

React is a Facebook managed Javascript library for UI development meanwhile Angular is a Google managed framework which is TypeScript-based. 


## Setup

1. Ensure the latest version of `Node.js` is installed 
2. Run the following command for Create React App to make a new project 
```
npx create-react-app my-app
```
3. Delete all files in the `src/` folder of the new project - we will be replacing the default source files for this project in later steps 
```bash
cd my-app 
cd src
rm -f *
cd .. 
```
4. Add a file called `index.css` in the src folder 
5. Add a file called `index.js` in the src folder 
6. Add in the code and the following three lines at the top of `index.js` in the src folder: 

```
import React from 'react'; 
import ReactDOM from 'react-dom'; 
import './index.css'; 
```

## Overview 

Now that we have the setup complete let's go over an overview of React. 

### What is React? 

React is a declartive, efficient, and flexible JavaScript library for building user interfaces. It lets users compose complex User Interfaces from small and isolated pieces of code called **components**. 

React has many different types of components but we will start with `React.Component` subclasses. 

```javascript
class ShoppingList extends React.Component {
    render(){
        return {
            <div className="shopping-list">
                <h1>Shopping List for {this.props.name}</h1>
                <u1>
                    <li>Instagram</li>
                </u1>
            </div>
        };
    }
}

// usage <ShoppingList name="Mark"/>
```

We use components to tell React what we want to see on the screen. When our data changes, React will efficiently update and re-render our components. 

In this case ShoppingList is a **React component class / React component type**. A component takes in parameters, called **props** (properties) and returns a hierarchy of views to display via the `render` method. 


The `render` method returns a description of what you want to see on the screen. React takes the description and displays the result. In particular, `render` returns a **React element** which is a lightweight description of what to render. Most React developers use a special syntax called `JSX` which makes these structures easier to write. The `<div />` syntax is transformed at build time to `Reach.createElement('div')`. The above example is equivalent to: 

```js
return React.createElement('div', {className: 'shoppping-list'}),
    React.createElement('h1', /* ... h1 children ... */),
    React.createElement('u1', /* ... u1 children ... */)
);
```

`createElement()` is described in more detail in the API reference, but we will continue using JSX in this project. 

JSX comes with the full power of JavaScript. Any JavaScript expressions can be placed within braces inside JSX. Each React element is a JavaScript object that can be stored in a variable or passed around in the program. 


The `ShoppingList` component above only renders built-in DOM components like `<div />` and `<li />` but the developer can compose and render custom React components too. For example, we can now refer to the whole shopping list by writing `<ShoppingList />`. Each React component is encapsulated and can operate independently; this allows users to build complex UIs from simple components. 


## Inspecting the Starter Code 

We will not be worrying about the CSS styling in this project and will instead be focusing on the React content of `src/index.js`. 

When inspecting the code you will notice that there are three React compenents: 

* Square 
* Board 
* Game 


```javascript 
// src/index.js 
import React from 'react'; 
import ReactDOM from 'react-dom'; 
import './index.css';


class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {/* TODO */}
      </button>
    );
  }
}

class Board extends React.Component {
  renderSquare(i) {
    return <Square />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(
  <Game />,
  document.getElementById('root')
);
```


The Square component renders a single `<button>` and the Board renders 9 squares. The Game component renders a board with placeholder values which will be modified later. There are currently no interactive components. 




## Passing Data Through Props 

To get started we will try passing some data from the Board component to the Square component. 

In Board's `renderSquare` method, we will be changing the code to pass a prop called `value` to the Square: 


```javascript
class Board extends React.Component {
    renderSquare(i){
        return <Square value={i} />;
    }
}
```

Change Square's render method to show this value: 

```javascript
class Square extends React.Component {
    render() {
        return (
            <button className="square">
                {this.props.value}
            </button>
        ); 
    }
}
```

Now when we run `npm start` we should see a number in each square in the rendered output. 

At this point we have completed "passing a prop" from a parent Board component to a child Square component. Passing props is how information flows in React apps, from parents to children. 



## Making an Interactive Component 

Now let's make it so that the Square component is filled with an "X" when we click it. First, change the button tag that is returned from the Square component's `render()` function: 

```javascript 
class Square extends React.Component { 
    render(){
        return (
            <button className="square" onClick=
            {function() { alert('click'); }}>
                {this.props.value}
            </button>
        );
    }
}
```

Now if we click on a Square, an alert should pop up in the browser. 

To save typing and avoid the confusing behavior of this, we will be using the arrow function syntax for event handlers: 

```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={()=>alert('click')}>
        {this.props.value}
      </button>
    )
  }
}
```

Notice that with `onClick={()=> alert('click')}` we are passing a function as the onClick prop. React will only call this function after a click. Forgetting `() =>` and writing `onClick={alert('click')}` is a common mistake and would fire the alert every time the component re-renders. 


For the next step, we want the Square component to "remember" that it got clicked, and fill it with an "X" mark. To "remember" things, components use `state`. 


React components can have state by setting `this.state` in their constructors. `this.state` should be considered as private to a React component that it's defined in. Let's store the current value of the Square in `this.state`, and change it when the Square is clicked. 

First we will add a constructor to the class to initialize the state: 


```javascript
class Square extends React.Component {
  constructor(props) {
    super(props); 
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

Note: In JavaScript classes, you always need to call `super` when definining the constructor of a subclass. All React component class that have a `constructor` should start it with a `super(props)` call. 


Now we will change the Square's render method to display the current stat's value when clicked: 

- replace `this.props.value` with `this.state.value` inside the `<button>` tag
- replace the `onClick={}` event handler with `onClick={() => this.setState({value: 'X'})}
- put the `className` and `onClick` props on separate lines for better readability. 


After these changes, the `<button>` tag that is retured by Square's `render` method looks like this: 


```javascript
class Square extends React.Component {
  constructor(props){
    super(props); 
    this.state = {
      value: null, 
    };
  }

  render() {
    return (
      <button
        className = "square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    ); 
  }
}
```

By calling `this.setState` from an `onClick` handler in the Square's `render` method, we tell React to re-render the Square whenever its `<button>` is clicked. After the update, the Square's `this.state.value` will be `'X'`, so we will see `X` on the game board. If you click on any Square, an `X` would show up. 

When `setState` is called in a component, React will automatically update the child components inside of it as well. 


# Developer Tools 

The React Devtools extension for Chrome allows developers to inspect a React component tree using the browser's developer tools. 

Functionalities are provided for seeing the props and the state of React components. 

After installing React DevTools, you can click "Inspect" on any element of the page bringing up the developer tools and the `React tabs` should appear as the last tab on the right. We can use "Components" to inspect the component tree. 


# Game Logic 

Now we have the basic building blocks for the tic-tac-toe game. To have a complete game, we need to alternate placing "X"s and "O"s on the board as well as add logic for determining a winner. 



## Lifting State Up 

Currently, each Square component maintains the game's state. To check for a winner, we will maintain the value of each of the 9 squares in one location.


One initial approach would be to get the Board to ask each Square for the Square's state. This approach is possibe but is discouraged because the code becomes difficult to understand, susceptible to bugs and hard to refactor. 

Instead, the ideal approach would be to **store the game's state in the parent Board component** instead of in each Square. The Board component can tell each Square what to display by passing a prop, just like we did when we passed a number to each Square. 


> To collect data from multiple children or to have child components communicate with each other, we need to **declare the shared state in the parent component instead**. The parent component can pass the state back down to the children using props - keeping the child components in sync with each other and with the parent component. 


Lifting state into a parent componet is common when React components are refactored - let's take this opportunity to try it out. 


Add a constructor to the Board and set the Board's initial state to contain an array of 9 nulls corresponding to the 9 squares: 

```javascript
class Board extends React.Component {
  constructor(props) {
    super(props); 
    this.state = {
      squares: Array(9).fill(null), 
    }; 
  }

  renderSquare(i) {
    return <Square value={i}>
  }
}
```

When the board is filled in later, the `this.state.squares` array will look something like the following: 


```javascript 
[
  'O', null, 'X', 
  'X', 'X', 'O', 
  'O', null, null, 
]
```

The Board's `renderSquare` method currently looks like this: 

```javascript
renderSquare(i) {
  return <Square value={i} />;
}
```

At the beginning, we passed the prop down from the Board to show the numbers 0-8 in each square. After that we replaced the numbers with an "X" mark determined by the Square's ow state - this causes the Square to ignore the `value` prop passed to it by the Board. 


We will now use the prop passing mechanism - the Board will be modified to instruct each individual Square about its current value (`X`, `O`, or `null`). We have already defined the `squares` array in the Board's constructor, and we will modify the Board's `renderSquare` method to read from it: 


```javascript 
renderSquare(i) {
  return <Square value={this.state.squares[i]} />;
}
```


Each Square will now receive a `value` prop that will determine how it will be filled. 


Next we need to change what happens when a Square is clicked. The Board component now maintains which squares are filled. We need to create a way for the Square to update the Board's state. Since state is considered to be private to a component that defines it, we cannot update the Board's state directly from Square. 


Instead, we pass down a function from the Board to the Square and we will have the Square call that function when a square is clicked. We will change the `renderSquare` method in Board to: 


```javascript
renderSquare(i) {
  return (
    <Square
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
    />
  ); 
}
```


> splitting the returned element into multiple lines for readability, and parentheses so that JavaScript doesn't insert a semicolon after `return` and break our code. 


Now we are passing down two props from Board to Square: `value` and `onClick`. The `onClick` prop is a function that Square can call when clicked. We wil lmake the following changes to Square: 

- replace `this.state.value` with `this.props.value` in Square's `render` method 
- replace `this.setState()` with `this.props.onClick()` in Square's `render` method 
- Delete the `constructor` from Square because Square no longer keeps track of the game's state
 

 After these changes, the Square component looks like this: 

 ```javascript
class Square extends React.Component {
  render() {
    return (
      <button
        className="square"
        onClick={() => this.props.onClick()}
      >
        {this.props.value}
      </button>
    );
  }
}
 ```

When a Square is clicked, the `onClick` function provided by the Board is called. Here is a review of how this is achieved: 


### Board Click FLow 

1. The `onClick` prop on the built-in Dom `<button>` component tells React to set up a click event listener 
2. When the button is clicked, React will call the `onClick` event handler that is defined in Square's `render()` method
3. This event handler calls `this.props.onClick`. The Square's `onClick` prop was specified by the Board. 
4. Since the Board passed `onClick={() => this.handleClick(i)}` to Square, the Square calls `this.handleClick(i)` when clicked
5. We have not defined the `handleClick()` method yet but that will be handled in the following step 



> The DOM `<button>` element's `onClick` attribute has a special meaning to React because it is a built-in component. In React it is conventional to use `on[Event]` names for props which represent events and `handle[Event]` for methods which handle the events


We will now add the `handleClick` function to the Board class: 


```javascript 
class Board extends React.Component {
  //...
  handleClick(i) {
    const squares = this.state.squares.slice(); 
    squares[i] = 'X';
    this.setState({squares: squares});
  }
  //...
}
```

After these changes, we are able to click on the Squares to fill them, the same as we had before. However, this time the state is stored in the Board component instead of in the individual Square components. When the Board state changes, the Square components re-render automatically. Keeping the state of all squares in the Board component will allow it to determine the winner in the future. 

Since the Square components no longer maintain state, the Square components receive values from the Board component and inform the Board component when they are clicked. In React terms, the Square components are now **controlled components**. The Board has complete control over them. 


> Note how in `handleClick`, we call `.slice()` to create a copy of the squares array to modify instead of modifying the existing array. This is important and will be discussed in the following section 



## Why Immutability is Important 

In the previous code example, it was suggested that the `.slice()` method would be used to create a copy of the `squares` array to modify instead of modifying the existing array - this is the concept of immutability and it is very important in React. 


There are generally two approaches to changing data. The first is to `mutate` the data by directly changing the data's values. The second approach is to replace the data with a new copy which has the desired changes. 


```javascript 
var player = {score: 1, name: 'Jeff'};
// with Mutation 
player.score = 2; 
// without Mutation 
var newPlayer = Object.assign({}, player, {score: 2}); 
```

The end result remains the same but by not mutating or changing the underlying data directly we gain the following benefits: 


### Complex Features Become Simple

Immutability makes complex features much easier to implement. Features such as time travel and moving to previous states becomes easy as avoiding direct data mutation allows us to keep previous versions of the game's history intact, and reuse them later. 


### Detecting Changes

Detecting changes in mutable objects is difficult because it requires to mutable object to be compared to previous copies of itself and the entire object tree to be traversed. 

Detecting changes in immutable objects is considerably easier. If the immutable object that is being referenced is different than the previous one, then we know that the object has changed. 


### Determining when to Re-Render in React 

The main benefit of immutability is that it helps you build pure components in React. Immutable data can easily determine if changes have been made which helps to determine when component requires re-rendering. 

Creating pure components allows us to optimize the front-end performance of React. 



# Function Components

We will now change the Square to be a **function component**. 

In React, **function components** are a simpler way to write components that only contain a `render` method and don't have their own state. Instead of defining a class which extends `React.Component`, we can write a function that takes `props` as input and returns what should be rendered. Function components are less tedious to write than classes and many components can be expressed in this way. 


We can replace the square class with this function: 

```javascript
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  ); 
}
```

We have changed `this.props` to `props` both times it appears 


# Taking Turns 

Now we need to fix the obvious defect which is that `O`'s cannot currently be marked on the board. 

We will set the first move to be `X` by default. We can set this default by modifying the initial state in our Board constructor: 

```js 
class Board extends React.Component {
  constructor(props){
    super(props);
    this.start = {
      squares: Array(9).fill(null), 
      xIsNext: true, 
    }; 
  }
}
```

Each time a player moves, `xIsNext` (boolean) will be flipped to determine which player goes next and the game's state will be saved. We will update the Board's `handleClick` function to flip the value of `xIsNext`: 

```js 
handleClick(i) {
  const squares = this.state.squares.slice(); 
  squares[i] = this.state.xIsNext ? 'X' : 'O'; 
  this.setState({
    squares: squares, 
    xIsNext: !this.state.xIsNext, 
  });
}
```

With this change, `X`'s and `O`'s can take turns. Let's also change the status text in Board's `render` so that it displays which player has the next turn: 

```js 
render() {
  const status = 'Next player: ' + 
  (this.state.xIsNext ? 'X' : 'O'); 

  return (
    ...
  )
}
```



