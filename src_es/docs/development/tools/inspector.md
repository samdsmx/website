---
title: Inspector de widget de Flutter 
description: Una breve reseña del inspector de widgets de Flutter.
---

El Inspector de Widget de Flutter es una poderosa herramienta para visualizar y explorar 
el árbol de widgets en Flutter.

{{site.alert.note}}
  Si bien aun puede acceder a Flutter inspector desde Android Studio, este
  es ahora parte del nuevo [Dart DevTools](https://flutter.github.io/devtools).
  Puedes encontrar documentación más actualizada en
  [DevTools wiki](https://flutter.github.io/devtools/inspector)
{{site.alert.end}}

El framework Flutter usa widgets como el [elemento principal de 
construcción](/docs/development/ui/widgets-intro) para todo, desde controles (text, buttons, 
toggles, etc.) hasta layouts (centering, padding, rows, columns, etc.). 
El inspector es una poderosa herramienta de Flutter para visualizar y explorar 
los árboles de widgets. Puede ser útil para:

* Comprender los layouts existentes
* Diagnosticar problemas con layouts 

![IntelliJ Flutter inspector window]({% asset tools/android-studio/visual-debugging.png @path %})

## Empezando con el inspector

El inspector está actualmente disponible en [el plugin Flutter
](/docs/get-started/editor) para Android Studio, o IntelliJ IDEA.

Para iniciar haz clic en "Select widget" en la barra de herramientas del inspector de Flutter y después haz clic 
sobre el dispositivo, para seleccionar un widget. El widget seleccionado se resaltará 
en el dispositivo y en el árbol de widgets.

![Select Demo]({% asset tools/android-studio/inspector_select_example.gif @path %})

Luego puedes navegar por el árbol interactivo de widgets en el IDE para ver 
de cerca los widgets y los valores de sus atributos. Si estas tratando de 
depurar un problema de layout, en la capa del árbol de widgets los detalles 
pueden ser insuficientes. En ese caso haz clic sobre la pestaña Render 
Tree para ver el render tree correspondiente en el mismo lugar del árbol. 
Cuando depuras problemas, los atributos claves que buscas son `size` y `constraints`. 
Las restricciones fluyen por el árbol hacia abajo y los tamaños fluyen hacia arriba.

![Switch Trees]({% asset tools/android-studio/switch_inspector_tree.gif @path %})

Para una demostración más completa del Inspector, por favor vea el reciente 
[DartConf talk](https://www.youtube.com/watch?v=JIcmJNT9DNI).

## Feedback

Si tienes sugerencias, o encuentras problemas, por favor 
[reporta estos en nuestro issue tracker]({{site.github}}/flutter/flutter-intellij/issues/new?labels=inspector)!
