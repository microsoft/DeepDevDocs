---
title: Use custom Docker images
---

# How to run on custom Docker images

DeepDev offer model inference service with GPU acceleration. By default, inference environment is defined by [conda environment file](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#sharing-an-environment). See [How to author model inference script](../02%20-%20Register%20Models/01-author-inference-script.md#how-to-author-model-inference-environment-file) for more information.

DeepDev also allow users offer their own docker images if conda environment file can not meet their requirements.

## Prepare Docker Image

Custom images must base on Azure Machine Learning's CUDA images, choose the mpi/cuda/cudnn version you want:
- mcr.microsoft.com/azureml/intelmpi2018.3-cuda9.0-cudnn7-ubuntu16.04
- mcr.microsoft.com/azureml/intelmpi2018.3-cuda10.0-cudnn7-ubuntu16.04
- mcr.microsoft.com/azureml/openmpi3.1.2-cuda9.0-cudnn7-ubuntu16.04
- mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.0-cudnn7-ubuntu16.04
- mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.0-cudnn7-ubuntu18.04
- mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04
- mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.2-cudnn7-ubuntu18.04
- mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.2-cudnn8-ubuntu18.04
- mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.0.3-cudnn8-ubuntu18.04

NOTE: Make sure conda's path `/opt/miniconda/bin` is still in `PATH` environment variable for `/bin/sh` shell after your own commits if you modified it. Or model deployment can fail.
Be sure to validate your image before submitting to DeepDev by running it locally: `docker run <image_name> /bin/sh -c 'conda --version'`. You are supposed to see output like "conda 4.5.11".

After the image is ready, it needs to be uploaded to a docker registry. [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) or [Docker Hub](https://hub.docker.com/) is recommended.

## Register Model

<!-- Maybe we want to let users modify models to change/add custom docker images in future. -->
Custom docker images are defined when registering the model. Existing models needs to be deleted and re-registered if they want to use custom docker images.

Call DeepDev's [Register Model API](../../api/index.yml#tag--Model) to use the custom docker image. Image information should be in `model_config.inference_properties`. Username and password of image registry should be given if the image repository is not public. Below is an example of the request:

``` HTTP
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

Call DeepDev's [Deploy Model API](../../api/index.yml#tag--Deployment) to use custom docker image to deploy the model.
