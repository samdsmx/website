---
title: Layouts en Flutter
short-title: Layout
description: Aprende como trabaja el mecanismo de layout de Flutter y como construir un layout.
diff2html: true
---

{% assign api = site.api | append: '/flutter' -%}
{% capture code -%} {{site.repo.this}}/tree/{{site.branch}}/src/_includes/code {%- endcapture -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}
{% assign rawExFile = 'https://raw.githubusercontent.com/flutter/website/master/examples' -%}
{% capture demo -%} {{site.repo.flutter}}/tree/{{site.branch}}/examples/flutter_gallery/lib/demo {%- endcapture -%}

<style>dl, dd { margin-bottom: 0; }</style>

{{site.alert.secondary}}
  <h4 class="no_toc">Lo que aprenderás</h4>

  * Los widgets son clases usadas para construir UIs.
  * Los widgets son usados tanto por el layout como por los elementos de UI.
  * Crea widgets complejos mediante composición de widgets simples.
{{site.alert.end}}

El corazón del mecanismo de layout de Flutter son los widgets. En Flutter, casi
todo es un widget&mdash;incluso los modelos de layout son widgets.
Las imágenes, iconos, y texto que ves en una app Flutter, son todo widgets.
Pero cosas que no ves también son widgets, como son filas, columnas,
y cuadrículas que organizan, restringen, y alinean los widgets visibles.

Creas un layout mediante la composición de widgets para construir widgets más complejos.
Por ejemplo, la primera captura de pantalla abajo muestra tres iconos con una etiqueta sobre cada uno de ellos:

<div class="row mb-4">
  <div class="col-12 text-center">
    {% asset ui/layout/lakes-icons.png class="border mt-1 mb-1 mw-100" alt="Sample layout" %}
    {% asset ui/layout/lakes-icons-visual.png class="border mt-1 mb-1 mw-100" alt="Sample layout with visual debugging" %}
  </div>
</div>

La segunda captura de pantalla muestra el layout visualmente, mostrando una fila de 
3 columnas donde cada columna contiene un icono y una etiqueta.

