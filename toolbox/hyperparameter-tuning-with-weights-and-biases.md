---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Hyperparameter Tuning with Weights & Biases

Neu.ro allows you to run model training in parallel with different hyperparameter combinations via integration with [Weights & Biases](https://www.wandb.com/). W&B is an experiment tracking tool for deep learning. The ML engineer only needs to initiate the process: prepare the code for training the model, set up the hyperparameter space, and start the search with just one command. Neu.ro is in charge of the rest.

To see this guide in action, check out our [recipe](https://github.com/neuromation/ml-recipe-hyperparam-wandb) in which we apply hyperparemeter tuning with W&B to an image classification task.

### Creating a Neu.ro Project

The Neu.ro project template contains an integration with Weights and Biases. To create a new project from a template, you need to follow a couple of steps. 

First, [Sign up](https://neu.ro/) and [install the CLI client](https://docs.neu.ro/getting-started#installing-cli).

Then, create a project:

```text
neuro project init
```

This command will then ask you a few simple questions:

```text
project_name \[Neuro Project]: Hyperparameter tuning test
project_slug \[hyperparameter-tuning-test]:
code_directory \[modules]:
```

Press `Enter` if you don't want to change the suggested value.

Then, change the working directory:

```text
cd hyperparameter-tuning-test
```

### Connecting Weights & Biases

Now, connect your project with [Weights & Biases](https://www.wandb.com/):

* [Register your W&B account](https://app.wandb.ai/login?signup=true)
* Find your API key \(also called a _token_\) on [W&B’s settings page](https://app.wandb.ai/settings)\(section “API keys”\). It should be a sequence like `cf23df2207d99a74fbe169e3eba035e633b65d94`.
* Save your API key \(token\) to a file in your local home directory `~`:

```text
export WANDB_SECRET_FILE=wandb-token.txt
echo "cf23df2207d99a74fbe169e3eba035e633b65d94" > ~/$WANDB_SECRET_FILE
```

* Download `hypertrain.yml` from [here](https://github.com/neuro-inc/ml-recipe-hyperparam-wandb/blob/master/.neuro/hypertrain.yml) to `.neuro/hypertrain.yml` in your project's directory.

Feel free to refer to the [W&B documentation](https://docs.wandb.com/library/api/examples) and [W&B example projects](https://github.com/wandb/examples) for instructions on how to use Weights & Biases in your code.

### Using Weights & Biases for Hyperparameter Tuning

If you have completed the previous steps, W&B is ready to use. To run hyperparameter tuning for the model, you need to:

* Define the list of hyperparameters \(in a `config/wandb-sweep.yaml` file\);
* Send the metrics to W&B after each run \(by using the `neuro-flow bake hypertrain` command\).

`.neuro/hypertrain.yml` and `config/wandb-sweep.yaml` have links to `train.py` \(you can look at an example [here](https://github.com/neuromation/ml-recipe-hyperparam-wandb/blob/66545469755b5b2bf74f461f5f6d91ed4d133d26/src/train.py)\). If you want to run `hypertrain` for another script, you can change the `program` property in `config/wandb-sweep.yaml` \(see below\). The script must contain the description of the model and the training loop.

The Python script must also receive parameters with the same names as specified in `config/wandb-sweep.yaml` as arguments of the command line and use them for model training/evaluation. For example, you can use command line parameters such as the [argparse](https://docs.python.org/3/library/argparse.html) Python module.

Here are some additional details:

`train.py` is a file that contains the model training code. It should log the metrics with W&B - here's an example for our case:

```text
wandb.log({'accuracy': 0.9})
```

`config/wandb-sweep.yaml` has the following structure:

```yaml
program: /project/src/train.py
method: grid
metric:
  name: accuracy01/valid
  goal: maximize
parameters:
  lr:
    values: [0.1, 0.01, 0.001]
  optimizer:
    values: ['sgd', 'adam']
  scheduler:
    values: ['const', 'cosine']
```

* Line 1: `/../train.py` is a default path to a file with the model training code.
* Line 2: a method that is used for hyperparameters tuning. For more information, refer to the W&B docs.
* Lines 4, 5: the name of the metric that is supposed to be optimized. The ML engineer can change this metric according to the task.
* Lines 6 -12: hyperparameter settings. The ML engineer should use them in the `train.py` file. Names, values, and ranges are changeable as well.

The name of the file `wandb-sweep.yaml` and the path to it can also be modified in `.neuro/hypertrain.yml` \(look for `WANDB_SWEEPS_FILE=...`within the `start_sweep` task definition\).

### Hyperparameter Tuning

Now that you have set up both Neu.ro and W&B and prepared your training script, it’s time to try hyperparameter tuning. To do this, run the following command:

```text
neuro-flow bake hypertrain --param token "$(cat ~/$WANDB_SECRET_FILE)"
```

This starts jobs that run the `train.py` script \(or whatever name you have chosen for it\) on the platform with different sets of hyperparameters in parallel. By default, just 2 jobs run at the same time. You can change this number by modifying the `id` list within the `worker_...` task definition in `.neuro/hypertrain.yml`:

```yaml
tasks:
...
  - id: worker_$[[ matrix.id ]]
    image: $[[ images.myimage.ref ]]
    preset: gpu-k80-small-p
    needs: [start_sweep]
    strategy:
      matrix:
        id: [1, 2] # <- e.g., replace with [1, 2, 3, 4, 5]
```

To monitor the hyperparameter tuning process, follow the link provided by W&B at the beginning of the process.

If you want to stop the hyperparameter tuning, terminate all related jobs:

```text
neuro-flow kill hypertrain
```

After that, verify that the jobs stopped by running `neuro-flow ps`.

