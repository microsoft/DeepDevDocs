---
title: Register new model
description: Register new pretrained model
---

# Register new model

Registering a new model can be done through the model_registration endpoint (`PUT https://deepdev-api.microsoft.com/api/v1.0/model/{name}/{version}`). By default, the registered model is private, which means only you can see it. You can also declare it as public while registration so that everyone can see your model. View [Request](#request) section for more information.

NOTE: Private models can not be seen by other users, but they still can be deployed. And the deployed model APIs are available universally. You may consider the inference APIs of private models as available but no one knows where to access.

## Prerequisites

- A trained transformer model, with associated model files
- An inference script and an inference environment definition

## Files

Before you register the model you will need to prepare the following files and have them ready in a storage account.

1. Model and related files (e.g. tokenizer file, configuration file, etc.)

2. Python inference script. You also need at least one fo the following environment definitions to setup the dependencies. For additional information on how to author inference scripts, see [Model inference script](author-inference-script)

   2.1. Conda environment YAML file.

   2.2. Docker base image.

3. (Optional) If the model can be trained or finetuned, a training script must be provided. You also need at least one fo the following environment definitions to setup the dependencies. For additional information on how to author training scripts, see [Model training script](author-training-script)

   3.1. Conda environment YAML file.

   3.2. Docker base image.

4. (Optional) If model trainined requires data pre-processing, a pre-processing script must be provided. You also need at least one fo the following environment definitions to setup the dependencies. For additional information on how to author pre-processing scripts, see [Data pre-processing script](author-pre-processing-script)

   3.1. Conda environment YAML file.

   3.2. Docker base image.

## Metadata

To register a model, you will need to provide the following metadata:

|                          |                                                                                                                                         |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                   | Name of the model                                                                                                                       |
| `version`                | Version of the model                                                                                                                    |
| `type`                   | Type of the model. Must be one of `ENCODER`, `DECODER`, `SEQ2SEQ`                                                                       |
| [Optional] `description` | Simple one-line description of the model. Default to empty string.                                                                      |
| `index_page_file`        | Index page in markdown describing the model.                                                                                            |
| `input_types`            | List of types of input that the model can take as input. Possible values are `SOURCE_CODE`, `NATURAL_LANGUAGE`, `CLASS_LABEL`, `OTHER`. |
| `input_languages`        | List of languages that the model can take as input. Only applicable for `SOURCE_CODE` and `NATURAL_LANGUAGE` input types.               |
| `input_description`      | English description of the model input.                                                                                                 |
| `output_types`           | List of types of output that the model can output. Possible values are `SOURCE_CODE`, `NATURAL_LANGUAGE`, `CLASS_LABEL`, `OTHER`.       |
| `output_languages`       | List of languages that the model can output. Only applicable for `SOURCE_CODE` and `NATURAL_LANGUAGE` output types.                     |
| `output_description`     | English description of the model output.                                                                                                |
| `num_params`             | Number of parameters in the model.                                                                                                      |

## Request

### PUT

To register a model, send a `PUT` request to `/api/v1.0/model/{name}/{version}`.

For full specifications on the request body. Please refer to our OpenAPI schema under the **API** tab.

The request body should be JSON-formatted and must contain at least a `model_config` section, a `model_files` section and a `model_metainfo` section.

`model_files` section contains base information of the model, such as model name, version, owner and whether the model is private or not. Set `model_files.is_private` to `false` if you would like your model to be public once registered.

All file references in `model_config`, such as inference entry script or conda YAML file, need to point to the local file names (i.e. the keys in `model_files`).

`model_files` contains all the files necessary for the model, including the inference files and training files. It is an object where the keys are the target filenames and the valuse are the source URL in the storage account.
During registration, the files will be copied to a private storage account under the target name.
Note that the URLs in `model_files` must be directly accessible, without additional authentication. Therefore, for private Azure Storage accounts, a SAS token with READ permission must be included for authorization.

`model_metainfo` section contains description of the model to make it easily consumable by others. For full specification, please refer to the API schema page.

### PATCH

To change a model's publicity, send a `PATCH` request to `/api/v1.0/model/{name}/{version}`.

Currently only `model_config.is_private` is allowed to be changed. Other properties sent with `PATCH` method will be ignored. Thus an effective request body may seems like:

``` JSON
{
   "model_config": {
      "is_private": false
   }
}
```
