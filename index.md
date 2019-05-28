---
layout: home
permalink: /
image:
  feature: main.jpg
---

<div class="tiles">
{% for post in site.posts %}
	{% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->
