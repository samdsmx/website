---
title: Construyendo un formulario con validaciones
prev:
  title: Trabjando con Pestañas
  path: /docs/cookbook/design/tabs
next:
  title: Crear y dar estilo a un campo de texto
  path: /docs/cookbook/forms/text-input
---

Las aplicaciones a menudo requieren que los usuarios ingresen información en un campo de texto. 
Por ejemplo, podrías estar trabajando en una aplicación que requiera que nuestros usuarios inicien 
sesión con una combinación de dirección de correo electrónico y contraseña.

Para hacer apps seguras y fáciles de usar, verifica si la información 
que el usuario ha proporcionado es válida. Si el usuario completó correctamente el formulario, 
procesa la información. Si el usuario envía información incorrecta, muestra un 
mensaje de error amistoso para informarle qué fue lo que salió 
mal.

En este ejemplo, aprende cómo agregar validación a un formulario con un solo 
campo de texto usando los siguientes pasos: 

  1. Crea una `Form` con un `GlobalKey`
  2. Añade un `TextFormField` con lógica de validación
  3. Crea un botón para validar y enviar el formulario

## 1. Crea una `Form` con un `GlobalKey`

Primero, crea un [`Form`]({{site.api}}/flutter/widgets/Form-class.html). 
El widget `Form`  actúa como un contenedor para agrupar y validar múltiples campos de 
formulario.

Cuando creas el formulario, proporciona una 
[`GlobalKey`]({{site.api}}/flutter/widgets/GlobalKey-class.html). 
Esto identificará de manera única el `Form`, 
y nos permitirá validar el formulario en un paso posterior. 

<!-- skip -->
```dart
// Define un widget de formulario personalizado
class MyCustomForm extends StatefulWidget {
  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// Define una clase de estado correspondiente. Esta clase contendrá los datos
// relacionados con el formulario.
class MyCustomFormState extends State<MyCustomForm> {
  // Crea una clave global que identificará de manera única el widget Form 
  // y nos permita validar el formulario
  //
  // Nota: Esto es un `GlobalKey<FormState>`, no un GlobalKey<MyCustomFormState>! 
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // Cree un widget Form usando el _formKey que creamos anteriormente
    return Form(
      key: _formKey,
      child: // Construiremos esto en los siguientes pasos!
    );
  }
}
```

{{site.alert.tip}}
Usar una `GlobalKey` es la manera recomendada para acceder a un formulario. Sin embargo, si tienes 
un árbol de widget más complejo, puedes usar el método 
[`Form.of`]({{site.api}}/flutter/widgets/Form/of.html) para 
acceder al formulario entre a través anidados.
{{site.alert.end}}

## 2. Agrega un `TextFormField` con lógica de validación

Aunque el `Form` está en su lugar, 
no hay una forma de que los usuarios introduzcan texto. 
Este es el trabajo de un 
[`TextFormField`]({{site.api}}/flutter/material/TextFormField-class.html).
El widget `TextFormField` dibuja un campo de texto de material design 
y puede mostrar los errores de validación cuando se producen.

Valida la entrada proporcionando una función `validator()` al 
`TextFormField`. Si la entrada del usuario no es válida, 
la función `validator` devuelve un `String` conteniendo un mensaje de error. 
Si no hay errores, la función de validación debe devolver null.

Para este ejemplo, crea un `validator` que asegure que el `TextFormField`
no esté vacío. Si esta vacio devuelve un mensaje de error amigable.

<!-- skip -->
```dart
TextFormField(
  // El validator recibe el texto que el usuario ha introducido
  validator: (value) {
    if (value.isEmpty) {
      return 'Enter some text';
    }
    return null;
  },
);
```

## 3. Crea un botón para validar y enviar el formulario

Ahora que tienes un formulario con un campo de texto, tendremos que proporcionar un botón que el 
usuario pueda pulsar para enviar la información. 

Cuando el usuario intente enviar el formulario, necesitaremos verificar si el formulario es válido. 
Si es así, muestra un mensaje de éxito. 
Si no (el campo de texto no tiene contenido) muestra el mensaje de error.

<!-- skip -->
```dart
RaisedButton(
  onPressed: () {
    // Validate devolverá true si el formulario es válido, o false si
    // el formulario no es válido.
    if (_formKey.currentState.validate()) {
      // Si el formulario es válido, muestre un snackbar. En el mundo real, a menudo
      // desea llamar a un servidor o guardar la información en una base de datos
      Scaffold
          .of(context)
          .showSnackBar(SnackBar(content: Text('Processing Data')));
    }
  },
  child: Text('Submit'),
);
```

### ¿Cómo funciona esto?

Para validar el formulario, usa el `_formKey` creado en el paso 1. Puedes usar el 
método `_formKey.currentState` para acceder a 
[`FormState`]({{site.api}}/flutter/widgets/FormState-class.html),
que Flutter crea automáticamente cuando creamos un `Form`. 

La clase `FormState` contiene el método `validate()`. Cuando se llama al método `validate()`, 
ejecutará la función `validator` para cada campo de texto en formulario. 
Si todo se ve bien, el método `validate()` devuelve `true`. Si algún campo de texto contiene errores, 
mostrará el mensaje de error para cada campo de texto no válido y devolverá 
`false`.

## Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appTitle = 'Form Validation Demo';

    return MaterialApp(
      title: appTitle,
      home: Scaffold(
        appBar: AppBar(
          title: Text(appTitle),
        ),
        body: MyCustomForm(),
      ),
    );
  }
}

// Crea un Widget Form
class MyCustomForm extends StatefulWidget {
  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// Crea una clase State correspondiente. Esta clase contendrá los datos relacionados con
// el formulario.
class MyCustomFormState extends State<MyCustomForm> {
  // Crea una clave global que identificará de manera única el widget Form
  // y nos permita validar el formulario
  //
  // Nota: Esto es un GlobalKey<FormState>, no un GlobalKey<MyCustomFormState>!
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // Crea un widget Form usando el _formKey que creamos anteriormente
    return Form(
      key: _formKey,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          TextFormField(
            validator: (value) {
              if (value.isEmpty) {
                return 'Enter some text';
              }
              return null;
            },
          ),
          Padding(
            padding: const EdgeInsets.symmetric(vertical: 16.0),
            child: RaisedButton(
              onPressed: () {
                // devolverá true si el formulario es válido, o falso si
                // el formulario no es válido.
                if (_formKey.currentState.validate()) {
                  // Si el formulario es válido, queremos mostrar un Snackbar
                  Scaffold.of(context)
                      .showSnackBar(SnackBar(content: Text('Processing Data')));
                }
              },
              child: Text('Submit'),
            ),
          ),
        ],
      ),
    );
  }
}
```

![Form Validation Demo](/images/cookbook/form-validation.gif){:.site-mobile-screenshot}