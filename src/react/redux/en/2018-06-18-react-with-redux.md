Readux with React
=================

Intro
-----

Redux can be user with:

- React apps
- Vue apps
- Plain HTML apps
- Vanilla JavaScript apps

In this lesson we will look at how we could hook Redux up to an app that uses Rects fot its UI.

React as UI
-----------

It´s time to move the TODO application being plain HTML and convert it to being powered by React. To do that, we will need to add:

- react
- react-dom
- babel

The changes we've just implemented should look pretty familiar - they were just converting parts of our app from HTML to being powered by React Components.

### Combining React and Redux

Alrighty, so you've learned React. You've built Redux and used it in a regular HTML application. But now we've started converting that HTML to a React application. It is time to connect the _React Components_ to the _Redux Store_.

It is important to notice two things:

- Where the `store.dispatch()` code goes in the React component
- How the React component is passed the Redux store as a prop

An important detail is that in the `index.html` app we have two TODO/GOALS applications:

1. The app build in Vanilla JavaScript
2. The app build in React

Both apps share the same redux store. It is the reason for the behavior that we have until now, that every time we add an item to the list from the React app, the changes are reflected in the Vanilla JavaScript app.

Another topic in the stage of the React is the DOM handling. In the Vanilla JavaScript app, when we called the `store.subscribe` to be notified whenever the store changed and whenever it did change, what we did was we got new _goals_, and the new _todos_, and then for each of those items, we added them to the DOM.

With React, you don't really have to do any of the DOM stuff, because React is really handling all of that for us behind the scenes. However, we still want to `subscribe()` to the store inside our React component to re-render our components. To re-render a component in React we should call the `setState()` to tell React at the state of the app changes, then we will update the UI. In this case, we don't have any state inside this component and it doesn't make sense to add any jus to cause a re-render. So we can do is use an anti-pattern calling the `this.forceUpdates()` React Component method to re-render that specific component without set the state.

### Summary

n this section, we converted our plain HTML application to one using React Components. We didn't implement any new features. Instead, we just improved the organization of the code by breaking out separate parts into reusable chunks.