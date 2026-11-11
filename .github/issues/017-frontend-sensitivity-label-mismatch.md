---
name: Bug Report
description: getSensitivityLabel falls back to 'Standard' for slider values that don't match presets exactly
title: "[Bug]: Sensitivity label falls back to Standard for non-preset slider values"
labels: ["bug", "needs-triage", "frontend", "ux"]
---

## Bug Description

`frontend/src/lib/constants.js:77-80` implements `getSensitivityLabel()` which looks for an exact match in `SENSITIVITY_LABELS`:

```js
export function getSensitivityLabel(value) {
  const pct = Math.round(value * 100);
  return SENSITIVITY_LABELS.find((s) => s.value === pct) || SENSITIVITY_LABELS[2];
}
```

The slider allows values 50-95 in steps of 5 (50, 55, 60, ..., 95). Only 50, 60, 70, 80, 90, 95 have preset labels. Values 55, 65, 75, 85 fall back to `SENSITIVITY_LABELS[2]` which is 'Standard' (70), even though the actual value differs. The displayed label doesn't match the actual sensitivity.

**Component:** Frontend (React Popup/Options)  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Set sensitivity slider to 65%
2. Label shows 'Standard' instead of an appropriate label for 65%

## Expected Behavior

Interpolate between labels or show the exact percentage without a mismatched label.

## Actual Behavior

Non-preset values show 'Standard' label regardless of actual value.

## Files Affected

- `frontend/src/lib/constants.js:77-80`
