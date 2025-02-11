+++
title = "Gestion de Transacciones"
description = "Serie que recopila una aprendizaje sobre Blockchain"
date = 2021-11-10T02:13:50Z
author = "Sergio Benítez"
tags = [
    "blockchain",
]
+++

Los Blockchains son sistemas basados en transacciones, por lo tanto el tema de transacciones es uno de los más importantes.

En esta publicación se van a revisar de manera conjunta los conceptos que se mencionaron en los de llaves, bloques, billeteras, entre otros. El próposito de este árticulo es atacar los siguientes puntos:

-   Entender las mecánicas de una transacción simple entre dos entidades, usando llaves privadas, llaves públicas, billeteras y un Blockchain público.
-   Determinar y generar el tipo de billetera adecuado segun las necesidades de la situación (No determinística, Determinista Secuencial, Determinista Jerárquica).
-   Crear y restaurar una billetera Bitcoin usando llaves privadas.
-   Reconocer la firma de transacciones como parte del ciclo de vida de la transacción y describir tanto su propósito como su proceso.

En esta sección vamos a explorar el componente de billeteras del framework Blockchain:

![img](../../images/blockchain/16-blockchain-wallets.webp "Wallets")


## Identitdad Blockchain

Así como un número de seguridad social, una cuenta bancaria o una licencia de conducción, la identidad en Blockchain lo establece a usted mismo en el mundo de la cadena de bloques. Para revisar el cómo se maneja identidad en Blockchain se van a revisar 2 casos:

1.  Describir como una persona que posee dinero tiene acceso para gastarlo garantizando que su transacción no puede ser rastreada.
2.  Dicha persona tiene la posibilidad de compartir su identidad con los demás para poder hacer transacciones con los otros usuarios en la red. Este escenario específico se soluciona en el Blockchain a través de billeteras.

Por ejemplo, una billtera Bitcoin establece la identidad en la cadena de bloques para permitir transacciones dentro de la misma. Una billetera se compone de los siguientes tres elementos:

-   Una llave privada: Un número secreto que permite gastar Bitcoins desde su billetera.
-   Una llave pública: Un clave que se comparte de manera publica pero no se usa para gastar Bitcoins.
-   Una dirección de billetera: Un identificador único para su billetera.

La dirección de una billetera es un identificador único para su billetera. Esta dirección le permite al propietario de la billetera compartir con los otros usuarios de la red para enviar y recibir Bitcoin. Es por eso que en los detalles de una transacción uno de los items que conforman el bloque son las direcciones de las billeteras de las partes relevantes en la transacción. No obstante, es válido cuestionarse si al exponer dicha información va a ser posible poder hacer un rastreo sobre quienes hacen parte de la transacción. La respuesta es que no se puede y esto se debe a como la billetera se crea.

En la creación de una billetera tiene protagonismo la llave privada y la llave pública. Las billeteras pueden tener una o más llaves privadas asociadas y no deben compartirse con nadie. Esta llave secreta se convierte en una llave pública y se puede compartir con cualquier persona con el fin de recibir Bitcoins. Una llave pública se genera desde una llave privada a través de un algoritmo de seguridad. Es por eso que toda llave pública se enlaza a una llave privada. Su relación es llamativa ya que se puede compartir una llave pública sin comprometer la seguridad de la llave privada. Por otra parte, una llave privada puede hacer seguimiento sobre una llave pública. Por último, el algoritmo para generar la llave pública hace que no sea posible resolver la llave privada desde la llave pública. La siguiente imagen ilustra la relación entre una llave privada y una llave pública:

![img](../../images/blockchain/17-private-public-keys.webp "Private and Public Keys Relationship")

Con este contexto, es tiempo de ahondar en el concepto de billeteras:

## Billeteras

La identidad en la cadena de bloques se logra con herramientas importantes como billeteras, direcciones y llaves. Además de que tener diferentes alternativas en la selección de las herramientas también es posible implementarlas en diferentes formas. Estas implementaciones se reflejan en diferentes tipos de billeteras.

### Tipos de Billeteras

-   **Billeteras no determinísticas:** También conocidas como billeteras aleatorias son billeteras en donde las llaves privadas se generan desde números aleatorios.
-   **Billeteras determinísticas:** Son billeteras en donde las direcciones, las llaves privada y las llaves públicas se pueden rastrear hasta sus palabras originales
-   **Billeteras jerarquicas determinísticas:** Un tipo avanzado de billetera determinista que contiene claves derivadas en una estructura de árbol.

### Escenarios para billeteras

A continuación se inventan una variedad de escenarios para explorar. Con base en las definiciones anteriores, considere la situación y decida qué tipo de billetera podría ser la más adecuada.

