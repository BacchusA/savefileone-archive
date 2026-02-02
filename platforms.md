---
layout: default
title: Platforms
permalink: /platforms/
---

# Platforms

<p class="small">Click a platform to expand its games.</p>

{% assign platforms = site.games | map: "platform" | uniq | sort %}

<div class="platforms">

{% for p in platforms %}
  {% assign games_for_platform = site.games | where: "platform", p | sort: "year" %}

  <details class="plat">
    <summary>
      <span class="plat-name">{{ p }}</span>
      <span class="plat-count">({{ games_for_platform | size }})</span>
    </summary>

    <div class="plat-panel">
      {% for g in games_for_platform %}
        <div class="plat-item">
          <a href="{{ site.baseurl }}{{ g.url }}">{{ g.year }} — {{ g.title }}</a>
        </div>
      {% endfor %}
    </div>
  </details>

{% endfor %}

</div>
