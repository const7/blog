---
layout: categories
title: Categories
permalink: /categories
---

<section class="container">
  <div class="list-category">
    <div>
      {% assign sorted_categories = site.categories | sort %}
      {% for category in sorted_categories %}
        <a href="#{{ category[0] }}">{{ category | first }}<sup>{{ category[1].size }}</sup></a>
        <!-- <span>{{ category[1].size }}</span> -->
      {% endfor %}
    </div>
  </div>
</section>

<section class="container">
  {% assign sorted_categories = site.categories | sort %}
  {% for category in sorted_categories %}
  <h3>{{ category | first }}</h3>
  <ol class="categories-list" id="{{ category[0] }}">
    {% for post in category.last %}
    <li class="categories-list-item">
      <span class="categories-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
      <a class="categories-list-name" href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ol>
  {% endfor %}
</section>
