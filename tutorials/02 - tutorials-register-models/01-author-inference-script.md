---
title: How To Author Model Inference Script
---

# How to author model inference script

You can find example inference scripts in the `examples/inference` folder.

For information on how to author the environment YAML file, see [here](#how-to-author-model-inference-environment-file)

## Prerequisites

The `deepdev` package is needed. See [package page](https://devdiv.visualstudio.com/InternalTools/_packaging?_a=package&feed=DataScienceTools&package=deepdev-services&protocolType=PyPI) for installation details.

## Structure

The training script must be written in python and must contain two functions named `init` and `run`, and a global variable for the model object.

The training script name must be a valid [PEP8 module name](https://www.python.org/dev/peps/pep-0008/#package-and-module-names). It should have a short lowercase name. Underscores are permitted but discouraged. Dashes, dots, and other punctuations will cause errors during deployment.

## `init` function

The `init` function needs to be decorated wiht `@model_init` from `deepdev.services.deploy` and should have exactly one argument that will be populate with the path to the model directory, which contains all the files listed during registration. You do not need to call this function as it will be called by the framework at run time.

The `init` function will populate the global model object which can then be used in the `run` function.

Here is an example of a valid `init` function:

```python
@model_init
def init(model_dir):
    global model
    model = fancy_model_loader(model_dir)
```

Note that the function must be named `init`, but the argument can have any name.

## `run` function

The `run` function should take exactly one non-default argument which is the body of the incoming request as a string and should return the model output as JSON-formattable object. Note that it is recommended to structure the input as a JSON-formatted string, although it is not required. Note that the function must be named `run`, but the argument can have any name.

Here is an example of the `run` function

```python
def run(some_input, extra_arg1='default value'):
    input_object = json.loads(some_input)
    if input_object['debug']:
        logging.info(f'Incoming request: {input_object}')
    preprocessed = tokenizer.tokenize(input_object['code'])
    preprocessed = normalize_code(preprocessed)
    output = model(preprocessed, context_length=input_object['context_length'])
    return output
```

## Testing your script locally

It is highly recommended to include a CLI and an executable block in your script, encapsulated by `if __name__ == "__main__"`. While deployment does not require an executable block, having a CLI can help you run the script locally and detect issues easily.

From the `__main__` block, you can call `init` directly with a model_dir, and you can call `run` with sample inputs.

Here is an example of the executable block

```python
if __name__ == '__main__':
    parser = ArgumentParser()
    parser = ArgumentParser()
    parser.add_argument("--model_dir", type=str, required=True)
    parser.add_argument("--code_before", type=str, required=True)
    parser.add_argument("--depth", type=int, default=10)
    parser.add_argument("--breadth", type=int, default=10)
    parser.add_argument("--min_tokens", type=int, default=2)
    parser.add_argument("--language", type=str, required=True)
    args = parser.parse_args()
    init(args.model_dir)
    payload = {
        "CodeBefore": args.code_before,
        "SearchDepth": args.depth,
        "SearchBreadth": args.breadth,
        "MinTokens": args.min_tokens,
        "Language": args.language,
    }
    result = run(json.dumps(payload))
    print("Model output: " + result)
```

# How to author model inference environment file

Inference environment must be defined as a conda YAML file. It should define the python environment needed to execute the inference script.

In addition, you also need to have the `azureml-defaults>=1.0.45` package under the `pip` section for deployment compatibility on Azure ML.

## Testing locally

It is recommended to test the environment locally to make sure that all the necessary packages are included.

To do so, create a new conda environment with the YAML file: `conda env create -f <environment_file.yml>`

Then, execute the inference script and verify the output.
