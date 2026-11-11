---
name: Bug Report
description: RELOAD_MODEL returns success before offscreen document is actually ready
title: "[Bug]: RELOAD_MODEL returns success before offscreen document re-initialization completes"
labels: ["bug", "needs-triage", "background"]
---

## Bug Description

The `RELOAD_MODEL` handler at `src/background.js:214-229` closes the existing offscreen document, recreates it, and immediately returns `{ success: true, status: modelStatus }` where `modelStatus` is still `'loading'`. The actual ready state only arrives later via the `OFFSCREEN_READY` message from the offscreen document. Callers that assume the model is ready immediately after `RELOAD_MODEL` will get incorrect status.

**Component:** Background Service Worker  
**Severity:** Medium  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Call `RELOAD_MODEL` via `chrome.runtime.sendMessage`
2. Check model status immediately after
3. Status is `'loading'` even though the function returned success

## Expected Behavior

Either wait for `OFFSCREEN_READY` before returning, or document that callers should poll for status.

## Actual Behavior

Returns immediately with `status: 'loading'`, misleading callers about model readiness.

## Files Affected

- `src/background.js:214-229` — `RELOAD_MODEL` handler
