---
name: Bug Report
description: No tests exist for frontend React components; only backend vanilla JS is tested
title: "[Bug]: No unit test coverage for frontend React components"
labels: ["bug", "needs-triage", "frontend", "testing"]
---

## Bug Description

The repository has 30 unit tests in `tests/unit.test.js` covering the heuristic detector, selectors, and error boundary utilities. However, there are **zero tests** for the React frontend components in `frontend/src/` (Popup, Options, ScanTab, StatsTab, SettingsTab, TrustedTab, etc.) or the messaging library (`frontend/src/lib/messaging.js`). This means regressions in the React popup/options UI would not be caught by CI.

**Component:** Frontend (React Popup/Options)  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Make a breaking change to a React component
2. Run `node tests/unit.test.js` — all 30 tests pass
3. Bug is only discovered when manually testing the popup/options page

## Expected Behavior

Add test coverage for React components using a testing library (React Testing Library, Vitest, or similar).

## Actual Behavior

No frontend component tests exist.

## Files Affected

- `tests/unit.test.js` — no frontend tests
- `frontend/src/` — entirely untested
