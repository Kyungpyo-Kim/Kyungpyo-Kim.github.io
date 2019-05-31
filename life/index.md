---
layout: study
title: "Life"
date: 2019-05-29T11:39:03-04:00
modified:
excerpt: "A collection of thoughts, inspiration, mistakes, and other minutia."
tags: []
image:
  feature: teaser/book004.jpg
  teaser:
---

<div class="tiles">
{% for post in site.categories.life %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->