---
layout: page
title: notes
permalink: /notes/
description: A collection of notes and research work I have completed.
nav: true
nav_order: 3
display_categories: [reinforcement learning, nlp, algebra, combinatorics]
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

<!-- pages/notes.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized notes -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_notes = site.notes | where: "category", category %}
  {% assign sorted_notes = categorized_notes | sort: "importance" %}
  <!-- Generate cards for each note -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for note in sorted_notes %}
      {% include notes_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for note in sorted_notes %}
      {% include notes.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display notes without categories -->

{% assign sorted_notes = site.notes | sort: "importance" %}

  <!-- Generate cards for each note -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for note in sorted_notes %}
      {% include notes_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for note in sorted_notes %}
      {% include notes.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>