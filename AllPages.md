---
title: All Pages
layout: default
---
{% assign siblings = site.pages %}

## All Pages
{% for page in siblings %}
- [{{ page.path}}]({{ page.url }})
{% endfor %}