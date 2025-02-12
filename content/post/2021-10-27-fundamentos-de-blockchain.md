+++
title = "Fundamentos de Blockchain"
description = "Serie que recopila contenidos sobre blockchain"
date = 2021-10-27T02:13:50Z
author = "Sergio Benítez"
tags = [
    "blockchain",
]
+++

Tiempo de explorar los componentes fundamentales del Blockchain. Para ello se va a romper lo que inicialmente puede parecer una caja negra y reconocer así el sistema subyacente al Blockchain. Es importante tener unas bases fuertes sobre dichos conceptos ya que en el futuro ayudarán a entender con facilidad los conceptos avanzados.

Para aterrizar estos conceptos, se va a implementar un proyecto que consiste en la creación y administración de identidad en un Blockchain. Para ello se va a estar revisando con frecuencia la siguiente gráfica conocida como el Blockchain Framework:

![img](../../images/blockchain/01-blockchain-framework.webp "Blockchain Framework")

El Blockchain Framework provee una vista general de los componentes claves del Blockchain, y puede ser interpretado como un mapa de ruta para el entendimiento de Blockchain.

Al ser Blockchain una tecnología nueva, es importante dedicar tiempo en el entendimiento de sus fundamentos. Hay que tener en cuenta que Blockchain es una tecnología que todavía se esta explorando y por ende sus avances vienen respaldados por una serie de reglas y regulaciones sobre el protocolo. Con el proyecto de identidad en Blockchain se va a reforzar el entendimiendo del protocolo, ya que motiva la uso de servicios que se van a crear en una aplicación para lograr establecer identidad en los servicios de Blockchain.

El Blockchain empezó como una forma para intentar resolver algunos problemas vistos en las transacciones financieras. Por lo tanto, es importante revisar en que consisten estás transacciones y discutir las soluciones que Blockchain ofrece.

## Transacciones Financieras

Para entrar al contexto financiero es importante hacerse las siguientes preguntas: ¿Qué hace que el dinero sea valioso? ¿Por qué la gente lo usa? ¿Hay alguna forma de mejorar el sistema acutal?. Para responder estas preguntas es necesario descomponer como funciona el sistema financiero actual.

Imagine un contexto financiero en donde no se ha realizado ninguna transacción. El objetivo es crear un sistema que le permita a la gente hacer transacciones. Para empezar, considere que cada persona tiene un tipo de producto o servicio y hay personas que necesitan estos productos y servicios. Para acceder a dichos productos las personas que lo ofrecen esperan algo a cambio por dicho producto. El problema inicial es que no se tiene ningun mecanismo para hacer transacciones entre ellos. Para solucionar esto se puede empezar a comerciar. Primero se debe dar un valor al producto que una persona esta ofreciendo. Hecho esto ya se esta en capacidad de negociar el producto con los demás.

Este es un buen comienzo pero ahora se enfrenta un nuevo problema; nadie podrá ponerse de acuerdo con el valor exacto del producto o servicio que se esta ofreciendo. Para ello, hay que valerse de algo en lo que todos estén de acuerdo y tenga cierto valor intrínseco, algo como el oro empieza a tener protagonismo en el sistema de transacciones. Si se puede definir el valor de un producto en términos de cierta cantidad de oro, se podrá definir el valor de todos los productos basados en la referencia del oro, y yendo más alla, se puede hacer una divisa para empezar a negociar en lugar del oro. Es decir que ahora se tiene dinero.

Con el dinero en contexto, es necesario que alguien empiece a rastrear y administrar las transacciones que se están haciendo en el sistema. Es aquí donde los bancos entran. Los banco mantienen la traza de las transacciones para conservar el dinero seguro y protegido. Su figura se conoce como un *tercero de confianza*, que en pocas palabras es una entidad que facilita la interacción entre dos partes (Esta figura va a tener protagonismo en el contexto del Blockchain). A través del banco se establece la seguridad que se necesita para almacenar el dinero y confiar en las transacciones.

Esto es algo valioso y los banco lo logran al manejar un *libro mayor* con la lista de todas las transacciones registradas. La información en este libro mayor contiene a la persona que envía el dinero, la persona que lo recibe ,cuando ocurrio y el monto enviado. La siguiente imagen resume la figura del *libro mayor*:

![img](../../images/blockchain/02-current-financials-transactions.webp "Transacciones financieras actuales")

Este libro mayor nos permite saber quién tiene dinero, quién debe dinero y cuanto dinero existe. Esta información puede ser utilizada para fomentar la confianza entre la gente que esta haciendo transacciones sin necesidad de conocerse entre ellos. Este mecanismo ayuda a resolver el problema del doble gasto, el cual sucede cuando una persona gasta la misma cantidad de dinero en más de una ocasión. En un sistema financiero prevenir este problema es importante ya que evita que la gente duplique dinero. Por mucho tiempo se ha usado el libro mayor como mecanismo de defensa al problema del doble gasto, ya que se pueden invalidar las transacciones duplicadas. No obstante, Blockchain ofrece una alternativa diferente para afrontar el problema del doble gasto.

### Problemas con el sistema actual

Estos son los problemas que presenta el sistema financiero actual: No hay una forma fácil de ver si las transacciones han sido manipuladas. Por otra parte, el tiempo de la transacción depende de los bancos para validar las transacciones.

Evidentemente se pueden afinar estas características y se recuerda que Blockchain fue creado con el objetivo de mejorar el sistema financiero. Su propuesta inicial es tratar de remplazar los bancos como un tercero de confianza. Si esto funciona o no, es algo que todavía se esta definiendo.

