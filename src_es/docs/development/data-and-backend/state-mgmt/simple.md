---
title: Gestión sencilla del estado de las aplicaciones
prev:
  title: Efímero vs estado de aplicación
  path: /docs/development/data-and-backend/state-mgmt/ephemeral-vs-app
next:
  title: Lista de enfoques
  path: /docs/development/data-and-backend/state-mgmt/options
---

Ahora que conoces la [programación 
declarativa de UI](/docs/development/data-and-backend/state-mgmt/declarative) y 
la diferencia entre [efímero y app 
state](/docs/development/data-and-backend/state-mgmt/ephemeral-vs-app), estás 
listo para aprender acerca de la gestión de estado de una app sencilla.

En esta página, vamos a usar el paquete `provider`. 
Si eres nuevo en Flutter y no 
tienes una razón de peso para elegir otro enfoque (Redux, Rx, hooks, etc.), 
este es probablemente el enfoque con el que deberías empezar. `Provider` es fácil 
de entender y no utiliza mucho código. También utiliza conceptos que son aplicables 
en todos los demás enfoques.

Dicho esto, si tienes una formación sólida en gestión de estados de otros 
frameworks reactivos, encontrarás paquetes y tutoriales en la 
[página siguiente](/docs/development/data-and-backend/state-mgmt/options).

## Nuestro ejemplo {% asset development/data-and-backend/state-mgmt/model-shopper-screencast alt="Un gif animado que muestra una aplicación Flutter en uso. Comienza con el usuario en una pantalla de inicio de sesión. Se conectan y se les lleva a la pantalla del catálogo, con una lista de artículos. Al hacer clic en varios elementos, y al hacerlo, los elementos se marcan como "añadidos". El usuario hace clic en un botón y es llevado a la vista del carro. Ellos ven los artículos allí. Vuelven al catálogo, y los artículos que compraron siguen mostrando "añadidos". Fin de la animación." class='site-image-right' %}

A modo de ejemplo, considera la siguiente aplicación sencilla.

La aplicación tiene tres pantallas separadas: una ventana de inicio de sesión, 
un catálogo y un carrito (representados por los widgets `MyLoginScreen`, 
`MyCatalog`, y `MyCart`, respectivamente). Podría ser una aplicación de compras, 
pero puedes imaginarte la misma estructura en una simple aplicación de redes 
sociales (reemplaza el catálogo por " muro " y el carrito por " favoritos ").

La pantalla del catálogo incluye una barra de aplicaciones personalizada (`MyAppBar`) 
y una vista de desplazamiento de muchos elementos de la lista (`MyListItems`).

Aquí está la aplicación visualizada como un árbol de widgets.

{% asset development/data-and-backend/state-mgmt/simple-widget-tree alt="A widget tree with MyApp at the top, and MyLoginScreen, MyCatalog and MyCart below it. MyLoginScreen and MyCart area leaf nodes, but MyCatalog have two children: MyAppBar and a list of MyListItems." %}

{% comment %}
  Source drawing for the png above: https://docs.google.com/drawings/d/1KXxAl_Ctxc-avhR4uE58BXBM6Tyhy0pQMCsSMFHVL_0/edit?zx=y4m1lzbhsrvx
{% endcomment %}

Así que tenemos al menos 6 subclases de `Widget`. Muchos de ellos necesitarán 
acceso a un estado que "pertenece" a otra parte. Por ejemplo, cada 
`MyListItem` podrá añadir a la carrito. También podrías querer ver 
si el artículo que está mostrando ya está en el carrito.

Esto nos lleva a nuestra primera pregunta: ¿dónde debemos poner 
el estado actual del carro? 


## Estado de elevación

En Flutter, tiene sentido mantener el estado por encima de los widgets que lo utilizan.

Por qué? En frameworks declarativos como Flutter, si quieres cambiar la interfaz 
de usuario, tienes que reconstruirla. No hay una manera fácil de tener 
`MyCart.updateWith(somethingNew)`. En otras palabras, es difícil cambiar 
imperativamente un widget desde fuera, llamando a un método en él. E incluso si 
pudieras hacer que esto funcione, estarías luchando contra el framework en 
lugar de dejar que te ayude.

<!-- skip -->
```dart
// MALO: NO HAGA ESTO
void myTapHandler() {
  var cartWidget = somehowGetMyCartWidget();
  cartWidget.updateWith(item);
}
```

Incluso si consigues que el código anterior funcione, tendrás que 
ocuparte de lo siguiente en el widget `MyCart`:

<!-- skip -->
```dart
// MALO: NO HAGA ESTO
Widget build(BuildContext context) {
  return SomeWidget(
    // The initial state of the cart.
  );
}

void updateWith(Item item) {
  // Somehow you need to change the UI from here.
}
```

