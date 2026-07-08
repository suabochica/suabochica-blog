+++
title = "Redux Asíncrono"
author = ["Sergio Benítez"]
description = "Cómo manejar peticiones asíncronas en Redux usando el middleware thunk y la técnica de actualizaciones optimistas"
date = 2018-06-19T00:00:00-05:00
draft = false
tags = ["react", "javascript", "redux"]
+++

Redux Asíncrono
===============

En este artículo vamos a trabajar con una base de datos remota (simulada). Usaremos una API proporcionada para interactuar con esta base de datos.

La habilidad más importante que aprenderás en esta lección es cómo hacer peticiones asíncronas en Redux. Si recuerdas, el flujo actual de Redux funciona así:

-   Se invoca `store.dispatch()`
-   Si el store de Redux se configuró con middleware, esas funciones se ejecutan
-   Luego se invoca el reducer

Pero, ¿cómo manejamos los casos en los que necesitamos interactuar con una API externa para obtener datos? Por ejemplo, ¿qué pasaría si nuestra app de tareas tuviera un botón que cargara las tareas existentes desde una base de datos? Si despachamos esa acción, actualmente no tenemos forma de esperar a que la lista de tareas remotas sea devuelta.

Después de leer este artículo, serás capaz de hacer peticiones asíncronas y trabajar con datos remotos en una aplicación Redux.

Datos externos
--------------

Vamos a usar una base de datos para interactuar con nuestra aplicación de tareas. Simulamos la base de datos para mantener ese aspecto del proyecto menos complejo. Esta es la etiqueta de script HTML que necesitas para añadir la base de datos a tu aplicación:

```js
<script src="https://tylermcginnis.com/goals-todos-api/index.js"></script>
```

> ### API basada en Promesas
>
> Los métodos de la API proporcionada están basados en Promesas. Echemos un vistazo al método `.fetchTodos()`:
>
> ```js
> API.fetchTodos = function () {
>  return new Promise((res, rej) => {
>    setTimeout(function () {
>      res(todos);
>    }, 2000);
>  });
> };
> ```
>
> Aquí estamos creando y devolviendo un objeto `Promise()`.
>
> Actualmente, nuestra app hace que el usuario espere un tiempo innecesariamente largo mientras la API obtiene todas las tareas y todos los objetivos. Como la API está basada en Promesas, podemos usar `Promise.all()` para esperar a que todas las Promesas se hayan resuelto antes de mostrar el contenido al usuario.
>
> Las promesas son asíncronas, por lo tanto trabajaremos con datos asíncronos y peticiones asíncronas.

Esta es una forma de trabajar con una API externa. Añadimos una nueva acción (`RECEIVE_DATA`), creamos un nuevo action creator y construimos un nuevo reducer para manejar los diferentes estados en los que puede estar nuestra app mientras obtenemos los datos remotos:

-   Antes de que la app tenga los datos
-   Mientras la app está obteniendo los datos
-   Después de que los datos se hayan recibido

Actualizaciones optimistas
--------------------------

Al trabajar con peticiones asíncronas, siempre habrá cierta latencia involucrada. Si no se tiene en cuenta, esto podría causar problemas extraños en la UI. Por ejemplo, supongamos que cuando un usuario quiere eliminar una tarea, todo el proceso desde que el usuario hace clic en "eliminar" hasta que ese ítem se elimina de la base de datos tarda dos segundos. Si diseñaste la UI para _esperar la confirmación del servidor_ antes de eliminar el ítem de la lista en el cliente, tu usuario haría clic en "eliminar" y luego tendría que esperar dos segundos para ver esa actualización en la UI. Esa no es la mejor experiencia.

Lo que puedes hacer es una técnica llamada **actualizaciones optimistas**. Con esta técnica, en lugar de esperar la confirmación del servidor, eliminas instantáneamente el ítem de la UI cuando el usuario hace clic en "eliminar" y luego, si el servidor responde con un error indicando que el ítem no se eliminó realmente, puedes volver a añadir la información. De esta forma, el usuario recibe ese feedback instantáneo de la UI, pero la petición sigue siendo asíncrona internamente.

Hasta ahora, hemos trasladado más funcionalidad hacia el uso de la API. Ahora usamos la base de datos para:

-   Eliminar tareas y objetivos
-   Cambiar el estado de una tarea
-   Guardar una nueva tarea u objetivo

Lo importante es que, para las operaciones de eliminación y cambio de estado, realizamos estas acciones de forma _optimista_. Es decir, asumimos que el cambio se realizará correctamente en el servidor, así que actualizamos la UI inmediatamente y solo revertimos al estado original si la API devuelve un error. Usar actualizaciones optimistas es mejor porque proporciona una experiencia más realista y dinámica al usuario.

Thunk
-----

Actualmente, el código para eliminar una tarea se ve así:

```js
removeItem(item) {
    const { dispatch } = this.props.store

    dispatch(removeTodoAction(item.id))

    return API.deleteTodo(item.id)
        .catch(() => {
            dispatch(addTodoAction(item))
            alert('Ha ocurrido un error. Inténtalo de nuevo.')
        })
    }
}
```

Aquí estamos mezclando el código específico del componente con el código específico de la API. Si movemos la lógica de obtención de datos desde nuestro componente al action creator, nuestro método `removeItem()` final podría verse así:

