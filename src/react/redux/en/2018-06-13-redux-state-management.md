Managing State
==============

State mismanagement causes some bugs. This means that your application was expecting the state to be one thing, but it is something else. The goal of _Redux_ is to make state management in any app you build, more predictable to achieve quality improvement.

The Store
---------

A traditional app has the application's data is sprinkled throughout the app. Redux's purpose store the app references the application data outside of the app and. With a change like this, if the data needs to be modified at all, then all of the data is located in one place and needs to be only changed once. Then the areas of the app that are referencing pieces of data will be updated since the source they're pulling from has changed.

The concept of putting all the state in a single location is called _State tree_

### State Tree

To start the understanding of the State Tree, let's check an example:

```js
{
    recipes: [
        { … },
        { … },
        { … }
    ],
    ingredients: [
        { … },
        { … },
        { … },
        { … },
        { … },
        { … }
    ],
    products: [
        { … },
        { … },
        { … },
        { … }
    ]
}
```

See how all the data of this imaginary cooking site is all in a single object. This also means that the state of the site is stored in one single location. So, the _State Tree_ is saving all the data in a single object. The state tree concept is symbolized with a triangle.

Now that all state is stored in one location, we have to figure out how to interact with it. Exist three ways to communicate with the state tree:

1. Getting the state
2. Listening for changes to the state
3. Updating the state

Then we combine the three items above and the state tree object itself into one unit which we called _the store_. We'll look at creating this store in the next section.

Creating Store
--------------

Remember that the store has the following information:

- The state tree
- A way to get the state tree
- A way to listen and respond to the state changing
- A way to update the state

To create the store, we will create a factory function that creates _store_ objects. Then we'll have the store keep track of the state, and we'll write the method to get the state from the store.

```js
function createStore() {
    let state;

    const getState = () => {
        return state;
    }

    return getState;
}
```

With this advance our `createStore()` factory function is currently:

- No takes arguments
- Sets up a local private variable to hold the sate
- Sets up a `getState()` function
- retrun an object that publicly exposes the `getState()` function

The `getState()` function returns the existing state variable.

Our next task is to listen for changes the state. To listen to the changes we add a `subscribe()` method to the factory function where will able subscribe a callback function called `listener`. Thereby, whenever the state changes internally, we can invoke the callback function and then the user cloud do anything they want.

```js
function createStore() {
    let state;
    let listeners = [];

    const getState = () => {
        return state;
    }

    const subscribe = (listener) => {
        listeners.push(listener);

        return () => {
            listeners = listeners.filter((unsubscribeListener) => unsubscribeListener !== listener);
        }
    }

    return {
        getState,
        subscribe
    }
}

const store = createStore();
store.subscribe(() => {
    console.log('The new state is', store.getStore());
})

const unsubscribe = store.subscribe = store.subscribe(() => {
    console.log('The store changed')
})
```

In the last snippet we can see that the `subscribe()` method handle the functionality of subscribing a callback function passed when is invoked in the listeners' array. Keep in mind that the user can subscribe more than one time. Similarly, we have to enable the possibility of unsubscribing for changes.

Before to go deep in the update state part, we will recap the goal of Redux: Increase the predictability of the state in our application. Therefore, we can't allow anything to update the state, because if we did it, that would drastically decrease predictability. So we have to establish a strict set of rules that guarantee us to increase predictability regarding updating the state. The first rule is:

> Only an event can change the state of the store.

Now, let's define what an _event_ is. An event is a source that produces a change of state. We can represent an event as part of an object. When an event takes place in a Redux app, we use a plain JavaScript object to keep track of what the specific event was. This object is called an **Action**. Next snippet is an example of an action:

```js
{
    type: "ADD_PRODUCT_TO_CART"
}
```

As you can see, an Action is just a plain JavaScript object. What makes this plain JavaScript object unique in Redux, is that every Action must have a `type` property. The purpose of the `type` property is to let our app (Redux) know what event just took place. Since an  Action is just a regular object, we can include extra data about the event that took place:

```js
{
    type: "ADD_PRODUCT_TO_CART",
    productId: 21
}
```

