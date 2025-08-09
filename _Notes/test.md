---
layout: default
---
## Pages by Prefix

{% include function/ends_with.html string='asdf' suffix='df' %}
{% if output %}
<b>True</b><br/>
{% else %}
<b>False</b><br/>
{% endif %}
<b>output</b> ({{ output }})<br/>