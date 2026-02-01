---
layout: default
title: Start Here
permalink: /start-here/
---

# Start Here

This is the guided path — a short list that explains the mission fast.

## The “Starter Exhibit” set
{% assign featured = site.games | where: "featured", true | sort: "year" %}
{% for g in featured %}
- **{{ g.year }}** — [{{ g.title }}]({{ site.baseurl }}{{ g.url }}) ({{ g.platform }})  
  {% if g.short_summary %}{{ g.short_summary }}{% endif %}
{% endfor %}

## Suggested viewing order
1. Pick one early game (pre-1975)
2. Then jump to one descendant that “inherits” the mechanic
3. Then browse the Timeline to keep marching forward
