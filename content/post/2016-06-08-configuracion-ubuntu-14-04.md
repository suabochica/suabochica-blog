+++
title = "Configuración Ubuntu 14.04 LTS"
date = 2016-06-08T02:13:50Z
author = "Sergio L. Benítez D."
description = "Recopilación de pasos para configurar Ubuntu"
+++

## Nota Previa

> Se señala que en el último mes la comunidad de Ubuntu liberó la versión 16.04 LTS Xenial Xerus, y en consecuencia el autor aclara que las configuraciones sugeridas en esta guía no han sido comprobadas sobre la versión en cuestión. Toda la información de esta guía es soportada por Ubuntu 14.04 LTS Trusty Tahr.

A través de este documento se va a compartir la configuración que el autor suele utilizar cuando trabaja en entornos de desarrollo con el sistema operativo Ubuntu. Además de tener un registro de acceso rápido sobre las parametrizaciones que muchas veces se realizan y con el tiempo se olvidan, se espera que esta configuración pueda ser utilizada y complementada por personas que trabajan con Ubuntu. Antes de describir los pasos de la configuración, se aclara que una propiedad de los sistemas operativos de distribución libre, es que son excesivamente personalizables. En consecuencia la configuración depende de las tareas específicas que ejecutará el usuario sobre el sistema operativo. El perfil del autor tiene un contexto en donde tareas de programación, edición de imágenes, reproducción multimedia y navegación de entornos de escritorio son frecuentes. Por tanto la parametrización de Ubuntu es influenciada por dichas condiciones.

## Las Siglas LTS

Como se observa en el título, el uso de la sigla LTS pretende especificar el contexto bajo el que se utiliza Ubuntu. LTS es la abreviación de Long Term Support (en español Soporte a Largo Plazo), y son versiones de Ubuntu que se liberan cada dos años con la característica de que dichas versiones recibirán soporte por 5 años. Las versiones LTS son liberadas considerando 5 criterios; Enfoque a empresas, compatibilidad con nuevo hardware, periodos de prueba más amplios, bases en propiedades funcionales existentes e innovación controlada. En últimas, los ciclos de las versiones LTS pretenden ofrecer a los usuarios de Ubuntu estabilidad, ya que las versiones estándar son liberadas cada seis meses y el frecuente movimiento de Ubuntu lo expone a escenarios de riesgo en términos funcionales y de compatibilidad. En tanto, esta guía se construye sobre una instalación limpia de Ubuntu 14.04 LTS Trusty Tahr.

* * *

## Configuraciones del Sistema

### 1. Cambiar las Fuentes de Software y Actualizar el Sistema

En las fuentes de software es recomendable agregar los __Canonical Partners__ para incrementar el número de aplicaciones y programas en el repositorio que se pueden instalar desde la línea de comandos o con ayuda de __Ubuntu Software Center__. Para ello, se abre la aplicación __Software and Updates__ y en la pestaña Other Software se habilitan los checkboxes correspondientes a los __Canonical Partners__. Una vez habilitados los __Canonical Partners__, se actualiza el sistema con el siguiente comando.

    sudo apt-get update && sudo apt-get upgrade

### 2. Instalar los Codecs Restringidos por Ubuntu

> Un codec, es un software que se utiliza para comprimir y descomprimir un archivo multimedia digital, tales como una canción o un video.

Por defecto, Ubuntu no tiene instalado varios codecs por restricciones legales en varios países, Al instalarlos de manera independiente, el uso no es responsabilidad de Ubuntu, sino del usuario que realiza la instalación. Con los codecs instalados, se pueden reproducir diferentes formatos multimedia tales como .mp3, .mp4 y .avi por nombrar algunos. La instalación se hace al ejecutar el siguiente comando:

    sudo apt-get install ubuntu-restricted-extras

### 3. Instalar Adobe Flash Player

Muchas de las páginas y aplicaciones web requiere del plugin Adobe Flash Player para reproducir los contenidos. YouTube es una de las aplicaciones que precisa de dicho plugin. Para instalar Flash Player se ejecuta el siguiente comando

    sudo apt-get install flashplugin-installer

### 4. Instalar Drivers adicionales

Ubuntu dispone de algunos controladores bajo propiedad de terceros para las funcionalidades de la tarjeta de red, tarjetas gráficas, entre otros. Es normal que se presenten inconvenientes con los controladores en Ubuntu, y una forma de lidiar el problema es instalar estos controladores adicionales. Para realizar dicha instalación se abre la aplicacións __Software and Updates__, y bajo la pestaña __Additional Drivers__, se podrán encontrar las recomendaciones que Ubuntu sugiere para solucionar el obstáculo de los controladores con los sistemas operativos de distribución libre .

### 5. Instalar Java

Java es un lenguaje de programación diseñado para producir programas que corran en cualquier sistema de computación. Al descargar un software en Java se obtiene un Java Runtime Environment (JRE). El JRE se compone de la Java Virtual Machine, las clases básicas y las librerías de apoyo de la plataforma Java. El JRE es la porción de ejecución del software Java necesaria para funcionar en un navegador web. Al igual que el plugin de Flash Player, algunas aplicaciones web requieren de la instalación de Java para un correcto funcionamiento. Existen muchas formas de instalar Java en Ubuntu, y aquí se utiliza la más simple, un comando:

    sudo apt-get install icedtea-7-plugin openjdk-7-jre

