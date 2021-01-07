+++
title = "Comandos Básicos en Bash"
date = 2018-09-10T02:13:50Z
author = "Sergio L. Benítez D."
description = "Comandos para navegar con Bash. Lectura, creación y borrado de archivos y carpetas con Bash"
tags = [
    "ops",
    "bash",
]
+++

### Índice

1. [Navegar en el sistema de archivos con Bash](#1-navegar-el-sistema-de-archivos-con-bash)
2. [Ver archivos y carpetas en Bash](#2-ver-archivos-y-directorios-en-bash)
3. [Crear y borrar archivos y carpetas en Bash](#3-crear-y-borrar-archivos-y-directorios-en-bash)
4. [Mover y copiar archivos y carpetas en Bash](#4-mover-y-copiar-archivos-y-directorios-en-bash)

## 1. Navegar el sistema de archivos con Bash

En esta sección se describirá cómo cambiar el directorio de trabajo usando el comando `cd`. Así mismo, se va a explicar como mostrar en una lista los contenidos de un directorio usando el comando `ls`. Por último se observará cómo funcionan las opciones `-l` y `-a`  del comando `ls`.

Para empezar, se debe abrir la aplicación Terminal. A través de esta aplicación el usuario interactua con Bash en macOS. Al abrir la Terminal por primera vez, Bash se ubicará en el directorio _home_ del usuario. Usted puede ver en donde se encuentra en Bash mirando la información que se despliega en la Terminal como se muestra a continuación:

    user:~ username$

El carácter de la virgulilla `~` es un carácter especial que representa el directorio _home_ del usuario actual. Para ver la ruta completa del directorio se utiliza el comando `pwd` que significa _print working directory_. El comando `pwd` imprime la ruta absoluta del actual directorio de trabajo:

    user:~ username$ pwd
    /Users/username


Si se quiere cambiar el directorio de trabajo actual se usa el comando `cd` que significa _change directory_. Imáginemos que se va a navegar al directorio `repos/suabochica`. A medida que escribe la ruta, puede presionar la tecla `Tab` para auto completar la ruta. Una vez cambiado el directorio, se puede ver que la porción después del carácter dos puntos (`:`) cambió para mostrar el nuevo directorio al que se navegó.

    user:~ username$ cd repos/suabochica
    user:suabochica username$

Ahora que esta en el directorio `suabochica` vamos a ver que otros archivos y carpetas se tienen en este directorio. Para hacer eso se ejecuta el comando `ls` que significa _list_. Asumimos que el directorio `suabochica` tiene dos archivos: `index.js` y `index.html`

    user:suabochica username$ ls
    index.html        index.js

Los archivos del directorio `suabochica` se muestran en una lista plana, y no se da mucha información de dichos archivos. Algo que podemos hacer es usar la opción `-l` del comando `ls` para mostrar un listado largo con más información de los contenidos de la carpeta actual:

    user:suabochica username$ ls -l
    -rw-r--r-- 1 username staff 0 Feb 19  20:23 index.html
    -rw-r--r-- 1 username staff 21 Feb 19  20:23 index.js

Esta información se lee de la siguiente manera:

+ `-rw-r--r--`: Son  los permisos que existen sobre el archivo
+ `username`: Es el usuario propietario del archivo
+ `staff`: Grupo propietario del archivo
+ `Feb 19`: Fecha de creación o de última modificación
+ `index.html`: Nombre del archivo

Generalmente, el sistema de archivos oculta algunas carpetas y algunos archivos. Para ver los archivos y los directorios escondidos, se usa la opción `-a` que significa _all_. Supongamos que `suabochica` es un repositorio `git`, y por tanto debe tener un directorio `/.git` oculto. El siguiente comando muestra el directorio escondido:

    user:suabochica username$ ls -la
    drwxr-xr-x 5 username staff 160 Feb 19  20:36 .
    drwxr-xr-x 3 username staff 96 Feb 19  20:36 .
    drwxr-xr-x 9 username staff 288 Feb 19  20:36 .git
    -rw-r--r-- 1 username staff 0 Feb 19  20:23 index.html
    -rw-r--r-- 1 username staff 21 Feb 19  20:23 index.js

En la tercera línea de la la salida del comando `ls -la` se puede ver la carpeta `/.git`. Un archivo y un directorio se diferencian por que los directorios tienen una `d` al principio de la información que indican los permisos. Esta salida también nos muestra dos directorios: `/.` y `/..`. El directorio `/.` corresponde al directorio de trabajo actual, y el directorio `/..` corresponde al directorio padre de la carpeta actual en la que se encuentra el usuario. Estas son carpetas especiales configuradas por el sistema operativo y el sistema de archivos.

En consecuencia, si usamos el comando `cd` sobre el directorio `/..` se va a regresar a la carpeta `repos`.

## 2. Ver archivos y directorios en Bash

Algunas veces cuando se trabaja desde la línea de comandos, puede ser útil ver los contenidos de un archivo directamente desde la terminal o abrir el archivo con determinada aplicación. En esta sección se explicará como ver los archivos desde la terminal usando los comandos `cat` y `less`. Para abrir los archivos y directorios se usará el comando `open`.

Tomaremos como referencia la estructura de archivos de un repositorio `npm` para indicar los propositos de los comandos `cat` y `less`. Estos repositorios suelen tener un archivo `package.json` en donde se listan todos los paquetes consumidos por la aplicación.

Una herramienta útil para revisar de manera rápida los contenidos de un archivo es `cat`. Se puede usar el comando `cat` sobre el archivo `package.json` de la siguiente manera:

  user:suabochica-npm username$ cat package.json

Este comando descarga el contenido del archivo `package.json` directamente en nuestra terminal de Bash y lo imprime. Dependiendo de la aplicación web, el `package.json` puede ser un archivo largo. Cuando el archivo es largo, es necesario usar la barra de desplazamiento y navegar hacia arriba para ver los contenidos al inicio del archivo. Adicionalmente, se puede usar un el parámetro `-n` del comando `cat` y el nombre de archivo para mostrar el contenido del archivo junto con los número de línea:

    user:suabochica-npm username$ cat -n package.json

Desafortunadamente, el comando `cat` es incómodo para ver archivos grandes ya que resulta confuso navegar solo con la barra de desplazamiento.

El comando apropiado para ver el contenido de los archivos grandes es `less`:

    user:suabochica-npm username$ less package.json

Este comando introduce unos cambios en la interfaz de usuario de la terminal, ya que los comandos previos y el historial desde Bash desaparece y lo único que podemos observar es el contenido del archivo desde el inicio.

`less` nos da unas herramientas de navegación desde el teclado que son familiares para aquellos desarrolladores que usan el editor `vim`.

+ `Shift + G` para saltar a la última línea del archivo
+ `G` para ir a la primera línea del archivo
+ `/` para ejecutar búsquedas de palabras en el contenido del archivo
+ `Q` para salir de la visualización de `less` y regresar a la pantalla normal de Bash.

Por último, otra herramienta útil para examinar archivos y carpetas es el comando `open`. Este comando usa la aplicación externa que este configurada por defecto para ver los contenidos de los directorios y los archivos.

    user:suabochica-npm username$ open .

En macOS, el comando anterior abrirá el directorio actual `/.` con la interfaz de usuario de **Finder**. El comando `open` también puede abrir con **Finder** los directorios escondidos.

Del mismo modos, también podemos usar el comando `open` sobre archivos. En este caso, open abrirá el archivo con la aplicación por defecto asociada a abrir los archivos con esa extensión. En consecuencia, si se ejecuta:

    user:suabochica-npm username$ open lib/npm.js

El archivo JavaScript se abrirá con el editor de texto asociado (Visual Studio Code o Sublime).

## 3. Crear y borrar archivos y directorios en Bash
Una de las cosas mas útiles que se pueden automatizar con Bash es crear y borrar archivos y directorios. En esta sección se explicará el comando `touch` para crear un archivo, y como anexar el contenido con el comando `echo`. Posteriormente, se revisará como crear directorios con el comando `mkdir` y por último se enseñara como remover archivos y directorios.

Para crear un archivo en Bash, se usa el comando `touch` en conjunto con el nombre del archivo. Para este caso, el nombre del archivo sera `file.txt`. El siguiente comando creará el archivo `file.txt`:

    $ touch file.txt

> El signo pesos (`$`) se utiliza para ilustrar que la session en bash corresponde a un usuario normal

Al utilizar el comando `ls` para ver los contenidos del directorio actual, observamos la existencia del archivo `file.txt`.

    $ ls
    file.txt

En este punto, el archivo `file.txt` estará vacío. Para agregar contenido dentro de este archivo se puede utilizar el comando `echo`.  `echo` es una especie de `console.log` en Bash. Si corremos el comando `echo 'hi'` se imprimirá el string `hi`

    $ echo 'hi'
    hi

Si se usa el comando `echo 'hi'` junto con el operador _right angle bracket_ (`>`) y también se especifica el nombre del archivo `file.txt`, la cadena de caracteres `'hi'` se redirigirá dentro del archivo `file.txt`

    $ echo 'hi' > file.txt
    $ cat file.txt
    hi

Si se ejecuta el mismo comando pero esta vez se cambia el contenido de la cadena de caracteres por `'hi again'` y se imprime el contenido con `cat` obtenemos:

    $ echo 'hi again' > file.txt
    $ cat file.txt
    hi again

Si se hace este proceso varias veces, el contenido del archivo `file.txt` será sobreescrito.

Si se quiere anexar contenido al archivo, por ejemplo la cadena de caracteres `hello world` se deben usar dos operadores _right angle bracket_ (`>>`):

    $ echo 'hello world' >> file.txt
    $ cat file.txt
    hi again
    hello world

El comando anterior anexa el contenido al final del archivo. Por otra parte, mientras `touch` es una forma común de crear inicialmente un archivo vacío, se puede utilizar un atajo para crear un archivo con contenido a través del comando `echo`. Para ello, se usa el comando `echo 'hola'`, el nombre del archivo que se va a crear y el operador _right angle bracket_:

    $ echo 'hola' > file2.txt
    $ cat file2.txt
    hola

Para hacer un directorio en Bash, se usa el comando `mkdir`. Solo es necesario pasar el nombre del directorio, en este caso, `folder`.

    $ mkdir folder

Al revisar los contenidos de nuestro directorio actual se observa lo siguiente:

    $ ls
    file.txt  file2.txt   folder

Se tienen los dos archivos que se crearon anteriormente y el directorio que se acabo de crear. Ahora se quiere crear varios directorios anidados al mismo tiempo. Para hacerlo, es necesario usar el indicador `-p` y así crear cada uno de esos directorios intermedios según sea necesario. De lo contrario, el comando `mkdir a/b/c` no permitirá la creación de las carpetas, ya que de forma predeterminada el comando `mkdir` espera una ruta de archivo hasta el final. Es decir, espera que los directorios `/a` y `/b` estén creados para llegar a `/c`.

    $ mkdir -p a/b/c
    $ ls
    a file.txt    file2.txt   folder
    $ ls a/b
    c

Para remover un archivo se usa el comando `rm`.  Continuando con el ejemplo anterior, vamos a remover el archivo `file.txt` y luego imprimimos los contenidos del directorio:

    $ rm file.txt
    $ ls
    a file2.txt   folder

Observe que el comando `rm` borra el archivo de manera permanente. El archivo no se mueve a papelera de reciclaje o algo por el estilo.

Por defecto el `rm` solo borra archivos. Para remover directorios es necesario la opción `-r` que significa _recursive_. Al pasar el indicador `-r` el comando removerá recursivamente el directorio y todo lo que contiene.

    $ rm -r folder
    $ ls
    a file2.txt

A menudo, se utiliza en conjunto el indicador `-r` y el indicador `-f`. La opción `-f` es una especie de opción nuclear e impide que Bash solicite confirmación del usuario para eliminar un archivo, así como el error de salida si no existe un archivo o directorio. Al ejecutar este comando sobre el directorio `/a`, se eliminará la carpeta y todos sus contenidos también.

    $ rm -rf a
    $ ls
    file2.txt

## 4. Mover y copiar archivos y directorios con Bash

En esta sección se explicará como mover y renombrar archivos con el comando `mv` y como copiarlos con el el comando `cp`.

El comando `mv` permite mover archivos y directorios. Imaginemos una carpeta que tiene un archivo JS (`index.js`) y un directorio `src`. El directorio `src` esta vacío.

Ahora se quiere mover el archivo `index.js` dentro de la carpeta `src`. Para lograrlo, solo es necesario pasar el archivo que se desea mover y luego la ruta destino. Es posible pasar el nombre completo del archivo en la ruta destino

    $ mv index.js src/index.js

Si el propósito es cambiar el nombre del archivo, se debe pasar el archivo original y el nuevo nombre del archivo como segundo parámetro. Por ejemplo, vamos a cambiar el nombre del archivo `a.js`   por `b.js`. El archivo `a.js` fue creado con el comando `touch a.js`. Para lograrlo se corre el siguiente comando:

    $ mv a.js b.js
    $ ls
    b.js  src

En esta carpeta se tiene también el directorio `src`. Ahora se quiere renombrar el directorio `src` por `lib`. Para hacerlo se ejecuta el siguiente comando:

    $ mv /src lib
    $ ls
    b.js  lib

Para concluir el tema de mover directorios, el último ejemplo consiste en mover todo el contenido de `lib` dentro del directorio `src`. Para recrear la carpeta `src` ejecutamos `mkdir src`, y ahora se va usar el asterisco (`*`) para agarrar todo los contenidos de `lib` (archivo y carpetas) y ponerlos en `src`. El comando para lograrlo es:

    $ mv lib/* src/

Se revisan los contenidos y se obtiene:

    $ ls lib/ (empty)
    $ ls src/
    index.js

Para copiar un archivo se usa el comando `cp`. Para ilustrar el comando se va a crear un archivo `README.md`. Este archivo se va a copiar y dicha copia se pondrá en el directorio `src`.

    % echo "hello" > README.md
    $ cp README.md src/README.md

Al copiar un archivo, también es posible cambiar el nombre de la copia si se desea. Por ahora se va a mantener el nombre del archivo original. Aún así, es necesario escribir el nombre del archivo en el segundo parámetro.

Si se imprimen los contenidos del directorio `src` se van a tener los mismos contenidos:

    $ ls src/
    README.md index.js
    $ car src/README.md
    hello

Finalmente si se quiere copiar todos los contenidos (archivos, carpetas y sub-carpetas) dentro de otro directorio, se debe usar el comando `cp` utilizando la opción `-R`, que significa _recursive_. El ejemplo para mostrar el uso recursivo será el hecho de copiar todo el contenido dentro del directorio `src`  dentro de `lib`. Nuevamente se usa el asterisco (`*`), que es un comodín en Bash que significa _todo_ y será una ayuda para lograr el cometido:

    $ cp -R src/* lib/
    $ ls lib/
    README.md index.js
    $ ls src/
    README.md     index.js

## Recapitulación
En este escrito se han explicado los comandos básicos de bash para navegar el sistema de archivos, ver los contenidos de los archivos y las carpetas, crear, borrar, mover y copiar archivos y carpetas. En la siguiente publicación se tratarán los comando avanzados `grep`, para hacer búsqueda de texto en archivos y `curl`, para hacer peticiones HTTP desde Bash.
