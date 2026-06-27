
+++
title = "Gestión del estado con Redux""
date = 2018-06-13T02:13:50Z
author = "Sergio L. Benítez D."
description = "Notas sobre el curso de JavaScript."
tags = [
    "javascript",
    "react",
    "redux",
]
+++

Gestión del estado con Redux
==================

Una mala gestión del estado provoca errores difíciles de rastrear. Esto significa que tu aplicación esperaba que el estado fuera una cosa, pero en realidad es otra. El objetivo de _Redux_ es hacer que la gestión del estado en cualquier aplicación que construyas sea más predecible, para así mejorar la calidad del software.

El Store
---------

En una aplicación tradicional, los datos están esparcidos por toda la aplicación. El propósito de Redux es almacenar los datos de la aplicación fuera de la propia aplicación. Con un cambio así, si los datos necesitan modificarse, están todos en un único lugar y solo hay que cambiarlos una vez. Entonces, las partes de la aplicación que referencian esos datos se actualizarán automáticamente, ya que la fuente de la que obtienen los datos ha cambiado.

El concepto de poner todo el estado en una única ubicación se llama _Árbol de Estado_ (State Tree).

### Árbol de Estado

Para empezar a entender el Árbol de Estado, veamos un ejemplo:

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

Observa cómo todos los datos de este sitio de cocina imaginario están en un único objeto. Esto también significa que el estado del sitio se almacena en un solo lugar. Por tanto, el _Árbol de Estado_ consiste en guardar todos los datos en un único objeto. El concepto de árbol de estado se simboliza con un triángulo.

Ahora que todo el estado está almacenado en un solo lugar, tenemos que averiguar cómo interactuar con él. Existen tres formas de comunicarse con el árbol de estado:

1. Obtener el estado
2. Escuchar los cambios del estado
3. Actualizar el estado

Combinamos los tres elementos anteriores junto con el propio objeto del árbol de estado en una sola unidad que llamamos _el store_. Veremos cómo crear este store en la siguiente sección.

Creando el Store
----------------

Recuerda que el store contiene la siguiente información:

- El árbol de estado
- Una forma de obtener el árbol de estado
- Una forma de escuchar y responder a los cambios de estado
- Una forma de actualizar el estado

Para crear el store, implementaremos una función factoría que cree objetos _store_. Luego haremos que el store haga un seguimiento del estado y escribiremos el método para obtener el estado desde el store.

```js
function createStore() {
    let state;

    const getState = () => {
        return state;
    }

    return getState;
}
```

Con este avance, nuestra función factoría `createStore()` actualmente:

- No recibe argumentos
- Configura una variable local privada para almacenar el estado
- Define una función `getState()`
- Retorna un objeto que expone públicamente la función `getState()`

La función `getState()` devuelve la variable de estado existente.

Nuestra siguiente tarea es escuchar los cambios del estado. Para escuchar los cambios, añadimos un método `subscribe()` a la función factoría donde podremos suscribir una función callback llamada `listener`. De este modo, cada vez que el estado cambie internamente, podremos invocar la función callback y el usuario podrá hacer lo que desee.

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

En el último fragmento podemos ver que el método `subscribe()` gestiona la funcionalidad de suscribir una función callback que se pasa al array de listeners cuando se invoca. Ten en cuenta que el usuario puede suscribirse más de una vez. De igual forma, tenemos que habilitar la posibilidad de cancelar la suscripción a los cambios.

Antes de profundizar en la parte de actualización del estado, recapitulemos el objetivo de Redux: aumentar la previsibilidad del estado en nuestra aplicación. Por lo tanto, no podemos permitir que cualquier cosa actualice el estado, porque si lo hiciéramos, la previsibilidad disminuiría drásticamente. Así que debemos establecer un conjunto estricto de reglas que nos garanticen aumentar la previsibilidad en cuanto a la actualización del estado. La primera regla es:

> Solo un evento puede cambiar el estado del store.

Ahora, definamos qué es un _evento_. Un evento es una fuente que produce un cambio de estado. Podemos representar un evento como parte de un objeto. Cuando un evento tiene lugar en una aplicación Redux, usamos un objeto plano de JavaScript para registrar cuál fue el evento específico. Este objeto se llama **Action**. El siguiente fragmento es un ejemplo de una acción:

```js
{
    type: "ADD_PRODUCT_TO_CART"
}
```

Como puedes ver, una Action es simplemente un objeto plano de JavaScript. Lo que hace único a este objeto en Redux es que cada Action debe tener una propiedad `type`. El propósito de la propiedad `type` es informar a nuestra aplicación (Redux) de qué evento acaba de ocurrir. Dado que una Action es un objeto común, podemos incluir datos adicionales sobre el evento que tuvo lugar:

```js
{
    type: "ADD_PRODUCT_TO_CART",
    productId: 21
}
```

En esta Action, incluimos el campo `productId`. Ahora sabemos qué producto se añadió al store. No obstante, en los objetos Action, es una buena práctica pasar la menor cantidad de datos posible en cada acción. Es decir, es preferible pasar el índice o ID de un producto en lugar del _objeto producto completo_.

