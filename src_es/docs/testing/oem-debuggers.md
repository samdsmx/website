---
title: Usando un depurador OEM
short-title: depuradores
description: Como conectar un depurador OEM, como es Xcode, a tu app Flutter en ejecución.
---

{{site.alert.tip}}
  Este documento aún no está completo. Las intrucciones para iOS están en proceso.
{{site.alert.end}}

Si estas escribiendo exclusivamente apps en Flutter con código Dart y no usas
bibliotecas especificas de la plataforma, o accediendo de otro modo a funcionalidades especificas 
de la plataforma, puedes depurar tu código usando el depurador de tu IDE
Solo la primera sección de esta guía, Depurando código Dart, es relevante para ti.

Si estas escribiendo un plugin especifico de plataforma o usando bibliotecas especificas 
de plataforma escritas en Swift, ObjectiveC, Java, o Kotlin, puedes depurar 
esta parte de tu código usando Xcode (para iOS) o Android Gradle (para Android).
Esta guia muestra como puedes conectar _dos_ depuradores a tu app Dart,
uno para Dart, y one para el código OEM.

## Depurando código Dart

Usa tu IDE para depuración Dart estándar. Estas instrucciones describen Android
Studio, pero puedes usar tu IDE preferido con los plugins para Flutter y Dart
instalados y configurados.

{{site.alert.tip}}
  Conecta a un dispositivo físico cuando depure, mejor que un emulador o 
  simulador, que no soportan modo profile. Para más información, mira
  [modos de Flutter]({{site.github}}/flutter/flutter/wiki/Flutter's-modes).
{{site.alert.end}}

### Dart debugger

* Abre tu proyecto en Android Studio. Si aún no tienes un proyecto,
  crea uno usando las instrucciones en [Test drive](/docs/get-started/test-drive).

* Simultáneamente abre el panel Debug y ehecyta la app en la vista Console
  pulsando en el icono bug 
  ({% asset 'testing/debugging/oem/debug-run.png' alt='Debug-run icon' %}).

  La primera vez que lanzas tu app es la más lenta.
  Deberías ver aparecer el panel Debug en la parte inferior de la ventana que 
  se ve algo parecido a lo siguiente:

  {% asset 'testing/debugging/oem/debug-pane.png' alt='Debug pane' %}

  Puedes configurar donde aparece el panel de debug, o incluso arrastralo a 
  su propia ventana usando el engranaje a la derecha en la barra del panel Debug.
  Esto es cierto para cada inspector en Android Studio.

* Añade un punto de interrupción en la línea `counter++`.

{% comment %}
No necesario para que los puntos de interrupción funcionen.
* Haz Hot reload de la app.
  {% asset 'get-started/hot-reload-button.png' alt='parece un rayo' %}
{% endcomment -%}

* En la app, haz click en el botón **+** (FloatingActionButton, o FAB, para resumir)
  para incrementar el contador. La app se pausa.

* Las siguientes capturas de pantalla muestran:
  * El punto de interrupción en el panel editor.
  * Estado de la app en el panel debug, cuando pausa en el punto de interrupción.
  * La variable `this` expandida para mostrar estos valores.

  {% asset 'testing/debugging/oem/debug-pane-action.png' alt='Estado de la app cuando alcanza el puntu de interrupción fijado' %}

Puedes saltar en, fuera o por encima de las declaraciones Dart, hacer hot reload o reanudar la app,
y usar el depurador de la misma manera que usarías cualquier depurador.
El botón **5: Debug** alterna que se muestre el panel debug.

### El inspector Flutter

Hay otras dos funcionalidades proporcionadas por el plugin Flutter que puedes encontrar 
util. El inspector Flutter es una herramienta para visualizar y explorar el 
árbol de widget de Flutter y ayudarte a:

* Entender los layouts existentes
* Diagnosticar problemas de layout

Alterna mostrar el inspector usando el botón vertical a la 
derecha de la ventana de Android Studio.

{% asset 'testing/debugging/oem/flutter-inspector.png' alt='Flutter inspector' %}

### Flutter outline

Flutter Outline muestra el método build de forma visual.
Nota que esto puede ser diferente que el árbol de widgets para 
el método build. Alterna mostrar el outline usando el botón vertical
a la derecha de la ventan de Android Studio.

{% asset 'testing/debugging/oem/flutter-outline.png' alt='captura de pantalla mostrando el inspector Flutter' %}

{% comment %}
TODO: Android Tips - How to assign a keyboard shortcut on the Mac?
{% endcomment %}

El resto de esta guía muestra como configurar tu entorno para depurar código OEM. 
Como esperarías, el proceso funciona diferente para iOS y Android.

{% comment %}
Considere moving the info below to a new page.
{% endcomment %}

{{site.alert.tip}}
  Conviertete en usuario pro de Android Studio instalando el plugin **Presentation
  Assistant**. Puedes encontrar e instalar este plugin usando 
  **Preferences** > **Browsing repositories...** y empezar a escribir
  _Presen_ en el campo de busqueda.

  Una vez instalado AS se reinicia, este plugin te ayuda a convertite en un usuario 
  pro:

  * Mostrándote el nombre y el atajo de teclado Windows/Linux/Mac de cualquier acción que 
    invoques.
  * Permitiéndote buscar y encontrar acciones, configuraciones, documentos, 
    y más disponibles.
  * Permitiéndote alternar preferencias, abrir vista, ejecutar acciones.
  * Permitiéndote asignar atajos de teclado (?? No puedo hacer que esto funcione en Mac.)

  Por ejemplo, prueba esto:

  * Mientras el foco esta en el panel Editor, pulsa **command-Shift-A** (Mac) or 
    **shift-control-A** (Windows y Linux).
    El plugin simultaneamente abre el panel de busqueda y muestra un consejo para 
    realizar esta mima operación en las tres plataformas.

    {% asset 'testing/debugging/oem/presentation-assistant-find-pane.png' alt='Find panel' %}
    Panel de busqueda de Presentation assistant

    {% asset 'testing/debugging/oem/presentation-assistant-teaches.png' alt='Find pane' %}
    Consejos de acciones de Presentation assistant para abrir este panel de busqueda 
    en Mac, Windows y Linux

  * Introduce _attach_ para ver lo siguiente:

    {% asset 'testing/debugging/oem/presentation-assistant-search-results.png' alt='Find panel' %}

  * Después de una actualización, puedes introducir _Flutter_ o _Dart_ para ver si hay nuevas acciones
    disponibles.

  Oculta el panel de busqueda de Presentation Assistant usando **Escape**.
{{site.alert.end}}


## Depurando con Android Gradle (Android)

Para depurar el código OEM de Android, necesitas una app que contenga código OEM de Android.
En esta sección, aprenderás como conectar dos depuradores a tu app: 1) el depurador 
Dart y, 2) el depurador Gradle de Android.

