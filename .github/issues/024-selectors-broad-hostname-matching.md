---
name: Bug Report
description: selectors.js broad hostname matching for Outlook may match unintended domains
title: "[Bug]: Broad Outlook hostname matching may match unintended subdomains"
labels: ["bug", "needs-triage", "selectors", "detection"]
---

## Bug Description

`src/selectors.js:156` matches Outlook providers using:

```js
} else if (hostname.includes('outlook.') || hostname.includes('office.com') || hostname.includes('office365.com')) {
```

The `hostname.includes('outlook.')` check is broad and could match domains like `outlook.evil.com` or `myoutlook-service.com` if they happen to contain 'outlook.' as a substring. Similarly, `hostname.includes('office.com')` matches `notoffice.com.evil.com`. While the content script only runs on `host_permissions` URLs (which are specific), the selector matching logic itself is overly permissive.

**Component:** Selectors  
**Severity:** Low  
**Provider:** Outlook

## Steps to Reproduce

1. Navigate to a domain that contains 'outlook.' as a substring (if somehow loaded)
2. Selector matching returns Outlook selectors for a non-Outlook site

## Expected Behavior

Use exact hostname matching or endsWith checks instead of includes.

## Actual Behavior

Substring matching could match unintended domains.

## Files Affected

- `src/selectors.js:156`