Observa ahora lo descriptivas que son las Actions en Redux.

Es momento de añadir la segunda regla para aumentar la previsibilidad en el estado de la aplicación:

> La función que devuelve el nuevo estado debe ser una función pura

Una **función pura** puede sonar un poco teórica, pero es el concepto que nos ayuda a mejorar la previsibilidad. Las funciones puras son fundamentales para entender cómo se actualiza el estado en las aplicaciones Redux. Por definición, las funciones puras:

1. Devuelven el mismo resultado si se pasan los mismos argumentos
2. Dependen únicamente de los argumentos que reciben
3. No producen efectos secundarios

Veamos un ejemplo de función pura, `square()`:

```js
const square = x = x + x;
```

`square()` es una función pura porque devuelve el mismo valor siempre, dado que se le pase el mismo argumento. No depende de ningún otro valor para producir ese resultado, y podemos esperar con seguridad solo ese resultado — sin efectos secundarios.

Por otro lado, veamos un ejemplo de función impura `calculateTip()`:

```js
const tipPercentage = 0.15;

const calculateTip = cost => cost * tipPercentage;
```

`calculateTip()` calcula y devuelve un valor numérico. Sin embargo, depende de una variable (`tipPercentage`) que vive fuera de la función para producir ese valor. Dado que no cumple uno de los requisitos de las funciones puras, `calculateTip()` es una función impura. No obstante, podríamos convertir esta función en una función pura pasando la variable externa, `tipPercentage`, como segundo argumento.

```js
const calculateTip = (cost, tipPercentage = 0.15) => cost * tipPercentage;
```

Para nuestros propósitos, la característica esencial de una función pura es que es **predecible**. Si tenemos una función que recibe nuestro estado y la acción que ocurrió, la función debería (¡si es pura!) devolver el mismo resultado _siempre_.

Volviendo al ejemplo de creación del store, ahora necesitamos una forma de vincular nuestro estado y nuestras acciones. Es decir, una forma de actualizar el estado interno del store basándonos en la acción específica que ocurrió. Para lograrlo debemos usar una función pura:

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

La función `todos()` es una función pura que toma el estado actual de la aplicación y la acción, y luego nos devuelve el nuevo estado de la aplicación. La función que maneja esta lógica se llama _reducer_, porque toma el estado y la acción y los reduce a un estado completamente nuevo.

Repasemos lo que tenemos hasta ahora: hay tres partes en la aplicación:

- Objetos Action: Representan los diferentes eventos que cambiarán el estado de nuestro store.
- Funciones Reducer: Función que toma el estado actual y la acción que ocurrió y devuelve el nuevo estado.
- Create Store: Responsable de crear el store propiamente dicho.

La última pieza que nos falta resolver es cómo actualizar el estado. Para ello, crearemos una nueva función llamada `dispatch()`. La función `dispatch()` es responsable de actualizar el estado dentro del store. Para hacerlo, la función necesita recibir la acción que le indicará a dispatch el evento específico que ocurrió dentro de la aplicación. Luego, dispatch realizará dos tareas que veremos más adelante.

Organicemos nuestro código y añadamos la función `dispatch()` al store:

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

Observa que cuando invocamos la función `createStore(reducer)` le estamos pasando el reducer.

El nuevo método `dispatch()` es bastante pequeño pero es vital para el funcionamiento de nuestro código del store. Para recapitular brevemente cómo funciona el método:

- `dispatch()` se llama con una Action
- El reducer que se pasó a `createStore()` se invoca con el árbol de estado actual y la acción... esto actualiza el árbol de estado
- Dado que el estado ha cambiado (potencialmente), se invocan todas las funciones listener registradas con el método `subscribe()`

La siguiente tabla resume la relación entre funcionalidad y método del store:

| **Funcionalidad**                                              | **Método del Store** |
|----------------------------------------------------------------|----------------------|
| Obtiene el estado actual                                       | `.getStore()`        |
| Recibe funciones que se llamarán cuando el estado cambie       | `.subscribe()`       |
| El estado de la aplicación                                     | El árbol de estado   |
| Modifica el estado                                             | `.dispatch()`        |


### Resumen

En esta sección, comenzamos a crear nuestro store construyendo una función `createStore()`. Hasta ahora, esta función mantiene un seguimiento del estado y proporciona un método para obtener el estado y otro para registrar funciones listener que se ejecutarán cada vez que el estado cambie.

Además, aprendimos muchos puntos importantes sobre Redux. Aprendimos sobre las funciones puras, la función Reducer (que, en sí misma, debe ser una función pura), el despacho de cambios en nuestro store y la identificación de qué partes de nuestro código son código de librería genérico y cuáles son específicas de nuestra aplicación.

Uniendo todas las piezas
-------------------------

El siguiente fragmento reúne todas las partes del store y también muestra cómo usarlo:

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

Desglosemos lo que hemos logrado:

