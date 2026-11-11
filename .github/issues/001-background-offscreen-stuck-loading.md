---
name: Bug Report
description: Offscreen document creation failure does not update modelStatus, leaving extension stuck in loading state
title: "[Bug]: Offscreen document single-instance error leaves modelStatus stuck at loading"
labels: ["bug", "needs-triage", "background"]
---

## Bug Description

When `chrome.offscreen.createDocument` fails with "Only a single offscreen document may be created", the catch block at `src/background.js:108` sets `offscreenDocumentCreated = true` and returns, but **never updates `modelStatus` from `'loading'`** or broadcasts a ready status. The extension remains stuck in the loading state indefinitely, and the popup status pill shows "Loading AI..." forever even though the offscreen document already exists from a previous session.

**Component:** Background Service Worker  
**Severity:** High  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Install extension, offscreen document is created successfully on first run
2. Background service worker restarts (browser restart or extension reload)
3. `createOffscreenDocument()` is called again
4. `chrome.offscreen.hasDocument()` returns true, so it skips creation — but wait, this path returns early correctly
5. The actual failure path: `createDocument` throws "Only a single offscreen document may be created"
6. Catch block sets `offscreenDocumentCreated = true` but `modelStatus` remains `'loading'`

## Expected Behavior

`modelStatus` should transition to `'ready'` and `broadcastModelStatus()` should be called when the offscreen document already exists.

## Actual Behavior

`modelStatus` stays at `'loading'`, popup shows "Loading AI..." indefinitely, scans return `useHeuristic: true` because `modelStatus !== 'ready'`.

## Console Logs / Errors

No error logged — the failure is silently swallowed at `src/background.js:108-111`.

## Files Affected

- `src/background.js:108-111` — catch block for single offscreen document error
