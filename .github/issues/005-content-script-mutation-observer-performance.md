---
name: Bug Report
description: Content script MutationObserver on subtree:true causes performance issues on Gmail/Outlook
title: "[Bug]: Content script MutationObserver subtree:true causes excessive DOM observation"
labels: ["bug", "needs-triage", "content-script", "performance"]
---

## Bug Description

`src/content.js:62-86` creates a `MutationObserver` observing `document.body || document.documentElement` with `childList: true, subtree: true`. Gmail and Outlook have highly dynamic DOMs with frequent mutations (auto-expanding threads, lazy-loading images, real-time updates). The observer fires on nearly every DOM change, and the debounced callback runs `document.querySelectorAll(selectors.messages)` on each tick. This is expensive on complex webmail interfaces.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** High  
**Provider:** Both/All

## Steps to Reproduce

1. Open Gmail or Outlook with the extension loaded
2. Open DevTools Performance panel
3. Observe frequent MutationObserver callbacks firing on scroll, message load, and UI updates

## Expected Behavior

Observe a narrower subtree (e.g., only the email list container) or use a more targeted observation strategy.

## Actual Behavior

Observes the entire document body subtree, causing frequent callbacks and expensive querySelectorAll operations.

## Files Affected

- `src/content.js:62-86` — `startEmailObserver()`
- `src/content.js:91-96` — `debouncedCheckForNewEmails()`
- `src/content.js:101-118` — `checkForNewEmails()`
