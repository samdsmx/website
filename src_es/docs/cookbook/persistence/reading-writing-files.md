---
title: Leyendo y Escribiendo Archivos
rev:
  title: Persistir datos con SQLite
  path: /docs/cookbook/persistence/sqlite
next:
  title: Almacenando datos clave-valor en disco
  path: /docs/cookbook/persistence/key-value
---

En algunos casos, puede ser útil leer y escribir archivos en el disco. 
Esto puede ser usado para conservar los datos entre las ejecuciones de la app, 
o para descargar datos de internet y guardarlos para su posterior uso offline.

Para guardar archivos en el disco, necesitas combinar el plugin
[`path_provider`]({{site.pub-pkg}}/path_provider) con 
la biblioteca [`dart:io`]({{site.api}}/flutter/dart-io/dart-io-library.html).

  
## Instrucciones

  1. Encuentra la ruta local correcta
  2. Crea una referencia a la ubicación del archivo
  3. Escriba datos en el archivo
  4. Lea datos del archivo
  
## 1. Encuentra la ruta local correcta

En este ejemplo, mostrarás un contador. Cuando el contador cambia, querrás escribir 
datos en el disco para que puedas volver a leerlos cuando se cargue la aplicación. Por 
lo tanto, debes preguntarte: ¿Dónde debería almacenar esta información?

El plugin [`path_provider`]({{site.pub-pkg}}/path_provider) 
proporciona una forma independiente de la plataforma para acceder a las ubicaciones de 
uso común en el sistema de archivos del dispositivo. El plugin actualmente admite 
el acceso a dos ubicaciones del sistema de archivos:

  * *Directorio temporal:* Un directorio temporal (caché) que el sistema puede borrar 
  en cualquier momento. En iOS, este corresponde al valor que 
  [`NSTemporaryDirectory()`](https://developer.apple.com/reference/foundation/1409211-nstemporarydirectory) 
  retorna. En Android, este es el valor que 
  [`getCacheDir()`](https://developer.android.com/reference/android/content/Context#getCacheDir()) 
  retorna.
  * *Directorio documentos:* Un directorio para que la aplicación almacene archivos 
  a los que solo puede acceder. El sistema borra el directorio solo cuando se elimina 
  la aplicación. En iOS, esto corresponde a `NSDocumentDirectory`. En Android, este 
  es el directorio `AppData`.
  
En warw caso, quieres almacenar información en el directorio de documentos. 
Puedes encontrar la ruta al directorio de documentos de esta manera:
  
<!-- skip -->
```dart
Future<String> get _localPath async {
  final directory = await getApplicationDocumentsDirectory();
  
  return directory.path;
}
```

## 2. Crea una referencia a la ubicación del archivo

Una vez que sepas dónde almacenar el archivo, tendrás que crear una referencia a la 
ubicación completa del archivo. Puedes usar la clase 
[`File`]({{site.api}}/flutter/dart-io/File-class.html) 
de la biblioteca [dart:io]({{site.api}}/flutter/dart-io/dart-io-library.html)
para lograr esto.

<!-- skip -->
```dart
Future<File> get _localFile async {
  final path = await _localPath;
  return File('$path/counter.txt');
}
```

## 3. Escribir datos en el archivo

Ahora que tienes un archivo `File` para trabajar, usalo para leer y escribir datos. 
Primero, escribe algunos datos en el archivo. Como estas trabajando con un contador, 
simplemente almacenarás el número entero como una cadena.

<!-- skip -->
```dart
Future<File> writeCounter(int counter) async {
  final file = await _localFile;
  
  // Escribir el archivo
  return file.writeAsString('$counter');
}
``` 

## 4. Leer datos del archivo

Ahora que tienes algunos datos en el disco, puedes leerlo. 
Una vez más, usa la clase `File`.

<!-- skip -->
```dart
Future<int> readCounter() async {
  try {
    final file = await _localFile;

    // Leer el archivo
    String contents = await file.readAsString();

    return int.parse(contents);
  } catch (e) {
    // Si encuentras un error, regresamos 0
    return 0;
  }
}
``` 

## Pruebas

Para probar el código que interactúa con los archivos, tendrás que simular llamadas al 
`MethodChannel`. El `MethodChannel` es la clase que usa Flutter para comunicarse con la 
plataforma anfitrion.

En estas pruebas, no puedes interactuar con el sistema de archivos en un dispositivo. 
Necesitas interactuar con el sistema de archivos de nuestro entorno de prueba.

Para simular la llamada al método, podemos proporcionar una función `setupAll` en nuestro 
archivo de prueba. Esta función se ejecutará antes de que se ejecuten las pruebas.

<!-- skip -->
```dart
setUpAll(() async {
  // Crea un directorio temporal para trabajar
  final directory = await Directory.systemTemp.createTemp();
  
  // Simula el MethodChannel para el plugin path_provider
  const MethodChannel('plugins.flutter.io/path_provider')
      .setMockMethodCallHandler((MethodCall methodCall) async {
    // Si estas obteniendo el directorio de documentos de la app, en su lugar, regresaremos
    // la ruta de el directorio temporal en el entorno de prueba.
    if (methodCall.method == 'getApplicationDocumentsDirectory') {
      return directory.path;
    }
    return null;
  });
});
``` 

## Ejemplo completo

```dart
import 'dart:async';
import 'dart:io';

import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';

void main() {
  runApp(
    MaterialApp(
      title: 'Leyendo y escribiendo archivos',
      home: FlutterDemo(storage: CounterStorage()),
    ),
  );
}

class CounterStorage {
  Future<String> get _localPath async {
    final directory = await getApplicationDocumentsDirectory();

    return directory.path;
  }

  Future<File> get _localFile async {
    final path = await _localPath;
    return File('$path/counter.txt');
  }

  Future<int> readCounter() async {
    try {
      final file = await _localFile;

      // Leer archivo
      String contents = await file.readAsString();

      return int.parse(contents);
    } catch (e) {
      // Si encuentras un error, regresamos 0
      return 0;
    }
  }

  Future<File> writeCounter(int counter) async {
    final file = await _localFile;

    // Escribir archivo
    return file.writeAsString('$counter');
  }
}

class FlutterDemo extends StatefulWidget {
  final CounterStorage storage;

  FlutterDemo({Key key, @required this.storage}) : super(key: key);

  @override
  _FlutterDemoState createState() => _FlutterDemoState();
}

class _FlutterDemoState extends State<FlutterDemo> {
  int _counter;

  @override
  void initState() {
    super.initState();
    widget.storage.readCounter().then((int value) {
      setState(() {
        _counter = value;
      });
    });
  }

  Future<File> _incrementCounter() {
    setState(() {
      _counter++;
    });

    // Escribe las variables como texto en el archivo
    return widget.storage.writeCounter(_counter);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Reading and Writing Files')),
      body: Center(
        child: Text(
          'Button tapped $_counter time${_counter == 1 ? '' : 's'}.',
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```
