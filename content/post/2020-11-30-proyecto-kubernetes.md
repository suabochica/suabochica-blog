+++
title = "Poryecto Kubernetes"
author = ["Sergio Benítez"]
description = "Uso de Kubernetes en aplicaciones reales"
date = 2020-11-30T00:00:00-05:00
lastmod = 2021-06-06T22:49:19-05:00
draft = false
tags = [
    "microservicios",
]
+++

## Intro {#intro}

Es tiempo de ir más lejos con el apredizaje de Kubernetes. Ya se repasaron los conceptos, y solo falta revisar como gerenciar Kubernetes con aplicativos reales.

Generalmente las aplicaciones reales exigen almacenamiento persistente, búsquedas TLS e implementaciones más avanzadas. Hasta el momento, Kubernetes ha parecido una mejor forma de ejecutar comandos Docker. Por ejemplo, con los comandos de `kubectl` se logra desplegar contenedores, y hacerlos disponibles por fuera del cluster.

En casos avanzados, es ideal declarar el estado desado de las infraestructuras y eso implica correr configuraciones en Kubernetes. Las configuraciones en k8s, permiten administrar los secretos, y determinar el flujo para actualizar los aplicativos.

Ahora, ¿qué siginifica el estado deseado?. Practicamente sería crear un conjunto de instrucciones hacia Kubernetes, y tras bastidores, la herramienta hará todo el trabajo, como la implementación de contenedores y la creación de balanceadores de carga.

Se prosigue con el uso de k8s en casos más avanzados.


## Revisión general de un despliegue {#revisión-general-de-un-despliegue}

El objetivo con Docker y Kubernetes es estar en capacidad de escalar y manejar contenedores en producción, y es aquí donde los despliegues empiezan a tener el protragonismo.

Los despliegues son formas declarativas para decir que va en donde. El uso de su estados es dado por el usuario final. Destrás de escenas, los displiegues en k8s usan un concepto llamado conjuntos de replica con el fin de asegurar que el número actual de pods es igual al número deseado.

Al momento de administrar los pods, el despliegue debe ser consciente de detalles de bajo nivel, como en que nodo esta el pod y en donde se esta ejecutando. El tiempo de vida del pod esta ligado al del nodo, lo que implica que si el nodo se cae, el por también lo hará.

Está administración de nodos se le delega e los despliegues, ya que con una serie de instrucciones se puede gestar efectivamente los estados de los nodos.

En el ejemplo de la siguiente imagen, se muestra un despliegue con tres replicas. Una de ellas está caida y la misma configuración estará en capacidad de iniciar un nuevo pod y encontrar un lugar para él. En este caso, el pod se inicia en el segundo nodo, comportamiento que resulta bastante útil.

{{< figure src="../../images/microservices/01-project_deployments.png" caption="Figure 1: El cliente envía una solicitud de log in" >}}

Es tiempo de combinar todo lo que se ha aprendido hasta ahora de pods y servicios y romper un aplicación monolito en pequeños servicios usando despliegues.


## Creando despliegues {#creando-despliegues}

Es momento de crear despliegues, uno por cada servicio: `auth`, `frontend` y `hello`. Se van a definir servicios internos para los despliegues de `auth` y `hello` y un servicio externo para el despliegue del `frontend`.

Es recomendable empezar con revisar los contenidos de los archivos de configuración. Para este caso puntual vamos a revisar el archivo `auth.yalm`

```bash
$ cat deployments/auth.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: auth
spec:
  replicas: 1
  selector:
    matchLabels:
      app: "auth"
  template:
    metadata:
      labels:
        app: auth
        track: stable
    spec:
      containers:
        - name: auth
          image: "udacity/example-auth:1.0.0"
          ports:
            - name: http
              containerPort: 80
            - name: health
              containerPort: 81
          resources:
            limits:
              cpu: 0.2
              memory: "10Mi"
          livenessProbe:
            httpGet:
              path: /healthz
              port: 81
              scheme: HTTP
            initialDelaySeconds: 5
            periodSeconds: 15
            timeoutSeconds: 5
          readinessProbe:
            httpGet:
              path: /readiness
              port: 81
              scheme: HTTP
            initialDelaySeconds: 5
            timeoutSeconds: 1
```

