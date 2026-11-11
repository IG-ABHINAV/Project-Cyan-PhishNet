---
name: Bug Report
description: content.js activeScans Set has no upper bound and could grow unbounded during high scan volume
title: "[Bug]: activeScans Set has no upper bound, potential memory growth"
labels: ["bug", "needs-triage", "content-script", "memory-leak"]
---

## Bug Description

`src/content.js:31` declares `const activeScans = new Set();` with no size limit. While `scannedEmailIds` is capped at 50 (lines 33, 112-114), `activeScans` has no cap. If many emails are being scanned simultaneously (e.g., a thread with dozens of messages opening at once), `activeScans` could grow without bound. Entries are removed in the `finally` block (line 254-256), but if `scanEmail` throws before reaching `finally`... actually `finally` always runs. The main concern is during normal operation with many concurrent scans.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** Low  
**Provider:** Both/All

## Steps to Reproduce

1. Open an email thread with many messages
2. Auto-scan triggers on each message
3. `activeScans` grows with each concurrent scan
4. Entries are cleaned up after each scan completes

## Expected Behavior

Cap `activeScans` size similar to `scannedEmailIds`.

## Actual Behavior

No upper bound on `activeScans`.

## Files Affected

- `src/content.js:31` — `activeScans` declaration
