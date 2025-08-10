---
layout: default
---
{% assign page_url_parts = page.url | remove: 'index.html'  | remove: '.html' | split: '/' | where_exp: 'p', 'p != ""' %}
{% assign page_url = page_url_parts | join: '/' %}
{% assign parent_url_parts_len = page_url_parts | size | minus: 1 %}
{% assign parent_url = page_url_parts | slice: 0, parent_url_parts_len | join: '/' %}

{% assign ancestors = '' | split: '' %}
{% for part in page_url_parts %}
  {% assign idx = forloop.index0 | plus: 1 %}

  {% assign cur_my_url = page_url_parts | slice: 0, idx | join: '/' %}
  {% assign cur_url = '/' | append: cur_my_url | append: '/index.html' %}
  {% assign cur_ancestor = site.Notes | where: "url", cur_url | first %}
  {% assign ancestors = ancestors | push: cur_ancestor %}
{% endfor %}

{% assign all_notes = site.Notes | sort: "order" %}
{% assign siblings = '' | split: '' %}
{% assign children = '' | split: '' %}
{% for note in all_notes %}
  {% assign idx = forloop.index0 %}
  {% assign note_url_parts = note.url | remove: 'index.html' | remove: '.html' | split: '/' | where_exp: 'p', 'p != ""' %}
  {% assign note_url = note_url_parts | join: '/' %}
  {% assign note_parent_parts_len = note_url_parts | size | minus: 1 %}
  {% assign note_parent_url = note_url_parts | slice: 0, note_parent_parts_len | join: '/' %}

  {% comment %}If current page, get next and previous.{% endcomment %}
  {% if note_url == page_url %}  
    {% assign prev_idx = idx | minus: 1 %}
    {% assign next_idx = idx | plus: 1 %}
    {% if idx | minus: 1 >= 0 %}
      {% assign prev = all_notes[prev_idx] %}
    {% endif %}
    {% assign next = all_notes[next_idx] %}
  {% endif %}

  {% comment %}Check if page is a sibling{% endcomment %}
  {% if note_parent_url == parent_url %}
    {% assign siblings = siblings | push: note %}
  {% endif %}
  
  {% comment %}If page is directory, check if note is child{% endcomment %}
  {% if page.slug == 'index' %}
    {% if note_parent_url == page_url %}
      {% assign children = children | push: note %}
    {% endif %}
  {% endif %}
{% endfor %}

{%- include breadcrumbs.html -%}
{%- include sidebar.html ancestors=ancestors siblings=siblings -%}

{% capture listing %}

# {{ page.title }}
{{ content }}

{% assign children_len = children | size %}
{% if children_len > 0 %}
## Contents
  {% for note in children %}
    {% assign note_url = note.url | remove: 'index.html' | remove: '.html' | split: '/' | where_exp: 'p', 'p != ""' | join: '/' %}
- [{{ note.title }}]({{ site.baseurl }}/{{ note_url }})
  {% endfor %}
{% endif %}

**site.url** ({{ site.url }}) <br/>
**site.baseurl** ({{ site.baseurl }}) <br/>
**page.path** ({{ page.path }}) <br/>
**page.relative_path** ({{ page.relative_path }}) <br/>
**page.url** ({{ page.url }})<br/>
**page.collection** ({{ page.collection }})<br/>

{% endcapture %}
{{ listing | markdownify }}

{%- include pagination.html prev=prev next=next -%}
