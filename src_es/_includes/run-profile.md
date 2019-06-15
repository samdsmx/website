## Ejecutar en modo Profile

{{site.alert.important}}
  _No_ pruebes el rendimiento de tu app con el modo depuración y hot reload habilitados.
{{site.alert.end}}

La app que has ejecutado hasta ahora esta en modo depuración que permite un desarrollo más rapido (ej., hot reload) con una gran sobrecarga de rendimiento. Por tanto, puedes esperar animaciones lentas en este modo. Para ver como rinden las apps en modo release,  prueba {{include.ide_profile}} el siguiente comando en la terminal.

```terminal
$ flutter run --profile
```

Las animaciones deben ser mucho mas suaves comparadas con el modo depuración.