---
title: Añadir un Drawer a la pantalla
prev:
  title: Efectos Fade in and out en un widget
  path: /docs/cookbook/animation/opacity-animation
next:
  title: Mostrando un snackbar
  path: /docs/cookbook/design/snackbars
---

En las aplicaciones que usan Material Design, 
hay dos opciones principales de navegación: tabs y drawers. 
Cuando no hay suficiente espacio para sostener las pestañas, 
los Drawers proporcionan una alternativa práctica. 

En Flutter, usa el widget 
[`Drawer`]({{site.api}}/flutter/material/Drawer-class.html) 
en combinación con un 
[`Scaffold`]({{site.api}}/flutter/material/Scaffold-class.html) para 
crear un layout con un Material Design Drawer.
Esta receta usa los siguientes pasos:

  1. Crea un `Scaffold`
  2. Añade un drawer
  3. Añade elementos al Drawer
  4. Cierra el drawer programáticamente
  
## 1. Crea un `Scaffold`

Para añadir un drawer a la app, envuélvelo en un widget 
[Scaffold]({{site.api}}/flutter/material/Scaffold-class.html). 
El widget Scaffold proporciona una estructura visual consistente a las aplicaciones 
que siguen las Directrices de Material Design. 
También admite componentes especiales de Material Design, 
como Drawers, AppBars, y SnackBars.

En este ejemplo, queremos crear un `Scaffold` con un `drawer`:

<!-- skip -->
```dart
Scaffold(
  drawer: // Añade un Drawer aquí en el siguiente paso.
);
```

## 2. Agrega un drawer

Ahora añade un drawer al `Scaffold`. Un drawer puede ser 
cualquier Widget, pero a menudo es mejor utilizar el widget 
`Drawer` de la [biblioteca Material]({{site.api}}/flutter/material/material-library.html), 
que se adhiere a las especificaciones de Material Design.

<!-- skip -->
```dart
Scaffold(
  drawer: Drawer(
    child: // Rellenaremos el Drawer en el siguiente paso.
  )
);
```

## 3. Añade elementos al Drawer

Ahora que tienes un `Drawer` en su lugar, añádele contenido. 
Para este ejemplo, 
usa un [`ListView`]({{site.api}}/flutter/widgets/ListView-class.html). 
Aunque puedes usar un Widget `Column`, `ListView` es útil en esta situación, ya que permite 
a los usuarios desplazarse por el drawer si el contenido ocupa más espacio del 
que soporta la pantalla.

Rellena el `ListView` con un [`DrawerHeader`]({{site.api}}/flutter/material/DrawerHeader-class.html) 
y dos widgets [`ListTile`]({{site.api}}/flutter/material/ListTile-class.html). 
Para obtener más información sobre 
cómo trabajar con Listas, por favor consulta la 
[recetas de Listas](/docs/cookbook#listas).

<!-- skip -->
```dart
Drawer(
  // Agrega un ListView al drawer. Esto asegura que el usuario pueda desplazarse
  // a través de las opciones en el drawer si no hay suficiente espacio vertical
  // para adaptarse a todo.
  child: ListView(
    // Importante: elimine cualquier padding del ListView.
    padding: EdgeInsets.zero,
    children: <Widget>[
      DrawerHeader(
        child: Text('Drawer Header'),
        decoration: BoxDecoration(
          color: Colors.blue,
        ),
      ),
      ListTile(
        title: Text('Item 1'),
        onTap: () {
          // Actualiza el estado de la aplicación.
          // ...
        },
      ),
      ListTile(
        title: Text('Item 2'),
        onTap: () {
          // Actualiza el estado de la aplicación.
          // ...
        },
      ),
    ],
  ),
);
```

## 4. Cierra el drawer programáticamente

Después de que un usuario toca un elemento, querrás cerrar el drawer. 
Puedes hacer esto usando la clase 
[Navigator]({{site.api}}/flutter/widgets/Navigator-class.html)!

Cuando un usuario abre el drawer, Flutter agregará el drawer a la pila de navegación. 
Por lo tanto, para cerrar el drawer, llama a `Navigator.pop(context)`.  

<!-- skip -->
```dart
ListTile(
  title: Text('Item 1'),
  onTap: () {
    // Actualiza el estado de la aplicación.
    // ...
    // Luego cierra el drawer. 
    Navigator.pop(context);
  },
),
```

## Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  final appTitle = 'Drawer Demo';

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: appTitle,
      home: MyHomePage(title: appTitle),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: Center(child: Text('My Page!')),
      drawer: Drawer(
        // Agrega un ListView al drawer. Esto asegura que el usuario pueda desplazarse
        // a través de las opciones en el drawer si no hay suficiente espacio vertical
        // para adaptarse a todo.
        child: ListView(
          // Importante: elimina cualquier padding del ListView.
          padding: EdgeInsets.zero,
          children: <Widget>[
            DrawerHeader(
              child: Text('Drawer Header'),
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
            ),
            ListTile(
              title: Text('Item 1'),
              onTap: () {
                // Actualiza el estado de la aplicación
                // ...
                // Luego cierra el drawer
                Navigator.pop(context);
              },
            ),
            ListTile(
              title: Text('Item 2'),
              onTap: () {
                // // Actualiza el estado de la aplicación
                // ...
                // Luego cierra el drawer
                Navigator.pop(context);
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

![Drawer Demo](/images/cookbook/drawer.png){:.site-mobile-screenshot}