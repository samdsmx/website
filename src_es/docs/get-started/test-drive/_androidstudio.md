<div class="tab-pane active" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## Crear una app nueva {#create-app}

 1. Seleccionar **File > New Flutter Project**
 1. Seleccionar **Flutter application** como tipo de proyecto, y presionar **Next**.
 1. Asegúrate que el campo **Flutter SDK Path** especifica la localización 
    del SDK. Instala el SKD si no lo has hecho ya.
 1. Introducir nombre de proyecto (ej. `myapp`), y presionar siguiente.
 1. Clic en **Finish**.
 1. Espera mientras Android Studio instala el SDK, y crear el proyecto.

Los comandos de arriba crean un directorio para el proyecto llamado `myapp`
el cual contiene una app demo sencilla que utiliza [Material Components][].

Dentro del directorio del proyecto, el código de tu app, esta en `lib/main.dart`.

## Ejecutar app

 1. Localiza la barra de herramientas principal de Android Studio:<br>
    ![Main IntelliJ toolbar][]
 1. En el **target selector**, selecciona un dispositivo android para ejecutar la app.
    Si ninguno esta en la lista como disponible, selecciona **Tools> Android > AVD Manager** y
    crea uno ahí mismo. Para más detalle, vea [Administrando AVDs][].
 1. Clic en el **Run icon** en la barra de herramientas, seleccionar **Run > Run** del menú.

{% capture save_changes -%}
  : invoke **Save All**, or click **Hot Reload**
  <i class="material-icons align-bottom">offline_bolt</i>.
  {% comment %} O, como una alternativa:
    {% asset 'get-started/hot-reload-button.png' alt='looks like a lightning bolt' %}.
  {% endcomment -%}
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}

[Main IntelliJ toolbar]: {% asset tools/android-studio/main-toolbar.png @path %}
[Managing AVDs]: https://developer.android.com/studio/run/managing-avds
[Material Components]: https://material.io/guidelines
</div>



  
   







   
   1. Si todo funciona, deberás de ver iniciar tu app en el dispositivo o 
      simulador:<br>
      ![App iniciada en Android](/images/flutter-starter-app-android.png)

## Probando hot reload

Flutter ofrece un ciclo de desarrollo rápido con _hot reaload_, la habilidad de recargar 
el código en una app ejecutando en vivo sin reiniciar o perder el estado de la app. simplemente
hace un cambio a tu código fuente, diciéndole a tu IDE o herramienta de línea de comandos que
quieres recargar, y ver los cambio en tu simulador, emulador, o dispositivo.

  1. Cambia el texto<br>`'You have pushed the button this many times:'`
     a<br>`'You have clicked the button this many times:'`
  1. No presione el botón de 'Stop'; permita que su app continué ejecutándose.
  1. Para ver tus cambios presione **Save All**, o clic
     **Hot Reload** (el botón con el icono del relámpago).

Deberás ver como el texto actualizado en la app ejecutándose casi inmediatamente.

