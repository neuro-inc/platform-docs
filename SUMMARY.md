# Table of contents

* [Introduction](README.md)
* [First Steps](first-steps/README.md)
  * [Getting Started](first-steps/getting-started.md)
  * [Training Your First Model](first-steps/training-your-first-model.md)
  * [Running Your Code](first-steps/running-your-code.md)
  * [Collaborative Development](first-steps/collaborative-development.md)
* [FAQ](faq.md)
* [Troubleshooting](troubleshooting.md)
* [References](references/README.md)
  * [Apolo CLI Reference](https://app.gitbook.com/o/-MMLX64i1AQdS3ehf2Kg/s/-MOkWy7dB5MDbkSII8iF/)
  * [Apolo Flow Reference](https://app.gitbook.com/o/-MMLX64i1AQdS3ehf2Kg/s/-MMLOF\_FqiWBMcOdY8cj/)
  * [Apolo Extras Reference](https://docs.apolo.us/neu-ro-extras-reference)
  * [Apolo Actions Reference](https://app.gitbook.com/o/-MMLX64i1AQdS3ehf2Kg/s/-Mb\_NcLuFCRM9bTy2KI0/)
  * [Python SDK Reference](https://apolo-sdk.readthedocs.io)

## Apolo Console <a href="#core" id="core"></a>

* [Getting started](apolo-console/getting-started/README.md)
  * [Sign Up, Login](apolo-console/getting-started/sign-up-login.md)
  * [Organizations](apolo-console/getting-started/organizations.md)
  * [Clusters](apolo-console/getting-started/clusters.md)
  * [Projects](apolo-console/getting-started/projects.md)
* [Apps](apolo-console/apps/README.md)
  * Pre-installed apps
    * [Files](apolo-console/files.md)
    * [Buckets](apolo-console/buckets.md)
    * [Disks](apolo-console/disks.md)
    * [Images](apolo-console/images.md)
    * [Secrets](apolo-console/secrets.md)
    * [Jobs](apolo-console/jobs.md)
    * [Flows](apolo-console/flows.md)
  * Available apps
    * [Terminal](apolo-console/apps/terminal.md)
    * [LLM Inference](apolo-console/apps/llm-inference.md)
    * [PostgreSQL](apolo-console/apps/postgre-sql.md)
    * [Text Embeddings Inference](apolo-console/apps/text-embeddings-inference.md)
    * [Jupyter Notebook](apolo-console/apps/jupyter-notebook.md)
    * [Jupyter Lab](apolo-console/apps/jupyter-lab.md)
    * [VS Code](apolo-console/apps/vs-code.md)
    * [PyCharm Community Edition](apolo-console/apps/py-charm.md)
    * [ML Flow](apolo-console/apps/ml-flow.md)
    * [Apolo Deploy](apolo-console/apps/apolo-deploy.md)
  
  
<!--
* [Platform Overview](core/platform-overview.md)
* [Clusters, Organizations, Projects and Roles](core/clusters-and-roles/README.md)
  * [Clusters](core/clusters-and-roles/clusters-resources.md)
  * [Organizations](core/clusters-and-roles/organizations.md)
  * [Projects](core/clusters-and-roles/projects.md)
  * [Roles](core/clusters-and-roles/roles.md)
* [Platform Storage](core/platform-storage/README.md)
  * [Storage](core/platform-storage/storage.md)
  * [Disks](core/platform-storage/disks.md)
  * [Buckets](core/platform-storage/buckets.md)
* [Working with the Platform](core/working-with-the-platform/README.md)
  * [Flows](core/working-with-the-platform/projects.md)
  * [Environments (Docker images)](core/working-with-the-platform/environments-docker-images.md)
  * [Jobs](core/working-with-the-platform/jobs.md)
  * [Secrets](core/working-with-the-platform/secrets.md)
* [Specific Tasks](core/specific-tasks/README.md)
  * [Accessing a job protected by platform SSO](core/specific-tasks/accessing-a-job-hidden-behind-the-platforms-auth.md)
-->

<!--
## Apolo Apolo Console <a href="#web" id="web"></a>

* [Terminal](web/terminal.md)
* [Working with Jupyter](web/working-with-jupyter/README.md)
  * [Jupyter Notebooks](web/working-with-jupyter/jupyter-notebooks.md)
  * [Jupyter Lab](web/working-with-jupyter/jupyterlab.md)
-->

<!--
## Apolo Toolbox <a href="#toolbox" id="toolbox"></a>

* [Remote Debugging](toolbox/remote-debugging/README.md)
  * [Remote Debugging with PyCharm Professional](toolbox/remote-debugging/remote-debugging-with-pycharm-professional.md)
  * [Remote Debugging with VS Code](toolbox/remote-debugging/remote-debugging-with-vs-code.md)
* [Experiment Tracking](toolbox/experiment-tracking/README.md)
  * [Experiment Tracking with TensorBoard](toolbox/experiment-tracking/experiment-tracking-with-tensorboard.md)
  * [Experiment Tracking with Weights & Biases](toolbox/experiment-tracking/experiment-tracking-with-weights-and-biases.md)
  * [Using MLFlow](toolbox/experiment-tracking/using-mlflow.md)
* [Hyperparameter Tuning](toolbox/hyperparameter-tuning/README.md)
  * [Hyperparameter Tuning with NNI](toolbox/hyperparameter-tuning/using-nni-for-hyper-parameter-tuning.md)
  * [Hyperparameter Tuning with Weights & Biases](toolbox/hyperparameter-tuning/hyperparameter-tuning-with-weights-and-biases.md)
* [Serving and Deploying](toolbox/serving-and-deploying/README.md)
  * [Serving Models With Model Deployment Controller](toolbox/serving-and-deploying/serving-models-with-model-deployment-controller.md)
  * [Deploying with Seldon](toolbox/serving-and-deploying/deploying-with-seldon.md)
  * [Serving Models with TorchServe](toolbox/serving-and-deploying/serving-models-with-torch-serve.md)
* [Working With the Sandbox](toolbox/working-with-the-sandbox/README.md)
  * [Training Pipeline with Label Studio and Pachyderm](toolbox/working-with-the-sandbox/training-pipeline-with-label-studio-and-pachyderm.md)
  * [Testing Models with Locust](toolbox/working-with-the-sandbox/testing-models-with-locust.md)
  * [Running Elastic Horovod Training](toolbox/working-with-the-sandbox/running-elastic-horovod-training.md)
  * [Deploying models with MLflow2Seldon](toolbox/working-with-the-sandbox/deploying-models-with-mlflow2seldon.md)
  * [Running RAPIDS](toolbox/working-with-the-sandbox/running-rapids.md)
  * [Model output explanation with SHAP](toolbox/working-with-the-sandbox/model-output-explanation-with-shap.md)
* [Distributed workloads](toolbox/distributed-workloads/README.md)
  * [Distributed Training in PyTorch](toolbox/distributed-workloads/distributed-training-in-pytorch.md)
  * [Running Apache Spark workloads](toolbox/distributed-workloads/running-apache-spark-workloads.md)
* [CI with GitHub Actions](toolbox/ci-with-github-actions.md)
-->

## ADMINISTRATION

* [Overview and Installation](administration/overview-and-installation/README.md)
  * [Architecture Overview](administration/overview-and-installation/architecture-overview.md)
  * [On-premise Installation Guide](administration/overview-and-installation/on-premise-installation-guide.md)
* [Cluster Management](administration/cluster-management/README.md)
  * [Creating a Cluster](administration/cluster-management/creating-a-cluster.md)
  * [Managing Users and Quotas](administration/cluster-management/managing-users-and-quotas.md)
  * [Managing organizations](administration/cluster-management/managing-organizations.md)
  * [Creating Node Pools](administration/cluster-management/creating-node-pools.md)
  * [Managing Presets](administration/cluster-management/managing-presets.md)
  * [Reports](administration/cluster-management/reports.md)
