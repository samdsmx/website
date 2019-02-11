<!DOCTYPE html>
<html lang="es">
  <head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Perfiles de rendimiento  - Flutter </title>
  <link rel="shortcut icon" href="/images/favicon.png">
  
  <!-- Google Tag Manager -->
  <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src= 'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-K5CZXWP');
  </script>
  <!-- End Google Tag Manager -->

  
  <meta name="description" content="Diagnóstico de problemas de rendimiento de la UI en Flutter.">
  <meta name="keywords" content=" ">

  <meta name="twitter:card" content="summary">
  <meta name="twitter:site" content="@EsFlutter">

  <meta property="og:title" content="Perfiles de rendimiento">
  <meta property="og:url" content="https://flutter.io/ui-performance/">
  <meta property="og:description" content="Diagnóstico de problemas de rendimiento de la UI en Flutter.">
  
  <meta property="og:image" content="https://flutter.io/images/flutter-logo-sharing.png">

  <link rel="stylesheet" href="/css/lavish-bootstrap.css">
  <link rel="stylesheet" href="/css/main.css">
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons+Extended" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css?family=Source+Code+Pro%7CRoboto:500,400italic,300,400" rel="stylesheet">
  
  <link rel="canonical" href="https://flutter.io/ui-performance/">

  <script async src="https://www.googletagmanager.com/gtag/js?id=UA-123350679-1"></script> 
  <script> 
    window.dataLayer = window.dataLayer || []; 
    function gtag(){
      dataLayer.push(arguments);
    } 
    gtag('js', new Date()); 
    gtag('config', 'UA-123350679-1'); 
  </script>

  <!-- <meta name="google-site-verification" content="HFqxhSbf9YA_0rBglNLzDiWnrHiK_w4cqDh2YD2GEY4" /> -->
</head>

  
  <body class="">
    <!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-K5CZXWP"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->

    <div id="overlay-under-drawer"><!-- for the drawer on narrow screens --></div>

    <header class="site-header">
  <div class="container-fluid header-contents">
    <div class="row">
      <div class="col-md-12">
        <i class="fa fa-bars" id="sidebar-toggle-button" aria-hidden="true" style="display:none"></i>
        <img src="/images/flutter-mark-square-100.png" alt="Flutter Logo" width="40" height="40" style="vertical-align:middle">

        <a class="site-title" href="/">Flutter</a>

        <nav class="site-nav">
          <a href="#" class="menu-icon">
            <i class="material-icons-extended">more_vert</i>
          </a>
          <div class="trigger">
            <a class="page-link" href="/docs/">Documentos</a>
            <a class="page-link" href="/showcase/">Portafolio</a>
            <a class="page-link" href="https://github.com/flutter/flutter">GitHub</a>
            <a class="page-link" href="https://pub.dartlang.org/flutter">Paquetes</a>
            <a class="page-link" href="/support/">Soporte</a>
            <form action="/search/" class="nav-searchbox" id="cse-search-box" style="display: inline">
              <input type="hidden" name="cx" value="007067728241810524621:gm6vraqlc8c">
              <input type="hidden" name="ie" value="UTF-8">
              <input type="hidden" name="hl" value="en">
              <input type="search" name="q" id="q" autocomplete="off" placeholder="Buscar">
            </form>
          </div>
        </nav>
      </div>
    </div> <!-- /.row -->
  </div> <!-- /.container -->
