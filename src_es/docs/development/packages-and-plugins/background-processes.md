---
title: Procesado en segundo plano
description: Donde encontrar más información sobre como implementar procesos en segundo plano en Flutter.

---

¿Alguna vez has querido ejecutar código Dart en segundo plano, incluso si tu app no es 
la app que está actualmente activa? Quizas querías implementar un
proceso que vigila el tiempo, o caputra los movimientos de la camara.
En Flutter, puedes ejecutar código Dart en segundo plano.

El mecanismo para esta funcionalidad implica configurar un isolate. _Isolates_
son el modelo de Dart para el multihilo, aunque un isolate difiere 
de un hilo convencional en que este no comparte memoria con el programa principal.
Configurarás tu isolate para ejecución en segundo plano usando callbacks y un 
callback dispatcher.

Para más información y un ejemplo de geofencing que usa ejecución en segundo plano 
de código Dart, mira [Ejecutar Dart en Segundo Plano con
Plugins Flutter y
Geofencing]({site.flutter-medium}}/executing-dart-in-the-background-with-flutter-plugins-and-geofencing-2b3e40a1a124),
un artículo en las Publicaciones de Flutter en Medium. Al final de este artículo,
encontrarás enlaces al código de ejemplo, y documentación relevante para Dart, 
iOS, y Android.

