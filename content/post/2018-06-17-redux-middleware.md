+++
title = "Redux Middleware"
author = ["Sergio Benítez"]
description = "Qué es el middleware de Redux, cómo funciona y cómo aplicarlo para interceptar acciones antes de que lleguen al reducer"
date = 2018-06-17T00:00:00-05:00
lastmod = 2026-07-03T12:48:30-05:00
draft = false
tags = ["react", "javascript", "redux"]
+++

Hasta ahora, tenemos los siguientes tres conceptos como base de Redux:

-   Alamcenamiento - (Store en inglés)
-   Acciones (Actions en inglés)
-   Reductores (Reducers en inglés)

En esta lección profundizaremos en el Middleware de Redux. Veremos cómo nos permite engancharnos al ciclo de vida de Redux y por qué eso es beneficioso.

Para empezar con el repaso del Middleware de Redux, imagina que quieres añadir como tarea pendiente u objetivo la acción de invertir en Bitcoin. Sin embargo, tu asesor financiero no para de decirte que es una mala idea. Así que decidimos añadir una nueva funcionalidad a nuestra app de tareas; cada vez que añadamos un ítem o un nuevo objetivo que contenga la palabra "Bitcoin", en lugar de añadirlo a la lista correspondiente, la aplicación nos avise de que es una mala idea añadir ese ítem.

Para conseguirlo, deberíamos engancharnos al momento justo después de que se despache una acción, pero antes de que llegue al reducer y modifique el estado. Entonces, en el momento de crear el store añadiremos la siguiente función:

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

Finalmente, tendremos que usar la función `checkAndDispatch()` en lugar de la función `dispatch()` habitual. Con estos cambios, no podremos añadir ítems a las listas que contengan la palabra "Bitcoin".


## Middleware nativo de Redux {#middleware-nativo-de-redux}

Ya has aprendido cómo Redux hace la gestión del estado más predecible: para cambiar el estado del store, se debe despachar una acción que describa ese cambio hacia el reducer. A su vez, el reducer produce el nuevo estado. Este nuevo estado reemplaza al estado anterior en el store. Así, la próxima vez que se llame a `store.getState()`, se devolverá el nuevo estado más actualizado.

Entre el despacho de una acción y la ejecución del reducer, podemos introducir código llamado **middleware** para interceptar la acción antes de que se invoque el reducer. La documentación de Redux describe el middleware como:

> Un punto de extensión de terceros entre el despacho de una acción y el momento en que esta llega al reducer.

Lo bueno del middleware es que, una vez que recibe la acción, puede realizar una serie de operaciones, entre ellas:

-   Producir un efecto secundario (p. ej., registrar información sobre el store)
-   Procesar la propia acción (p. ej., hacer una petición HTTP asíncrona)
-   Redirigir la acción (p. ej., a otro middleware)
-   Despachar acciones complementarias

Incluso alguna combinación de lo anterior. El middleware puede hacer cualquiera de estas cosas _antes_ de pasar la acción al reducer.

Vamos a reemplazar nuestra función `checkAndDispatch()` por la auténtica función middleware de Redux.

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


### Dónde encaja el Middleware {#dónde-encaja-el-middleware}

La forma en que tuvimos que estructurar nuestro código originalmente implicaba que nuestra función `checkAndDispatch()` _tenía_ que ejecutarse _antes_ de `store.dispatch()`. ¿Por qué? Porque cuando se invoca `store.dispatch()`, este llama inmediatamente al reducer que se pasó al invocar `createStore()`. Si recuerdas la primera lección, así era nuestra función `dispatch()`:

```js
const dispatch = (action) => {
    state = reducer(state, action);
    listener.forEach((listener) => listener());
}
```

Así que puedes ver que llamar a `store.dispatch()` invocará inmediatamente la función `reducer()`. No hay forma de ejecutar nada entre ambas llamadas. Por eso tuvimos que crear nuestra `checkAndDispatch()`, para poder ejecutar código de verificación antes de llamar a `store.dispatch()`.

Sin embargo, esto no es mantenible. Si quisiéramos añadir otra comprobación, necesitaríamos escribir _otra_ función precedente, que luego llamara a `checkAndDispatch()`, y esta _luego_ a `store.dispatch()`. Nada mantenible.

Con la funcionalidad de middleware de Redux, podemos ejecutar código _entre_ la llamada a `store.dispatch()` y `reducer()`. Esto funciona porque la versión de `dispatch()` de Redux es más sofisticada que la nuestra, y porque proporcionamos las funciones middleware al crear el store:

```js
const store = Redux.createStore(<reducer-function>, <middleware-function>)
```

El método `createStore()` de Redux recibe la función reducer como primer argumento, y puede recibir un segundo argumento con las funciones middleware a ejecutar. Como configuramos el store de Redux teniendo conocimiento de la función middleware, esta se ejecuta entre `store.dispatch()` y la invocación del reducer.


### Aplicando Middleware {#aplicando-middleware}

La función `applyMiddleware()` es un argumento opcional de `createStore()`. Su firma es:

```text
applyMiddleware(...middlewares)
```

Fíjate en el operador spread sobre el parámetro `middlewares`. Esto significa que podemos pasar tantos middleware como queramos. Los middleware se ejecutan en el orden en que se proporcionaron a `applyMiddleware()`.

Actualmente tenemos el middleware `checker` aplicado a nuestra app. Vamos a añadir un nuevo middleware `logger`.

> #### Funciones que devuelven funciones {#funciones-que-devuelven-funciones}
> El middleware de Redux aprovecha un concepto llamado **funciones de orden superior**. Una función de orden superior es aquella que:
> -   Acepta una función como argumento
> -   Devuelve una función
> Las funciones de orden superior son una técnica de programación potente que permite que las funciones sean mucho más dinámicas.
> De hecho, ya has escrito una función de orden superior en este curso. La función `createRemoveButton()` es de orden superior porque se espera que el parámetro `onClick` sea una función (ya que `onClick` se configura como callback de un event listener).

### Un nuevo Middleware: Logging {#un-nuevo-middleware-logging}

Podemos usar múltiples funciones middleware en una misma aplicación. Vamos a crear una nueva función middleware llamada `logger` que registrará información sobre el estado y la acción.

Los beneficios de esta función middleware `logger()` son enormes durante el desarrollo de la aplicación. Usaremos este middleware para interceptar todas las llamadas a dispatch y registrar cuál es la acción que se está despachando y a qué cambia el estado _después_ de que el reducer se haya ejecutado. Poder ver este tipo de información será de gran ayuda mientras desarrollamos nuestra app. Podemos usar esta información para saber qué está pasando en nuestra app y para ayudarnos a localizar cualquier molesto bug que aparezca.

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


### Resumen {#resumen}

En esta publicación hemos visto cómo usar middleware. Según la documentación de Redux:

> El middleware es la forma recomendada de extender Redux con funcionalidad personalizada.

El middleware se añade al store de Redux usando `Redux.applyMiddleware()`. Solo se puede añadir middleware al crear inicialmente el store:

```js
const store = Redux.createStore( <reducer-function>, Redux.applyMiddleware(<middleware-functions>) )
```
