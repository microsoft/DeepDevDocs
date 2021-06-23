---
title: Sending requests
---

# Accessing the API

## Using Postman

### Authentcation

Under the `Headers` tab, create a new header with name `Authorization` and value `Bearer <API token>`.

![](./postman_auth.png)

## Using cUrl

### Authentication

Set the `Authorization` header with the value `Bearer <API token>`, `<API token>` can be found under `Settings` -> `Your API key` on the website.

`curl -H "Authorization: Bearer <API token>" ...`

## Using the API playground

Another way to access the API

### Authentication

After generating an API key, you will now be able to access the DeepDev API playground which is accessible from the `API` left navbar item in the DeepDev website.

Navigate to the `Authentication` section in the left sidebar, and paste the API key into the `HTTP Bearer` field.

Finally, click the `SET` button to ensure that the API key is used for subsequent API operations.

### Using the API endpoints

Now that you are authenticated, you can access the API endpoints. For each endpoint, after filling out the appropriate parameters you can send the request by pressing the `TRY` button at the bottom of the expanded endpoint container.
