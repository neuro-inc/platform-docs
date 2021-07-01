# Testing Models with Locust

[Locust](https://locust.io/) is an open-source load testing tool. You can use it in conjunction with Neu.ro to test your models and make sure they work as expected before deploying them to production.

## Using Locust with the platform

### Prerequisites

Before testing the model with Locust, you will need to make a test deployment via MLflow. To do this, you will need to run the following command:

```text
$ neuro-flow run deploy_inference_platform --param run_id <run-id>
```

You can check the required run ID in MLflow:

![](../../.gitbook/assets/image%20%28224%29.png)

By running the `deploy_inference_platform` job, you gain access to the target model's binary and deploy it to the Neu.ro cluster.

Once the job is running, it will also generate a predictable URI which you can later use to access the model in Locust.

### Running Locust

You can specify how the Neu.ro platform will use Locust via the `.neuro/live.yml` file in the project's root folder.

In the `jobs` section of the `live.yml` file, you will find a default `locust` job description that looks like this:

```text
locust:
    image: locustio/locust:1.4.1
    name: $[[ flow.title ]]-locust
    http_port: 8080
    http_auth: False
    life_span: 1d
    detach: True
    browse: True
    params:
      endpoint_url: 
        default: ~
        descr: |
          Examples:
          https://demo-oss-dogs-test-inference--<user-name>.jobs.<cluster-name>.org.neu.ro/api/v1.0/predictions - if model deployed as platform job
          http://seldon.<cluster-name>.org.neu.ro/seldon/seldon/<model-name>-<model-stage>/api/v1.0/predictions -if model is deployed in Seldon
    volumes:
      - $[[ upload(volumes.src).ref_ro ]]
      - $[[ upload(volumes.config).ref_ro ]]
      - $[[ volumes.remote_dataset.ref_ro ]]
    env:
      DOG_IDS: "n02085936, n02088094"
      IMGS_DIR: $[[ volumes.remote_dataset.mount ]]/images/Images/
      PYTHONPATH: $[[ volumes.src.mount ]]/..
    cmd: |
      -f $[[ volumes.src.mount ]]/locust.py --web-port 8080 -H $[[ params.endpoint_url ]]
```

You can always fine-tune the job depending on your needs by changing the values of various parameters such as `http_port` or `life_span` in the description.

To run a `locust` job, execute the following:

```text
$ neuro-flow run locust
```

This job will use a `locust.py` file specified in the `cmd` section of the job definition. This file will tell Locust what specific tests to run and how to run them. You can [learn more about writing Locust files here](https://docs.locust.io/en/stable/writing-a-locustfile.html).

Once the Locust job is up and running, you will need to open Locust in your browser by using the predictable URI generated in the previous steps.

Next, enter the locust URL of the job on the platform in the **Host** field and click **Start swarming**:

![](../../.gitbook/assets/image%20%28223%29.png)

When this is done, you can modify the amount of simulated users and the spawn rate and monitor the testing process through various charts:

![](../../.gitbook/assets/image%20%28225%29.png)

