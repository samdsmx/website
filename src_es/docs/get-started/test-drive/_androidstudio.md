<div class="tab-pane active" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## Crea la app {#create-app}

 1. Seleccionar **File > New Flutter Project**
 1. Seleccionar **Flutter application** como tipo de proyecto, y presionar **Next**.
 1. Asegúrate que el campo **Flutter SDK Path** especifica la localización 
    del SDK. Instala el SKD si no lo has hecho ya.
 1. Introducir nombre de proyecto (ej. `myapp`), y presionar siguiente.
 1. Clic en **Finish**.
 1. Espera mientras Android Studio instala el SDK, y crear el proyecto.

Los comandos de arriba crean un directorio para el proyecto llamado `myapp`
el cual contiene una app demo sencilla que utiliza [Material Components][].

{% include_relative _main-code-note.md  %}

## Ejecutar app

 1. Localiza la barra de herramientas principal de Android Studio:<br>
    ![Main IntelliJ toolbar][]{:.mw-100}
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
[Managing AVDs]: {{site.android-dev}}/studio/run/managing-avds
[Material Components]: {{site.material}}/guidelines
</div>
