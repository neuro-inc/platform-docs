# Experiment Tracking with Weights & Biases

### Introduction

In this tutorial, we show how to set up experiment tracking via Weights & Biases on the platform using flow template.

### Creating a flow

First, make sure that you have the CLI and cookiecutter installed (refer to [#installing-the-cli](../../first-steps/getting-started.md#installing-the-cli "mention"))

Then, initialize an empty flow:

```bash
$ cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
```

### Connecting Weights & Biases

Now, connect your project with [Weights & Biases](https://www.wandb.com/):

1. [Register your W\&B account](https://app.wandb.ai/login?signup=true)
2. Find your API key (it is also called a token) on [W\&B’s settings page](https://app.wandb.ai/settings)(section “API keys”). It should be a sequence like `cf23df2207d99a74fbe169e3eba035e633b65d94`.

After that, create a secret:

```
$ apolo secret add wandb-token "cf23df2207d99a74fbe169e3eba035e633b65d94"
```

Open `.neuro/live.yaml`, find `remote_debug` section within `jobs` in it and add the following lines at the end of `remote_debug`:

```yaml
jobs:
  remote_debug:
     ...
    env:
        WANDB_API_KEY: secret:wandb-token
```

Now, you can start using W\&B API in your code.

### Testing

Change default preset to `cpu-small` in `.neuro/live.yaml`to avoid consuming GPU for this test:

```yaml
defaults:
  preset: cpu-small
```

Run a development job and connect to the job's shell:

```bash
$ apolo-flow run remote_debug
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
$ apolo-flow kill remote_debug
```
