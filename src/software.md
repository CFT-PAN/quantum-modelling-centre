---
layout: home
title: "Research Software"
permalink: /software/
---

<ul>
  {% for post in site.software %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
