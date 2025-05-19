---
layout: home
title: "Research Software"
permalink: /software/
---

{% for post in site.software %}
- [{{post.title}}]({% link {{ post.path }} %} )
{% endfor %}
