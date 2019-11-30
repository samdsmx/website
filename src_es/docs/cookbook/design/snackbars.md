---
title: Mostrar un snackbar
description: Como implementar un SnackBar para mostrar mensajes
prev:
  title: Añadir un Drawer a la pantalla
  path: /docs/cookbook/design/drawer
next:
  title: Exportar fuentes desde un paquete
  path: /docs/cookbook/design/package-fonts
---

Puede ser útil informar brevemente a nuestros usuarios cuando ciertas acciones
se llevan a cabo. Por ejemplo, cuando un usuario borra un mensaje de una lista, 
querrás informarles que el mensaje ha sido borrado. 
Puede que incluso queramos darle una opción para deshacer la acción 

En Material Design, este es el trabajo de un 
[SnackBar]({{site.api}}/flutter/material/SnackBar-class.html).
Esta receta implementa un snackbar usando los siguientes pasos:

  1. Crea un `Scaffold`
  2. Muestra un `SnackBar`
  3. Proporciona una acción adicional
  
## 1. Crea un `Scaffold`

Cuando creas aplicaciones que siguen las directrices de Material Design, 
das a tus aplicaciones una estructura visual consistente. 
En este ejemplo, mostrarás el SnackBar en la parte 
inferior de la pantalla, sin solapar otros Widgets importantes, 
como el `FloatingActionButton`.

El Widget 
[Scaffold]({{site.api}}/flutter/material/Scaffold-class.html)
de la [biblioteca Material]({{site.api}}/flutter/material/material-library.html) crea esta 
estructura visual para nosotros y asegura que los Widgets 
importantes no se superpongan.

<!-- skip -->
```dart
Scaffold(
  appBar: AppBar(
    title: Text('SnackBar Demo'),
  ),
  body: SnackBarPage(), // Completa este código en el siguiente paso
);
```

## 2. Muestra un `SnackBar`

Con el `Scaffold` en su lugar, muestra un `SnackBar`.
Primero, crea un `SnackBar`, luego muéstralo usando el `Scaffold`.

<!-- skip -->
```dart
final snackBar = SnackBar(content: Text('Yay! A SnackBar!'));

// Encuentra el Scaffold en el árbol de widgets y úsalo para mostrar un SnackBar
Scaffold.of(context).showSnackBar(snackBar);
```

## 3. Proporciona una acción adicional

Podrías querer proporcionar una acción adicional al 
usuario cuando se muestre el SnackBar. 
Por ejemplo, si el usuario ha eliminado accidentalmente un mensaje, 
puedes ofrecer una acción opcional en el SnackBar para recuperar 
el mensaje.

Aquí hay un ejemplo de proporcionar un `action` 
adicional al Widget `SnackBar` .

```dart
final snackBar = SnackBar(
  content: Text('Yay! A SnackBar!'),
  action: SnackBarAction(
    label: 'Undo',
    onPressed: () {
      // Algo de código para deshacer el cambio.
    },
  ),
);
``` 

## Ejemplo completo 

{{site.alert.note}}
  En este ejemplo, el SnackBar se muestra cuando un usuario pulse un botón.
  Para obtener más información sobre cómo trabajar con las entradas del usuario, mira la sección 
  [Manejando Gestos](/docs/cookbook#gestures) del cookbook.
{{site.alert.end}}

```dart
import 'package:flutter/material.dart';

void main() => runApp(SnackBarDemo());

class SnackBarDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'SnackBar Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text('SnackBar Demo'),
        ),
        body: SnackBarPage(),
      ),
    );
  }
}

class SnackBarPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: RaisedButton(
        onPressed: () {
          final snackBar = SnackBar(
            content: Text('Yay! A SnackBar!'),
            action: SnackBarAction(
              label: 'Undo',
              onPressed: () {
                // Algo de código para deshacer el cambio.
              },
            ),
          );

          // Encuentra el Scaffold en el árbol de widgets 
          // y úsalo para mostrar un SnackBar.
          Scaffold.of(context).showSnackBar(snackBar);
        },
        child: Text('Show SnackBar'),
      ),
    );
  }
}
```

![SnackBar Demo](/images/cookbook/snackbar.gif){:.site-mobile-screenshot}