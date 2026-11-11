---
name: Bug Report
description: openThreatModal backdrop click handler may not work correctly because card click bubbles to backdrop
title: "[Bug]: Threat modal backdrop click may not close modal when clicking card area"
labels: ["bug", "needs-triage", "content-script", "ux"]
---

## Bug Description

`src/content.js:710-712`:

```js
backdrop.addEventListener('click', (e) => {
  if (e.target === backdrop) closeModal();
});
```

The modal card is inside the backdrop. Clicking on the card sets `e.target` to a card element (not the backdrop), so the modal won't close when clicking the card area — only when clicking outside the card on the backdrop itself. This is actually intentional (clicking card content shouldn't close), but clicking on card padding/margins that belong to the card element also won't close. More importantly, the close button at line 726 is added separately, and the Escape key listener is added at line 723. The modal UX is mostly functional, but the backdrop click only works on the exact backdrop element, not on transparent areas around the card.

**Component:** Content Script (Gmail/Outlook DOM extraction)  
**Severity:** Low  
**Provider:** Both/All

## Steps to Reproduce

1. Open threat modal by clicking a badge
2. Click on the backdrop area outside the card — modal closes
3. Click on the card area — modal stays open (expected behavior)

## Expected Behavior

Backdrop click outside card closes modal — already works for true backdrop area.

## Actual Behavior

Works correctly for backdrop-only clicks; card clicks don't close (by design).

## Files Affected

- `src/content.js:710-712` — backdrop click handler
