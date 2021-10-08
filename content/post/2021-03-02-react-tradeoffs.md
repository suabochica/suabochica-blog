+++
title = "Compensaciones de React"
author = ["Sergio Benítez"]
description = "Serie que recopila una descripción general de React"
date = 2021-03-02T00:00:00-05:00
lastmod = 2021-10-06T20:21:48-05:00
draft = false
tags = [
  "react"
]
+++

React es una excelente librería pero no se debe dejar sesgar por su popularidad. La comunidad ha identificado 6 compensaciones claves a las que el desarrollador se expone por usar React. Dichos puntos serán desarrollados a continuación.


## Tradeoff 1: Frameworks vs. Librería {#tradeoff-1-frameworks-vs-dot-librería}

Los competidores de React, como Angular o Vue se ofertan en el mercado como frameworks y no como librerías. Fundamentalmente, considerar que un framework es mejor que una libreria es un tema de compensaciones.

A continuación se enlistan las ventajas de seguir un enfoque de framework:

-   Los frameworks contienen opiniones más claras sobre su uso, y por ende se puede gastar menos tiempo tratando de escoger entre muchas opciones. Definitivamente esta característica reduce la fatiga en la toma de decisiones.
-   Las opiniones claras sobre los usos de un framework también implican menos gastos en configuraciones.
-   Los frameworks pueden reforzar la coherencia entre los miembros de un equipo

Por otro lado, el enfoque de librería de React también ofrece sus ventajas:

-   React es mucho más ligero que la mayoría de los frameworks
-   El peso bajo de React, permite que se pueda espolvorear sobre aplicaciones existentes. (e.g. El caso de Facebook que migró su aplicación PHP a React paulatinamente)
-   React lo obliga a eligir solo las cosas que necesita para su trabajo. Al dar ese poder de elección en definitiva se puede escoger las mejores tecnologías.
-   Actualmente, React cuenta con plantillas populares sobre las cuales se puede trabajar.

Como React es una librería enfocada a componentes, sus funcionalidades son muy limitadas si se compara con un framework como Angular. En la siguiente tabla se especifica como suplir las funcionalidades que por defecto ya estan incuildas en el framework:

| Features        | React            | Angular     |
|-----------------|------------------|-------------|
| Components      | ✔                | ✔           |
| Testing         | Jest             | ✔           |
| HTTP library    | Axios            | ✔           |
| Routing         | React Router     | ✔           |
| i18n            | react-intl       | ✔           |
| Animation       | react-motion     | ✔           |
| Form validation | react-forms      | ✔           |
| CLI             | create-react-app | angular-cli |

Con React se puede halar solo los elementos que precisamos de la lista.


## Tradeoff 2: Conciso vs. Explícito {#tradeoff-2-conciso-vs-dot-explícito}

React obliga a gastar algo de tiempo en ser más explicíto al momento de cablear una lógica de presentación. Esta característica ayuda a que se obtenga un mejor entendimiento sobre lo que el código esta haciendo. Un ejemplo concreto es el _two-way binding_ contra el _one-way binding_.

Angular es popular por usar un enfoque de encuadernación bidireccional para sincronizar la actualización de datos con el evento que los dispara. El siguiente snippet es un ejemplo de como se regulizar datos en Angular.

```javascript
let user = 'Cory';

<input
  type="text"
  value={user}
/>
```

Se puede observar que es un código corto, pero por debajo se están ejecutando diferentes procesos de manera automática para sincronizar los datos.

En contraste, React usa un enfoque de encuadernación unidireccional. A continuación se muestra un código de ejemplo que sería el equivalente a la muestra anterior, pero bajo el contexto de React:

```javascript
state = { user: 'Cory' };

function handleChange(event) {
  this.setState({
    user. event.target.value
  });
}

<input
  type="text"
  value={this.state.user}
  onChange={this.handleChange}
/>
```

Notesé que en esta versión se es mucho mas descriptivo. Aquí se declara explícitamente un manejador del evento `change` que se referencia en uno de los atributos de la etiqueta `<input>`. Este trabajo extra, tiene sus beneficios. El primero es que le ofrece al desarrollador más control en la implementación por que no hay código que se este ejecutando por defecto. Al ser más explícito, es evidente el cambio del estado generado por la ocurrencia de un evento, dando un mejor entendimiento de la lógics y haciendo más sencillo el proceso de depuración.

En resumen, esta es la compensación. Con React las implementaciones son más extensas pero el beneficio que generan es que resulta un código fácil de mantener, con mayor claridad, fiabilidad y rendimiento.


## Tradeoff 3: Centrado a plantillas vs. Centrado a JavaScript {#tradeoff-3-centrado-a-plantillas-vs-dot-centrado-a-javascript}

