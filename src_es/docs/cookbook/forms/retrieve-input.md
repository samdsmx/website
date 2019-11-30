---
title: Obtener el valor de un campo de texto
prev:
  title: Manejar los cambios en un campo de texto
  path: /docs/cookbook/forms/text-field-changes
next:
  title: Añadir Material Touch Ripples
  path: /docs/cookbook/gestures/ripples
---

En esta receta, 
aprende cómo recuperar el texto que un usuario ha escrito en un 
campo de texto usando los siguientes pasos:

  1. Crea un `TextEditingController`
  2. Proporciona el `TextEditingController` a un `TextField`
  3. Muestra el valor actual del campo de texto

## 1. Crea un `TextEditingController`

Para recuperar el texto que un usuario ha escrito en un campo de texto, crea un [`TextEditingController`]({{site.api}}/flutter/widgets/TextEditingController-class.html) y 
proporciónaselo a un `TextField` o `TextFormField`

{{site.alert.secondary}}
  **Important:** LLama a `dispose` del `TextEditingController` cuando hayas terminado de usarlo. Esto asegurará que descartemos cualquier recurso utilizado por el objeto.
{{site.alert.end}}

<!-- skip -->
```dart
// Define un widget de formulario personalizado
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

// Define la clase State correspondiente. Esta clase contendrá los datos relacionados con
// nuestro formulario.
class _MyCustomFormState extends State<MyCustomForm> {
  // Crea un controlador de texto y úsalo para recuperar el valor actual
  // del TextField.
  final myController = TextEditingController();

  @override
  void dispose() {
    // Limpia el controlador cuando el Widget se descarte
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // ¡Lo completaremos en el siguiente paso!
  }
}
```

## 2. Proporciona el `TextEditingController` a un `TextField`

Ahora que tienes un `TextEditingController`, conéctalo a un 
campo de texto usando la propiedad `controller`:

<!-- skip -->
```dart
TextField(
  controller: myController,
);
```

## 3. Muestra el valor actual del campo de texto

Después de que hayas proporcionado el `TextEditingController` al 
campo de texto, empezamos a leer valores. Usa el método [`text`]({{site.api}}/flutter/widgets/TextEditingController/text.html) 
proporcionado por el `TextEditingController` para recuperar el String de texto 
que el usuario haya ingresado en el campo de texto.

El siguiente código muestra un cuadro de diálogo de alerta con el valor 
actual del campo de texto cuando el usuario pulsa un botón de acción flotante.  

<!-- skip -->
```dart
FloatingActionButton(
  // Cuando el usuario pulsa el botón, muestra un diálogo de alerta con el
  // texto que el usuario ha ingresado en nuestro campo de texto.
  onPressed: () {
    return showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          // Recupera el texto que el usuario ha digitado utilizando nuestro
          // TextEditingController
          content: Text(myController.text),
        );
      },
    );
  },
  tooltip: 'Show me the value!',
  child: Icon(Icons.text_fields),
);
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

// Define la clase State correspondiente. Esta clase contendrá los datos relacionados con
// nuestro formulario.
class _MyCustomFormState extends State<MyCustomForm> {
  // Crea un controlador de texto y úsalo para recuperar el valor actual
  // del TextField.
  final myController = TextEditingController();

  @override
  void dispose() {
    // Limpia el controlador cuando el Widget se descarte
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          controller: myController,
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // Cuando el usuario pulsa el botón, muestra un diálogo de alerta con el
        // texto que el usuario ha digitado en nuestro campo de texto.
        onPressed: () {
          return showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                // Recupera el texto que el usuario ha digitado utilizando nuestro
                // TextEditingController
                content: Text(myController.text),
              );
            },
          );
        },
        tooltip: 'Show me the value!',
        child: Icon(Icons.text_fields),
      ),
    );
  }
}
```

![Retrieve Text Input Demo](/images/cookbook/retrieve-input.gif){:.site-mobile-screenshot}