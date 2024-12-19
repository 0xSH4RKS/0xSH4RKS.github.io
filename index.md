---
title: /home
layout: home
permalink: /
---

# whoami

![Thomas Hayen](assets/sh4rksicon.jpg){: .profile-image}

I am a seasoned cybersecurity professional with a robust background in red teaming, malware development, and security training. I have demonstrated expertise in identifying and mitigating complex security threats, contributing significantly to enhancing organizational security postures.

Throughout my career, I have been dedicated to advancing cybersecurity practices, focusing on developing innovative strategies to counteract emerging threats. My commitment to continuous learning and knowledge sharing has established me as a respected figure in the cybersecurity community.

My professional journey reflects a steadfast dedication to the field of cybersecurity, marked by a series of accomplishments that underscore my skills and expertise. My contributions have been instrumental in fortifying the security frameworks of the organizations I have collaborated with.

For a comprehensive overview of my professional experience and achievements, please visit my [LinkedIn profile](https://www.linkedin.com/in/thomashayen/).

# Latest Ramblings

{% for ramble in site.ramblings limit: 5 %}
## [{{ ramble.title }}]({{ ramble.url }})
*Published on {{ ramble.date | date: "%B %d, %Y" }}*

{{ ramble.excerpt | strip_html | truncatewords: 30 }}

[Read more]({{ ramble.url }})

---
{% endfor %}
