## Introduction 

This project was created using the help of the tutorial provided by `reactjs`, and was meant as a starter project introducing me to many of the functionalities provided by React. 


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

