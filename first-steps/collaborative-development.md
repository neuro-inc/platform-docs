# Collaborative Development

## Introduction

As you already [know](getting-started.md#understanding-core-concepts), the core concepts in Neu.ro are _jobs_, _storage_, and _environments_.

You can share a job, a path on the storage, or an image on the platform registry with your teammates, granting them permission to read, update, or even remove this entity.

We recommend keeping the neuro-flow's flow code in a Git repository. In this case, each teammate will have a local copy of the repository and may run jobs independently. To set up your flow, please follow these steps.

## Initiating a new flow

First, you will need to create a new flow from the template.&#x20;

To do this, install the [**cookiecutter**](https://github.com/cookiecutter/cookiecutter) package and initialize **cookiecutter-neuro-project**:

```
$ pipx install cookiecutter
$ cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
```

The latter command will prompt you to enter some information about the flow and then create it based on your responses.

## Pushing the flow to a Git repository

Then, you need to put this new flow into a Git repository. Just follow the instructions for the Git hosting of your choice (for example, here are the [instructions for GitHub](https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)).

## Organizing your data

You have a few options for storing your flow data in a shared space.

### Platform storage

You can upload data to your platform storage both through the CLI and through the Web UI.&#x20;

{% tabs %}
{% tab title="CLI" %}
To upload data through the CLI, use the `neuro cp` command. For example:

```
$ neuro cp -r <local-folder-with-data> storage:cifar-10
```

This will upload data from your local folder to the `cifar-10` folder on your platform storage.
{% endtab %}

{% tab title="Web UI" %}
To upload data through the Web UI, you will first need to open the Filebrowser. To do this, click **RUN A JOB** in the **Storage browser** widget:

![](<../.gitbook/assets/image (254).png>)

After that, click **RUN** in the new window:\


![](<../.gitbook/assets/image (245).png>)

Once the Filebrowser is opened, navigate to the folder you want to upload your data to and click the **Upload** icon:

![](<../.gitbook/assets/zobrazhennya (29).png>)

Then, select the files or folder you want to upload:

![](<../.gitbook/assets/zobrazhennya (34).png>)
{% endtab %}
{% endtabs %}

After you have your files uploaded to the platform storage, you can share them with your teammates. Sharing is implemented differently in the CLI and the Web UI.

{% tabs %}
{% tab title="CLI" %}
You can give permanent access to folders and files through the CLI with the help of the `neuro share` command.&#x20;

```
$ neuro share storage:cifar-10 alice manage
```

This will share the `cifar-10` storage folder with Alice and give her `manage`-level access to it (this means she will be able to read, change, and delete files in this folder).

After that, you need to update the `data/remote:` value in the flow's `.neuro/live.yaml` file to keep the full URI of your data. This allows your teammates to use this data folder in their copies of the project (here, `default` is the name of our default cluster, and `bob` is your username on the platform):

```
  data:
    remote: storage://default/bob/cifar-10
    mount: /project/data
    local: data
```

After that, your data becomes available in the `/data` folder in the local file system of the jobs you and your teammates work with.
{% endtab %}

{% tab title="Web UI" %}
Sharing folders and files through the Filebrowser gives temporary access to them through the Web UI to any user with a link to them.

Select the files and/or folders you want to share in the Filebrowser and click the **Share** icon:

![](<../.gitbook/assets/image (99).png>)

You can then create permanent and temporary access links for the selected items. To create a permanent link, click **Get Permanent Link**:

![](<../.gitbook/assets/image (97).png>)

To create a temporary link, specify the required time period and click the **Create** icon:

![](<../.gitbook/assets/image (100).png>)
{% endtab %}
{% endtabs %}

### Buckets

You can use AWS or GCP buckets to store the data outside the Neu.ro platform. In this case, you need to add your access tokens to the flow's `config` folder according to [AWS](https://docs.neu.ro/toolbox/accessing-object-storage-in-aws) and [GCP](https://docs.neu.ro/toolbox/accessing-object-storage-in-gcp) guides. Note that Git doesn't track these tokens, so your teammates also have to put their tokens in their local copies of the flow.

### Public resources

Your data may also be available at some public resource that doesn’t require any authentication. In this case, you may either put a copy of this data to the platform storage (see above) or download the data to the job container’s local file system on every run (if the data size is relatively small).

## Setting up the flow and running jobs

Now all your teammates can clone the flow configuration and start working on it in their local copies. Here are some steps every teammate should follow independently.

* To set up the working environment, run `neuro-flow build train` (this is a necessary step to perform every time you update pip dependencies in `requirements.txt` or system requirements in `apt.txt`).&#x20;
* To run a Jupyter Notebooks session, run `neuro-flow run jupyter`. Notebooks are saved in the `<flow>/notebooks` folder on your platform storage. To download them to the local copy of the project, run `neuro-flow download notebooks`.
* To run training from source code, update `.neuro/live.yaml` for your `train` job and run `neuro-flow run train`. For example:

```
jobs:
    train:
    ...
    bash: |
        python $[[ volumes.code.mount ]]/train.py
```

You can get more information about the Neu.ro flow's functionality in the `HELP.md` file in your flow folder.

## Sharing running jobs&#x20;

You can share any job you run on the platform with your teammates.&#x20;

To do that, you will need to know the ID or the name of the job you want to share. \
The ID is a job's unique identifier, while the name may repeat for different job runs.

### Viewing job IDs and names

You can view the IDs and names of currently running jobs available to you both in the CLI and the Web UI.

{% tabs %}
{% tab title="CLI" %}
To view the list of currently running jobs, run `neuro ps`.&#x20;

You can also check a particular job's status `neuro status <my-cool-job>`.
{% endtab %}

{% tab title="Web UI" %}
You can view the IDs and names of all currently running jobs in the left part of the **Jobs** section. Make sure the job filter is set to **Running**.

Clicking the job ID will open the **Job Details** window.

![](<../.gitbook/assets/image (216).png>)
{% endtab %}
{% endtabs %}

### Sharing jobs

{% tabs %}
{% tab title="CLI" %}
To share the `jupyter-awesome-project` job with an ID of `job-fb835ab1-5285-4360-8ee1-880a8ebf824c` with Alice (where `awesome-project` is your project's slug), run:

```
$ neuro share job:job-fb835ab1-5285-4360-8ee1-880a8ebf824c alice read
```

You can also share jobs using their names:

```
$ neuro share job:jupyter-awesome-project alice read
```

However, keep in mind that different runs of the same job can have the same name.
{% endtab %}

{% tab title="Web UI" %}
To share a job, click **Share** in the drop-down list to its right:

![](<../.gitbook/assets/image (212).png>)

Next, enter the name of the user you want to share the job with and the access level. When this is done, click **SHARE**:

![](<../.gitbook/assets/zobrazhennya (30).png>)
{% endtab %}
{% endtabs %}

This allows Alice to access this job either via its ID or its full URI. The URI consists of a cluster name, the owner's user name, and the job's name or ID: `job://default/bob/jupyter-awesome-project`.

```
# read the logs
neuro logs job://default/bob/jupyter-awesome-project
neuro logs job-fb835ab1-5285-4360-8ee1-880a8ebf824c   

# run the interactive bash session:
neuro exec job://default/bob/jupyter-awesome-project bash  
neuro exec job-fb835ab1-5285-4360-8ee1-880a8ebf824c bash   
    
# open web interface in the default web browser:
neuro browse job://default/bob/jupyter-awesome-project 
neuro browse job-fb835ab1-5285-4360-8ee1-880a8ebf824c
```

Also, Alice gets access to this job in her [Web UI](https://app.neu.ro/) and can monitor the job's logs or work with it there.

Please note that, if someone gets `write`-level access to your Jupyter Notebooks job, they can modify the notebooks on your platform storage. Therefore, to update those notebooks in the Git repository, you have to download them, commit, and push.

You can instantly share a new job by adding `--share <username>` when running it.

There is also a shortcut for sharing all your jobs (past, current, and future ones alike) with your teammates in the CLI:

```
$ neuro share job: alice read
```

## Sharing Docker images&#x20;

Our project contains a [base environment](https://hub.docker.com/r/neuromation/base) we recommend using for most projects. This environment is based on [deepo](https://github.com/ufoym/deepo). It contains recent versions of the most popular ML/DL libraries (including Tensorflow 2.0 and PyTorch 1.4). When you run `neuro-flow build train`, additional dependencies you state in `requirements.txt` and `apt.txt` are installed in that environment, which is then saved on the platform's Docker registry. In this case, there is no need to share the images with teammates, as they build similar images from the same code base.

In rare cases, though, you may want to use a different image as a base. If that image is public, all you need to do is to update the `images/train/ref` variable in the project's`.neuro/live.yaml`file:

```
images:
  train:
    ref: ufoym/deepo
```

If the image is not public, you need to make it available to your teammates:

```
# upload to your registry:
$ neuro image push project-specific-docker-image

# share with your teammates:
$ neuro share image:project-specific-docker-image alice read

# update the .neuro/live.yaml file with the full URI of your image:
images:
  myimage:
    ref: image://default/bob/project-specific-docker-image
```

Please note that some functionality may be missing in custom Docker images. In particular, you may need to log into AWS and GCP manually from within your jobs.&#x20;
