+++
title = "React Hooks"
author = ["Sergio Benítez"]
description = "Serie que pretende explicar la funcionalidad de React Hooks"
date = 2021-05-02T00:00:00-05:00
lastmod = 2021-10-02T22:54:25-05:00
draft = false
tags = [
  "react",
  "react-hooks",
]
+++

Los ganchos de React (React Hooks en inglés) representan un enorme paso hacia adelante hacia la construcción de aplicaciones plenamente funcionales con un uso mínimo de componentes de clase.

El manejo de estado y los eventos de ciclo de vida de un componente React requieren menos código y ganan legibilidad con el uso de los Hooks. Los Hooks respetan la forma en la que JavaScript fue diseñado para ser programado, y por ende se pueden implementar Hooks personalizados.

Los temas a revisar en esta serie son:

-   React Hooks básicos incorporados
-   React Hooks avanzados
-   Comparación entre Hooks y componentes de clase

El propósito de estas publicaciones es lograr un entendimiento pleno de como usar los React Hooks para crear aplicaciones con solo componentes funcionales.

Para tener garantías del objetivo, es pre requisito tener conocimientos previos y sentirse cómo con las construcción de aplicaciones web con React.

## ¿Qué son los React Hooks?

React Hooks agrega la habilidad de administrar el estado de React y la interfaz de usuario con los eventos del ciclo de vida de React en componentes funcionales, apoyándose en un estilo declarativo.

Quizas, el mayor aporte de React Hooks es la creación de componentes en una forma sencilla y de menor complejidad.

Básicamente los React Hooks son una vía para adjuntar lógica reutilizable en un componente existente. Antes de Hooks, la forma común de adjuntar lógica externa a un componente era usando las propiedades en la función `render` ó a través del patrón de componentes de orden superior. Lo íncomodo de este enfoque es que con frecuencia era necesario restructurar el código para atender una dependencia externa, haciendo engorroso y díficil de seguir el desarrollo.

La propuesta de React Hooks es una solución a este problema creando un mecanismo para extraer código que se pueda reutilizar entre componentes sin necesidad the introducir anidación en el árbol de componentes.

Siendo espécificos, los React Hooks son funciones JavaScript que le permiten al desarrollador usar el estado y los métodos del ciclo de vida dentro de los componentes React funcionales. En consecuencia, ahora se pueden construir aplicaciones React sin usar las clases de JavaScript. Ya no habrán más preocupaciones por lidiar con la palabra clave `this`.

La siguiente tabla recopila el antes y el después del manejo de lógica retulizable en React:

| Antes de hooks                | Después de hooks                                                                 |
|-------------------------------|----------------------------------------------------------------------------------|
| Propiedades del render        | Adjunta lógica reutilizable en un componente existente                           |
| Componentes de orden superior | Usa el estado y los métodos de ciclos de vida dentro de un comoponente funcional |
| Código más complego           | Construye le 100% de la aplicación con componentes funcionales                   |


Los tres React Hooks más populares son:

-   useState
-   useRef
-   useEffect

Tiempo de dar un vistazo mas profundo a cada uno de ellos:

## `useState`

En los últimos años del mundo de la programación front-end, no importa el framework o la librería que se este utilizando, la siguiente declaración a sido una tendencia:

> El estado ayuda a construir aplicaciones web de alto rendimiento

La idea de estado consiste en que cada vez que se ve algo nuevo en la pantalla, sea rastreado como un estado nuevo. Para aterrizar este concepto se presenta el siguiente ejemplo:

![img](../../images/react/react-hooks/01-two-way-data-binding.png "Two way data-binding")

Esta  imagen ilustra los elementos relevantes para hacer un manejo de estado con la digitación de un correo electrónicio dentro de una etiqueta `<Input/>` que representa una caja de texto. Este elemento de entrada interactua con un modelo de datos en donde se va a almacenar lo que el usuairo esta escribiendo en el navegador. Cada caracter digitado actualizará el almacenamiento y posterior a dicha actualización en el modelo de datos la salida de este evento estará registrada en una etiqueta `<Label>`. El almacenamiento de datos es conocido como *estado*.

A continuación se comparte una primera implementación del escenario descrito anteriormente. El plan es enganchar un campo de entrada de texto y cada vez que el valor cambie, como cuando un usuario escribe un carácter, se salvará el nuevo valor del campo de entrada como estado de React para posteriormente imprimir la salida de ese valor como una etiqueta debajo del campo de texto:

```jsx
    import React, { useState } from "react";
    
    const InputElement = () => {
      const results = useState("");
      const inputText = results[0];
      const setInputText = results[1];
    
      return <input
        onChange={(event) => {}}
        placeholder="Enter some text"
      />
    };
    
    export default InputElement;
```

Al consultar este código en el navegador, se observa un campo de entrada de texto con su respectivo placeholder. Se recuerda que los campos de entrada de HTML soportan el evento `onChange` al cual se le puede asignar una función que será llamada cada vez que un usuario oprima una tecla. A dicha función se le pasa un evento como argumento y por ahora no va a retornar nada.

Tiempo de usar el primer hook. Se recuerda que los React Hooks son componibles lo que significa que se crean dentro del componente funcional tal y como se muestran en las primeras líneas del componente en el código anterior.

`useState` es un React Hook que esta incluido dentro del paquete de la libreria React y es el que se usa para administrar estado en un componente funcional. Es por esta razón que se importa en la primera línea del snippet y al ser llamada se le pasa un valor inicial del estado sobre el cual se hará seguimiento. `useState` retornará un arreglo de dos valores con las versiones previas y actuales del estado. El primer valor, llamdo `inputText` es una referencia al estado mismo, que para este caso es el modelo de datos. El segundo valor del arreglo, con el nombre `setInputText` es una función que se llama para actualizar el modelo de datos.

Se puede usar destructuración sobre el `useState` para hacer más explícito el código, tal y como se muestra a continuación:

```jsx
    import React, { useState } from "react";
    
    const InputElement = () => {
      const [ inputText, setInputText ] = useState("");
    
      return <input
        placeholder="Enter some text"
        onChange{(event) => {}}
      />
    };
    
    export default InputElement;
```

Esta es la forma típica de hacer uso del hook `useState`. Retomando la funcionalidad del componente, por ahora se tiene que el campo de texto de entrada esta asociado al evento `onChange` para capturar el texto escrito por el usuario. La función `setInputText` al ser llamada actualiza el estado y dicho cambio se ve reflejado en la variable de lectura definida como `inputText`. Es siguiente paso es muy predecible, ya que consiste en asociar estos tres elementos, llamand la función `setInputText` dentro del evento `onChange` que se le esta pasando al campo de texto, como señala el siguiente snippet:

```jsx
    import React, { useState } from "react";
    
    const InputElement = () => {
      const [ inputText, setInputText ] = useState("");
    
      return <input
        placeholder="Enter some text"
        onChange{(event) => {setInputText(event.target.value)}}
      />
    };
    
    export default InputElement;
```

Para complementar la propuesta del diagrama con un enlace de datos bidireccional se debe imprimir el valor de la variable `inputText` actualizado, como se ilustra a continuación:

```jsx
    import React, { useState } from "react";
    
    const InputElement = () => {
      const [ inputText, setInputText ] = useState("");
    
      return <div>
        <input
          placeholder="Enter some text"
          onChange{(event) => {setInputText(event.target.value)}}
        />
        <br/>
        {inputText}
      </div>
    };
    
    export default InputElement;
```

Ahora bien, para un mejor entendimiento de como fuciona el estado en React, se va a agregar un detalle en la implementación actual para imprimir el historial de la variable `inputText`.

Para lograr este objetivo se sigue un esquema parecido al del evento `onChange` en el valor `inputText`. Primero, se crea un nuevo estado que se inicializa con un arreglo vacío. Posteriormente, cada vez que se ejecute el evento `onChange` se va registrar dicha actualización en la variable `historyList`. Para ello, se utiliza en <span class="underline">spread operator</span> y se agrega al arreglo `historyList` el valor introducido por el usuario en el campo de texto. Por último se renderiza el valor del `historyList` en una lista con ayuda de la función `.map`. El siguiente código conoslida la descripción previa:

```jsx
    import React, { useState } from "react";
    
    const InputElement = () => {
      const [ inputText, setInputText ] = useState("");
      const [ historyList, setHistoryList ] = useState([]);
    
      return <div>
        <input
          placeholder="Enter some text"
          onChange{(event) => {
            setInputText(event.target.value)
            setHistoryList([...historyList, event.target.value])
          }}
        />
        <br/>
        {inputText}
        </hr><br/>
        {historyList.map(record => {
          return <li>record</li>
        })}
      </div>
    };
    
    export default InputElement;
```

