+++
title = "Primeros pasos con React Native"
date = 2018-07-26T00:00:00Z
author = "Sergio L. Benítez D."
description = "Primer post de una serie sobre React Native — qué es, cómo funciona internamente y cómo configurar un entorno de desarrollo con CRNA y Expo."
tags = [
    "javascript",
    "react",
    "react-native",
]
+++

Bienvenido. Este es el primer post de una serie sobre React Native. Esto es lo que cubriremos:

- **Post 1** — Por qué existe React Native y cómo poner en marcha un entorno de desarrollo sin volverte loco.
- **Post 2** — Las diferencias reales entre React y React Native (no es solo `<div>` vs `<View>`).
- **Post 3** — Estilos y layout. Porque flexbox en móvil es su propia bestia.
- **Post 4** — Navegación. El enrutamiento en nativo es un paradigma completamente distinto.
- **Post 5** — Funcionalidades nativas como geolocalización, notificaciones y publicación en la tienda de apps.

A lo largo de esta serie construiremos una app llamada **UdaciFitness** — un rastreador de entrenamiento de triatlón. Cubre cinco actividades:

- Correr
- Bicicleta
- Natación
- Dormir
- Comer

Conectaremos un calendario de entrenamiento, estableceremos itinerarios diarios y añadiremos recordatorios para registrar datos. Al final de la serie tendrás una app real y funcional en React Native.

¿Qué es React Native y por qué existe?
---------------------------------------

**React Native** te permite usar React para construir aplicaciones nativas de iOS y Android. ¿La verdadera ventaja? Puedes tener un solo equipo de UI en lugar de un equipo web, un equipo iOS *y* un equipo Android.

Seguramente has escuchado la frase:

*"Escribe una vez, ejecuta en cualquier lado."*

La idea es seductora: un solo código base para web, iOS y Android. Herramientas como Adobe PhoneGap y Apache Cordova apostaron por esto. Pero en la práctica es brutal — cada plataforma tiene una experiencia tan única que un solo código termina sintiéndose mal en todas partes.

El lema de React Native es diferente:

*"Aprende una vez, escribe en cualquier lado."*

Una vez que conoces React, puedes tomar esos mismos principios — composición de componentes, UI declarativa — y construir no solo para la web, sino también para plataformas nativas. No estás compartiendo un código base entre plataformas. Estás compartiendo una forma de pensar.

### React Native por dentro

Cuando React se presentó por primera vez, el gran atractivo era el **DOM Virtual**. Hoy es algo estándar, pero en ese entonces fue revolucionario.

El proceso clave dentro del DOM Virtual es la **reconciliación**. El objetivo es actualizar la UI basándose en el nuevo estado de la forma más eficiente posible. React construye un nuevo árbol de elementos, lo compara con el árbol anterior y determina exactamente qué cambió. Luego aplica solo las actualizaciones estrictamente necesarias.

Aquí está la clave: ¿y si en lugar de apuntar al DOM de la web, apuntamos a las vistas de iOS o Android? Eso es React Native. Internamente, muchos de los mismos principios — DOM Virtual, reconciliación, el algoritmo de diferencias — aplican tanto para web como para móvil. En otras palabras, el enfoque "aprende una vez, escribe en cualquier lado" de React Native no es un truco — es literalmente el mismo motor, apuntando a una superficie de renderizado diferente.

Configuración del entorno de desarrollo
---------------------------------------

### Create React Native App

Desarrollar para Android e iOS implica mantener dos entornos de desarrollo separados: Xcode y Android Studio. Eso es mucha configuración — cada una de esas herramientas tiene *su propio conjunto de cursos*.

Aquí entra **Create React Native App** (CRNA). Es como Create React App pero para móvil. Instala el CLI via npm, crea un proyecto y listo. No necesitas Xcode. Ni Android Studio. Es el camino más rápido para llegar a "Hola Mundo" en un dispositivo real, con una sola herramienta de construcción y sin compromiso.

#### Ventajas
- Tiempo mínimo para llegar a "Hola Mundo".
- Desarrolla en tu propio dispositivo.
- Una sola herramienta de construcción.

#### Desventajas
- No funciona si estás añadiendo React Native a una app nativa existente.
- No funciona si necesitas construir tu propio puente hacia una API nativa que CRNA no expone.

#### Instalando CRNA

```bash
npm install -g create-react-native-app
```

O con **yarn**:

```bash
yarn global add create-react-native-app
```

O incluso más corto:

```bash
yarn create react-native-app my-app
```

Necesitarás tener Watchman instalado (`brew install watchman`) para que el comando `yarn start` funcione.

### Expo

Expo es el arma secreta. Hace que todo en React Native sea más fácil. No necesitas Android Studio. Ni Xcode. Incluso puedes desarrollar para iOS desde Windows o Linux.

Con Expo, cargas y ejecutas proyectos CRNA usando JavaScript puro — sin compilación de código nativo. Descarga la app de Expo en tu dispositivo y ya estás listo.

Los simuladores son una alternativa, pero personalmente prefiero desarrollar en un dispositivo real. Ahorras tiempo en configuración y obtienes una sensación precisa de cómo se comporta tu app realmente.

### El entorno

Un proyecto CRNA soporta:

- ES5 y ES6
- Object Spread Operator
- Funciones asíncronas
- JSX
- Flow
- Fetch

Ya que estamos construyendo con JavaScript puro, esto no debería sorprender. Antes de empezar a programar, toma los archivos de utilidades de la carpeta `utils` — son el andamiaje que usaremos después. La conclusión: sin importar si tu objetivo es iOS o Android, o si estás en Mac, Windows o Linux, estás construyendo con el mismo JavaScript de siempre.

Usando el depurador
-------------------

### Cómo depurar

Una de las mejores cosas de React Native es que toma la experiencia de desarrollo web a la que estás acostumbrado y la lleva al entorno nativo. La recarga en vivo y la depuración simplemente funcionan.

Para abrir el menú de desarrollador, *agita tu teléfono* mientras la app se ejecuta con Expo. Verás una opción llamada **Debug JS Remote**. Al seleccionarla se abre una pestaña del navegador que ejecuta tu código JavaScript de React Native como un web worker. Desde ahí, abre las Herramientas de Desarrollo y depura igual que lo harías en la web.

Otra joya es **Toggle Inspector**. Es como el inspector de modelo de caja que usamos en la web — toca cualquier elemento y ve todos los estilos aplicados.

### Refrescando la app

En la web, cuando algo falla, recargas. En desarrollo nativo, normalmente tendrías que recompilar. Con React Native, solo *agitas tu teléfono* y presionas **Reload**.

Resumen
-------

El enfoque "aprende una vez, escribe en cualquier lado" de React Native significa que construyes tanto para web como para nativo usando los mismos principios — DOM Virtual, reconciliación y componentes declarativos — solo que apuntando a una superficie de renderizado diferente.

En cuanto a herramientas, Create React Native App genera un proyecto funcional con configuración mínima y sin necesidad de Xcode o Android Studio. Combínalo con Expo y estarás desarrollando en tu propio dispositivo, con una sola herramienta de construcción y sin compromiso. Cuando algo falle, agita tu teléfono para depurar usando las Herramientas de Desarrollo del navegador, recarga tu JavaScript sin recompilar o inspecciona elementos como lo harías en la web.
