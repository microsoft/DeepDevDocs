---
title: Use custom Docker images
internal: true
---

# Run on custom Docker images

Microsoft DeepDev offer model inference service with GPU acceleration. By default, inference environment is defined by [conda environment file](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#sharing-an-environment). See [How to author model inference script](../02%20-%20Register%20Models/01-author-inference-script.md#how-to-author-model-inference-environment-file) for more information.

Microsoft DeepDev also allow users to bring their own docker images if a conda environment file alone cannot meet their requirements.

## Prepare Docker Image

Custom images must base on Azure Machine Learning's GPU [images](https://mcr.microsoft.com/en-us/catalog?search=azureml). Some common images are:

- mcr.microsoft.com/azureml/minimal-ubuntu18.04-py37-cuda11.0.3-gpu-inference
- mcr.microsoft.com/azureml/pytorch-1.9-ubuntu18.04-py37-cuda11.0.3-gpu-inference
- mcr.microsoft.com/azureml/tensorflow-2.4-ubuntu18.04-py37-cuda11.0.3-gpu-inference

_Note_: You can add `:latest` tag to the image link to get the latest version of the image (e.g. `mcr.microsoft.com/azureml/minimal-ubuntu18.04-py37-cuda11.0.3-gpu-inference:latest`).

Make sure conda's path `/opt/miniconda/bin` is still in `PATH` environment variable for `/bin/sh` shell after your own commits if you modified it. Or model deployment may fail.
Be sure to validate your image before submitting to Microsoft DeepDev by running it locally: `docker run <image_name> /bin/sh -c 'conda --version'`. You should see output like "conda 4.5.11".

After the image is ready, it needs to be uploaded to a docker registry. [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) or [Docker Hub](https://hub.docker.com/) is recommended.

## Register Model

<!-- Maybe we want to let users modify models to change/add custom docker images in future. -->

Custom docker images are defined when registering the model. Existing models needs to be deleted and re-registered if they want to use custom docker images.

Call Microsoft DeepDev's [Register Model API](../../api/index.yml#tag--Model) to use the custom docker image. Image information should be in `model_config.inference_properties`. Username and password of image registry should be given if the image repository is not public. Below is an example of the request:

```HTTP
PUT /api/model/MODEL_NAME/MODEL_VERSION HTTP/1.1
Content-Type: application/json

{
    "model_config": {
        "type": "SEQ2SEQ",
        "inference_properties": {
            "entry_script": "inference.py",
            "conda_env": "conda.yml",
            "docker_image": "ORGANIZATION/REPOSITORY:TAG",
            "docker_registry": {
                "url": "customimage.azurecr.io",
                "username": "USERNAME",
                "password": "PASSWORD"
            }
        }
    },
    "model_files": {},
    "model_metainfo": {}
}
```

## Deploy Model

Call Microsoft DeepDev's [Deploy Model API](../../api/index.yml#tag--Deployment) to use custom docker image to deploy the model.
