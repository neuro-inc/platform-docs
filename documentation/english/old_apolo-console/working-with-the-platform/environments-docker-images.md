# Environments (Docker images)

Apolo uses Docker containers to run jobs in isolated environments. To run a container, you need to use Docker images that are templates containing an application and all the dependencies to run that application. Thus, a Docker container is a running instance of a Docker image.

Apolo lets you use Docker images either from the public Docker registry (such as Docker Hub) or from a platform's cluster registry (also called a private repository). Every cluster has one platform registry, reusing images from one cluster in another cluster requires copying this image using `apolo-extras`.

In these tutorials, we will use a sample project - Nero-Assistant - to understand the available options. Before you can work in an environment, make sure that you have initialized a new flow (see the [Getting Started](../../first-steps/getting-started.md) guide for details).

### **How do I run a job using a container image from a public registry?**

You can run jobs from images both on the public Dockerhub registry and on the platform registry. You can use the `apolo run` command to run a job:

`apolo run [OPTIONS] IMAGE -- [CMD]`

**Parameters**

| Name   | Description                                                     |
| ------ | --------------------------------------------------------------- |
| IMAGE  | The full URI of the image on which you want to run the command. |
| \[CMD] | The commands that you want to pass to the container.            |

**Sample output**

```
> apolo run -n job369 -s cpu-small ubuntu
√ Job ID: job-2610359c-58b1-4bfc-af69-52ee6e330b1f
√ Name: job369
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://job369--jane-doe.jobs.default.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: running
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
```

Note: The job name that you provide must contain only lowercase letters, number, or hyphens; and must begin with a letter.

When a job is created, it is added to the queue and its status is set to Pending. Once the job starts running, a job ID is assigned to it, and its status is set to Running. All current jobs are also listed on the Apolo web interface.

For more information, see the CLI's [run](https://app.gitbook.com/s/-MOkWy7dB5MDbkSII8iF/commands/job#run "mention") command reference.

You can view the list of jobs currently running using the command: `apolo ps`. This also lists the ID of the jobs. You can terminate a job using the command: `apolo kill <job ID>`.

### **How can I view all images that I have access to?**

Apolo lets you create multiple images, and you can view information about the images that you own or have access to from the command prompt. To view all images, you must run the following command:

`apolo image ls [OPTIONS]`

**Sample Output**

```
> apolo image ls
image:apolo-tutorial
image:apolo-assistant
```

You can view the tags for an image by running the command:

`apolo image tags [OPTIONS] IMAGE`

**Sample output**

```
> apolo image tags image://default/org/clarytyllc/apolo-assistant
image://default/org/clarytyllc/apolo-assistant:v1.5.1
```

