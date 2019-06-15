---
title: Flutter para desarrolladores React Native
description: Aprende como aplicar tus conocimientos como desarrollador React Native cuando construyes aplicaciones en Flutter
---

Este documento es para desarrolladores de React Native (RN) que buscan aplicar su conocimiento 
existente de RN para construir aplicaciones móviles con Flutter. Si entiendes los fundamentos del 
framework RN entonces puedes utilizar este documento como una manera de empezar a aprender el 
desarrollo en Flutter.

Este documento puede usarse como un cookbook recorriéndolo aleatoriamente y encontrando las 
preguntas que son más relevantes a tus necesidades.


## Introducción a Dart para desarrolladores de JavaScript

Al igual que React Native, Flutter utiliza vistas al estilo reactive. Sin embargo, mientras que RN 
transpila a los widgets nativos, Flutter compila siempre a código nativo. Flutter controla cada píxel 
de la pantalla, lo que evita problemas de rendimiento causados por la necesidad de un JavaScript 
bridge.

Dart es un lenguaje fácil de aprender y ofrece las siguientes características:

* Proporciona un lenguaje de programación escalable y open source para crear aplicaciones web, 
de servidor y de móvil.
* Proporciona un lenguaje orientado a objetos de herencia simple que usa una sintaxis estilo C 
que se compila AOT en nativo.
* Transcompila opcionalmente en JavaScript.
* Soporta interfaces y clases abstractas.

Algunos ejemplos de las diferencias entre JavaScript y Dart se describen a 
continuación.

### Punto de entrada

JavaScript no tiene una función de entrada predefinida: tú defines el punto de entrada.

```js
// JavaScript
function startHere() {
  // Puede ser utilizado como punto de entrada
}
```
En Dart, cada app debe tener una función main() principal que sirva como punto de 
entrada a la app.

<!-- skip -->
```dart
// Dart
main() {
}
```

Pruébalo en [DartPad]({{site.dartpad}}/0df636e00f348bdec2bc1c8ebc7daeb1).

### Imprimir en la consola

Para imprimir en la consola en Dart, usa `print`.

```js
// JavaScript
console.log("Hello world!");
```

<!-- skip -->
```dart
// Dart
print('Hello world!');
```

Pruébalo en 
[DartPad]({{site.dartpad}}/cf9e652f77636224d3e37d96dcf238e5).

### Variables

Dart es type safe —utiliza una combinación de static type comprobando y ejecutando
checks para garantizar que el valor de una variable siempre coincida con el static
type de la variable. Aunque los types son obligatorios, algunas anotaciones type son 
opcionales porque Dart realiza la inferencia del type.

#### Creando y asignando variables

En JavaScript, las variables no se pueden escribir.

En [Dart]({{site.dart-site}}/dart-2), las variables deben tener un type explícito o 
el sistema deberá deducir el type apropiado automáticamente.

```js
// JavaScript
var name = "JavaScript";
```

<!-- skip -->
```dart
// Dart
String name = 'dart'; // Explicitamente definido el type como un string.
var otherName = 'Dart'; // String inferido.
// Ambos son aceptables en Dart.
```

Pruébalo en 
[DartPad]({{site.dartpad}}/3f4625c16e05eec396d6046883739612).

Para más información, consulta [System Type 
de Dart]({{site.dart-site}}/guides/language/sound-dart).

#### Valor por defecto

En JavaScript, las variables no inicializadas son "indefinidas".

En Dart, las variables no inicializadas tienen un valor inicial `null`. Debido a que los números son 
objetos en Dart, incluso las variables no inicializadas con tipos numéricos 
tienen el valor `null`.

```js
// JavaScript
var name; // == undefined
```

<!-- skip -->
```dart
// Dart
var name; // == null
int x; // == null
```

Pruébalo en the 
[DartPad]({{site.dartpad}}/57ec21faa8b6fe2326ffd74e9781a2c7).

