+++
title = "Guía comprensiva sobre patrones de diseño en JavaScript"
date = 2020-06-23T02:13:50Z
author = "Sergio L. Benítez D."
description = "Patrones de diseño en JavaScript"
tags = [
    "javascript",
]
+++

Un buen desarrollador en JavaScript debe luchar por escribir un código limpio, saludable y mantenible. Con frecuencia el desarrolador resuelve problemas interesantes que no necesariamente requieren soluciones únicas y paralelamente es posible que haya escrito un código similar para solucionar un problema completamente diferente ha alguno que haya manejado antes. Puede que no lo sepa, pero al hacer esto ha utilizado un **patrón de diseño** en JavaScript.

> Los patrones de diseño son soluciones retutilizables a problemas comunes en el diseño de software.

En cualquier lenguaje de programación hay un gran número de desarrolladores que hacen y prueban muchas de estas soluciones reutilizables. En consecuencia, tales soluciones resultan ser tan útiles ya que nos ayudan a escribir código de manera optimizada para la reparación de problemas frecuentes.

Los principales beneficion que se obtienen de los patrones de diseño son:

- **Son soluciones comprobadas:** Debido a que los patrones de diseño son recursos utilizados con frecuencia por desarrolladores, puede estar seguro de que funcionan. Además, puede tener la certeza de que se revisaron varias veces yy que probablemente se implementaron optimizaciones.
- **Son fáciles de reutilizar:** Los patrones de diseño documentan soluciones reutilizables que pueden ser modificados para resovler multiples problemas particulares, ya que no están vinculado a un problema en específico.
- **Son expresivos:** Los patrones de diseño pueden explicar una admirable solución de manera elegante y sencilla.
- **Facilitan la comunicación:** Cuando los desarrolladores están familiarizados con los patrones de diseño, pueden comunicarse más facilmente entre sí acerca de las posibles soluciones a un problema determinado.
- **Previenen la necesidad de refactorizar el código:** Cuando una aplicación está escrita con patrones de diseño, hay una alta probabilidad de no acudir a refactorizar el código más adelante puesto que un patrón de diseño aplicado correctamente a un problema determinado ya es un solucion óptima.
- **Reducen el tamaño del código base:** Los patrones de diseño al ser soluciones elegantes y óptimas, requiren menoscódigo que otras soluciones.

Con estos beneficios en mente, vamos a revisar algunos patrones de diseño aplicados sobre JavaScript. 

Una breve historia de JavaScript
================================

JavaScript es uno de los lenguajes de programación más populares para el desarrollo web en la actualidad. Inicialmente fue hecho como un tipo de _pegamento_ para varios elementos HTML exhibidos en el navegador Netscape, el cual solo podia presentar contenidos HTML estáticos en sus primeras versiones. La idea de liderar un lenguaje scripting en clientes desató una guerra entre los grandes jugadores de la industria del desarrollo de navegadores.

Cada jugador queria publicar su propia implementación de un lenguaje de scripting. Netscape, actualmente Mozilla público JavaScript, bajo la autoría de Brendan Eich, Microsoft hizo JScript, y asi otras compañias presentaron su propuesta. Sin embargo, las diferencias entre dichas implementaciones era considerable ya que eran orientadas a navegadores específicos. Pronto,iba a ser necesario un estándar, una solución transversal a todos los cliente que pudiera unificar el proceso de desarrollo y simplificar la creación de páginas web. Dicho estándar se llegó con el nombre de **ECMAScript**