Necesitarás tener en cuenta el estado actual de la interfaz de usuario y 
aplicarle los nuevos datos. Es difícil evitar los errores de esta manera.

En Flutter, construyes un nuevo widget cada vez que su contenido cambia. 
En lugar de `MyCart.updateWith(somethingNew)` (una llamada de método) 
usas `MyCart(contents)` (un constructor). Debido a que sólo puedes 
construir nuevos widgets en los métodos de construcción de sus 
padres, si quieres cambiar los `contenidos`, necesita vivir en el 
padre de `MyCart` o superior.

<?code-excerpt "state_mgmt/simple/lib/src/provider.dart (myTapHandler)"?>
```dart
// CORRECTO
void myTapHandler(BuildContext context) {
  var cartModel = somehowGetMyCartModel(context);
  cartModel.add(item);
}
```

Ahora `MyCart` tiene sólo una ruta de código para construir cualquier versión de la interfaz de usuario.

<?code-excerpt "state_mgmt/simple/lib/src/provider.dart (build)"?>
```dart
// CORRECTO
Widget build(BuildContext context) {
  var cartModel = somehowGetMyCartModel(context);
  return SomeWidget(
    // Just construct the UI once, using the current state of the cart.
    // ···
  );
}
```

En nuestro ejemplo, `contents` necesitan vivir en `MyApp`. Cada vez que cambie, 
recontruye `MyCart` desde arriba (hablaremos más de esto despues). 
Debido a esto, `MyCart` no necesita preocuparse por el ciclo de 
vida&mdash;soló declara qué mostrar para cualquier `contents` dado. Cuando eso cambia, 
el widget `MyCart` viejo desaparecer y es completamente reemplazado por uno nuevo.

{% asset development/data-and-backend/state-mgmt/simple-widget-tree-with-cart alt="Same widget tree as above, but now we show a small 'cart' badge next to MyApp, and there are two arrows here. One comes from one of the MyListItems to the 'cart', and another one goes from the 'cart' to the MyCart widget." %}

{% comment %}
  Source drawing for the png above: https://docs.google.com/drawings/d/1ErMyaX4fwfbIW9ABuPAlHELLGMsU6cdxPDFz_elsS9k/edit?zx=j42inp8903pt
{% endcomment %}

Esto es lo que queremos decir cuando decimos que los widgets son inmutables. 
No cambian&mdash;son reemplazados.

Ahora que sabemos dónde poner el estado del carro, veamos cómo 
acceder a él.

## Acceso al estado

Cuando el usuario hace clic en uno de los artículos del catálogo, 
se añade al carrito. Pero ya que el carro vive encima de `MyListItem`, 
¿cómo lo hacemos?

Una opción simple es proporcionar un callback a la que `MyListItem` 
puede llamar cuando se hace clic en el. Las funciones de Dart son 
objetos de primera clase, por lo que puedes compartirlos como quieras. 
Así, dentro de `MiCatálogo` puedes tener lo siguiente:

<?code-excerpt "state_mgmt/simple/lib/src/passing_callbacks.dart (methods)"?>
```dart
@override
Widget build(BuildContext context) {
  return SomeWidget(
    // Contruye el widget, pasando la referencia al método de superior.
    MyListItem(myTapCallback),
  );
}

void myTapCallback(Item item) {
  print('user tapped on $item');
}
```

Esto funciona bien, pero para el estado de la aplicación que necesitas modificar 
desde muchos lugares diferentes, tendrías que pasar un montón de llamadas de 
retorno&mdash;lo que envejece bastante rápido.

Afortunadamente, Flutter tiene mecanismos para que los widgets proporcionen datos y 
servicios a sus descendientes (en otras palabras, no sólo a sus hijos, sino a 
cualquier widget que se encuentre debajo de ellos). Como es de esperar de Flutter, 
donde _Everything es un Widget™_, estos mecanismos son sólo tipos especiales de 
widgets&mdash;`InheritedWidgets`, `InheritedNotifier`, `InheritedModel`, y más. 
No los cubriremos aquí, porque son un poco de bajo nivel para lo que 
estamos tratando de hacer.

En su lugar, vamos a usar un paquete que funciona con los widgets de bajo 
nivel pero que es fácil de usar. Se llama `provider`.

Con `provider`, no tienes que preocuparte por las llamadas de 
retorno o `InheritedWidgets`. Pero necesitas entender 3 conceptos:

* ChangeNotifier
* ChangeNotifierProvider
* Consumer


## ChangeNotifier

`ChangeNotifier` es una clase sencilla incluida en el SDK de Flutter SDK que proporciona
notificaciones a los cambios a sus 'listeners'. En otras palabras, si algo es 
un `ChangeNotifier`, puedes suscribirte a sus cambios. (Es una forma de 
Observable, para aquellos que estén familiarizados con el término.)

