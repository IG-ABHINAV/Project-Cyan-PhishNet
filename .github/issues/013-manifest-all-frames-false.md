---
name: Bug Report
description: manifest.json all_frames:false means content script doesn't run in iframes used by Gmail/Outlook
title: "[Bug]: Content script all_frames:false may miss email content loaded in iframes"
labels: ["bug", "needs-triage", "content-script", "manifest"]
---

## Bug Description

`manifest.json:42` sets `"all_frames": false` for the content script. Gmail and Outlook may load email content, compose areas, or side panels in iframes. With `all_frames: false`, the content script only runs in the top-level frame, potentially missing email content rendered in iframes.

**Component:** Selectors / Content Script  
**Severity:** Low  
**Provider:** Gmail, Outlook

## Steps to Reproduce

1. Open an email whose content is rendered in an iframe
2. Content script does not execute in that iframe
3. No badge or scanning occurs for that email

## Expected Behavior

Set `all_frames: true` if email content can be in iframes, or add frame-specific matching.

## Actual Behavior

Content script only runs in top-level frame.

## Files Affected

- `manifest.json:42` — `all_frames: false`