Para más información, consulta la documentación sobre 
[variables]({{site.dart-site}}/guides/language/language-tour#variables).

### Comprobando null o cero

En JavaScript, los valores de 1 o cualquier objeto no nulo se tratan como true.

```js
// JavaScript
var myNull = null;
if (!myNull) {
  console.log("null is treated as false");
}
var zero = 0;
if (!zero) {
  console.log("0 is treated as false");
}
```
En Dart, solo el valor booleano `true` es tratado como verdadero.

<!-- skip -->
```dart
// Dart
var myNull = null;
if (myNull == null) {
  print('use "== null" to check null');
}
var zero = 0;
if (zero == 0) {
  print('use "== 0" to check zero');
}
```

Pruébalo en 
[DartPad]({{site.dartpad}}/c85038ad677963cb6dc943eb1a0b72e6).

### Funciones

Las funciones de Dart y JavaScript son generalmente similares. La diferencia principal es 
la declaración.

```js
// JavaScript
function fn() {
  return true;
}
```

<!-- skip -->
```dart
// Dart
fn() {
  return true;
}
// también se puede escribir como
bool fn() {
  return true;
}
```

Pruébalo en 
[DartPad]({{site.dartpad}}/5454e8bfadf3000179d19b9bc6be9918).

Para más información, consulta la documentación sobre 
[funciones]({{site.dart-site}}/guides/language/language-tour#functions).

### Programación asincrónica

#### Futures

Al igual que JavaScript, Dart admite la ejecución de un solo hilo. En JavaScript, 
el objeto Promise representa la finalización (o falla) eventual de una operación 
asincrónica y su valor resultante.


Mientras que Dart usa objetos [`Future`]({{site.dart-site}}/tutorials/language/futures) para manejar esto.

```js
// JavaScript
_getIPAddress = () => {
  const url="https://httpbin.org/ip";
  return fetch(url)
    .then(response => response.json())
    .then(responseJson => {
      console.log(responseJson.origin);
    })
    .catch(error => {
      console.error(error);
    });
};
```

<!-- skip -->
```dart
// Dart
_getIPAddress() {
  final url = 'https://httpbin.org/ip';
  HttpRequest.request(url).then((value) {
      print(json.decode(value.responseText)['origin']);
  }).catchError((error) => print(error));
}
```

Pruébalo en 
[DartPad]({{site.dartpad}}/b68eb981456c5eec03daa3c05ee59486).

Para más información, consulta la documentación sobre 
[Futures]({{site.dart-site}}/tutorials/language/futures).  

#### `async` y `await`

La declaración de función `async` define una función asíncrona.  

En JavaScript, la función `async` devuelve un `Promise`. El operador `await` se utiliza 
para esperar un `Promise`.

```js
// JavaScript
async _getIPAddress() {
  const url="https://httpbin.org/ip";
  const response = await fetch(url);
  const json = await response.json();
  const data = await json.origin;
  console.log(data);
}
```

En Dart, una función `async` devuelve un `Future`, y el cuerpo de la función se programa 
para su ejecución posterior. El operador `await` se utiliza para esperar por 
un `Future`.

<!-- skip -->
```dart
// Dart
_getIPAddress() async {
  final url = 'https://httpbin.org/ip';
  var request = await HttpRequest.request(url);
  String ip = json.decode(request.responseText)['origin'];
  print(ip);
}
```

Pruébalo en 
[DartPad]({{site.dartpad}}/96e845a844d8f8d91c6f5b826ef38951).

Para más información, consulta la documentación [`async` y
 `await`]({{site.dart-site}}/guides/language/language-tour#asynchrony-support).

## Los fundamentos
### ¿Cómo creo una app Flutter?

Para crear una app usando React Native, debes ejecutar `create-react-native-app`
desde la línea de comando.

{% prettify %}
$ create-react-native-app <projectname>
{% endprettify%}

Para crear una aplicación en Flutter, haz una de las siguientes acciones:

* Usa el comando `flutter create` desde la línea de comando. Asegúrate de que el SDK de Flutter
 esté definido en tu PATH.
* Usa un IDE con los plugins de Flutter y Dart instalados.

{% prettify %}
$ flutter create <projectname>
{% endprettify%}

Para más información, consulta el tutorial [Inicia: Panorama general](/get-started/) que te 
guiará en la creación de una app contador que se ejecuta dando clic a un botón. La creación de 
un proyecto Flutter construye todos los archivos que se necesitan para ejecutar una app de 
ejemplo tanto en dispositivos Android como en iOS.

### ¿Cómo puedo ejecutar mi aplicación?

En React Native, ejecutaría `npm run` o `yarn run` desde el directorio del 
proyecto.

 Puedes ejecutar aplicaciones Flutter de un par de maneras:

 * Usa `flutter run` desde el directorio raíz del proyecto.
 * Usa la opción "run" en un IDE con los plugins de Flutter y Dart.

 Tu app se ejecuta sobre un dispositivo conectado, el simulador de iOS o el emulador de Android.

Para más información, consulta la documentación de Flutter [Iniciando](/get-started/).

### ¿Cómo importo widgets?

En React Native, necesitas importar cada componente requerido.

```js
//React Native
import React from "react";
import { StyleSheet, Text, View } from "react-native";
```

En Flutter, para usar widgets de la biblioteca Material Design, importa el paquete `material.dart`. para usar widgets estilo iOS, importa la biblioteca Cupertino. Para utilizar un conjunto de widgets más básico, importa la biblioteca Widgets. O bien, puedes escribir tu propia biblioteca de widgets e importarla.

<!-- skip -->
```dart
import 'package:flutter/material.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter/my_widgets.dart';
```
Cualquiera sea el paquete de widgets que importes, Dart solo ingresa los widgets que se usan 
en tu aplicación.

Para más información, consulta el [Catálogo de Widgets deFlutter](/docs/development/ui/widgets).

### ¿Cuál es el equivalente de la app React Native "Hello world!" en Flutter?

En React Native, la clase `HelloWorldApp` amplía `React.Component` e implementa 
el método render devolviendo un componente de vista.

```js
// React Native
import React from "react";
import { StyleSheet, Text, View } from "react-native";

export default class App extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text>Hello world!</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center"
  }
});
```

En Flutter, puedes crear una aplicación "Hello world" idéntica utilizando los widgets `Center` y `Text` de la biblioteca principal de widgets. El widget `Center` se convierte en la raíz del árbol de widgets y tiene un hijo, el widget `Text`.

<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() {
  runApp(
    Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}

```

Las siguientes imágenes muestran la interfaz de usuario de Android y iOS para la app 
básica "Hello world!" de Flutter.  

{% include android-ios-figure-pair.md image="react-native/hello-world-basic.png" alt="Hello world app" class="border" %}

Ahora que has visto la aplicación Flutter más básica, la siguiente sección muestra 
cómo aprovechar las valiosas bibliotecas de widget de Flutter para crear una 
aplicación moderna y pulida.

### ¿Cómo uso los widgets y los encajo para formar un árbol de widgets?

En Flutter, casi todo es un widget.

Los widgets son los componentes básicos de la interfaz de usuario de una aplicación. Compones 
widgets en una jerarquía, llamada árbol de widgets. Cada widget se encaja dentro de un widget 
adre y hereda propiedades de su padre. Incluso el objeto de la aplicación en sí es un widget. 
No hay un objeto de "aplicación" separado. En cambio, el widget raíz cumple 
esta función.

Un widget puede definir:

* Un elemento estructural como un botón o menú   
* Un elemento estilístico, como un font o un esquema de color  
* Un aspecto de layout como el padding o el alignment  

El siguiente ejemplo muestra la app "¡Hola, mundo!" utilizando widgets de la biblioteca 
 Material. En este ejemplo, el árbol de widgets está encajado dentro del widget raíz de 
  `MaterialApp`.


<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello world'),
        ),
      ),
    );
  }
}

```


Las siguientes imágenes muestran "¡Hola mundo!" construido a partir de widgets de Material Design. ¡Obtendrás más funcionalidad de forma gratuita que en la aplicación básica "Hello world!".

{% include android-ios-figure-pair.md image="react-native/hello-world.png" alt="Hello world app" %}

Al escribir una aplicación, usarás dos tipos de widgets: [StatelessWidget]({{site.api}}/flutter/widgets/StatelessWidget-class.html) o
 [StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html). 
 Un StatelessWidget es exactamente lo que parece: un widget sin estado. Un widget StatelessWidget 
 se crea una vez y nunca cambia su apariencia. Un elemento StatefulWidget cambia dinámicamente 
 de estado según los datos recibidos o la entrada del usuario.

La diferencia importante entre los widgets sin estado y con estado es que StatefulWidgets tiene un 
objeto de estado que almacena datos de estado y los transporta a través de reconstrucciones de árbol, 
para que no se pierda.

En aplicaciones simples o básicas, es fácil encajar widgets, pero a medida que la base de 
códigos se hace más grande y la aplicación se vuelve más compleja, debe romper widgets 
profundamente anidados en funciones que devuelven el widget o 
clases más pequeñas.  

### ¿Cómo creo componentes reutilizables?

En React Native, definirías una clase para crear un componente reutilizable y luego usarías 
métodos props para establecer o devolver propiedades y valores de los elementos 
seleccionados. En el siguiente ejemplo, la clase `CustomCard` se define y luego 
se utiliza dentro de una clase padre.

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title="Press"
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}

// Usage
<CustomCard onPress={this.onPress} index={item.key} />
```

En Flutter, define una clase para crear un widget personalizado y luego reutiliza el widget. 
También puedes definir y llamar a una función que devuelve un widget reutilizable como se 
muestra en la función `build` en el siguiente ejemplo.

{% prettify dart %}

// Flutter
class CustomCard extends StatelessWidget {
  [[highlight]]CustomCard({@required this.index, @required [[/highlight]]
     [[highlight]]this.onPress});[[/highlight]]

  final index;
  final Function onPress;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Column(
        children: <Widget>[
          Text('Card $index'),
          FlatButton(
            child: const Text('Press'),
            onPressed: this.onPress,
          ),
        ],
      )
    );
  }
}
    ...
// Usage
CustomCard(
  [[highlight]]index: index,[[/highlight]]
  [[highlight]]onPress: () { [[/highlight]]
    print('Card $index');
  },
)
    ...

{% endprettify %}

En el ejemplo anterior, el constructor de la clase `CustomCard` usa la sintaxis de llaves `{ }` para indicar los 
[parámetros opcionales]({{site.dart-site}}/guides/language/language-tour#optional-parameters) nombrados.

Para requerir estos campos, elimina las llaves del constructor o 
agrega `@required` al constructor.



Las siguientes capturas de pantalla muestran un ejemplo de la clase reutilizable CustomCard.

{% include android-ios-figure-pair.md image="react-native/custom-cards.png" alt="Custom cards" class="border" %}




## Estructura y recursos del proyecto

### ¿Dónde empiezo a escribir el código?

Comienza con el archivo `main.dart` Se autogenera cuando se crea una app
Flutter.

<!-- skip -->
```dart
// Dart
void main(){
 print("Hello, this is the main function.");
}
```

En Flutter, el archivo de punto de entrada es `’projectname’/lib/main.dart`  y 
la ejecución comienza desde la función `main`.

### ¿Cómo se estructuran los archivos en una aplicación Flutter? 

Cuando creas un nuevo proyecto en Flutter, se construye la siguiente estructura de directorios. Puedes personalizarlo más tarde, pero aquí es donde empiezas.

```
┬
└ projectname
  ┬
  ├ android      - Contiene archivos específicos de Android.
  ├ build        - Almacena archivos build de iOS y Android.
  ├ ios          - Contiene archivos específicos de iOS.
  ├ lib          - Contiene archivos de origen Dart accesibles externamente.
    ┬
    └ src        - Contiene archivos fuente adicionales.
    └ main.dart  - El punto de entrada de Flutter y el inicio de una nueva app.
                   Esto se genera automáticamente cuando creas un proyecto 
                   de Flutter.
                   Es donde empiezas a escribir tu código de Dart.
  ├ test         - Contiene archivos de prueba automatizados.
  └ pubspec.yaml - Contiene los metadatos de la aplicación Flutter.
                   Esto es equivalente al archivo package.json en React Native.
```

### ¿Dónde pongo mis recursos y activos y cómo los uso?

Un recurso o activo de Flutter es un archivo que se incluye y se implementa con su aplicación 
y es accesible en tiempo de ejecución. Las aplicaciones de Flutter pueden incluir los 
siguientes tipos de activos:
* Datos estáticos como archivos JSON  
* Archivos de configuración  
* Iconos e imágenes (JPEG, PNG, GIF, Animated GIF, WebP, Animated WebP, BMP, 
  and WBMP)

Flutter utiliza el archivo `pubspec.yaml` ubicado en la raíz de tu proyecto, para identificar 
los activos requeridos por una app.

```yaml
flutter:
  assets:
    - assets/my_icon.png
    - assets/background.png
```

La subsección de `assets` especifica los archivos que deberían incluirse con la app.
Cada asset se identifica mediante una ruta explícita relativa al archivo `pubspec.yaml`
donde se encuentra el archivo de assets. El orden en que se declaran los assets no importa. 
El directorio actual utilizado (`assets` en este caso) no importa. Sin embargo,
mientras que los assets se pueden colocar en cualquier directorio de aplicaciones, es 
una buena práctica colocarlos en el directorio de assets.

Durante un build, Flutter coloca los assets en un archivo especial llamado *asset bundle*, 
del cual las apps leen en tiempo de ejecución. *asset bundle*, qué aplicaciones se leen en 
tiempo de ejecución cuando se especifica la ruta de un asset en la sección de `pubspec.yaml`, 
el proceso build busca cualquier archivo con el mismo nombre en subdirectorios adyacentes. 
Estos archivos también se incluyen en el paquete asset bundle junto con el asset especificado. 
Flutter utiliza variantes de assets al elegir imágenes apropiadas para su aplicación. 

En React Native, debes agregar una imagen estática colocando el archivo de imagen en un 
directorio de código fuente y haciendo referencia a él.

```js
<Image source={require("./my-icon.png")} />
```

En Flutter, agrega una imagen estática a tu aplicación utilizando la clase AssetImage en el 
método de construcción de un widget.

<!-- skip -->
```dart
image: AssetImage('assets/background.png'),
```

Para más información, consulta [Recursos e 
imágenes](/docs/development/ui/assets-and-images).

### ¿Cómo puedo cargar imágenes en una red?

En React Native, debes especificar el `uri` en la prop `source` del componente `Image`
y también proporcionar el tamaño si es necesario.

En Flutter, usa el constructor `Image.network` para incluir una imagen de una URL.

<!-- skip -->
```dart
// Flutter
body: Image.network(
          'https://flutter.io/images/owl.jpg',
```

### ¿Cómo instalo paquetes y plugins de paquetes?

Flutter soporta el uso de paquetes compartidos aportados por otros desarrolladores a los ecosistemas 
Flutter y Dart. Esto te permite construir rápidamente tu aplicación sin tener que desarrollar todo 
desde cero. Los paquetes que contienen código específico de la plataforma se conocen como plugins 
de paquetes.

En React Native, usarías `yarn add {package-name}` o `npm install --save
{package-name}` para instalar paquetes desde la línea de comando.

En Flutter, instala un paquete siguiendo las siguientes instrucciones:

1. Agrega el nombre y la versión del paquete a la sección de dependencias `pubspec.yaml`.
El siguiente ejemplo muestra cómo añadir el paquete Dart `google_sign_in` al archivo
`pubspec.yaml`. Comprueba tus espacios cuando trabajes en el archivo YAML file 
porque **el espacio en blanco importa**! 

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_sign_in: ^3.0.3
```

2. Instala el paquete desde la línea de comando usando `flutter pub get`.
Si usas un IDE, este a menudo ejecuta `flutter pub get` por ti, o puede que te pida 
que lo hagas.
3. Importa el paquete en el código de tu app como se muestra a continuación:

<!-- skip -->
```dart
import 'package:flutter/cupertino.dart';
```

Para más información, consulta [Usando 
Paquetes](/docs/development/packages-and-plugins/using-packages) y 
[Desarrollando 
Paquetes y Plugins](/docs/development/packages-and-plugins/developing-packages).

Puedes encontrar muchos paquetes compartidos por los desarrolladores de Flutter en la sección 
[Paquetes Flutter]({{site.pub}}/flutter/) de 
[pub.dartlang.org]({{site.pub}}/).

## Flutter widgets

En Flutter, construyes tu interfaz de usuario a partir de widgets que describen cómo debería ser su 
apariencia dada su configuración y estado actual.

Los widgets a menudo se componen de muchos widgets pequeños, de un solo propósito que 
están anidados para producir efectos poderosos. Por ejemplo, el widget Container 
consta de varios widgets responsables del layout, el pintado, el posicionamiento 
y el dimensionado. Específicamente, el widget `Container` incluye los widgets 
`LimitedBox`, `ConstrainedBox`, `Align`, `Padding`, `DecoratedBox`, y `Transform`. 
En lugar de subclasificar `Container` para producir un efecto personalizado, puedes 
componer estos y otros widgets simples de formas nuevas y únicas.

El widget `Center` es otro ejemplo de cómo puedes controlar el layout. Para centrar un 
widget, envuélvelo en un widget `Center` y luego usa widgets de layout para alinear, filas, 
columnas y cuadrículas. Estos widgets de layout no tienen una representación visual propia. 
Al contrario, su único propósito es controlar algún aspecto del layout de otro widget. 
Para entender por qué un widget se renderiza de cierta manera, a menudo es útil inspeccionar 
los widgets vecinos.

Para más información, consulta la [Descripción técnica general 
de Flutter](/docs/resources/technical-overview).

Para más información sobre los widgets principales del paquete Widgets, consulta
[Widgets Básicos de Flutter](/docs/development/ui/widgets/basics), 
el [Catálogo de Widgets de Flutter](/docs/development/ui/widgets), o 
el [Índice de Widgets de Flutter](/docs/reference/widgets).

## Vistas

### ¿Cuál es el equivalente del contenedor `View`?

En React Native, `View` es un contenedor que soporta el layout con `Flexbox`, estilo, 
manejo táctil y controles de accesibilidad.

En Flutter, puedes utilizar los widgets de la biblioteca Widgets, tales como 
[Container]({{site.api}}/flutter/widgets/Container-class.html), 
[Column]({{site.api}}/flutter/widgets/Column-class.html), 
[Row]({{site.api}}/flutter/widgets/Row-class.html), y 
[Center]({{site.api}}/flutter/widgets/Center-class.html).

Para más información, consulta el catálogo de [Widgets de Layout](/docs/development/ui/widgets/layout).

### ¿Cuál es el equivalente de `FlatList` o `SectionList`?

Una `List` es una lista desplegable de componentes dispuestos verticalmente.

En React Native, `FlatList` o `SectionList` se utilizan para renderizar listas 
simples o seccionadas.

```js
// React Native
<FlatList
  data={[ ... ]}
  renderItem={({ item }) => <Text>{item.key}</Text>}
/>
```

[`ListView`]({{site.api}}/flutter/widgets/ListView-class.html) es 
el widget de scrolling más utilizado de Flutter. El constructor por defecto 
toma una lista explícita de hijos. 
[`ListView`]({{site.api}}/flutter/widgets/ListView-class.html) es 
más apropiado para un pequeño número de widgets. Para una lista grande o 
infinita, usa `ListView.builder`, que construye sus hijos por demanda y sólo 
construye aquellos que son visibles.


<!-- skip -->
```dart
// Flutter
var data = [ ... ];
ListView.builder(
  itemCount: data.length,
  itemBuilder: (context, int index) {
    return Text(
      data[index],
    );
  },
)
```

{% include android-ios-figure-pair.md image="react-native/flatlist.gif" alt="Flat list" class="border" %}


Para obtener más información sobre cómo implementar una lista de desplazamiento 
infinito, consulta la sección 
[Escribe tu primera app Flutter, 
Part 1]({{site.codelabs}}/codelabs/first-flutter-app-pt1) en codelab.

### ¿Cómo uso un Canvas para dibujar o pintar?

En React Native, los componentes canvas no están presentes, por lo que se utilizan bibliotecas de terceros como `react-native-canvas`.

```js
// React Native
handleCanvas = canvas => {
  const ctx = canvas.getContext("2d");
  ctx.fillStyle = "skyblue";
  ctx.beginPath();
  ctx.arc(75, 75, 50, 0, 2 * Math.PI);
  ctx.fillRect(150, 100, 300, 300);
  ctx.stroke();
};

render() {
  return (
    <View>
      <Canvas ref={this.handleCanvas} />
    </View>
  );
}
```
En Flutter, puedes usar 
[`CustomPaint`]({{site.api}}/flutter/widgets/CustomPaint-class.html) y 
las clases [`CustomPainter`]({{site.api}}/flutter/rendering/CustomPainter-class.html) 
para dibujar en el canvas.

El siguiente ejemplo muestra cómo dibujar durante la fase de pintado usando el widget 
`CustomPaint`. Implementa la clase abstracta, CustomPainter, y la pasa a la 
propiedad del painter CustomPaint. Las subclases de CustomPaint deben implementar 
los métodos `paint` y `shouldRepaint`.

<!-- skip -->
```dart
// Flutter
class MyCanvasPainter extends CustomPainter {

  @override
  void paint(Canvas canvas, Size size) {
    Paint paint = Paint();
    paint.color = Colors.amber;
    canvas.drawCircle(Offset(100.0, 200.0), 40.0, paint);
    Paint paintRect = Paint();
    paintRect.color = Colors.lightBlue;
    Rect rect = Rect.fromPoints(Offset(150.0, 300.0), Offset(300.0, 400.0));
    canvas.drawRect(rect, paintRect);
  }

  bool shouldRepaint(MyCanvasPainter oldDelegate) => false;
  bool shouldRebuildSemantics(MyCanvasPainter oldDelegate) => false;
}
class _MyCanvasState extends State<MyCanvas> {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: CustomPaint(
        painter: MyCanvasPainter(),
      ),
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/canvas.png" alt="Canvas" class="border" %}

## Layouts

### ¿Cómo utilizo los widgets para definir las propiedades del layout?

En React Native, la mayor parte del layout se puede hacer con las props 
que se pasan a un componente específico. Por ejemplo, puede utilizar el 
`style` en el componente `View` para especificar las propiedades 
flexbox. Para ordenar los componentes en una columna, se deben especificar propiedades como: 
`flexDirection: “column”`.

```js
// React Native
<View
  style={%raw%}{{
    flex: 1,
    flexDirection: "column",
    justifyContent: "space-between",
    alignItems: "center"
  }}{%endraw%}
>
```

En Flutter, el layout se define principalmente por widgets específicamente diseñados para 
proporcionar layout, combinado con widgets de control y sus propiedades style.

Por ejemplo, los widgets 
[Column]({{site.api}}/flutter/widgets/Column-class.html) y 
[Row]({{site.api}}/flutter/widgets/Row-class.html) toman 
un array de hijos y los alinean vertical y horizontalmente respectivamente. Un widget 
[Container]({{site.api}}/flutter/widgets/Container-class.html) toma una
combinación de propiedades de layout y estilo, y un widget 
[`Center`]({{site.api}}/flutter/widgets/Center-class.html) 
que centra sus widgets hijos.

<!-- skip -->
```dart
// Flutter
Center(
  child: Column(
    children: <Widget>[
      Container(
        color: Colors.red,
        width: 100.0,
        height: 100.0,
      ),
      Container(
        color: Colors.blue,
        width: 100.0,
        height: 100.0,
      ),
      Container(
        color: Colors.green,
        width: 100.0,
        height: 100.0,
      ),
    ],
  ),
)
```

Flutter proporciona una variedad de widgets de layout en su biblioteca principal de widgets. 
Por ejemplo, [`Padding`]({{site.api}}/flutter/widgets/Padding-class.html), 
[`Align`]({{site.api}}/flutter/widgets/Align-class.html), y 
[`Stack`]({{site.api}}/flutter/widgets/Stack-class.html).

Para obtener una lista completa, consulta [Layout Widgets](/docs/development/ui/widgets/layout).

{% include android-ios-figure-pair.md image="react-native/basic-layout.gif" alt="Layout" class="border" %}

### ¿Cómo puedo crear capas de widgets?

En React Native, los componentes se pueden poner en capas utilizando el posicionamiento "absoluto".

Flutter usa el widget 
[`Stack`]({{site.api}}/flutter/widgets/Stack-class.html) 
para organizar los widgets hijos en capas. Los widgets pueden superponerse 
total o parcialmente al widget base.

El widget `Stack` posiciona a sus hijos con relación a los bordes de la caja. Este
es útil si simplemente deseas superponer varios widgets hijo.

<!-- skip -->
```dart
// Flutter
Stack(
  alignment: const Alignment(0.6, 0.6),
  children: <Widget>[
    CircleAvatar(
      backgroundImage: NetworkImage(
        "https://avatars3.githubusercontent.com/u/14101776?v=4"),
    ),
    Container(
      decoration: BoxDecoration(
          color: Colors.black45,
      ),
      child: Text('Flutter'),
    ),
  ],
)
```

El ejemplo anterior utiliza `Stack` para superponer un Contenedor (que muestra su 
`Text` sobre un fondo negro translúcido) encima de `CircleAvatar`. 
El Stack desplaza el texto usando la propiedad de alineación y las coordenadas de Alineación.

{% include android-ios-figure-pair.md image="react-native/stack.png" alt="Stack" class="border" %}

Para más información, 
consulta la documentación de la clase 
[Stack]({{site.api}}/flutter/widgets/Stack-class.html).

## Estilo

### ¿Cómo le doy estilo a mi componente?

En React Native, el estilo en línea y `stylesheets.create` se utilizan 
para modelar componentes.

```js
// React Native
<View style={styles.container}>
  <Text style={%raw%}{{ fontSize: 32, color: "cyan", fontWeight: "600" }}{%endraw%}>
    This is a sample text
  </Text>
</View>

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center"
  }
});
```

En Flutter, un widget `Text` puede tomar una clase `TextStyle` por su propiedad de estilo. 
Si deseas utilizar el mismo estilo de texto en varios lugares, puedes crear una 
clase [`TextStyle`]({{site.api}}/flutter/dart-ui/TextStyle-class.html) y 
utilizarla para varios widgets `Text`.

<!-- skip -->
```dart
// Flutter
var textStyle = TextStyle(fontSize: 32.0, color: Colors.cyan, fontWeight:
   FontWeight.w600);
	...
Center(
  child: Column(
    children: <Widget>[
      Text(
        'Sample text',
        style: textStyle,
      ),
      Padding(
        padding: EdgeInsets.all(20.0),
        child: Icon(Icons.lightbulb_outline,
          size: 48.0, color: Colors.redAccent)
      ),
    ],
  ),
)
```
{% include android-ios-figure-pair.md image="react-native/flutterstyling.gif" alt="Styling" class="border" %}

### ¿Cómo uso `Iconos` y `Colores`?

React Native no incluye soporte para iconos, por lo que se utilizan bibliotecas de terceros.

En Flutter, la importación de la biblioteca Material también arrastra 
el rico conjunto de [iconos]({{site.api}}/flutter/material/Icons-class.html) 
y [colores]({{site.api}}/flutter/material/Colors-class.html) de Material.

<!-- skip -->
```dart
Icon(Icons.lightbulb_outline, color: Colors.redAccent)
```

Cuando utilices la clase `Iconos`, asegúrate de establecer `uses-material-design: true` 
en el archivo `pubspec.yaml` del proyecto. Esto asegura que la fuente `MaterialIcons`, 
que muestra los iconos, está incluida en tu aplicación.
{% prettify dart %}
name: my_awesome_application
flutter: [[highlight]]uses-material-design: true[[/highlight]]
{% endprettify %}

El paquete [Cupertino (iOS-style)](/docs/development/ui/widgets/cupertino) de Flutter proporciona 
widgets de alta fidelidad para el lenguaje de diseño actual de iOS. Para utilizar la fuente 
`CupertinoIcons`, añade una dependencia para `cupertino_icons` en tu archivo del proyecto `pubspec.yaml`.

```yaml
name: my_awesome_application
dependencies:
  cupertino_icons: ^0.1.0
```

Para personalizar globalmente los colores y estilos de los componentes, usa `ThemeData` 
para especificar los colores predeterminados para varios aspectos del theme. 
Establece la propiedad del theme en `MaterialApp` en el objeto `ThemeData`. La clase 
[`Colors`]({{site.api}}/flutter/material/Colors-class.html) 
proporciona los colores de la [paleta de 
colores]({{site.material}}/guidelines/style/color.html) de Material Design.

 El siguiente ejemplo establece la muestra primaria en `azul` y la selección de 
 texto en `rojo`.

<!-- skip -->
{% prettify dart %}
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        [[highlight]]primarySwatch: Colors.blue,[[/highlight]]
        [[highlight]]textSelectionColor: Colors.red[[/highlight]]
      ),
      home: SampleAppPage(),
    );
  }
}
{% endprettify %}

### ¿Cómo añado themes de estilo?

En React Native, se definen themes comunes para componentes en hojas de estilo 
y luego se utiliza en los componentes.

En Flutter, crea un estilo uniforme para casi todo definiendo el estilo en 
la clase 
[`ThemeData`]({{site.api}}/flutter/material/ThemeData-class.html) 
y pasándolo a la propiedad theme en el 
widget
 [`MaterialApp`]({{site.api}}/flutter/material/MaterialApp-class.html).

<!-- skip -->
```dart
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        primaryColor: Colors.cyan,
        brightness: Brightness.dark,
      ),
      home: StylingPage(),
    );
  }
