# Accessing Object Storage in GCP

###

### Introduction

This tutorial demonstrates how to access your Google Cloud Storage from the Neuro platform. You will create a new Neuro project, a new project in GCP, a service account, and a bucket that's accessible from a job on the Neuro platform.

Make sure you have [Neu.ro CLI](https://neu-ro.gitbook.io/neu-ro-cli-reference/) and **** [**cookiecutter**](https://github.com/cookiecutter/cookiecutter) installed.

### Creating Neuro and GCP Projects

To create a new Neuro project and build an image, run:

```bash
$ cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
$ cd <project-id>
$ neuro-flow build myimage
```

It's a good practice to limit the scope of access to a specific GCP project. To create a new GCP Project, run:

```bash
PROJECT_ID=${PWD##*/}  # name of the current directory
gcloud projects create $PROJECT_ID
gcloud config set project $PROJECT_ID
```

Make sure to [set a billing account](https://cloud.google.com/billing/docs/how-to/modify-project) for your GCP project. See [Creating and Managing Projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects) for details.

### Creating a Service Account and Uploading an Account Key

First, create a service account for the job:

```bash
SA_NAME="neuro-job"
gcloud iam service-accounts create $SA_NAME  \
    --description "Neuro Platform Job Service Account" \
    --display-name "Neuro Platform Job"
```

See [Creating and managing service accounts](https://cloud.google.com/iam/docs/creating-managing-service-accounts#iam-service-accounts-create-gcloud) for more details.

Then, download the account key:

```bash
gcloud iam service-accounts keys create ~/$SA_NAME-key.json \
  --iam-account $SA_NAME@$PROJECT_ID.iam.gserviceaccount.com
```

Make sure that the newly created key is located at `~/`.

Create a new secret for the file:

```bash
$ neuro secret add gcp-key @~/$SA_NAME-key.json
```

Open `.neuro/live.yaml`, find the `remote_debug` section within `jobs`, and add the following lines at the end of `remote_debug`:

```bash
jobs:
  remote_debug:
     ...
     secret_files: '["secret:gcp-key:/var/secrets/gcp.json"]'
     additional_env_vars: '{"GOOGLE_APPLICATION_CREDENTIALS": "/var/secrets/gcp.json"}'
```

### Creating a Bucket and Granting Access

Now, create a new bucket. Remember: bucket names are globally unique (see more information on [bucket naming conventions](https://cloud.google.com/storage/docs/naming)).

```bash
BUCKET_NAME="my-neuro-bucket-42"
gsutil mb gs://$BUCKET_NAME/
```

Grant access to the bucket:

```bash
# Permissions for gsutil:
PERM="storage.objectAdmin"
gsutil iam ch serviceAccount:$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com:roles/$PERM gs://$BUCKET_NAME

# Permissions for client APIs:
PERM="storage.legacyBucketOwner"
gsutil iam ch serviceAccount:$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com:roles/$PERM gs://$BUCKET_NAME
```

### Testing

Create a file and upload it into Google Cloud Storage Bucket:

```bash
echo "Hello World" | gsutil cp - gs://$BUCKET_NAME/hello.txt
```

Change the default preset to `cpu-small` in `.neuro/live.yaml`to avoid consuming GPU for this test:

```bash
defaults:
  preset: cpu-small
```

Run a development job and connect to the job's shell:

```bash
$ neuro-flow run remote_debug
```

In your job's shell, activate the service account for CLI:

```bash
gcloud auth activate-service-account --key-file $GOOGLE_APPLICATION_CREDENTIALS
```

And try to use `gsutil` to access your bucket:

```bash
gsutil cat gs://my-neuro-bucket-42/hello.txt
```

Please note that in `remote_debug`, `train`, and `jupyter` jobs the environment variable `GOOGLE_APPLICATION_CREDENTIALS` points to your key file. So you can use it to [authenticate other libraries](https://cloud.google.com/storage/docs/reference/libraries).

For instance, you can access your bucket via Python API provided by package `google-cloud-storage`:

```
>>> from google.cloud import storage
>>> bucket = storage.Client().get_bucket("my-neuro-bucket-42")
>>> text = bucket.get_blob("hello.txt").download_as_string()
>>> print(text)
b'Hello World\n'
```

To close remote terminal session, press `^D` or type `exit`.

Please don't forget to terminate your job when you don't need it anymore:

```bash
$ neuro-flow kill remote_debug
```
