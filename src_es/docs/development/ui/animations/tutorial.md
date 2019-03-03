---
title: "Tutorial de animaciones"
short-title: Tutorial
descripcion: Un tutorial mostrando como construir animaciones explícitas en Flutter
diff2html: true
---

{% assign api = '{{site.api}}/flutter' -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}

<?code-excerpt path-base="animation"?>

{{site.alert.secondary}}
  <h4 class="no_toc">Lo que aprenderás:</h4>

  * Como usar clases fundamentales de la biblioteca de animaciones para añadir 
    animaciones a un widget.
  * Cuando usar `AnimatedWidget` vs. `AnimatedBuilder`.
{{site.alert.end}}

Este tutorial te muestra como construir animaciones en Flutter.
Después de introducir algunos conceptos esenciales, clases y métodos, 
de la biblioteca de animaciones, te conduce a través de 5 ejemplos de animación. Los ejemplos se basan unos en otros,
introduciéndote en diferentes aspectos de la biblioteca de animaciones.

El SDK de Flutter también proporciona animaciones de transición, como son
[FadeTransition][], [SizeTransition][], y [SlideTransition][].
Estas animaciones simples son ejecutadas definiendo un punto de inicio y de fin. Son más simples que las animaciones explícitas, 
que describimos aquí.

<a name="concepts"></a>
## Conceptos y clases esenciales de animaciones

{{site.alert.secondary}}
  <h4 class="no_toc">¿Qué aprenderás?</h4>

* El objeto [Animation][], una clase principal en la biblioteca de animaciones de Flutter,
  interpola los valores usados para guiar una animación.
* El objeto `Animation` conoce el estado actual de una animación (por ejemplo,
  ya sea que, ha comenzado, parado, o se mueve adelante o hacia atrás),
  pero no sabe nada de lo que aparece en la pantalla.
* Un [AnimationController][] administra el Animation.
* Un [CurvedAnimation][] define progresiones como una curva no lineal.
* Un [Tween][] interpola entre el rango de datos usados por el objeto que 
  está siendo animado. Por ejemplo, un Tween podría definir una interpolación 
  de rojo a azul, o de 0 a 255.
* Usa Listeners y StatusListeners para monitorizar los cambios de estado de la animación.
{{site.alert.end}}

El sistema de animaciones en Flutter está basado en 
objetos [`Animation`][] tipados. Los widgets pueden incorporar 
estos objetos animation en sus funciones build directamente al 
leer su valor actual y escuchar sus cambios de estado, o 
pueden usarlos como la base de animaciones más elaboradas que pasan a través de otros widgets.

<a name="animation-class"></a>
### Animation<wbr>\<double>

En Flutter, un objeto Animation no sabe nada sobre que hay en la pantalla.
Un objeto `Animation` es una clase abstracta que entiende su valor actual y 
su estado (completado o rechazado). Uno de los tipos de animation más comúnmente 
usados es `Animation<double>`.

Un objeto `Animation` en Flutter es una clase que genera secuencialmente números 
interpolándolos entre dos valores durante una cierta duración.
La salida de un objeto `Animation` puede ser lineal, una curva, una función por pasos,
o cualquier otro mapeado que puedas idear. Dependiendo de como el objeto `Animation`
se controle, podría ejecutarse en modo inverso, o incluso cambiar la dirección en 
el medio.

Los objetos Animation pueden también interpolar otros tipos diferentes a double, como 
`Animation<Color>` o `Animation<Size>`.

El objeto `Animation` tiene estado. El valor actual siempre esta disponible 
en la propiedad `.value`.

Un objeto Animation no conoce nada sobre renderizado o funciones `build()`.

### CurvedAnimation

Un objeto [CurvedAnimation][] define el progreso de una animación como una curva no lineal.

<?code-excerpt "animate5/lib/main.dart (CurvedAnimation)"?>
```dart
animation = CurvedAnimation(parent: controller, curve: Curves.easeIn);
```

