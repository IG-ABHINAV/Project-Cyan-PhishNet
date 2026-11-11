---
name: Bug Report
description: frontend main.jsx uses typeof check for chrome/browser that could be undefined outside extension context
title: "[Bug]: frontend main.jsx api variable may be undefined outside extension"
labels: ["bug", "needs-triage", "frontend", "react"]
---

## Bug Description

`frontend/src/popup/main.jsx:88-90`:

```js
const api = typeof chrome !== 'undefined' ? chrome : browser;
if (api?.runtime?.openOptionsPage) api.runtime.openOptionsPage();
```

If neither `chrome` nor `browser` is defined (running outside an extension context), `api` is `undefined`, and the optional chaining `api?.runtime?.openOptionsPage` safely returns `undefined`. This is actually safe, but the pattern of falling back to a potentially undefined `browser` variable without checking `typeof browser` first could throw a ReferenceError in strict environments if `browser` is not declared at all.

**Component:** Frontend (React Popup/Options)  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Open popup HTML outside of extension context
2. `typeof chrome` is 'undefined', falls back to `browser`
3. If `browser` is not declared, ReferenceError

## Expected Behavior

Check `typeof browser !== 'undefined'` before using it.

## Actual Behavior

Potential ReferenceError if `browser` is undeclared.

## Files Affected

- `frontend/src/popup/main.jsx:88`
- `frontend/src/lib/messaging.js:8` — same pattern
