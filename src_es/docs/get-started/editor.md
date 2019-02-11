---
title: "Empezar: Configurar Editor"
prev:
  title: Instalar
  path: /get-started/install
next:
  title: Test drive
  path: /get-started/test-drive
toc: false
---

Puedes construir apps con Flutter utilizando cualquier editor de texto combinado con nuestras
herramientas en línea de comando. De cualquier manera, recomendamos utilizar alguno de nuestros 
plugin para una mejor experiencia. Con nuestros plugins de edición, podrás auto completar código, 
sintaxis resaltada, asistencia al editar widgets, apoyo para ejecutar & depurar, y más. 

Siga los siguientes pasos para agregar un complemento al editor para Androind Studio, IntelliJ 
o VS Code. Si quiere utilizar un editor diferente, está bien, simplemente salte al 
[siguiente paso: crear y ejecutar tu primer app](/get-started/test-drive/).

% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="androidstudio-tab" href="#androidstudio" role="tab" aria-controls="androidstudio" aria-selected="true">Android Studio / IntelliJ</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="vscode-tab" href="#vscode" role="tab" aria-controls="vscode" aria-selected="false">Visual Studio Code</a>
  </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div class="tab-content">

<div class="tab-pane active" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

### Instalar Android Studio

Android Studio ofrece una experiencia completa e integrada del IDE para Flutter. 

   * [Android Studio](https://developer.android.com/studio/index.html), version 3.0 o superior.

Otra opción puede ser utilizar IntelliJ:

   * [IntelliJ IDEA Community](https://www.jetbrains.com/idea/download/), version 2017.1 o superior.
   * [IntelliJ IDEA Ultimate](https://www.jetbrains.com/idea/download/), version 2017.1 o superior.

### Instalar los plugins de Flutter y Dart

Para instalar estos:

   1. Inicie Android Studio.
   1. Abra preferencias de complementos (**Preferences>Plugins** en macOS,
      **File>Settings>Plugins** en Windows & Linux).
   1. Seleccione **Browse repositories…**,  elige el complemento de Flutter y presione
      `install`.
   1. Presione `Yes` cuando aparezca para instalar el complemento de Dart.
   1. Presione `Restart` cuando aparezca.

</div>
<div class="tab-pane" id="vscode" role="tabpanel" aria-labelledby="vscode-tab" markdown="1">

### Instalar VS Code

VS Code es un editor ligero con Flutter, asistencia al ejecutar y depurar.

  * [VS Code](https://code.visualstudio.com/), La versión estable más reciente.

### Instalar el complemento de Flutter 

  1. Inicie VS Code
  1. Llame **View>Command Palette...**
  1. Teclee 'install', y seleccione la acción **'Extensions: Install Extension'**
  1. Introduzca `flutter` en el campo de busqueda, seleccione 'Flutter' en la lista, 
     y presione **Install**
  1. Seleccione 'OK' para recargar VS Code

## Valida tu configuración con Flutter Doctor

  1. Llame a **View>Command Palette...**
  1. Teclee 'doctor', y seleccione la acción **'Flutter: Run Flutter Doctor'** 
  1. Revise la salida en el panel 'OUTPUT' para cualquier inconveniente.

</div>

</div>{% comment %} End: Tab panes. {% endcomment -%}

## Siguiente paso

Hagamos una prueba: crea un proyecto, ejecútalo y 
experimenta 'hot reload'.
