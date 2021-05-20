---
title: Register new pretrained model
sidebar_position: 1
keywords:
description: Register new pretrained model
---

# Register new pretrained model

Registering a new model can be done through the `add_model` endpoint.

## Prerequisites

- A trained transformer model

## Files

Before you register the model you will need to prepare the following files and have them ready in a storage account.

1. Model and related files (e.g. tokenizer file, configuration file, etc.)

2. Python inference script. You also need at least one fo the following environment definitions. For additional information on how to author inference scripts, see [author_inference_script.md](./author_inference_script.md)

   2.1. (Optional) Inference conda environment YAML file.

   2.2. (Optional) Inference docker base image.

## Metadata

To register a model, you will need to provide the following metadata:

|                          |                                                                   |
| ------------------------ | ----------------------------------------------------------------- |
| `name`                   | Name of the model                                                 |
| `version`                | Version of the model                                              |
| `type`                   | Type of the model. Must be one of `ENCODER`, `DECODER`, `SEQ2SEQ` |
| [Optional] `description` | Description of the model. Default to empty string.                |

## Request

To register a model, send a POST request to https://platform-services-models.azurewebsites.net/api/add.

The request body should be JSON-formatted and must contain 2 parameters: `model_config` and `model_files`.

`model_config` needs to follow the same schema as [ModelProperties](https://athenaplatformdocs.z5.web.core.windows.net/platform/services/models/model_properties.html#ModelProperties), where fields without default values are required.
All file references such as inference entry script or conda YAML file need to point to the local file names (i.e. the keys in `model_files`).

`model_files` contains all the files necessary for the model, including the inference files and training files. It is an object where the keys are the target filenames and the valuse are the source URL in the storage account.
During registration, the files will be copied to a private storage account under the target name.
Note that the source URL must be directly accessible. Therefore, for private storage accounts, a SAS token must be included for authentication.

## Examples

|                                      |                                                                                       |
| ------------------------------------ | ------------------------------------------------------------------------------------- |
| Requst body of API call              | [`/examples/requests/gptc_add.request.json`](../../examples/gptc_add.requset.json)    |
| Inference script                     | [`/examples/deploy/gptc_deploy_entry.py`](../../examples/deploy/gptc_deploy_entry.py) |
| Inference environment YAML file      | [`/examples/deploy/gptc_deploy_env.yml`](../../examples/deploy/gptc_deploy_env.yml)   |
| Training resources                   | [`/examples/train/`](../../examples/train/)                                           |
| Pre-processing script                | [`/examples/preprocess/entry.py`](../../examples/preprocess/entry.py)                 |
| Pre-Processing environment YAML file | [`/examples/preprocess/conda.yml`](../../examples/preprocess/conda.yml)               |
