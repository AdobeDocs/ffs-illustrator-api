---
title: Adobe Illustrator API Key Concepts
description: Learn important concepts for using Illustrator APIs.
keywords:
  - Adobe Illustrator API
  - Illustrator automation
  - document processing
  - Custom Scripts API
  - vectorize
  - REST API
  - cloud services
  - design automation
  - creative automation
  - batch processing
og:
  title: Adobe Illustrator API Key Concepts
  description: Learn important concepts for using Illustrator APIs.
twitter:
  card: summary
  title: Adobe Illustrator API Key Concepts
  description: Learn important concepts for using Illustrator APIs.
---

# Illustrator API Key Concepts

Consider these important concepts when using Illustrator APIs.

## Pre-signed URLs

### What is a pre-signed URL?

A pre-signed URL is a URL that grants temporary access to a specific resource, typically in cloud storage, with predefined permissions and an expiration time. It allows users to upload or download files securely
without needing direct access to the storage service's credentials.

While using Illustrator APIs, the input and output assets are stored as pre-signed URLs.

### Input assets

The platform supports multiple asset types for input. These asset types indicate storage repositories from which the platform can download. You can provide the input asset information within the `assets` array.

Illustrator APIs support the following input assets:

- **AWS S3**: [Use a pre-signed GET/PUT/POST URL.][5]

- **Dropbox**: [Generate temporary upload/download links.][6]

- **Azure**: [Use a Shared Access Signature (SAS) in Azure Storage][7] for GET/PUT/POST operations.

```json
"assets" :[
    {
        "source" : {
            "url" : "https://xyz-blob.core.windows.net/Template.ai"
        },
        "destination" : "template1.ai"
    },
    {
        "source" : {
            "url" : "https://s3.eu-west-1.amazonaws/xyz/assets.zip"
        },
        "destination" : "assets.zip"
    }
]
```

In the examples above, you can see that data is divided into `source` and `destination`.

The `source` attribute is where the asset is downloaded from. The `destination` attribute refers to where the asset would be downloaded to.

## About Vectorize (beta)

The **Vectorize** public beta service turns raster artwork (PNG or JPEG) into SVG output using server-side Image Trace-style processing. You submit a job with an **`input`** object (`source.url` + `mediaType`) and optional **`settings`** fields (`preset`, `generateSvg`, `autoSettings`). The job runs asynchronously; poll **`GET /v1/status/{jobId}`** for completion and read presigned download URLs from the response when the job succeeds.

For a step-by-step guide, see [Vectorize guide](../../guides/image-vectorize/index.md).

## About custom script bundles

To create a script to use with the Custom Scripts API, you'll need to prepare a script bundle, a ZIP file with a predefined structure.

### Custom script bundle structure

The structure for a simple custom script bundle would look like this:

```text
custom-script-folder
|------ manifest.json
|------ script.js
```

|File|Description|Required|
|---|---|---|
|manifest.json|The custom script manifest. All the details of the script are described in this file.|YES|
|script.js|The primary executable for the script. This script gets executed by the product script engine and, depending on the product script engine support, it can depend on other files in nested directories in the ZIP file.|YES|

### Custom Script manifest

The manifest file is a plain JSON file with the following structure:

```json
{
    "manifestVersion": "1.0.0",
    "id": "<Unique ID for the custom script>",
    "name": "<Name of the custom script>",
    "version": "<x.y.z>",
    "host": {
        "app": "illustrator",
        "appVersionStrategy": "latest_version" | "fixed_major_version" | "fixed_major_and_minor_version",
        "majorAppVersion": 20,  // optional depending on strategy
        "minorAppVersion": 1  // optional depending on strategy
    },
    "apiEntryPoints": [
        {
            "type": "capability",
            "path": "script.js",
            "language": "extendscript"
        }
    ]
}
```

