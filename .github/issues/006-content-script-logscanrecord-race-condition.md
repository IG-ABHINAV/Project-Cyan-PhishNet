---
name: Bug Report
description: logScanRecord read-modify-write on chrome.storage.local has race conditions during rapid scans
title: "[Bug]: logScanRecord race condition on concurrent storage updates"
labels: ["bug", "needs-triage", "content-script", "storage"]
---

## Bug Description

`src/content.js:263-289` implements `logScanRecord()` using a read-modify-write pattern on `chrome.storage.local`: it reads `scanHistory` and `scanStats`, increments counters, appends a record, and writes back. If multiple emails are scanned in rapid succession (e.g., thread with multiple messages, or user quickly opens/closes emails), concurrent calls can read the same state and overwrite each other's updates, causing lost scan records and incorrect stats.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** High  
**Provider:** Both/All

## Steps to Reproduce

1. Open an email thread with multiple messages
2. Auto-scan triggers on each message opening
3. Multiple `logScanRecord` calls execute concurrently
4. Some scan records are lost due to lost updates

## Expected Behavior

Use atomic storage operations or a write queue to serialize storage updates.

## Actual Behavior

Concurrent read-modify-write cycles cause lost scan records and incorrect `scanStats` counters.

## Files Affected

- `src/content.js:263-289` — `logScanRecord()`
