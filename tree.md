---
layout: default
title: Ancestry Tree
permalink: /tree/
---

const overlay = document.getElementById("treeOverlay");
const expandBtn = document.getElementById("treeExpandBtn");
const closeBtn  = document.getElementById("treeCloseBtn");
const resetBtn  = document.getElementById("treeResetBtn");

const frameMermaid  = document.getElementById("treeMermaid");
const overlayInner  = document.getElementById("treeOverlayInner");
const viewport      = document.getElementById("treeOverlayViewport");

// ✅ PORTAL: remove overlay from constrained layout and attach to body
document.body.appendChild(overlay);

function openOverlay(){
  overlayInner.innerHTML = "";
  overlayInner.appendChild(frameMermaid);

  overlay.classList.add("is-open");
  overlay.setAttribute("aria-hidden", "false");
  document.body.classList.add("no-scroll");

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
}

expandBtn?.addEventListener("click", openOverlay);
closeBtn?.addEventListener("click", closeOverlay);
resetBtn?.addEventListener("click", resetView);

overlay?.addEventListener("click", (e) => {
  if (e.target === overlay) closeOverlay();
});

window.addEventListener("keydown", (e) => {
  if (e.key === "Escape" && overlay.classList.contains("is-open")) closeOverlay();
});