```

Un `tema` puede aplicarse incluso sin usar el widget `MaterialApp`. El 
[`Theme`]({{site.api}}/flutter/material/Theme-class.html) 
toma un widget `ThemeData` en su parámetro `data` y aplica el 
`ThemeData` a todos sus widgets hijos.

<!-- skip -->
```Dart
 @override
  Widget build(BuildContext context) {
    return Theme(
      data: ThemeData(
        primaryColor: Colors.cyan,
        brightness: brightness,
      ),
      child: Scaffold(
         backgroundColor: Theme.of(context).primaryColor,
              ...
              ...
      ),
    );
  }
```

## Gestión de Estado

State es la información que se puede leer sincrónicamente cuando se construye un widget o 
información que puede cambiar durante la vida útil de un widget. Para manejar el estado de 
la app en Flutter, usa un
 [StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html) acoplado 
 con un objeto State.

### El StatelessWidget

Un `StatelessWidget` en Flutter es un widget que no requiere un cambio de estado 
no tiene estado interno a gestionar.

Los widgets sin estado son útiles cuando la parte de la interfaz de usuario que estás 
describiendo no depende de nada más que de la información de configuración del 
propio objeto y del 
[`BuildContext`]({{site.api}}/flutter/widgets/BuildContext-class.html) 
en el cual se eleva el widget.

[AboutDialog]({{site.api}}/flutter/material/AboutDialog-class.html), 
[CircleAvatar]({{site.api}}/flutter/material/CircleAvatar-class.html), 
y [Text]({{site.api}}/flutter/widgets/Text-class.html) 
son ejemplos de widgets sin estado que subclasifican el 
[StatelessWidget]({{site.api}}/flutter/widgets/StatelessWidget-class.html).


<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() => runApp(MyStatelessWidget(text: "StatelessWidget Example to show immutable data"));

class MyStatelessWidget extends StatelessWidget {
  final String text;
  MyStatelessWidget({Key key, this.text}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        text,
        textDirection: TextDirection.ltr,
      ),
    );
  }
}
```

