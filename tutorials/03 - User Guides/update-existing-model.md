---
title: Update existing model
internal: true
---

# Update existing model

Once a model is deployed, there are a few things that can be updated without needing the model to be redeployed. You can update the model visibility, change the Model Overview readme, and change the details of the model.

To perform an update, send a `PATCH` request to `/api/v1.0/model/{name}/{version}` with the appropriate payload.

_Note_: a single `PATCH` request can modify multiple aspects of the model (e.g. model visibility and model overview).

## Change Model Visibility

To change a model's visibility, send a `PATCH` request to `/api/v1.0/model/{name}/{version}` with the following payload:

```JSON
{
   "model_config": {
      "private": false
   }
}
```

## Change Model Overview

To change the Model Overview readme file, send a `PATCH` request to `/api/v1.0/model/{name}/{version}` with the following payload:

```JSON
{
   "model_files":
    {
        "index.md": "https://storage.blob.core.windows.net/path_to_file/SAS_KEY"
    }    
}
```

## Change Model Details

To change the details of a Model, send a `PATCH` request to `/api/v1.0/model/{name}/{version}` with the following payload:

```JSON
{
    "model_metainfo": {
      "input_types": [
        "NEW_INPUT_TYPE"
      ],
      "input_languages": [
        "NEW_INPUT_LANGUAGE"
      ],
      "input_description": "New input description.",
      "output_types": [
        "NEW_OUTPUT_TYPE"
      ],
      "output_languages": [
        "NEW_OUTPUT_LANGUAGE"
      ],
      "output_description": "New output description.",
      "num_params": number_of_parameters
    }
}
```