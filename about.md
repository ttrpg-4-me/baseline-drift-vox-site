---
layout: page
title: About Us
permalink: /about/
---

<div class="about-intro">
  {% capture about_content %}{% include_relative text/about_us.md %}{% endcapture %}
  {{ about_content | markdownify }}
</div>

<h2>Key Personnel</h2>

<div class="bio-grid">
  {% for bio in site.bios %}
  <div class="bio-card">
    {% if bio.image %}
    <img class="bio-image" src="{{ bio.image }}" alt="Personnel photo">
    {% endif %}
    <div class="bio-info">
      {{ bio.content | markdownify }}
    </div>
  </div>
  {% endfor %}

  {% if site.bios.size == 0 %}
  <p class="dim">[No personnel on file. Add .md files to text/_bios/ to populate this section.]</p>
  {% endif %}
</div>