En el ejemplo anterior, usaste el constructor de la clase `MyStatelessWidget` 
para pasar el `text`, que está marcado como `final`. Esta clase extiende de 
`StatelessWidget`—contiene datos inmutables.

El método `build` de un widget sin estado se llama típicamente en sólo tres
 situaciones:

* Cuando el widget se inserta en un árbol
* Cuando el padre del widget cambia su configuración
* Cuando un 
[`InheritedWidget`]({{site.api}}/flutter/widgets/InheritedWidget-class.html) 
del que depende, cambia

### El StatefulWidget

Un [StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html) 
es un widget que cambia de estado. Utiliza el `setState`
para gestionar los cambios de estado de un `StatefulWidget`. Una llamada a `setState`
 le dice al framework Flutter que algo ha cambiado en un estado, que
 hace que una aplicación ejecute de nuevo el método `build` para que la aplicación pueda reflejar el cambio.

El estado es la información que se puede leer sincrónicamente cuando se construye un widget y puede 
cambiar durante la vida útil del widget. Es responsabilidad del implementador del widget asegurarse 
de que el estado sea notificado rápidamente cuando el estado cambie. Utiliza `StatefulWidget` cuando 
un widget puede cambiar dinámicamente. Por ejemplo, el estado del widget cambia escribiendo en un 
formulario o moviendo un deslizador. O bien, puede cambiar con el tiempo, tal vez una fuente de datos 
actualice la interfaz de usuario.

 [Checkbox]({{site.api}}/flutter/material/Checkbox-class.html), 
 [Radio]({{site.api}}/flutter/material/Radio-class.html), 
 [Slider]({{site.api}}/flutter/material/Slider-class.html), 
 [InkWell]({{site.api}}/flutter/material/InkWell-class.html), 
 [Form]({{site.api}}/flutter/widgets/Form-class.html), 
 y [TextField]({{site.api}}/flutter/material/TextField-class.html) 
 son ejemplos de widgets stateful, aquella subclase 
 [StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html).

El siguiente ejemplo declara un `StatefulWidget` que requiere un método `createState()`. 
Este método crea el objeto state que gestiona el estado del widget, 
`_MyStatefulWidgetState`.

<!-- skip -->
```dart
class MyStatefulWidget extends StatefulWidget {
  MyStatefulWidget({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}
```

La siguiente clase de estado, `_MyStatefulWidgetState`, implementa el método `build()` para el 
widget. Cuando el estado cambia, por ejemplo, si el usuario conmuta el botón, se llama a
`setState` con el nuevo valor de conmutación. Esto hace que el framework reconstruya este 
widget en la interfaz de usuario.

<!-- skip -->
```dart
class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  bool showtext=true;
  bool toggleState=true;
  Timer t2;

  void toggleBlinkState(){
    setState((){
      toggleState=!toggleState;
    });
    var twenty = const Duration(milliseconds: 1000);
    if(toggleState==false) {
      t2 = Timer.periodic(twenty, (Timer t) {
        toggleShowText();
      });
    } else {
      t2.cancel();
    }
  }

  void toggleShowText(){
    setState((){
      showtext=!showtext;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          children: <Widget>[
            (showtext
              ?(Text('This execution will be done before you can blink.'))
              :(Container())
            ),
            Padding(
              padding: EdgeInsets.only(top: 70.0),
              child: RaisedButton(
                onPressed: toggleBlinkState,
                child: (toggleState
                  ?( Text('Blink'))
                  :(Text('Stop Blinking'))
                )
              )
            )
          ],
        ),
      ),
    );
  }
}
```

### ¿Cuáles son las mejores prácticas de StatefulWidget y StatelessWidget?

Aquí tienes algunas cosas que debes considerar al diseñar tu widget.

#### 1. Determinar si un widget debe ser un StatefulWidget o un StatelessWidget


En Flutter, los widgets son Stateful o Stateless dependiendo de si
 dependen de un cambio de estado.

* Si un widget cambia, el usuario interactúa con él o una fuente de datos interrumpe la 
UI, entonces está es Stateful.
* Si un widget es final o inmutable, entonces es Stateless.

#### 2. Determinar qué objeto gestiona el estado del widget (para un StatefulWidget)

En Flutter, hay tres maneras principales de gestionar el estado:

* El widget gestiona su propio estado
* El widget padre gestiona el estado del widget
* Un enfoque intermedio

Al decidir qué enfoque utilizar, ten en cuenta los siguientes principios:

* Si el estado en cuestión son datos de usuario, por ejemplo, el modo marcado o no marcado 
de una casilla de verificación, o la posición de un deslizador, entonces el estado se 
gestiona mejor con el widget padre.
* Si el estado en cuestión es estético, por ejemplo, una animación, entonces el propio 
widget gestiona mejor el estado.
* En caso de duda, deja que el widget padre gestione el estado del widget hijo.

#### 3. Subclase StatefulWidget y State

La clase `MyStatefulWidget` gestiona su propio estado, extiende `StatefulWidget`, 
sobrescribe el método `createState()` para crear el objeto State, y el 
framework llama `createState()` para construir el widget. En este ejemplo, 
`createState()` crea una instancia de `_MyStatefulWidgetState`, que se implementa 
en la siguiente mejor práctica.

<!-- skip -->
```dart
class MyStatefulWidget extends StatefulWidget {
  MyStatefulWidget({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {

  @override
  Widget build(BuildContext context) {
    ...
  }
}
```

#### 4. Agregar StatefulWidget al árbol de widgets

Añade tu `StatefulWidget` personalizado al árbol de widgets en el método build de la aplicación.