</header>


    <!-- Page Content -->
    <div class="container-fluid contents">
      <!-- Content Row -->
      <div class="row">

        <!-- Sidebar Column -->
        <div id="side-nav-container" class="col-sm-3">
          <ul id="mysidebar" class="nav">

  <li class="sidebar-title">Empezar</li>

    <ul class="sidebar-items">
      <li><a href="/get-started/install/">1: Instalación</a></li>
      <li><a href="/get-started/editor/">2: Configuración del editor</a></li>
      <li><a href="/get-started/test-drive/">3: Prueba inicial</a></li>
      <li><a href="/get-started/codelab/">4: Escribe tu primera app</a></li>
      <li><a href="/get-started/learn-more/">5: Aprender más</a></li>
    </ul>

  <li class="sidebar-title">Construye UIs</li>

    <ul class="sidebar-items">
      <li><a href="/widgets-intro/">Paseo por el framework</a></li>
      <li><a href="/widgets/">Catálogo de Widgets</a></li>
      <li><a href="/cookbook/">Cookbook</a></li>
      <li><a href="/catalog/samples/">Catálogo de Muestra</a></li>
      <li><a href="/codelabs">Codelabs</a></li>
      <li><a href="/tutorials/layout/">Construyendo layouts - Tutorial</a></li>
      <li><a href="/tutorials/interactive/">Agregando interactividad - Tutorial</a></li>
      <li><a href="/web-analogs/">Flutter para Web devs</a></li>
      <li><a href="/flutter-for-android/">Flutter para Android devs</a></li>
      <li><a href="/flutter-for-ios/">Flutter para iOS devs</a></li>
      <li><a href="/flutter-for-react-native/">Flutter para React Native devs</a></li>
      <li><a href="/flutter-for-xamarin-forms/">Flutter para Xamarin.Forms devs</a></li>
      <li><a href="/gestures/">Gestos</a></li>
      <li><a href="/animations/">Animaciones</a></li>
      <li><a href="/layout/">Box constraints</a></li>
      <li><a href="/assets-and-images/">Recursos e imágenes</a></li>
      <li><a href="/tutorials/internationalization">Internacionalización</a></li>
      <li><a href="/accessibility">Accesibilidad</a></li>
    </ul>

  <li class="sidebar-title">Usa APIs y SDK del dispositivo</li>

    <ul class="sidebar-items">
      <li><a href="/using-packages/">Usando paquetes</a></li>
      <li><a href="/developing-packages/">Desarrollando paquetes</a></li>
      <li><a href="/platform-channels/">Código específico de la plataforma</a></li>
      <li><a href="/json/">JSON y serialización</a></li>
      
    </ul>

  <li class="sidebar-title">Desarrollo y herramientas</li>

    <ul class="sidebar-items">
      <li><a href="/using-ide/">Utilizando Flutter IDEs</a></li>
      <li><a href="/hot-reload/">Usando hot reload</a></li>
      <li><a href="/testing/">Prueba tu app</a></li>
      <li><a href="/debugging/">Depura tu app</a></li>
      <li><a href="/ui-performance/">Perfiles de rendimiento</a></li>
      <li><a href="/inspector/">Inspecciona tu UI</a></li>
      <li><a href="/android-release/">Build y release para Android</a></li>
      <li><a href="/ios-release/">Build y release para iOS</a></li>
      <li><a href="/fastlane-cd/">Despliegue continuo (CD) con Fastlane</a></li>
      <li><a href="/upgrading/">Actualiza tu instalación de Flutter</a></li>
      <li><a href="/formatting/">Formatea tu código fuente</a></li>
    </ul>

  <li class="sidebar-title">Más detalles</li>

    <ul class="sidebar-items">
      <li><a href="/faq/">FAQ</a></li>
      <li><a href="/technical-overview">Resumen técnico</a></li>
      <li><a href="https://docs.google.com/presentation/d/1B3p0kP6NV_XMOimRV09Ms75ymIjU5gr6GGIX74Om_DE/edit?usp=sharing">Presentación de la magia de Flutter</a></li>
      <li><a href="https://docs.google.com/presentation/d/1cw7A4HbvM_Abv320rVgPVGiUP2msVs7tfGbkgdrTy0I/edit?usp=sharing">Diagramas de arquitectura</a></li>
      <li><a href="https://www.youtube.com/watch?v=dkyY9WCGMi0">Framework de diseño en capas <i class="fa fa-video-camera" aria-hidden="true"></i></a></li>
      <li><a href="https://www.youtube.com/watch?v=UUfXWzp0-DU">Representación de pipeline en Flutter <i class="fa fa-video-camera" aria-hidden="true"></i></a></li>
    </ul>

</ul>

        </div>

        
        

        <!-- Content Column -->
        <div class="col-sm-9 main-contents">
          <div class="main-contents-body">
            <div class="alert alert-info alert__banner" role="alert">
  Únete a nosotros a
    <strong><a href="https://developers.google.com/events/flutter-live" target="_blank" rel="noopener">Flutter Live</a></strong>
    el 4 de Diciembre para una celebración de Flutter. ¡Estad atentos a las actualizaciones!
    <br>
    Por favor rellena esta <a href="https://google.qualtrics.com/jfe/form/SV_cPkBzdAhCHFqTqJ?Source=Website">encuesta</a> para ayudar a mejorar Flutter (Abierta hasta el 12 de Noviembre)
</div>
<article class="post-content">
  
  
  <header class="post-header">
      <div class="btn-group contribute" role="group">
         <a href="https://github.com/flutter/website/blob/master/src/es_ES/ui-performance/index.md" class="btn text-muted btn-sm">
            <i class="fa fa-pencil"></i> Editar Fuente
         </a>
         <a href="https://github.com/flutter-es/website/issues/new?title=Issue desde la página Perfiles de rendimiento&body=From URL: https://flutter.io/ui-performance/&labels=dev: docs - website" class="btn btn-sm">
            <i class="fa fa-github"></i> Crear Issue
        </a>
     </div>
   <div>
    <h1 class="post-title">Perfiles de rendimiento </h1>
   </div>

  </header>
  

  <div class="whats-the-point">

  <p><b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>Lo que aprenderás:</b></p>

  <ul>
    <li>El objetivo de Flutter es proporcionar un rendimiento de 60 frames por segundo (fps), o un rendimiento de 120 fps en dispositivos capaces de realizar actualizaciones de 120 Hz.</li>
    <li>Para 60fps, los frames se renderizan aproximadamente cada 16ms.</li>
    <li>El Jank se produce cuando la UI no se renderiza fluidamente. Por ejemplo, de vez en cuando, un frame tarda 10 veces más en renderizarse, por lo que se descarta, y la animación se sacude visiblemente.</li>
  </ul>

</div>

<p>Se dice que “una aplicación <em>rápida</em> es genial, pero una <em>fluida</em> es aún mejor”.
Si tu aplicación no se está renderizando correctamente, ¿cómo la arreglas? ¿Por dónde empiezas?
Esta guía te muestra por dónde empezar, los pasos a seguir y las herramientas que pueden ayudarte.</p>

<aside class="alert alert-info">
  <p><strong>Nota:</strong> El rendimiento de una app está determinado por más de una medida.
