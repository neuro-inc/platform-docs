# Hyperparameter Tuning with NNI

### Introduction

This tutorial demonstrates how to use [NNI](https://github.com/microsoft/nni) (an open-source tool from Microsoft) for Hyperparameter Tuning on Neu.ro. You will create a new Neu.ro project, integrate it with NNI and run multiple tuning workers to speed up the search process.

Before moving forward with the tutorial, make sure you have [Neu.ro CLI](../../first-steps/getting-started.md#installing-cli) and [**cookiecutter**](https://github.com/cookiecutter/cookiecutter) installed.

### Creating a Neu.ro project

To create a new Neu.ro project, run:

```bash
$ cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
cd <project-id>
```

### Populating the Experiment Code and Integrating With Neu.ro

We're going to use this [NNI example code](https://github.com/microsoft/nni/tree/master/examples/trials/mnist-tfv2) with a MNIST dataset. Put the [mnist.py](https://github.com/microsoft/nni/blob/master/examples/trials/mnist-tfv2/mnist.py) file to the `modules` folder and [search\_space.json](https://github.com/microsoft/nni/blob/master/examples/trials/mnist-tfv2/search\_space.json) to the `config` folder.

Then, add the following lines to `requirements.txt`:

```bash
nni==2.0 # Required for Hyper-parameter search
neuro-sdk # Required by Neu.ro NNI integration script
Jinja2>=2.11.2 # Required by Neu.ro NNI integration script
```

We are now ready to build our image:

```bash
$ neuro-flow build myimage
```

While Docker builds our image, we can continue setting up the NNI integration.

Add the following lines _at the end_ of `.neuro/live.yml`:

```bash
  nni_worker:
    image: $[[ images.myimage.ref ]]
    life_span: 1d
    multi: true
    detach: true
    volumes:
      - $[[ volumes.code.ref_ro ]]
      - $[[ volumes.config.ref_ro ]]
      - $[[ volumes.project.ref ]]
    env:
      EXPOSE_SSH: "yes"
    bash: |
      sleep infinity

  nni_master:
    image: $[[ images.myimage.ref ]]
    life_span: 1d
    http_port: 8080
    http_auth: false
    pass_config: true
    detach: true
    browse: true
    volumes:
      - $[[ volumes.code.ref_ro ]]
      - $[[ volumes.config.ref ]]
      - $[[ volumes.project.ref ]]
    bash: |
      cd $[[ volumes.config.mount ]] && python3 prepare-nni-config.py 
      cd $[[ volumes.project.mount ]] && USER=root nnictl create --config $[[ volumes.config.mount ]]/nni-config.yml -f
```

This will add some new jobs to work with NNI.

Finally, put [`nni-config-template.yml`](https://github.com/neuromation/ml-recipe-nni/blob/master/config/nni-config-template.yml) and [`prepare-nni-config.py`](https://github.com/neuromation/ml-recipe-nni/blob/master/config/prepare-nni-config.py) to the `config` folder and [Makefile](https://github.com/neuro-inc/ml-recipe-nni/blob/master/Makefile) to the root folder of your project.

Once `neuro-flow build myimage` completes, your project is ready for running on Neu.ro.

### Running the Tuning Jobs

The only thing left is to run

```bash
make hypertrain
```

This command will:

* Run 3 worker nodes. They can be configured via `N_JOBS` in Makefile and `preset` in `.neuro/live.yaml`parameters respectfully.
* Run the master node with the `cpu-small` preset.
* Auto-generate a NNI configuration file for the master node pointing at the workers.
* Run the training process and automatically open the NNI web interface in your browser.

You can track experiment progress and intermediate results from this web UI. When the workers are done, you can get the final hyperparameter values and download the logs if needed.

![NNI Hyperparameter Tuning GUI](../../.gitbook/assets/screen-shot-2020-05-12-at-12.43.02-pm.png)

Once you're done, you can shut down the workers and the master node by running

```bash
$ neuro-flow kill ALL
```
