---
layout: page
title: "Categories"
permalink: /categories/
order: 3
---

<!-- [cdz]: 参考
https://stackoverflow.com/questions/34514852/having-jekyll-categories-count-and-linked-to-a-list-of-posts-related-to-it -->
<ul class="tag-box inline">
  {% for category in site.categories %}
    <li>
    <div><a href="{{ site.baseurl }}/categories/{{ category[0] }}">{{ category[0]}} <span>  ({{ category[1].size }}) </span></a></div>
    </li>
  {% endfor %}
</ul>