- Creamos una función llamada `createStore()` que devuelve un objeto _store_
- A `createStore()` se le debe pasar una función _reducer_ cuando se invoca
- El objeto store tiene tres métodos:
    - `.getState()`: Se usa para obtener el estado actual del store
    - `.subscribe()`: Se usa para proporcionar una función listener que el store llamará cuando el estado cambie
    - `.dispatch()`: Se usa para realizar cambios en el estado del store
- Los métodos del objeto store tienen acceso al estado del store mediante clausura

Gestionando más Estado
----------------------

Añadamos las siguientes acciones a nuestro reducer:

- La acción `REMOVE_TODO`
- La acción `TOGGLE_TODO`

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

¡Ahora la aplicación puede manejar la _eliminación_ de un elemento todo, así como el _cambio de estado_ de un todo (completado o no completado)! Para hacer esto posible, actualizamos nuestro reducer `todos` para que pueda responder a acciones del tipo `REMOVE_TODO` y `TOGGLE_TODO`. Ten en cuenta que, al igual que el reducer `todos` original, simplemente devolvemos el estado original si el reducer recibe un tipo de acción que no le concierne.

Para eliminar un elemento todo, llamamos a `filter()` sobre el estado. Esto devuelve un nuevo estado (un array) con solo los elementos todo cuyos `id` no coincidan con el `id` del todo que queremos eliminar.

Para manejar el cambio de estado de un todo, queremos cambiar el valor de la propiedad `complete` en el `id` que se pase en la acción. Mapeamos todo el estado y, si `todo.id` coincide con `action.id`, usamos `Object.assign()` para devolver un nuevo objeto con las propiedades combinadas.

### Añadiendo Metas

Hagamos la aplicación un poco más compleja y añadamos una segunda pieza de estado para que nuestra aplicación haga seguimiento: las metas (goals).

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

Ahora tenemos una función reducer para el contexto de `goals`. Sin embargo, tenemos un problema: la función `createStore()` solo maneja una _única función reducer_. No podemos llamar a `createStore()` pasándole dos funciones reducer. Para resolver este inconveniente, en lugar de pasarle a `createStore()` nuestro reducer `todos`, crearemos una función `rootReducer` que será responsable de llamar al reducer correcto cada vez que se despachen acciones específicas. El siguiente código muestra un ejemplo de función `rootReducer` llamada `app()`.

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

La composición de reducers puede sonar intimidante, pero es más simple de lo que parece. La idea es que puedes crear un reducer para gestionar no solo cada sección de tu store de Redux, sino también cualquier dato anidado. Supongamos que estamos tratando con un árbol de estado que tiene esta estructura:

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

Tenemos tres propiedades principales en nuestro árbol de estado: users, settings y tweets. Naturalmente, crearíamos un reducer individual para cada una de ellas y luego un único root reducer usando el método `combineReducers` de Redux.

```js
const reducer = combineReducers({
    users,
    settings,
    tweets
})
```

`combineReducers`, por debajo, es nuestro primer vistazo a la composición de reducers. `combineReducers` es responsable de invocar todos los demás reducers, pasándoles la porción del estado que les concierne. Estamos creando un root reducer componiendo varios reducers juntos. Con esto en mente, echemos un vistazo más de cerca a nuestro reducer de tweets y cómo podemos aprovechar de nuevo la composición de reducers para hacerlo más modular.

En concreto, veamos cómo un usuario podría cambiar su avatar con la forma en que nuestro store está actualmente estructurado. Aquí está el esqueleto con el que partiremos:

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

Lo que nos interesa es ese último caso, UPDATE_AVATAR. Este es interesante porque _tenemos datos anidados_ — y recuerda, los reducers deben ser puros y no pueden mutar el estado. Aquí hay un enfoque posible.

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

Eso son muchos operadores de propagación. La razón es que, para cada nivel, queremos propagar todas las propiedades de ese nivel en los nuevos objetos que estamos creando (por inmutabilidad). ¿Y si, igual que separamos nuestros reducers de tweets, users y settings pasándoles la porción del árbol de estado que les concierne, hiciéramos lo mismo con nuestro reducer de tweets y sus datos anidados? Haciendo eso, el código anterior se transformaría en algo así:

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

Todo lo que hemos hecho es separar cada nivel de nuestros datos anidados de tweets en sus propios reducers. Luego, tal como hicimos con nuestro root reducer, pasamos a esos reducers la porción del estado que les concierne.

Buenas Prácticas
----------------

Para mejorar la legibilidad de nuestro código, podemos aplicar las siguientes dos prácticas:

1. Almacenar las cadenas de los tipos de acción en variables para evitar errores de _typo_.
2. Usar funciones creadoras de acciones (action creators) para crear nuestros objetos de acción.

El siguiente fragmento ilustra estas prácticas:

```js
/* Crear un Action Creator con un tipo de acción almacenado en una variable.
 *
 * Necesitas crear un action creator llamado 'mealCreator' que debe:
 *   - Aceptar un id
 *   - Devolver una acción Redux con una propiedad 'type' cuyo valor sea 'CREATE_MEAL'
 *   - Incluir el id pasado al action creator
*/

const CREATE_MEAL = 'CREATE_MEAL';

function mealCreator (id) {
    return {
        type: CREATE_MEAL,
        id: id
    }
}
```
