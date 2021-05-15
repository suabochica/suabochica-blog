+++
title = "Construyendo contenedores con docker"
author = ["Sergio Benítez"]
description = "Usa docker para construir contenedores de imágenes para empacar una aplicación y sus dependencias y así hacer un despliegue en una sola máquina"
date = 2020-11-10T00:00:00-05:00
lastmod = 2021-05-15T18:21:05-05:00
draft = false
tags = [
    "microservicios",
]
+++

## Por qué docker {#por-qué-docker}

Luego de tener un conocimiento sólido sobre el diseño de aplicaciones modernas
a través del patrón de diseño de microservicios, el siguiente paso es desplegar
el código.

El primer criterio a considerar es las dependecias de las aplicaciones. La
secuencia empezaría con la construcción de paquetes y distribuir todas estas
aplicaciones, y es de esperarse que eso requiera mucho trabajo.

Una opción es decifrar todas las dependecias y construir un paquete nativo para
cada distribución de linux que se quierar soportar, ó se puede omitir este
enfoque y contruir un contenedor de imagen.

Ahora bien, ¿qué es un contenedor de imagen?. Básicamente, es un formato de
empaque que no solo incluye la aplicación, sino todas las dependencias o
información de tiempo de ejecución requerida para ejecutarla.

Una analogía adecuada, es un teléfono celular. En su celular, se descarga una
aplicación autocontenida y la ejecuta. Los contenedores son la misma cosa,
pero para sus servidores. La tecnología de contenedores esta construida para la
mayoría de los sistemas operativos, pero no es precisamente fácil de usar desde
el primer momento. Para ello, es necesario una herramienta como docker, ya que
provee un API con las último en la tecnología de contenedores y facilita la
_creación_, _distribución_ y la _ejecución_ de contenedores de imágenes en los
servidores.


### Tutorial básico de docker {#tutorial-básico-de-docker}

1.  Clonar: se pueden clonar imágenes en repositorios de GitHub, los cuales

contienen todo lo necesario para construir la imagen y correrla como un
contenedor. Los siguientes comandos permiten clonar un respositorio de  prueba
suministrado por docker:

```bash
docker run --name repo alpine/git clone \
https://github.com/docker/gettign-started.git

docker cp repo:/git/getting-started/ .
```

1.  Build: Ahora, es tiempo de construir la imagen. Una imagen de docker es un

sistema privado de archivos para tu contenedor. El provee todos los archivos y
el código que el contenedor precisa.

```bash
cd getting-started

docker build -t docker101tutorial .
```

1.  Run: Ejecute su primer contenedor. Inicie el contenedor basado en la imagen

que se construyó en el paso previo. Al ejecutar el contenedor se lanza la
aplicación con recursos privados, asegurados y aislados del resto de tu máquina.

```bash
docker run -d -p 80:80 --name docker-tutorial docker101tutorial
```

1.  Share: Por último, salve y comparta su imagen en Docker Hub con el fin de que

otros usuarios puedan descargarla y ejecutarla en cualquier máquina.

```bash
docker tag docker101tutorial username/docker101tutorial

docker push username/docker101tutorial
```


## Revisión general de docker {#revisión-general-de-docker}

Docker és una herramienta open source que hace fácil la creación, distribución y
y ejecución de aplicaciones. Lo hace empaquetando las aplicaciones como un
conjunto de contenedores de imágenes que envuelve todas las dependencias
incluidas con él.

Los contenedores de imágenes pueden correr en cualquier máquina que tenga docker
instalado, haciendo posible el sueño de cualquier administrador de paquetes, ya
que docker ofrece contendores reproducibles y consistentes.

Al igual que los procesos en un computadores, los contenedores pueden prenderse
y apagarse rápidamente. Al mismo tiempo, los contenedores proveen los beneficios
de una máquina virual como el aislamiento entre las aplicaciones que se están
ejecutando en una misma máquina. Por ende, no hay preocupaciones por conflictos
de dependencias entre aplicaciones.


## Instalando aplicaciones con herramientas nativas del sistema operativo {#instalando-aplicaciones-con-herramientas-nativas-del-sistema-operativo}

Luego de conectase  al GCS, se va a dar un vistazo a como administrar
aplicaciones sin docker. Para empezar se crea una máquina virtual de Ubuntu para
jugar con ella usando el gcloud.

