+++
title = "¿Por qué React?"
author = ["Sergio Benítez"]
description = "Serie que recopila una descripción general de React"
date = 2021-02-18T00:00:00-05:00
lastmod = 2021-10-06T20:21:48-05:00
draft = false
tags = [
  "react"
]
+++

Esta serie estará dividida por tres publicaciones:

1.  Por qué usar React?
2.  Compesasiones de usar React (Trade offs)
3.  Por qué *no* usar React?

El objetivo de esta serie es evaluar el panorama que ofrece React como herramienta para el desarrollo de aplicaciones web. En este articulo se cubren los siguientes temas:

1.  [Historia](#orga257130)
2.  [¿Por qué React?](#org1a0d41e)
3.  [Razón 1: Flexibilidad](#org2def2dd)
4.  [Razón 2: Experiencia de desarrollo](#org3fb497e)
5.  [Razón 3: Inversión corporativa](#org13bd303)
6.  [Razón 4: Comunidad](#org44c8ba8)
7.  [Razón 5: Rendimiento](#org7211e7c)
8.  [Razón 6: Óptimo para pruebas](#orgc3a683a)
9.  [Resumen](#orgc3a57e2)

# Historia {#orga257130}

A continuación se muestra una línea de tiempo con la historia de React:

![img](../../images/react/01-react-big-pic-history.png "React History")
  
React ya lleva casi una década en el mercado y quizas el hecho más importante en su historia fue su liberación a código abierto, ya que este evento marco la pauta para un crecimiento en la comunidad que se vío reflejado en la implementación de los nuevos complementos como React Native y los nuevos features como React Hooks. Esta misma comunidad liderada por el equipo dedicado de Facebook esta en constante actividad para actualizar documentación y desarrollar mejoras sobre la librería.

# ¿Por qué React? {#org1a0d41e}

Una de las principales dudas a resolver en esta serie es identificar el por qué usar React ante tantas alternativas que hay actualmente para el desarrollo de aplicaciones web. Le respuesta en detalle será desarrollada a lo largo de estás publicaciones. No obstante, se pueden anticipar seis razones importantes que soportan la elección de React:

1.  Flexibilidad
2.  Experiencia de desarrolladores
3.  Inversión y compromiso corporativo
4.  Soporte de la comunidad
5.  Rendimiento
6.  Fácil de probar

# Razón 1: Flexibilidad {#org2def2dd}

Quizas la razón más convincente para seleccionar React es que una vez se aprende, se esta en capacidad de construir interfaces de usuario para una gran variedad de plataformas y casos de uso. React es notablemente flexible, ya que incorpora menos opciones que sus competidores. React es una librería, no un framework. Al ser una librería resulta ser una herramienta más flexible, ya que se adapta a diferentes contextos.

Cuando React fue creado, solo había un único caso de uso a solucionar; la creación de componentes para aplicaciones web. Sin embargo, a medida de que su popularidad fue creciendo, su ecosistema se fue ampliando para abordar otros casos de usos como los que se enlistan a continuación:

-   Sitios estáticos a tráves de Gatsby
-   Desarrollo de aplicaciones móviles a través de React Native
-   Desarrollo de aplicaciones de escritorio con ayuda de Electron
-   Proyectos server-rendered a través del framework NextJS
-   Desarrollo de aplicaciones en realidad virtual a través de React VR

En resumen, la gran ventaja de react es:

> Aprende React una vez, y escríbelo en todas parte.

React es muy vérsatil ya que el motor de renderizado esta separado de React. Para hacer aplicaciones web, es necesario el uso de \`react-dom\` para renderizar los componentes HMTL. Para React Native, se usa el motor \`react-native\` para renderizar los componentes móviles. El mismo enfoque se tiene con React VR.

Actualmente, hay mas de una docena de motores de renderizado para React: generación de PDFs, implementaciones sobre canvas y componentes en WebGL.

En cuanto a server-side rendering, el motor `react-dom` provee una función llamada `renderToString` que renderiza el componente a una cadena de carácteres de HTML. Este hecho resulta útil cuando se pretende renderizar los componentes de React en el servidor, lo cual significa que se puede usar React para reemplazar una tecnología tradicional para la renderización en servidores. Librerías como NextJS nos facilitan este tipo de tareas.

Ahora bien, ya que React es tan ligero y flexible, uno de sus principales usos es integrarlo en aplicaciones existentes. De hecho, esta es la razón principal para la cuál React fue diseñado. Facebook usó React para reemplazar lentamente su aplicación renderizada en el servidor implementada en PHP. Se puede empezar por porciones pequeñas de una página web, como la calificación en estrellas de un curso web y posteriormente extenderse a piezas más grandes como un bloque que aglomera toda la información de un curso web; título, duración, nivel, fecha de publicación y calificación. Este enfoque se puede aplicar hasta lograr finalmente la reconstrucción de toda la página, logrando así una migración de bajo riesgo.

Por útlimo, React es soportado por la mayoría de los navegadores modernos. En consecuencia, se puede confiar en que su uso no implicaría configuraciones adicionales para poder ser utilizado en un cliente específico.

# Razón 2: Experiencia de desarrollo {#org3fb497e}

Uno de los efectos comunes con React, es que una vez se utiliza se querrá aplicar en todas partes. La retroalimentación rápida de la experiencia de desarrollo, combinado con el intuitivo API de la librería, establece una meta díficil de derrocar.

El siguiente snippet es un ejemplo que ilustra las bases para comprender React:

```jsx
    import React from 'react';
    
    function HelloWorld(props) {
      return <div> Hello {props.name}</div>;
    }
```

Básicamente es una función que retorna un código que parece HTML. Para usar componentes en React, el primer paso es importar la librería. El segundo paso es declarar el componente usando una función estándar de JavaScript. Esta función recibe variables a través de un objeto llamdo `props`. Estos son todos los detalles a contemplar.

Por otra parte, es posible declarar componentes en React usando la clases estándares de JavaScript, como se muestra a continuación:

```jsx
    import React from 'react';
    
    class HelloWorld extends React.Component {
      render () {
        return <div> Hello {props.name}</div>;
      }
    }
```

La parte llamativa de ambos snippets, es el código que parece HTML, ya que no es normal encontrar una sintaxis HTML en archivos JavaScript, porque sencillamente no funcionaria. Este tipo de sintaxis es llamada JSX, y es un código que se compila a JavaScript de la siguiente manera:

```jsx
    <h1 color="red"> Heading here </h1>
    // is equal to
    React.createElement("h1", {color: "red"}, "Heading here");
```

El argumento esta en la función `createElement` de React que recibe los siguientes tres argumentos:

1.  El nombre de la etiqueta
2.  Un objeto que especifica los atributos a configurar
3.  El contenido que va dentro de la etiqueta

En últimas, JSX se compila a código JavaScript. Se esta en libertad de escribir solo JavaScript, pero la mayoria de los desarrolladores React terminan familiarizandose con JSX porque resulta más fácil de leer y anidar.

Un dato llamativo es el enfoque que utiliza React llamado *HTML in JS*. Frameworks como Angular y Vue mejorar el poder de HTML inventando su propia sintaxis para operaciones simples como los bucles. Es común encontrar este tipo de sintaxis en estos frameworks:

```jsx
    <div *ngFor="let user of users">// Angular
    <div v-for="let user of users">// Vue
    {{#each user in users}} // Ember
```

Este enfoque es conocido como *JS in HTML*. React invierte los roles y para lograr operaciones de bucles nos ofrece algo como:

```jsx
    {users.map(createUser)} // Ember
```

Nuevamente, solo es necesario seguir usando JavaScript. Es por esta razón que la comunidad de React resalta con frecuencia que el uso de esta librería hace que los desarrolladors mejores sus habilidades en JavaScript.

# Razón 3: Inversión corporativa {#org13bd303}

Muchas empresas de renonmbre han realizado inversiones profundas en React y su ecosistema. React fue creado por Facebook, y por ende es una librería muy utilizada dentro de la empresa. Facebook esta muy comprometida con el mantenimiento y la innovación de React, y aunque es un proyecto de código abierto, cuatro de los seis sub proyectos con más compromisos de React son apoyados por empleados de tiempo completo de Facebook.

Las actualizaciones de Facebook publicadas en React son administradas a través de un *codemod*. Un codemod es una herramienta de líneas de comando a la cual se puede apuntar desde su código base para automatizar los cambios introducidos a la librería. A través de esta línea de comandos se pueden actualizar automaticamente los componentes React viejos a la última especificación. A lo largo de los años, cuando se introducen cambios a la versión de React, el equipo de Facebook publica consistentemente el codemod respectivo con el fin de actualizar el proyecto a la versión más reciente. En consecuencia, la compatibilidad entre las versiones React es el reflejo del soporte y la responsabilidad qu tienen las empresas con la librería.

El codemod existe porque Facebook lo necesita. Hoy en día existen más de 50.000 componentes React en producción. De cierto modo, esto es un benefico por usar React, ya que hay garantías de retrocompatibilidad entre versiones por que existe un equipo dedicado en mantener esta carácteristica. El resultado de está dinámica es una librería estable para el desarrollo de las aplicaciones web.

# Razón 4: Comunidad {#org44c8ba8}

La comunidad de React es enorme y es una de las maś activas. Desde el 2013 la popularidad de React ha crecido tanto que hoy en día tiene más de 160k estrellas en su repositorio de GitHub, clasificandolo en el podio de los repositorios más populares en GitHub.

Los números de React son muy llamativos:

-   [React](https://github.com/facebook/react) tiene más de 160,000 estrellas en su repositorio de GitHub, con más de 1,300 contribuidores
-   En [npm](https:www.npmjs/package/react) registra millones de descargas por semana
-   En [Stackshare](https://stackshare.io/tools/top) se registra que más de 8000 compañias usan React y lo califica como la quinta herramienta más popular en open source.
-   [Reactiflux](https://www.reactiflux.com/) es una comunidad de chat con mas de 110,000 miembros dispuestos a dar soporte

Estos datos son relevantes porque incrementa la probabilidad de que la respuesta a su caso puntual exista en internet. Es decir, un problema que este enfrentando, posiblemente ya haya sido resuelto por otro desarrollador.

Otro hecho importante es que gracias a la flexibilidad de React varias compañias ha creado sus propias librerias de componentes React. Por nombrar algunos casos, se tiene UI Fabric de Microsoft para lograr aplicaciones visualmente parecidas a Office; Material UI para implementar las guías del Material Design de Google; En fin, la lista es larga y puede consultare en el repositoria GitHub [awesome-react](https:github.com/enagx/awesome-react).

La inversión profunda de la comunidad ha logrado una variedad y madurez de proyectos basados en React como los que se enlistan a continuación:

-   React Router, para hacer enrutamientos
-   Redux o Mobx, para manejos complejos de datos
-   Jest, para pruebas automatizadas
-   GraphQL, como alternativa a los llamados RESTful API desde el cliente
-   NextJS, para configuraciones de renderizaciones en el lado del servidor

Es importante resaltar que la lista sigue creciendo, y todo esto es gracias a una gran comunidad que día a día sigue manifestando su compromiso con el proyecto.

# Razón 5: Rendimiento {#org7211e7c}

Cuando React fue liberado, dió un golpe fuerte sobre la competencia en cuanto a rendimiento. El equipo de React reconoció que JavaScript es rápido y que el DOM hacía que pareciera lento. Identificaron que modificar el DOM es costoso ya que para reflejar nuevos estados, el redibujo de la página se hacia sobre una proción significante de la misma, aun asi cuando el cambio era menor. Por lo tanto, el equipo encontró una forma para actualizar el DOM de manera más eficiente que ayuda a mejorar el rendimiento de una aplicación web. Esta solución es conocida como el *DOM virtual*.

Detraś de escenas cuando se cambian datos React inteligentemente descubre la forma más eficiente de actualizar el DOM, ya que realiza un monitoreo sobre el estado de cada componente. Cuando el estado de un componente cambia, React compara el estado del DOM existente con el estado de como debería verse el DOM con el cambio. Así determina una actualización menos costosa porque dicha comparación le ayuda a identificar con facilidad cuales son las actualizaciones puntuales que se deben renderizar.

Este enfoque tiene varios beneficios. El primero es que ayuda a evitar basura de diseño cuando el navegador recacula la posición de todo el contenido de la página al momento de que un elemento del DOM cambia. El segundo es que ayuda a ahorrar consumo de bateria y procesamiento. Y por último habilita un modelo simple de programación, puesto que cuando los datos cambian, React actualiza eficientemente el DOM de manera automática.

Actualmente, muchas liberias usan este mismo enfoque pero React sigue manteniendo tiempos competitivos y la comunidad considera que no son comunes las optimizaciones de rendimiento.

Por último, el peso de `react` y `react-dom` equivale a ~35k. El tamaño de la librería es importante para el rendimiento, y en caso de que se considere este tamaño algo elevado, hay alternativas como Inferno o Preact en donde la librería es más ligera a cambio del descarte de unas funcionalidades que no se consideran escenciales para el desarrollo.

# Razón 6: Óptimo para pruebas {#orgc3a683a}

Típicamente hacer pruebas en el frontend es difícil y es por esta razón que pocos equipos hacen un frontend comprensivo y fácil de probar. Aquí React resulta atractivo gracias a que su diseño es amigable con las pruebas automatizadas. 

En las pruebas tradicionales de interfaces de usuario siempre hay molestías con la configuración de los ambientes de pruebas y hay que ser cuidadoso con la integración de los proyectos de código abierto que permitan hacer este tipo de comprobaciones. Con React, este paso es resuelto con el uso del paquete `create-react-app` ya que los ambientes de pruebas son configurados por defecto.

Otra punto en las pruebas tradicionales de UI es la necesidad de un navegador. En cambio, con React se pueden ejecutar las pruebas de los componentes web en memoria a través de NodeJS. Esta característica también otorga un beneficio en rendimiento, ya que hacer pruebas UI sobre un navegador es un proceso lento, mientras que correrlas en memoria con ayuda de una línea de comandos es lo suficientemente rápido para ejecutar un conjunto grande de pruebas cada vez que se hagan modificaciones.

Adicionalmente las pruebas de integración tradicionales de UI suelen ser muy frágiles, en el sentido de que cualquier cambio puede desencadenar varios fallos. Con React se pueden escribir pruebas unitarias deterministicas de confianza ya que el componente web se prueba de manera aislada.

Por último, hacer pruebas UI tradicionales requiere tiempo y mantenimiento ya que se tiene que hacer una interacción cuidadosa con el DOM para probar el UI. En contraste, las pruebas en React se escriben rápidamente usando herramientas populares como Jest y Enzyme. Dichas herramientas también facilitan la actualización de las pruebas en la mayoria de los casos, con una sola pulsación de una tecla pra confirmar que la salida cambio tal y como se esperaba.

Con React, la mayoria de los componentes pueden ser funciones puras, lo cual significa que dada una entrada, se produce la misma salida y por lo tanto no hay efectos secundarios. Probar este tipo de componentes resulta algo trivial. Por ejemplo, en la siguiente función si se pasa el parametro world como propos siempre se va a obtener la etiqueta `<div> Hello world </div>` como salida, haciendo que la función sea confiable, deterministica y sin efectos secundarios:

```jsx
    function HelloWorld(props) {
      return (
        <div>
            Hello {props.message}
        </div>
      )
    }
```

Actualmente hay muchas alternativas para automatizar pruebas en JavaScript. Dada la premisa de que React es puro JavaScript, cualquier framework para probar JavaScript sirve para probar React. No obstante, la más popular es Jest. Otra buena herramienta es React Testing Library, ya que permite ejecutar las pruebas sin necesidad de un navegador y se apoya sobre NodeJS para hacer correr las mismas en memoria.

# Resumen {#orgc3a57e2}

En esta publicaciones se compartieron las principales razones por la cuales React es popular:

-   Flexibilidad
-   Experiencia de desarrollo
-   Soporte de comunidad
-   Rendimiento
-   Óptimo para pruebas

En la siguiente publicación se va a revisar las compensaciones a las que se expone el uso de React

