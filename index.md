---
layout: default
title: Home
---

## About

<div class="card">
  <div class="card-text">
    <p><strong>Save File One</strong> is a playable museum of video game history.</p>

    <p>Each entry is an exhibit page: what the game did, why it mattered, and how its ideas flowed forward. Some entries are technologies and cultural artifacts—because games didn’t appear out of nowhere; they inherited interfaces, hardware, and habits from earlier machines.</p>

    <p>This archive pairs two viewing modes:</p>
    <ul>
      <li><strong>Short episodes</strong> for the exhibit-style overview</li>
      <li><strong>Longplays</strong> for the full artifact, preserved in motion</li>
    </ul>

    <p>Use <strong>Timeline</strong> to browse chronologically, <strong>Platforms</strong> to follow hardware lineages, and <strong>Tree</strong> to see influence relationships across the collection.</p>
  </div>
</div>

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

## Catalog (Recently Updated)

{% assign updated_games = site.games | where_exp: "g", "g.updated != nil" | sort: "updated" | reverse %}
{% assign non_updated = site.games | where_exp: "g", "g.updated == nil" | sort: "year" | reverse %}
{% assign sorted = updated_games | concat: non_updated %}


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
