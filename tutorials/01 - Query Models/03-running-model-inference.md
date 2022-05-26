---
title: Running model inference
description: Running model inference
---

# Running model inference

### On Microsoft DeepDev website

Under the Model Overview page, you can navigate to the `Inference` tab to access the API documentation for that particular model, as well as the playground. Each model will have a required format for the inference payload. An example payload can be found in the Model Overview page.

### Using the API

To query a model, send a `POST` request with the appropriate model payload to `https://deepdev-api.microsoft.com/api/v1.0/inference/<model_name>/<model_version>/<model_owner>`. To get more information on the model payload schema, you can visit the Model Overview page on the website.

**Getting model inference schema**

Send a `GET` request to the model API definition endpoint at `https://deepdev-api.microsoft.com/api/v1.0/inference/{model_name}/{model_version}/{model_owner}/swagger`, where `<model_name>`, `<model_version>`, `<model_owner>` are the correponding values for the model you want to query.

The response will be an OpenAPI 3.0 schema in YAML.