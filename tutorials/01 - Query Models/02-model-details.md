---
title: Model Details
description: Get details about a model
---

# Model details

### On Microsoft DeepDev website

On our website, you can see the model details by clicking on the models in the library. The model name, version and owner information are displayed at the top of the page.

This page will give you additional information on the model and how to execute it.

### Using the API

Send a `GET` request to model detail API at `https://deepdev-api.microsoft.com/api/v1.0/model/<name>/<version>/<owner>`, where `<name>`, `<version>`, `<owner>` are the correponding values for the model you want to query.

## Model deployment status

During model development,

### On Microsoft DeepDev website

Each model will have a deployment badge next to its name on the model details page indicating whether the model is properly deploy and ready to use.

### Using the API

Send a `GET` request to model deployment details API at `https://deepdev-api.microsoft.com/api/v1.0/deployment/<name>/<version>/<owner>`, where `<name>`, `<version>`, `<owner>` are the correponding values for the model you want to query.

The deployment details endpoint will give you the model's current deployment status, as well as the inference URL (if the model is ready to be queried).
