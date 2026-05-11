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
2. **Submit** `POST .../v1/images/vectorize` with `input` (source URL + `mediaType`) and, if needed, optional `settings.preset`. See the [Vectorize API (public beta)](/api/beta/index.md) reference for the request schema.
3. **Poll** `GET .../v1/status/{jobId}` using the `jobId` or `statusUrl` from the submit response until `status` is `succeeded` or `failed`.
4. **Download** SVG output from the presigned URLs in the `outputs` array when the job succeeds.

## Required input for Vectorize

Use the following request shape:

| Parameter | Required | Description |
| --- | --- | --- |
| `input` | Yes | Input file details. |
| `input.source.url` | Yes | Pre-signed URL of the input file. |
| `input.mediaType` | Yes | Media type of the input file. Allowed values: `image/png`, `image/jpeg`. |
| `settings` | No | Optional object containing only `preset` (see below). |

## Example request body

```json
{
  "input": {
    "source": { "url": "https://example.blob.core.windows.net/container/art.png?sas..." },
    "mediaType": "image/png"
  },
  "settings": {
    "preset": "enhanced_general"
  }
}
```

## Request parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `settings.preset` | string | Optional. One of: **`enhanced_general`** — vector-looking generated output; **`high_fidelity_photo`** — photorealistic tracing. Omit to use the service default. Any other value returns 400. |

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
