---
title: Índice de widgets
short-title: Widgets
show_breadcrumbs: false
---

{% assign sorted = site.data.catalog.widgets | sort:'name' -%}

Este es un listado alfabético de casi todos los widgets que vienen con 
Flutter. También puedes [navegar por los widgets por categoría][catalog].

Quizás también quieras revisar nuestra serie de vídeos Widget of the Week 
en el [canal de Youtube de Flutter]({{site.social.youtube}}). Cada corto episodio 
trata de un widget de Flutter diferente. Para ver más series de videos, mira 
nuestra página de [videos](/docs/resources/videos).

<iframe width="560" height="315" src="https://www.youtube.com/embed/b_sQ9bMltGU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Widget of the Week playlist](https://www.youtube.com/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)

<div class="card-deck card-deck--responsive">
{% for comp in sorted %}
    <div class="card">
        <a href="{{comp.link}}">
            <div class="card-image-holder">
                {{comp.image}}
            </div>
        </a>
        <div class="card-body">
            <a href="{{comp.link}}"><header class="card-title">{{comp.name}}</header></a>
            <p class="card-text">{{comp.description}}</p>
        </div>
        <div class="card-footer card-footer--transparent">
            <a href="{{comp.link}}">Documentación</a>
        </div>
    </div>
{% endfor %}
</div>

[catalog]: /docs/development/ui/widgets