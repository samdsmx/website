---
title: "Efecto “Fade in” en imágenes con un placeholder"
---

Al mostrar imágenes usando el widget `Image` predeterminado, es posible que notes que simplemente aparecen en la pantalla a medida que se cargan. Esto puede parecer visualmente molesto para tus usuarios.

En vez de eso, ¿no sería bueno si pudieras mostrar un placeholder al principio, y 
las imágenes se desvanecerían a medida que se cargan? ¡Podemos usar el Widget [`FadeInImage`](https://docs.flutter.io/flutter/widgets/FadeInImage-class.html) 
 empaquetado con Flutter exactamente para este propósito!

`FadeInImage` funciona con imágenes de cualquier tipo: en memoria, recursos locales o imágenes de Internet.

## En Memoria

En este ejemplo, usaremos el paquete [transparent_image](https://pub.dartlang.org/packages/transparent_image) para un simple placeholder transparente.

<!-- skip -->
```dart
FadeInImage.memoryNetwork(
  placeholder: kTransparentImage,
  image: 'https://github.com/flutter/website/blob/master/src/_includes/code/layout/lakes/images/lake.jpg?raw=true',
);
```

### Ejemplo completo

```dart
import 'package:flutter/material.dart';
import 'package:transparent_image/transparent_image.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Fade in images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Stack(
          children: <Widget>[
            Center(child: CircularProgressIndicator()),
            Center(
              child: FadeInImage.memoryNetwork(
                placeholder: kTransparentImage,
                image:
                    'https://github.com/flutter/website/blob/master/src/_includes/code/layout/lakes/images/lake.jpg?raw=true',
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

![Fading In Image Demo](/images/cookbook/fading-in-images.gif){:.site-mobile-screenshot}

### Desde el asset bundle

También puedes considerar el uso de assets locales para placeholders. Primero, agrega el asset al archivo `pubspec.yaml` del proyecto (mira [Assets e imágenes](/docs/development/ui/assets-and-images)para mas detalles):

<!-- skip -->
```diff
 flutter:
   assets:
+    - assets/loading.gif
```

Luego, usa el constructor [`FadeInImage.assetNetwork`](https://docs.flutter.io/flutter/widgets/FadeInImage/FadeInImage.assetNetwork.html):

<!-- skip -->
```dart
FadeInImage.assetNetwork(
  placeholder: 'assets/loading.gif',
  image: 'https://github.com/flutter/website/blob/master/src/_includes/code/layout/lakes/images/lake.jpg?raw=true',
);
```

### Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Fade in images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Center(
          child: FadeInImage.assetNetwork(
            placeholder: 'assets/loading.gif',
            image:
                'https://github.com/flutter/website/blob/master/src/_includes/code/layout/lakes/images/lake.jpg?raw=true',
          ),
        ),
      ),
    );
  }
}
```

![Asset fade-in](/images/cookbook/fading-in-asset-demo.gif){:.site-mobile-screenshot}
