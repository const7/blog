---
layout: post
title: Wiki
permalink: /wiki
---

> 常用操作/工具等。

<ul class="wiki-item">
    {% for wiki in site.wiki %}
    {% if wiki.title != "Wiki Template" %}
    <li class="wiki-item"><a href="{{ site.url }}{{ wiki.url }}">{{ wiki.title }}</a></li>
    {% endif %}
    {% endfor %}
</ul>