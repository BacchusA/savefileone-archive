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
        - Prefer explicit month field if present.
        - Otherwise derive month from release_date if present.
        {%- endcomment -%}

        {% for month_name in months %}

          {% assign games_month = "" | split: "" %}

          {% for g in games_year %}
            {% assign derived_month = "" %}
            {% if g.release_date %}
              {% assign derived_month = g.release_date | date: "%B" %}
            {% endif %}

            {% if g.month == month_name or derived_month == month_name %}
              {% assign games_month = games_month | push: g %}
            {% endif %}
          {% endfor %}

          {% if games_month.size > 0 %}
          <details class="tl-month">
            <summary>
              <span class="tl-month-label">{{ month_name }}</span>
              <span class="tl-year-count">({{ games_month | size }})</span>
            </summary>

            <div class="tl-panel tl-subpanel">

              {%- comment -%} KNOWN DAYS (1..31), explicit day OR derived day {%- endcomment -%}
              {% for d in (1..31) %}

                {% assign games_day = "" | split: "" %}

                {% for g in games_month %}
                  {% assign derived_day = "" %}
                  {% if g.release_date %}
                    {% assign derived_day = g.release_date | date: "%-d" %}
                  {% endif %}

                  {% if g.day == d or derived_day == d %}
                    {% assign games_day = games_day | push: g %}
                  {% endif %}
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

              {%- comment -%} Unknown day inside a known month (month known, but no day and no release_date) {%- endcomment -%}
              {% assign games_day_unknown = "" | split: "" %}
              {% for g in games_month %}
                {% if g.day == nil and g.release_date == nil %}
                  {% assign games_day_unknown = games_day_unknown | push: g %}
                {% endif %}
              {% endfor %}

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

        {% assign games_month_unknown = "" | split: "" %}
        {% for g in games_year %}
          {% if g.month == nil and g.release_date == nil %}
            {% assign games_month_unknown = games_month_unknown | push: g %}
          {% endif %}
        {% endfor %}

        {% if games_month_unknown.size > 0 %}

        <details class="tl-month tl-unknown">
          <summary>
            <span class="tl-month-label">Unknown month</span>
            <span class="tl-year-count">({{ games_month_unknown | size }})</span>
          </summary>

          <div class="tl-panel tl-subpanel">

            {%- comment -%} Group by explicit day if present (rare, but supported) {%- endcomment -%}
            {% for d in (1..31) %}

              {% assign games_day = "" | split: "" %}
              {% for g in games_month_unknown %}
                {% if g.day == d %}
                  {% assign games_day = games_day | push: g %}
                {% endif %}
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
            {% assign games_day_unknown2 = "" | split: "" %}
            {% for g in games_month_unknown %}
              {% if g.day == nil %}
                {% assign games_day_unknown2 = games_day_unknown2 | push: g %}
              {% endif %}
            {% endfor %}

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
