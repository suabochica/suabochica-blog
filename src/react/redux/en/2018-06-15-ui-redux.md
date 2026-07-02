UI + Redux
==========

In this lesson, we will take the state management library and add some UI to it. This will allow us to take a closer look at how our custom library can manage the state in an actual applicaiton.

UI
--

After create an `index.html` that consumes the state management in the library let's add the User Interface that represent this logic. Since our project has two pieces of state, we'll need two areas:

1. Todo list area
2. Goals area

Let's add the respective markup to represent this areas.

UI + State
----------

Now is time to reflect the changes of state in our User Interface. To get this result we need to liste for when the buttons are clicked; we did this with plain DOM `addEventListener()` method:

```js
document.getElementById('todoBtn').addEventListener('click', addTodo);
document.getElementById('goalBtn').addEventListener('click', addGoal);
```

Pressing the `#todoBtn` will call `addTodo()` which will add the new item to the state:

```js
function addTodo () {
    const input = document.getElementById('todo');
    const name = input.value;

    input.value = '';

    store.dispatch(addTodoAction({
        name: name,
        complete: false,
        id: generateId()
    }));
}
```

This methdo will extract the information from the input field, reset the input field, and then dispatch an `addTodoAction` Action Creator with the text that the user tuèd into the input field.

So we're using the UI to change the state of our store, but these changes are not reflecting the new state visually in the UI. Let´s add this items in their respective lists. To achieve this we have some DOM Skills like:

- accessing elements with `document.getElementById()`
- adding listeners with `.addEventListener()`
- accessing the `.value` property on an element
- creating a new element with `.createElement()`
- adding new content with `.appendChild()`
- etc.

### Summary

In this section, we connected our functioning state application with a front-end UI. We added some form fields and buttons to our UI that can be used to add new Todo items and Goal items to the state. Updating the state will also cause the entire application to re-render so that the visual representation of the application matches that of the info stored in the state object.

This is Redux
-------------

Until now, we use our own implementation of state management. But all we did is part of the Redux library. We're going to transition away from our custom code to using the actual Redux library. While we're working on this simple project, we'll be linkin to the hosted version of the Redux library:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/redux/3.7.2/redux.min.js"></script>
```

Now, the specific points in the transition are:

- Link the Redux library
- Delete the `createStore()` function
- Swap the `createStore(app)` call with `Redux.createStore(app)`
- Replace the `app()` function with `Redux.combimeReducers()`

### Summary
In this section, we replaced the code we wrote in the previous lesson with the actual Redux library code. We saw that swapping out our code with Redux's code didn't change anything with how our application functions or how our app-specific code works. _Redux is just a predictable state container_.