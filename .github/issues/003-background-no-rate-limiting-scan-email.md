---
name: Bug Report
description: No rate limiting on SCAN_EMAIL allows content script to spam background with scan requests
title: "[Bug]: No rate limiting on SCAN_EMAIL message handler"
labels: ["bug", "needs-triage", "background", "dos"]
---

## Bug Description

The `SCAN_EMAIL` handler in `src/background.js:144-156` has no rate limiting or deduplication. A content script (or any extension message sender) can send rapid `SCAN_EMAIL` requests, each triggering a full model inference via the offscreen document. This could cause excessive CPU usage, model thrashing, and degraded browser performance.

**Component:** Background Service Worker  
**Severity:** Medium  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Open an email in Gmail/Outlook
2. Rapidly trigger scans (e.g., via console or automated script)
3. Each scan triggers `runInference()` → offscreen document model inference

## Expected Behavior

Rate-limit or deduplicate scan requests (e.g., max 1 scan per email per 5 seconds, or queue requests).

## Actual Behavior

Unrestricted scan requests can flood the inference pipeline.

## Files Affected

- `src/background.js:144-156` — `SCAN_EMAIL` handler