{{site.alert.note}}
  La mayoría de las capturas de pantalla en este tutorial se muestran con 
  `debugPaintSizeEnabled` fijado en true por eso puedes ver el layout visualmente.
  Para más información, mira
  [Depuración Visual](/docs/testing/debugging#visual-debugging), una sección en 
  [Depurar apps Flutter](/docs/testing/debugging).
{{site.alert.end}}

Aqui está un diagrama del árbol de widgets para este UI:

{% asset ui/layout/sample-flutter-layout.png class="mw-100" alt="Node tree" %}
{:.text-center}

La mayoría de esto podría verse como podría esperarse, pero puede que te estes preguntando 
sobre los Container (mostrados en rosa). [Container][] es una clase widget que te permite
personalizar su widget hijo. Usa un `Container` cuando quieras añadir
padding, márgenes, bordes, o color de fondo, por nombrar alguna de sus 
capacidades.

En este ejemplo, cada widget [Text][] se situa en un `Container` para añadir márgenes.
La fila entera, [Row][], también esta colocada en un `Container` para añadir padding alrededor de la 
fila.

El resto del UI en este ejemplo es controlado por propiedades.
Fija un color para [Icon][] usando su propiedad `color`.
Us la propiedad `Text.style` para fijar la fuente, su color, tamañao, y así sucesivamente.
Columns y rows tienen propiedades que te permiten especificar como se alinearán 
sus hijos vertical u horizontalmente, y cuanto espacio deben ocupar 
los hijos.

## Da layout a un widget

¿Cómo puedes configurar el layout de un único widget? Esta sección te enseña 
como crear y mostrar un simple widget. También muestra el código completo 
para una app Hello World simple.

En Flutter, solo toma unos pocos pasos poner un texto, un icono, o una imagen 
en la pantalla.

### 1. Selecciona un widget de layout

Elige entre una variedad de [widgets de layout][] basándote 
en como quieres alinear o restringir el widget visible,
ya que estas características son normalmente pasadas al 
widget contenido.

Este ejemplo usa [Center][] el cual centra su contenido
horizontal y verticalmente.

### 2. Crea un widget visible

Por ejemplo, crea un widget [Text][]:

<?code-excerpt "layout/base/lib/main.dart (text)" replace="/child: //g"?>
```dart
Text('Hello World'),
```

Crea un widget [Image][]:

<?code-excerpt "layout/lakes/step5/lib/main.dart (Image-asset)" remove="/width|height/"?>
```dart
Image.asset(
  'images/lake.jpg',
  fit: BoxFit.cover,
),
```

Crea un widget [Icon][]:

<?code-excerpt "layout/lakes/step5/lib/main.dart (Icon)"?>
```dart
Icon(
  Icons.star,
  color: Colors.red[500],
),
```

### 3. Añade el widget visible al widget de layout

<?code-excerpt path-base="layout/base"?>

Todos los widgets de layout tiene alguna de las siguientes:

- Una propiedad `child` que toma un único hijo -- por ejemplo, `Center` o
  `Container`
- Una propiedad `children` que toma una lista de widgets -- por ejemplo, `Row`,
  `Column`, `ListView`, o `Stack`.

Añade un widget `Text` al widget `Center`:

<?code-excerpt "lib/main.dart (centered-text)" replace="/body: //g"?>
```dart
Center(
  child: Text('Hello World'),
),
```

### 4. Añade el widget de layout a la página

Una app Flutter es en si misma un widget, y la mayoría de los widgets tienen un método 
[build()][]. Instanciar y devolver un widget en el método `build()` de la app 
muestra el widget.

#### Apps Material

Para una app `Material`, puedes usar un widget [Scaffold][]; este proporciona un banner 
por defecto, color de fondo, y tiene una API para añadir drawers, snack bars, y bottom
sheets. Entonces puedes añadir el widget `Center` directamente a la propiedad `body` para 
la página principal.

<?code-excerpt path-base="layout/base"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter layout demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Flutter layout demo'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

{{site.alert.note}}
  La [biblioteca Material][] implementa widgets que siguen los principios 
  [Material Design][]. Cuando diseñas tu UI, puede usar exclusivamente 
  widgets de la [biblioteca de widgets][] estandar, o puedes usar widgets de 
  la biblioteca Material. Puedes mezclar widgets de ambas bibliotecas, puedes
  personalizar widgets existentes, o puedes construir tu propio conjunto de 
  widgets.
{{site.alert.end}}

#### Apps No-Material

Para una app no-Material, puedes añadir el widget `Center` al método build de 
la app:

<?code-excerpt path-base="layout/non_material"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(color: Colors.white),
      child: Center(
        child: Text(
          'Hello World',
          textDirection: TextDirection.ltr,
          style: TextStyle(
            fontSize: 32,
            color: Colors.black87,
          ),
        ),
      ),
    );
  }
}
```

Por defecto una app no-Material no incluye un `AppBar`, título, o color de 
fondo. Si quieres estas características en una app no-Material, tienes que construirlas 
tu mismo. Esta app cambia el color de fondo a blanco y el texto a gris 
oscuro para imitar una app Material.

<div class="row">
<div class="col-md-6" markdown="1">
  ¡Esto es todo! Cuando ejecutes la app, debes ver _Hello World_.

  Código fuente de la App:
  - [Material app]({{examples}}/layout/base)
  - [Non-Material app]({{examples}}/layout/non_material)
</div>
<div class="col-md-6">
  {% include app-figure.md img-class="site-mobile-screenshot border w-75"
      image="ui/layout/hello-world.png" alt="Hello World" %}
</div>
</div>

<hr>

## Organiza multiples widgets vertical y horizontalmente

<?code-excerpt path-base=""?>

Uno de los patrones más comunes de layout es organizar los widgets vertical 
u horizontalmente. Puedes usar un widget Row para organizar widgets horizontalmente,
y un widget Column para organizar widgets verticalmente.

{{site.alert.secondary}}
  <h4 class="no_toc">Lo que aprenderás</h4>

  * Row y Column son dos de los más comúnmente usados patrones de layout.
  * Row y Column toman ambos una lista de widgets hijos.
  * Un widget hijo puede ser él mismo un Row, Column, u otro widget complejo.
  * Puedes especificar como un Row o Column alinea sus hijos, tanto vertical 
    como horizontalmente.
  * Puedes estirar o restringir widgets hijo específicos.
  * Puedes especificar como los widgets hijo usa el espacio disponible del Row o Column.
{{site.alert.end}}

Para crear una fila o columna en Flutter, añades una lista de widgets hijo a un widget
[Row][] o [Column][]. Sucesivamente, cada hijo puede ser en si mismo una fila o columna. 
El siguiente ejemplo muestra como es posible anidar filas o columnas
dentro de filas o columnas.

Este layout está organizado como un Row. La fila contiene dos hijos:
una columna en la izquierda, y una imagen en la derecha:

{% asset ui/layout/pavlova-diagram.png class="mw-100"
    alt="Screenshot with callouts showing the row containing two children" %}

El árbol de widgets de la columna izquierda anida filas y columnas.

{% asset ui/layout/pavlova-left-column-diagram.png class="mw-100"
    alt="Diagram showing a left column broken down to its sub-rows and sub-columns" %}

Implementarás algo del layout del código de la app Pavlova en
[Anidando filas y columnas](#anidando-filas-y-columnas).

{{site.alert.note}}
  Row y Column son widgets primitivos básicos para layout horizontales 
  y verticales&mdash;estos widgets de bajo nivel permiten una personalización 
  máxima. Flutter también ofrece widgets especializaos de alto nivel 
  que deben ser suficientes para tus necesidades. Por ejemplo, en lugar de un Row
  podrías preferir 
  [ListTile]({{api}}/material/ListTile-class.html),
  un widget de fácil uso con propiedades para iconos iniciales y finales,
  y hasta 3 líneas de texto. En lugar de Column, podrías preferir
  [ListView]({{api}}/widgets/ListView-class.html),
  un layout de tipo columna que permite el scroll automático si su contenido es demasiado largo
  para ajustarse al espacio disponible. Para más información,
  mira [Widgets de layout comunes](#widgets-de-layout-comunes).
{{site.alert.end}}

### Alineación de widgets

Controlas como una fila o columna alinea sus hijos usando 
las propiedades `mainAxisAlignment` y `crossAxisAlignment`.
Para una fila, el eje principal corre horizontalmente y el eje transversal corre 
verticalmente. Para una columna, el eje principal corre verticalmente y el eje 
transversal corre horizontalmente.

<div class="mb-2 text-center">
  {% asset ui/layout/row-diagram.png class="mb-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a row" %}
  {% asset ui/layout/column-diagram.png class="mb-2 mr-2 ml-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a column" %}
</div>

Las clases [MainAxisAlignment]({{api}}/rendering/MainAxisAlignment-class.html)
y [CrossAxisAlignment]({{api}}/rendering/CrossAxisAlignment-class.html)
ofrecen una variedad de constantes para controlar la alineación.

{{site.alert.note}}
  Cuando añades imágenes a tu proyecto,
  necesitas actualizar el fichero pubspec para acceder a estas&mdash;este
  ejemplo usa `Image.asset` para mostrar imágenes. Para más información,
  mira este ejemplo de [fichero 
  pubspec.yaml]({{examples}}/layout/row/pubspec.yaml),
  o [Añadir assets e imágenes en Flutter](/docs/development/ui/assets-and-images).
  No necesitas hacer esto si estas referenciando imágenes online usando 
  `Image.network`.
{{site.alert.end}}

En el siguiente ejemplo, cada una de las 3 imágenes tiene 100 pixels de ancho.
La caja de renderizado (en este caso, la pantalla entera) tiene más de 300 pixeles de ancho,
entonces fijando la alineación del eje principal a `spaceEvenly` divide el espacio libre 
horizontal igualitariamente entre, antes, y después de cada imagen.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/row_column/lib/main.dart (Row)" replace="/Row/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Row!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-spaceevenly-visual.png class="mw-100" alt="Row with 3 evenly spaced images" %}

  **App source:** [row_column]({{examples}}/layout/row_column)
</div>
</div>

Las columnas trabajan de la misma manera que las filas. El siguiente ejemplo muestra una columna 
de 3 imágenes, cada una de 100 pixels de alto. la altura de la caja de renderizado 
(en este caso, la pantalla entera) tiene más de 300 pixels, entonces 
fijando la alineación en el eje principal a `spaceEvenly` divide el espacio libre vertical 
igualitariamente entre, por encima, y por debajo de cada imagen.

<div class="row">
<div class="col-lg-8" markdown="1">
  <?code-excerpt "layout/row_column/lib/main.dart (Column)" replace="/Column/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Column!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}

  **App source:** [row_column]({{examples}}/layout/row_column)
</div>
<div class="col-lg-4 text-center">
  {% asset ui/layout/column-visual.png class="mb-4" height="250px"
      alt="Column showing 3 images spaced evenly" %}
</div>
</div>

### Dimensionando widgets

Cuando un layout es demasiado grande para ajustarse al dispositivo, una franja con un patrón 
de rayas amarillas y negras
aparece a lo largo del limite afectado. Here is an [example][sizing] of a row that is
too wide:

{% asset ui/layout/layout-too-large.png class="mw-100" alt="Overly-wide row" %}
{:.text-center}

Widgets can be sized to fit within a row or column by using the [Expanded][]
widget. To fix the previous example where the row of images is too wide for its
render box, wrap each image with an `Expanded` widget.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images)" replace="/Expanded/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      [!Expanded!](
        child: Image.asset('images/pic1.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic2.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-expanded-2-visual.png class="mw-100"
      alt="Row of 3 images that are too wide, but each is constrained to take only 1/3 of the space" %}

  **App source:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

Perhaps you want a widget to occupy twice as much space as its siblings. For
this, use the `Expanded` widget `flex` property, an integer that determines the
flex factor for a widget. The default flex factor is 1. The following code sets
the flex factor of the middle image to 2:

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images-with-flex)" replace="/flex.*/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      Expanded(
        child: Image.asset('images/pic1.jpg'),
      ),
      Expanded(
        [!flex: 2,!]
        child: Image.asset('images/pic2.jpg'),
      ),
      Expanded(
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/row-expanded-visual.png class="mw-100"
      alt="Row of 3 images with the middle image twice as wide as the others" %}

  **App source:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

[sizing]: {{examples}}/layout/sizing

### Packing widgets

By default, a row or column occupies as much space along its main axis
as possible, but if you want to pack the children closely together,
set its `mainAxisSize` to `MainAxisSize.min`. The following example
uses this property to pack the star icons together.

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/pavlova/lib/main.dart (stars)" replace="/mainAxisSize.*/[!$&!]/g; /\w+ \w+ = //g; /;//g"?>
  {% prettify dart context="html" %}
  Row(
    [!mainAxisSize: MainAxisSize.min,!]
    children: [
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.black),
      Icon(Icons.star, color: Colors.black),
    ],
  )
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  {% asset ui/layout/packed.png class="border mw-100"
      alt="Row of 5 stars, packed together in the middle of the row" %}

  **App source:** [pavlova]({{examples}}/layout/pavlova)
</div>
</div>

### Nesting rows and columns

The layout framework allows you to nest rows and columns inside of rows
and columns as deeply as you need. Let's look the code for the outlined section
of the following layout:

{% asset ui/layout/pavlova-large-annotated.png class="border mw-100"
    alt="Screenshot of the pavlova app, with the ratings and icon rows outlined in red" %}
{:.text-center}

The outlined section is implemented as two rows. The ratings row contains
five stars and the number of reviews. The icons row contains three
columns of icons and text.

The widget tree for the ratings row:

{% asset ui/layout/widget-tree-pavlova-rating-row.png class="mw-100" alt="Ratings row widget tree" %}
{:.text-center}

The `ratings` variable creates a row containing a smaller row of 5 star icons,
and text:

<?code-excerpt "layout/pavlova/lib/main.dart (ratings)" replace="/ratings/[!$&!]/g"?>
```dart
var stars = Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.black),
    Icon(Icons.star, color: Colors.black),
  ],
);

