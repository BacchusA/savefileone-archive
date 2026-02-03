---
layout: default
title: Home
---
## About

<div class="card">
  <div class="card-text">
    <p><strong>Save File One</strong> is a museum of video game history, built to document how games evolved, why certain ideas survived, and what each milestone passed down to the next generation.</p>

    <p>This archive treats every entry like an exhibit. Not just <em>what it is</em>, but <em>what it changed</em>: mechanics, interfaces, hardware constraints, audience expectations, and the design “grammar” that later games inherited.</p>

    <p>If a title mattered historically, because it introduced a mechanic, defined a genre pattern, or proved a technology—it belongs here. Sometimes that means documenting pre-video artifacts and enabling tech (shooting galleries, photoelectric sensors, radar displays), because the medium didn’t start at a blank screen.</p>

    <p>Each exhibit is paired with a YouTube gameplay video viewing modes that preserve the experience in full—no shortcuts, no summary edits:</p>
    
    <p>Use the <strong>Timeline</strong> to browse chronologically, <strong>Platforms</strong> to follow the hardware thread, and the <strong>Tree</strong> to explore influence relationships across the collection.</p>

    <p>The goal is simple: build a living index of the medium, one artifact at a time, so anyone can trace how we got from blinking lights to modern worlds.</p>
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
  {% if g.platform %}<span class="chip chip-teal">{{ g.platform }}</span>{% endif %}
  <span class="chip chip-gold">{{ g.year }}</span>
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
{% for g in sorted limit:6 %}
  <div class="card">
    <div class="card-title">
      <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
    </div>
    <div class="card-meta">
  {% if g.platform %}<span class="chip chip-teal">{{ g.platform }}</span>{% endif %}
  <span class="chip chip-gold">{{ g.year }}</span>
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