El rendimiento a veces se refiere a la velocidad en bruto, pero también a la fluidez de la UI y la falta de fluidez. Otros ejemplos de rendimiento incluyen I/O o velocidad de red.
Esta página principalmente se centra en el segundo tipo de rendimiento (fluidez de la UI), pero puedes utilizar la mayoría de las mismas herramientas que se usan para diagnosticar otros problemas de rendimiento.</p>
</aside>

<ul id="markdown-toc">
  <li><a href="#diagnóstico-de-problemas-de-rendimiento" id="markdown-toc-diagnóstico-de-problemas-de-rendimiento">Diagnóstico de problemas de rendimiento</a>    <ul>
      <li><a href="#conéctate-a-un-dispositivo-físico" id="markdown-toc-conéctate-a-un-dispositivo-físico">Conéctate a un dispositivo físico</a></li>
      <li><a href="#ejecutar-en-modo-profile" id="markdown-toc-ejecutar-en-modo-profile">Ejecutar en modo profile</a></li>
    </ul>
  </li>
  <li><a href="#la-capa-sobrepuesta-de-rendimiento" id="markdown-toc-la-capa-sobrepuesta-de-rendimiento">La capa sobrepuesta de rendimiento</a>    <ul>
      <li><a href="#visualización-de-la-capa-sobrepuesta-de-rendimiento" id="markdown-toc-visualización-de-la-capa-sobrepuesta-de-rendimiento">Visualización de la capa sobrepuesta de rendimiento</a>        <ul>
          <li><a href="#usando-el-flutter-inspector" id="markdown-toc-usando-el-flutter-inspector">Usando el Flutter Inspector</a></li>
          <li><a href="#en-vs-code" id="markdown-toc-en-vs-code">En VS Code</a></li>
          <li><a href="#desde-la-línea-de-comando" id="markdown-toc-desde-la-línea-de-comando">Desde la línea de Comando</a></li>
          <li><a href="#programáticamente" id="markdown-toc-programáticamente">Programáticamente</a></li>
        </ul>
      </li>
      <li><a href="#identificando-problemas-en-la-ui-de-gráficas" id="markdown-toc-identificando-problemas-en-la-ui-de-gráficas">Identificando problemas en la UI de gráficas</a>        <ul>
          <li><a href="#mostrando-el-observatory" id="markdown-toc-mostrando-el-observatory">Mostrando el Observatory</a></li>
          <li><a href="#usando-el-timeline-de-observatory" id="markdown-toc-usando-el-timeline-de-observatory">Usando el timeline de Observatory</a></li>
        </ul>
      </li>
      <li><a href="#identificación-de-problemas-en-el-gráfico-de-la-gpu" id="markdown-toc-identificación-de-problemas-en-el-gráfico-de-la-gpu">Identificación de problemas en el gráfico de la GPU</a>        <ul>
          <li><a href="#comprobación-de-capas-fuera-de-pantalla" id="markdown-toc-comprobación-de-capas-fuera-de-pantalla">Comprobación de capas fuera de pantalla</a></li>
          <li><a href="#comprobación-de-imágenes-no-almacenadas-en-caché" id="markdown-toc-comprobación-de-imágenes-no-almacenadas-en-caché">Comprobación de imágenes no almacenadas en caché</a></li>
        </ul>
      </li>
    </ul>
  </li>
  <li><a href="#indicadores-de-depuración" id="markdown-toc-indicadores-de-depuración">Indicadores de depuración</a></li>
  <li><a href="#benchmarking" id="markdown-toc-benchmarking">Benchmarking</a></li>
  <li><a href="#más-información" id="markdown-toc-más-información">Más información</a></li>
</ul>

<aside class="alert alert-info">
  <p><strong>Nota</strong>: Para realizar el seguimiento dentro de tu código Dart, consulta <a href="/debugging/#tracing-any-dart-code-performance">Seguimiento del rendimiento de cualquier código Dart</a>
en la página <a href="/debugging">Depura tu app</a>.</p>
</aside>

<h2 id="diagnóstico-de-problemas-de-rendimiento">Diagnóstico de problemas de rendimiento</h2>

<p>Para diagnosticar una aplicación con problemas de rendimiento, habilitarás la capa sobrepuesta de rendimiento para ver los subprocesos de la UI y la GPU. Antes de empezar, deberás asegurarte de que estás ejecutando en modo profile y de que no estás usando un emulador. Para obtener mejores resultados, puedes elegir el dispositivo más lento que tus usuarios puedan utilizar.</p>

<h3 id="conéctate-a-un-dispositivo-físico">Conéctate a un dispositivo físico</h3>

<p>Casi toda la depuración del rendimiento de las aplicaciones Flutter debe realizarse en un dispositivo físico Android o iOS, con su aplicación Flutter ejecutándose en modo profile. Usar el modo debug, o ejecutar aplicaciones en simuladores o emuladores, generalmente no es indicativo del comportamiento final de las compilaciones del modo release. <em>Deberías considerar comprobar el rendimiento en el dispositivo más lento que tus usuarios puedan usar razonablemente</em>.</p>

