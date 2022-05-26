---
title: Common Requests
---

# Common Requests

Here is a list of requests that are used to perform basic functionality. Replace `<model_name>` with the model's name and `<model_version>` with the model's version.

### Get all public models

`curl -X GET "https://deepdev-api.microsoft.com/api/v1.0/models"  -H "Authorization: Bearer <api_key>" -H "Accept: application/json"`

### Run inference on a model

`curl -X POST "https://deepdev-api.microsoft.com/api/v1.0/inference/<model_name>/<model_version>/<model_owner>" -H "Authorization: Bearer <api_key>" -H "Content-Type: application/json" -d '<json_payload>'`

### Add a new model (Microsoft-internal only)

`curl -X POST "https://deepdev-api.microsoft.com/api/v1.0/inference/<model_name>/<model_version>/<model_owner>" -H "Authorization: Bearer <api_key>" -H "Content-Type: application/json" -d '<json_payload>'`

### Deploy a model (Model owner only)

`curl -X PUT "https://deepdev-api.microsoft.com/api/v1.0/deployment/<model_name>/<model_version>" -H "Accept: application/json" -H "Authorization: Bearer <api-key>" `

### Delete a model deployment (Model owner only)

`curl -X DELETE "https://deepdev-api.microsoft.com/api/v1.0/deployment/<model_name>/<model_version>" -H "Accept: application/json" -H "Authorization: Bearer <api-key>" `

### Delete a model (Model owner only)

`curl -X DELETE "https://deepdev-api.microsoft.com/api/v1.0/model/<model_name>/<model_version>" -H "Accept: application/json" -H "Authorization: Bearer <api-key>" `