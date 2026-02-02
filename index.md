---
layout: default
title: Home
---

## Featured

{% assign featured = site.games | where: "featured", true | sort: "year" %}
{% if featured.size == 0 %}
*(No featured games yet — add `featured: true` to any game file.)*
{% endif %}

<div class="cardgrid">
{% for g in featured %}
  <div class="card">
    <div class="card-title">
      <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
    </div>
    <div class="card-meta">
      {{ g.year }}
      {% if g.month %} • {{ g.month }}{% endif %}
      {% if g.day %} {{ g.day }}{% endif %}
      {% if g.platform %} • {{ g.platform }}{% endif %}
    </div>
    {% if g.short_summary %}
    <div class="card-text">{{ g.short_summary }}</div>
    {% endif %}
  </div>
{% endfor %}
</div>

---

## Catalog (Newest first)

{% assign sorted = site.games | sort: "year" | reverse %}

<div class="cardgrid">
{% for g in sorted %}
  <div class="card">
    <div class="card-title">
      <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
    </div>
    <div class="card-meta">
      {{ g.year }}
      {% if g.month %} • {{ g.month }}{% endif %}
      {% if g.day %} {{ g.day }}{% endif %}
      {% if g.platform %} • {{ g.platform }}{% endif %}
    </div>
    {% if g.short_summary %}
    <div class="card-text">{{ g.short_summary }}</div>
    {% endif %}
  </div>
{% endfor %}
</div>

<p class="small" style="margin-top:18px;">
Tip: mark a game as featured by adding <code>featured: true</code> in its front-matter.
</p>
