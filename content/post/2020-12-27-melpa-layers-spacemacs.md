+++
title = "MELPA, paquetes en Spacemacs"
author = ["Sergio Benítez"]
description = "Instalar paquetes en melpa dentro de spacemacs"
date = 2020-12-27T00:00:00-05:00
lastmod = 2021-01-31T12:09:45-05:00
draft = false
tags = [
  "emacs",
  "spacemacs",
]
+++

## Layers y MELPA {#layers-y-melpa}

MELPA (Milkypostman’s Emacs Lisp Package Archive) es un repositorio de paquetes
que contiene una cantidad enorme de utilidades que complementan Emacs. Su
configuración es a través del archivo `packages.el` que funciona como un
administrador de paquetes. Pero en Spacemacs, esta configuración es diferente.
Más adelante se retomará el tema.

Los layers son un concepto de Spacemacs el cual se define como:

> Una unidad de configuración recopilada que se puede habilitar o deshabilitar en Spacemacs. Generalmente un layer reúne uno o más paquetes de MELPA sazonados con código de configuración para que funcionen bien entre sí, bajo el contexto de Spacemacs en general.

Los layers son probrablemente la funcionalidad más grande de Spacemacs sobre el
Emacs vanilla. El proyecto entero está organizado con una serie de layers, cada
uno para una funcionalidad en particular. Esta es la [lista completa](https://www.spacemacs.org/layers/LAYERS.html) de los
layers de Spacemacs.

No obstante, es posible un escenario en donde se requiere utilizar un
complemento puntual que no es ofrecido por los layers de Spacemacs y si por los
paquetes en MELPA.

Afortunadamente los creadores de Spacemacs anticiparon este caso y en el archivo
de configuración `.spacemacs` disponen de una función cuyos argumentos son los
nombres de los paquetes que no están empaquetdos en un layer.

A
continuación se muestra como se hace la instalación de los paquetes
`drag-stuff` y `atom-one-dark-theme`:

```elisp
;; List of additional packages that will be installed without being wrapped
;; in a layer (generally the packages are installed only and should still be
;; loaded using load/require/use-package in the user-config section below in
;; this file). If you need some configuration for these packages, then
;; consider creating a layer. You can also put the configuration in
;; `dotspacemacs/user-config'. To use a local version of a package, use the
;; `:location' property: '(your-package :location "~/path/to/your-package/")
;; Also include the dependencies as they will not be resolved automatically.
dotspacemacs-additional-packages '(drag-stuff
                                   atom-one-dark-theme)
```

Traduciendo las líneas comentadas en esta función, se dice que después de
agregar los paquetes, estos serán instalados y para cargarlos se requiere
especificar la acción en la sección `user-config` del archivo `.spacemacs`. Esta
sección también es la encargada de definir las configuraciones necesarias sobre
el paquete instalado. Como ejemplo, se va a utilizar una configuración sobre el
paquete `drag-stuff` para intercalar líneas de un archivo de texto con la
combinación de teclas `Ctrl + Shift + Up/Down arrow`:

```elisp
(defun dotspacemacs/user-config ()
  "Configuration for user code:
This function is called at the very end of Spacemacs startup, after layer
configuration.
Put your configuration code here, except for variables that should be set
before packages are loaded."
  ;; Drag setup to move selection up and down
  (drag-stuff-mode t)
  (global-set-key (kbd "<C-S-up>") 'drag-stuff-up)
  (global-set-key (kbd "<C-S-down>") 'drag-stuff-down)
)
```

De este modo se pueden usar paquetes en MELPA dentro de Spacemacs. Habrán
paquetes que requieran configuraciones más complejas, pero en términos generales
este proceso es la guía para lograr la convivencia entre paquetes MELPA y
Spacemacs.


## Referencia {#referencia}

-   [How to install a MELPA package](https://github.com/syl20bnr/spacemacs/issues/5968)
