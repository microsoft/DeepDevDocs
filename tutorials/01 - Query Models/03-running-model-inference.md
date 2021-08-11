---
title: Running model inference
keywords:
description: Running model inference
---

# Running model inference

### On Microsoft DeepDev website

Under the model page, you can navigate to the `Inference` tab to access the API documentation for that particular model, as well as the playground.

### Using the API

To query the model, send a `POST` request with the appropriate model payload to `https://deepdev-api.microsoft.com/api/v1.0/inference/<name>/<version>/<owner>`. To get more information on the model payload schema, you can visit the model detail page on the website.

**Getting model inference schema**

Send a `GET` request to the model API definition endpoint at `https://deepdev-api.microsoft.com/api/v1.0/inference/{name}/{version}/{owner}/swagger`, where `<name>`, `<version>`, `<owner>` are the correponding values for the model you want to query.

The response will be an OpenAPI 3.0 schema in YAML.
