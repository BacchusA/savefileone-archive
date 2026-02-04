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

<div class="tree-toolbar">
  <button id="treeExpandBtn" class="btn" type="button">Expand</button>
</div>

<div id="treeFrame" class="card tree-frame">
  <div class="card-text">
    <div id="treeMermaid" class="mermaid">
flowchart LR
{% assign all = site.games %}

%% Nodes
{% for g in all %}
  {{ g.year }}-{{ g.title | slugify }}["{{ g.title }} ({{ g.year }})"]
{% endfor %}

%% Clickable nodes (no tooltip text)
{% for g in all %}
  click {{ g.year }}-{{ g.title | slugify }} "{{ site.baseurl }}{{ g.url }}" _self
{% endfor %}

%% Edges (only when match exists)
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

<!-- Overlay -->
<div id="treeOverlay" class="tree-overlay" aria-hidden="true">
  <div class="tree-overlay__topbar">
    <div class="tree-overlay__title">Ancestry Tree</div>
    <div class="tree-overlay__actions">
      <button id="treeResetBtn" class="btn btn-ghost" type="button">Reset view</button>
      <button id="treeCloseBtn" class="btn" type="button">Close</button>
    </div>
  </div>

  <div id="treeOverlayViewport" class="tree-overlay__viewport">
    <div id="treeOverlayInner" class="tree-overlay__inner"></div>
  </div>
</div>

<script type="module">
  import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
  mermaid.initialize({ startOnLoad: true, theme: "dark", securityLevel: "loose" });

  const overlay = document.getElementById("treeOverlay");
  const expandBtn = document.getElementById("treeExpandBtn");
  const closeBtn  = document.getElementById("treeCloseBtn");
  const resetBtn  = document.getElementById("treeResetBtn");

  const frameMermaid  = document.getElementById("treeMermaid");
  const overlayInner  = document.getElementById("treeOverlayInner");
  const viewport      = document.getElementById("treeOverlayViewport");

  function openOverlay(){
    // Move the rendered SVG into the overlay for a true “big view”
    overlayInner.innerHTML = "";
    overlayInner.appendChild(frameMermaid);

    overlay.classList.add("is-open");
    overlay.setAttribute("aria-hidden", "false");
    document.body.classList.add("no-scroll");

    // Center-ish start
    requestAnimationFrame(() => {
      viewport.scrollLeft = Math.max(0, (overlayInner.scrollWidth - viewport.clientWidth) / 2);
      viewport.scrollTop  = Math.max(0, (overlayInner.scrollHeight - viewport.clientHeight) / 6);
    });
  }

  function closeOverlay(){
    // Put it back in the page card
    const originalHolder = document.querySelector("#treeFrame .card-text");
    originalHolder.appendChild(frameMermaid);
overlay.classList.remove("is-open");
    overlay.setAttribute("aria-hidden", "true");
    document.body.classList.remove("no-scroll");
  }
function resetView(){
    viewport.scrollLeft = 0;
    viewport.scrollTop = 0;
  }

  expandBtn?.addEventListener("click", openOverlay);
  closeBtn?.addEventListener("click", closeOverlay);
  resetBtn?.addEventListener("click", resetView);

overlay?.addEventListener("click", (e) => {
    // click the dark backdrop to close
    if (e.target === overlay) closeOverlay();
  });

  window.addEventListener("keydown", (e) => {
    if (e.key === "Escape" && overlay.classList.contains("is-open")) closeOverlay();
  });

  // Drag-to-pan in overlay
  let isDown = false, startX = 0, startY = 0, startL = 0, startT = 0;
  viewport.addEventListener("mousedown", (e) => {
    isDown = true;
    viewport.classList.add("dragging");
    startX = e.clientX; startY = e.clientY;
    startL = viewport.scrollLeft; startT = viewport.scrollTop;
  });
  window.addEventListener("mouseup", () => {
    isDown = false;
    viewport.classList.remove("dragging");
  });
  window.addEventListener("mousemove", (e) => {
    if (!isDown) return;
    const dx = e.clientX - startX;
    const dy = e.clientY - startY;
    viewport.scrollLeft = startL - dx;
    viewport.scrollTop  = startT - dy;
  });
</script>
