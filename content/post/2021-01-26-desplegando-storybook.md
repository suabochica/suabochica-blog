+++
title = "Desplegando Storybook"
author = ["Sergio Benítez"]
description = "Serie que recopila los beneficios de usar Storybook"
date = 2021-01-26T00:00:00-05:00
lastmod = 2021-09-07T22:28:44-05:00
draft = false
+++

## Desplegando Storybook {#desplegando-storybook}

Hasta el momento, solo se ha trabajado con Storybook en un entorno local bajo una etapa de desarrollo que hace fácil evidenciar los cambios introducidos en el navegador. Sin embargo, la construcción del ambiente de desarrollo no es muy buena en rendimiento, ya que por ejemplo los archivos JavaScript no están minificados. Afortunadamente, Storybook tiene la habilidad de construir una copia de producción fácilmente. Si se revisa la sección de scripts en el archivo `package.json` se tienen las siguiente líneas:

```json
{...
  "scripts": {
    "lint": "eslint --fix ./",
    "storybook": "start-storybook -s ./Images -p 6006",
    "build-storybook": "build-storybook -s ./Images",
  }
}
```

Para construir una copia de producción se ejecuta el script `build-storybook` desde la línea de comandos:

```nil
$ npm run build-storybook
...

Output directory: Users/suabochica/Development/storybook/storybook-static
```

El comando crea un directorio de salida en donde se almacena la copia de producción del proyecto. Para revisar el resultado se pone la siguiente ruta en el navegador:

```nil
files:///Users/suabochica/Development/storybook/storybook-static/index.html
```

Dado que Storybook construye una versión estática del proyecto, el alojamiento es bastante sencillo ya que solo se necesita una servidor web.


### Opciones para alojamientos estático {#opciones-para-alojamientos-estático}

