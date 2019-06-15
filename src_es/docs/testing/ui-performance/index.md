---
title: Perfiles de Rendimiento en Flutter
subtitle: Dónde buscar cuando tu aplicación Flutter deja caer frames en la UI.
description: Diagnóstico de problemas de rendimiento de la UI en Flutter.
---

{{site.alert.secondary}}
  <h4 class="no_toc">Lo que aprenderás:</h4>

  * El objetivo de Flutter es proporcionar un rendimiento de 60 frames por segundo (fps), 
    o un rendimiento de 120 fps en dispositivos capaces de realizar actualizaciones de 120 Hz.
  * Para 60fps, los frames se renderizan aproximadamente cada 16ms.
  * El Jank se produce cuando la UI no se renderiza fluidamente. Por ejemplo, de vez en cuando, 
    un frame tarda 10 veces más en renderizarse, por lo que se descarta, y la animación 
    se sacude visiblemente.
{{site.alert.end}}

Se dice que "una aplicación _rápida_ es genial, pero una _fluida_ es aún mejor".
Si tu aplicación no se está renderizando correctamente, ¿cómo la arreglas? ¿Por dónde empiezas?
Esta guía te muestra por dónde empezar, los pasos a seguir y las herramientas que pueden 
ayudarte.

{{site.alert.note}}
  - El rendimiento de una app está determinado por más de una medida. 
    El rendimiento a veces se refiere a la velocidad en bruto, pero también a 
    la fluidez de la UI y la falta de fluidez. Otros ejemplos de rendimiento 
    incluyen I/O o velocidad de red. Esta página principalmente se centra en el 
    segundo tipo de rendimiento (fluidez de la UI), pero puedes utilizar la mayoría de 
    las mismas herramientas que se usan para diagnosticar otros problemas de rendimiento.
  - Para realizar el seguimiento dentro de tu código Dart, consulta [Seguimiento del rendimiento de 
    cualquier código Dart][] 
    en la página [Depura tu app][]
{{site.alert.end}}

## Diagnóstico de problemas de rendimiento

Para diagnosticar una aplicación con problemas de rendimiento, habilitarás la capa 
sobrepuesta de rendimiento para ver los subprocesos de la UI y la GPU. Antes de empezar, 
deberás asegurarte de que estás ejecutando en modo profile y de que no estás usando un 
emulador. Para obtener mejores resultados, puedes elegir el dispositivo más lento que 
tus usuarios puedan utilizar.

{% comment %}
{{site.alert.secondary}}
  <h4 class="no_toc">¿Que aprenderás?</h4>

* Realiza el Profile de tu aplicación en un dispositivo físico.
* Realiza el Profile de tu app en [profile mode][].
* Comprueba el rendimiento en el dispositivo más lento que puedan utilizar tus usuarios.
* Comienza activando la capa de rendimiento.

{{site.alert.end}}
{% endcomment %}

### Conéctate a un dispositivo físico

Casi toda la depuración del rendimiento de las aplicaciones Flutter debe realizarse en un 
dispositivo físico Android o iOS, con su aplicación Flutter ejecutándose en modo profile. 
Usar el modo debug, o ejecutar aplicaciones en simuladores o emuladores, generalmente no 
es indicativo del comportamiento final de las compilaciones del modo release. _Deberías 
considerar comprobar el rendimiento en el dispositivo más lento que tus usuarios 
puedan usar razonablemente_.

{{site.alert.secondary}}
  <h4 class="no_toc" markdown="1">***Por qué debe ejecutarse en un dispositivo real:**</h4>

* Los simuladores y emuladores no utilizan el mismo hardware, por lo que sus características de 
rendimiento son diferentes&mdash; algunas operaciones son más rápidas en los simuladores que en 
los dispositivos reales, y otras son más lentas.
* El modo depuración permite realizar comprobaciones adicionales (como asserts) que no se ejecutan en 
compilaciones profile o release, y estas comprobaciones pueden ser costosas.
* El modo de depuración también ejecuta el código de una manera diferente que el modo release. La 
construcción del debug compila el código de Dart "just in time" (JIT) a medida que la aplicación se 
ejecuta, pero las construcciones de profile y release se precompilan a instrucciones nativas 
(también llamadas "ahead of time", o AOT) antes de que la aplicación se cargue en el dispositivo. 
JIT puede hacer que la aplicación se pause para la compilación de JIT, lo que a su vez puede 
causar jank.
{{site.alert.end}}