final [!ratings!] = Container(
  padding: EdgeInsets.all(20),
  child: Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      stars,
      Text(
        '170 Reviews',
        style: TextStyle(
          color: Colors.black,
          fontWeight: FontWeight.w800,
          fontFamily: 'Roboto',
          letterSpacing: 0.5,
          fontSize: 20,
        ),
      ),
    ],
  ),
);
```

{{site.alert.tip}}
  To minimize the visual confusion that can result from heavily nested layout
  code, implement pieces of the UI in variables and functions.
{{site.alert.end}}

The icons row, below the ratings row, contains 3 columns; each column contains
an icon and two lines of text, as you can see in its widget tree:

{% asset ui/layout/widget-tree-pavlova-icon-row.png class="mw-100" alt="Icon widget tree" %}
{:.text-center}

The `iconList` variable defines the icons row:

<?code-excerpt "layout/pavlova/lib/main.dart (iconList)" replace="/iconList/[!$&!]/g"?>
```dart
final descTextStyle = TextStyle(
  color: Colors.black,
  fontWeight: FontWeight.w800,
  fontFamily: 'Roboto',
  letterSpacing: 0.5,
  fontSize: 18,
  height: 2,
);

// DefaultTextStyle.merge() allows you to create a default text
// style that is inherited by its child and all subsequent children.
final [!iconList!] = DefaultTextStyle.merge(
  style: descTextStyle,
  child: Container(
    padding: EdgeInsets.all(20),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
      children: [
        Column(
          children: [
            Icon(Icons.kitchen, color: Colors.green[500]),
            Text('PREP:'),
            Text('25 min'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.timer, color: Colors.green[500]),
            Text('COOK:'),
            Text('1 hr'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.restaurant, color: Colors.green[500]),
            Text('FEEDS:'),
            Text('4-6'),
          ],
        ),
      ],
    ),
  ),
);
```

The `leftColumn` variable contains the ratings and icons rows, as well as the
title and text that describes the Pavlova:

<?code-excerpt "layout/pavlova/lib/main.dart (leftColumn)" replace="/leftColumn/[!$&!]/g"?>
```dart
final [!leftColumn!] = Container(
  padding: EdgeInsets.fromLTRB(20, 30, 20, 20),
  child: Column(
    children: [
      titleText,
      subTitle,
      ratings,
      iconList,
    ],
  ),
);
```

The left column is placed in a `Container` to constrain its width.
Finally, the UI is constructed with the entire row (containing the
left column and the image) inside a `Card`.

The [Pavlova image][] is from [Pixabay][].
You can embed an image from the net using `Image.network()` but,
for this example, the image is saved to an images directory in the project,
added to the [pubspec file,]({{examples}}/layout/pavlova/pubspec.yaml)
and accessed using `Images.asset()`. For more information, see
[Adding assets and images](/docs/development/ui/assets-and-images).

<?code-excerpt "layout/pavlova/lib/main.dart (body)"?>
```dart
body: Center(
  child: Container(
    margin: EdgeInsets.fromLTRB(0, 40, 0, 30),
    height: 600,
    child: Card(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            width: 440,
            child: leftColumn,
          ),
          mainImage,
        ],
      ),
    ),
  ),
),
```

{{site.alert.tip}}
  The Pavlova example runs best horizontally on a wide device, such as a tablet.
  If you are running this example in the iOS simulator, you can select a
  different device using the **Hardware > Device** menu. For this example, we
  recommend the iPad Pro. You can change its orientation to landscape mode using
  **Hardware > Rotate**. You can also change the size of the simulator window
  (without changing the number of logical pixels) using **Window > Scale**.
{{site.alert.end}}

**App source:** [pavlova]({{examples}}/layout/pavlova)

[Pavlova image]: https://pixabay.com/en/photos/pavlova
[Pixabay]: https://pixabay.com/en/photos/pavlova

<hr>

## Widgets de layout comunes

Flutter has a rich library of layout widgets. Here are a few of those most
commonly used. The intent is to get you up and running as quickly as possible,
rather than overwhelm you with a complete list.  For information on other
available widgets, refer to the [Widget catalog][],
or use the Search box in the [API reference docs]({{api}}).
Also, the widget pages in the API docs often make suggestions
about similar widgets that might better suit your needs.

The following widgets fall into two categories: standard widgets from the
[biblioteca de widgets][], and specialized widgets from the [biblioteca Material][]. Any
app can use the widgets library but only Material apps can use the Material
Components library.

### Standard widgets

* [Container](#container): Adds padding, margins, borders, background color, or
  other decorations to a widget.
* [GridView](#gridview): Lays widgets out as a scrollable grid.
* [ListView](#listview): Lays widgets out as a scrollable list.
* [Stack](#stack): Overlaps a widget on top of another.

### Material widgets

* [Card](#card): Organizes related info into a box with rounded corners and a
  drop shadow.
* [ListTile](#listtile): Organizes up to 3 lines of text, and optional leading
  and trailing icons, into a row.

### Container

Many layouts make liberal use of [Container][]s to separate widgets using
padding, or to add borders or margins. You can change the device's background
by placing the entire layout into a `Container` and changing its background
color or image.

<div class="row">
<div class="col-lg-6" markdown="1">
  <h4 class="no_toc">Summary (Container)</h4>

  * Add padding, margins, borders
  * Change background color or image
  * Contains a single child widget, but that child can be a Row, Column,
    or even the root of a widget tree
</div>
<div class="col-lg-6 text-center">
  {% asset ui/layout/margin-padding-border.png class="mb-4 mw-100"
      width="230px"
      alt="Diagram showing: margin, border, padding, and content" %}
</div>
</div>

#### Examples (Container)
{:.no_toc}

This layout consists of a column with two rows, each containing 2 images. A
[Container][] is used to change the background color of the column to a lighter
grey.

<div class="row">
<div class="col-lg-7">
  <?code-excerpt "layout/container/lib/main.dart (column)" replace="/\bContainer/[!$&!]/g;"?>
  {% prettify dart context="html" %}
  Widget _buildImageColumn() => [!Container!](
        decoration: BoxDecoration(
          color: Colors.black26,
        ),
        child: Column(
          children: [
            _buildImageRow(1),
            _buildImageRow(3),
          ],
        ),
      );
  {% endprettify %}
</div>
<div class="col-lg-5 text-center">
  {% asset ui/layout/container.png class="mb-4 mw-100" width="230px"
      alt="Screenshot showing 2 rows, each containing 2 images" %}
</div>
</div>

A `Container` is also used to add a rounded border and margins to each image:

<?code-excerpt "layout/container/lib/main.dart (row)" replace="/\bContainer/[!$&!]/g;"?>
```dart
Widget _buildDecoratedImage(int imageIndex) => Expanded(
      child: [!Container!](
        decoration: BoxDecoration(
          border: Border.all(width: 10, color: Colors.black38),
          borderRadius: const BorderRadius.all(const Radius.circular(8)),
        ),
        margin: const EdgeInsets.all(4),
        child: Image.asset('images/pic$imageIndex.jpg'),
      ),
    );

