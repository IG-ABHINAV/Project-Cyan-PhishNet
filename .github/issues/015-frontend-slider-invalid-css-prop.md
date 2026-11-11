---
name: Bug Report
description: frontend Slider.jsx passes invalid css prop to native input element
title: "[Bug]: Slider.jsx passes non-standard css prop to input element"
labels: ["bug", "needs-triage", "frontend", "react"]
---

## Bug Description

`frontend/src/components/Slider.jsx:34-36` passes a `css` prop to a native `<input>` element:

```jsx
css={{
  '&::-webkit-slider-thumb': {},
}}
```

React does not support a `css` prop on DOM elements. This is a no-op that does nothing and could confuse developers attempting to style the slider thumb. The slider thumb styling is not applied as intended.

**Component:** Frontend (React Popup/Options)  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Open the options page or popup settings tab
2. Inspect the sensitivity slider
3. The `css` prop has no effect; slider thumb uses default browser styling

## Expected Behavior

Use inline `style` object or CSS classes for slider thumb styling.

## Actual Behavior

Invalid `css` prop is passed to input element; no thumb styling applied.

## Files Affected

- `frontend/src/components/Slider.jsx:34-36`