* Crea una app básica con Flutter.

* Reemplaza `lib/main.dart` con el siguiente código del 
paquete
[`url_launcher`]({{site.pub}}/packages/url_launcher):

{% prettify dart %}
// Copyright 2017 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

import 'dart:async';

import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'URL Launcher',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'URL Launcher'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  Future<void> _launched;

  Future<void> _launchInBrowser(String url) async {
    if (await canLaunch(url)) {
      await launch(url, forceSafariVC: false, forceWebView: false);
    } else {
      throw 'Could not launch $url';
    }
  }

  Future<void> _launchInWebViewOrVC(String url) async {
    if (await canLaunch(url)) {
      await launch(url, forceSafariVC: true, forceWebView: true);
    } else {
      throw 'Could not launch $url';
    }
  }

  Widget _launchStatus(BuildContext context, AsyncSnapshot<void> snapshot) {
    if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    } else {
      return Text('');
    }
  }

  @override
  Widget build(BuildContext context) {
    String toLaunch = 'https://flutter.io';
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Padding(
              padding: EdgeInsets.all(16.0),
              child: Text(toLaunch),
            ),
            RaisedButton(
              onPressed: () => setState(() {
                    _launched = _launchInBrowser(toLaunch);
                  }),
              child: Text('Launch in browser'),
            ),
            Padding(padding: EdgeInsets.all(16.0)),
            RaisedButton(
              onPressed: () => setState(() {
                    _launched = _launchInWebViewOrVC(toLaunch);
                  }),
              child: Text('Launch in app'),
            ),
            Padding(padding: EdgeInsets.all(16.0)),
            FutureBuilder<void>(future: _launched, builder: _launchStatus),
          ],
        ),
      ),
    );
  }
}
{% endprettify %}

* Añade la dependencia `url_launcher` al fichero pubspec,
  y ejecuta flutter packages get:

{% prettify yaml %}
name: flutter_app
description: A new Flutter application.
version: 1.0.0+1