- **Escenarion 1**: Quiero crear billeteras de papel derivadas de una clave maestra para poder almacenarlas y recuperarlas todas de forma determinista. ¿Qué billetera se usa para lograr esto?

__Respuesta_: Billetera Secuencial Determinista

- **Escenario 2**: Estoy creando un servidor web que vende widgets y quiero generar una dirección de transacción única para cada cliente habilitando un seguimiento de forma independiente. Quiero poder tomar una clave pública maestra y generar una secuencia de claves subpúblicas, cada una asociada con una transacción y colocarla en un servidor web público y asegurarme de que el servidor web no tenga claves privadas.

_Respuesta_: Billetera Jerárquica Determinista

- **Escenarion 3**: Se está auditando una cadena de suministro de calzado. Los auditores reciben una clave pública para que puedan ver todas las transacciones del subárbol, pero no pueden desbloquearlo. ¿Qué billetera se usa para lograr esto?

__Respuesta_: Billetera Jerárquica Determinista

- **Escenario 4**: Un sitio web de redes sociales de Blockchain utiliza claves privadas para proteger los datos personales de los usuarios. Utiliza billeteras para servicios de back-end que usan claves privadas que no se derivan de una semilla. ¿Qué billetera se usa para lograr esto?

__Respuesta_: Billetera No Determinista

A lo largo de esta sección, se analizo más de cerca las diferencias entre las billeteras deterministas y no deterministas. Luego, pasó por una variedad de escenarios para ayudar a determinar qué tipo de billetera es la más adecuada para una circunstancia determinada.

Es importante tener presenta que la billetera es el primer paso para describir una pieza increíblemente importante para la seguridad de su identidad de Blockchain: ¡las claves privadas!

## Llaves Privadas

Las carteras son geniales, pero es lo que hay dentro lo que cuenta. ¡Dentro de las carteras encontrarás llaves!

En esta sección, se repasará el concepto de llaves y se averiguará para qué sirven. Esto ayuda a comprender cómo contribuyen a la seguridad de su identidad de Blockchain. ¡A partir de ahí, la idea es que el lecto genere sus propias llaves privadas!

Las llaves privadas son un número aleatorio de 256 bits entre 1 y 2<sup>256</sup> y pueden representarse en diferentes formatos (e.g. Hex, WIF, WIF-Compressed). Esto puede resultar confuso, pero recuerde que independientemente del formato siempre se trata del mismo número. El formato más convencional es el hexadecimal; 256 bits en hexadecimal son 32 bytes y usan 64 carácteres desde el rango de 0 a 9 y de la A hasta la F. En este punto se puede cuestionar cómo es posible que una llave privada sea completamente única? No obstante el rango de 1 a 2<sup>256</sup> es enorme y nos brinda una icreíble cantidad de posibilidades. Para aterrizar estas posibilidades se comparte el siguiente comparativo. En la tierra hay alrededor de 2<sup>63</sup> granos de arena. Esto no esta ni cerca de las posibles 2<sup>256</sup> llaves privadas que se pueden generar. Ante este número tan amplio, solo se necesita escoger uno.

Las siguiente definiciones son relevantes:

-   **Llave privada:** Un número aleatorio de 256 bits entre 1 y 2<sup>256</sup>.
-   **Entropía:** Un número aleatorio de 256 bits entre 1 y 2<sup>256</sup>.

### Genere su Propia Llave Privada

Una vez entendido el concepto de llaves privadas se procede a generar una propia. Estos son los métodos para crear una clave privada:

