---
layout: page
permalink: /categories/Life
title: ""
order: 1000
---


{% for post in site.categories.Life %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
