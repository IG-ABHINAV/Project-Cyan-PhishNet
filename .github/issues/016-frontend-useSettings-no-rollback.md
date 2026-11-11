---
name: Bug Report
description: useSettings hook applies optimistic local state update without rollback on failure
title: "[Bug]: useSettings optimistic update has no rollback on background update failure"
labels: ["bug", "needs-triage", "frontend", "settings"]
---

## Bug Description

`frontend/src/hooks/useSettings.js:30-36` updates local React state immediately and calls `updateSettings()` in the background, but does not await the result or roll back if the background update fails:

```js
const update = useCallback(async (key, value) => {
  setSettings((prev) => {
    const next = { ...prev, [key]: value };
    updateSettings(next);  // not awaited, errors swallowed
    return next;
  });
}, []);
```

If `updateSettings` fails (e.g., background service worker unavailable), the UI shows the new value but the background was not updated. On next settings load, the value reverts, causing a confusing flicker.

**Component:** Frontend (React Popup/Options)  
**Severity:** Medium  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Change a setting in the popup or options page
2. Background service worker is temporarily unavailable
3. Local state updates, background does not
4. Reload popup — setting reverts to old value

## Expected Behavior

Await `updateSettings()` result and roll back local state on failure.

## Actual Behavior

Optimistic update with no rollback.

## Files Affected

- `frontend/src/hooks/useSettings.js:30-36`
