---
layout: notpost
title: Archives
permalink: /archives
---

<section class="container">
    <!-- group by year -->
    {% assign group_post = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
    {% for post in group_post %}
    <h3>{{ post.name }}<sup><small>{{ post.items.size }}</small></sup></h3>
        <!-- list posts of current year -->
        <ol class="items-list">
            {% for item in post.items %}
                <li class="items-list-item">
                    <span class="items-list-meta">{{ item.date | date:"%Y-%m-%d" }}</span>
                    <a class="items-list-name" href="{{ site.baseurl }}{{ item.url }}">{{ item.title }}</a>
                </li>
            {% endfor %}
        </ol>
    {% endfor %}
</section>
