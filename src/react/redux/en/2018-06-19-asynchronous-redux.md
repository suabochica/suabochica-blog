Asynchronous Redux
==================

In this post, we're going to be working with a (simulated) remote database. We'll use a provided API to interact with this database.

The important skill that you'll be learning in this lesson is how to make asynchronous requests in Redux. If you recall, the way Redux works right now is:

- `store.dispatch()` calls are made
- If the Redux store was set up with any middleware, those functions are run
- Then the reducer is invoked

But how do we handle the case where we need to interact with an external API to fetch data. For example, what if our To dos app had a button that would load existing To dos from a database? If we dispatch that action, we currently do not have a way to wait for the list of remote Todo items to be returned.

After going through this words, you'll be able to make asynchronous requests and work with remote data in a Redux application.

External Data
-------------

We're going to use a database to interact with our To dos application. We're simulating the database to keep that aspect of the project less complex. This is the HTML script tag you need to add the database to your application:

```js
<script src="https://tylermcginnis.com/goals-todos-api/index.js"></script>
```

> ### Promise-Based API
> The methods in the provided API are all Promise-based. Let's take a look at the `.fetchTodos()` method:
> ```js
> API.fetchTodos = function () {
>  return new Promise((res, rej) => {
>    setTimeout(function () {
>      res(todos);
>    }, 2000);
>  });
>};
> ```
> Here, we are creating and returning a `Promise()` object.
>
> Currently our app makes that the user wait unnecessarily long amount of time while the API fetch all the Todos and all the Goals. Since the API is Promised-based, we can use `Promise.all()` to wait all Promises have resolved before displaying the content to the user.
>
> Promises are asynchronous, therefore we will work with asynchronous data and asynchronous requests.

This way is an option to work with an external API. We added a new action (RECEIVE_DATA), created a new action creator, and built a new reducer to handle the different states our app can be in while getting our remote data:

- Before the app has the data
- While the app is fetching the data
- After the data has been received

Optimistic Updates
------------------

When dealing with asynchronous requests, there will always be some delay involved. If not taken into consideration, this could cause some weird UI issues. For example, let’s say when a user wants to delete a todo item, that whole process from when the user clicks “delete” to when that item is removed from the database takes two seconds. If you designed the UI to _wait for the confirmation from the server_ to remove the item from the list on the client, your user would click “delete” and then would have to wait for two seconds to see that update in the UI. That’s not the best experience.

What you can do is a technique called *optimistic updates*. In this technique, instead of waiting for confirmation from the server, just instantly remove the user from the UI when the user clicks “delete”, then, if the server responds back with an error that the user wasn’t actually deleted, you can add the information back in. This way your user gets that instant feedback from the UI, but, under the hood, the request is still asynchronous.

So far, swapped more functionality over to using the API. We now use the database to:

- Remove To dos and Goals
- Toggle the state of a To dos
- Save a new To do or Goal

What's important is that for the removing and toggling, we're doing these actions _optimistically_. So we're assuming the change will succeed correctly on the server, so we update the UI immediately, and then only roll back to the original state if the API returns an error. Doing optimistic updates is better because it provides a more realistic and dynamic experience to the user.

Thunk
-----

Currently, the code for removing a todo item looks like this:

```js
removeItem(item) {
    const { dispatch } = this.props.store

    dispatch(removeTodoAction(item.id))

    return API.deleteTodo(item.id)
        .catch(() => {
            dispatch(addTodoAction(item))
            alert('An error occured. Try again.')
        })
    }
}
```

Here, we are mixing our component-specific code with teh API-specific code. If we move the data-fetching logic from our component to the action creator, our final `removeItem()` method might look like:

```js
removeItem(item) {
    const { dispatch } = this.props.store

    return dispatch(handleDeleteTodo(item))
}
```

The `handleDeleteTodo` action creator makes an asynchronous request before it returns the action. We can't return here a promise because every action creator needs to return an _object_:

