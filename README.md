![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)

# React State

In React, we can't change the values in props object. They are meant to be **immutable**. So, if we can't change the value in a props object how do we update our page? The answer is: **State**.

## Learning Objectives

- Review passing data to a React component via props.
- Identify state in a React app.
- Modify the state of a React component through events.
- Distinguish container and presentational components.

## Set Up

1. Change into your sei directory with `cd ~/sei/sandbox`
1. Once you're in the **sandbox** directory, type `npx create-react-app react-state` and press enter.
1. After it's completed and you're returned to the prompt, type `cd react-state` to switch into the project directory.
1. Open the application in VS Code by typing `code .`.
1. In VS Code, click the run button next to the **start** script in the NPM SCRIPTS area of the Explorer pane **_or_** from the Terminal, type `npm start` to launch the development server.

## Framing

In computer science terms, we descibe systems as stateful if they are designed to remember preceding events or user interactions; the remembered information is called the **state** of the system.

So far in this program, we used an imperative model of programming. React uses a declarative style of programming. **_Declarative programming_** is like describing a picture, whereas **_imperative programming_** is like a set of instructions for painting that picture. React enables us to design views for each **state** in our application, and it handles efficiently updating and rendering views when our data changes.

With React, we give up the control of changing the user interface (UI) to React. Let's consider an example in vanilla JavaScript:

```js
if (gameOver === true) {
  const restart = document.querySelector('#restart');
  restart.style.display = 'inline-block';
} else {
  restart.style.display = 'none';
}
```

In React, we describe how React should render the UI based on the current data and conditions that exist:

```jsx
// ...
return (
  {gameOver ? <button onClick={restartGame}>Restart</button> : null}
)
```

The game being over is one _state_ our game can be in. We know it's in this state when the `gameOver` variable evaluates to `true`. In that case, we want React to render the button. If the game isn't over, we can render something else or nothing at all.

## Changing the State

Up to now, we've seen that we can use props to pass data into components. The components in turn, can use that data when rendering content to the page. What we haven't seen is how to make the pages dynamically change based on user interactions or some other event. Since the props object cannot be changed in React once it's rendered, our app has been completely static.

To make our app dynamic we need to designate some data as state because the only way to tell React to update the page is to change the data that's in state. Any components that are dependent upon state data &mdash; including any props that are passed state data &mdash; will be rerendered automatically if the state they are dependent upon changes.

