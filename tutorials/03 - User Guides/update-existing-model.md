---
title: Update existing model
internal: true
---

# Update existing model

When a model is registered, it's private by default unless you declare it as public. Private models can't be seen by other users. You can modify the visibility of an existing private model.

## Request

To change a model's visibility, send a `PATCH` request to `/api/v1.0/model/{name}/{version}`.

Only `model_config.private` is allowed to be sent. Other properties sent with `PATCH` method will be ignored. Thus an effective request body may seems like:

```JSON
{
   "model_config": {
      "private": false
   }
}
```
