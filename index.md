---
layout: default
title: Home
---
## About Save File One — Living Catalog

<p class="small">
Save File One is a growing catalog of games I’ve played. Every page is a simple reference entry for the game plus a longplay link, so you can browse by timeline, platform, and whatever rabbit hole you fall into.
</p>

<div class="buttonrow">
  <a class="btn" href="{{ '/timeline/' | relative_url }}">Browse Timeline</a>
  <a class="btn" href="{{ '/platforms/' | relative_url }}">Browse Platforms</a>
</div>

---

## Recently Updated

<div class="cardgrid">
  {% assign games_sorted = site.games | sort: "updated" | reverse %}
  {% for g in games_sorted limit:6 %}
    <div class="card">
      <div class="card-text">

        <div class="card-top">
          <div class="card-title">
            <a href="{{ g.url | relative_url }}">{{ g.title }}</a>
          </div>

          <div class="card-meta">
            {% if g.year %}<span class="chip chip-gold">{{ g.year }}</span>{% endif %}
            {% if g.platform %}<span class="chip chip-cyan">{{ g.platform }}</span>{% endif %}
          </div>
        </div>

        {% if g.short_summary %}
          <p class="card-summary">{{ g.short_summary }}</p>
        {% endif %}

        <div class="card-actions">
          {% if g.youtube_longplay and g.youtube_longplay != "" %}
            <a class="btn" href="{{ g.youtube_longplay }}" target="_blank" rel="noopener">Watch Longplay</a>
          {% else %}
            <a class="btn btn-ghost" href="{{ g.url | relative_url }}">Open Page</a>
          {% endif %}

          {% if g.youtube_main_episode and g.youtube_main_episode != "" %}
            <a class="btn btn-ghost" href="{{ g.youtube_main_episode }}" target="_blank" rel="noopener">Short Episode</a>
          {% endif %}
        </div>

      </div>
    </div>
  {% endfor %}
</div>

<p class="small muted" style="margin-top:10px;">
  Showing the 6 most recently updated entries.
</p>

