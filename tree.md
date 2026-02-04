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
  <div class="tree-zoom">
    <button id="treeZoomOutBtn" class="btn btn-ghost" type="button" aria-label="Zoom out">−</button>
    <div id="treeZoomLabel" class="tree-zoom__label">100%</div>
    <button id="treeZoomInBtn" class="btn btn-ghost" type="button" aria-label="Zoom in">+</button>
  </div>

  <button id="treeFitBtn" class="btn btn-ghost" type="button">Fit</button>
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

  const overlay   = document.getElementById("treeOverlay");
  const expandBtn = document.getElementById("treeExpandBtn");
  const closeBtn  = document.getElementById("treeCloseBtn");
  const resetBtn  = document.getElementById("treeResetBtn");
  const fitBtn    = document.getElementById("treeFitBtn");


  const frameMermaid = document.getElementById("treeMermaid");
  const overlayInner = document.getElementById("treeOverlayInner");
  const viewport     = document.getElementById("treeOverlayViewport");

  const zoomInBtn  = document.getElementById("treeZoomInBtn");
  const zoomOutBtn = document.getElementById("treeZoomOutBtn");
  const zoomLabel  = document.getElementById("treeZoomLabel");


  // ✅ PORTAL: move overlay to <body> so it can't be constrained by layout
  if (overlay && overlay.parentElement !== document.body) {
    document.body.appendChild(overlay);
  }

  function openOverlay(){
    overlayInner.innerHTML = "";
    overlayInner.appendChild(frameMermaid);

    overlay.classList.add("is-open");
    overlay.setAttribute("aria-hidden", "false");
    document.body.classList.add("no-scroll");
    applyZoom();
   setTimeout(() => fitToScreen(), 120);




    requestAnimationFrame(() => {
      viewport.scrollLeft = Math.max(0, (overlayInner.scrollWidth - viewport.clientWidth) / 2);
      viewport.scrollTop  = Math.max(0, (overlayInner.scrollHeight - viewport.clientHeight) / 6);
    });
  }

  function closeOverlay(){
    const originalHolder = document.querySelector("#treeFrame .card-text");
    originalHolder.appendChild(frameMermaid);

    overlay.classList.remove("is-open");
    overlay.setAttribute("aria-hidden", "true");
    document.body.classList.remove("no-scroll");
  }

  function resetView(){
  viewport.scrollLeft = 0;
  viewport.scrollTop  = 0;
  zoom = 1;
  applyZoom();
}

  // ===== Zoom (transform on overlayInner) =====
let zoom = 1;
const ZOOM_MIN = 0.25;
const ZOOM_MAX = 2.5;
const ZOOM_STEP_BTN = 0.1;
const ZOOM_STEP_WHEEL = 0.08;

function clamp(v, min, max){ return Math.min(max, Math.max(min, v)); }

function applyZoom(){
  // scale from top-left for predictable scrolling
  overlayInner.style.transformOrigin = "0 0";
  overlayInner.style.transform = `scale(${zoom})`;

  if (zoomLabel) zoomLabel.textContent = `${Math.round(zoom * 100)}%`;
}

// Keep the point under the mouse “stable” while zooming
function zoomAtPoint(newZoom, clientX, clientY){
  newZoom = clamp(newZoom, ZOOM_MIN, ZOOM_MAX);

  const rect = viewport.getBoundingClientRect();
  const x = clientX - rect.left + viewport.scrollLeft;
  const y = clientY - rect.top  + viewport.scrollTop;

  const scale = newZoom / zoom;

  // Adjust scroll so the zoom focuses around the cursor point
  viewport.scrollLeft = (x * scale) - (clientX - rect.left);
  viewport.scrollTop  = (y * scale) - (clientY - rect.top);

  zoom = newZoom;
  applyZoom();
}


  expandBtn?.addEventListener("click", openOverlay);
  closeBtn?.addEventListener("click", closeOverlay);
  resetBtn?.addEventListener("click", resetView);
  fitBtn?.addEventListener("click", fitToScreen);


  zoomInBtn?.addEventListener("click", () => {
  const r = viewport.getBoundingClientRect();
  zoomAtPoint(zoom + ZOOM_STEP_BTN, r.left + r.width/2, r.top + r.height/2);
});

zoomOutBtn?.addEventListener("click", () => {
  const r = viewport.getBoundingClientRect();
  zoomAtPoint(zoom - ZOOM_STEP_BTN, r.left + r.width/2, r.top + r.height/2);
});


  overlay?.addEventListener("click", (e) => {
    if (e.target === overlay) closeOverlay();
  });

  window.addEventListener("keydown", (e) => {
    if (e.key === "Escape" && overlay.classList.contains("is-open")) closeOverlay();
  });

  // Drag-to-pan (optional)
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

  viewport.addEventListener("wheel", (e) => {
  // Require CTRL so normal scrolling still works
  if (!e.ctrlKey) return;

  e.preventDefault();

  const direction = Math.sign(e.deltaY);
  const next = zoom * (direction > 0 ? (1 - ZOOM_STEP_WHEEL) : (1 + ZOOM_STEP_WHEEL));
  zoomAtPoint(next, e.clientX, e.clientY);
}, { passive: false });
  function fitToScreen(){
  // Find the rendered Mermaid SVG
  const svg = overlayInner.querySelector("svg");
  if (!svg) return;

  // Some Mermaid SVGs have padding; bbox gives actual drawn bounds
  const bbox = svg.getBBox();

  // Visible area inside the scroll viewport
  const padding = 40; // breathing room around the graph
  const viewW = viewport.clientWidth - padding;
  const viewH = viewport.clientHeight - padding;

  if (viewW <= 0 || viewH <= 0) return;

  // Required zoom so bbox fits inside viewport
  const scaleX = viewW / bbox.width;
  const scaleY = viewH / bbox.height;

  // Choose the smaller scale so both dimensions fit
  let newZoom = Math.min(scaleX, scaleY);

  // Clamp to your zoom bounds
  newZoom = clamp(newZoom, ZOOM_MIN, ZOOM_MAX);

  // Apply zoom
  zoom = newZoom;
  applyZoom();

  // After zoom, center the bbox inside the viewport
  // bbox.x/y are in SVG units; multiplied by zoom gives scaled content size
  const targetX = (bbox.x * zoom) - (padding / 2);
  const targetY = (bbox.y * zoom) - (padding / 2);

  const centerX = targetX + (bbox.width * zoom) / 2;
  const centerY = targetY + (bbox.height * zoom) / 2;

  viewport.scrollLeft = Math.max(0, centerX - viewport.clientWidth / 2);
  viewport.scrollTop  = Math.max(0, centerY - viewport.clientHeight / 2);
}


</script>