### Ejecutar en modo profile

El modo profile de Flutter compila e inicia tu aplicación de forma casi 
idéntica al modo release, pero con la funcionalidad adicional suficiente 
para permitir la depuración de problemas de rendimiento.
Por ejemplo, el modo de perfil proporciona información de trazabilidad a las herramientas de perfilado.

Lanza la aplicación en modo profile de la siguiente manera:

{% comment %}
Aún no disponible en VS Code.
{% endcomment %}

- En Android Studio e IntelliJ, usa el elemento del menú
    **Run > Flutter Run main.dart in Profile Mode**.
- En VS Code, abre tu archivo `launch.json`, y asigna la propiedad
`flutterMode` a `profile`
(cuando termines el profile, cámbialo de nuevo hacia `release` o `debug`):

```json
"configurations": [
	{
		"name": "Flutter",
		"request": "launch",
		"type": "dart",
		"flutterMode": "profile"
	}
]
```
- Desde la línea de comando, usa el 
parámetro `--profile`:

```terminal
  $ flutter run --profile
```

Para obtener más información sobre cómo funcionan los diferentes modos, consulta 
[Modos en Flutter]([].

Comenzarás activando la capa sobrepuesta de rendimiento, como se explica en la 
siguiente sección.

## La capa sobrepuesta de rendimiento

La capa sobrepuesta de rendimiento muestra las estadísticas en dos gráficos que muestran 
dónde se está gastando el tiempo tu aplicación.
Si la UI está en jank (saltando frames), estos gráficos te ayudan a averiguar por qué.
Los gráficos se muestran encima de tu aplicación en ejecución, pero no se dibujan 
como un widget normal; el propio motor Flutter pinta la capa sobrepuesta de rendimiento 
y sólo tiene un impacto mínimo en el rendimiento.
Cada gráfico representa los últimos 300 frames para ese hilo.

En esta sección se describe cómo habilitar la función
[PerformanceOverlay,][]
y usarla para diagnosticar la causa del jank en tu aplicación.
La siguiente captura de pantalla muestra la capa sobrepuesta de rendimiento que se está 
ejecutando en el ejemplo de Flutter Gallery:

<center><img src="images/performance-overlay-green.png" alt="screenshot of performance overlay showing zero jank"></center>
<center>La capa sobrepuesta de rendimiento muestra el hilo de la UI (arriba) 
y el hilo de la GPU (abajo). Las barras verdes verticales representan el frame actual.</center><br>

Flutter utiliza varios hilos para hacer su trabajo. Todo tu código de Dart se ejecuta en el hilo 
de la UI. Aunque no tienes acceso directo a ningún otro hilo, tus acciones en el hilo de la UI 
tienen consecuencias de rendimiento en otros hilos.

1. Platform thread<br>
   El hilo principal de la plataforma. El código del Plugin se ejecuta aquí.
   Para más información, consulta 
   la documentación para iOS
   [UIKit][]
   , o la documentación para Android
   [MainThread][].
   Este hilo no se muestra en la capa sobrepuesta de rendimiento.

1. UI thread<br>
   El hilo UI ejecuta el código Dart en la VM de Dart.
   Este hilo incluye el código que escribiste, y el código ejecutado por el framework de 
   Flutter en beneficio de tu aplicación.
   Cuando la aplicación crea y muestra una escena, el subproceso de la interfaz de usuario 
   crea un _árbol de capas_, un objeto ligero que contiene comandos de trazado agnósticos del 
   dispositivo, y envía el árbol de capas al hilo de la GPU para que se renderice en el 
   dispositivo. _No bloquees este hilo!_ Se muestra en la fila inferior de la capa 
   sobrepuesta de rendimiento.

1. GPU thread<br>
   El hilo de la GPU toma el árbol de capas y lo muestra hablando a la GPU 
   (unidad de    procesamiento gráfico). No puedes acceder directamente al 
   hilo de la GPU o a sus datos, pero, si este hilo es lento, es el resultado 
   de algo que has hecho en el código de Dart.
   Skia, la biblioteca de gráficos se ejecuta en este hilo, que a veces se 
   llama el hilo _rasterizador_.
   Se muestra en la fila superior de la capa sobrepuesta de rendimiento.

1. I/O thread<br>
   Realiza tareas costosas (principalmente I/O) que de otro modo bloquearían
   ya sea la UI o la GPU.
   Este hilo no se muestra en la capa sobrepuesta de rendimiento.


Para más información sobre esos hilos, consulta
[Notas de la Arquitectua.][]

Cada frame debe ser creado y mostrado en 1/60 de segundo. (aproximadamente 16ms). 
Un frame que excede este límite (en cualquier gráfico)
no se muestra, resultando en jank, y una barra roja vertical aparece 
en una o ambas gráficas.
Si aparece una barra roja en el gráfico de UI, el código de Dart sale demasiado costoso.
Si aparece una barra vertical roja en el gráfico de la GPU, la escena se hace demasiado 
complicada como para renderizar rápidamente.

<center><img src="images/performance-overlay-jank.png" alt="Screenshot of performance overlay showing jank with red bars."></center>
<center>Las barras rojas verticales indican que el frame actual es muy costoso tanto para el renderizado como para el pintado.<br>Cuando ambas gráficas estén rojas, comienza por diagnosticar el hilo de la UI (Dart VM).</center><br>

### Visualización de la capa sobrepuesta de rendimiento

Puede alternar la visualización de la capa sobrepuesta de rendimiento como se indica a continuación:

* Usando el Flutter Inspector
* Desde la línea de comando
* Programáticamente

#### Usando el Flutter Inspector

La manera más fácil de habilitar el widget PerformanceOverlay es habilitándolo en el 
Flutter inspector, que está disponible a través del plugin Flutter para su IDE. 
La vista Inspector se abre de forma predeterminada cuando se ejecuta una aplicación. 
Si el inspector no está abierto, puedes mostrarlo de la siguiente manera.

En Android Studio e IntelliJ IDEA:

1. Selecciona **View > Tool Windows > Flutter Inspector**.
1. En la barra de herramientas, selecciona el icono que parece una biblioteca 
   (![bookshelf-like icon][]).
  
   ![IntelliJ Flutter inspector Window][]

El Flutter Inspector está disponible en Android Studio e IntelliJ.
Obtenga más información sobre lo que el Inspector puede hacer en el documento
[Widget inspector][], así como el archivo
[Flutter Inspector talk][]
presentado en el DartConf 2018.

#### En VS Code

1. Seleciona **View > Command Palette…** para mostrar la paleta de comandos.
1. En el campo de texto, escribes "performance" y selecciona
   **Toggle Performance Overlay** de la lista que aparece.
   Si este comando no está disponible, asegúrate de que la aplicación esté ejecutándose.

#### Desde la línea de Comando

Conmute la capa sobrepuesta de rendimiento con la tecla **P** desde la línea de comando.

#### Programáticamente

Puede habilitar programáticamente el widget PerformanceOverlay 
estableciendo la propiedad `showPerformanceOverlay` en `true` en el 
constructor de MaterialApp o WidgetsApp:

<!-- skip -->
{% prettify dart %}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      [[highlight]]showPerformanceOverlay: true,[[/highlight]]
      title: 'My Awesome App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'My Awesome App'),
    );
  }
}
{% endprettify %}