```js
removeItem(item) {
    const { dispatch } = this.props.store

    return dispatch(handleDeleteTodo(item))
}
```

El action creator `handleDeleteTodo` hace una petición asíncrona antes de devolver la acción. Aquí no podemos devolver una promesa porque cada action creator debe devolver un _objeto_:

```js
function asyncActionCreator (id) {
    return {
        type: ADD_USER,
        user: ??
    };
}
```

Para resolver esto, debemos combinar nuestros conocimientos de programación funcional con nuestros conocimientos sobre el middleware de Redux. Recuerda que el middleware se sitúa _entre_ el despacho de una acción y la ejecución del reducer. El reducer espera recibir un objeto de acción, pero en lugar de devolver un objeto, podemos devolver una función.

Así pues, podemos usar el middleware para comprobar si la acción devuelta es una función o un objeto. Si es un objeto, las cosas funcionarán normalmente. Sin embargo, si la acción es una función, el middleware puede invocarla y pasarle cualquier información que necesite (por ejemplo, una referencia al método `dispatch()`). Esta función podría hacer lo que necesite, como realizar peticiones de red asíncronas, y luego puede despachar una acción _diferente_ que devuelva un objeto normal cuando haya terminado.

Un action creator que devuelve una función puede verse así:

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

Observa que ya no estamos devolviendo la acción en sí. En su lugar, devolvemos una función que recibe dispatch como parámetro. Luego llamamos a esta función cuando tenemos los datos.

Ahora bien, esto no funcionará de inmediato, pero hay buenas noticias: podemos añadir middleware a nuestra app para soportarlo. Añadamos la librería [redux-thunk](https://github.com/gaearon/redux-thunk):

```js
<script src="https://unpkg.com/redux-thunk@2.2.0/dist/redux-thunk.min.js"></script>
```

### Beneficios de los Thunks

Por defecto, el store de Redux solo soporta el flujo _síncrono_ de datos. Middleware como **thunk** ayuda a soportar la _asincronía_ en una aplicación Redux. Puedes pensar en thunk como un envoltorio para el método `dispatch()` del store: en lugar de devolver objetos de acción, podemos usar action creators thunk para despachar funciones (o incluso Promesas).

Sin thunks, los despachos síncronos son el comportamiento predeterminado. _Podríamos_ seguir haciendo llamadas a la API desde los componentes React (por ejemplo, usando el método del ciclo de vida `componentDidMount()` para hacer estas peticiones), pero usar el middleware thunk nos da una separación de responsabilidades más limpia. Los componentes no necesitan manejar lo que sucede después de una llamada asíncrona, ya que la lógica de la API se _traslada_ de los componentes a los action creators. Esto también favorece una mayor previsibilidad, ya que los action creators se convierten en la fuente de cada cambio de estado. Con thunks, despachamos una acción solo cuando la petición al servidor se resuelve.

> ### Orden de ejecución con el middleware Thunk
>
> Esperamos que la petición a la API ocurra primero. `TodoAPIUtil.fetchTodos()` debe resolverse antes de que se pueda hacer cualquier otra cosa. Una vez que la petición se resuelve, el middleware thunk invoca la función con `dispatch()`. Ten en cuenta: la acción solo se despacha _después_ de que la petición a la API se haya resuelto.

En resumen, si una aplicación web requiere interacción con un servidor, aplicar middleware como **thunk** ayuda a resolver el problema del flujo de datos asíncrono. El middleware thunk nos permite escribir action creators que devuelven _funciones_ en lugar de objetos.

Al llamar a nuestra API en un _action creator_, hacemos que el action creator sea responsable de obtener los datos que necesita para crear la acción. Dado que movemos el código de obtención de datos a los action creators, construimos una separación más limpia entre nuestra lógica de UI y nuestra lógica de obtención de datos. Como resultado, los thunks pueden usarse para retrasar el despacho de una acción, o para despachar solo si se cumple una determinada condición (por ejemplo, que una petición se haya resuelto).

Aprovechando los Thunks en nuestra app
--------------------------------------

Convertir a thunks mejora las responsabilidades del código. Asegúrate de marcar cada uno de los siguientes puntos:

-   Convertir los objetivos (Goals) para usar thunks
-   Convertir las tareas (Todos) para usar thunks
-   Convertir la obtención de los datos iniciales para usar thunks

### Más opciones asíncronas

Las preguntas más comunes que recibo sobre este curso giran en torno a temas más avanzados de obtención de datos con Redux. Me he resistido a tratarlos porque normalmente introducen _mucha_ complejidad, y los beneficios no se perciben hasta que tus necesidades de obtención de datos son lo suficientemente grandes. Dicho esto, ahora que tienes una base sólida en Redux y, específicamente, en Redux asíncrono, estarás en una buena posición para leer sobre las diferentes opciones y decidir si alguna funcionaría mejor para el tipo de aplicación en la que estás trabajando. Te animo a que investigues ambas opciones (populares).

-   [Redux Promise](https://github.com/redux-utilities/redux-promise) - Middleware de promesas compatible con FSA para Redux.
-   [Redux Saga](https://github.com/redux-saga/redux-saga) - Un modelo alternativo de efectos secundarios para aplicaciones Redux.

Hemos usado la librería thunk que instalamos en la sección anterior para hacer nuestro código más enfocado y mantenible.
