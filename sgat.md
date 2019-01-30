---
layout: page
title: Tags
permalink: /tags/
---
<div class="post">
{% assign tags = site.tags | sort %}
{% for tag in tags %}
<ul>
 <li>
  <a href="/tag/{{ tag | first | slugify }}/">{{ tag[0] | replace:'-', ' ' }} ({{ tag | last | size }})</a>
 </li>
</ul>
{% endfor %}
</div>
