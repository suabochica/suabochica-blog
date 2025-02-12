+++
title = "¿Cómo funciona a JavaScript?"
date = 2018-05-23T02:13:50Z
author = "Sergio L. Benítez D."
description = "Notas sobre el curso de JavaScript."
tags = [
    "javascript",
]
+++

En esta publicación se va a explorar el funcionamiento de JavaScript:

1.  [Analizadores (Parsers) y Motores de JavaScript](#org2695b14)
2.  [Contexto de Ejecución](#orgc3c7c48)
    1.  [Fases del contexto de ejecución](#org3140a56)
    2.  [Hoisting en Práctica](#org4157e4d)
3.  [Ámbitos (Scoping)](#org0be08f8)
4.  [La palabra clave `this`](#orgd5d8c55)
5.  [Recapitulación](#org25bedeb)

# Analizadores (Parsers) y Motores de JavaScript {#org2695b14}

JavaScript siempre está alojado en algún **entorno** (típicamente un navegador). El navegador es donde se ejecuta JavaScript. También puede haber otros anfitriones como el servidor web *NodeJS* o incluso algunas aplicaciones que aceptan la entrada de código de JavaScript. Pero por ahora, siempre nos centraremos en el navegador. Así que, cuando escribes código JavaScript, y quieres ejecutarlo, suceden detrás de escenas los siguientes pasos:

1.  Nuestro host de JavaScript tiene un motor de JavaScript que toma el código y lo ejecuta. Existen motores diferentes según el navegador. Chrome utiliza el *motor V8* y Firefox usa *SpiderMonkey*, por ejemplo.
2.  Lo primero que sucede dentro del motor es que el código JavaScript es analizado por un parser, que lee el código línea por línea y verifica si la sintaxis del código que le has proporcionado es correcta. Así, el analizador conoce las reglas de JavaScript y cómo debe estar escrito para ser válido. Si el analizador *no encuentra errores*, produce una estructura de datos llamada **Árbol de Sintaxis Abstracta (Abstract Syntax Tree)**, que luego se traduce en código máquina. Si el analizado *encuentra errores*, este lanza un error un detiene la ejecución.

El *Código Máquina* ya no es código JavaScript, es un conjunto de instrucciones que pueden ser ejecutadas directamente por el procesador de la computadora. Solo cuando tu código ya ha sido convertido en código máquina, se ejecuta y realiza su trabajo. Esa es la idea básica de lo que realmente sucede cuando eliges ejecutar tu código.

# Contexto de Ejecución {#orgc3c7c48}

El **contexto de ejecución** es un concepto que te ayuda a enfocarte en el orden en que el código se ejecuta. Todo el código JavaScript necesita operar en un entorno llamado contexto de ejecución. Entonces, puedes imaginar un contexto de ejecución como contenedor que almacena variables y en el que se evalúa y ejecuta una parte del código.

JavaScript siempre necesita un entorno para ser ejecutado haciendo implícito un contexto de ejecución llamado **Contexto Global**. En el contexto global, todo el código que no está dentro de ninguna función se ejecuta. Además, puedes pensar en un contexto de ejecución como un objeto. El contexto de ejecución global está asociado con el objeto global, que en el caso del navegador es el objeto **window**. Así que, todo lo que declaramos en el contexto global se adjunta automáticamente al objeto window en el navegador. Las siguientes instrucciones muestra una equivalencia entre una variable en un contexto de ejecucuión y el contexto global:

-   Declarar una variable llamada `lastName`.
-   Declarar una variable llamada `window.lastName`.
-   Ambas declaraciones son exactamente lo mismo. Esto significa que `lastName === window.lastName` es verdadero.

Complementario al contexto de ejecución esta la figura de la **pila de ejecución**. Para indagar la figura revisemos el siguiente código:

```js
    var name = 'John';      // 1. Global Execution context
    
    function first() {
        var a = 'Hello!';
        second();           // 3. Push the second() function in the execution stack
        var x = a + name;   // 7. Pop the first() function from the execution stack
    }
    
    function second() {
        var b = 'Hi!';
        third();            // 4. Push the third() function in the execution stack
        var y = b + name;   // 6. Pop the second() function from the execution stack
    }
    
    function third() {
        var c = 'Hey!';
        var z = c + name;   // 5. Pop the third() function from the execution stack
    }
    
    first();                // 2. Push the first() function in the execution stack
```

Recuerda que el código que no está dentro de ninguna función se ejecuta en el **contexto global**. Pero, ¿qué pasa con el código dentro de una función? En realidad, es muy simple, porque cada vez que llamas a una función, esta obtiene su propio contexto de ejecución. Es en este escenario cuando la estructura de la **pila de ejecución** entra en juego, y su responsabilidad es organizar los diferentes contextos de ejecución y determinar cuál es el contexto de ejecución activo. En el último código, las líneas comentadas indican el orden en el que cada contexto se añade o elimina de la pila de ejecución.

## Fases del contexto de ejecución {#org3140a56}

Hemos señalado que cada función crea su propio contexto de ejecución, pero exactamente ¿cómo sucede eso? Para entender la creación de un contexto de ejecución, en la siguiente lista es indican las 3 propiedades de un objeto de contexto de ejecución:

-   **Objeto de Variables (Variable Object, VO)**, que contendrá los argumentos de la función en una declaración de variable, así como las declaraciones de funciones.
-   **Cadena de Ámbito (Scope Chain)**, que contiene los objetos de variables actuales, así como los objetos de variables de todos sus padres.
-   La famosa palabra clave `this`.

Ahora bien la pregunta de cuando se crea un contexto de ejecución tiene más relevancia. Cuando se llama a una función, un nuevo contexto de ejecución se coloca en la parte superior de la pila de ejecución, y esto pasa por dos fases:

1.  Fase de Creación: Se definen las propiedades del objeto de contexto de ejecución. Creación del objeto de variables; Creación de la cadena de ámbito; Determinación del valor de la variable `this`.

2.  Fase de Ejecución: El código de la función que generó el contexto de ejecución actual se ejecuta línea por línea, y todas las variables se definen. Si es el contexto global, entonces se ejecuta el código global.

Ahora es momento de entrar en detalle sobre la creación del **objeto de variables** en la fase de creación:

-   Se crea el objeto de `argument`, que contiene todos los argumentos que se pasaron a la función.
-   Se escanea el código en busca de **declaraciones de funciones**: para cada función, se crea una propiedad en el objeto de variables que apunta a la función. Esto significa que todas las funciones se almacenarán en el objeto de variables, incluso antes de que el código comience a ejecutarse.
-   Se escanea el código en busca de **declaraciones de variables**: para cada variable, se crea una propiedad en el objeto de variables y se establece en undefined.

El último punto a evaluar es lo que los desarrolladores comúnmente llaman **hoisting** en JavaScript. El hoisting en JavaScript significa que las funciones y las variables están disponibles antes de que la fase de ejecución realmente comience. El hoisting se hace de manera diferente para funciones y variables; las funciones ya están definidas y las variables se configuran como `undefined` antes de que comience la fase de ejecución; las variables solo se definen en la fase de ejecución.

En resumen, cada contexto de ejecución tiene un objeto que almacena muchos datos importantes que la función utilizará mientras se está ejecutando, y esto sucede incluso antes de que se ejecute el código.

## Hoisting en Práctica {#org4157e4d}

El **hoisting** puede sonar un poco confuso al principio, pero después de algunos ejemplos y algo de práctica, será sencillo de entender. En el siguiente ejemplo, vas a utilizar funciones muy simples porque queremos enfocarnos en cómo funciona todo y no exactamente en lo que hace el código. Antes de comenzar, recordemos que el hoisting se aplica a funciones y variables de manera diferente. Para el hoisting en funciones tenemos el siguiente código:

```jsx
    calculateAge(1989) //-> 29: By hoisting this call also works! despite not having declared the function yet
    
    function calculateAge(year) {
        console.log(2018 - year);
    }
    
    calculateAge(1991) //-> 27: This is the expected behavior. Call the function after his declaration.
```

El código anterior utiliza una declaración de función `calculateAge(year)`. La primera llamada funciona porque hay hoisting para las declaraciones de funciones. Así que en la fase de creación del contexto de ejecución (en este caso, el contexto de ejecución global), la declaración de la función `calculateAge(year)` se almacena en el objeto de variables incluso antes de que se ejecute el código. Por esta razón, cuando entramos en la fase de ejecución, la función `calculateAge(year)` ya está disponible para que la usemos.

Ahora, revisemos qué sucede con las expresiones de función:

```jsx
    retirement(1989) //-> Uncaugth TypeError: retirement is not a function
    
    var retirement = function(year) {
        console.log(65 - (2018 - year));
    }
    
    retirement(1991) //-> 38: This is the expected behavior. Call the function after his declaration
```

Como se puede observar se obiente el error `retirement no es una función`. Por lo tanto cuna la sitáxis de funciones de expresión no podemos llamar la expresión antes de que sea declarada. *El hoisting solo funciona con la declaración de funciones*.

Para las variables, revisemos el siguiente código:

```jsx
    console.log(age); //-> undefined
    var age = 23;
    console.log(age); //-> 23
```

Como puedes ver, si intentas usar una variable antes de declararla, el **hoisting** asignará el tipo de dato `undefined` a la variable, porque recuerda que en la fase de creación del objeto de variables el código se escanea en busca de declaraciones de variables y luego las variables se establecen en `undefined`.

Ahora, llevemos esto un paso más allá con una mezcla de código de funciones y variables:

```jsx
    console.log(age);       //-> 1. undefined
    var age = 23
    
    function foo() {
        console.log.(age);  //-> 2. undefined
        var age = 65;
        console.log.(age);  //-> 3. 65
    }
    
    foo();
    console.log(age);       //-> 4. 23
```

La clave para identificar cómo se ejecuta este código es tener claro los diferentes objetos de contexto de ejecución que se crean durante la fase de ejecución. En el código anterior, tenemos el contexto de ejecución global que almacenará la variable `var age = 23` y la declaración de la función `foo()`. También tenemos el objeto de contexto de ejecución de `foo()`, en el cual podemos almacenar la variable `var age = 65`. Observa que ambas variables tienen el mismo nombre, pero esto no importa porque son dos variables completamente diferentes, gracias a los distintos objetos de variables asociados a cada objeto de contexto de ejecución.

El caso de uso más importante para el hoisting ni siquiera son las variables, sino el hecho de que podemos usar declaraciones de funciones antes de declararlas en nuestro código.

# Ámbitos (Scoping) {#org0be08f8}

Ahora es momento de profundizar en el segundo paso de la fase de creación: la creación de la **cadena de ámbitos (Scoping Chain)**. Para empezar, ¿qué significa scoping? El scoping responde a la pregunta: ¿dónde podemos acceder a una determinada variable?

En programación, **scoping** se refiere al contexto en el que las variables, funciones y objetos son accesibles o visibles. Define desde dónde y hasta dónde se puede acceder a estas entidades en el código.

En JavaScript, cada función crea un ámbito, un espacio o entorno en el que las variables que define son accesibles. También existen otros 2 tipos de ámbitos en JavaScript: el *ámbito global* y el *pseudo-ámbito de bloque*, pero en JavaScript, el más relevante es el *ámbito de función*. El ámbito de función maneja el **scoping léxico**, lo que significa que una función tiene acceso al ámbito de la función exterior o padre, basado en su posición léxica (la posición de algo en el código). Observa el siguiente ejemplo para obtener una mejor idea de cómo funciona el scoping:

Existen diferentes tipos de ámbito en JavaScript:

```jsx
    var a = 'Hello!';
    first();
    
    function first() {
        // ↑ first() scope: [VO first] + [VO global]
        var b = 'Hi!';
        second();
    
        function second() {
            // ↑ second() scope: [VO second] + [VO first] + [VO global]
            var c = 'Hey!';
            third(); //-> You can call the third() function because of scoping.
        }
    }
    
    function third() {
        var d = 'Edward';
        console.log(a + b + c + d); //-> Reference Error, c is not defined
    }
```

La pila de ejecución del anterior códio es:

1.  Contexto de ejecución global.
2.  Contexto de ejecución de la función `first()`.
3.  Contexto de ejecución de la función `second()`.
4.  Contexto de ejecución de la función `third()`.

Básicamente el contexto de ejecución determina el orden en el que una función es llamada.

Ahora bien, la cadena de ámbito para el último código es:

1.  Ámbito de la función `third()`.
2.  Ámbito de la función `second()` (anidada en la función `first(`)
3.  Ámbito de la función `first()`.
4.  Ámbito global.

Entonces, la cadena de ámbitos (scope chain) está determinada por el orden en que las funciones están escritas léxicamente en el código. En conclusión, el orden en que las funciones son llamadas no determina el ámbito de las variables dentro de esas funciones.

Las variables `b` y `c` definidas en las funciones `first()` y `second()` están fuera del ámbito de `third()`. Por lo tanto, el motor de JavaScript lanza un `Reference Error` sobre `c`. Los contextos de ejecución almacenan la cadena de ámbitos de cada función en el objeto de variables, pero esto no afecta a la cadena de ámbitos en sí misma.

# La Palabra Clave `this` {#orgd5d8c55}

Hasta ahora se ha comentado que un contexto de ejecución tiene la fases de creación y de ejecución. En la fase de creación, hemos hablado sobre la creación del objeto de variables y la creación de la cadena de ámbitos. El tercer y último paso de esta fase de creación es determinar y asignar el valor de la variable `this`. La variable `this` es una variable que cada contexto de ejecución obtiene, y luego se almacena en el objeto del contexto de ejecución. Entonces, ¿a qué apunta la palabra clave this?

-   En una llamada de *función regular*, la palabra clave `this` apunta al objeto global (el objeto window en el navegador).
-   En una llamada de *método*, la variable `this` apunta al objeto que está llamando al método.

La palabra clave `this` no se le asigna un valor hasta que la función en la que está definida sea realmente llamada. La palabra clave this está vinculada a un contexto de ejecución, que solo se crea tan pronto como se invoca la función.

```jsx
    console.log(this); //-> Window object
    
    calculateAge(1985);
    
    function calculateAge(year) {
        console.log(2018 - year); //-> 33
        console.log(this); //-> Window object
    }
```

En ambos ejemplos, la palabra clave `this` está vinculada al **contexto de ejecución global**, y su valor es el objeto `window`. En el caso de `calculateAge()`, la palabra clave `this` está vinculada al objeto `window` porque `calculateAge()` es una función regular y no un método. Entonces, como sabemos, en el código de una **función regular**, `this` siempre apunta al objeto del contexto de ejecución global, que en este caso es el objeto `window`. Ahora, revisemos el uso de la palabra clave this en **métodos de objetos**:

```jsx
    var edward = {
        name: 'Edward'
        yearOfBirth: 1990,
        calculateAge: function() {
            console.log(this); //-> edward object
            console.log(2018 - yearOfBirth);
    
            function innerFunction() {
                console.log(this) //-> Window object
            }
            innerFunction()
        }
    }
    
    edward.calculateAge();
```

En este ejemplo, el objeto que llama a la función `calculateAge()` es `edward`. La palabra clave `this` ahora está apuntando al objeto `edward` porque `this` se refiere al objeto que llamó al método. El escenario complicado es la función interna `innerFunction()`. La palabra clave this en `innerFunction()` apunta al objeto `window`, pero ¿por qué? –la gente espera que la palabra clave `this` apunte al objeto `edward` –.

La razón es que `innerFunction()` es una función regular, por lo que el objeto predeterminado es el objeto `window`, como vimos antes. Ahora es momento de profundizar en el concepto de **métodos prestados (method borrowing)** con ayuda del siguiente código.

```jsx
    var edward = {
        name: 'Edward'
        yearOfBirth: 1990,
        calculateAge: function() {
            console.log(this); //-> edward object | alphonse object
            console.log(2018 - yearOfBirth); //-> 28 | 26
        }
    }
    
    edward.calculateAge();
    
    var alphonse = {
        name: 'Alphonse'
        yearOfBirth: 1993,
    }
    
    alphonse.calculateAge = edward.calculateAge;
    alphonse.calculateAge();
```

Dos cosas importantes en este código:

1.  La línea `alphonse.calculateAge = edward.calculateAge;`. Como puedes ver, no usamos los paréntesis `()`, porque los paréntesis son para llamar a una función. En este caso, simplemente tratamos la función como una variable, lo que nos permite usar el método prestado (method borrowing), donde el objeto `alphonse` ahora tiene una función prestada del objeto `edward`.
2.  Al llamar a la función `calculateAge()` desde el objeto `alphonse`, la palabra clave `this` ahora apunta al objeto `alphonse`, como era de esperarse. Esto es una prueba de que la palabra clave `this` se asigna solo cuando el objeto llama al método.

# Recapitulación {#org25bedeb}

Son 5 cosas que quiero compartir con esta publicación:

1.  Motor de JavaScript y Fases de Creación de Contexto de Ejecución:
    -   Los navegadores utilizan distintos motores de JavaScript como V8 en Chrome y SpiderMonkey en Firefox.
    -   En la fase de creación del contexto de ejecución, se crean varios objetos importantes como el **objeto de variables (Variable Object)**, la **cadena de ámbitos (Scope Chain)**, y se define el valor de la palabra clave this.

2.  Hoisting en Funciones y Variables:
    -   En las funciones, **hoisting** permite que se puedan utilizar antes de ser declaradas en el código.
    -   Las variables también recibe el **hoisting**, pero con un valor de `undefined` hasta que el código las asigne en la fase de ejecución.

3.  Cadena de Ámbitos (Scope Chain):
    -   El **scoping** responde a la pregunta de dónde se puede acceder a una variable.
    -   Las funciones crean su propio ámbito y tienen acceso a las variables en los ámbitos exteriores debido al scoping léxico.
    -   La cadena de ámbitos se determina por el orden léxico de las funciones en el código, no por el orden en que se llaman.

4.  Palabra Clave `this`:
    -   En una función regular, `this` apunta al **objeto global** (`window` en el navegador).
    -   En una llamada a un método, `this` apunta al objeto que llama al método.
    -   El valor de `this` se asigna solo cuando la función es invocada.

5.  Métodos Prestados:
    -   Es posible &ldquo;prestar&rdquo; métodos de un objeto a otro.
    -   Al hacerlo, la palabra clave `this` cambia para apuntar al objeto que está llamando al método, lo que muestra que `this` se asigna dinámicamente en tiempo de ejecución.
