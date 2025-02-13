# Deploying models with MLflow2Seldon

[MLFlow2Seldon deployer](https://github.com/neuro-inc/mlops-k8s-mlflow2seldon) is an integration service for deploying [MLFlow-registered model](https://www.mlflow.org/docs/latest/model-registry.html)s as REST/GRPC API to a Kubernetes cluster using [Seldon-core](https://www.seldon.io/tech/products/core/).

## How it works

This service is running inside of a Kubernetes cluster with deployed Seldon-core. It synchronizes the MLFlow state to Seldon-core within the Kubernetes cluster by continuously fetching MLFlow-server registered models running as platform jobs via MLFlow Python SDK.

For instance, if a version of a MLFlow-registered model gets assigned to the staging/production stage, the corresponding model binary gets deployed from MLFlow into the K8s cluster as **SeldonDeployment** (exposing REST/GRPC APIs). If the stage assignment gets removed/updated - the corresponding SeldonDeployment is changed respectively.

Given that, all interaction with the service is done implicitly via the MLFlow server state. There is no need to execute particular commands/workloads directly against this service.

## Prerequisites

Here's the setup of MLflow and Seldon you need to have before running MLflow2Seldon:

### MLflow

* [ ] is up and running as a [platform job](https://github.com/neuro-actions/mlflow)
* [ ] server version is at least 1.11.0
* [ ] platform SSO is disabled
* [ ] artifact store mounted as a local path on platform storage

### Seldon

* [ ] SeldonDeployment container image ([model wrapper](https://docs.seldon.io/projects/seldon-core/en/stable/python/python\_wrapping\_docker.html)) is stored on the platform registry and on the same cluster with MLFlow
* [ ] **seldon-core-operator** version is at least 1.5.0
* [ ] `kubectl` tool is authenticated to communicate with a Kubernetes cluster on which Seldon is deployed

## Deployment

### Via a CLI command

To deploy a model with MLflow2Seldon through the CLI, run the following command:

```
$ make helm_deploy
```

This command will prompt you to enter some additional information - for example, what is the MLFlow URL, which cluster should be used, etc.&#x20;

### Via environment variables

Set the following environment variables to use MLflow2Seldon:

* `M2S_MLFLOW_HOST` - MLFlow server host name (example: [_https://mlflow--user.jobs.cluster.org.neu.ro_](https://mlflow--user.jobs.cluster.org.neu.ro)).
* `M2S_MLFLOW_STORAGE_ROOT` - artifact root path on the platform storage (example: _storage:myproject/mlruns_).
* `M2S_SELDON_NEURO_DEF_IMAGE` - docker image stored on the platform registry  which will be used to deploy the model (example: _image:myproject/seldon:v1_). You can also configure the service to use another platform image for deployment by tagging the respective registered _model_ (not a _model version_) with the tag named after the value of the `M2S_MLFLOW_DEPLOY_IMG_TAG` chart parameter. For example, with a tag named "_deployment-image_" and the value "_image:myproject/seldon:v2"_.
* `M2S_SRC_NEURO_CLUSTER` - Apolo cluster on which the deployment image, MLflow artifacts, and MLFlow itself are hosted (_demo\_cluster_).

### Via helm chart

Direct use of a helm chart is possible, but it may be less convenient - all info required by the makefile should be passed as chart values.

## Cleanup

When you're done working with MLflow2Seldon and need to clean up the space it was occupying, just run the following command:

```
$ make helm_delete
```

&#x20;This command will delete:

* All resources required for this service created by the helm chart and the service itself
* The Kubernetes namespace in which SeldonDeployments were created (M2S\_SELDON\_DEPLOYMENT\_NS)
