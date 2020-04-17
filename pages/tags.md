---
layout: tags
title: Tags
permalink: /tags
---

<section class="container">
  <div class="list-tag">
    <div>
      {% assign sorted_tags = site.tags | sort %}
      {% for tag in sorted_tags %}
        <a href="#{{ tag[0] }}">{{ tag | first }}<sup>{{ tag[1].size }}</sup></a>
      {% endfor %}
    </div>
  </div>
</section>

<section class="container">
  {% assign sorted_tags = site.tags | sort %}
  {% for tag in sorted_tags %}
  <h3>{{ tag | first }}</h3>
  <ol class="tags-list" id="{{ tag[0] }}">
    {% for post in tag.last %}
    <li class="tags-list-item">
      <span class="tags-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
      <a class="tags-list-name" href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ol>
  {% endfor %}
</section>
