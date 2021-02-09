+++
title = "Seguridad"
date = 2019-02-18T02:13:50Z
author = "Sergio L. Benítez D."
description = "Resticción de orígenes, anulación del same origin policy, CORS y fallos de seguridad"
tags = [
    "HTTP",
]
+++

Se estarán preguntando por que volver a hablar de seguridad si este tema ya fue trabajado en la explicación de `TLS` y su uso con `HTTP`.

`HTTPS` cubre muchos ángulos de ataque para ciertos escenarios como el espionaje de tráfico o suplantación de un servidor web. Sin embargo, la puerta de entrada más segura del mundo no valdrá de nada si hay una ventana abierta en el primer piso.

`HTTP`contiene un riesgo de seguridad muy sutil. Además, el historial del protocolo y la compatibilidad con sus versiones anteriores acumulan una serie de inconvenientes que no fueron tratados en los tiempos donde la seguridad no era un tema prioritario como.

En esta publicación aprenderemos cómo ciertas opciones de diseño podrían permitir que un atacante robe datos credenciales y cómo usted, como desarrollador web puede proteger una aplicación web de dichos ataques.

## Índice
1. [Orígenes](#orígenes)
2. [Anulando Same Origin Policy](#anulando-same-origin-policy)
3. [CORS](#cors)
4. [Fallo de seguridad: CSRF](#fallo-de-seguridad-csrf)
5. [Fallo de seguridad: XSS](#fallo-de-seguridad-xss)

## Orígenes

Como una regla general, a JavaScript no se le permite acceder a los datos de ningún otro origen que no sea el suyo. Un origen se componen de tres partes:

1. El esquema de datos
2. El nombre del host
3. El puerto

Para la página de esta publicación `http://www.suabochica.com:400` tenemos que:

1. `http://` es el esquema de datos
2. `www.suabochica.com` es el nombre del host
3. `400` es el puerto

Si se cambia cualquiera de estas partes se tiene un origen distinto y se aplicarán reglas diferentes. Además de los problemas de contenido mixto que mencionamos anteriormente, esta es otra razón para no mezclar URLs con esquemas de datos `HTTP` y `HTTPS` ya que serán orígenes diferentes.

Pero, ¿Cuáles son estas reglas que se aplican una vez que está trabajando en varios orígenes?

En primer lugar, no puede realizar solicitudes `GET` a otros orígenes. En realidad, bajo ciertos criterios se pueden hacer dichas peticiones pero nunca se va poder leer la respuesta.

En segundo lugar, no puede inspeccionar `<iframes>` o ventanas con JavaScript si son de un origen diferente.

Estas reglas tienen mucho sentido. Si se pudieran hacer solicitudes `GET` a otros orígenes, se podría configurar un sitio web personal para realizar peticiones `GET` a `https://facebook.com` y obtener todos los mensajes de facebook.

Definitivamente no queremos eso y esta restricción se llama **same origin policy** o restricción del mismo origen..

No obstante, recuerde que si hay reglas, entonces también hay excepciones para esas reglas.

Como bien sabe, se puede incluir hojas de estilo, imágenes, videos e incluso scripts de otros orígenes, así como enviar datos de formularios a otros orígenes. El usuario final no podrá decir que una imagen se carga desde nuestro servidor, mientras que  otro se carga desde Instagram. Sin embargo, para los desarrolladores web hay una diferencia.

Un desarrollador web no puede interactuar con una etiqueta de imagen que muestre una imagen de origen cruzado de la misma manera que podría hacerlo con una imagen del mismo origen. Por ejemplo, usted no puede inspeccionar los píxeles de una imagen que se encuentre dentro del elemento `<canvas>`. Lo mismo ocurre con una etiqueta `<script>` que incluye un recurso de script de origen cruzado.

En el último escenario, los contenidos del script de origen cruzado aparecerán vacíos, o en el caso de las API más modernas se arrojará explícitamente un error. En cambio, para un script en el mismo origen el acceso a sus contenidos están habilitados.

Es importante tener en cuenta que el navegador es el que aplica la _same origin policy_. No es el servidor, sino el cliente el que no le permitirá enviar solicitudes a orígenes diferente. Para evitar esta restricción se se pueden revisar las técnicas que se explicaran en la siguiente sección

## Anulando Same Origin Policy
A veces, la intención es permitir que otras personas accedan a sus recursos, incluso si son de otro origen. Esto es principalmente relevante para los proveedores de API que desean que otros sitios puedan usar el servicio, pero la misma política lo impide.

Actualmente, puede lograr fácilmente el intercambio de recursos en diferentes orígenes usando algunas técnicas. En esta sección compartiré tres: CORS, JSONP y Message Passing.

### CORS
El desarrollador web puede compartir recursos con un conjunto de direcciones `HTTP` llamado **CORS** (Cross Origin Resources Sharing). Esta es la solución de ingeniería más poderosa para la restricción de origen único. Pero hasta hace unos años, el soporte para CORS era bastante deficiente y la gente tenía que idear sus propias técnicas. Esta técnica sera evaluada detalladamente en la siguiente sección

### JSONP
Una de las técnicas más antiguas es **JSONP** (JSON Padding). En lugar de simplemente devolver datos , JSONP devuelve un script que contiene los datos. Esto explota el hecho de que los scripts de otros orígenes se ejecutarán y compartirán el entorno de ejecución con sus propios scripts

Las API basadas en JSONP esperan incluir el nombre de la función como un parámetro de consulta. El servidor devolverá un nuevo script que llama a la función que usted nombró.

Veamos un ejemplo ficticio para ilustrar JSONP. Digamos que estamos construyendo una aplicación en `mycourselist.com` que quiere enumerar todos los cursos de Udacity o Udemy en los que el usuario esta inscrito. El enfoque ingenuo sería realizar una solicitud `GET` al API de `udacity.com` y `udemy.com` y utilizar los datos de retorno para generar una lista para el usuario. Sin embargo esto fallará con la excepción de seguridad ya que el _host_  `mycourselist` difiere del host `udacity` y `udemy`.

¿Cómo se vería esta API si es compatible con JSONP? En la petición se agrega un nombre de función (para este ejemplo será `getCourses`) a la URL y se incluye con una etiqueta script de la siguiente manera:

    <script src="https://api.udacity.com/courses?status=enrolled&callback=getCourses">

El servidor envolverá todos los datos en una función con el mismo nombre que se proporciona en la URL como parámetro de consulta. El desarrollador debe definir esta función porque cuando regresan las respuestas, se ejecuta la llamada a la función y ahora así tiene acceso a los datos solicitados por la función que se envío como argumento. La función en el servidor sería:

    getCourses([course1, course2, ...])

### Message Passing
Otra técnica que fue diseñada explícitamente para permitir comunicación de origen cruzado se llama **message passing**. `postMessage()` es una función que se puede llamara para pasar un mensaje a otras ventana es iframes , incluso si provienen de un origen diferente. Estro crea un evento de mensaje al que se subscribe como cualquier otro evento. Por seguridad, el receptor puede inspeccionar el origen y el contenido del mensaje.

Si bien `postMessage()` es mucho más limpio y permite un control más granular que las otras opciones de origen cruzado, lamentablemente no ha sido adoptado por los proveedores de API.

## CORS
Vamos a entrar en detalle con la técnica CORS, ya que ha sido adoptada por los proveedores de APIs como la primera instancia para compartir recursos.

Los encabezados CORS (`Referrer` desde el cliente y `Access-Control-Allow-Origin` en el servidor)  permiten hacer peticiones a otros origines sin depender de JavaScript, aunque sí necesitan algún código desde el lado del servidor. CORS habilita un servicio para especificar un conjunto de orígenes que tienen permiso para acceder a sus recursos. Si el encabezado de referencia de la solicitud está en esa lista, podrá inspeccionar la respuesta y utilizar los datos. ¡Problema resuelto!.

Sin embargo, al observar detenidamente se dará cuenta de que cuando el servidor envía los encabezados, la solicitud ya se habrá ejecutado. Esto puede ser problemático con las operaciones destructivas porque ya es demasiado tarde para ignorar la petición. Aquí es donde entran en juego las _preflight requests_ o las solicitudes de verificación previa.

Una preflight request utiliza el método `OPTIONS` del encabezado de una solicitud `HTTP` y permite que el navegador indique que contenidos están permitidos y qué no. Un servidor no debe ejecutar ningún tipo de lógica de negocios, y solo debe retornar los encabezados.

No obstante, no todas las solicitudes pueden ser sometidas a una verificación previa. Las solicitudes con etiquetas de imagen o los formularios no son preflight request. Por lo tanto, cualquier tipo de solicitud `GET` se enviará de inmediato, pero esta petición no podrá leer la respuesta si CORS no lo permite.

Los detalles sobre cuándo se envían realmente las preflight requests, son complejas y extensas. Si esta interesado en profundizar en este tema puede revisar la documentación de la MDN para [preflighted requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#Preflighted_requests).

Ahora tenemos un par de alternativas para lidiar con el same origin policy. Si alguna vez participa en la publicación de un API, lo recomendable es contemplar CORS desde el principio y habilitarlo en su servidor.

## Fallo de seguridad: CSRF

Como acabamos de enterarnos, las solicitudes que parecen que provienen de un formulario no serán revisadas previamente. No se puede leer una respuesta si el servidor no lo permite. No obstante, no ver la respuesta no es suficiente para causar estragos.

Imagine que un banco tiene un formulario para transferir dinero. Si usted no es una una buena persona y todo lo que quiere hacer es transferir dinero a su propia cuenta es probable que no le interesen las respuestas del servidor.

En vez de intentar ver la respuesta del servidor usted decide configurar un sitio web que falsifica una solicitud de la misma URL que utiliza el formulario y configura todos los parámetros para que el dinero se transfiera a su cuenta si que el usuario original se de cuenta.

Es por eso que este tipo de ataque se denomina **CSRF** (Cross-Sites Request Forgery) o _falsificación de solicitudes entre sitios_ al traducirla a español. Por supuesto,  los bancos tienen mecanismos sofisticados de protección, pero para la mayoría de los propósitos, un token CRSF es suficiente protección.

Si el token CSRF es un campo adicional que se agrega al formulario y es colocado e inicializado desde el servidor. Si alguien está enviando una solicitud, el vendedor verifica ese token contra el que tiene almacenado y solo ejecuta la solicitud si dichos tokens coinciden.

## Fallo de seguridad: XSS

Siempre que un sitio web habilite entradas al usuario, el desarrollador debe ser muy cuidadoso y estar en constante vigilancia. Las entradas de los usuarios pueden ser cualquier cosa y es su responsabilidad como desarrollador asegurarse de que estos contenidos no afecten su sitio.

Un usuario despistado podría romper el sitio por accidente. Un atacante podría explotar esta falla y hacer que su sitio web haga cosas para las cuales no fue implementada.

No validar las entradas de los usuarios es una de las vulnerabilidades más antiguas en la web y se denomina **XSS** (Cross-Site Scripting). El nombre proviene del hecho de que JavaScript se puede insertar en el sitio web x pero se ejecuta desde el sitio web y así poder obtener todos los datos del sitio web y.

Un ejemplo típico es un sitio web que solicita el nombre del usuario al momento de querer dejar un comentario. Si esta entrada no está validada, el nombre de un usuario se puede crear de tal manera que contenga código JavaScript. (e.g `Sua <script> alert(Hola)</script> Bochica`  )

Eso significa que cada usuario que lea ese comentario solo verá el nombre, pero el código se ejecutará sin el conocimiento de los usuarios.

En el gran esquema de las cosas, este ejemplo aquí es bastante inofensivo, pero el script tiene acceso a todos los datos del sitio, incluyendo el DOM y las Cookies. Incluso podría hacer solicitudes `GET` desde el origen del sitio, y así saltar la restricción del same policy origin.

Un XSS bien diseñado puede ser perjudicial. La única forma de protegerse de este tipo de ataques es seguir una _regla de oro_ que siempre aplica en la ingeniería de software. _Valide las entradas de los usuario desde el lado del servidor_

## Recapitulación

Seguridad no es un tema fácil y los problemas que cubrimos en este documento requieren de consideraciones tanto de front-end como de back-end. Ahora el lector esta en capacidad de detectar posibles vulnerabilidades y cómo verificar si son explotables.

Cuando este a punto de agregar cualquier tipo de campo de entrada es su aplicación web, debe tener en cuenta los fallos de CSRF y XSS e identificar si hay riesgos potenciales que deben ser mitigados. De este modo tanto el desarrollador hará un gran favor para el mismo y los usuarios de su sitio web.
