# Using MLFlow

MLflow is an open-source platform for managing the machine learning lifecycle. It mainly focuses on the following four aspects:

* **Tracking**
* **Projects**
* **Models**
* **Model Registry**

You can use MLFlow in conjunction with Apolo to prepare efficient ML pipelines.

We have created a [repository with a project](https://github.com/neuro-inc/mlops-demo-oss-dogs) that can help you quickly check how this works.&#x20;

## Connecting MLFlow and Apolo

First, you will need to create a folder in your project's `volumes` section that will serve as a store for artifacts and SQLite data. Here, we'll name it `mlruns`:

```
mlruns:
  remote: storage:${{ flow.flow_id }}/mlruns
  mount: /usr/local/share/mlruns
```

Then, add the `mlflow` action in the `jobs` section. This will allow you to run MLFlow in your project by triggering the Github action from our repository without the need to configure it manually.

```
mlflow:
  action: gh:neuro-actions/mlflow@v1.17.0
  args:
    backend_store_uri: sqlite:///${{ volumes.mlruns.mount }}/mlflow.db
    default_artifact_root: ${{ volumes.mlruns.mount }}
    volumes: "${{ to_json( [volumes.mlruns.ref_rw] ) }}"
```

Now mount the newly created volume (`mlruns` in this case) in the `volumes` section of the `train` job's description:

```
volumes:
  - $[[ upload(volumes.data).ref_ro ]]
  - $[[ upload(volumes.code).ref_ro ]]
  - $[[ volumes.mlruns.ref_rw ]]
```

You can now pass the MLFLow URI to the training script and parse it to integrate your script execution with MLFlow running on our platform. Just add the following in the `bash` section of the `train` job's description:

```
--mlflow_uri http://${{ inspect_job('mlflow').internal_hostname_named }}:5000 \
```

Use the following code to parse the new `mlflow_uri` argument:

```
parser.add_argument(
    '--mlflow_uri',
    type=str,
    default='http//localhost:5000',
    help='Mlflow URI.'
)
```

After this, you can start using MLFlow's functionality in your project's script.