Probablemente ya conozcas la aplicación de ejemplo de Flutter Gallery.
Para utilizar la capa sobrepuesta de rendimiento con Flutter Gallery, usa la copia en el directorio de 
[ejemplos][]
que fue instalado con Flutter, 
y ejecuta la aplicación en modo profile. El programa se escribe de manera 
que el menú de la app le permita cambiar dinámicamente la ventana superpuesta,
así como habilitar la comprobación de llamadas a `saveLayer` y 
la presencia de imágenes en caché.

{{site.alert.note}}
  No puede habilitar la capa sobrepuesta de rendimiento 
  en la app Flutter Gallery descargada desde el App Store. Esa versión 
  de la aplicación está compilada en modo release (no en modo profile), 
  y no proporciona un menú para habilitar o deshabilitar la ventana superpuesta.
{{site.alert.end}}

### Identificando problemas en la UI de gráficas

Si la capa sobrepuesta de rendimiento aparece en rojo en el gráfico de interfaz de usuario, 
comienza por perfilar la VM de Dart, incluso si el gráfico de la GPU también aparece en rojo.

#### Perfil de rendimiento con Dart DevTools

Dart DevTools proporciona características como la creación de perfiles, el examen del heap, 
y la visualización de la cobertura de código. La vista _timeline_ de DevTools le 
permite capturar una instantánea del stack en un momento dado.
Al abrir el timeline del DevTools desde el Flutter Inspector, usarás una versión 
que ha sido personalizada para las aplicaciones Flutter.

