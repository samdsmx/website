---
title: Uso una fuente personalizada
description: Cómo usar fuentes personalizadas
prev:
  title: Usar themes para compartir colores y estilos de fuente
  path: /docs/cookbook/design/themes
next:
  title: Trabajar con tabs
  path: /docs/cookbook/design/tabs
---

Aunque Android e iOS ofrecen fuentes de sistema de alta calidad, 
una de las solicitudes más comunes de los diseñadores es utilizar fuentes personalizadas. 
Por ejemplo, puedes tener una fuente personalizada de nuestro diseñador 
o quizás has descargado una fuente de 
[Google Fonts](https://fonts.google.com/).

Flutter trabaja con fuentes personalizadas y puedes aplicar una 
fuente personalizada a toda la app o a widgets individuales.
Esta receta crea una app que usa fuentes personalizadas 
con los siguientes pasos:

  1. Importa los archivos de fuentes
  2. Declara la fuente en `pubspec.yaml`
  3. Establece una fuente como predeterminada 
  4. Usa una fuente en un widget específico
  
## 1. Importa los archivos de fuentes

Para trabajar con una fuente, necesitamos importar los archivos de fuente al proyecto. 
Es una práctica común colocar los archivos de fuentes en una carpeta de `fonts` o `assets` 
en la raíz de un proyecto Flutter. 

Por ejemplo, si queremos importar los archivos de fuentes Raleway y Roboto Mono 
a nuestro proyecto, la estructura de la carpeta podría verse así:

```
awesome_app/
  fonts/
    Raleway-Regular.ttf
    Raleway-Italic.ttf
    RobotoMono-Regular.ttf
    RobotoMono-Bold.ttf
```

## 2. Declara la fuente en pubspec

Ahora que has identificado una fuente, dile a Flutter dónde encontrarla. 
Puedes hacerlo incluyendo una definición de fuente en `pubspec.yaml`.

```yaml
flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: fonts/Raleway-Regular.ttf
        - asset: fonts/Raleway-Italic.ttf
          style: italic
    - family: RobotoMono
      fonts:
        - asset: fonts/RobotoMono-Regular.ttf
        - asset: fonts/RobotoMono-Bold.ttf
          weight: 700
```

### definiciones de opciones de `pubspec.yaml` 

La `family` determina el nombre de la fuente que podemos 
usar en la propiedad
[`fontFamily`](https://docs.flutter.io/flutter/painting/TextStyle/fontFamily.html)
de un objeto 
[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html).

El `asset` es una ruta al archivo de fuente, relativa al archivo `pubspec.yaml` .
Estos archivos contienen los contornos de los glifos en la fuente. 
Al construir la app, estos archivos se incluyen en el paquete de asset de la app.

Una sola fuente puede hacer referencia a muchos archivos diferentes 
con diferentes pesos y estilos de contorno:

  * La propiedad `weight` especifica el peso de los contornos en el 
    archivo como un múltiplo entero de 100 entre 100 y 900. 
    Estos valores corresponden al 
    [`FontWeight`](https://docs.flutter.io/flutter/dart-ui/FontWeight-class.html)
    y se pueden utilizar en la propiedad 
    [`fontWeight`](https://docs.flutter.io/flutter/painting/TextStyle/fontWeight.html)
    de un objeto 
    [`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html).

  * La propiedad `style` especifica si los contornos del archivo son `italic` o `normal`. Estos valores corresponden al 
    [`FontStyle`](https://docs.flutter.io/flutter/dart-ui/FontStyle-class.html)
    y se pueden utilizar en la propiedad [fontStyle](https://docs.flutter.io/flutter/painting/TextStyle/fontStyle.html)
    de un objeto [`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html).

## 3. Establece una fuente como predeterminada 

Tenemos dos opciones sobre cómo aplicar fuentes al texto: como fuente predeterminada o sólo dentro de widgets específicos.

Para usar una fuente por defecto, podemos establecer la propiedad `fontFamily` como parte del 
 `theme` de las aplicaciones. El valor que proporcionamos a `fontFamily` debe coincidir con el nombre de `family` declarado en el `pubspec.yaml`. 

<!-- skip -->
```dart
MaterialApp(
  title: 'Custom Fonts',
  // Establecer Raleway como la fuente predeterminada de la aplicación.
  theme: ThemeData(fontFamily: 'Raleway'),
  home: MyHomePage(),
);
```

Para obtener más información sobre temas, mira la receta 
["Usar Themes para compartir estilos de fuentes y colores"](/docs/cookbook/design/themes/).

## 4. Usa una fuente en un widget específico

Si queremos aplicar la fuente a un widget específico, como un Widget de `Text` , 
podemos proporcionar un [`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html) al widget.

En este ejemplo, aplicaremos la fuente RobotoMono a un único widget de  `Text`. 
Una vez más, la `fontFamily` debe coincidir con el nombre `family` que declaramos en 
`pubspec.yaml`. 

<!-- skip -->
```dart
Text(
  'Roboto Mono sample',
  style: TextStyle(fontFamily: 'RobotoMono'),
);
```

### Estilo de texto

Si un objeto [`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)
especifica un peso o estilo para el que no existe un archivo de fuente exacto, 
el motor utiliza uno de los archivos más genéricos para la fuente e intenta 
extrapolar los contornos para el peso y estilo solicitados.  

## Ejemplo completo

### Fuentes

Las fuentes Raleway y RobotoMono se descargaron de [Google Fonts](https://fonts.google.com/).

### `pubspec.yaml`

```yaml
name: custom_fonts
description: Un ejemplo de cómo usar fuentes personalizadas con Flutter

dependencies:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: fonts/Raleway-Regular.ttf
        - asset: fonts/Raleway-Italic.ttf
          style: italic
    - family: RobotoMono
      fonts:
        - asset: fonts/RobotoMono-Regular.ttf
        - asset: fonts/RobotoMono-Bold.ttf
          weight: 700
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
      title: 'Custom Fonts',
      // Establecer Raleway como la fuente predeterminada de la aplicación.
      theme: ThemeData(fontFamily: 'Raleway'),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // AppBar utilizará la fuente Raleway predeterminada de la aplicación.
      appBar: AppBar(title: Text('Custom Fonts')),
      body: Center(
        // Este Widget de texto usará la fuente RobotoMono.
        child: Text(
          'Roboto Mono sample',
          style: TextStyle(fontFamily: 'RobotoMono'),
        ),
      ),
    );
  }
}
```

![Custom Fonts Demo](/images/cookbook/fonts.png){:.site-mobile-screenshot}