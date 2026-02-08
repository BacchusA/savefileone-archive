---
layout: default
title: Timeline
permalink: /timeline/
---

# Timeline

<p class="small">
Browse by release date when known. If month/day are missing (or no release_date), the game goes into Unknown buckets automatically.
</p>

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

        {%- comment -%}
        KNOWN MONTHS:
        - Prefer explicit month field if you still use it.
        - Otherwise derive month from release_date if present.
        {%- endcomment -%}

        {% for month_name in months %}
          {% assign games_month_explicit = games_year | where: "month", month_name %}

          {% assign games_month_from_date = games_year | where_exp: "g",
            "g.release_date != nil and (g.release_date | date: '%B') == month_name" %}

          {%- comment -%}
          Merge the two lists without duplicates:
          We'll start with explicit, then append date-derived that aren't already in explicit.
          {%- endcomment -%}

          {% assign games_month = games_month_explicit %}
          {% for g2 in games_month_from_date %}
            {% unless games_month_explicit contains g2 %}
              {% assign games_month = games_month | push: g2 %}
            {% endunless %}
          {% endfor %}

          {% if games_month.size > 0 %}
          <details class="tl-month">
            <summary>
              <span class="tl-month-label">{{ month_name }}</span>
              <span class="tl-year-count">({{ games_month | size }})</span>
            </summary>

            <div class="tl-panel tl-subpanel">

              {%- comment -%} KNOWN DAYS (1..31), explicit day OR derived from release_date {%- endcomment -%}
              {% for d in (1..31) %}

                {% assign games_day_explicit = games_month | where: "day", d %}

                {% assign games_day_from_date = games_month | where_exp: "g",
                  "g.release_date != nil and (g.release_date | date: '%-d') == d" %}

                {% assign games_day = games_day_explicit %}
                {% for g3 in games_day_from_date %}
                  {% unless games_day_explicit contains g3 %}
                    {% assign games_day = games_day | push: g3 %}
                  {% endunless %}
                {% endfor %}

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

                        <div class="tl-game-meta small">
                          {% if g.platform %}{{ g.platform }}{% endif %}
                          {% if g.release_date %}<span class="muted"> · {{ g.release_date }}</span>{% endif %}
                        </div>

                        {% if g.youtube_longplay and g.youtube_longplay != "" %}
                          <div class="tl-game-actions">
                            <a class="btn btn-small" href="{{ g.youtube_longplay }}" target="_blank" rel="noopener">
                              Watch Longplay
                            </a>
                          </div>
                        {% endif %}
                      </div>
                    {% endfor %}
                  </div>
                </details>
                {% endif %}

              {% endfor %}

              {%- comment -%}
              UNKNOWN DAY inside a KNOWN MONTH:
              Explicit month exists OR release_date month matches, but day is missing.
              {%- endcomment -%}

              {% assign games_day_unknown = games_month | where_exp: "g",
                "g.day == nil and (g.release_date == nil or (g.release_date | date: '%-d') == '')" %}

              {%- comment -%}
              The check above is intentionally gentle; Liquid date formatting on nil won't run anyway.
              If release_date exists, day will always exist, so it won't end up here.
              {%- endcomment -%}

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

                      <div class="tl-game-meta small">
                        {% if g.platform %}{{ g.platform }}{% endif %}
                      </div>

                      {% if g.youtube_longplay and g.youtube_longplay != "" %}
                        <div class="tl-game-actions">
                          <a class="btn btn-small" href="{{ g.youtube_longplay }}" target="_blank" rel="noopener">
                            Watch Longplay
                          </a>
                        </div>
                      {% endif %}
                    </div>
                  {% endfor %}
                </div>
              </details>
              {% endif %}

            </div>
          </details>
          {% endif %}
        {% endfor %}

        {%- comment -%}
        UNKNOWN MONTH bucket:
        - No explicit month
        - AND no release_date
        {%- endcomment -%}

        {% assign games_month_unknown = games_year | where_exp: "g", "g.month == nil and g.release_date == nil" %}
        {% if games_month_unknown.size > 0 %}

        <details class="tl-month tl-unknown">
          <summary>
            <span class="tl-month-label">Unknown month</span>
            <span class="tl-year-count">({{ games_month_unknown | size }})</span>
          </summary>

          <div class="tl-panel tl-subpanel">

            {%- comment -%} Group by explicit day if present (rare, but supported) {%- endcomment -%}
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

                      <div class="tl-game-meta small">
                        {% if g.platform %}{{ g.platform }}{% endif %}
                      </div>

                      {% if g.youtube_longplay and g.youtube_longplay != "" %}
                        <div class="tl-game-actions">
                          <a class="btn btn-small" href="{{ g.youtube_longplay }}" target="_blank" rel="noopener">
                            Watch Longplay
                          </a>
                        </div>
                      {% endif %}
                    </div>
                  {% endfor %}
                </div>
              </details>

              {% endif %}
            {% endfor %}

            {%- comment -%} Unknown day inside unknown month {%- endcomment -%}
            {% assign games_day_unknown2 = games_month_unknown | where_exp: "g", "g.day == nil" %}
            {% if games_day_unknown2.size > 0 %}

            <details class="tl-day tl-unknown">
              <summary>
                <span class="tl-day-label">Unknown day</span>
                <span class="tl-year-count">({{ games_day_unknown2 | size }})</span>
              </summary>

              <div class="tl-day-list">
                {% assign ud_sorted2 = games_day_unknown2 | sort: "title" %}
                {% for g in ud_sorted2 %}
                  <div class="tl-game">
                    <div class="tl-game-title">
                      <a href="{{ site.baseurl }}{{ g.url }}">{{ g.title }}</a>
                    </div>

                    <div class="tl-game-meta small">
                      {% if g.platform %}{{ g.platform }}{% endif %}
                    </div>

                    {% if g.youtube_longplay and g.youtube_longplay != "" %}
                      <div class="tl-game-actions">
                        <a class="btn btn-small" href="{{ g.youtube_longplay }}" target="_blank" rel="noopener">
                          Watch Longplay
                        </a>
                      </div>
                    {% endif %}
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
