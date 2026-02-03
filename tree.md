---
layout: default
title: Ancestry Tree
permalink: /tree/
---

# Ancestry Tree

<p class="small">
This graph is generated from each game's <code>influences:</code> list.
If an influence name exactly matches another game’s <code>title</code>, it will link correctly.
</p>

<div class="card" style="margin: 14px 0;">
  <div class="card-text">
    Tip: For best results, keep influence <code>name</code> values identical to the target game’s <code>title</code>.
  </div>
</div>

<div class="card">
  <div class="card-text">
    <div class="mermaid">
flowchart LR
{% assign all = site.games %}

%% Define nodes (unique IDs: year + slugified title)
{% for g in all %}
  {{ g.year }}-{{ g.title | slugify }}["{{ g.title }} ({{ g.year }})"]
{% endfor %}

%% Make nodes clickable
{% for g in all %}
 click {{ g.year }}-{{ g.title | slugify }} "{{ site.baseurl }}{{ g.url }}" _self
{% endfor %}

%% Define edges: Influence -> Game (only if influence name matches a title)
{% for g in all %}
  {% if g.influences %}
    {% for i in g.influences %}
      {% assign match = all | where: "title", i.name | first %}
      {% if match %}
        {{ match.year }}-{{ match.title | slugify }} --> {{ g.year }}-{{ g.title | slugify }}
      {% endif %}
    {% endfor %}
  {% endif %}
{% endfor %}
    </div>
  </div>
</div>

<script type="module">
  import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
  mermaid.initialize({ startOnLoad: true, theme: "dark", securityLevel: "loose" });
</script>
