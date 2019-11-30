---
title: Anima una transición entre páginas
next:
  title: Anima las propiedades de un container
  path: /docs/cookbook/animation/animated-container
---

# Anima una transición entre páginas

Una lenguaje de diseño, como es Material, define comportamientos estándar 
cuando se hace transición entre rutas (o pantallas). Algunas veces, una transición 
entre pantallas personalizada puede hacer una app más única. Para ayudar,
[PageRouteBuilder]({{site.api}}/flutter/widgets/PageRouteBuilder-class.html)
proporciona un objeto 
[Animation]({{site.api}}/flutter/animation/Animation-class.html). 
Este `Animation` puede ser usado con los objetos 
[Tween]({{site.api}}/flutter/animation/Tween-class.html) y
[Curve]({{site.api}}/flutter/animation/Curve-class.html) para 
personalizar la animación de la transición. Esta receta muestra como hacer la 
transición entre rutas animando la nueva ruta en la vista desde la parte baja de la pantalla.

Para crear una transición entre rutas personalizada, esta receta usa los siguientes pasos:

1. Configura un PageRouteBuilder
2. Crea un `Tween`
3. Añade un `AnimatedWidget`
4. Usa un `CurveTween`
5. Combina los dos `Tween`s

# 1. Configura un PageRouteBuilder

Para empezar, usa un
[PageRouteBuilder]({{site.api}}/flutter/widgets/PageRouteBuilder-class.html)
para crear una [Route]({{site.api}}/flutter/widgets/Route-class.html).
`PageRouteBuilder` tiene dos callbacks, uno para construir el contenido de la ruta 
(`pageBuilder`), y uno para construir la transición entre rutas (`transitionsBuilder`).

{{site.alert.note}}
El parámetro `child` en transitionsBuilder es el widget retornado desde 
pageBuilder. La función `pageBuilder` solo es llamada la primera vez que la ruta 
es construida. El framework puede evitar trabajo extra porque el `child` se mantiene el mismo 
durante la transición.
{{site.alert.end}}

El siguiente ejemplo crea dos rutas: una ruta home con un botón "Go!", y 
una segunda ruta titulada "Page 2".

```dart
import 'package:flutter/material.dart';

main() {
  runApp(MaterialApp(
    home: Page1(),
  ));
}

class Page1 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: RaisedButton(
          child: Text('Go!'),
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      return child;
    },
  );
}

class Page2 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```

# 2. Crea un Tween

Para hacer que la nueva página anime desde la parte de abajo, se debe animar desde 
`Offset(0,1)` a `Offset(0, 0)` (usualmente definido usando el constructor `Offset.zero`). 
En este caso, el Offset es un vector 2D para el widget 
[FractionalTranslation]({{site.api}}/flutter/widgets/FractionalTranslation-class.html). 
Fijar el argumento `dy` a 1 representa una transición vertical a todo el 
alto de la página.

El callback `transitionsBuilder` tiene un parámetro `animation`. Este es un
`Animation<double>` que produce valores entre 0 y 1. Convierte el 
`Animation<double>` en un `Animation<Offset>` usando un Tween:

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0);
  var end = Offset.zero;
  var tween = Tween(begin: begin, end: end);
  var offsetAnimation = animation.drive(tween);
  return child;
},
```

# 3. Usa un AnimatedWidget

Flutter tiene un conjunto de widgets que heredan de 
[AnimatedWidget]({{site.api}}/flutter/widgets/AnimatedWidget-class.html)
que se reconstruyen a sí mismos cuando el valor del animation cambia. Por ejemplo,
SlideTransition toma un `Animation<Offset>` y traslada su hijo (usando un widget 
FractionalTranslation) cada vez que el valor del animation cambia.

AnimatedWidget devuelve un 
[SlideTransition]({{site.api}}/flutter/widgets/SlideTransition-class.html)
con el `Animation<Offset>` y el widget hijo:

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0);
  var end = Offset.zero;
  var tween = Tween(begin: begin, end: end);
  var offsetAnimation = animation.drive(tween);

  return SlideTransition(
    position: offsetAnimation,
    child: child,
  );
},
```

# 4. Usa un CurveTween

Flutter proporciona una selección de curvas easing que ajustan el ratio de animación 
a lo largo del tiempo. La clase 
[Curves]({{site.api}}/flutter/animation/Curves-class.html) 
proporciona un conjunto predefinido de curvas usadas comúnmente. Por ejemplo, `Curves.easeOut`
hará que la animación comience rápidamente y termina despacio.

Para usar una Curve, crea un nuevo
[CurveTween]({{site.api}}/flutter/animation/CurveTween-class.html)
y pásale una Curve:

```dart
var curve = Curves.ease;
var curveTween = CurveTween(curve: curve);
```

Este nuevo Tween aún produce valores desde 0 a 1. En el siguiente paso, será combinado 
con el `Tween<Offset>` del paso 2.

# 5. Combina los dos Tweens

Para combinar los, usa
[chain()]({{site.api}}/flutter/animation/Animatable/chain.html):

```dart
var begin = Offset(0.0, 1.0);
var end = Offset.zero;
var curve = Curves.ease;

var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));
```

Entonces usa este tween pasándolo al `animation.drive()`. Estp crea un nuevo 
`Animation<Offset>` que puede ser tomado por el widget `SlideTransition`:

```dart
return SlideTransition(
  position: animation.drive(tween),
  child: child,
);
```

Este nuevo Tween (o Animatable) produce valores `Offset` por primera vez evaluando 
el `CurveTween`, y despues evaluando el `Tween<Offset>.` Cuando la animación está en marcha, los 
valores son computados en este orden:

1. El animation (proporcionado al transitionsBuilder callback) produce valores 
   entre 0 y 1.
2. El CurveTween mapea estos valores en nuevos valores entre 0 and 1 basados en su 
   Curve.
3. El `Tween<Offset>` mapea el valor `double` a valores `Offset`.

Otra manera de crear un `Animation<Offset>` con curva easing es usar un 
`CurvedAnimation`:

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0); 
  var end = Offset.zero;
  var curve = Curves.ease;
  
  var tween = Tween(begin: begin, end: end);
  var curvedAnimation = CurvedAnimation(
   parent: animation,
   curve: curve,
  );

  return SlideTransition(
   position: tween.animate(curvedAnimation),
   child: child,
  );
}
```

# Ejemplo completo

```dart
import 'package:flutter/material.dart';

main() {
  runApp(MaterialApp(
    home: Page1(),
  ));
}

class Page1 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: RaisedButton(
          child: Text('Go!'),
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      var begin = Offset(0.0, 1.0);
      var end = Offset.zero;
      var curve = Curves.ease;

      var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));

      return SlideTransition(
        position: animation.drive(tween),
        child: child,
      );
    },
  );
}

class Page2 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```

![Demo showing a custom page route transition animating up from the bottom of the screen](/images/cookbook/page-route-animation.gif){:.site-mobile-screenshot}