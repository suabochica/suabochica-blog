+++
title = "Creando historias"
author = ["Sergio Benítez"]
description = "Serie que recopila los beneficios de usar Storybook"
date = 2021-01-14T00:00:00-05:00
lastmod = 2021-09-04T11:16:03-05:00
draft = false
tags = [
  "storybook",
]
+++

Los secciones abordadas en este artículo, toman como referencia el proyecto alojado en el siguiente repositorio de github:

-   <https://github.com/taylonr/storybook-getting-started>


## Creando historias {#creando-historias}

Una historia captura el estado renderizado de un componente de la interfaz de usuario. Los desarrolladores escriben varias historias por componente que describen todos los estados "interesantes" que un componente puede soportar.

En el proyecto que se compartió al inicio de esta publicación, en la rama `m3-installing-storybook`, se tiene el siguiente estado: Dos historias están ya definidas, Welcome y Button, y dentro de la historia Button se encuentran dos items, `with text` y `with some emoji`. Estos dos items son ejemplos de historias. Al revisar los contenido de cada uno, se obtiene que la etiqueta del botón es diferente, demostrando así dos posibles usos para el componente Button.

Cada componente de ejemplo tiene un conjunto de historias que muestran los estados que soporta. Puede explorar las historias en la interfaz de usuario y ver el código detrás de ellas en archivos que terminan en `.stories.js` o `.stories.ts`. Las historias están escritas en Component Story Format (CSF), un estándar basado en módulos de ES6, para escribir ejemplos de componentes.

Si se revisa el proyecto a nivel del editor de texto, se tiene el siguiente contenido dentro de archivo `stories/index.stories.js`:

```js
// stories/index.stories.js

import React from 'react';

import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import { linkTo } from '@storybook/addon-links';

import { Button, Welcome } from '@storybook/react/demo';

storiesOf('Welcome', module).add('to Storybook', () => <Welcome showApp={linkTo('Button')} />);

storiesOf('Button', module)
  .add('with text', () => (
    <Button onClick={action('clicked')}>Hello Button</Button>
  ))
  .add('with some emoji', () => (
    <Button onClick={action('clicked')}>
      <span role="img" aria-label="so cool">
        😀 😎 👍 💯
      </span>
    </Button>
  ));

```

Es evidente que en este archivo se están enlistando cada una de las historias que se describieron anteriormente. Cada grupo de historias se empieza con la función `storiesOf`. Debajo de la función `storiesOf` correspondiente al Button se tienen dos instancias de la función `add`, y es así como se definen las historias individuales en grupos de historias más grandes. Como se puede dar cuenta, dentro de la función `add` se retornan componentes React. En resumen, aquí están las partes de una historia.

1.  El título del grupo: Generalmente suele ser el nombre del componente (e.g. Button).
2.  El titulo de la historia: A menudo suele describir los estados de la historia (e.g. with text).
3.  El cuerpo de la historia: Son los componentes a renderizar junto con sus respectivas propiedades.

<div class="notes">
  <div></div>

