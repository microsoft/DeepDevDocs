---
title: Discover models
description: Discover Microsoft DeepDev models
---

# Discover Models

## List models

### On Microsoft DeepDev website

You can discover the library of Microsoft DeepDev models through the website. Simply go to the `Models` tab. From the model library, you can see the list of models along with their name, version and owner name, as well as a short description of the model. You can click on each individual model to see more detail.

### Using the API

To list the models on the catalog, send a `GET` request to the list models API at `https://deepdev-api.microsoft.com/api/v1.0/models`.

The request will return the list of models on the Microsoft DeepDev platform in JSON format. Each model will include their name, version and owner information, as well as additional model type and architecture information.
