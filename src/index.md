---
layout: home
title: "News"
---

{% for post in site.posts %}
- [{{post.title}}]({% link {{ post.path }} %} )
{% endfor %}
