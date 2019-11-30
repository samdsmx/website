---
title: "Trabajando con Pestañas"
prev:
  title: Uso de fuentes personalizadas
  path: /docs/cookbook/design/fonts
next:
  title: Construir un formulario con validación
  path: /docs/cookbook/forms/validation
---

Trabajar con pestañas(tabs) es un patrón común en las aplicaciones que siguen las pautas de 
Material Design. Flutter incluye una forma conveniente de crear diseños de pestañas como parte de 
la [biblioteca Material({{site.api}}/flutter/material/material-library.html).

{{site.alert.note}}
  Para crear pestañas en una app Cupertino app, mira
  el codelab 
  [Building a Cupertino app with
  Flutter](https://codelabs.developers.google.com/codelabs/flutter-cupertino).
{{site.alert.end}}

Esta receta crea un ejemplo con pestañas usando los siguientes pasos;

  1. Crea un `TabController`.
  2. Crea las pestañas.
  3. Crea el contenido para cada pestaña.

## 1. Crea un `TabController`

Para que las pestañas funcionen, necesitaremos mantener sincronizadas las secciones 
de pestañas y contenido 
seleccionadas. Este es el trabajo de 
[`TabController`]({{site.api}}/flutter/material/TabController-class.html).

Podemos crear manualmente un `TabController` o automáticamente usando el Widget 
[`DefaultTabController`]({{site.api}}/flutter/material/DefaultTabController-class.html). 

Usar `DefaultTabController` es la opción más simple, ya que creará 
un `TabController` para nosotros y lo pondrá a disposición de todos los widgets descendientes.

<!-- skip -->
```dart
DefaultTabController(
  // La cantidad de pestañas / secciones de contenido a mostrar
  length: 3,
  child: // Completa este código en el siguiente paso
);
```

## 2. Crea las pestañas

Ahora que tenemos un `TabController` para trabajar, podemos crear nuestras pestañas usando 
el Widget [`TabBar`]({{site.api}}/flutter/material/TabController-class.html). 
En este ejemplo, crearemos un 
`TabBar` con 3 Widgets [`Tab`]({{site.api}}/flutter/material/Tab-class.html) 
y lo colocaremos dentro de un 
[`AppBar`]({{site.api}}/flutter/material/AppBar-class.html).

<!-- skip -->
```dart
DefaultTabController(
  length: 3,
  child: Scaffold(
    appBar: AppBar(
      bottom: TabBar(
        tabs: [
          Tab(icon: Icon(Icons.directions_car)),
          Tab(icon: Icon(Icons.directions_transit)),
          Tab(icon: Icon(Icons.directions_bike)),
        ],
      ),
    ),
  ),
);
```

Por defecto, `TabBar` busca en el árbol de widgets el 
`DefaultTabController` más cercano. 
Si estás creando manualmente un `TabController`, 
deberás pasarlo al `TabBar`.

## 3. Crea el contenido para cada pestaña

Ahora que tienes pestañas, muestra el contenido cuando se seleccione una pestaña. 
Para este propósito, usa el Widget 
[`TabBarView`]({{site.api}}/flutter/material/TabBarView-class.html).

{{site.alert.note}}
 El orden es importante y debe corresponder al orden de las 
 pestañas en el `TabBar`.
{{site.alert.end}}

<!-- skip -->
```dart
TabBarView(
  children: [
    Icon(Icons.directions_car),
    Icon(Icons.directions_transit),
    Icon(Icons.directions_bike),
  ],
);
```

## Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(TabBarDemo());
}

class TabBarDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: DefaultTabController(
        length: 3,
        child: Scaffold(
          appBar: AppBar(
            bottom: TabBar(
              tabs: [
                Tab(icon: Icon(Icons.directions_car)),
                Tab(icon: Icon(Icons.directions_transit)),
                Tab(icon: Icon(Icons.directions_bike)),
              ],
            ),
            title: Text('Tabs Demo'),
          ),
          body: TabBarView(
            children: [
              Icon(Icons.directions_car),
              Icon(Icons.directions_transit),
              Icon(Icons.directions_bike),
            ],
          ),
        ),
      ),
    );
  }
}
```

![Tabs Demo](/images/cookbook/tabs.gif){:.site-mobile-screenshot}