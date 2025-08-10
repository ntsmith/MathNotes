---
layout: default
---
## Pages by Prefix

{% include function/children_by_prefix.html prefix='/Notes/Math/' from=site.Notes %}
{% if output %}
<b>True</b><br/>
{% else %}
<b>False</b><br/>
{% endif %}
<b>output</b> ({{ output }})<br/>