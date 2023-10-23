---
title: /press
layout: home
permalink: /press
---

# Press

{% for press in site.press %}
  <h2><a href="{{ press.url }}">{{ press.title }}</a></h2>
  <p>{{ press.content | truncatewords: 50 }}</p>
{% endfor %}
