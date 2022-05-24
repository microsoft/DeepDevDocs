---
title: Discover models
description: Discover Microsoft DeepDev models
---

# Discover Models

## List all available models

### On Microsoft DeepDev website

You can discover the library of Microsoft DeepDev models through the website. Simply go to the `Models` tab. From the model library, you can see the list of models along with their name, version and owner name, as well as a short description of the model. Clicking on a model will bring you to the Model Overview page containing more details including a long description, input/output formats, and examples.

### Using the API

To list the models on the catalog, send a `GET` request to the list models API at `https://deepdev-api.microsoft.com/api/v1.0/models`. See [Common Requests](/docs/Basics/common-curl-requests)

The request will return the list of models on the Microsoft DeepDev platform in JSON format. Each model will include their name, version and owner information, as well as additional model type and architecture information.
