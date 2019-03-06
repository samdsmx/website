---
title: Reportar errores a un servicio
prev:
  title: Trabajando con listas grandes
  path: /docs/cookbook/lists/long-lists
next:
  title: Animando un Widget entre pantallas
  path: /docs/cookbook/navigation/hero-animations
---

Aunque siempre tratamos de crear aplicaciones que estén libres de bugs, 
seguramente aparecerán de vez en cuando. Dado que las aplicaciones con bugs conducen a usuarios y 
clientes insatisfechos, es importante entender con qué frecuencia nuestros usuarios experimentan 
bugs y dónde ocurren. De esta manera, podemos priorizar los bugs con el mayor impacto y 
trabajar para corregirlos.

¿Cómo puedes determinar con qué frecuencia tus usuarios experimentan bugs? Cada vez que 
ocurre un error, puedes crear un informe que contenga el error que ocurrió y el stacktrace asociado. 
A continuación, puedes enviar el informe a un servicio de seguimiento de errores, 
como Sentry, Fabric, o Rollbar. 

El servicio de seguimiento de errores agregará todos las caídas que experimentan nuestros usuarios y 
las agrupará para nosotros. Esto nos permite saber con qué frecuencia nuestra aplicación falla y 
dónde se encuentran los problemas de nuestros usuarios. 

En esta receta, verás cómo informar errores al servicio de informes de fallas 
[Sentry](https://sentry.io/welcome/).

## Instrucciones

  1. Obtén un DSN de Sentry
  2. Importa el paquete Sentry
  3. Crea un `SentryClient`
  4. Crea una función para informar errores
  5. Captura e informa errores Dart
  6. Captura e informa errores Flutter

## 1. Obtén un DSN de Sentry

Antes de poder informar los errores a Sentry, necesitas un "DSN" para identificar de 
manera única tu aplicación con el servicio Sentry.io.

Para obtener un DSN, usa los siguientes pasos: 

  1. [Crea una cuenta con Sentry](https://sentry.io/signup/)
  2. Inicia sesión en la cuenta
  3. Crea una nueva app
  4. Copia el DSN 

## 2. Importa el paquete Sentry

Importar el paquete 
[`sentry`](https://pub.dartlang.org/packages/sentry) 
facilita el envío de informes al servicio de seguimiento de errores 
de Sentry.

```yaml
dependencies:
  sentry: <latest_version>
```

## 3. Crea un `SentryClient`

Crea un `SentryClient`. Usarás `SentryClient` para enviar informes 
de errores al servicio sentry!

<!-- skip -->
```dart
final SentryClient _sentry = SentryClient(dsn: "App DSN goes Here");
```

## 4. Crea una función para informar errores

¡Con Sentry listo, puedes comenzar a reportar errores! Como no querrás informar errores 
a Sentry durante el desarrollo, primero crearrás una función que nos permita saber 
si estas en modo debug o producción.

<!-- skip -->
```dart
bool get isInDebugMode {
  // Supongamos que estas en modo de producción
  bool inDebugMode = false;
  
  // Las expresiones assert solo se evalúan durante el desarrollo. 
  // Son ignoradas en producción. Por lo tanto, este código solo asignará true a `inDebugMode` 
  // en nuestros entorno de desarollo.
  assert(inDebugMode = true);
  
  return inDebugMode;
}
```   

A continuación, poduedes usar esta función en combinación con `SentryClient` 
para informar errores cuando la app está en modo de producción.

<!-- skip -->
```dart
Future<void> _reportError(dynamic error, dynamic stackTrace) async {
  // Imprime la excepción a la consola
  print('Caught error: $error');
  if (isInDebugMode) {
    // Imprime el stacktrace completo en modo de depuración
    print(stackTrace);
    return;
  } else {
    // Envía Exception y Stacktrace a Sentry en modo Producción
    _sentry.captureException(
      exception: error,
      stackTrace: stackTrace,
    ); 
  }
}
```

## 5. Captura e informa errores Dart

Ahora que tienes una función para informar errores según el entorno, necesitaa una 
forma de capturar los errores de Dart para poder informarlos. 

Para esta tarea, ejecuta tu aplicación dentro de una 
[`Zone`]({{site.api}}/flutter/dart-async/Zone-class.html) personalizada. Las Zonas 
establecen un contexto de ejecución para el código. Esto proporciona una forma conveniente 
de capturar todos los errores que ocurren dentro de ese contexto al proporcionar un 
función `onError`.

En este caso, ejecutarás la app en una nueva `Zone` y capturaremos todos los errores al 
proporcionar un callback `onError` .

<!-- skip -->
```dart
runZoned<Future<voird>>(() async {
  runApp(CrashyApp());
}, onError: (error, stackTrace) {
  // Siempre que ocurra un error, llama a la función `_reportError`. Esto envia
  // errors Dart a la consola de desarrollo o Sentry dependiendo del entorno.
  _reportError(error, stackTrace);
});
```

## 6. Captura e informa errores Flutter

Además de los errores de Dart, Flutter puede arrojar errores adicionales, como las excepciones de 
plataforma que se producen al llamar al código nativo. Debes asegurarte de capturar e informar 
también este tipo de errores.

Para capturar errores de Flutter, podemos sobrescribir la propiedad 
[`FlutterError.onError`]({{site.api}}/flutter/foundation/FlutterError/onError.html). 
En este caso, si estamos en modo de depuración, usaremos una función de Flutter para darle formato 
al error apropiadamente. Si estamos en modo de producción, enviaremos el error a nuestro 
callback `onError`  definido  en el paso anterior.  

<!-- skip -->
```dart
// Esto captura los errores informados por el framework Flutter.
FlutterError.onError = (FlutterErrorDetails details) {
  if (isInDebugMode) {
    // En modo de desarrollo, simplemente imprime en la consola..
    FlutterError.dumpErrorToConsole(details);
  } else {
    // En modo de producción, informa a la zona de aplicación para informar a
    // Sentry.
    Zone.current.handleUncaughtError(details.exception, details.stack);
  }
};
```

## Ejemplo completo

Para ver un ejemplo de trabajo, por favor mira la aplicación de ejemplo 
[Crashy]({{site.github}}/flutter/crashy) . 
