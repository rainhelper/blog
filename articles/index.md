---
layout: archive
title: "Articles"
excerpt: "article test"
image:
  feature:
  teaser:
---

<div class="tiles">
{% for post in site.categories.articles %}
  {% include post-list.html %}
{% endfor %}

{% include paginator.html %}
</div><!-- /.tiles -->
