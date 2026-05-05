---
title: Custom Script Guide for Illustrator API
description: >-
  Guide to writing and executing custom scripts with the Illustrator API for
  document automation and processing.
keywords:
  - Adobe Illustrator API
  - Illustrator automation
  - document processing
  - Custom Scripts API
  - REST API
  - cloud services
  - design automation
  - creative automation
  - batch processing
og:
  title: Custom Script Guide for Illustrator API
  description: >-
    Guide to writing and executing custom scripts with the Illustrator API for
    document automation and processing.
twitter:
  card: summary
  title: Custom Script Guide for Illustrator API
  description: >-
    Guide to writing and executing custom scripts with the Illustrator API for
    document automation and processing.
---

# Custom Script Guide

Use this document to construct and execute custom script files for the Illustrator Custom Scripts API.

Illustrator API supports custom scripts. The script's author defines the custom attributes and values for a particular endpoint using *script.jsx* files in the custom script bundle. Refer to the examples below to construct your scripts.

If you aren't familiar with the concept of custom scripts, you can find more information in the [Key concepts](/getting-started/concepts/index.md) section.

## Custom script ZIP structure

The custom script must be packaged as a ZIP archive containing:

```text
my-capability.zip
  ├── script.jsx     — Script with main() function
  └── manifest.json  — Capability metadata
```

- **script.jsx**: The script file that **must** define a `main()` function as the entry point.
- **manifest.json**: Contains capability metadata. The capability name used in the execute URL is derived from this file. Please refer to [API Capability Naming Rules](../../getting-started/concepts/index.md#api-capability-naming-rules) for script name requirements.

## Required input in a custom script

The system, by default, sends a string-type argument named `"params"`, which needs to be parsed inside the script to retrieve the values of the attributes.

- **params** - Information about what to do with the reserved input params.

| Parameter | Required | Description |
| --- | --- | --- |
| `targetDocument` | Yes | The name of the Illustrator document to process. |
| `outputFolderPath` | Yes | The output folder path for generated files. |
| `zip` | No | Should be used for zipped assets only. |
| `fontFolderPath` | No | Specifies a font folder. Requires the `zip` param to be set. |

```json
{
    "assets": [
        {"path": "template.ai"},
        {"path": "assets.zip"},
        ...
    ],
    "params": {
        "targetDocument": "template.ai",
        "outputFolderPath": "results",
        "zip": "assets.zip",
        "assetDataFolderPath" : "assets/pngData"
        ...
    }
}
```

To use the argument, `"params"` must be parsed by the script and the values of `targetDocument` and `outputFolderPath` must be extracted.

The existing scripts must be tweaked to accept the arguments correctly:

**How to fetch params passed in script**

```javascript
var arg1 = paramsMap.targetDocument;
var arg2 = paramsMap.outputFolderPath;
// Some processing
```

All `"params"` passed in the request can be fetched using `"paramsMap".<paramKey>` in the script.

### Input examples

For example, below is a sample input and sample script code to open a document and access data present inside the `"assets"` folder:

**Example input request body**

```json
{
    "assets": [
        {
            "source": {
                "url": "<Pre-signed URL of the document>"
            },
            "destination": "template.ai"
        },
        {
            "source": {
                "url": "<Pre-signed URL of the document>"
            },
            "destination": "assets.zip"
        }
    ],
    "params": {
        "targetDocument": "template.ai",
        "outputFolderPath": "results",
        "zip": "assets.zip",
        "assetDataFolderPath" : "assets",
        "someParam": "sampleValue"
    }
}
```

Structure after unzipping `assets.zip`:

```text
C:\\basefolder\\assets
                  ├── pngData/
                  └── jpegData/
```

**Transformed input request sent to the script**

```json
{
    "assets": [
        {
            "path": "template.ai"
        }
    ],
    "params": {
        "targetDocument": "C:\\basefolder\\template.ai",
        "outputFolderPath": "C:\\basefolder\\results",
        "assetDataFolderPath" : "C:\\basefolder\\assets",
        "someParam": "sampleValue"
    }
}
```

Any parameter ending with `"FolderPath"` will be processed and converted to an absolute path, as done with `"assetDataFolderPath"`.


**Sample code that takes the input (from above)**

```javascript
var inputFile = new File(paramsMap.targetDocument);
var outputFolder = paramsMap.outputFolderPath;
var jpegDataPath = paramsMap.assetDataFolderPath + "/jpegData/";
var extraParam = paramsMap.someParam;

if (!inputFile.exists) {
    throw new Error('Input file not found: ' + inputFile.fsName);
}
someProcessing(extraParam, jpegDataPath);

var doc = app.open(inputFile);
var outputPath = paramsMap.outputPath + "/result_output.ai";
var outputFile = new File(outputPath);

doc.exportAsFormat(ExportFileType.ET_AI, outputFile);
doc.close(SaveOptions.DONOTSAVECHANGES);
```
The document should be closed without saving changes using `doc.close(SaveOptions.DONOTSAVECHANGES);`.

## Execute a custom script

Assets specified in the execution request are downloaded on the local file system using the specified identifiers. The custom script should be authored to work against locally downloaded assets.

The execution request includes a JSON dictionary as a parameter. The custom script defines the parameters and they are passed as-is to it during execution.

The generated output should be exported in `outputFolderPath` via the script. Exported assets will be uploaded to the target location.

For full API details, see the [Custom Scripts API Reference](https://developer-stage.adobe.com/firefly-services/docs/illustrator/api/#tag/Custom-Scripts).

## Providing output from a custom script

Use the information below to output data, files, or logs correctly from a script.

### If an execution is successful

When a custom script execution succeeds, the status API returns a response with `"status": "succeeded"`. The response includes job metadata, any data returned by the script, and pre-signed URLs for the output assets.

**Example success response**

```json
{
    "jobId": "<jobid>",
    "timestamp": "<timestamp>",
    "status": "succeeded",
    "data": {
        "conversionDetails": {
            "inputFile": "<Illustrator target document>",
            "outputFolder": "<outputFolderName>",
            "success": true
        }
    },
    "outputs": [
        {
            "destination": {
                "url": "<Pre-signed URL for the output asset>"
            },
            "source": "<outputFolderName>\\debug.log"
        },
        {
            "destination": {
                "url": "<Pre-signed URL for the output asset>"
            },
            "source": "<outputFolderName>\\outputResult.ai"
        }
    ]
}
```

### If an execution fails

When a custom script execution fails, the status API returns a response with `"status": "failed"` along with error details.

**Example failure response**

```json
{
    "jobId": "<jobId>",
    "timestamp": "<date-time>",
    "status": "failed",
    "errors": [
        {
            "error_code": "<error-code>",
            "message": "<error-message>"
        }
    ]
}
```

| Field | Description |
| --- | --- |
| `jobId` | The unique identifier for the job. |
| `timestamp` | The time the job completed. |
| `status` | The job status. `failed` indicates the script execution was unsuccessful. |
| `errors` | An array of error objects. Each entry contains an `error_code` and a `message` describing the failure. |

## Logging

Logging can be important for debugging your own scripts and to keep track of decisions made during a script execution.

You can log data to a `<debug>.log` file, which will be created in `outputFolderPath` via the script.
Even if the execution fails, these logs will be uploaded and a link will be provided in the response.