* * *

## Configuraciones del Entorno de Escritorio

Para desarrollar esta sección es adecuado clarificar el concepto de entorno de escritorio:

> En informática, un entorno de escritorio es una implementación de la metáfora del escritorio  hecha de un paquete de programas que se ejecutan en el sistema operativo de una computadora y que comparten una interfaz gráfica de usuario común.

La aclaración se hace, ya que como se mencionó anteriormente, Linux es tan personalizable que se puede configurar el entorno de escritorio del sistema operativo. Por defecto, Ubuntu 14.04 tiene como entorno de escritorio __Unity__. Sin embargo, al igual que varias personas de la comunidad de Linux, dicho entorno no convence en un 100% al autor, y por tanto, se prefiere utilizar el efectivo __Gnome__. Existen varios entornos de escritorio para Linux, pero por preferencia del autor en temas de usabilidad y configuración, en esta guía se explicará como instalar __Gnome__.

### 6. Instalar Gnome Classic Desktop

Para instalar Gnome Classic Desktop se abre la Terminal y se ejecuta el siguiente comando:

    sudo apt-get install gnome-session-fallback

Durante el proceso de la instalación, se pedirá una confirmación para poder utilizar el espacio en disco que será destinado para el programa. Se pulsa la tecla "y" y se presiona enter para terminar la instalación. Cuando esto suceda, se cierra la sesión de Unity y en el formulario de autenticación para entrar al escritorio saldrán las siguientes opciones: _Gnome Flashback (Compiz)_ y _Gnome Flashback (Metacity)_.

La diferencia entre estas nuevas opciones radica en el gestor de ventanas; __Compiz__ y __Metacity__:

> Un gestor de ventanas es una software que controla la ubicación, el comportamiento, las animaciones y la apariencia de las ventanas de la interfaz gráfica de usuario.

__Compiz__ trabaja con un compositor 3D para desplegar las ventanas mientras que __Metacity__ utiliza un compositor 2D. Ambos gestores ofrecen desde animaciones, esquinas de pantalla con funcionalidad y la parametrización de atajos de teclado (shortcuts) para personalizar nuestra navegación dentro de las ventanas del entorno de escritorio. Para computadores con tarjetas gráficas básicas lo recomendable es utilizar __Metacity__.

### 7. Instalar Mutate

Una de las funcionalidades que el autor aplaude del sistema operativo OS X, es el Spotlight. Spotlight es un buscador  de aplicaciones, archivos y documentos sobre el disco duro de nuestra computadora. Se habilita  a través de una combinación de teclas (Ctrl + Barra Espaciadora) y se digita el archivo a la aplicación que se desea consultar. La aplicación equivalente para Ubuntu se llama Mutate y su instalación se hace por medio de la ejecución de los siguientes comandos:

    sudo add-apt-repository ppa:mutate/ppa
    sudo apt-get update
    sudo apt-get install mutate

### 8. Instalar Gloobus Prevew

Otra funcionalidad que se considera acertada del sistema operativo OS X, es la previsualización de archivos. Para ello, simplemente el usuario se ubica en el archivo que desea consultar a través de explorador de archivos y se pulsa la barra espaciadora para poder previsualizar el archivo sin necesidad de abrirlo. Para homologar esta propiedad, se puede instalar la aplicación __Gloobus Preview__ con la ejecución de los siguientes comandos:

    sudo add-apt-repository ppa:nilarimogard/webupd8
    sudo apt-get update
    sudo apt-get install gloobus-preview gloobus-sushi unoconv gnumeric

* * *

## Algunos Programas Adicionales

### 9. Instalar Sublime Text

Uno de los editores de texto más utilizado por la comunidad de programadores es Sublime Text. Una de las ventajas de utilizar este editor es que está disponible para los diferentes sistemas operativos y gracias a su extensa comunidad, existe una gran variedad de paquetes que permiten personalizar el editor a nuestro antojo. Para instalar el editor en Ubuntu se ejecutan los siguientes comandos en la Terminal:

    sudo add-apt-repository ppa:webupd8team/sublime-text-3
    sudo apt-get update
    sudo apt-get install sublime-text-installer

### 10. Instalar GIMP

GIMP es un programa para la manipulación de imágenes. Ofrece una gran cantidad de herramientas para realizar trabajos asombrosos de diseño gráfico, ilustración y fotografía por nombrar algunas disciplinas. Se suele categorizar como el Photoshop de software libre y para instalar su última versión en Ubuntu, se ejecutan los siguientes comandos:

    sudo add-apt-repository ppa:otto-kesselgulasch/gimp
    sudo apt-get update && sudo apt-get install gimp

### 11. Instalar VLC Player

VLC es un reproductor multimedia libre y de código abierto multiplataforma y un «framework» que reproduce la mayoría de archivos multimedia, así como DVD, Audio CD, VCD y diversos protocolos de transmisión. Para realizar su instalación en Ubuntu se ejecutan los siguientes comandos:

    sudo add-apt-repository ppa:videolan/master-daily
    sudo apt-get update && sudo apt-get install vlc
