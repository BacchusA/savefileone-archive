---
layout: default
title: Platforms
permalink: /platforms/
---

# Platforms

{% assign platforms = site.games | map: "platform" | uniq | sort %}

{% for p in platforms %}
## {{ p }}

{% assign games_for_platform = site.games | where: "platform", p | sort: "year" %}

{% for g in games_for_platform %}
- **{{ g.year }}** — [{{ g.title }}]({{ site.baseurl }}{{ g.url }})
{% endfor %}

{% endfor %}
