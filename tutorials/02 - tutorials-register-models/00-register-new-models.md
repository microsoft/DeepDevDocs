---
title: Register New Pretrained Model
description: Register new pretrained model
---

# Register new pretrained model

Registering a new model can be done through the `add_model` endpoint.

## Prerequisites

- A trained transformer model

## Files

Before you register the model you will need to prepare the following files and have them ready in a storage account.

1. Model and related files (e.g. tokenizer file, configuration file, etc.)

2. Python inference script. You also need at least one fo the following environment definitions to setup the dependencies. For additional information on how to author inference scripts, see [author_inference_script.md](./author_inference_script.md)

   2.1. Conda environment YAML file.

   2.2. Docker base image.

3. (Optional) If the model can be trained or finetuned, a training script must be provided. You also need at least one fo the following environment definitions to setup the dependencies. For additional information on how to author training scripts, see [author_training_script.md](./author_training_script.md)

   3.1. Conda environment YAML file.

   3.2. Docker base image.

4. (Optional) If model trainined requires data pre-processing, a pre-processing script must be provided. You also need at least one fo the following environment definitions to setup the dependencies. For additional information on how to author pre-processing scripts, see [author_pre_processing_script.md](./author_pre_processing_script.md)

   3.1. Conda environment YAML file.

   3.2. Docker base image.

## Metadata

To register a model, you will need to provide the following metadata:

|                          |                                                                   |
| ------------------------ | ----------------------------------------------------------------- |
| `name`                   | Name of the model                                                 |
| `version`                | Version of the model                                              |
| `type`                   | Type of the model. Must be one of `ENCODER`, `DECODER`, `SEQ2SEQ` |
| [Optional] `description` | Description of the model. Default to empty string.                |

## Request

To register a model, send a POST request to `/api/add_models` endpoint.

For full specifications on the request body. Please refer to our OpenAPI schema under the **API** tab.

The request body should be JSON-formatted and must contain 2 parameters: `model_config` and `model_files`.

All file references in `model_config`, such as inference entry script or conda YAML file, need to point to the local file names (i.e. the keys in `model_files`).

`model_files` contains all the files necessary for the model, including the inference files and training files. It is an object where the keys are the target filenames and the valuse are the source URL in the storage account.
During registration, the files will be copied to a private storage account under the target name.
Note that the URLs in `model_files` must be directly accessible, without additional authentication. Therefore, for private Azure Storage accounts, a SAS token with READ permission must be included for authorization.

## Examples

|                                      |                                                                                       |
| ------------------------------------ | ------------------------------------------------------------------------------------- |
| Requst body of API call              | [`/examples/requests/gptc_add.request.json`](../../examples/gptc_add.requset.json)    |
| Inference script                     | [`/examples/deploy/gptc_deploy_entry.py`](../../examples/deploy/gptc_deploy_entry.py) |
| Inference environment YAML file      | [`/examples/deploy/gptc_deploy_env.yml`](../../examples/deploy/gptc_deploy_env.yml)   |
| Training resources                   | [`/examples/train/`](../../examples/train/)                                           |
| Pre-processing script                | [`/examples/preprocess/entry.py`](../../examples/preprocess/entry.py)                 |
| Pre-Processing environment YAML file | [`/examples/preprocess/conda.yml`](../../examples/preprocess/conda.yml)               |
