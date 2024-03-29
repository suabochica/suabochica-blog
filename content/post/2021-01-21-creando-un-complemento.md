+++
title = "Creando un complemento"
author = ["Sergio Benítez"]
description = "Serie que recopila los beneficios de usar Storybook"
date = 2021-01-21T00:00:00-05:00
lastmod = 2021-09-05T23:12:57-05:00
draft = false
tags = [
  "storybook",
]
+++

## Creando un complemento {#creando-un-complemento}

Una vez repasados los temas de escritura de historias y uso de complementos, el siguiente paso es indagar sobre la creación de complementos. Es posible que en algún momento se precise de alguna funcionalidad que todavía no existe como un complemento de Storybook.

Antes de escribir el complemento es adecuado definir que va a hacer dicho complemento. Para este caso se va a crear una nueva pestaña en la sección de paneles llamada **See Also**, la cual redireccionará a historias relacionadas con la historia actual. La siguiente es una imagen con la vista del nuevo componente:

{{< figure src="../../images/storybook/10-storybook-see-also-add-on.png" caption="Figure 1: Add-on info show button" >}}

Detras de escenas este es el paso a paso del uso del complemento personalizado.

1.  El usuario seleciona una historia desde la barra lateral, en donde está el explorador de historias.
2.  Al seleccionar la historia el evento de cambio es ejecutado. Es así como Storybook carga una historia, incluyendo todos los datos necesarios para visualizar la misma.
3.  La idea es capturar este evento para que con algún código se agregue el panel See Also, junto con los otros paneles que la historia carga. Asi mismo, durante la etapa en la que Storybook procesa la información de la historia que esta siendo cargada, se va a adjuntar la información de las historias relacionadas que serán agregadas al panel See Also.
4.  Por último, cuando el usario hace clic sober la historia que esta dentro del panel See Also, el panel captura ese evento y a través del API de Storybook se indica que información se va a cargar.

En terminos generales este será el complemento a implementar.


## Creando el panel {#creando-el-panel}

Para crear el panel See Also se va a crear un directorio `seeAlso` con un archivo `register.js` el cual tendrá el siguiente contendio:

```javascript
// seeAlso/register.js
import React from 'react';
import addons from '@storybook/addons';

class SeeAlsoPanel extends React.Component {

  render() {
    return <div>See also panel </div>;
  }
}

// Register the addon with a unique name.
addons.register('SEEALSO', (api) => {
  addons.addPanel('SEEALSO/panel', {
    title: 'See Also',
    render: ({ key }) => <SeeAlsoPanel key={key} api={api} />,
  });
});
```

Varias cosas a resaltar de este snippet. Primero es la importación de `React` y de `addons` desde los respectivos paquetes. Con React se va a crear el componente `SeeAlsoPanel` que será renderizado dentro del panel See Also. La integración entre este componente y Storybook se realiza con el objeto `addon`. La función `register` del objeto `addon` recibe como argumentos un identificador para el complemento y una función con el objeto `api` como argumento. Este objeto `api` será revisado con mayor detalle más adelante. Dentro de esta función se hace el llamado a la función `addPanel`, a quién se le pasa como primer argumento el identificador del complemento y como segundo parámetro obtiene un objeto de configuración. En dicho objeto se indica el título del panel y en la propiedad `render` se llama el componente SeeAlsoPanel con las propieades key y api.

Ahora bien, para habilitar este complemento dentro de Storybook es necesario agregarlo al registro de complementos de Storybook, de la misma forma que se hizo con Knobs. En ese orden de ideas el archivo `.storybook/addons.js` debe actualizarse de la siguiente manera.

```javascript
// .storybook/addons.js
import '@storybook/addon-actions/register';
import '@storybook/addon-info/register';
import '@storybook/addon-knobs/register';
import '../seeAlso/register'
```

Se reinicia el servidor de Storybook y al abrir el navegador se podrá visualizar el panel See Also cuyo contenido es el texto "See also panel" que por ahora es un texto estático. En las siguientes secciones se va a revisar como agregarle datos al complemento y como atender los eventos de Storybook.


## Recibiendo datos {#recibiendo-datos}

Por ahora el componente SeeAlso no resulta útil si no esta en capacidad de mostrar datos. Para interactuar con datos, se va a hacer uso del estado en los componentes React. En ese orden de ideas se va a modificar el archivo `seeAlso/register.js` de la siguiente manera.

