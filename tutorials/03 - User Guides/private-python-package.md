---
title: Private python packages
tags: MicrosoftOnly
internal: true
---

# How to use private python packages

Models may depend on third party python packages to do inference. Including packages which are not intent to be public. Microsoft DeepDev allows model owners to use private packages. These packages should be uploaded at the same time when registering the model.

Private packages will be uploaded to a private feed managed by Microsoft DeepDev. The feed will only be used by deploying the models. Offer the url of private packages the model needs in model registration request body, along side with `model_config` and `model_files`.

The packages are strongly recommended to be packed in [Wheel Binary Package Format](https://www.python.org/dev/peps/pep-0491/). However, source distribution packages is also supported with several file extension names: "tar.gz", "tar.bz2", "tar.xz", "tar.Z", "tar" and "zip". File name of source distribution packages should follow the pattern `{package_name}.{version}.{ext}`. Check [File name of a Source Distribution](https://www.python.org/dev/peps/pep-0625/) for more information.

Private package files are recommended to be put in Azure Blob Storage to pass to Microsoft DeepDev. By using Azure Blob Storage, limited access to the files are available by [Shared Access Signatures(SAS)](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview).

Here is an example for payload uploading three private packages when registering the model:

```JSON
{
    "model_config": { ... },
    "private_packages": [
        "https://URL.OF.PRIVATE.PACKAGE/1/PACKAGE_NAME-0.0.0-py3-none-any.whl",
        "https://URL.OF.PRIVATE.PACKAGE/2/PACKAGE_NAME_2-0.0.0.tar.gz",
        "https://URL.OF.PRIVATE.PACKAGE/3/PACKAGE_NAME_3-0.0.0.zip"
    ],
    "model_files": { ... }
}
```
