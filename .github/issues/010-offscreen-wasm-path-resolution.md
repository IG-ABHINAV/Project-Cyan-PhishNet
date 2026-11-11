---
name: Bug Report
description: offscreen.js wasmPaths set via chrome.runtime.getURL may not resolve correctly in offscreen document context
title: "[Bug]: offscreen.js wasmPaths may not resolve from offscreen document context"
labels: ["bug", "needs-triage", "offscreen", "model"]
---

## Bug Description

`src/offscreen.js:22` sets `env.backends.onnx.wasm.wasmPaths = chrome.runtime.getURL('vendor/ort-wasm/')`. In the offscreen document context, `chrome.runtime.getURL` should resolve extension resources, but there are reports of WASM path resolution failures in offscreen documents across Chrome versions, which would prevent ONNX Runtime from loading the WASM worker and cause model initialization failure.

**Component:** Offscreen Document (Model inference)  
**Severity:** Medium  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Load extension in Chrome
2. Background creates offscreen document
3. Offscreen document loads Transformers.js and attempts to initialize ONNX Runtime
4. WASM paths fail to resolve, model loading fails

## Expected Behavior

WASM assets load correctly from the offscreen document context.

## Actual Behavior

Potential WASM path resolution failure in offscreen document, causing model load failure and degradation to heuristic-only mode.

## Files Affected

- `src/offscreen.js:22` — `wasmPaths` assignment
