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

/* ===== Tree overlay / expanded view ===== */
.no-scroll{ overflow: hidden; }

.tree-toolbar{
  display:flex;
  justify-content:flex-end;
  margin: 12px 0;
}

.btn-ghost{
  background: rgba(0,0,0,0.22);
  border: 1px solid rgba(255,255,255,0.14);
}

.tree-overlay{
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.70);
  backdrop-filter: blur(6px);
  opacity: 0;
  pointer-events: none;
  transition: opacity 140ms ease;
  z-index: 9999;
}
.tree-overlay.is-open{
  opacity: 1;
  pointer-events: auto;
}

.tree-overlay__topbar{
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap: 12px;
  padding: 14px 16px;
  border-bottom: 1px solid rgba(255,255,255,0.14);
  background: linear-gradient(180deg, rgba(255,255,255,0.06), rgba(0,0,0,0.38));
}
.tree-overlay__title{
  font-weight: 800;
  letter-spacing: 0.4px;
  color: rgba(255,255,255,0.92);
}
.tree-overlay__actions{
  display:flex;
  gap: 10px;
  flex-wrap: wrap;
}

.tree-overlay__viewport{
  position: absolute;
  inset: 56px 12px 12px 12px; /* below topbar */
  border-radius: 18px;
  border: 1px solid rgba(255,255,255,0.16);
  background: rgba(0,0,0,0.55);
  overflow: auto;
  cursor: grab;
}
.tree-overlay__viewport.dragging{ cursor: grabbing; }

.tree-overlay__inner{
  min-width: 1400px; /* forces horizontal scrolling even for small trees */
  min-height: 900px;
  padding: 18px;
}

/* Make Mermaid svg scale up while keeping crisp lines */
.tree-overlay__inner svg{
  width: max-content;
  height: auto;
}

