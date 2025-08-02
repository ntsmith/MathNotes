---
layout: default
---
{% capture listing %}
# {{ page.title }}

{% if page.name == "index.md" %}

  {% assign subdirs = site.pages | where: "name", "index.md" %}
  {% assign subdirs_len = subdirs | size %}

  {% assign prefix = page.dir %}
  {% assign prefix_len = prefix | size %}

  {% assign all_sub_dirs = "" | split: "," %}
  {% for p in subdirs %}
    {% assign d = p.dir %}
    {% assign suffix_len = d | size | minus: prefix_len %}
    {% assign d_prefix = d | slice: 0, prefix_len %}
    {% assign d_suffix_len = d | slice: prefix_len, suffix_len | split: "/" | size %}
    {% if d_prefix == prefix and d_suffix_len == 1 %}
      {% assign d_page = site.pages | where: "url", d | first %}
      {% assign all_sub_dirs = all_sub_dirs | push: d_page %}
      {% assign all_sub_dirs_len = all_sub_dirs | size %}
    {% endif %}
  {% endfor %}

  {% assign siblings = site.pages |where_exp: "p", "p.dir == page.dir" | where_exp: "p", "p.name != page.name" | where_exp: "p", "p.name != index_name" %}
  {% assign siblings_len = siblings | size %}
  {% if siblings_len > 0 %}
## Sections
    {% for p in siblings %}
- [{{ p.title }}]({{ p.url }})
    {% endfor %}
  {% endif %}

  {% if all_sub_dirs_len > 0 %}
## SubModules
    {% for d_page in all_sub_dirs %}
- [{{ d_page.title }}]({{ d_page.url }})
    {% endfor %}
  {% endif %}

{% endif %}

{% endcapture %}
{{ listing | markdownify }}

{{ content }}