In this Action, we include the `productId` field. Now we know which product was added to the store. Nevertheless, in Action objects, it's better practice to pass as little data as possible in each action. That is, prefer passing the index or ID of a product rather than the _entire product object_ itself.

See now how descriptive are the Actions in Redux.

Time to add the second rule to increase predictability in the state app:

> The function that returns the new state ness to be a pure function

A **pure function** can be a bit theoretical, but is the concept that helps us to improve predictability. Pure functions are integral to how the state in Redux applications is updated. By definition, pure functions:

1. Return the same result if the same arguments are passed in
2. Depend solely on the arguments passed into them
3. Do not produce side effects

Let's check and example of a pure function, `square()`:

```js
const square = x = x + x;
```

`square()` is a pure function because it outputs the same value every single time, given that the same argument is passed into it. There is no dependence on any other values to produce that result, and we can safely expect just that result to be returned -- no side effects.

On the other hand, let's check our an example of an impure function `calculateTip()`:

```js
const tipPercentage = 0.15;

const calculateTip = cost => cost * tipPercentage;
```

`calculateTip()` calculates and returns a number value. However, it relies on a variable (`tipPercentage`) that lives outside the function to produce that value. Since it fails one of the requirements of pure functions, `calculateTip()` is an impure function. However, we could convert this function to a pure function by passing in the external variable, `tipPercentage`, as the second argument to this function.

```js
const calculateTip = (cost, tipPercentage = 0.15) => cost * tipPercentage;
```

For our purposes, the essential feature of a pure function is that it's **predictable**. If we have a function that takes in our state and action that occurred, the function should (if it's pure!) return the same result _every single time_.

Back to the create store example, now we need is a way to tie our state and our actions together. This means a way to update the internal state of the store based on the specific action that occurred. The get that we should use a pure function:

```js
function todos(state = [], action) {
    if (action.type === 'ADD_TODO') {
        return state.concat([action.todo]);
    }

    return state
}

function createStore() {
    let state;
    let listeners = [];

    const getState = () => {
        return state;
    }

    const subscribe = (listener) => {
        listeners.push(listener);

        return () => {
            listeners = listeners.filter((unsubscribeListener) => unsubscribeListener !== listener);
        }
    }

    return {
        getState,
        subscribe
    }
}
```

The `todos()` function is a pure function that takes the current state of the app and action and then returns us the new state of the application. The function that handles this logic is called _reducer_, because it takes the state and action and then reducing that to a brand new state.

Let's review what we have so far: There are three parts of the app;

- Actions Objects: Represents the different events that will change the state of our store.
- Reducers Functions: Function which takes the current state and action which occurred and returns the new state.
- Create Store: Responsible for creating the actual store.

The last part we have to figure out is how to update state. To do that, we will create a new function called `dispatch()`. The `dispatch()` function is responsible for updating the state inside the actual store. To do that, the function needs to receive the action which is going to tell to the dispatch the specific event that occurred inside of the application. Then the dispatch will perform two task that we will see later.

Let's organize our code and add the `dispatch()` function to the store:

```js
// Library Code
function createStore(reducer) {
    let state;
    let listeners = [];

    const getState = () => {
        return state;
    }

    const subscribe = (listener) => {
        listeners.push(listener);

        return () => {
            listeners = listeners.filter((unsubscribeListener) => unsubscribeListener !== listener);
        }
    }

    const dispatch = (action) => {
        state = reducer(state, action);
        listeners.forEach((listener) => listener());
    }

    return {
        getState,
        subscribe,
        dispatch
    }
}

// App Code
function todos(state = [], action) {
    if (action.type === 'ADD_TODO') {
        return state.concat([action.todo]);
    }

    return state
}

createStore(todos)
```

Check that when we invoke the `createStore(reducer)` function we are passing the reducer.

The new `dispatch()` method is pretty small but is vital to our functioning store code. To briefly recap how the method functions:

- `dispatch()` is called with an Action
- The reducer that was passed to `createStore()` is called with the current state tree and the action...this updates the state tree
- Because the state has (potentially) changed, all listener functions that have been registered with the `subscribe()` method are called

The next table summarizes the relationship between functionality and store method:

