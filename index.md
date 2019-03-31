---
title: CS56
permalink: "/"
---

# {{site.course}}, {{site.quarter}}

{% include collapse-button.html label="Information" id="info-list" %}
<div class="collapse" id="info-list">
 <div class="card card-body">
  {% include info_list.html %}
 </div>
</div>

{% include collapse-button.html label="Labs" id="labs" %}
<div class="collapse" id="labs">
 <div class="card card-body">
 {% include lab_table.html %}
 </div>
</div>

{% include collapse-button.html label="Homework" id="hwk" %}
<div class="collapse" id="hwk">
 <div class="card card-body">
  {% include hwk_table.html %}
 </div>
</div>

{% include collapse-button.html label="Lectures" id="lectures" %}
<div class="collapse" id="lectures">
 <div class="card card-body">
  
  {%include lectures_table.html %}
 </div>
</div>