ECMAScript es la especificación del estándar para el lenguaje de scripting que todos los navegadores intentan soportar, y hay multiples implementaciones o dialectos del mismo. El más popular sin duda alguna es JavaScript. Desde su primera publicación, ECMAScript ha estandarizado cosas importantes, consolidando una estabilidad en su versión número 5, ya que logro ser soportada en muchos navegadores. Para información específics delmestándar se recomienda visitar la [página oficial](https://www.ecma-international.org/publications/standards/Ecma-262.htm) del estándar.

¿Qué es JavaScript?
-------------------
Antes de empezar a estudiar los patrones de diseño en JavaScript es importante hablar de JavaScript como tal. Una definición aproximada sobre el lenguaje puede ser la siguiente:

>JavaScript es un lenguaje de programación interpretado, ligero y multiparadigma con funciones de primera clase, popularmente conocido como el lenguaje de scripting para páginas web

JavaScript es un lenguaje de scripting fácil de implementar y aprender, cuyo código es interpretado en vez de compilado. Al ser multiparadigma, JavaScript da soporte para estilos procesales, orientado a objetos y programación funcional, característica que lo hace flexible para los desarrolladores.

Hasta ahora, hemos citado características que son similares a otros lenguajes de programación. Es hora de dar un vistazo a una propiedades específicas de JavaScript con respecto a otros lenguajes que merecen una atención especial.

JavaScript soporta funciones de primera clase
--------------------
JavaScript trata las funciones como ciudadanos de primera clase, lo que significa que se pueden pasar funciones como parámetros de otras funciones, de la misma forma que pasamos variables. Por ejemplo:

```javascript
// Enviamos la función como un parámetro que va ha
// ser ejecutada dentro de la función llamada
function performOperation(a, b, callback) {
    var c = a + b;
    callback(c)
}

performOperation(2, 3, function(result) {
    console.log(result) // prints 5
});
```

JavaScript es basado en prototipos
-----------------------
Como en muchos lenguajes orientado a objetos, JavaScript soporta objetos, y los primeros términos que vienen a la cabeza después de hablar de objetos son _clases_ y _herencia_. Aquí JavaScript se vuelve un poco quisquilloso, ya que el lenguaje no soporta classes directamentes, sino que lo hace a tráve de herencia basada en prototipos.

En la versión ES6 del estándar la palabra formal `class` fue introducida. No obstante, por debajo se sigue utilizando la herencia basada en prototipos. Si se traspila un código JavaScript bajo el estándar ES6 que utilice la palabra reservada `class` a la versión ES5, se podra evidenciar el uso de la herencia basada en prototipos.

La programación basada en prototipos es un estilo de programación orientada a objetos, cuyo comportamiento de herencia se realiza mediante un proceso de reutilización de objetos existentes. Estos objetos existentes son conocidos como prototipos y habilitan la delegación de comportamientos. Esta característica se ra citada al momento de revisar varios de los patrones de diseño en JavaScript. 

JavaScript event loops
-----------------------
En JavaScript es común mencionar el término **callback function**. Un callback function, es una función que se envía como paramétro de otra función, y es ejecutsda después de que se active un evento. Recuerde que en JavaScript las funciones son ciudadanos de primera clase. Usualmente las callback function se suscriben a eventos como el clic de un mouse ó le presión de una tecla del teclado.

Un evento debe tener un listener adjunto para ser atendido, de lo contrario, sería un evento perdido. Cada vez que un evento se ejecuta, un mensaje es enviado a una fila de mensajes que son procesados sincronicamente, a través de un mecanismo FIFO (First in first out). Toda esta estructura es conocida como el **event loop** y la siguiente imagen ayuda a sintetizar dicha estructura.

![]()

Cada uno de los mensajes en la fila tiene una función asociada. Una vez el mensaje sale de la fila, la función adjunta es ejecutada completamente en tiempo de ejecución antes de procesar el siguiente mensaje. En otras palabras, si dicha función tiene un llamado a otra función, ambas funciones serán ejecutadas antes de procesar el siguiente mensaje en la fila. Este comportamiento es conocido como _run-to-completion_, y en pseudo código puede ser:

```javascript
while (queue.waitForMessage()) {
    queue.processNextMessage();
}
```

El método `queue.waitForMessage()`  espera los nuevos mensajes de manera sincrona. Cada uno de los mensajes que está siendo procesado tiene su propio stack, y es procesado hasta que la pila este vacía. Una vez termina, un nuevo mensaje es atendido desde la fila, en caso que exista.

Es importante mencionar que JavaScript es un lengaje _non-blocking_, lo que significa que cuando una operación asíncrona esta siendo procesada, el programa esta en capacidad de procesar otras cosas, tales como, la recepción de nuevas entradas por parte del usuario, y no bloquea la ejecución del hilo principal. Esta propiedad es muy útil y es un tema que puede abordar un árticulo completo. Por ahora, vamos a dejarlo por fuera del alcance de esta publicación.

What are design patterns?
-----------------------
Los patrones de diseño son soluciones reutilizables a problema que ocurren con frecuencia en el diseño de software. A continuación se va a dar un vistazo a algunas categorias de patrones de diseño:

### Proto-patrón
Antes de comenzar es recomendable hacer la siguiente pregunta ¿Cómo se crea un patrón? Suponga que reconoció un problema repetitivo y encontró su solución respectiva. No obstante, esta solución no es reconocida globalmente, pero usted la utiliza cada vez que se topa con el problema. Después de varios usos, usted considera  que la solución puede beneficiar a una comunidad de desarrolladores.

No cante victoria porque esto definitivamente no es un patrón. A menudo se confunden buenas prácticas de escritura de código con patrones. Para reconocer que su solución realmente es un patrón es necesario conocer las opiniones de otros desarrolladores al respecto, compararlo con otros patrones y familiarizarlos con el contexto de su propuesta. Hay una fase por la que tiene que pasar un patron antes de convertirse en un patrón completo, y es el proto-patrón

El proto-patrón es un patrón futuro si pasa cierto periodo de prueba por parte de varios desarrolladores y escenarios en donde se demuestra ser útil. Hay una gran cantidad de trabajo y documentación a desarrollar para lograr que un patrón sea completo y reconocido por la comunidad. 


### Anti-patrones
Así como un patrón de diseño representa una buena práctica, un anti-patrón representa una mala práctica.

Un ejemplo de un anti-patrón sería modificar el `Object` de la clase prototipo. Casi todos los objetos en JavaScript heredan del `Object` (recuerden que JavaScript usa herencia basada en prototipos) e imaginar un escenario en donde se altera el prototipo, definitivamente seria nadar en contra de la corriente. Cambiar el prototipo de `Object`, implicaría que el cambio se reflejaría en todos los objetos que heredan el prototipo, causando así un escenario con alta probabilidad de causar comportamientos no esperados. 

Otro ejemplo similar, es la modificación de objetos que no le pertenecen. Un ejemplo de esto sería anular una función de un objeto utilizado en muchos escenarios de la aplicación. Si está trabajando con un equipo grande, imagine la confusión que esto causaría; rápidamente se encontraría con colisiones de nombres, implementaciones incompatibles y pesadillas de mantenimiento.

La moraleja a compartir es que así como es de útil conocer todas las buenas prácticas y soluciones, también es muy importante conocer las malas. De esta manera, se pueden identificar y evitar cometer el error por adelantado.

### Categorización de patrones de diseño

Los patrones de diseño pueden categorizarse en muchas formas, pero las más populares son las siguientes:

- Creacionales
- Estructurales
- Conductuales
- De Concurrencia
- Arquitucterales

#### Patrones de diseño para creación
Estos patrones tratan con mecanismos de creación de objetos que optimizan la producción de los mismos en comparación a un enfoque básico. En ocaciones la creación básica de objetos puede generar problemas de diseño futuros. Los patrones de diseño de creación evitan estos inconvenientes, controlando de alguna manera la invención de objetos. Algunos de los patrones de diseño populares en esta categoría son:

- El método de fábrica
- Fábrica abstracta
- Constructor
- Prototipo
- Singleton

#### Patrones de diseño para estructuras
Estos patrones se ocupan de las relaciones entre objetos. Se aseguran de que si una parte de un sistema cambia, todo el sistema no necesita cambiar junto a la modificación realizada. Los patrones más populares en esta categoría son:

- Adaptador
- Puente
- Compuesto
- Decorador
- Fachada
- Flyweight
- Proxy

#### Patrones de diseño para concurrencia
Estos patrones de diseño tratan con paradigmas de programación multiproceso. Algunos de los más populares son:

- Objeto activo
- Reacción Nuclear
- Planificador

#### Patrones de diseño para arquitectura
Estos patrones de diseño son usados con fines arquitectonicos. Los mas famosos son:

- MVC (Model-View-Controller)
- MVP (Model-View-Presenter)
- MVVM (Model-View-ViewModel)

En la siguiente sección, se dará un vistazo más de cerca a algunos de los patrones de diseño antes mencionados con ejemplos proporcionados para una mejor comprensión.

### Ejemplos de patrones de diseño
Cada uno de los patrones de diseño representa una solución puntual a un problema en específico. No existe un conjunto universal de patrones que siempre se adapte mejor a determinado entorno. Es necesario saber cuando un patrón en particular resultará útil y si proporcinará un valor real al contexto que se esté enfrentando. Para ello, la experiencia es la mejor herramienta para pulir la capacidad de determinar cuál patrón de diseño utilizar para resolver un problema en particular.

Recuerde que la aplicación de un patrón incorrecto a un ambiente determinado puede provocar efectos no deseados, como por ejemplo, complejidad innecesaria del código, sobrecarga en el rendimiento o la aparición de un anti patrón.

Todos estos elementos deben ser considerados al momento de aplicar un patrón de diseño en el código. Los siguientes ejemplos resultarán útiles para un desarrollador en JavaScript.

#### Patrón constructor
Cuando se piensa en los lenguajes de programación clásicos orientados a objetos, un constructor es una función especial en una clase que inicializa un objeto con un cojunto de valores por defecto. En JavaScript se tienene tres formas para crear objetos: A través de las llaves, por medio del método `create` de `Object`, y con ayuda de la palabra reservada `new`:

```javascript
const instance = {};
const instance = Object.create(Object.prototype);
const instance = new Object();
```

Después de crear un objeto, hay cuatro formas de agregarle propiedades al mismo: la notación punto, la notación de corchetes, el método `defineProperty` de `Object` para definir una sola propiedad y el método `defineProperties` para definit multiples propiedades:

```javascript
instance.key = "A key's values";
instance["key"] = "A key's values";
Object.defineProperty(
  instance,
  "key",
  {
    value: "A key's value",
    writable: true,
    enumerable: true,
    configurable: true,
  }
);
Object.defineProperties(
  instance,
  {
    "firstKey": {
      value: "First key's value",
      writable: true,
    }
    "secondKey": {
      value: "Second key's value",
      writable: false,
    }
  }
);
```

La forma más popular de crear objetos en JavaScript es a través de las llaves, agregando las propiedades pro medio de la notación punto, o la notación de corchetes.

Como se mencionó anteriormente, JavaScript no soporta clases nativas, pero soporta constructores mediante el uso de la palabra `new` como prefijo a una llamada de una función. De esta forma se puede usar la función como un constructor e inicializar sus propoiedades de la misma forma que se hace en un lenguaje clásico con un constructor.

```javascript
function Person(name, age, isDeveloper) {
  this.name = name;
  this.age = age;
  this.isDeveloper = isDeveloper || false;
  
  this.writesCode = function() {
    console.log(this.isDeveloper ? "This person does write code" "This person does not write code");
  }
}

const person1 = new Person("Bob", 38, true);
const person2 = new Person("Bart", 32, false);

person1.writesCode(); // prints out: This person does write code;
person2.writesCode(); // prints out: This person does not write code;
```

Sin embargo, todavía hay un espacio para mejorar esta implementación y es aprovechando el hecho de que JavaScript usa herencia basada en prototipos. Con el código actual, el método `writesCode` se redefine para cada una de las instancias del constructor `Person`. Este detalle se puede evitar definiento el método dentro de la función prototipo:

```javascript
function Person(name, age, isDeveloper) {
  this.name = name;
  this.age = age;
  this.isDeveloper = isDeveloper || false;
}

Person.prototype.writesCode = function() {
  console.log(this.isDeveloper ? "This person does write code" "This person does not write code");
}

const person1 = new Person("Bob", 38, true);
const person2 = new Person("Bart", 32, false);

person1.writesCode(); // prints out: This person does write code;
person2.writesCode(); // prints out: This person does not write code;
```

Ahora, ambas instancias del constructor `Person` pueden acceder a la instancia compartida del método `writesCode`.

#### Patrón módulo
Una de las características de JavaScript es que no admite modificadores de acceso. En un lenguaje de programación orientado a objetos, el desarrolador al momento de definir una clase, determina los derechos de acceso para la misma, y es comun ver palabras claves como `public`, `private` o `protected`. Dado que JavaScript en su forma simple no admite clases ni modificadores de accesos, la comunidad ha encontrado una dorma de imitar este comportamiento cuando sea necesario.

Antes de entrar en los detalles del patrón módulo, es necesario hablar del concepto _closure_. Un closure en una función con acceso al scope del padre, incluso despues de que la función padre se haya cerrado. Por lo tanto, en JavaScript se imita el comportamiento de los modificadores de acceso a través del scope. Se complementa el concepto con el siguiente ejemplo:

```javascript
let counterIncrementer = (function() {
  let counter = 0;
  
  return function() {
    return ++counter;
  };
})();

console.log(counterIncrementer()); // prints out: 1
console.log(counterIncrementer()); // prints out: 2
console.log(counterIncrementer()); // prints out: 3
```

Como se puede observar, al usar IIFE (Inmediately Invoked Function Expression) se ha vinculado la variable de contador a una función que fue invocada y cerrada, pero que aún puede ser accedere a la función interna que incrementa la variable `counter`. Dado que la variable `counter` no se pudede acceder por fuera de la expresión de la función, se convierte en una variable privada mediante la manipulación del scope.

Usando closuere, se puende crear objetos con partes públicas y privadas. Estos se denominan módulos y son útiles cuando se pretende ocultar determinadas partes de un objeto y solo exponer una interfaz al quién este usando el módulo. Por ejemplo:

```javascript
let collection = (function() {
  // private members
  let objects = [];
  
  // public members
  return {
    addObject: function(object) {
      objects.push(object);
    },
    removeObject: function(object) {
      let index = objects.indexOf(object);

      if (index >= 0)
        objects.splice(index, 1);
    },
    getObject: function() {
      return JSON.parse(JSON.stringify(objects));
    },
  }
})();

collection.addObject("Bob");
collection.addObject("Alice");
collection.addObject("Franck");
console.log(collection.getObjects()); // prints ["Bob", "Alice", "Franck"]
collection.removeObject("Alice"); 
console.log(collection.getObjects()); // prints ["Bob", "Franck"]
```

Lo más útil que ofrece este patrón es la separación de las partes públicas y privadas de un objeto. Este concepto es familiar para los desarrolladores con conocimiento en la programación orientada a objetos.

No obstante, en JavaScript se siguen presentando algunas particularidades. Si se desea cambiar la visibilidad de un miembro, se debe modificar el códgo en cada una de las implementaciones que hayan consumido dicho miembro, por la naturaleza de como se acceden a las partes públicas y privadas del objeto. Por otra parte, los métodos que se agregan al objeto posterior a su creación no pueden acceder a los miembros privados del mismo.


#### Patrón de módulo revelador

Este patrón es una mejora del patrón que se explicó anteriormente. La principal diferencia es que ahora la lógica del objeto estará en el ámbito privado del módulo y luego se expone las partes públicas mediante el retorno de un objeto anónimo. Una ventaja con este enfoque es que ahora se podrá cambiar el nombre de los miembros privados cuando se asignen a los miembros públicos. A continuación se muestra el código que aplica el patrón

```javascript
const namesCollection = (function() {
    // private members
    var objects = [];

    function addObject(object) {
        objects.push(object);
    }

    function removeObject(object) {
        var index = objects.indexOf(object);
        if (index >= 0) {
            objects.splice(index, 1);
        }
    }

    function getObjects() {
        return JSON.parse(JSON.stringify(objects));
    }

    // public members
    return {
        addName: addObject,
        removeName: removeObject,
        getNames: getObjects
    };
})();

namesCollection.addName("Bob");
namesCollection.addName("Alice");
namesCollection.addName("Franck");
console.log(namesCollection.getNames()); // prints ["Bob", "Alice", "Franck"]
namesCollection.removeName("Alice");
console.log(namesCollection.getNames()); // prints ["Bob", "Franck"]
```

El patrón de módulo revelador es una de la tres formas en la que se puede implementar el patrón módulo. Cómo se señalo anteriormente, su particularidad esta en como se exponen los miembros públicos. Con esta implementación es mucho más fácil y legible el acceso a los objetos. No obstante, este patrón puede resultar frágil en los siguientes escenarios:

1. Al tener una función privada que se refiere a una función pública, no se puede anular la función pública ya que la función privada continuará refiriéndose a la implementación privada de la función, introduciendo así inconsistencia.
2. Al tener un miembro público apuntando a una variable privada que intenta anular el miembro público por fuera del módulo, la otras funciones seguirán haciendo referencia al valor privado de la variable.

Es importante tener presente que bajo este esquema, la parte privada del objeto esta en capacidad de suprimir la parte público de no ser bien manejado.

#### Patrón singleton
El patrón singleton es utilizado cuando se necesita exactamente una instancia de una clase. Por ejemplo, cuando se precisa tener un objeto que contenga una configuración para un fin determinado. Para este casos, no es necesario crear un nuevo objeto cada vez que se requiera el objeto de configuración en algun lugar del sistema. En su lugar, se instancia el singleton como se muestra a continuación:

````javascript
let singleton = (function() {
  let config;
  
  function initializeConfiguration(values) {
    values = values || {};
    this.randomNumber = Math.random();
    this.number = values.number || 5;
    this.size = values.size || 10;
  }
  
  return {
    getConfig: function(values) {
      if (config === undefined)
        config = new initializeConfiguration(values);
        
      return config;
    }
  };
})();

let configObject = singleton.getConfig({ "size": 8 }) // prints number 5, size: 0, randomNumber: {{ some random decimal value }}

let configObject1 = singleton.getConfig({ "number": 8 }) // prints number 8, size: 10, randomNumber: {{ same random decimal value as in first log  }}
```

Algo importante a resaltar en este snippet, es que el número aleatorio y los valores que se pasan como parámetro de la función `getObject` son siempre los mismos. Por otra parte, el singleton solo debe tener un solo pundo de acceso para recuperar sus valores. Una de sus desventajas es que es un patrón díficil de probar ya que simular el comportamiento de una instancia singleton es complicado porque no hay control sobre su creación.

#### Patrón de observador
El patrón de observador es una herramienta muy útil cuando hay un escenario en el que se precisa mejorar la comunicación entre partes dispares del sistema de forma optimizada. Promueve el acoplamiento flojo entre objetos.

Hay varias versiones de este patrón, pero en su forma más básica hay dos partes principales: el sujeto y el observador.

Un sujeto maneja las tres posibles operaciones relacionadas con un tema determinado al que se suscriben los observadores. Estas operaciones puntualmente son: la suscripción, dar de baja a la suscripción y la notificación de un evento público que ocurre sobre el tema en cuestión.

Sin embargo, hay una variación llamada editor/suscriptor cuya principal diferencia es que se promueve un acoplamiento más flexible.

En el patrón observador, el sujeto tiene las referencias a los observadores suscritos y llama los métodos desde los mismos objetos. Por otra parte, en el patrón editor/suscriptor se tienen canales que sirven como puente de comunicación entre las partes. El editor dispara un evento y ejecuta la función callback enviada para ese evento.

A continuación se presenta un ejemplo corto del patrón editor/suscriptor. La implementación del patrón observador puede encontrarse fácilmente en línea.

```javascript
let publisherSubscriber = {};

(function container {
  let id = 0;
  
  container.subscribe = function(topic, callback) {
    if (!(topic in container)) {
      container[topic] = [];
    }
    
    container[topic].push({
      "id": ++id,
      "callback": callback,
    });
    
    return id;
  }

  container.unsubscribe = function(topic, id) {
    let subscribers = [];
    
    for (let subscriber of container[topic]) {
      if (subscriber.id !== id) {
        subscribers.push(subscriber);
      }
    }
    
    container[topic] = subscribers;
  }
  
  container.publish = function(topic, data) {
    for (let subscriber of container[topic]) {
      subscriber.callback(data);
    }
  }
})(publisherSubscriber);

let subscriptionId1 = publisherSubscriber.subscribe("mouseClicked", function(data) {
  console.log("I am Bob's callback function for a mouse clicked event and this is my event")
});

let subscriptionId2 = publisherSubscriber.subscribe("mouseHovered", function(data) {
  console.log("I am Bob's callback function for a mouse hovered event and this is my event")
});

let subscriptionId3 = publisherSubscriber.subscribe("mouseClicked", function(data) {
  console.log("I am Alice's callback function for a mouse clicked event and this is my event")
});

publisherSubscriber.publish("mouseClicked", {"data": "data1"});
publisherSubscriber.publish("mouseHovered", {"data": "data2"}); // output: there are 3 logs executed

publisherSubscriber.unsubscribe("mouseClicked", subscriptionId3);

publisherSubscriber.publish("mouseClicked", {"data": "data1"});
publisherSubscriber.publish("mouseHovered", {"data": "data2"}); // output: there are 2 logs executed
```

Este patrón de diseño es útil en situaciones en dodne se necesitar realizar varias operaciones en un solo evento que se dispara. Imagine que hay un escenario en donde se requiere hacer múltiples llamadas AJAX a un servicio backend y luego hacer otra llamadas AJAX dependiendo del resultado. Una opción es anidar las llamadas AJAX exponiendose así al callback hell. Por otra parte el patrón editor/suscriptor ofrece una solución mucho más elegante.

La desventaja de usar este patrón es su dificultad para probar varias partes del sistema. No existe una forma elegante de saber si las partes suscritas del sistema se están comportando como se espera.

#### Patrón mediador
El patrón mediador resulta útil cuando se cuenta con sistemas desacoplados. Bajo un contexto en el que varias partes de un sistema requieren comunicarse y coordinarse, probablemente el una buena solución sería introducir un mediador.

Un mediador es un objeto que se utiliza como punto central para la comunicación entre partes dispares de un sistema y maneja el flujo de trabajo entre ellas. Es importante resaltar que el mediador maneja el flujo de trabajo ya que existe una gran similitud con el patrón editor/suscriptor. La diferencia entre un mediador y un editor/suscriptor es que el el primero maneja el flujo de trabajo mientras que el segundo usa un tipo de comunicación disparar y olividar. El editor/suscriptor es un agregador de eventos, lo que significa que se encarga de activar y de informar a los suscriptores pertinentes qué eventos se activaron. Al agregador de eventos no le importa cuantas veces se activa un evento, algo que si es relevante para un patrón mediador.

Un ejemplo de mediador es un tipo de interfaz asistente. Se va a suponer que se tienen un proceso de registro extenso para un sistema en el que se ha trabajado. A menudo, cuando se requiere mucha información del usuario, es una buena práctica dividirla en varios pasos. De esta forma, el código será más limpio y mantenible y el usuario no se verá abrumado por la cantidad de información que se solicita para finalizar el registro.

Un mediador es un objeto que manejaría los pasos de registro, teniendo en cuenta los diferente flujos de trabajo que podrían ocurrir por el simple hecho de que cada usuario tiene un proceso de registro único.

El beneficio obvio de este patrón de diseño es la mejora de la comunicación entre las partes de un sistema, que ahora se comunican a través del mediador.

La desventaja es que ahora se ha introducido un punto de falla en el sistema, lo que significa que si el mediador falla, todo el sistema podría dejar de funcionar.

#### Patrón prototipo
Como ya se mencionó, JavaScript no soporta clases de forma nativa. La herencia entre objetos es implementada usando programación basada en prototipos.

Esto permite crear objectos que pueden servir como prototipo para otros objetos que se están creando. El objeto prototipo se utiliza como modelo para cada objeto que crea el constructor.

A continuación se muestra un ejemplo sencillo para complementar el uso de este patrón:

```javascript
const personPrototype = {
  sayHi: function() {
    console.log(`Hello, my name is ${this.name}, and I am ${this.age}`)
  }
  sayBye: function() {
    console.log(`Bye bye!`)
  }
};

function Person(name, age) {
  name = name || "John Doe";
  age = age || 26;
  
  function constructorFunction(name, age) {
    this.name = name;
    this.age = age;
  }
  
  constructorFunction.protoype = personPrototype;
  
  let instance = new constructorFunction(name, age);
  
  return instance;
}

let person1 = Person();
let person2 = Person("Bob", 38);

person1.sayHi(); // prints: Hello, my name is John Doe, and I am 26
person1.sayHi(); // prints: Hello, my name is Bob, and I am 38
```

Observe como la herencia de prototipos también mejora el rendimiento porque ambos objetos contienen una referencia a las funciones que se implementan en el propio prototipo, en lugar de cada uno de los objetos.
