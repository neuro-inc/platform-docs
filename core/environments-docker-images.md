# Environments \(Docker images\)

Neu.ro uses Docker containers to run jobs in isolated environments. To run a container, you need to use Docker images that are templates containing an application and all the dependencies to run that application. Thus, a Docker container is a running instance of a Docker image.

Neu.ro lets you use Docker images either from the public Docker registry \(such as Docker Hub\) or from a platform cluster registry \(also called private repository\). Every cluster has one platform registry, so you cannot reuse images from one cluster in another cluster.

There are several ways to add custom images to the platform registries. These images can be shared with your teammates.

![Managing environments](../.gitbook/assets/environments-2.png)

In these tutorials, we will use the example of a sample project - Nero-Assistant - to understand the available options. Before you can work in an environment, ensure that you have initiated a new project \(see Getting Started guide for details\).

### **How do I run a job in a container from a public registry image?**

You can run jobs from images both on the public Docker registry or a platform registry. You can use the neuro run command to run a job:

`neuro run [OPTIONS] IMAGE [CMD]`

**Parameters**

| Name | Description |
| :--- | :--- |
| IMAGE | The full URI of the image where you want to run the command. |
| \[CMD\] | The commands that you want to pass to the container. |

**Sample output**

```text
> neuro run -n job369 -s cpu-small ubuntu
Job ID: job-1712b7de-0072-48f5-bcc1-f64d3256730e Status: pending
Name: job369
Http URL: https://job369--clarytyllc.jobs.neuro-public.org.neu.ro
Shortcuts:
  neuro status job369     # check job status
  neuro logs job369       # monitor job stdout
  neuro top job369        # display real-time job telemetry
  neuro exec job369 bash  # execute bash shell to the job
  neuro kill job369       # kill job
Status: pending Creating
Status: pending Scheduling
Status: succeeded
Terminal is attached to the remote job, so you receive the job's output.
Use 'Ctrl-C' to detach (it will NOT terminate the job), or restart the
job with `--detach` option.
```

Note: The job name that you provide must contain only lowercase letters, number, or hyphens; and must begin with a letter.

When a job is created, it is added to the queue and its status is set to Pending. Once the job starts running it is assigned a job ID, and the status is set to Running. All current jobs are also listed on the Neu.ro web interface.

For more information, see the [CLI Reference for Run](../references/cli-reference/).

You can view the list of jobs currently running using the command: `neuro ps`. This also lists the job-id of the jobs. You can terminate a job using the command: `neuro kill <job-id>`. 

### **How can I view all images that I have access to?**

Neu.ro lets you create multiple images, and you view information about the images that you own or have access to from the command prompt. To view all images, you must run the command:

`neuro image ls [OPTIONS]` 

**Sample Output**

```text
> neuro image ls
image:neuromation-neuro-tutorial
image:neuromation-nero-assistant
```

You can view the tags for an image by running the command:

`neuro image tags [OPTIONS] IMAGE`

**Sample output**

```text
> neuro image tags image://neuro-public/clarytyllc/neuromation-nero-assistant
image://neuro-public/clarytyllc/neuromation-nero-assistant:v1.5.1
```