En la publicación previa, se expusieron los enfoques "JS" en HTML y "HTML" en JS. El primer enfoque es el que usan frameworks como Angular en donde es preciso aprender su sintáxis única. Por otro lado, el enfoque de React motiva a aprender JavaScript. Para revisar esta compensación se comparte el siguiente ejemplo, en donde a través de una condición sobre el rol de usuario se establece si se muestra o no un encabezado. La versión Angular del ejemplo sería:

```javascript
<h1 *ngIf="isAdmin">Hi Admin</h1>
```

Para Vue, el enfoque es similar, con la diferencia de que hay que usar su propia sintáxis:

```javascript
<h1 v-if="isAdmin">Hi Admin</h1>
```

En el caso de React se usa el operador lógico de JavaScript `&&`

```javascript
{isAdmin && <h1>Hi Admin</h1>}
```

La parte que no es familiar, es el lado derecho del operador. No obstante, se debe tener presenta que la rendericacón del "Hi Admin" solo se presenta si la condicion `isAdmin` es verdadera. Al ser este enfoque JavaScript pleno, se puede obtener soporte de autocompletar y mensajes de error suministrados por el editor.

Ahora consideré un ejemplo con loops. La versión angular sería:

```javascript
<h1 *ngfor="let user of users">{{user.name}}</h1>
```

Para Vue, el enfoque es similar, con el detalle que se menciono anteriormente:

```javascript
<h1 v-for="user in users">{{user.name}}</h1>
```

En el caso de React se usa la función `.map` de JavaScript para iterar sobre los arreglos, y a través de una función lambda se accede a la propiedad `name` del item `user`.

```javascript
users.map(user => <div>{user.name}</div>)
```

Nuevamente, lo único diferente en la versión de React es su sintáxis del retrono de la función lambda envuelta en unas llaves. Se recuerda que el enfoque de React no es preferible por el hecho de que sea corto, si no por su característica de ser puro JavaScript.

Por último se evalua un ejemplo con el evento de dar clic en un botón de borrar. La versión de angular es:

```javascript
<button (click)="delete()">Delete</button>
```

Para Vue se tiene:

```javascript
<button v-on:click="delete">Delete</button>
```

En el caso de React se declara un manejador `onClick` nativo de JavaScript en camleCase para identificar la sintáxis JSX y dentro de las llaves se hace el llamado a la función que manejará el clic.

```javascript
<button onClick={delete}>Delete</button>
```

Como conclusión de todos ejemplos, ya es notable el porque el API de React es mucho más pequeño en tamaño que sus competidores, puesto que usa JavaScript plano. Por otra parte, el uso de React motiva a los desarroladores a mejorar sus habilidades en JavaScript, haciendo mucho más intuitivo la transferencia de habilidades.


## Tradeoff 4: Plantilla separada vs. Archivo único {#tradeoff-4-plantilla-separada-vs-dot-archivo-único}

Los patrones como el MVC, popularizado por separar el modelo, la vista y el controlador en diferentes archivos, es tradicionalmente reconocido en aplicaciones web por consolidar el modelo de los datos en un archivo JS, la vista en un HTML y el controlador para la interacción con el modelo en otro archivo JS.

En contraste, con React cada componente tiene una preocupación autónoma. De este modo cada componente se sostiene por sí solo y se puede componer con otros componentes para crear interfaces de usuario complejas y ricas. Esto significa que el marcado HTML y la lógica son ubicados en el mismo archivo.

Cuando React fue presentado en el 2013, la audiencia fue muy escéptica ya que el diseño de React iba en contra de lo que se habia consolidado como buena práctica en el desarrollo web al manejar las plantillas HTML y la lógica JavaScript en archivos separados. En la superficie la propuesta de React parecía estar violando el principio de separación de preocupaciones. No obstante, un árticulo publicado por Ben Alman bajo el nombre _Repensando las mejores prácticas establecidad_, empezó a aclarar la idea de la separación de preocupaciones fomentada por React.

La siguiente imagen muestra la visión de React del nuevo enfoque de separación de responsabilidades.

{{< figure src="../../images/react/02-react-separation-of-concerns.png" caption="Figure 1: React History" >}}

En resumidas cuentas, en vez de tener tecnologías separadas con preocupaciones entrelazadas, se tiene que cada componente (un botón, un acordión o un texto de entrada), es una preocupación por si sola. Con el tiempo la comunidad fue respaldando este enfoque, ya que por experiencia la separación de preocupaciones basada en tecnologías puede obstaculizar la depuración y relentiza la retroalimentación del desarrollo por el simple hecho de lograr que los archivos separados estén sincronizados. Hoy en día varios desarrolladores sustentan que la separación por componentes vale la pena.

