# Jobs

Job is the simplest workload unit on platform. You can run it in a given environment (container image) on a given resource preset with storage paths attached. Jobs are the building blocks of your flows and should be planned carefully for optimal use of the resources.

Before you start a job, you must decide:

* The Docker image to use to run the job. Note that the job terminates if the Docker container fails unless a specific restart policy is used for the job.
* The preset - a combination of CPU, GPU, and memory resources to use.

In complex flows, you have multiple jobs running with different preset resources that are best suitable for these specific jobs.

### What are presets?

Apolo lets you run a job in an environment on a given preset with several parts of the storage attached. A preset here is a combination of CPU, GPU, and memory resources allocated.

You must decide and set the amount of CPU, GPU, or memory resources you want to use for a job. By default, the first preset in a list of presets displayed in `apolo config show` is used.

For example, we are using the **cpu-small** preset in the following command.

**Sample command:**

```
$ apolo run --preset cpu-small --name test ubuntu -- echo Hello, World! 
√ Job ID: job-aba12927-08f0-402c-8ed0-0a014bbdf87b
√ Name: test
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://test--jane-doe.jobs.default.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: succeeded
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
Hello, World!
```

Apolo cluster resources might be sliced into presets to be suitable for running different kinds of workloads. Cluster managers or administrators are able to do so. Some of the jobs may also require GPU resources. You can view the list of available presets using the `apolo config show` command.

```
$ apolo config show
User Configuration:                                       
 User Name            jane-doe                      
 Current Cluster      default                             
 Current Org          <no-org>                            
 Current Project      default                           
 Credits Quota        unlimited                           
 Jobs Quota           unlimited                           
 API URL              https://staging.neu.ro/api/v1       
 Docker Registry URL  https://registry.default.org.neu.ro 
Resource Presets:                                                                                                          
 Name               #CPU   Memory   Round Robin   Preemptible Node   GPU                     Jobs Avail   Credits per hour 

 cpu-small             1     4.0G                                                                  55   1                
 cpu-medium            3    11.0G                                                                  15   2                
 cpu-large             7    28.0G                                                                   7   4                
 gpu-k80-small         3    57.0G                                  1 x nvidia-tesla-k80            30   10               
 gpu-k80-small-p       3    57.0G                                  1 x nvidia-tesla-k80            10   1                
 gpu-v100-small        7    57.0G                                  1 x nvidia-tesla-v100           10   32               
 gpu-v100-small-p      7    57.0G                                  1 x nvidia-tesla-v100           10   4                
 gpu-small             3    57.0G                                  1 x nvidia-tesla-k80            30   10               
 gpu-small-p           3    57.0G                                  1 x nvidia-tesla-k80            10   1                
 gpu-v100-large       63   480.0G                                  8 x nvidia-tesla-v100            4   256              
 gpu-v100-large-p     63   480.0G                                  8 x nvidia-tesla-v100            4   32               
 gpu-v100-2           15   115.0M                                  2 x nvidia-tesla-v100           16   64               
 gpu-v100-4           31   235.0M                                  4 x nvidia-tesla-v100            8   128              
 gpu-v100-8           63   480.0M                                  8 x nvidia-tesla-v100            4   256
```

The command lists the available presets and their configurations. For example, the **cpu-small** preset includes 1 CPU, 4GB of memory, and no GPU. Whereas, the **gpu-k80-small** includes 5 CPU, 48GB memory, and an nvidia-tesla-k80 GPU.

### How do I run a job?

On a startup, each job obtains a unique ID. For your convenience, you can give a job a name. There can only be a single PENDING or RUNNING job with a given name (so you could recreate jobs with the same name).

Each job has access to its ephemeral storage (which is essentially a part of SSD on the physical machine this job runs on). This type of storage is fast but not persistent: as soon as you kill the job, the data is lost.

To make the data persistent, you can mount volumes of platform storage to the job. This type of storage is slightly slower and has some limitations. For example, running model training on data from the mounted folder is generally 10-20% slower.

{% tabs %}
{% tab title="CLI" %}
To run a job in CLI, you can use the `apolo run` command. This command accepts a lot of different arguments, most of which are explained in this and the following sections.

**Sample commands:**

* **Run a fast job without mounting storage:**

```
$ apolo run --preset cpu-small --name job230 ubuntu -- echo Hello, World!
√ Job ID: job-3ef0d955-bd2e-491a-aaea-f17b418fd4e8
√ Name: job230
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://job230--jane-doe.jobs.default.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: succeeded
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
Hello, World!
```

* **Running a long training job with mounting storage**

```
$ apolo run --name job303 --volume storage:nero-assistant/ModelCode/:/code:rw --preset cpu-small neuromation/base -- python code/train.py -d /data
√ Job ID: job-5a4942de-06ac-489d-8ac8-399640904991
√ Name: job303
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://job303--jane-doe.jobs.default.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: succeeded
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
Your training script here. Data directory: /data
```
{% endtab %}

