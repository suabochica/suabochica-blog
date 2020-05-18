+++
title = "El Ciclo Solicitud/Respuesta de HTTP"
date = 2018-12-19T02:13:50Z
author = "Sergio L. Benítez D."
description = "Qué es una solicitud HTTP, obtener una y multiples solicitudes HTTP. Enviar datos con una petición POST HTTP"
+++

En esto documento se revisarán los siguientes contenidos:

1. [Solicitudes HTTP](#solicitudes-http)
2. [Obteniendo una sola solicitud](#obteniendo-una-sola-solicitud)
3. [Obteniendo múltiples solicitudes](#obteniendo-múltiples-solicitudes)
4. [Enviando datos con una solicitud POST](#enviando-datos-con-una-solicitud-post)
5. [De XHR a Fetch](#de-xhr-a-fetch)

## Solicitudes HTTP

El Internet ha existido por mas tiempo que la web. Mientras que los computadores ya tenían formas para comunicarse sobre el Internet como el correo electrónico o FTP (_File Transfer Protocol_), No había existido una manera común y pública para publicar y acceder a documentos.

Bajo este contexto es que **Tim Berners-Lee** entra en acción. El quería abrir un mecanismo para que los investigadores publicaran, leyeran y comentaran sus artículos a través de Internet. Para ello, creó documentos de hipertexto bajo un lenguaje de marcado que llamó HTML (_Hypertext Markup Language_). Luego, Berners-Lee diseño HTTP (_Hypertext Transfer Protocol_) para transferir los documentos HTML.  

El hipertexto suena como un termino complejo y futurista, pero simplemente significa que los textos en el documento pueden tener referencias a otros documentos. Estas referencias se llaman _enlaces_.

Al obtener un documento, el usuario no solo puede leer el documento, también puede navegar a documentos relacionados a través de los enlaces. Especialmente en el contexto científico para el cual Berners-Lee diseño este ecosistema, la característica del hipertexto resulto ser increíblemente útil. Sin embargo, el hipertexto puede referirse a más que texto. Los archivos se pueden enlazar a imágenes, códigos, estilos o cualquier otra cosa, 

Ahora, _¿Cómo funciona HTTP?_.  Una analogía válida para comprender el funcionamiento de HTTP es un sistema de formularios en hojas de papel diligenciados por un emisor y un remitente que se comunican a través de una conexión de tubos.

En su versión original, llamada HTTP/0.9, el protocolo constaba de una plantilla para realizar una solicitud de un documento en un servidor. En dicha plantilla se especificaba la acción que se iba a ejecutar en el servidor, es decir, si el documento se iba a almacenar o si se iba a recuperar, el lugar del servidor, el nombre del documento e información adicional que se considerara necesaria. Posteriormente se entrega la petición al servidor.

Como el servidor entiende HTTP, este sabe que es lo que desea el cliente. El protocolo también consta de unas plantillas para las respuestas. Estas plantillas son más versátiles ya que una sola solicitud puede tener una variedad de resultados: errores, formularios ilegibles, o un redireccionamiento a un servidor diferente. Cuando el servidor encuentra una respuesta llena un formulario en el que notifica al cliente quién es el encargado de responder su solicitud. 

## Obteniendo una sola solicitud

Retomando la analogía, el sistema de tubos y los formatos son básicamente el concepto subyacente de HTTP: **solicitud y respuesta**. En vez de estar transportando el papel por tubos, el protocolo transporta datos a través de cables de red.

Así es como luce una _solicitud HTTP_:

```
GET /pictures/foo.jpg HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0
Connection: keep-alive
Accept: text/html
If-None-Match: b2arb0a1r6a
```
Este es el texto exacto que el navegador envía al servidor para solicitar una imagen llamada `foo.jpg`

Por el momento vamos a descomponer la primera línea de esta petición:
+ `GET`:  Corresponde al verbo HTTP. El protocolo también es capaz ejecutar otras acciones y este tema sera evaluado en la sección de REST APIs. Para este ejemplo, el verbo `GET` especifica que el navegador quiere que el servidor le envíe datos.
+ `/pictures/foo.jpg`: Correspond a la ruta y el nombre del documento que queremos obtener. Aquí se fija que el usuario nos envíe un archivo llamado `foo.jpg` que esta localizado en la carpeta `/pictures`.
+ `HTTP/1.1` Corresponde a la versión del protocolo que se esta usando. Hoy en día HTTP/1.1 es la versión mas común y ampliamente soportada. Sin embargo, se esta haciendo una transición a la version HTTP/2 de la cual hablaremos en su respectiva sección.

Esta parte de la solicitud:

```
Host: www.google.com
User-Agent: Mozilla/5.0
Connection: keep-alive
Accept: text/html
If-None-Match: b2arb0a1r6a
```

Se llamada la sección de encabezado, y por ende contiene los encabezados. Los encabezados son datos adicionales sobre la misma solicitud. La mayoría de estos encabezados están estandarizados y todos son opcionales menos el encabezado `Host.`. En consecuencia la solicitud HTTP más pequeña que se quiera realizar debe tener la siguiente forma:

```
GET /pictures/foo.jpg HTTP/1.1
Host: www.google.com
```

La _respuesta HTTP_ es similar:

```
HTTP/1.1 200 OK
Content-Length: 16824
Server: Apache
Content-type: text/html
Date: Wed, 06, Oct, 2018
Etag: b2arb0a1r6a

<binart data>
```

Y su más grande diferencia con respecto a una solicitud HTTP es la primera línea, ya que aquí se puede encontrar un código de estado (`200` ) de la respuesta que nos indica si la solicitud se cumplió satisfactoriamente, si el documento no se encontró o si el servidor va a redirigir la petición a algún otro sitio.

La respuesta HTTP también tiene una sección de encabezados que no solo contiene datos sobre el documento si no que también tiene datos del servidor y la conexión. Nuevamente, estos encabezados son opcionales y el único encabezado obligatorio es `Content-Length` para mostrar al cliente cuantos bytes de datos debe esperar en la respuesta.

Por último, `<binary data>` corresponde al documento real que se va a enviar al cliente. Puede ser una imagen JPEG, un documento HTML o lo que sea que usted quiera transferir al usuario.

## Obteniendo múltiples solicitudes
En la anterior sección se explico como ir a buscar un solo documento a través de una solicitud HTTP. Estos documento pueden ser cualquier tipo de dato, pero por convención en la web todo comienza con un documento índice (_index document_).

El documento índice es lo que el servidor le enviará de vuelta a la si no se define ningún archivo explícito en la solicitud HTTP.

Al escribir una URL (_Uniform Resource Locator_) en la barra de búsqueda del navegador, el usuario le esta dando la instrucción al navegador de abrir una nueva conexión con el servidor identificado con el _hostname_ de la URL, y así obtener el documento especificado en la ruta de dicha URL.

En la mayoría de los casos el servidor responde con un archivo `index.html`  y el navegador empieza a analizarlo. Este paso es muy interesante ya que el navegador ejecuta una serie de pasos para poder manejar los datos recibidos.

Después de que el navegador termine de analizar la respuesta del servidor las cosas empiezan a ponerse un poco rudas, puesto que a medida que el navegador esta leyendo el archivo `index.html`, este archivo probablemente va a tener referencias a otros documento que se necesiten para mostrar el sitio web.  Estos documentos pueden ser imágenes, hojas de estilos, scripts, entre otros.

Para cada uno de estos recursos se envía otra solicitud y una vez que el navegador recibe la respuesta, se repite todo este proceso de análisis y el envío de nuevas solicitudes. Esto también significa que cada uno de estos recursos puede a su vez atraer recursos adicionales. Cada página de destino tendrá su propio conjunto de dependencias como imágenes, CSS y archivos JavaScript.

En conclusión, una sola URL puede enviar muchas peticiones de las que usted se podría dar por enterado.

## Enviando datos con una solicitud POST
Hasta ahora, solo hemos hecho solicitudes al servidor para que nos envíe datos a través del método `GET`.

No obstante, en ocasiones se desea que el usuario escriba algunos datos o cargue una imagen que será enviada al servidor. En este tipo de escenarios es donde el método `POST` entra en el juego

Con una solicitud `POST`, la petición en sí también puede tener una carga útil similar a la que se ha mostrado en las respuestas HTTP. Lo que pasa con esos datos que son enviados al servidor a través del método `POST` es responsabilidad del desarrollador back-end y no sera tratado en esta publicación.

Sin embargo, es importante saber que las solicitudes `POST` son manejadas de manera diferente a las solicitudes `GET` por los proxies y los navegadores.

Alguna vez se ha encontrado con el siguiente mensaje:

```
Confirm Form Rebsumission
The page that you are looking for used information that you entered. Returning to that page might cause any action you took to be repeated. Do you want to continue?
```

Esto es el resultado de una solicitud `POST`. Si se intenta volver a cargar dicho sitio web, el navegador le pedirá que confirme esta acción de recarga, ya que las solicitudes `POST` pueden ser operaciones destructivas y la repeticiones de estas operaciones pueden llegar a ser más destructiva de lo que originalmente pretendía.

Es por esta razón que se considera una buena práctica de los desarrolladores back-end responder las solicitudes `POST` con una redirección y no con un sitio web para evitar este tipo de comportamientos. Para el usuario, esta redirección es invisible, pero evita el aviso de recarga.

## De XHR a Fetch
El navegador hace un montón de trabajo pesado para nosotros. Pero, como es frecuente en la vida, nosotros no siempre sabemos los que queremos desde un principio.

Imagine un sitio web que quiere mostrar una imagen del clima actual. Nosotros no sabemos que clima va a hacer cuando el usuario visite el sitio web en el futuro. Por supuesto, podríamos tener cargadas todas las imágenes que posiblemente podría necesitar desde el principio, pero a simple vista esta solución no es práctica.

Por tanto, es tiempo de hablar de **AJAX** (_Asynchronous JavaScript And XML_). AJAX es un grupo de tecnologías web que permiten hacer solicitudes programáticamente con JavaScript en lugar de navegar y recargar de manera efectiva todo el sitio web.

XHR (versión corta de _XMLHttpRequest _), es la forma más popular para hacer este tipo de peticiones. Sin embargo, la API de XHR es confuso y esta obsoleta en comparación con lo que JavaScript ofrece hoy en día. A continuación, se muestra un código que realiza una petición AJAX con XHR.

```javascript
var xhr = new XMLHttpRequest();

xhr.open('GET', '/animals/cat.json', true):
xhr.onreadystatechange = function(e) {
if (this.readyState === 4 && this.status === 200) {
  console.log(this.responseText)
}
};

xhr.send()
```
Por estas razones vamos a saltar la explicación de XHR y vamos a enfocar el siguiente párrafo en su sucesor, Fetch.

**Fetch** hace exactamente la misma cosa que XHR pero con una API más limpia que utiliza promesas, y por lo tanto, se integra mucho mejor con el resto de las API modernas de JavaScript. Con Fetch, usted puede utilizar todos los métodos HTTP que especifica el protocolo, y se tiene control sobre casi todos los encabezados de las solicitudes HTTP. El siguiente código es una solicitud AJAX que usa Fetch.

```javascript
fetch("/animals/cat.json")
.then(response => response.text())
.then(body => console.log(body));
```
## Summary
En este documento revisamos el ciclo solicitud/respuesta de HTTP y por ende hay un entendimiento sólido sobre como HTTP funciona y que es lo que realmente hace el navegador cuando un usuario visita un sitio web. Así mismo, hay conocimiento sobre como los metadatos (encabezados) son adjuntados en las solicitudes y las respuestas, y como solicitar documentos adicionales usando JavaScript.

En la siguiente publicación de la serie, vamos a revisar de manera detallada el protocolo HTTP y los bytes reales que se envían entres el servidor y el navegador.