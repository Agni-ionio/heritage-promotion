---
layout: page
title: POIs List
permalink: /pois/
---

<h2 class="category-title lakes">
  🌿 Lakes & Serenity
</h2>
<ul>
{% for p in site.pois %}
  {% if p.category == "lake" %}
    <li>
      <a href="{{ p.url | relative_url }}">{{ p.title }}</a>
    </li>
  {% endif %}
{% endfor %}
</ul>

<h2 class="category-title rivers">
  🌊 Rivers & Adventure
</h2>
<ul>
{% for p in site.pois %}
  {% if p.category == "river" %}
    <li>
      <a href="{{ p.url | relative_url }}">{{ p.title }}</a>
    </li>
  {% endif %}
{% endfor %}
</ul>

<h2 class="category-title mountains">
  🏔️ Mountain Escapes
</h2>
<ul>
{% for p in site.pois %}
  {% if p.category == "mountain" %}
    <li>
      <a href="{{ p.url | relative_url }}">{{ p.title }}</a>
    </li>
  {% endif %}
{% endfor %}
</ul>


