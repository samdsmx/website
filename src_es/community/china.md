---
title: Usar Flutter en China
description: Como encontrar una version del sitio de Flutter que este traducida a Chino Simplificado.
toc: true
---

{% assign path = 'flutter_infra/releases/stable/windows/flutter_windows_v1.0.0-stable.zip' -%}

La comunidad Flutter ha confeccionado una version en Chino Simplificado del 
sitio web de Flutter disponible en
[https://flutter-io.cn](https://flutter-io.cn).

Si prefieres instalar Flutter usando un [installation
bundle](/docs/development/tools/sdk/archive),
puedes reemplazar la url del dominio original con un mirror verificado para 
aceleralo. Por ejemplo:

* Original URL:<br>
  [`https://storage.googleapis.com/{{path}}`](https://storage.googleapis.com/{{path}})

* Mirrored URL:<br>
  [`https://storage.flutter-io.cn/{{path}}`](https://storage.flutter-io.cn/{{path}})

Debes también configurar dos variables de entorno para actualizar Flutter y usar el repositorio de paquetes pub
en China. Las instrucciones están abajo.

{{site.alert.important}}
  Usa sitios mirror solo si has _verificado_ el proveedor.
  El equipo de Flutter no puede verificar su fiabilidad o seguridad.
{{site.alert.end}}

## Configurar Flutter para usar un sitio mirror

Si estas instalando o usando Flutter en China, puede ser útil usar 
un sitio mirror local digno de confianza que hospeda las dependencias de Flutter.
Para instruir a la herramienta Flutter para usar una ubicación alternativa de 
almacenamiento, necesitas configurar dos variables de entorno, `PUB_HOSTED_URL` y
`FLUTTER_STORAGE_BASE_URL`, antes de ejecutar el comando `flutter`.

Tomando MacOS o Linux como un ejemplo, aquí están los primeros pasos en 
el proceso de configuración para usar un sitio mirror. Ejecuta lo siguiente en una 
terminal Bash desde el directorio en el que desees guardar tu copia local de Flutter:


```terminal
$ export PUB_HOSTED_URL=https://pub.flutter-io.cn
$ export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
$ git clone -b dev {{site.github}}/flutter/flutter.git
$ export PATH="$PWD/flutter/bin:$PATH"
$ cd ./flutter
$ flutter doctor
```

Después de estos pasos, deberías porder continuar
[configurando Flutter](/docs/get-started/editor) normalmente.
Desde aquí cuando, los paquetes sean obtenidos por `flutter packages get` serán 
descargados desde `flutter-io.cn` en cualquier terminal en la que `PUB_HOSTED_URL`
y `FLUTTER_STORAGE_BASE_URL` estén configurados.

El servidor `flutter-io.cn` es un mirror provisional para dependencias y paquetes 
de Flutter mantenido por [GDG China]().
El equipo de Flutter no garantiza la disponibilidad a largo plazo de este servicio.
Eres libre de usar otros mirrors si estos están disponibles. Si estas interesado
en configurar tu propio mirror en China, contacta
[flutter-dev@googlegroups.com](mailto:flutter-dev@googlegroups.com)
para obtener asistencia.

{{site.alert.secondary}}
  **Error conocido:** Ejecutar la app Flutter Gallery desde la fuente requiere assets hospedados en
  un dominio que esta solución actualmente no soporta. Puedes suscribirte 
  al [Issue #13763]({{site.github}}/flutter/flutter/issues/13763)
  para recibir actualizaciones. Mientras tanto, puedes obtener Flutter Gallery
  desde Google Play o terceras app stores de tu confianza.
{{site.alert.end}}

## Community-run mirror sites

* Shanghai Jiaotong University Linux User Group
  * `FLUTTER_STORAGE_BASE_URL`: [https://mirrors.sjtug.sjtu.edu.cn/](https://mirrors.sjtug.sjtu.edu.cn)
  * `PUB_HOSTED_URL`: [https://dart-pub.mirrors.sjtug.sjtu.edu.cn/](https://dart-pub.mirrors.sjtug.sjtu.edu.cn)