```bash
$ gcloud config set project [PROJECT_ID]
$ gcloud compute instances create ubuntu --image-project ubuntu-os-cloud --image ubuntu-1604-xenial-v20160429
```

Al crear la máquina virtual, se usa el commando ssh de gcloud para establecer la
conxexión con dicha máquina:

```bash
$ gcloud compute ssh ubuntu
```

Para validar la conexión a la máquina virtual, en el shell debe aparecer la
siguiente información al inicio de cada línea: `username@ubuntu:~$`.

Ahora, con ayuda de un administrador de paquetes nativos se va a instalar NGINX
y todas sus dependencias:

```bash
$ sudo apt-get update
$ sudo apt-get install nginx
```

La instalación de NGINX se valida con el comando `nginx -v` y la consola
imprimirá una salida como esta: `nginx version: nginx/1.10.3 (Ubuntu)`.
Complementariamente, para saber si NGINX está corriendo, se usa el comando
`systemctl`.

```bash
$ sudo systemctl status nginx
```

Este comando imprime los logs relacionados al estado de NGINX, y así se podrá
saber si esta siendo ejecutado. Por último, se prueba si NIGNX esta disponible
localmente con ayuda del `curl`:

```bash
$ curl http://127.0.0.0
```

¡Perfecto! los sistemas operativos modernos hacen muy fácil instalar, iniciar y
ejecutar aplicaciones.


## Problema: ¿Cómo instalar dos versiones? {#problema-cómo-instalar-dos-versiones}

Ahora se tratará de instalar dos versiones de NGINX en la máquina virtual. Si se
instala nuevamente usando el administrador de paquetes nativo se obtiene un
mensaje diciendo que NGINX ya está instalado con su última versión.

Al usar el comando `systemctl` se observa que solo esta corriendo una instancia
de NGINX y para confirmar dicho ustado se usa el comando `ps` en conjunto con
`grep` como se muestra a continuación:

```bash
$ sudo ps aux | grep nginx
```

En está salida se indica que solo hay un proceso NGINX corriendo con su
respectivo worker. Por lo tanto, solo hay una instancia principal de la
aplicación que se esta ejecutando.

Para lograr crear dos instancias de NGINX en la misma máquina virual usando las
herramientas nativas del sistema operativo es necesario modificar los scripts de
inicialización. Para el caso puntual de NGINX, los cambios irián en el archivo
`/etc/init/nginx.conf`.

Adicionalmente, se requiere administrar los puertos, ya que las dos instancias
no pueden enlazarse al mismo puerto. En resumen, hay mucha complejidad para
hacer las configuraciones con las herramientas nativas del sistema operativo y
lograr correr dos instancias de NGINX con versiones diferentes en la misma
máquina virtual.

Por defecto, la mayoría de los sistemas operativos solo permite instalar una
versión de una aplicación y ejecutar una instancia de la misma. Eso significa
que para lograr el propósito de correr dos instancias de una misma aplicación
con versiones diferentes en una máquina virtual hay que reconsiderar el enfoque.
Este nuevo enfoque son los contenedores.


## Revisión general del contendor {#revisión-general-del-contendor}

Los contenedores fueron desarrollados para solucionar los problemas con la
instalación y ejecución de software a través de diferentes sistemas operativos.

Ya se ha visto los problemas de instalar y ejecutar aplicaciones sobre una única
máquina. Ahora, imaginesé instalar NGINX en multiples máquinas y a través de
diferentes sistemas operativos. Eso sería una pesadilla.

Con contenedores se obtienen paquetes autocontenidos _independientes_ que evitan
conflictos con las versiones. Eso es parte del atractivo de los contenedores de
imágenes. Ellos son fáciles de distribuir porque se encargan de todas sus
dependencias por si mismos.

Adicionalmente, los contendores ofrecen _aislamiento_ sobre varios procesos.
En secciones previas, se habló de aislamiento a nivel de sistema de archivos,
pero también existen otros tipos de aislamiento, como por ejemplo el aislamiento
 de red. El aislamiento de red significa que cada instancia tendrá su propia
dirección IP, y por ende cada máquina tendrá disponible el puerto 80, que es el
puerto por defecto utilizado por NGINX, ahorrando así los problemas de lidiar
con la configuración de los scripts de inicialización.

