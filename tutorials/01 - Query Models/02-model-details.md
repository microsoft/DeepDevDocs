---
title: Model Details
description: Get details about a model
---

# Model details

### On Microsoft DeepDev website

On our website, you can see the model details in the Model Overview page by clicking on any model in the library. The Model Overview page provides a long description, relevant publications, input details, and examples. The model name, version and owner information are displayed at the top of the page. Additional information such as input and output types and languages can be found in the `Details` tab of the Model Overview page.

### Using the API

Send a `GET` request to model detail API at `https://deepdev-api.microsoft.com/api/v1.0/model/<model_name>/<model_version>/<model_owner>`, where `<model_name>`, `<model_version>`, `<model_owner>` are the correponding values for the model you want to query. Note that private models can only be viewed by the model owner.

## Model deployment status

During model development, you can get the current status of deployment either through the DeepDev website or via the API.

### On Microsoft DeepDev website

Each model will have a deployment badge next to its name on the Model Overview page indicating whether the model is properly deploy and ready to use. The status can be `Creating`, `Ready`, and `Failed`.

### Using the API

Send a `GET` request to model deployment details API at `https://deepdev-api.microsoft.com/api/v1.0/deployment/<model_name>/<model_version>/<model_owner>`, where `<model_name>`, `<model_version>`, `<model_owner>` are the correponding values for the model you want to query.

The deployment details endpoint will give you the model's current deployment status, as well as the inference URL (if the model is ready to be queried).
