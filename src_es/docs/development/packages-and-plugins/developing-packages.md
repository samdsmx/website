---
title: Desarrollando paquetes & Plugins
short-title: Desarrollando
description: Como escribir paquetes y plugins para Flutter.
---

## Introducción a los paquetes

Los paquetes permiten la creación de código modular que puede ser compartido fácilmente. Un 
paquete mínimo consiste en:

* Un fichero `pubspec.yaml`: Un fichero de metadatos que declara el nombre del paquete,
  versión, autor, etc.

* Un directorio `lib` conteniendo el código público del paquete, como mínimo un 
  único fichero `<package-name>.dart`.

{{site.alert.note}}
  Para una lista de "que hacer" y "que no hacer" cuando escribimos un plugin óptimo,
  mira [Writing a good
  plugin]({{site.flutter-medium}}/writing-a-good-flutter-plugin-1a561b986c9c)
  en Medium.
{{site.alert.end}}

### Tipos de paquetes {#types}

Los paquetes pueden contener varios tipos de contenido:

* *Paquetes Dart*: Paquetes generales escritos en Dart, por ejemplo el 
  paquete [`path`]({{site.pub}}/packages/path). Algunos de estos 
  pueden contener funcionalidades específicas de Flutter y así tener una
  dependencia del framework Flutter, restringiendo su uso solo a Flutter, 
  por ejemplo el paquete [`fluro`]({{site.pub}}/packages/fluro).

* *Paquetes de Plugin*: Un paquete especializado de Dart que contiene una API 
  escrita en código Dart combinado con una implementación específica de plataforma 
  para Android (usando Java o Kotlin), y/o para iOS (usando ObjC o Swift). Un ejemplo 
  concreto es el paquete de plugin [`battery`]({{site.pub}}/packages/battery).

## Desarrollando paquetes Dart {#dart}

### Paso 1: Crea el paquete

Para crear un paquete Dart, usa la etiqueta `--template=package` con `flutter create`:

```terminal
$ flutter create --template=package hello
```

Esto crea un proyecto de paquete en la carpeta `hello/` con el siguiente
contenido especializado:

* `lib/hello.dart`:
   - El código Dart para el paquete.
