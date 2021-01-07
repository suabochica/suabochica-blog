+++
title = "Características de Bash"
date = 2018-11-01T02:13:50Z
author = "Sergio L. Benítez D."
description = "Encadenamiento de comando, redireccionamiento de contexto input/output y creación y ejecución de bash scripts"
tags = [
    "ops",
    "bash",
]
+++

### Índice
1. [Encadenamiento de comandos con Pipes (`|`)](#encadenamiento-de-comandos-con-pipes-)
2. [Redireccionamiento I/O](#redireccionamiento-io)
3. [Crear y ejecutar scripts de Bash](#crear-y-ejecutar-scripts-de-bash)

## Encadenamiento de comandos con Pipes (`|`)

Pipe (`|`) es una de las características más poderosas en Bash ya que permite encadenar de manera eficiente una serie de comandos y enviar datos a través de ellos.

Los pipes (una posible traducción al español sería tuberías o canalizaciones) son utilizados para dirigir la salida de un comando hacia la entrada de otro. Veamos que significa esto. Por ejemplo, para revisar los procesos que están corriendo en mi computador, yo puedo correr el comando `ps ax` y este arroja una tabla como la siguiente:

    | PID | TT | STAT | TIME    | COMMAND           |
    |-----|----|------|---------|-------------------|
    | 1   | ?? | Ss   | 0:09.87 | /sbin/lauchd      |
    | 57  | ?? | Ss   | 0:00.55 | /usr/sbin/syslogd |
    | ... | .. | ...  | ...     | ...               |

En esta tabla cada una de las filas representa un proceso que se esta corriendo en el computador. Si deseamos ver todos los procesos de Chrome que están corriendo actualmente, podemos combinar el  comando `ps ax` con `grep` de la siguiente forma.

Primero ejecutamos el comando `ps ax` y luego canalizamos el comando `grep`. Normalmente, el comando `grep` se ejecuta sobre un archivo, pero cuando lo canalizamos, básicamente estamos diciendo que tome la salida del comando anterior, y la pase como entrada del siguiente comando.

    $ ps ax | grep Chrome

Al correr este comando solo se van a mostrar los procesos de Chrome que se están corriendo. Esta salida es ilegible, por tanto podemos ir más allá y canalizar nuestra instrucción nuevamente con el comando `less` y así visualizar la salida de una manera diferente:

    $ ps ax | grep Chrome | less

Otro buen ejemplo de pipes es cuando queremos ver el tamaño de un archivo luego de ser parseado con `uglifyjs` y comprimido con `gzip`

Luego de tener instalado a nivel global el comando `uglifyjs` a través de `npm` , podemos utilizar esta librería para crear la siguiente instrucción:

    $ uglifyjs -c -m -- index.js

Las banderas `-c` y `-m` significan _compress_ y _mangle_ respectivamente. Al final del comando pasamos el archivo que queremos comprimir. En este caso `index.js`. A partir de ahí, vamos a canalizar esta salida con `gzip`. Vamos a pasar el indicador `-9` y así decirle a `gzip` que sea utilizado con la configuración para hacer la compresión máxima.

    $ uglifyjs -c -m -- index.js | gzip -9

Por último, necesitamos saber el tamaño del archivo que generaron estos dos comandos. Por tanto, requerimos canalizar el comando `wc` .

    $ uglifyjs -c -m -- index.js | gzip -9 | wc

`wc` significa _word count_. Vamos a pasar la bandera `-c` para decirle al comando que en vez de darnos el reconteo de palabras nos devuelva el conteo de sus entradas.

    $ uglifyjs -c -m -- index.js | gzip -9 | wc -c

Al correr esta instrucción obtenemos que el archivo `index.js` comprimido tiene un peso de 280 bytes. ¡Genial!

## Redireccionamiento I/O

En esta lección veremos como redireccionar la salida de nuestros comandos usando el operador stdout (`>`) . Este operador nos dejar tomar una salida de un comando y enviársela a un archivo. Es posible adjuntar un archivo utilizando (`>>`).

Al correr un comando como `ls` , su salida es mostrada en la Terminal. La Terminal es la salida estandár. Si nosotros quisiéramos redirigir esta salida a un archivo, debemos usar el operador de soporte angular (`>`) y damos un nombre al archivo de salida como `ls.txt`

    $ ls > ls.txt

Al ejecutar esta instrucción, la salida sera capturada y direccionada dentro de un archivo en vez de mostrar el resultado en la Terminal.

Si el archivo `ls.txt` no existe, el comando lo crea sobre la marcha por nosotros. Al ejecutar el comando `cat` sobre el archivo `ls.txt` podemos ver que su contenido corresponde a los archivos y directorios que se encuentran dentro de la carpeta actual.

Por otra parte, es posible agregar contenido a un archivo. Cuando usamos el comando `echo "hi"` la salida se muestra en la Terminal. En vez de utilizar la salida estándar podemos dirigir este contenido y agregarlo al archivo `ls.txt`:

    $ echo "hi" >> ls.txt

Por tanto al usar el comando `cat` sobre el archivo `ls.txt` obtenemos:

    $ cat ls.txt
    index.js
    ls.txt
    hi

Existen muchas posibilidades con el redireccionamiento de entradas y salidas. Es recomendable revisar la [documentación oficial de Bash](https://www.tldp.org/LDP/abs/html/io-redirection.html) para más detalles.

## Crear y ejecutar scripts de Bash
En esta sección aprenderemos a encapsular cierta lógica que queremos ejecutar más adelante en u script. Después de crear un script, tenemos que usar `chmod u+x` para darle permisos de ejecución. Si deseamos que nuestro script haga uso de parámetros que deben ser suministrado al momento de invocarlo, es recomendable usar la sintaxis `$1` y `$2`, también conocidos como parámetros posicionales.

También vamos a aprender sobre la variable de entorno `$PATH` y así mover nuestro script a una carpeta ya almacenada dentro de `$PATH` para que sea ejecutado in cualquier directorio de nuestro shell.

Empecemos creando nuestro script de Bash. Vamos a llamarlo `script.sh`. Para crearlo utilizamos el comando `touch`

    $ touch script.sh

Agreguemos contenido en nuestro archivo. Para ello debemos utilizar nuestro editor de texto de preferencia. El que yo recomiendo es `vim`. 

Agregamos el siguiente contenido a nuestro script: `echo Hello World`. Para correr un script de bash digitamos `./` y el nombre del script, el cual es `script.sh`

    $ ./script.sh

Al ejecutar el script obtenemos el siguiente aviso:

    $ ~bash: /.script.sh: Permission denied

Este comportamiento es esperado, y para entenderlo podemos correr `ls -l` el cual nos indicará la siguiente información:

    $ ls -l
    ...
    -rw-r--r-- 1 suabochica staff 01 Oct 18 11:21 script.sh

Como podemos ver, nuestro usuario tiene permisos para leer y es escribir sobre el archivo `script.sh`, pero no tiene permisos para ejecutarlo.

Al crear un archivo se le asigna una configuración de permisos por defecto. Para arreglar el problema,  vamos a correr un comando llamado _change mode_ `chmod`. Vamos a usar la expresión `u+x` para indicar que vamos a agregar permisos de ejecución al usuario y luego pasamos el archivo.

    $ chmod u+x script.sh

Para validar que los permisos sobre el archivo se ha modificado volvemos a imprimir una lista larga y podremos observar que ahora hay una `x` en la salida la cual indica los permisos de ejecución:

    $ ls -l
    ...
    -rwx-r--r-- 1 suabochica staff 01 Oct 18 11:22 script.sh

Ahora al correr nuestro script obtenemos:

    $ ./script.sh
    Hello World

¡Funciona!. Ahora vamos a modificar nuestro script para que acepte parámetros en línea de comando. Para ello es necesario revisar como funcionan las variables en Bash. Para referencia una variable en Bash se utiliza el signo de pesos `$` y luego el nombre de la variable.

Nuestro ejemplo consiste en hacer un saludo personalizable en donde tendremos que hacer una referencia a una variable.

En Bash, se pasan parámetros a los scripts de la misma forma que se pasan parámetros a un comando, con una lista separada por espacios después del nombre del comando.

Vamos a referenciar cada uno de los parámetros que vamos a pasar en nuestro script usando el argumento de número. Para obtener el primer argumento que vamos a posicionar en nuestro script usamos `$1`. Modificamos el archivo`script.sh`  para que tenga el siguiente contenido

    echo "$1 Work"

Guardamos esta modificación y ejecutamos el script con un parámetro.

    $ ./script.sh "Fellas"
    Fellas World

¡Funciona!. Puede pasar tantos parámetros a sus scripts como desee y todos serán referenciados por el argumento de número que son.

Vamos a crear un script para un contexto más realista, en donde al ejecutar el script se cree toda la estructura de archivos para un nuevo proyecto en JavaScript. El script se va a llamar `init-js`.

    $ touch init-js.sh

En la primera línea del script vamos a indicar el siguiente mensaje de log `echo "Initializing JS project at"`. Vamos a pasar como parámetro el directorio actual de trabajo desde donde se va a ejecutar el script.

Para hacer eso utilizamos una variable que va a almacenar la salida del comando `pwd` en nuestro Bash shell. Si utilizamos el signo pesos y ponemos entre paréntesis el comando estamos diciendo, "Ejecute este comando y ponga el resultado en línea"

    echo "Initializing JS project at $(pwd)"

Ahora vamos a inicializar un repositorio git a través del comando `git init`. Luego vamos a crear nuestro archivo `package.json`  utilizando el comando `npm init -y`. La bandera `-y` le dice a `npm` que haga una configuración con todos los valores predeterminados y no solicite ninguna entrada.

De hecho, podemos dejar una nota en nuestro script para ilustrar la descripción sobre la creación del archivo  `package.json`. En Bash, podemos dejar comentarios utilizando el símbolo numeral (`#`). Por tanto el contenido de nuestro script estaría así>

    echo "Initializing JS project at $(pwd)"
    git init
    npm init # create package.json with all defaults

Por convención, es recomendable tener un directorio `src` en los proyectos, `mkdir src`

    echo "Initializing JS project at $(pwd)"
    git init
    npm init # create package.json with all defaults
    mkdir src

En Javascript, todo los proyecto parten desde un archivo `index.js` en el directorio `src`. Si usted tienen instalado Visual Studio Code, este editor instala un comando en la linea de comandos llamado `code`. Si usted corre `code` y pasa un directorio, este sera abierto con Visual Studio Code.

El directorio de trabajo actual es referenciado utilizando el carácter punto (`.`). Si usted quiere puede abrir el archivo `src/index.js` usando el archivo `open`. Así se abrirá el archivo `index. js` con la aplicación determinada para editar los archivos JavaScript.

    echo "Initializing JS project at $(pwd)"
    git init
    npm init # create package.json with all defaults
    mkdir src
    touch src/index.js
    code . # open src/index.js

Este script se ve muy bien. Guardemos su contenido y agreguemos permisos de ejecución a nuestro usuario para que pueda ejecutar el script:

    $ chmod u+x init-js.sh

Es tiempo de ejecutar nuestro script en la carpeta actual y veremos si funciona:

    $ ./inti-js.sh

Voila! el editor Visual Studio Code abre el archivo `index.js` de nuestro nuevo proyecto JavaScript. Esto esta bien, pero podríamos hacerlo mejor si el comando se corre en cualquier carpeta del sistema de archivos que el usuario desee.

En este momento, si queremos correr este script en una carpeta diferente, tenemos que copiar el script allí, ejecutarlo y probablemente eliminarlo después de usarlo. Para evitar este proceso tan tedioso podemos agregar scripts a nuestra variable de entorno `$PATH`.

Esto hará que nuestros scripts estén disponibles para ejecutarse en Bash desde cualquier lugar del sistema de archivo que queramos.

Pero primero, veamos qué es nuestra variable `$PATH`. Si ejecutamos `echo $PATH` vamos a obtener como salida una larga cadena de caracteres.

    /Users/sergiobenitezdiaz/.rbenv/shims:/Users/sergiobenitezdiaz/.nvm/versions/node/v8.9.1/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/opt/ImageMagick/bin:/usr/local/bin

Esto es básicamente una lista de carpetas que están separadas por dos puntos. En estás rutas el shell busca ejecutables o scripts.

Por ejemplo, si ejecutamos `which node`, el comando `which` nos va a indicar en donde esta el ejecutable de node.js.

    which node
    /Users/sergiobenitezdiaz/.nvm/versions/node/v8.9.1/bin/node

Se puede ver que node.js vive en la carpeta `.nvm`. Es este el lugar donde Bash busca el ejecutable para correr node.js.

Existen muchas formas de agregar cosas en la variable `$PATH`. La forma mas simple de agregar un ejecutable es copiando el script `index-js.sh` en la ruta `/usr/local/bin` , ya que es una ruta configurada por defecto en Bash para buscar ejecutables.

    $ cp init-js.sh /usr/local/bin/init-js

Al correr `which init-js`, se puede ver que el script vive en la carpeta  `usr/local/bin/init-js`

Momento de probar si nuestro script se puede ejecutar desde cualquier carpeta del sistema de archivos. Vamos a crear un directorio de prueba, utilizamos el comando `cd` para entrar a esta carpeta y ejecutamos:

    init-js

Se puede ver que el script se corrió y nuevamente se abrió el editor Visual Studio Code en el archivo `index.js` de nuestro proyecto JavaScript.
