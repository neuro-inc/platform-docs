---
description: Custom-built app for no-code model inference server deployment on the platform
---

# Serving Models With Model Deployment Controller

### How it works

Our Model Deployment Controller is a web application, that connects to your MLFlow instance, retrieves the list of Registered Models (in Staging or Production stages) and enables you to create an MLFlow Serving (or Triton - in case your model was saved in ONNX format) job, which will serve the chosen model. It is possible to select the image, which you want to use for serving, and we provide a list of sensible defaults, curated by us. Additionally, you can specify whether the model endpoints need to be protected by the platform auth.

### Prerequisites

#### MLFlow

You need to have a running MLFlow instance with `--serve-artifacts` flag enabled. If this instance was launched via the Dashboard, you will be able to automatically connect to this instance by injecting the `MLFLOW_TRACKING_URI` into the model deployment job. Otherwise, you would need to pass it manually (e.g. by mounting a secret into env variable from Dashboard or passing the appropriate value if running the deployment app job via Neuro CLI).

Additionally, in case your MLFlow instance is protected by auth, you would need to pass `MLFLOW_TRACKING_TOKEN` to the deployment app as well.

> Note: currently only `mlflow>=2.0` instances are actively supported

### Starting the Model Deployment Controller

Model deployment web app can be launched either via Dashboard or via CLI.

#### Via Dashboard

On the Dashboard page, select **Model Deployment Controller** and click on **RUN A JOB** button. In the corresponding dialogue you can choose the preset you would like to use to run the deployment controller (this is not the preset that will be used for model serving). If you've previously launched an instance of MLFlow via Dashboard, you will see a notice regarding the integration of the URI of this instance into the Deployment Controller via the env variable. Otherwise, you need to pass the URI of your instance as `MLFLOW_TRACKING_URI` env variable to the Controller (e.g. via secret).

> If your MLFlow instance uses authentication, you might need to supply `MLFLOW_TRACKING_TOKEN` as well.

> Notice that we automatically mount the folder from your storage, which will be used as model repository in case you want to deploy to Triton Inference Server.

<figure><img src="../../.gitbook/assets/image (35) (1).png" alt="Model Deployment Controller Dialog"><figcaption><p>Model Deployment Controller pop-up dialogue</p></figcaption></figure>

After pressing **RUN** you will be redirected to the newly started Model Deployment Controller.

#### Via CLI

In case you need more flexibility, you can resort to running the Deployment Controller via `neuro-cli`. Below is an example of a command, that will launch the equivalent of the job, started via Dashboard:

```
neuro run --preset cpu-small \
  --http-port 8501 \
  --volume storage:global/triton_model_repo:/tmp/triton_model_repo:rw \
  --env TRITON_MODEL_REPO=/tmp/triton_model_repo \
  --env MLFLOW_TRACKING_URI=<YOUR_MLFLOW_INSTANCE_URI> \
  --env TRITON_MODEL_REPO_STORAGE=storage:global/triton_model_repo \
  --life-span 1d \
  --pass-config \
  ghcr.io/neuro-inc/job-deploy-app:pipelines
```

### Model Deployment

<figure><img src="../../.gitbook/assets/image (25) (1).png" alt=""><figcaption><p>Deployment Controller UI</p></figcaption></figure>

Use the web app to select the registered model from the MLFlow instance (it must be in Staging or Production), select server type (MLFlow or Triton), resource preset, image name, and tag (we provide recommended defaults, but you can also choose any of your images) and whether to require platform auth for server access.

<figure><img src="../../.gitbook/assets/image (13) (1).png" alt=""><figcaption><p>UI for deploying to MLFlow Server</p></figcaption></figure>

If you oped for Triton server deployment, you will also be able to select an already running Triton instance (if it was previously used to deploy models) or create a new one otherwise.

<figure><img src="../../.gitbook/assets/image (21) (1).png" alt=""><figcaption><p>UI for deploying to Triton Inference Server</p></figcaption></figure>

Clicking on Deploy will start the deployment process and you'll be able to see your model in Deployed models tab.

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>List of inference servers</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (15) (1).png" alt=""><figcaption><p>List of deployed models</p></figcaption></figure>

### Examples

[This Jupyter Notebook](https://github.com/neuro-inc/mlops-job-deploy-app/blob/5f5cc948fdb7931afaac8b856941cac3a4c54de5/examples/model\_training\_ans\_serving\_tutorial.ipynb) provides and example of training models and exporting them to MLFlow, including ONNX format for Triton Inference.
