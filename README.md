Spacemacs Cookbook
=
<br>
[![Built with Spacemacs](https://cdn.rawgit.com/syl20bnr/spacemacs/442d025779da2f62fc86c2082703697714db6514/assets/spacemacs-badge.svg)](http://github.com/syl20bnr/spacemacs)

*Actualizado para Spacemacs 0.200*

<br>
<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Tabla de contenidos**

- [Introducción](#introducción)
- [Cookbook](#cookbook)
    - [Dotfiles](#dotfiles)
    - [Versionado de configuración](#versionado-de-configuración)
    - [Helm](#helm)
    - [Registros](#registros)
    - [Pegar desde el portapapeles](#pegar-desde-el-portapapeles)
    - [Pegar valor devuelto por comando](#pegar-valor-devuelto-por-comando)
    - [Cambiar fuente](#cambiar-fuente)
    - [Mostrar número de líneas](#mostrar-número-de-líneas)
    - [Manipulación de ventanas](#manipulación-de-ventanas)
    - [Manipulación de buffers](#manipulación-de-buffers)
    - [Reemplazar texto](#reemplazar-texto)
    - [Marks](#marks)
    - [Bookmarks](#bookmarks)
    - [Comandos de edición](#comandos-de-edición)
    - [Intercambiar cadenas](#intercambiar-cadenas)
    - [Reemplazo de ocurrencias](#reemplazo-de-ocurrencias)
    - [Gestión de proyectos](#gestión-de-proyectos)
    - [Buscar texto con ag](#buscar-texto-con-ag)
    - [Reemplazar texto en proyecto](#reemplazar-texto-en-proyecto)
    - [Snippets](#snippets)
    - [Buffer de búsqueda en archivo](#buffer-de-búsqueda-en-archivo)
    - [Layouts](#layouts)
    - [Corrector ortográfico](#corrector-ortográfico)
- [Dired](#dired)
- [Git](#git)
    - [version-control](#version-control)
    - [Magit](#magit)
    - [helm-gitignore](#helm-gitignore)
- [Markdown](#markdown)
    - [Vista previa](#vista-previa)
- [Org](#org)
    - [Cabeceras](#cabeceras)
    - [Texto y Enlaces](#texto-y-enlaces)
    - [Tareas](#tareas)
    - [Listas](#listas)
    - [Tablas](#tablas)
    - [Agenda](#agenda)
    - [org-capture y org-refile](#org-capture-y-org-refile)
    - [Links Utiles](#links-utiles)
- [Javascript](#javascript)
- [PHP](#php)
- [Licencia](#licencia)

<!-- markdown-toc end -->

<br>
Introducción
====

<br>
Esta guía presenta varios ejemplos prácticos para la configuración y uso de Spacemacs, un proyecto basado en Emacs. Este documento asume que el lector utiliza Spacemacs en `evil-mode` y que maneja los comandos básicos para modificar distintos aspectos de configuración. Antes de empezar a leer recomiendo leer la [guía introductoria](http://spacemacs.org/doc/QUICK_START.html) y realizar el `evil-tutor` (`SPC h T`).

<br>
Muchas de las técnicas mostradas aquí requieren activar ciertas `layers` de configuración. La cantidad de paquetes, opciones de configuración y comandos puede resultar abrumadora tanto para usuarios novatos como para expertos. Recordar siempre que el comando `SPC h SPC` permite abrir un modo de ayuda para buscar documentación referida a cierto aspecto de configuración. Esto es especialmente útil para saber detalles acerca de la implementación de ciertas `layers` de configuración y sus respectivos comandos. Siempre que haya una funcionalidad que requiera una layer en particular será debidamente advertido.

<br>
Si bien esta guía trata de ser lo más completa posible, definitivamente no es un reemplazo de la documentación provista por los desarrolladores. Podemos consultar la documentación completa [aquí](http://spacemacs.org/doc/DOCUMENTATION).

<br>
Podés encontrar mi configuración actual de Spacemacs en [este repositorio](https://github.com/emaphp/spacemacs.d).

<br>
Cookbook
====

<br>
Dotfiles
----

<br>
Por `dotfiles` queremos decir aquellos archivos destinados a almacenar nuestra configuración. Spacemacs, en caso de no encontrar ningún archivo de configuración personalizada, generará uno por nosotros con el nombre `~/.spacemacs`. Existen 2 comandos que debemos recordar ya que seguramente realizaremos varios cambios durante esta guía:

<br>
 * `SPC f e d`: Abre en un nuevo buffer el archivo de configuración actual.
 * `SPC f e R`: Realiza una recarga de las opciones de configuración.

<br>
Los cambios son luego almacenados haciendo `SPC f s`.

<br>
Versionado de configuración
----

Si bien utilizar el archivo `~/.spacemacs` parecerá útil al principio, Spacemacs nos ofrece otra posibilidad. En el caso de que el programa no encuentre este archivo, tratará de cargar la configuración desde `~/.spacemacs.d/init.el`. Esto nos ofrece la posibilidad de versionar nuestra configuración para poder descargarla (o compartirla) cuando queramos. Para lograr esto hacemos lo siguiente:

<br>
 * Crear la carpeta `~/.spacemacs.d`.
 * Inicializarla como repositorio Git.
 * Copiar nuestro archivo `.spacemacs` al nuevo directorio con el nombre `init.el`.
 * Hacer una copia de respaldo de `.spacemacs` con otro nombre.
 * Agregar `init.el` a stage, hacer commit y luego push.

<br>
Helm
----

<br>
Helm proporciona varias funcionalidades en torno al manejo de buffers, archivos, proyectos, etc. Varias acciones requerirán que manejemos la interfaz provista por Helm.

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Helm ~ Atajos genéricos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">C-n <em>o</em> C-j</td>
      <td>Mueve el cursor al siguiente elemento</td>
    </tr>
    <tr>
      <td>C-p <em>o</em> C-k</td>
      <td>Mueve el cursor al elemento anterior</td>
    </tr>
    <tr>
      <td>[Tab]</td>
      <td>Activa auto-completado</td>
    </tr>
    <tr>
      <td>[Enter] <em>o</em> C-m</td>
      <td>Abre el elemento seleccionado</td>
    </tr>
    <tr>
      <td>C-z</td>
      <td>Ejecuta helm-select-action, para ejecutar acciones generales</td>
    </tr>
    <tr>
      <td>C-t</td>
      <td>Modifica la disposición del buffer</td>
    </tr>
  </tbody>
</table>

<br>
Con `SPC b b` invocamos `helm-mini`, un selector de buffer con auto-completado.

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Helm Mini</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">[Tab]</td>
      <td>Visualiza el buffer seleccionado</td>
    </tr>
    <tr>
      <td>C-c o</td>
      <td>Abre el buffer seleccionado en otra ventana</td>
    </tr>
    <tr>
      <td>C-x C-s</td>
      <td>Guarda el buffer seleccionado</td>
    </tr>
    <tr>
      <td>C-c d</td>
      <td>Elimina el buffer seleccionado</td>
    </tr>
    <tr>
      <td>C-SPC</td>
      <td>Marca el buffer seleccionado</td>
    </tr>
    <tr>
      <td>M-a</td>
      <td>Marca todos los buffers</td>
    </tr>
    <tr>
      <td>M-m</td>
      <td>Marca/Desmarca buffers (toggle)</td>
    </tr>
    <tr>
      <td>M-C</td>
      <td>Permite copiar los elementos marcados</td>
    </tr>
    <tr>
      <td>M-R</td>
      <td>Permite renombrar los elementos marcados</td>
    </tr>
    <tr>
      <td>M-D</td>
      <td>Elimina los buffers marcados</td>
    </tr>
    <tr>
      <td>C-s</td>
      <td>Permite correr un Multi-Occur en el buffer seleccionado</td>
    </tr>
    <tr>
      <td>C-x C-d</td>
      <td>Muestra un listado de otros buffers contenidos en el mismo directorio (útil para proyectos)</td>
    </tr>
  </tbody>
</table>

<br>
Con `SPC f f` ejecutamos `helm-find-files`, un buscador de archivos con auto-completado. Recomiendo usar esta utilidad para crear nuevos archivos por sobre el tradicional `find-files`. Los atajos para marcar/copiar/renombrar/eliminar son los mismos que en `helm-mini` por lo que se omiten.

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">helm-find-files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">C-l</td>
      <td>Ingresa al directorio seleccionado</td>
    </tr>
    <tr>
      <td>C-h o S-[Tab]</td>
      <td>Sube un nivel de directorio</td>
    </tr>
    <tr>
      <td>[Tab]</td>
      <td>Entra al directorio o visualiza el archivo seleccionado</td>
    </tr>
  </tbody>
</table>

<br>
Haciendo `SPC f r` invocamos a `helm-recentf`, una lista de archivos abiertos recientemente. Los atajos son similares a los vistos en `helm-find-files`.

<br>
Registros
----

<br>
A medida que vayamos trabajando sobre un documento se presentarán varias complicaciones al copiar/pegar texto sucesivamente. Los registros son una alternativa elegante que presenta varios beneficios:

 * Dada la naturaleza del kill-ring, los valores se irán apilando a medida que dure la sesión. Esto se volverá engorroso con el tiempo.
 * Utilizar registros provee de muchas ventajas a la hora de combinarlas con macros.

<br>
Spacemacs presenta una solución parcial al primer problema: los valores que insertamos con `p` o `P` pueden intercambiarse con `C-p` y `C-n`. En `insert-mode` esta funcionalidad se realiza con `M-y`.

<br>
Para realizar una operación con un registro ingresamos `"`, el nombre del registro y el tipo de operación. Por ejemplo, suponiendo que queramos utilizar el registro `a`, podemos realizar estas operaciones:

<br>
 * `"ayy`   : Copiar la línea actual al registro `a`.
 * `"ay`    : Copiar texto seleccionado al registro `a` (view-mode).
 * `"ap`    : Pega el valor del registro `a`. Usar `"aP` para pegar antes del cursor.

<br>
Pegar desde el portapapeles
----

<br>
Al principio, los atajos de teclado parecerán poco intuitivos a nuevos usuarios. Esto, combinado con el hecho de que los atajos para copiar, pegar y cortar dependen del modo en el que nos encontremos puede resultar frustrante. El simple hecho de copiar un texto del portapapeles para reemplazar otro en otro buffer puede también resultar complicado. He aquí un procedimiento de ejemplo:

<br>
 * Ingresar en `view-mode` (`v`, `V` o `C-v`).
 * Seleccionar la región que queremos reemplazar.
 * Copiar el texto que queramos ingresar.
 * Hacer `"*p`.

<br>
Este último paso pega el contenido de un registro especial. El registro `*` representa el portapapeles en sistemas GNU/Linux.

<br>
Pegar valor devuelto por comando
----

<br>
Supongamos que deseamos insertar en el documento actual la versión de `Emacs` que estamos utilizando. Haremos:

<br>
 * `SPC :`, para ingresar un comando por nombre.
 * Ingresamos `version` (también funciona con `emacs-version`).
 * Hacemos `C-u` y luego `[Enter]`.

<br>
Recordar que, fuera de este uso, `C-u` invoca `evil-scroll-up`.

<br>
Cambiar fuente
----

<br>
La fuente tipográfica se configura en nuestro archivo `.spacemacs` (o `.spacemacs.d/init.el` según el caso). En el hook `spacemacs/init` se define el valor por defecto de la siguiente manera:

<br>
```emacs-lisp
dotspacemacs-default-font '("Source Code Pro"
                             :size 13
                             :weight normal
                             :width normal
                             :powerline-scale 1.1)
```
<br>
Aquí podemos tanto modificar el valor directamente o agregar la misma linea modificada en el hook `user-init`.

<br>
Mostrar número de líneas
----

<br>
Spacemacs tiene un valor de configuración específico para esto:

<br>
```emacs-lisp
(setq-default dotspacemacs-line-numbers t)
```

<br>
Alternativamente podemos activar `linum-mode` globalmente:

<br>
```emacs-lisp
(global-linum-mode)
```

<br>
Manipulación de ventanas
----

<br>
Llamamos ventana al área visual donde se muestra el contenido de un buffer. Cada ventana es Spacemacs recibe un número que puede verse en su esquina inferior izquierda. Esta numeración tiene por objetivo moverse entre ventanas de manera sencilla. Por ejemplo, para moverse a la ventana 2 haremos `SPC 2`. Por defecto, empezamos con una sola ventana pero podemos generar más con los siguientes atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Ventanas</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">SPC w /</td>
      <td>Divide una ventana en 2 ventanas orientadas verticalmente</td>
    </tr>
    <tr>
      <td>SPC w -</td>
      <td>Divide una ventana en 2 ventanas orientadas horizontalmente</td>
    </tr>
    <tr>
      <td>SPC w d</td>
      <td>Cierra la ventana actual</td>
    </tr>
    <tr>
      <td>SPC w D</td>
      <td>Cierra otra ventana. La ventana a cerrar corresponde a la letra que aparece en rojo en la esquina superior izquierda</td>
    </tr>
    <tr>
      <td>SPC w L</td>
      <td>Mueve la ventana a la derecha</td>
    </tr>
    <tr>
      <td>SPC w H</td>
      <td>Mueve la ventana a la izquierda</td>
    </tr>
    <tr>
      <td>SPC w J</td>
      <td>Mueve la ventana hacia abajo</td>
    </tr>
    <tr>
      <td>SPC w K</td>
      <td>Mueve la ventana hacia arriba</td>
    </tr>
    <tr>
      <td>SPC w S</td>
      <td>Divide una ventana en 2 ventanas orientadas horizontalmente y pone el cursor en la nueva</td>
    </tr>
    <tr>
      <td>SPC w V</td>
      <td>Divide una ventana en 2 ventanas orientadas verticalmente y pone el cursor en la nueva</td>
    </tr>
    <tr>
      <td>SPC w w</td>
      <td>Mueve el foco entre ventanas</td>
    </tr>
    <tr>
      <td>SPC w R</td>
      <td>Rota la disposición de las ventanas</td>
    </tr>
  </tbody>
</table>

<br>
Manipulación de buffers
----

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Buffers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">SPC b b</td>
      <td>Invoca a helm-mini, mostrando el listado de buffers abiertos</td>
    </tr>
    <tr>
      <td>SPC b d</td>
      <td>Elimina el buffer actual</td>
    </tr>
    <tr>
      <td>SPC b D</td>
      <td>Elimina todos los buffers excepto el actual</td>
    </tr>
    <tr>
      <td>SPC [Tab]</td>
      <td>Intercambia el buffer actual con el anterior</td>
    </tr>
    <tr>
      <td>SPC b e</td>
      <td>Elimina el contenido de un buffer</td>
    </tr>
    <tr>
      <td>SPC b w</td>
      <td>Permite alternar entre modo "solo lectura" y modo normal</td>
    </tr>
    <tr>
      <td>SPC b K</td>
      <td>Elimina todos los buffers excepto el actual</td>
    </tr>
    <tr>
      <td>SPC b R</td>
      <td>Revierte el contenido del buffer (cargar desde archivo)</td>
    </tr>
    <tr>
      <td>SPC b s</td>
      <td>Abre el buffer scratch</td>
    </tr>
    <tr>
      <td>SPC b Y</td>
      <td>Copia el contenido del buffer al portapapeles</td>
    </tr>
    <tr>
      <td>SPC b P</td>
      <td>Reemplaza el contenido del buffer con el del portapapeles</td>
    </tr>
  </tbody>
</table>

<br>
Para eliminar un buffer junto con la ventana donde se visualiza ingresar la combinación `SPC u SPC b d`. Haciendo `SPC b .` entramos al buffer transient-state. En este estado tenemos los siguientes atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Buffer Transtient State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">n</td>
      <td>Ir al siguiente buffer</td>
    </tr>
    <tr>
      <td>N</td>
      <td>Ir al buffer anterior</td>
    </tr>
    <tr>
      <td>K</td>
      <td>Cierra el buffer actual</td>
    </tr>
  </tbody>
</table>

<br>
Presionando cualquier otra tecla saldremos del transient-state.

<br>
Reemplazar texto
----

<br>
Parte de la efectividad de `evil-mode` es el soporte para comandos de reemplazo de texto. Haremos una rápida recorrida por las esenciales.

<br>
 * `:%s/foo/bar`         : Reemplaza ocurrencias de `foo` con `bar` en todo el documento.
 * `:%s/foo/bar/g`       : Lo mismo que lo anterior pero también reemplazando múltiples ocurrencias de `foo` en la misma línea.
 * `:%s/foo/bar/gc`      : Lo mismo que lo anterior pero cada ocurrencia requerirá una confirmación.
 * `:%s/foo/bar/gci`     : Lo mismo que lo anterior pero ignorando entre minúsculas y mayúsculas.
 * `:%s/\<foo\>/bar/gci` : Lo mismo que lo anterior, pero haciendo la búsqueda solo por palabra completa.

<br>
Los siguientes comandos reemplazan teniendo en cuenta la línea y la posición del cursor.

<br>
 * `:25,50s/foo/bar`   : Reemplaza ocurrencias de `foo` con `bar` entre las lineas 25 y 50.
 * `:.,$s/foo/bar`     : Lo mismo que lo anterior pero desde la posición del cursor hasta el fin del documento.
 * `:^,.+4/foo/bar`    : Lo mismo que lo anterior pero desde el comienzo del documento hasta 4 lineas después de la ocupada por el cursor.

<br>
Las cadenas de búsqueda y reemplazo también soportan expresiones regulares. Aquí algunos ejemplos:

<br>
 * `:%s/.an\>/vim/g`            : Reemplaza expresiones terminadas en *an* (*pan*, *fan*, *can*) con *vim*. La expresión reemplazará solo las últimas 3 letras.
 * `:%s/drive_\w/vim/g`         : Reemplaza expresiones de tipo *drive_a*, *drive_b* por *vim*. La expresión `\w` corresponde a caracteres entre a y z.
 * `:%s/file[0-9]+/vim/g`       : Reemplaza expresiones como *file1* y *file542* por *vim*. El dígito al final de la expresión debe contener 1 o más caracteres.
 * `:%s/\(\w+\)\.htm/\1.html/g` : Reemplaza expresiones como `index.htm` y `test.htm` por `index.html` y `test.html` respectivamente. La cadena entre \( y \) se captura y es luego utilizada como reemplazo. La expresión \1 es llamada `backreference`.

<br>
Para conocer más acerca de expresiones regulares compatibles con Vim podemos visitar [vimregex](http://vimregex.com/).

<br>
Marks
----

<br>
Los marks permiten mantener un listado de posiciones relevantes en un documento. Al igual que los registros, estos también se reconocen a través de letras. Suponiendo que tengamos el cursor en el encabezado de una sección:

<br>
 * Para guardar la posición actual en el marcador `h` haremos `mh`.
 * Luego, para volver a esta posición haremos `'h`.

<br>
Podemos usar los marcadores para establecer el área de texto que usamos con una expresión de reemplazo. Para modificar el área entre el marcador y la posición actual hacemos `:'h,.s/foo/bar/g`.

<br>
Para obtener el listado de marks creados en un documento correr el comando `:marks`.

<br>
Bookmarks
----

<br>
Los bookmarks son similares a los marks con la diferencia de que estos persisten una vez que cerramos un documento. Haciendo `SPC f b` activamos `helm-bookmarks`. Este buffer nos permite crear nuevos bookmarks o navegar a otros ya existentes. La interfaz provee estos atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Helm Filtered Bookmarks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">C-d</td>
      <td>Elimina un bookmark. Podemos borrar varios seleccionándolos con C-SPC</td>
    </tr>
    <tr>
      <td>M-e</td>
      <td>Edita el bookmark seleccionado</td>
    </tr>
  </tbody>
</table>

<br>
Comandos de edición
----------------

<br>
Este es un listado de atajos de edición que puede resultar útil conocer:

<br>
 * **Números**: `evil-numbers` permite modificar valores numéricos con `SPC n +` y `SPC n -`. Estos atajos entran en un mini estado que permite seguir modificando el valor actual con `+` y `-`.
 * **Comentarios**: Para modificar el texto dentro de comentarios multilínea (`/* ... */`) hacer `ci8`. Para borrar hacer `di8`.
 * **Argumentos**: Dentro del listado de argumentos de una función, hacer `cia` para modificar y `daa` para eliminar.
 * **Buffer**: La combinación `cig` permite modificar el contenido entero de un buffer. Para borrar todo hacer `dig`.
 * **Comillas**: Para meter un texto entre comillas primero lo seleccionamos  y luego ingresamos `s"`. Para pasar un texto entre comillas dobles a comillas simples hacer `cs"'`. Para eliminar las comillas del todo hacer `ds"`. 
 * **Espacios sobrantes**: Spacemacs permite eliminar líneas de espacios sobrantes haciendo `SPC x d w`.
 * **Un solo espacio**: Podemos reducir el número de espacios entre palabras a uno posicionando el cursor sobre cualquiera de ellos e invocando a `just-one-space`.
 * **Mayúsculas y minúsculas**: `SPC x u` pasa a minúsculas el texto seleccionado. Utilizar `SPC x U` para pasar a mayúsculas.
 * **Expandir selección**: Haciendo `SPC v` entramos en el modo `expand-region`. En este modo podemos expandir y contraer una selección con `v` y `V`, `r` para volver a la original y `[Esc]` para salir.
 * **Editar líneas**: Ingresando `J` concatenamos la línea actual con la siguiente. `SPC j n` divide la línea justo en la posición del cursor. `SPC j k` mueve el cursor a la siguiente línea y la auto indenta.
 * **Comentarios**: Podemos comentar una selección haciendo `M-;`. Haciendo `SPC c l` comentamos la línea actual. `SPC c p` comenta un parágrafo completo.

<br>
Deshacer
----

Para deshacer un cambio (*undo*) ingresar `u`. Para repetir un cambio (*redo*) hacer `C-?`. Podemos ver el historial de cambios del documento haciendo `SPC a u`. Este comando invoca `undo-tree-visualize`. Para salir ingresamos `q`.

<br>
Intercambiar cadenas
--------------------

<br>
`evil-exchange` es un plugin integrado a Spacemacs que permite intercambiar 2 valores seleccionados de la siguiente manera:

<br>
 * Entramos a `visual-mode` (`v`).
 * Seleccionamos el valor a intercambiar.
 * Ingresamos `gx`.
 * Volvemos a entrar en `visual-mode`.
 * Seleccionamos el valor de intercambio.
 * Ingresamos `gx` nuevamente.

<br>
Para cancelar la operación puede ingresarse `gX`.

<br>
Reemplazo de ocurrencias
------------------------

<br>
`iedit` permite modificar múltiples ocurrencias de un símbolo, muy útil para tareas de refactorizado. Para entrar en este modo desde `normal-state`, ubicar el cursor en el símbolo a modificar y hacer `SPC s e`. `iedit` utiliza un **cursor rojo** para notificar que se encuentra activo. En este modo poseemos los siguientes atajos de edición:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">iedit ~ Edición</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="25%">S</td>
      <td>Permite reemplazar el símbolo por el valor ingresado</a>
    </tr>
    <tr>
      <td>A</td>
      <td>Permite concatenar al final de la ocurrencia</td>
    </tr>
    <tr>
      <td>I</td>
      <td>Mueve concatenar al comienzo de la ocurrencia</td>
    </tr>
    <tr>
      <td>D</td>
      <td>Elimina todas las ocurrencias</td>
    </tr>
    <tr>
      <td>p</td>
      <td>Reemplaza las ocurrencias por un valor copiado previamente</td>
    </tr>
    <tr>
      <td>U</td>
      <td>Pasa las coincidencias a mayúsculas</td>
    </tr>
    <tr>
      <td>C-U</td>
      <td>Pasa las coincidencias a minúsculas</td>
    </tr>
    <tr>
      <td>#</td>
      <td>Permite encabezar las ocurrencias con una cifra numérica. Anteponer <em>SPC u</em> para modificar cifra y formato</td>
    </tr>
  </tbody>
</table>

<br>
Luego de realizar la modificación ingresamos `[Esc]` para salir de este modo. También podemos navegar entre las ocurrencias con los siguientes atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">iedit ~ Navegación</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="20%">0</td>
      <td>Mueve el cursor al principio de la ocurrencia actual</td>
    </tr>
    <tr>
      <td>$</td>
      <td>Mueve el cursor al final de la ocurrencia actual</td>
    </tr>
    <tr>
      <td>gg</td>
      <td>Mueve el cursor a la primera ocurrencia</td>
    </tr>
    <tr>
      <td>G</td>
      <td>Mueve el cursor a la última ocurrencia</td>
    </tr>
    <tr>
      <td>n</td>
      <td>Mueve el cursor a la próxima ocurrencia</td>
    </tr>
    <tr>
      <td>N</td>
      <td>Mueve el cursor a la ocurrencia anterior</td>
    </tr>
    <tr>
      <td>[Tab]</td>
      <td>Permite ignorar una ocurrencia seleccionada</td>
    </td>
  </tbody>
</table>

<br>
En caso de querer limitar el alcance de la modificación se puede utilizar los siguientes atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">iedit ~ Scope</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="25%">L</td>
      <td>Restringe el alcance a la línea actual</td>
    </tr>
    <tr>
      <td>K</td>
      <td>Incrementa el alcance superior por una línea</td>
    </tr>
    <tr>
      <td>J</td>
      <td>Incrementa el alcance inferior por una línea</td>
    </tr>
    <tr>
      <td>F</td>
      <td>Restringe el alcance a la función actual</td>
    </tr>
  </tbody>
</table>

<br>
Podemos también pasar a este modo desde `expand-region` ingresando simplemente `e` despues de realizar la selección.

<br>
Gestión de proyectos
----

<br>
[Projectile](https://github.com/bbatsov/projectile) es una extensión que facilita el acceso a archivos dentro de un proyecto, lo que resulta útil para organizar grandes repositorios de código. Cada vez que accedamos a un directorio conteniendo un repositorio Git, Projectile se encargará de agregarlo a un listado interno de proyectos. Haciendo `SPC p p` invocamos a `helm-projectile`, un selector que lista los proyectos abiertos anteriormente. Si abrimos un archivo y luego `SPC p h` podremos acceder a otros archivos dentro del mismo proyecto. Para cerrar todos los buffers abiertos dentro de un proyecto hacemos `SPC p k`.

<br>
Si hacemos `SPC p t` se abrirá una ventana ejecutando NeoTree, una extensión para navegación de directorios. Para navegar entre los directorios utilizamos `h`, `j`, `k` y `l`. Con `[Enter]` abrimos un archivo y con `R` fijamos el directorio seleccionado como el raíz. NeoTree siempre se ejecutará en la ventana `0`, por lo que podemos navegar de vuelta haciendo `SPC 0`. Para actualizar el arbol de directorios hacer `gr`.

<br>
Por último, si deseamos realizar una búsqueda dentro de un proyecto hacemos `SPC s g p`. Este atajo permitirá realizar una búsqueda usando `grep` dentro de la carpeta conteniendo el proyecto. Spacemacs también soporta hacer búsquedas utilizando otros comandos como `ack` y `ag`.

<br>
Buscar texto con ag
-------------------

<br>
Es posible realizar búsquedas utilizando `ag` en lugar de `grep`. Para esto primero debemos instalar esta herramienta en el sistema. Por ejemplo, en sistemas Debian haremos:

<br>
``` shell
sudo apt-get install silversearcher-ag
```

<br>
Ahora, para realizar una búsqueda en un proyecto utilizando `ag` haremos `SPC s a p`.

<br>
Como alternativa podemos instalar [ag.el](https://github.com/Wilfred/ag.el), una herramienta para búsqueda avanzada que utiliza `ag`. Para esto necesitamos agregar `ag` al listado de paquetes extra en `dotspacemacs-additional-packages`.

``` emacs-lisp
   dotspacemacs-additional-packages '(ag)
```

<br>
Luego, agregamos un nuevo atajo invocando a `ag-project`. Este comando realizará una búsqueda sobre un proyecto utilizando `ag`.

<br>
``` emacs-lisp
  (spacemacs/set-leader-keys "op" 'ag-project)
```

<br>
Los resultados de la búsqueda se listarán en una ventana aparte. Para abrir un resultado ingresamos `[Enter]`. En caso de querer visualizarlo sin cambiar de ventana hacemos `C-o`.

<br>
Para cerrar los buffers con resultados de búsqueda hacemos `SPC :` y luego `ag-kill-buffers`.

<br>
Reemplazar texto en proyecto
----------------------------

<br>
Combinando `iedit` y `ag` podemos reemplazar texto en varios archivos dentro de un proyecto de la siguiente manera:

<br>
 * Abrimos el proyecto: `SPC p p`.
 * Abrimos un buffer de búsqueda con `SPC /`.
 * Ingresamos el texto a reemplazar y hacemos `C-c C-e`.
 * Ingresamos a modo `iedit` ya sea haciendo `SPC s e` o `SPC v e`.
 * Realizamos la modificación y hacemos `C-c C-c`.

<br>
Snippets
----

<br>
Activando el layer `auto-completion`,  se incluirán los modos `yasnippet` y `auto-yasnippet`. Por defecto, Spacemacs definirá un listado de directorios en los cuales tratará de cargar snippets. Este valor es configurable desde el archivo `.spacemacs` (o `.spacemacs.d/init.el`):

<br>
```emacs-lisp
(auto-completion :variables
                 auto-completion-private-snippets-directory "~/.spacemacs.d/snippets")
```

<br>
Para crear un nuevo snippet hacer `SPC :` e ingresar `yas-new-snippet`. Se abrirá un nuevo buffer en modo `Snippet`. En la parte superior definimos un nombre y una `key`, que será el id del nuevo snippet:

<br>
```
# -*- mode: snippet -*-
# name: lorem
# key: lorem
# --
Fusce sagittis, libero non molestie mollis, magna orci ultrices dolor, at vulputate neque nulla lacinia eros.
```

<br>
Para guardar el snippet hacemos `C-c C-c`. Por defecto, los snippets se almacenan en una carpeta homónima al modo corriendo en el buffer desde donde invocamos a `yas-new-snippet`. Para utilizar un snippet ingresamos su `key` y luego `M-/`. Podemos ver un listado de snippets disponibles en el modo actual haciendo `SPC i s`.

<br>

> Tip: Spacemacs permite insertar "lorem ipsum" en varios formatos. Buscar comandos bajo el prefijo `SPC i l`.

<br>
Una alternativa a este procedimiento es abrir el buffer `* scratch *` y activar `snippet-mode`. Luego, crear un snippet y hacer `SPC :` invocando a `yas-load-snippet-buffer`. Esta función cargará el snippet del buffer actual en la tabla del modo que definamos. *Este método no guarda el contenido del snippet*.

<br>
Los snippets también soportan argumentos. Supongamos que estamos en `web-mode` y queremos un snippet para insertar un `jumbotron` de [Bootstrap](http://getbootstrap.com):

<br>
```
# -*- mode: snippet -*-
# name: bsjumbo
# key: bsjumbo
# --
<div class="jumbotron">
  <h1>$1</h1>
  <p>$2</p>
</div>
```

<br>
Ahora, al insertar `bsjumbo` y hacer `M-/`, el cursor se posicionará automaticamente en el primer argumento (`$1`). Ingresando un valor y luego `[Tab]` el cursor se moverá al siguiente argumento. Para probar que los argumentos funcionan correctamente podemos hacer `SPC :` y luego invocar `yas-tryout-snippet`, lo que abrirá un buffer de prueba donde podemos probar el snippet.

<br>
`auto-yasnippet` permite crear snippets de sesión de manera sencilla. Supongamos que deseamos introducir el siguiente código:

``` javascript
var nombre = obj.nombre;
var apellido = obj.apellido;
var edad = obj.edad;
```
<br>
Para crear un snippet ingresamos la primera línea así:

<br>

``` javascript
var ~nombre = obj.~nombre;
```

<br>
El símbolo `~` indica que el valor corresponde a un argumento. Para crear un snippet a partír de esta línea hacemos: `SPC i S c`. Este atajo invoca a `aya-create`, lo cual reemplaza los argumentos por el valor por defecto ingresado y crea el snippet. Ahora nos ubicamos abajo y hacemos `SPC i S e`. Este comando (`auto-yasnippet-expand`) expande el snippet anterior. En nuestro caso, solo resta ingresar a `edit-mode` para ingresar el nombre de la propiedad. `auto-yasnippet` reemplaza el argumento tantas veces como se haya especificado en la expresión. En caso de haber definido más argumentos podemos saltar a la posición correspondiente con `[Tab]`.

<br>
Buffer de búsqueda en archivo
-----------------------------

<br>
`helm-swoop` es una gran herramienta de búsqueda que combina la funcionalidad de `occur` con la conveniencia de Helm. En cualquier parte de un documento hacemos `SPC s s` para invocar un buffer Helm de búsqueda. Este buffer irá mostrando las coincidencias encontradas en una lista navegable a medida que vayamos insertando texto. Para realizar la búsqueda por el valor bajo el cursor hacer `SPC s S`. La combinación `SPC s C-s` permite realizar la búsqueda sobre todos los buffers abiertos.

<br>
Layouts
-------

<br>
Si alguna vez utilizaste Eclipse seguramente el concepto de *perspectivas* te resulte familiar. Los *layouts* representan configuraciones aplicadas a un conjunto de ventanas. Estas configuraciones almacenan la disposición de cada ventana y el conjunto de buffers visualizados. Haciendo `SPC l` entramos al *layouts transient state*. En este modo contamos con los siguientes atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Layouts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">?</td>
      <td>Muestra la ayuda para el uso de layouts</td>
    </tr>
    <tr>
      <td>l</td>
      <td>Permite acceder a un layout por nombre. Introduciendo un nuevo nombre almacena la disposición actual en un nuevo layout</td>
    </tr>
    <tr>
      <td>[Tab]</td>
      <td>Permite navegar entre layouts</td>
    </tr>
    <tr>
      <td>1..9</td>
      <td>Permite acceder a un layout por identificador</td>
    </tr>
    <tr>
      <td>b</td>
      <td>Permite acceder a un buffer en el layout actual</td>
    </tr>
    <tr>
      <td>a</td>
      <td>Permite agregar buffers al layout actual</td>
    </tr>
    <tr>
      <td>r</td>
      <td>Remueve un buffer del layout actual</td>
    </tr>
    <tr>
      <td>s</td>
      <td>Guarda el layout a un archivo</td>
    </tr>
    <tr>
      <td>L</td>
      <td>Carga un layout desde un archivo</td>
    </tr>
    <tr>
      <td>n</td>
      <td>Navega al siguiente layout</td>
    </tr>
    <tr>
      <td>p</td>
      <td>Navega al layout anterior</td>
    </tr>
    <tr>
      <td>R</td>
      <td>Renombra el layout actual</td>
    </tr>
    <tr>
      <td>c</td>
      <td>Cierra el layout sin cerrar los buffers</td>
    </tr>
    <tr>
      <td>x</td>
      <td>Cierra el layout y los buffers</td>
    </tr>
  </tbody>
</table>

<br>
Haciendo `SPC p l` podemos crear layouts a partír de proyectos. De esta manera podemos navegar entre proyectos sin perder la disposición de los buffers y ventanas. El nombre del proyecto se muestra a un lado del número de ventana, al igual que con los demás layouts.

<br>
Por defecto, Spacemacs define 2 layouts customizadas. Las mismas se acceden haciendo `SPC l o` y reciben los nombres *org* y *spacemacs*. La primera está orientada a trabajar con archivos de `org-agenda`, la segunda para definir aspectos de configuración.

<br>
Esta layer posee más aspectos de configuración que podemos ver en la documentación referida a `spacemacs-layouts`.

<br>
Corrector ortográfico
----

<br>
Antes de realizar una corrección ortográfica sobre un documento asegurarse de que se cuenta con el listado de palabras correspondiente al lenguaje en uso. En sistemas *Debian* puede hacerse a través de la instalación de un paquete. Por ejemplo, para descargar el diccionario para el lenguaje español podemos hacer:

<br>
```shell
 sudo apt-get install aspell-es
```

<br>
Para entrar en modo de corrección ortográfica hacer `SPC :` e ingresar `ispell`.

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">ispell</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">0-9</td>
      <td>Reemplaza una palabra por una de las opciones listadas arriba</td>
    </tr>
    <tr>
      <td>r</td>
      <td>Permite reemplazar la palabra por otra</td>
    </tr>
    <tr>
      <td>i</td>
      <td>Agrega la palabra al diccionario</td>
    </tr>
    <tr>
      <td>a</td>
      <td>Ignorar palabra por esta sesión</td>
    </tr>
    <tr>
      <td>?</td>
      <td>Muestra las opciones de ayuda</td>
    </tr>
    <tr>
      <td>x</td>
      <td>Suspende la ejecución del corrector ortográfico</td>
    </tr>
  </tbody>
</table>

<br>
Podemos entrar en un modo de edición haciendo `C-r`. Este modo permite hacer modificaciones en el buffer mientras realizamos la corrección. Para recuperar la sesión del corrector haremos `C-M-c`.

<br>
Para suspender la sesión de corrección actual haremos `C-g`. Haciendo `SPC u M-$` recuperamos la sesión en el punto en el que la dejamos.

<br>
Dired
====

<br>
Dired es el navegador de directorios de Emacs. Accedemos a él haciendo `SPC f f` y luego eligiendo un directorio. Alternativamente, podemos hacer `SPC f j`, lo que ejecutará `dired-jump`. Este comando abre el directorio conteniendo el archivo que tengamos abierto en el momento. En Dired tenemos el siguiente listado de opciones:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Dired</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">+</td>
      <td>Permite crear un directorio</td>
    </tr>
    <tr>
      <td>q</td>
      <td>Cierra la sesión de Dired</td>
    </tr>
    <tr>
      <td>n</td>
      <td>Mueve el cursor al siguiente elemento</td>
    </tr>
    <tr>
      <td>p</td>
      <td>Mueve el cursor al elemento anterior</td>
    </tr>
    <tr>
      <td>p</td>
      <td>Mueve el cursor al elemento anterior</td>
    </tr>
    <tr>
      <td>[Enter] o f</td>
      <td>Abre el archivo o abre una nueva sesión de Dired con el nuevo directorio</td>
    </tr>
    <tr>
      <td>v</td>
      <td>Abre el archivo en view-mode</td>
    </tr>
    <tr>
      <td>o</td>
      <td>Abre el archivo en otra ventana</td>
    </tr>
    <tr>
      <td>^</td>
      <td>Permite moverse al directorio padre</td>
    </tr>
    <tr>
      <td>m</td>
      <td>Marca un archivo o subdirectorio</td>
    </tr>
    <tr>
      <td>u</td>
      <td>Desmarca un archivo o subdirectorio</td>
    </tr>
    <tr>
      <td>C</td>
      <td>Copia los elementos marcados</td>
    </tr>
    <tr>
      <td>* .</td>
      <td>Permite marcar archivos por extensión</td>
    </tr>
    <tr>
      <td>R</td>
      <td>Permite renombrar/mover archivos/directorios</td>
    </tr>
    <tr>
      <td>D</td>
      <td>Elimina los elementos marcados</td>
    </tr>
    <tr>
      <td>d</td>
      <td>Marcar archivos para eliminación</td>
    </tr>
    <tr>
      <td>x</td>
      <td>Elimina los archivos marcados para eliminación</td>
    </tr>
  </tbody>
</table>

<br>
Para una lista mas extensa de comandos descargar [esta guía](https://www.gnu.org/software/emacs/refcards/pdf/dired-ref.pdf).

<br>
Git
====

<br>
Spacemacs incorpora 2 layers de configuración para control de versionado en Git: `version-control` y `git`.

<br>
version-control
---------------

<br>
Esta layer agrega márgenes a los buffers dentro de un repositorio mostrando las diferencias respecto al contenido versionado. Haciendo `SPC g .` entramos al `vcs transient-state`. En este estado podemos recorrer los cambios introducidos con `n` y `N`. En cada cambio podemos luego mostrar las diferencias con `h` o revertir a la versión anterior con `r`.

<br>
Magit
-----

<br>
[Magit](https://magit.vc/) es una extensión que implementa una interfaz Git mnemónica muy fácil de utilizar. Suponiendo que tenemos abierto un archivo dentro de un repositorio Git, para empezar a utilizar Magit ingresamos `SPC g s`. Este atajo ejecuta `magit-status`, el comando principal de Magit. Tras ser ejecutado, se abrirá una ventana de estado mostrando el branch actual y el estado de cambios que aún no están en stage. En esta ventana podemos ingresar el siguiente listado de comandos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Magit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="25%">?</td>
      <td>Muestra el listado de acciones disponibles</td>
    </tr>
    <tr>
      <td>[Tab]</td>
      <td>Expande/Oculta el sumario de cambios en el elemento seleccionado</td>
    </tr>
    <tr>
      <td>s</td>
      <td>Hace staging de un cambio. El cursor deberá estar sobre alguno de los cambios en el listado</td>
    </tr>
    <tr>
      <td>S</td>
      <td>Hace staging de todos los cambios</td>
    </tr>
    <tr>
      <td>u</td>
      <td>Remueve un ítem de stage</td>
    </tr>
    <tr>
      <td>U</td>
      <td>Remueve todos los elementos de stage</td>
    </tr>
    <tr>
      <td>x</td>
      <td>Descarta los cambios en el elemento activo</td>
    </tr>
    <tr>
      <td>b b</td>
      <td>Realiza checkout de un branch</td>
    </tr>
    <tr>
      <td>b c</td>
      <td>Crea un nuevo branch</td>
    </tr>
  </tbody>
</table>

<br>
Si ingresamos `c` entramos en el menú para `Commit`. Ingresando `c` nuevamente podremos ingresar un mensaje de commit. En esta vista veremos el listado de cambios en la otra ventana. Para desplazar el contenido de la misma sin dejar la ventana actual podemos hacer `M-PgDown` y `M-PgUp`. Una vez ingresemos el mensaje haremos `C-c C-c` para finalizar el commit. Podemos luego realizar un `push` de los cambios ingresando `P` y luego eligiendo el branch a pushear.

<br>
A partír de Spacemacs 0.200, Magit agrega un *dispatch menu* accesible haciendo `SPC g m`.

<br>
helm-gitignore
--------------

<br>
`helm-gitignore` es una extensión que permite autogenerar archivos `.gitignore` utilizando [gitignore.io](https://www.gitignore.io/). Simplemente hacemos `SPC g I` y elegimos el tipo de archivo a generar desde la lista.

<br>
Markdown
====

<br>
Markdown es un formato sencillo para generar documentación que tiene la ventaja de ser abierto y soportado por Github, Gitlab y Bitbucket. Estos son algunos de los comandos soportados por `markdown-mode`:

<br>
<table>
  <thead>
    <tr>
      <th colspan="2">Markdown Mode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">SPC m h !</td>
      <td>Inserta una cabecera</td>
    </tr>
    <tr>
      <td>SPC m h @</td>
      <td>Inserta una subcabecera</td>
    </tr>
    <tr>
      <td>SPC m i l</td>
      <td>Inserta un enlace. El texto del mismo será aquel bajo el cursor</td>
    </tr>
    <tr>
      <td>SPC m i u</td>
      <td>Inserta una URL</td>
    </tr>
    <tr>
      <td>SPC m i i</td>
      <td>Inserta una imagen</td>
    </tr>
    <tr>
      <td>SPC m k</td>
      <td>Elimina el elemento bajo el cursos</td>
    </tr>
  </tbody>
</table>

<br>
El prefijo `SPC m h` también puede ser seguido de un número entre 1 y 6. El número indica el nivel de anidamiento de la cabecera a crear.

<br>
Seleccionando un fragmento de texto podemos aplicarle las siguientes transformaciones:

<br>
<table>
  <thead>
    <tr>
      <th colspan="2">Markdown Mode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">SPC m x b</td>
      <td>Pasa el texto en negrita</td>
    </tr>
    <tr>
      <td>SPC m x i</td>
      <td>Pasa el texto a itálica</td>
    </tr>
    <tr>
      <td>SPC m x q</td>
      <td>Convierte el texto a blockquote</td>
    </tr>
    <tr>
      <td>SPC m x c</td>
      <td>Convierte el texto a código</td>
    </tr>
    <tr>
      <td>SPC m x C</td>
      <td>Encierra el texto como código fuente. Permite definir el lenguaje</td>
    </tr>
  </tbody>
</table>

<br>
Una vez hayamos completado el documento podemos auto-generar un listado de contenidos con `markdown-toc` haciendo `SPC :` e ingresando `markdown-toc-generate-toc`.

<br>
Este modo también permite navegar entre elementos del mismo *orden*. Por ejemplo, podemos movernos entre las subcabeceras de un elemento con `gj` y `gk`.

<br>
Vista previa
------------

<br>
Para poder generar una vista previa del documento hacemos `SPC m c p`, lo que ejecutará `markdown-preview`. Para que esto funcione será necesario instalar el comando `markdown` en el sistema. Este comando crea un archivo xHTML temporal con nuestro documento que es luego abierto en el browser. Para transformar la vista previa a HTML5 utilizando UTF-8 abrimos nuestro archivo de configuración y agregamos esto dentro del hook `dotspacemacs/user-init`.

<br>
``` emacs-lisp
  (eval-after-load "markdown-mode"
    '(defalias 'markdown-add-xhtml-header-and-footer 'as/markdown-add-xhtml-header-and-footer))

  (defun as/markdown-add-xhtml-header-and-footer (title)
    "Wrap XHTML header and footer with given TITLE around current buffer."
    (goto-char (point-min))
    (insert "<!DOCTYPE html5>\n"
            "<html>\n"
            "<head>\n<title>")
    (insert title)
    (insert "</title>\n")
    (insert "<meta charset=\"utf-8\" />\n")
    (when (> (length markdown-css-paths) 0)
      (insert (mapconcat 'markdown-stylesheet-link-string markdown-css-paths "\n")))
    (insert "\n</head>\n\n"
            "<body>\n\n")
    (goto-char (point-max))
    (insert "\n"
            "</body>\n"
            "</html>\n"))
```

<br>
Será necesario reiniciar despues de este cambio. Esta configuración fue tomada de <https://www.emacswiki.org/emacs/MarkdownMode>.

<br>
Org
====

<br>
`org-mode` es un modo que permite administrar notas y actividades en texto plano. Dada la complejidad del mismo vamos a ver simplemente un resumen de los comandos disponibles. Comencemos por introducir algunos términos:

<br>
 * **Cabeceras**: Las cabeceras agrupan un conjunto de subcabeceras o actividades. Por lo general, los archivos en `org-mode` tendrán una cabecera con múltiples subcabeceras y actividades.
 * **Tareas**: Son actividades a realizar. Se reconocen por el uso de la palabra clave `TODO`. Pueden tener fecha de comienzo y finalización (o `deadline`).
 * **org-agenda**: Es la manera en la que `org-mode` organiza múltiples archivos `org`.
 * **org-capture**: Es una utilidad para capturar notas mientras se trabaja.

<br>
Cabeceras
---------

<br>
Para empezar a utilizar este modo haremos lo siguiente:

<br>
 * Abrimos un nuevo archivo (`SPC f f`) con el nombre `cookbook.org`.
 * Al abrirlo vamos a ver la palabra `Org` en la `mode-line` indicando que estamos en `org-mode`.
 * Para insertar una cabecera hacemos `SPC m h I`.
 * Agregamos un texto para identificar a la cabecera.
 * Ahora hacemos `SPC m h i`. Una nueva cabecera es creada. Le asignamos otro nombre.

<br>
Si se hizo todo correctamente entonces deberíamos obtener 2 cabeceras distintas. Ahora nos ubicamos en la subcabecera y hacemos lo siguiente:

<br>
 * Ingresamos `o` para agregar un texto descriptivo bajo una cabecera.
 * Ahora hacemos `M-o`. Una subcabecera es creada, a la cual le ponemos otro nombre.
 * Haciendo `SPC m h i` agregaremos otra subcabecera.

<br>
Podemos navegar entre cabeceras utilizando los atajos provistos por `evil-org-mode`.

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Navegación entre cabeceras</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">gj</td>
      <td>Mueve el cursor a la cabecera siguiente del mismo nivel</td>
    </tr>
    <tr>
      <td>gk</td>
      <td>Mueve el cursor a la cabecera anterior del mismo nivel</td>
    </tr>
    <tr>
      <td>gh</td>
      <td>Mueve el cursor a la cabecera padre</td>
    </tr>
    <tr>
      <td>gl</td>
      <td>Mueve el cursor a la próxima cabecera visible</td>
    </tr>
  </tbody>
</table>

<br>
Existen otros atajos cuyo comportamiento depende de la posición del cursor en la cabecera. Por ejemplo, hacer `M-[Enter]` agrega una cabecera del mismo nivel. Si el cursor está en el espacio en blanco entre el `bullet` y el texto de la cabecera el elemento es insertado arriba. En el caso de que se encuentre sobre texto, el texto ubicado despues del cursor es usado para el nombre de la nueva cabecera. Podemos anular este comportamiento haciendo `SPC u` previo a ingresar `M-[Enter]`.

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Cabeceras</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">M-[Enter]</td>
      <td>Inserta una nueva cabecera al mismo nivel que la actual</td>
    </tr>
    <tr>
      <td>C-[Enter]</td>
      <td>Igual a lo anterior, pero la cabecera será agregada respetando el cuerpo de la cabecera actual</td>
    </tr>
    <tr>
      <td>M-[Shift]-[Enter]</td>
      <td>Inserta una nueva tarea al mismo nivel que la cabecera</td>
    </tr>
    <tr>
      <td>M-k</td>
      <td>Mueve la cabecera hacia arriba</td>
    </tr>
    <tr>
      <td>M-j</td>
      <td>Mueve la cabecera hacia abajo</td>
    </tr>
    <tr>
      <td>M-h</td>
      <td>Sube la jerarquía de la cabecera</td>
    </tr>
    <tr>
      <td>M-H</td>
      <td>Sube la jerarquía de la subcabecera incluyendo subcabeceras</td>
    </tr>
    <tr>
      <td>M-l</td>
      <td>Baja la jerarquía de la cabecera</td>
    </tr>
    <tr>
      <td>M-L</td>
      <td>Baja la jerarquía de la cabecera incluyendo subcabeceras</td>
    </tr>
  </tbody>
</table>

<br>
Texto y Enlaces
-------

<br>
En `org-mode` podemos aplicar distintos tipos de transformaciones al texto insertado, así como también agregar enlaces. Los atajos comenzados con `SPC m x` permite modificar un texto en view mode.

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">org-mode ~ Texto</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="25%">SPC m x b</td>
      <td>Inserta texto en negrita</td>
    </tr>
    <tr>
      <td>SPC m x c</td>
      <td>Inserta texto como código</td>
    </tr>
    <tr>
      <td>SPC m x i</td>
      <td>Inserta texto en itálica</td>
    </tr>
    <tr>
      <td>SPC m x u</td>
      <td>Inserta texto subrayado</td>
    </tr>
    <tr>
      <td>SPC m i l</td>
      <td>Inserta un enlace</td>
    </tr>
    <tr>
      <td>SPC u SPC m i l</td>
      <td>Permite insertar un enlace a otro archivo org</td>
  </tbody>
</table>

<br>
Haciendo `SPC m i l` sobre un enlace existente nos permite modificarlo. En caso de querer solo copiar la ruta del enlace podemos agregar lo siguiente a nuestra configuración:

<br>
``` emacs-lisp
  ;; Copia un enlace en modo org al portapapeles
  (defun copy-org-link ()
    (interactive "P")
    (cond
     ((org-in-regexp org-bracket-link-regexp 1)
      (kill-new (org-link-unescape (org-match-string-no-properties 1)))
      (message "Link copied"))
     ((message "No link at point"))))
```

<br>
Esta función copiará al `kill-ring` la ruta del enlace ubicado bajo el cursor (siempre y cuando nos encontremos en `org-mode`). Luego podemos asociar una atajo para mayor comodidad:

<br>
``` emacs-lisp
  ;; Asociar "SPC o o" a copy-org-link
  (spacemacs/set-leader-keys "oo" 'copy-org-link)
```

<br>
Tareas
------

<br>
Ingresando `t` podemos transformar una cabecera en un `TODO` (o tarea). Ingresando `t` nuevamente marcamos la tarea como finalizada. Ubicando el cursor sobre una tarea podemos hacer uso de los siguientes atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Tareas</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">M-t</td>
      <td>Agrega una subtarea</td>
    </tr>
    <tr>
      <td>C-c ,</td>
      <td>Permite definir la prioridad de la tarea. Los valores soportados son "A", "B" y "C"</td>
    </tr>
    <tr>
      <td>C-c C-c</td>
      <td>Permite definir tags. Los tags ingresados deben ser separados por el carácter ":"</td>
    </tr>
    <tr>
      <td>SPC m d</td>
      <td>Permite definir una fecha límite (o deadline)</td>
    </tr>
    <tr>
      <td>SPC m s</td>
      <td>Permite definir una fecha de comienzo (o scheduled)</td>
    </tr>
    <tr>
      <td>SPC m P</td>
      <td>Permite definir propiedades. Las propiedades trabajan como tags pero con valores</td>
    </tr>
  </tbody>
</table>

<br>
Podemos definir los estados por los que transita una tarea ya sea en el mismo archivo o en nuestro propio archivo de configuración. Para esto, primero nos ubicamos en la primera línea del documento y agregamos lo siguiente:

<br>
`#+TODO: PENDIENTE EN-PROGRESO(s) | TERMINADA`

<br>
Esta línea define 4 estados: Pendiente, En Progreso y Terminada. El segundo estado define un alías entre paréntesis. Este se utiliza para asignar rápidamente un estado al ingresar `t` sobre la tarea. El símbolo `|` sirve para indicar que los estados ubicados a la derecha son finales. Para que los cambios tomen efecto necesitamos reabrir el archivo.

<br>
Si pretendemos que los estados se apliquen globalmente entonces podemos agregar lo siguiente a la configuración:

<br>

``` emacs-lisp
  (setq org-todo-keywords
        '((sequence "PENDIENTE" "EN-PROGRESO" "|" "TERMINADA")))
```

<br>
De manera similar podemos definir tags personalizados en el documento:

<br>
`#+TAGS: { @TRABAJO(t) @FAMILIA(f) } TELEFONO(T) PROYECTO(p)`

<br>
En este caso definimos 2 tags excluyentes (Trabajo y Familia). En caso de querer definirlos directamente en la configuración de Spacemacs debemos agregar lo siguiente:

``` emacs-lisp
(setq org-tag-alist '((:startgroup . nil)
                      ("@TRABAJO" . ?t) ("@FAMILIA" . ?f)
                      (:endgroup . nil)
                      ("TELEFONO" . ?T) ("PROYECTO" . ?p)))
```

<br>
Listas
------

<br>
El cuerpo bajo la cabecera o tarea puede incluir listas ordenadas y desordenadas. Las listas desordenadas se encabezan con `-`, `+` y `*`. Las ordenadas con `1` y `1.`.

<br>
```
* Listado de libros
  1. El lenguaje de programación C
  2. Beginning Linux Programming
    - Tomar apuntes
    - Probar scripts Bash
  3. The Linux Programming Interface :: Michael Kerrisk
```

<br>
Las listas pueden contener `checkboxes`. Esto se logra ingresando `[ ]` en el encabezado del elemento. Los `checkboxes` pueden ser chequeados y deschequados haciendo `C-c C-c`.

<br>
```
* TODO Implementar script de conexión
 - [ ] Descargar librería
 - [ ] Obtener credenciales
 - [ ] Testear conexión
```

<br>
Tablas
------

<br>
`org-mode` incluye soporte para tablas. Para agregar una tabla nos posicionamos bajo una cabecera y hacemos `SPC m t n`. A continuación ingresamos las dimensiones en columnas x filas (por defecto 5x2). Dentro de una tabla tenemos los siguientes atajos:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">Tablas</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="30%">[Tab]</td>
      <td>Avanza una celda. También funciona para modo de inserción</td>
    </tr>
    <tr>
      <td>[Shift]-[Tab]</td>
      <td>Retrocede una celda</td>
    </tr>
    <tr>
      <td>M-j</td>
      <td>Mueve la fila hacia abajo</td>
    </tr>
    <tr>
      <td>M-k</td>
      <td>Mueve la fila hacia arriba</td>
    </tr>
    <tr>
      <td>M-h</td>
      <td>Mueve una columna completa hacia la izquierda</td>
    </tr>
    <tr>
      <td>M-l</td>
      <td>Mueve una columna completa hacia la derecha</td>
    </tr>
    <tr>
      <td>M-J</td>
      <td>Agrega una fila, desplazando la actual hacia abajo</td>
    </tr>
    <tr>
      <td>M-K</td>
      <td>Elimina una fila</td>
    </tr>
    <tr>
      <td>M-L</td>
      <td>Agrega una columna, desplazando la actual hacia la derecha</td>
    </tr>
    <tr>
      <td>M-H</td>
      <td>Elimina una columna</td>
    </tr>
    <tr>
      <td>SPC m t b</td>
      <td>Vacía el contenido de una celda</td>
    </tr>
    <tr>
      <td>SPC m t s</td>
      <td>Ordena la tabla por la columna actual</td>
    </tr>
    <tr>
      <td>M-a</td>
      <td>Desplaza el cursor al comienzo de la celda</td>
    </tr>
    <tr>
      <td>M-e</td>
      <td>Desplaza el cursor al final de una celda</td>
    </tr>
    <tr>
      <td>SPC m t i h</td>
      <td>Inserta una línea separadora</td>
    </tr>
  </tbody>
</table>

<br>
Para insertar valores simplemente nos ubicamos en una celda y entramos en modo de inserción. Podemos avanzar a la siguiente presionando `[Tab]`.

<br>
Agenda
------

<br>
Utilizar `org-mode` eficientemente requerirá hacer uso de `org-agenda` y `org-capture`. Una configuración inicial para usar ambos puede ser la siguiente:

<br>
``` emacs-lisp
  ;; Setear configuración de org-mode a través de un hook
  (with-eval-after-load 'org
    (setq org-directory "~/git/org")
    (setq org-agenda-files (list "~/git/org/agenda"))
    (setq org-default-notes-file "notas.org")
    )
```

<br>
En Spacemacs, la configuración de `org-mode` debe realizarse a través `with-eval-after-load` o de otra manera no funcionará. La configuración utilizada define los siguientes valores:

<br>
 * El directorio que contendrá los archivos `.org` (`~/git/org`)
 * El directorio que contendrá los archivos de la agenda (`~/git/org/agenda`)
 * El archivo para notas (`notas.org`)

<br>
En este caso hemos decidido incluir los archivos `.org` en un repositorio Git por conveniencia. Este paso es sencillo de realizar y permite hacer una copia de seguridad de nuestras actividades.

<br>
Para utilizar la agenda podemos crear un archivo `.org` dentro de `~/git/org/agenda` conteniendo varias tareas por hacer. Es buena idea también asignarles `tags` para, más adelante, poder realizar búsquedas. Haciendo `SPC m a` ingresamos al modo agenda. En este modo podemos hacer lo siguiente:

<br>
 * Presionando `t` podemos ver una lista de tareas pendientes.
 * Presionando `s` podemos buscar elementos por nombre de `tag`.

<br>
En los listados que obtengamos podemos navegar al archivo haciendo `[Enter]` o abrirlo en la otra ventana con `[Tab]`. Para salir de este modo basta con ingresar `q`.

<br>
Si no nos encontramos en un buffer utilizando `org-mode` podemos acceder a la agenda con `SPC a o o`.

<br>
Otro uso interesante es visualizar la agenda de la semana. Ubicando el cursor sobre una tarea y haciendo `SPC m s` invocamos a `org-scheduled`. Desde aquí podemos definir una fecha pactada de comienzo para la actividad. Si luego volvemos a ejecutar `org-agenda` y elegimos la opción `a` podremos ver la actividad en la lista. También podemos navegar entre semanas haciendo `M-h` y `M-l`. `org-agenda` también se encarga de agregar las fechas *deadline* en caso de que las hayamos definido.

org-capture y org-refile
------------------------

<br>
En ocasiones necesitamos apuntar una actividad mientras trabajamos. `org-capture` es una utilidad que almacena notas y tareas en un archivo de manera sencilla. Para usarla vamos a agregar unas líneas extra en nuestra configuración:

<br>
``` emacs-lisp
  ;; Definir atajo para org-capture
  (spacemacs/set-leader-keys "oc" 'org-capture)

  ;; Abrir archivo de notas con F12
  (global-set-key (kbd "<f12>")
                  (lambda () (interactive) (find-file "~/git/org/notas.org")))
```

<br>
Ahora, haciendo `SPC o c` invocamos `org-capture`. Antes de definir el contenido de la actividad tenemos que elegir la plantilla que vamos a utilizar para la misma. Como no hemos definido ninguna, elegimos la opción por defecto con `t`. Luego definimos el nombre y contenido de la tarea. Para guardarla hacemos `C-c C-c`. Por defecto, `org-capture` almacena los datos en el archivo que hayamos definido como `org-default-notes-file` (o `notes.org` en caso de no definirlo). Este archivo estará ubicado en la carpeta que hayamos definido como `org-directory`.

<br>
Ahora haciendo F12 podemos abrir el archivo generado. A veces resulta apropiado mover notas de este archivo a otros para organizarse mejor. Este proceso se llama *refile*. Para generar una vista de posibles destinos para notas almacenadas a través de `org-capture` vamos a agregar la siguientes líneas de configuración:

<br>
``` emacs-lisp
  ;; Definir destinos por defecto para org-refile
  (setq org-refile-targets
        '((nil :maxlevel . 3)
          (org-agenda-files :maxlevel . 3)))
```

<br>
Recordar agregar esto al hook de configuración de `org-mode`. Supongamos que en nuestra agenda poseamos una archivo llamado `trabajo.org`, con una cabecera `Trabajo` y 2 subcabeceras llamadas `Proyecto A` y `Proyecto B`. Si ahora vamos al archivo de notas y nos ubicamos sobre una tarea creada previamente podremos mover el contenido de la misma a alguna de esas cabeceras haciendo `SPC m R`. Será necesario luego guardar los cambios en el archivo destino luego de esto.

<br>
Links Utiles
------------

<br>
Para conocer más acerca de `org-mode` recomiendo visitar estos enlaces:

<br>
 * [Org-mode beginning at the basics](http://orgmode.org/worg/org-tutorials/org4beginners.html)
 * [David O'Toole Org tutorial](http://orgmode.org/worg/org-tutorials/orgtutorial_dto-es.html) (en castellano)
 * [Org-Mode Beginners Customization Guide](http://orgmode.org/worg/org-configs/org-customization-guide.html)
 * [La guía compacta de Org-mode](http://www.davidam.com/docu/orgguide.es.html) (en castellano)

<br>
Javascript
====

<br>
Este modo requiere tener instalada una versión reciente de Node. Previo a activar este modo debemos instalar `tern`, `js-beautify` y `jshint`.

<br>
```shell
 npm install -g tern js-beautify jshint
```

<br>
Ahora solo resta agregar `javascript` al listado de layers en el archivo de configuración. Este modo ofrece 2 valores de configuración para la indentación en archivos Javascript y JSON.

<br>
```emacs-lisp
  ;; Javascript
  (setq-default js2-basic-offset 2)
  ;; JSON
  (setq-default js-indent-level 2)
```

<br>
`js2-mode` presenta gran cantidad de atajos para tareas de refactorizado:

<br>
<table width="95%">
  <thead>
    <tr>
      <th colspan="2">js2-mode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>SPC m w</td>
      <td>Activa/desactiva advertencias y errores generados</td>
    </tr>
    <tr>
      <td width="30%">SPC m =</td>
      <td>Aplica web-beautify</td>
    </tr>
    <tr>
      <td>SPC m g g</td>
      <td>Salta a la definición del símbolo bajo el cursor</td>
    </tr>
    <tr>
      <td>SPC m g G</td>
      <td>Salta a la definición del símbolo que ingresemos</td>
    </tr>
    <tr>
      <td>SPC m r r V</td>
      <td>Permite renombrar el símbolo bajo el cursor</td>
    </tr>
    <tr>
      <td>SPC m z e</td>
      <td>Permite mostrar/ocular el cuerpo de una función</td>
    </tr>
  </tbody>
</table>

<br>
Podemos también refactorizar un elemento haciendo `SPC m r r v`. En este modo se utilizan varios cursores. Luego de ingresar el atajo tipeamos `c`, el nuevo identificador y `[Enter]`.

<br>
PHP
====

<br>
Luego de agregar el layer `php` y realizar la recarga del archivo de configuración ingresar `SPC :` y luego `php-extras-generate-eldoc`. Al ejecutar esta función Spacemacs descargará el listado de funciones de PHP para proveer documentación en `php-mode`. Este proceso puede demorar varios minutos.

<br>
Licencia
========

<br>
Liberado bajo licencia MIT.
