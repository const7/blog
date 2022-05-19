---
layout: notpost
title: Tags
permalink: /tags
---

<section class="container">
  <div class="list-tags">
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
  <ol class="items-list" id="{{ tag[0] }}">
    {% for post in tag.last %}
    <li class="items-list-item">
      <span class="items-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
      <a class="items-list-name" href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ol>
  {% endfor %}
</section>
