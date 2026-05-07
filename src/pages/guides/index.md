---
title: Guides - Firefly Services Illustrator API
description: >-
  In-depth guides for the Illustrator API, including custom script bundles,
  vectorize jobs, capability metadata, and integration patterns.
hideBreadcrumbNav: true
keywords:
  - firefly
  - illustrator
  - api
  - guides
  - custom scripts
  - vectorize
  - documentation
---

# Illustrator API Guides

Use these guides when you need step-by-step detail beyond the [API reference](../api/index.md) and [Getting Started](../getting-started/index.md) topics. Guides focus on specific workflows, file formats, and best practices for Illustrator API integrations.

## Available guides

### [Custom Script Guide](custom-scripts/index.md)

Learn how to package and run custom script bundles with the Custom Scripts API: ZIP layout, `manifest.json`, `script.jsx` entry points, and how capability names map to execution URLs. Pair this guide with the [Custom Scripts API (public beta)](../api/beta/index.md) reference for request and response shapes.

### [vectorize guide](image-vectorize/index.md)

Run raster-to-vector (Image Trace) jobs: `input` (`source.url` + `mediaType`), `settings` options, polling `GET /v1/status/{jobId}`, and downloading SVG output. Use with the [public beta API reference](../api/beta/index.md); align optional trace fields with the Vectorize service UAP.

## Related topics

- [**Authentication**](../getting-started/index.md) — OAuth Server-to-Server credentials and access tokens for Illustrator API.
- [**Key concepts**](../getting-started/concepts/index.md) — Terminology, including API capability naming rules referenced from the Custom Script Guide.