| **Functionality**                                             | **Store Method** |
|---------------------------------------------------------------|------------------|
| Gets the current state                                        | `.getStore()`    |
| Takes in functions that will be called when the state changes | `.subscribe()`   |
| the application's state                                       | The state tree   |
| Modifies the state                                            | `.dispatch()`    |


### Summary

In this section, we started creating our store by building out a `createStore()` function. So far, this function keeps track of the state and provides a method to get the state and one to keep track of listener functions that will be run whenever the state changes.

Also, we learned about many important points about Redux. We learned about pure functions, a Reducer function (which, itself, needs to be a pure function), dispatching changes in our store, and identifying which parts of our code are generic library code and which are specific to our app.

Putting it all together
-----------------------

The next snippet recopile all the parts of the store and also show how use it:

```js
// Library Code
function createStore(reducer) {
    let state;
    let listeners = [];

    const getState = () => {
        return state;
    }

    const subscribe = (listener) => {
        listeners.push(listener);

        return () => {
            listeners = listeners.filter((unsubscribeListener) => unsubscribeListener !== listener);
        }
    }

    const dispatch = (action) => {
        state = reducer(state, action);
        listeners.forEach((listener) => listener());
    }

    return {
        getState,
        subscribe,
        dispatch
    }
}

// App Code
function todos(state = [], action) {
    if (action.type === 'ADD_TODO') {
        return state.concat([action.todo]);
    }

    return state
}

const store = createStore(todos);

store.subscribe(() => {
    console.log('The new state is:', store.getState());
});

store.dispatch({
    type: 'ADD_TODO',
    id: 0,
    name: 'Learn Redux',
    complete: false
})

store.dispatch({
    type: 'ADD_TODO',
    id: 1,
    name: 'Learn React',
    complete: true
})
```

Let's break down what we've accomplished:

- We created a function called `createStore()` that returns a _store_ object
- `createStore()` must be passed a _reducer_ function when invoked
- The store object has three methods on it:
    - `.getState()`: Used to get the current state from the store
    - `.subscribe()`: Used to provide a listener function the store will call when state changes
    - `.dispatch()`: Used to make changes to the store's state
- The store object's methods have access to the state of the store via closure

Managing more State
-------------------

Let's add the next actions to our reducer:

- The `REMOVE_TODO` action
- The `TOGGLE_TODO` action

```js
// Library Code
function createStore(reducer) {
    let state;
    let listeners = [];

    const getState = () => {
        return state;
    }

    const subscribe = (listener) => {
        listeners.push(listener);

        return () => {
            listeners = listeners.filter((unsubscribeListener) => unsubscribeListener !== listener);
        }
    }

    const dispatch = (action) => {
        state = reducer(state, action);
        listeners.forEach((listener) => listener());
    }

    return {
        getState,
        subscribe,
        dispatch
    }
}

// App Code
function todos(state = [], action) {
    switch(action.type) {
        case 'ADD_TODO':
            return state.concat([action.todo]);
        case 'REMOVE_TODO':
            return state.filter((todo) => todo.id !== action.id);
        case 'TOGGLE_TODO':
            return state.map((todo) => todo.id !== action.id ? todo : Object.assign({}, todo, {complete: !todo.complete}));
        default
            return state;
    }
}

const store = createStore(todos);

store.subscribe(() => {
    console.log('The new state is:', store.getState());
});

store.dispatch({
    type: 'ADD_TODO',
    id: 0,
    name: 'Learn Redux',
    complete: false
})

store.dispatch({
    type: 'ADD_TODO',
    id: 1,
    name: 'Learn React',
    complete: true
})
```

Now the app can handle _removing_ a todo item, as well as _toggling_ a todo item (as complete or incomplete)! To make this all possible, we updated our `todos` reducer to be able to respond to actions of the type `REMOVE_TODO` and `TOGGLE_TODO`. Note that just like the original `todos` reducer, we simply return the original state if the reducer receives an action type that it's not concerned with.

To remove a todo item, we called `filter()` on the state. This returns a new state (an array) with only todo items whose `id`'s do not match the `id` of the todo we want to remove.

To handle toggling a todo item, we want to change the value of the `complete` property on whatever `id` is passed along on the action. We mapped over the entire state, and if todo.id matched action.id, we used `Object.assign()` to return a new object with merged properties:

### Adding Goals
Let's make the app a bit more complicated and add in a second piece of state for our app to track - goals.

```js
// Library Code
function createStore(reducer) {
    let state;
    let listeners = [];

    const getState = () => {
        return state;
    }

    const subscribe = (listener) => {
        listeners.push(listener);

        return () => {
            listeners = listeners.filter((unsubscribeListener) => unsubscribeListener !== listener);
        }
    }

    const dispatch = (action) => {
        state = reducer(state, action);
        listeners.forEach((listener) => listener());
    }

    return {
        getState,
        subscribe,
        dispatch
    }
}

// App Code
function todos(state = [], action) {
    switch(action.type) {
        case 'ADD_TODO':
            return state.concat([action.todo]);
        case 'REMOVE_TODO':
            return state.filter((todo) => todo.id !== action.id);
        case 'TOGGLE_TODO':
            return state.map((todo) => todo.id !== action.id ? todo : Object.assign({}, todo, {complete: !todo.complete}));
        default
            return state;
    }
}

function goals(state = [], action) {
    switch(action.type) {
        case 'ADD_TODO':
            return state.concat([action.goal]);
        case 'REMOVE_TODO':
            return state.filter((goal) => goal.id !== action.id);
        default
            return state;
    }
}

const store = createStore(todos);

store.subscribe(() => {
    console.log('The new state is:', store.getState());
});

store.dispatch({
    type: 'ADD_TODO',
    id: 0,
    name: 'Learn Redux',
    complete: false
})

store.dispatch({
    type: 'ADD_TODO',
    id: 1,
    name: 'Learn React',
    complete: true
})
```

Now we have reducer function to the `goals` context. However, we got a problem: The `createStore()` function only handle a _single reducer function_. We can' call `createStore()` passing it two reducer functions. To solve this inconvenient, instead of passing `createStore()` out single `todos` reducer, we will create a `rootReducer` function which will be responsible for calling the correct reducer whenever specific actions are dispatched. The next code handle shows an example of `rootReducer` function called `app()`.

```js
// Library Code
function createStore(reducer) {
    let state;
    let listeners = [];

    const getState = () => {
        return state;
    }

    const subscribe = (listener) => {
        listeners.push(listener);

        return () => {
            listeners = listeners.filter((unsubscribeListener) => unsubscribeListener !== listener);
        }
    }

    const dispatch = (action) => {
        state = reducer(state, action);
        listeners.forEach((listener) => listener());
    }

    return {
        getState,
        subscribe,
        dispatch
    }
}

// App Code
function todos(state = [], action) {
    switch(action.type) {
        case 'ADD_TODO':
            return state.concat([action.todo]);
        case 'REMOVE_TODO':
            return state.filter((todo) => todo.id !== action.id);
        case 'TOGGLE_TODO':
            return state.map((todo) => todo.id !== action.id ? todo : Object.assign({}, todo, {complete: !todo.complete}));
        default
            return state;
    }
}

function goals(state = [], action) {
    switch(action.type) {
        case 'ADD_TODO':
            return state.concat([action.goal]);
        case 'REMOVE_TODO':
            return state.filter((goal) => goal.id !== action.id);
        default
            return state;
    }
}

function app(state = {}, action) {
    return {
        todos: todos(state.todos, action),
        goals: goals(state.goals ,action)
    }
}

const store = createStore(app);

store.subscribe(() => {
    console.log('The new state is:', store.getState());
});

store.dispatch({
    type: 'ADD_TODO',
    todo: {
        id: 0,
        name: 'Learn Redux',
        complete: false
    }
})

store.dispatch({
    type: 'ADD_TODO',
    todo: {
        id: 1,
        name: 'Learn React',
        complete: false
    }
})

store.dispatch({
    type: 'ADD_TODO',
    todo: {
        id: 2,
        name: 'Learn React Native',
        complete: false
    }
})

store.dispatch({
    type: 'TOGGLE_TODO',
    id: 1
})

store.dispatch({
    type: 'REMOVE_TODO',
    id: 2
})

store.dispatch({
    type: 'ADD_GOAL',
    goal: {
        id: 0,
        name: 'Llose 20 pounds',
    }
})

store.dispatch({
    type: 'ADD_GOAL',
    goal: {
        id: 1,
        name: 'Learn Angular',
    }
})

store.dispatch({
    type: 'REMOVE_GOAL',
    id: 1
})
```