En esta salida se obtiene información útil como el número de replicas, las etiquetas que se van a usar y la versión de la imagen de docker sobre la cual se va a generar el contenedor. Se resalta que dicha configurarción se puede editar cuando se requiera necesario.

Para crear el despliegue del servicio se usa el siguiente comando:

```bash
$ kubectl create -f deployments/auth.yaml
deployment.apps/auth created
```

Al igual que cualquier otro objeto kubernetes, se puede usar el comando `describe` para obtener más información sobre el despliegue del servicio:

```bash
$ kubectl describe deployments auth
Name:                   auth
Namespace:              default
CreationTimestamp:      Mon, 04 Jan 2021 21:02:35 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=auth
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=auth
           track=stable
  Containers:
   auth:
    Image:       udacity/example-auth:1.0.0
    Ports:       80/TCP, 81/TCP
    Host Ports:  0/TCP, 0/TCP
    Limits:
      cpu:        200m
      memory:     10Mi
    Liveness:     http-get http://:81/healthz delay=5s timeout=5s period=15s #success=1 #failure=3
    Readiness:    http-get http://:81/readiness delay=5s timeout=1s period=10s #success=1 #failure=3
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   auth-784c79df7f (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  99s   deployment-controller  Scaled up replica set auth-784c79df7f to 1
```

Con esta configurarción, ya se está en capacidad de crear el servicio de `auth`, siguiendo un proceso similar al que se descrbió anteriormente:

```bash
$ kubectl create -f services/auth.yaml
services.apps/auth created
```

Se repiten estos pasos con cada uno de los servicios:

```bash
$ kubectl create -f deployments/hello.yaml
deployment.apps/hello created

$ kubectl create -f services/hello.yaml
services.apps/hello created

$ kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
configmap/nginx-frontend-conf created

$ kubectl create -f deployments/frontend.yaml
deployment.apps/frontend created

$ kubectl create -f services/frontend.yaml
services.apps/frontend created
```

Para el caso del servicio frontend, es necesario configurar NGINX tal y como se reviso previamente, a través del ~configmap.

Con dicha configuración, se esta listo para interectuar con el servicio `frontend` agarrando la dirección IP externa y usando `curl` para golpearla.

```bash
$ kubectl get services frontend
NAME       TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)         AGE
frontend   LoadBalancer   10.3.241.59   35.188.198.204   443:31017/TCP   5m47s

$ curl -k https://35.188.198.204
{"message": "hello"}
```

Y esto es todo, ahora se tiene una aplicación de multiservicios desplegada usando k8s. Estas habilidades permitirán desplegar applicaciones más complicadas sobre k8s, usando colecciones de despliegues y servicios.


## Revisión generar de escalamiento {#revisión-generar-de-escalamiento}

El escalamiento es hecho con la actualización del valor en la propuedad replicas de nuestro manifiesto de despliegue. Esta es considerada una buena práctica, ya que a pesar de tener métodos imperativos con `kubectl scale` no hay estado guardado en ninguna parte.

Por debajo, los despliegues crean un conjunto de replicas para manejar la creación, supresión y actulización de pods. Los despliegues administran por cuenta propia los conjuntos de replicas y por ende no se debe preocuparse por ellos.

Esta característica de los despliegues hace que el escalamiento hacia arriba y hacia abajo sea igual de facil para uno, dos o n nodos. En la siguiente imagen se muestra como el despliegue de `auth` se amplia por tres replicas:

{{< figure src="../../images/microservices/02-project_scaling.png" caption="Figure 2: El cliente envía una solicitud de log in" >}}


## Despliegues de escalamiento {#despliegues-de-escalamiento}

Es importante tener presente que cada despliegue es mapeado hacia un conjunto de replicas activo. El siguiente comando permite visualizar el conjunto actual de replicas que se están ejecutando:

```bash
$ kubectl get replicasets
NAME                DESIRED   CURRENT   READY   AGE
auth-784c79df7f     1         1         1       3h38m
frontend-868c46fc   1         1         0       3h31m
hello-67dfcd5745    1         1         1       3h32m
nginx-26dfcd5745    1         1         1       3h32m
```

Los conjuntos de replicas son escalados hacia los despliegues por cada servicio y este proceso se puede hacer de manera independiente. Como se menciono anteriormente, la verdadera fortaleza de k8s viene cuando se trabaja de manera declarativa, en vez de usar comandos imperativos de `kubctl`.

Si se quiere ver cuantos pods de `hello` se estan corriendo, se ejecuta el siguiente comando:

```bash
$ kubectl get pods -l "app=hello,track=stable"
NAME
hello-67dfcd5745
```

Para escalar el despliegue `hello` con tres replicas, se debe actualizar la propiedad `replicas: 3` en el archivo `deployments/hello.yaml`. Para aplicar los cambios se corre el siguiente comando:

```bash
$ kubectl apply -f deployments/hello.yaml
deployment "hello" configured
```

Se ejecuta nuevamente el `replicasets` y se verán los cambios:

```bash
$ kubectl get replicasets
NAME                DESIRED   CURRENT   READY   AGE
auth-784c79df7f     1         1         1       3h38m
frontend-868c46fc   1         1         0       3h31m
hello-67dfcd5745    3         3         3       3h32m
nginx-26dfcd5745    1         1         1       3h32m
```

El valor `DESIRED` para indicar el número deseado de replicas fue actualizado. Al correr el comando `kubectl get pods` también se observa el cambio:

```bash
$ kubectl get pods
NAME                      READY   STATUS              RESTARTS   AGE
auth-784c79df7f-jwdgp     1/1     Running             0          3h52m
frontend-868c46fc-k7nl5   0/1     ContainerCreating   0          3h45m
hello-67dfcd5745-7fswt    1/1     Running             0          3h45m
hello-67dfcd5745-7fsst    1/1     Running             0          3h45m
hello-67dfcd5745-7fszt    1/1     Running             0          3h45m
nginx-87dfcd5745-7fszt    1/1     Running             0          3h45m
monolith                  1/1     Running             0          3h45m
secure-monolith           1/1     Running             0          3h45m
```

Similarmente, el comando describe sobre `hello` es consistente con el resultado:

```bash
$ kubectl describe deployments hello
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=hello
            track=stable
  Containers:
    hello:
    Image:       udacity/example-hello:1.0.0
    Ports:       80/TCP, 81/TCP
    Host Ports:  0/TCP, 0/TCP
    Limits:
      cpu:        200m
      memory:     10Mi
    Liveness:     http-get http://:81/healthz delay=5s timeout=5s period=15s #success=1 #failure=3
    Readiness:    http-get http://:81/readiness delay=5s timeout=1s period=10s #success=1 #failure=3
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   hello-67dfcd5745 (3/3 replicas created)
Events:          <none>
```

En este punto se tienen múltiples copias del servicio `hello` corriendo en k8s y se tiene un solo servicio frontend que esta de intermediario sobre el tráfico hacia los tres pods. Esto perimite compartir la carga y escalar los contenedores en k8s.


## Descripción general de actualizaciones {#descripción-general-de-actualizaciones}

El último escenario que hace falta abordar, es el de las actualizaciones a nuevas actualizaciones de la aplicación. Obviamente, las actualizaciones de los contenedores deben proteger los datos y pueden ser nuevos cambios en el frontend para los usuarios. No obstante, sería arriesgado implementar todos los cambios al mismo tiempo.

En vez de eso, se puede utilizar `kubeclt rollout`, el cual funcionará de la siguiente manera.

Retomando el mapa de un despliegue con tres replicas de un pod, tal y como se muestra en la siguiente imagen, se decide actualizar los pods a una nueva versión disparando el comando `kubeclt rollout`.