{% tab title="Apolo Console" %}
To run a job in Apolo Console, you'll need to use the **Custom Job** widget on your dashboard.&#x20;

First, click **RUN JOB**:&#x20;

![](<../../.gitbook/assets/image (23) (1).png>)

Next, you will need to choose the preset to run the job on. For example, **cpu-small**:

![](<../../.gitbook/assets/image (3) (1) (1).png>)

Then, specify the image that will be used in the job:

![](<../../.gitbook/assets/image (17) (1).png>)

You can now enter the command you want to run in the corresponding field:

![](<../../.gitbook/assets/image (31) (1).png>)

You can optionally enter a name for this job and a HTTP port that will be associated with this job. The default port value is 80.

![](<../../.gitbook/assets/image (7) (1).png>)

And now, you can just click **RUN** to run the job:

![](<../../.gitbook/assets/image (29) (6).png>)

You will learn more about various job parameters such as images, HTTP ports, and others, in this and following topics.
{% endtab %}
{% endtabs %}

### How can I see the list of currently running jobs?

You can use the `apolo ps` command to list the jobs that are currently running. You can use various options to filter the list of jobs based on status, owner, or by name. To know information about a particular job, you can use the `apolo job status` command.

**Sample Commands:**

1. **See the list of all currently running jobs**

```
$ apolo ps
ID                                       NAME   STATUS  WHEN           IMAGE          OWNER  
job-3erw4f2e-cc57-4e4b-af04-c795b76d9ca8 job363 running 6 seconds ago  ubuntu:latest  <you>  
job-d2c04f2e-cc57-4e4b-af04-c795b76d9ca8 job390 pending 26 seconds ago ubuntu:latest  <you>  
```

2. **See the list of jobs in the pending status**

```
$ apolo ps -s pending
ID                                        NAME   STATUS   WHEN          IMAGE         OWNER   
job-d2c04f2e-cc57-4e4b-af04-c795b76d9ca8  job390 running  3 minutes ago ubuntu:latest <you>   
```

### Can I connect to a job when it is running?

When running a job, you may sometimes want to connect to it and execute a command. You can use the `apolo job exec` command to connect to a running job.

**Sample command:**

* **Running a simple list command in the container hosting the job**

```
$ apolo job exec job363 -- ls
bin dev home lib32 libx32 mnt proc run srv tmp varboot etc lib lib64 media opt root sbin sys usr
Connection to ssh-auth.neuro-public.org.neu.ro closed.
```

* **Providing a bash terminal to the container hosting the job**

```
$ apolo job exec job363 -- /bin/bash
root@job-36d59977-84d2-40e5-9475-e4af25a06b6c:/# echo "Hello, World!"
Hello, World!
root@job-36d59977-84d2-40e5-9475-e4af25a06b6c:/# exit
exit 
Connection to ssh-auth.neuro-public.org.neu.ro closed.
```

A bash terminal lets you work on the container while the job is running.

### What are job states?

A job is run until completion or until it is killed. A job goes through many states until it completes or fails. You can view a job's current state by using the `apolo job status` command.

**Sample command:**

```
$ apolo job status filebrowser-49249
Job                      job-d31c2ce9-f27b-4de0-9b60-b619ff6ff2af
Name                     filebrowser-49249
Tags                     kind:web-widget, target:filebrowser
Owner                    jane-doe
Cluster                  default
Status                   running
Image                    filebrowser/filebrowser:v2-alpine
Command                  --noauth --root /var/storage
Resources                 Memory              4.0G
                          CPU                  1.0
                          Extended SHM space  True
Life span                1d
TTY                      False
Volumes                   /var/storage         storage:
                          /var/storage/.neuro  storage:.neuro/
Internal Hostname        job-d31c2ce9-f27b-4de0-9b60-b619ff6ff2af.platform-jobs
Internal Hostname Named  filebrowser-49249--jane-doe.platform-jobs
Http URL                 https://filebrowser-49249--jane-doe.jobs.default.org.neu.ro
Http port                80
Http authentication      True
Environment               NEURO_STEAL_CONFIG   /var/storage/.neuro/85f68be9-6230-40ca-9c07-80d43275ee94-cfg
                          NEURO_PASSED_CONFIG  eyJ0b2tlbiI6ICJleUpoYkdjaU9pSklVekkxTmlJc0luUjVjQ0k2SWtwWFZDSjkuZXlK…
Created                  2021-01-12T19:36:37.468683+00:00
Started                  2021-01-12T19:36:52.733308+00:00
```

A job can have one of the following states:

* Pending: When the job is created and the resources for the job are allocated.
* Pulling: When resources for the job are being accessed.
* Running: When a job is being executed.
* Restarted: When a job failed, but was restarted by Apolo due to restart policy configuration
* Complete: When a job is complete.
* Failed: When a job fails and exits with an error code.

### How do I expose the HTTP server running in a job?