```js
function asyncActionCreator (id) {
    return {
        type: ADD_USER,
        user: ??
    };
}
```

To solve this, we have to mix our knowledge in functional programming with our knowledge of Redux middleware. Remove that the middleware sits _between_ the dispatching of an action, and the running of the reducer. The reducer expects to receive and action object, but we can instead of returning an object, return a function.

So, we can use the middleware to check if the returned action is either a function or an object. If is an object, then things will work as normal. However if the action is a function, it can invoke the function and pass it whatever information it needs (e.g. reference to the `dispatch()` method). this function could do anything it needs to do, like making asynchronous network requests, and can then dispatch a _different_ action that return a regular object when its finished.

And action creator that return a function can looks like:

```js
function asyncActionCreator (id) {
    return (dispatch) => {
        return API.fetchUser(id)
            .then(user) => {
                dispatch(addUser(user));
            }
    }
}
```

Notice that we’re no longer returning the action itself! Instead, we’re returning a function that is being passed dispatch. We then call this function when we have the data.

Now, this won’t work out of the box, but there's some good news: we can add some middleware to our app to support it! Lets add the [redux/thunk library](https://github.com/gaearon/redux-thunk)

```js
<script src="https://unpkg.com/redux-thunk@2.2.0/dist/redux-thunk.min.js"></script>
```

### Benefits of Thunks

Out of the box, the Redux store can only support the _synchronous_ flow of data. Middleware like **thunk** helps support _asynchronicity_ in a Redux application. You can think of thunk as a wrapper for the store’s `dispatch()` method; rather than returning action objects, we can use thunk action creators to dispatch functions (or even or Promises).

Without thunks, synchronous dispatches are the default. We _could_ still make API calls from React components (e.g., using the `componentDidMount()` lifecycle method to make these requests) -- but using thunk middleware gives us a cleaner separation of concerns. Components don't need to handle what happens after an asynchronous call, since API logic is _moved away_ from components to action creators. This also lends itself to greater predictability, since action creators will become the source of every change in state. With thunks, we dispatch an action only when the server request is resolved!

> ### Execution order with the Thunk middleware
> We expect the API request to occur first. `TodoAPIUtil.fetchTodos()` needs to be resolved before anything else can be done. Once the request is resolved, thunk middleware then invokes the function with `dispatch()`. Keep in mind: the action is only ever dispatched _after_ the API request is resolved.

In short, if a web application requires interaction with a server, applying middleware such as **thunk** helps solve the issue of asynchronous data flow. Thunk middleware allows us to write action creators that return _functions_ rather than objects.

By calling our API in an _action creator_, we make the action creator responsible for fetching the data it needs to create the action. Since we move the data-fetching code to action creators, we build a cleaner separation between our UI logic and our data-fetching logic. As a result, thunks can then be used to delay an action dispatch, or to dispatch only if a certain condition is met (e.g., a request is resolved).

Leveraging Thunks in our App
----------------------------

Converting to thunks improves the responsibilities of the code. Make sure to check off each of the following:

- Converted the Goals to use thunks
- Converted the To dos to use thunks
- Converted the fetching of the initial data to use thunks

### More Asynchronous Options

The most common requests I get for this course are around more advanced data-fetching topics with Redux. I've resisted because typically they bring in _a lot_ of complexity, while the benefits aren't seen until your data-fetching needs become large enough. With that said, now that you have a solid foundation on Redux and specifically, asynchronous Redux, you'll be in a good position to read up on the different options to decide if any would work best for the type of application you're working on. I encourage to read up on both of the other (popular) options.

- [Redux Promise](https://github.com/redux-utilities/redux-promise) - FSA-compliant promise middleware for Redux.
- [Redux Saga](https://github.com/redux-saga/redux-saga) - An alternative side effect model for Redux apps

Now, we used the thunk library that we installed in the previous section to make our code more singularly-focused and maintainable.