## Configurar Android (sin Android Studio)

### Instalar Java

```terminal
$ sudo apt update
$ sudo apt install default-jre
$ sudo apt install default-jdk
```

### Instalar el SDK de Android

Descarga el [Android SDK tools]({{site.android-dev}}/studio/#downloads) y 
selecciona la opción “Command Line Tools only”.

Arrastra y suelta el zip descargado en tu carpeta Linux Files la app 
Chrome OS Files. Esto lo mueve al directorio home, anotado como $TOOLS_PATH 
(`~/`).

Descomprime los tools y despues añádelo a tu path.

```terminal
$ unzip ~/sdk-tools-linux*
$ export PATH="$PATH:$TOOLS_PATH/tools/bin"
```

Navega a donde quieres almacenar los paquetes del SDK ($PLATFORM_PATH en este resumen) y 
descarga los paquetes del SDK usando la sdkmanager tool (los números de version aquí 
son los últimos cuando se publicó este artículo):

```terminal
$ sdkmanager "build-tools;28.0.3" "emulator" "tools" "platform-tools" 
"platforms;android-28" "extras;google;google_play_services" 
"extras;google;webdriver" "system-images;android-28;google_apis_playstore;x86_64"
```

Añade las Android platform tools a tu path (debes encontrar esto donde ejecutaste el comando 
sdkmanager: $PLATFORM_PATH):

```terminal
$ export PATH="$PATH:$PLATFORM_PATH/platform-tools
```

Configura la variable ANDROID_HOME a donde hayas descomprimido las sdk-tools antes (tu 
$TOOLS_PATH):

```terminal
$ export ANDROID_HOME="$TOOLS_PATH"
```

Ahora, ejecuta flutter doctor para aceptar las licencias android:

```terminal
$ flutter doctor --android-licenses
```