Widget _buildImageRow(int imageIndex) => Row(
      children: [
        _buildDecoratedImage(imageIndex),
        _buildDecoratedImage(imageIndex + 1),
      ],
    );
```

You can find more `Container` examples in the [tutorial][] and the [Flutter
Gallery][].

**App source:** [container]({{examples}}/layout/container)

<hr>

### GridView

Use [GridView][] to lay widgets out as a two-dimensional list. `GridView`
provides two pre-fabricated lists, or you can build your own custom grid. When a
`GridView` detects that its contents are too long to fit the render box, it
automatically scrolls.

#### Summary (GridView)
{:.no_toc}

* Lays widgets out in a grid
* Detects when the column content exceeds the render box and automatically
  provides scrolling
* Build your own custom grid, or use one of the provided grids:
  * `GridView.count` allows you to specify the number of columns
  * `GridView.extent` allows you to specify the maximum pixel width of a tile
{% comment %}
* Use `MediaQuery.of(context).orientation` to create a grid that changes
  its layout depending on whether the device is in landscape or portrait mode.
{% endcomment %}

{{site.alert.note}}
  When displaying a two-dimensional list where it's important which
  row and column a cell occupies (for example,
  it's the entry in the "calorie" column for the "avocado" row), use
  [Table]({{api}}/widgets/Table-class.html) or
  [DataTable]({{api}}/material/DataTable-class.html).
{{site.alert.end}}

#### Examples (GridView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/gridview-extent.png class="mw-100" alt="A 3-column grid of photos" %}
  {:.text-center}

  Uses `GridView.extent` to create a grid with tiles a maximum 150 pixels wide.

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/gridview-count-flutter-gallery.png class="mw-100"
      alt="A 2 column grid with footers" %}
  {:.text-center}

  Uses `GridView.count` to create a grid that's 2 tiles wide in portrait mode,
  and 3 tiles wide in landscape mode. The titles are created by setting the
  `footer` property for each [GridTile][].

  **Dart code:** [grid_list_demo.dart]({{demo}}/material/grid_list_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (grid)" replace="/\GridView/[!$&!]/g;"?>
```dart
Widget _buildGrid() => [!GridView!].extent(
    maxCrossAxisExtent: 150,
    padding: const EdgeInsets.all(4),
    mainAxisSpacing: 4,
    crossAxisSpacing: 4,
    children: _buildGridTileList(30));

