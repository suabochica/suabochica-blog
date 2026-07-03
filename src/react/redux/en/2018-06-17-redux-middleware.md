Redux Middleware
================

Intro
-----

So far, we have the next three concepts as the foundation of Redux:

- Store
- Actions
- Reducers

In this lesson we will deep into Redux Middleware. Check how it allows us to hook into the Redux lifecycle and why that is beneficial.

To start the review of Redux Middleware, imagine that you want to add as a to do or a goal the task of invest in Bitcoin. However, your financial adviser keeps insisting that is a bad idea. So we decide to add a new feature to our To Do app that whenever we add a item or a new goal that contains the 'Bitcoin' word, then instead of adding it to the individual list the app will alert that is a bad idea add the item.

To achieve this description, we should hook into the moment after an action is dispatched, but before it ever hits the reducer and modifies the state. Then add the momento of create the store we will add the next function:

```js
function checkAndDispatch (store, action) {
    if (
        action.type === ADD_TODO &&
        action.todo.name.toLowerCase().includes('bitcoin')
    ) {
        return alert("Nope, that's a bad idea");
    } if (
        action.type === ADD_GOAL &&
        action.goal.name.toLowerCase().includes('bitcoin')
    ) {
        return alert("Nope, that's a bad idea");
    }

    return store.dispatch(action);
}
```

Finally, we have to use `checkAndDispatch()` function instead of the regular `dispatch()` function. With this changes, we can't add items to the lists that contains the 'Bitcoin' word.

Native Redux Middleware
-----------------------

You’ve learned how Redux makes state management more predictable: in order to change the store’s state, an action describing that change must be dispatched to the reducer. In turn, the reducer produces the new state. This new state replaces the previous state in the store. So the next time `store.getState()` is called, the new, most up-to-date state is returned.

Between the dispatching of an action and the reducer running, we can introduce code called **middleware** to intercept the action before the reducer is invoked. The Redux docs describe middleware as:

> A third-party extension point between dispatching an action, and the moment it reaches the reducer.

What's great about middleware is that once it receives the action, it can carry out a number of operations, including:

- producing a side effect (e.g., logging information about the store)
- processing the action itself (e.g., making an asynchronous HTTP request)
- redirecting the action (e.g., to another piece of middleware)
- dispatching supplementary actions

... or even some combination of the above. Middleware can do any of these _before_ passing the action along to the reducer.

Let's replace our `checkAndDispatch()` function with the real Redux middleware function.

```js
const checker = (store) => (next) => (action) => {
    if (
        action.type === ADD_TODO &&
        action.todo.name.toLowerCase().includes('bitcoin')
    ) {
        return alert("Nope, that's a bad idea");
    } if (
        action.type === ADD_GOAL &&
        action.goal.name.toLowerCase().includes('bitcoin')
    ) {
        return alert("Nope, that's a bad idea");
    }

    return next(action);
}

const store = Redux.createStore(Redux.combineReducers({
    todos,
    goals
}), Redux.applyMiddleware(checker));
```

### Where Middleware Fits
The way we had to structure our code originally, our `checkAndDispatch()` function _had_ to run _before_ `store.dispatch()`. Why is this? Because when `store.dispatch()` is invoked, it immediately calls the reducer that was passed in when `createStore()` was invoked. If you remember back to the first lesson, this is what our `dispatch()` function looked like:

```js
const dispatch = (action) => {
    state = reducer(state, action);
    listener.forEach((listener) => listener());
}
```

So you can see that calling `store.dispatch()` will immediately invoke the `reducer()` function. There's no way to run anything in between the two function calls. So that's why we had to make our `checkAndDispatch()` so that we can run verification code before calling `store.dispatch()`.

However, this is not maintainable. If we wanted to add another check, then we would need to write _another_ preceding function, that then call `checkAndDispatch()` that _then_ `store.dispatch()`. Not maintainable at all.

With Redux's middleware feature, we can run code _between_ the call to `store.dispatch()` and `reducer()`. The reason this works, is because Redux's version of `dispatch()` is more sophisticated than ours was, and becase we provide the middleware functions when we create the store:

```js
const store = Redux.createStore(<reducer-function>, <middleware-function>)
```

Redux's `createStore()` method takes the reducer function as its first argument, but then it can take a second argument of the middleware functions to run. Because we set up the Redux store with knowledge of the middleware function, it runs the middleware function between `store.dispatch()` and the invocation of the reducer.

### Applying Middleware
The `applyMiddleware()` function is an optional argument into `createStore()`. Their signature is:

    applyMiddleware(...middlewares)

Note the spread operator on the `middlewares` parameter. This means that we can pass in as many different middleware as we want!. Middleware is called in the order in which the were provided to `applyMiddleware()`.

We currently have the `checker` middleware applied to our app. Lets add a new `logger` middleware.

> ### Function Returning Functions
> Redux middleware leverages a concept called **higher-order functions**. A higher-order function is a function that either:
> - Accepts a function as an argument
> - Returns a function
>
> Higher-order functions are a powerful programming technique that allow functions to be significantly more dynamic.
>
> You've actually already written a higher-order function in this course. The `createRemoveButton()` function is a higher-order function because the `onClick` parameter is expected to be a function (because `onClick` is set up as an event listener callback function.

### A New Middlerware: Logging

Ee can use multiple middleware functions in a single application, let's create a new middleware function called `logger` that will log out information about the state and action.

The benefits of this `logger()` middleware function are huge while developing the application. We'll use this middleware to intercept all dispatch calls and log out what the action is that's being dispatched and what the state changes to _after_ the reducer has run. Being able to see this kind of information will be immensely helpful while we're developing our app. We can use this info to help us know what's going on in our app and to help us track down any pesky bugs that creep in.

```js
const logger = (store) => (next) => (action) => {
    console.group(action.type);
        console.log('The action: ', action);
        const result = next(action);
        console.log('The new state: ', store.getState());
    console.groupEnd();
    return result;
}

const store = Redux.createStore(Redux.combineReducers({
    todos,
    goals
}), Redux.applyMiddleware(checker, logger));
```

### Summary

In this section, we looked at using middleware. According to the Redux docs:

> Middleware is the suggested way to extend Redux with custom functionality.

Middleware is added to the Redux store using `Redux.applyMiddleware()`. You can only add middleware when you initially create the store:

```js
const store = Redux.createStore( <reducer-function>, Redux.applyMiddleware(<middleware-functions>) )
```
