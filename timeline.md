---
layout: default
title: Timeline
permalink: /timeline/
---

# Timeline

<p class="small">Click a year to expand its games.</p>

<div class="timeline">
{% assign sorted = site.games | sort: "year" %}
{% assign current_year = "" %}

{% for g in sorted %}
  {% if g.year != current_year %}
    {% if current_year != "" %}
        </div>
      </details>
    </div>
    {% endif %}

    {% assign current_year = g.year %}

    <div class="tl-item">
      <div class="tl-dot"></div>

      <details class="tl-year">
        <summary>
          <span class="tl-year-label">{{ current_year }}</span>
          <span class="tl-year-count">
            ({{ site.games | where: "year", current_year | size }} item{% if (site.games | where: "year", current_year | size) != 1 %}s{% endif %})
          </span>
        </summary>

        <div class="tl-panel">
  {% endif %}

          <div class="tl-game">
            <div class="tl-game-title">
              <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
            </div>
            {% endif %}
          </div>

{% endfor %}

{% if current_year != "" %}
        </div>
      </details>
    </div>
{% endif %}
</div>
