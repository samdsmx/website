---
title: Documentación de Flutter
short-title: Docs
description: La landing page para la documentación de Flutter.
---

<div class="card-deck">
{% for card in site.data.docs_cards -%}
  <a class="card" href="{{card.url}}">
    <div class="card-body">
      <header class="card-title">{{card.name}}</header>
      <p class="card-text">{{card.description}}</p>
    </div>
  </a>
{% endfor -%}
</div>

## Que hay de nuevo en este sitio

**26 de Febrero, 2019 **

Flutter released [version
1.2](https://developers.googleblog.com/2019/02/launching-flutter-12-at-mobile-world.html)
today at Mobile World Congress (MWC) in Barcelona!

In addition, here are some new docs we've released:

* We've updated our [state management
  advice](/docs/development/data-and-backend/state-mgmt/intro).
  New pages include an
  [introduction](/docs/development/data-and-backend/state-mgmt/intro),
  [thinking declaratively](/docs/development/data-and-backend/state-mgmt/declarative), 
  [ephemeral vs app state](/docs/development/data-and-backend/state-mgmt/ephemeral-vs-app),
  [simple app state management](/docs/development/data-and-backend/state-mgmt/simple),
  and [different state management
  options](/docs/development/data-and-backend/state-mgmt/options).
  Documenting state management is a tricky thing, as there is no one-size-fits-all
  approach. We'd love your feedback on these new docs!
* A new page on [Performance best practices](/docs/testing/best-practices).
* Also at MWC, we announced a preview version of the new Dart DevTools
  for profiling and debugging Dart and Flutter apps. You can find the docs on the
  [DevTools wiki](https://flutter.github.io/devtools/).
  In particular, check out the DevTool's [Flutter widget
  inspector](https://flutter.github.io/devtools/inspector) for debugging
  your UI, or the [timeline
  view](https://flutter.github.io/devtools/timeline) for profiling your Flutter
  application. Try them out and let us know what you think!
* An update to the [Performance profiling](/docs/testing/ui-performance)
  page that incorporates the new Dart DevTools UI.
* Updates to the [Android
  Studio/IntelliJ](/docs/development/tools/android-studio)
  and [VS Code](/docs/development/tools/vs-code) pages incorporating info from
  the new Dart DevTools UI.

If you have questions or comments about any of these docs, [file an
issue]({{site.repo.this}}/issues).

[What's new archive](/docs/whats-new-archive)

## ¿Nuevo en Flutter?

Cuando ya has pasado por [Empezar](/docs/get-started/install),
incluyendo [Escribe tu Primera App Flutter,](/docs/get-started/codelab)
aquí hay algunos nuevos pasos.

[Flutter para desarrolladores Android](/docs/get-started/flutter-for/android-devs)
: Revisa estos consejos si tienes experiencia en Android.

[Flutter para desarrolladores iOS](/docs/get-started/flutter-for/ios-devs)
: Revsa estos consejos si tienes experiencia en iOS.

[Flutter para desarrolladores Web](/docs/get-started/flutter-for/web-devs)
: Revisa estas analogías HTML -> Flutter si tienes experiencia en web.

[Flutter para desarrolladores React Native](/docs/get-started/flutter-for/react-native-devs)
: Revisa estos consejos si tienes experiencia en React Native.

[Flutter para desarrolladores Xamarin.Forms](/docs/get-started/flutter-for/xamarin-forms-devs)
: Revisa estos consejos si tienes experiencia con Xamarin Forms.

[Construyes layouts en Flutter](/docs/development/ui/layout)
: Aprende como crear layouts en Flutter, donde todo es un widget.

[Añade interactividad a tu app Flutter](/docs/development/ui/interactive)
: Aprende como añadir un widget stateful a tu app.

[Un tour por el framework de widgets de Flutter](/docs/development/ui/widgets-intro)
: Aprende más sobre el framework estilo-reactivo de Flutter.

[FAQ](/docs/resources/faq)
: Obtén las respuestas a las preguntas más frecuentes.


## ¿Buscas mejorar tus conocimientos?

Una vez domines lo básico, prueba estas páginas.

[Cookbook](/docs/cookbook)
: Una (creciente) colección de recetas que cubren los casos de uso común en Flutter.

[Apps de ejemplo en GitHub]({{site.github}}/flutter/samples/blob/master/INDEX.md)
: Una (creciente) colección de apps de ejemplo que muestra las mejores prácticas en  Flutter.

[Añadir assets e imágenes en Flutter](/docs/development/ui/assets-and-images)
: Como añadir recursos a tu app Flutter.

[Animaciones en Flutter](/docs/development/ui/animations)
: Como crear animaciones estándar, hero o escalonadas, por nombrar algunos de los 
estilos de animación que soporta Flutter.

[Navegación y rutas](/docs/development/ui/navigation)
: Como crear y navegar a una pantalla nueva (lamada _ruta_ en Flutter).

[Internacionalización](/docs/development/accessibility-and-localization/internationalization)
: ¡Sé global! Como internacionalizar tu app Flutter.

[Dart Efectivo]({{site.dart-site}}/guides/language/effective-dart)
: Guía sobre escribir mejor código Dart.

## Temas especializados

Sumérgete más profundamente en temas de tu interés.

[Flutter Widget inspector](/docs/development/tools/inspector)
: Como usar el widget inspector, una poderosa herramienta que te permite
  explorar el árbol de widgets, desacrivar el banner
  "DEBUG", mostrar la capa supuerpuesta de rendimiento, y mucho más.

[Fuentes personalizadas](/docs/cookbook/design/fonts)
: Como añadir nuevas fuentes a tu app.

[Entrada de texto](/docs/cookbook/forms/text-input)
: Como configurar una entrada de texto básica.

[Depurar aplicaciones Flutter](/docs/testing/debugging)
: Herramientas y consejos para depurar tu app.

Esta no es una lista completa. Por favor usa la navigación izquierda,
o el campo de buesqueda para encontrar otros temas.