dependencies:
  flutter:
    sdk: flutter

  [[highlight]]url_launcher: ^3.0.3[[/highlight]]
  cupertino_icons: ^0.1.2

dev_dependencies:
  flutter_test:
    sdk: flutter
{% endprettify %}

* Haz clic en el icono de depurar
  ({% asset 'testing/debugging/oem/debug-run.png' alt='Debug-run icon' %})
  para simultaneamente abrir el panel Debug y lanzar la app.
  Espera que la app se inicie en el dispositivo, y a que el panel debug 
  indique **Connected**.
  (Esto puede tomar un minuto la primera vez pero es más rápido en los siguientes
   lanzamientos.) La app contiene dos botones: 1) **Launch in browser**
   abre flutter.io en el navegador predeterminado de tu teléfono y 2) **Launch
   in app** abre flutter.io dentro de tu app.

  {% asset 'testing/debugging/oem/launch-flutter-io.png' alt='captura de pantalla conteniendo dos botones para abrir flutter.io' %}

* Haz clic en el botón **Attach debugger to Android process** (
  {% asset 'testing/debugging/oem/attach-process-button.png' alt='se ve como un rectángulo con un pequeño escarabajo verde sobrepuesto' %} )

{{site.alert.tip}}
  Si este botón no aparece en la barra de menú Projects menu bar, asegúrate que estas
  dentro de un proyecto Flutter pero no en un <em>plugin de Flutter</em>.
{{site.alert.end}}

* En el diálogo process, deberías ver una entrada para cada dispositivo conectado.
  Selecciona **show all processes** para mostrar los proceso disponibles por cada
  dispositivo.

* Elige el proceso que quieras vincular. En este caso, es el
  `com.google.clickcount`
   (o <strong>com.<em>company</em>.<em>app_name</em></strong>)
   process for the Motorola Moto G.

  {% asset 'testing/debugging/oem/choose-process-dialog.png' alt='captura de pantalla conteniendo dos botones para abrir flutter.io' %}

*  En el panel debug, deberías ahora ver un tab para **Android Debugger**.

* En el panel de proyecto, expande
  <strong><em>app_name</em> > android > app > src > main > java > io.flutter plugins</strong>.
  Haz doble click en **GeneratedProjectRegistrant** para abrir el código Java en el panel de edición.

Ambos depuradores, el de Dart y el OEM, están interactuando con el mismo proceso.
Usa uno de ellos, o ambos, para fijar puntos de interripción, examinar el stack, reanudar la ejecución...
En otras palabras, DEPURAR!

  {% asset 'testing/debugging/oem/dart-debugger.png' alt='caputra de pantalla de Android Studio en el panel de debug Dart.' %}
  El panel de depuración de Dart con dos puntos de interrupción fijados en `lib/main.dart`.

  {% asset 'testing/debugging/oem/android-debugger.png' alt='captura de pantalla de Android Studio en el panel de debug Android.' %}
  El panel de depuración Android con un punto de interrupción fijado en 
   `GeneratedPluginRegistrant.java`.
  Alterna entre los depuradores pulsando el depurador apropiado en la banda del 
   panel Debug.

## Depurar con Xcode (iOS)

Para depurar código OEM en iOS, necesitas una app que contenga código OEM iOS.
En esta sección, aprenderás como conectar dos depuradores a tu app: 1) el depurador
de Dart y, 2) el depurador de Xcode.

## Recursos

Los siguientes recursos tienen más información sobre depuración en Flutter,
iOS, y Android:

### Flutter

* [Depurando Apps Flutter](/docs/testing/debugging)
* [Depuración avanzada](/docs/development/tools/android-studio#advanced-debugging), a section in
  [Desarrollar Apps Flutter en un IDE](/docs/development/tools/android-studio).
* [Perfiles de Rendimiento](/docs/testing/ui-performance)

### Android

Puedes encontrar los siguientes recursos de depuración en 
[developer.android.com]({{site.android-dev}}).

* [Depura tu app]({{site.android-dev}}/studio/debug)
* [Android Debug
  Bridge (adb)]({{site.android-dev}}/studio/command-line/adb)

### iOS

Puedes encontrar los siguientes recuros de depuración en 
[developer.apple.com](https://developer.apple.com).

* [Depurar](https://developer.apple.com/support/debugging/)
* [Instruments Help](https://help.apple.com/instruments/mac/current/)
