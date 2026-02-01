---
layout: default
title: Timeline
permalink: /timeline/
---

# Timeline

{% assign sorted = site.games | sort: "year" %}
{% for g in sorted %}
### {{ g.year }} — [{{ g.title }}]({{ site.baseurl }}{{ g.url }})
**Platform:** {{ g.platform }}  
{% if g.short_summary %}{{ g.short_summary }}{% endif %}

{% endfor %}
