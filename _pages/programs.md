---
layout: page
title: programs
permalink: /programs/
description: A collection of programs and software projects I have developed.
nav: true
nav_order: 4
display_categories: [graphics, games, applications]
horizontal: false
---
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-0823RLC0T3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

<!-- pages/programs.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized programs -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_programs = site.programs | where: "category", category %}
  {% assign sorted_programs = categorized_programs | sort: "importance" %}
  <!-- Generate cards for each program -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for program in sorted_programs %}
      {% include programs_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for program in sorted_programs %}
      {% include programs.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display programs without categories -->

{% assign sorted_programs = site.programs | sort: "importance" %}

  <!-- Generate cards for each program -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for program in sorted_programs %}
      {% include programs_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for program in sorted_programs %}
      {% include programs.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>