{{site.alert.note}}
  La clase [Curves][] define la mayoría de curvas usadas comúnmente, o puedes crear la tuya propia. 
  Por ejemplo:

  <?code-excerpt "animate5/lib/main.dart (ShakeCurve)" plaster="none"?>
  {% prettify dart context="html" %}
  import 'dart:math';

  class ShakeCurve extends Curve {
    @override
    double transform(double t) => sin(t * pi * 2);
  }
  {% endprettify %}
{{site.alert.end}}

`CurvedAnimation` y `AnimationController` (descrito en la siguiente sección), 
son ambas de tipo `Animation<double>`, puedes pasarlas de forma intercambiable.
El objeto CurvedAnimation envuelve el objeto que está modificando&mdash;no puedes 
hacer una subclase de `AnimationController` para implementar una curva.

### Animation&shy;Controller

[AnimationController][] es un objeto Animation especial que genera un nuevo valor 
cada vez que el hardware esta preparado para un nuevo frame. Por defecto,
un `AnimationController` produce linealmente números desde 0.0 a 1.0
durante una duración dada. Por ejemplo, este código crea un objeto Animation, 
pero no comienza su ejecución:

<?code-excerpt "animate5/lib/main.dart (AnimationController)"?>
```dart
controller =
    AnimationController(duration: const Duration(seconds: 2), vsync: this);
```

`AnimationController` deriva de `Animation<double>`, por esto puede ser 
usado donde quiera que se necesite un objeto `Animation`. Sin embargo, 
`AnimationController` tiene métodos adicionales para controlar la animación. 
Por ejemplo, inicias una animación con el método `.forward()`. La generación 
de números está vinculada al refresco de la pantalla, normalmente son 
generados 60 numeros por segundo. Después de que cada número es generado, 
cada objeto Animation llama a sus objetos Listener asociados. Para crear 
una lista personalizada para cada hijo, mira
[RepaintBoundary][].

Cuando creas un `AnimationController`, le pasas un argumento `vsync`. 
La presencia de `vsync` previene animaciones fuera de pantalla que consuman 
recursos innecesarios. Puedes usar tu objeto stateful como vsync 
añadiendo SingleTickerProviderStateMixin a la definición de la clase.
Puedes ver un ejemplo de esto en 
[animate1]({{examples}}/animation/animate1/lib/main.dart)
en GitHub.
{% comment %}
The `vsync` object ties the ticking of the animation controller to
the visiblity of the widget, so that when the animating widget goes
off-screen, the ticking stops, and when the widget is restored, it
starts again (without stopping the clock, so it's as if it had
been ticking the whole time, but without using the CPU.)
To use your custom State object as the `vsync`, include the
`TickerProviderStateMixin` when defining the custom State class.
{% endcomment -%}

{{site.alert.note}}
En algunos casos, una posición podria sobrepasar el rango 0.0-1.0 del 
`AnimationController`. Por ejemplo, la función `fling()` te permite proporcionar 
velocidad, fuerza y posicion (vía el objeto Force).
La posición puede ser cualquiera y puede estar fuera del rango entre 0.0 y 1.0.

Un `CurvedAnimation` puede también sobrepasar el rango de 0.0 a 1.0,
en cambio `AnimationController` no puede. Dependiendo de la curva seleccionada,
la salida de `CurvedAnimation` puede ser un rango más amplio que la entrada.
Por ejemplo, las curvas elásticas como Curves.elasticIn sobrepasaran 
significativamente el rango por defecto tanto por abajo como por arriba.
{{site.alert.end}}

### Tween

Por defecto, el objeto `AnimationController` tiene rangos entre 0.0 y 1.0.
Si necesitas un rango diferente o un tipo de datos diferente, 
puedes usar `Tween` para configurar un objeto animation que interpole 
un rango o tipo de dato diferente. Por ejemplo, el siguiente Tween
va desde -200.0 a 0.0:

<?code-excerpt "animate5/lib/main.dart (tween)"?>
```dart
tween = Tween<double>(begin: -200, end: 0);
```

