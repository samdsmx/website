Después de que el compilado de la app se complete, verás la app inicial en tu dispositivo.

{% include app-figure.md img-class="site-mobile-screenshot border" image="starter-app.png" caption="Starter app" platform="iOS" %}

## Probando el hot reload

Flutter ofrece un ciclo de desarrollo rápido con _hot reload_, la habilidad de recargar 
el código en una app ejecutando en vivo sin reiniciar o perder el estado de la app. simplemente
hace un cambio a tu código fuente, diciéndole a tu IDE o herramienta de línea de comandos que
quieres recargar, y ver los cambio en tu simulador, emulador, o dispositivo.

 1. Open `lib/main.dart`.
 1. Cambia el texto
    <code class="text-nowrap">
    'You have <del>pushed</del> the button this many times'
    </code>
    a
    <code class="text-nowrap">
      'You have <ins>clicked</ins> the button this many times'
    </code>.

    {{site.alert.important}}
      No presione el botón de 'Stop'; permita que su app continué ejecutándose.
    {{site.alert.end}}

  1. Guarda tus cambios{{include.save_changes}}

  Verás el texto actualizado en la app en ejecución casí inmediatamante.
