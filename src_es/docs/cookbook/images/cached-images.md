---
title: Trabajando con imágenes en caché
prev:
  title: Efecto 'Fade in' en imágenes con un placeholder
  path: /docs/cookbook/images/fading-in-images
next:
  title: Listas básicas
  path: /docs/cookbook/lists/basic-list
---

En algunos casos, puede ser útil almacenar en caché las imágenes a medida que se descargan de la web para que puedan usarse sin conexión. Para este propósito, emplearemos el paquete  
[`cached_network_image`]({{site.pub-pkg}}/cached_network_image)
.

Además del almacenamiento en caché, el paquete cached_image_network también admite  
placeholders e imágenes que se van desvaneciendo a medida que se cargan.

<!-- skip -->
```dart
CachedNetworkImage(
  imageUrl: 'https://picsum.photos/250?image=9',
);
```

## Agrega un placeholder

El paquete `cached_network_image` te permite usar cualquier Widget como placeholder.
En este ejemplo, mostrarás un spinner mientras se carga la imagen.

<!-- skip -->
```dart
CachedNetworkImage(
  placeholder: CircularProgressIndicator(),
  imageUrl: 'https://picsum.photos/250?image=9',
);
``` 

## Ejemplo completo

<!-- skip -->
```dart
import 'package:flutter/material.dart';
import 'package:cached_network_image/cached_network_image.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Imágenes en caché';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Center(
          child: CachedNetworkImage(
            placeholder: CircularProgressIndicator(),
            imageUrl:
                'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```