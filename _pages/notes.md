---
layout: page
title: notes
permalink: /notes/
description: A collection of notes I have previously written.
nav: true
nav_order: 3
display_categories: [reinforcement learning, combinatorics, nlp, algebra]
horizontal: false
---

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