// The images are saved with names pic0.jpg, pic1.jpg...pic29.jpg.
// The List.generate() constructor allows an easy way to create
// a list when objects have a predictable naming pattern.
List<Container> _buildGridTileList(int count) => List.generate(
    count, (i) => Container(child: Image.asset('images/pic$i.jpg')));
```

<hr>

### ListView

[ListView]({{api}}/widgets/ListView-class.html),
a column-like widget, automatically provides scrolling when
its content is too long for its render box.

#### Summary (ListView)
{:.no_toc}

* A specialized [Column][] for organizing a list of boxes
* Can be laid out horizontally or vertically
* Detects when its content won't fit and provides scrolling
* Less configurable than `Column`, but easier to use and supports scrolling

#### Examples (ListView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listview.png class="border mw-100"
      alt="ListView containing movie theaters and restaurants" %}
  {:.text-center}

  Uses `ListView` to display a list of businesses using `ListTile`s. A `Divider`
  separates the theaters from the restaurants.

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listview-flutter-gallery.png class="border mw-100"
      alt="ListView containing shades of blue" %}
  {:.text-center}

  Uses `ListView` to display the [Colors]({{api}}/material/Colors-class.html) from
  the [Material Design palette]({{site.material}}/guidelines/style/color.html)
  for a particular color family.

  **Dart code:** [colors_demo.dart]({{demo}}/colors_demo.dart) from the
  [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (list)" replace="/\ListView/[!$&!]/g;"?>
```dart
Widget _buildList() => [!ListView!](
      children: [
        _tile('CineArts at the Empire', '85 W Portal Ave', Icons.theaters),
        _tile('The Castro Theater', '429 Castro St', Icons.theaters),
        _tile('Alamo Drafthouse Cinema', '2550 Mission St', Icons.theaters),
        _tile('Roxie Theater', '3117 16th St', Icons.theaters),
        _tile('United Artists Stonestown Twin', '501 Buckingham Way',
            Icons.theaters),
        _tile('AMC Metreon 16', '135 4th St #3000', Icons.theaters),
        Divider(),
        _tile('K\'s Kitchen', '757 Monterey Blvd', Icons.restaurant),
        _tile('Emmy\'s Restaurant', '1923 Ocean Ave', Icons.restaurant),
        _tile(
            'Chaiya Thai Restaurant', '272 Claremont Blvd', Icons.restaurant),
        _tile('La Ciccia', '291 30th St', Icons.restaurant),
      ],
    );

