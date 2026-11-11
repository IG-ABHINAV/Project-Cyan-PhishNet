---
name: Bug Report
description: popup.js setInterval refreshStatus is never explicitly cleared
title: "[Bug]: popup.js status refresh interval not explicitly cleared"
labels: ["bug", "needs-triage", "popup", "memory-leak"]
---

## Bug Description

`src/popup.js:75` sets up a polling interval:

```js
setInterval(refreshStatus, 4000);
```

The interval ID is never stored or cleared. When the popup is closed, the entire page is destroyed, which clears the interval automatically. However, it's not explicitly cleared, which is poor practice and could be an issue if the popup is ever converted to a persistent background page or if the pattern is copied elsewhere.

**Component:** Popup UI  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Open popup
2. `setInterval(refreshStatus, 4000)` starts polling every 4 seconds
3. Close popup — interval is cleaned up with page destruction

## Expected Behavior

Store interval ID and clear it in a cleanup handler.

## Actual Behavior

Interval relies on page destruction for cleanup.

## Files Affected

- `src/popup.js:75`
