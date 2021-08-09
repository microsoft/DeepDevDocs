---
title: Model inference schema
---

# How to define model inference schema

It is strongly recommended to decorate `run` function with input and output samples. So that a swagger file can be automatically generated to tell users how to call the inference api.

To do so, add the package [`inference_schema`](https://pypi.org/project/inference-schema/) to your inference environment YAML file.

`inference_schema` offers two decorators `input_schema` and `output_schema`. It also offers three kinds of sample data wrapper classes. `StandardPythonParameterType` is the most commonly used one among them.

> NOTE: Microsoft DeepDev follows the same convention as Azure Machine Learning for composing the input and output schema. For more information, please refer to [Azure Machine Learning's documentation](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-advanced-entry-script#automatically-generate-a-swagger-schema).

## `input_schema`

`input_schema` decorator accepts two positional parameters, the first one is parameter name, whose value should be exactly the same as `run` function's parameter name. The second one should be an sample value wrapped by `StandardPythonParameterType`.

Use multiple `input_schema` decorators if your `run` function has multiple parameters.

If you want to make any parameter optional, give it a default value in `run` function and pass in `convert_to_provided_type=False` to its `input_schema` decorator.

## `output_schema`

Unlike `input_schema`, one `run` function can only have one `output_schema` decorator. It only accepts one parameter which is the sample output of your inference API. The sample value should also be wrapped by `StandardPythonParameterType`.

## Example

Here is an example for `run` function with both input and output schema defined:

```python
from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.standard_py_parameter_type import (
    StandardPythonParameterType,
)

@input_schema("input_param", StandardPythonParameterType("input sequence.")) # Mandatory
@input_schema(
    "beam_size", StandardPythonParameterType(3), convert_to_provided_type=False
) # Optional
@output_schema(StandardPythonParameterType([
    (98.3, "Output sequence.")
]))
def run(input_param, beam_size=5):
    preprocessed = tokenizer.tokenize(input_param)
    preprocessed = normalize(preprocessed)

    results = model(preprocessed)

    return results
```

In above example, users should call the inference api with JSON-formatted request body:

```json
{
  "input_param": "input sequence.",
  "beam_size": 3
}
```

And get response like:

```json
[[98.3, "Output sequence."]]
```
