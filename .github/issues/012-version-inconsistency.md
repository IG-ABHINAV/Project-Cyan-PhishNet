---
name: Bug Report
description: Version inconsistency across manifest, package.json, popup footer, and build scripts
title: "[Bug]: Inconsistent version numbers across manifest, package.json, popup footer, and build scripts"
labels: ["bug", "needs-triage", "build", "versioning"]
---

## Bug Description

Version numbers are inconsistent across the project:
- `manifest.json:4` — version `1.1.0`
- `package.json:2` — version `1.1.0`
- `src/popup.html:812` — footer says `PhishNet v1.0.0`
- `build-firefox.sh:40` — creates `phishnet-v1.0.0-firefox.zip` (hardcoded)
- CI `package` job — correctly uses `${{ steps.version.outputs.version }}` from manifest

This inconsistency confuses users about which version they're running and makes release tracking unreliable.

**Component:** Build / Configuration  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Open extension popup — footer shows "v1.0.0"
2. Check manifest — shows version "1.1.0"
3. Check Firefox build script — creates "phishnet-v1.0.0-firefox.zip"

## Expected Behavior

Single source of truth for version (manifest.json or package.json), referenced by all build scripts and UI footers.

## Actual Behavior

Four different version references with two different values (1.0.0 and 1.1.0).

## Files Affected

- `manifest.json:4` — version field
- `package.json:2` — version field
- `src/popup.html:812` — footer text "PhishNet v1.0.0"
- `build-firefox.sh:40` — hardcoded zip name