Reducer composition sounds intimidating, but it's simpler than you might think. The idea is that you can create a reducer to manage not only each section of your Redux store but also any nested data as well. Let's say we were dealing with a state tree like had this structure:

```js
{
    users: {},
    setting: {},
    tweets: {
        btyxlj: {
            id: 'btyxlj',
            text: 'What is a jQuery?',
            author: {
                name: 'Tyler McGinnis',
                id: 'tylermcginnis',
                avatar: 'twt.com/tm.png'
            }
        }
    }
}
```

We have three main properties on our state tree: users, settings, and tweets. Naturally, we'd create a single reducer for both of those and then create a single root reducer using Redux's "combineReducers" method.

```js
const reducer = combineReducers({
    users,
    settings,
    tweets
})
```

`combineReducers`, under the hood, is our first look at reducer composition. `combineReducers` is responsible for invoking all the other reducers, passing them the portion of their state that they care about. We're making one root reducer, by composing a bunch of other reducers together. With that in mind, let's take a closer look at our tweets reducer and how we can leverage reducer composition again to make it more compartmentalized.

Specifically, let's look at how a user might change their avatar with the way our store is currently structured. Here's the skeleton with what we'll start out with:

```js
function tweets (state = {}, action) {
    switch(action.type){
        case ADD_TWEET :
        ...
        case REMOVE_TWEET :
            ...
        case UPDATE_AVATAR :
            ???
    }
}
```

What we're interested in is that last one, UPDATE_AVATAR. This one is interesting because _we have some nested data_ - and remember, reducers have to be pure and can't mutate any state. Here's one approach.

```js
function tweets (state = {}, action) {
switch(action.type){
    case ADD_TWEET :
        ...
    case REMOVE_TWEET :
        ...
    case UPDATE_AVATAR :
        return {
            ...state,
            [action.tweetId]: {
            ...state[action.tweetId],
                author: {
                ...state[action.tweetId].author,
                    avatar: action.newAvatar
                }
            }
        }
    }
}
```

That's a lot of spread operators. The reason for that is because, for every layer, we're wanting to spread all the properties of that layer on the new objects we're creating (because, immutability). What if, just like we separated our tweets, users, and settings reducers by passing them the slice of the state tree they care about, what if we do the same thing for our tweets reducer and its nested data. Doing that, the code above would be transformed to look like this:

```js
function author (state, action) {
    switch (action.type) {
        case : UPDATE_AVATAR
            return {
                ...state,
                avatar: action.newAvatar
            }
        default :
            state
    }
}

function tweet (state, action) {
    switch (action.type) {
        case ADD_TWEET :
            ...
        case REMOVE_TWEET :
            ...
        case : UPDATE_AVATAR
            return {
                ...state,
                author: author(state.author, action)
            }
        default :
            state
    }
}

function tweets (state = {}, action) {
    switch(action.type){
        case ADD_TWEET :
            ...
        case REMOVE_TWEET :
            ...
        case UPDATE_AVATAR :
            return {
                ...state,
                [action.tweetId]: tweet(state[action.tweetId], action)
            }
        default :
            state
        }
}
```

All we've done is separated out each layer of our nested tweets data into their reducers. Then, just like we did with our root reducer, we're passing those reducers the slice of the state they care about.

Better Practices
----------------

To improve our code readability, we can perform the next two practices:

1. Store the action types strings into variables to avoiding `typo` errors.
2. Use action creators functions to create our actions objects

The next snippet illustrates these practice:

```js
/* Create An Action Creator with a action type stored in a variable.
 *
 * You need to create an action creator called 'mealCreator' that should:
 *   - Accept an id
 *   - Return a Redux action with a 'type' property that has a value of 'CREATE_MEAL'
 *   - Include the id passed to the action creator
*/

const CREATE_MEAL = 'CREATE_MEAL';

function mealCreator (id) {
    return {
        type: CREATE_MEAL,
        id: id
    }
}
```