Note that you must provide the full URI to view tags for an image. You may want to add tags when you push an image or save an image. For more information, see [Upload a custom image to the platform registry](https://docs.google.com/document/d/1m8UXbxcjruOuBnlTTuRe7GsvjPSudjMcMp4nQaYV020/edit#heading=h.1i45ap465lyc).  


### **How can I upload a custom image to the platform registry?**

Neu.ro provides a base public Docker image based on deepo. You can customize the base image by installing or configuring packages, or updating settings by providing the docker engine instructions. You can pass instructions for image customization using Dockerfiles.   

Neu.ro lets you upload your custom image to the platform that can be then used to run jobs. You can also share this custom image with your co-workers. It is a best practice to add tags to your image for better tracking.  

To upload a custom image, run this command:

`neuro image push [OPTIONS] LOCAL_IMAGE [REMOTE_IMAGE]`  

**Parameters**

| Name | Description |
| :--- | :--- |
| LOCAL\_IMAGE | The local custom image that you want to upload. The image name should not contain “image://”.  |
| REMOTE\_IMAGE | The remote image to which you want to upload. |

**Sample Output**

```text
> neuro push neuromation-nero-assistant image:nero-assistant:v2
Pushing image neuromation-nero-assistant => image://neuro-public/mrsmariyadavydova/nero-assistant:v2
> b7f3b88ae387: Pushed
> e6a8e7191cdf: Pushing [=> ]               2.736MB/93.82MB
> 9dfa40a0da3b: Pushing [===============> ] 1.219MB/3.966MB
```

After pushing the image, run the neuro image ls command to check if the push has worked. If the push was successful, then you should see the local image as well as the platform image.  

```text
> neuro images
image:neuro-public/mrsmariyadavydova/nero-assistant:latest
image:neuromation-neuro-tutorial
image:neuromation-nero-assistant
```

### **How can I save a running job as a custom image?**

There are several ways in which you can create custom images. You can create a custom image out of a running job. Before you save a job, you must know the id or name of the job you want to save.  

You can use the neuro job save command to save a job as a custom image:

`neuro job save [JOB] [IMAGE]`
  
**Parameters**

| Name | Description |
| :--- | :--- |
| JOB | The id or name of the job that you want to save as a custom image. |
| IMAGE | The commands that you want to pass to the container. |

**Sample output**

```text
> neuro job save job363 image:ubuntu-custom
Saving job-16339fe4-9559-4c4c-9437-e6e7d5d0721e -> image://neuro-public/clarytyllc/ubuntu-custom:latest
Creating image image://neuro-public/clarytyllc/ubuntu-custom:latest image from the job container
Image created
Pushing image clarytyllc/ubuntu-custom:latest => image://neuro-public/clarytyllc/ubuntu-custom:latest
8891751e0a17: Pushed
2a19bd70fcd4: Pushed
9e53fd489559: Pushed
7789f1a3d4e9: Pushed
image://neuro-public/clarytyllc/ubuntu-custom:latest
```

We have saved the job **job363** as a custom image **ubuntu-custom**.   

### **How can I use an image from a platform registry?**

Neu.ro lets you work with jobs, environments, and storage. Jobs are run on environments \(or containers\) that are isolated with their own storage, CPU, and memory. You can run jobs both on the public Docker registry or a platform registry. To better manage your resources, you can specify the CPU and memory to be used by the job.  

You can use the neuro run command to run a job:

`neuro run [OPTIONS] IMAGE [CMD]`

**Parameters**

| Name | Description |
| :--- | :--- |
| IMAGE | The full URI of the image where you want to run the command. |
| \[CMD\] | The commands that you want to pass to the container. |

**Sample output**

```text
> neuro run -n job363 -s cpu-small image:ubuntu-patched:v2 echo Hello World
Job ID: job-21beb932-1cdb-4b55-b286-10a99752a9f1 Status: pending
Name: job363
Http URL: https://job363--clarytyllc.jobs.neuro-public.org.neu.ro
Shortcuts:
  neuro status job363     # check job status
  neuro logs job363       # monitor job stdout
  neuro top job363        # display real-time job telemetry
  neuro exec job363 bash  # execute bash shell to the job
  neuro kill job363       # kill job
Status: pending Creating
Status: pending Scheduling
Status: succeeded
Terminal is attached to the remote job, so you receive the job's output.
Use 'Ctrl-C' to detach (it will NOT terminate the job), or restart the
job with `--detach` option.                
Hello World
```

We have run a named job **job363** using a small CPU resource preset on the patched ubuntu image.   


### **How do I download an image from the platform registry?**

Neu.ro lets you pull images from the platform registry. You can specify tags of the images to pull a certain tag; else, the image with the latest tag is pulled.  

To pull an image from the platform, run this command:

`neuro image pull [OPTIONS] REMOTE_IMAGE [LOCAL_IMAGE]`  

**Parameters**

| Name | Description |
| :--- | :--- |
| REMOTE\_IMAGE | The remote image that you want to pull. |
| \[LOCAL\_IMAGE\] | The local custom image that you want to pull to. The image name should not contain “image://”.  |

**Sample Output**

```text
> neuro pull image:ubuntu-patched:v1
> Pulling image image://neuro-public/mrsmariyadavydova/ubuntu-patched:v1 => ubuntu-patched:v1
> d51af753c3d3: Downloading [========>                   ]  4.972MB/28.56MB
> fc878cd0a91c: Download complete
> 6154df8ff988: Download complete
> fee5db0ff82f: Download complete
> 8d6a6e6d0908: Download complete
```

## How do I share custom images with my teammates?

Neu.ro lets you share your custom image with your team without having to upload the image to a platform. You could opt to push your custom image to the public registry if you want to share your custom image with a large audience.  

To share your image, run the command:

`neuro share [OPTIONS] URI USER [read|write|manage]`  

The access you want to give the USER to the shared image:

* Read: User can only view the image and not make any changes.
* Write: Provided user can make changes to the image.
* Manage: User can share the image.

**Sample command**

`neuro share image://neuro-public/clarytyllc/neuromation-nero-assistant mrsmariyadavydova manage`  
