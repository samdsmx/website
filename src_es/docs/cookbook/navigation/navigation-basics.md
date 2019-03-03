---
title: Navegar a una nueva pantalla y volver
description: Como navegar entre rutas
prev:
  title: Animar un Widget entre pantallas
  path: /docs/cookbook/navigation/hero-animations
next:
  title: Navegar a rutas con nombre
  path: /docs/cookbook/navigation/named-routes
---

La mayoría de las aplicaciones contienen varias pantallas para mostrar diferentes tipos de 
información. Por ejemplo, una app puede tener una pantalla que muestre productos. Los usuarios 
pueden entonces pulsar un producto para obtener más información sobre él en una nueva pantalla.

{{site.alert.info}}
  **Terminología**: En Flutter, _pantallas_ y _paginas_ se llaman _rutas_.
  El resto de este documento usa el término rutas.
{{site.alert.end}}

En Android, una ruta es equivalente a un Activity. 
En iOS, una ruta es equivalente a un ViewController.
En Flutter, una ruta es solo un widget.

¿Cómo navegamos a una nueva ruta? Usando 
[`Navigator`]({{site.api}}/flutter/widgets/Navigator-class.html)!

## Instrucciones

Las siguientes secciones muestran como navigar entre dos rutas,
usando estos pasos:

  1. Crea dos rutas
  2. Navega a la segunda ruta usando `Navigator.push`
  3. Regresa a la primera ruta usando `Navigator.pop`

## 1.  Crea dos rutas

Primero, crea dos rutas para trabajar con ellas. Como este es un ejemplo básico,
cada ruta contiene solo un simple bóton. Pulsando el botón en la 
primera ruta navegas a la segunda ruta. Pulsando el botón en la 
segunda ruta vuelves a la primera ruta.

Primero, configura la estructura visual.

```dart
class FirstRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Route'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Open route'),
          onPressed: () {
            // Navega a la segunda ruta cuando se pulsa.
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Route"),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: () {
            // Regresa a la primera ruta cuando se pulsa.
          },
          child: Text('Go back!'),
        ),
      ),
    );
  }
}
```

## 2. Navega a la segunda pantalla usando `Navigator.push`

Para navegar a una nueva ruta, usa el método 
[`Navigator.push`]({{site.api}}/flutter/widgets/Navigator/push.html). 
El método `push` agregará una `Route` a la pila de rutas administradas por 
el Navigator. Pero ¿de dónde viene la `Route`? 
Puedes crear la tuya, o usar un 
[`MaterialPageRoute`]({{site.api}}/flutter/material/MaterialPageRoute-class.html). 
`MaterialPageRoute` es muy práctico, ya que la transición a la 
nueva ruta usa una animación específica de la plataforma. 

En el método `build()` del widget `FirstRoute`, actualiza el 
callback `onPressed`:

<!-- skip -->
```dart
// Dentro del widget `FirstRoute`
onPressed: () {
  Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => SecondRoute()),
  );
}
``` 

## 3. Regresa a la primer pantalla usando `Navigator.pop`

¿Cómo cerramos la segunda ruta y volvemos a la primera? Usando el método 
[`Navigator.pop`]({{site.api}}/flutter/widgets/Navigator/pop.html)! 
El método `pop` elimina la `Route` actual de 
la pila de rutas administradas por Navigator.

Para implementar un regreso al la ruta original, actualiza el callback 
`onPressed` en el widget `SecondRoute` 

<!-- skip -->
```dart
// Dentro del widget SecondRoute
onPressed: () {
  Navigator.pop(context);
}
```    

## Ejemplo Completo

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    title: 'Navigation Basics',
    home: FirstRoute(),
  ));
}

class FirstRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Route'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Open route'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondRoute()),
            );
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Route"),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Go back!'),
        ),
      ),
    );
  }
}
```

{% comment %}
We need a new GIF that shows "Route" instead of "Screen".
{% endcomment %}

![Navigation Basics Demo](/images/cookbook/navigation-basics.gif){:.site-mobile-screenshot}
