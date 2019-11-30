---
title: Usar Themes para compartir colores y estilos de fuente
short-title: Themes
description: Como compartir colores y fuentes a lo largo de una app usando Themes.
prev:
  title: Actualizar el UI basado en la orientación
  path: /docs/cookbook/design/orientation
next:
  title: Uso de fuentes personalizadas
  path: /docs/cookbook/design/fonts
---

Para compartir colores y estilos de fuente a través de nuestra aplicación, usa los themes. 
Puedes definir un theme app-widep ara la app completa, o usar widgets `Theme` 
que definen los colores y estilos de fuente para una parte 
particular de la aplicación. De hecho, 
los themes app-wide son simplemente widgets `Theme` 
creados en la raíz de las apps por por `MaterialApp`. 

Después de definir un Theme, úsalo dentro de tus propios widgets. 
Además, los widgets Material proporcionados por Flutter utilizarán nuestro Theme para establecer los 
colores de fondo y los estilos de fuente para AppBars, Buttons, Checkboxes, y más.    

## Creando una app theme

Para compartir un Theme a lo largo de toda la app, proporciona un
[`ThemeData`]({{site.api}}/flutter/material/ThemeData-class.html)
al constructor de `MaterialApp`.

Si no se proporciona ningún `theme` , Flutter crea un theme por defecto por ti.

<!-- skip -->
```dart
MaterialApp(
  title: title,
  theme: ThemeData(
    // Define el brightness y colores por defecto
    brightness: Brightness.dark,
    primaryColor: Colors.lightBlue[800],
    accentColor: Colors.cyan[600],

    // Define la familia de fuente por defecto
    fontFamily: 'Montserrat',
    
    // Define el TextTheme por defecto. Usa esto para espicificar el estilo de texto por defecto 
    // para cabeceras, títulos, cuerpos de texto, y más.
    textTheme: TextTheme(
      headline: TextStyle(fontSize: 72.0, fontWeight: FontWeight.bold),
      title: TextStyle(fontSize: 36.0, fontStyle: FontStyle.italic),
      body1: TextStyle(fontSize: 14.0, fontFamily: 'Hind'),
     ),
  )
);
```

Mira la documentación de [ThemeData]({{site.api}}/flutter/material/ThemeData-class.html)
para ver todos los colores y fuentes que puede definir.

## Temas para parte de una aplicación

Si queremos sobrescribir el theme app-wide en parte de nuestra aplicación, podemos 
envolver una sección de nuestra aplicación en un Widget `Theme`.

Hay dos formas de abordar esto: crear un `ThemeData` único, o 
extender el theme padre.

### Creando un `ThemeData` único

Si no quieres heredar ninguno de los colores de la aplicación o estilos de fuente, crea una instancia de 
 `ThemeData()` y pásala al widget `Theme` .

<!-- skip -->
```dart
Theme(
  // Crea un theme único con "ThemeData"
  data: ThemeData(
    accentColor: Colors.yellow,
  ),
  child: FloatingActionButton(
    onPressed: () {},
    child: Icon(Icons.add),
  ),
);
```

### Extendiendo el tema padre

En lugar de sobrescribir todo, a menudo tiene sentido extender el tema 
padre. Puedes lograr esto utilizando el 
método 
[`copyWith`]({{site.api}}/flutter/material/ThemeData/copyWith.html).

<!-- skip -->
```dart
Theme(
  // Encuentra y extiende el theme padre usando "copyWith". Mira la siguiente  
  // sección para más información sobre `Theme.of`.
  data: Theme.of(context).copyWith(accentColor: Colors.yellow),
  child: FloatingActionButton(
    onPressed: null,
    child: Icon(Icons.add),
  ),
);
```

## Usando un Theme

Ahora que has definido un theme, úsalo en los métodos `build` del widget 
utilizando la función `Theme.of(context)`.

`Theme.of(context)` busca en el árbol de widgets y devuelve el `Theme` 
más cercano en el árbol. Si tenemos un `Theme` stand-alone definido sobre nuestro Widget, 
lo devuelve. Si no, devuelve el tema de la App.

De hecho, el `FloatingActionButton` usa esta técnica exacta para encontrar el 
`accentColor`!
 
<!-- skip -->
```dart
Container(
  color: Theme.of(context).accentColor,
  child: Text(
    'Texto con un color de fondo',
    style: Theme.of(context).textTheme.title,
  ),
);
```   

## Ejemplo completo

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appName = 'Custom Themes';

    return MaterialApp(
      title: title,
      theme: ThemeData(
        // Define el brightness y colores por defecto
        brightness: Brightness.dark,
        primaryColor: Colors.lightBlue[800],
        accentColor: Colors.cyan[600],

        // Define la familia de fuente por defecto
        fontFamily: 'Montserrat',
        
        // Define el TextTheme por defecto. Usa esto para espicificar el estilo de texto por defecto 
        // para cabeceras, títulos, cuerpos de texto, y más.
        textTheme: TextTheme(
          headline: TextStyle(fontSize: 72.0, fontWeight: FontWeight.bold),
          title: TextStyle(fontSize: 36.0, fontStyle: FontStyle.italic),
          body1: TextStyle(fontSize: 14.0, fontFamily: 'Hind'),
        ),
      )
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  MyHomePage({Key key, @required this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: Container(
          color: Theme.of(context).accentColor,
          child: Text(
            'Text with a background color',
            style: Theme.of(context).textTheme.title,
          ),
        ),
      ),
      floatingActionButton: Theme(
        data: Theme.of(context).copyWith(
          colorScheme:
              Theme.of(context).colorScheme.copyWith(secondary: Colors.yellow),
        ),
        child: FloatingActionButton(
          onPressed: null,
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}
```

![Themes Demo](/images/cookbook/themes.png){:.site-mobile-screenshot}