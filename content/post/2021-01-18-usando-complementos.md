+++
title = "Usando complementos"
author = ["Sergio Benítez"]
description = "Serie que recopila los beneficios de usar Storybook"
date = 2021-01-18T00:00:00-05:00
lastmod = 2021-09-05T23:06:10-05:00
draft = false
tags = [
  "storybook",
]
+++

## Usando complementos {#usando-complementos}

Tener la disposición de como su componente se va a ver dentro de la aplicación final es un primer paso importante, pero ser capaz de ver como se comportará es igual de importante. Es tiempo de revisar como se interactua con un componente dentro de Storybook.

En la publicación pasada se dió un vistazo al complemento `action` para atender el evento del clic sobre el componente botón. Es conveniente revisar nuevamente como se configuró el uso de este complemento:

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

Dos cosas a reslatar de este snippet. Primero el complemento se importa directamente desde Storybook. Luego, la función `action` se usa dentro del componente CallToAction al indicar que la función `onClick` va a llamar a la función `action`. Dentro de esta última función el mensaje `"button-click` será procesado y se obtiene como salida un registro de la ejecución del evento dentro del panel Actions de la interfaz de Storybook.

Por otra parte, un patrón que se puede encontrar entre los complementos es que suelen agregar funcionalidades a la previsualización de la historia y a la sección de paneles que conforman la parte superior e inferior del GUI de Storybook respectivamente.

Los complementos son muy útiles para abordar los posibles comportamientos que puede manifestar un componente.


## Complementos con información {#complementos-con-información}

Una de las funcionalidades más llamativas de Storybook es la capacidad de ver exactamente lo que se necesita implementar para tener el componente funcionando en la aplicación. Esto se logra a través del complemente `info`. Para empezar  a utilizarlo primero debe ser instalado:

```nil
$ npm i -D @storybook/addon-info
```

Una vez instalado, se puede utilizar en los componentse tal y como se muestra a continuación, en el componentse Button:

```javascript
// stories/button.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import { withInfo } from '@storybook/addon-info';
import CallToAction from '../components/cta-button';

storiesOf('Button', module)
  .addDecorator(withInfo)
  .add('Call to Action', () => (
    <CallToAction
      label="Submit"
      onClick={action("button-click")}
    />
));
```

Este complemento se implementa ligeramente diferente al de `actions`, ya que requiere del uso de la función `addDecorator` de Storybook para React, el cual retorna un compenente empaquetado. Si revisa el navegador se obtiene el siguiente resultado:

{{< figure src="../../images/storybook/07-storybook-add-on-info-button.png" caption="Figure 1: Add-on info show button" >}}

Al dar clic en el botón Show Info, la vista cambia por:

{{< figure src="../../images/storybook/08-storybook-add-on-info-show.png" caption="Figure 2: Add-on info show info" >}}

Ahora está disponible la información necesaria para usar el componente adecuadamente. La visualización de la documentación puede modificarse, ya que la función `withInfo` puede recibir como argumento un objeto de configuración. Por ejemplo, se puede considerar innecesario que se tenga que dar clic en el botón Show Info para ver la información. Hay una vista alternativa en donde el componente se muestra dentro de la documentación. Para habilitarla se pasa el  siguiente objeto com parámetro de la función `withInfo`,

```javascript
// stories/button.stories.js
...
storiesOf('Button', module)
  .addDecorator(withInfo({
     inline: true,
   }))
));
```

Este complemento es bastante útil pero actualmente solo esta habilitado en el  componente Button. Si se revisa el componente Banner, no hay ningún botón Show Info que permita visualizar la documentación del Banner. Para habilitar el  complemento `info` globalmente en Storybook, se deben deshacer los cambios  mostrados previamente, y se debe actualizar el `.storybook/config.js`, con el siguiente código:

```javascript
// .storybook/config.js
import { addDecorators, addParameters, configure } from '@storybook/react';
import { withInfo } from '@storybook/addon-info';
import crfTheme from './crfTheme';

...

addDecorator(withInfo({
    inline: true
}));

configure(loadStories, module);
```

Con este cambio, se podrá visualizar la documentación de todos los componentes que estan alojados en Storybook.


## Knobs {#knobs}

Un complemento bastante útil en Storybook es Knobs, ya que permite editar las propiedades de un componente dinamicamente.

