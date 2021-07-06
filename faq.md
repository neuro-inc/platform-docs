# FAQ

## How to upload and download Data

You can upload your datasets to the Platform using Neuro CLI. Neuro CLI supports basic file system operations for copying and moving files to and from the platform storage.

From your terminal or command prompt, change to the directory containing your dataset, and run:

```text
neuro cp -r data/ storage:data/
```

The URI `storage:data/` indicates that the destination is the platform. In a similar fashion,

```text
neuro cp -r storage:data/ data/
```

downloads dataset to your current directory locally.

You can access your dataset from within a container by giving `--volume storage:data/:/var/storage/data/:rw` to `neuro run` as a parameter when starting a new job.

## How to browse data with Filebrowser

[Filebrowser](https://github.com/filebrowser/filebrowser) is a web-based file management interface. You can use it to view and manage your storage on Neuro Platform. To start the job and open the tool in your default browser, run the following command:

```text
neuro run \
    --name filebrowser \
    --preset cpu-small \
    --http 80 \
    --detach \
    --browse \
    --volume storage::/srv:rw \
    filebrowser/filebrowser
```

Filebrowser requires logging in; the default credentials are `admin`/`admin`. For more complex user setup, please refer to [Filebrowser documentation](https://filebrowser.xyz).

## How to connect to a running job

To work with your dataset from within a container, to troubleshoot a model, or to get shell access to a GPU instance, you can execute a command shell within a running job in interactive mode.

To do so, copy the job id of a running job \(you can run `neuro ps` to see the list\), and run:

```text
neuro exec <job-id or job-name> bash
```

For example,

```text
neuro exec training bash
```

This command starts bash within the running job and connects your terminal to it.

## How to run a job in a custom environment

Assuming you have a local Docker image named `helloworld` built on your local machine, you can push it to the Neuro Platform by running:

```text
neuro push helloworld
```

After that, you can start the job by running:

```text
neuro run image:helloworld
```

## How to kill all running jobs

To kill all jobs that are currently running on your behalf, run the following command:

```text
 neuro kill `neuro -q ps -o <user>`
```

For example,

```text
 neuro kill `neuro -q ps -o mariyadavydova`
```

## How to run two or more commands in a job

Sometimes you want to execute two or three commands in a job without having to connect to it. For example, you may want to change the working directory and to run training. To achieve this, you need to wrap your commands in `”bash -c ‘<commands>’”` call, like this:

```text
"bash -c 'cd /project && python mnist/train.py'"
```

## How to get output from a running job

There are two ways to get the output of your running job:

* Run it without the `--detach` option.
* Connect to a running job output with `neuro log <JOB>`, where JOB is either the id or the name of your job.

In some cases, Python caches the output of the scripts, so that you won’t get any output until the job finishes. To overcome this problem, provide `-u` option to `python`, like this:

```text
"bash -c 'cd /project && python -u mnist/train.py'"
```

## How to access a job hidden behind the platform's Auth

### Generate a service account

Generate a service account via `neuro service-account create --name <sa-name>`. Make sure to store the **Role** and the **Auth token** - you will use them to authenticate your requests.

#### Example:

```text
    $ neuro service-account create --name test
     Id               service-account-b41aa732-4bb5-45e4-94c1-a078ca013255
     Name             test
     Role             janedoe/service-accounts/test
     Owner            janedoe
     Default cluster  neuro-compute
     Created at       now
     
    Full token with cluster and API url embedded (this value can be used as NEURO_PASSED_CONFIG environment variable):
    eyJ0b2tlbi<hidden>SJ9
    
    Just auth token (this value can be passed to neuro config login-with-token):
    eyJhbGciOi<hidden>Np0
    
    Save it to some secure place, you will be unable to retrieve it later!
```

In this case, `janedoe/service-accounts/test` is the needed role and  
`eyJhbGciOi<hidden>Np0` is the needed authentication token.

### Start the required job

Start the job you want to access later.

#### Example:

```text
$ neuro run --http 8080 --name mytestjob python python -m http.server --cgi 8080
```

### Share access to the jab with the service account

Share access to the job with the service account role from step 1 by using the   
`neuro acl grant job:<job-id-or-name> <role-name> read` command.

#### Example:

```text
$ neuro acl grant job:mytestjob janedoe/service-accounts/test read
```

### Use the auth token

Use the token from step 1 to authenticate the request with header via the `"cookie: sat=<token-here>;"` command.

#### Example:

```text
    $ curl https://mytestjob--janedoe.jobs.neuro-compute.org.neu.ro -H "cookie: sat=eyJhbGciOi<hidden>Np0"
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Directory listing for /</title>
    </head>
    <body>
    <h1>Directory listing for /</h1>
    <hr>
    <ul>
    <li><a href=".dockerenv">.dockerenv</a></li>
    <li><a href="bin/">bin/</a></li>
    <li><a href="boot/">boot/</a></li>
    ....
```