ListTile _tile(String title, String subtitle, IconData icon) => ListTile(
      title: Text(title,
          style: TextStyle(
            fontWeight: FontWeight.w500,
            fontSize: 20,
          )),
      subtitle: Text(subtitle),
      leading: Icon(
        icon,
        color: Colors.blue[500],
      ),
    );
```

<hr>

### Stack

Use [Stack][] to arrange widgets on top of a base widget&mdash;often an image.
The widgets can completely or partially overlap the base widget.

#### Summary (Stack)
{:.no_toc}

* Use for widgets that overlap another widget
* The first widget in the list of children is the base widget;
  subsequent children are overlaid on top of that base widget
* A `Stack`'s content can't scroll
* You can choose to clip children that exceed the render box

#### Examples (Stack)
{:.no_toc}

<div class="row">
<div class="col-lg-7" markdown="1">
  {% asset ui/layout/stack.png class="mw-100" width="200px" alt="Circular avatar image with a label" %}
  {:.text-center}

  Uses `Stack` to overlay a `Container` (that displays its `Text` on a translucent
  black background) on top of a `CircleAvatar`.
  The `Stack` offsets the text using the `alignment` property and
  `Alignment`s.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-5" markdown="1">
  {% asset ui/layout/stack-flutter-gallery.png class="mw-100" alt="An image with a grey gradient across the top" %}
  {:.text-center}

  Uses `Stack` to overlay a gradient to the top of the image. The gradient
  ensures that the toolbar's icons are distinct against the image.

  **Dart code:** [contacts_demo.dart]({{demo}}/contacts_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Stack)" replace="/\bStack/[!$&!]/g;"?>
```dart
Widget _buildStack() => [!Stack!](
    alignment: const Alignment(0.6, 0.6),
    children: [
      CircleAvatar(
        backgroundImage: AssetImage('images/pic.jpg'),
        radius: 100,
      ),
      Container(
        decoration: BoxDecoration(
          color: Colors.black45,
        ),
        child: Text(
          'Mia B',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
        ),
      ),
    ],
  );