Es importante aclarar una práctica sugerida con el uso del Hook `useState`. Una alternativa a esta aproximación es el uso de un solo `useState` que se encargara de administrar el estado del `inputText` y el `historyList` a través de un objeto JavaScript. No obstante este enfoque complica la legibilidad del código y no ofrece ninguna ganancia. La recomendación del equipo React es usar multiple llamados a `useState`

Con este ejemplo se obtiene un entendimiento sólido sobre como usar React Hooks en general, ya que todos los Hooks funcionan bajo las siguientes premisas:

-   Solo están disponibles en componentes funcionales.
-   Por convención usan el prefijo `use`.
-   Todos contribuyen a los eventos de ciclo de vida y la administración del estado en un componente React.

## `useRef`

Una definición para el `useRef` hook es:

> Mecanismo que se utiliza principalmente para permitir el acceso directo a un elemento en el DOM

Es común usar los hooks `useState` y `useRef` para hacer cambios sobre lo que el usario esta viendo en la aplicación React. En casos muy esquivos bajo el contexto de React, es necesario implementar un acceso directo sobre un elemento HTML, aunque la recomendación sea evitarlo.

Para comprender el funcionamiento del `useRef` un escenario apropiado es un programa que cambie una imagen a blanco y negro por una imagen a color cuando se pase el cursor por encima de la imagen, y por supuesto la imagen vuelva a blanco y negro cuando el cursor ya no este por encima de la imagen. Este escenario sugiere un acceso directo sobre un elemento HTML: la imagen.

Para empezar, se va a crear el archivo `/pages/ImageChangeOnMouseOver`, bajo el contexto de una aplicación estructurada con NextJS con el siguiente contenido:

```jsx
    import React from "react";
    
    const ImageChangeOnMouseOver = () => {
      return (
        <div>
          <img src="/static/speakers/bw/speaker01.jsp" alt="" />
          &nbsp;
          &nbsp;
          <img src="/static/speakers/bw/speaker02.jsp" alt="" />
        </div>
      );
    };
    
    export default ImageChangeOnMouseOver;
```

Esta primera versión renderizará dos imágenes en el navegador a blanco y negro. Por ahora este contenido es estático y no hay acciones asociadas al evento de pasar el cursor sobre la imagen. Se resalta que las imágenes son contenidos estáticos y es una convención almacenarlas en la ruta `public/static` como se indica en el código anterior.

Para habilitar el efecto del hover sobre una imagen, se va a crear un componente con el nombre `ImageToggleOnMouseOver` al cual se delegará el uso de `useRef` y utilizará dos propiedades: `primaryImg` y `secondaryImg`. Cada imagen atenderá el estado por defecto y el estado del hover respectivamente. En ese orden de ideas, esta sería la actualización sobre el archivo `pages/ImageChangeOnMouseOver.js`

```jsx
    import React from "react";
    import ImageToggleOnMouseOver from "../src/ImageToggleOnMouseOver";
    
    const ImageChangeOnMouseOver = () => {
      return (
        <div>
          <ImageToggleOnMouseOver
            primaryImg="/static/speakers/bw/speaker-01.jpg"
            secondaryImg="/static/speakers/color/speaker-01.jpg"
            alt=""
          />
          &nbsp;
          &nbsp;
          <ImageToggleOnMouseOver
            primaryImg="/static/speakers/bw/speaker-02.jpg"
            secondaryImg="/static/speakers/color/speaker-02.jpg"
            alt=""
          />
        </div>
      );
    };
    
    export default ImageChangeOnMouseOver;
```

Bajo la estructura de carpetas de NextJS, la convención es incluir el nuevo compnente en la siguiente ruta: `src/ImageToggleOnMouseOver.js`. La idea es inicial es hacer este componente funcional lo más simple posible y por ahora el primer objetivo es renderizar la imagen a blanco y negro. La primera versión del `ImageToggleOnMouseOver` tendría la siguiente contenido:

```jsx
    import React, { useRef } from "react";
    
    const ImageToggleOnMouseOver = ({ primaryImg, secondaryImg }) => {
      return (
        <img
          src={primaryImg}
          alt=""
        />
      );
    };
    
    export default ImageToggleOnMouseOver;
```

Al revisar estos cambios en el navegador, el resultado visual es exactemente el mismo, dos imágenes en blanco y negro, con la diferencia de que ahora se esta utilizando un componente funcional de por medio.

