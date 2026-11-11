---
name: Bug Report
description: offscreen.js warmup inference uses hardcoded test string that doesn't exercise real code paths
title: "[Bug]: offscreen.js warmup uses unrealistic test string"
labels: ["bug", "needs-triage", "offscreen", "model"]
---

## Bug Description

`src/offscreen.js:74` warms up the model with:

```js
await classifier('Test email for warmup');
```

This short, benign string doesn't exercise the full inference pipeline (tokenization with longer text, multi-class probability distribution, etc.). A more realistic warmup would better initialize the ONNX Runtime and reveal model loading issues earlier.

**Component:** Offscreen Document (Model inference)  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Extension loads model
2. Warmup runs with 'Test email for warmup'
3. Model marked ready after warmup

## Expected Behavior

Use a more representative warmup string or skip warmup and mark ready after successful pipeline load.

## Actual Behavior

Minimal warmup that doesn't fully exercise the inference pipeline.

## Files Affected

- `src/offscreen.js:74`
