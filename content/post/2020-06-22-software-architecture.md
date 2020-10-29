+++
title = "Guía comprensiva sobre patrones de diseño en JavaScript"
date = 2020-06-23T02:13:50Z
author = "Sergio L. Benítez D."
description = "Breve introdución de programación funcional en JavaScript."
+++

Un buen desarrollador en JavaScript debe luchar por escribir un código limpio, saludable y mantenible. Con frecuencia el desarrolador resuelve problemas interesantes que no necesariamente requieren soluciones únicas y paralelamente es posible que haya escrito un código similar para solucionar un problema completamente diferente ha alguno que haya manejado antes. Puede que no lo sepa, pero al hacer esto ha utilizado un **patrón de diseño** en JavaScript.

> Los patrones de diseño son soluciones retutilizables a problemas comunes en el diseño de software.

En cualquier lenguaje de programación hay un gran número de desarrolladores que hacen y prueban muchas de estas soluciones reutilizables. En consecuencia, tales soluciones resultan ser tan útiles ya que nos ayudan a escribir código de manera optimizada para la reparación de problemas frecuentes.

Los principales beneficion que se obtienen de los patrones de diseño son:

- **Son soluciones comprobadas:** Debido a que los patrones de diseño son recursos utilizados con frecuencia por desarrolladores, puede estar seguro de que funcionan. Además, puede tener la certeza de que se revisaron varias veces yy que probablemente se implementaron optimizaciones.
- **Son fáciles de reutilizar:** Los patrones de diseño documentan soluciones reutilizables que pueden ser modificados para resovler multiples problemas particulares, ya que no están vinculado a un problema en específico.
- **Son expresivos:** Los patrones de diseño pueden explicar una admirable solución de manera elegante y sencilla.
- **Facilitan la comunicación:** Cuando los desarrolladores están familiarizados con los patrones de diseño, pueden comunicarse más facilmente entre sí acerca de las posibles soluciones a un problema determinado.
- **Previenen la necesidad de refactorizar el código:**Cuando una aplicación está escrita con patrones de diseño, hay una alta probabilidad de no acudir a refactorizar el código más adelante puesto que un patrón de diseño aplicado correctamente a un problema determinado ya es un solucion óptima.
- **Reducen el tamaño del código base:** Los patrones de diseño al ser soluciones elegantes y óptimas, requiren menoscódigo que otras soluciones.

Con estos beneficios en mente, vamos a revisar algunos patrones de diseño aplicados sobre JavaScript. 

Una breve historia de Javascript
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
En JavaScript es común mencionar el término **callback function**. Un callback function, es una función que se envía como paramétro de otra función, y es ejecutsda después de que se active un evento. Recuerde que en JavaScript las funciones son ciudadanos de primera clase. Usualmente las callback function se subscriven a eventos como el clic de un mouse ó le presión de una tecla del teclado.

Un evento debe tener un listener adjunto para ser atendido, de lo contrario, sería un evento perdido. Cada vez que un evento se ejecuta, un mensaje es enviado a una fila de mensajes que son procesados sincronicamente, a través de un mecanismo FIFO (First in first out). Toda esta estructura es conocida como el **event loop** y la siguiente imagen ayuda a sintetizar dicha estructura.

![]()

Cada uno de los mensajes en la fila tiene una función asociada. Una vez el mensaje sale de la fila, la función adjunta es ejecutada completamente en tiempo de ejecución antes de procesar el siguiente mensaje. En otras palabras, si dicha función tiene un llamado a otra función, amabos funciones serán ejecutadas antes de procesar el siguiente mensaje en la fila. Este comportamiento es conocido como _run-to-completion_, y en pseudo código puede ser:

```javascript
while (queue.waitForMessage()) {
    queue.processNextMessage();
}
```

El método `queue.waitForMessage()`  espera los nuevos mensajes de manera sincrona. Cada uno de los mensajes que está siendo procesado tiene su propio stack, y es procesado hasta que la pila este vacía. Una vez termina, un nuevo mensaje es atendido desde la fila, en caso que exista.

Es importante mencionar que JavaScript es un lengaje _non-blocking_, lo que significa que cuando una operación asíncrona esta siendo procesada, el programa esta en capacidad de procesar otras cosas, tales como, la recepción de nuevas entradas por parte del usuario, y no bloquea la ejecución del hilo principal. Esta propiedad es muy útil y es un tema que puede abordar un árticulo completo. Por ahora, vamos a dejarlo por fuera del alcance de esta publicación.