Un `Tween` es un objeto stateless que solo toma las propiedades `begin` y `end`.
El único trabajo de un `Tween` es definir un mapeado entre un rango de entrada 
y un rango de salida. El rango de entrada en normalment 0.0 a 1.0,
pero esto no es un requisito.

Un Tween hereda de `Animatable<T>`, no de `Animation<T>`.
Un Animatable, como un Animation, no tiene porque tener una salida de tipo double.
Por ejemplo, `ColorTween` especifica una progresión entre dos colores.

<?code-excerpt "animate5/lib/main.dart (colorTween)"?>
```dart
colorTween = ColorTween(begin: Colors.transparent, end: Colors.black54);
```

Un objeto `Tween` no almacena ningun estado. En cambio, provee el método 
`evaluate(Animation<double> animation)` que aplica la función de mapeado 
al valor actual del objeto Animation. El valor actual del 
objeto `Animation` puede ser encontrado en el método `.value`.
La función evaluate function también realiza algunas labores de limpieza, 
como asegurar que se devuelva begin y end cuando los valores del objeto 
animation sean 0.0 y 1.0, respectivamente.

#### Tween.animate

Para usar el objeto `Tween`, llama a `animate()` en Tween, pasado en el 
objeto controller. Por ejemplo, el siguiente código genera los valores 
enteros entre 0 y 255 en el trascurso de 500 ms.

<?code-excerpt "animate5/lib/main.dart (IntTween)"?>
```dart
AnimationController controller = AnimationController(
    duration: const Duration(milliseconds: 500), vsync: this);
Animation<int> alpha = IntTween(begin: 0, end: 255).animate(controller);
```
{{site.alert.note}}
El método `animate()` devuelve un Animation, no un Animatable.
{{site.alert.end}}

El siguiente ejemplo muestra un controller, un curve, y un `Tween`:

<?code-excerpt "animate5/lib/main.dart (IntTween-curve)"?>
```dart
AnimationController controller = AnimationController(
    duration: const Duration(milliseconds: 500), vsync: this);
final Animation curve =
    CurvedAnimation(parent: controller, curve: Curves.easeOut);
Animation<int> alpha = IntTween(begin: 0, end: 255).animate(curve);
```

### Notificaciones de Animation