<aside class="alert alert-info">
  <p><strong>Por qué debe ejecutarse en un dispositivo real:</strong></p>

  <ul>
    <li>Los simuladores y emuladores no utilizan el mismo hardware, por lo que sus características de rendimiento son diferentes— algunas operaciones son más rápidas en los simuladores que en los dispositivos reales, y otras son más lentas.</li>
    <li>El modo depuración permite realizar comprobaciones adicionales (como asserts) que no se ejecutan en compilaciones profile o release, y estas comprobaciones pueden ser costosas.</li>
    <li>El modo de depuración también ejecuta el código de una manera diferente que el modo release. La construcción del debug compila el código de Dart “just in time” (JIT) a medida que la aplicación se ejecuta, pero las construcciones de profile y release se precompilan a instrucciones nativas (también llamadas “ahead of time”, o AOT) antes de que la aplicación se cargue en el dispositivo. JIT puede hacer que la aplicación se pause para la compilación de JIT, lo que a su vez puede causar jank.</li>
  </ul>

</aside>

<h3 id="ejecutar-en-modo-profile">Ejecutar en modo profile</h3>

<p>El modo profile de Flutter compila e inicia tu aplicación de forma casi idéntica al modo release, pero con la funcionalidad adicional suficiente para permitir la depuración de problemas de rendimiento.
Por ejemplo, el modo de perfil proporciona información de trazabilidad a
<a href="https://dart-lang.github.io/observatory/">Observatory</a> y otras herramientas.</p>

<p>Lanza la aplicación en modo profile de la siguiente manera:</p>

<ul>
<li>
    <p>En Android Studio e IntelliJ, usa el elemento del menú
    <strong>Run &gt; Flutter Run main.dart in Profile Mode</strong>.</p>
  </li>
<li>
    <p>En VS Code, abre tu archivo <code class="highlighter-rouge">launch.json</code>, y asigna la propiedad
<code class="highlighter-rouge">flutterMode</code> a <code class="highlighter-rouge">profile</code>
(cuando termines el profile, cámbialo de nuevo hacia <code class="highlighter-rouge">release</code> o <code class="highlighter-rouge">debug</code>):</p>

    <div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>"configurations": [
	{
		"name": "Flutter",
		"request": "launch",
		"type": "dart",
		"flutterMode": "profile"
	}
]
</code></pre></div>    </div>
  </li>
<li>
    <p>Desde la línea de comando, usa el parámetro <code class="highlighter-rouge">--profile</code>:</p>

    <pre class="lang-sh"><code>$ flutter run --profile</code></pre>
  </li>
</ul>

<p>Para obtener más información sobre cómo funcionan los diferentes modos, consulta
 <a href="https://github.com/flutter/flutter/wiki/Flutter%27s-modes">Modos en Flutter</a>.</p>

<p>Comenzarás activando la capa sobrepuesta de rendimiento, como se explica en la siguiente sección.</p>

<h2 id="la-capa-sobrepuesta-de-rendimiento">La capa sobrepuesta de rendimiento</h2>

<p>La capa sobrepuesta de rendimiento muestra las estadísticas en dos gráficos que muestran dónde se está gastando el tiempo tu aplicación.
Si la UI está en jank (saltando frames), estos gráficos te ayudan a averiguar por qué.
Los gráficos se muestran encima de tu aplicación en ejecución, pero no se dibujan como un widget normal; el propio motor Flutter pinta la capa sobrepuesta de rendimiento y sólo tiene un impacto mínimo en el rendimiento.
Cada gráfico representa los últimos 300 frames para ese hilo.</p>

<p>En esta sección se describe cómo habilitar la función
<a href="https://docs.flutter.io/flutter/widgets/PerformanceOverlay-class.html">PerformanceOverlay,</a>
y usarla para diagnosticar la causa del jank en tu aplicación.
La siguiente captura de pantalla muestra la capa sobrepuesta de rendimiento que se está ejecutando en el ejemplo de Flutter Gallery:</p>

<center><img src="images/performance-overlay-green.png" alt="screenshot of performance overlay showing zero jank" /></center>
<center>La capa sobrepuesta de rendimiento muestra el hilo de la UI (arriba) y el hilo de la GPU (abajo). Las barras verdes verticales representan el frame actual.</center>
<p><br /></p>

<p>Flutter utiliza varios hilos para hacer su trabajo. Todo tu código de Dart se ejecuta en el hilo de la UI. Aunque no tienes acceso directo a ningún otro hilo, tus acciones en el hilo de la UI tienen consecuencias de rendimiento en otros hilos.</p>

<ol>
  <li>
    <p>Platform thread<br />
El hilo principal de la plataforma. El código del Plugin se ejecuta aquí.
Para más información, consulta la documentación para iOS
<a href="https://developer.apple.com/documentation/uikit">UIKit</a>
, o la documentación para Android
<a href="https://developer.android.com/reference/android/support/annotation/MainThread.html">MainThread</a>.
Este hilo no se muestra en la capa sobrepuesta de rendimiento.</p>
  </li>
  <li>
    <p>UI thread<br />
El hilo UI ejecuta el código Dart en la VM de Dart.
Este hilo incluye el código que escribiste, y el código ejecutado por el framework de Flutter en beneficio de tu aplicación.
Cuando la aplicación crea y muestra una escena, el subproceso de la interfaz de usuario crea un <em>árbol de capas</em>, un objeto ligero que contiene comandos de trazado agnósticos del dispositivo, y envía el árbol de capas al hilo de la GPU para que se renderice en el dispositivo. <em>No bloquees este hilo!</em> Se muestra en la fila inferior de la capa sobrepuesta de rendimiento.</p>
  </li>
  <li>
    <p>GPU thread<br />
