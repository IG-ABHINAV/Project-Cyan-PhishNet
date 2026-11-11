---
name: Bug Report
description: Duplicate popup implementations exist: src/popup.html (vanilla JS) and frontend/popup.html (React)
title: "[Bug]: Two conflicting popup implementations with no clear single source of truth"
labels: ["bug", "needs-triage", "popup", "architecture"]
---

## Bug Description

The repository contains two completely different popup implementations:
1. `src/popup.html` + `src/popup.js` — vanilla JavaScript popup with inline CSS
2. `frontend/popup.html` + `frontend/src/popup/main.jsx` — React-based popup built by Vite into `dist/popup.html`

The manifest (`manifest.json:13`) points `"default_popup"` to `dist/popup.html` (the React build output). However, `src/popup.html` exists as dead code that is never referenced but duplicates functionality. The React popup requires a build step (`npm run build`) before it works, while the vanilla popup works without building. This creates confusion about which popup is active and can lead to developers editing the wrong file.

**Component:** Popup UI  
**Severity:** High  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Clone repository
2. Open `src/popup.html` and make changes expecting them to appear in the extension
3. Load extension — changes don't appear because manifest points to `dist/popup.html`
4. Conversely, edit `frontend/src/popup/main.jsx` without building — changes don't appear because dist is stale

## Expected Behavior

Single popup implementation. Either remove the vanilla `src/popup.html`/`src/popup.js` or make the manifest point to it. Document which is the source of truth.

## Actual Behavior

Two divergent popup implementations; only the built React version is used by the manifest, but the vanilla version remains in the source tree.

## Files Affected

- `manifest.json:13` — `default_popup` points to `dist/popup.html`
- `src/popup.html` — vanilla popup (dead code)
- `src/popup.js` — vanilla popup script (dead code)
- `frontend/popup.html` — React popup source
- `frontend/src/popup/main.jsx` — React popup component
