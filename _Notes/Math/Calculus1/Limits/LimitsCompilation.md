---
title: Limits Compilation
layout: folder
children:
  - LimitLaws.html
  - Theorem1.html
---
{% assign page_url_parts = page.url | split: '/' %}
{% assign page_url = page_url_parts | join: '/' %}

{% assign dir_url_len = page_url_parts | size | minus: 1 %}
{% assign dir_url_parts = page_url_parts | slice: 0, dir_url_len %}
{% for child_name in page.children %}
  {% assign child_url = dir_url_parts | push: child_name  | join: '/' %}
  {% assign child = site.Notes | where: 'url', child_url | first %}
## {{ child.title }}
  {{ child.content }}
{% endfor %}