<!-- skip -->
```dart
class MyStatelessWidget extends StatelessWidget {
  // Este widget es la raíz de tu aplicación.

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyStatefulWidget(title: 'State Change Demo'),
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/state-change.gif" alt="State change" class="border" %}

## Props

En React Native, la mayoría de los componentes se pueden personalizar cuando se crean con 
diferentes parámetros o propiedades, llamados `props`. Estos parámetros pueden ser usados 
en un componente hijo usando `this.props`.

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title="Press"
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}
class App extends React.Component {

  onPress = index => {
    console.log("Card ", index);
  };

  render() {
    return (
      <View>
        <FlatList
          data={[ ... ]}
          renderItem={({ item }) => (
            <CustomCard onPress={this.onPress} index={item.key} />
          )}
        />
      </View>
    );
  }
}
```

En Flutter, se asigna una variable o función local marcada como `final` con la propiedad 
recibida en el constructor parametrizado.

<!-- skip -->
```dart
// Flutter
class CustomCard extends StatelessWidget {

  CustomCard({@required this.index, @required this.onPress});
  final index;
  final Function onPress;

  @override
  Widget build(BuildContext context) {
  return Card(
    child: Column(
      children: <Widget>[
        Text('Card $index'),
        FlatButton(
          child: const Text('Press'),
          onPressed: this.onPress,
        ),
      ],
    ));
  }
}
    ...
//Usage
CustomCard(
  index: index,
  onPress: () {
    print('Card $index');
  },
)
```

{% include android-ios-figure-pair.md image="react-native/modular.png" alt="Cards" class="border" %}

## Almacenamiento local

Si no necesitas almacenar muchos datos y no requieres estructura, puedes usar 
`shared_preferences` que te permite leer y escribir parejas clave-valor 
persistentes de tipos de datos primitivos: booleans, floats, ints, longs, 
y strings.

### ¿Cómo puedo almacenar parejas persistentes de clave-valor que son globales para la aplicación?

En React Native, se utilizan las funciones `setItem` y `getItem` del 
componente `AsyncStorage` para almacenar y recuperar datos que son 
persistentes y globales para la aplicación.

<!-- skip -->
```js
// React Native
await AsyncStorage.setItem( "counterkey", json.stringify(++this.state.counter));
AsyncStorage.getItem("counterkey").then(value => {
  if (value != null) {
    this.setState({ counter: value });
  }
});
```

En Flutter, usa los plugins 
[`shared_preferences`]({{site.github}}/flutter/plugins/tree/master/packages/shared_preferences) 
para almacenar y recuperar datos key-value que son persistentes y globales 
para la app. El plugin `shared_preferences` envuelve `NSUserDefaults` en iOS 
y `SharedPreferences` en Android, proporcionando un almacenamiento persistente 
para datos simples. Para usar el plugin, agrega `shared_preferences` como una 
dependencia en el archivo `pubspec.yaml` y luego importa el paquete en tu archivo Dart.

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: ^0.4.3
```

<!-- skip -->
```dart
// Dart
import 'package:shared_preferences/shared_preferences.dart';
```

Para implementar datos persistentes, utiliza los métodos setter proporcionados por la 
clase `SharedPreferences`. Los métodos de setter están disponibles para varios 
tipos primitivos, tales como `setInt`, `setBool`, y `setString`. Para leer los 
datos, utiliza el método getter apropiado proporcionado por la clase 
`SharedPreferences`. Para cada setter hay un método getter correspondiente, 
por ejemplo, `getInt`, `getBool`, y `getString`.


<!-- skip -->
```dart
SharedPreferences prefs = await SharedPreferences.getInstance();
_counter = prefs.getInt('counter');
prefs.setInt('counter', ++_counter);
setState(() {
  _counter = _counter;
});
```


## Enrutamiento

La mayoría de las aplicaciones contienen varias pantallas para mostrar diferentes tipos de 
información. Por ejemplo, puedes tener una pantalla de producto que muestre imágenes en la 
que los usuarios puedan hacer clic sobre una imagen de producto para obtener más información 
sobre el producto en una nueva pantalla.

En Android, las nuevas pantallas son new Activities. En iOS, las nuevas pantallas son new 
ViewControllers. ¡En Flutter, las pantallas son sólo Widgets! Y para navegar a nuevas pantallas 
en Flutter, usa el widget Navigator.

### ¿Cómo navego entre pantallas?

En React Native, hay tres navegadores principales: StackNavigator, TabNavigator,
y DrawerNavigator. Cada uno proporciona una manera de configurar y definir las pantallas.

<!-- skip -->
```js
// React Native
const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: "#e91e63" } }
);
const SimpleApp = StackNavigator({
  Home: { screen: MyApp },
  stackScreen: { screen: StackScreen }
});
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

En Flutter, hay dos widgets principales utilizados para navegar entre pantallas:
* Un [Route]({{site.api}}/flutter/widgets/Route-class.html) es una abstracción 
para una pantalla o página de una app.
* Un [Navigator]({{site.api}}/flutter/widgets/Navigator-class.html) es un 
widget que gestiona rutas.

Un "Navegador" se define como un widget que gestiona un conjunto de widgets hijo con 
una disciplina stack. El navegador gestiona un stack de objetos `Route` y proporciona 
métodos para gestionar el stack, como 
[`Navigator.push`]({{site.api}}/flutter/widgets/Navigator/push.html) 
y [`Navigator.pop`]({{site.api}}/flutter/widgets/Navigator/pop.html). 
Se puede especificar una lista de rutas en el widget 
[`MaterialApp`]({{site.api}}/flutter/material/MaterialApp-class.html), 
o se pueden construir sobre la marcha, por ejemplo, en animaciones de héroes. 
El siguiente ejemplo especifica rutas con nombre en el widget `MaterialApp`.

<!-- skip -->
```dart
// Flutter
class NavigationApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
            ...
      routes: <String, WidgetBuilder>{
        '/a': (BuildContext context) => usualNavscreen(),
        '/b': (BuildContext context) => drawerNavscreen(),
      }
            ...
  );
  }
}
```

Para navegar a una ruta con nombre, se utiliza el método 
[of]({{site.api}}/flutter/widgets/Navigator/of.html) 
del widget `Navigator` para especificar el `BuildContext` 
(un manejador para la ubicación de un widget en el directorio
árbol de widgets). El nombre de la ruta se pasa a la función `pushNamed` para
navegar hasta la ruta especificada.

<!-- skip -->
```dart
Navigator.of(context).pushNamed('/a');
```

También puedes usar el método push de `Navigator` que agrega la 
[`route`]({{site.api}}/flutter/widgets/Route-class.html) 
dada a la historia del navegador que delimita más estrechamente el 
[`context`]({{site.api}}/flutter/widgets/BuildContext-class.html) 
dado, y las transiciones hacia él. En el siguiente ejemplo, el widget 
[`MaterialPageRoute`]({{site.api}}/flutter/material/MaterialPageRoute-class.html)
es una ruta modal que sustituye toda la pantalla por una transición adaptada a 
la plataforma. Toma un 
[`WidgetBuilder`]({{site.api}}/flutter/widgets/WidgetBuilder.html) 
como un parámetro requerido.

<!-- skip -->
```dart
Navigator.push(context, MaterialPageRoute(builder: (BuildContext context)
 => UsualNavscreen()));
```

### ¿Cómo se utiliza la navegación por tab y drawer?

En las aplicaciones de Material Design, hay dos opciones principales para la navegación Flutter: 
tabs y drawers. Cuando no hay espacio suficiente para sostener tabs, los drawers
son una buena alternativa.


#### Navegación por Tab

En React Native, `createBottomTabNavigator` y `TabNavigation` se utilizan para
mostrar tabs  y para navegación por tab.

```js
// React Native
import { createBottomTabNavigator } from 'react-navigation';

const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: "#e91e63" } }
);
```

Flutter proporciona varios widgets especializados para la navegación por drawer y tab:
* [TabController]({{site.api}}/flutter/material/TabController-class.html)&mdash;Coordina 
la selección de pestañas entre una TabBar y una TabBarView.  
* [TabBar]({{site.api}}/flutter/material/TabBar-class.html)&mdash;Muestra una fila 
horizontal de pestañas.  
* [Tab]({{site.api}}/flutter/material/Tab-class.html)&mdash;Crea una pestaña TabBar de 
material design.  
* [TabBarView]({{site.api}}/flutter/material/TabBarView-class.html)&mdash;Muestra el 
widget que corresponde a la pestaña actualmente seleccionada.


<!-- skip -->
```dart
// Flutter
TabController controller=TabController(length: 2, vsync: this);

TabBar(
  tabs: <Tab>[
    Tab(icon: Icon(Icons.person),),
    Tab(icon: Icon(Icons.email),),
  ],
  controller: controller,
),

```


Se necesita un `TabController` para coordinar la selección de tabs entre un `TabBar` y un 
`TabBarView`. El argumento length del constructor `TabController` es el número total de tabs. 
Se requiere un `TickerProvider` para activar la notificación siempre que un 
frame active un cambio de estado. El `TickerProvider` es `vsync`. Pasa el argumento 
`vsync: this` al constructor `TabController` cada vez que crees 
un nuevo `TabController`.

El [TickerProvider]({{site.api}}/flutter/scheduler/TickerProvider-class.html) 
es una interfaz implementada por clases que pueden despachar objetos 
[`Ticker`]({{site.api}}/flutter/scheduler/Ticker-class.html). 
Los Tickers pueden ser utilizados por cualquier objeto que deba ser notificado 
siempre que se active un frame, pero se utilizan más comúnmente de forma indirecta a través de un 
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html). 
Los `AnimationControllers` requieren de un `TickerProvider` para obtener su `Ticker`. 
Si estás creando un AnimationController desde un estado, puedes usar la clase
 [`TickerProviderStateMixin`]({{site.api}}/flutter/widgets/TickerProviderStateMixin-class.html) 
 o la clase [`SingleTickerProviderStateMixin`]({{site.api}}/flutter/widgets/SingleTickerProviderStateMixin-class.html) 
 para obtener un `TickerProvider` adecuado.

El widget [`Scaffold`]({{site.api}}/flutter/material/Scaffold-class.html) 
envuelve un nuevo widget `TabBar` y crea dos tabs.
El widget `TabBarView` se pasa como el parámetro `body` del widget `Scaffold`. 
Todas las pantallas correspondientes al widget `TabBar` son hijas del widget `TabBarView` 
junto con el mismo `TabController`.


