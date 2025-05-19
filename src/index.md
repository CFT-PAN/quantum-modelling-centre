---
layout: home
title: "News"
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ site.baseurl }}{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

{% for post in site.posts %}
- [{{post.title}}]({% link {{ post.path }} %} )
{% endfor %}
