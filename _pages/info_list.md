---
title: Info Pages List
permalink: "/info_list/"
---

# {{site.course}}, {{site.quarter}}

## Course Information

<ul>
{% for item in site.info %}
<li><a href="{{item.url | relative_url }}"  data-ajax="false">{{item.title }}</a></li>
{% endfor %}
</ul>


