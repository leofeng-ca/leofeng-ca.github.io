---
layout: page
permalink: /teaching/
title: Teaching
description: Teaching assistant experience
nav: true
nav_order: 2
---

<div class="cv">
  {% for entry in site.data.teaching %}
    <div class="card mt-3 p-3">
      <h3 class="card-title font-weight-medium">{{ entry.title }}</h3>
      <div>
        {% include cv/time_table.html %}
      </div>
    </div>
  {% endfor %}
</div>
