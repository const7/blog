---
layout: notpost
title: Wiki
permalink: /wiki
---

<section class="container">

  <!-- description -->
  <blockquote>
  <p>常用操作/工具等。</p>
  </blockquote>

  <!-- wiki list -->
  {% for wiki in site.wiki %}
  {% if wiki.title != "Wiki Template" %}
    <li class="wiki-item"><a href="{{ site.baseurl }}{{ wiki.url }}">{{ wiki.title }}</a></li>
  {% endif %}
  {% endfor %}

</section>