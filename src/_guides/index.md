---
title: Documentación de Dart
description: Aprende a usar el lenguaje y las bibliotecas de Dart.
toc: false
---

¡Bienvenido a la documentación de Dart!
Para obtener una lista de los cambios en este sitio (páginas nuevas, 
lineamientos nuevos y más), consulta la [Página de novedades][].

[Página de novedades]: /guides/whats-new

Estas son algunas de las páginas más visitadas de este sitio:

{% comment %}
Para actualizar estas tarjetas, edita src/_data/docs_cards.yml.
{% endcomment %}

<div class="card-grid">
{% for card in site.data.docs_cards -%}
  {% capture index0Modulo3 %}{{ forloop.index0 | modulo:3 }}{% endcapture %}
  {% capture indexModulo3 %}{{ forloop.index | modulo:3 }}{% endcapture %}
  <div class="card">
    <h3><a href="{{card.url}}">{{card.name}}</a></h3>
    <p>{{card.description}}</p>
  </div>
{% endfor -%}
</div>
