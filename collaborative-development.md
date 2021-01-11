# Collaborative Development

As you already [know](getting-started.md#understanding-core-concepts), the core concepts in Neu.ro are jobs, storage, and environments. You can share a job, a path on storage, or an image on the platform registry with your teammates, granting them permission to read, update, or even remove this entity.

We recommend keeping the project code in the Git repository. In this case, each teammate has a local copy of the repository and may run jobs independently. To set up your project, please follow these steps.

### Initiating a new Neu.ro project

First, you need to create a new project from the Neu.ro project template. Run `neuro project init` and answer several simple questions.

### Pushing the project to a git repository

Then, you need to put this new project into a git repository. Follow the instructions for the Git hosting of your choice \(for example, here are the [instructions for GitHub](https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)\).

### Organizing your data

You have several options for storing your project data in a shared place.

#### Platform storage

You can upload data to your platform storage using the `neuro cp` command in CLI or FileBrowser in Web UI. In this case, you need to explicitly give your teammates access to this data.

For example, this is how you can upload data to the `cifar-10` folder in your storage and share it with Alice:

```text
neuro cp -r local-folder-with-data storage:cifar-10
neuro share storage:cifar-10 alice manage
```

After that, you need to update the `data/remote:` value in the project's `.neuro/live.yaml` file to keep the full URI of your data. This allows your teammates to use this data folder in their copies of the project \(here, `neuro-compute` is the name of our default cluster, and `bob` is your user name on the platform\):

```text
  data:
    remote: storage://neuro-compute/bob/cifar-10
    mount: /project/data
    local: data
```

After that, your data becomes available in the `/data` folder in the local file system of the jobs you and your teammates work with.

#### Buckets

You can use AWS or GCP buckets to store the data outside the Neu.ro platform. In this case, you need to add your access tokens to the project's `config` folder according to [AWS](https://docs.neu.ro/toolbox/accessing-object-storage-in-aws) and [GCP](https://docs.neu.ro/toolbox/accessing-object-storage-in-gcp) guides. Note that Git doesn't track these tokens, so your teammates also have to put their tokens in their local copies of the project .

#### Public resources

Your data may also be available at some public resource that doesn’t require any authentication. In this case, you may either put a copy of this data to the platform storage \(see above\) or download the data to the job container’s local file system on every run \(if the data size is relatively small\).

### Setting up the project and running jobs

Now all your teammates can clone the project and start working on it through their local copies. Here are some steps every teammate should follow independently.

* To set up the working environment, run `neuro-flow build myimage` \(this is a necessary step to perform every time you update pip dependencies in `requirements.txt` or system requirements in `apt.txt`\). 
* To run a Jupyter Notebooks session, run `neuro-flow run jupyter`. Notebooks are saved in the `<project>/notebooks` folder on your platform storage. To download them to the local copy of the project, run `neuro-flow download notebooks`.
* To run training from source code, update `.neuro/live.yaml` for your `train` job and run `neuro-flow run train` for example:

```text
jobs:
    train:
    ...
    bash: |
        python $[[ volumes.code.mount ]]/train.py
```

You can get more information about the Neu.ro project's functionality in the `HELP.md` file in your project folder.

### Sharing running jobs 

You can share any job you run on the platform with your teammates. To get a list of running jobs that are available to you \(i.e., your jobs and the ones shared with you\), run `neuro ps`. 

Each job has two properties that are essential for sharing: ID and name. The ID is a unique identifier, while the name may repeat for different job runs. To get the job ID, you can either take a look at the job list \(`neuro ps`\) or check a particular job's status \(`neuro status my-cool-job`\).

For example, to share the `jupyter-awesome-project` job with an ID of `job-fb835ab1-5285-4360-8ee1-880a8ebf824c` with Alice \(where `awesome-project` is your project's slug\), run:

```text
neuro share job:job-fb835ab1-5285-4360-8ee1-880a8ebf824c alice read
```

This command allows Alice to access this job either via its ID or its full URI which consists of a cluster name, the owner's user name, and the job's name or ID: `job://neuro-compute/bob/jupyter-awesome-project`:

```text
# read the logs
neuro logs job://neuro-compute/bob/jupyter-awesome-project
neuro logs job-fb835ab1-5285-4360-8ee1-880a8ebf824c   

# run the interactive bash session:
neuro exec job://neuro-compute/bob/jupyter-awesome-project bash  
neuro exec job-fb835ab1-5285-4360-8ee1-880a8ebf824c bash   
    
# open web interface in the default web browser:
neuro browse job://neuro-compute/bob/jupyter-awesome-project 
neuro browse job-fb835ab1-5285-4360-8ee1-880a8ebf824c
```

Also, Alice gets access to this job in her [Web UI](https://app.neu.ro/) and can monitor the job's logs or work with it there.

Please note that if someone gets the `write` access to your Jupyter Notebooks job, they can modify the notebooks on your platform storage. Therefore, to update those notebooks in the git repository, you have to download them, commit, and push.

There is also a shortcut for sharing all your jobs \(past, current, and future ones alike\) with your teammates:

```text
neuro share job: alice read
```

### Share Docker images 

Our project contains a [base environment](https://hub.docker.com/r/neuromation/base) we recommend using for most projects. This environment is based on [deepo](https://github.com/ufoym/deepo). It contains recent versions of the most popular ML/DL libraries \(including Tensorflow 2.0 and PyTorch 1.4\). When you run `neuro-flow build myimage`, additional dependencies you state in `requirements.txt` and `apt.txt` are installed in that environment, which is then saved on the platform's Docker registry. In this case, there is no need to share the images with teammates, as they build similar images from the same code base.

In rare cases, though, you may want to use a different image as a base. If that image is public, all you need to do is to update the `images/myimage/ref` variable in the project's`.neuro/live.yaml`file:

```text
images:
  myimage:
    ref: ufoym/deepo
```

If the image is not public, you need to make available to your teammates:

```text
# upload to your registry:
neuro image push project-specific-docker-image

# share with your teammates:
neuro share image:project-specific-docker-image alice read

# update the .neuro/live.yaml file with the full URI of your image:
images:
  myimage:
    ref: image://neuro-compute/bob/project-specific-docker-image
```

Please note that some functionality may be missing in custom Docker images. In particular, you may need to log into AWS and GCP manually from within your jobs. 