Algo importante para considerar, es que mientras se corran dos máquinas
virtuales en un computador, se tendrán dos sistemas operativos completamente
separados, y multiples contenedores se ejecutaran sobre el mismo sistema
operativo, debido a que los contenedores son una construcción lógica usada
dentro del sistema operativo. Esto hace que los contenedores sean ligeros y sean
fáciles de prender o apagar.


## Instalando imágenes con docker {#instalando-imágenes-con-docker}

Tiempo para ver porque docker realmente resplandece al demostrar lo sencillo
que resulta tener multiples versiones de NGINX ejecutandose en la misma máquina.
Es aquí donde docker agrega valor, ya que permite ejecutar aplicaciones auto
contentidas con sus propias versiones y con paqutes que incluyen sus propias
dependencias. Cabe resaltar que dos versiones necesitan dependecias distintas y
docker permite dicho escenario al crear dos contendores con las respectivas
versiones.

Además, docker abstrae el administrador de paquetes del sistema operativo. Si es
un contenedor con Red Hat se utiliza `RPM` y de ser Debian se usa `apt-get`.
Esto se logra con aprendizaje sobre la línea de comandos de docker.

Para empezar, se instala docker con el siguiente comando:

```bash
sudo apt-get install docker.io
```

Una vez instalado, ya se esta listo para trabajar con imágenes de docker. Se
puede consultar la disponibilidad de las imágenes de docker con el siguiente
comando:

```bash
sudo docker images
```

Por el momento, no se verá ningún resultado ya que no hay ninguna imagen
instalada en el sistema. Con el siguiente comando se instalará una imagen de
NGINX con la misma versión se instaló a traves del administrador de paquetes
nativo del sistema operativo.

```bash
sudo docker pull nginx:1.10.0
```

Este comando tomará algo de tiempo para descargar la imagen del repositorio,
puesto que se esta halando NGINX con todas sus dependencias para que la imagen
pueda ser autónoma. Los repositorios se desarrollarán más adelante, por ahora,
el enfoque está en la imágenes de docker.

Al correr nuevamente el comando para consultar las imágenes de docker se obtiene
un resultado, la imagen de NGINX. La versión del NGINX instalado con docker se
puede corroborar con el siguiente comando:

```bash
sudo dpkg -l | grep nginx
```

Notesé que ahora hay dos paqutes de nginx instalados, uno con el administrador
de paquetes del sistema operativo y otro con docker, ambos con la misma versión.


## Corriendo imágenes con docker {#corriendo-imágenes-con-docker}

Ya se sabe que se pueden instalar multiples instancias de NGINX via docker. Pero
, ¿se pueden ejecutar? Se realizará la prueba respectiva.

Primero, se usa el siguiente comando para correr la primera instancia de NGINX:

```bash
$ sudo docker run -d nginx:1.10.0
```

Para verificar la lista de los procesos docker que se están ejcutando
actualmente se pone en marcha el siguiente comando:

```bash
$ sudo docker ps
```

Ahora, se va a correr otra instancia de NGINX con una versión diferente.

```bash
$ sudo docker run -d nginx:1.9.3
```

Dado que esta versión no se ha descargado previamente, el comando `docker run`
traerá automaticamente la imagen correspondiente de la versión 1.9.3 de NGINX.
Se puede repetir este proceso para n instancias que se requieran. Por tanto,
se ejecutará nuevamente otra instancia.

```bash
$ sudo docker run -d nginx:1.10.0
```

