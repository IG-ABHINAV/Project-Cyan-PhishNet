---
name: Bug Report
description: Badge click handler calls preventDefault and stopPropagation which can interfere with Gmail/Outlook click handlers
title: "[Bug]: Badge click preventDefault/stopPropagation may interfere with host page interactions"
labels: ["bug", "needs-triage", "content-script", "ux"]
---

## Bug Description

`src/content.js:493-497` adds a click listener to the verdict badge that calls both `e.stopPropagation()` and `e.preventDefault()`. While this prevents the badge click from triggering Gmail/Outlook's own email-row click handlers, `preventDefault()` on a `span` element is generally unnecessary (spans don't have default actions), and `stopPropagation()` could interfere with legitimate host-page event handling if Gmail/Outlook relies on event bubbling for certain interactions.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** Medium  
**Provider:** Both/All

## Steps to Reproduce

1. Click a PhishNet verdict badge in Gmail
2. The badge opens the threat modal as expected
3. However, `stopPropagation()` prevents the click from bubbling, which could interfere with Gmail's selection or action shortcuts if the badge overlaps an interactive row

## Expected Behavior

Use `stopPropagation()` only if needed to prevent row selection; avoid `preventDefault()` on non-interactive elements.

## Actual Behavior

Both `stopPropagation()` and `preventDefault()` are called on every badge click.

## Files Affected

- `src/content.js:493-497` — badge click event listener
- `src/content.js:498-504` — badge keydown event listener
