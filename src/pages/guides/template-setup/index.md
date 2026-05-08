---
title: Template setup guide for Illustrator API Data Merge
description: Prepare Adobe Illustrator templates and data sources for the Data merge API using desktop variables workflows, with links to HelpX for detailed steps.
keywords:
  - Adobe Illustrator API
  - Data merge
  - Illustrator template
  - variables
  - CSV
  - Firefly Services
  - REST API
  - design automation
hideBreadcrumbNav: true
---

# Template setup guide for Data Merge templates

Use this guide when you are preparing an Adobe Illustrator (`.ai`) file and related data so you can **run Data merge jobs** through the [Illustrator API](../../api/index.md). It summarizes what you do in **Illustrator on the desktop** and points to official HelpX topics for step-by-step UI instructions.

## When do API users need these instructions?

Before you call the [asynchronous Data merge operation](../../api/index.md), you need a template whose variables and bindings match how each CSV row should populate the artwork.

For service behavior and limits (for example, CSV row count, asset size, and automatic variable-to-object mapping), see [Technical usage notes — About auto-mapping for Data Merge](../../getting-started/usage/index.md).

**For authoring the template, you'll need to use the Illustrator app.** Variables, data sets, bindings, and typical CSV or XML preparation are done in Illustrator (for example, through the **Variables** panel). The API merges **CSV** data with your uploaded template in the cloud; it does not replace building or validating that template in the product.

Follow the Adobe product documentation on HelpX collected here for the latest guidance in each of the use cases below:

## Merge data

**When you need this.** Use this topic when you are planning the full data-driven design flow in Illustrator: template document, data file, variables, previews, and optional desktop batch export. Understanding merge on the desktop helps you design templates that behave predictably when merged at scale through the API.

Adobe HelpX explains how to merge data efficiently to create dynamic, personalized designs: create an Illustrator document to use as the template, prepare a source file in CSV or XML format, open **Window > Variables** to import a data source, bind variables to objects, preview each data set, and optionally use **Window > Actions** to export a batch of files from the data. HelpX also describes saving a **data merge template** when you define variables, including saving to **SVG** with **Include Adobe Graphics Server Data** for variable substitution in downstream workflows.

[Merge data in Adobe Illustrator (HelpX)](https://helpx.adobe.com/illustrator/desktop/automate-visualize-data/automate-actions/merge-data.html)

## Set up data source files

**When you need this.** Follow this when you are authoring or validating comma-delimited (`.csv`) or XML source files whose fields must align with variables and naming rules—especially the first row and special prefixes that tell Illustrator how to treat each column.

Data source files should be saved as comma-delimited (`.csv`) or XML. For CSV, records are separated by paragraph breaks and fields by commas or tabs; the file can include text or paths that point to images. In the first row, use an at sign (`@`) before a field name for text or image paths, a percent sign (`%`) for graphs, and a number sign (`#`) for visibility, with `true` or `false` per field as needed. Field names should not contain spaces (for example, `Company_Name` instead of `Company Name`). Save from supported spreadsheet apps in the formats HelpX lists; some CSV variants (such as Mac Comma Separated) are not supported for data merge. For XML, HelpX covers defining variables, using the **Data Set** control in the **Variables** panel, saving and loading variable libraries, and editing the XML in a text editor.

[Set up data source files in Adobe Illustrator (HelpX)](https://helpx.adobe.com/illustrator/desktop/automate-visualize-data/automate-actions/set-up-data-source-files.html)

## Import data source files

**When you need this.** Use this after your CSV or XML file is prepared and you need to attach it to the open document, switch data sets, or update the artboard from the **Variables** panel before saving the `.ai` template for API use.

You can import a data source in the **Variables** panel and bind variables to your data. Only one data source file can be selected per document. Choose **Window > Variables**, then **Import**, pick a CSV or XML file in the dialog, and use the **Data Set** menu to switch sets, rename, delete, or update data. HelpX includes tips for navigating data sets and a note about UTF-8 CSV files from Excel when double-byte characters are involved.

[Import CSV or XML data source files (HelpX)](https://helpx.adobe.com/illustrator/desktop/automate-visualize-data/automate-actions/import-data-source-files.html)

## Variable panel overview

**When you need this.** Read this when you want a single overview of how the **Variables** panel ties together data sources, variables, and multiple artwork variations from CSV or XML before you rely on the same structure in an API-driven merge.

HelpX walks through merging a design with CSV or XML data and creating multiple artwork variations from the **Variables** panel, including working with data sets and preview behavior. Use it alongside the sections above and below when you are new to variables-driven layouts.

[Variable panel overview (HelpX)](https://helpx.adobe.com/illustrator/desktop/automate-visualize-data/automate-actions/variable-panel-overview.html)

## Work with variables

**When you need this.** Use this when you are defining variable types, binding or unbinding objects, locking variables, or driving visibility from data so the template matches what your CSV will supply at merge time.

You can edit a variable’s name or type, unbind a variable from its object, or lock variables to restrict certain edits while still allowing binding changes. HelpX covers **Window > Variables**, selecting objects, using **Make Object Dynamic**, creating variables with **New Variable**, and using **Make Visibility Dynamic** for visibility driven by data. Variable names do not support surrogate pair and 4-byte characters; using them shows an error.

[Work with variables in Illustrator (HelpX)](https://helpx.adobe.com/illustrator/desktop/automate-visualize-data/automate-actions/work-with-variables.html)

## Edit dynamic objects

**When you need this.** Use this when you need to change bound content, select all objects tied to variables, or ensure object naming is valid for XML or SVG export paths alongside your API template.

Dynamic object names appear in the **Variables** panel as in the **Layers** panel. For SVG workflows, names must follow XML naming rules; Illustrator can assign valid XML IDs. HelpX explains setting **XML ID** in preferences and how to select bound objects from the **Variables** panel menu, then edit text on the artboard, replace linked images, edit graph data, or change visibility in the **Layers** panel for visibility variables.

[Edit dynamic objects in Adobe Illustrator (HelpX)](https://helpx.adobe.com/illustrator/desktop/automate-visualize-data/automate-actions/edit-dynamic-objects.html)

## Related topics

- [**API reference**](../api/index.md) — Data merge and job status endpoints.
- [**Technical usage notes**](../getting-started/usage/index.md) — Data Merge limits and auto-mapping from CSV headers to layer names.
- [**Authentication**](../getting-started/index.md) — Credentials and access tokens for Illustrator API.
