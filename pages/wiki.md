---
layout: wiki
title: Wiki
permalink: /wiki
---

> 常用操作/工具等。

{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" %}
  <li class="wiki-item"><a href="{{ site.baseurl }}{{ wiki.url }}">{{ wiki.title }}</a></li>
{% endif %}
{% endfor %}