Note that you must provide the full URI to view tags for an image. You may want to add tags when you push an image or save an image. For more information, see Apolo CLI image [tags](https://app.gitbook.com/s/-MOkWy7dB5MDbkSII8iF/commands/image#tags "mention")command reference.

### **How can I upload a custom image to the platform registry?**

Apolo provides a base public Docker image based on deepo. You can customize the base image by installing or configuring packages, or updating settings by providing the docker engine instructions. You can pass instructions for image customization using Dockerfile.

Apolo lets you upload your custom images to the platform and then use them to run jobs. You can also share these custom images with your co-workers. It's a best practice to add tags to your image for better tracking.

To upload a custom image, run the following command:

`apolo image push [OPTIONS] LOCAL_IMAGE [REMOTE_IMAGE]`

**Parameters**

| Name          | Description                                                                             |
| ------------- | --------------------------------------------------------------------------------------- |
| LOCAL\_IMAGE  | The local container image that you want to upload.                                      |
| REMOTE\_IMAGE | The remote image to which you want to upload. The image name should start with “image:” |

**Sample Output**

```
> apolo push neuromation-nero-assistant image:nero-assistant:v2
Pushing image neuromation-nero-assistant => image://default/org/user/nero-assistant:v2
> b7f3b88ae387: Pushed
> e6a8e7191cdf: Pushing [=> ]               2.736MB/93.82MB
> 9dfa40a0da3b: Pushing [===============> ] 1.219MB/3.966MB
```

After pushing the image, run the `apolo image ls` command to check if the push has worked. If the push was successful, you should see the local image as well as the platform image.

```
> apolo images
image:nero-assistant:latest
image:neuromation-neuro-tutorial
image:neuromation-nero-assistant
```

### **How can I save a running job as a custom image?**

There are several ways in which you can create custom images. One of them is creating a custom image from a running job. Before you save a job, you must know its ID or name.

You can use the `apolo job save` command to save a job as a custom image:

`apolo job save [JOB] [IMAGE]`

**Parameters**

| Name  | Description                                                        |
| ----- | ------------------------------------------------------------------ |
| JOB   | The ID or name of the job that you want to save as a custom image. |
| IMAGE | The commands that you want to pass to the container.               |

**Sample output**

```
> apolo job save job363 image:ubuntu-custom
Saving job-16339fe4-9559-4c4c-9437-e6e7d5d0721e -> image://default/clarytyllc/ubuntu-custom:latest
Creating image image://default/clarytyllc/ubuntu-custom:latest image from the job container
Image created
Pushing image clarytyllc/ubuntu-custom:latest => image://default/clarytyllc/ubuntu-custom:latest
8891751e0a17: Pushed
2a19bd70fcd4: Pushed
9e53fd489559: Pushed
7789f1a3d4e9: Pushed
image://default/clarytyllc/ubuntu-custom:latest
```

We have saved the job **job363** as a custom image **ubuntu-custom**.

### **How can I use an image from a platform registry?**

Apolo lets you work with jobs, environments, and storage. Jobs are run on environments (or containers) that are isolated with their own storage, CPU, and memory. You can run jobs both on the public Docker registry or a platform registry. To better manage your resources, you can specify the CPU and the amount of memory the job will use.

You can use the `apolo run` command to run a job:

`apolo run [OPTIONS] IMAGE -- [CMD]`

**Parameters**

| Name   | Description                                                     |
| ------ | --------------------------------------------------------------- |
| IMAGE  | The full URI of the image on which you want to run the command. |
| \[CMD] | The commands that you want to pass to the container.            |

**Sample output**

```
> apolo run -n job363 -s cpu-small image:ubuntu-patched:v2 -- echo Hello World
Job ID: job-21beb932-1cdb-4b55-b286-10a99752a9f1 Status: pending
Name: job363
Http URL: https://job363--clarytyllc.jobs.default.org.neu.ro
Status: pending Creating
Status: pending Scheduling
Status: succeeded
Terminal is attached to the remote job, so you receive the job's output.
Use 'Ctrl-C' to detach (it will NOT terminate the job), or restart the
job with `--detach` option.                
Hello World
```

We have run a named job **job363** using a small CPU resource preset on the patched ubuntu image.

### **How can I download an image from the platform registry?**

Apolo lets you pull images from the platform registry. You can specify tags of the images to pull a certain tag; else, the image with the latest tag is pulled.

To pull an image from the platform, run this command:

`apolo image pull [OPTIONS] REMOTE_IMAGE [LOCAL_IMAGE]`

**Parameters**

| Name            | Description                                      |
| --------------- | ------------------------------------------------ |
| REMOTE\_IMAGE   | The remote image that you want to pull.          |
| \[LOCAL\_IMAGE] | The local custom image that you want to pull to. |

**Sample Output**

```
> apolo pull image:ubuntu-patched:v1
> Pulling image image://default/org/user/ubuntu-patched:v1 => ubuntu-patched:v1
> d51af753c3d3: Downloading [========>                   ]  4.972MB/28.56MB
> fc878cd0a91c: Download complete
> 6154df8ff988: Download complete
> fee5db0ff82f: Download complete
> 8d6a6e6d0908: Download complete
```

### How can I share custom images with my teammates?

First of all, refer to [projects.md](../clusters-and-roles/projects.md "mention")description, inviting teammates into projects simplifies collaboration. However, of a ad-hock share is required, proceed reading.

Apolo lets you share your custom image with your team without having to upload the image to a platform. You could opt to push your custom image to the public registry if you want to share your container image with a large audience.

To share your image, run the command:

`apolo share [OPTIONS] URI USER [read|write|manage]`

The permissions you may give the user for the shared image:

* Read: The user can only pull the image or run job using it and can't make any changes.
* Write: Provided user can make changes to the image or even remove it.
* Manage: The user can share the image.

**Sample command**

```
$ apolo share image://default/org/usr/neuromation-nero-assistant otheruser manage
```

### Operations with images from other clusters

All operations that were described earlier can be performed with images from other clusters.&#x20;

For example, to push a local `neuromation-nero-assistant` image to the platform registry on the `<cluster-name>` cluster into `<proj>` project that is within `<org>` organization hit:

```
$ apolo push neuromation-nero-assistant image:/<cluster-name>/<org>/<proj>/nero-assistant:v2
```