Ahora es momento de manejar los eventos `onMouseOver` y `onMouseOut` sobre la imagen a través de los atributos de eventos. De esta manera, cuando determinado evento sea activado, se implementará la instrucción de cambiar el atributo `src` con la ruta de la imagen. Bajo este contexto el hook `useRef` entra en acción.

La forma en como `useRef` funciona es a través de la declaración de una constante, para este caso puntual `imageRef`, la cual será asignada al atributo `ref`. Con dicha asignación se habilita el acceso a la propiedad `.current` para obtener todos los atributos de la imagen, como por ejemplo el atributo `src` con la ruta de la imagen. Al tener acceso a este atributo, es posible sobreescribir el valor de la ruta de la imagen. El último paso es asociar la ruta con la imagen a color al evento `onMouseOver` y la ruta con la imagen a blanco y negro al evento `onMouseOut`. A continuación se ilustra esta descripción en código:

```jsx
    import React, { useRef } from "react";
    
    const ImageToggleOnMouseOver = ({ primaryImg, secondaryImg }) => {
      const imageRef = useRef(null);
    
      return (
        <img
          onMouseOver={() => {
            imageRef.current.src = secondaryImg;
          }}
          onMouseOut={() => {
            imageRef.current.src = primaryImg;
          }}
          src={primaryImg}
          alt=""
          ref={imageRef}
        />
      );
    };
    
    export default ImageToggleOnMouseOver;
```

Al abrir el navegador, se observa que las imágenes cambian de blanco y negro a color cuando el cursor del ratón se posiciona sobre alguna de ellas.

Esto es el hook `useRef` en pocas palabras. Un mecanismo para acceder a las propiedades de un elemento DOM y adicionar tareas especificas sobre dicho elemento.

Un característica llamativa de los hooks es que se pueden combinar, tal y como se explicará a continuación en un ejemplo que plantea un caso de uso para usar `useRef` en conjunto con `useEffect`. El ejemplo también servirá para dar explorar el hook `useEffect`.

## `useEffect`

El uso del hook `useEffect` en componentes funcionales es similar a las funciones de ciclos de vida `componentDidMount`, `componentDidUpdate` y `componentWillUnmount` en un componente de clase React.

Luego de que un componente es renderizado y el DOM esta listo para actualizaciones, bajo el contexto de un componente de clase la función `componentDidMount` es llamada. Para un componente funcional, la función que se pasa como primer argumento del `useEffect` es ejecutada.

Si el componente se actualiza, como por ejemplo un cambio en su estado, dentro del contexto de componentes de clase la función `componentDidUpdate` es llamada mientras que para un componente funcional la misma función `useEffect` es ejecutada con algunas condiciones espécificas que se revisarán más adelante.

Justo antes de que el componente se desmonte, en un componente de clase la función `componentWillUnmount` es llamada y para el caso de un componente funcional la función que se retorne en el primer argumento del `useEffect` será llamada.

Hay unas diferencias sútiles entre los métodos de un componente de clase y como funciona el `useEffect`. Ambos son llamados después de que el componente sea renderizado, pero puede ser un poco complicado cuando se hacen referenecias a elementos del DOM de manera directa y también cuando se define un estado de forma asíncrona.

> Incluso, hay otro hook popular llamado `useLayoutEffect` que resulta similar a las funciones `componentDidMount` y `componentDidUnmount` de un componente de clase. Su propósito es asegurar que el código escrito en un componente funcional funcione como se espera con componente funcionales.

La siguiente tabla es un comparativo entre estos dos tipos de componentes bajo el contexto de React:

| Componente de clase React      | Componente funcional React                 |
|--------------------------------|--------------------------------------------|
| `componentDidMount() {...}`    | `useEffect(() => {...})`                   |
| `componentDidUpdate() {...}`   | `useEffect(() => {...})`                   |
| `componentWillUnmount() {...}` | `useEffect(() => {... return () => {...})` |

Otra forma de ver `useEffect` es la de un mecanismo para agregar <span class="underline">efectos secundarios</span> a un componente funcional, hecho que antes de React Hooks no era posible.

La gente suele referirse a los efectos secundarios en componentes funcionales como algo contradictorio, ya que por lo general el propósito es que el componente funcional sea puro y libre de efectos secundarios. Es decir, que si el componente se llama con el mismo parametro una y otra vez, el mismo resultado se va a obtener sobre cada uno de estos llamados.

Por definición `useEffect` introduce efectos secundarios en el componente funcional, específicamente luego de que el componente ha sido renderizado, en los escenarios que se consolidaron en la última tabla.