{{site.alert.note}}
  Observatory ha sido reemplazado por Dart DevTools.
  Esta herramienta basada en web esta aún en desarrollo pero disponible para previsualziar. Para
  instalación e instrucciones de como empezar , mira [DevTools' docs][].
{{site.alert.end}}

 ### Identificación de problemas en el gráfico de la GPU
A veces, una escena da como resultado un árbol de capas que es 
fácil de construir, pero cuyo renderizado en el hilo de la GPU es costoso. 
Cuando esto sucede, el gráfico de UI no tiene color rojo, pero el gráfico de 
la GPU muestra color rojo.
En este caso, tendrás que averiguar qué está haciendo tu código para que el código de 
renderizado sea lento. Los tipos específicos de cargas de trabajo son más difíciles para 
la GPU. Pueden implicar llamadas innecesarias a
[`saveLayer`][],
opacidades entrecruzadas con múltiples objetos, y acoplados o sombras en 
situaciones específicas.

Si sospechas que la fuente de la lentitud es durante una animación, usa la propiedad del botón 
[timeDilation][]
para ralentizar enormemente la animación.

También puedes reducir la velocidad de la animación utilizando el Flutter Inspector.
En el menú de engranajes del inspector, selecciona **Enable Slow Animations**.
Si deseas un mayor control de la velocidad de la animación, establece la propiedad 
[timeDilation][]
en tu código.

¿La lentitud está en el primer frame o en toda la animación?
Si se trata de toda la animación, ¿el acoplado está causando la ralentización?
Tal vez haya una forma alternativa de dibujar la escena que no utilice el acoplado. 
Por ejemplo, superponga esquinas opacas a un cuadrado en lugar de hacer recortes 
en un rectángulo redondeado.
Si se trata de una escena estática que se está desvaneciendo, girando o manipulando de otra manera, 
tal vez un
[RepaintBoundary][]
puede ayudar.

#### Comprobación de capas fuera de pantalla

El método 
[`saveLayer`][]
es uno de los métodos más costosos en el framework Flutter. Es útil cuando se aplica el post 
procesamiento a la escena, pero puede ralentizar tu aplicación y debería evitarse si no la necesitas. 
Incluso si no llama a `saveLayer` explícitamente, pueden producirse llamadas implícitas 
en su nombre. Puedes comprobar si tu escena 
está usando `saveLayer` 
con el switch 
 [PerformanceOverlayLayer.checkerboardOffscreenLayers](https://docs.flutter.io/flutter/rendering/PerformanceOverlayLayer/checkerboardOffscreenLayers.html).

{% comment %}
[TODO: Documento deshabilitando las gráficas y checkerboardRasterCachedImages.
Flutter Inspector no parece soportar esto?]
{% endcomment %}

Una vez que el switch esté habilitado, ejecuta la aplicación y busca cualquier 
imagen que este delineada con un cuadro parpadeante. La caja parpadea de frame 
a frame si un nuevo frame se está renderizando. Por ejemplo, tal vez tenga un 
grupo de objetos con opacidades que se renderizan utilizando `saveLayer`. En este 
caso, probablemente sea más eficaz aplicar una opacidad a cada widget individual, 
en lugar de un widget padre más arriba en el árbol de widgets. Lo mismo ocurre 
con otras operaciones potencialmente costosas, como acoplamiento o sombras.

<aside class="alert alert-info" markdown="1">
**Nota:** Opacidad, acoplamiento, y sombras no son, por sí mismas, una mala idea.
Sin embargo, aplicarlos a la parte superior del árbol de widgets puede causar 
llamadas adicionales a `saveLayer`, y un procesamiento innecesario.
</aside>

Cuando encuentres llamadas a `saveLayer`, hazte estas preguntas:

* ¿Necesita la aplicación este efecto?
* ¿Se puede eliminar alguna de estas llamadas?
* ¿Puedo aplicar el mismo efecto a un elemento individual en lugar de a un grupo?

#### Comprobación de imágenes no almacenadas en caché

Almacenamiento en caché de una imagen con
[RepaintBoundary][]
es bueno, _cuando tiene sentido_.

Una de las operaciones más costosas, desde la perspectiva de los recursos, es renderizar 
una textura usando un archivo de imagen. Primero, la imagen comprimida se 
obtiene del almacenamiento persistente.
La imagen se descomprime en la memoria del host (memoria de la GPU) y se transfiere.
a la memoria del dispositivo (RAM).

En otras palabras, la I/O de imagenes puede ser costosa.
La caché proporciona instantáneas de jerarquías complejas para que sea más fácil
en los frames siguientes. _Debido a que las entradas de caché de imágenes tipo raster 
o `"bitmap image"` son costosas de construir y ocupan mucha memoria de la GPU, 4
almacena en caché imágenes sólo cuando sea absolutamente necesario._

Puedes ver qué imágenes están siendo almacenadas en caché activando 
el switch
[PerformanceOverlayLayer.checkerboardRasterCachedImages][].

{% comment %}
[TODO: Document how to do this, either via UI or programmatically.
At this point, disable the graphs and checkerboardOffScreenLayers.]
{% endcomment %}

Ejecuta la app y busca imágenes renderizadas con un tablero de colores aleatorios, indicando 
que la imagen está almacenada en caché. A medida que interactúas con la escena, las imágenes 
en el tablero deben permanecer constantes; no quieres ver parpadeos, lo que indicaría que la 
imagen en caché se está volviendo a almacenar.

En la mayoría de los casos, deseas ver los tableros en imágenes estáticas, pero no en imágenes no 
estáticas. Si una imagen estática no está almacenada en caché, puedes guardarla en caché 
colocándola en un widget
[RepaintBoundary][]. 
Aunque el motor puede ignorar un límite de repintado si piensa que la imagen no es 
lo suficientemente compleja.

### Visualizar el widget rebuild profiler

El framework Flutter está diseñado para hacer para que sea difícil que crees 
aplicacioens que no tengan 60fps y sean suaves. Sin embargo, si sufres 'jank',
esto es porque un simple bug causa que la UI sea reconstruida más de lo necesario en cada frame. El Widget rebuild profiler
ayuda a depurar y arreglar problemas de rendimiento debidos 
a este tipo de bugs.

Puedes ver el recuento de rebuilts de la pantalla y frame actual  en el widget inspector. Para detalles de como hacer esto, mira
[Show performance data][] en la página [Android Studio / IntelliJ][].

## Indicadores de depuración

Flutter proporciona una amplia variedad de indicadores y funciones de depuración para ayudarle a 
depurar su aplicación en varios puntos a lo largo del ciclo de desarrollo.
Para usar estas características, debe compilar en modo debug.
La siguiente lista, aunque no completa,
resalta algunas de los indicadores más útiles  (y una función)
desde la 
[biblioteca de renderizado][]
para depurar problemas de rendimiento.

* [`debugDumpRenderTree()`][]<br>
  Llama a esta función, cuando no esté en una fase de diseño o repintado, para volcar el árbol de 
  renderizado a la consola. (Pulsa **t** desde `flutter run`
  para llamar a este comando.) Busca por "RepaintBoundary" para ver diagnósticos
  sobre lo útil que es un límite.
* [`debugPaintLayerBordersEnabled`][]
* [`debugRepaintRainbowEnabled`][]<br>
  Habilita esta propiedad y ejecuta tu aplicación para ver si hay partes de tu UI q
  ue no estén cambiando (por ejemplo, un encabezado estático) que estén rotando 
  a través de muchos colores en la salida.
  Esas áreas son candidatas para agregar límites de repintado
* [`debugPrintMarkNeedsLayoutStack`][]<br>
  Habilita esta propiedad si estás viendo más diseños de los que esperas (por ejemplo, 
  en el timeline, en un profile o desde una sentencia "print" dentro de un método de layout). 
  Una vez activada, la consola se inunda de trazos de pila que muestran por qué 
  cada objeto renderizado está siendo marcado como corrupto para el 
  layout.
* [`debugPrintMarkNeedsPaintStacks`][]<br>
  Similar a `debugPrintMarkNeedsLayoutStack`, pero por exceso de pintado.

Puedes aprender sobre otras etiquetas de depuración en [Depurando apps Flutter][].


## Benchmarking

Puedes medir y hacer un seguimiento del rendimiento de tu aplicación escribiendo pruebas de referencia.
La biblioteca Flutter Driver proporciona soporte para el benchmarking. Usando este framework de 
pruebas de integración, es posible generar métricas para realizar el seguimiento de lo siguiente:

* Jank
* Tamaño de descarga
* Eficiencia de la batería
* Tiempo de inicio

El seguimiento de estos puntos de referencia te permite estar informado cuando se introduce 
una regresión que afecta negativamente al rendimiento.

Para más información, mira [Test de integración][],
una sección en [Probando apps Flutter][].

## Más información

Los siguientes recursos proporcionan más información sobre el uso de las herramientas de 
Flutter y depurando en Flutter:

* [Depura tu app][]
* [Inspecciona tu UI][]
* [Habla con Flutter Inspector][],
  presentado en el DartConf 2018
* [¿Porqué Flutter usa Dart?][],
  un artículo sobre Hackernoon.
* [Observatory: Un Profiler para Apss de Dart]([]
* Documentos [Flutter API][], 
  particularmente la clase
  [PerformanceOverlay][], 
  y el paquete
  [dart:developer][].

[IntelliJ Flutter inspector window]: {% asset tools/android-studio/visual-debugging.png @path %}

[RepaintBoundary]: {{site.api}}/flutter/widgets/RepaintBoundary-class.html
[Seguimiento del rendimiento de 
    cualquier código Dart]: /docs/testing/debugging#tracing-any-dart-code-performance
[Depurando apps Flutter]: /docs/testing/debugging
[examples]: {{site.github}}/flutter/flutter/tree/master/examples/flutter_gallery
[Modos en Flutter]: /docs/testing/build-modes
[profile mode]: /docs/testing/build-modes#profile
[PerformanceOverlay]: {{site.api}}/flutter/widgets/PerformanceOverlay-class.html
[dart:developer]: {{site.api}}/flutter/dart-developer/dart-developer-library.html
[PerformanceOverlayLayer.checkerboardOffscreenLayers]: {{site.api}}/flutter/rendering/PerformanceOverlayLayer/checkerboardOffscreenLayers.html
[PerformanceOverlayLayer.checkerboardRasterCacheImages]: {{site.api}}/flutter/rendering/PerformanceOverlayLayer/checkerboardRasterCacheImages.html
[Flutter API]: {{site.api}}
[UIKit]: https://developer.apple.com/documentation/uikit
[MainThread]: {{site.android-dev}}/reference/android/support/annotation/MainThread
[Test de integración]: /docs/testing#integration-tests
[Notas de la Arquitectua.]: {{site.github}}/flutter/engine/wiki#architecture-notes
[Widget inspector]: /docs/development/tools/inspector
[Flutter Inspector talk]: https://www.youtube.com/watch?v=JIcmJNT9DNI
[bookshelf-like icon]: /docs/testing/ui-performance/images/performance-overlay-icon.png
[line-chart icon]: /docs/testing/ui-performance/images/observatory-timeline-icon.png
[stopwatch icon]: /docs/testing/ui-performance/images/observatory-icon.png
[issues or feature requests]: {{site.github}}/dart-lang/sdk/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-observatory
[`saveLayer`]: {{site.api}}/flutter/dart-ui/Canvas/saveLayer.html
[timeDilation]: {{site.api}}/flutter/scheduler/timeDilation.html
[Show performance data]: /docs/development/tools/android-studio#show-performance-data
[Android Studio / IntelliJ]: /docs/development/tools/android-studio
[rendering library]: {{site.api}}/flutter/rendering/rendering-library.html
[`debugDumpRenderTree()`]: {{site.api}}/flutter/rendering/debugDumpRenderTree.html
[`debugPaintLayerBordersEnabled`]: {{site.api}}/flutter/rendering/debugPaintLayerBordersEnabled.html
[`debugRepaintRainbowEnabled`]: {{site.api}}/flutter/rendering/debugRepaintRainbowEnabled.html
[`debugPrintMarkNeedsLayoutStack`]: {{site.api}}/flutter/rendering/debugPrintMarkNeedsLayoutStacks.html
[`debugPrintMarkNeedsPaintStacks`]: {{site.api}}/flutter/rendering/debugPrintMarkNeedsPaintStacks.html
[Probando apps Flutter]: /docs/testing
[¿Porqué Flutter usa Dart?]: https://hackernoon.com/why-flutter-uses-dart-dd635a054ebf
[Dart DevTools]: https://flutter.github.io/devtools
[DevTools' docs]: https://flutter.github.io/devtools