<!-- skip -->
```dart
// Flutter

class _NavigationHomePageState extends State<NavigationHomePage> with SingleTickerProviderStateMixin {
  TabController controller=TabController(length: 2, vsync: this);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      bottomNavigationBar: Material (
        child: TabBar(
          tabs: <Tab> [
            Tab(icon: Icon(Icons.person),)
            Tab(icon: Icon(Icons.email),),
          ],
          controller: controller,
        ),
        color: Colors.blue,
      ),
      body: TabBarView(
        children: <Widget> [
          home.homeScreen(),
          tabScreen.tabScreen()
        ],
        controller: controller,
      )
    );
  }
}
```

#### Navegación por Drawer 

En React Native, importa los paquetes de navegación react necesarios y luego utiliza 
"createDrawerNavigator" y "DrawerNavigation".

```js
// React Native
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

En Flutter, podemos usar el widget `Drawer` en combinación con un `Scaffold` para crear un layout 
con un drawer de Material Design. Para añadir un `Drawer` a una aplicación, envuélvelo en un 
widget `Scaffold`. El widget `Scaffold` proporciona una estructura visual consistente a las 
aplicaciones que siguen las pautas de [Material Design](https://material.io/design/). 
También soporta componentes especiales de Material Design, tales como `Drawers`, 
`AppBars`, y `SnackBars`.

El widget `Drawer` es un panel de Material Design que se desliza horizontalmente desde 
el borde de un `Scaffold` para mostrar enlaces de navegación en una aplicación. Puedes 
proporcionar un widget 
[`Button`]({{site.api}}/flutter/material/RaisedButton-class.html), un widget 
[`Text`]({{site.api}}/flutter/widgets/Text-class.html), o una lista de elementos 
que se mostrarán como el hijo del Widget `Drawer`. En el siguiente ejemplo, el widget 
[`ListTile`]({{site.api}}/flutter/material/ListTile-class.html) 
proporciona la navegación con tap.

<!-- skip -->
```dart
// Flutter
Drawer(
  child:ListTile(
    leading: Icon(Icons.change_history),
    title: Text('Screen2'),
    onTap: () {
      Navigator.of(context).pushNamed("/b");
    },
  ),
  elevation: 20.0,
),
```

El widget `Scaffold` también incluye un widget `AppBar` que muestra automáticamente un 
IconButton apropiado para mostrar el `Drawer` cuando un Drawer está disponible en el 
`Scaffold`. El `Scaffold` maneja automáticamente el gesto de edge-swipe 
para mostrar el `Drawer`.

<!-- skip -->
```dart
// Flutter
@override
Widget build(BuildContext context) {
  return Scaffold(
    drawer: Drawer(
      child: ListTile(
        leading: Icon(Icons.change_history),
        title: Text('Screen2'),
        onTap: () {
          Navigator.of(context).pushNamed("/b");
        },
      ),
      elevation: 20.0,
    ),
    appBar: AppBar(
      title: Text("Home"),
    ),
    body: Container(),
  );
}
```

{% include android-ios-figure-pair.md image="react-native/navigation.gif" alt="Navigation" class="border" %}

## Detección de gestos y manejo de eventos táctiles

Para escuchar y responder a los gestos, Flutter soporta taps, drags, and 
scaling. El sistema de gestos en Flutter tiene dos capas separadas. 
La primera capa incluye eventos de puntero en bruto, que describen la 
ubicación y el movimiento de los punteros (como los movimientos touches, mice, 
y styli) a través de la pantalla. La segunda capa incluye gestos, que describen acciones 
semánticas que consisten en uno o más movimientos del puntero.

### ¿Cómo puedo añadir un clic o pulsar listeners a un widget?

En React Native, los listeners se añaden a los componentes usando `PanResponder` 
o los componentes `Touchable`.

```js
// React Native
<TouchableOpacity
  onPress={() => {
    console.log("Press");
  }}
  onLongPress={() => {
    console.log("Long Press");
  }}
>
  <Text>Tap or Long Press</Text>
</TouchableOpacity>
```

Para gestos más complejos y combinar varios toques en un solo gesto, 
se utiliza 
[`PanResponder`](https://facebook.github.io/react-native/docs/panresponder.html).

```js
// React Native
class App extends Component {

  componentWillMount() {
    this._panResponder = PanResponder.create({
      onMoveShouldSetPanResponder: (event, gestureState) =>
        !!getDirection(gestureState),
      onPanResponderMove: (event, gestureState) => true,
      onPanResponderRelease: (event, gestureState) => {
        const drag = getDirection(gestureState);
      },
      onPanResponderTerminationRequest: (event, gestureState) => true
    });
  }

  render() {
    return (
      <View style={styles.container} {...this._panResponder.panHandlers}>
        <View style={styles.center}>
          <Text>Swipe Horizontally or Vertically</Text>
        </View>
      </View>
    );
  }
}
```

En Flutter, para añadir un clic (o pulsar) en un listener a un widget, utiliza un 
botón o un widget táctil que tenga un campo `onPress: field`. O bien, agrega 
detección de gestos a cualquier widget envolviéndolo en un 
[`Detector de gestos`]({{site.api}}/flutter/widgets/GestureDetector-class.html).

<!-- skip -->
```dart
// Flutter
GestureDetector(
  child: Scaffold(
    appBar: AppBar(
      title: Text("Gestures"),
    ),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Text('Tap, Long Press, Swipe Horizontally or Vertically '),
        ],
      )
    ),
  ),
  onTap: () {
    print('Tapped');
  },
  onLongPress: () {
    print('Long Pressed');
  },
  onVerticalDragEnd: (DragEndDetails value) {
    print('Swiped Vertically');
  },
  onHorizontalDragEnd: (DragEndDetails value) {
    print('Swiped Horizontally');
  },
);
```
Para obtener más información, incluida una lista de callbacks Flutter `GestureDetector`,
 consulta la 
 [Clase 
 GestureDetector]({{site.api}}/flutter/widgets/GestureDetector-class.html#Properties).

{% include android-ios-figure-pair.md image="react-native/flutter-gestures.gif" alt="Gestures" class="border" %}

## Hacer peticiones de red HTTP

Obtener datos de Internet es común para la mayoría de las aplicaciones. Y en Flutter, 
el paquete `http` proporciona la forma más sencilla de obtener datos de Internet.

### ¿Cómo obtengo datos de llamadas a la API?

React Native proporciona la API de obtención de datos para la conexión en red: tú haces 
una petición de búsqueda de datos y luego recibes la respuesta para obtener los datos.

```js
// React Native
_getIPAddress = () => {
  fetch("https://httpbin.org/ip")
    .then(response => response.json())
    .then(responseJson => {
      this.setState({ _ipAddress: responseJson.origin });
    })
    .catch(error => {
      console.error(error);
    });
};
```

Flutter usa el paquete `http`. Para instalar el paquete `http`, añádelo a la sección 
de dependencias de nuestro pubspec.yaml.

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: <latest_version>
```

Flutter utiliza el cliente de soporte del core de HTTP 
[`dart:io`]({{site.api}}/flutter/dart-io/dart-io-library.html). 
Para crear un Cliente HTTP, importa `dart:io`..

<!-- skip -->
```dart
import 'dart:io';
```

El cliente soporta las siguientes operaciones HTTP: GET, POST, PUT y DELETE.

<!-- skip -->
```dart
// Flutter
final url = Uri.https('httpbin.org', 'ip');
final httpClient = HttpClient();
_getIPAddress() async {
  var request = await httpClient.getUrl(url);
  var response = await request.close();
  var responseBody = await response.transform(utf8.decoder).join();
  String ip = json.decode(responseBody)['origin'];
  setState(() {
    _ipAddress = ip;
  });
}
```

{% include android-ios-figure-pair.md image="react-native/api-calls.gif" alt="API calls" class="border" %}

## Form input

Los campos de texto permiten a los usuarios escribir texto en su aplicación para que puedan ser 
utilizados para cuando se guarda el formulario, aplicaciones de mensajería, experiencias de 
búsqueda y mucho más. Flutter proporciona dos widgets principales de campos de texto: 
[TextField]({{site.api}}/flutter/material/TextField-class.html) y 
[TextFormField]({{site.api}}/flutter/material/TextFormField-class.html).

### ¿Cómo utilizo los widgets de los campos de texto?

En React Native, para ingresar texto se usa un componente `TextInput` para mostrar un 
cuadro de entrada de texto y luego se usa callback para almacenar el valor en una variable.

```js
// React Native
<TextInput
  placeholder="Enter your Password"
  onChangeText={password => this.setState({ password })}
 />
<Button title="Submit" onPress={this.validate} />
```

En Flutter, utiliza la clase 
[`TextEditingController`]({{site.api}}/flutter/widgets/TextEditingController-class.html) 
para administrar un widget `TextField`. Cada vez que se modifica el campo de texto, el 
controlador notifica a sus listeners.

Los listeners leen el texto y las propiedades de selección para saber lo que el usuario 
escribió en el campo. Puedes acceder al texto en `TextField` por la propiedad `text` 
del controlador.

<!-- skip -->
```dart
// Flutter
final TextEditingController _controller = TextEditingController();
      ...
TextField(
  controller: _controller,
  decoration: InputDecoration(
    hintText: 'Type something', labelText: "Text Field "
  ),
),
RaisedButton(
  child: Text('Submit'),
  onPressed: () {
    showDialog(
      context: context,
        child: AlertDialog(
          title: Text('Alert'),
          content: Text('You typed ${_controller.text}'),
        ),
     );
   },
 ),
)
```

En este ejemplo, cuando un usuario pulsa el botón de enviar, aparece un cuadro de diálogo 
de alerta con el texto actual introducido en el campo de texto. Esto se consigue mediante un widget 
[`alertDialog`]({{site.api}}/flutter/material/AlertDialog-class.html) 
que muestra el mensaje de alerta, y el texto del `TextField` que es accedido 
por la propiedad `text` de 
[TextEditingController]({{site.api}}/flutter/widgets/TextEditingController-class.html).

### ¿Cómo utilizo los widgets Form?

En Flutter, utiliza el widget 
[`Form`]({{site.api}}/flutter/widgets/Form-class.html) 
donde los widgets 
[`TextFormField`]({{site.api}}/flutter/material/TextFormField-class.html) 
junto con el botón enviar se pasan como hijos. El widget `TextFormField` 
tiene un parámetro llamado 
[`onSaved`]({{site.api}}/flutter/widgets/FormField/onSaved.html) 
que toma un callback y se ejecuta cuando se guarda la forma. Un objeto `FormState` 
se utiliza para guardar, restablecer o validar cada `FormField` que sea descendiente 
de este `Form`. Para obtener el `FormState`, puedes usar `Form.of` con un contexto cuyo 
antecesor es el Form, o pasar un GlobalKey al constructor de Form y 
llamar a GlobalKey.currentState.

