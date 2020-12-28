---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Collaborative Development

As you already [know](getting-started.md#understanding-core-concepts), the core concepts in Neu.ro are jobs, storage, and environments. You can share a job, a path on storage, or an image on the platform registry with your teammates, granting them permission to read, update, or even remove this entity.

We recommend keeping the project code in the Git repository. In this case, each teammate has a local copy of the repository and may run jobs independently. To set up your project, please follow these steps.

### Initiate the Neu.ro project

First, you need to create a new project from the Neu.ro project template. Run `neuro project init` and answer several simple questions.

### Initiate a git repository and push this project to Git

Then, you need to put this new project into the git repository. Follow the instructions for the Git hosting of your choice \(for example, here are the [instructions for GitHub](https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)\).

### Organize your data

You have several options for storing your project data in a shared place.

#### Platform storage

You can upload data to your platform storage using `neuro cp` command in CLI or FileBrowser in Web UI. In this case, you need to share the data with your teammates explicitly.

For example, this is how you can upload the data at the `cifar-10` folder in your storage and share it with Alice:

```text
neuro cp -r local-folder-with-data storage:cifar-10
neuro share storage:cifar-10 alice manage
```

After that, you need to update the `data/remote:` value in the project's `.neuro/live.yaml` file to keep the full URI of your data. This step allows your teammates to use this data folder in their project copies as well \(here `neuro-public` is the name of our default cluster, and `bob` is your platform user name\):

```text
  data:
    remote: storage://neuro-public/bob/cifar-10
    mount: /project/data
    local: data
```

After that, your data becomes available in the `/data` folder in the local file system of the jobs you and your teammates run.

#### Buckets

You can use AWS or GCP buckets to keep the data outside the Neu.ro platform. In this case, you need to add your access tokens in the project `config` folder according to [AWS](https://docs.neu.ro/toolbox/accessing-object-storage-in-aws) and [GCP](https://docs.neu.ro/toolbox/accessing-object-storage-in-gcp) guides. Note that Git does not track these tokens, so your teammates also have to put their tokens in their local project copies .

#### Public resources

Your data may also be available at some public resource that doesn’t require any authentication. In this case, you may either put a copy of data on the platform storage \(see above\) or download the data to the job container’s local file system on every run \(if the data size is relatively small\).

### Set up the project and run jobs

Now all your teammates can clone the project and start working on it through their local copies. Here are some steps every teammate should make independently.

* To set up the working environment, run `neuro-flow build myimage` \(this is a necessary step to perform every time you update pip dependencies in `requirements.txt` or system requirements in `apt.txt`\). 
* To run a Jupyter Notebooks session, run `neuro-flow run jupyter`. Notebooks are saved in the `<project>/notebooks` folder on your platform storage. To download them in the local copy of the project, run `neuro-flow download notebooks`.
* To run training from source code, update `.neuro/live.yaml` for your `train` job and run `neuro-flow run train` for example:

```text
jobs:
    train:
    ...
    bash: |
        python $[[ volumes.code.mount ]]/train.py
```

You may get more information about the Neu.ro project functionality in the `HELP.md` file in your project folder.

### Share running jobs 

Any job you run on the platform you can share with your teammates. To get a list of running jobs that are available to you \(i.e., yours and the ones shared with you\), run `neuro ps`. 

There are two properties of each job, which are essential for sharing: ID and name. The ID is a unique identifier, while the name may repeat for different job runs. To get the job ID, you may take a look at the job list \(`neuro ps`\) or check out a particular job status \(`neuro status my-cool-job`\).

For example, to share a `jupyter-awesome-project` job with ID `job-fb835ab1-5285-4360-8ee1-880a8ebf824c` with Alice \(where `awesome-project` is a short name of your project\), run:

```text
neuro share job:job-fb835ab1-5285-4360-8ee1-880a8ebf824c alice read
```

This command allows Alice to access this job via its ID or its full URI, which consists of a cluster name, an owner user name, and the job name or ID: `job://neuro-public/bob/jupyter-awesome-project`:

```text
# read the logs
neuro logs job://neuro-public/bob/jupyter-awesome-project
neuro logs job-fb835ab1-5285-4360-8ee1-880a8ebf824c   

# run the interactive bash session:
neuro exec job://neuro-public/bob/jupyter-awesome-project bash  
neuro exec job-fb835ab1-5285-4360-8ee1-880a8ebf824c bash   
    
# open web interface in the default web browser:
neuro browse job://neuro-public/bob/jupyter-awesome-project 
neuro browse job-fb835ab1-5285-4360-8ee1-880a8ebf824c
```

Also, Alice gets access to this job in her [Web UI](https://app.neu.ro/) and can monitor the logs or open the web interface right there.

Please note that if someone gets the `write` access to your Jupyter Notebooks job, they can modify the notebooks on your platform storage. Therefore, to update those notebooks in the git repository, you have to download them, commit, and push.

There is also a shortcut for sharing all your jobs \(past, current, and future ones alike\) with your teammates:

```text
neuro share job: alice read
```

### Share Docker images 

Our project contains a [base environment](https://hub.docker.com/r/neuromation/base) we recommend using for most projects. This environment is based on [deepo](https://github.com/ufoym/deepo). It contains recent versions of the most popular ML/DL libraries \(including Tensorflow 2.0 and PyTorch 1.4\). When you run `neuro-flow build myimage`, additional dependencies you state in `requirements.txt` and `apt.txt` are installed in that environment, which is then saved in your platform Docker registry. In this case, there is no need to share the images between the teammates, as they build similar images from the same code base.

In rare cases, though, you may want to use a specific image as a base one. If that image is public, all you need to do is to update the `images/myimage/ref` variable in the project `.neuro/live.yaml`file:

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
    ref: image://neuro-public/bob/project-specific-docker-image
```

Please note that some functionality may be missing in the custom base images. In particular, you may need to log into AWS and GCP manually from within your jobs. 