Al verificar la lista de procesos que se están corriendo actualmente en docker,
se obtiene la siguiente salida:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED
96fef877f851        nginx:1.10.0        "nginx -g 'daemon of…"   5 seconds ago
03349cc81243        nginx:1.9.3         "nginx -g 'daemon of…"   13 seconds ago
1012cf1911d0        nginx:1.10.0        "nginx -g 'daemon of…"   25 seconds ago
```

Con el siguiente comando, se consiguen los procesos actuales de NGINX
que están siendo ejecutados en el sistema operativo:

```bash
$ sudo ps aux | grep nginx
root     11382  0.0  0.1  31684  5152 ?        Ss   13:06   0:00 nginx: master process nginx -g daemon off;
syslog   11422  0.0  0.0  32068  2952 ?        S    13:06   0:00 nginx: worker process
root     11601  0.0  0.1  31500  4952 ?        Ss   13:06   0:00 nginx: master process nginx -g daemon off;
syslog   11643  0.0  0.0  31876  2816 ?        S    13:06   0:00 nginx: worker process
root     11759  0.0  0.1  31684  5072 ?        Ss   13:06   0:00 nginx: master process nginx -g daemon off;
syslog   11795  0.0  0.0  32068  2888 ?        S    13:06   0:00 nginx: worker process
sergio_+ 11906  0.0  0.0  12948   972 pts/0    S+   13:10   0:00 grep --color=auto nginx
```

Nótese que en esta salida, hay tres procesos maestros de NGINX corriendo. Docker
hace esto muy fácil, ya que no hay que intervenir sobre scripts de
inicialización o configuración, especificar puertos y otros procesos tediosos
para correr multiples instancias al mismo tiempo.


## Hablando con las instancias de docker {#hablando-con-las-instancias-de-docker}

Antes de establecer una comunicación con las instancias de docker, es necesario
saber que contendores están siendo ejecutados con el comando que se reviso
anteriormente.

```bash
$ sudo docker ps
```

De esta salida, se puede usar el identificador del contenedor para establecer el
destinatario de la comunicación. Con ayuda del comando `inspect` de docker se
obtiene información relevante al contenedor que se está ejecutando.

```bash
$ sudo docker inspect 96fef877f851
```

En la log de salida se puede ver la dirección IP del contenedor. Con ayuda de
curl se puede golpear la instancia NGINX que esta corriendo dentro del
contenedor.

```bash
$ curl http://172.17.0.4
```

Asi se comunica con una instancia en particular. Ahora que se terminó
la inspección de las instancias, es tiempo de limpiar los procesos de docker.
Una instancia de docker puede detenerse con el comando `stop`, el cuál recibe
como argumento el identificador del contenedor, de manera similar a como se uso
el comando `inspect`:

```bash
$ sudo docker stop 96fef877f851
```

Para remover el contenedor de docker del sistema, se usa el comando `rm`:

```bash
$ sudo docker rm 96fef877f851
```

Este comando también borrará todos los archivos asociados al contenedor.


## Creando imágenes de docker propias {#creando-imágenes-de-docker-propias}

Hasta ahora se han construido y ejecutado imágenes de docker ya hechas de NIGNX.
Es tiempo de aprender a construir imágenes propias de docker. Como buena
práctica, no se va a construir esta aplicación con docker. En su lugar, se va a
tomar un binario proveniente del pipeline de integración continua. Posterior a
ello, si se usa docker para empacar la aplicación como una imagen contenedora, y
para lograrlo, se usa un Dockerfile.

Los Dockerfiles son documentos de texto que contienen todos los pasos necesarios
para construir una imagen desde la línea de comandos. Al usar el comando `build`
de docker, el Dockerfile automatiza el proceso mediante la ejecución de las
instrucciones en líneas de comando y asi construir la imagen resultante.

Es importante preservar el nombre `Dockerfile`, ya que es una convención que usa
docker para ejecutar las instrucciones definidas en el archivo.

El comando que usa docker para crear la imagen, también crea un contexto
de todos los archivos en el directorio y sus repectivos subdirectorios. En este
paso el demonio de construcción irá a buscar el archivo llamado Dockerfile.

Para imágenes nuevas, la buena practica es iniciar con un directorio vacío,
acompañado del Dockerfile. Porsteriormente, se agregan los archivos necesarios
en el directorio para construir la imagen de docker.

El siguiente texto, es un ejemplo de un Dockerfile para la imagen `hello`:

```bash
# Dockerfile
FROM alpine:3.1
MANTAINER Sergio Benitez <sl.benitezd@gmail.com>
ADD hello /usr/bin/hello
ENTRYPOINT ["hello"]
```

Cada línea en un Dockerfile comienza con un comando. Para este caso puntual se
usan cuatro comandos: `FROM`, `MANTAINER`, `ADD` y `ENTRYPOINT`.  El Dockerfile,
también tiene sintáxis para comentarios, usando el carácter numeral \`#\`.

El comando `FROM` le dice a docker cuál es la imagen base sobre la que se va a
construir la imagen nueva. Generalmente se usa Alpine Linux, ya que es pequeña,
tiene un administrador de paquetes y permite depurar los contenedores en
producción. En consecuencia, es la imagen base que se usa por defecto para las
imágenes oficiales de docker.

El comando `MANTAINER` indica el author y la persona que hace el mantenimiento
de la imagen.

El comando `ADD` toma un archivo o directorio de la máquina anfitriona y la
agrega al sistema de archivos del contendor en la ubicación especificada. Para
este caso se esta copiando el binario `hello` del pipeline de integración
continua en el contenedor de la imangen.

Por último, el comando `ENTRYPOINT` que permitar correr el contenedor como un
ejecutable, por lo que cuando el contenedor se inicia, va a ejecutar la
aplicación hello.

Hay muchos más comandos para utilizar en el Dockerfile, por ahora, estos son
suficiente.


## Creando una imagen {#creando-una-imagen}

Docker también puede ser utilizado para construir ambientes. Primero se
construye la aplicación tal y como se ha visto en las sesiones pasadas. En ese
orden de ideas, se va a hacer un nuevo directorio para almacenar el código fuente
de la aplicación.

```bash
$ mkdir -p $GOPATH/src/github.com/kelseyhightower
```

Ahora, se accede al nuevo directorio y se clona la aplicación de ejemplo desde
github. Esta aplicación es un monolíto.

```bash
$ cd $GOPATH/src/github.com/kelseyhightower
$ git clone https://github.com/kelseyhightower/app.git
```

Con el código fuente en su lugar, es tiempo de construir el binario de la
aplicación. Para ello se navega hasta la aplicación y se ejecuta el comando
`build` de go:

```bash
$ cd app/monolith
$ go build -tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'
```

Ya con el binario generado, se puede proceder a la cración del contenedor de la
aplicación. Se da un vistazo al Dockerfile de la aplicación, para tener algo
de contexto:

```bash
$ cat Dockerfile
FROM alpine:3.1
MANTAINER Kelsey Hightower <kelsey.hightower@gmail.com>
ADD hello /usr/bin/monolith
ENTRYPOINT ["monolith"]
```

El comando `ADD` del Dockerfile esta copiando el binario que se generó
anteriormente con el comando `build` de go. El comando `ENTRYPOINT` declara el
punto de entrada para el contenedor, que para este caso será la aplicación
`monolith`.

Con el Dockerfile y el binario del monolíto en su lugar, es tiempo de crear la
imagen de docker del monolíto. Para ello, se usa el comando `build` de docker
para actualizar el contexto en el demononio de docker que es ejecutado con dicho
comando. El comando `build` va a reproducir una imange de la aplicación
`monolith`.

```bash
$ docker build -t monolith:1.0.0
```

Cuando la construcción de la imagen este completa, se usa el comando `images` de
docker para ver la imangen del monolíto.

```bash
$ docker images monolith:1.0.0
```

Para asegurarse de que la imagen del monolíto esta funcionando de manera
esperada, se usa el comando `run` de docker para crear el contender de la imagen
.

```bash
$ sudo docker run -d monolith:1.0.0
```

Con en contenedor del monolíto en ejecución, se usa el comando `inspect` de
docker para recuperar la dirección IP sobre la que se esta cargando el servidor
web de la aplicación.

```bash
$ docker inspect monolith:1.0.0
...
   "IPAddress": 172.18.0.2
