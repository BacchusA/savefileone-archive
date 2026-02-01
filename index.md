---
layout: default
title: Home
---

# Save File One — Archive

Welcome to the Archive: exhibit pages that explain what a game did, why it mattered, and who it influenced.

- [Start Here]({{ site.baseurl }}/start-here/)
- [Browse the timeline]({{ site.baseurl }}/timeline/)
- [Browse by platform]({{ site.baseurl }}/platforms/)

---

## Featured

{% assign featured = site.games | where: "featured", true | sort: "year" %}
{% if featured.size == 0 %}
*(No featured games yet — add `featured: true` to any game file.)*
{% endif %}

<div style="display:grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 14px;">
{% for g in featured %}
  <div style="border:1px solid #ddd; border-radius:12px; padding:12px;">
    <div style="font-weight:800; font-size:18px;">
      <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
    </div>
    <div style="opacity:0.8; margin-top:4px;">
      {{ g.year }} • {{ g.platform }}
    </div>
    {% if g.short_summary %}
    <div style="margin-top:8px;">{{ g.short_summary }}</div>
    {% endif %}
  </div>
{% endfor %}
</div>

---

## Catalog (Newest first)

{% assign sorted = site.games | sort: "year" | reverse %}

<div style="display:grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 14px;">
{% for g in sorted %}
  <div style="border:1px solid #eee; border-radius:12px; padding:12px;">
    <div style="font-weight:800; font-size:18px;">
      <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
    </div>
    <div style="opacity:0.8; margin-top:4px;">
      {{ g.year }} • {{ g.platform }}
    </div>
    {% if g.short_summary %}
    <div style="margin-top:8px;">{{ g.short_summary }}</div>
    {% endif %}
  </div>
{% endfor %}
</div>