![Diagram showing how state changes cause components to rerender](https://media.git.generalassemb.ly/user/17300/files/39d10a00-0f85-11eb-91a2-afabd530b7c8)

## Class-based vs. Function Components with Hooks

In order to designate some data as state in a React component, there are two different syntaxes that can be used:

1. ES6 **class** syntax
1. Function components with hooks

There are differences in the way each of these behaves and looks. Originally when React was created there was only class syntax. Over the years, as React became more comprehensive and as the base grew, some challenges with this syntax became more evident. For one, class syntax was harder to test using test automation tools that existed in the industry. Many people also argued that it was harder for humans to reason about class-based components, particularly since classes themselves were new to Javascript. As a result, the React team and community rethought the approach and came up with **Hooks** as an alternative.

Hooks were introduced in version 16.8 in February 2019. Today, much of the new code that is written in React uses hooks, but there's **a lot** of legacy code (and tutorials) that still exists using the class syntax, and some people still prefer to write their new code using it. We're going to focus on hooks in this program because most people find it a lot easier to work with and learn.

### We Do: Add State to a Component

Let's start by everything in App.js with the following:

```jsx
import React from 'react';

function App() {
  return (
    <main>
      <button>Toggle State</button>
    </main>
  );
}

export default App;
```

We should now have a very basic component that renders a button to the page.

Next, we're going to use the useState Hook to designate a piece of data as part of our application's state. To use this method (that's all a Hook is a method in the React class), we need to import it. We're going to also be importing it from the `react` package in our `node_modules` folder, so we'll change the first line of the App.js file to read:

```js
import React, { useState } from 'react';
```

This syntax is called **destructuring**. Destructuring is a cool JavaScript feature that let's us name parts of an array or object so that we don't have to use the dot notation. Let's take a quick look at it by watching this quick [video](https://www.youtube.com/watch?v=G4T2ZgJPKbw).

The way that we'll use the `useState` method in our code will also use destructuring. In this case, we'll be destructuring an array that is returned from useState. This array contains two elements. The first is the actual state data and the second is a method that allows us to change the data. Let's see the pattern:

```jsx
const [data, setData] = useState(initialState);
```

You can name the elements of the array anything that you like but it's convention to give the data element an intuitive and descriptive name as we have always done with our variables, and name the second element, the same as the first but with `set` prepended to the name. For example:

```jsx
const [movies, setMovies] = useState([]);

const [gameOver, setGameOver] = useState(false);

const [friends, setFriends] = useState([{ name: 'Tabitha' }, { name: 'Esin' }]);
```

We're going to only have one variable in our state right now, but you can add as many pieces of state as you need to a component.

Add the following line to the `App` function **before** the `return`:

```jsx
import React, { useState } from 'react';

function App() {
  // Add state for showing:
  const [showing, setShowing] = useState(true);

  return (
    <main>
      <button>Toggle State</button>
    </main>
  );
}

export default App;
```

Cool! Now we have some state, but how can we tell? There's an app for that... Install the [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) from the Chrome Extension Store.

## Listening for User Interactions

In the past when we wanted to listen for an event, we created an event listener and attached it to an element in the DOM. Remember though, we're not interacting directly with the DOM in React. That's React's job.

With React's declarative programming model, when we want to listen for an event on an element we'll just attach the event handler directly to the element. Let's see the pattern:

```jsx
function AlertButton() {
  // Add the onClick and give it a callback function
  return <button onClick={() => alert('You clicked me!')}>Click me</button>;
}
```

We can find a comprehensive list of all of the events React supports in the [docs](https://reactjs.org/docs/events.html#supported-events).

Let's have our button toggle the showing state when it's clicked. How might we do that?

<details>
    <summary>Solution</summary>
```js
<button onClick={() => setShowing(!showing)}>Toggle State</button>
```
</details>

## Setting State from a Child Component

So this is really cool, but it'd be so cool if we could filter the movies when a user clicks on the **Must See Movies** in the Header component.

We can do that by passing the `filterMovies` method to the Header component as a prop.

```jsx
<Header filterMovies={this.filterMovies} />
```

Now in our Header component let's use the method.

```jsx
function Header(props) {
  return (
    <header>
      <h1>Reelz: The Movie App</h1>
      <Welcome name="Jen" firstTime={false} />
      <div>
        <ul>
          <li>Now Playing</li>
          <li onClick={props.filterMovies}>Must See Movies</li>
        </ul>
      </div>
    </header>
  );
}
```

## You Do: Show All Movies

Create a function that uses `setState()` to set the movies property in state back to the movies that we have in memory.

Test it inside of App first by making the "Click me" button reset the movie list. Then, once it's working pass it as a prop to the Header component and make it so that when a user clicks on the "Now Playing" link, it displays all of the movies again.

## Showing and Hiding Elements

Let's make a toggle so that we can show or hide our movie list. We need to create a new bit of state for this, so add to our state object in App:

```jsx
this.state = {
  movies: movies,
  showMovieList: true,
};
```

Now we'll wrap our Movies component in a JavaScript expression.

```jsx
{
  this.state.showMovieList && <Movies movies={this.state.movies} />;
}
```

Check the browser to see if anything changed. Now manually change the value of the `showMovieList` property to false and check the browser again.

## You Do: Show and Hide Toggle

Create a method called `toggleMovies` so that it updates the state of `showMovieList` by toggling the value between `true` and `false`.

## Additional Resources

- [Visual Guide to State in React | Dave Ceddia](https://daveceddia.com/visual-guide-to-state-in-react/)
- [Component State | Codecademy](https://www.codecademy.com/courses/react-101/lessons/this-state)
- [React State | React Docs](https://reactjs.org/docs/state-and-lifecycle.html)
- [Thinking in React | React Docs](https://reactjs.org/docs/thinking-in-react.html)
- [Imperative vs. Declarative Javascript](http://www.tysoncadenhead.com/blog/the-state-of-javascript-a-shift-from-imperative-to-declarative#.VxgGxZMrKfQ)
- [Styling in React](http://survivejs.com/webpack_react/styling_react/)

## [License](LICENSE)

All content is licensed under a CC­BY­NC­SA 4.0 license.

All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
