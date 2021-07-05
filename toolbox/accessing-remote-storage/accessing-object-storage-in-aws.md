# Accessing Object Storage in AWS

### Introduction

This tutorial demonstrates how to access your AWS S3 from Neuro Platform. You will set up a new Neuro project, create an S3 bucket, and make it is accessible from Neuro Platform jobs.

Make sure you have [Neu.ro CLI](https://neu-ro.gitbook.io/neu-ro-cli-reference/) installed.

### Creating Neuro Project

To create a new Neuro project, run:

```bash
neuro project init
cd <project-slug>
neuro-flow build myimage
```

### Creating an AWS IAM User

Follow [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).

In AWS Console, go to "Services" drop-down list, "IAM" \(Identity and Access Management\). On the left-hand panel, choose "Access management" -&gt; "Users", click the "Add user" button, go through the wizard, and as a result you'll have a new user added:

![](../../.gitbook/assets/1_add_user.png)

Ensure that this user has "AmazonS3FullAccess" in the list of permissions.

Then, you'll need to create an access key for the newly created user. For that, go to the user description, then to the "Security credentials" tab, and press the "Create access key" button:

![](../../.gitbook/assets/2_create_key.png)

Put these credentials to the local file in the home directory `~/aws-credentials.txt`. For example:

```text
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

Set appropriate permissions to the secret file:

```bash
chmod 600 ~/aws-credentials.txt
```

Set up the Neuro Platform to use this file and check that your Neuro project detects it:

```bash
neuro secret add aws-key @~/aws-credentials.txt
```

Open `.neuro/live.yaml`, find `remote_debug` section within `jobs` in it and add the following lines at the end of `remote_debug`:

```bash
jobs:
  remote_debug:
     ...
     secret_files: '["secret:aws-key:/var/secrets/aws.txt"]'
     additional_env_vars: '{"AWS_CONFIG_FILE": "/var/secrets/aws.txt"}'
```

### Creating a Bucket and Granting Access

Now, create a new S3 bucket. Remember: bucket names are globally unique.

```bash
BUCKET_NAME="my-neuro-bucket-42"
aws s3 mb s3://$BUCKET_NAME/
```

### Testing

Create a file and upload it into S3 Bucket:

```bash
echo "Hello World" | aws s3 cp - s3://$BUCKET_NAME/hello.txt
```

Change default preset to `cpu-small` in `.neuro/live.yaml`to avoid consuming GPU for this test:

```bash
defaults:
  preset: cpu-small
```

Run a development job and connect to the job's shell:

```bash
neuro-flow run remote_debug
```

In your job's shell, try to use `s3` to access your bucket:

```bash
aws s3 cp s3://my-neuro-bucket-42/hello.txt -
```

To close the remote terminal session, press `^D` or type `exit`.

Please don't forget to terminate the job when you've done working with it:

```bash
neuro-flow kill remote-debug
```

