---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Running Your Code

Oftentimes you don't start a project from scratch. Instead of that you use someones or your own old code as a baseline and develop your solution on top of it. This guide demonstrates how to take an existing code base, convert it into Neu.ro project, and start developing on the platform.

### Prerequisites

1. Make sure that you have the Neuro CLI [installed](getting-started.md#installing-cli) and logged in.
2. Install the `neuro-flow` package:

```bash
pip install -U neuro-flow
```

### Configuration

As an example we'll use the GitHub [repo](https://github.com/songyouwei/ABSA-PyTorch) that contains PyTorch implementations for Aspect-Based Sentiment Analysis models \(see [Attentional Encoder Network for Targeted Sentiment Classification](https://paperswithcode.com/paper/attentional-encoder-network-for-targeted) for more details\). 

First, let's clone the repo and navigate to the created folder:

```bash
git clone git@github.com:songyouwei/ABSA-PyTorch.git
cd ABSA-PyTorch
```

Now, we need to add a couple of files:

* `Dockerfile` contains a very basic Docker image configuration. We need this file to build a custom Docker image which is based on `pytorch/pytorch` public images and contains this repo requirements \(which are gracefully listed by the repo mainteiner in `requirements.txt`\):

{% code title="Dockerfile" %}
```bash
FROM pytorch/pytorch
COPY . /cfg
RUN pip install --progress-bar=off -U --no-cache-dir -r /cfg/requirements.txt
```
{% endcode %}

* `.neuro/live.yml` contains minimal configuration allowing us to run this repo scripts right on the platform through handy short commands:

{% code title=".neuro/live.yml" %}
```yaml
kind: live
title: Sentiment Analysis Training
id: absa

volumes:
  project:
    remote: storage:${{ flow.id }}
    mount: /project
    local: .

images:
  pytorch:
    ref: image:${{ flow.id }}:v1.0
    dockerfile: ${{ flow.workspace }}/Dockerfile
    context: ${{ flow.workspace }}

jobs:
  train:
    image: ${{ images.pytorch.ref }}
    preset: gpu-small
    volumes:
      - ${{ volumes.project.ref_rw }}
    bash: |
        cd ${{ volumes.project.mount }}
        python train.py --model_name bert_spc --dataset restaurant
```
{% endcode %}

Here is a brief explanation of this config:

* `volumes` section contains declarations of connections between your computer file system and the platform storage; here we state that we want the entire project folder to be uploaded to storage at `storage:absa` folder and be mounted inside jobs `/project`;
* `images` section contains declarations of Docker images created in this project; here we declare our image which is decribed in `Dockerfile` above;
* `jobs` section is the one where action happens; here we declare a `train` job which runs our training script with a couple of parameters.

### Running code

Now it's time to run several command that set up the project environment and run training.

* Fisrt, create volumes and upload project to platform storage:

```text
neuro-flow mkvolumes
neuro-flow upload ALL
```

* Then, build an image:

```text
neuro-flow build pytorch
```

* Finally, run training:

```text
neuro-flow run train
```

Please run `neuro-flow --help` to get more information about available commands. 
