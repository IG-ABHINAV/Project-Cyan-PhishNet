---
name: Bug Report
description: heuristic-detector.js keyword-based domain flagging causes false positives on legitimate domains containing security-related words
title: "[Bug]: Heuristic keyword domain check causes false positives on legitimate domains"
labels: ["bug", "needs-triage", "detection", "false-positive"]
---

## Bug Description

`src/utils/heuristic-detector.js:317-320` flags any domain containing keywords like `secure`, `verify`, `account`, `login`, `signin`, `update`, `confirm`, `banking`, `credential` unless it's an official subdomain (`domain.startsWith(kw + '.')`) or ends with `.kw.com`. This causes false positives for legitimate domains like:
- `secure.apple.com` — passes (starts with `secure.`)
- But `account-services.google.com` — `domain.includes('account')` is true, `domain.startsWith('account.')` is true — passes
- `myaccount.google.com` — tested in unit tests, passes
- However, `secure-login.microsoft.com` — `domain.includes('secure')` true, `startsWith('secure.')` true — passes

The issue is with domains like `account-verify.microsoft.com` where the keyword is not at the start but the domain is actually legitimate. More critically, domains like `paypal-security.com` would be flagged because `domain.includes('security')`... wait, 'security' is not in the keyword list. But `verify-paypal.com` would be flagged because `domain.includes('verify')` is true and it doesn't start with `verify.` or end with `.verify.com`.

**Component:** Heuristic Detection Engine  
**Severity:** Medium  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Receive email from `verify-account.service.com` (legitimate but contains keywords)
2. Heuristic detector flags it as suspicious due to keyword matching

## Expected Behavior

Refine keyword matching to reduce false positives, perhaps by requiring multiple signals or checking against a known-legitimate list.

## Actual Behavior

Domains containing common security-related keywords are flagged even when legitimate.

## Files Affected

- `src/utils/heuristic-detector.js:317-320` — keyword domain check in `isUrlSuspicious()`