En consecuencia, asociar los efectos secundarios como algo contradictorio es una premisa que se puede refutar. Por ejemplo, si se quieren agregar oyentes a los elementos DOM renderizados en un componente funcional, `useEffect` es la herramienta apropiada para abordar dicho escenario. Por su puesto cuando el componente desaparece, es responsabilidad del desarrollador remover los oyentes para evitar una fuga potencial de recursos en la aplicación web.

Se puede decir que esta descripción corresponde a un componente funcional puro, ya que así como se agregan oyentes, también se remueven de acuerdo al ciclo de vida del componente. No obstante, se resalta que una función pura garantiza el mismo resultado dado los parámentros de entrada, y el hecho de que se remuevan los oyentes no significa que siempre se vaya a obtener la misma salida.

Por último, se presenta la sintaxis de un componente funcional que utilizar `useEffect`:

```jsx
    import React, { useEffect } from "react";
    
    const Syntax = () => {
      const [ checkBoxValue, setCheckBoxValue ] = useState(false);
    
      useEffect(() => {
        console.log('in useEffect');
        return () => {
          console.log('in useEffect clean up');
        }
      }, [checkBoxValue]);
    
      return (<div></div>);
    };
    
    export default Syntax;

```

El primer parámetro debe ser una función. Para este ejemplo se tiene una función lambda que imprime el mensaje `in useEffect` en la consola y retorna otra función que como se explico anteriormente, será llamada cuando el componente se desmonte. Esta función imprimirá el mensaje `in useEffect clean up`.

El segundo parámetro de `useEffect` es un arreglo con las dependencias del componente funcional. Si se ignora, entonces la función del primer parámetro sera ejecutada cuando el componente se renderize por primera vez y posteriormente en cada actualización subsecuente del componente. Si el arreglo es vacío, entonces la función asociada en el primer argumento del `useEffect` solo será llamada en el primer renderizado del componente.

Si se quiere que el componente sea renderizado basado en ciertas condiciones, los valores para cumplir dichas restricciones deben se parte de este arreglo. Para el ejemplo anterior, la condición aplicada es sobre la variable `checkBoxValue` que representa un booleano y hace parte del estado del componente. Esto significa que si el valor de `checkBoxValue` cambia, la función `useEffect` será llamada.
 
Trayendo a coalición el ejemplo previo de las imágenes que cambian de blanco y negro a color cuando el cursor del ratón se posiciona sobre una de ellas, y viceversa cuando el cursor esta por fuera del rectángulo de la imagen, se va a realizar una modificación sobre el evento que da color a la imagen para revisar un caso que combina los hooks revisados hasta el momento: `useState`, `useRef` y `useEffect`.

Esta modificación consiste en que el evento para dar color sobre la imagen es la interacción de la barra de desplazamiento del navegador. Si la imagen esta dentro de la vista completa del navegador, se mostrará la imagen a color. De lo contrario la imagen quedara a blanco y negro. La interacción co la barra de desplazamiento determinará el cálculo para colorear las imágenes.

## Evento scroll

Se va a iniciar la implementación con base al contenido del archivo `ImageToggleOnMouseOver` y se van a realizar las siguiente modificaciones:

-   Renombrar el archivo por `ImageToggleOnScroll`
-   Definir un `useState` para validar si la image se encuentra en la vista del navegador
-   Definir un `useEffect` para atender la activación del evento `scroll`

A contnuación se muestra un snippet con las modificaciones aplicadas:

```jsx
    import React, { useState, useRef, useEffect } from "react";
    
    const ImageToggleOnScroll = ({ primaryImg, secondaryImg }) => {
      const imageRef = useRef(null);
      const [inView, setInView] = useState(false);
    
      useEffect(() => {
        window.addEventListener("scroll", scrollHandler)
    
        return () => {
          window.removeEventListener("scroll", scrollHandler)
        };
      }, []);
    
      const isInView = () => {
        const rect = imageRef.current.getBoundingClientRect();
    
        return rect.top >= 0 && rect.bottom <= window.innerHeight;
      };
    
      const scrollHandler = () => {
        setInView(isInView());
      };
    
      return (
        <img
          src={inView ? secondaryImg : primaryImg}
          alt=""
          ref={imageRef}
        />
      );
    };
    
    export default ImageChangeOnMouseOver;
```

El primer detalle a resaltar, es el uso de `useRef` para habilitar el acceso sobre la imange en el DOM.

