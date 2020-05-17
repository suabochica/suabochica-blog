+++
title = "Comandos Avanzados en Bash"
date = 2018-10-03T02:13:50Z
author = "Sergio L. Benítez D."
description = "Comando grep para búsqueda de texto y comando curl para hacer peticiones HTTP con Bash"
+++

### Índice

1. [Búsquedas de texto con grep](#1-búsquedas-de-texto-con-grep)
2. [Hacer peticiones HTTP en Bash con curl](#2-hacer-peticiones-http-con-curl)

## 1. Búsquedas de texto con grep

En esta sección, se va a utilizar el comando `grep` para encontrar patrones de texto. Adicionalmente, se explicarán algunas opciones que el comando `grep` tiene las cuales lo hacen más poderoso y fácil de leer.

Para empezar, revisemos el siguiente ejemplo:

    $ grep "npm.config.get" lib/npm.js

Este comando recibe entre comillas (") el patrón de texto que se desea buscar y el archivo sobre el que se va a realizar la búsqueda. El comando va imprimir un resultado en donde cada línea es una coincidencia que se encontró en el archivo. Estas coincidencias no son necesariamente una secuencia, las líneas pueden corresponder a diferentes partes del archivo, es por eso que esta primera aproximación del comando `grep` no es tan asertiva.

Del mismo modo, podemos indicar la búsqueda en multiples archivos. Para hacer la búsqueda sobre cada archivo JavaScript de la carpeta padre `lib` se ejecuta:

    $ grep "npm.config.get" lib/**/*.js

La salida de este comando va a imprimir una buena cantidad de archivos. La salida es ligeramente diferente cuando se hace la búsqueda sobre múltiples archivos. Primero se muestra el nombre del archivo en donde se encontró la coincidencia y luego la línea con la coincidencia. Si hay varias coincidencias en uno de los archivos, la salida repite la impresión del nombre del archivo y el texto de búsqueda cuantas veces se presente la coincidencia en ese archivo.

Ahora es momento de empezar a utilizar algunas de las opciones de `grep` para facilitar la lectura de la búsqueda de texto. La primera opción que se va a utilizar es `--color` para colorear la coincidencia en el resultado:

    $ grep --color "npm.config.get" lib/npm.js

Otra opción útil es `-n` para indicar el número de la línea en donde la coincidencia fue encontrada. Este presentación resulta muy útil si posteriormente se quiere abrir el archivo en un editor de texto y saltar directo a la línea.

    $ grep --color -n "npm.config.get" lib/npm.js

Otra bandera eficiente es `-C` para mostrar el contexto al rededor de la coincidencia. Al usarlo junto con el indicador `1` usted puede ver la línea anterior y la línea posterior a la coincidencia.

    $ grep --color -n -C 1 "npm.config.get" lib/npm.js

El indicador puede modificarse al número que usted desee en caso de que necesite más contexto. El resultado de este comando muestra las coincidencias separadas por bloques

Una de las características más llamativas de `grep` es su soporte de expresiones regulares (**regex**). La opción que activa el soporte de regex es `-e`.  Para ilustrar el uso de regex vamos a usar la expresión regular `[gs]` para hacer una búsqueda de dos textos: `npm.config.get` y `npm.config.set` es el archivo `lib/npm.js`. Para ello:

    $ grep --color -n -e -C 1 "npm.config.[gs]et" lib/npm.js

`grep` tiene una tonelada de opciones. Para revisarlas con mayor detalle se puede correr el comando `man grep` y ver todas las opciones.

## 2. Hacer peticiones HTTP con curl

`curl` es como el usuario hace peticiones HTTP desde la línea de comando. Es eficiente para probar APIs porque le da al usuario control total sobre todos los encabezados que se envían, y no es necesario interactuar con una interfaz de usuario para enviar la petición.

En esta sección aprenderemos a hacer peticiones HTTP, cambiar el método HTTP, cómo establecer los encabezados y cómo enviar datos de formularios codificados en JSON. También veremos cómo aumentar la legibilidad de los resultados dividiendo solicitudes HTTP largas en varias líneas y dando un formato a las respuestas con `jsome`, un módulo de **nodejs**.

Para hacer una solicitud `GET` a un sitio web, tan solo usamos `curl` y luego pasamos URL como parámetro.

    $ curl https://example.com

Esta es la forma más básica de `curl`. Por defecto, se hace la petición `GET`, se toma el cuerpo de la respuesta y lo devuelve a su salida estándar. Ahora usemos el comando consumiendo el _API de Star Wars_.

    $ curl https://swapi.co/api/people/1/

Respuesta:

```
{"name":"Luke Skywalker","height":"172","mass":"77","hair_color":"blond","skin_color":"fair","eye_color":"blue","birth_year":"19BBY","gender":"male","homeworld":"https://swapi.co/api/planets/1/","films":["https://swapi.co/api/films/2/","https://swapi.co/api/films/6/","https://swapi.co/api/films/3/","https://swapi.co/api/films/1/","https://swapi.co/api/films/7/"],"species":["https://swapi.co/api/species/1/"],"vehicles":["https://swapi.co/api/vehicles/14/","https://swapi.co/api/vehicles/30/"],"starships":["https://swapi.co/api/starships/12/","https://swapi.co/api/starships/22/"],"created":"2014-12-09T13:50:51.644000Z","edited":"2014-12-20T21:17:56.891000Z","url":"https://swapi.co/api/people/1/"}
```

Algunas veces puede resultar útil inspeccionar los encabezados de la respuesta. Para hacer eso, vamos a pasar la opción `-i`, que significa _include_, y por ende nos va a incluir los encabezados de la petición HTTP. Si llamamos el API de Star Wars con esta bandera:

    $ curl -i https://swapi.co/api/people/2

Obtenemos:

```
HTTP/2 301
date: Mon, 17 Sep 2018 22:48:26 GMT
content-type: text/html; charset=utf-8
set-cookie: __cfduid=d412dc3c24eb8ea699693c0d36c0b7be91537224506; expires=Tue, 17-Sep-19 22:48:26 GMT; path=/; domain=.swapi.co; HttpOnly; Secure
location: https://swapi.co/api/people/2/
x-frame-options: SAMEORIGIN
via: 1.1 vegur
expect-ct: max-age=604800, report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"
server: cloudflare
cf-ray: 45bf1ececc6795b6-IAD
```

El `301` nos indica  que la petición HTTP al API esta tratando de redirigirnos. De forma predeterminada `curl` no sigue las redirecciones como usualmente lo hace un navegador.  Si hacemos la misma solicitud pero esta vez agregamos la barra inclinada `/`  al final del comando, la petición es éxitosa

    $ curl -i https://swapi.co/api/people/2/

Para habilitar el redireccionamiento en `curl` se puede pasar la opción `-L`. Esta opción le indicará al comando que siga los redireccionamientos. En ese orden de ideas, si ejecutamos:

    $ curl -iL https://swapi.co/api/people/2

Se observa que obtenemos el `301` y luego un `200` y la respuesta JSON que estamos esperando.

Es tiempo de saltar hacia las construcción de peticiones HTTP más complicadas. Frecuentemente, es necesario pasar un encabezado de autorización o un cookie para acceder a un recurso protegido. Para habilitar autorizaciones se usa la opción `-H`, que significa _header_.

Al ejecutar `curl -H` se debe garantizar que el contenido del encabezado este dentro de comillas dobles (`"`) y luego se pasa la URL a la que vamos a hacer la petición HTTP. Ese _endpoint_ falso esta esperando un encabezado de autorización y nuestro comando hará una petición `GET` y retornará alguna publicación falsa en de un blog.

    $ curl -H "Authorization: Bearer 123" localhost:3000/api-protected/posts

Hasta el momento solo se han hecho peticiones HTTP `GET`. Para cambiar el método se usa la opción `-X` y en este caso en particular vamos a hacer un `POST` de un contenido JSON.

Para ello se debe pasar un encabezado como `Content-Type: application/json`. Luego pasamos el cuerpo de la petición y se va a usar la bandera `-d` para indicar que posterior a esta bandera se va a definir la siguiente estructura JSON `{"title": "Bash", "author":"Sergio"}`. Por último se pasa la URL que va a recibir nuestra petición:

    $ curl -X POST "Content-Type: application/json" -d {"title": "Bash", "author":"Sergio"} http://localhost:3000/api/posts

Para verificar el éxito de nuestra petición `POST`, podemos hacer una petición `GET`:

    $ curl http://localhost:3000/api/posts
    {
        "title": "Bash",
        "author": "Sergio",
        "id": 1
    }

Esto indica que tenemos una nueva publicación en nuestro blog.

Publicar una URL en forma codificada es similar, ya que existe la opción `--data-urlencode` en la cual especificamos los parámetros de `title` y `author` para generar nuestra nueva publicación. El comando para utilizar esta bandera sería:

    $ curl -i -X POST --data-urlencode title="Bash again" --data-urlencode author="Sergio" localhost:3000/api/posts

Si el comando funciona, la respuesta HTTP será un 201. Al momento de construir comandos `curl` muy largos, es útil separar la petición visualmente en múltiples líneas. Vamos a hacer un solicitud `PUT` sobre una de las publicaciones que acabamos de hacer. Para hacer la separación de líneas simplemente colocamos el carácter de la barra diagonal inversa `\` y presionar enter. Esta acción nos da otra línea para completar nuestro comando:

    $ curl -i -X PUT \

Ahora, vamos a pasar un JSON a nuestro comando actual:

    $ curl -i -X PUT \
    > -d '{"title": "Bash changed"}' \

Es turno de añadir el encabezado de nuestro tipo de contenido:

    $ curl -i -X PUT \
    > -d '{"title": "Bash changed"}' \
    > -H "Content-Type: application/json"

Y por último pasamos la URL a la que vamos hacer la petición. En este ejemplo vamos a cambiar la segunda publicación creada. Por tanto nuestro comando será:

    $ curl -i -X PUT \
    > -d '{"title": "Bash changed"}' \
    > -H "Content-Type: application/json"
    > http://localhost:3000/api/posts/2

Si todo funciona nuestro API retornará un 200. Al tratar con respuestas HTTP grandes, puede ser útil guardar la respuesta en un archivo. Esto lo podemos hacer con la opción `-o`. Para ejemplificar el uso de esta bandera, vamos a utilizar `curl` incluyendo los encabezados de la respuesta, haciendo una redirección a `google.com`. Luego, usando la opción `-o`, vamos a guardar la respuesta en un archivo `google.txt`. El comando seria el siguiente:

    $ curl -iL https://google.com -o google.txt

La respuesta en la terminal será diferente ya que el resultado se guardo en un archivo. Para verificar esta respuesta, se debe inspeccionar el archivo `google.txt`:

    $ less google.txt

En este archivo observamos que tenemos la información de los encabezado, primero se recibió un `301` y luego un `200`. Por último esta la respuesta HTML desde `google.com`

Cuando se trata con respuestas JSON grandes, estas pueden ser ilegibles con el formato establecido por defecto por Bash. Si recordamos la respuesta que recibimos del API de Star Wars, nos damos cuenta de lo díficil que resulta su lectura.

Una cosa que podemos hacer es utilizar el operador pipe `|`  y canalizar la respuesta a otro comando. Para ilustrar el uso del pipe, es necesario instalar el módulo `jsome` a nivel global de **nodejs**:

    $ curl https://swapi.com/api/people/1/ | jsome

Al ejecutar este comando, la respuesta sera canalizada a `jsome`. Este modulo formatea los archivos JSON haciendo más fácil su lectura.
