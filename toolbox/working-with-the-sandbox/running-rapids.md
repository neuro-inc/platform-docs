# Running RAPIDS

[RAPIDS](https://rapids.ai/) is an NVIDIA toolset for preparing and executing end-to-end data science and analytics pipelines on GPUs. It's implemented as a Python library.

To use RAPIDS on Neu.ro, you can run it in conjunction with Jupyter.

## Quick start

We created a [preconfigured project](https://github.com/neuro-inc/ml-recipe-rapids) that you can download and use for running RAPIDS.

Just copy the repository and run the following command in the CLI:

```text
$ neuro-flow run jupyter
```

This will open Jupyter in your browser, and you can start working with RAPIDS.

## Running RAPIDS in your own project

To run RAPIDS in your own Neu.ro project, you will need to add the following live action description in its `.neuro/live.yml` file:

```text
kind: live
title: <action_title>

jobs:
  jupyter:
    title: "<job_title>"
    image: rapidsai/rapidsai:cuda10.1-runtime-ubuntu18.04-py3.8
    preset: gpu-v100-small
    http_port: 8888
    http_auth: false
    browse: True
    detach: True
    life_span: 3h
```

Now, when you run `neuro-flow run jupyter` in the CLI, an instance of Jupyter will be opened in your browser, containing all necessary RAPIDS notebooks from the `rapidsai/rapidsai:cuda10.1-runtime-ubuntu18.04-py3.8` image.

