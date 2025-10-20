---
title: "All Categories"
layout: single
permalink: /categories/
---

# Categories

{% for category in site.categories %}
- [{{ category[0] | capitalize }}](#{{ category[0] | slugify }})
{% endfor %}

---

{% for category in site.categories %}
## <a id="{{ category[0] | slugify }}"></a>{{ category[0] | capitalize }}

<div class="entries-list">
  {% for post in category[1] %}
    <article class="entry">
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <p><small>{{ post.date | date: "%B %d, %Y" }}</small></p>
      {% if post.excerpt %}
        <p>{{ post.excerpt }}</p>
      {% endif %}
    </article>
  {% endfor %}
</div>

---
{% endfor %}
