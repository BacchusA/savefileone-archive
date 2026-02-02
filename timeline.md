---
layout: default
title: Timeline
permalink: /timeline/
---

# Timeline

<p class="small">Click a year → month → day to drill down. Unknown dates are grouped automatically.</p>

<div class="timeline">

{% assign years_sorted = site.games | map: "year" | uniq | sort %}

{% for y in years_sorted %}
  {% assign games_year = site.games | where: "year", y %}

  <div class="tl-item">
    <div class="tl-dot"></div>

    <details class="tl-year">
      <summary>
        <span class="tl-year-label">{{ y }}</span>
        <span class="tl-year-count">({{ games_year | size }} item{% if games_year.size != 1 %}s{% endif %})</span>
      </summary>

      <div class="tl-panel tl-nested">

        {%- comment -%} Known months 1..12 {%- endcomment -%}
        {% for m in (1..12) %}
          {% assign games_month = games_year | where: "month", m %}
          {% if games_month.size > 0 %}

            <details class="tl-month">
              <summary>
                <span class="tl-month-label">{{ m | prepend: "0" | slice: -2, 2 }}</span>
                <span class="tl-year-count">({{ games_month | size }})</span>
              </summary>

              <div class="tl-panel tl-subpanel">

                {%- comment -%} Known days 1..31 {%- endcomment -%}
                {% for d in (1..31) %}
                  {% assign games_day = games_month | where: "day", d %}
                  {% if games_day.size > 0 %}

                    <details class="tl-day">
                      <summary>
                        <span class="tl-day-label">{{ d | prepend: "0" | slice: -2, 2 }}</span>
                        <span class="tl-year-count">({{ games_day | size }})</span>
                      </summary>

                      <div class="tl-day-list">
                        {% assign games_day_sorted = games_day | sort: "title" %}
                        {% for g in games_day_sorted %}
                          <div class="tl-game">
                            <div class="tl-game-title">
                              <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
                            </div>
                          </div>
                        {% endfor %}
                      </div>
                    </details>

                  {% endif %}
                {% endfor %}

                {%- comment -%} Unknown day for this month {%- endcomment -%}
                {% assign games_day_unknown = games_month | where_exp: "g", "g.day == nil" %}
                {% if games_day_unknown.size > 0 %}

                  <details class="tl-day tl-unknown">
                    <summary>
                      <span class="tl-day-label">Unknown day</span>
                      <span class="tl-year-count">({{ games_day_unknown | size }})</span>
                    </summary>

                    <div class="tl-day-list">
                      {% assign games_ud_sorted = games_day_unknown | sort: "title" %}
                      {% for g in games_ud_sorted %}
                        <div class="tl-game">
                          <div class="tl-game-title">
                            <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
                          </div>
                        </div>
                      {% endfor %}
                    </div>
                  </details>

                {% endif %}

              </div>
            </details>

          {% endif %}
        {% endfor %}

        {%- comment -%} Unknown month bucket {%- endcomment -%}
        {% assign games_month_unknown = games_year | where_exp: "g", "g.month == nil" %}
        {% if games_month_unknown.size > 0 %}

          <details class="tl-month tl-unknown">
            <summary>
              <span class="tl-month-label">Unknown month</span>
              <span class="tl-year-count">({{ games_month_unknown | size }})</span>
            </summary>

            <div class="tl-panel tl-subpanel">

              {%- comment -%} If any have a day even without month, group by day; else Unknown day {%- endcomment -%}
              {% for d in (1..31) %}
                {% assign games_day = games_month_unknown | where: "day", d %}
                {% if games_day.size > 0 %}

                  <details class="tl-day">
                    <summary>
                      <span class="tl-day-label">{{ d | prepend: "0" | slice: -2, 2 }}</span>
                      <span class="tl-year-count">({{ games_day | size }})</span>
                    </summary>

                    <div class="tl-day-list">
                      {% assign games_day_sorted = games_day | sort: "title" %}
                      {% for g in games_day_sorted %}
                        <div class="tl-game">
                          <div class="tl-game-title">
                            <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
                          </div>
                        </div>
                      {% endfor %}
                    </div>
                  </details>

                {% endif %}
              {% endfor %}

              {% assign games_day_unknown = games_month_unknown | where_exp: "g", "g.day == nil" %}
              {% if games_day_unknown.size > 0 %}

                <details class="tl-day tl-unknown">
                  <summary>
                    <span class="tl-day-label">Unknown day</span>
                    <span class="tl-year-count">({{ games_day_unknown | size }})</span>
                  </summary>

                  <div class="tl-day-list">
                    {% assign games_ud_sorted = games_day_unknown | sort: "title" %}
                    {% for g in games_ud_sorted %}
                      <div class="tl-game">
                        <div class="tl-game-title">
                          <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
                        </div>
                      </div>
                    {% endfor %}
                  </div>
                </details>

              {% endif %}

            </div>
          </details>

        {% endif %}

      </div>
    </details>
  </div>

{% endfor %}

</div>
