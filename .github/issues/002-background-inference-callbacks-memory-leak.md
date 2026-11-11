---
name: Bug Report
description: pendingInferenceCallbacks Map can leak entries if inference never resolves and timeout does not fire
title: "[Bug]: pendingInferenceCallbacks memory leak on unresolved inference requests"
labels: ["bug", "needs-triage", "background", "memory-leak"]
---

## Bug Description

The `pendingInferenceCallbacks` Map in `src/background.js:20` stores resolve/reject callbacks keyed by `requestId`. Entries are removed on timeout (15s), resolve, or reject. However, if the offscreen document is suspended, the service worker goes idle, or the `RUN_INFERENCE` message is never delivered, entries can remain in the Map indefinitely with no cleanup mechanism beyond the 15-second timeout.

**Component:** Background Service Worker  
**Severity:** Medium  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Trigger a scan via `SCAN_EMAIL`
2. The offscreen document fails to process the request (e.g., model not loaded, offscreen document terminated)
3. The 15-second timeout should fire and clean up, but if the service worker is suspended before the timeout callback runs, the entry persists
4. Repeated failed scans accumulate entries in the Map

## Expected Behavior

Entries should be cleaned up via a maximum Map size, periodic cleanup, or when the service worker shuts down.

## Actual Behavior

Entries accumulate in `pendingInferenceCallbacks` with no upper bound beyond the timeout, causing a slow memory leak over extended browsing sessions.

## Files Affected

- `src/background.js:20` — `pendingInferenceCallbacks` Map declaration
- `src/background.js:272-306` — `runInference()` function
