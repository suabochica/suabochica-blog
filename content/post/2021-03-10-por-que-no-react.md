+++
title = "Por qué no React?"
author = ["Sergio Benítez"]
description = "Serie que recopila una descripción general de React"
date = 2021-03-10T00:00:00-05:00
lastmod = 2021-10-07T06:17:42-05:00
draft = false
tags = [
  "react"
]
+++

Con el propósito de abordar un panorama general sobre React, es momento de presentar los posibles problemas a los que se enfrete por haber elegido esta libreria.

En esta publicación se van a compartir las preocupaciones comunes que se han manifestado en React por parte de la comunidad de desarroladores. Uno de los principales puntos en contra era su licencia patentada. No obstante, con la liberación de la versión 16 de React, el proyecto ahora usa el estandár MIT con una licencia de código abierto. Este es un testimonio de la inicativa que tiene la comunidad que soporta React para ir mejorando el proyecto.

Por otra parte, la idea es identificar que algunas de las preocupaciones son validas y otras son simplemente conceptos erróneos o problemas que pueden mitigarse fácilmente.


## Preocupación 1: HTML y JSX difieren {#preocupación-1-html-y-jsx-difieren}

Repasemos el contexto de como la sintáxis opcional JSX se compila en JavaScript:

```javascript
<h1 color="red"> Heading here </h1>
// is equal to
React.createElement("h1", {color: "red"}, "Heading here");
```

Cabe resaltar que el uso de JSX es opcional, pero gracias a su similitud con HTML, y su legibilidad, ha sido fuertemente aceptada por la comunidad. El JavaScript pleno que corresponde a la última parte del ejemplo es lo que realmente se envía al navegador.

Ahora bien, la sintáxis JSX es 99% lo mismo a HTML pero difieren en cosas muy puntuales que son agrupadas en la siguiente tabla:

| HTML                 | JSX                      |
|----------------------|--------------------------|
| for                  | htmlFor                  |
| class                | className                |
| <style color="blue"> | <style={{color:'blue'}}> |
| <!-- Comment -->     | {_\* Comment \*_}        |

Como se puede observar, aprender estás diferencias es trivial, ya que la lista de diferencias es muy corta y fácil de asimilar.

Una preocupación frecuente con los primeros pasos que se dan en React es cuando los proyectos cuentan con un buen número de archivos HTML, ya que eso significaría que se deben convertir muchos archivos al formato JSX. Actualemente, React ofrece las siguientes formas para manejar esta transición:

-   Usar la función buscar y reemplazar del editor
-   Usar un compilador en línea
-   Usar el paquete `htmltojsx` en npm

La elección es parte del desarrollador, pero en últimas si es necesario hacer la conversión a JSX lo cual en definitiva es una molestía.


## Preocupación 2: Paso de compilación es requerido {#preocupación-2-paso-de-compilación-es-requerido}

Esta preocupación esta muy relacionada con la anterior, ya que como se menciono antes, para al usar la sintáxis JSX React requiere de un paso de construcción que compile el JSX en llamados JavaScript planos que el navegador pueda comprender.

En la práctica esto es trivial para el desarrollo de las aplicaciones web modernas, ya que exigen un paso de construcción para ejecutar tareas de minificación y unificación de archivos que reduzcan el uso de ancho de banda. Tareas de traspilación para que el código que use las funcionalidades modernas de JavaScript pueda ser interpretado en la mayoría de los navegadores, y por último, aplicar linters y correr pruebas automatizadas sobre el código de la aplicación web.

En consecuencia, compilar el código JSX es una labor que se puede realizar de manera automática en este paso de construcción. Por otra parte, los traspiladores más famosos como los son, Babel y TypeScript, están en campacidad de compilar la sintáxis JSX por nosotros.

Por último, el paquete Create React App es un generador de proyectos que le ahorra al desarrollador todos los pasos de configuración necesarios para crear una aplicación web moderna con tan solo correr un comando. Al correr este comando accedemos a un paso de construcción por defecto que administra varias tareas, entre ellas la compilación automática de JSX.


## Preocupación 3: Conflictos entre versiones {#preocupación-3-conflictos-entre-versiones}

Como se mencionó anterior mente React con React DOM pesan al rededor de 35Kb al ser minificado y comprimido con gzip. Este tamaño es razonable pero hay algunas desventajas que se presentan en tiempo de ejecución. Por ejemplo, no se pueden correr dos versiones de React al mismo tiempo en la misma página. Por ende, los todos los componentes web deben respetar la misma versión es su respectiva página.

