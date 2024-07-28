# Serving Models with TorchServe

### Introduction

This tutorial demonstrates how to use [TorchServe](https://pytorch.org/serve/) (a flexible tool for serving PyTorch models) for serving models on Apolo.&#x20;

Before moving forward with the tutorial, make sure you have platform CLI installed.

### Preparing Local Files

First, clone the [TorchServe repository](https://github.com/pytorch/serve):

```
$ git clone git@github.com:pytorch/serve.git
```

You can also use the following version of this command:

```
$ git clone https://github.com/pytorch/serve.git
```

Then, copy the model you would like to serve to your local repository. In this example, we will use [DenseNet](https://pytorch.org/hub/pytorch\_vision\_densenet/):

```
$ curl -o serve/examples/image_classifier/densenet161-8d451a50.pth https://download.pytorch.org/models/densenet161-8d451a50.pth
```

You can now copy the files from your local repository to the platform storage:

```
$ apolo cp -r serve storage:
```

### Serving the Model

As you have all necessary files on the platform storage, you can mount them as a volume to your jobs. Run the following command:

```
$ apolo run --name serve --volume storage:serve:/home/serve:rw --preset gpu-small --http 8080 --no-http-auth --detach pytorch/torchserve:0.1.1-cuda10.1-cudnn7-runtime
```

Now, run a bash terminal from within this job:

```
$ apolo exec serve bash
```

Create the `model-store` folder:

```
$ mkdir /home/serve/model-store
```

Run [Torch Model Archiver](https://pypi.org/project/torch-model-archiver/) on your model:

```
$ torch-model-archiver --model-name densenet161 --version 1.0 --model-file /home/serve/examples/image_classifier/densenet_161/model.py --serialized-file /home/serve/examples/image_classifier/densenet161-8d451a50.pth --export-path /home/serve/model-store --extra-files /home/serve/examples/image_classifier/index_to_name.json --handler image_classifier
```

Before serving the model, run:

```
$ torchserve --stop
```

Now, you can serve your model. Here are the commands you can use for `densenet161`:

```
$ torchserve --start --ncs --model-store /home/serve/model-store --models densenet161.mar
  curl -O https://raw.githubusercontent.com/pytorch/serve/master/docs/images/kitten_small.jpg
  curl -v https://serve--your-user-name.jobs.default.org.neu.ro/predictions/densenet161 -T kitten_small.jpg
```

### Accessing the Management API

To access the serving job's management API, you would first need to port-forward the serving job:

```
$ apolo port-forward serve 8081:8081
```

After that, you can copy the `densenet161` folder to your local machine:

```
$ curl http://localhost:8081/models/densenet161
```

