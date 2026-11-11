---
name: Bug Report
description: Content script uses hardcoded default settings if GET_SETTINGS call fails
title: "[Bug]: Content script falls back to hardcoded settings when background is unavailable"
labels: ["bug", "needs-triage", "content-script", "settings"]
---

## Bug Description

`src/content.js:167` defines a hardcoded default settings object that is used when `chrome.runtime.sendMessage({ type: 'GET_SETTINGS' })` fails. These defaults (`autoScan: true, sensitivityThreshold: 0.7, whitelist: []`) may not match the user's actual stored settings, causing the content script to scan or skip scanning incorrectly when the background service worker is temporarily unavailable.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Background service worker is idle or not yet initialized
2. Content script attempts to scan an email
3. `GET_SETTINGS` call fails or returns undefined
4. Content script uses hardcoded defaults instead of user's actual settings

## Expected Behavior

Cache settings locally in the content script and update them via `SETTINGS_UPDATE` messages from background.

## Actual Behavior

Hardcoded defaults are used as a fallback.

## Files Affected

- `src/content.js:167` — hardcoded default settings object
