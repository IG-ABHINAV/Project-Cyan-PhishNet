---
name: Bug Report
description: content.js safeQuery with document fallback could extract wrong subject when element is detached
title: "[Bug]: content.js subject extraction falls back to document when element.closest returns null"
labels: ["bug", "needs-triage", "content-script", "detection"]
---

## Bug Description

`src/content.js:307`:

```js
const subjectEl = safeQuery(element.closest('[data-thread-id]') || element.closest('[data-convid]') || document, subjectSelectors, 'Subject');
```

If `element` is detached from the DOM or `element.closest()` returns null, the fallback is `document`. This means `safeQuery(document, subjectSelectors, 'Subject')` could match a subject element anywhere on the page — potentially the wrong email's subject, a compose subject, or a different thread's subject — leading to incorrect email content extraction.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** Medium  
**Provider:** Gmail, Outlook

## Steps to Reproduce

1. Content script processes an email element
2. `element.closest('[data-thread-id]')` returns null (element not in expected DOM structure)
3. Falls back to `document` for subject extraction
4. Wrong subject text is extracted from elsewhere on the page

## Expected Behavior

If no thread container is found, use `element` itself as the search root, not `document`.

## Actual Behavior

Falls back to searching the entire document for subject selectors.

## Files Affected

- `src/content.js:307` — subject extraction fallback