Nota: Cabe resaltar que la configuración de Storybook ha ido actualizandose con el lanzamiento de las últimas versiones. El artículo [Declarative Storybook configuration](https://medium.com/storybookjs/declarative-storybook-configuration-49912f77b78) aborda de manera clara la transición a los nuevos estándares de configuración.

</div>


## Escribiendo su primera historia {#escribiendo-su-primera-historia}

La primera historia que se va a escribir es otro uso de Button. Un caso paritcular de un botón es el _call to action_ para incentivar una acción en el usuario, como por ejemplo agregar un producto a un carro de compras. A continuación se muestra el códido de este elemento:

```javascript
// components/form/cta-button.js
import React from 'react';
import Button from './button';

const CallToAction = props => (
  <Button className="crf-button crf-button--call-to-action" {...props} />
);

export default CallToAction;
```

Para empezar, este componente importa el genérico Button. Luego especifíca algunos nombres de clases para estilos a través del atributo `className`, ya que és la palabra reservada en react para agregar selectores CSS en nuestro componente. Por último se usa el spread operator sobre los props (i. e. `{...props}`) para pasar todos los atributos que precisa el componente. Con esta sintaxis se esta diciendo que todos los atributos configurados en el CTA se van a aplicar sobre el botón base.

Ahora es tiempo de visualizar el componente en Storybook. Para ello, se va a crear el siguiente archivo dentro de la carpeta `/stories`:

```javascript
// stories/button.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import CallToAction from '../components/cta-button';

storiesOf('Button', module)
  .add('Call to Action', () => (
    <CallToAction label="Submit" />
));
```

Con este cambio, al abrir nuevamente Storybook en el navegador se puede ver que la historia CallToAction fue agregada al grupo Button, así este en un archivo diferente. Por ahora, este bóton no tienen ningun estilo en particular y es momento de personalizarlos, habilitando los selectores CSS definidos para el componente. Para lograr este objetivo se debe actualizar el archivo `config.js`.

```javascript
// .storybook/config.js
import { configure } from '@storybook/react';

import "../bootstrap-reboot.min.css"
import "../bootstrap.css"
import "../bootstrap-grid.css"
import '../main.css';

// automatically import all files ending in *.stories.js
const req = require.context('../stories', true, /\.stories\.js$/);
function loadStories() {
  req.keys().forEach(filename => req(filename));
}

configure(loadStories, module);
```

Se puede notar que lo que se agregó en este código fueron las importaciones de los estilos definidos por Bootstrap en sus respectivos archivos. Al salvar estos cambios, el servidor de Storybook va a reconstruir los archvios y si se revisa nuevamente el navegador, el botón del CallToAction ahora tiene un fondo naranja y un texto blanco, evidenciando que los selectores definidos en el componente están consumiendo los estilos establecidos por Bootstrap.

Ahora bien, regresando al estado actual del CallToAction, el botón solo está recibiendo una etiqueta y no se esta suministrando una vía para atender el evento del clic. Afortunadamente, Storybook cuenta con un complemento que viene por defecto en su istalación llamado <span class="underline">actions</span>. Para usarlo, se deben realizar los siguientes cambios en el archivo `stories/button.stories.js`:

```javascript
// stories/button.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import CallToAction from '../components/cta-button';

storiesOf('Button', module)
  .add('Call to Action', () => (
    <CallToAction
      label="Submit"
      onClick={action("button-click")}
    />
));
```

Primero, se importo el complemento directamente desde Storybook, y luego se agregó una nueva propiedad llamada `onClick` en la definición del componente CallToAction. Si se revisa el navegador, se pueda dar cuenta que no hay ningún cambio visual sobre el componente, pero ahora, si se da clic sobre el botón, en el panel de herramienta se vera un registro del evento ejecuto sobre el botón. Para ete caso, bajo la pestaña Actions se va imprimir el mensaje "button-click".

Esto es un pasabocas de los alcances de los complementos de Storybook. Este tema sera desarrollado más adelante.


## Usando assets en la historia {#usando-assets-en-la-historia}

Hay casos en donde los componentes requieren de assets para poder implementarse. Un ejemplo explicito es un banner. Bajo el contexto del e-commerce se requieren dos tipos de banner: Major y Minor. Los siguientes snippets corresponden a las implementaciones de estos componente en React:

```javascript
// components/major.banner.js
import React from 'react';
import PropTypes from 'prop-types';

const MajorBanner = ({
  title, subtitle, body, photo,
}) => (
  <div className="jumbotron jumbotron-fluid crf-hero d-flex" style={{ backgroundImage: `url("./${photo}")` }}>
    <div className="container d-flex flex-column justify-content-center align-items-sm-stretch align-items-md-center">
      <h1 className="col-sm-12">{title}</h1>
      <h2>{subtitle}</h2>
      <p className="lead">{body}</p>
    </div>
  </div>
);

MajorBanner.propTypes = {
  /** The last line of the text */
  body: PropTypes.string,
  /** The URL to the background image */
  photo: PropTypes.string,
  /** The middle line of text. Stands out due to color */
  subtitle: PropTypes.string,
  /** The top and most prominent portion of the text */
  title: PropTypes.string,
};

MajorBanner.defaultProps = {
  body: null,
  photo: null,
  subtitle: null,
  title: null,
};

export default MajorBanner;
```

```javascript
// components/minor.banner.js
import React from 'react';
import PropTypes from 'prop-types';

const MinorBanner = ({
  title, subtitle, body, leftPhoto, rightPhoto,
}) => (
  <div className="container crf-cigar-banner">
    <div className="row">
      <div className="crf-cigar-banner--container d-flex justify-content-center align-items-center">
        {
                    leftPhoto && <img alt="Brown Boots" className="order-sm-0 order-md-0" src={leftPhoto} />
                }

        <div className="crf-cigar-banner--text order-sm-2 order-md-1">
          <div className="text-light">{title}</div>
          <div className="text-secondary">{subtitle}</div>
          <div className="text-primary">{body}</div>
        </div>
        {
                    rightPhoto && <img alt="Grey Boots" className="order-sm-1 order-md-2" src={rightPhoto} />
                }
      </div>
    </div>
  </div>
);

MinorBanner.propTypes = {
  /** The last line of the text */
  body: PropTypes.string,
  /** The URL to the left image */
  leftPhoto: PropTypes.string,
  /** The URL to the right image */
  rightPhoto: PropTypes.string,
  /** The middle line of text. Stands out due to color */
  subtitle: PropTypes.string,
  /** The top and most prominent portion of the text */
  title: PropTypes.string,
};

MinorBanner.defaultProps = {
  body: null,
  leftPhoto: null,
  rightPhoto: null,
  subtitle: null,
  title: null,
};

export default MinorBanner;
```

Notesé que ambos componentes requiere como propiedades algunas fotografías. Ahora es tiempo de definir la historia para el banner. La historia se va a organizar de la siguiente manera:

-   Major Banner
    -   With Only Tytle
    -   With All Text Options
-   Minor Banner
    -   No Picture
    -   With Pictures

En consecuencia, se va a crear un archivo `stories/banner.stories.js` con el siguiente contenido:

```javascript
// stories/banner.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import MajorBanner from '../components/major.banner';
import MinorBanner from '../components/minor.banner';

storiesOf('Major Banner', module)
  .add('With Only Title', () => (
    <MajorBanner title="Banner Title" photo="People Outdoors/shutterstock_116403520.jpg" />
  ))
  .add('With All Text Options', () => (
    <MajorBanner
      photo="People Outdoors/shutterstock_116403520.jpg"
      title="Banner Title"
      subtitle="Banner Subtitle"
      body="Banner Body"
    />
  ));

storiesOf('Minor Banner', module)
  .add('No Pictures', () => (
    <MinorBanner title="Banner Title" subtitle="Banner Subtitle" body="Banner Body" />
  ))
  .add('With Pictures', () => (
    <MinorBanner
      title="Banner Title"
      subtitle="Banner Subtitle"
      body="Banner Body"
      leftPhoto="Products/boots/shutterstock_66842440.jpg"
      rightPhoto="Products/boots/shutterstock_1121278055.jpg"
    />
  ));
```

Se se revisa el estado actualmente de las nuevas historias que se agregaron, se observa que fueron incluidas dentro del panel lateral de Storybook, pero los componentes se están renderizando sin las fotografías. Esto se debe a que Storybook no tiene las herramienta para encontrar las rutas especificadas en las propiedades `photo` del MajorBanner y `leftPhoto`, `rightPhoto` del MinorBanner.

Para solucionar este percance, se debe actualizar el script de Storybook en el `packages.json` indicando la carpeta en donde se van a almacenar los assets del proyecto. Para esta caso puntual el directorio es `/Images` y en consecuencia el script quedaría así:

```json
// package.json
...
  "scripts": {
    "storybook": "start-storybook -s ./Images -p 6006",
  },
...
```

Con la opción `-s` se especifica la ruta en donde se van a almacenar los archivos estáticos del proyecto. El gran beneficio de realizar esta configuración es que ahora los componentes se están perfilando para consolidarse en la applicación final


## Agrupando historias {#agrupando-historias}

Storybook es una herramienta utilizada para realizar prototipado rápido y es útil saber rapidamente que componentes existen. Para ello, definir una convención para organizar las historias es de gran ayuda, y Storybook ofrece varias sintáxis para establecer la estructura del directorio de historias:

Actualmente, el navegador despliega las siguientes historias:

{{< figure src="../images/storybook/03-storybook-current-stories.png" caption="Figure 1: Current stories organization" >}}

La primera asociación a realizar es la de agrupar el Major y el Minor Banner, en una categoria Banner. Para lograr esta jerarquía se debe actualizar el título de las historias del banner bajo la siguiente convención:

```javascript
// stories/banner.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import MajorBanner from '../components/major.banner';
import MinorBanner from '../components/minor.banner';

storiesOf('Banners/Major', module)
  .add('With Only Title', () => (...))
  .add('With All Text Options', () => (..));

storiesOf('Banners/Minor', module)
  .add('No Pictures', () => (...))
  .add('With Pictures', () => (...));
```

Con este cambio en el título, el navegador va a organizar las historia como se muestra en la siguiente imagen:

{{< figure src="../images/storybook/04-storybook-grouping-stories-by-component.png" caption="Figure 2: Organizing stories by component" >}}

Se puede agregar otro nivel en el árbol utilizando el carácter `/`. Por ejemplo, si el título de la historia se cambia por `'Banners/Adverstiment/Major'`, se mostrarían tres niveles en la estructura del árbol. Esta covención es equivalente a las rutas de los archivos definidas a través de una terminal.

Adicionalmente, Storybook ofrece otra sintaxis para agrupar las historias dentro de etiquetas, tal y como se muestra en este snippet:

```javascript
// stories/banner.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import MajorBanner from '../components/major.banner';
import MinorBanner from '../components/minor.banner';

storiesOf('Component | Banners/Major', module)
  .add('With Only Title', () => (...))
  .add('With All Text Options', () => (..));

storiesOf('Component | Banners/Minor', module)
  .add('No Pictures', () => (...))
  .add('With Pictures', () => (...));
```

Ahora, el panel lateral de Storybook organizará las historias con el formato de abajo:

{{< figure src="../images/storybook/05-storybook-grouping-stories-by-label.png" caption="Figure 3: Organizing stories by label" >}}

Esta ofertas en sintáxis ayudan a romper Storybook en distinciones significativas. Algunas etiquetas comunes son 'Foundational' en donde se agrupan cosas como la tipografía y el espaciado. Otra es 'Form elements' para reunir elementos como botones y cajas de entrada de texto. En últimas la elección de un esquema de organización realmente la toma el equipo de trabajo.


## Tematizando Storybook {#tematizando-storybook}

Otro de los atractivos de Storybook es que es una herramienta personalizable. Eso significa, que para cuestiones de marca se pueden hacer una serie de configuraciones para evitar que Storybook se visualice como una herramienta genérica.

Siguiendo con el hilo del e-commerce, a continuación se va a crear un archivo `.storybook/crfTheme.js` en donde se va a configurar el tema de la marca sobre Storybook:

```javascript
// .storybook/crfTheme.js
import { create } from '@storybook/theming';

export default create({
    base: 'dark',
    brandTitle: 'Carved Rock Fitness',
    brandImage: 'Logos/carved-rock-logo-yellow.png',
    colorSecondary: '#faa541'
});
```

Lo mas importante en este snippet es la importación de la función `create`, la cual recibe como argumento un objeto con las siguientes configuraciones:

-   La propiedad `base` indica que nuestra tema va a ser oscuro, en vez de claro.
-   La propiedad `brandTitle` establece el título de la marca
-   La propiedad `brandImage` usa el logo de la marca
-   La propiedad `colorSecondary` actualiza el color de fondo de la historia consultada

Para poder utilizar este archivo de configuración se debe acualizar el archivo `.storybook/config.js`.

```javascript
// .storybook/config.js
import { addParameters, configure } from '@storybook/react';
import crfTheme from './crfTheme';

...

addParameters({
  options: {
    theme: crfTheme
  }
})

configure(loadStories, module);
```

Lo relevante en este snippet en cuanto personalización de tema es la importación de la función `addParameters` y el archivo `crfTheme` implementado anteriormente. La función `addParameters` recibe como argumento un objeto con las posibles opciones personificables de Storybook. Para este caso puntual a la llave `theme` se le pasa el valor `crfTheme`. Si se actualiza el navegador, ahora la interfaz de Storybook será como:

{{< figure src="../images/storybook/06-storybook-theming.png" caption="Figure 4: Theming Storybook" >}}

Storybook tiene muchos más elementos que son personalizables. Para saber más sobre dichas opciones se hace la invitación a revisar la documentación oficial de Storybook en la sección [Theming](https://storybook.js.org/docs/react/configure/theming).


## Problemas comunes y recordatorios {#problemas-comunes-y-recordatorios}

A continuación se recopila una serie de problemas comunes que se presentan con storybook:

-   La historia no se muestra. Para solucionar este inconveniente, aseguresé de que el nombre del archvio es correcto y coincide con la expresión definida en el archivo de configuración. Para este caso la ruta es `../stories`
-   Los componentes no estan con los estilos esperados. Para resolver este problema recuerde importar los archivos CSS en el `config.js`
-   Los assets están perdidos. Para ajustar este contratiempo, recuerde indicar la ruta de los archivos éstaticos en el script de storybook definido en el `package.json`.
