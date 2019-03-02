---
title: Catálogo de Widgets
short-title: Catálogo
toc: false
---

Crea hermosas apps más rápido con la colección de widgets visuales, estructurales,
de plataforma, e interactivos de Flutter. Además de navegar por los widgets por categoría,
también puedes ver todos los widgets en el [índice de widgets](/docs/reference/widgets).

<div class="card-deck card-deck--responsive">
{% for section in site.data.catalog.index %}
    <div class="card">
        <div class="card-body">
            <a href="{{section.id}}"><header class="card-title">{{section.name}}</header></a>
            <p class="card-text">{{section.description}}</p>
        </div>
        <div class="card-footer card-footer--transparent">
            <a href="{{section.id}}">Visitar</a>
        </div>
    </div>
{% endfor %}
</div>
