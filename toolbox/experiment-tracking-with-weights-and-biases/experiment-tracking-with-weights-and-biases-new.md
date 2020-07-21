# Experiment Tracking with Weights & Biases — NEW



### Introduction

In this tutorial, we show how to set up experiment tracking via Weights & Biases on Neuro Platform using Neuro Project Template.

### Creating Neuro Project

To create a new Neuro project, run:

```bash
neuro project init
cd <project-slug>
make setup
```

### Authenticating W&B

Register a [W&B account](https://app.wandb.ai/login) \(note: W&B offers _free limited accounts_ for individual researchers, see [W&B pricing policy](https://www.wandb.com/pricing)\). Then, download your API key on [W&B settings page](https://app.wandb.ai/settings) and save it to a local file in your home directory`~/wandb-token.txt`.

After that, create a new secret for the file:

```text
neuro secret add wandb-key --from-file=~/$WANDB_SECRET_FILE
```

Next, find the following line in `Makefile`:

```bash
SECRETS?=
```

And replace the line with that one:

```bash
SECRETS?="-e WANDB_API_KEY=secret:wandb-key"
```

### Testing

Run a development job and connect to the job's shell:

```bash
export PRESET=cpu-small  # to avoid consuming GPU for this test
make develop
make connect-develop
```

In your job's shell, try to use `wandb`:

```bash
wandb status
```

You should see something like:

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

Please find a real-world example of W&B usage on Neuro Platform in our ML Recipe for Hierarchical Attention. You can also find other examples on how to use experiment tracking and other features of W&B [in official W&B's examples repo](https://github.com/wandb/examples).

To close remote terminal session, press `^D` or type `exit`.

Please don't forget to terminate your job when you don't need it anymore:

```bash
make kill-develop
```

