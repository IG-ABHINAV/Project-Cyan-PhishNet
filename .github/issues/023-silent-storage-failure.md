---
name: Bug Report
description: content.js uses optional chaining on chrome.storage?.local which silently fails in incognito or restricted contexts
title: "[Bug]: Optional chaining on chrome.storage silently fails when storage unavailable"
labels: ["bug", "needs-triage", "content-script", "storage"]
---

## Bug Description

`src/content.js:264` uses `if (!chrome.storage?.local) return;` which silently returns if `chrome.storage` or `chrome.storage.local` is unavailable (e.g., incognito mode with storage restrictions, or enterprise policies disabling storage). Scan history and stats are silently lost with no indication to the user.

Similarly, `src/background.js:192` uses `if (chrome.storage?.local)` for `CLEAR_HISTORY` with the same silent-fail pattern.

**Component:** Content Script / Background  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Open extension in incognito mode or with storage disabled
2. Scan emails — no history is recorded
3. No error or indication that history is not being saved

## Expected Behavior

Log a warning or inform the user that storage is unavailable.

## Actual Behavior

Silent failure — no history recorded, no error shown.

## Files Affected

- `src/content.js:264` — `logScanRecord()` storage guard
- `src/background.js:192` — `CLEAR_HISTORY` storage guard
