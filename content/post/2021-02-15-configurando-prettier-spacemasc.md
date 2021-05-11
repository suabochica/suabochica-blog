+++
title = "Configurando Prettier en Spacemacs"
author = ["Sergio Benítez"]
description = "En esta publicación se comparte como configurar prettier para spacemacs y también como ejecutar un formato automatico mientras se salvan los archivos"
date = 2021-02-15T00:00:00-05:00
lastmod = 2021-05-10T21:31:58-05:00
draft = false
+++

## ¿Qué es Prettier? {#qué-es-prettier}

Prettier es un formateador de código obstinado que admite muchos leguajes de programación y se integra con la mayoría de los editores. Se puede instalar a través de `npm` o `yarn` y la recomendación es hacer una instalación global en el sistema para que su editor lo integre correctamente e incluso se pueda aplicar el formato automaticamente mientras guarda los archivos.


## ¿Cómo configurar Prettier? {#cómo-configurar-prettier}

Para empezar, asegúrese de que prettier está instalado globalmente en el sistema ejecutando el siguiente comando:

```nil
npm i -g prettier
```

Luego, verifique que está usando spacemacs dentro de la rama de `develop`. La configuración de spacemacs suele estar en el directorio `./emacs.d`.

```nil
cd ~/.emacs.d
git checkout develop
git pull origin develop
```

Agregué el layer `prettier` en la función `defun dotspacemacs/layers` bajo la sección `dotspacemacs-configuration-layers` en el archivo de configuración de spacemacs. Para acceder a este archivo teclee la combinación `SPC f e d`. A continuación se muestra el snippet que debé agregarse en la configuración:

```lisp
(defun dotspacemacs/layers ()
...
   dotspacemacs-configuration-layers
   '(
     prettier
     ...)
```

Actualice el layer de `javascript` estableciendo el valor `prettier` en la variable `javascript-fmt-tool`. Esta variable le suministra al lenguaje cual formateador usar para darle formato a los archivos `.js`.

```lisp
(defun dotspacemacs/layers ()
...
   dotspacemacs-configuration-layers
   '(
     prettier
     (javascript :variables
                 javascript-fmt-tool 'prettier)
     ...)
```

Con estas instrucciones los archivos `.js` pueden ser formateados con prettier dentro de spacemacs.

Para que spacemacs ejecute prettier sobre los archivos `.js` al momento de salvar los archivos es necesario crear un `js2-mode-hook`. Se usa `js2-mode-hook` como ejemplo ya que este es el modo principal en el que se abren los archivos javascript.

Para saber la lista de modos en los que esta trabajando spacemacs, puede usar el comando `describe-mode`. El comando se activa con la combinación `SPC h d m`.

Con ayuda del `before-save-hook` se establece el formateo automático del archivo \`.js\` antes de que se guarde un buffer en su archivo. El siguiente snippet debe ser agregado en la funcón `defun dotspacemacs/user-config` del archivo de configuración de spacemacs:

```lisp
(defun dotspacemacs/user-config ()
...
    (add-hook 'js2-mode-hook
          (lambda ()
            (add-hook 'before-save-hook 'prettier-js nil 'make-it-local)))
```
