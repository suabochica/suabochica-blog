+++
title = "Buscar y reemplazar en multiples archivos"
author = ["Sergio Benítez"]
description = "Instalar paquetes en melpa dentro de spacemacs"
date = 2020-12-30T00:00:00-05:00
lastmod = 2021-02-09T12:06:25-05:00
draft = false
tags = [
  "emacs",
  "spacemacs",
]
+++

Una de las funcionalidades más usadas en los diferentes IDEs o editores de texto
es la búsqueda y el remplazo de un texto específico en diferentes archivos.

En editores como Vim o Spacemacs, estás funcionalidades no son tan intuitivas y
en ocaciones para lograr este tipo de beneficios es necesario hacer una
configuración previa.

Para el caso de Spacemacs, este es el proceso para buscar y reemplazar textos en
multiples archivos:

-   Con `helm-ag` se puede buscar a través de todos los archivos en un proyecto
    `projectile` con la combinacion `SPC /` ó `SPC s p`.
-   Introduzca la consulta de búsqueda y una lista de archivos con dicha ocurrencia
    se mostrará.
-   Luego, golpee la combincación `C-c C-e` y Spacemacs creará un buffer
    `helm-ag-edit`.
-   Ahora se puede usar cualquier comando que funcione en un buffer. Esto quiere
    decir que una simple búsqueda y remplazo de Vim como `%s/alice/bob/g` puede ser
    ejecutada.
-   Por último, teclee `C-c C-c` para guardar los cambios.

¡Eso es todo!. Ahora disponemos de la funcionalidad de búsqueda y reemplazo en
varios archivos dentro de Spacemacs. Mi recomendación es que intenten usar
editores como Vim o Spacemacs, por razones que ire compartiendo en diferentes
publicaciones. Actualmente, Spacemacs es mi elección de editor diario, y espero
que los contenidos publicados sean de utilidad.


## Referencia {#referencia}

-   [Search and Replace in Multiple Files with Spacemacs](https://rameezkhan.me/search-and-replace-spacemacs/)
