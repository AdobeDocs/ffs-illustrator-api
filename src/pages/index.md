---
title: Overview - Firefly Services Illustrator API
description: This is the overview page of Firefly Illustrator API.
keywords:
  - firefly
  - illustrator
  - api
  - reference
  - documentation
  - openapi
---

<SuperHero slots="image, heading, text" background="rgba(255, 174, 0, 0.81)" />

![A firefly with its path traced by a marker pen](./illustrator-hero.png)

# Illustrator API

Programmatically scale your artwork to use anywhere your audience is, from business cards to billboards or digital ads.

## Overview

Adobe Illustrator, the industry-standard vector graphics app that lets you create logos, icons, drawings, typography, and complex illustrations for any medium.

With Firefly's Illustrator API, automate Adobe Illustrator's functionality for use cases like rendition, preview, data merge, custom script, image trace, Recolor, Manifest, and Document operations, and everything in between.

Explore all the services details in the [Illustrator API reference](api/index.md).

### What's the Illustrator Data Merge service?

Data merge uses Adobe Illustrator to create variations of a document by substituting placeholder variables with data from external sources.
You could change the names of participants on event badges, or vary images across web banners and postcards, all without having to redo your dazzling, original artwork.

Merge an Illustrator document with a data source file. Create one design, and then quickly produce variations by importing the names or the images from a data source file.

### What's the Illustrator Rendition service?

The Rendition service allows you to convert Adobe Illustrator files into various output formats. It supports conversion of .ai documents into multiple web- and print-ready formats. The output is provided as a single converted file.

### What's the Illustrator Custom Scripts (beta) service?

<inlinealert variant="info" slots="heading, text"/>

 Public beta

The Custom Scripts services are in public beta. They are available to all users but APIs in this section are subject to change.

The Custom Scripts (beta) service allows you to execute custom scripts for Adobe Illustrator. It supports comprehensive scripting capabilities for document manipulation and processing.

These beta services include:

- [Submit a custom script](api/beta/index.md#operation/registerCustomScriptCapability)
- [Execute a custom script](api/beta/index.md#operation/executeCustomScriptCapability)

You find more information about the Custom Scripts (beta) service in the [Custom Scripts API (public beta)](api/beta/index.md) reference.
You find more information about how to write and execute custom scripts in the [Custom Script Guide](guides/custom-scripts/index.md).

### What's the Illustrator Vectorize (beta) service?

<inlinealert variant="info" slots="heading, text"/>

 Public beta

The Vectorize service is in public beta. It is available to all users, but APIs in this section are subject to change.

The Vectorize workflow lets you submit raster images and retrieve vectorized Illustrator documents without packaging a custom script. You provide presigned URLs for inputs, poll `GET /v1/status/{jobId}`, and download outputs from presigned URLs when the job completes.

- [Submit a Vectorize job](api/beta/index.md#operation/submitImageVectorizeJob)

Read the [Vectorize API (public beta)](api/beta/index.md) reference for request and response shapes, and the [Vectorize guide](guides/image-vectorize/index.md) for the end-to-end workflow.

## Discover

<DiscoverBlock slots="heading, link, text"/>

### Get started

[**Getting Started**](getting-started/index.md)

Get started with authentication and other prerequisites.

<DiscoverBlock slots="link, text"/>

[**Try the API**](api/index.md)

Try the API with Swagger UI. Explore, make calls, with full endpoint descriptions.

<DiscoverBlock slots="heading, link, text"/>

### Custom Scripts and Vectorize API (public beta)

[**Try the Illustrator public beta API**](api/beta/index.md)

Explore the public beta reference for Custom Scripts and Vectorize.

<Resources slots="heading, links"/>

#### Resources

* [What is Adobe Illustrator?](https://www.adobe.com/learn/illustrator/web/what-is-illustrator?learnIn=1&locale=en)
* [Adobe Illustrator](https://www.adobe.com/products/illustrator.html)
* [Prepare a data source file](https://helpx.adobe.com/uk/illustrator/using/data-driven-graphics-templates-variables.html#prepare-data-source-file)