Applications you run within a job might expose some web interface, such as Jupyter Notebooks, TensorBoard, and others. When you run such a job, you may need to access this web interface in your browser. For that, you need to pass a port that should be exposed via the `--port` option (which is 80 by default).

To open the exposed interface in the browser, there are several options:

* Pass `--browse` as a `apolo run` parameter. In this case, an OS default web browser will open up as soon as the job is running;
* Run `apolo job browse <NAME or ID>` when the job is already running;
* Manually open http URL of the job, reported by Apolo CLI on job startup;
* Click on the HTTP URL for this job at the Apolo web dashboard.

All jobs you run are hidden behind SSO by default. This means that if you share a link to the job web interface with someone, they will have to log into the platform and have granted at least a read permission to access the job (see the section below). To expose a job to everyone you need to pass `--no-http-auth` to `apolo run`. We strongly recommend avoiding this option unless you know what you are doing.

Example:

```
apolo run --name filebrowser-demo --preset cpu-small --http 8085 --no-http-auth --browse --volume storage::/srv:rw filebrowser/filebrowser --noauth --port 8085
```

This command runs a FileBrowser hosting your project's storage root folder on 8085 port, exposes this port, removes SSO check, and opens the web interface in your default browser when the job is running.

### How do I control the job duration?

You can control the duration of time for which jobs run using the `life-span` configuration parameter. The default life-span is 1 (one) day. You can update the default `life-span` parameter for your CLI in the \[job] section of the global configuration file. The global configuration file is located in the standard apolo config path. The Apolo CLI uses the `~/.neuro` folder by default, and the path for global config file is `~/.neuro/user.toml`.

The parameter limits the default job run time, and is in string format. For example, a value of 2d3h20min would limit the job run time to 2 days, 3 hours, and 20 minutes.

**Example:**

```
# jobs section
[job]
life-span = "2d3h20min"
```

You can also set this parameter for specific jobs by using the corresponding option:&#x20;

```
$ apolo run --life-span 2h <image>
```

It's also possible to increase a job's lifespan through `bump-life-span`:

```
$ apolo job bump-life-span <job-name-or-id> 2d
```

This will add 2 days to the current lifespan of the `<job-name-or-id>` job.

### How do I terminate a job?

You can terminate any job using the `apolo job kill` command. You must know the job name or job id to terminate a job.

**Sample command:**

```
$ apolo job kill filebrowser-49249
job-d31c2ce9-f27b-4de0-9b60-b619ff6ff2af
```

### Can I share a job with others?

Yes, Apolo lets you share any running jobs with you teammates as well as any other platform artifact. Refer to [projects.md](../clusters-and-roles/projects.md "mention") for long-term collaboration with your teammates or to CLI's [grant](https://app.gitbook.com/s/-MOkWy7dB5MDbkSII8iF/commands/acl#grant "mention") command for ad-hoc cases.

### Where can I find a job's logs?

You can view the complete log for a job using the `apolo job logs [job name or id]` command. This command displays logs for the specified job.

The log is also displayed if you don't pass the `--detach` option when the job is run. The `--detach` option ensures that the job is not attached to logs and doesn't wait for an exit code.

Job logs are persisted for some long period of time (configurable per cluster), so you could refer to them later.

**Sample Command:**

```
$ apolo job logs filebrowser-49249
2021/01/12 19:36:52 Using config file: /.filebrowser.json
2021/01/12 19:36:52 Listening on [::]:80
2021/01/12 19:36:55 /: 404 10.60.0.10 <nil>
2021/01/12 21:11:23 Caught signal terminated: shutting down.
2021/01/12 21:11:23 accept tcp [::]:80: use of closed network connection
```

### Can I manage jobs from the Apolo Console?

Apolo provides an intuitive interface that lets you manage jobs. The **Jobs** page of the Apolo web interface lists all the jobs.

![](<../../.gitbook/assets/image (209).png>)

You can view the web interface of the job by clicking the 'three dots' icon near the job and then clicking **HTTP URL**.

![](<../../.gitbook/assets/image (202).png>)

To view the log and and other details about a job, click on the job ID.

![Job Details section](<../../.gitbook/assets/image (96) (1).png>)

By default, the **Jobs** page displays all currently running jobs. You can filter jobs by status using the corresponding drop-down list:

![](<../../.gitbook/assets/image (214).png>)

You can search for specific jobs using the **Search** field. The search functionality works with job names, IDs, and tags.

![](<../../.gitbook/assets/image (194).png>)

The UI also lets you kill or rerun a job by clicking **KILL** or **RERUN** in the drop-down menu accessible through the 'three dots' icon near the job.

![KILL and RERUN options](<../../.gitbook/assets/image (101).png>)

### Monitoring jobs

You can monitor various parameters of your currently running jobs such as CPU and GPU usage, network traffic, prices, etc., in the Apolo Console.

All of this information is accessible in the [Grafana](https://grafana.com/)-based **User Jobs Monitor** feature. To access this feature, go to the **Information** tab and click **USER JOBS MONITOR**:\
&#x20;&#x20;
