---
title: Flutter SDK releases
short-title: Releases
description: Todo los releases actuales del SDK de Flutter,  tanto estable como de desarrollo.
toc: false
---

<style>
.scrollable-table {
  overflow-y: scroll;
  max-height: 20rem;
}
</style>

El canal {{site.sdk.channel | capitalize }} contiene la compilación más estable de Flutter. Mira [canales de Flutter][] para más detalles.

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="windows-tab" href="#windows" role="tab" aria-controls="windows" aria-selected="true">Windows</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="macos-tab" href="#macos" role="tab" aria-controls="macos" aria-selected="false">macOS</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="linux-tab" href="#linux" role="tab" aria-controls="linux" aria-selected="false">Linux</a>
  </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div id="sdk-archives" class="tab-content">
{% include_relative _os.md os="Windows" %}
{% include_relative _os.md os="macOS" %}
{% include_relative _os.md os="Linux" %}
</div>

## Canal masterMaster channel

No hay disponibles bundles de instalación para master. Sin embargo, puedes obtener el SDK
directamente desde el [repositorio de GitHub]({{site.repo.flutter}}) clonando el canal master,
y entonces desencadenando la descarga de las dependencias del SDK:

```terminal
$ git clone -b master https://github.com/flutter/flutter.git
$ ./flutter/bin/flutter --version
```

Para detalles adicionales sobre como los bundles de instalación estan estructurados, mira
[bundles de Instalación][].

[canales de Flutter]: {{site.repo.flutter}}/wiki/Flutter-build-release-channels
[bundles de Instalación]: {{site.repo.flutter}}/wiki/Flutter-Installation-Bundles