```

<hr>

### Card

A [Card][], from the [biblioteca Material][], contains related nuggets of
information and can be composed from almost any widget, but is often used with
[ListTile][]. `Card` has a single child, but its child can be a column, row,
list, grid, or other widget that supports multiple children. By default, a
`Card` shrinks its size to 0 by 0 pixels. You can use [SizedBox][] to constrain
the size of a card.

In Flutter, a `Card` features slightly rounded corners and a drop shadow, giving
it a 3D effect. Changing a `Card`'s `elevation` property allows you to control
the drop shadow effect. Setting the elevation to 24, for example, visually lifts
the `Card` further from the surface and causes the shadow to become more
dispersed. For a list of supported elevation values, see [Elevation][] in the
[Material guidelines][Material Design]. Specifying an unsupported value disables
the drop shadow entirely.

#### Summary (Card)
{:.no_toc}

* Implements a [Material card][]
* Used for presenting related nuggets of information
* Accepts a single child, but that child can be a `Row`, `Column`, or other
  widget that holds a list of children
* Displayed with rounded corners and a drop shadow
* A `Card`'s content can't scroll
* From the [biblioteca Material][]

#### Examples (Card)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card.png class="mw-100" alt="Card containing 3 ListTiles" %}
  {:.text-center}

  A `Card` containing 3 ListTiles and sized by wrapping it with a `SizedBox`. A
  `Divider` separates the first and second `ListTiles`.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card-flutter-gallery.png class="mw-100"
      alt="Card containing an image, text and buttons" %}
  {:.text-center}

  A `Card` containing an image and text.

  **Dart code:** [cards_demo.dart]({{demo}}/material/cards_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Card)" replace="/\bCard/[!$&!]/g;"?>
```dart
Widget _buildCard() => SizedBox(
    height: 210,
    child: [!Card!](
      child: Column(
        children: [
          ListTile(
            title: Text('1625 Main Street',
                style: TextStyle(fontWeight: FontWeight.w500)),
            subtitle: Text('My City, CA 99984'),
            leading: Icon(
              Icons.restaurant_menu,
              color: Colors.blue[500],
            ),
          ),
          Divider(),
          ListTile(
            title: Text('(408) 555-1212',
                style: TextStyle(fontWeight: FontWeight.w500)),
            leading: Icon(
              Icons.contact_phone,
              color: Colors.blue[500],
            ),
          ),
          ListTile(
            title: Text('costa@example.com'),
            leading: Icon(
              Icons.contact_mail,
              color: Colors.blue[500],
            ),
          ),
        ],
      ),
    ),
  );