```

Por último, usamos `curl` sobre la IP del paso previo para probar si el
contennedor esta funcionado como se espera.

```bash
$ curl 172.18.0.2
{"message": "Hello"}
```

¡Funciona!. Como se pudo observar, construir imágenes es muy fácil cuando se
usan Dockerfiles y el comando `build` de docker.


## Creando otros contenedores {#creando-otros-contenedores}

Para complementar el ejercicio se van a crear imágenes de docker para los
microservicios restantes: `auth` y `hello`.

Para construir la aplicación `auth` se ejecutan los siguientes comandos:

```bash
cd $GOPATH/src/github.com/udacity/ud615/app
cd auth
go build --tags netgo --ldflags '-extldflags "-lm - lstdc++ -static"'
sudo docker build -t auth:1.0.0 .
CID2=$(sudo docker run -d auth:1.0.0)
```

Similarmente, se construye la aplicación `hello`

```bash
cd $GOPATH/src/github.com/udacity/ud615/app
cd hello
go build --tags netgo --ldflags '-extldflags "-lm - lstdc++ -static"'
sudo docker build -t hello:1.0.0 .
CID3=$(sudo docker run -d hello:1.0.0)
```

Para ver los contenedores que se están ejecutando usamos el comando `ps`:

```bash
sudo docker ps -a
```


## Registros públicos vs privados {#registros-públicos-vs-privados}

Una vez se tiene la aplicación empaquetada se procede a copiarla en un servidor
y luego ejecutarla. Esta propuesta es válida para un conjunto pequeño de
máquinas. Si se escala la magnitud de la aplicación a cientos de máquinas es
necesario empujar los contenedores de imágenes a un repositorio remoto y
aprovechar herramientas como docker para obtener esas imágenes desde un lugar
central cuando se necesite correrlas. Este lugar central es conocido como docker
hub.

Por defecto, todas las imágenes empujadas a docker hub son públicas, y por ende
cualquier persona puede utilizarlas. No obstante, existe la opción de hacer que
las imágenes sean privadas y limitar el acceso a ellas. Docker hub ofrece la
flexibilidad para compartir imagenes públicas y privadas.

Un caso muy frecuente de un uso mixto de permisos en las imágenes es cuando las
compañias desarrollan proyectos de código abierto que comparten al mundo, pero
guardan su salsa secreta para sí mismos.


### Alternativas a docker hub {#alternativas-a-docker-hub}

-   Quay
-   Google Cloud Registry


## Empujando imágenes {#empujando-imágenes}

Como se abordo anteriormente, docker no solo ayuda a construir imágenes, también
ofrece una plataforma para compartirlas. Para lograr este objetivo, es necesario
un huésped para las imágenes en un registro de contenedores remoto como Docker
Hub.

Por defecto todas las imágenes de docker van hacia Docker Hub el cual asume que
se tiene su propio repositorio a través de un nombre de usuario perteneciente a
una cuenta creada en la plataforma. Por lo tanto, se debe agregar el nombre de
usuario a la imagen de docker a través del comando `tag` de docker:

```bash
$ docker tag -h
Usage: docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/] [USERNAME/] NAME[:TAG]
```

Como se puede ver en la salida de la opción `h`, el comando demanda una
combinación de imagen con etiqueta, un nombre de usuario y un repositorio. El
comando resultante sería:

```bash
$ docker tag monolith:1.0.0 kelseyhightower/monolith:1.0.0
```

Al validar nuevamente con el comando `images` se observa que en la salida se
asigna un valor a la columna `TAG`. En este registro también se observa que el
nombre de la imagen tiene el mismo identificador que la imagen `monolith`.

```bash
$ docker images