El hilo de la GPU toma el árbol de capas y lo muestra hablando a la GPU (unidad de procesamiento gráfico). No puedes acceder directamente al hilo de la GPU o a sus datos, pero, si este hilo es lento, es el resultado de algo que has hecho en el código de Dart.
Skia, la biblioteca de gráficos se ejecuta en este hilo, que a veces se llama el hilo <em>rasterizador</em>.
Se muestra en la fila inferior de la capa sobrepuesta de rendimiento.</p>
  </li>
  <li>
    <p>I/O thread<br />
Realiza tareas costosas (principalmente I/O) que de otro modo bloquearían
ya sea la UI o la GPU.
Este hilo no se muestra en la capa sobrepuesta de rendimiento.</p>
  </li>
</ol>

<p>Para más información sobre esos hilos, consulta
<a href="https://github.com/flutter/engine/wiki#architecture-notes">Notas de la Arquitectua.</a></p>

<p>Cada frame debe ser creado y mostrado en 1/60 de segundo. (aproximadamente 16ms). Un frame que excede este límite (en cualquier gráfico)
no se muestra, resultando en jank, y una barra roja vertical aparece en una o ambas gráficas.
Si aparece una barra roja en el gráfico de UI, el código de Dart sale demasiado costoso.
Si aparece una barra vertical roja en el gráfico de la GPU, la escena se hace demasiado complicada como para renderizar rápidamente.</p>

<center><img src="images/performance-overlay-jank.png" alt="Screenshot of performance overlay showing jank with red bars." /></center>
<center>Las barras rojas verticales indican que el frame actual es muy costoso tanto para el renderizado como para el pintado.<br />Cuando ambas gráficas estén rojas, comienza por diagnosticar el hilo de la UI (Dart VM).</center>
<p><br /></p>

<h3 id="visualización-de-la-capa-sobrepuesta-de-rendimiento">Visualización de la capa sobrepuesta de rendimiento</h3>

<p>Puede alternar la visualización de la capa sobrepuesta de rendimiento como se indica a continuación:</p>

<ul>
  <li>Usando el Flutter Inspector</li>
  <li>Desde la línea de comando</li>
  <li>Programáticamente</li>
</ul>

<h4 id="usando-el-flutter-inspector">Usando el Flutter Inspector</h4>

<p>La manera más fácil de habilitar el widget PerformanceOverlay es habilitándolo en el Flutter Inspector, que está disponible a través del plugin Flutter para su IDE. La vista Inspector se abre de forma predeterminada cuando se ejecuta una aplicación. Si el inspector no está abierto, puedes mostrarlo de la siguiente manera.</p>

<p>En Android Studio e IntelliJ IDEA:</p>

<ol>
  <li>Selecciona <strong>View &gt; Tool Windows &gt; Flutter Inspector</strong>.</li>
  <li>En la barra de herramientas, selecciona el icono que parece una biblioteca (<img src="images/performance-overlay-icon.png" alt="icon that resembles a bookshelf" />).</li>
</ol>

<p><img src="/images/intellij/visual-debugging.png" alt="IntelliJ Flutter Inspector Window" /><br /></p>

<p>El Flutter Inspector está disponible en Android Studio e IntelliJ.
Obtenga más información sobre lo que el Inspector puede hacer en el documento
<a href="/inspector/">Inspecciona tu UI</a>, así como el archivo
<a href="https://www.youtube.com/watch?v=JIcmJNT9DNI">Flutter Inspector talk</a>
presentado en el DartConf 2018.</p>

<h4 id="en-vs-code">En VS Code</h4>

<ol>
  <li>Seleciona <strong>View &gt; Command Palette…</strong> para mostrar la paleta de comandos.</li>
  <li>En el campo de texto, escribes “performance” y selecciona
<strong>Toggle Performance Overlay</strong> de la lista que aparece.
Si este comando no está disponible, asegúrate de que la aplicación esté ejecutándose.</li>
</ol>

<h4 id="desde-la-línea-de-comando">Desde la línea de Comando</h4>

<p>Conmute la capa sobrepuesta de rendimiento con la tecla <strong>P</strong> desde la línea de comando.</p>

<h4 id="programáticamente">Programáticamente</h4>

<p>Puede habilitar programáticamente el widget PerformanceOverlay estableciendo la propiedad <code class="highlighter-rouge">showPerformanceOverlay</code> en <code class="highlighter-rouge">true</code> en el constructor de MaterialApp o WidgetsApp:</p>

<!-- skip -->
<pre class="lang-dart"><code>class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      <span class="highlight">showPerformanceOverlay: true,</span>
      title: &#39;My Awesome App&#39;,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: &#39;My Awesome App&#39;),
    );
  }
}</code></pre>

<p>Probablemente ya conozcas la aplicación de ejemplo de Flutter Gallery.
Para utilizar la capa sobrepuesta de rendimiento con Flutter Gallery, usa la copia en el directorio de 
<a href="https://github.com/flutter/flutter/tree/master/examples/flutter_gallery">ejemplos</a>
que fue instalado con Flutter, y ejecuta la aplicación en modo profile. El programa se escribe de manera que el menú de la app le permita cambiar dinámicamente la ventana superpuesta,
así como habilitar la comprobación de llamadas a <code class="highlighter-rouge">saveLayer</code> y la presencia de imágenes en caché.</p>

