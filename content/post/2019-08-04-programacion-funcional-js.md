+++
title = "Suficiente Programación Functional en JavaScript"
date = 2019-08-03T02:13:50Z
author = "Sergio L. Benítez D."
description = "Breve introdución de programación funcional en JavaScript."
+++

En los últimos años, la industria del desarrollo de software ha presenciado un resurgimiento de la programación funcional. Con el adjetivo _suficiente_ se pretende dar una introducción a la programación funcional en JavaScript sin atascarse en la jerga y la teoría matemática que puede ser intimidante.

En su lugar, se va a dar una introducción accesible al paradigma para obtener la educación y la confianza necesaria para comenzar a utilizar este estilo de programación.

## Tabla de contenidos
+ Funciones de orden superior
+ Inmutabilidad
+ Currying
+ Funciones puras
+ Aplicaciones parciales
+ Programación libre de puntos
+ Composición funcional
+ Orden de argumentos
+ Propiedad asociativa
+ Depuración

>Nota: Esta publicación utiliza el contenido del curso [Just Enough Functional Programming in JavaScript](https://egghead.io/courses/just-enough-functional-programming-in-javascript) dictado por Kyle Shevlin. 

## Funciones de orden superior

Una función de orden superior hace al menos una de las siguientes cosas, o a menudo ambas:

1. Aceptar una función como parámetro
2. Retornar una nueva función

Para demostrar estas propiedades se va a construir una función de orden superior, `withCount()`, que modifica cualquier función que se le pase como parámetro y contará la cantidad de veces que dicha función ha sido llamada.

```js
const withCount = fn => {}
```

Para lograrlo, se va a usar una variable `count`. Por otro lado, se va a devolver una función que utiliza el _rest operator_ de ES6 para recopilar los argumentos que se le pasaron a la función.Dentro del cuerpo de la función que se retorna, se incrementa el conteo y se registra un mensaje que va a imprimir el valor del conteo. Por ultimo, se hace un retorno recursivo de `fn`con los argumentos para volver a ejecutar el proceso descrito anteriormente:

```js
const withCount = fn => {
	let count = 0
	return (...args) => {
		console.log(`Call count: ${++count}`) 
		return fn(...args)
	}
}
```

Ahora que tenemos nuestra función de orden superior `withCount`, se va a crear una función `add` sencilla que sera pasada como parámetro de `withCount`.  `add` recibirá como parámetros a `x` e `y`y retornará la suma de ambos. Por último se va a crear la función `countedAdd`la cual utilizará `withCount` y `add`.

```js
const withCount = fn => {
	let count = 0
	return (...args) => {
		console.log(`Call count: ${++count}`) 
		return fn(...args)
	}
}

const add = (x, y) => x + y
const countedAdd = withCount(add)

```

Tiempo de imprimir diferente usos de `countedAdd` en consola modificando algunos argumentos.

```js
	console.log(countedAdd(1,2)) // Call count: 1
	console.log(countedAdd(2,2)) // Call count: 2
	console.log(countedAdd(3,2)) // Call count: 3
```

¡Ahí Esta!, se demostró que la función `withCount` cumple con las dos condiciones para ser categorizada como una función de orden superior.

## Inmutabilidad

Se va a revisar los conceptos de inmutabilidad, la diferencia entre estructuras de datos mutables e inmutables, y cómo cambiar los datos entre patrones mutables e inmutables. Los datos inmutables son necesarios en programación funcional porque las mutaciones son efectos secundarios. La transformación de los datos no debe afectar la fuente original de los datos, por el contrario debe retornar una nueva fuente con los cambios aplicados.

Las estructuras de datos mutables pueden ser cambiadas posterior a sus creaciones, y las estructuras de datos inmutables no. Las mutaciones puede ser pensadas como un efecto secundario en nuestras aplicaciones. Podemos hacer una demostración con los arreglos de JavaScript.

Se va a crear un arreglo `a` con algunos valors. Luego se va asigna la variable `a` a `b`. Con esta asignación se va a crear una nueva variable con la misma referencia en memoria de `a`. 

```js
const a = [1, 2, 3]
const b = a
console.log(a === b) // true
```

Ahora, si se hace una actualización de la variable `b` agregando un valor al arreglo, y su imprimimos en consola la variable `a` se observara que dicha variable también fue actualizada. Esto sucede porque `a` y `b` no son arreglos diferentes. Ellos están referenciando al mismo arreglo

```js
const a = [1, 2, 3]
const b = a
b.push(4)
console.log(a) // [1, 2, 3]
```

> Este mismo escenario aplica a los objetos.

En conclusión al hacer cambios en una variable, dichos cambios se replican en la otra variable. Esto puede ser un problema en el código. La funcionalidad que opera en `b` cambiaría `a`, incluso si no tuviéramos la intención de hacerlo. Este contexto rompe la pureza de las funciones. Cuando se hacen actualizaciones en los datos el objetivo es devolver nuevas estructuras de datos que contengan todos los elementos del estado anterior de la estructura, además de nuestras actualizaciones. 


Por instancia, `push` es una mutación sobre un arreglo. Como vimos, cambia todas las referencias al mismo arreglo. No obstante, es posible crear una función `push` inmutable. Esta función recibirá el `valor` y un `arreglo`, y retornará un nuevo arreglo usando el _spread operator_ de ES6 para crear un `clone`, y así, agregar el valor al arreglo clonado:

```js
const immPush = value => array => {
	const clone = [...array]
	clone.push(value)

	return clone
}
```

Si se crea nuevamente un arreglo, y hacemos un segundo arreglo en el que agregamos un nuevo valor usando la función `immPush` se observará que la variable `a` no será actualizada y por ende, `a` y `b` no serán iguales, ya que ahora tienen referencias a diferentes arreglos.

```js
const immPush = value => array => {
	const clone = [...array]
	clone.push(value)

	return clone
}

const a = [1, 2, 3]
const b . = push(4)(a)

console.log(a) // [1, 2, 3]
console.log(b) // false
```

> Se pueden crear funcionalidades similares para manejar los cambios con objetos.

### Metáfora: Tomar una bebida de un vaso
Una metáfora que ayuda a entender los conceptos de inmutabilidad es la de tomar un baso de agua. Para ello se van a crear dos clases, un vaso mutable y un vaso inmutable.

Se va a crear un método `takeDrink` en ambas clases, y se va a mostrar la diferencia entre manejar mutabilidad e inmutabilidad. Comencemos con `MutableGlass`. Esta clase tendrá un `constructor` que tomará un contenido y una cantidad, y hará las asignaciones respectivas a través del contexto de `this`.

```js
class MutableGlass {
	constructor(content, amount) {
		this.content = content
		this.amount = amount
	}
}
```

Tiempo de crear el método `takeDrink` que va a tomar un valor como parámetro. En esta clase se va a actualizar `this.amount` directamente y se va a retornar la instancia `this` del vaso. `this.amount` igual a `Math.max` se utiliza para garantizar que el vaso nunca va a tener una cantidad menor a cero, ya que no se puede tomar la cantidad que no está en el vaso. Por último se devuelve `this`.

```js
class MutableGlass {
	constructor(content, amount) {
		this.content = content
		this.amount = amount
	}

	takeDrink(value) {
		this.amount = Math.max(this.amount - value, 0)

		return this
	}
}
```

Se crea un vaso `mg1` que va a ser igual a una nueva instancia de `MutableGlass`.  Se define como contenido `water` y una cantidad de `100`

```js
const mg1 = new MutableGlass('water', 100)
```

Ahora, si se almacena el valor retornado después de tomar una bebida en una variable `mg2`, se puede observar que `mg1` y `mg2` son la misma instancia a través de una comparación estrictamente igual.

```js
const mg1 = new MutableGlass('water', 100)
const mg2 = mg1.takeDrink(20)
console.log(mg1 === mg2) // true
console.log(mg1.amount === mg2.amount) //true
```

Esto se debe a que este contexto maneja mutabilidad. El valor se cambia directamente, y por tanto la estructura de datos se cambia después de su creación. 

Revisemos el escenario de la clase `Immutable Glass`. El `constructor` será él mismo y va a recibir un contenido y una cantidad. Se va a crear un método `takeDrink`que también recibe un valor como parámetro pero un gran cambio ocurrirá en su contenido:

```js
class ImmutableGlass {
	constructor(content, amount) {
		this.content = content
		this.amount = amount
	}

	takeDrink(value) {
		return new ImmutableGlass
	}
}
```

Cuando se tome una bebida, en vez de regresar el mismo vaso que teníamos antes, se va a retornar un nuevo vaso con un nuevo contenido y la cantidad respectiva. Este cambio se refleja en el `new ImmutableGlass`.

Sin embargo, la versión anterior está incompleta ya que no contempla el contenido y la cantidad.  En consecuencia, se va a pasar el `content` ya que debe ser el mismo (no vamos a convertir agua en vino :p). En cuanto al `amount`, se hará el mismo cálculo de la versión mutable. El resultado es el siguiente:

```js
class ImmutableGlass {
	constructor(content, amount) {
		this.content = content
		this.amount = amount
	}

	takeDrink(value) {
		return new ImmutableGlass(this.content, Math.max(this.amount - value, 0))
	}
}
```

Ahora cuando se creen vasos y se hagan sus respectivas actualizaciones, sus instancias ya no serán las mismas. Para ilustrar, se va a crear una variable `ig1` con una instancia de `ImmutableGlass`. Nuevamente el contenido será `water` y la cantidad `100`. Entonces, si se almacena el vaso después de tomar una bebida en una variable `ig2`  con un consumo de `20`  se observará que `ig1` e `ig2` ya no tienen la misma instancia.

```js
const ig1 = new ImmutableGlass('water', 100)
const ig2 = ig1.takeDrink(20)
console.log(ig1 === ig2) // false
console.log(ig1.amount === ig2.amount) // false
```

Con estos resultados se cierra la metáfora de tomar una bebida de un vaso de agua y se explican las diferencias entre escenarios mutables e inmutables.

## Currying

Currying es el acto de tomar una función que normalmente recibe más de un argumento, y se hace una refactorización para recibir solo un argumento a la ves. Un concepto relacionado con currying es la _aridad_, la cual es el numero de argumentos que recibe una función y es bastante útil para el conocimiento de programación funcional.

Para ilustrar currying vamos a hacer un refactorización sobre la siguiente función `add`

```js
function add(x, y) {
	return x + y
}
```

Ahora retomando la primera definición de currying, debemos hacer que esta función solo acepte un argumento a la vez. Es ta condición convierte la función `add` en una función de orden superior. Es importante tener en cuenta que la función `add` ahora se va a evaluar una ver se reciba el último argumento.

```js
function add(x) {
	return function(y) {
		return x + y
	}
}
```

Si se trata de imprimir en consola la nueva función add vamos a tener la siguiente respuesta:

```js
conosle.log(add(3,4)) // [Function]
```

Esto ocurre porque el segunda variable es ignorada y se recibe la función correspondiente al primer `return`. En vez de este llamado, lo que se hace es guardar la función retornada en una variable. Llamemos esta función `addThree`. Ahora se puede tomar nuestra función `add` y pasar el último argumento.


```js
function add(x) {
	return function(y) {
		return x + y
	}
}

const addThree = add(3)

console.log(addThree(4)) 	// 7
console.log(addThree(10))	// 13
console.log(addThree(18))	// 21
```

Podemos usar curry en las funciones sucintamente usando las _arrow function_ de ES2015. Las arrow function implícitamente retornan cada expresión que viene después de la flecha `=>`. De este modo nuestro función `add` sería:

```js
const addArrow = x => y => x + y
```

En este punto se va a introducir algo de jerga con la palabra **aridad**. Aridad describe el número de argumentos que una función recibe. Dependiendo del número de argumento que reciba, hay palabras específicas para describir estas funciones:

1. 1 argumento: función unaria
2. 2 argumentos: función binaria
3. 3 argumentos: función ternaria
4. ...

## Funciones Puras
Una _función pura_ es una función que deriva su salida únicamente de sus entradas y no causa efectos secundarios en la aplicación o en le mundo exterior Las funciones puras más comunes que las personas han encontrado son la funciones matemáticas.

```js
// f(x) = x + 1
const f = x => x + 1
```
Es fácil razonar que esta es una función pura. Obtenemos la misma salida dada la entrada, y no hay un mundo exterior para que podamos causar un efecto secundario. La mejor forma de consolidar el concepto de funciones puras es haciendo comparaciones con funciones impuras, a continuación se aglomeran una serie de ejemplos de funciones impuras.

### Estado global
La primera función impura que se va a revisar es una cuya salidas no derive únicamente de sus entradas. Considere una función 	`carTotal`. Por alguna razón se decidió que uno de los precios necesita una constante global. La función `carTotal` tomará una cantidad y la multiplicará contra esta constante.

```js
const COST_OF_ITEM = 19
const carTotal = quantity => COST_OF_ITEM * quantity

console.log(carTotal(2) // 38
console.log(carTotal(2) // 38
```

A simple vista, esta función parece pura ya que siempre se obtiene la misma salida basado en su entrada. Tal y como lo muestran las impresiones en consola.

Aunque se obtiene el mismo resultado esta función es impura, ya que el estado de la aplicación tiene un efecto sobre la salida de la función. Al cambiar el valor de la constante `COST_OF_ITEM` y hacer la impresión sobre la terminal se va a obtener un resultado diferente.

```js
const COST_OF_ITEM = 14
const carTotal = quantity => COST_OF_ITEM * quantity

console.log(carTotal(2) // 28
console.log(carTotal(2) // 28
```

### Misma entrada, diferente salida
La segunda función impura que se va a revisar es aquella que tiene la misma entrada pero devuelve diferentes salidas. A menudo las aplicaciones necesitan que se creen objetos con únicos identificadores (`ids`). Se va a crear una función `generateID`.

Esta función retornará varios enteros aleatorios. Se puede observar que esta función es impura por si misma. Al llamarla varias veces, cada llamado va a retornar un resultado diferente.

```js
const generateID = () =>
	Math.floor(Math.random() * 1000)

console.log(generateID()) // 23
console.log(generateID()) // 13
console.log(generateID()) // 73
```

Ahora se va usar la función impura `generateID` dentro de una función de fabrica para crear objetos de usuarios. Para crear un usuario se requieren un `name` , un `age` ,  y un `id` creado a través de `generateID`.

```js
const generateID = () =>
	Math.floor(Math.random() * 1000)

const createUser = (name, age) => ({
	id: generateID(),
	name, 
	age,
})

console.log(createUser('Sergio', 27)) // { id:123, name: 'Sergio', age: 27  }
console.log(createUser('Sergio', 27)) // { id:321, name: 'Sergio', age: 27  }
```

Cuando se imprime en consola la función de fabrica `createUser` se van a mostrar objetos muy similares pero que no son iguales, ya que sus `ids` diferentes.

La impureza de `generateID` hace que la función de fabrica `createUser` sea impura también. La forma de corregir esta impureza heredada es moviendo las funciones impuras fuera de la función de fábrica y llamarla solo cuando esperamos que el efecto secundario ocurra, y pasar el ID como parámetro de `createUser`.

### Efectos secundarios
El tercer ejemplo es una función que crea un efecto secundario en la aplicación. De manera parecida al ejemplo de estado global, se pueden crear funciones que muten el estado de la aplicación causando efectos secundarios.

Para ello, vamos a hacer seguimiento de un valor mutable en nuestra aplicación. En este caso una variable `id`. Si se crea una función que cambie este valor, esta función será impura. Por ejemplo, en una aplicación que maneja platos de comida, se decidió usar una función de fábrica `createFoodItem`.

```js
let id = 0

const createFoodItem = name => ({
	id: ++id,
	name
})

console.log(createFoodItem('Cheseburgers')) // { id: 1, name: 'Cheseburgers' }
console.log(createFoodItem('Milkshakes')) // { id: 1, name: 'Milkshakes' }
console.log(id) // 2
```

Al imprimir en consola la función de fábrica varias veces se obtienen diferentes platos y también se observa que el  valor de `id` incrementa tal y com esperamos, pero al imprimir el valor de `id`, su valor ha sido mutado.

### El mundo exterior
Nuestro último ejemplo de función impura es el efecto secundario sobre el mundo exterior. Créalo o no, en estos ejemplo hemos usado muchas veces una  función impura que afecta el mundo exterior, `console.log`.

Cada vez que se usa el `console.log` se afecta la terminal. Esto es un efecto secundario. Si se tiene una función que usa el `console.log` dentro de su cuerpo (e.g `logger`) y recibe un mensaje que imprime como salida, esta función también será impura, aun así se escriba un mensaje genial como "Hi, Functional Programming"

```js
const logger = msg => {
	console.log(msg)
}

logger('Hi Functional Programming')
```

## Aplicaciones Parciales

Una aplicación parcial ocurre cada vez que una función con currying tiene alguna pero no toda su función aplicada.

De manera detallada, se puede hacer que los argumentos pasados a una función con currying sean almacenados en un closure y así poder reutilizarlos en el programa. Ya que cada argumento –menos el último–, devuelve una nueva función, se crean facílmente funciones reutilizables al proporcionar algunos de estos argumentos de antemano. La reutilización se da cuando se comparten estas funciones parcialmente aplicadas con otras partes del código.

Para demostrar el concepto de aplicaciones parciales, se va a crear una función con curry `getFromAPI`, la cual va a recibir una `baseURL`, un `endpoint` y un `callback`, que será llamado una vez se hayan recibidos los datos desde la petición.

```js
const fetch = require('node-fetch')
const getFromAPI = baseURL => endpoint => callback 
	fetch(`${baseURL}${endpoint}`)
		.then(res => res.json())
		.then(data => callback(data))
		.catch(err => {
			console.error(err.message)
		})
```

En este ejemplo, se utiliza el método `fetch` de node para recuperar los datos. La UR es una combinación de `basURL` y `endpoint`. Se convierte la respuesta en un JSON. Posteriormente, se hace una llamada del `callback` con el `data` como argumento y utilizamos el `catch` para registrar errores.

Ahora se va aplicar parcialmente el `baseURL`. Para ello se va a utilizar el API público de GitHub y se va a tener una función parcialmente aplicada llamada `getGitHub` a la cual se le pueden pasar diferentes endpoints. Los endpoints que se van a utilizar son `users` y `repositories`


```js
const fetch = require('node-fetch')
const getFromAPI = baseURL => endpoint => callback 
	fetch(`${baseURL}${endpoint}`)
		.then(res => res.json())
		.then(data => callback(data))
		.catch(err => {
			console.error(err.message)
		})

const getGithub = getFromAPI('https://api.github.com')

const getGithubUsers = getGithub('/users')
const getGithubRepositories = getGithub('/repositories')
```

Ahora que se tienen dos nuevas funciones cada una de ellas aplicadas parcialmente a la misma `baseURL`, pero están parcialmente aplicadas a diferentes endpoints. La última cosa que se puede suministrar a estas funciones es el callback que activará el `fetch`. Se va a usar la función `getGithubUsers` y se proporcionarán diferentes callbacks a cada una: `user.login` y `user.avatar_url`.

```js
getGithubUsers(data => {
	console.log(data.map(user => user.login))
))

getGithubUsers(data => {
	console.log(data.map(user => user.avatar_url))
))

```

En consecuencia, vamos a tener como resultado una lista de los nombres de algunos usuarios de GitHub y una lista de las urls en donde se almacenan los avatares de algunos usuario del GitHub.

## Programación libre de puntos

La programación libre de puntos consiste en pasar funciones con nombre como argumentos para evitar escribir funciones anónimas con variables internas. Para desarrollar este concepto, revisemos el siguiente ejemplo.

Con frecuencia, al pasar funciones como argumentos de otras funciones o métodos se usan funciones anónimas con variables internas. Si se considera el método `map`, es muy posible encontrar que la implementación de duplicar los valores de un arreglo sea algo como:

```js
const array [1, 2, 3]

array.map(x => x * 2)
```
Este código funciona, pero deja un espacio abierto para bugs y malentendidos. `x` actúa como una variable interna y es un marcador de posición para los datos. Se puede cambiar su nombre a `foo`, `bar` o `y`, pero estos nombres no dan mucha información sobre los datos, e inclusive no indica qué estamos haciendo con ellos.

La programación libre de puntos es remover esas funciones anónimas con variables internas a través del uso de funciones con nombres que hacen llamados a otras funciones. Para el caso de duplicar los valores del arreglo, se debe crear una función `double` que recibe cualquier valor como argumento y lo retorna duplicado. Entonces, se puede pasar la función `double` directamente en el `array`. Al usar programación libre  puntos ganamos legibilidad:

```js
const array [1, 2, 3]
const double = x => x * 2;

array.map(double);
```

Por tanto, la programación libre de puntos da las siguientes ventajas:
+ Aumenta la legibilidad del código.
+ Disminuye los tiempos en la búsqueda de errores.
+ Hace que el código sea más fácil de componer y por ende de comprobar, ya que se manejan unidades.

## Composición funcional
La composición funcional es la construcción de funcionalidades complejas a través de la combinación de funciones simples. En este sentido, la composición es la anidación de funciones, pasando el resultado de una como entrada de la siguiente. En lugar de crear una anidación indecifrable, se usa una función de orden superior llamada `compose`, la cual toma todas las funciones que se quieren combinar y retorna una nueva función que se utilizará en la aplicación.

Para ilustrar el concepto de composición, que es el corazón de la programación funcional, vamos a recordar temas de las clases de matemáticas. Se van a utilizar dos funciones, la función `f` tomará un valor `x` y le sumará dos unidades. La función `g` recibirá un valor `x` y los multiplicará por tres. Al aplicar la composición `f(g(x))` el resultado será la combinación de ambas funciones.

```js
const f = x => x + 2
const g = x => x * 3

console.log(f(g(5))) // 17
```
> Recuerde la propiedad `f(g(x)) !== g(f(x))`, esta propiedad es importante, ya que determina el orden en qué debe realizarse la composición.

En el código anterior se observa que la ejecución para llegar al resultado de 17 fue la siguiente:

1. 5 * 3 = 15, ejecución de la función `g`
2. 15 + 2 = 17, ejecución de la función `f`

Por lo general, las aplicaciones no tienen nombres de una sola letra, sin embargo, eso significa que anidar varias funciones para lograr una composición puede ser algo engorroso.

Considere el siguiente caso de transformación de una cadena de caracteres. Se va a empezar con una función `scream`, que tomará la cadena de caracteres y la pondrá en mayúsculas. Luego se hará una función `exclaim`, que tomará el string y le agregará el signo de exclamación. Por último se implementara una función `repeat` la cual duplicará el string dos veces, agregando un espacio entre cada uno. El orden para hacer esta composición será de adentro hacia afuera.

```js
const scream = str => str.toUpperCase()
const exclaim = str => `${str}!`
const repeat = str => `${str} ${str}`


console.log(repeat(exclaim(scream('I love composition')))) // I LOVE COMPOSITION! I LOVE COMPOSITION!
```
Como puede ver, la composición es larga y difícil de leer, especialmente si se tuviera un mayor número de funciones. Una mejor forma de hacer esta composición sería creando una función de orden superior que acepte cualquier número de funciones como argumentos, y cree una composición con ellas. Esta función se llamara `compose` y tendrá la siguiente implementación:

```js
cons compose = (...fns) => x 
	=> fns.reduceRight(
		(acc, fn) => fn(acc),
		x
	)
```

`compose` devolverá una función que esta esperando su valor inicial, el cual se llamará `x`.  A partir de aquí, se tendrá una serie de funciones. Tenga en cuenta que el orden en que dichas funciones serán llamadas. Para este caso ira de izquierda a derecha, es decir, primero se llama `scream`, luego `exclaim` y por último `repeat`. Es por esta razón que se utiliza el método `reduceRight`.

El primer argumento del `reduceRight` es una función que recibe un acumulador y la función actual para devolver con cada iteración el resultado de llamar al valor acumulado en esa función actual. El segundo argumento es el valor inicial `x`. con esta función de orden superior se puede hacer la siguiente composición:

```js
const withExuberance = compose(
	repeat,
	exclaim,
	scream
)

console.log(withExuberance('I love composition')) // I LOVE COMPOSITION! I LOVE COMPOSITION!
```

Una nota relevante, es que ciertas librerías de utilidades como `lodash` o `ramda`,  vienen con una función `pipe` para hacer estas composiciones. Su implementación es similar al función `compose`, y la diferencia es que el orden de los argumentos ha sido invertido. es decir para lograr la misma respuesta en la transformación del string la composición seria la siguiente:

```js
const withExuberance = pipe(
	scream,
	exclaim,
	repeat
)

console.log(withExuberance('I love composition')) // I LOVE COMPOSITION! I LOVE COMPOSITION!
```

## Orden de argumentos
Un detalle importante en la programación funcional es ligar sus principios al orden de los argumentos de las funciones. Al ordenar los argumentos de manera específica, se permite que las funciones se beneficien con aplicaciones parciales, mejor reutilización de código y facilidad en la composición funcional. Una regla establecida es enfocarse en _proporcionar los datos como último argumento de las funciones_ para que el resultado de una función se pueda canalizar como el argumento de otra.

El orden de los argumentos en las funciones sin curry es algo trivial. Al tener una función `map` que recibe un `array` y un `callback` y los usa porque recibe todos los argumentos al mismos tiempo, realmente no hay ninguna diferencia si se cambia el orden a `callback` y luego `array`.

```js
const map = (cb, array) => array.map(cb) // ===
const mapTwo = (array, cb) => array.map(cb)
```

Estas funciones necesitan ambos argumentos para funcionar y no se pueden crear aplicaciones parciales sobre estas funciones. No obstante, si trabajamos las funciones con curry, hay una gran diferencia.

Se va crear una función `map` con curry en donde primero se recibe un `array` y luego un `callback` para así retornar `array.map` pasando el `callback` como argumento. Con este esquema, es posible crear una arreglo y una función para el callback. Si se crea una función parcialmente aplicada suministrado el arreglo, es posible pasar diferentes callbacks y obtener el resultado correcto, ya que los datos están bloqueados

```js
const map = array => cb => array.map(cb)

const arr = [1, 2, 3, 4, 5]
const double = n => n * 2

const withArr = map(arr)

console.log(withArr(double)) // [2, 4, 6, 8, 10]
console.log(withArr(n => n * 3)) // [3, 6, 9, 12, 15]
```

Con este escenario, lo único que se puede cambiar es el `callback`. No hay ninguna utilidad extra al momento de llamar el método `map` directamente con la matriz.

Si se cambia el orden de los argumentos para recibir el `callback` primero y luego el `array`, se va a obtener mayor utilidad de esta función con curry.

Con este orden de argumentos es posible crear una función `withDouble` que use la función `map` proporciona el `callback` y espera a que se doblen los valores en el arreglo.

```js
const map = cb => array => array.map(cb)

const arr = [1, 2, 3, 4, 5]
const double = n => n * 2

const withDouble = map(double)

console.log(withDouble(arr)) // [2, 4, 6, 8, 10]
console.log(withDouble([2, 4, 6, 8, 10)) // [4, 8, 12, 16, 20]
```

Una forma muy útil para pensar en el orden de los argumentos de las funciones con curry es: _ir del argumentos más específico hasta el argumento menos específico_. Los argumentos menos específicos en cada casos siempre serán los datos representados con los tipos primitivos del lenguaje de programación (booleanos, números, cadena de caracteres, objetos o arreglos).

Ahora un ultimo ejemplo con una función `prop` para derivar el valor de un objeto al recibir una `key` y luego un objeto para devolver el valor almacenado con esa `key`. Para ello, se pueden crear funciones parcialmente aplicadas que están diseñadas para recuperar valores de cualquier objeto.

Una de esta funciones sera `propName` con el argumento específico `name`. Luego se creará una lista `people` que tendrá objetos con la propiedad `name`. Finalmente, se puede usar la función `propName` que espera como último argumento los datos y la combinación con la función `map`. Al imprimir esta composición en consola, se obtienen un arreglo con los nombres del objeto `people`.

```js
const prop = key => object => object[key]

const propName = prop('name')

const people = [
	{ name: 'Leonardo' },
	{ name: 'Rafael' },
	{ name: 'Miguel Angel' },
	{ name: 'Donatelo' },
]

console.log(map(propName)(peope)) // ['Leonardo', 'Rafael', 'Miguel Angel', 'Donatelo' ]
```

## Propiedad asociativa

 La propiedad asociativa es un principio matemático que demuestra que al agrupar valores cuando se hacen adiciones o multiplicaciones no se altera el resultado:

```
1 + 2 + 3 		===
(1 + 2) + 3 	===
1 + (2 + 3)
```

Este mismo principio aplica para la composición de funciones. Se van a retomar las funciones `compose`, `scream`, `exclaim` and `repeat` para demostrar la propiedad asociativa en la composición de funciones. El siguiente código es el último estado de dicho ejemplo:

```js
const str = 'I love functional composition'

const withExuberance = compose(
	repeat,
	exclaim,
	scream,
)

console.log(withExuberance(str))	// I LOVE FUNCTIONAL COMPOSITION! I LOVE FUNCTIONAL COMPOSITION!
```

Perfecto, ahora se va a utilizar la propiedad asociativa para hace sub-composiciones entre las composiciones y crear la misma funcionalidad. Empecemos con `repeat` and `exclaim` para crear una función `repeatExcitedly`. Luego se agrega la función `scream`y se pasa la misma cadena de caracteres para obtener el mismo resultado.

```js
const str = 'I love functional composition'

const repeatExcitedly = compose(
	repeat,
	exclaim,
)

console.log(
	compose(
		repeatExcitedly,
		scream,
	)(str)
)	// I LOVE FUNCTIONAL COMPOSITION! I LOVE FUNCTIONAL COMPOSITION!
```

Otra sub-composición que se puede hacer es la combinación de `exclaim` y `scream` para crear la función `screamLoudly` así parezca redundante. Al usar la `screamLoudly` en conjunto con `repeat` y pasamos el `string` se obtiene el mismo resultado.

```js
const str = 'I love functional composition'

const screamLoudly = compose(
	exclaim,
	scream,
)

console.log(
	compose(
		screamLoudly,
		repeat,
	)(str)
)	// I LOVE FUNCTIONAL COMPOSITION! I LOVE FUNCTIONAL COMPOSITION!
```

La propiedad asociativa permite que logremos el mismo resultado con diferente orden en las operaciones.

## Depuración
Las composiciones funcionales son deliberadamente opacas, sin una forma obvia de "visualizar" los datos a medida que se transforman. Esto es genial cuando la función funciona, ya que resulta difícil agregar errores, pero es desafiante cuando nuestras composiciones no son correctas. Para hacer una depuración de estas composiciones se usa una función `trace` para imprimir los valores que se van transformando. Esta propuesto permite continuar usando funciones libre de punto y al mismo tiempo proponer una vista de nuestras composiciones.

Para ilustrar la depuraciones en funciones libre de puntos, se va a crear una función `slugify` que recibirá una lista `bookTitles` a los cuales les va a remplazar los espacios por guiones medios `-` y dejará el titulo todo en minúsculas para cumplir con las convenciones de un formato URL. Se va a introducir un error a propósito en el código para habilitar la depuración.

```js
const bookTitle = [
	'The Culture Code',
	'Designing Your Life',
	'Algorithms to Live By'
]

const slugify = compose(
	join('-'),
	map(lowerCase),
	map(split(' '))
)

const slugs = slugify(bookTitles)

console.log(slugs) // str.toLowerCase is not a function
```
El error esta diciendo que `toLowerCase` no es una función, lo que probablemente significa que al momento de ejecutar este método el valor que se tiene no es una cadena de caracteres. Por tanto, se necesita una función que brinde un efecto secundario para habilitar la impresión del valor actual.

Esta función se llamara `trace` y recibe un `msg` como primer argumento y un valor `x` como segundo argumento. Con ayuda del operador coma, se va a imprimir el mensaje y el valor.

```js
const trace = msg => x (console.log(msg, x), x)
```

Ahora se ubica la función `trace` antes y después de las funciones de composición para ver el paso a paso del valor que se esta transformando. Se ubicará el `trace` antes del `lowercase` y antes y después del `split`. Recuerde que con las composiciones trabajamos de derecha a izquierda, o de abajo hacia arriba.

```js
const bookTitle = [
	'The Culture Code',
	'Designing Your Life',
	'Algorithms to Live By'
]

const trace = msg => x (console.log(msg, x), x)

const slugify = compose(
	join('-'),
	trace('after lowercase') 	// 3. str.toLowerCase is not a function
	map(lowerCase),
	trace('after split') 		// 2. [['The Culture Code', 'Designing Your Life','Algorithms to Live By']]
	map(split(' '))
	trace('before split') 		// 1. ['The Culture Code', 'Designing Your Life','Algorithms to Live By']
)

const slugs = slugify(bookTitles)

console.log(slugs) // str.toLowerCase is not a function
```

Antes del split tenemos nuestra lista de títulos de libros, y después del `after split` se recupera una un arreglo de dos dimensiones. Lo que esta sucediendo es que el `split` esta tomando la cadena de caracteres, divide los espacios y hace arreglos con ellos mismos. El `map(lowerCase)` esta fallando porque esta esperando un string y esta recibiendo un arreglo.

Para solucionar este inconveniente se puede cambiar el orden de la composición, haciendo primero el `map(lowercase)` y luego el `map(split(''))`. Se renombran los mensajes del `trace` y se observa el siguiente cambio:

```js
const bookTitle = [
	'The Culture Code',
	'Designing Your Life',
	'Algorithms to Live By'
]

const trace = msg => x (console.log(msg, x), x)

const slugify = compose(
	join('-'),
	trace('after split') 			// 3. [['the culture code'], ['designing your life'],['algorithms to live by']]
	map(split(' ')),
	trace('after lowercase') 		// 2. ['the culture code', 'designing your life','algorithms to live by']
	map(lowerCase),
	trace('before lowercase') 		// 1. ['The Culture Code', 'Designing Your Life','Algorithms to Live By']
)

const slugs = slugify(bookTitles)

console.log(slugs)
```

Ahora se tiene que antes del `map(lowercase)` esta el arreglo inicial. Después del `map(lowercase)` se tiene un arreglo unidimensional con los nombres en minúsculas. El error esta luego del `map(split(' '))` ya que nuevamente se obtiene un arreglo de dos dimensiones en donde cada cadena de caracteres fue separada por sus espacios y por cada título individual. 

Lo que sucede es que cuando se llama `join` sobre este arreglo, el guis se pone realmente entre el último y el primer valor de cada arreglo. Se necesita es hacer al llamado de `map` sobre `join` también. El resultado sería:

```js
const slugify = compose(
	map(join('-')),
	trace('after split') 			// 3. ['the-culture-code', 'designing-your-life','algorithms-to-live-by']
	map(split(' ')),
	trace('after lowercase') 		// 2. ['the culture code', 'designing your life','algorithms to live by']
	map(lowerCase),
	trace('before lowercase') 		// 1. ['The Culture Code', 'Designing Your Life','Algorithms to Live By']
)
```
Ahora tenemos el resultado esperado. Ya sabiendo que la composición funciona se remueven los  llamados a la función `trace` para limpiar el código:

```js
const slugify = compose(
	map(join('-')),
	map(split(' ')),
	map(lowerCase),
)
```

Es de esta forma que se hace depuración de las composiciones funcionales.
