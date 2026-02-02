---
layout: default
title: Timeline
permalink: /timeline/
---

# Timeline

<p class="small">Click a year → month → day. If month/day are missing, the game goes into Unknown buckets automatically.</p>

<div class="timeline">

{% assign months = "January|February|March|April|May|June|July|August|September|October|November|December" | split: "|" %}
{% assign years_sorted = site.games | map: "year" | uniq | sort %}

{% for y in years_sorted %}
  {% assign games_year = site.games | where: "year", y %}

  <div class="tl-item">

    <details class="tl-year">
      <summary>
        <span class="tl-year-label">{{ y }}</span>
        <span class="tl-year-count">({{ games_year | size }})</span>
      </summary>

      <div class="tl-panel tl-nested">

        {%- comment -%} Known months by NAME {%- endcomment -%}
        {% for month_name in months %}
          {% assign games_month = games_year | where: "month", month_name %}
          {% if games_month.size > 0 %}

          <details class="tl-month">
            <summary>
              <span class="tl-month-label">{{ month_name }}</span>
              <span class="tl-year-count">({{ games_month | size }})</span>
            </summary>

            <div class="tl-panel tl-subpanel">

              {%- comment -%} Known days {%- endcomment -%}
              {% for d in (1..31) %}
                {% assign games_day = games_month | where: "day", d %}
                {% if games_day.size > 0 %}

                <details class="tl-day">
                  <summary>
                    <span class="tl-day-label">{{ d }}</span>
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

              {%- comment -%} Unknown day for this known month {%- endcomment -%}
              {% assign games_day_unknown = games_month | where_exp: "g", "g.day == nil" %}
              {% if games_day_unknown.size > 0 %}

              <details class="tl-day tl-unknown">
                <summary>
                  <span class="tl-day-label">Unknown day</span>
                  <span class="tl-year-count">({{ games_day_unknown | size }})</span>
                </summary>

                <div class="tl-day-list">
                  {% assign ud_sorted = games_day_unknown | sort: "title" %}
                  {% for g in ud_sorted %}
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

        {%- comment -%} Unknown month bucket (month missing entirely) {%- endcomment -%}
        {% assign games_month_unknown = games_year | where_exp: "g", "g.month == nil" %}
        {% if games_month_unknown.size > 0 %}

        <details class="tl-month tl-unknown">
          <summary>
            <span class="tl-month-label">Unknown month</span>
            <span class="tl-year-count">({{ games_month_unknown | size }})</span>
          </summary>

          <div class="tl-panel tl-subpanel">

            {%- comment -%} Group by day if present {%- endcomment -%}
            {% for d in (1..31) %}
              {% assign games_day = games_month_unknown | where: "day", d %}
              {% if games_day.size > 0 %}

              <details class="tl-day">
                <summary>
                  <span class="tl-day-label">{{ d }}</span>
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

            {%- comment -%} Unknown day inside unknown month {%- endcomment -%}
            {% assign games_day_unknown = games_month_unknown | where_exp: "g", "g.day == nil" %}
            {% if games_day_unknown.size > 0 %}

            <details class="tl-day tl-unknown">
              <summary>
                <span class="tl-day-label">Unknown day</span>
                <span class="tl-year-count">({{ games_day_unknown | size }})</span>
              </summary>

              <div class="tl-day-list">
                {% assign ud_sorted2 = games_day_unknown | sort: "title" %}
                {% for g in ud_sorted2 %}
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
