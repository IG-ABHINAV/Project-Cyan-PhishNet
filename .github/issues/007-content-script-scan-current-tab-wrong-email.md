---
name: Bug Report
description: SCAN_CURRENT_EMAIL handler selects the last email element which may not be the open email
title: "[Bug]: SCAN_CURRENT_EMAIL scans the last email element, not the currently open email"
labels: ["bug", "needs-triage", "content-script"]
---

## Bug Description

`src/content.js:782-800` handles `SCAN_CURRENT_EMAIL` by selecting `messageElements[messageElements.length - 1]` — the last email element in the DOM. This is not necessarily the email the user has open/selected. In Gmail's thread view, the last message in the DOM may be an older message in the thread, not the currently viewed one.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** Medium  
**Provider:** Gmail, Outlook

## Steps to Reproduce

1. Open Gmail with a thread containing multiple emails
2. Open a specific email in the thread (not the last one in DOM order)
3. Click "Scan Current Email" in the popup
4. The scan analyzes the last DOM element instead of the open email

## Expected Behavior

Scan the currently focused/visible email element, not the last one in the DOM.

## Actual Behavior

Scans `messageElements[messageElements.length - 1]` which may be the wrong email.

## Files Affected

- `src/content.js:782-800` — `SCAN_CURRENT_EMAIL` handler
