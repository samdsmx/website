<div class="tab-pane" id="terminal" role="tabpanel" aria-labelledby="terminal-tab" markdown="1">

## Crea la app

Usa el comando `flutter create` para crear un nuevo proyecto:

```terminal
$ flutter create myapp
$ cd myapp
```

El comando crea un directorio de proyecto Fluuter llamado `myapp` que
contiene una app simple de demostración que usa 
[Material Components](https://material.io/guidelines/).

En el directorio del proyecto, el código de tu está en `lib/main.dart`.

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

{% include_relative _try-hot-reload.md %}

[Instalar]: /get-started/install
</div>
