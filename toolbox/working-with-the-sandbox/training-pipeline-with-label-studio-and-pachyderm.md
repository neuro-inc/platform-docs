# Training Pipeline with Label Studio and Pachyderm

You can create an efficient image processing and model training pipeline by using the Neu.ro platform in conjunction with [Label Studio](https://labelstud.io/) and [Pachyderm](https://www.pachyderm.com/) in the sandbox.

To achieve this, you will need to set up a Pachyderm pipeline that will trigger model training or re-training on every new dataset update that affects image labels. In this way, every time you process images through Label Studio, your model will be automatically re-trained.

Once your sandbox environment is set up and you have a running Pachyderm cluster, you will need to create the Pachyderm pipeline:

```text
$ neuro-flow run create_pipeline --param mlflow_storage $MLFLOW_STORAGE --param mlflow_uri $MLFLOW_URI
```

You will then need to download the dataset to platform storage by running

```text
$ neuro-flow run prepare_remote_dataset
```

Select images from the dataset and put them under Pachyderm:

```text
$ neuro-flow run extend_data --param extend_dataset_by <number_of_images>
```

You can now test the pipeline by opening Label Studio in a browser:

```text
$ neuro-flow run label_studio
```

Once the images are processed, Label Studio will automatically close and commit a new dataset version.

This, in turn, will trigger the Pachyderm pipeline and start model training. You can follow this process in the Pachyderm pipeline logs:

```text
pachctl config update context default --pachd-address <Pachyderm server address>
pachctl logs -f -p train
```

