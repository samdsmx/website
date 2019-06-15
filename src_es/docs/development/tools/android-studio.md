---
title: Android Studio / IntelliJ
description: How to develop Flutter apps in Android Studio or other IntelliJ products.
---

<ul class="nav nav-tabs" id="ide" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" role="tab" aria-selected="true">Android Studio / IntelliJ</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="/docs/development/tools/vs-code" role="tab" aria-selected="false">Visual Studio Code</a>
  </li>
</ul>

## Instalación y configuración

Sigue las instrucciones de [Configura un Editor](/docs/get-started/editor?tab=androidstudio)
para instalar los plugins Dart y Flutter.

### Actualizando los plugins<a name="updating"/>

Las actualizaciones de los plugins se envían regularmente. Deberías recibir un aviso 
en IntelliJ cuando haya una actualización disponible.

Para buscar actualizaciones manualmente:

 1. Abre preferencias (**Android Studio>Check for Updates...** en macOS, 
    **Help>Check for Updates...** en Linux).
 1. Si `dart` o `flutter` están instalados, actualízalos.

## Creando proyectos

### Creando un nuevo proyecto

Para crear un nuevo proyecto de Flutter a partir de la plantilla de la aplicación de inicio Flutter:

1. En el IDE, haz clic **Create New Project** desde la ventana 'Welcome' o
**File>New>Project...** desde la ventana principal del IDE.
1. Selecciona **Flutter** en el menú, y clic en **Next**.
1. Ingresa tu **Project name** y **Project location** deseado.
1. Si piensas publicar esta aplicación, [establece el dominio de la compañía](#note).
1. Clic en **Finish**.

<aside class="alert alert-info" markdown="1">
  <h4 id="note" class="no_toc">Setting the company domain</h4>

  Al crear una nueva aplicación, algunos plugins de IDE de Flutter piden un 
  nombre de la organización en orden de dominio inverso, 
  algo así como `com.example`. Junto con el nombre de la aplicación, esto se usa 
  como el nombre del paquete para Android y el ID del paquete para iOS cuando se 
  lanza la aplicación. Si crees que alguna vez lanzarás esta aplicación, es mejor 
  especificar estos ahora. No se pueden cambiar una vez que la aplicación sea 
  lanzada. El nombre de su organización debe ser único.
</aside>

### Creando un nuevo proyecto a partir de código fuente existente

Para crear un nuevo proyecto de Flutter que contenga los archivos de código fuente 
de Flutter existente:

 1. En el IDE, clic **Create New Project** desde la ventana 'Welcome' o 
    **File>New>Project...** desde la ventana principal del IDE.
    
    {{site.alert.important}}
      *No* uses la opción **New>Project from existing sources...** para proyectos Flutter.
    {{site.alert.end}}
 1. Seleccione **Flutter** en el menú, y clic en **Next**.
 1. En **Project location**, ingresa o navega hasta el directorio que contiene tus 
    archivos de código fuente existentes de Flutter.
1. Clic en **Finish**.

## Edición de código y visualización de problemas de código

El plugin Dart realiza análisis de código que permite:

* Resaltado de sintaxis
* Completado de código basado en análisis de tipo enriquecido.
* Navegando para escribir declaraciones (**Navigate>Declaration**) y buscar tipo
  de usos (**Edit>Find>Find Usages**).
* Visualización de todos los problemas actuales del código fuente (**View>Tool   Windows>Dart Analysis**). 
  Cualquier problema de análisis se muestra en el panel Análisis de Dart:<br>
  ![Dart Analysis pane]({% asset tools/android-studio/dart-analysis.png @path %})

## Ejecución y depuración

La ejecución y la depuración se controlan desde la barra de herramientas principal:

![Main IntelliJ toolbar]({% asset tools/android-studio/main-toolbar.png @path %})

### Seleccionando un objetivo

Cuando se abre un proyecto de Flutter en el IDE, debería ver un conjunto de botones 
específicos de Flutter en el lado derecho de la barra de herramientas.

{{site.alert.note}}
  Si los botones Ejecutar y Depurar están desactivados y no hay ningún objetivo en 
  la lista, Flutter no ha podido descubrir ningún dispositivo o simulador iOS o Android 
  conectado. Necesita conectar un dispositivo o iniciar un simulador para continuar.
{{site.alert.end}}

 1. Ubica el botón desplegable **Flutter Target Selector**. Esto muestra una lista de 
    objetivos disponibles. 
 2. Selecciona el destino en el que deseas que se inicie tu aplicación. 
    Cuando conectas dispositivos o inicias simuladores, aparecen entradas adicionales.

### Ejecuta la aplicación sin puntos de interrupción

 1. Clic en **Play icon** en la barra de herramientas, o invoca **Run>Run**. 
   * El panel inferior **Run** muestra el resultado de los registros:<br> 
   ![Log pane]({% asset tools/android-studio/log.png @path %})

### Ejecutar aplicación con puntos de interrupción

 1. Si lo deseas, establezca puntos de interrupción en tu código fuente.
 1. Clic en **Debug icon** en la barra de herramientas, o invoca **Run>Debug**.
    * El panel inferior **Debugger** muestra Stack Frames y Variables.
    * El panel inferior **Console** muestra la salida de registros detallados.
    * La depuración se basa en una configuración de inicio predeterminada. Para 
      personalizar esto, haz clic en el botón desplegable a la derecha del 
      selector de dispositivo, y selecciona **Edit configuration**.

## Edición rápida y actualización en ciclo de desarrollo

Flutter ofrece el mejor ciclo de desarrollo de su clase, permitiéndote ver 
el efecto de tus cambios casi instantáneamente con la función de "hot reload". Mira
[Usando Hot reloads](hot-reload) para más detalles.

## Depuración avanzada

### Depuración de problemas de diseño visual

Para depurar un problema visual, inicia la aplicación con 'Debbuger', y luego abre la
ventana de la herramienta Flutter Inspector usando 'View> Tool Windows> Flutter Inspector'.

![Flutter Inspector Window]({% asset tools/android-studio/visual-debugging.png @path %})

Esto ofrece muchas herramientas de depuración; para detalles sobre estas mira
[Depurando Apps en Flutter][].

* **Enable Select Widget Mode**: Selecciona un widget en el dispositivo para inspeccionarlo en el
  [Flutter Inspector](/docs/development/tools/inspector).
* **Refresh widget info**: Recarga la información del widget actual.
* **Show/hide Performance Overlay**: Mostrar gráficos de rendimiento para los hilos de la GPU y la CPU.
* **Toggle Platform Mode**: Alternar entre renderizado para Android o iOS.
* **Show Debug Paint**: Agregue sugerencias de depuración visual a la presentación que muestra 
  bordes, relleno, alineación y espaciadores.
* **Show paint baselines**: Provoca que cada RenderBox pinte una linea en cada una 
  de sus lineas bases de texto.
* **Enable slow animations**: Ralentiza las animaciones para permitir la inspección 
  visual.

También disponible en el menú de acciones adicionales:

* **Show Repaint Rainbow**: Muestra colores rotativos en las capas cuando repinta.
* **Hide Debug Mode Banner**: Ocultar el banner 'debug' incluso cuando se ejecuta una compilación de depuración.
* **Highlight nodes displayed in both trees**: En el inspector, resalta 
  los nodos que se muestran tanto en los detalles como en resumen del árbol.

### Ver datos de rendimiento

Para ver los datos de rendimiento, incluyendo la información de reconstrucción de widget,
inicia la app en modo **Debug**, y entonces abre la Performance tool window
usando **View > Tool Windows > Flutter Performance**.

![Flutter performance window]({% asset tools/android-studio/widget-rebuild-info.png @path %})

Para ver las estadísticas sobre que widgets son reconstruidos, y con que frecuencia,
haz clic en **Show widget rebuild information** en el panel **Performance**.
La cuenta exacta de reconstrucciones para este frame se muestra en la 
segunda columna por la derecha. Para un alto número de reconstrucciones, se muestra 
un círculo amarillo girando. La columna más a la derecha muestra cuantas 
veces fue reconstruido un widget desde que se entró en la pantalla actual.
Para widgets que no son reconstruidos, se muestra un círculo gris sólido.
En otro caso, se muestra un círculo gris girando.

{{site.alert.secondary}}
  La app mostrada en esta captura de pantalla ha sido diseñada para dar 
  un mal rendimiento, y el 'rebuild profiler' te da una pista 
  sobre que esta ocurriendo en el frame que puede estar causando 
  mal rendimiento. El 'widget rebuild profiler' no es una herramienta de 
  diagnóstico, por si sola, sobre mal rendimiento.
{{site.alert.end}}

El propósito de esta funcionalidad es hacerte consciente cuando los 
widgets son reconstruidos&mdash;puede que no te des cuenta de que esto 
está sucediendo mirando el código. Si los widgets se están reconstruyendo 
de una manera que no esperabas,
es una señal probable, que deberías refacorizar tu código fragmentando 
los métodos build grandes en múltiples widgets.

Esta heramienta puede ayudarte a depurar al menos cuatro problemas 
comunes de rendimiento:

1. La pantalla completa (o partes grandes de ella) son construidos con un 
   único StatefulWidget, causando la construcción innecesasria del interfaz de usuario. 
   Sepáralo en widgers más pequeños con funciones `build()` más pequeñas.

1. Los widgets fuera de la pantalla se están reconstruyendo. Esto puede suceder, por ejemplo,
   cuando un ListView está anidado en un Column alto que se extiende fuera de la pantalla.
   O cuando RepaintBoundary no esta configurada para una lista que se extiende fuera 
   de la pantalla, causando que la lista entera se vuelva a dibujar.

1. La función `build()` para un AnimatedBuilder dibuja un subárbol que 
   no necesita ser animado, causando la reconstrucción innecesaria de objetos 
   estáticos.

1. Un widget Opacity es colocado innecesariamente alto en el árbol de widget.
   O, una animación Opacity se cre manipulando directamente la propiedad 
   opacity de un widget Opacity, provocando que el propio widget
   y su subárbol sean reconstruidos.

Puedes hacer clic en una linea en la tabla para navegar a la linea en 
la fuente donde se crea el widget. A medida que se ejecuta el código,
los iconos giratorios también se muestran en el panel de código para ayudarte 
a visualizar que reconstrucciones están ocurriendo.

Fíjate que el hecho de que se produzcan numerosas reconstrucciones no indica necesariamente un problema.
Normalmente solo deberías preocuparte por las excesivas reconstrucciones si ya 
has ejecutado la app en modo perfilado y verificado que el rendimiento 
no es el que esperas.

Y recuerda, _la información de reconstrucción está solo disponible en un 
build de tipo debug_. Pruena el rendimineto de la ap en un dispositivo real con 
un perfil de tipo build, pero depura los problemas de rendimiento en un 
build de tipo debug.

### Depurar con Dart DevTools

Dart DevTools son un conjunto de herramientas de depuración y perfilado presentada 
en un interfaz de usuario basado en html. DevTools reemplaza la anterior herramienta 
de perfilado basada en navegador, Observatory.

DevTools está aíun en desarrollo pero disponible para previsualización. Para 
instrucciones de instalación y como empezar, mira las [DevTools' docs][].

## Consejos de edición para el código Flutter

Si tienes consejos adicionales que debemos compartir, por favor [háznoslo saber][]!

### Asistencias y soluciones rápidas

Las asistencias son cambios de código relacionados con un determinado identificador 
de código. Varias de ellas están disponibles cuando el cursor se coloca en un 
identificador de widget Flutter, como se indica mediante el icono de bombilla amarilla. 
La asistencia se puede invocar haciendo clic en la bombilla o usando el atajo de teclado 
(`Alt` +`Enter` en Linux y Windows, `Option` +`Return` en macOS), 
como se ilustra aquí:

![IntelliJ editing assists]({% asset tools/android-studio/assists.gif @path %})

Las soluciones rápidas son similares, solo se muestran con un código que tiene un error 
y pueden ayudar a corregirlo. Están indicados con una bombilla roja.

#### Ajustar con la nueva asistencia de widgets

Esto se puede usar cuando tiene un widget que desea envolver en un widget circundante, 
por ejemplo, si desea envolver un widget en un `Row` o un `Column`.

####  Ajustar la lista de widgets con la nueva asistencia de widgets

Similar a la asistencia anterior, pero para envolver una lista existente de widgets en 
lugar de un widget individual.

#### Asistencia para convertir child a children
Cambiar un argumento child a un argumento children, y ajustar el valor del argumento en una lista.

### Live templates

Los Live templates se pueden usar para acelerar la entrada a estructuras de código 
típicas. Se invocan al escribir su 'prefijo' y luego seleccionarlo en la ventana de 
finalización del código:

![IntelliJ live templates]({% asset tools/android-studio/templates.gif @path %})

El plugin Flutter incluye las siguientes plantillas:

* Prefijo `stless`: Crea una nueva subclase de `StatelessWidget`.
* Prefijo `stful`: Crea una nueva subclase de `StatefulWidget` y su subclase State 
  asociada.
* Prefijo `stanim`: Crea una nueva subclase de `StatefulWidget`, y su subclase de 
  estado asociada que incluye un campo inicializado con un 
  `AnimationController`.

Puedes también definir plantillas personalizadas en **Settings > Editor > Live Templates**.

### Atajos de teclado

**Hot reload**

En Linux (keymap _Por defecto para XWin_) y windows los atajos de teclado
son `Controle`+`Alt`+`;` y `Control`+`Backslash`.

En macOS (keymap _Mac OS X 10.5+ copy_) los atajos de teclado son `Command`+`Option`;
y `Command`+`Backslash`.

Las asignaciones de teclado se pueden cambiar en las Preferencias / Configuraciones de 
IDE: Seleccione *Mapa de teclas*, luego ingrese _flutter_ en el cuadro de búsqueda en 
la esquina superior derecha. Haga clic derecho en el enlace que desea cambiar y _Agregue acceso directo al teclado_.

![IntelliJ Settings Keymap]({% asset tools/android-studio/keymap-settings-flutter-plugin.png @path %})

### 'Hot reload' vs 'Full Application Restarts'

Hot reload funciona inyectando archivos actualizados de código fuente en la 
máquina virtual Dart en ejecución (máquina virtual). Esto incluye no solo agregar nuevas 
clases, sino también agregar métodos y campos a las clases existentes, y cambiar las 
funciones existentes. Sin embargo, algunos tipos de cambios de código no pueden ser recargados en caliente:

* Inicializadores variables globales.
* Inicializadores de campo estáticos.
* El método `main()` de la aplicación.

Para estos cambios, puedes reiniciar completamente tu aplicación, sin tener que finalizar 
tu sesión de depuración:

1. No hagas clic en el botón Detener; simplemente vuelve a hacer clic en el botón Ejecutar 
(si estás en una sesión de ejecución) o en el botón Depurar (si estás en una sesión de depuración), 
o haga clic y haz clic en el botón 'recargar en caliente'.

## Editar código Android en Android Studio con soporte completo del IDE {#android-ide}

Abrir el directorio del proyecto Flutter no expone todos los ficheros Android 
al IDE. Las apps Flutter contienen un subdirectorio llamado `android`. Si abres 
este subdirectorio como un proyecto separado en Android Studio, el IDE 
permitirá editar y refactorizar todos los ficheros Android (como los scripts Gradle) con soporte completo.

Si ya tienes abierto un proyecto completo como una app Flutter en in Android
Studio, hay dos maneras equivalentes de abrir los ficheros Android por su cuenta 
para editar en el IDE. Antes de probar esto, asegúrate que tienes la última version 
de Android Studio y plugins de Flutter.

* En la ["project view"][], debes ver un subdirectorio inmediatamente bajo el raiz 
  de tu app flutter llamado `android`. Haz clic derecho en él,
  entonces selecciona **Flutter > Open Android module in Android Studio**.
* O, puedes abrir cualquier fichero dentro del subdirectorio `android` para 
  editar. Deberías entonces ver un banner "Flutter commands" en la parte superior del 
  editor con un link etiquetado **Open for Editing in Android Studio**.
  Haz click en este link.

Para ámbas opciones, Android Studio te da la opción de usar una ventana separada o 
de reemplazar la ventana existente con el nuevo proyecto. Cualquier opción es buena.

Si no tienes ya abierto el proyecto Flutter en Android studio,
puedes abrir los ficheros Android como su propio proyecto desde inicio:

1. Haz clic en **Open an existing Android Studio Project** en la pantalla de bienvenida,
   o **File > Open** si Android Studio ya está abierto.
2. Abre el subdirectorio `android` bajo el raiz de la app flutter. Por
   ejemplo si el proyecto se llama `flutter_app`, abre `flutter_app/android`.

Si no has ejecutado la app Flutter todavía, verás un reporte de error de compilación en Android Studio cuando abres el proyecto `android`. Ejecuta `flutter pub get` en 
el directorio raíz de la app y recompila el proyecto seleccionando **Build > Make**
para corregir esto.

## Edición de código de Android en IntelliJ IDEA {#edit-android-code}

Para habilitar la edición del código de Android en IntelliJ IDEA, debes configurar la ubicación 
del SDK de Android:

1. En **Preferences > Plugins**, habilita **Android Support** si aún no lo has hecho.
1. Clic Derecho en la carpeta **android** en la vista Project, y selecciona **Open
Module Settings**.
1. En la pestaña **Sources**, localiza el campo **Language level**, y selecciona nivel '8'
o superior.
1. En la pestaña **Dependencies**, localiza el campo **Module SDK**, y selecciona un
SDK de Android. Si ningún SDK es listado, haga clic en **New...** y especifica la localización del
SDK de Android. Asegúrate de seleccionar un SDK de Android que coincida con el utilizado por Flutter 
(según lo informado por `flutter doctor`).
1. Clic en **OK**.

## Consejos y trucos

* [Flutter IDE cheat sheet, MacOS version][]
* [Flutter IDE cheat sheet, Windows & Linux version][]

## Solución de problemas

### Problemas conocidos y comentarios

Los problemas conocidos importantes que pueden afectar a tu experiencia están documentados en 
el archivo [README del plugin de Flutter][].

Todos los errores conocidos se siguen en los issue trackers:

* Plugin de Flutter: [GitHub issue tracker][].
* Dart plugin: [JetBrains YouTrack][].

Todos los comentarios son bien recibidos, tanto sobre errores/problemas como sobre solicitudes de 
funciones. Antes de presentar nuevos problemas, por favor:

* Haga una búsqueda rápida en los issue trackers para ver si el problema ya fue rastreado.
* Asegúrese de tener [actualizado](#updating) la versión más reciente del 
plugin.

Cuando presente nuevos problemas, incluya el resultado de [`flutter doctor`][].

[DevTools' docs]: https://flutter.github.io/devtools
[GitHub issue tracker]: {{site.repo.flutter}}-intellij/issues
[JetBrains YouTrack]: https://youtrack.jetbrains.com/issues?q=%23dart%20%23Unresolved
[`flutter doctor`]: /docs/resources/bug-reports#provide-some-flutter-diagnostics
[Flutter IDE cheat sheet, MacOS version]: /docs/resources/Flutter-IntelliJ-cheat-sheet-MacOS.pdf
[Flutter IDE cheat sheet, Windows & Linux version]: /docs/resources/Flutter-IntelliJ-cheat-sheet-WindowsLinux.pdf
[Depurando Apps en Flutter]: /docs/testing/debugging
[README del plugin de Flutter]: {{site.repo.flutter}}-intellij/blob/master/README.md
["project view"]: {{site.android-dev}}/studio/projects/#ProjectView
[háznoslo saber]: {{site.github}}/flutter/website/issues/new
