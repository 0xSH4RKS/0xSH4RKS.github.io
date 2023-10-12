---
title: /ramblings
layout: home
permalink: /ramblings
---

# Ramblings

{% for ramble in site.ramblings %}
  <h2><a href="{{ ramble.url }}">{{ ramble.title }}</a></h2>
  <p>{{ ramble.content | truncatewords: 50 }}</p>
{% endfor %}
