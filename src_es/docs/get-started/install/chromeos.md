---
title: Instalación en Chrome OS
short-title: Chrome OS
# js: [{defer: true, url: /assets/archive.js}]
next:
  title: Configura un editor
  path: /docs/get-started/editor
---

{% assign os = 'linux' -%}

## Requisitos del sistema

Para instalar y ejecutar Flutter, tu entorno de desarrollo debe tener estos 
requisitos mínimos:

- **Sistema Operativo**: Linux (64-bit)
- **Espacio en Disco**: 600 MB (no incluye el espacio en disco para IDE/herramientas).
- **Herramientas**: Flutter depende de que estas herramientas de linea de comando estén 
disponibles en tu entorno.
  - `bash`
  - `curl`
  - `git` 2.x
  - `mkdir`
  - `rm`
  - `unzip`
  - `which`
  - `xz-utils`
- **Librerías compartidas**: el comando `test` de Flutter necesita que esta librería 
este disponible en tu entorno.
  - `libGLU.so.1` - proporcionado por mesa packages e.g. `libglu1-mesa` en Ubuntu/Debian

Para la mejor experiencia por ahora, deberías poner tu dispositivo Chrome OS en 
modo desarrollador (esto es necesario para instalar apps en tu dispositivo Chrome OS). Para 
más información, mira [como habilitar el modo desarrollador en tu Chromebook](https://www.androidcentral.com/how-enable-developer-mode-chrome-os).

{% include_relative _get-sdk.md %}

{% include_relative _path-mac-linux.md %}

{% include_relative _chromeos-android-sdk-setup.md %}

## Nuevo paso

[Nuevo paso: Configurar el editor](/docs/get-started/editor)

## Flutter y Chrome OS, Consejos y Trucos

¿Cómo ejecutar tu apliación? En Chrome OS, puedes conectar tu teléfono 
(actualmente solo en Dev channel) o lánzalo directamente al contenedor del dispositivo Android. 
Para hacer esto debes habilitar el modo desarrollador en tu máquina, y después conectar 
al contenedor local con ADB:

```terminal
$ adb connect 100.115.92.2:5555
```

¿Quieres construir tu primera app optimizada para Chrome OS? Clona el repositorio flutter-samples 
y compila nuestro ejemplo específico de Mejores Prácticas en Chrome OS:

```terminal
$ git clone https://github.com/flutter/samples
$ cd samples/chrome-os-best-practices
$ flutter run
```

¿Te preguntas como acceder a tus accesos directos favoritos con la tecla de función en 
el teclado de Chrome OS?
* Presiona la tecla de busqueda junto con 1 para acceder a F1–F12.

Para la versión actual de Chrome OS, solo ciertos puertos de Crostini son 
expuestos al resto del entorno. Aquí hay un ejemplo de como lanzar  
Flutter DevTools para una app Android con puertos que funcionará:

```terminal
$ flutter pub global run devtools -p 8000
$ cd path/to/your/app
$ flutter run --observatory-port=8080
```

Entonces, navega a http://localhost:8000/?port=8080 en tu navegador Chrome.

#### Flutter Chrome OS Lint Analysis

El equipo de Flutter esta añadiendo Lint Analysis para Chrome OS comprueba que están 
disponibles para asegurar que la app que estas construyecndo funcionará bien 
en Chrome OS. Esto buscas cosas como el hardware requerido en tu Android 
Manifest que no está disponible en dispositivos Chrome OS, permisos que implicarán 
solocitudes de hardware no soportado, así como otras propiedades o código 
que brinden una experiencia menor en estos dispositivos.

Para activar esto, actualiza o crea tu fichero analysis_options.yaml 
para incluir estas opciones:


```yaml
include: package:flutter/analysis_options_user.yaml
analyzer:
 optional-checks:
   chrome-os-manifest-checks
```

Para ejecutar esto desde la línea de comandos:

```terminal
$ flutter analyze
```

La salida de esto puede verse como:

```terminal
Analyzing ...                                                      
warning • This hardware feature is not supported on Chrome OS • 
android/app/src/main/AndroidManifest.xml:4:33 • unsupported_chrome_os_hardware
```


Esta funcionalidad esta aún en desarrollo, vuelve aquí para ver instrucciones sobre 
como puedes hacer esta funcionalidad funcionar con tu Chrome OS como objetivo de tus apps 
Flutter próximamente.
