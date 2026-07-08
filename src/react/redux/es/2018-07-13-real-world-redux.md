Redux en el Mundo Real
=======================

Hasta ahora hemos cubierto todas las características de Redux construyendo una aplicación simple de todos y metas. Este proyecto fue excelente para aprender, pero es algo rudimentario y no representa las complejidades de un proyecto real. Así que usemos:

- React
- Redux
- Bindings de React Redux
- Middleware de Redux

Para construir una aplicación compleja del mundo real. El objetivo es construir un [redux-twitter](https://tylermcginnis.com/projects/redux-twitter/) con estas características particulares:

- Escribir un nuevo tweet
- Responder a un tweet
- Dar like a un tweet
- Persistir todas las interacciones de los tweets

Todo el desarrollo estará en la carpeta [reactnd_chirper_app_master](https://github.com/suabochica/react-nanodegree/tree/master/react_redux/reactnd_chirper_app_master).

Recorrido del Proyecto
----------------------

Para ayudarte a solidificar tu comprensión de React y Redux, haremos un recorrido por el proyecto. El proyecto que construiremos se llama "Chirper". Construir este clon simple de Twitter te ayudará a practicar la mejora de la predecibilidad del estado de una aplicación; establecer reglas estrictas para obtener, escuchar y actualizar el store; e identificar qué estado debería vivir dentro de Redux y qué estado dentro de los componentes de React.

Como ocurre con la mayoría de las cosas, hay más de una forma correcta de lograr un resultado exitoso. Discutiremos un enfoque para construir un proyecto React/Redux. Te animamos a que encuentres un enfoque que funcione para ti. Independientemente del enfoque que elijas, asegúrate siempre de planificar la arquitectura de tu proyecto _antes_ de empezar a codificar.

### La Importancia de Planificar tu Proyecto

Muchos desarrolladores cometen el error de empezar a codificar antes de haber pensado en cuál debería ser la arquitectura de su aplicación. Este enfoque resulta en pasar una cantidad increíble de tiempo depurando, reestructurando el código, ¡y a veces incluso empezando desde cero!

Confía en nosotros, planificar tu proyecto antes de empezar a codificar te ahorrará mucho tiempo después.

En nuestro proyecto Chirper, repasaremos tanto las etapas de planificación como las de codificación del proyecto.

### Planificación de la Arquitectura de tu App React/Redux

En la Etapa de Planificación, repasaremos 4 pasos que te ayudarán a definir la arquitectura de tu aplicación, que suele ser la parte más complicada.

1. Identificar cómo debería verse cada vista
2. Dividir cada vista en una jerarquía de componentes
3. Determinar qué eventos ocurren en la aplicación
4. Determinar qué datos viven en el store

### Codificación por Etapas

Construiremos el proyecto juntos, desglosando cada fase del desarrollo. Lo primero que haremos será observar las diferentes vistas que debería tener el proyecto final.

¡Manos a la obra!

Paso 1: Identificar Cada Vista
------------------------------

Necesitamos determinar el aspecto y la funcionalidad de cada vista en tu aplicación. Uno de los mejores enfoques es dibujar cada vista de la aplicación en papel para que tengas una buena idea de qué información y datos planeas tener en cada página.

> En lugar de papel y lápiz, puedes usar [software para crear mockups](https://codingsans.com/blog/mockup-tools).

### Vista de la Página de Dashboard

#### Requisitos de la Vista Dashboard
- Está ubicada en la ruta principal (`/`)
- Muestra los tweets ordenados desde el más reciente hasta el más antiguo
- Cada tweet mostrará:
    - El autor
    - La marca de tiempo
    - A quién está respondiendo el autor
    - El texto del tweet
    - Un botón de respuesta –con el número de respuestas–
    - Un botón de like –con el número de likes–

### Vista de la Página de Tweet

#### Requisitos de la Vista Tweet
- Está ubicada en `/tweet/:id`
- Muestra un tweet individual
    - El autor
    - La marca de tiempo
    - Un botón de respuesta –con el número de respuestas–
    - Un botón de like –con el número de likes–
- Tiene un formulario de respuesta
- Muestra todas las respuestas

### Vista para Crear un Nuevo Tweet

#### Requisitos de la Vista Nuevo Tweet
- Está ubicada en `/new`
- Tiene un cuadro de texto para agregar un nuevo tweet

### Resumen de Vistas

Estas son las 3 vistas en nuestra aplicación:
- Dashboard
- Tweet
- Nuevo Tweet

Ahora tenemos una idea clara de lo que estamos tratando de construir y podemos estar seguros de que nuestras vistas cumplen con todos los requisitos establecidos.

Paso 2: Dividir Cada Vista en una Jerarquía de Componentes
----------------------------------------------------------

Hagamos dos cosas:

- Dibujar recuadros alrededor de cada componente
- Organizar nuestros componentes en una jerarquía

> Para determinar si algo debería ser un componente, debes seguir el **Principio de Responsabilidad Única**.

### Componentes para la Vista Dashboard
- **App** — El contenedor general del proyecto
- **Navigation** — Muestra la navegación
- **Tweets List** — Responsable de la lista completa de tweets
- **Tweet** — Encargado de mostrar el contenido de un tweet individual

### Componentes para la Vista Tweet
- **App** — El contenedor general del proyecto
- **Navigation** — Muestra la navegación
- **Tweets Container** — Muestra una lista de tweets
- **Tweet** — Muestra el contenido de un tweet individual
- **New Tweet** — Muestra el formulario para crear un nuevo tweet (respuesta)

### Componentes para la Vista Nuevo Tweet
- **App** — El contenedor general del proyecto
- **Navigation** — Muestra la navegación
- **New Tweet** — Muestra el formulario para crear un nuevo tweet

### Todos los Componentes

Así que, según cómo dividí las cosas, la aplicación tendrá los siguientes componentes:

- App
- Navigation
- Tweet List
- Tweet Container
- Tweet
- New Tweet

Esta jerarquía de componentes nos indica qué componentes se usarán dentro de otros componentes. Nos proporciona el esqueleto de nuestra aplicación. Todos estos son componentes de presentación. Por ahora, no nos importa qué componentes se actualizarán a contenedores. A medida que empecemos a construir el store, crearemos componentes adicionales que serán componentes contenedores para obtener datos del store y pasarlos a los componentes de presentación que los necesiten.

Hasta ahora, no hemos hecho nada que sea específico de Redux; todos los pasos anteriores son aplicables y útiles para aplicaciones React que no usan Redux.

Recuerda que a Redux no le importa _cómo_ se ve nuestra aplicación ni qué componentes usa. En cambio, ofrece una forma de gestionar el _estado_ de la aplicación de manera predecible. Cuando hablamos de _estado_, realmente hablamos de _datos_ —no cualquier tipo de datos dentro de la aplicación, sino datos que pueden cambiar según los eventos en la aplicación.

Paso 3: Determinar los Eventos en la App
-----------------------------------------

Necesitamos observar _qué_ está sucediendo en cada componente. Determinemos qué acciones está realizando la aplicación o el usuario **sobre los datos**. ¿Se están estableciendo, modificando o eliminando los datos? Entonces necesitaremos una acción para hacer seguimiento de ese evento.

Pongamos en _cursiva_ la acción y **negrilla** el dato.

### Componente Tweets List

Para el componente Tweets List, la única información que vemos es que tendremos que obtener una lista de todos los tweets. Así que para este componente, solo necesitamos:

- _Obtener_ los **tweets**

Por lo tanto, el tipo de acción para este evento sería probablemente `GET_LIST_OF_TWEETS`.

### Componente Tweet

- _Obtenemos_ un tweet en particular de una lista de **tweets**
- _Obtenemos_ el **authedUser (usuario que ha iniciado sesión)** para que el usuario pueda _alternar_ los likes en cada **tweet**
- _Obtenemos_ el **authedUser** para que el usuario pueda _responder_ a un **tweet**

### Componente Tweet Container

- _Obtenemos_ un tweet específico de una lista de **tweets**
- _Obtenemos_ las respuestas a un tweet específico de una lista de **tweets**

### Componente New Tweet

- _Obtenemos_ el **authedUser**, para que el usuario pueda _crear_ un nuevo **tweet**
- _Establecemos_ el **texto del nuevo tweet**

Paso 4: Datos y el Store
-------------------------

Recuerda que los principales problemas que Redux (y los bindings de react-redux) pretendía resolver eran:

- Propagación de props a través de todo el árbol de componentes
- Garantizar la consistencia y predecibilidad del estado en toda la aplicación

Según Dan Abramov, el creador de Redux, deberíamos seguir el principio para determinar si almacenar un dato en el store o en un componente de React:

> Usa Redux para el estado que importa globalmente o que se muta de formas complejas... La regla general es: _haz lo que sea menos incómodo_.

Para cada dato del Paso 3, veamos si es utilizado por múltiples componentes o se muta de forma compleja.

### _Texto del nuevo tweet usado por:_ Componente New Tweet

Este dato no es utilizado por múltiples componentes ni se muta de forma compleja. Eso significa que es un excelente candidato para el estado del componente en lugar del estado de la aplicación que reside en el store.

### _Tweets usados por:_ Componente Dashboard, Componente Tweet Page y Componente Tweet

En el Componente Tweet Page, necesitamos mostrar los tweets de respuesta. Echemos un vistazo al código inicial en `_Data.js`:

```js
let tweets = {
    tweetId: {
        id: tweetId,
        text: tweetText,
        author: userId,
        timestamp: timestamp,
        likes: [userId1, userId2],
        replies: [tweetId1, tweetId2],
        replyingTo: tweetId_OR_null
    }
};

```

Para obtener los tweets de respuesta, podemos obtener el tweet con un id específico de la lista de todos los tweets y acceder a su propiedad `replies`.

En el **Componente Dashboard** necesitamos acceder a la lista actual de tweets. Si el componente Dashboard conoce el ID del tweet que debe mostrarse, puede simplemente pasar un ID al Componente Tweet, que renderizará el tweet.

En el **Componente Tweet**, necesitamos seleccionar un tweet con un ID específico de la lista actual de tweets.

Eso significa que podemos almacenar los tweets en el store y convertir el Componente Dashboard, el Componente Tweet Page y el Componente Tweet en contenedores —componentes que tienen acceso al store a través de la función `connect()`.

Tan pronto como los datos cambien (por ejemplo, alguien da like a un tweet), todos los componentes que usan esos datos se actualizarán.

Ten en cuenta que cada tweet contiene el nombre del autor y su avatar. Una forma de modelar el estado es:

```js
tweets: {
    tweetId: {tweetId, authorId, authorName, authorAvatar, timestamp, text, likes, replies, replyingTo},
    tweetId: {tweetId, authorId, authorName, authorAvatar, timestamp, text, likes, replies, replyingTo}
}
```

Modelar el estado de esta manera no es incorrecto, pero es inconveniente si queremos extender la funcionalidad de la aplicación en el futuro para poder encontrar tweets hechos por un autor en particular. Además, esta forma de almacenar los datos mezcla dos tipos de objetos:

- datos de tweets
- datos de usuarios

Esto va en contra de la recomendación de normalizar los estados. Según la documentación de Redux, estos son los principios de normalización del estado:

- Cada tipo de datos obtiene su propia "tabla" en el estado.
- Cada "tabla de datos" debe almacenar los elementos individuales en un objeto, con los IDs de los elementos como claves y los elementos mismos como valores.
- Cualquier referencia a elementos individuales debe hacerse almacenando el ID del elemento.
- Los arrays de IDs deben usarse para indicar orden.

Entonces, el estado normalizado de la aplicación se vería así:

```js
{
    tweets: {
        tweetId: { tweetId, authorId, timestamp, text, likes, replies, replyingTo},
        tweetId: { tweetId, authorId, timestamp, text, likes, replies, replyingTo}
    },
    users: {
        userId: {userId, userName, avatar, tweetsArray},
        userId: {userId, userName, avatar, tweetsArray}
    }
}
```

### _authedUser usado por:_ Componente Tweet y Componente New Tweet

Cada Componente Tweet necesita mostrar si el usuario que ha iniciado sesión ha dado like a un tweet. Para hacer eso, necesitamos saber quién es el usuario logueado. Observando la Jerarquía de Componentes del Paso 2, sabemos que el Componente Tweet es utilizado por múltiples componentes. Por lo tanto, necesitamos actualizar este componente a un contenedor para que pueda acceder al dato `authedUser` del store y ver si debe mostrar un corazón rojo.

También sabemos que para cada nuevo tweet, tendremos que registrar quién es el autor del tweet. La forma de React de almacenar el estado es poner el estado en el componente padre más arriba en la jerarquía y luego pasarlo hacia abajo a todos los hijos que lo necesiten. En esta aplicación, eso significaría almacenarlo en el Componente App.

Una forma de hacerlo es almacenar el `authedUser` en el Componente App y luego pasarlo hacia abajo a los componentes que necesiten acceso. Aunque esto funciona, es incómodo. Sería más simple almacenar el `authedUser` en el store y luego proporcionar al Componente Tweet acceso al store. El Componente New Tweet podría entonces simplemente despachar una acción con el texto del nuevo tweet y el id del tweet al que estamos respondiendo como parámetros para guardar el nuevo tweet.

Guardar un tweet es una operación asíncrona; podríamos usar Redux Thunks para hacerlo. Los Thunks nos dan acceso al store, por lo que podríamos tener el siguiente creador de acciones:

```js
function handleAddTweet(text, replyingTo) {
    return (dispatch, getState) => {
        const { authedUser } = getState();

        return saveTweetToDatabase({
            text,
            author: authedUser,
            replyingTo
        }).then(tweet => dispatch(addTweet(tweet)));
    };
}
```

Generalmente, acceder al store desde un creador de acciones se considera un antipatrón. Dan Abramov dice que los pocos casos de uso en los que es aceptable hacerlo son:

> Para verificar datos antes de hacer una solicitud o para verificar si estás autenticado (es decir, hacer un dispatch condicional).

Otra razón para mantener el dato `authedUser` en el store es que si extendemos nuestra aplicación para incluir la capacidad de iniciar y cerrar sesión, esta funcionalidad sería fácil de gestionar con Redux.

El Componente New Tweet no necesita acceder al dato `authedUser`, pero _sí_ necesita poder despachar una acción para que el reducer sepa que se ha creado un nuevo tweet. Para tener acceso al método `dispatch()`, un componente debe estar conectado al store. En otras palabras, debe ser un contenedor. Así que sabemos que el Componente Tweet y el Componente New Tweet se actualizarán a contenedores. Finalmente, obtenemos:

| **El Store**   |
|:--------------:|
|     Tweets     |
|     Users      |
|   authedUser   |

¡Nuestro store está listo! Mientras creábamos nuestro store, también determinamos qué componentes se actualizarían a contenedores, por lo que nuestro esqueleto de aplicación ahora está aún más completo. Es hora de empezar a codificar.

Acciones
--------

Comencemos con la Vista Dashboard que muestra una lista de tweets y un menú.

Necesitamos observar _qué_ está sucediendo en esta vista. Determinemos qué acciones está realizando la aplicación o el usuario **sobre los datos** — ¿se están estableciendo, modificando o eliminando los datos?

Cuando la aplicación se carga, se muestra la Vista Dashboard. Por lo tanto, el Componente Dashboard necesita:

- _Obtener_ los **tweets**
- _Obtener_ los **users**
- _Obtener_ el **authedUser**

Estos datos se almacenan en una base de datos. Para que esta vista cargue todos los tweets —incluyendo los avatares de sus autores— necesitamos:

1. Obtener los datos de `tweets` y `users` desde la base de datos
2. Luego, pasar esos datos al componente

Como buena práctica en aplicaciones React, debes hacer la solicitud a la API en el método de ciclo de vida `componentDidMount()`, y en aplicaciones React/Redux debes hacer las solicitudes a la API desde creadores de acciones asíncronos.

Los Creadores de Acciones devuelven acciones (es decir, objetos simples de JavaScript que luego van a todos nuestros reducers). Hacer una solicitud a la API es una acción asíncrona, por lo que no podemos simplemente enviar un objeto simple de JavaScript a nuestros reducers. El middleware de Redux puede obtener acceso a una acción cuando está en camino a los reducers. Usaremos el middleware `redux-thunk` en este ejemplo.

Si el middleware Redux Thunk está habilitado —lo cual se hace mediante la función `applyMiddleware()`—, entonces cada vez que tu creador de acciones devuelva una función en lugar de un objeto JavaScript, irá al middleware redux-thunk.

Dan Abramov describe lo que sucede a continuación:

> "El middleware llamará a esa función con el método dispatch como primer argumento. La acción solo llegará a los reducers una vez que se complete la solicitud a la API. También _tragará_ tales acciones, así que no te preocupes porque tus reducers reciban argumentos de función extraños. Tus reducers solo recibirán acciones de objeto simple, ya sea emitidas directamente o emitidas por las funciones como acabamos de describir."

Aquí se ve cómo es un creador de acciones thunk:

```js
function handleInitialData () {
    return function (dispatch) {}
}
```

Que es equivalente a esto en ES6:

```js
function handleInitialData () {
    return (dispatch) => {}
}
```

Ahora, necesitamos dar a nuestros componentes acceso a los datos que llegaron. Esto significa que necesitamos poblar el store con `tweets` y `users`.

| **El Store**                                                                         |
|--------------------------------------------------------------------------------------|
| **Tweets:** El resultado de una acción de tweets pasando por su reducer de tweets    |
| **Users:** El resultado de una acción de users pasando por su reducer de users       |
| **authedUser:** El resultado de una acción de authedUser pasando por su reducer       |

Reducers
--------

Un Reducer describe _cómo_ cambia el estado de la aplicación. A menudo verás el Operador de Propagación de Objetos (`...`) usado dentro de un reducer porque un reducer **debe devolver un nuevo objeto** en lugar de mutar el estado anterior. Los Reducers tienen la siguiente firma:

```js
(previousState, action) => newState
```

La última tabla ilustra cómo la aplicación modificará el estado.

### Inicializando el Estado

Hay 2 formas de inicializar el estado dentro del store:

- Puedes pasar el estado inicial (o parte de él) como `preloadedState()` a la función `createStore()`. Por ejemplo:

```js
const store = createStore (
    rootReducer,
    { tweets: {} }
)
```

- Puedes incluir un parámetro de estado predeterminado como primer argumento dentro de una función reducer particular. Por ejemplo:

```js
function tweets (state = {}, action) {
    // cuerpo del reducer
}
```

En la aplicación, inicializamos cada porción del store estableciendo un valor de `state` predeterminado como primer parámetro dentro de cada función reducer (opción dos). Así que la porción **tweets** del estado en el store se ha inicializado como un objeto vacío. La porción **users** del estado en el store se ha inicializado como un objeto vacío. Y la porción **authedUser** del estado en el store se ha inicializado como null.

Por lo tanto, tenemos un reducer `tweets` para gestionar la porción _tweets_ del estado, un reducer `users` para gestionar la porción _users_ del estado, y un reducer `authedUser` para gestionar la porción _authedUser_ del estado. Cada uno de estos reducers gestionará solo su propia parte del estado. Combinaremos todos estos reducers en un reducer principal raíz, que combinará los resultados de llamar al reducer `tweets`, al reducer `users` y al reducer `authedUser` en un solo objeto de estado. Recuerda, necesitamos hacer esto porque la función `createStore()` solo acepta un único reducer.

```js
combineReducers({
    authedUser: authedUser,
    tweets: tweets,
    users: users
})
```

O usando la abreviatura de propiedad de ES6, puede ser simplemente:

```js
combineReducers({
    authedUser,
    tweets,
    users
})
```

Ahora que todos nuestros reducers están configurados, necesitamos crear el store y proporcionarlo a nuestra aplicación. Para usar realmente cualquier código que hayamos escrito hasta este punto, necesitamos instalar el paquete `redux`. Luego, para proporcionar el store a nuestra aplicación, también necesitaremos instalar el paquete `react-redux`.

Las aplicaciones Redux tienen un único store. Tenemos que pasar el Root Reducer a nuestra función `createStore()` para que el store sepa qué piezas de estado debe tener. El objetivo de crear un store es permitir que los componentes puedan acceder a él sin tener que pasar los datos a través de múltiples componentes.

El componente `Provider` (que viene del paquete react-redux) hace posible que todos los componentes accedan al store a través de la función `connect()`.

### Middleware

La última configuración implica configurar las funciones de Middleware de la aplicación. Creemos un middleware _logger_ que nos ayude a ver la acción y el estado del store mientras interactuamos con nuestra aplicación. Además, dado que el creador de acciones `handleInitialData()` en `share.js` devuelve una función, necesitaremos instalar el paquete `redux-thunk`.

Conectemos nuestro middleware `redux-thunk` para que nuestros creadores de acciones thunk funcionen realmente. Pondremos el middleware logger para facilitar la depuración. Todo middleware sigue este patrón de currying:

```js
const logger = (store) => (next) => (action) => {
    // ...
}
```

La variable `logger` se asigna a una función que toma el `store` como argumento. Esa función devuelve otra función, a la que se le pasa `next` (que es el siguiente middleware en la línea o la función dispatch). Esa otra función devuelve otra función a la que se le pasa una `action`. Una vez dentro de esa tercera función, tenemos acceso a `store`, `next` y `action`.

Es importante notar que el valor del parámetro `next` será determinado por la función `applyMiddleware()`. Todo el middleware se llamará en el orden en que se liste en esa función. En nuestro caso, `next` será `dispatch` porque `logger` es el último middleware listado en esa función. Aquí está la conexión del middleware:

```js
export default applyMiddleware(
    thunk,
    logger
);
```

Cada cosa devuelta por un creador de acciones —ya sea una acción o una función— pasará por el middleware thunk. El siguiente fragmento es el código fuente del middleware `thunk`:

```js
function createThunkMiddleware(extraArgument) {
    return ({ dispatch, getState }) => next => action => {
        if (typeof action === 'function') {
            return action(dispatch, getState, extraArgument);
        }
        return next(action);
    };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

Si el middleware thunk ve una _acción_, esa acción se enviará al siguiente middleware en la línea —el middleware logger—. Si ve una _función_, el middleware `thunk` llamará a esa función. Esa función puede contener efectos secundarios —como llamadas a API— y despachar acciones, objetos simples de JavaScript. Estas acciones despachadas pasarán nuevamente por todo el middleware. El middleware thunk verá que es una acción simple y la pasará al siguiente middleware, el logger.

Una vez dentro del logger:

```js
const logger = store => next => action => {
    console.group(action.type);
        console.log("The action:", action);
        const returnValue = next(action);
        console.log("The new state:", store.getState());
    console.groupEnd();

return returnValue;
};
```

Por lo tanto, el orden en que pasas tus argumentos dentro de la función `applyMiddleware()` importa.

Inicializando los Datos de la App
----------------------------------

Hemos determinado previamente que necesitamos obtener los datos de `users` y `tweets` de nuestra base de datos y enviar esos datos a nuestro store, junto con el dato `authedUser`, cuando la página principal se cargue.

También hemos creado un creador de acciones thunk que obtiene los datos de la base de datos y luego despacha acciones al store para establecer las tres piezas de estado que tenemos en nuestro store:

- `users`
- `tweets`
- `authedUser`

Así es como se ve el creador de acciones thunk `handleInitialData()`:

```js
function handleInitialData () {
    return (dispatch) => {
        return getInitialData()
        .then(({ users, tweets }) => {
            dispatch(receiveUsers(users));
            dispatch(receiveTweets(tweets));
            dispatch(setAuthedUser(AUTHED_ID));
        });
    };
}
```

Ahora la pregunta es _dónde_ despachamos este creador de acciones. Cuando recorrimos la arquitectura de la aplicación, vimos que el Componente _App_ contendrá todos los demás componentes. Si cargamos los datos iniciales desde el Componente _App_, entonces no importa a qué ruta vaya nuestro usuario, verá todos los datos correctos. Así que la respuesta es el Componente App.

Usar la función `connect()` actualiza un componente a contenedor. Los contenedores pueden leer el estado del store y despachar acciones.

Componente Dashboard
--------------------

En la aplicación, el estado normalizado se vería así:

```js
{
    tweets: {
        tweetId: { id del tweet, id del autor, timestamp, text, likes, replies, replyingTo},
        tweetId: { id del tweet, id del autor, timestamp, text, likes, replies, replyingTo}
    },
    users: {
        userId: { id del usuario, nombre del usuario, avatar, array de tweets},
        userId: { id del usuario, nombre del usuario, avatar, array de tweets}
    }
}
```

En la Etapa de Planificación, también determinamos que el Componente Dashboard será un contenedor, ya que necesitará acceso a la parte de `tweets` del store para mostrar la lista de tweets.

Para hacer un contenedor, necesitamos usar la función `connect()`. Recuerda que la firma de la función connect se ve así:

```js
connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
```

Estos detalles sobre `mapStateToProps` y `mapDispatchToProps` son cruciales:

- **`mapStateToProps`**: Si se especifica este argumento, el nuevo componente se suscribirá a las actualizaciones del store de Redux. Esto significa que cada vez que el store se actualice, se llamará a `mapStateToProps`. Los resultados de `mapStateToProps` deben ser un objeto simple, que se fusionará en las props del componente. Si no quieres suscribirte a las actualizaciones del store, pasa `null` o `undefined` en lugar de `mapStateToProps`.

- **`mapDispatchToProps`**: Si se pasa un objeto, se asume que cada función dentro de él es un creador de acciones de Redux. Un objeto con los mismos nombres de función, pero con cada creador de acciones envuelto en una llamada dispatch para que puedan ser invocados directamente, se fusionará en las props del componente. Si se pasa una función, se le dará dispatch como primer parámetro. Depende de ti devolver un objeto que de alguna manera use dispatch para vincular los creadores de acciones a tu manera.

Ahora, en la Jerarquía de Componentes definimos que el Componente Tweet estará dentro del Componente Dashboard. Si el Componente Dashboard conoce el ID del tweet que debe mostrarse, puede simplemente pasar ese ID al Componente Tweet, que renderizará el tweet.

La firma de la función `mapStateToProps` es:

```js
mapStateToProps(state, [ownProps])
```

- `state` es el estado dentro del store
- `ownProps` son las propiedades que se han pasado a este componente desde un componente padre

Como solo nos importa la parte de `tweets` del store, podemos usar destructuring para pasar la parte de `tweets` del estado en el store como parámetro a la función `mapStateToProps()`.

```js
function mapStateToProps( {tweets} ){
    return { tweetIds: Object.keys(tweets) };
}
```

Los puntos importantes a destacar son:
- **tweets** es la porción del estado que le importa a este componente
- **tweetIds** aparecerá como una propiedad en este contenedor

Componente Tweet
----------------

Según nuestro paso 4, el Componente Tweet necesitará acceso a los siguientes datos:

- `users`
- `tweets`
- `authedUser`

Así que el Componente Tweet será un contenedor. Un detalle importante es que en el `<Dashboard />` tenemos que pasar la prop `id` al Componente Tweet: `<Tweet id={id}/>`. Además, la función `mapStateToProps` del Componente Tweet recibirá como segundo argumento (`ownProps`) un objeto que tiene una propiedad `id` con este valor. La función `mapStateToProps` se ve así:

```js
function mapStateToProps ({authedUser, users, tweets}, { id }) {
    const tweet = tweets[id];

    return {
        authedUser,
        tweet: formatTweet(tweet, users[tweet.author], authedUser)
    };
}
```

Observa que `mapStateToProps` acepta dos argumentos:

- el estado del store
- las props pasadas al Componente Tweet

Estamos destructurando ambos argumentos. Del store, estamos extrayendo:

- el dato `authedUser`
- el dato `users`
- el dato `tweets`

Luego obtenemos el `id` de las props pasadas al Componente Tweet. Necesitamos ambos datos —provenientes del estado del store y de las props del componente— para poder determinar qué Tweet debe mostrarse en el Componente Tweet.

El último detalle que tenemos que manejar en la función `mapStateToProps` del Componente Tweet está relacionado con la funcionalidad de respuesta a tweets. Para manejar este escenario, tenemos que agregar una validación para identificar el tweet padre. Así que la función `mapStateToProps` ahora se ve así:

```js
function mapStateToProps ({authedUser, users, tweets}, { id }) {
    const tweet = tweets[id];
    const parentTweet = tweet ? tweets[tweet.replyingTo] : null;

    return {
        authedUser,
        tweet: tweet
        ? formatTweet(tweet, users[tweet.author], authedUser, parentTweet)
        : null
    };
}
```

Ahora estamos obteniendo todos los datos que necesitamos del store. Es hora de construir la UI del Componente Tweet.

### `react-redux-loading`

Esta librería es un conjunto de un componente, un reducer y sus respectivos creadores de acciones que nos permiten mostrar una barra de carga mientras la aplicación recupera todos los datos.

Dando Like a un Tweet
---------------------

En la Etapa de Planificación, descubrimos que necesitábamos dar al Componente Tweet acceso al dato `authedUser` para que el tweet muestre correctamente si el usuario logueado le ha dado like o no, y para que el usuario pueda responder tweets. Además, una vez que el usuario da o quita el like a un tweet, esa información debe reflejarse en el store para que otros componentes muestren los datos correctos.

Necesitaremos escribir un creador de acciones asíncrono, ya que necesitamos registrar si el usuario logueado ha dado like a un tweet no solo en el store, sino también en la base de datos. _¡Redux thunks al rescate!_

Así que podemos crear un creador de acciones thunk como:

```js
function handleToggleTweet (info) {
    return (dispatch) => {
        saveLikeToggle(info)
            .then(() => {
                dispatch(toggleTweet(info));
            })
            .catch((e) => {
                console.warn('Error in handleToggleTweet: ', e);
                alert('There was an error liking the tweet. Try again.');
            });
    };
}
```

El código solo actualiza la UI una vez que recibimos confirmación de que la actualización en el backend fue exitosa. Esto puede hacer que la aplicación parezca lenta. Un enfoque común para las actualizaciones de UI es la Actualización Optimista (Optimistic Updating): actualizar la UI _antes_ de que la acción se registre en el backend para que parezca más rápida.

### Reducer de Like a Tweet

Recuerda que el reducer `tweets` determinará cómo cambia la parte de `tweets` del estado. Al dar o quitar like a un tweet, el estado de ese tweet específico necesita cambiar —ya sea la propiedad `likes` del tweet— necesitará cambiar para incluir al usuario que hizo clic, si está dando like al tweet, o el nombre del usuario deberá eliminarse de la propiedad `likes` del tweet (que es un array de autores).

Componente New Tweet
--------------------

Trabajemos ahora en la lógica de agregar un nuevo tweet. Una vez que el usuario envía un nuevo tweet, debería aparecer en la lista de todos los tweets y agregarse a nuestra base de datos. Dado que este tweet será utilizado por más de un componente, sabemos que queremos asegurarnos de que el store se modifique para reflejar la lista actualizada de tweets. Registrar tweets en una base de datos es una operación asíncrona, por lo que podemos usar **Redux Thunk** para realizar la solicitud a la API.

Con el Componente New Tweet usaremos el estado del componente de React en lugar de Redux, porque es más fácil. Poner el estado del formulario en Redux no nos daría ningún beneficio y, de hecho, sería más complicado que poner los detalles del formulario dentro del estado del componente.

Sabemos que nuestro store se ve así:

```js
{
    tweets: {
        tweetId: { tweetId, authorId, timestamp, text, likes, replies, replyingTo},
        tweetId: { tweetId, authorId, timestamp, text, likes, replies, replyingTo}
    },
    users: {
        userId: {userId, userName, avatar, tweets array},
        userId: {userId, userName, avatar, tweets array}
    },
    authedUser: userId
}
```

Comencemos a trabajar en el Reducer de Nuevo Tweet. ¿Cómo modificaremos el estado para reflejar el nuevo tweet? Este será un proceso de dos partes:

1. El nuevo tweet debe agregarse a la lista de tweets
2. Un tweet existente debe modificarse si el nuevo tweet es una respuesta a otro tweet

En este reducer haremos:

1. Concatenar el nuevo tweet a la lista de tweets existentes. Recuerda que el **Operador de Propagación de Objetos** nos ofrece la forma más concisa de hacerlo.
2. Modificar la propiedad `replies` del tweet al que el nuevo tweet está respondiendo.

En el Paso 2 de la Etapa de Planificación, determinamos que el Componente New Tweet se mostrará dentro del Componente App cuando el usuario vaya a la página `/new` y que estará dentro del Componente Tweet Page cuando el usuario esté en la página `/tweet/:id`.

Cuando el usuario está en la ruta `/new`, el nuevo tweet no estará adjunto a otro tweet. Cuando el usuario está en la ruta `/tweet/:id`, el nuevo tweet estará adjunto al tweet ya mostrado. Observa que la ruta ya contiene el `id` del tweet padre. Podemos simplemente pasar el `id` de la ruta al Componente New Tweet cada vez que estemos creando un tweet de respuesta.

¿Qué sucede cuando alguien hace clic en "Enviar" para agregar un nuevo tweet? El Componente New Tweet necesitará comunicarse con nuestro store. Nos comunicamos con el store despachando acciones. `dispatch` es un método del store. Esto significa que el Componente New Tweet necesita estar `connect()`ado a Redux. Una vez que un componente está conectado al store, tendrá `dispatch` en sus `props`.
