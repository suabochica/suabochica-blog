+++
title = "Introducción a micro servicios"
author = ["Sergio Benítez"]
description = "Usa docker para construir contenedores de imágenes para empacar una aplicación y sus dependencias y así hacer un despliegue en una sola máquina"
date = 2020-11-10T00:00:00-05:00
lastmod = 2021-05-15T18:21:05-05:00
draft = false
tags = [
    "microservicios",
]
+++

## La evolución de las aplicaciones {#la-evolución-de-las-aplicaciones}

Antes de desarrollar a fondo el concepto de microservicios, debemos estar 
seguros de por que los microservicios están diseñados de cierta forma. Para ello,
vamos a apoyarnos en la interpretación de Adrian Cockcroft, *cloud architect*
en Netflix, sobre la evolución de las aplicaciones.

Adrian comparte su visión sobre el desarrollo de aplicaciones en la última
decada, partiendo de que algunos años atrás era común encontrar
proyectos con mas the 5 millones de lineas de código, en donde el proceso
de **build** tardaba tiempos considerables y el **release** se hacia con poca 
frecuencia.

Lo que empezó a pasar más recientemente fue que ese monolito empezó a
romperse en pequeñas partes de software. Parte de los microservicios es hacer
estos pequeños trozos de software. Este cambio, logra que los tiempos de los
builds pasen de horas a segundos. Por otra parte, dichos builds son empaquetados
en un contenedor cuyo despliegue solo tomara algunos segundos, logrando así que
los realeases se ejecuten con mayor frecuencia.

En definitiva, uno de los objetivos de los microservicios es incrementar la
velocidad de desarrollo.

Imagine un escenario en donde cientos de personas están construyendo una aplicación 
monolito, y una parte del equipo encuentra que algo esta fallando y luego en la 
etapa de pruebas encuentran que dicho fallo está retrasando el release por una 
semana, esto hace que el trabajo de las 99 personas no sea publicado en 
producción en el tiempo esperado. Siguiendo la misma línea, se decide seguir 
adelante con el release y posteriormente en producción se identifica que el 
falló explotó, obligando así el retroceso a una version previa, lo cual en
tiempo se traduce a otra semana más. En conclusion, tener demasiadas personas
contribuyendo a un único gran proyecto, expone el riesgo de que un caso
especifíco bloquée una gran cantidad de trabajo, obligando así a crear puntos
de revisión para coordinar la entrega del trabajo.

Por tanto, quebrar este monolito en microservicios básicamente es liberar al
equipo de trabajo para que se desenvuelvan a su propia velocidad. Uno de los
mayores beneficios ofrecidos por los microservicios es que no es necesario
coordinar la entrega de trabajo. El propósito es dejar que cada uno liberé su
código bajo su propio ciclo. Para ello es necesario que las interfaces sean
estables y que cada pieza de código que se despliega es independiente de otra.
No se debe pedir permiso para publicar los cambios, pero si es necesario
comunicar los cambios que van a ser publicados.

Por otra parte, la infraestructura de los microservicios necesita de
automatización para que sea efectiva. Es aquí donde las tareas operacionales
son relevantes. 



## Microservicios {#micorservicios}

Microservicios es un enfoque de arquitectura para diseñar aplicaciones bajo las
siguientes pautas:

-   Desarrollo modular
-   Fácil de desplegar
-   Escalamiento independiente

Varios de los beneficios del patrón de diseño de los microservicios se ejercen para
cualquier aplición. Los *rapid developments* y el *continuos delivery* son algunos
de ellos. Paralelamente, los microservicios ejercen presión sobre los limites de
la mayoria de las herramientas de automatización e infrasestructura. Es esta la 
razón por la cual es necesario el uso de una herramienta avanzada como Kubernetes.

## Configuración {#configuración}

Para entrar al mundo de los microservicios es necesario hacer una configuración
previa sobre una plataforma cloud. Para este caso vamos a utilizar Google Cloud.

