---
layout: page
permalink: /categories/Tech
title: ""
order: 1000
---


{% for post in site.categories.Tech %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}