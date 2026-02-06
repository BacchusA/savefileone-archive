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

    <hr class="hr-soft" />

    <p><strong>Confidence Tags</strong> appear on some exhibits to show what kind of evidence supports a claim:</p>
    <ul>
      <li><strong>[primary-source]</strong> — Patents, manuals, interviews, and other original records from the time.</li>
      <li><strong>[secondary-source]</strong> — Books, articles, documentaries, and other research discussing the originals.</li>
      <li><strong>[inference]</strong> — A logical connection based on available facts, even if not stated directly.</li>
    </ul>

    <hr class="hr-soft" />

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