En `provider`, `ChangeNotifier` es una manera de encapsular el estado de tu 
aplicación. Para apps muy simples, te basta con un único `ChangeNotifier`. 
En las más complejas, tendrás multitud de modelos, y por tanto muchos 
`ChangeNotifiers`. (No necesitas usar `ChangeNotifier` con `provider` 
, pero es una clase sencilla para trabajar con ella.)

En nuestro ejemplo de aplicación de compras, queremos gestionar el estado del carro en 
un `ChangeNotifier`. Creamos una nueva clase que extiende lo extiende como esta:

<?code-excerpt "state_mgmt/simple/lib/src/provider.dart (model)" replace="/ChangeNotifier/[!$&!]/g;/notifyListeners/[!$&!]/g"?>
```dart
class CartModel extends [!ChangeNotifier!] {
  /// Estado privado del carrito.
  final List<Item> _items = [];

  /// Una vista inmodificable de items en el carrito.
  UnmodifiableListView<Item> get items => UnmodifiableListView(_items);

   /// El precio total actual de todos los items (asumiendo que todos cuestan $42).
  int get totalPrice => _items.length * 42;

  /// Añadir [item] al carro. Esta es la única manera de modificar el carrito desde fuera.
  void add(Item item) {
    _items.add(item);
   // Esta llamada dice a los widgets que están escuchando este modelo que se reconstruyan.
    [!notifyListeners!]();
  }
}
```

El único código que es específico de `ChangeNotifier` es la llamada 
a `notifyListeners()`. Llama a este método cada vez que el modelo cambie de una manera que 
pueda cambiar la interfaz de usuario de tu aplicación. Todo lo demás en `CartModel` es 
el modelo mismo y su lógica de negocio.

