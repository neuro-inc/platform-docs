# Getting Started

## Introduction

There are two things you will need to do before you start working with Neu.ro:

1. [Install the CLI client](getting-started.md#installing-the-cli).
2. [Understand the platform's core concepts](getting-started.md#understanding-the-core-concepts).

After this, you're free to explore the platform and it's functionality. As a good starting point, we've included a section about [development on GPU with Jupyter Notebooks](getting-started.md#developing-on-gpu-with-jupyter-notebooks).

## Installing the CLI

You can use the [Web Terminal](https://apps.neu.ro/shell?cluster_name=neuro-compute) as a quick way to explore the platform that doesn't require any installation.

However, installing the Neu.ro CLI provides the full scope of functionality, so we highly recommend this method for long-term use.

### Installation instructions

{% tabs %}
{% tab title="Linux and Mac OS" %}
Neu.ro CLI requires Python 3 installed \(recommended: 3.7, required: &gt;= 3.6\). We suggest using the [Anaconda Python 3.7 Distribution](https://www.anaconda.com/distribution/).

```text
> pip install -U neuro-cli neuro-extras neuro-flow
> neuro login
```
{% endtab %}

{% tab title="Windows" %}
There are several ways to make Neu.ro CLI work on Windows, but we highly recommend using the [Anaconda Python 3.7 Distribution](https://www.anaconda.com/distribution/) with default installation settings.

When you have it up and running, run the following commands in Conda Prompt:

```text
> conda install -c conda-forge make
> conda install -c conda-forge git    
> pip install -U neuro-cli neuro-extras neuro-flow
> pip install -U certifi
> neuro login
```

To make sure that all commands you can find in our documentation work properly, don't forget to run `bash` every time you open Conda Prompt.
{% endtab %}
{% endtabs %}

## Understanding the core concepts

On the **Neu.ro Core** level, you will work with jobs, environments, and storage. To be more specific, a job \(an execution unit\) runs in a given environment \(Docker container\) on a given preset \(a combination of CPU, GPU, and memory resources allocated for this job\) with several storage instances \(block or object storage\) attached.

Here are some examples.

### Hello, World!

Run a job on CPU which prints “Hello, World!” and shuts down:

```text
> neuro run --preset cpu-small --name test ubuntu echo Hello, World!
```

Executing this command will result in an output like this:

```text
√ Job ID: job-7dd12c3c-ae8d-4492-bdb9-99509fda4f8c
√ Name: test
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://test--jane-doe.jobs.neuro-compute.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: succeeded
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
Hello, World!
```

### A simple GPU job

Run a job on GPU in the default Neu.ro environment \(`neuromation/base`\) that checks if CUDA is available in this environment:

```text
> neuro run --preset gpu-k80-small --name test neuromation/base python -c "import torch; print(torch.cuda.is_available());"
```

We used the `gpu-k80-small` preset for this job. To see the full list of presets you can use, run the following command:

```text
> neuro config show
```

### Working with platform storage

Create a new directory `demo` in the root directory of your platform storage:

```text
> neuro mkdir -p storage:demo
```

Run a job that mounts the `demo` directory from platform storage to the `/demo` directory in the job container and creates a file in it:

```text
> neuro run --preset cpu-small --name test --volume storage:demo:/demo:rw ubuntu bash -c "echo Hello >> /demo/hello.txt"
```

Check that the file you have just created is actually on the storage:

```text
> neuro ls storage:demo
```

## Developing on GPU with Jupyter Notebooks

Development in Jupyter Notebooks is a good example of how the Neuro Platform can be used. While you can run a Jupyter Notebooks session in one command through CLI or in one click in the web UI, we recommend project-based development. To simplify the process, we provide a project template which is a part of the Neu.ro Toolbox. This template provides the basic necessary folder structure and integrations with several recommended tools.

### Initializing a project

To initialize a new project from the template, run:

```text
> neuro project init
```

This command will prompt you to enter some info about your project:

```text
project_name [Name of the project]: Neuro Tutorial
project_slug [neuro-tutorial]:
code_directory [modules]:
```

{% hint style="info" %}
Default values are indicated by square brackets **\[ \].** You can use them by pressing **Enter**.
{% endhint %}

To navigate to the project directory, run:

```text
> cd neuro-tutorial
```

### Project structure

The structure of the project's folder will look like this:

```text
neuro-tutorial
├── .neuro/             <- neuro and neuro-flow CLI configuration files
├── config/             <- configuration files for various integrations
├── data/               <- training and testing datasets (we do not keep it under source control)
├── notebooks/          <- Jupyter notebooks
├── modules/            <- source code of models
├── results/            <- training artifacts
├── .gitignore          <- default .gitignore for a Python ML project
├── .neuro.toml         <- autogenerated config file
├── HELP.md             <- autogenerated template reference
├── README.md           <- autogenerated informational file
├── Dockerfile          <- description of base image used for your project
├── apt.txt             <- list of system packages to be installed in the training environment
├── requirements.txt    <- list of Python dependencies to be installed in the training environment
└── setup.cfg           <- linter settings (Python code quality checking)
```

The template contains the `neuro/live.yaml` configuration file for `neuro-flow`. This file guarantees a proper connection between the project structure, the base environment that we provide, and actions with storage and jobs. For example, the `upload` command synchronizes sub-folders on your local machine with sub-folders on the persistent platform storage, and those sub-folders are synchronized with the corresponding sub-folders in job containers.

### Setting up the environment and running Jupyter

To set up the project environment, run:

```text
> neuro-flow build myimage
```

When this command is executed, system packages from `apt.txt` and pip dependencies from `requirements.txt` are installed to the base environment. It supports CUDA by default and contains the most popular ML/AI frameworks such as Tensorflow and Pytorch.

To start a Jupyter Notebooks session on GPU, run:

```text
> neuro-flow run jupyter
```

This command open Jupyter Notebooks interface in your default browser.

Now, when you edit notebooks, they are updated on your platform storage. To download them locally \(for example, to connect them to a version control system\), run:

```text
> neuro-flow download notebooks
```

Don’t forget to terminate your job when you no longer need it \(the files won’t disappear after that\):

```text
> neuro-flow kill jupyter
```

To check how much GPU and CPU hours you have left, run:

```text
> neuro config show-quota
```