Si se compara con la construcción estandarizada de componentes web, no es necesario enfrentarse a conflictos entre versiones porque este enfoque no precisa de tiempos de ejecución ya que el componente se construye directamente en el navegador. No obstante, en la publicación pasada se agruparon los argumentos para seguir favoreciondo el uso de React sobre el estándar web.

Por otra parte, hay herramientas interesantes a considerar como Svelte o Skate, que ofrecen todas las funcionalidades para un desarrollo de componentes web sin depender de un framework y incluyen el tiempo de ejecución en cada componente, evitando así potenciales conflictos entre versiones.

Finalmente, dado que React es una librería que puede complementarse con otras herramientas para lograr propósitos puntuales, como por ejemplo React Router, se requiere hacer una validación de la compatibilidad de veriones entre ambas tecnologías.

A contincuación se comparten tres consejos para evitar conflictos entre versiones:

1.  Establecer el estándar sobre una versión
2.  Actualice React cuando actualice librerías relacionadas
3.  Maneje la actualización de versioines a nivel de equipo


## Preocupación 4: Recursos desactualizados {#preocupación-4-recursos-desactualizados}

Otro tema a lidiar con React es la revisión de contenidos desactualizados en búsquedas web. React tiene una comunidad muy grande y fue liberado como código abierto en 2013. Al hacer una búsqueda de _react example_ en Google, se obtienen más de 300 millones de resultados. En StackOverflow hay más de 189k hilos asociados a la etiqueta React. Esta bien tener muchos recusros pero hay un riesgo alto y es la consulta de contenidos depreciados.

Desde la versión del 2013 hasta el día de hoy se han presentado varios cambios dentro de React, y por ende algunos patrones y algunas funcionalidades ya han sido reemplazados. Un ejemplo puntual es el siguiente:

```javascript
// Viejo
import {render} from 'react';
```

```javascript
// Nuevo
import {render} from 'react-dom';
```

Se recuerda, que hoy en día React tiene soporte en diferentes plataformas, como ReactNative o ReactVR, por lo tanto es conveniente importar la función render de la librería adecuada. Otro ejemplo es la creación de clases:

```javascript
// Viejo
React.createClass
```

```javascript
// Nuevo
var crc = require('create-react-class');
```

En las últimas versiones, para seguir usando el estilo `createClass` es necesario importar el paquete `'create-react-class'`. El último ejemplo de estaś transiciones esta relacionado con los mixins y su reemplazo llamado hooks.

La moraleja esta en siempre revisar la documentación oficial de React para estar al tanto de las funcionalidades y los patrones que se han venido actualizando.


## Preocupación 5: Fatiga de toma de deciciones {#preocupación-5-fatiga-de-toma-de-deciciones}

La última preocupación a abordar es la fatiga en la toma de decisiones que expone React. Al ser una librería tan ligera y flexible el desarrollo se abre a un campo en donde existen muchas alternativas para hacer las mismas cosas.

Los primeros pasos en React pueden resultar intimidantes. Para asimilar esta inducción una recomendación es definir el desarrollo en función de estas cinco decisiones claves:

1.  Ambiente de desarrollo
2.  Clases o funciones
3.  Tipos
4.  Estado
5.  Estilismo

Tiempo para desarrollar cada una de estas decisiones.

Primero está el ambiente de desarollo. Actualmente, GitHub cuenta con más de [100 proyectos](https:javascriptstuff.com/react-starter-projects) para crear entornos de desarrollo con React. En consecuencia, puede ser una ardua tarea revisar la mayoria de estos generadores de proyecto en React para identificar cuál se debe usar. La recomendación es usar `create-react-app` puesto que es el entorno de desarrollo oficial apoyado por Facebook. Este geneardor es una plataforma madura para crear aplicaciones React y ofrece las siguientes caracteristicas:

-   Pruebas automatizadas
-   Transpilación
-   Linting
-   Empaquetación (Bundling)
-   Compilación automatizada

Algunos desarrolladores optan por construir su propio ambiente React. No está de más intentar hacer esta configuración.

La segunda decisión es si se declaran los componentes a través de clases o funciones. A continuación se muestran dos snippets con dichas aproximaciones:

```javascript
// Class
class Greeting extends React.Component {
  render() {
    return <h1>Hello</h1>
  }
}
```

```javascript
// Function
function Greeting {
  render() {
    return <h1>Hello</h1>
  }
}
```

Ambos endoques cumplen el mismo objetivo, pero hoy en día los desarrolladores de React han optado por usar los componentes como funciones, ya que su sintáxis es mas concisa y tiende a evitar bugs.

La tercera decisión es el manejo de tipos con las siguientes alternativas: PropTypes, TypeScript ó Flow. A continuación se revisa la propuesta de PropTypes con un componente simple llamado `Greeting`:

```javascript
import React from "react";
import PropTypes from "prop-types";

// Function
function Greeting(props) {
  render() {
    return (<h1>Hello {props.name}</h1>)
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```

Para este ejemplo las propiedades del componente están declaradas al final. Se resalta que con PropTypes los tipos son validados solo en tiempo de ejecución y durante el desarrollo.

La segunta opción es TypeScript y se versión es la siguiente:

```javascript
import * as React from "react";

interface Props {
  name: string;
}
// Function
function Greeting(props: Props) {
  render() {
    return (<h1>Hello {props.name}</h1>)
  }
}
```

En esta versión se usa una funcionalidad de TypeScript llamada `interface` para establecer las propiedades del componente y dentro de la definición del componente se aclara que el argumento `prop` es de typo `Props`. A diferencia de PropTypes, con TypeScript las validaciones son hechas en tiempo de compilación, lo que significa que los errores serán identificados más temprano.

La tercera opción es Flow, un proyecto de Facebook para agregar validaciones de tipos estáticos a JavaScript. A diferencia de TypeScript, Flow utiliza anotaciones sobre el código JavaScript para inferir los tipos del mismo. El siguiente ejemplo es la versión del `Greeting` component con Flow:

```javascript
// @flow
import React from "react";

type Props {
  name: string;
}
// Function
function Greeting(props: Props) {
  render() {
    return (<h1>Hello {props.name}</h1>)
  }
}
```

El punto más relevante en este snippet es la anotación al principio de cada archivo para habilitar la validación por parte de Flow. La declaración de los props y su especificación del tipo en el argumento que recibe la función del componente es similar a la versión de TypeScript. Ahora bien, Flow corre en un proceso diferente y por ende los tipos son validados cuando se corre dicho proceso.

Para un primer acercamiento a React, PropTypes es recomendable ya que su aprendizaje es trivial y no requiere de configuraciones adicionales. Si el desarrollador esta familiarizado con TypeScript, la mejor opción es optar por dicha propuesta, resaltando que `create-react-app` ya tiene una configuración por defecto para suministra soporte a TypeScript. Hoy en día, TypeScript es la decisión más popular entre los desarrolladores.

La cuarta decisión es el manejo de estado, cuyo contexto suele apoyarse en librerías externas a React. Se resalta que al hablar de estado se hace referencia a los datos de la aplicación web. Para el manejo de estado se tienen las siguientes alternativas: React plano, Flux, Redux y MobX.

React plano funciona muy bien, por lo cuál el uso de una librería externa es algo opcional. La diferencia entre React plano y las librerías externas es el enfoque, ya que React plano administra el estado a través de componentes mientras que Redux o Flux lo hacen a través de la centralización del mismo conun almacenamiento inmutable. Actualemente, la opción más popular entre las librerias es Redux.

Por último, esta la propuesta de MobX que resulta más ligera ya que su enfoque esta basado en administrar el estado a través de observables como estructuras de datos.

La última decisión es la de estilismo que muchos consideran como absurda ya que actualmente hay más de 50 alternativas para afrontar el problema. No obstante, se recalca que React funciona espectacularmente con el tradicional CSS y también con uno de los preprocesadores más populares como lo es SASS. En ese orden de ideas, la recomendación es usar el enfoque que usted ya conozca.

En resumidas cuenta se tiene:

| Decisión               | Recomendación           |
|------------------------|-------------------------|
| Ambiente de desarrollo | create-react-app        |
| Clases o funciones     | Funciones               |
| Tipos                  | PropTypes ó TypeScript  |
| Estado                 | React plano             |
| Estilismo              | Lo que usted ya conozca |

Definitivamente, la cantidad de opciones que se tiene para aforntar problemas puntuales con React es algo intimidante. Por lo tanto, se motiva a revisar estás recomendaciones como un punto de partidad para que usted establezca su propio criterio.


## Resumen {#resumen}

En este módulo, se consideraron las preocupaciones comunes que se escuchan por trabajar con React: JSX difiere de HTML pero hay varias herramientas que permiten convertir de una sintáxis a otra. El paso de compilación requerido para JSX ya es algo trivial puesto que el paso de compilación va a ser necesario para otro tipo de tareas, como por ejemplo la minificación de arcivos. Los conflictos potenciales entre las versiones de React se sobrellevan con actualizaciones sencillas a través de codemods cuando sea necesario. Las viejas funcionalidades en búsquedas web promueven el uso efectivo de documentaciones oficiales para evitar confusiones. Y por último esta la fatiga en toma de decisiones, una preocupación empírica cuya recomendación es comenzar de lo sencillo a lo complejo para ir construyendo un criterio propio.
