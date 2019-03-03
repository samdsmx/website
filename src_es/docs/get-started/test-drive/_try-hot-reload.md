Después de que el compilado de la app se complete, verás la app inicial en tu dispositivo.

{% include app-figure.md img-class="site-mobile-screenshot border"
    path-prefix="get-started" platform="iOS" image="starter-app.png"
    caption="Starter app" %}

## Probando el hot reload

Flutter ofrece un ciclo de desarrollo rápido con _hot reload_, la habilidad de recargar 
el código en una app ejecutando en vivo sin reiniciar o perder el estado de la app. simplemente
hace un cambio a tu código fuente, diciéndole a tu IDE o herramienta de línea de comandos que
quieres recargar, y ver los cambio en tu simulador, emulador, o dispositivo.

 1. Open `lib/main.dart`.
 1. Cambia el texto
     {% prettify dart %}
      'You have [[strike]]pushed[[/strike]] the button this many times'
    {% endprettify %}
    a
     {% prettify dart %}
      'You have [!clicked!] the button this many times'
    {% endprettify %}

    {{site.alert.important}}
      Do _not_ stop your app. Let your app run.
    {{site.alert.end}}

  1. Guarda tus cambios{{include.save_changes}}

  Verás el texto actualizado en la app en ejecución casí inmediatamante.
