---
title: Exportar fuentes de un paquete
prev:
  title: Mostrar un snackbars
  path: /docs/cookbook/design/snackbars
next:
  title: Actualizar el UI basado en la orientación
  path: /docs/cookbook/design/orientation
---

En lugar de declarar una fuente como parte de una app, 
puedes declarar una fuente como parte de un paquete separado. 
Esta es una forma conveniente de compartir la misma fuente a través 
de varios proyectos diferentes, 
o para desarrolladores que publican sus paquetes en el [sitio web de pub][].
Esta receta sigue los siguientes pasos:

  1. Agrega una fuente a un paquete
  2. Agrega el paquete y la fuente a la aplicación
  3. Usa la fuente
  
## 1. Agrega una fuente a un paquete

Para exportar una fuente desde un paquete, necesitas importar los archivos de fuente en la carpeta `lib` 
del paquete del proyecto. Puedes colocar los archivos de fuente directamente en la carpeta `lib` 
o en un subdirectorio, como `lib/fonts`. 

En este ejemplo, asume que tienes una biblioteca Flutter llamada 
`awesome_package` con fuentes que viven en una carpeta `lib/fonts`.

```
awesome_package/
  lib/
    awesome_package.dart
    fonts/
      Raleway-Regular.ttf
      Raleway-Italic.ttf
```

## 2. Agrega el paquete y la fuente a la aplicación

Ahora puedes usar las fuentes en el paquete  
actualizando `pubspec.yaml` en el directorio raíz de la *app*. 

### Añade el paquete a la app

```yaml
dependencies:
  awesome_package: <latest_version>
```

### Declara los font assets

Ahora que has importado el paquete, dile a Flutter dónde encontrar 
las fuentes del `awesome_package`.

Para declarar las fuentes del paquete, prefija la ruta a la fuente con 
`packages/awesome_package`. 
Esto le indica a Flutter que busque en la carpeta 
`lib` del paquete para la fuente.

```yaml
flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
        - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
          style: italic
```

## 3. Usa la fuente

Usa [`TextStyle`][] para cambiar la apariencia del texto. 
Para usar paquetes de fuentes, necesitas no sólo 
declarar qué fuente quieres usar, sino también declarar el `package` al que pertenece la fuente. 

<!-- skip -->
```dart
Text(
  'Using the Raleway font from the awesome_package',
  style: TextStyle(
    fontFamily: 'Raleway',
    package: 'awesome_package',
  ),
);
```

## Ejemplo completo

### Fuentes

Las fuentes Raleway y RobotoMono se descargaron de 
[Google Fonts](https://fonts.google.com/).

### `pubspec.yaml`

```yaml
name: package_fonts
description: Un ejemplo de cómo usar el paquete de fuentes con Flutter

dependencies:
  awesome_package:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
        - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
          style: italic
  uses-material-design: true
```

### `main.dart`

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Package Fonts',
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // AppBar usa la fuente Raleway predeterminada de la aplicación
      appBar: AppBar(title: Text('Package Fonts')),
      body: Center(
        // Este widget de texto usala fuente RobotoMono.
        child: Text(
          'Using the Raleway font from the awesome_package',
          style: TextStyle(
            fontFamily: 'Raleway',
            package: 'awesome_package',
          ),
        ),
      ),
    );
  }
}
```

![Package Fonts Demo](/images/cookbook/package-fonts.png){:.site-mobile-screenshot}

[Pub site]: {{site.pub}} 
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html