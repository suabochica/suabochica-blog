+++
title = "Instalando Storybook"
author = ["Sergio Benítez"]
description = "Serie que recopila los beneficios de usar Storybook"
date = 2021-01-11T00:00:00-05:00
lastmod = 2021-07-20T20:05:26-05:00
draft = false
+++

## Instalando Storybook {#instalando-storybook}

Antes de instalar Storybook es necesario instalar [NodeJS](https://nodejs.org/en/). Para validar si se tiene instalado el runtime se puede ejecutar el siguiente comando:

```nil
$ node -v
v12.0.1
```

Una vez instalado NodeJS, se inicializa un proyecto NodeJS corriendo el siguiente comando:

```nil
$ npm init -y
Wrote to /mnt/c/Users/suabochica/Development/suabochica-storybook/package.json:
```

```json
{
  "name": "suabochica-storybook",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/suabochica/suabochica-storybook.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/suabochica/suabochica-storybook/issues"
  },
  "homepage": "https://github.com/suabochica/suabochica-storybook#readme"
}
```

Con el proyecto NodeJS creado es momento de instalar algunas dependencias para correr Storybook con React. En ese orden de ideas, se ejecuta este comando:

```nil
$ npm i -s react react-dom
```

`i` es una abreviatura de `install` y `-s` es una opción para indicar que el paquete se instale en la sección `dependencies` del archivo `package.json`.

Tiempo de instalar Storybook con soporte para React:

```nil
$ npm i @storybook/react -D
```

`-D` es una opción para indicar que el paquete se instale en la sección `devDependencies` del archivo `package.json`.

El último paquete que se va a instalar para resolver temas de traspilación es `babel-loader`:

```nil
$ npm i babel-loader @babel/core -D
```

Una vez instalado todos los paquetes es tiempo de definir un script dentro del archivo `package.json` para inicializar el servidor de Storybook. El siguiente es un ejemplo del `package.json` con el script configurado:

```json
{
  "name": "suabochica-storybook",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "storybook": "start-storybook"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  },
  "devDependencies": {
    "@babel/core": "^7.12.10",
    "@storybook/react": "^6.1.14",
    "babel-loader": "^8.2.2"
  }
}
```

Por último, se debe crear un directorio con un archivo de configuración en la siguiente ruta `.storybook/config.js`. El contenido de dicho archivo es:

```js
import { configure } from "@storybook/react";

function loadStories() {
    const req = require.context('../stories', true, /\.stories\.js$/);

    req.keys().forEach(filename => req(filename));
}

configure(loadStories, module);
```

Básicamente se esta indicando que todos los archivos dentro de la carpeta `/stories` son requeridos para la cargar las historias dentro de Storybook. Esto implica la creación del drectorio `/stories` que por ahora puede estar vacio.

Tiempo de probar la instalación ejecutando el script de Storybook:

```nil
$ npm run storybook
```

Si se imprime la siguiente salida:

```nil
╭──────────────────────────────────────────────────────╮
│                                                      │
│   Storybook 6.1.14 started                           │
│   36 s for preview                                   │
│                                                      │
│    Local:            http://localhost:9001/          │
│    On your network:  http://192.168.224.161:9001/    │
│                                                      │
╰──────────────────────────────────────────────────────╯
```

Significa que la instalación ha sido exitosa


### Errores {#errores}

Eseta configuración no funciona con la version 13.3.0 de node. Al correr el comando `npm run storybook` se obtiene el siguiente error:

```nil
suabochica-storybook on 䳭 main [!?] is 📦 v1.0.0 via ⬢ v13.3.0 took 1m36s
❯ npm run storybook

> suabochica-storybook@1.0.0 storybook /mnt/c/Users/suabochica/Development/suabochica-storybook
> start-storybook -p 9001

internal/modules/cjs/loader.js:621
  throw e;
  ^

Error: No valid exports main found for '/mnt/c/Users/suabochica/Development/suabochica-storybook/node_modules/colorette'
    at resolveExportsTarget (internal/modules/cjs/loader.js:618:9)
    at applyExports (internal/modules/cjs/loader.js:499:14)
    at resolveExports (internal/modules/cjs/loader.js:548:12)
    at Function.Module._findPath (internal/modules/cjs/loader.js:650:22)
    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:948:27)
    at Function.Module._load (internal/modules/cjs/loader.js:854:27)
    at Module.require (internal/modules/cjs/loader.js:1023:19)
    at require (internal/modules/cjs/helpers.js:72:18)
    at Object.<anonymous> (/mnt/c/Users/suabochica/Development/suabochica-storybook/node_modules/autoprefixer/lib/autoprefixer.js:5:17)
    at Module._compile (internal/modules/cjs/loader.js:1128:30) {
  code: 'MODULE_NOT_FOUND'
}
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! suabochica-storybook@1.0.0 storybook: `start-storybook -p 9001`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the suabochica-storybook@1.0.0 storybook script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/suabochica/.npm/_logs/2021-01-12T17_17_11_966Z-debug.log
```

La solución es actualizar la versión de node ala 14.15.4


## Agregando Storybook a un proyecto existente {#agregando-storybook-a-un-proyecto-existente}

Para agregar Storybook en un proyecto existente, que generalmente debe estar bajo un sistema de control de versiones distribuido como git, tan solo se debe ejecutar el siguiente comando:

```nil
$ npx -p @storybook/cli sb init
```

`npx` es un corredor de paquetes que le permite ejecutar herramientas CLI que están alojadas en el registro de node. Con este comando, Storybook realizará los siguientes pasos:

1.  Detectará el tipo de proyecto (e.g. React)
2.  Agregará soporte sobre Storybook para la aplicación actual.
3.  Instalará las dependencias pertinentes.

Si no ahy ningún percance la salidar de este comando le indicara ejecutar el siguiente comando:

```nil
$ npm run storybook
```

Este comando constuye y lanzará el sandbox de Storybook en su navegador. Tras bambalinas, el comando inicial lo que hace es crear las carpetas `/.storybook` y `/stories` con una configuración por defecto, muy parecida a la que se compartió en la sección anterior. Adicionalmente, instala las dependencias de Storybook en función al tipo de proyecto y agrega los scripts para lanzar y construir el sandbox de Storybook.


## Aplicaciones de ejemplo {#aplicaciones-de-ejemplo}

La empresa Carved Rock Fitness lo ha contratado para que usted colabore con la construcción de un e-commerce. Actualmente la compañia cuenta con un home page como el que se muestra a continuación:

{{< figure src="../../images/storybook/01-storybook-interface.png" caption="Figure 1: Web Components on a homepage" >}}

En rojo se resaltan algunos elementos UI del sitio web: una barra de menú, un avatar, una barra de búsqueda, etc. La mayoria de estos elementos pueden reutilizarse a lo largo de toda la aplicación y perfilarlos como candidatos para convertirlos en componentes. Adicionalemnte, algunos precisarán de una documentación para determinar como y cuando deben ser utilizados.

Actualmente, Carved Rock Fitness tiene los diseños de dos páginas adicionales; la página de producto y la págica del carrito de compras. No obstante estás páginas no han sido implementadas aún.

Alguno elementos como la barra de menú y el avatar se repiten en estos bosquejos. Sin embargo hay nuevos elementos en estos diseños, como por ejemplo, contadores para determinar la cantidad y el tamaño del producto, que al final podrián ser el mismo componente con la variación de que para la cantidad se usan números y para el tamaño se usan textos. Por otra parte está el botón de agregar al carrito, cuyos estilos pueden ser reutilizados en otras páginas.

El punto es que cada uno de los bosquejos son una oportunidad para luchar en contra de los problemas de comunicación entre los roles que participan en la construcción del sitio web, promoviendo un camino que parte desde la frustación yse dirige hacia la colaboración de equipo, convirtiento estos elementos de UI, en componentes y por último en historias. Una vez se consoliden las historias, el equipo estará en capacidad de validar si las implementaciones coinciden con los diseños establecidos.