<aside class="alert alert-info">
  <p><strong>Nota:</strong> No puede habilitar la capa sobrepuesta de rendimiento en la app Flutter Gallery descargada desde el App Store. Esa versión de la aplicación está compilada en modo release (no en modo profile), y no proporciona un menú para habilitar o deshabilitar la ventana superpuesta.</p>
</aside>

<h3 id="identificando-problemas-en-la-ui-de-gráficas">Identificando problemas en la UI de gráficas</h3>

<p>Si la capa sobrepuesta de rendimiento aparece en rojo en el gráfico de interfaz de usuario, comienza por perfilar la VM de Dart, incluso si el gráfico de la GPU también aparece en rojo. Para ello, utiliza 
<a href="https://dart-lang.github.io/observatory/">Observatory</a>, la herramienta para profile de Dart.</p>

<h4 id="mostrando-el-observatory">Mostrando el Observatory</h4>

<p>El Observatory proporciona características como la creación de perfiles, el examen del heap, y la visualización de la cobertura de código. La vista <em>timeline</em> del Observatory le permite capturar una instantánea del stack en un momento dado.
Al abrir el timeline del Observatory desde el Flutter Inspector, usarás una versión que ha sido personalizada para las aplicaciones Flutter.</p>

<p>Ve a la vista del timeline de Flutter desde el navegador así:</p>

<ol>
<li>
    <p>Para abrir la vista del timeline, usa el ícono del gráfico de líneas
 (<img src="images/observatory-timeline-icon.png" alt="zig-zag line chart icon" />).</p>

    <p>(En su lugar, puedes abrir Observatory usando el icono del cronómetro (<img src="images/observatory-icon.png" alt="stopwatch icon used by Observatory" />),
pero el enlace “view <u>inspector</u>” te lleva a la versión estándar del timeline, no a la versión personalizada para Flutter).</p>

    <p><img src="/images/intellij/visual-debugging.png" alt="IntelliJ Flutter Inspector Window" /></p>
  </li>

<li>
    <p>En VS Code, abre la paleta de comandos e introduce “observatory”. 
Selecciona <strong>Flutter: Open Observatory Timeline</strong> de la lista que aparece. Si este comando no está disponible, asegúrate de que la aplicación esté ejecutándose.</p>
  </li>
</ol>

<p><br /></p>

<h4 id="usando-el-timeline-de-observatory">Usando el timeline de Observatory</h4>

<aside class="alert alert-info">
  <p><strong>Nota:</strong> La UI de Observatory y la página timeline personalizada de Flutter se encuentra actualmente en desarrollo. 
Por esta razón, no estamos documentando completamente la UI en este momento. 
Si te sientes cómodo experimentando con Observatory, y te gustaría retroalimentarnos, por favor, envíanos los
 <a href="https://github.com/dart-lang/sdk/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-observatory">issues or feature requests</a> 
 a medida que los encuentres.</p>
</aside>

<h3 id="identificación-de-problemas-en-el-gráfico-de-la-gpu">Identificación de problemas en el gráfico de la GPU</h3>
<p>A veces, una escena da como resultado un árbol de capas que es fácil de construir, pero cuyo renderizado en el hilo de la GPU es costoso. Cuando esto sucede, el gráfico de UI no tiene color rojo, pero el gráfico de la GPU muestra color rojo.
En este caso, tendrás que averiguar qué está haciendo tu código para que el código de renderizado sea lento. Los tipos específicos de cargas de trabajo son más difíciles para la GPU. Pueden implicar llamadas innecesarias a
<a href="https://docs.flutter.io/flutter/dart-ui/Canvas/saveLayer.html"><code class="highlighter-rouge">saveLayer</code></a>,
opacidades entrecruzadas con múltiples objetos, y acoplados o sombras en situaciones específicas.</p>

<p>Si sospechas que la fuente de la lentitud es durante una animación, usa la propiedad del botón 
<a href="https://docs.flutter.io/flutter/scheduler/timeDilation.html">timeDilation</a>
para ralentizar enormemente la animación.</p>

<p>También puedes reducir la velocidad de la animación utilizando el Flutter Inspector.
En el menú de engranajes del inspector, selecciona <strong>Enable Slow Animations</strong>.
Si deseas un mayor control de la velocidad de la animación, establece la propiedad 
<a href="https://docs.flutter.io/flutter/scheduler/timeDilation.html">timeDilation</a>
en tu código.</p>

<p>¿La lentitud está en el primer frame o en toda la animación?
Si se trata de toda la animación, ¿el acoplado está causando la ralentización?
Tal vez haya una forma alternativa de dibujar la escena que no utilice el acoplado. Por ejemplo, superponga esquinas opacas a un cuadrado en lugar de hacer recortes en un rectángulo redondeado.
Si se trata de una escena estática que se está desvaneciendo, girando o manipulando de otra manera, tal vez un
<a href="https://docs.flutter.io/flutter/widgets/RepaintBoundary-class.html">RepaintBoundary</a>
puede ayudar.</p>

<h4 id="comprobación-de-capas-fuera-de-pantalla">Comprobación de capas fuera de pantalla</h4>