-   El vieja escuela, que consiste en tomar papel, lapiz y un par de dados y seguir las siguientes [instrucciones](https://bitcointalk.org/index.php?topic=297077.msg3197393#msg3197393).
-   El generador de billeteras fuera de lína. Siga estas [instrucciones](https://github.com/bigmob/cryptosteel-tutorial/wiki/How-to-generate-private-key-offline-with-Bitaddress) parausar un generador de billeteras como BitAddress.
-   Usar un software de billeteras como [Electrum](https://electrum.org/#home).

En esta sección, se repasaron los conceptos básicos de las claves privadas, por qué son seguras y cómo crear una. Después se compartieron instrucciones para crear una propia. Próximamente, se usará el conocimiento de billeteras y claves para establecer identidades propias de cadena de bloques usando una billetera Bitcoin.

## Obtenga su Propia Billetera

Los tipos de billeteras y las claves privadas son interesantes, pero hasta ahora todo ha sido teoría. Es tiempo de conseguir una billetera propia para aterrizar esta teoría a la práctica.

Conseguir una billetera y trabajar con ella por cuenta propia ayuda a ver lo que realmente está pasando con estos nuevos conceptos. ¡También te da la oportunidad de crear tu propia identidad Blockchain!

Hay muchas billeteras para elegir, y cualquiera de ellas puede tener sentido para usted según sus necesidades. Siéntase libre de elegir cualquier billetera que desee, pero investigue un poco antes de instalar una para usted.

Una buena manera de comenzar es simplemente buscar en Google &ldquo;carteras de Bitcoin&rdquo; y comenzar a buscar.

La recomendación en esta publicación es la cartera de Electrum, citada previamente y las razones son la siguientes: Funciona en casi cualquier computadora. Es rápido y ligero. E incluye toda la funcionalidad que se necesita.

Para usar electrum, deberá instalarlo en su computadora. Analicemos los detalles de cómo hacerlo.

Para obtener más información sobre cómo comenzar con electrum, consulte el siguiente [enlace](https://www.youtube.com/watch?v=WdVlH9N2oKU). Esta es una excelente descripción general de las características básicas de esta billetera.

## Restaurando la Identidad en Blockchain

¡Toda persona olvida las contraseñas de vez en cuando, y nadie es inmune a las historias de terror de contraseñas!

Hay momentos en los que puede terminar necesitando restaurar la identidad de su billetera.

Esto podría suceder por algunas razones

-   Es posible que olvide su contraseña.
-   Podrías perder el dispositivo de verificación en dos pasos.
-   El servicio de billetera podría incluso dejar de estar disponible.
-   También es posible que pierdas tu computadora o te la roben.

Ojalá ninguna de estas cosas suceda, ¡pero es importante estar preparado por si acaso!

Para ayudarlo a prepararse para situaciones como esta, es útil saber cómo restaurar su billetera Bitcoin.


### Formas Para Recuperar Una Cartera

Si alguna vez necesita restaurar su billetera, hay 2 formas de hacerlo. Puede hacerlo con las palabras de la billetera que guardó o con las claves privadas de cuando creó su billetera por primera vez.


#### Usando Una Semilla

Una forma de restaurar una billetera es usando una semilla. La &rsquo;semilla&rsquo; son las 12 palabras que le dieron al crear su billetera. ¡Si puede recordar estas palabras, puede usarlas para restaurar su billetera!

El beneficio de restaurar su identidad usando la semilla es que puede ser mucho más simple que usar la clave privada. Es más fácil recordar una lista de palabras que una serie aleatoria de números y letras.

La parte más difícil de todo esto es almacenar o recordar esta información de manera segura para cuando la necesite. Cualquier otra persona que descubra la lista de palabras puede acceder a la billetera y a los fondos vinculados a ella.

¡Así que tenga mucho cuidado!

#### Usando Una Clave Privada

Otra forma de restaurar una billetera es con una clave privada.

Al restaurar una billetera usando una clave privada, hay 2 formas de hacerlo. Puede importar o barrer esta clave, y es útil comprender la diferencia.

Al importar una clave privada, tendrá una billetera de origen y una billetera de destino. Es probable que la billetera de destino ya esté llena con un grupo de claves privadas. Para importar la clave, mueva la clave privada de la billetera de origen a la billetera de destino.

Esto da como resultado que obtenga acceso tanto a la billetera de origen como a la billetera de destino.

La desventaja de importar es que la clave privada de la billetera de origen está esencialmente comprometida desde que se compartió. Si alguien obtiene acceso a la clave privada de la billetera de origen, puede acceder a esos Bitcoins.

Vea más sobre cómo importar una clave privada en [BitcoinElectrum.com](https://bitcoinelectrum.com/importing-your-private-keys-into-electrum/).

Cuando barre una clave privada, agrega una clave privada de una billetera de origen a la billetera de destino. Todos los Bitcoins que pertenecen a esa clave privada se transfieren de la billetera de origen a la billetera de destino.

Esto es un poco diferente a importar porque elimina completamente los fondos de la billetera original. Ahora solo usará esta nueva billetera para realizar transacciones futuras.

Vea más sobre cómo barrer una clave privada en [BitcoinElectrum.com](https://bitcoinelectrum.com/sweeping-your-private-keys-into-electrum/).

#### ¿Cuál Deberías Elegir?

¿Por qué importaría o barrería una clave privada? Barra una billetera si le preocupa la seguridad de la billetera. Esto podría suceder en el caso de que crea que alguien podría tener acceso a su clave privada. El barrido limpia completamente la billetera para que nadie tenga acceso a sus Bitcoins.

Si está seguro de que nadie ha tenido acceso a su clave privada, y de que nadie lo hará nunca, puede importar la clave en su lugar. Esto es útil en los casos en que necesita que los fondos estén disponibles en varias billeteras.

En caso de duda, quédese con el barrido. Es más seguro de esta manera y evita algunos problemas asociados con la importación de una billetera.

## Firma de una Transacción

Al tener una billetera, eventualmente comenzará a realizar algunas transacciones por sí mismo. En esta sección, se retrocedera un poco a la teoría para explorar algunas ideas más sobre lo que hace que esas transacciones sean seguras. El propósito es que esto le ayude a tener la confianza para comenzar a trabajar con estas herramientas por cuenta propia.

Después de crear una transacción, hay algunas cosas a considerar.

-   ¿Cómo se sabe que la transacción es válida?
-   ¿Quién es el dueño de la transacción?

Las transacciones se validan y la propiedad se asigna mediante lo que se conoce como *firma digital*. Estas firmas son una pieza importante subyacente a la seguridad de la cadena de bloques. Es tiempo de analizar cómo funciona la firma de una transacción.

Una firma establece una prueba de propiedad para cada transacción en la cadena de bloques. Las transacciones empiezan con transmiciones a la red cuando una billetera firma la transacción. Al igual que en papel y lapiz, Bitcoin require de una firma del emisor antes de que la transacción se envie a la red. Ahora bien, cómo esta prueba de propiedad ayuda a establecer seguridad en la cadena de bloques? Pues a diferencia de un sistema de correos electrónicos, en la cadena de bloques la firma es creada usando una dirección de cartera que en terminos familiares es equivalente a una cuenta bancaria. Esa dirección es única para su propietario, y esta enlazada a una llave privada. En la cadena de bloques es necesario usar la llave privada para ejecutar una transacción.

Una vez la transacción es firmada va desde el emisor hasta el receptor como una mensaje de transacción. En Bitcoin este mensaje es propagado a la cadena como una <span class="underline">Unspent Transaction Output</span> conocido como UTXO. Solo los UTXO pueden ser usados como entradas para aceptar una transacción. Cada uno de estos UTXO contienen condiciones sobre una prueba de propiedad basado en la llave privada para poder hacer efectiva la transacción de los fondos.

Como se comento previamente, las transacciones se realizan a partir de múltiples entradas y salidas. Al comprender las entradas, las salidas, las tarifas de los mineros y la cantidad enviada, podemos determinar el saldo de las billeteras de los usuarios antes y después de realizar las transacciones.

Para recapitular, esta es la secuencia de pasos para generar una transacción en una cadena de bloques:

1.  Demostrar la propiedad de la dirección firmando con clave privada.
2.  Ciclo de vida de la transacción del remitente al receptor.
3.  Salida de transacción no gastada UXTO.

Con los aspectos básicos de la firma de una transacción se abre la oportunidad de explorar cómo firmar y verificar un mensaje (el mismo concepto se aplica para firmar una transacción) usando las bibliotecas Bitcoin Javascript [bitcoinjs-lib](https://github.com/bitcoinjs/bitcoinjs-lib) y [bitcore-message](https://github.com/bitpay/bitcore-message).


<a id="org2c745ed"></a>

## Ciclo de Vida de una Transacción en Blockchain

Se ha cubierto una gran cantidad de información y es momento de sintetizar en un ejemplo el ciclo de vida de una transacción para poder llegar a la cadena de bloques. El ejemplo empiezan con dos usuario Bitcoin, Joe y Brandy. Joe queire enviar 1 BTC a brandy y a continuación se describe el paso a paso de como lograr esta solicitud:

1.  Joe obtiene la dirección de billetara de Brandy.
2.  Con esta información, Joe crea una nueva transacción de 1 BTC desde su billtera e incluye tarifa de transacción de0.003 BTC.
3.  Verifica su información y envía la transacción.
4.  La billetera de Joe inicia el algoritmo para firmar la transacción con su llave privada.
5.  La a transacción ahora se transmite al grupo de memoria dentro de la red
6.  La tranascción eventualmente es aceptada por los mineros.
7.  Los mineros agrupas la transacción en un bloque.
8.  Se ejecuta la prueba de trabajo.
9.  Se asigna un valor hash al bloque.
10. El bloque se coloca en la cadena de bloques.
11. Cuando el bloque obtiene las confirmaciones se acepta como una transacción válida en la red.
12. Una vez la transacción es aceptada, Brandy obtiene el 1 BTC.

Si se entiende todo este flujo, esta yendo por el gran camino para entender el Blockchain. En este ejemplo obviamente se excluyeron algunos detalles pero sirve como una historia rápida para tener un panorama general de como se comportan las transacciones en el Blockchain.

## Recapitulación

-   Tu identidad Blockchain es la forma en que te estableces en el mundo, en este caso en el mundo Blockchain.
-   La Dirección de Billetera es un identificador único para su billetera.
-   La Llave Privada es un número secreto que te permite gastar Bitcoins desde tu billetera.
-   La Llave Pública es una clave públicamente compartible que no se puede utilizar para gastar Bitcoins.
-   Analizamos diferentes tipos de billeteras, como billeteras no deterministas, billeteras deterministas e incluso distintos tipos de billeteras deterministas como las billeteras HD.