```javascript
// seeAlso/register.js
import React from 'react';
import addons from '@storybook/addons';

class SeeAlsoPanel extends React.Component {
  state = {seeAlso: {}};
  setData = (seeAlso) => {
    this.setState({seeAlso});
  }

  componentDidMount() {
    const {api} = this.props;
    api.on("seeAlso/storySelected", this.setData);
  }

  componentWillMount() {
    const {api} = this.props;
    api.off("seeAlso/storySelected", this.setData);
  }

  render() {
    const {seeAlso} = this.state;
    return <div>{seeAlso.label}</div>;
  }
}

// Register the addon with a unique name.
addons.register('SEEALSO', (api) => {
  addons.addPanel('SEEALSO/panel', {
    title: 'See Also',
    render: ({ key }) => <SeeAlsoPanel key={key} api={api} />,
  });
});
```

El primer detalle es la definción de `state`, inicalmente como un objeto vacío. Luego se tiene la definición de la función `setData` que como su nombre lo indica, al momento de ser llamado va a actualizar el estado del componente con ayuda de la función `setState`.

Posteriormente se tienen los llamados de las funciones que nos permiten acceder al ciclo de vida del componente React: `componentDidMount` y `componentWillMount`. el cuerpo de ambos funciones es muy parecido, ya que recuperan el objeto `api` desde las propiedades del componente y la diferencia esta en la suscripción y la cancelación a la suscripción del evento `seeAlso/storySelected` con los métodos `on` y `off` respectivamente.

Por último, en la función `render` del componente en vez de quemar un texto dentro de la etiqueta `div`, se imprime la etiqueta `label` del objeto `seeAlso`.

Ahora bien, esta pendiente buscar una forma para usar el complemento y para ello se va a crear un archvo `seeAlso/index.js` con el siguiente código:

```javascript
// seeAlso/index.js
import addons, { makeDecorator } from '@storybook/addons';

const wihtSeeAlso = makeDecorator({
  name: 'withSeeAlso',
  parameterName: 'seeAlso',
  skipIfNoParametersOrOptions: true,
  wrapper: (getStory, context, {parameters}) => {
    const channel = addons.getChannel();

    channel.emit("seeAlso/storySelected", parameters);

    return getStory(context);
  }
})
export default withSeeAlso;
```

El protagonista en este snippet es la función `makeDecorator`. Se resalta que un decorador en el contexto de Storybook es una forma de empaquetar en una historia funcionalidad extra en la renderización. Esta función obtiene como parámetro un objeto de configuración con diferentes propiedades. Para este caso se usaron las propiedades `name`, `parameterName`, `skipIfNoParametersOrOptions` y `wrapper`. Las tres primeras se explican por si solas, y la propiedad `wrapper` es la llamativa, puesto que su valor es una función que recibe tres parámetros, `getStory`, `context` y `parameters` y en su cuerpo emite el evento que permite la interacción entre Storybook y el complemento SeeAlso.

En ese orden de ideas, se obtiene un canal de Storybook con el que se comunica el panel SeeAlso que se creo en el archivo `register.js`. Este canal emite el evento al que se suscribe en la función `componentDidMount` junto con los parámetros que se envian desde la historia. Al final, se llama la función `getStory` con el argumento `context` para permitirle a Storybook que continue con todo lo demás que debe procesar para una historia en particular.

Este es un resumen de los pasos descritos:

-   Se modificó el registro para empezar a escuchar eventos
-   Se actualizó el componente con manejo de estado
-   Se usa el estado durante la renderización del componente
-   Se creó un decorador para comunicar Storybook con el complemento

Este complemento esta empezando a tener forma. Es tiempo de empezar a utilizarlo.


## Usando el nuevo complemento {#usando-el-nuevo-complemento}

Es tiempo de emepzar a usar el complemento SeeAlso, y la historia candidato para hacerlos es el MajorBanner. En ese orden de ideas se introducen las siguientes modificaciones en el archivo `stories/banner.stories.js`

```javascript
// stories/banner.stories.js
import React from 'react';
import { storiesOf } from '@storybook/react';
import { text } from '@storybook/addon-knobs';
import MajorBanner from '../components/major.banner';
import MinorBanner from '../components/minor.banner';
import withSeeAlso from '../seeAlso/index';

storiesOf('Components | Banners/Major', module)
  .addDecorator(withSeeAlso)
  .addParameters({
    info: {
      text: `
        ### When to Use
        This banner should be used, at most, once per page. When it is used, it should be placed at the top of the page, below the navigation bar. This banner is considered "Shouting", the focus is to grab the attention of the user before they have a chance to see anything else.
        ___
        **Location:**  1st element below navigation
        **Max Quantity:** 1
        **See Also:** Minor Banner
      `,
    },
    seeAlso: {
        story: 'Components | Banners/Minor',
        label: 'Minor Banners'
    },
  })
  .add('With Only Title', () => (
    <MajorBanner title="Banner Title" photo="People Outdoors/shutterstock_116403520.jpg" />
  ))
  .add('With All Text Options', () => {
    const title = text('Title', 'Banner Title');
    return (
      <MajorBanner
        photo="People Outdoors/shutterstock_116403520.jpg"
        title={title}
        subtitle="Banner Subtitle"
        body="Banner Body"
      />
    );
  });

storiesOf('Components | Banners/Minor', module)
  .add(...)
  .add(...);
```

