---
title: Comportamientos y adaptaciones específicas de plataforma
---

## Filosofía de adaptación

Hay de forma general 2 casos de adaptabilidad a la plataforma:

1. Cosas que tienen un comportamiento específico en el entorno del Sistema Operativo (como es la edición de texto
   y el scroll) y sería 'incorrecto' si tuviera un comportamiento diferente.
2. Cosas que se implementan convencionalmente en apps que usa el OEM SDK 
   (como es usar pestañas paralelas en iOS o mostrar un 
   [android.app.AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html)
   en Android).

Este artículo cubre las adaptaciones automáticas proporcionadas por Flutter 
en el caso 1 en Android e iOS.

Para el caso 2, Flutter empaqueta los medios para producir el efecto apropiado de las 
convenciones de la plataforma pero no hace las adpataciones de forma automática por lo que 
se necesita una decisión de diseño. Para una discusion sobre esto, mira [#8410](https://github.com/flutter/flutter/issues/8410#issuecomment-468034023).

## Navegación entre páginas

Flutter proporciona los patrones de navegación vistos en Android e iOS y tambien 
adapta automáticamnte las animaciones de navegación a la plataforma correspondiente.

### Transiciones de Navegación

En **Android**, la transición 
[Navigator.push]({{site.api}}/flutter/widgets/Navigator/push.html)
por defecto es modelada despues de 
[startActivity()](https://developer.android.com/reference/android/app/Activity.html#startActivity(android.content.Intent)),
que generalmente tiene una variante de animación de abajo hacia arriba.

En **iOS**:

* La API 
  [Navigator.push]({{site.api}}/flutter/widgets/Navigator/push.html)
  API produce una transición Show/Push estilo iOS que se anima de 
  principio a inicio dependiendo de la configuración RTL local. La página por debajo de la nueva
  ruta tambien se desliza con parallax en la misma dirección que en iOS.
* Una transición separada de estilo abajo-arriba sucede cuando se hace push de una página de una ruta 
  cuando [PageRoute.fullscreenDialog]({{site.api}}/flutter/widgets/PageRoute-class.html)
  es true. Esto representa el estilo de transición Present/Modal de iOS y es usado
  típicamente en páginas modales a pantalla completa.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 12px;" src="../../images/platform-adaptations/navigation-android.gif" class="figure-img img-fluid" alt="An animation of the bottom-up page transition on Android" />
        <figcaption class="figure-caption">
          Android page transition
        </figcaption>
      </figure>
    </div>
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 22px;" src="../../images/platform-adaptations/navigation-ios.gif" class="figure-img img-fluid" alt="An animation of the end-start style push page transition on iOS" />
        <figcaption class="figure-caption">
          iOS push transition
        </figcaption>
      </figure>
    </div>
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 22px;" src="../../images/platform-adaptations/navigation-ios-modal.gif" class="figure-img img-fluid" alt="An animation of the bottom-up style present page transition on iOS" />
        <figcaption class="figure-caption">
          iOS present transition
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Detalles de transición específicos de plataforma

En **Android**, existen 2 estilos de animación para la transición entre páginas dependiendo de 
tu version de SO:

* Anteriores a API 28 usan una animación de abajo-arriba que [desliza hacia arriba y hace fade  
  in]({{site.api}}/flutter/material/FadeUpwardsPageTransitionsBuilder-class.html).
* En la API 28 y posteriores, la animación abajo-arriba [desliza y hace un clip-reveals
  up]({{site.api}}/flutter/material/OpenUpwardsPageTransitionsBuilder-class.html).

En **iOS** cuando la transición estilo push es usada, en el paquete de Flutter,
[CupertinoNavigationBar]({{site.api}}/flutter/cupertino/CupertinoNavigationBar-class.html)
y [CupertinoSliverNavigationBar]({{site.api}}/flutter/cupertino/CupertinoSliverNavigationBar-class.html)
animan automáticamente cada subcomponente a su correspondiente 
subcomponente en las páginas siguiente o previas de CupertinoNavigationBar o
CupertinoSliverNavigationBar.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 12px;" src="../../images/platform-adaptations/navigation-android.gif" class="figure-img img-fluid" alt="An animation of the page transition on Android pre-Android P" />
        <figcaption class="figure-caption">
          Android Pre-P
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img style="border-radius: 12px;" src="../../images/platform-adaptations/navigation-android-p.gif" class="figure-img img-fluid" alt="An animation of the page transition on Android on Android P" />
        <figcaption class="figure-caption">
          Android Post-P
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img style="border-radius: 22px;" src="../../images/platform-adaptations/navigation-ios-nav-bar.gif" class="figure-img img-fluid" alt="An animation of the nav bar transitions during a page transition on iOS" />
        <figcaption class="figure-caption">
          iOS Nav Bar
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Navegación hacia atrás

En **Android**, el botón atrás del SO, por defecto, es expedido a Flutter y se hace pop de 
la ruta superior del 
Navigator de 
[WidgetsApp]({{site.api}}/flutter/widgets/WidgetsApp-class.html).

En **iOS**, un gesto de deslizar desde el borde puede ser usado para hacer pop de la ruta superior.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 12px;" src="../../images/platform-adaptations/navigation-android-back.gif" class="figure-img img-fluid" alt="A page transition triggered by the Android back button" />
        <figcaption class="figure-caption">
          Android back button
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img style="border-radius: 22px;" src="../../images/platform-adaptations/navigation-ios-back.gif" class="figure-img img-fluid" alt="A page transition triggered by an iOS back swipe gesture" />
        <figcaption class="figure-caption">
          iOS back swipe gesture
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## Hacer scroll

Hacer scroll es una parte importante del look and feel de la plataforma, y Flutter
ajusta automáticamente el comportamiento del scroll para ajustarse a la plataforma actual.

### Simulación física

Tanto Android como iOS tienen una simulación físcica del scroll que son difíciles de 
describer verbalmente. Generalmente, el scroll en iOS tiene más peso y 
fricción dinámica sin embargo Android tiene una fricción más estática. Por lo tanto, iOS gana 
velocidad más gradualmente pero se detiene menos bruscamente y es más resbaladizo 
a velocidades lentas.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/scroll-soft.gif" class="figure-img img-fluid rounded" alt="A soft fling where the iOS scrollable slid longer at lower speed than Android" />
        <figcaption class="figure-caption">
          Soft fling comparison
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/scroll-medium.gif" class="figure-img img-fluid rounded" alt="A medium force fling where the Android scrollable reached speed faster and stopped more abruptly after reaching a longer distance" />
        <figcaption class="figure-caption">
          Medium fling comparison
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/scroll-strong.gif" class="figure-img img-fluid rounded" alt="A strong fling where the Android scrollable reach speed faster and reached significantly more distance" />
        <figcaption class="figure-caption">
          Strong fling comparison
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Comportamiento en el límite del scroll

En **Android**, hacer scroll más alla del limite de una área scrollable muestra un 
[overscroll glow indicator]({{site.api}}/flutter/widgets/GlowingOverscrollIndicator-class.html)
(basado en el color del theme Material actual).

En **iOS**, hacer scoll más alla del límite de un área scrollable provoca 
[overscroll]({{site.api}}/flutter/widgets/BouncingScrollPhysics-class.html)
con una resistencia creciente y vuelve atrás.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/scroll-overscroll.gif" class="figure-img img-fluid rounded" alt="Android and iOS scrollables being flung past their edge and exhibiting platform specific overscroll behavior" />
        <figcaption class="figure-caption">
          Dynamic overscroll comparison
        </figcaption>
      </figure>
    </div>
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/scroll-static-overscroll.gif" class="figure-img img-fluid rounded" alt="Android and iOS scrollables being overscrolled from a resting position and exhibiting platform specific overscroll behavior" />
        <figcaption class="figure-caption">
          Static overscroll comparison
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Momento

En **iOS**, repetidos deslizamientos en la misma dirección acumulan el momento y genera 
más velocidad en cada deslizamiento. No hay un comportamiento equivalente 
en *Android*.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/scroll-momentum-ios.gif" class="figure-img img-fluid rounded" alt="Repeated scroll flings building momentum on iOS" />
        <figcaption class="figure-caption">
          iOS scroll momentum
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Volver arriba

En **iOS**, tocando la barra de estado del SO provoca que el controlodar de scroll principal 
haga scroll hacia la posición superior. No hay un compartamiento equivalente en **Android**.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 22px;" src="../../images/platform-adaptations/scroll-tap-to-top-ios.gif" class="figure-img img-fluid" alt="Tapping the status bar scrolls the primary scrollable back to the top" />
        <figcaption class="figure-caption">
          iOS status bar tap to top
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## Tipografía

Cuando se usa el paquete Material, la tipografía se configura por defecto automáticamente 
a la familia de fuente apropiada para la plataforma. En Android, se usa la fuente Roboto.
En iOS, se usa la familia de fuente del SO San Francisco.

Cuando se usa el paquete Cupertino, el [theme por 
defecto](https://github.com/flutter/flutter/blob/master/packages/flutter/lib/src/cupertino/text_theme.dart)
siempre usa la fuente San Francisco.

La licencia de la fuente San Francisco limita su uso únicamente al software que se ejecuta en iOS,
macOS, o tvOS. Por lo tanto una fuente sustitutiva es usada cuando se ejecuta en Android 
si la plataforma esta sobreescrita en el modo depuración o si se usa el theme Cupertino 
por defecto.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/typography-android.png" class="figure-img img-fluid rounded" alt="Roboto font on Android" />
        <figcaption class="figure-caption">
          Roboto on Android
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/typography-ios.png" class="figure-img img-fluid rounded" alt="San Francisco font on iOS" />
        <figcaption class="figure-caption">
          San Francisco on iOS
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## Iconografía

Cuando se usa el paquete Material, ciertos iconos se muestran diferentes gráficos 
automáticamente dependiendo de la plataforma. Por ejemplo, los 3 puntos del botón overflow 
son verticales en iOS y horizontales en Android. El botón atras es un simple ángulo en iOS 
y una flecha en Android.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/iconography-android.png" class="figure-img img-fluid rounded" alt="Android appropriate icons" />
        <figcaption class="figure-caption">
          Icons on Android
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/iconography-ios.png" class="figure-img img-fluid rounded" alt="iOS appropriate icons" />
        <figcaption class="figure-caption">
          Icons on iOS
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## Feedback háptico

Los paquetes Material y Cupertino desencadenan el apropiado feedback háptico 
en ciertos escenarios.

Por ejemplo, una selección de palabras mediante una pulsación larga en un campo 
de texto desencadena una vibración en Android pero no en iOS.

Hacer scroll a través de los elementos de un selector desencadena un 'ligero golpe de impacto' 
pero ningún feedback en Android.

## Edición de texto

Flutter tambien realiza las siguientes adaptaciones mientras se edita el contenido 
de un campo de texto para coincidir con la plataforma actual.

### Gestos de navegación en el teclado

En **Android**, se pueden realizar deslizamientos horizontales en la barra espaciadora del teclado 
de software para mover el cursor en los campos de texto Material y Cupertino.

En **iOS** los dispositivos con capacidades 3D Touch, un gesto de apretar y arrastrar,
puede hacerse en el teclado de software para mover el cursos en 2D mediante un cursor 
flotante. Esto funciona tanto en los campos de texto Material como Cupertino.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/text-keyboard-move-android.gif" class="figure-img img-fluid rounded" alt="Moving the cursor via the space key on Android" />
        <figcaption class="figure-caption">
          Android space key cursor move
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/text-keyboard-move-ios.gif" class="figure-img img-fluid rounded" alt="Moving the cursor via 3D Touch drag on the keyboard on iOS" />
        <figcaption class="figure-caption">
          iOS 3D Touch drag cursor move
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Barra de herramientas de selección de texto

Con **Material en Android**, la barra de herramientas de selección de estilo Android 
se muestra cuando se hace una selección de texto en un campo de texto.

Con **Material en iOS** o cuando se usa **Cupertino**, la barra de herramientas de selección 
de estilo iOS se muestra cuando se hace una selección de texto en un campo de texto.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/text-toolbar-android.png" class="figure-img img-fluid rounded" alt="Android appropriate text toolbar" />
        <figcaption class="figure-caption">
          Android text selection toolbar
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/text-toolbar-ios.png" class="figure-img img-fluid rounded" alt="iOS appropriate text toolbar" />
        <figcaption class="figure-caption">
          iOS text selection toolbar
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Gesto tap simple

Con **Material en Android**, un tap simple en un campo de texto coloca el cursor en la 
localización del tap.

Una selección de texto también muestra un icono arrastrable para poder mover el cursor.

Con **Material en iOS** o cuando se usa **Cupertino**, un tap simple en un campo 
de texto coloca el cursos cerca del limte de la palabra pulsada.

Las selecciones de texto no tienen un icono arrastrable en iOS.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/text-single-tap-android.gif" class="figure-img img-fluid rounded" alt="Moving the cursor to the tapped position on Android" />
        <figcaption class="figure-caption">
          Android tap
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/text-single-tap-ios.gif" class="figure-img img-fluid rounded" alt="Moving the cursor to the nearest edge of the tapped word on iOS" />
        <figcaption class="figure-caption">
          iOS tap
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Gesto long-press

Con **Material en Android**, un gesto long press selecciona la palabra bajo el gesto long
press. La barra de herramientas de selección se muestra cuando se suelta.

Con **Material en iOS** o cuando se usa **Cupertino**, un gesto long press coloca el cursor 
en la localización del gesto long pres. La barra de herramientas de selección se muestra cuando se 
suelta.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/text-long-press-android.gif" class="figure-img img-fluid rounded" alt="Selecting a word via long press on Android" />
        <figcaption class="figure-caption">
          Android long press
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/text-long-press-ios.gif" class="figure-img img-fluid rounded" alt="Selecting a position via long press on iOS" />
        <figcaption class="figure-caption">
          iOS long press
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Gesto Long-press drag

Con **Material en Android**, arrastrar mientras se mantiene la presión larga expande 
las palabras seleccionadas.

Con **Material en iOS** o cuando se usa **Cupertino**, arrstrar mientras se mantiene la 
presión larga mueve el cursor.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/text-long-press-drag-android.gif" class="figure-img img-fluid rounded" alt="Expanding word selection via long press drag on Android" />
        <figcaption class="figure-caption">
          Android long press drag
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/text-long-press-drag-ios.gif" class="figure-img img-fluid rounded" alt="Moving the cursor via long press drag on iOS" />
        <figcaption class="figure-caption">
          iOS long press drag
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Gesto doble tap

Tanto en Android como en iOS, un doble tap selecciona la palabra que recibe el 
doble tap y muestra la barra de herramientas de selección.

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="../../images/platform-adaptations/text-double-tap-android.gif" class="figure-img img-fluid rounded" alt="Selecting a word via double tap on Android" />
        <figcaption class="figure-caption">
          Android double tap
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="../../images/platform-adaptations/text-double-tap-ios.gif" class="figure-img img-fluid rounded" alt="Selecting a word via double tap on iOS" />
        <figcaption class="figure-caption">
          iOS double tap
        </figcaption>
      </figure>
    </div>
  </div>
</div>