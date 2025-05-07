---
title: /home
layout: home
permalink: /
---

# whoami

![Thomas Hayen](assets/sh4rksicon.jpg){: .profile-image}

Cybersecurity professional with experience in red teaming, malware analysis, and security training. Passionate about offensive and defensive security, I enjoy tackling complex threats and sharing knowledge with the community.

Currently Team Coordinator of the Red Team @easi | HackTheBox Ambassador
Twitter/X: @[0xSH4RKS](https://x.com/0xSH4RKS)

For more details, check out my [LinkedIn profile](https://www.linkedin.com/in/thomashayen/).

# Latest Ramblings

{% assign ramblings = site.ramblings %}

{%- for ramble in ramblings -%}
  {%- if ramble.published_date == nil -%}
    {%- assign ramble_published = ramble.date -%}
  {%- else -%}
    {%- assign ramble_published = ramble.published_date -%}
  {%- endif -%}
{%- endfor -%}

{% assign sorted_ramblings = ramblings | sort: "published_date" | reverse %}

{% for ramble in sorted_ramblings limit: 5 %}
## [{{ ramble.title }}]({{ ramble.url }})
*Published on {{ ramble.published_date | default: ramble.date | date: "%B %d, %Y" }}*

{{ ramble.excerpt | strip_html | truncatewords: 30 }}

[Read more]({{ ramble.url }})

---
{% endfor %}