<!-- skip -->
```dart
final formKey = GlobalKey<FormState>();

...

Form(
  key:formKey,
  child: Column(
    children: <Widget>[
      TextFormField(
        validator: (value) => !value.contains('@') ? 'Not a valid email.' : null,
        onSaved: (val) => _email = val,
        decoration: const InputDecoration(
          hintText: 'Enter your email',
          labelText: 'Email',
        ),
      ),
      RaisedButton(
        onPressed: _submit,
        child: Text('Login'),
      ),
    ],
  ),
)
```

El siguiente ejemplo muestra cómo `Form.save()` y `formKey` (que es un
`GlobalKey`) se utilizan para guardar la forma al enviarla.

<!-- skip -->
```dart
void _submit() {
  final form = formKey.currentState;
  if (form.validate()) {
    form.save();
    showDialog(
      context: context,
      child: AlertDialog(
        title: Text('Alert'),
        content: Text('Email: $_email, password: $_password'),
      )
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/input-fields.gif" alt="Input" class="border" %}

## Código específico de la plataforma

Cuando se crea una app multiplataforma,  deseas reutilizar la mayor cantidad de código 
posible entre plataformas. Sin embargo, pueden surgir escenarios donde tenga sentido que 
el código sea diferente dependiendo del sistema operativo. Esto requiere de una 
implementación separada al declarar una plataforma específica.

En React Native, se utilizaría la siguiente implementación:

```js
// React Native
if (Platform.OS === "ios") {
  return "iOS";
} else if (Platform.OS === "android") {
  return "android";
} else {
  return "not recognised";
}
```
En Flutter, utiliza la siguiente implementación:
<!-- skip -->
```dart
// Flutter
if (Theme.of(context).platform == TargetPlatform.iOS) {
  return "iOS";
} else if (Theme.of(context).platform == TargetPlatform.android) {
  return "android";
} else if (Theme.of(context).platform == TargetPlatform.fuchsia) {
  return "fuchsia";
} else {
  return "not recognised ";
}
```

## Debugging

Antes de ejecutar tus aplicaciones, comprueba tu código con `flutter analyze`. El analizador 
de flutter (que se envuelve alrededor de la herramienta "Dartanalyzer") examina tu código 
y te ayuda a identificar posibles problemas. Si estás usando un IDE que soporte Flutter, 
esto sucede automáticamente.

### ¿Cómo accedo al menú de desarrollador en la app?

En React Native, se puede acceder al menú de desarrollador agitando el dispositivo: ⌘D 
para el Simulador iOS o ⌘M para el emulador Android.

En Flutter, si estás utilizando un IDE, puedes utilizar las herramientas del IDE. 
Si inicias tu aplicación usando `flutter run` también puedes acceder al menú pulsando 
la `h` en la ventana del terminal, o escribiendo los siguientes shortcuts:

<div class="table-wrapper" markdown="1">
| Acción| Terminal Shortcut| Funciones y propiedades de Debug |
| :------- | :------: | :------ |
| Jerarquía de widgets de la aplicación| `w`| debugDumpApp()|
| Árbol de Renderizado de la app | `t`| debugDumpRenderTree()|
| Layers| `L`| debugDumpLayerTree()|
| Accesibilidad | `S` (orden transitoria) or<br>`U` (orden inverso de la prueba de éxito)|debugDumpSemantics()|
| Para alternar el inspector de widgets | `i` | WidgetsApp. showWidgetInspectorOverride|
| Para alternar la visualización de las líneas de construcción| `p` | debugPaintSizeEnabled|
| Para simular diferentes sistemas operativos| `o` | defaultTargetPlatform|
| Para mostrar la ventana superpuesta de rendimiento | `P` | WidgetsApp. showPerformanceOverlay|
| Para guardar una captura de pantalla de flutter. png| `s` ||
| Para salir| `q` ||
{:.table.table-striped}
</div>

### ¿Cómo realizo un hot reload?

La función de hot reload de Flutter te ayuda rápida y fácilmente a probar, crear UIs, 
añadir características y corregir errores. En lugar de recompilar tu aplicación cada 
vez que hagas un cambio, puedes recargar tu aplicación instantáneamente. La aplicación 
se actualiza para reflejar tus cambios y se conserva el estado actual de la aplicación.

En React Native, el acceso directo es ⌘R para el iOS Simulator y en los emuladores 
de Android debes pulsar R dos veces.

En Flutter, si utilizas el IDE IntelliJ o Android Studio, puedes seleccionar la opción 
Guardar todo (⌘s/ctrl-s), o puedes pulsar el botón Hot Reload en la barra de herramientas. 
Si estás ejecutando la app en la línea de comando usando `flutter run`, escribe `r` 
en la Terminal de windows. También puedes realizar un reinicio total escribiendo `R` 
en la Terminal de windows.

### ¿Qué herramientas puedo usar para depurar mi aplicación en Flutter?

Hay varias opciones y herramientas que puedes usar cuando necesites depurar tu
Flutter app.

Además del analizador de Flutter, el 
[`Dart Observatory`](https://dart-lang.github.io/observatory/) es una herramienta utilizada para 
establecer el perfil y depurar tus aplicaciones de Dart. Si iniciaste tu aplicación usando 
`flutter run` en el Terminal, puedes abrir la página web en la URL del Observatorio impresa en la 
ventana de la terminal, por ejemplo: `http://127.0.0.1:8100/`.

El Observatorio incluye apoyo para la elaboración de perfiles, el examen del heap, la observación de 
líneas de código ejecutadas, la depuración de memory leaks y la fragmentación de memoria. Para más 
información, consulta la documentación del 
[Observatorio](https://dart-lang.github.io/observatory/). El Observatorio es
incluido gratuitamente al descargar e instalar el SDK de Dart.

Si estás usando un IDE, puedes depurar tu aplicación usando el debugger del IDE.

Si utilizas IntelliJ y Android Studio, puedes utilizar el Flutter Inspector.
 El Flutter Inspector hace que sea mucho más fácil entender por qué tu aplicación está 
 renderizando de la forma en que lo hace. Te permite:
* Ver la estructura de la UI de tu aplicación como un árbol de widgets
* Selecciona un punto en tu dispositivo o simulador y encuentra el widget 
correspondiente que renderizó dichos píxeles.
* Ver las propiedades de widgets específicos
* Identificar rápidamente los problemas de layout y determinar su causa

La vista del Inspector de Flutter puede abrirse desde View > Tool Windows > Flutter 
Inspector. El contenido se muestra únicamente cuando una aplicación está en ejecución.

Para inspeccionar un widget específico, selecciona la acción **Toggle inspect mode** en la barra 
de herramientas y, a continuación, haz clic en el widget deseado desde el teléfono o el simulador 
adjunto. El widget se resalta en la UI de la app. Verás el widget en la jerarquía de widgets en 
IntelliJ y las propiedades individuales para ese widget.

Para más información, consulta 
[Depura tu app](/docs/testing/debugging).

## Animación

Una animación bien diseñada hace que la UI sea intuitiva, contribuye al aspecto de una 
aplicación pulida y mejora la experiencia del usuario. El soporte de animación de Flutter 
facilita la implementación de animaciones simples y complejas. El SDK de Flutter incluye 
muchos widgets de Material Design que incluyen efectos de movimiento estándar y puedes 
personalizar fácilmente dichos efectos para personalizar tu 
aplicación.

En React Native, las API Animated se utilizan para crear animaciones.

En Flutter, utiliza el comando 
[`Animation`]({{site.api}}/flutter/animation/Animation-class.html)
y la clase 
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html).  
La animación es una clase abstracta que entiende su valor actual y su estado (completado o 
descartado). La clase `AnimationController` te permite reproducir una animación hacia adelante 
o hacia atrás, o detener la animación y ajustarla a un valor específico para personalizar 
el movimiento.

### ¿Cómo puedo añadir una simple animación fade-in?

En el siguiente ejemplo de React Native , se crea un componente animado, "FadeInView", 
utilizando la API Animated. Se definen el estado de opacidad inicial, el estado final y 
la duración de la transición. El componente de animación se agrega dentro del componente 
Animated, el estado de opacidad FadeAnim se asigna a la opacidad del componente Text que 
deseamos animar, y luego se llama a `start()` 
para iniciar la animación.

```js
// React Native
class FadeInView extends React.Component {
  state = {
    fadeAnim: new Animated.Value(0) // Valor inicial para la opacidad: 0
  };
  componentDidMount() {
    Animated.timing(this.state.fadeAnim, {
      toValue: 1,
      duration: 10000
    }).start();
  }
  render() {
    return (
      <Animated.View style={%raw%}{{...this.props.style, opacity: this.state.fadeAnim }}{%endraw%} >
        {this.props.children}
      </Animated.View>
    );
  }
}
    ...
<FadeInView>
  <Text> Fading in </Text>
</FadeInView>
    ...
```

Para crear la misma animación en Flutter, crea un objeto 
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html)
llamado `controller` y especifica la duración. Por defecto, un `AnimationController`
produce linealmente valores que van de 0.0 a 1.0, durante una duración determinada.
 El controlador de animación genera un nuevo valor cuando el dispositivo que 
 ejecuta la aplicación está listo para mostrar un nuevo frame. Típicamente, 
 esta tasa es de alrededor de 60 valores por segundo.

Al definir un `AnimationController`, debes pasar un objeto `vsync`. 
La presencia de `vsync` evita que las animaciones fuera de la pantalla 
consuman recursos innecesarios. Puedes usar tu objeto stateful 
como `vsync añadiendo `TickerProviderStateMixin' a la definición de la clase. 
Un `AnimationController` necesita un TickerProvider, que se configura 
usando el argumento `vsync` en el constructor.

Un [`Tween`]({{site.api}}/flutter/animation/Tween-class.html) describe 
la interpolación entre un valor inicial y final o la asignación de un rango de 
entrada a un rango de salida. Para usar un objeto `Tween` con una animación, 
llama al método `animate` del objeto `Tween` y pásale el objeto `Animation` 
que deseas modificar.

Para este ejemplo, se utiliza un widget 
[`FadeTransition`]({{site.api}}/flutter/widgets/FadeTransition-class.html) 
y la propiedad `opacity` se asigna al objeto `animation`.

