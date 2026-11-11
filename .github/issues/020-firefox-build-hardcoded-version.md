---
name: Bug Report
description: build-firefox.sh hardcodes version v1.0.0 instead of reading from manifest/package.json
title: "[Bug]: Firefox build script hardcodes version instead of reading from manifest"
labels: ["bug", "needs-triage", "build", "firefox"]
---

## Bug Description

`build-firefox.sh:40` creates the zip file with a hardcoded name:

```bash
with zipfile.ZipFile('../phishnet-v1.0.0-firefox.zip', 'w', zipfile.ZIP_DEFLATED) as z:
```

This should read the version from `manifest.json` or `package.json` like the CI pipeline does. The hardcoded v1.0.0 is out of sync with the current version (1.1.0).

**Component:** Build / Configuration  
**Severity:** Low  
**Provider:** Not provider-specific

## Steps to Reproduce

1. Run `./build-firefox.sh`
2. Output file is `phishnet-v1.0.0-firefox.zip` regardless of actual manifest version

## Expected Behavior

Zip filename should use the version from manifest.json.

## Actual Behavior

Hardcoded v1.0.0 in zip filename.

## Files Affected

- `build-firefox.sh:40`
