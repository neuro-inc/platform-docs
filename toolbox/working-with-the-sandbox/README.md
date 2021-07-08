# Working With the Sandbox

## What is the sandbox?

The sandbox is a custom space on Neu.ro that was set up by our team to provide an out-of the box experience to test the platform's capabilities.

For example, our [OSS dogs sandbox](https://github.com/neuro-inc/mlops-demo-oss-dogs) was created to showcase a model that's able to classify two specific dog breeds from images. 

If you have followed the [Getting Started guide](../../first-steps/getting-started.md) and already have a deployed Pachyderm cluster that's ready to create and run pipelines, you can receive access to the sandbox and start working with it according to its `readme` file without the need to set up integrations with various third-party tools.

## Tool integrations

The Dogs sandbox comes integrated with a few required tools right away:

* [Pachyderm](https://www.pachyderm.com/) - A data layer that provides functionality for facilitating a full machine learning lifecycle. We use it for creating a pipeline that triggers model re-training on dataset updates that affect image labels.
* [Label Studio](https://labelstud.io/) - An open-source data labeling tool. It provides image classification,

  object detection, and semantic segmentation functionality that we need in this sandbox.

* [MLFlow](https://mlflow.org/) - A ML lifecycle management platform with main focus on experimentation, reproducibility, deployment, and a central model registry.
* [SHAP](https://github.com/slundberg/shap) - A tool that employs a game theory approach to the task of explaining outputs of machine learning models.
* [Locust](https://locust.io/) - An open-source load testing tool that can be used to make sure models work as expected.

## How to receive access to the sandbox

The general setup of the sandbox is performed on a per-client basis and includes several Kubernetes-native tools. This implies initial setup of the cluster from our side, so if you're interested in getting your own sandbox, please contact us at `mlops@neu.ro`.

