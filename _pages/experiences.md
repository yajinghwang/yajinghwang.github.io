---
layout: page
permalink: /experiences/
title: experiences
description: past research, industry and public-interest experiences
years: [2026,2025,2024,2023,2022]
nav: true
nav_order: 4
---
<!-- _pages/publications.md -->
<div class="publications">

{%- for y in page.years %}
  {% bibliography -f experiences -q @*[year={{y}}]* %}
{% endfor %}

</div>