---
title: Flutter para web
description: "Anuncio del preview release de Flutter para web."
---

Flutter para web es una implementación de Flutter que se renderiza 
usando tecnología web basada en estándares: HTML, CSS y JavaScript.
Con Flutter para web, puedes compilar código existente de Flutter 
escrito en Dart en una experiencia de usuario que se puede 
incrustar en el navegador e implementar en cualquier servidor web. 
Puedes utilizar todas las funcionalidades de Flutter,
y no necesitas un plug-in para el navegador.

Flutter para web esta actualmente disponible como technical preview.

Revisa el [repositorio](https://github.com/flutter/flutter_web)
para las instrucciones sobre como empezar.

<img src="/images/Dart-framework-v-browser-framework.png"
     alt="showing Flutter architecture for C++ vs Flutter for web"
     width="100%">

Añadir soporte web implica implementar la capa de dibujado principal de 
Flutter sobre las APIs estandar del navegador. Usando una combinación de DOM, Canvas,
y CSS, podemos ofrecer una experiencia de usuario de alta calidad y buen rendimiento, 
para los navegadores modernos. Hemos implementado este capa de dibujado 
principal completamente en Dart y usado el compilador JavaScript optimizado 
de Dart para compilar el core y el framework de Flutter junto con tu 
aplicación en un único fichero fuente, minificado, que puede ser 
desplegado en cualquier servidor web.

En esta etapa temprana de desarrollo, prevemos que Flutter para la web 
sea valioso en muchos escenarios. Por ejemplo:

* **Una [Progressive Web
  Application](https://developers.google.com/web/progressive-web-apps/)
  conectada, construida con Flutter.** El soporte web para Flutter permite que 
  las aplicaciones basadas en movil se empaqueten como una PWA para llegar 
  a una variedad más amplia de dispositivos o para proporcionar una experiencia web 
  complemantaria a una aplicación web existente.

* **Contenido interactivo incrustado.** Flutter proporciona un poderoso entorno 
  para crear ricos componentes ,basados en datos, que pueden ser facilmente 
  alojados en una web existente. Ya sea para visualización de datos, una herramienta 
  online como un cofigurador para coches, o un gráfico incrustado, Flutter puede 
  proporcionar un enfoque de desarrollo productivo para contenido web incrustrado.

* **Incorporar contenido dinámico en una aplicación móvil Flutter.** Una forma 
  establecida de proporcionar actualizaciones de contenido dinámico dentro de una 
  aplicación móvil existente es el uso de un web view, que puede cargar y mostrar 
  información de forma dinámica. El soporte unificado que ahora Flutter ofrece 
  para un entorno unificado para contenido web y móvil te permite desplegar 
  contenido online o incrustrado en una app sin reescribir el código.

## Notas sobre la technical preview

La technical preview de Flutter para web es tu oportunidad para probar nuestro trabajo.
Antes de empezar, aquí hay algunas notas:

* Hemos desarrolado Flutter para web en un fork del repo de Flutter. Esto permite 
  rápidas iteraciones mientras mantenemos el core de Flutter estable.

* Ya hemos empezado el trabajo para unir el código web en el repositorio core. 
  Eventualmente, tendremos un único SDK/Framework con un conjunto de 
  widgets que funcionan en todas las plataformas.

* Las APIs de widgets de Flutter son idénticas a la APIs para móvil, pero
  están empaquetadas por separado temporalmente.

* Puedes re-empaquetar código existente de Flutter para hacerlo funcionar en 
  web, pero hay algunas advertencias mientras estemos en preview. Revisa las 
  [instrucciones](https://github.com/flutter/flutter_web#) para más detalles.

Ve al repositorio [flutter web](https://github.com/flutter/flutter_web)
para empezar. Revisa los 
[ejemplos de Flutter para web](https://flutter.github.io/samples/).
Gracias por 
[enviar cualquier problema](https://goo.gle/flutter_web_issue) que encuentres.
Tambien puedes escribirnos cualquier pregunta por chat en nuestro
[canal Gitter](https://gitter.im/flutter/flutter_web).