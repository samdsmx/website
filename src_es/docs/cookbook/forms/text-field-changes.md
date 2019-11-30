---
title: Manejando cambios en un campo de texto
prev:
  title: Centra el foco en un Text Field
  path: /docs/cookbook/forms/focus
next:
  title: Obtener el valor de un campo de texto
  path: /docs/cookbook/forms/retrieve-input
---

En algunos casos, es útil ejecutar una función callback cada vez que cambia el texto 
en un campo de texto. Por ejemplo, es posible que desees crear una pantalla de búsqueda 
con la funcionalidad de autocompletar que actualice los 
resultados a medida que el usuario escribe.

¿Cómo puedes ejecutar una función callback cada vez que cambia el texto? 
Con Flutter, tienes dos opciones:

  1. Proporciona un callback `onChanged` a un `TextField`
  2. Usa un `TextEditingController`

## 1. Proporciona un callback `onChanged` a un `TextField`

El enfoque más simple es proporcionar un 
[`onChanged`]({{site.api}}/flutter/material/TextField/onChanged.html) 
callback a un 
[`TextField`]({{site.api}}/flutter/material/TextField-class.html). 
Siempre que el texto cambie, se invocará el callback. Una desventaja de este 
enfoque es que no funciona con Widgets `TextFormField` .

En este ejemplo, imprimiremos el valor actual del campo de texto en la consola 
cada vez que cambie el texto.

<!-- skip -->
```dart
TextField(
  onChanged: (text) {
    print("First text field: $text");
  },
);
```

## 2. Usa un `TextEditingController`

Un enfoque más poderoso, pero más elaborado, es suministrar un
[`TextEditingController`]({{site.api}}/flutter/widgets/TextEditingController-class.html)
como propiedad del 
[`controller`]({{site.api}}/flutter/material/TextField/controller.html)
de `TextField` o de un `TextFormField`.

Para ser notificado cuando el texto cambie, escucha al controlador usando 
su método 
[`addListener`]({{site.api}}/flutter/foundation/ChangeNotifier/addListener.html) usando los siguientes pasos:

  1. Crea un `TextEditingController`
  2. Conecta el `TextEditingController` a un `TextField`
  3. Crea una función para imprimir el último valor
  4. Escucha al controlador por cambios

### Crea un `TextEditingController`

Crea un `TextEditingController`:

<!-- skip -->
```dart
// Define un Widget de formulario personalizado
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

// Define una clase de estado correspondiente. Esta clase contendrá los datos relacionados
// con nuestro formulario.
class _MyCustomFormState extends State<MyCustomForm> {
  // Crea un controlador de texto. Lo usaremos para recuperar el valor actual 
  // del TextField!
  final myController = TextEditingController();

  @override
  void dispose() {
    // Limpia el controlador cuando el widget se elimine del árbol de widgets
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Lo completaremos en el siguiente paso!
  }
}
```

{{site.alert.note}}
  Recuerda llamar al método `dispose` del `TextEditingController` 
  cuando ya no sea necesario. Esto asegurará que descartemos 
  cualquier recurso utilizado por el objeto.
{{site.alert.end}}

### Conecta el `TextEditingController` a un `TextField`

Suministra el `TextEditingController` a un 
`TextField` o a un `TextFormField`. Una vez has conectado estas dos 
clases juntas, puedes empezar a escuchar los 
cambios en el campo de texto. 

<!-- skip -->
```dart
TextField(
  controller: myController,
);
```

### Crea una función para imprimir el último valor

Necesitas una función para ejecutar cada vez que cambie el texto. 
Crea un método en la clase `_MyCustomFormState` que imprima el valor actual del campo de texto.

Este método vivirá dentro de nuestra clase `_MyCustomFormState` .

<!-- skip -->
```dart
_printLatestValue() {
  print("Second text field: ${myController.text}");
}
```

### Escucha al controlador por cambios

Finalmente, tenemos que escuchar el `TextEditingController` y ejecutar el método 
`_printLatestValue()`  cada vez que el texto cambie. Utilizaremos el método 
[`addListener`]({{site.api}}/flutter/foundation/ChangeNotifier/addListener.html)  para este propósito.

Comienza a escuchar los cambios cuando se inicializa la clase 
`_MyCustomFormState` , y dejaremos de escuchar cuando se elimine 
`_MyCustomFormState` .

<!-- skip -->
```dart
class _MyCustomFormState extends State<MyCustomForm> {
  @override
  void initState() {
    super.initState();

    // Comienza a escuchar los cambios 
    myController.addListener(_printLatestValue);
  }

}
```

## Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Retrieve Text Input',
      home: MyCustomForm(),
    );
  }
}

// Define un widget de formulario personalizado
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

// Define una clase de estado correspondiente. Esta clase contendrá los datos relacionados
// con nuestro formulario.
class _MyCustomFormState extends State<MyCustomForm> {
  // Crea un controlador de texto y úsalo para recuperar el valor actual
  // del TextField.
  final myController = TextEditingController();

  @override
  void initState() {
    super.initState();

    myController.addListener(_printLatestValue);
  }

  @override
  void dispose() {
    // Limpia el controlador cuando el widget se elimine del árbol de widgets
    // Esto también elimina el listener _printLatestValue
    myController.dispose();
    super.dispose();
  }

  _printLatestValue() {
    print("Second text field: ${myController.text}");
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: <Widget>[
            TextField(
              onChanged: (text) {
                print("First text field: $text");
              },
            ),
            TextField(
              controller: myController,
            ),
          ],
        ),
      ),
    );
  }
}
```