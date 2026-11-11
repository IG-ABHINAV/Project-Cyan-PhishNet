---
name: Bug Report
description: background.js initOnce called from onInstalled, onStartup, and top-level — potential redundant initialization
title: "[Bug]: Multiple init triggers can cause redundant background initialization"
labels: ["bug", "needs-triage", "background", "lifecycle"]
---

## Bug Description

`src/background.js` initializes via three paths:
1. Top-level call at line 367: `initOnce()`
2. `chrome.runtime.onInstalled.addListener` at line 341 → `initOnce()`
3. `chrome.runtime.onStartup.addListener` at line 349 → `initOnce()`

While `initOnce()` uses a promise guard (`initPromise`) to prevent double initialization, there's a subtle race: if `init()` fails and sets `initPromise = null` (line 360), a subsequent `onInstalled` or `onStartup` event could trigger a retry. This is generally desirable, but if multiple events fire in quick succession during browser startup, the guard may not prevent all redundant work.

Additionally, `init()` calls `createOffscreenDocument()` and `broadcastModelStatus()` unconditionally. If the service worker is restarted while an offscreen document already exists from a previous session, the `hasDocument()` check should catch this, but there's a window where the offscreen document from the previous session might be in the process of closing.

**Component:** Background Service Worker  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Start browser with extension installed
2. `onStartup` fires → `initOnce()` → `init()`
3. `onInstalled` may also fire → `initOnce()` → guarded by initPromise, returns existing promise

## Expected Behavior

Single, well-defined initialization path with clear lifecycle management.

## Actual Behavior

Three potential entry points for initialization, guarded but not perfectly coordinated.

## Files Affected

- `src/background.js:341-364` — lifecycle listeners and initOnce guard