{{< figure src="../../images/microservices/03-project_update1.png" caption="Figure 3: rolling update step 1" >}}

Como se observa en la imagen, un nuevo pod se publica y entonces el servicio empieza a enrutar el tráfico hacia este nuevo pod, con la versión actualizada. Esto significa que en determinado momento, se van a tener dos pods, uno con la primera versión y otro con la segunda, y el tráfico será dirigido hacia ambos.

Posteriormente, el tráfico hacia el pod viejo es detenido, y por último se deshace de él por completo. Las líneas amarillas en las imágenes representan el tráfico entre el servicio y el pod. En la siguiente imagen se consolida la descripción realziada.

{{< figure src="../../images/microservices/03-project_update2.png" caption="Figure 4: rolling update step 2" >}}

En este punto, el ciclo continua a través de todas las replicas hasta tener todos los pods en la segunda versión, como ilustra la siguiente imagen.

{{< figure src="../../images/microservices/03-project_update3.png" caption="Figure 5: rolling update step 3" >}}

Al final, todos los pods en el servicio, contaran con la segunda versión, luego de haber terminado la actualización.


## Actualizaciones continuas {#actualizaciones-continuas}

Kubernetes hace fácil la implementación de actualizaciones en las aplicaciones al modificar y administrar los despliegues. Nuevamente se va a modificar el archivo `deployments/auth.yalm` para usar la versión `2.0.0` de la imagen en el contenedor, tal y como se muestra a continuación:

```bash
$ vim deployments/auth.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: auth
spec:
  replicas: 1
  selector:
    matchLabels:
      app: "auth"
  template:
    metadata:
      labels:
        app: auth
        track: stable
    spec:
      containers:
        - name: auth
          image: "udacity/example-auth:2.0.0" # here!
          ports:
            - name: http
              containerPort: 80
            - name: health
              containerPort: 81
          resources:
            limits:
              cpu: 0.2
              memory: "10Mi"
          livenessProbe:
            httpGet:
              path: /healthz
              port: 81
              scheme: HTTP
            initialDelaySeconds: 5
            periodSeconds: 15
            timeoutSeconds: 5
          readinessProbe:
            httpGet:
              path: /readiness
              port: 81
              scheme: HTTP
            initialDelaySeconds: 5
            timeoutSeconds: 1
```

Para aplicar la actualización se ejecuta el siguiente comando:

```bash
$ kubectl apply -f deployments/auth.yaml
deployment "auth" configured
```

Para hacer seguimiento sobre el progreso de la actualización se usa el comando `kubectl describe`:

```bash
$ kubectl describe deployments auth
Name:                   auth
Namespace:              default
CreationTimestamp:      Mon, 04 Jan 2021 21:02:35 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 2
Selector:               app=auth
Replicas:               1 desired | 1 updated | 2 total | 1 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  1 max unavailable, 1 max surge # here!
Pod Template:
  Labels:  app=auth
           track=stable
  Containers:
   auth:
    Image:       udacity/example-auth:2.0.0
    Ports:       80/TCP, 81/TCP
    Host Ports:  0/TCP, 0/TCP
    Limits:
      cpu:        200m
      memory:     10Mi
    Liveness:     http-get http://:81/healthz delay=5s timeout=5s period=15s #success=1 #failure=3
    Readiness:    http-get http://:81/readiness delay=5s timeout=1s period=10s #success=1 #failure=3
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  auth-784c79df7f (1/1 replicas created)
NewReplicaSet:   auth-56746f6f6 (1/1 replicas created) # here!
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  10s   deployment-controller  Scaled up replica set auth-56746f6f6 to 1
```

Para revisar la estrategia de actualizaciones, se localiza la información en `RollingUpdateStrategy: 1 max unavailable, 1 max surge`. Como se puede observar, hay garantías sobre el número de pods que están siempre dispondibles.

En la propiedad `NewReplicaSet: auth-56746f6f6 (1/1 replicas created)` se asegura que el contenedor `auth` esta corriendo en su última verisión.