Se van a recalcar cada uno de los cambios introducidos en este snippet. El primero es la importación del complemento `witSeeAlso`. El segundo, es el uso del método `addDecorator` que recibe como argumento el complemento importado previamente. El tercer cambio es la propiedad `seeAlso` del objeto que se pasa como parámetro de la función `addParameters`. Se recuerda que el complemento será ignorado si no se pasan los parámetros respectivos, que para este caso es un objeto con las propiedades `story`, cuyo valor es el grupo de la historia que se va a relacionar, y el `label` el cual será el nombre que se mostrará en el contenido del panel SeeAlso.

Si se examina el servidor de Storybook en el navegador se puede evidenciar que el nombre de la historia relaiconada aparece como contenido del panel SeeAlso. No obstante, por ahora este texto es estático y si se revisa otra historia, por ejemplo, Button, se observa que en el contenido del panel SeeAlso se esta apareciendo nuevamente la historia MinorBanner, y este comportamiento no tiene sentido. La razón por la cual se mantiene esta relación es por que no se esta limpiando el estado en nuestro complemento al momento de consultar una nueva historia. Para atender este comportamiendo se debe actualizar el contenido del archivo `seeAlso/register.js` como se muestra a continuación:

```javascript
// seeAlso/register.js
import React from 'react';
import PropTypes from 'prop-types';
import addons from '@storybook/addons';
import { STORY_CHANGED } from '@storybook/core-events';

class SeeAlsoPanel extends React.Component {
  state = { seeAlso: {} };

  componentDidMount() {
    const { api } = this.props;
    api.on('pluralsightSeeAlso/storySelected', this.setData);
    api.on(STORY_CHANGED, this.clearState);
  }

  componentWillUnmount() {
    const { api } = this.props;
    api.off('pluralsightSeeAlso/storySelected', this.setData);
  }

  setData = (seeAlso) => {
    this.setState({ seeAlso });
  };

  clearState = () => {
    this.setState({ seeAlso: null });
  }

  render() {
    const {api} = this.props;
    const { seeAlso } = this.state;
    return seeAlso
      ? (<div> {seeAlso.label}</div>)
      : null;
  }
}

SeeAlsoPanel.propTypes = {
  api: PropTypes.object.isRequired,
};

// Register the addon with a unique name.
addons.register('SEEALSO', (api) => {
  addons.addPanel('SEEALSO/panel', {
    title: 'See Also',
    render: ({ key }) => <SeeAlsoPanel key={key} api={api} />,
  });
});
```

Nuevamente hay varias cosas para destacar. Primero esta la importación del evento `STORY_CHANGED` desde el núcleo de Storybook. El complemento se suscribe a dicho evento en la función `componentDidMount` y ejecuta una nueva función llamada `clearState` para asignar el objeto del estado a un nulo. De esta manera se soluciona el inconveniente de asociar la historia del Button con el MinorBanner.

Ahora bien, sigue pendiente una tarea, y es redirigir desde el panel SeeAlso a la historia que se encuentra dentro de su contenido cuando se hace clic sobre la etiqueta de dicha historia. Para ello se deben hacer unas modificaciones en la función `render` del archivo `seeAlso/register.js`

```javascript
// seeAlso/register.js
import React from 'react';
import PropTypes from 'prop-types';
import addons from '@storybook/addons';
import { STORY_CHANGED } from '@storybook/core-events';

class SeeAlsoPanel extends React.Component {
  state = { seeAlso: {} };

  componentDidMount() {...}

  componentWillUnmount() {...}

  setData = (seeAlso) => {...};

  clearState = () => {...}

  render() {
    const {api} = this.props;
    const { seeAlso } = this.state;
    return seeAlso ? (
      <a onClick={() => api.selectStory(seeAlso.story)}
          style={{ paddingLeft: '20px', cursor: 'pointer' }}
      >
        {seeAlso.label}
      </a>
    ) : null;
  }
}

SeeAlsoPanel.propTypes = {
  api: PropTypes.object.isRequired,
};

// Register the addon with a unique name.
addons.register('SEEALSO', (api) => {
  addons.addPanel('SEEALSO/panel', {
    title: 'See Also',
    render: ({ key }) => <SeeAlsoPanel key={key} api={api} />,
  });
});
```