<p>El método 
<a href="https://docs.flutter.io/flutter/dart-ui/Canvas/saveLayer.html"><code class="highlighter-rouge">saveLayer</code></a>
es uno de los métodos más costosos en el framework Flutter. Es útil cuando se aplica el post procesamiento a la escena, pero puede ralentizar tu aplicación y debería evitarse si no la necesitas. Incluso si no llama a <code class="highlighter-rouge">saveLayer</code> explícitamente, pueden producirse llamadas implícitas en su nombre. Puedes comprobar si tu escena está usando <code class="highlighter-rouge">saveLayer</code> con el switch 
 <a href="https://docs.flutter.io/flutter/rendering/PerformanceOverlayLayer/checkerboardOffscreenLayers.html">PerformanceOverlayLayer.checkerboardOffscreenLayers</a>.</p>

<p>Una vez que el switch esté habilitado, ejecuta la aplicación y busca cualquier imagen que este delineada con un cuadro parpadeante. La caja parpadea de frame a frame si un nuevo frame se está renderizando. Por ejemplo, tal vez tenga un grupo de objetos con opacidades que se renderizan utilizando <code class="highlighter-rouge">saveLayer</code>. En este caso, probablemente sea más eficaz aplicar una opacidad a cada widget individual, en lugar de un widget padre más arriba en el árbol de widgets. Lo mismo ocurre con otras operaciones potencialmente costosas, como acoplamiento o sombras.</p>

<aside class="alert alert-info">
  <p><strong>Nota:</strong> Opacidad, acoplamiento, y sombras no son, por sí mismas, una mala idea.
Sin embargo, aplicarlos a la parte superior del árbol de widgets puede causar llamadas adicionales a <code class="highlighter-rouge">saveLayer</code>, y un procesamiento innecesario.</p>
</aside>

<p>Cuando encuentres llamadas a <code class="highlighter-rouge">saveLayer</code>, hazte estas preguntas:</p>

<ul>
  <li>¿Necesita la aplicación este efecto?</li>
  <li>¿Se puede eliminar alguna de estas llamadas?</li>
  <li>¿Puedo aplicar el mismo efecto a un elemento individual en lugar de a un grupo?</li>
</ul>

<h4 id="comprobación-de-imágenes-no-almacenadas-en-caché">Comprobación de imágenes no almacenadas en caché</h4>

<p>Almacenamiento en caché de una imagen con
<a href="https://docs.flutter.io/flutter/widgets/RepaintBoundary-class.html">RepaintBoundary</a>
es bueno, <em>cuando tiene sentido</em>.</p>

<p>Una de las operaciones más costosas, desde la perspectiva de los recursos, es renderizar una textura usando un archivo de imagen. Primero, la imagen comprimida se obtiene del almacenamiento persistente.
La imagen se descomprime en la memoria del host (memoria de la GPU) y se transfiere.
a la memoria del dispositivo (RAM).</p>

<p>En otras palabras, la I/O de imagenes puede ser costosa.
La caché proporciona instantáneas de jerarquías complejas para que sea más fácil
en los frames siguientes.
<em>Debido a que las entradas de caché de imágenes tipo raster o <code class="highlighter-rouge">"bitmap image"</code> son costosas de construir y ocupan mucha memoria de la GPU, almacena en caché imágenes sólo cuando sea absolutamente necesario.</em></p>

<p>Puedes ver qué imágenes están siendo almacenadas en caché activando el switch
<a href="https://docs.flutter.io/flutter/widgets/PerformanceOverlay/checkerboardRasterCacheImages.html">PerformanceOverlayLayer.checkerboardRasterCachedImages</a>.</p>

<p>Ejecuta la app y busca imágenes renderizadas con un tablero de colores aleatorios, indicando que la imagen está almacenada en caché. A medida que interactúas con la escena, las imágenes en el tablero deben permanecer constantes; no quieres ver parpadeos, lo que indicaría que la imagen en caché se está volviendo a almacenar.</p>

<p>En la mayoría de los casos, deseas ver los tableros en imágenes estáticas, pero no en imágenes no estáticas. Si una imagen estática no está almacenada en caché, puedes guardarla en caché colocándola en un widget
<a href="https://docs.flutter.io/flutter/widgets/RepaintBoundary-class.html">RepaintBoundary</a>. 
Aunque el motor puede ignorar un límite de repintado si piensa que la imagen no es lo suficientemente compleja.</p>

<h2 id="indicadores-de-depuración">Indicadores de depuración</h2>

<p>Flutter proporciona una amplia variedad de indicadores y funciones de depuración para ayudarle a depurar su aplicación en varios puntos a lo largo del ciclo de desarrollo.
Para usar estas características, debe compilar en modo debug.
La siguiente lista, aunque no completa,
resalta algunas de los indicadores más útiles  (y una función)
desde la 
<a href="https://docs.flutter.io/flutter/rendering/rendering-library.html">biblioteca de renderizado</a>
para depurar problemas de rendimiento.</p>

<ul>
  <li><a href="https://docs.flutter.io/flutter/rendering/debugDumpRenderTree.html"><code class="highlighter-rouge">debugDumpRenderTree()</code></a><br />
Llama a esta función, cuando no esté en una fase de diseño o repintado, para volcar el árbol de renderizado a la consola. (Pulsa <strong>t</strong> desde <code class="highlighter-rouge">flutter run</code>
para llamar a este comando.) Busca por “RepaintBoundary” para ver diagnósticos
sobre lo útil que es un límite.</li>
  <li><a href="https://docs.flutter.io/flutter/rendering/debugPaintLayerBordersEnabled.html"><code class="highlighter-rouge">debugPaintLayerBordersEnabled</code></a></li>
  <li><a href="https://docs.flutter.io/flutter/rendering/debugRepaintRainbowEnabled.html"><code class="highlighter-rouge">debugRepaintRainbowEnabled</code></a><br />
