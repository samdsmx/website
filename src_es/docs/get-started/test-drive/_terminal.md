<div class="tab-pane" id="terminal" role="tabpanel" aria-labelledby="terminal-tab" markdown="1">

## Crea la app

Usa el comando `flutter create` para crear un nuevo proyecto:

```terminal
$ flutter create myapp
$ cd myapp
```

El comando crea un directorio de proyecto Fluuter llamado `myapp` que
contiene una app simple de demostración que usa 
[Material Components]({{site.material}}/guidelines/).

{% include_relative _main-code-note.md  %}

## Ejecuta la app

 1. Verifica que un dispositivo Android esté ejecutándose. Si no     se muestra ninguno, sigue las instrucciones específicas
    en la página [Instalar][] para tu SO.

    ```terminal
    $ flutter devices
    ```

 2. Ejecuta la app con el siguiente comando:

    ```terminal
    $ flutter run
    ```

{% capture save_changes -%}
.
1. Escribe <kbd>r</kbd> en la ventana de la terminal.
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}

[Install]: /docs/get-started/install
</div>
