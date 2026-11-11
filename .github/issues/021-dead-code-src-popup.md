---
name: Bug Report
description: src/popup.html and src/popup.js are dead code not referenced by manifest or any build process
title: "[Bug]: src/popup.html and src/popup.js are orphaned dead code"
labels: ["bug", "needs-triage", "popup", "dead-code"]
---

## Bug Description

`src/popup.html` and `src/popup.js` exist in the source tree but are not referenced by `manifest.json` (which points to `dist/popup.html`), nor by any build process, nor imported by any other file. They are complete dead code that duplicates the React popup implementation. This creates maintenance confusion — developers may edit these files expecting changes to take effect, but they never will.

**Component:** Popup UI  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Edit `src/popup.html` or `src/popup.js`
2. Load extension — no changes appear
3. Reason: manifest references `dist/popup.html` (React build), not `src/popup.html`

## Expected Behavior

Either remove the dead code or make it the active popup (update manifest and remove React implementation).

## Actual Behavior

Orphaned files that look functional but are never used.

## Files Affected

- `src/popup.html`
- `src/popup.js`
