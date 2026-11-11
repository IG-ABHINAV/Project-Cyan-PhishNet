---
name: Bug Report
description: Button.jsx onFocus handler references potentially undefined variant boxShadow
title: "[Bug]: Button.jsx onFocus boxShadow may be undefined for unknown variants"
labels: ["bug", "needs-triage", "frontend", "react"]
---

## Bug Description

`frontend/src/components/Button.jsx:101` sets focus box shadow using:

```js
e.currentTarget.style.boxShadow = `0 0 0 3px var(--pn-coral-pale), ${variants[variant].boxShadow || ''}`;
```

If an unknown `variant` is passed (not in the `variants` object), `variants[variant]` is `undefined`, and accessing `.boxShadow` throws a TypeError. The `|| ''` only applies to the property access result, not the object itself.

**Component:** Frontend (React Popup/Options)  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Render a Button with an unknown variant prop
2. Focus the button
3. TypeError: Cannot read properties of undefined (reading 'boxShadow')

## Expected Behavior

Safely handle unknown variants with a default boxShadow.

## Actual Behavior

Potential runtime TypeError on focus for unknown variants.

## Files Affected

- `frontend/src/components/Button.jsx:101`