Lo segundo es la lógica para identificar si una imagen esta dentro de la vista completa del navegador. El hook `useState` va a almacenar la condición y de manera complementaria, la función `isInView` es la encargada de hacer los calculos para determinar si la imagen se encuentra dentro de la vista del navegador. Para ello se utiliza la función `getBoundingClientRect` para recuperar las dimensiones del elemento en el DOM, y con ayuda del `window.innerHeight` se establecen los rangos para determinar si la condición es válda.

El tercer punto es la administración del evento `scroll` en el `useEffect`. la función `scrollHandler` actualizará el estado `inView` si se cumplen los calculos definidos en `isInView`. Dentro del `useEffect` se agrega la subscripción al evento scroll cuando se renderiza el componente por primera vez, y se remueve la subscripción cuando se desmonta el componente para evitar fugas en el uso de recursos.

Por último, en la maquetación del componente dentro del atributo `src` se usa una sintaxis de condición ternaria para definir que imagen renderizar dentro del componente. La imagen a color si se encuentra dentro de la vista completa del navegador o la image a blanco y negro si la imagen esta por fuera de la vista del navegador.

## Crear el archivo ImageChangeOnScroll

El siguiente paso es consumir el componente `ImageToggleOnScroll`. Para ello se va a crear un archivo `ImageChangeOnScroll` con el siguiente contedio.

```jsx
    import React from "react";
    import ImageToggleOnScroll from "../src/ImageToggleOnScroll";
    
    const ImageChangeOnScroll = () => {
      return (
        <div>
          {[01, 02, 03, 04].map((speakerId) => {
            return (
              <div key={speakerId}>
                <ImageToggleOnScroll
                  primaryImg="/static/speakers/bw/speaker-${speakerId}.jpg"
                  secondaryImg="/static/speakers/color/speaker-${speakerId}.jpg"
                  alt=""
                />
              </div>
            );
          })}
        </div>
      );
    };
    
    export default ImageChangeOnScroll;
```

En primera instancia, esta página debe importar el componente `ImageToggleOnScroll` para poder utilizarlo. Para renderizar varias imágenes se va a hacer un `.map` sobre un arreglo con los identificadores de las imágenes y con ayuda de los strings literales se va a consumir la ruta de la imagen de manera recursiva.

Por otra parte, se observa que en el uso del componente `<ImageToggleOnScroll>` se suministran las rutas de las imágenes dependiendo el caso con los atributos `primaryImg` y `secondaryImg`.

Actualmente tenemos un comportamiento por mejorar en el estado de la aplicativo. Al momento de renderizar las imágenes por primera vez en el navegador, todas las imágenes se muestran en su versión blanco y negro. El resultado esperado es que las imágenes que se encuentran dentro de la vista completa del navegador este en su versión a color. Los siguientes cambios ayudarán a cumplir con este objetivo:

```jsx
    import React, { useState, useRef, useEffect } from "react";
    
    const ImageToggleOnScroll = ({ primaryImg, secondaryImg }) => {
      const imageRef = useRef(null);
      const [inView, setInView] = useState(false);
    
      useEffect(() => {
        setInView(isInView());
        window.addEventListener("scroll", scrollHandler)
    
        return () => {
          window.removeEventListener("scroll", scrollHandler)
        };
      }, []);
    
      const isInView = () => {
        const rect = imageRef.current.getBoundingClientRect();
    
        return rect.top >= 0 && rect.bottom <= window.innerHeight;
      };
    
      const scrollHandler = () => {
        setInView(isInView());
      };
    
      return (
        <img
          src={inView ? secondaryImg : primaryImg}
          alt=""
          ref={imageRef}
        />
      );
    };
    
    export default ImageChangeOnMouseOver;
```

Se recuerda que estos cambios se realizan sobre el archivo `ImageToggleOnScroll`. En esta primera aproximación lo único que se agrego fue el llamado de la función `setInView()` dentro del `useEffect` una vez el componente se haya renderizado por primera vez. Al probar esta modificación en el navegador tenemos el resultados esperado, las imágenes que están en la vista completa del navegador están con su versión a color.

No obstante si se revisa de manera detallada hay un comportamiento sutilmente disruptivo cuando se recarga la página del navegador. Básicamente en menos de un segundo la imagen se renderiza en su versión a blanco y negro y en un instante se muestra la versión a color. Para evitar este comportamiento, en el siguiente snippet se ilustra una solución:

```jsx
    import React, { useState, useRef, useEffect } from "react";
    
    const ImageToggleOnScroll = ({ primaryImg, secondaryImg }) => {
      const imageRef = useRef(null);
      const [isLoading, setIsLoading] = useState(true);
      const [inView, setInView] = useState(false);
    
      useEffect(() => {
        setIsLoading(false);
        setInView(isInView());
        window.addEventListener("scroll", scrollHandler)
    
        return () => {
          window.removeEventListener("scroll", scrollHandler)
        };
      }, []);
    
      const isInView = () => {
        const rect = imageRef.current.getBoundingClientRect();
    
        return rect.top >= 0 && rect.bottom <= window.innerHeight;
      };
    
      const scrollHandler = () => {
        setInView(isInView());
      };
    
      return (
        <img
          src={
            isLoading ? "path_to_loader"
              : inView ? secondaryImg : primaryImg
          }
          alt=""
          ref={imageRef}
        />
      );
    };
    
    export default ImageChangeOnMouseOver;
```

Se puede observar la creación de un nuevo estado llamado `isLoading` para determinar en que parte del ciclo de vida del componente se encuentra la ejecución. Por defecto se inicializa en `true`. Luego, dentro de la función `useEffect` se hace el llamado de `seIsLoading(false)` para determinar cuando el componente ya fue renderizado.

Por último, en el JSX del componente dentro del atributo `src` se agrega la condición `isLoading` que de ser cierta, entonces muestra un loader, de lo contrario aplica la condición `inView` que determina que versión de la imagen se va a renderizar.

# Agregando un efecto secundario al componente Scroller 

Para el estado actual de la paǵina que da color a las imágenes en función a la barra de desplazamiento se usó el hook `useEffect` tomando ventaja del conocimiento previo de que la función que se pasa como primer parámetro de `useEffect` es ejecutada despues de que el componente haya sido completamente renderizado por primera vez. Al poner un arreglo vació `[]` como segundo parámetro de `useEffect` se establece que la instrucción de ejecutar la función que se pasa como primer parámetro se hace cuando el comoponente es renderizado y no sobre actualizaciones subsecuentes.

El detalle de pasar un arreglo vació como segundo parámetro del `useEffect` es utilizado para optimizar la ejecución del componente.

Para ahondar en el contexto del segundo parámetro del `useEffect` se expone el siguiente ejemplo: Dentro del componente `ImageChangeOnScroll`, que actualmente es el encargado de mostrar las imágenes de los expositores, se va a atender el evento del `MouseOver` sobre cada imagen del expositor para estrablecer el título de la ventana del navegador con el identificador de la imagen asociada a determinado expositor. El siguiente código atiende este nuevo efecto secundario:

```jsx
    import React, {useState, useEffect} from "react";
    import ImageToggleOnScroll from "../src/ImageToggleOnScroll";
    
    const ImageChangeOnScroll = () => {
      const [currentSpeakerId, setCurrentSpeakerId] = useState(0);
      const [mouseEventCounter, setMouseEventCounter] = useState(0);
    
      useEffect(() => {
        window.document.title = `SpeakerId: ${currentSpeakerId}`;
        console.log(`useEffect: setting title to ${currentSpeakerId}`);
      });
    
      return (
        <div>
          <span>mouseEventCounter: ${mouseEventCounter}</span>
          {[01, 02, 03, 04].map((speakerId) => {
            return (
              <div key={speakerId}
                onMouseOver={() => {
                  setCurrentSpeakerId(speakerId);
                  setMouseEventCounter(mouseEventCounter++);
                }
              }>
                <ImageToggleOnScroll
                  primaryImg="/static/speakers/bw/speaker-${speakerId}.jpg"
                  secondaryImg="/static/speakers/color/speaker-${speakerId}.jpg"
                  alt=""
                />
              </div>
            );
          })}
        </div>
      );
    };
    
    export default ImageChangeOnScroll;
```

Varias cosas por explicar en este snippet. El primer punto es la creación de un estado `[currentSpeakerId, setCurrentSpeakerId] = useState(0)` como respuesta al evento `onMouseOver` que permitirá definir el título de la ventana del navegador con el identificador de la imagen del expositor sobre el cual se ubico el cursor del ratón.

El segundo detalle esta dentro del retorno del componente. Aquí se agrego el evento `onMouseOver` sobre el padre `<div>` del cual se va a recuperar el identificador del expositor y actua como un empaquetador de la imagen. Se puede observar que es sobre este evento se ejecutan las funciones `setCurrentSpeakerId(speakerId)` y `setMouseEventCounter(mouseEventCounter++)`

