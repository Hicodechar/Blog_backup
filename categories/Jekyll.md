---
layout: page
permalink: /categories/Jekyll
title: ""
order: 1000
---


{% for post in site.categories.Jekyll %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
