---
title: "Manejando gestos Tap"
rev:
  title: Añadir el efecto ripple de Material Touch
  path: /docs/cookbook/gestures/ripples
next:
  title: Implementar Swipe para Cancelar
  path: /docs/cookbook/gestures/dismissible

---

No solo queremos mostrar información a nuestros usuarios, queremos que nuestros usuarios 
interactúen con nuestras aplicaciones. Entonces, ¿cómo respondemos a acciones fundamentales 
como tapping y dragging? ¡Usaremos el widget 
[`GestureDetector`]({{site.api}}/flutter/widgets/GestureDetector-class.html)!

Digamos que queremos hacer un botón personalizado que muestre un snackbar cuando hagamos tap. 
¿Cómo enfocaríamos esto?

## Instrucciones

  1. Crea el botón
  2. Envuélvelo en un `GestureDetector` con un callback `onTap` 

<!-- skip -->
```dart
// Nuestro GestureDetector envuelve nuestro botón
GestureDetector(
  // Cuando el hijo reciba un tap, muestra un snackbar 
  onTap: () {
    final snackBar = SnackBar(content: Text("Tap"));

    Scaffold.of(context).showSnackBar(snackBar);
  },
  // Nuestro botón personalizado!
  child: Container(
    padding: EdgeInsets.all(12.0),
    decoration: BoxDecoration(
      color: Theme.of(context).buttonColor,
      borderRadius: BorderRadius.circular(8.0),
    ),
    child: Text('My Button'),
  ),
);
```

## Notas

  1. Si deseas agregar el efecto Material Ripple a tu botón, por favor consulta la receta
  "[Añadiendo efecto ripple de Material Touch](/docs/cookbook/gestures/ripples/)".
  2. Si bien hemos creado un botón personalizado para demostrar estos conceptos, Flutter 
   incluye un puñado de botones: [RaisedButton]({{site.api}}/flutter/material/RaisedButton-class.html), 
  [FlatButton]({{site.api}}/flutter/material/FlatButton-class.html), 
  y [CupertinoButton]({{site.api}}/flutter/cupertino/CupertinoButton-class.html)
    

## Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Gesture Demo';

    return MaterialApp(
      title: title,
      home: MyHomePage(title: title),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(child: MyButton()),
    );
  }
}

class MyButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Nuestro GestureDetector envuelve nuestro botón
    return GestureDetector(
      // Cuando el hijo reciba un tap, muestra un snackbar
      onTap: () {
        final snackBar = SnackBar(content: Text("Tap"));

        Scaffold.of(context).showSnackBar(snackBar);
      },
      // Nuestro botón personalizado!
      child: Container(
        padding: EdgeInsets.all(12.0),
        decoration: BoxDecoration(
          color: Theme.of(context).buttonColor,
          borderRadius: BorderRadius.circular(8.0),
        ),
        child: Text('My Button'),
      ),
    );
  }
}
```

![Handling Taps Demo](/images/cookbook/handling-taps.gif){:.site-mobile-screenshot}