En ese orden de ideas es necesario crear una cuenta Google Compute Engine (GCE)
la cual permitirá la creación de  máquinas virtuales, redes y volúmenes de
almacenamiento. GCE también suministra un entorno de computo llamado
[Google Cloud Shell](https://cloud.google.com/shell/docs).

Para crear una cuenta en Google Cloud Platform (GCP) es necesario asociar una
tarjeta de crédito. Es importante resaltar que no hay costo de subscripción,
pero si se harán cobros sobre los recursos que se utilicen.

Los pasos a seguir son:

1.  Crear un proyecto en GCP
2.  Habilitar el Compute Engine API
3.  Habilitar el Kubernetes Engine API
4.  Configurar el Google Cloud Shell

Posteriormente, vamos a descargar una version reciente de Go, ejecutando los
siguientes comandos:

    wget https://storage.googleapis.com/golang/go1.6.2.linux-amd64.tar.gz
    sudo rm -rf /usr/local/go
    sudo tar -C /usr/local -xzf go1.6.2.linux-amd64.tar.gz
    echo "export GOPATH=~/go" >> ~/.bashrc
    source ~/.bashrc

Y por último vamos a descargar un proyecto de Udacity:

    mkdir -p $GOPATH/src/github.com/udacity
    cd $GOPATH/src/github.com/udacity
    git clone https://github.com/udacity/ud615

## Build del proyecto monolito {#build-del-proyecto-monolito}

Si revisamos los contenido del proyecto, vamos a tener la siguientes estructura
de directorios:

    drwxr-xr-x 2 auth
    drwxr-xr-x 2 certgen
    drwxr-xr-x 2 Godeps
    drwxr-xr-x 2 handlers
    drwxr-xr-x 2 health
    drwxr-xr-x 2 hello
    drwxr-xr-x 2 monolith
    -rw-r--r-- 1 README.md
    drwxr-xr-x 2 user
    drwxr-xr-x 4 vendor

Notése que el proyecto se compone de tres aplicaciones: monoliht, hello y auth.
El servicio de `monolith`, combina los microservicios de `hello` y `auth`.

Para correr la applicación `monolith` ejecutamos los siguientes comandos:

    app $ mkdir bin
    app $ go build -o ./bin/monolith .monolith/
    app $ sudo ./bin/monolith -http 0.0.0.0:10080

Estos comandos van a levantar un servidor HTTP el cual vamos a consumir con un 
cliente HTTP llamado `curl`, ejecutando el siguiente comando en otra sesión del
Google Cloud Shell:

    app $ curl http://127.0.0.1:10080
    {"message":"Hello"}

Si se recibe la respuesta `{"message":"Hello"}`, el consumo del servicio ha sido
exitoso y significa que el servidor esta funcionando.

Ahora vamos a hacer una petición segura:

    app $ curl http://127.0.0.1:10080/secure
    authorization failed

Esta solicitud HTTP falla, porque ese necesario obtener un [Json Web Token (JWT)](https://jwt.io)
desde el endpoint de inicio de sesión. Para ello ejecutamos el siguiente
comando:

    app $ curl http://127.0.0.1:10080/login -u user
    Enter host password for 'user':
    {"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVzZXJAZXhhbXBsZS5jb20iLCJleHAiOjE2MDQ0MTA3MDAsImlhdCI6MTYwNDE1MTUwMCwiaXNzI
    joiYXV0aC5zZXJ2aWNlIiwic3ViIjoidXNlciJ9.SV2qMc2v5pnBbGRpZr21stucfvk_RmjQ6mbarjPc10s"}

En la solicitud al endpoint `login` especificamos el inicio de sesión con el
usuario `user` cuya contraseña es `password`. Si se otorgan los credenciales
apropiadamente vamos a obtener como respuesta el JWT.

Ahora, ya se puede hacer la solicitud a nuestro enpoint de seguridad con el
siguiente comando:

    app $ curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVzZXJAZXhhbXBsZS5jb20iLCJleHAiOjE2MDQ0MTA3MDAsImlhdCI6MTYwNDE1MTUwMCwiaXNzI
    joiYXV0aC5zZXJ2aWNlIiwic3ViIjoidXNlciJ9.SV2qMc2v5pnBbGRpZr21stucfvk_RmjQ6mbarjPc10s" http://127.0.0.1:10080/secure
    {"message":"Hello"}

Como esperamos, ahora si esta funcionando. Es importante resaltar que la
aplicación `monoliht` aprovecha muchos beneficios encontrado en los 12 factores
de una aplicación. Por ejemplo, estamos obteniendo logs estandarizados sin
necesidad de tocar los archivos logs. Por otra parte, el modelo de la aplicación
esta auto contenida, y todas nuestras dependencias están agrupadas en el
directorio `/vendor`.

Seguir estas practicas hace que la aplicación sea mas fácil de mantener y
desplegar en la nube.

## Los 12 Factores {#los-12-factores}

Los 12 factores es una guía de buenas prácticas para construir aplicaciones 
desplegables de Software-as-a-Service (SaaS). El concepto resalta tres
importantes cirterios a tener en cuenta en el diseño de aplicaciones
modernas:

-   **Portabilidad**: Eliminar elementos que varían entre los ambientes de ejecución,

tales como dependencias o configuraciones.

-   **Continuamente desplegable**: Generalmente las aplicaciones que usan los 12

facotres son desplegadas en plataformas cloud (e.g. GCP ó Amazon), y se enfocan
en mantener los ambientes de desarrollo y producción uniformes. Esto significa
que se puede configurar herramientas para implementar actualizaciones de código
rápidamente.

-   **Escalabilidad**: Las aplicaciones deben atender las demandas de los usuarios,

mientras se usan las herramientas y las prácticas que se emplean desde un
principio.

El patrón de diseño de microservicios siguen la mayoría de estos factores. Ahora
es tiempo de utilizar Docker y Kubernetes para atender estas pautas

### Referencias {#referencias}

-   [12-factor manifesto](https://12factor.net)


## Ejemplos {#ejemplos}

Ahora vamos a aterrizar algunos de los 12 factores asociandolos a unos 
escenarios especifícos.

Al momento de decidir alamacenar el código de la aplicación en Git, estamos
aplicando el factor de *código base*, ya que el código esta sobre una
herramienta que nos ofrece controles de revisión y la posibilidad de correr
muchos despliegues.

Por otra parte, cuando se imprimen las salidas en un \`stdout\` estamos aplicando
el factor *logs*. Esto permite que los desarrolladores usen los logs como una 
corriente de eventos. Por ejemplo, si se presenta un falló al intentar 
conectarse a una base de datos, se podría hace una búsqueda de mensajes de
error en la consola y así identificar la causa del fallo.

Por último, el uso the declaraciones de importanciones es un caso de uso del
factor *dependencias*. Los paquetes externos son almacenados con la aplicación,
hecho que reduce los puntos de fallos del despliegue del código dentro de
diferentes ambientes de ejecución.

Estos son unos ejemplos que nos puede ayudar a comprender mejor la propuesta
de los 12 factores.

# Refactorizar a MSA {#refactorizar-a-msa}

> MSA: Micro Services Arhcitecture

Tiempo de desglosar la aplicación monolito en microservicios. Idealmente el
monolito se descompone al rededor de una funcionalidad autocontenida. Para
este caso vamos a crear dos microservicios: `auth`, para manejar procesos de 
autorización y autenticación y un servicio `hello`.

Cada servicio tiene su propio binario, y para empezar se va a hacer el build de
los respectivos servicios para posteriormente ver como funciona el nuevo flujo
del despliegue de la aplicación.

Comencemos con el servicio `hello`.

    app $ go build -o bin/hello ./hello/
    app $ ./bin/hello -http ":10080" -health ":10081"

En un nueva sesión de terminal, vamos a hacer los mismo para el servicio `auth`:

    app $ go build -o bin/auth ./auth/
    app $ ./bin/auth -http ":10090" -health ":10091"

Ahora que tenemos ambos servicios corriendo vamos a usar el comando `curl` en
una nueva sesión de terminal para interactuar con los servicios de `auth` y 
`hello`.

    TOKEN=$(curl 127.0.0.1:10090/login -u user | jq -r '.token')
    curl -H "Authorization:  Bearer $TOKEN" http://127.0.0.1:10082/secure

Si todo sale como se espera, recibiremos la respuesta `{"message":"Hello"}`, con
la salvedad de que ahora tenemos multiples binarios que administran la
aplicación y nuestro despliegue se esta haciendo dos veces más complicado, lo
que significa que nuestros clientes necesitan saber como hablar con ambos
servicios por separado.

La comlejidad adicional solo crece con el número de servicios que este
utilizando la aplicación. Este problema es exactamente lo que impulsa la
adopción de contenedores de aplicaciones y con ayuda de plataformas de 
administración como Kubernetes, vamos a empezar a coordinarlos.

### Soluciones ofrecidas por MSA {#soluciones-ofrecidas-por-msa}

Es importante tener en cuenta los escenarios sobre los cuales una arquitectura
de microservicios ofrece soluciones. Para los monolitos, hacer un cambio en una
parte del código implica una recompilación de todo el binario. Al tener piezas
independientes de código, podemos enviar los binarios por separado. Esto
significa que los servicios son unidades de funcionalidad que pueden ser
reemplazadas o actualizadas separadamente del resto del sistema, obteniendo así
componentes estrechamente acoplados.

Otra solución de los MSA, es el mantenimiento, ya que trabajamos con componentes
de escala independiente. Cada servicio es tan pequeño y esto incrementa las
posibilidades de que sea más fácil de entender. Por otra parte, se fomenta el
desarrollo de un código base robusto y estable que deberiía ser fácil de
mantener.

## JWT {#jwt}

Json Web Tokens (JWT), es un método compacto y auto contenido para transferir
datos de manera segura como objetos JSON. Es un estándar útil, porque envía 
información que puede verificada y validada con una firma digital.

Los JWTs son medios compactos de almacenamiento de datos faciles de descodificar
o codificar en la mayoria de los lenguajes.

Por último, JWT es ideal para autenticación e intercambio de información gracias
a su tamaño y al hecho de que pueden ser firmados. Esto nos permite auditar la
identidad de quienes envían la información y tener garantías sobre que los datos
enviados nos están alterados.


## Como funciona JWT {#como-funciona-jwt}

![img](../images/microservices/01-how_jwt_works.png "El cliente envía una solicitud de log in")

![img](../images/microservices/02-how_jwt_works.png "El servidor crea un JWT")

![img](../images/microservices/03-how_jwt_works.png "El servidor retorna el JWT al cliente")

![img](../images/microservices/04-how_jwt_works.png "El cliente envía una copia del JWT cuando hace una petición")

![img](../images/microservices/05-how_jwt_works.png "El servidor valida la firma del JWT")

![img](../images/microservices/06-how_jwt_works.png "El servidor envía la respuesta al cliente")

## Recapitulación {#recapitualción}

En esta publicasión se desarrolló la siguiente idea: Entendimiento del
diseño de las aplicaciones modernas, incluyendo el patrón de diseño de los
microservicios.

Se puede observar como las aplicaciones modernas están empujando los limites de
las herramientas de automatización actuales. En vez de tener un gran binario, 
ahora estas aplicaciones estan compuestas por muchas partes que son más 
pequeñas, más fáciles de escribir y desplegar.

Esta nueva propuesta nos obliga a revisar las herramientas y la infrasestructura
que se está utilizando para apoyar estas aplicaciones modernas. En consecuencia,
es necesario optar por nuevas herramientas de automatización como Docker y
Kubernetes.

