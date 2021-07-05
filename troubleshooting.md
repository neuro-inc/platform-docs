# Troubleshooting

## Lost/unknown job ID

The first step in any investigation is knowing a job ID. If you started your job with `neuro run`, the job's ID was printed in the output. 

However, if you can't find the initial terminal output, you can use one of these commands to find a specific job:

{% tabs %}
{% tab title="neuro run jobs" %}
`neuro ps` prints only running jobs.   
`neuro ps -a` prints all jobs.  
`neuro ps -s failed` prints all jobs with the Failed status.
{% endtab %}

{% tab title="neuro-flow jobs" %}
Run `neuro-flow ps` to get the list of all jobs.
{% endtab %}
{% endtabs %}

## Image build failed

When you run `neuro-flow build IMAGE_NAME`, neuro-flow uploads the build context to the platform and creates a platform job that uses [Kaniko](https://github.com/GoogleContainerTools/kaniko) to build a docker image and push it to the platform registry.

If building fails, you can check the job's status and logs to get more information. 

#### Getting a job's status

To check a job's status, run: 

```text
$ neuro status <job-ID> 
```

The **Status transitions** section in the output can help you learn at which step the job failed. 

#### Getting builder logs

To check builder logs, run:

```text
$ neuro logs <job-ID> 
```

## neuro run / neuro-flow run failed

There are a few main reasons your job may fail. Here are some of the most common:

#### Incorrect image name

This can happen if you have a typo in the image name or if the specified image was not built before running a job. List of all images can be accessed by running `neuro image ls`. You can also list tags for a particular image via `neuro image tags <IMAGE_URI>`.

#### Incorrect volume mounted

You might have an invalid volume mounted to the job. For example, you've mounted a volume to the `/my-project` folder, but your code expects `/my_project`. You can double-check it in the logs.

#### Cluster scale up failed

If you see a **Cluster Scale Up Failed** error in the status, it usually means you’ve requested resources that are not available in the cluster at the moment. For example, all GPUs are busy, so your job can’t be scheduled.

#### Code issues

You may have an error in your python script that prevents the job from running properly.

## Can’t access my job via HTTP

There are a few steps to troubleshooting such issues.

#### Checking for an open HTTP port

The first point of interest is whether you have an open HTTP port for your job. To check this, you can: 

{% tabs %}
{% tab title="neuro run jobs" %}
Use the `--http_port` parameter.
{% endtab %}

{% tab title="neuro-flow jobs" %}
Use the `http_port:` option.
{% endtab %}
{% endtabs %}

#### Checking the listening IP

Next step would be to make sure your web app listens on 0.0.0.0, not on 127.0.0.1 or `localhost` — otherwise it won't be able to accept incoming requests from the outside of the container.

#### Disabling HTTP authentication

And finally, if you can access your job via browser, but `curl` and similar tools don’t work, most likely you didn’t disable HTTP authentication. The Neu.ro platform puts an HTTP authentication layer in front of your app by default for security reasons. 

You can disable this behaviour manually when running jobs:

{% tabs %}
{% tab title="neuro run jobs" %}
Use the `--no-http-auth` parameter.
{% endtab %}

{% tab title="neuro-flow jobs" %}
Use the `http_auth: False` option.
{% endtab %}
{% endtabs %}

## Troubleshooting a running job

Just like with Docker, you can get a shell in a running job to check its state. To do this, run:

```text
$ neuro exec JOB_ID /bin/sh
```

Note: In Docker you would typically add the `-it` parameters to the command, but they’re not necessary for `neuro exec`.