El primer paso es instalar el addon:

```nil
$ npm i @storybook/addon-knobs -D
```

Luego, se tienen que hacer las siguientes modificaciones en el archivo
`.storybook/config.js`:

```javascript
// .storybook/config.js
import { addDecorators, addParameters, configure } from '@storybook/react';
import { withInfo } from '@storybook/addon-info';
import { withKnobs } from '@storybook/addon-knobs';
import crfTheme from './crfTheme';

...

addDecorator(withInfo({
    inline: true
}));

addDecorator(withKnobs);

configure(loadStories, module);
```

Un detalle a considerar es el orden en el que se agregan los decoradores. Siempre el decorador del complemento `info` tiene que ser el primero para evitar comportamientos inesperados.

Se prosige con la actuialización en el código de alguno de los componentes para consumir knobs. En este caso se va a utilizar el MajorBanner With All Text Options como ejemplo:

```javascript
// stories/banner.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import { text } from '@storybook/addon-knobs';
import MajorBanner from '../components/major.banner';
import MinorBanner from '../components/minor.banner';

storiesOf('Major Banner', module)
  .add('With Only Title', () => (...))
  .add('With All Text Options', () => {
    const title = text('Title', 'BannerTitle');
    return (
      <MajorBanner
        photo="People Outdoors/shutterstock_116403520.jpg"
        title={title}
        subtitle="Banner Subtitle"
        body="Banner Body"
      />
    )
  });

storiesOf('Minor Banner', module)
  .add('No Pictures', () => (...))
  .add('With Pictures', () => (...));
```

Dos cosas a resaltar en este snippter. Primero, la importación de la función `text` desde knobs. Segundo, el uso de esta función en la historia With All Text Options se ve reflejado en una asignación a la variable `title` cuyo valor es igual al llamado de la función `text`, la cual recibe dos argumentos: El nombre de la propiedad, y el valor por defecto con el que se inicializa la propiedad.

Para terminar el consumo del complemento knobs en el componente, se actualiza la propiedad `title` en el `<MajorBanner>` para que  referencie la variable que se definió previamente.

El último paso es agregar el complemento `knobs` al registro de complementos en Storybook. En consecuencia el contenido del archivo `.storybook/addons.js`  debe ser el siguiente:

```javascript
// .storybook/addons.js
import '@storybook/addon-actions/register';
import '@storybook/addon-info/register';
import '@storybook/addon-knobs/register';
```

Se guarda el archivo y se reinicia el servidor de Storybook. Al revisar en el navegador se tendra una interfaz como la que se muestra a continuación:

{{< figure src="../../images/storybook/09-storybook-add-on-knobs.png" caption="Figure 3: Add-on knobs with a title" >}}

Ahora es tiempo de poner en práctica el poder de `knobs`. Si se cambia la entrada con el texto 'Banner Title' por 'Knobs' se puede observar como el texto del título en el componente se actualiza a 'Knobs'.