-   [Netlify](https://www.netlify.com) es una gran opción ya que se integra a varios proveedores de alojamiento de desarrollo de software, como GitHub, y es capaz de halar la rama que contiene el proyecto y ejecutar un comando, para este caso puntual, el comando sería `npm run build-storybook`.
-   AWS, es otra opción que permite copia los contenidos del folder con los estáticos generado por Storybook dentro de un bucket S3 y usar varias herramientas de build, como pipelines para consolidar el comando que crea la copia de producción.
-   Por último, el proyecto puede alojarse en cualquier infraestructura existente con la que el desarrollador esté familiarizado.

No importa el camino que se escoja, Stoybook ofrece la construcción de un entorno de producción que se adapata con facilidad a despliegues públicos.


## Soporte de Storybook {#soporte-de-storybook}

El soporte sobre Storybook es muy activo gracias a la enorme comunidad que esta detrás de esta herramienta. Para enfrentar casos específicos que no fueron abordados en esta serie de árticulos, se invita a dar un vistazo a los siguientes recursos:


### La documentación oficial de Storybook {#la-documentación-oficial-de-storybook}

<https://storybook.js.org/docs/react/get-started/introduction>

Este recurso cubre algunos de los temas revisados en la serie. Por ejemplo, el como empezar con el uso de Storybook. Adicionalmente, hay una sección de configuraciones que no se mencionó a lo largo de las publicaciones; hacer una configuración personalizada de Storybook en Webpack, Storybook para Vue, Storybook para Angular, entre otros.

Un aspecto llamativo de esta documentación es que a medida de que se va leyendo, el lector esta en capacidad de crear una solicitud de cambio (pull request en inglés) para mejorar las guías de la herramienta. Cada página tiene una opción de edición para solicitar modificaciones en los contenidos de la misma.


### El ejemplo oficial de Storybook {#el-ejemplo-oficial-de-storybook}

<https://storybooks-official.netlify.app/?path=/story/ui-panel--default>

Este recurso es quizas el más llamativo de los tres, puesto que es una documentación interactiva y una referencia del trabajo que se puede lograr con Storybook. Storybook tiene su propio Storybook, es decir usan su herramienta para construir su aplicación.

El proyecto esta almacenado en Netlify, un buen candidato para alojar sitios con contenidos estáticos, tal y como se recomendo previamente. Al entrar a este sitio web, la curiosidad es la mejor guía. Por ejemplo, en la sección de complementos, hay uno relacionado a accesibilidad. Se puede explorar el complemento antes de tomar la decisión de incluirlo en el propio Storybook.


### El canal oficial de Storybook en Discord {#el-canal-oficial-de-storybook-en-discord}

<https://discord.com/invite/UUt2PJb>

El último recurso es el chat en Discord de Storybook. Si no ha podido encontrar una solución a su problema luego de revisar la documentación, esta alternativa es bastante útil para abordar problemas en tiempo real. Allí va encontrar canales para frameworks específicos que le ayudarán a realizar su pregunta a la persona indicada.


## Actividades relacionadas {#actividades-relacionadas}

La primera actividad a realizar, es el complmento sobre el trabajo hecho con el ejercicio de ejemplo que se utiizó en estas publicaciones. Hay varias historias por desarrollar, como por ejemplo el filtro de productos por demografía.

Por otra parte, consultar que otros complementos están disponbiles para Storybook no esta de más. Se recomienda dar un vistazo al complemento de Viewports para atender temas de diseño responsivo y validar la visualización de las historias en diferentes tamaños de pantallas.

Se resalta nuevamente que Storybook no es un sistema de diseño. El enfoque apropiado sería que Storybook es una herramienta para construir un sistema de diseño y es importante hacer la distinción. El argumento parte de que el sistem de diseño es un mutuo acuerdo entre los miembros del equipo que requiere de mucho trabajo para se consolidado. Para la comprensión de los sistemas de diseño se invita a hacer la lectura de los siguientes libros:

-   [Design Systems](https://www.smashingmagazine.com/design-systems-book/), de Smashing Magazine
-   [Building Design Systems](https://www.apress.com/gp/book/9781484245132), de Apress

No obstante, hay toda una comunidad generando contenidos relativos a sistemas de diseño que se evidencian en coferencias o incluso en redes sociales.


## Recapitualción {#recapitualción}

En esta serie de publicaciones relacionadas a Storybook se trabajaron los siguientes temas:

-   Instalación y configuración de la herramienta
-   Adición de historias individuales y grupales
-   Personalización del tema de Storybook
-   Interacción con historias a trabés del complementos Knobs
-   Uso y creación de la documentación
-   Creación de complementos

Por otra parte, es importante volver a citar porque Storybook es una alternativa llamativa y que relación puede tener con los diferentes roles de un equipo:

-   Para un administrador, es una herramienta efectiva para lograr que todo el equipo se encuentre en la misma página ya que las discusiones sobre los comportamientos de los complementos parten de un mismo ambiente de pruebas.
-   Para un diseñador, hay una gran oportunidad para mejorar su flujo de trabajo. Este rol estará en capacidad de ejecutar chequeos de calidad sobre el proyecto al revisar la historia dentro de Storybook, teniendo encuenta que estos componentes son los mismos que se están usando dentro de la aplicación. Adicionalmente, el diseñador podrá participar activamente en la documentación de la historia entregando las guías necesarias al desarrollador para la implementación del componente diseñado.
-   Para un desarrollador, se podrá interactuar de manera rápida y sencilla con cada componente en entornos aislados, logrando replicas sobre estados que pueden se engorrosos de generar en la aplicación final. Además, se puede sacar provecho de la funcionalidades ofrecidas por los diferentes complementos, como Knobs, para visualizar datos en la historia de manera dinámica.

La conclusión es que Storybook es una herramienta para la colaboración y comunicación entre equipos al proveer repositorios centrales de documentación y ejemplos reales de casos de uso a través del sandbox.
