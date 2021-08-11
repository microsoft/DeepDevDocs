---
title: Data pre-processing script
tags: MicrosoftOnly
---

# How to author data pre-processing script

## Prerequisite

The `deepdev` package is needed. See [package page](https://devdiv.visualstudio.com/OnlineServices/_packaging?_a=package&feed=DeepDev&package=deepdev&protocolType=PyPI&version=1.0.1&view=overview) for installation details.

## Authoring

The pre-processing script should be written in Python with a valid PEP8 module name, namely "Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability."

There are two functions you need to implement in the script: `run` and `init`.

### `init` function

`init` function accepts a single parameter `data_dir`, whose value is the raw data directory containing all the data files. You can treat the directory as your workspace and do anything there.

A raw data directory may look like:

```
raw-data-directory
    ├─ subfolder
    │    ├─ subsubfolder
    │    │    └─ 10m.txt
    │    └─ Lorem.txt
    └─ enwik9.zip
```

To tell the users how to prepare for the raw data, you need to compose a markdown file telling them about the structure you want for the raw data directory. Microsoft DeepDev will host that markdown file as a web page introducing to users.

This function should implement tasks that need to be done before processing, like decompressing archived files. The return value of this function must be a dict which maps the data file paths that needs to be processed to the paths of output files. This is important because files not listed as key of the dictionary will be just ignored during processing.

Here is an example of the `init` function:

```python
import os
import zipfile
from typing import Dict

def init(data_dir: str) -> Dict[str, str]:
    wiki_txt_zip = os.path.join(data_dir, "enwik9.zip")

    with zipfile.ZipFile(wiki_txt_zip, "r") as zip_ref:
        zip_ref.extractall(data_dir)

    return {
        "enwik9": "enwik9.bin",
        "subfolder/Lorem.txt": "subfolder/Lorem.bin",
        "subfolder/subsubfolder/10m.txt": "subfolder/subsubfolder/10m.bin",
    }
```

Important: The function must be named `init`, but the argument can have any name besides `data_dir`.

### `run` function

`run` function is the one actually executed during data processing. This function is called for each line in each data file. So you just need to define how to process a single line of data and Microsoft DeepDev will handle the parallel computation & management for you. It accepts two parameters `data_line` and `file`.

`data_line` represents for a single line of the data file. `run` function should transform the line into what it should be and return that.

`file` is the file where current data line comes from. It will be helpful if you have different processing logic for different files, otherwise you can just ignore it. Value of this parameter will be the same as keys of the dict returned by `init`.

Here is an example for `run` function:

```python
import binascii

def _to_hex_repr(string: str, encoding: str = "utf8") -> str:
    return "".join(
        map(
            lambda x: f"\\x{binascii.hexlify(bytes([x])).decode()}",
            string.encode(encoding),
        )
    )

def run(data_line: str, file: str) -> str:
    return _to_hex_repr(data_line.rstrip("\r\n"))
```

Microsoft DeepDev allows a processing time of **2** seconds per line, or a timeout error will be raised. And if `run` function fails and throws an exception during runtime, the whole pre-processing job will fail and the error message will be logged.

If you want to ignore some data lines, just return `None` there and this line will be ignored from output file.

> NOTE: returning empty string is different from returning `None`, empty string means writing an empty line in the output file.

**raw folder mode**

Sometimes data files can not be easily separated into lines, e.g. binary files like images, or the pre-processing logic is too complicated to use the above workflow. In this case, you can decorate your `run` function with `@raw_folder`. If so, `run` function only accepts one single parameter `data_dir` just like `init` function.

Now you are in charge of everything during processing. Like parallel programming using [multiprocessing](https://docs.python.org/3/library/multiprocessing.html) library to utilize all the CPU cores and do error handling. The return value should be a single string value which is path of the folder containing processed data files.

Here is an example:

```python
from deepdev.services.preprocess import raw_folder

@raw_folder
def run(data_dir: str):
    target_dir = "PATH/TO/PROCESSED/DATA"

    # Read files from `data_dir`
    # Do processing
    # Write files into `target_dir`

    return os.path.abspath(target_dir)
```

# How to author pre-processing environment file

You can define the environment for pre-processing just like for inference or training, see [How to author model inference environment file](./author_inference_script.md#how-to-author-model-inference-environment-file). You can even reuse the conda environment files when registering your model. Please refer to [Register Models](./register_models.md) on how to register model.

Here is an example:

```yaml
name: preprocessing
channels:
  - defaults
dependencies:
  - python=3
  - pip
  - pip:
      - deepdev
```

For more information about the format of environment file, refer to conda's documentation on [Managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#create-env-file-manually).

## Testing locally

It is highly recommended to test the environment and your pre-processing script locally to make sure that all the necessary packages are included and logic is correct.

To do so, create a new conda environment with the YAML file: `conda env create -f <environment_file.yml>`, prepare a small sample dataset on your local machine and run `init` and `run` function against that. Verify the output files after the execution.