En detalle esta es la propuesta de Blockchain: Actualmente todos los bancos recopilan toda la información que es crucial en el sistema financiero y también tienen control total sobre los datos que se manejan allí. Es decir, tienen acceso a todos los movimientos de nuestro dinero y están en capacidad de escoger si quieren compartir esta infomación con sus clientes. Quizás sería mejor que todo el mundo tuviera acceso a estos registros y en vez de tener un libro mayor mantendio por el banco, se podría crear un libro mayor compartido al cual todo el mundo pudiera acceder. De ser esto posible, todo el mundo tendría control completo sobre su información.

En resumidas cuentas, el Blockchain hace posible que se puedan omitir los terceros en las transacciones, haciendo que el dinero se envíe más rapido y por otra parte se reducen los impuestos asociados al simple hecho de hacer una transacción.

Algo que se debe anotar, es que todavía no se sabe si esta propuesta funciona realmente. No obstante, se ha manifestado un maravilloso progreso hasta ahora. Se resalta que las propuestas de Blockchain estan en construcción. Blockchain no es una solución definitiva, es más bien una alternativa que debe ser contemplada y probada para validar su efectividad. La invitación esta abierta a hacer parte de esta solución. 

## Introducción al Bitcoin

La cadena de bloques es un tema enorme que abarca muchas plataformas e industrias. Con tanto que aprender y las nuevas actualizaciones que ocurren todos los días, es difícil saber por dónde empezar. Creemos que el mejor lugar para comenzar es donde todo comenzó: ¡con Bitcoin!

Los conceptos e ideas que desarrolló Bitcoin continúan influyendo en todas las demás cadenas de bloques. Por esa razón, usaremos bitcoin como una forma de ayudar a recorrer algunos conceptos importantes del programa.

### ¿Qué es Bitcoin?

El Blockchain empezó con su primera implementación usando un nuevo tipo de moneda conocido como Bitcoin.

> Bitcoin es una moneda digital que utiliza el Blockchain para facilitar las transacciones finacieras

Bitcoin sentó las bases para otras ideas subyacentes a otras cadenas de bloques. Por lo tanto entedende com funciona Bitcoin es un paso importante para entender las ideas que se están presentando sobre todas las industrias.