El primer cambio es que se reemplazo la etiqueta HTML `<div>` por `<anchor>` y se hace uso de la función de React `onClick` a la cual se le pasa una función que dentro de su cuerpo utiliza el objeto `api` de Storybook recuperado desde las propiedades del componente React para llamar la función `selectStory` que recibe como argumento la propiedad `seeAlso.story` que se definió en los parámetros del archivo `stories/banner.stories.js`. Para este caso puntual, ese valor es `'Components | Banners/Minor'`.

Adicionalmente, se uso la propiedad `style` dentro del jsx para especificar un `paddingLeft`  y actualizar el cursor a un apuntador cuando se flota por encima del nombre de la historia relacionada.

Al validar en el navegador estos últimos cambios, el complemento esta funcionando tal y como se espera, redirigiendo al la historia relacionado al momento de dar clic sobre la etiqueta que aparece en el contenido del panel SeeAlso.


## API de Storybook {#api-de-storybook}

Anteriormente se uso el API de Storybook para saltar de una historia a otra. No obstante, el API ofrece muchas más funcionalidades que resultan útiles si se quieren desarrollar complementos avanzados.

En el complemento SeeAlso se usaron las siguientes funciones del API:

-   `getChannel`: Para proveer un canal de comunicación entre la historia y el administrador de Storybook. Así es como se activa el evento de seleccionar una historia.
-   `register`: Para habilitar el API de Storybook dentro del complemento. Si se recuerda el arhcivo `.storybook/register.js` al momento de definir el complemento se pasa como uno de los parámetros el objeto `api` de Storybook y es así como se pueden usar funciones como `selectStory`.
-   `addPanel`: Para crear el nuevo panel dentro de la sección de paneles de Storybook.
-   `selectStory`: Para seleccionar especificamente el tipo de historia ó las historias que son parte de la historia. En el caso puntual del ejercicio se uso sobre `'Components | Banners/Minor'`.

Ahora bien, estás son algunas funciones adicionales del API de Storybook que pueden ser de utilidad:

-   `selectInCurrentKind`: Esta función es parecida a `selectStory`, pero solo opera sobre el tipo actual de historia. Recibe como parámetro el nombre de la historia. Por lo tanto, si se usará sobre el tipo MinnorBanner, permitirá seleccionar cualquiera de sus dos sub historias; `No Pictures` o `With Pictures`. Pero no le permitiría seleccionar las historias de MajorBanner.
-   `get/setQueryParams`: Esta función es útil si se decide interactuar con parámetros en las URLs. Un caso común es tener un almacenamiento temporal para el complemento, agregando o removiendo parámetros a la URL.
-   `onStory(fn)`: Esta función toma otra función como parámetro, y es llamada cuando el usuario navega entre historias. La función callback es llamada con el tipo y la historia, para habilitar la ejecución de acciones cuando la historia cambia.

El uso de estás funciones habilita la creación de complementos más poderosos, ofreciendo la posibilidad de extender Storybook para que se ajuste a las necesidades de los equipos.


## Recapitulación {#recapitulación}

De los posibles usos de Storybook, quizas las creación de complementos sea la menos utilizada. Sin embargo, es importante tener presente la personalización que se puede ejercer sobre la herramienta.

Para la creación de un nuevo complemento, hay cuatro aspectos fundamentales:

1.  El componente de panel: Este fue creado en el archivo de registro y esta en capacidad de aceptrar un mostrar los datos que vienen desde la historia.
2.  La función de registro: También es parte del archivo de registro y se usa para garantizar que el nuevo panel estará en la lista de paneles de Storybook
3.  El decorador para la historia: Este fue creado en el archivo índice y es usado para enviar eventos al nuevo panel.
4.  Los parámetros establecidos para el complemento: Son agregado desde la historia y su objetivo es atender la configuración del complemento.

Algunos problema comunes con sus respectiva soluciones son:

-   Los datos no son recibidos; Recuerde que los nombres de los eventos deben coincidir.
-   Los datos sobrantes; Valide que dentro del flujo del complemento hay un espacio para limpiar el estado del mismo.
-   El API de storybook no funciona; Aseguresé de que la variable para asignar el API esta declarada.