```
<hr>

### ListTile

Use [ListTile][], a specialized row widget from the [biblioteca Material][], for an
easy way to create a row containing up to 3 lines of text and optional leading
and trailing icons. `ListTile` is most commonly used in [Card][] or
[ListView][], but can be used elsewhere.

#### Summary (ListTile)
{:.no_toc}

* A specialized row that contains up to 3 lines of text and optional icons
* Less configurable than `Row`, but easier to use
* From the [biblioteca Material][]

#### Examples (ListTile)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/card.png class="mw-100" alt="Card containing 3 ListTiles" %}
  {:.text-center}

  A `Card` containing 3 `ListTiles`.

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)
</div>
<div class="col-lg-6" markdown="1">
  {% asset ui/layout/listtile-flutter-gallery.png class="border mw-100" height="200px"
      alt="3 ListTiles, each containing a pull-down button" %}
  {:.text-center}

  Uses `ListTile` to list 3 drop down button types.<br>
  **Dart code:** [buttons_demo.dart]({{demo}}/material/buttons_demo.dart)
  from the [Flutter Gallery][]
</div>
</div>

<hr>

## Resources

The following resources may help when writing layout code.

* [Layout tutorial](/docs/development/ui/layout/tutorial)
: Learn how to build a layout.
* [Widget Overview](/docs/development/ui/widgets)
: Describes many of the widgets available in Flutter.
* [HTML/CSS Analogs in Flutter](/docs/get-started/flutter-for/web-devs)
: For those familiar with web programming, this page maps HTML/CSS functionality
  to Flutter features.
* [Flutter Gallery][]
: Demo app showcasing many Material Design widgets and other Flutter features.
* [Flutter API documentation]({{api}})
: Reference documentation for all of the Flutter libraries.
* [Dealing with Box Constraints in Flutter](/docs/development/ui/layout/box-constraints)
: Discusses how widgets are constrained by their render boxes.
* [Adding Assets and Images in Flutter](/docs/development/ui/assets-and-images)
: Explains how to add images and other assets to your app's package.
* [Zero to One with Flutter]({{site.medium}}/@mravn/zero-to-one-with-flutter-43b13fd7b354)
: One person's experience writing his first Flutter app.

[build()]: {{api}}/widgets/StatelessWidget/build.html
[Card]: {{api}}/material/Card-class.html
[Center]: {{api}}/widgets/Center-class.html
[Column]: {{api}}/widgets/Column-class.html
[Container]: {{api}}/widgets/Container-class.html
[Elevation]: {{site.material}}/design/environment/elevation.html
[Expanded]: {{api}}/widgets/Expanded-class.html
[Flutter Gallery]: {{site.repo.flutter}}/tree/master/examples/flutter_gallery
[GridView]: {{api}}/widgets/GridView-class.html
[GridTile]: {{api}}/material/GridTile-class.html
[Icon]: {{api}}/material/Icons-class.html
[Image]: {{api}}/widgets/Image-class.html
[widgets de layout]: /docs/development/ui/widgets/layout
[ListTile]: {{api}}/material/ListTile-class.html
[ListView]: {{api}}/widgets/ListView-class.html
[Material card]: {{site.material}}/design/components/cards.html
[Material Design]: {{site.material}}/design
[biblioteca Material]: {{api}}/material/material-library.html
[Row]: {{api}}/widgets/Row-class.html
[Scaffold]: {{api}}/material/Scaffold-class.html
[SizedBox]: {{api}}/widgets/SizedBox-class.html
[Stack]: {{api}}/widgets/Stack-class.html
[Text]: {{api}}/widgets/Text-class.html
[tutorial]: /docs/development/ui/layout/tutorial
[biblioteca de widgets]: {{api}}/widgets/widgets-library.html
[Widget catalog]: /docs/development/ui/widgets