| Field | Type | Description | Required |
|-------|:----:|-------------|----------|
| `manifestVersion` | string | The version of the manifest file format. Currently, only 1.0.0 is supported. | YES |
| `name` | string| Script name used to invoke the capability. Please refer to [API Capability Naming Rules](#api-capability-naming-rules) for more details. | YES |
| `version` | string| The version number of the custom script, in x.y.z format. The version must be three segments and each version component must be between 0 and 99. | YES |
| `host.app` | string| The host application used to execute this script. Currently, the only valid value is `illustrator`. | YES |
| `host.appVersionStrategy` | string| How the system selects the app version: latest_version, fixed_major_version, or fixed_major_and_minor_version. | NO |
| `host.majorAppVersion` | string| The major version of the app (e.g. 20 in 20.0.34). Required when appVersionStrategy is fixed_major_version or fixed_major_and_minor_version. | NO |
| `host.minorAppVersion` | string| The minor version of the app (e.g. 0 in 20.0.34). Required when appVersionStrategy is fixed_major_and_minor_version. | NO |
| `apiEntryPoints` | array | An array of `<EntryPointDefinition>` objects. Describes the API entry points for the custom script. | NO |

- When a customer registers a script without specifying the strategy, the system automatically chooses latest_version as the default strategy.
- majorAppVersion parameter is mandatory if appVersionStrategy is fixed_major_version.
- majorAppVersion and minorAppVersion are mandatory if appVersionStrategy is fixed_major_and_minor_version.

## API Capability Naming Rules

The script name is the `name` field in your custom script manifest. It identifies your capability when registering and invoking the script. Names must follow the rules below. Any non-compliant names will not be permitted for registration.

**Rules**

- **Length:** 3–255 characters.
- **Allowed characters** (any other character is rejected):
  - **Alphanumeric:** a-z, A-Z, 0-9
  - **Hyphen (-), underscore (_), period (.)**
  - **Forward slash (/):** Allowed only as a segment separator for hierarchical names (for example, `renditions/jpeg`). Do not use leading or trailing slashes, or consecutive slashes.

**Valid name examples**

| Example | Notes |
|--------|--------|
| `my-script` | Lowercase, hyphen and 3+ characters. |
| `export_to_idml` | Underscores allowed. |
| `Rendition.v1` | Period and mixed case allowed. |
| `renditions/jpeg` | Forward slash as segment separator for a hierarchy. |
| `com.acme.scripts/processor` | Combined period and slash. |

**Names that are rejected**

| Example | Reason |
|--------|--------|
| `ab` | Too short (fewer than 3 characters). |
| `my script` | Space not allowed. |
| `script@work` | At sign (@) not allowed. |
| `script#2` | Hash (#) not allowed. |
| `renditions//jpeg` | Consecutive slashes not allowed. |
| `/renditions/jpeg` | Leading slash not allowed. |
| `renditions/jpeg/` | Trailing slash not allowed. |
| `café` or `naïve` | Accented characters not allowed. |

### The `apiEntryPoints` field

In the manifest file, the `apiEntryPoints` attribute is an array of `EntryPointDefinition` objects with the following format:

|Field|Type|Description|Required|
|---|---|---|---|
|`type`|string|The type of entry point. Valid values are `capability`.|YES|
|`path`|string|The file path should be used based on the type. The default is to look for the files in the root directory of the ZIP file. However, this can also be any nested path in the ZIP file.|YES|
|`language`|string|The language of the script. It can be an extended script, UXP script, or JavaScript.|YES|

- Each entry point specifies a custom script or a custom script specification.
- There can be only one entry of each type in the array.
- The maximum size of the array is 3.

There's no need to define an entry point if the default values are being used for them.

### The `script.js` file

The script's author defines the custom attributes and values for a particular endpoint using *script.js* file in the custom script bundle.

The execution of any script depends on the following attributes:

| Attribute | Input Request Mapping | Description |
| --- | --- | --- |
| `assets` | assets->destination field | This contains a list of input assets, like .ai, .pdf, .jpeg, etc. |
| `params` | params | User input/arguments that are used inside script. |
| `jobID` | Auto-generated | The job ID. |
| `workingFolder` | Auto-generated | The working folder for the job. This is the base directory. Inside this directory, all the assets and scripts are downloaded. (for example c:\\baseFolder\\assets). |

For examples, see [Custom Script Guide](../../guides/custom-scripts/index.md).

### Zipping and updating a custom script bundle

When zipping the files, don't place them inside a folder first. Instead, directly select the files and create the ZIP. This way when it's unzipped the files will appear directly instead of appearing inside a folder.

The custom script bundle can be updated by incrementing the version in the script manifest. The updated ZIP bundle can be uploaded using the submission endpoint.

[//]: # (Links)
[5]: https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html
[6]: https://dropbox.github.io/dropbox-api-v2-explorer/#files_get_temporary_link
[7]: https://learn.microsoft.com/en-us/azure/ai-services/translator/document-translation/how-to-guides/create-sas-tokens?tabs=Containers
