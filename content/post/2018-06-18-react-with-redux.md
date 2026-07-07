+++
title = "React con Redux"
date = 2018-06-18T02:13:50Z
author = "Sergio L. Benítez D."
description = "Notas sobre cómo conectar componentes React al store de Redux para gestionar el estado de la aplicación."
tags = [
    "javascript",
    "react",
    "redux",
]
+++

Redux se puede usar con:

- Aplicaciones React
- Aplicaciones Vue
- Aplicaciones HTML simples
- Aplicaciones JavaScript vanilla

En esta lección veremos cómo conectar Redux a una aplicación que usa React para su interfaz de usuario.
Los cambios están regsitrados en el [commit#c4c81c6](https://github.com/suabochica/react-nanodegree/commit/c6cf9a4b8f33adbd6425c8ba3ba94b845a091168) del respositorio react-nanodegree.

React como UI
-------------

Es hora de convertir nuestra aplicación de tareas de HTML plano a una basada en React. Para ello, necesitaremos añadir:

- react
- react-dom
- babel

Los cambios que acabamos de implementar deberían resultarte familiares: simplemente hemos convertido partes de nuestra app de HTML a Componentes de React.

### Combinando React y Redux

Muy bien, ya has aprendido React. Has construido Redux y lo has usado en una aplicación HTML normal. Pero ahora hemos empezado a convertir ese HTML a una aplicación React. Es el momento de conectar los _Componentes de React_ al _Store de Redux_.

Es importante notar dos cosas:

- Dónde va el código de `store.dispatch()` en el componente React
- Cómo se pasa el store de Redux al componente React como prop

Un detalle importante es que en el `index.html` tenemos dos aplicaciones de TAREAS/OBJETIVOS:

1. La app construida con JavaScript vanilla
2. La app construida con React

Ambas aplicaciones comparten el mismo store de Redux. Esta es la razón del comportamiento que observamos hasta ahora: cada vez que añadimos un ítem a la lista desde la app React, los cambios se reflejan en la app de JavaScript vanilla.

Otro tema en el uso de React es el manejo del DOM. En la app de JavaScript vanilla, cuando llamábamos a `store.subscribe` para recibir notificaciones de los cambios del store, lo que hacíamos era obtener los nuevos _goals_ y los nuevos _todos_ y, por cada uno de esos ítems, los añadíamos al DOM.

Con React, no tienes que hacer nada de manipulación del DOM, porque React se encarga de todo eso por nosotros. Sin embargo, aún queremos `subscribe()` al store dentro de nuestro componente React para re-renderizar nuestros componentes. Para re-renderizar un componente en React deberíamos llamar a `setState()` para indicarle a React que el estado de la app ha cambiado y que actualice la UI. En este caso, no tenemos estado dentro de este componente y no tiene sentido añadir ninguno solo para provocar un re-renderizado. Así que podemos usar un anti-patrón llamando al método `this.forceUpdate()` del componente React para re-renderizar ese componente específico sin modificar el estado.

### Resumen

En esta sección, convertimos nuestra aplicación de HTML plano a una que usa Componentes de React. No implementamos ninguna funcionalidad nueva. En su lugar, mejoramos la organización del código separando distintas partes en bloques reutilizables.
