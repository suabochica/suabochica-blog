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