* `test/hello_test.dart`:
   - Los [test unitarios](/docs/testing#unit-tests) para el paquete.

### Paso 2: Implementa el paquete

Para paquetes Dart puros, simplemente añade la funcionalidad dentro del fichero 
main dentro de `lib/<package name>.dart`, o en múltiples ficheros en el directorio `lib`.

Para probar el paquete, añade [test unitarios](/docs/testing#unit-tests)
en el directorio `test`.

Para detalles adicionales sobre como organizar 
el contenido de un paquete, mira la documentación 
en [Dart library
 package]({{site.dart-site}}/guides/libraries/create-library-packages).

## Desarrollar paquetes de plugin {#plugin}

Si quieres desarrollar un paquete que hace llamadas a APIs específicas de plataforma, 
necesitas desarrollar un paquete de plugin. Un paquete de plugin es una versión especializada 
de un paquete Dart, que además del contenido descrito anteriormente también contiene
implementaciones específicas de plataforma, escritas para Android (código Java o Kotlin) 
o para iOS (código Objective-C o Swift), o para ambas. La API se conecta con las
implementaciones específicas de plataforma usando 
[platform channels](/docs/development/platform-integration/platform-channels).

### Paso 1: Crea el paquete

Para crear el paquete de plugin, usa la opción `--template=plugin` con `flutter create`.

Usa la opción `--org` para especificar tu organización, usando notación de nombre de
dominio inversa. Este valor es usado en varios paquetes y bundle identifiers en el código
generado Android e iOS.

```terminal
$ flutter create --org com.example --template=plugin hello
```

Esto crea un proyecto de plugin en la carpeta `hello/` con el siguiente contenido 
especializado:

* `lib/hello.dart`:
   - La API dart para el plugin.
* <code>android/src/main/java/com/example/&#8203;hello/HelloPlugin.java</code>:
   - La implementación específica de la API del plugin.
* `ios/Classes/HelloPlugin.m`:
   - La implementación ,especifica de plataforma iOS, de la API del plugin.
* `example/`:
   - Una app Flutter app que dependa del plugin, e ilustra como usarlo.

Por defecto, el proyecto de plugin usa código Objective-C para iOS y 
código Java para Android. Si prefieres Swift o Kotlin, puedes especificar el 
lenguaje iOS usando `-i` y/o el lenguaje Android usando `-a`. Por ejemplo:

```terminal
$ flutter create --template=plugin -i swift -a kotlin hello
```

### Paso 2: Implementa el paquete {#edit-plugin-package}

Como un paquete de plugin, contiene código de diversas plataformas escritos en múltiples 
lenguajes de programación, son necesarios algunos pasos específicos para asegurar una 
experiencia adecuada.

#### Paso 2a: Define la API del paquete (.dart)

La API del paquete de plugin es definida en código Dart. Abre la carpeta principal `hello/`
en tu [editor Flutter favorito](/docs/get-started/editor). Localiza el fichero 
`lib/hello.dart`.

#### Paso 2b: Añade el código de la plataforma Android (.java/.kt)

Te recomendamos que edites el código Android usando Android Studio.

Después de editar el código de la plataforma Android en Android Studio, primero asegúrate que 
el código ha sido compilado al menos una vez (ej., ejecuta la app de ejemplo desde tu IDE/editor,
o en terminal ejecuta `cd hello/example; flutter build apk`).

A continuación,

1. Lanza Android Studio
1. Selecciona 'Import project' en el diálogo 'Welcome to Android Studio', o selecciona
'File > New > Import Project...'' en el menú, y selecciona el fichero 
`hello/example/android/build.gradle`.
1. En el diálogo 'Gradle Sync' , selecciona 'OK'.
1. En el diálogo 'Android Gradle Plugin Update', selecciona 'Don't remind me again
   for this project'.

El código de la plataforma Android de tu plugin está ubicado en 
<code>hello/java/com.example.hello/&#8203;HelloPlugin</code>.

Puedes ejecutar la app de ejemplo desde Android Studio presionando el botón &#9654;.

#### Paso 2c: Añade el código de la plataforma iOS (.h+.m/.swift)

Te recomendamos editar tu código iOS en Xcode.

Después de editar el código de la plataforma iOS en Xcode, primero asegúrate que el 
código ha sido compilado al menos una vez (ej., ejecuta la app de ejemplo desde tu IDE/editor,
o en una terminal ejecuta `cd hello/example; flutter build ios --no-codesign`).

A continuación,

1. Lanza Xcode
1. Selecciona 'File > Open', y selecciona el fichero `hello/example/ios/Runner.xcworkspace`.

El código de la plataforma iOS de tu plugin está ubicado en `Pods/Development
Pods/hello/Classes/` en Project Navigator.

Puedes ejecutar la app de ejemplo presionando el botón &#9654;.

#### Paso 2d: Conecta la API y el código de plataforma

Finalmente, necesitas conectar la API escrita en código Dart con las implementaciones 
específicas de plataforma. 
Esto se consigue usando [platform channels](/platform-channels/).

## Añadiendo documentación

Es una práctica recomendada añadir la siguiente documentación a todos los paquetes:

1. Un fichero `README.md` que hace de introducción al paquete
1. Un fichero `CHANGELOG.md` que documenta los cambios en cada versión
1. Un fichero [`LICENSE`](#adding-licenses-to-the-license-file) conteniendo los términos bajo los que el paquete es licensiado
1. Documentación de la API para todas las APIs públicas (mira abajo para más detalles)

### Documentación de API

Cuando publicas un paquete, la documentación de la API es generada y publicada automáticamente a 
dartdocs.org, mira por ejemplo la [documentación de device_info]({site.pub-api}}/device_info/latest).

Si quieres generar documentación de la API localmente en tu máquina de desarrollo 
, usa los siguientes comandos:

1. Cambia al directorio en que está ubicado tu paquete:

   `cd ~/dev/mypackage`

1. Di a la herramienta de documentación donde esta el SDK de Flutter (
  cámbialo para reflejar donde está ubicado):

   `export FLUTTER_ROOT=~/dev/flutter` (en macOS o Linux)

   `set FLUTTER_ROOT=~/dev/flutter` (en Windows)

1. Ejecuta la herramienta `dartdoc` (que viene como parte del SDK de Flutter):

   `$FLUTTER_ROOT/bin/cache/dart-sdk/bin/dartdoc` (en macOS o Linux)

   `%FLUTTER_ROOT%\bin\cache\dart-sdk\bin\dartdoc` (en Windows)

Para consejos sobre como escribir la documentación de la API, mira [Effective Dart: Documentation]({{site.dart-site}}/guides/language/effective-dart/documentation).

### Añadir licencias al fichero LICENSE

Cada licencia individual dentro del fichero LICENSE deben separarse por una 
de 80 guiones.

Si un fichero de LICENSE contiene más de una licensia de componente, cada 
licencia de componente debe empezar con el nombre del paquete al que se aplica 
esta licencia de componente, con cada nombre de paquete en su propia linea, y la 
lista de nombres de paquetes separadas del texto de la licencia actual por una línea 
en blanco. (Los paquete deben coincidir con el nombre del paquete en pub. Por
ejemplo, un paquete puede contener código de múltiples fuentes de terceros, y 
debe incluir una licencia para cada uno.)

Good:
```
package_1

<some license text>

--------------------------------------------------------------------------------
package_2

<some license text>
```

Also good:
```
package_1

<some license text>

--------------------------------------------------------------------------------
package_1
package_2

<some license text>
```

Bad:
```
<some license text>

--------------------------------------------------------------------------------
<some license text>
```

Also bad:
```
package_1

<some license text>
--------------------------------------------------------------------------------
<some license text>
```

## Publicando paquetes {#publish}

Una vez que has implementado un paquete, puedes publicarlo en 
[Pub]({{site.pub}}), para que otros desarrolladores 
puedan usarlo fácilmente.

Antes de publicar, asegúrate de revisar el `pubspec.yaml`, `README.md`, y
`CHANGELOG.md` para estar seguro de que su contenido está completo y es correcto.

A continuación, ejecuta el comando dry-run para ver si todos los análisis pasan correctamente:

```terminal
$ flutter pub pub publish --dry-run
```
(Fijate en la redundancia `pub pub`, que es necesaria mientras el [issue #33302)(https://github.com/flutter/flutter/issues/33302) es resuelto).

Finalmente, ejecuta el comando actual para publicar:

```terminal
$ flutter pub publish
```

Para detales sobre publicación, 
mira [Pub publishing 
docs](https://www.dartlang.org/tools/pub/publishing).

## Manejando interdependencias entre paquetes {#dependencies}

Si has desarrollado el paquete `hello` que depende de la API Dart expuesta por otro paquete,
necesitas añadir este paquete a la sección `dependencies` en tu fichero `pubspec.yaml`.
El código a continuación hace que la API Dart del plugin `url_launcher` esté disponible para `hello`:

En `hello/pubspec.yaml`:
```yaml
dependencies:
  url_launcher: ^0.4.2
```

Ahora puedes hacer `import 'package:url_launcher/url_launcher.dart'` y `launch(someUrl)` en 
el código Dart de `hello`.

Esto no es diferente de como incluyes paquetes en aplicaciones Flutter o en cualquier otro proyecto Dart.

Pero si `hello` resulta ser un paquete de _plugin_ cuyo código específico de plataforma necesita acceso al código 
específico de plataforma de la API expuesta por `url_launcher`, también necesitas añadir declaraciones 
de dependencia adecuadas a tus archivos de compilación específicos de plataforma, como mostramos abajo.

### Android

En `hello/android/build.gradle`:
```groovy
android {
    // varias líneas saltadas
    dependencies {
        provided rootProject.findProject(":url_launcher")
    }
}
```
Ahora puedes hacer `import io.flutter.plugins.urllauncher.UrlLauncherPlugin` y acceder a la clase 
`UrlLauncherPlugin` en el código fuente en `hello/android/src`.

### iOS

En `hello/ios/hello.podspec`:
```ruby
Pod::Spec.new do |s|
  # varias líneas saltadas
  s.dependency 'url_launcher'
```
Ahora puedes hacer `#import "UrlLauncherPlugin.h"` y acceder a la clase `UrlLauncherPlugin` 
en el código fuente en `hello/ios/Classes`.