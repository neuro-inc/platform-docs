# FAQ

## How to upload and download Data

You can upload your datasets to the Platform using Apolo CLI. Apolo CLI supports basic file system operations for copying and moving files to and from the platform storage.

From your terminal or command prompt, change to the directory containing your dataset, and run:

```
apolo cp -r data/ storage:data/
```

The URI `storage:data/` indicates that the destination is the platform. In a similar fashion,

```
apolo cp -r storage:data/ data/
```

downloads dataset to your current directory locally.

You can access your dataset from within a container by giving `--volume storage:data/:/var/storage/data/:rw` to `apolo run` as a parameter when starting a new job.

## How to connect to a running job

To work with your dataset from within a container, to troubleshoot a model, or to get shell access to a GPU instance, you can execute a command shell within a running job in interactive mode.

To do so, copy the job id of a running job (you can run `apolo ps` to see the list), and run:

```
apolo exec <job-id or job-name> bash
```

For example,

```
apolo exec training bash
```

This command starts bash within the running job and connects your terminal to it.

## How to run a job in a custom environment

Assuming you have a local Docker image named `helloworld` built on your local machine, you can push it to the Apolo Platform by running:

```
apolo push helloworld
```

After that, you can start the job by running:

```
apolo run image:helloworld
```

## How to kill all running jobs

To kill all jobs that are currently running on your behalf, run the following command:

```
 apolo kill `apolo -q ps -o ME`
```

## How to run two or more commands in a job

Sometimes you want to execute two or three commands in a job without having to connect to it. For example, you may want to change the working directory and to run training. To achieve this, you need to wrap your commands in `”bash -c ‘<commands>’”` call, like this:

```
"bash -c 'cd /project && python mnist/train.py'"
```

## How to get output from a running job

There are two ways to get the output of your running job:

* Run it without the `--detach` option.
* Connect to a running job output with `apolo log <JOB>`, where JOB is either the id or the name of your job.

In some cases, Python caches the output of the scripts, so that you won’t get any output until the job finishes. To overcome this problem, provide `-u` option to `python`, like this:

```
"bash -c 'cd /project && python -u mnist/train.py'"
```

## How to check storage usage&#x20;

To check the usage of storage space, run the following command:

```
$ apolo run -v storage://:/var/storage ubuntu du -h -d 1 /var/storage
```

If you want to check storage space in any specific folder, just provide the folder name to this command in the following way:

```
$ apolo run -v storage:FOLDER_NAME:/var/storage ubuntu du -h -d 1 /var/storage
```