Un objeto [Animation][] puede tener Listeners y StatusListeners,
definidos con `addListener()` y `addStatusListener()`.
Un `Listener` es llamado cada vez que el valor del objeto animation cambia.
El comportamiento mas habitual de un `Listener` es llamar a `setState()` 
para provocar un rebuild. Un `StatusListener` es llamado cuando una animación empieza, 
finaliza, se mueve hacia delante, o se mueve hacia atrás, como es definido por `AnimationStatus`.
La nueva sección tiene un ejemplo del método `addListener()`, 
y [Monitoriza el progreso de la animación](#monitoring) monstrando un ejemplo de 
`addStatusListener()`.

---

## Ejemplo de animaciones

Esta sección te conduce a través de 5 ejemplos de animaciones.
Cada sección proporciona un enlace al código fuente del ejemplo.

### Rendering animations

{{site.alert.secondary}}
  <h4 class="no_toc">¿Qué aprenderás?</h4>

* Como añadir animación básica a un widget usando `addListener()` y
  `setState()`.
* Cada vez que el objeto Animation genera un nuevo número, la función `addListener()`
  llama a `setState()`.
* Como definir un `AnimatedController` con el parámetro `vsync` requerido.
* Entendindo la sintaxis "`..`" en "`..addListener`", también conocida como 
  _cascade notation_ en Dart.
* Para hacer una clase privada, inicia su nombre con un guión bajo (`_`).
{{site.alert.end}}

Hasta ahora has aprendido como generar una secuencia de números en el trascurso del un tiempo.
Nada se ha renderizado en la pantalla. Para renderizar con un objeto 
`Animation`;, guarda el objeto Animation como un miembro de tu Widget, entonces 
usa su valor para decidir que dibujar.

Considera la siguiente aplicación que dibuja el logo de Flutter 
sin animación:

<?code-excerpt "animate0/lib/main.dart"?>
```dart
import 'package:flutter/material.dart';

void main() => runApp(LogoApp());

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        margin: EdgeInsets.symmetric(vertical: 10),
        height: 300,
        width: 300,
        child: FlutterLogo(),
      ),
    );
  }
}
```

Lo siguiente muestra el mismo código modificado para animar el logo 
para crecer de nada al tamaño completo. Cuando defines 
un `AnimationController`, debes pasarlo en un objeto `vsync`.
El parámetro `vsync` es descrito en la sección 
[AnimationController](#animationcontroller).

Los cambios desde el ejemplo no animado están resaltados:

<?code-excerpt "animate{0,1}/lib/main.dart"?>
```diff
--- animate0/lib/main.dart
+++ animate1/lib/main.dart
@@ -1,3 +1,4 @@
+import 'package:flutter/animation.dart';
 import 'package:flutter/material.dart';

 void main() => runApp(LogoApp());
@@ -6,16 +7,39 @@
   _LogoAppState createState() => _LogoAppState();
 }

-class _LogoAppState extends State<LogoApp> {
+class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
+  Animation<double> animation;
+  AnimationController controller;
+
+  @override
+  void initState() {
+    super.initState();
+    controller =
+        AnimationController(duration: const Duration(seconds: 2), vsync: this);
+    animation = Tween<double>(begin: 0, end: 300).animate(controller)
+      ..addListener(() {
+        setState(() {
+          // The state that has changed here is the animation object’s value.
+        });
+      });
+    controller.forward();
+  }
+
   @override
   Widget build(BuildContext context) {
     return Center(
       child: Container(
         margin: EdgeInsets.symmetric(vertical: 10),
-        height: 300,
-        width: 300,
+        height: animation.value,
+        width: animation.value,
         child: FlutterLogo(),
       ),
     );
   }
+
+  @override
+  void dispose() {
+    controller.dispose();
+    super.dispose();
+  }
 }
```

La función `addListener()` llama a `setState()`, cada vez que el objeto 
`Animation` genera un nuevo número, el frame actual es marcado como dirty,
lo caul fuerza al método `build()` a ser llamado de nuevo.
En la función `build()`, el container cambia su tamaño porque su altura y anchura
ahora usan `animation.value` en lugar de un valor fijo.
Deseche con el método dispose el controlador cuando la animación haya terminado para prevenir 
_memory leaks_.

Con estos pocos cambioss, habrás creado, ¡tu primera animación en Flutter!
Puedes encontrar el código fuente para este ejemplo en,
[animate1.](https://raw.githubusercontent.com/flutter/website/master/src/_includes/code/animation/animate1/main.dart)

<aside class="alert alert-success" markdown="1">
**Trucos del lenguaje Dart:**
PUede que no estes familiarizado cocn la notacion en cascada de Dart&mdash;los dos 
puntos en `..addListener()`. Esta sintaxis significa que el método `addListener()`
es llamado con el valor devuelto desde `animate()`.
Considera el siguiente ejemplo:

<?code-excerpt "animate1/lib/main.dart (addListener)" replace="/animation.*|\.\.addListener/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!animation = Tween<double>(begin: 0, end: 300).animate(controller)!]
    [!..addListener!](() {
      // ···
    });
  {% endprettify %}

Este código es equivalente a:

<?code-excerpt "animate1/lib/main.dart (addListener)" replace="/animation.*/$&;/g; /  \./animation/g; /animation.*/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!animation = Tween<double>(begin: 0, end: 300).animate(controller);!]
  [!animation.addListener(() {!]
      // ···
    });
  {% endprettify %}

Puedes aprender más sobre la notación en cascada en el 
[Dart Language Tour.]({{site.dart-site}}/guides/language/language-tour)
</aside>

###  Simplificando con AnimatedWidget

{{site.alert.secondary}}
  <h4 class="no_toc">¿Qué aprenderás?</h4>

* Como usar la clase helper `AnimatedWidget` (en lugar de `addListener()`
  y `setState()`) para crear un widget que se anime.
* Usa `AnimatedWidget` para crear un widget que realiza una animación reutilizable.
  Para separar la transición desde el widget, usa un
  [AnimatedBuilder.](#refactoring-with-animatedbuilder)
* Ejemplos de AnimatedWidgets en la API de Flutter: AnimatedBuilder,
  AnimatedModalBarrier, DecoratedBoxTransition, FadeTransition,
  PositionedTransition, RelativePositionedTransition, RotationTransition,
  ScaleTransition, SizeTransition, SlideTransition.
{{site.alert.end}}

La clase `AnimatedWidget` te permte separar el código del widger 
del código de la animación en la llamada a `setState()`. `AnimatedWidget` 
no neceita mantener un objeto State para sostener la animación.

<?code-excerpt path-base="animation/animate2"?>
<?code-excerpt "lib/main.dart (AnimatedLogo)" title?>
```dart
class AnimatedLogo extends AnimatedWidget {
  AnimatedLogo({Key key, Animation<double> animation})
      : super(key: key, listenable: animation);

  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return Center(
      child: Container(
        margin: EdgeInsets.symmetric(vertical: 10),
        height: animation.value,
        width: animation.value,
        child: FlutterLogo(),
      ),
    );
  }
}
```
<?code-excerpt path-base="animation"?>

`LogoApp` pasa el objeto `Animation` a la clase base y usa 
`animation.value` para fijar el alto y el ancho del container, funcionando entonces 
exactamente igual que antes.

<?code-excerpt "animate{1,2}/lib/main.dart" from="class _LogoAppState" diff-u="6"?>
```diff
--- animate1/lib/main.dart
+++ animate2/lib/main.dart
@@ -10,2 +27,2 @@
 class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
   Animation<double> animation;
@@ -13,32 +30,18 @@

   @override
   void initState() {
     super.initState();
     controller =
         AnimationController(duration: const Duration(seconds: 2), vsync: this);
-    animation = Tween<double>(begin: 0, end: 300).animate(controller)
-      ..addListener(() {
-        setState(() {
-          // The state that has changed here is the animation object’s value.
-        });
-      });
+    animation = Tween<double>(begin: 0, end: 300).animate(controller);
     controller.forward();
   }

   @override
-  Widget build(BuildContext context) {
-    return Center(
-      child: Container(
-        margin: EdgeInsets.symmetric(vertical: 10),
-        height: animation.value,
-        width: animation.value,
-        child: FlutterLogo(),
-      ),
-    );
-  }
+  Widget build(BuildContext context) => AnimatedLogo(animation: animation);

   @override
   void dispose() {
     controller.dispose();
     super.dispose();
   }
```

**App source:** [animate2]({{examples}}/animation/animate2)


<a name="monitoring"></a>
### Monitorzando el progreso de la animación

{{site.alert.secondary}}
  <h4 class="no_toc">¿Qué aprenderás?</h4>

* Usa `addStatusListener()` para notificaciones de cambios del estado de la animacion, 
  como empezando, parando, o invirtiendo la dirección.
* Ejecuta una animación en un loop infinito, invirtiendo la dirección cuando la animación 
  ha sido completada o regresado a su estado de inicio.
{{site.alert.end}}

A menudo es útil saber cuando una animación cambia su estado,
como cuando finaliza, avanza hacia delante, o hacia atrás.
Puedes obtener notificaciones de esto con `addStatusListener()`.
El siguiente códgo modifica el ejemplo previo
para que escuche los cambios de estado e imprima una actualización.
Las líneas resaltadas muestran los cambios:

<?code-excerpt "animate3/lib/main.dart (print state)" plaster="none" replace="/\/\/ (\.\..*)/$1;/g; /\.\..*/[!$&!]/g; /\n  }/$&\n  \/\/ .../g"?>
```dart
class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  Animation<double> animation;
  AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller =
        AnimationController(duration: const Duration(seconds: 2), vsync: this);
    animation = Tween<double>(begin: 0, end: 300).animate(controller)
      [!..addStatusListener((state) => print('$state'));!]
    controller.forward();
  }
  // ...
}
```

Ejecutar este código produce líneas como las siguientes:

```console
AnimationStatus.forward
AnimationStatus.completed
```

A continuación, usa `addStatusListener()` para invertir la animación en el principio 
o en el final. Esto crea un efecto "respiración":

<?code-excerpt "animate{2,3}/lib/main.dart" to="/^   }/" diff-u="4"?>
```diff
--- animate2/lib/main.dart
+++ animate3/lib/main.dart
@@ -32,7 +32,15 @@
   void initState() {
     super.initState();
     controller =
         AnimationController(duration: const Duration(seconds: 2), vsync: this);
-    animation = Tween<double>(begin: 0, end: 300).animate(controller);
+    animation = Tween<double>(begin: 0, end: 300).animate(controller)
+      ..addStatusListener((status) {
+        if (status == AnimationStatus.completed) {
+          controller.reverse();
+        } else if (status == AnimationStatus.dismissed) {
+          controller.forward();
+        }
+      })
+      ..addStatusListener((state) => print('$state'));
     controller.forward();
   }
```

**App source:** [animate3]({{examples}}/animation/animate3)

### Refactorizando con AnimatedBuilder

{{site.alert.secondary}}
  <h4 class="no_toc">¿Qué aprenderás?</h4>

* Un [AnimatedBuilder][] entiende como renderizar la transición.
* Un `AnimatedBuilder` no conoce como renderizar el widget, ni tampoco 
  administra el objeto Animation.
* Usa `AnimatedBuilder` para describir una animación como parte de un método build 
  de otro widget. Si solo quieres definir un widget con una animación reusable, 
  use [AnimatedWidget.](#simplifying-with-animatedwidget)
* Ejemplos de AnimatedBuilders en la API de Flutter: `BottomSheet`, `ExpansionTile`,
  `PopupMenu`, `ProgressIndicator`, `RefreshIndicator`, `Scaffold`, `SnackBar`, `TabBar`,
  `TextField`.
{{site.alert.end}}

Un problema con el código en el ejemplo 
[animate3]({{examples}}/animation/animate3/lib/main.dart)
, es que cambiar la animación requiere 
cambiar el widget que renderiza el logo.
Una mejor solución es separar las responsabilidades 
en dos clases diferentes:

* Renderizar el logo
* Definir el objeto Animation
* Renderizar la transición

Puedes conseguir esta separación con la ayuda de la clase 
`AnimatedBuilder`. Un `AnimatedBuilder` es una clase separada en el 
árbol de renderizado. Como `AnimatedWidget`, `AnimatedBuilder `automáticamente 
escucha las notificaciones del objeto Animation, y marca 
el árbol de widgets como _dirty_ cuando sea necesario, entonces no necesitas 
llamar a `addListener()`.

El árbol de widgets para el ejemplo 
[animate5]({{examples}}/animation/animate4/lib/main.dart)
se ve como esto:

{% asset 'ui/AnimatedBuilder-WidgetTree.png'
    alt="AnimatedBuilder widget tree" class="d-block mx-auto" width="160px" %}

Empezando por el fondo del árbol de widget, el código para renderizar el 
logo es sencillo:

<?code-excerpt "animate4/lib/main.dart (LogoWidget)"?>
```dart
class LogoWidget extends StatelessWidget {
  // Leave out the height and width so it fills the animating parent
  Widget build(BuildContext context) => Container(
        margin: EdgeInsets.symmetric(vertical: 10),
        child: FlutterLogo(),
      );
}
```

Los tres bloques centrales en el diagrama son todos creados en el método 
`build()` en `GrowTransition`. El widget `GrowTransition` en sí mismo 
es stateless y soporta el conjunto final de variable necesarias para 
definir la animación de transición. La función `build()` crea y devuelve 
el `AnimatedBuilder`, que toma el método (constructor anónimo) y 
el objeto `LogoWidget` como parámetros. El trabajo de renderizar 
la transición actualmente ocure en el método (construcor anónimo), 
que crea un `Container` del tamaño apropiado para forzar a 
`LogoWidget` a ajustarse para llenarlo.

Un punto complicado en el código más abajo, es que la propiedad _child_ se 
ve como si se hubiera definido dos veces. Lo que está ocurriendo es 
que la referencia externa del hijo esta siendo pasada al `AnimatedBuilder`, 
el cual pase este a la función anónima, que usa este objeto 
como su hijo. La red resulta en que `AnimatedBuilder` es insertado entre los dos widgets 
en el árbol de renderizado.

<?code-excerpt "animate4/lib/main.dart (GrowTransition)"?>
```dart
class GrowTransition extends StatelessWidget {
  GrowTransition({this.child, this.animation});

  final Widget child;
  final Animation<double> animation;

  Widget build(BuildContext context) => Center(
        child: AnimatedBuilder(
            animation: animation,
            builder: (context, child) => Container(
                  height: animation.value,
                  width: animation.value,
                  child: child,
                ),
            child: child),
      );
}
```

Finalmente, el código para iniciar la animación se ve muy similar 
al primer ejemplo,
[animate1.]({{examples}}/animation/animate2/lib/main.dart)
El método `initState()` crea un `AnimationController`
y un `Tween`, entonces vincula estos con `animate()`. La mágia ocurre en el método 
`build()`, que devuelve un objeto `GrowTransition` con un 
`LogoWidget` como hijo, un objeto animation para dirigir la transición.
Estos son los tres elementos listados en los puntos más arriba.

<?code-excerpt "animate{2,4}/lib/main.dart" from="class _LogoAppState" diff-u="10"?>
```diff
--- animate2/lib/main.dart
+++ animate4/lib/main.dart
@@ -27,22 +36,25 @@
 class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
   Animation<double> animation;
   AnimationController controller;

   @override
   void initState() {
     super.initState();
     controller =
         AnimationController(duration: const Duration(seconds: 2), vsync: this);
     animation = Tween<double>(begin: 0, end: 300).animate(controller);
     controller.forward();
   }

   @override
-  Widget build(BuildContext context) => AnimatedLogo(animation: animation);
+  Widget build(BuildContext context) => GrowTransition(
+        child: LogoWidget(),
+        animation: animation,
+      );

   @override
   void dispose() {
     controller.dispose();
     super.dispose();
   }
 }
```

**App source:** [animate4]({{examples}}/animation/animate4)


### Animaciones simultáneas

{{site.alert.secondary}}
  <h4 class="no_toc">¿Qué aprenderás?</h4>

* La clase [Curves][]
  define un array de curvas usadas comúnmente que puedes usar con un 
  [CurvedAnimation][].
{{site.alert.end}}

En esta sección, construirás el ejemplo de [monitorizando 
el progreso de la animación](#monitoring)
([animate3]({{examples}}/animation/animate3/lib/main.dart)),
que usa `AnimatedWidget` para animarlo dentro y fuera continuamente. 
Considera el caso en que queras animar adentro y afuera mientras que 
animas la opacidad de transparente a opaco.

{{site.alert.note}}
Este ejemplo muestra como usar múltiples tweens en el mismo animation controller, 
donde cada uno administra un efecto diferente en la animación.
Solo para fines ilustrativos. Si quieres interpolar la opacidad y el tamaño 
en código de producción, problamente deberías usar 
[FadeTransition][] y [SizeTransition][] en su lugar.
{{site.alert.end}}

Cada tween administra un aspecto de la animación. Por ejemplo:

<?code-excerpt "animate5/lib/main.dart (tweens)" plaster="none"?>
```dart
controller =
    AnimationController(duration: const Duration(seconds: 2), vsync: this);
sizeAnimation = Tween<double>(begin: 0, end: 300).animate(controller);
opacityAnimation = Tween<double>(begin: 0.1, end: 1).animate(controller);
```

Puedes obtber el tamaño con `sizeAnimation.value` y la opacidad con 
`opacityAnimation.value`, pero el construcor para `AnimatedWidget`
solo toma un único objeto Animation. Para resolver este problema,
el ejemplo crea su propio objeto Tween y calcula los 
valores explícitamente.

El widget `AnimatedLogo` fue cambiado para encapsular sus propios objetos `Tween`.
Su método `build` llama a la función `Tween.evaluate()` del Tween en el objeto
animation padre para calcular el tamaño requerido y los valores de opacidad.

El siguiente código muestra los cambios con resaltado:

<?code-excerpt "animate5/lib/main.dart (diff)" replace="/(static final|child: Opacity|opacity:|_sizeTween\.|CurvedAnimation).*/[!$&!]/g"?>
```dart
class AnimatedLogo extends AnimatedWidget {
  // Make the Tweens static because they don't change.
  [!static final _opacityTween = Tween<double>(begin: 0.1, end: 1);!]
  [!static final _sizeTween = Tween<double>(begin: 0, end: 300);!]

  AnimatedLogo({Key key, Animation<double> animation})
      : super(key: key, listenable: animation);

  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return Center(
      [!child: Opacity(!]
        [!opacity: _opacityTween.evaluate(animation),!]
        child: Container(
          margin: EdgeInsets.symmetric(vertical: 10),
          height: [!_sizeTween.evaluate(animation),!]
          width: [!_sizeTween.evaluate(animation),!]
          child: FlutterLogo(),
        ),
      ),
    );
  }
}

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  Animation<double> animation;
  AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller =
        AnimationController(duration: const Duration(seconds: 2), vsync: this);
    animation = [!CurvedAnimation(parent: controller, curve: Curves.easeIn)!]
      ..addStatusListener((status) {
        if (status == AnimationStatus.completed) {
          controller.reverse();
        } else if (status == AnimationStatus.dismissed) {
          controller.forward();
        }
      });
    controller.forward();
  }

  @override
  Widget build(BuildContext context) => AnimatedLogo(animation: animation);

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }
}
```
**App source:** [animate5]({{examples}}/animation/animate5)

## Siguientes pasos

Este tutorial te da una base para crear animaciones en Flutter usando `Tweens`, 
pero hay muchas otras clases a explorar.
Puedes investigar las clases especializadas `Tween`,
animaciones específicas de Material Design, `ReverseAnimation`, elementos compartidos 
en transiciones (también conocidas como animaciones Hero), simulaciones físicas y 
métodos `fling()`. Mira la 
[página animaciones](/docs/development/ui/animations) 
para los últimos documentos y ejemplos disponibles.

AnimatedWidget]: {{site.api}}/flutter/widgets/AnimatedWidget-class.html
[Animatable]: {{site.api}}/flutter/animation/Animatable-class.html
[Animation]: {{site.api}}/flutter/animation/Animation-class.html
[AnimatedBuilder]: {{site.api}}/flutter/widgets/AnimatedBuilder-class.html
[AnimationController]: {{site.api}}/flutter/animation/AnimationController-class.html
[Curves]: {{site.api}}/flutter/animation/Curves-class.html
[CurvedAnimation]: {{site.api}}/flutter/animation/CurvedAnimation-class.html
[FadeTransition]: {{site.api}}/flutter/widgets/FadeTransition-class.html
[RepaintBoundary]: {{site.api}}/flutter/widgets/RepaintBoundary-class.html
[SlideTransition]: {{site.api}}/flutter/widgets/SlideTransition-class.html
[SizeTransition]: {{site.api}}/flutter/widgets/SizeTransition-class.html
[Tween]: {{site.api}}/flutter/animation/Tween-class.html