`ChangeNotifier` es parte de `flutter:foundation` y no depende de clases
de mayor nivel en Flutter. Es facilmente testable (no necesitas usar 
[widget testing](/docs/testing#widget-tests) para ello). Por ejemplo,
aquí hay un sencillo test unitario de `CartModel`:

<?code-excerpt "state_mgmt/simple/test/model_test.dart (test)"?>
```dart
test('adding item increases total cost', () {
  final cart = CartModel();
  final startingPrice = cart.totalPrice;
  cart.addListener(() {
    expect(cart.totalPrice, greaterThan(startingPrice));
  });
  cart.add(Item('Dash'));
});
```


## ChangeNotifierProvider

`ChangeNotifierProvider` es el widget que proporciona una instancia de 
un `ChangeNotifier` a sus descendientes. Viene del paquete `provider`.

Ya sabemos dónde colocar `ChangeNotifierProvider`: por encima de los widgets que 
necesitarán acceder a él. En el caso de `CartModel`, eso significa en algún lugar 
por encima de `MyCart` y `MyCatalog`.

No quieres colocar `ChangeNotifierProvider` más arriba de lo que sea necesario 
(porque no quieres contaminar el alcance). Pero en nuestro caso, el 
único widget que está encima de `MyCart` y `MyCatalog` es `MyApp`.

<?code-excerpt "state_mgmt/simple/lib/main.dart (main)" replace="/ChangeNotifierProvider/[!$&!]/g"?>
```dart
void main() {
  runApp(
    [!ChangeNotifierProvider!](
      builder: (context) => CartModel(),
      child: MyApp(),
    ),
  );
}
```

Fijate que hemos definido un builder que creará una nueva instancia
de `CartModel`. `ChangeNotifierProvider` es lo suficientemente inteligente como para no 
reconstruir `CartModel` a no ser que sea absolutamete necesario. Este también llamará automáticamente a 
`dispose()` en `CartModel` cuando la instancia ya no sea necesaria.  

Si quieres proporcionar más de una clase, puedes usar `MultiProvider`:

<?code-excerpt "state_mgmt/simple/lib/main.dart (multi-provider-main)" replace="/multiProviderMain/main/g;/MultiProvider/[!$&!]/g"?>
```dart
void main() {
  runApp(
    [!MultiProvider!](
      providers: [
        ChangeNotifierProvider(builder: (context) => CartModel()),
        Provider(builder: (context) => SomeOtherClass()),
      ],
      child: MyApp(),
    ),
  );
}
```

## Consumer

Ahora que `CartModel` se proporciona a los widgets de nuestra aplicación a través de la 
declaración `ChangeNotifierProvider` en la parte superior, podemos empezar a usarlo.

Esto se hace a través del widget `Consumer`.

<?code-excerpt "state_mgmt/simple/lib/src/provider.dart (descendant)" replace="/Consumer/[!$&!]/g"?>
```dart
return [!Consumer!]<CartModel>(
  builder: (context, cart, child) {
    return Text("Total price: ${cart.totalPrice}");
  },
);
```

Debemos especificar el tipo de modelo al que queremos acceder. 
En este caso, queremos `CartModel`, así que escribimos 
`Consumer<CartModel>`. Si no especifica el genérico (`<CartModel>`), 
el paquete `provider` no podrá ayudarte. `Provider` está basado en tipos, 
y sin el tipo, no sabe lo que quieres.

El único argumento requerido del widget `Consumer` es el builder. 
Builder es una función que se llama cada vez que 
`ChangeNotifier` cambia. (En otras palabras, cuando llamas `notifyListeners()` 
en tu modelo, todos los métodos builder de todos los widgets `Consumer` correspondientes 
son llamados.)

El builder es llamado con tres atributos. El primero es `context`, que también se 
obtiene en todos los métodos de build. 

El segundo argumento de la función builder en una instancia 
de `ChangeNotifier`. Es lo que pediamos en primer lugar. Puedes 
usar los datos en el modelo para definir cómo debería ser la interfaz de usuario 
en cualquier punto dado.

El tercer atributo es `child`, que está ahí para la optimización.
Si tienes un gran subárbol de widgets bajo tu `Consumer`
que _no cambia_ cuando el model cambia, puedes construir este una sola vez y 
obtenerlo a traves del builder.

<?code-excerpt "state_mgmt/simple/lib/src/performance.dart (child)" replace="/\bchild\b/[!$&!]/g"?>
```dart
return Consumer<CartModel>(
  builder: (context, cart, [!child!]) => Stack(
        children: [
          // Usa SomeExpensiveWidget aquí, sin reconstruirlo cada vez.
          [!child!],
          Text("Total price: ${cart.totalPrice}"),
        ],
      ),
  // Construye el widget costoso aquí.
  [!child!]: SomeExpensiveWidget(),
);
```

Es una mejor práctica poner tus widgets `Consumer` tan profundo en el árbol como 
sea posible. No quieres reconstruir grandes porciones de la interfaz de usuario 
solo porque algún detalle cambie en algun lado.

<?code-excerpt "state_mgmt/simple/lib/src/performance.dart (nonLeafDescendant)"?>
```dart
// NO HAGAS ESTO
return Consumer<CartModel>(
  builder: (context, cart, child) {
    return HumongousWidget(
      // ...
      child: AnotherMonstrousWidget(
        // ...
        child: Text('Total price: ${cart.totalPrice}'),
      ),
    );
  },
);
```

En su lugar:

<?code-excerpt "state_mgmt/simple/lib/src/performance.dart (leafDescendant)"?>
```dart
// HAZ ESTO
return HumongousWidget(
  // ...
  child: AnotherMonstrousWidget(
    // ...
    child: Consumer<CartModel>(
      builder: (context, cart, child) {
        return Text('Total price: ${cart.totalPrice}');
      },
    ),
  ),
);
```

### Provider.of

A veces, no necesitas realmente los _datos_ en el modelo para cambiar la interfaz de usuario, 
pero aún así necesitas acceder a ella. Por ejemplo, un botón `ClearCart` 
quiere permitir al usuario eliminar todo del carrito. 
No necesitas mostrar el contenido del carrito, sólo 
necesita llamar al método `clear()`.

Podríamos usar `Consumer<CartModel>` para esto, 
pero eso sería un desperdicio. Estaríamos pidiendo al framework 
reconstruir un widget que no necesita ser reconstruido. 

Para este caso de uso, podemos usar `Provider.of`, con el parámetro `listen` 
fijado a `false`. 

<?code-excerpt "state_mgmt/simple/lib/src/performance.dart (nonRebuilding)" replace="/listen: false/[!$&!]/g"?>
```dart
Provider.of<CartModel>(context, [!listen: false!]).add(item);
```

Usar la línea anterior en un método build no hará que este widget se 
reconstruya cuando `notifyListeners` sea llamado.


## Uniendo todo

Puedes [revisar el 
ejemplo]({{site.github}}/filiph/samples/tree/provider-shopper/provider_shopper)
covierto en este artículo. Si quieres algo más simple,
puedes ver como la sencilla app Counter se ve cuando se [construye con 
`provider`](https://github.com/flutter/samples/tree/master/provider_counter).

Cuando estes preparado para jugar con `provider` tu mismo,
no olvides añadir primero la dependencia en tu `pubspec.yaml`.

```yaml
name: my_name
description: Blah blah blah.

# ...

dependencies:
  flutter:
    sdk: flutter

  provider: ^2.0.0

dev_dependencies:
  # ...
```

Ahora puedes `import 'package:provider/provider.dart';`
y empezar a contruir.