REPOSITORY                     TAG     IMAGE ID        CREATED          SIZE
monolith                       1.0.0   04d702954792    9 minutes ago    .37 MB
kelseyhightower/monolith       1.0.0   04d702954792    9 minutes ago    .37 MB
```

Ahora ya se está listo para empujar el contenedor de imagen a Docker Hub. Para
ello, es necesario iniciar sesión con el comando `login`

```bash
$ docker login
```

Posteriormente, se ejecuta el comando `push` como se muestra a continuación:

```bash
$ docker push kelseyhightower/monolith:1.0.0
```

De este modo se logra hospedar una imagen de docker en un repositorio para
compartirlo con otras personas.


## Outro {#outro}

Ya se sabe como empaquetar aplicaciones en imágenes de docker distribuir los
paquetes, y ejecutarlos es diferentes rangos de máquinas. Este hecho, desbloquea
las puertas de una automatización más avanzada. Ahora es posible abstaer los
detalles para empaquetar y correr la aplicación en un sistema operativo
específico. Ahora la atención está en el desarrollo de tus ideas y hacer
usuarios felices.

No obstante, este es el primer paso. Hasta ahora se ha trabajdo en una única
máquina. La demanda de usuarios en los días actuales no es soportada por una
única máquina. La pregunta es ¿Es posible coordinar, distribuir y administrar
contenedores a esa escala?. En un nivel más fundamental, se deberían correr
contenedores específicos sobre máquinas puntuales.

Algo importante a considerar es las aplicaciones se pueden ejecutar por si
mismas, entregando valor y siendo fáciles de monitorear y mantener. Con dichas
caracteristicas, es posible usar plataformas como Kubernetes para manejar toda
esta complejidad.