Habilita esta propiedad y ejecuta tu aplicación para ver si hay partes de tu UI que no estén cambiando (por ejemplo, un encabezado estático) que estén rotando a través de muchos colores en la salida.
Esas áreas son candidatas para agregar límites de repintado</li>
  <li><a href="https://docs.flutter.io/flutter/rendering/debugPrintMarkNeedsLayoutStacks.html"><code class="highlighter-rouge">debugPrintMarkNeedsLayoutStack</code></a><br />
Habilita esta propiedad si estás viendo más diseños de los que esperas (por ejemplo, en el timeline, en un profile o desde una sentencia “print” dentro de un método de diseño). Una vez activada, la consola se inunda de trazos de pila que muestran por qué cada objeto renderizado está siendo marcado como corrupto para el diseño.</li>
  <li><a href="https://docs.flutter.io/flutter/rendering/debugPrintMarkNeedsPaintStacks.html"><code class="highlighter-rouge">debugPrintMarkNeedsPaintStacks</code></a><br />
Similar a <code class="highlighter-rouge">debugPrintMarkNeedsLayoutStack</code>, pero por exceso de pintado.</li>
</ul>

<p>Puedes obtener más información sobre otros indicadores de depuración en
<a href="https://flutter.io/debugging/">Depurando Apps de Flutter</a>.</p>

<h2 id="benchmarking">Benchmarking</h2>

<p>Puedes medir y hacer un seguimiento del rendimiento de tu aplicación escribiendo pruebas de referencia.
La biblioteca Flutter Driver proporciona soporte para el benchmarking. Usando este
framework de pruebas de integración, es posible generar métricas para realizar el seguimiento de lo siguiente:</p>

<ul>
  <li>Jank</li>
  <li>Tamaño de descarga</li>
  <li>Eficiencia de la batería</li>
  <li>Tiempo de inicio</li>
</ul>

<p>El seguimiento de estos puntos de referencia te permite estar informado cuando se introduce una regresión que afecta negativamente al rendimiento.</p>

<p>Para más información, consulta
<a href="/testing/#pruebas-de-integración">Pruebas de Integration</a>,
una sección en <a href="https://flutter.io/testing/">Prueba tu app</a>.</p>

<h2 id="más-información">Más información</h2>

<p>Los siguientes recursos proporcionan más información sobre el uso de las herramientas de Flutter y depurando en Flutter:</p>

<ul>
  <li><a href="/debugging/">Depura tu app</a></li>
  <li><a href="/inspector/">Inspecciona tu UI</a></li>
  <li><a href="https://www.youtube.com/watch?v=JIcmJNT9DNI">Habla con Flutter Inspector</a>,
presentado en el DartConf 2018</li>
  <li><a href="https://hackernoon.com/why-flutter-uses-dart-dd635a054ebf">¿Porqué Flutter usa Dart?</a>,
un artículo sobre Hackernoon.</li>
  <li><a href="https://dart-lang.github.io/observatory/">Observatory: Un Profiler para Apss de Dart</a></li>
  <li>Documentos <a href="https://docs.flutter.io/index.html">Flutter API</a>, particularmente la clase
<a href="https://docs.flutter.io/flutter/widgets/PerformanceOverlay-class.html">PerformanceOverlay</a>, y el paquete
<a href="https://docs.flutter.io/flutter/dart-developer/dart-developer-library.html">dart:developer</a>.</li>
</ul>

</article>

          </div>
        </div>

        

      </div> <!-- /.row -->
    </div> <!-- /.container -->

    <footer class="site-footer">
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-12">
        <div class="logo">
          <img src="/images/flutter-mono-81x100.png" alt="Flutter Logo" width="81" height="100">
        </div>
          <p class="site-footer__link-list">
            <a href="https://groups.google.com/forum/#!forum/flutter-dev">flutter-dev@</a> &bull;
            <a href="https://twitter.com/flutterio">twitter</a> &bull;
            <a href="https://github.com/flutter/">github</a> &bull;
            <a href="/tos">terms</a> &bull;
            <a href="https://www.google.com/intl/en/policies/privacy/">Privacidad</a> &bull;
            <a href="https://flutter-io.cn">社区中文资源</a>
          </p>

          <p class="licenses">
            Salvo que se indique lo contrario,
            este trabajo se encuentra bajo licencia de
            <a rel="license" href="https://creativecommons.org/licenses/by/4.0/">Creative
            Commons Attribution 4.0 International License</a>,
            y los códigos de muestra tiene licencia bajo BSD License.
          </p>
      </div>
    </div>
  </div>
</footer>


    <link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js"></script>
    <script src="/js/sidebar_toggle.js"></script>
    <script src="/js/customscripts.js"></script>
    <script src="/js/prism.js"></script>
    <script src="/js/tabs.js"></script>
    <script src="/js/archive.js"></script>
    
    <script async="" defer="" src="//survey.g.doubleclick.net/async_survey?site=at3ul57xpub2vk3oxt2ytw365i"></script>
  </body>
</html>