Una vez la implementación de la actualización está completa, se pueden ver los pods que se están corriendo sobre el servicio `auth` con el comando `kubeclt get pods`.

```bash
$ kubectl get pods
NAME                      READY   STATUS              RESTARTS   AGE
auth-784c79df7f-jwdgp     1/1     Running             0          2m
frontend-868c46fc-k7nl5   0/1     ContainerCreating   0          3h45m
hello-67dfcd5745-7fswt    1/1     Running             0          3h45m
hello-67dfcd5745-7fsst    1/1     Running             0          3h45m
hello-67dfcd5745-7fszt    1/1     Running             0          3h45m
nginx-87dfcd5745-7fszt    1/1     Running             0          3h45m
monolith                  1/1     Running             0          3h45m
secure-monolith           1/1     Running             0          3h45m
```

El indicador para saber si la nueva versión esta siendo utilizada es el tiempo registrado en la columna `AGE`. Este valor determina el tiempo que lleva corriendo ese pod, y al ser un periodo corto, nos da guías para identificar que la última version fue levantada hace dos minutos. Por defecto, el contendor nuevo reemplaza los previos.

La prueba definitiva para saber si el pod esta corriendo es usar el `kubectl describe` con el nombre específico del pod:

```bash
$ kubectl describe pods auth-784c79df7f-jwdgp
Priority:     0
Node:         gke-k0-default-pool-c1896f1f-wh9h/10.128.0.7
Start Time:   Tue, 05 Jan 2021 21:40:03 +0000
Labels:       app=auth
              pod-template-hash=56746f6f6
              track=stable
Annotations:  <none>
Status:       Running
IP:           10.0.5.4
IPs:
  IP:           10.0.5.4
Controlled By:  ReplicaSet/auth-56746f6f6
Containers:
  auth:
    Container ID:   docker://d93a62df764bce076044030ce9419efbd3260b481904727996de2bd91ec568d4
    Image:          udacity/example-auth:2.0.0 # here!
    Image ID:       docker-pullable://udacity/example-auth@sha256:02e142517709d35ca6cd51f8c1416a1c9514bcc0c369126fa0942007d23cf79a
    Ports:          80/TCP, 81/TCP
    Host Ports:     0/TCP, 0/TCP
    State:          Running
      Started:      Tue, 05 Jan 2021 21:40:06 +0000
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     200m
      memory:  10Mi
    Requests:
      cpu:        200m
      memory:     10Mi
    Liveness:     http-get http://:81/healthz delay=5s timeout=5s period=15s #success=1 #failure=3
    Readiness:    http-get http://:81/readiness delay=5s timeout=1s period=10s #success=1 #failure=3
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-gv274 (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-gv274:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-gv274
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  14m   default-scheduler  Successfully assigned default/auth-56746f6f6-wgzfq to gke-k0-default-pool-c1896f1f-wh9h
  Normal  Pulling    14m   kubelet            Pulling image "udacity/example-auth:2.0.0"
  Normal  Pulled     14m   kubelet            Successfully pulled image "udacity/example-auth:2.0.0"
  Normal  Created    14m   kubelet            Created container auth
  Normal  Started    14m   kubelet            Started container auth
```

En la propiedad `Image: udacity/example-auth:2.0.0` se puede validar la versión de la imagen del contenedor que se esta corriendo en el pod.

Nuevamente, las actualizaciones en k8s conservan un enfoque declarativo limpio que hace sencillo implementar cambios dentro de los pods. Es cuestion de organizar unos cuantos comandos del `kubctl`.


## Outro {#outro}

Esta última publicación se explicaron los temas de escalamiento y actualización de clusteres en la nube. Al usar estas funcionalidades de k8s, las aplicaciones en la nube son más elásticas y finalmente pueden ser llamadas listas para producción.

Recuerde que el objetivo es el diseño de aplicaciones modernas a través de paquetes decorativos y contenedores distribuidos.