Para iniciar la animación, utiliza `controller.forward()`. También se pueden realizar otras 
operaciones usando el controlador `fling()` o `repeat()`. 
Para este ejemplo, el widget 
[`FlutterLogo`]({{site.api}}/flutter/material/FlutterLogo-class.html) se utiliza 
dentro del widget `FadeTransition`.

<!-- skip -->
```dart

// Flutter
import 'package:flutter/material.dart';

void main() {
  runApp(Center(child: LogoFade()));
}

class LogoFade extends StatefulWidget {
  _LogoFadeState createState() => _LogoFadeState();
}

class _LogoFadeState extends State<LogoFade> with TickerProviderStateMixin {
  Animation animation;
  AnimationController controller;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 3000), vsync: this);
    final CurvedAnimation curve =
    CurvedAnimation(parent: controller, curve: Curves.easeIn);
    animation = Tween(begin: 0.0, end: 1.0).animate(curve);
    controller.forward();
  }

  Widget build(BuildContext context) {
    return FadeTransition(
      opacity: animation,
      child: Container(
        height: 300.0,
        width: 300.0,
        child: FlutterLogo(),
      ),
    );
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}
```

{% include android-ios-figure-pair.md image="react-native/flutter-fade.gif" alt="Flutter fade" class="border" %}

### ¿Cómo añado animación de deslizamiento a las tarjetas?

En React Native, se utilizan las librerías `PanResponder` o de terceros para la animación de 
deslizamiento.

En Flutter, para añadir una animación de deslizamiento, utiliza el widget 
[`Dismissible`]({{site.api}}/flutter/widgets/Dismissible-class.html) 
y encaja los widgets hijo.

<!-- skip -->
```dart
child: Dismissible(
  key: key,
  onDismissed: (DismissDirection dir) {
    cards.removeLast();
  },
  child: Container(
    ...
  ),
),
```

{% include android-ios-figure-pair.md image="react-native/card-swipe.gif" alt="Card swipe" class="border" %}


## Componentes equivalentes de React Native y Flutter Widget

La siguiente tabla lista los componentes más comunmente utilizados de React Native 
mapeados al widget Flutter correspondiente y a las propiedades comunes del widget.

<div class="table-wrapper" markdown="1">
| Componente React Native                                                                     | Flutter Widget                                                                                             | Descripción                                                                                                                            |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| [Button](https://facebook.github.io/react-native/docs/button.html)                        | [Raised Button]({{site.api}}/flutter/material/RaisedButton-class.html)                           | Un botón básico mejorado.                                                                              |
|                                                                                           |  onPressed [requerido]                                                                                        | El callback cuando el botón es pulsado o activado de otra manera.                                                          |
|                                                                                           | Child                                                                              | La etiqueta del botón.                                                                                                      |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Button](https://facebook.github.io/react-native/docs/button.html)                        | [Flat Button]({{site.api}}/flutter/material/FlatButton-class.html)                               | Un botón plano básico.                                                                                                        |
|                                                                                           |  onPressed [requerido]                                                                                        | El callback cuando el botón es pulsado o activado de otra manera.                                                            |
|                                                                                           | Child                                                                              | La etiqueta del botón.                                                                                                      |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [ScrollView](https://facebook.github.io/react-native/docs/scrollview.html)                | [ListView]({{site.api}}/flutter/widgets/ListView-class.html)                                    | Una lista desplazable de widgets dispuestos linealmente.|
||        children                                                                              | 	( <Widget\> [ ])  Lista de widgets hijos para mostrar.
||controller |[ [Scroll Controller]({{site.api}}/flutter/widgets/ScrollController-class.html) ] Un objeto que se puede usar para controlar un widget desplazable.
||itemExtent|[ double ] Si no es nulo, obliga a los hijos a tener la extensión dada en la dirección de desplazamiento.
||scroll Direction|[ [Axis]({{site.api}}/flutter/painting/Axis-class.html) ] El eje en el que se desplaza la vista de desplazamiento.
||                                                                                                            |                                                                                                                                        |
| [FlatList](https://facebook.github.io/react-native/docs/flatlist.html)                    | [ListView. builder()]({{site.api}}/flutter/widgets/ListView/ListView.builder.html)               | El constructor de un array lineal de widgets que se crean bajo demanda.
||itemBuilder [requerido] |[[ Indexed Widget Builder]({{site.api}}/flutter/widgets/IndexedWidgetBuilder.html)] ayuda en la construcción de los hijos a petición. Esta callback se llama sólo con índices mayores o iguales a cero y menores que itemCount.
||itemCount |[ int ] mejora la capacidad del ListView para estimar la extensión máxima de desplazamiento.
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Image]({{site.api}}/flutter/widgets/Image-class.html)                         | [Image](https://facebook.github.io/react-native/docs/image.html)                                           | Un widget que muestra una imagen.                                                                                                       |
|                                                                                           |  image [requerido]                                                                                          | La imagen a mostrar.                                                                                                                  |
|                                                                                           | Image. asset                                                                                                | Se proporcionan varios constructores para las diversas formas en que se puede especificar una imagen.                                                 |
|                                                                                           | width, height, color, alignment                                                                            | El estilo y diseño para la imagen.                                                                                                         |
|                                                                                           | fit                                                                                                        | Inscripción de la imagen en el espacio asignado durante el diseño                                                                           |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Modal](https://facebook.github.io/react-native/docs/modal.html)                          | [ModalRoute]({{site.api}}/flutter/widgets/ModalRoute-class.html)                                | Una ruta que bloquea la interacción con rutas anteriores.                                                                                  |
|                                                                                           | animation                                                                                                  | La animación que dirige la transición de la ruta y la transición hacia adelante de la ruta anterior.                                          |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [Activity Indicator](https://facebook.github.io/react-native/docs/activityindicator.html) | [Circular Progress Indicator]({{site.api}}/flutter/material/CircularProgressIndicator-class.html) | Un widget que muestra el progreso a lo largo de un círculo.                                                                                           |
|                                                                                           | strokeWidth                                                                                                | El ancho de la línea utilizada para dibujar el círculo.                                                                                         |
|                                                                                           | backgroundColor                                                                                            | El color de fondo del indicador de progreso. El tema actual es `ThemeData.backgroundColor` por defecto.                                   |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [Activity Indicator](https://facebook.github.io/react-native/docs/activityindicator.html) | [Linear Progress Indicator]({{site.api}}/flutter/material/LinearProgressIndicator-class.html)     | Un widget que muestra el progreso a lo largo de un círculo.                                                                                           |
|                                                                                           | value                                                                                                      | El valor de este indicador de progreso.                                                                                                   |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Refresh Control](https://facebook.github.io/react-native/docs/refreshcontrol.html)        | [Refresh Indicator]({{site.api}}/flutter/material/RefreshIndicator-class.html)                   | Un widget que soporta el modismo de Material "deslizar para actualizar".                                                                          |
|                                                                                           | color                                                                                                      | El color principal del indicador de progreso.                                                                                             |
|                                                                                           | onRefresh                                                                                                  | Una función que se llama cuando un usuario arrastra el indicador de actualización lo suficiente como para demostrar que desea que la app se actualice.  |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Container]({{site.api}}/flutter/widgets/Container-class.html)                                  | Un widget que rodea a un widget hijo.                                                                                                                |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Column]({{site.api}}/flutter/widgets/Column-class.html)                                        | Un widget que muestra a sus hijos en una matriz vertical.                                                                                              |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Row]({{site.api}}/flutter/widgets/Row-class.html)                                              | Un widget que muestra a sus hijos en una matriz horizontal.                                                                                            |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Center]({{site.api}}/flutter/widgets/Center-class.html)                                        | Un widget que centra a su hijo dentro de sí mismo.                                                                                                       |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Padding]({{site.api}}/flutter/widgets/Padding-class.html)                                      | Un widget que inserta a su hijo utilizando el padding proporcionado.                                                                                                 |
|                                                                                           | padding [requerido]                                                                                         | [ EdgeInsets ] La cantidad de espacio para insertar al hijo.
|||
| [Touchable Opacity](https://facebook.github.io/react-native/docs/touchableopacity.html)    | [Gesture Detector]({{site.api}}/flutter/widgets/GestureDetector-class.html)                      | Un widget que detecta gestos.                                                                                                                       |
|                                                                                           | onTap                                                                                                      | Un callback cuando un toque ocurre.                                                                                                               |
|                                                                                           | onDoubleTap                                                                                                | Un callback cuando un toque ocurre en el mismo lugar dos veces seguidas.
|||
| [Text Input]({{site.api}}/flutter/services/TextInput-class.html)                | [Text Input](https://facebook.github.io/react-native/docs/textinput.html)                                   | La interfaz para el control de entrada de texto del sistema.                                                                                           |
|                                                                                           | controller                                                                                                 | [ [Text Editing Controller]({{site.api}}/flutter/widgets/TextEditingController-class.html) ] para acceder y modificar el texto.
|||
| [Text](https://facebook.github.io/react-native/docs/text.html)                          | [Text]({{site.api}}/flutter/widgets/Text-class.html)                                            | El widget Texto que muestra una cadena de texto con un único estilo.                                                                                                                                                                           |
|                                                                                         | data                                                                                                      | [ String ] El texto a mostrar.                                                                                                                                                                              |
|                                                                                         | textDirection                                                                                             | [ [Text Align]( {{site.api}}/flutter/dart-ui/TextAlign-class.html) ] La dirección en la que fluye el texto.                                                                                    |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [Switch](https://facebook.github.io/react-native/docs/switch.html)                      | [Switch]({{site.api}}/flutter/material/Switch-class.html)                                      | Un switch de material design.                                                                                                                                                                                    |
|                                                                                         | value [requerido]                                                                                          | [ boolean ] Si este switch está encendido o apagado.                                                                                                                                                                 |
|                                                                                         | onChanged [requerido]                                                                                      | [ callback ] Se llama cuando el usuario activa o desactiva el switch.                                                                                                                                               |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [Slider](https://facebook.github.io/react-native/docs/slider.html)                      | [Slider]({{site.api}}/flutter/material/Slider-class.html)                                      | Se utiliza para seleccionar entre un rango de valores.                                                                                                                                                                       |
|                                                                                         | value [requerido]                                                                                          | [ double ] El valor actual del slider.                                                                                                                                                                           |
|                                                                                         | onChanged [requerido]                                                                                      | Se llama cuando el usuario selecciona un nuevo valor para el slider.                                                                                                                                                      |
{:.table.table-striped}
</div>