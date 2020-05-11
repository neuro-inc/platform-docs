# Hyperparameter Tuning with Weights and Biases

Neu.ro allows you to run model training in parallel with different hyperparameter combinations via integration with [Weights & Biases](https://www.wandb.com/). W&B is an experiment tracking tool for deep learning. The ML engineer only needs to initiate the process: prepare the code for the model training, set up the hyperparameters space, and start the search with just one command. Neu.ro is in charge of the rest.

To see this guide in action, check out our [recipe](https://github.com/neuromation/ml-recipe-hyperparam-wandb), where we apply hyperparemeter tuning with W&B to image classification task.

### Creating a Neu.ro project

The Neu.ro project template contains an integration with Weights and Biases. To create a new project from a template, you need to follow a couple of steps. 

First, [Sign up](https://neu.ro/) and [install CLI client](https://docs.neu.ro/getting-started#installing-cli)

Then, create a project:

```text
neuro project init
```

This command will then ask you several simple questions:

```text
project_name \[Neuro Project]: Hyperparameter tuning test
project_slug \[hyperparameter-tuning-test]:
code_directory \[modules]:
```

Press `Enter` if you do not want to change a suggested value.

Then, change the working directory:

```text
cd hyperparameter-tuning-test
```

### Connecting Weights & Biases

Now, connect your project with [Weights & Biases](https://www.wandb.com/):

1. [Register your W&B account](https://app.wandb.ai/login?signup=true)
2. Find your API key \(it is also called a token\) on [W&B’s settings page](https://app.wandb.ai/settings)\(section “API keys”\). It should be a sequence like `cf23df2207d99a74fbe169e3eba035e633b65d94`.
3. Save your API key \(token\) to a file in a local directory `./config/` and protect it by setting appropriate permissions to make W&B available on Neu.ro platform:

```text
export WANDB_SECRET_FILE=wandb-token.txt
echo "cf23df2207d99a74fbe169e3eba035e633b65d94" > config/$WANDB_SECRET_FILE
chmod 600 config/$WANDB_SECRET_FILE
```

After that, check that Neu.ro can access and use this file for authentication:

```text
make wandb-check-auth
```

In case of success, this command should return something like:

```text
Weights & Biases will be authenticated via key file:
'/project-path/config/wandb-token.txt'.
```

Now, if you run a `develop`, `train`, `jupyter`, or other jobs \(see `Makefile` for the full list of commands\), Neu.ro authenticates W&B via your API key by running `wandb login` at job’s startup.

Technically, authentication in W&B is being done as follows: when you start any job in an environment derived from the base one, Neu.ro checks if the environment variable `NM_WANDB_TOKEN_PATH` is set and then stores the path to the existing file. Then \(before the job starts\), it runs the command `wandb login $(cat $NM_WANDB_TOKEN_PATH)` to create a connection between W&B and Neu.ro.

Please find instructions on using Weights & Biases in your code in [W&B documentation](https://docs.wandb.com/library/api/examples) and [W&B example projects](https://github.com/wandb/examples).

### Using Weights & Biases for hyperparameter tuning

If you have completed the previous parts of the instruction, W&B is ready to use. To run hyperparameter tuning for the model, you need to:

* define the list of hyperparameters \(in a `wandb-sweep.yaml` file\), and
* send the metrics to W&B after each run \(by using `Makefile` and `make hypertrain` command\).

`Makefile` and `wandb-sweep.yaml` have links to `train.py`. If you want to run `hypertrain` for another script, you can change the `program` property in `wandb-sweep.yaml` \(see below\). The script must contain the description of the model and the training loop.

The Python script must also receive parameters with the same names, as specified in `wandb-sweep.yaml` as arguments of the command line and use them for model training/evaluation. For example, you can use command line parameters such as the [argparse](https://docs.python.org/3/library/argparse.html) Python module.

There are more details below:

1. `train.py` is a file that contains the model training code. It should log the metrics with W&B, for example, in our case:

```text
wandb.log({'accuracy': 0.9})
```

1. `wandb-sweep.yaml` has the following structure:

![wandb-sweep.yaml example](https://blog.neu.ro/assets/wandb-yaml.png)

* Line 1: `/../train.py` is a default path to a file with model training code.
* Line 2: a method that is used for the hyperparameters tuning. For more information, see W&B docs.
* Lines 4, 5: the name of the metric that is supposed to be optimized. The ML engineer can change this metric according to the task.
* Lines 6 -12: hyperparameter settings. The ML engineer should use them in the `train.py` file. Names, values, and ranges are changeable as well.

The name of the file `wandb-sweep.yaml` and the path to it may also be modified in Makefile in the root directory of your project.

### Hyperparameter tuning

Now that you have set up both Neu.ro and W&B and prepared your training script, it’s time to try hyperparameter tuning. For that, you should run the following command:

```text
make hypertrain
```

This starts jobs that run `train.py` script \(or whatever name you have chosen\) on the platform with different sets of hyperparameters in parallel. By default, just 3 jobs run at the same time. You can change this number by modifying `N_JOBS` variable in `Makefile` or adding it at the end of `make` command:

```text
make hypertrain N_JOBS=10
```

To monitor the hyperparameter tuning process, follow the link which W&B provides at the beginning of the process.

If you want to stop the hyperparameter tuning, terminate all related jobs:

```text
make kill-hypertrain-all
```

After that, verify that the jobs stopped \(`make ps`\), and then delete unused sweeps from the local file `.wandb_sweeps`.