Una analogía válida es pensar los componentes anidados de React como unas matriuscas. El concepto es acertado porque cada matriusca pueden contener a otra matriusca más pequeña en su interior. El modelo de componentes de React funciona de la misma forma. Componentes simples y reusables se pueden componer juntos para construir interfaces de usuario complejas. Una página puede ser considerada como una anidación de múltiples componentes React.


## Tradeoff 5: Estándar vs. No estándar {#tradeoff-5-estándar-vs-dot-no-estándar}

React es una de las muchas tecnologías no estandarizadas para crear componentes web. El estándar para componentes web ha existido durante años sin mucho uso todavía y por ende es valido preguntarse ¿por qué los desarrolladores no están construyendo aplicaciones con el estándar de los componentes web?. Antes de abordar esta pregunta, se va a revisar el estándar para componentes web. La siguiente imagen consolida el estándar en sus cuatro tecnologías principales:

{{< figure src="../../images/react/03-react-standard-web-components.png" caption="Figure 2: Web Component Standard" >}}

Ahora bien, ¿por qué los desarrolladores no fomentan el uso del estándar para los componentes web?. Básicamente es por los siguientes cuatro argumentos:

1.  El soporte ofrecido por los navegadores hacia el estándar es irregular, y por ende hay que recurrir a los polyfills para tener garantías sobre un entregable estable
2.  No hay nada nuevo en la propuesta del estándar para componentes web que librerías como React o Frameworks como Angular no aborden
3.  Las tecnologías en JavaScript están en continua innovación
4.  El estándar de componentes web, solo se limita a correr en el navegador. En proyectos como React, ya hay otro tipo de plataformas como los dispositivos móviles o realidad virtual.

En consecuencia, no se obtiene ningún beneficio por usar el estándar. En la siguiente tabla se consolida un comparativo entre el equivalente del estándar web con la librería React:

| Componentes Web          | React                                     |
|--------------------------|-------------------------------------------|
| Plantillas               | JSX, JS                                   |
| Elementos personalizados | Declaración de componentes React          |
| DOM de sombra            | Módulos CSS, CSS en JS ó estilos en línea |
| Importaciones            | Un componente por archivo                 |

Cómo se puede observar, el estándar web de componentes es muy limitado, y los desarrolladores han decidido inclinarse por las tecnologías que no son parte por el estándar gracias a su frecuente innovación y al soporte que tienen por la mayoría de los navegadores.


## Tradeoff 6: Apoyo de comunidad vs. Apoyo corporativo {#tradeoff-6-apoyo-de-comunidad-vs-dot-apoyo-corporativo}

La última compensación a considerar es el apoyo de comunidad contra el apoyo corporativo. Muchos proyectos de código abierto hechos con JavaScript son impulsados por comunidades. React es un proyecto de código abierto pero esta activamente mantenido por Facebook. Esto significa que React está orientado hacia las necesidades de Facebook. Por lo tanto, si sus aplicaciones son muy diferentes a lo que Facebook este construyendo, entonces descartar React sería viable. No obstante, tal y como se aludio anteriormente, el soporte corporativo puede tener los siguientes beneficios:

El primero, es que Facebook tiene un equipo de desarrollo de tiempo completo dedicado al proyecto que está encargado de planificar las liberaciones, actualizar la documentación y proveer soporte sobre la marcha en plataformas como Github. Es segundo es que actualmente hay mas de mil contribuidores en el repositorio de Github, dejando en evidencia una comunidad activa y comprometida. El tercero, es que para nadie es un secreto que Facebook es una de las empresas más valoradas globalmente e incluso si Facebook dejará de dar soporte a React, el proyecto puede mantenerse sin la dedicación de un equipo de tiempo completo por un buen tiempo. Por último, Facebook a liberado más de 50,000 componentes en producción obligando a que esten proporcinando un soporte signigicativo al proyecto.


## Resumen {#resumen}

En la siguente tabla, se recopilan las compensaciones desarrolladas en este árticulo:

| Otros                 | React                           |
|-----------------------|---------------------------------|
| Framework             | Librería                        |
| Consiso               | Explícito                       |
| Centrado a plantillas | Centrado a JS                   |
| Plantillas separadas  | Componentes en un archivo único |
| Estándar              | No Estándar                     |
| Soporte de comunidad  | Soporte corporativos            |

Es importante aclarar que con compensaciones no hay una respuesta correcta. Pero al menos ahora se tiene un entendimiento sobré lo que se va a obtener y lo que se va a enfrentar al momento de utilizar React.
