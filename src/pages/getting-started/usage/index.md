---
title: "Technical usage notes for Illustrator API"
description: "Details about what's currently supported, limitations, and workarounds for the Illustrator API."
keywords:
  - firefly
  - illustrator
  - limitations
  - rate limits
  - fonts
---
# Technical usage notes

This page has details about what's currently supported, limitations, and workarounds for the Illustrator API to help optimize your API implementations and understand service boundaries.

## Known limitations

These are known limitations to the Illustrator API:

* A maximum of 5000 rows are supported in CSV files.
* Any asset's (AI file, CSV file, font files) file size should not exceed 1GB.

## About retries

The service doesn't currently support automatic retries. In case of any type of failure, users should retry the operation.

## About auto-mapping for Data Merge

If variable-to-object mappings are not defined in the Illustrator file, the service can automatically generate them by reading the CSV header row and matching each column name with an object in the Illustrator Layers panel with the exact same name.

When a match to the exact name is found, the variable-to-object mapping is created automatically. The match must be exact and is case-sensitive.

## Illustrator API supported fonts

These are the currently supported Postscript fonts for Illustrator API's. Additionally the user can use any fonts they are authorized to access via [Adobe Fonts](https://fonts.adobe.com/fonts).

| Font Name                      |
|--------------------------------|
| AcuminVariableConcept          |
| AdobeArabic-Regular            |
| AdobeInvisible2-Regular        |
| AdobeMingStd-Light             |
| AdobeMyungjoStd-Medium         |
| AdobeSongStd-Light             |
| EmojiOneColor                  |
| KozGoPr6N-Regular              |
| MinionVariableConcept-Italic   |
| MinionVariableConcept-Roman    |
| MyriadHebrew-Regular           |
| MyriadPro-Regular              |
| MyriadVariableConcept-Italic   |
| MyriadVariableConcept-Roman    |
| SourceCodeVariable-Italic      |
| SourceCodeVariable-Roman       |
| SourceHanSansSC-Regular        |
| SourceSansVariable-Italic      |
| SourceSansVariable-Roman       |
| SourceSerifVariable-Roman      |
| TrajanColor-Concept            |