Bitcoin usa la idea de bloques para agrupar y validar transacciones. Un bloque es un grupo de transacciones. Bitcoin es una secuela de un árticulo publicado en 1991 por Stuart Haber y Scoot Stornetta llamado [How to Time-Stamp a Digital Document](https://www.anf.es/pdf/Haber_Stornetta.pdf) en donde se presenta la idea de agrupar, validar y enlazar documentos. En dicho árticulo también se explica como esta estructura forma una cadene de docuemntos al dia completamente validados.

En 2008, un autor bajo el alias de Satoshi Nakamoto publicado un árticulo llamado Blockchain: [A Peer-to-Peer Electronic Cash System](https://bitcoin.org/bitcoin.pdf) en donde se comparte todas las bases del Bitcoin que se conoce hoy en día. Este árticulo cambio por completo la forma en la que la socieda entiende una  divisa. El 3 Enero del 2009, Nakamoto liberó el software de Bitcoin, Desatando los primeros Bitcoins al mundo. En dicho árticulo se comparte los fundamentos del Bitcoin, Blockchain y otro tipo de criptomonedas. Por lo tanto se recomiendo realizar la lectura del árticulo teniendo en cuenta las siguientes preguntas:

-   ¿Cuál es el problema que Bitcoin soluciona?
-   ¿Qué soluciones ofrece Bitcoin?
-   ¿Qué componentes son usados para desarrollar es sistema de Bitcoin?

Muchos de los conceptos expuestos por Nakamoto se extienden del contexto de Bitcoin y las criptomonedas, puesto que estos conceptos son la base de la construcción de un Blockchain robusta para cualquier aplicación.

Sorprendentemente, solo hasta que Bitcoin fue desarrollado se empezó a populizar el potencial de la tecnología subyacente al sistema, el Blockchain. Abstraer esta ideas a aplicacione diferentes a contexto finacieros y criptomonedas es como Blockchain esta en capacidad de empezar a transformar muchas industrias.

Una conclusión importante es: Bitcoin no es *el* Blockchain, es un Blockchain.

Actualmente hay muchas cadenas de bloques, e incluso se va a construir una propia. La idea es también repasar otras cadenas de bloques y plataformas. No obstante se comienza con Bitcoin porque una vez se comprendan estas ideas básicas, podrá aplicarlas en cualquier lugar.

## Hashing

Hasing es una idea que ya se menciono anteriormente. Básicamente es una forma de crear una huella dactilar digital para una pieza de datos. Es una idea fundamental para lograr que el Blockchain funcione, y ademas es un punto de partida para conectar dinámicas más avanzadas dentro de la cadena de bloque.

En la siguiente gŕafica se resalta la parte del framework que se va a a desarrollar en esta sección:

![img](../../images/blockchain/03-blockchain-hashing.webp "Hashsing Component")

Un *Valor Hash* es na huella digital para información. Es una cadena de caracteres de texto y números que representa un conjunto de datos. Estos datos se pasan a través de una *Función de Hashing*. Esta función mapea un grupo de datos a un valor único de hash. Ese valor hash se convierte en el único de acceso a los datos agrupados por la función. El término SHA256 corresponde a una funcíon de hash. SHA significa Secure Hashing Algorithm, y el 256 representa la salida del algoritmo. Para este caso el valor hash es un número en 256 bits. Bitcoin usa el SHA256 para crear un valor hash único al bloque que componen la cadena de bloques. La siguente gráfica muestra la relación de un bloque, con una función hash y un valor hash:

![img](../../images/blockchain/04-SHA256.webp "SHA256")

Los hashes son utilizados para referenciar bloques dentro de la cadena y también para identificar las uniones de bloques en el Blockchain. Para entender como funciona la cadena, es necesario entender como funcionan los bloques en si mismo, tema que se abordará en la siguiente sección.

### Hashing Demo

Ahora que se tienen una idea de como funcionan los valores hash, se puede hacer una pequeña demonstración. La idea es visitar [anders.com](https://andersbrownworth.com/blockchain/hash) y en la pestaña Hashs empezar a introducir contenidos aleatorios como textos y números. Los importante a tener en cuenta aquí es que cada vez quese modifica el contenido el valor del hash es actualizados y dicho valor es único. 

Eso concluye esta introducción al hash. Tenga en cuenta estas ideas a medida que avanzamos en la lección. Cada una de estas nuevas ideas se complementan entre sí. Es útil comprender cada uno de ellos de forma aislada para que esté mejor preparado para verlos como un sistema más adelante.

## Bloques

Los bloques son un componente fundamental del Blockchain. La palabla &ldquo;bloque&rdquo; es una forma apropiada de pensar como la información es almacenada. No obstante el bloque es un poco más complejo de lo que inicialmente se espera.Por lo tanto, en esta sección  se va a revisar que es un bloque puntualmente, ¿por qué son importantes? y las bases para entender cómo funcionan.

![img](../../images/blockchain/05-blockchain-blocks.webp "Blocks Component")

Un *Bloque* es un contenedor de una lista de transacciones que se agregarán a la cadena de bloques. Como se ha visto el Blockchain es una libro mayor digital compartido que registra una lista de transacciones que se realizar en una red. Tener una lista gigante de transacciones fomenta una gestión tediosa sobre la misma. La propuesta de Blockchain es separar esta lista gigante de transacciones en secciones pequeñas llamadas bloques. A medida de que las transacciones son hechas, estas van siendo empaquetadas en los bloques y agregadas al Blockchain. Al tener la separación de bloques se puede gestionar el sistema entero de manera más eficiente.

A demás de las transacciones, los bloques tambien contienen otro tipo de información. A mayor detalle el bloques se compone de dos partes: un encabezado y un cuerpo. En el cuerpo se guarda la transacciones como tal y en el encabezado se tienen detalles sobre la estructura de los datos que están dentro del bloques. la siguiente lista recopila los detalles que se encuentran en un encabezado del bloque:

-   Valor hash del bloque previo
-   El tiempo en el que el bloque fue hecho
-   La raíz Merkle
-   Nonce

El **valor hash del bloque previo**, se explica por si solo. Lo importante de esta información es que suministra lo necesario para identificar de manera sencilla que bloque viene después o antes de otro bloque en el Blockchain. En otras palabras, esta es la base que forma el Blockchain entero.

El **tiempo** en el que el bloque fue creado también se explica por si solo. Su uso corresponde a identificar de manera precisa cuando ciertas transacciones fueron ejecutadas y pude ayudar a atrapar casos como cuando alguién intenta gastar el mismo dinero dos veces, ya que la información registrada permite saber cual transacción fue válidbasado en cual se presentó primero. Este sello de tiempo es la solución al problema de doble gasto que se menciono en la primera publicación de esta serie.

**La raíz Merkle**, es el hash que representa cada transacción dentro del bloque. Para obtenerlo, un par de las transacciones que están dentro del bloque son agrupadas rápidamente y cada par se representa con un hash único. Luego el hash de las dos parejas de transacciones es nuevamente agrupado para generar otro hash único. Este proceso se repite recursivamente hasta lograr a un valor hash único con todas las transacciones dentro del bloque. Este valor hash final es conocido como la raíz de Merkle. Con este proceso, ahora es posible revertir cada uno de los valores hash dentro del bloque para realizar una búsqueda sobre la transacción original que se alamcenó en el bloque. Todo partiendo desde la raíz de Merkle.

Por último, El **Nonce** tiene más que ver con algunos temas de minería que se van a desarrollar más adelante, pero una introducción rápida es que el nonce es un número arbitrario que solo puede ser utilizado una vez. Cuando se crea un hash para un bloque no cualquier valor va a funcionar. El sistema solicitará un valor hash específico que comienza con cierto número de ceros. Estás restricciones adicionales hacen que el hash sea más díficil de encontrar. Para resolver este valor, se deben combinar todos los bloques de datos con el nonce para tratar de generar el valor hash correcto. Los computadores adivinan una y otra vez el nonce hasta que finalmente obtienen el valor que cumple con las restricciones. La cantindad de ceros al inicio del nonce es conocido como la dificultad del bloque.

Paralelo al encabezado, se tiene una información adicional que corresponde al tamaño del bloque. El tamaño del bloque es la cantidad de espacio que un bloque tiene para almacenar información, y funciona tal cual los limitantes de espacio que tienen los computadores o los dispositivos móviles. Este tamaño es definido por el desarrollador y ayuda a controlar cosas como cuánto tiempo tomará un bloque en completarse o cuántos bloques estarán en la cadena. Una vez decidido, todos los bloques en la cadena tendrán el mismo tamaño y solo podrá ser modificado cuando se actualiceel software.

El último dato adicional es de un bloque es un valor del hash de si mismo, el cuál servirá para identificarlo en el futuro. Si los datos del bloque son modificados, un nuevo valor hash será generado. Esto es importante ya que permite saber cuando un bloque ha sido manipulado, haciendo así el Blockchain una forma segura para mantener el historial de la información que usted está compartiendo.

### Blocks Demo

Ahora que se tiene una idea básica de como los bloques funcionan, es tiempo de revisar una demostración simple. Para ello, se invita a navegar nuevamente a la página [anders.com](https://andersbrownworth.com/blockchain/block) pero esta vez visitando la pestaña Block.

En está página, se va a mostrar un formulario dentro de un contenedor que inicialmente aparecerá con un fondo rojo. Si se revisan los campos de formulario estos corresponden a las partes del bloque que se especificaron previamente. La invitación es a agregar un contenido dentro del campo **Data** y luego dar clic en el botón **Mine**. Este evento cambiará el fondo del contenedor a verde indicando que el bloque, es un bloque válido. Si se actualiza el contenino de el campo **Data** se osberva que el contenedor vuelve al fondo rojo, indicado que el bloque es ínvalido. Básicamente, este es el comportamiento esperado de los bloques. Recordemos que cualquier modificación cambiará el valor hash del bloque, teniendo así un historial transparente de la información dentro del bloque.

Con el entendimiento de los bloques el siguiente paso es responder cómo los valores hash y los bloques funcionan en conjunto para crear el Blockchain.

## Blockchain

Cómo probablemente ya se habrá notado, la palabra &ldquo;blockchain&rdquo; agrupa todos los componentes que se ilustraron en la imagen del Blockchain Framework. No obstante, específicamente el Blockchain es el lugar donde los datos son almacenados. Todos los otros componentes son el sistema al rededor del Blockchain que ayudan a lograr su funcionamientos.

![img](../../images/blockchain/06-blockchain-blockchain.webp "Blockchain Component")

El Blockchain es un libro mayor digital que contiene el historial entero de las transacciones hechas en una red. Es la acomulación de bloques enlazados unidos entre sí por valores hash que se van creando sobre el tiempo. Toda la información en el Blockchain es permanente y no puede ser cambiada, lo que significa que los datos son inmutables.

Para construir el Blockchain se necesitan dos cosas: bloques y valores hash.

![img](../../images/blockchain/07-blockchain-composition.webp "Blockchain Parts")

Como se mencionó en la sección de bloques, en el encabezdo cada bloque tiene su valor hash y el valor hash del bloque previo. Estós valores hash encadenan los bloques desde el bloques más reciente hasta el primer bloque creado. Al estar los bloques enlazadaos con valores hash, se obtienen unas características interesantes. Primero, se sabe que si se cambian los datos en un bloque eso va a crear un nuevo valor hash para e bloque, haciendolo inválido y compartiendo que el bloque ha sido cambiado. Este comportamiento se replica cuando el bloque hace parte de la cadena e invalida todos los bloques siguientes al bloque cuyos datos fueron modificados. La siguiente imagen ilustra este comportamiento.

![img](../../images/blockchain/08-blockchain-invalid-chain.webp "Blockchain Invalid Chain")

Para identificar la secuencia de los bloques, se tiene el número del bloque el cual indica el orden de llegada del bloque a la cadena. El primer bloque de la cadena, también es conocido como el bloque genésis.

### Blockchain Demo

Con las bases adquiridas del componente Blockchain es tiempo de atender otra demostración. La idea con las demostraciones es interectuar desde temprano con las ideas básicas del componente Blockchain.

El demo puede seguirse desde las pestaña Blockchain de la página [anders.com](https://andersbrownworth.com/blockchain/blockchain).

Nuevamente se va a observar el formulario del bloque pero emulado en una cadena. La invitación aqui es agregar diferente información en los cuerpos de los bloques y marcar los bloques como válidos al dar clic en el botón **Mine**. Una vez se tengan los tres primeros bloques validados, se va a cambiar el cuerpo del bloque número dos. Al hacer este cambio se nota que tanto el bloque número dos como los bloques posteriores se vuelven inválidos al obtener un fondo rojo en el contenedor.

Con las bases del componente Blockchain la siguiente pregunta a resolver es ¿dónde se almacena la información? La respuesta es interesante ya que la información se en una red de usuarios que tienen su propia copia de la cadena de bloques. Ningún usuario es propietario de los datos, todos tienen acceso y todos pueden participar. Esto es asombroso y para revisar como funciona dicha red se debe revisar las redes distribuidas de igual a igual.

## Redes Distribuidas de igual a igual (Peer-to-Peer a.k.a P2P)

La idea de una red en el contexto de Blockchain permite que la cadena de bloques eluda la necesidad de terceros como se mencionó anteriormente. En esta sección, se repasará cómo sucede eso ¿qué es la red? y ¿por qué es tan importante para hacer una cadena de bloques efectiva?

![img](../../images/blockchain/09-blockchain-network.webp "Blockchain Network")

Como se puede observar, este componente se forma de dos conceptos que son convenientes evaluarlos de manera independiente: **redes distribuidas** y **redes peer-to-peer**.

Una red peer-to-peer es una red de computadores que permite compartir información entre usuarios. Los usuarios, conocidos como nodos, pueden enviar información de manera directa entre ellos, sin tener dependencia de una autoridad central para mantener la información. Por lo tanto, es una forma para conectar usuarios que están en capacidad de compartir información. Un ejemplo cercano de una red P2P es la red sobre la que operan nuestros télefonos móviles. No obstante Blockchain no es solo una red P2P, también es una red distribuida.

Una red distribuida es una red que permite que la información se difunda entre muchos usuarios. Mientras que una red P2P permite comunicación abierta entre usuarios, la red distribuida permite que la información en si misma pertenezca a dichos usuarios. Para entender las redes distribuidas es de gran ayuda compararla con los tipos de red que se muestra en la siguiente imagen:

![img](../../images/blockchain/10-network-types.webp "Network Types")

Antes de empezar con la revisión detallada de cada una de las redes, se debe tener presente que cada tipo de red tienes sus propios casos de uso y fortalezas. El Blockchain es una red distribuida porque sencillamente es el mejor tipo de red que se adapta a su propuesta.

En una red centralizada todo se conecta a un propietario central. El propietario puede ser una persona, una compañia o una base de datos. Aquí hay una única fuente que contiene toda la información del sistema. Para imaginar este tipo de red, piense en una biblioteca que contiene todos los libros del mundo. Cada vez que se quiere obtener información es necesario ir a esta biblioteca. Esta puede ser una forma efectiva y bastante común de compartir información. No obstante, si la biblioteca es destruida, toda la información se perdería. En las redes centralizadas es muy complicado recuperar la información en caso de perdida. Otro detalle encontra es la velocidad. Para obtener el libro, se tiene que ir directamente a la biblioteca sin importar en que parte del mundo usted se encuentra.

Para atender esas falencias se creo una red descentralizada. En una red descentralizada no hay un único punto donde toda la información existe. En su lugar, toda la información es difundida a lo largo de multiple ubicaciones. Por lo tanto, en vez de tener todos los libros en una sola biblioteca, se tienen muchas librerías dispersadas en la ciudad. Duplicados de cada libro pueden existir en cada biblioteca. Si una biblioteca es destruida, se van a perder algunos recursos pero no se va a perder toda la información. Es posible tener respaldo de los libros en otras ubicaciones. Al tener las bibliotecas difundidas por la ciudad también se van a tener accessos más rápidos a los libros, ya que los usuarios podrán ir a la biblioteca más cercana.

Por último una red distribuida es una red centralizada forzada a sus límites más lejanos. En un sistema distribuido todos tienen una copia de la información, con el mismo accesos y el mismo control que cualquier otro. No hay ningún tipo de centralización. Extendiendo el ejemplo de las bibliotecas, imagine que una biblioteca va a contener 50 libros, la cantidad de libros necesaria para llenar un estante personal. Ahora, un grupo de personas va a obtener una copia de esos 50 libros para ponerlos en sus estantes personales. De esta forma se crea una red donde todos tienen una acceso rápido a la información que ya existe. Esta es la idea detrás de las redes distribuidas y ya no habría necesidad de ir a una biblioteca para conseguir los libros.

Regresando al Blockchain, esta descripción es importante por que en sí el Blockchain es una red distribuida que le da a todos sus usuarios propiedad de la información permitiendo la descarga de una copia del Blockchain en sus computadores locales, otorgando acceso completo a la infomación contenida en la cadena de bloques. De esta forma se elude la dependencia a terceros.

La verdad, resulta divertido visualizar lo que sucede en la red del Blockchain. La forma en que la información es compartida entre la red distribuida de igual a igual resulta muy interesante puesto que aquí se manifiesta la forma en como las transacciones se manejan y se incorporan a la red.

Antes de que las crear la red, y eventualmente el Blockchain, las transacciones son contenidas en el pool de memoria. Este componente será revisado en la siguiente sección.

## Grupo de Memoria

Antes de ingresar al Blockchain o hacerse parte de la red, las transacciones entran en un grupo de memeoria.

El grupo de memoria (aka mempool por su nombre en inglés) es el lugar de espera de las transacciones sin confirmar antes de entrar al Blockchain. La cadena de bloques solo puede manejar cierta información a la vez, y la acumulación de información va aquí, en el mempool. Es tiempo de revisar como funciona el grupo de memoria y entender qué lo hace importante en el ecosistema del Blockchain.

![img](../../images/blockchain/11-blockchain-mempool.webp "Blockchain Network")

En cualquier red, las transacciones suceden a medida de que los usuarios las ejecutan. No obstante, eso no significa que la red este en capacidad de antender todas las transacciones al mismo tiempo. En otras palabras, el número de transacciones que se hacen se someten al número de transacciones que una red puede operar. Para mantener está dinámica es necesario contar con una línea de espera conocida como el mempool.

Su nombre se debe al hecho de que las transacciones se ubican en la memoria RAM de todos los nodos de la red Blockchain. Por consiguiente, cuando se empiezan a hacer transacciones en el Blockchain estás no suceden instantaneamente. Hay un tiempo de espera dedicado a determinar si la transacción ha sido verificada por la red. Esta confirmación es hecha por nodos especiales en la red de Bitcoin llamados mineros. Al validar las transacciones antes de ser agregadas al Blockchain, los mineros ayuda a dar garantias sobre el consenso en el Blockchain. La minería es un tema que se verá más adelante.

Para visualizar de forma efectiva el mempool, se hace la invitación a navegar los siguientes recursos en Blockchain.com:

-   [Unconfirmed Transactions](https://www.blockchain.com/btc/unconfirmed-transactions)
-   [Blockchain Data Charts](https://www.blockchain.com/charts)

Una transacción deja el mempool cuando un minero lo agrega a un bloque. Este proceso garantiza la seguridad y la validación en la cadena de bloques. Sin embargo hay otras razones por las cuales una transacción deja el mempool.

-   La transacción expira por tiempo de espera (14 días luego de haber entrado al grupo de memoria)
-   La transacción estaba en la parte inferior del mempool (cuando se ordenó por tarifa por tamaño), el mempool alcanzó su límite de tamaño y se aceptó una nueva transacción con una tarifa más alta, desalojando la transacción en la parte inferior.
-   La transacción o uno de sus antepasados ​​no confirmados entra en conflicto con una transacción que se incluyó en un bloque.

El mempool es un espacio temporal para revisión antes de enviar la información de manera permantente al Blockchain. Su propóposito es proveer seguridad en las transacciones. Una vez la transacción se incluye en el bloque, dicho bloque se va a confirmar seis veces. Es decir, cinco bloques adicionales van a agregar a la misma cadena y es aquí donde el bloque se categoriza como irrevocable y va a requerir una cantidad considerable de computación para invalidar o recalcular el acuerdo de estos seis bloques. Es así como el Blockchain propone una fuente de verdad transparente segura e inmutable.

En este punto, se discutió qué es el grupo de memoria, por qué es importante y cómo ayuda a contribuir a las transacciones que ingresan a la cadena de bloques.

¡Comprender el hash, los bloques, las cadenas de bloques, las redes y ahora las mempools es un gran paso!

Las preguntas que puede tener en este momento son:

-   ¿Quién toma las decisiones por sobre el mempool?
-   ¿Quién decide cuándo una transacción es válida o qué datos pertenecen a un bloque?

Estas preguntas se responden en el Consenso. Es aquí dodne todas las decisiones para establecer y hacer crecer el Blockchain son contempladas.

## Consenso

El consenso es un tema extenso. No se abordará en su totalidad, pero se va a hacer un enfásis en las piezas que son importantes para ser un desarrollador Blockchain exitoso.

![img](../../images/blockchain/12-blockchain-consensus.webp "Blockchain Consensus")

El Consenso es la forma como Blockchain toma decisiones. En principio es una idea, pero esta idea es implementada a través de algoritmos diferentes. Todos estos algoritmos son formas diferentes de intentar lograr un consenso de manera más eficaz. Cosas como prueba de trabajo, prueba de participación y DBFT son algoritmos de consenso que se discutirán a continuación.

### Problema de los Generales Bizantinos

Las bases de un consenso se encuentran en el problema de los generales bizantinos. Por lo tanto es relevante revisar en qué consiste el problema ¿cómo se soluciona? y ¿cómo ayuda a que una red logre un consenso?

El problema es el siguiente: Nueve generales que comandan una porción equitativa del ejercito Bizantino rodean una ciudad y necesitan formar un plan para atacarla. Los generales deben decidir si atacar o retirarse. La advertencia es que los nueve deben estar de acuerdo en atacar o retirarse. Si solo un grupo de los generales decide atacar y los otros se retiran serán sobrepasados y perderan la batalla. Además hay otro tipo de complicaciones. De los nueve generales, podría haber algunos traidores que están en capacidad de interrumpir la votación del grupo. Por ejemplo si cuatro de los nueve generales apoya el ataque, y los otro cuatro apoyan la retirada, el general restante puede tener una mala intención y estropear todo. Los generales están separados físicamente y su voto se envía a través de mensajeros. No obstante, los mensajeros están expuestos a ser capturados. Por tanto corren el riesgo de no poder entregar el voto, o crear un voto falso.

![img](../../images/blockchain/13-byzantine-generals-problem.webp "Byzantine General&rsquo;s problem")

Este problema puede ser traspolarse al contexto de Blockchain, en donde los generales son reemplazados por nodos, y en vez de hablar de una guerra, se habla de la red Blockchain. La misma idea se traslada en ambas situaciones, es necesario garantizar confianza entre los usuarios de una red distribuida cuando no tienen una forma efectiva de comunicación. La propuesta de Bitcoin para solucionar el problema es el uso de un algoritmo de prueba de trabajo. Es una forma de lograr consenso sin una autoridad central, hecho que le permite a Bitcoin solucionar el problema del doble-gasto. por lo tanto es tiempo de entrar a revisar en que consiste el algoritmo de prueba de trabajo.

## Prueba de trabajo

Uno de los primeros algoritmos creados para consenso es el de **prueba de trabajo** el cual fue propuesto por Bitcoin para lo solución del problema de los generales Bizantinos. La idea detrás de la prueba de trabajo es que quién ponga más trabajo para contribuir en el sistema es el más digno de confianza. Para ello el sistema debe determinar que la información puede ser costosa de producir, pero fácil de verificar.

En el contexto de la prueba de trabajo, hay un costo inicial de recursos conocido como trabajo enfocado a generar el valor hash del bloque. Este trabajo puede ser fácilmente validado por el resto de la red al revisar que fue hecho de manera correcta.

En Blockchain, cada nodo de la red esta involucrado en resolver un problema para demostrar que ha realizado un trabajo previo. Haber puesto tiempo en hacer este trabajo es señal para que el sistema pueda confiar en el resultado del trabajo realizado. Los nodos que intentan resolver el problema son conocidos como mineros. Minar para encontrar la solución de estos problemas va a requerir un gran costo de energía computacional. Estos mineros están constantemente en una carrera para resolver cada problema tan rápido como puedan y así poder construir el siguiente bloque. En retorno de su tiempo y recursos se les pagan tarifas de transacción que vienen directamente de los usuarios que están haciendo las transacciones. En el caso de Bitcoin, la red los remunera con Bitcoins que son creados específicamente como recompensa por minar el nuevo bloque. Estas nuevas monedas creadas en la minería es la única forma de introducir monedas nuevas en el sistema.

La carrera de los mineros para resolver el problema merece una revisión más profunda. Los mineros tratan de encontrar el Nonce de cada bolque nuevo. El Nonce ya se reviso en la sección de Bloque, pero no esta de más hacer una recapitulación para tener más contexto sobre el porqué fue hecho. Cuando se crea el valor hash de un bloque no cualquier valor va a funcionar. El sistema solicita un valor hash muy específico que empieza con cierto número de ceros. Estas limitaciones adicionales hacen que el hash sea mucho más díficil de encontrar. Para encontrar ese valor, se deben combinar todos los datos en los bloques con los nodos para tratar de generar el valor hash correcto.Los computadores adivinan el Nonce una y otra vez hasta que finalmente dan con el valor has que cumple con todas las limitaciones establecidas. La razón por la cual esto es importante se verá cuando se aborden temas de minería.

#### Demo de una prueba de trabajo

Con las bases descritas anteriormente sobre como el algoritmo de prueba de trabajo ayuda a manejar el consenso, puede ser de utilidad verlo en acción. Para ello la invitación es nuevamente visitar la página de [anders.com](https://andersbrownworth.com/blockchain/block) en la pestaña de Block.

En esta página, agregue un contenido al cuerpo del bloque en el campo Data. Al introducir algún contenido el bloque queda inválidado ya que se necesita encontrar un hash para el mismo. Para este caso en vez de dar clic en Mine, se va a enfocar el Campo Nonce del formulario. El número en el Nonce, es el número que el computador estra tratando de encontrar para resovler el problema requerido por el algoritmo de prueba de trabajo. No es necesario tener un computador para resolver esto, pero con un computador será mucho más rapido. Puede intentarlo usted mismo actualizando el contenido del campo Nonce. Para ello introduzca el número 2 en el formulario. Como se podrá observar el hash es actualizado. Para poder encontrar el valor hash del bloque se tiene que cambiar el Nonce hasta que el Hash tenga un número 0 de lectura al comienzo. Para el caso del número 2, se obtiene un 5 y por ende no nos funcionará. Si se actualiza el valor del Nonce por 3, se va obtener un 0 al inicio del valor del hash. No obstante, el problema se resuelve cuando el valor hash inicie con cuatro ceros seguidos. Es por esto que los nodos mineros tienen que probar muchas combinaciones aleatoreas hasta lograr encontrar el valor hash del bloque.

Entre más ceros a la izquierda se necesiten, más dificultad tendrá la búsqueda del Hash. Esto se debe a que un has con más ceros indicaáa una solicitud más específica. Por lo tanto mayour sera el tiempo invertido para adivinar a combinación apropiada. Estos ceros a la izquierda es lo que se conoce como la dificultad del bloque.

En Bitcoin, la dificultad del bloque se ajusta de automaticamente para garantizar que un nuevo bloque se crea cada 10 minutos. Si el bloque se crea muy rápido, entonces la dificultad se incrementa para relentizar el proceso. Pero si la creación es muy lenta, entonces la dificultad se disminuye para hacer el proceso de prueba de trabajo más rápido. Los 10 minutos establecidos para crear un bloque fue una decisión tomada por los desarrolladores. Este rango de tiempo se considera un buen balance entre tener una red segura que pueda mantener la creación de nuevos bloques.

No obstante, el algoritmo de prueba de tabajo tiene algunos inconvenientes que se pueden mejorar, en la siguiente sección se revisarán estos puntos.

#### Problemas de la prueba de trabajo

Prueba de trabajo fue la primera implementación para el consenso del Blockchain propuesto y ejecutado por Bitcoin. Recordemos que el algoritmo de consenso es importante en el Blockchain para garantizar una red digna de confianza y una versión de verdad. Ademán también previente adversidades potenciales para romper el sistema. No obstante el algoritmpo de prueba de trabajo presenta los siguientes problemas:

1.  Consumo de energía extremadamente alto
2.  El monopolio de mineros genera preocupación por centralización

El consumo de energía elevado se debe a que los cálculos de la prueba de trabajo requieren de mucho poder computacional, implicando costos tanto financieros como ambientales. Para minar Bitcoin, se usan plataformas mineras. Es decir computadores de alta potencia para procesar nuevos bloques. Estas plataformas pueden hacerse en hogares pero tambien existen granjas mineras de Bitcoin. En 2017, una de las minas más grandes de Bitcoin ubicada al interior de Mongolia contaba con 25 mil máquinas trabajando las 24 horas al día para minar Bitcoin. Se dice que la minería de Bitcoin esta consumiendo más electricidad que 159 países al rededor del mundo. En 2017, la se registro una estadística de que la energia utilizada para minar Bitcoins usaba más energia que dos millones de horas en Estados Unidos.

El monopolio de minería se debe también al alto consumos que requiere el algoritmo de prueba de trabajo. Los recursos para abastecer esta demanda de energía representa una ventaja injusta. El sigueinte diagrama muestra la distribución de los grupos mineros mas grandes al rededor del mundo:

![img](../../images/blockchain/14-pooled-mining.webp "Pooled Mining")

Como se puede observar esta distribución es sesgada. Estos grandes grupos mineros están en capacidad de controlar la mayor parte de la red ya que al tener los nodos que ofrecen más recursos a la red tienen más participación en la definición de un bloque válido. Por lo tanto se proporciona una tendencia a tener una red centralizada en vez de una red distribuida, que fue la intención original.

Para resolver estos retos, nuevos algoritmos se ha propuesto. Este sera el tema de la siguiente sección.

## Prueba de Participación

La prueba de participación es otro algoritmo utilizado para lograr el consenso en un blockchain. La idea principal detrás de la prueba de participación es que se enfoca en dar votos para los miembros, dependiendo en que tanta participación ellos tienen para el éxito de la cadena.

La participación en este caso significa aquellos nodos que tienen mayores inversiones en el sistema y pretenden  salidas positivas del mismo. Se consideran candidatos para realizar una votación y así generar un impacto positivo para el sistema. En el algoritmo de prueba de participación no se tienen mineros y en su lugar hay validadores. Los validadores no requieren de costos computacionales ya que su función como interesados es determinar qué bloque entrará en la cadena de bloques. Con el fin de validar transacciones y crear bloques, los validadores ponen sus propias monedas como apuesta. Si ellos validan una transacción fraudolenta, ellos pierden sus posesiones y el derecho al rol de validador en futuras votaciones. En teoría, esta revisión incentiva a que el sistema valida solo transacciones de veraces. Un validador que tenga una apuesta de 400 monedas sobre un bloque, tiene más participación que un validador con una apuesta de 100 monedas por otro bloque.

Ahora bien, el validador puede distribuir sus monedas en todos los bloques para asi asegurar una ganancia en la apuesta. Esta opción es conocida como *nada en juego*, y es un problema para la prueba de participación. Hay un inconveniente en Blockchains que se extieneden bajo el paradigma de prueba de participación que no se ha mencionado, y es la posibilidad de que la cadena se bifurque. En determinado momento dos bloques pueden recibir la misma instrucción de ser agregados a la cadena y crear dos secuencias de cadena de bloques. Por otra parte, puede haber un intento malicioso de reecribir el historico y reversar la última transacción.

Afortunadamente, los partidiarios de la prueba de participación tienen estrategia en contra de este tipo de ataques. Una estrategia es el *Slasher Solutoin* lo que implica penalizar a los validadores si crean simultáneamente bloques en múltiples cadenas. Otra alternativa es que los protocolos tienen sus mecanismos de penalización para validadores que crean bloques en la cadena equivocada. Por lo tanto se incentiva a que los validadores sean conscientes sobre los Blockchain en donde van a ubicar su participación.

A continuación se comparte una lista de las plataformas que actualmente usan la prueba de participación:

-   [Ethereum](https://ethereum.org/en/), cambio de PoW a PoS en el proyecto casper
-   [DASH](https://www.dash.org/), un pionero de PoS
-   [LISK](https://lisk.com/), una herramienta enfocada en la creación de aplicaciones descentralizadas.

La prueba de participación es un algoritmo popular para lograr consenso, pero hay otras alternativas. Una de ellas es **Tolerancia Delegada a Fallas Bizantinas** (DBFT por sus siglas en inglés) y sera indagando en la siguiente sección.

## DBFT

La **Tolerancia Delegada a Fallas Bizantincas** o DBFT en corto es otro algortimo importate de consenso. A diferencia de la prueba de participación, DBFT intenta lograr el consenso asignando roles que ayuden a coordinar el consenso.

Para lograrlos, DBFT no cuenta con mineros y en su lugar hace una división entre nodos ordinarios y nodos de consenso. La mayoria de los nodos en la red son ciudadanos ordinarios que pueden intercambiar activos pero no participan en la validación de bloques. Los nodos de consenso tienen el poder de validar cada bloque escrito en el Blockchain y representan a los otros nodos que compoenen la red. Una analogía válida corresponde a los oficiales gubernamentales elegidos para representar a la mayoria de los constituyentes.

![img](../../images/blockchain/15-dbft.webp "How works DBFT")

Para que un nodo ordinario se convierta en un nodo de consenso es necesario cumplir con algunos criterios que varian de una plataforma a otra. Algunas pueden incluir el uso de un equipo especial para tener una conexión a internet dedicada, ó tener cierta participación en la red.

Los nodos de consenso hacen un seguimiento sobre los bloques que se propuestos para ser agregados a la cadena de bloques. Cuando es tiempo de decidir cuál sera el siguiente bloque a agregar un nodo de consenso de todo el grupo de nodos de consenso es seleccionado de manera aleatoria y se le delega la función de *expositor* y los otros nodos quedan como *delegados*. El expositor crea un bloque nuevo y lo propone a los delegados. Dos tercios de los delegados necesitan aprobar este bloque antes de pasarlo al Blockchain. Si el bloque no pasa, el nodo expositor vuelve a ser un delegado y se elige otro expositor para que realice una nueva propuesta de bloque.

DBFT es muchos más rápido que la prueba de trabajo ya que requiere de menos recursos y no hay rompecabezars de cifrado por solucionar. Por otra parte es resistente a la bifurcación de cadena, ya que en cualquier momento solo habrá una versión de la verdad.

No obstante, DBFT no es imune a todos los problemas. Los escenarios más potenciales son:

1.  Un expositor deshonesto
2.  Un delegado deshonesto

Para compensar estos problemas, algunas plataformas de Blockchain libera datos acerca de honestidad y funcionamiento de cada uno de los delegados y los expositores que van a participar en la votación.

Actualmente, NEO es el defensor y procursor del DBFT. Es una plataforma de criptomoneda diseñada para construir redes escalables de las aplicaciones descentralizads.

Si bien la prueba de trabajo y la prueba de participación son sin duda los algorítmos más populares para consenso, nuevos mecanismos siguen apareciendo. Se recuerda que el algoritmo de consenso logra su reputación por garantizar segurida para un periodo largo de tiempo. Por ejemplo, Bitcoin se ha mantenido como una red segura desde el 2009. A continuacón se comparte una lista de los diferentes algoritmos de consenso:

-   [PoW en el artículo de Bitcoin](https://bitcoin.org/bitcoin.pdf)
-   [PoS Ethereum FAQs](https://github.com/ethereum/wiki/wiki/Proof-of-Stake-FAQ)
-   [Protocolo de Consenso de NEO](https://steemit.com/neo/@basiccrypto/neo-s-consensus-protocol-how-delegated-byzantine-fault-tolerance-works)
-   [Prueba de Actividad](https://www.coinbureau.com/blockchain/proof-of-activity-explained-hybrid-consensus-algorithm/)
-   [Prueba de Quemadura](https://99bitcoins.com/what-is-proof-of-burn/)

Con esto dicho, no hay un mecanismo de consenso perfecto y probablemente nunca existirá. Pero es interesante ver como nuevos Blockchains viente con sus propios mecanismos y a su vez lograr entender los puntos a favor y en contra de dichas propuestas.

## Recapitulación

En esta publicación se discutieron las bases del Blockchain.

-   La Blockchain se está desarrollando rápidamente, con toneladas de nuevas herramientas e ideas que surgen todo el tiempo y sus bases parten de la industria financiera.
-   Bitcoin no es *la* blockchain, es *una* blockchain. Hoy en día, hay muchas blockchains.
-   EL Blockchain framework se componen de 9 componentes.
-   El Hashing es una forma para crear firmas digitales para un pedazo de datos.
-   Un Bloque es una forma interesante para pensar en como la información se almacena.
-   El Blockchain es una libro digital compartido que registra una lista de transacciones.
-   Una Red punto a punto es una red de computadoras que permite compartir información entre usuarios.
-   Una Red distribuida es una red que permite que la información se distribuya entre muchos usuarios.
-   El Grupo de Memoria (también conocido como mempool) es el lugar de espera para las transacciones antes de que ingresen a la cadena de bloques.
-   El Consenso impulsa todas las decisiones que se toman para establecer y hacer crecer la cadena de bloques.
-   Si bien la prueba de trabajo y la prueba de participación son definitivamente las opciones más populares, hay mecanismos más nuevos que están surgiendo y demostrando su eficacia.
-   No existe un mecanismo de consenso &ldquo;perfecto&rdquo; y es probable que nunca lo haya, pero es interesante ver que surgen criptomonedas más nuevas con sus propios protocolos y es importante comprender sus pros y contras.