Knobs permite hacer más que edición de textos. Algunas opciones disponibles son booleanos, números, objetos, selects, fechas o incluso archivos. Para ver la lista de los knobs disponibles por facor revisar el siguiente [enlace](https://github.com/storybookjs/storybook/tree/master/addons/knobs).


## Sobreescribiendo configuraciones {#sobreescribiendo-configuraciones}

Otra de las funcionalidades atractivas de Storybook es que permite establecer configuraciones a nivel global, grupal e individual.

Todas las configuraciones globales están agrupadas en el archivos
`.storybook/config.js`

```javascript
// .storybook/config.js
import { addParameters, configure, addDecorator } from '@storybook/react';
import { withInfo } from '@storybook/addon-info';
import { withKnobs } from '@storybook/addon-knobs';

import crfTheme from './crfTheme';

import "../bootstrap-reboot.min.css"
import "../bootstrap.css"
import "../bootstrap-grid.css"
import '../main.css';

// automatically import all files ending in *.stories.js
const req = require.context('../stories', true, /\.stories\.js$/);
function loadStories() {
  req.keys().forEach(filename => req(filename));
}

addParameters({
  options: {
    theme: crfTheme
  }
})

addDecorator(withInfo({
  inline: true
}));

addDecorator(withKnobs);

configure(loadStories, module);
```

Ahora bien, se ha decidido que el componente MajorBanner, no debe mostrar su documentación bajo el formato `iniline` que esta definido como una configuración global. Para sobreescribir este formato se hace la siguiente modificación en el archivo `stories/banner.stories.js`

```javascript
// stories/banner.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import { text } from '@storybook/addon-knobs';
import MajorBanner from '../components/major.banner';
import MinorBanner from '../components/minor.banner';

storiesOf('Major Banner', module)
  .addParameters({
     info: {
       inline: false
     }
   }).add('With Only Title', () => (...))
  .add('With All Text Options', () => {
    const title = text('Title', 'BannerTitle');
    return (
      <MajorBanner
        photo="People Outdoors/shutterstock_116403520.jpg"
        title={title}
        subtitle="Banner Subtitle"
        body="Banner Body"
      />
    )
  });

storiesOf('Minor Banner', module)
  .add('No Pictures', () => (...))
  .add('With Pictures', () => (...));
```

Si se examina Storybook dentro del navegador, se puede evidenciar que las historias de MajorBanner ya no siguen el formato `inline` y muestran el botón Show Info en la parte superior derecha del visualizador de historias. No obstante, tanto el Button como el MinorBanner siguen mostrandose bajo el formato `iniline`. Es así como se sobreescribe una configurarción global de un complemento en un grupo de historias.

Anular las configuraciones globales, también puede hacerse a nivel de historias. A continuación se muestra como se va a sobreescribir el complemento `info` en la historia MinorBanner - No Pictures, para deshabilitar la vista `inline` en la historia:

```javascript
// stories/banner.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import { text } from '@storybook/addon-knobs';
import MajorBanner from '../components/major.banner';
import MinorBanner from '../components/minor.banner';

storiesOf('Major Banner', module)
  .addParameters(...)
  .add('With Only Title', () => (...))
  .add('With All Text Options', () => {...});

storiesOf('Minor Banner', module)
  .add('No Pictures', () => (
    <MinorBanner title="Banner Title" subtitle="Banner Subtitle" body="Banner Body" />
  ), {
    info: {
      inline: false,
    },
  })
  .add('With Pictures', () => (...));
```

Al verificar en el navegador, puede darse cuenta de que el formato inline ya no se aplica en el Minnor Banner - No Pictures, pero si en el Minnor Banner - With Pictures.

Es importante resaltar que el objeto para realizar las configuraciones en los tres casos fue practicamente el mismo. Por otra parte, varias de las opciones del complemento `info` se pueden sobreescribir; desde las fuentes de los encabezados y los snippets, el uso de tablas de propiedades hasta los estilos y el número máximo de propiedades.

En reseumen, este es la relación de las configuraciones por jerarquía:

-   Las configuraciones globales se hacen en `.storybook/config.js`
-   las configuracioens grupales se hacen en `stories/story.js` con la función `addParameters`
-   las configuracioens por historia se hacen en `stories/story.js` con el segundo

parámetro de la función `add`


## Revisión {#revisión}

Se cubrieron varios contenidos en esta publicación y es tiempo de destacar los puntos clave.

El principal dato para llevar es que Storybook ofrece una gran cantidad de opciones de configuración que se habilitan a través de complementos. Para este documentos se trabajaron tres complementos:

1.  `actions`, instalado por defecto, permite capturar eventos sobre las historias.
2.  `info`, el cual suministra el código fuente, la trabla de propiedades y texto

personalizable al rededor de la historia

1.  `knobs`, el cual permite ediciones dinámicas de los datos que se muestran en

la historia.

Todos los complementos se pueden consultar en la [página oficial de addons](https://storybook.js.org/addons) para Storybook. Hay unos muy útiles para trabajar temas de accesibilidad y otros sobre diseño responsivo.

Por otra parte, el uso de complementos desencadena un nuevo concepto, los decoradores. Los decoradores son envolturas para los componentes y son usados sobre las historias para habilitar cambios en los paneles o en el visualizador de historias.

Recuerde que para evitar problemas, una vez instalado el complemento, este se debe registrar en el archivo `addons.js` y posteriormente reiniciar el servidor de Storybook para así habilitar el consumo del mismo.

Por último, las configuraciones se pueden escribir a tres niveles: global, grupal y a nivel de historia.
