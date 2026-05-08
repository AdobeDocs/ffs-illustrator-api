---
title: Vectorize Guide for Illustrator API
description: >-
  Guide to running Vectorize jobs with the Illustrator API for raster-to-vector
  conversion.
keywords:
  - Adobe Illustrator API
  - Illustrator automation
  - image trace
  - vectorize
  - REST API
  - cloud services
  - raster to vector
og:
  title: Vectorize Guide for Illustrator API
  description: >-
    Guide to running Vectorize jobs with the Illustrator API for raster-to-vector
    conversion.
twitter:
  card: summary
  title: Vectorize Guide for Illustrator API
  description: >-
    Guide to running Vectorize jobs with the Illustrator API for raster-to-vector
    conversion.
---

# Vectorize Guide

Use this document to submit and monitor Vectorize jobs with the Illustrator API.

Illustrator API supports Vectorize workflows for raster-to-vector conversion (Image Trace style). The service accepts PNG/JPEG input, runs vectorization in Illustrator, and returns presigned URLs for SVG output when the job succeeds.

If you are new to pre-signed URLs and job polling, read [Key concepts](/getting-started/concepts/index.md) first.

## Workflow

1. **Upload** your raster file to your storage and obtain a **presigned GET URL** (or equivalent temporary download URL supported by the platform).
2. **Submit** `POST .../v1/images/vectorize` with `input` (source URL + `mediaType`) and `settings` fields. See the [Vectorize API (public beta)](/api/beta/index.md) reference for the request schema.
3. **Poll** `GET .../v1/status/{jobId}` using the `jobId` or `statusUrl` from the submit response until `status` is `succeeded` or `failed`.
4. **Download** SVG output from the presigned URLs in the `outputs` array when the job succeeds.

## Required input for Vectorize

Use the following request shape:

| Parameter | Required | Description |
| --- | --- | --- |
| `input` | Yes | Input file details. |
| `input.source.url` | Yes | Pre-signed URL of the input file. |
| `input.mediaType` | Yes | Media type of the input file. Allowed values: `image/png`, `image/jpeg`. |
| `settings` | No | Vectorization options and tuning controls. |

## Example request body

```json
{
  "input": {
    "source": { "url": "https://example.blob.core.windows.net/container/art.png?sas..." },
    "mediaType": "image/png"
  },
  "settings": {
    "preset": "auto",
    "generateSvg": true,
    "autoSettings": true
  }
}
```

## Request parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `settings.preset` | string | Optional vectorization preset name (case-insensitive). If omitted, preset is auto-selected from image complexity. Invalid values return 400. |
| `settings.generateSvg` | boolean | When true, generate SVG output (`image/svg+xml`). If omitted, defaults to true. |
| `settings.autoSettings` | boolean | When true, choose settings automatically and skip applying most manual tuning fields in the legacy path. If omitted, defaults to true. |
| `settings.colorMode` | string | Color mode: `color`, `grayscale`, or `black_and_white`. |
| `settings.colorPaletteType` | string | For color mode: `automatic`, `limited`, or `fulltone`. |
| `settings.numberOfColors` | integer | Max colors when `colorMode` is `color` and `colorPaletteType` is `limited`. Minimum 1. |
| `settings.colorFidelity` | number | Color accuracy percentage. Range: 0-100. |
| `settings.grayscale` | number | Grayscale accuracy percentage. Range: 0-100. |
| `settings.blackAndWhiteThreshold` | integer | Pixels darker than this become black in `black_and_white` mode. Range: 1-255. |
| `settings.pathFidelity` | number | Path fitting tightness percentage. Range: 0-100. |
| `settings.cornerFidelity` | number | Corner emphasis percentage. Range: 0-100. |
| `settings.noiseFidelity` | integer | Noise reduction strength. Range: 1-100. |
| `settings.overlappingPaths` | boolean | `true` for stacked paths; `false` for cut-out (abutting) paths. |
| `settings.filledPaths` | boolean | When true, produce filled regions. |
| `settings.strokedPaths` | boolean | When true, produce stroked paths. |
| `settings.maxStrokeWeight` | integer | Max stroke width in pixels when stroked paths are enabled. Minimum 1. |
| `settings.snapCurvesToLines` | boolean | Replace slightly curved segments with straight lines. |
| `settings.ignoreWhite` | boolean | Remove white fills. |

## Status and outputs

Polling uses the Vectorize status operation (`GET /v1/status/{jobId}`). When `status` is `succeeded`, use the presigned URLs in `outputs` to download generated SVG files. Error messages and HTTP status codes follow the same error schema as other beta operations in this reference.

### Submit response (202 Accepted)

```json
{
  "jobId": "f7da0875-7919-486d-b915-7258c89f09e0",
  "statusUrl": "<url>/v1/status/f7da0875-7919-486d-b915-7258c89f09e0"
}
```

### Status response (succeeded)

```json
{
  "jobId": "f7da0875-7919-486d-b915-7258c89f09e0",
  "status": "succeeded",
  "outputs": [
    {
      "destination": {
        "url": "<PRESIGNED_URL>"
      },
      "mediaType": "image/svg+xml"
    }
  ]
}
```

### Status response (failed)

```json
{
  "jobId": "fe0f4a1a-a213-44bd-b37b-8aa2f636449c",
  "status": "failed",
  "error_code": "<ERROR_CODE>",
  "message": "<MESSAGE>"
}
```

## Related links

- [**Vectorize API (public beta)**](/api/beta/index.md) — OpenAPI reference (`submitImageVectorizeJob`, `vectorizeJobStatus`).
- [**Custom Script Guide**](/guides/custom-scripts/index.md) — Another beta workflow that also uses async job polling.