## Evento del ratón

El estado `[mouseEventCounter, setMouseEventCounter] = useState(0)` es otro efecto secundario que se incluye para tener un registro de las veces que se activa el evento `onMouseOver`. Dicho conteo se esta registrando en la parte superior del navegador dentro de una etiqueta `<span>`.

Por último, el detalle más relevante es el uso de la función `useEffect` para actualizar el título de la ventana del navegador, aprovechando la ventaja de los momentos del ciclo de vida de un componente funcional que expone este hook. Aquí explicitamente se asigna el título del documento al valor del `currentSpeakerId` y con ayuda de un log se deja un registro del valor actual de dicha variable.

Adicionalmente, para este caso la función `useEffect` no recibe un segundo parámetro, indicando así que la función `useEffect` se va a llamar cada vez que se presente un cambio en el componente.

## Señalar el problema de optimización asociado al evento del botón

Al realizar las pruebas en el navegador se puede observar el comportamiento esperado en la actualización del título de la ventana del navegador con el identificador de la imagen cuando el usuario posiciona el cursor sobre la imagen de algún expositor. No obstante hay un comportamiento que se evidencia dentro de la consola del navegador que puede ser considerado un bug.

Cuando se posiciona el cursor sobre una imagen, se deja un registro en la consoloa. Si el cursor se posiciona por fuera de la imagen y se vuelve a colocar sobre la imagen inicial se vuelve a tener un registro en la consola del llamado al `useEffect`, lo que significa que el título de la ventana del navegador se actualizó dos veces con el mismo identificador de la imagen. El escenario ideal sería evitar el segundo llamado a la actualización del título de la ventana cuando el identificador previo coincide con el identificador actual. A continuación se tiene una propuesta para optimizar este comportamiento:

```jsx
    import React, {useState, useEffect} from "react";
    import ImageToggleOnScroll from "../src/ImageToggleOnScroll";
    
    const ImageChangeOnScroll = () => {
      const [currentSpeakerId, setCurrentSpeakerId] = useState(0);
      const [mouseEventCounter, setMouseEventCounter] = useState(0);
    
      useEffect(() => {
        window.document.title = `SpeakerId: ${currentSpeakerId}`;
        console.log(`useEffect: setting title to ${currentSpeakerId}`);
      }, [currentSpeakerId]);
    
      return (
        <div>
          <span>mouseEventCounter: ${mouseEventCounter}</span>
          {[01, 02, 03, 04].map((speakerId) => {
            return (
              <div key={speakerId}
                onMouseOver={() => {
                  setCurrentSpeakerId(speakerId);
                  setMouseEventCounter(mouseEventCounter++);
                }
              }>
                <ImageToggleOnScroll
                  primaryImg="/static/speakers/bw/speaker-${speakerId}.jpg"
                  secondaryImg="/static/speakers/color/speaker-${speakerId}.jpg"
                  alt=""
                />
              </div>
            );
          })}
        </div>
      );
    };
    
    export default ImageChangeOnScroll;
```

El único cambio en este código corresponde al segundo parámetro del `useEffect`. Se agregó el siguiente valor `[currentSpeakerId]` en el arreglo de dependencia. Ahora al probar en el navegador se tiene el resultado esperado con el comportamiento de posicionar el cursor sobre una imagen, ubicarlo por fuera de la misma y volverlo a colocar sobre la imagen previa. Si se revisa la consola del navegador, ya no hay registro del llamado a `useEffect`. No obstante si se posiciona el cursor sobre otra imagen, el llamado si se ejecuta.

En pocas palabras esta es la optimización, usar el arreglo de dependencias para evitar que el llamado de `useEffect` no se realice de no ser necesario.

Por otra parte, una pregunta valida es porque se decidió mostrar un contador de los eventos del ratón en la pagína web. La razón es que dicho contador suministra información de que comportamiento esta generando el evento. Si el contador se hubiera omitido, probablemente nunca se hubiera identificado el problema de optimización descrito anteriormente.

# Conclusiones

En esta publicación se explicaron los hooks más usados en el mundo de React:

- `useState` es para rastrear el estado de manera fácil sobre detalles puntuales.
- `useEffect` ofrece una forma limpia para configurar funcionalidades dentro del ciclo de vida de un componente funcional en react, como por ejemplo, el estado.
- `useRef` suministra el control que en ocaciones se precisa sobre elementos del DOM.

