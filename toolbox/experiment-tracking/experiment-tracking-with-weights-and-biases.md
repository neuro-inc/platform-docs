# Experiment Tracking with Weights & Biases

### Introduction

In this tutorial, we show how to set up experiment tracking via Weights & Biases on Neuro Platform using Neuro Project Template.

### Creating a Neuro Project

First, make sure that you have the Neu.ro CLI client and [**cookiecutter**](https://github.com/cookiecutter/cookiecutter) installed and configured:

```bash
$ pip install pipx
$ pipx install neuro-all cookiecutter
$ neuro login
```

Then, initialize an empty project:

```bash
$ cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
```

### Connecting Weights & Biases

Now, connect your project with [Weights & Biases](https://www.wandb.com/):

1. [Register your W\&B account](https://app.wandb.ai/login?signup=true)
2. Find your API key (it is also called a token) on [W\&B’s settings page](https://app.wandb.ai/settings)(section “API keys”). It should be a sequence like `cf23df2207d99a74fbe169e3eba035e633b65d94`.
3. Save your API key (token) to a file in your local home directory `~` and protect it by setting appropriate permissions to make W\&B available on Neu.ro platform:

```
export WANDB_SECRET_FILE=wandb-token.txt
echo "cf23df2207d99a74fbe169e3eba035e633b65d94" > ~/$WANDB_SECRET_FILE
chmod 600 ~/$WANDB_SECRET_FILE
```

After that, create a Neu.ro secret:

```
$ neuro secret add wandb-token @~/$WANDB_SECRET_FILE
```

Open `.neuro/live.yaml`, find `remote_debug` section within `jobs` in it and add the following lines at the end of `remote_debug`:

```bash
jobs:
  remote_debug:
    ...
    args:
     ...
     additional_env_vars: '{"WANDB_API_KEY": "secret:wandb-token"}'
```

Now, you can start using W\&B API in your code.

### Testing

Change default preset to `cpu-small` in `.neuro/live.yaml`to avoid consuming GPU for this test:

```bash
defaults:
  preset: cpu-small
```

Run a development job and connect to the job's shell:

```bash
$ neuro-flow run remote_debug
```

In your job's shell, try to use `wandb`:

```bash
wandb status
```

You should see something like the following:

```bash
Logged in? True
Current Settings
{
  "anonymous": "false",
  "base_url": "https://api.wandb.ai",
  "entity": null,
  "git_remote": "origin",
  "ignore_globs": [],
  "project": null,
  "run": "latest",
  "section": "default"
}
```

You can also find more examples on how to use experiment tracking and other features of W\&B in the [official W\&B's examples repo](https://github.com/wandb/examples).

To close the remote terminal session, press `^D` or type `exit`.

Please don't forget to terminate your job when you don't need it anymore:

```bash
$ neuro-flow kill remote_debug
```
