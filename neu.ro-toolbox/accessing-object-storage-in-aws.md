# Accessing Object Storage in AWS

### Introduction

This tutorial demonstrates how to access your AWS S3 from Neuro Platform. You will set up a new Neuro project, create an S3 bucket, and make it is accessible from Neuro Platform jobs.

Make sure you have [Neu.ro CLI](../getting-started/installing-cli.md) installed.

### Creating Neuro Project

To create a new Neuro project, run:

```bash
neuro project init
cd <project-slug>
make setup
```

### Creating an AWS IAM User

Follow [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).

Briefly, in AWS Console, go to "Services" drop-down list, "IAM" \(Identity and Access Management\). On the left-hand panel choose "Access management" -&gt; "Users", click the blue button "Add user", go through the wizard, and as a result you'll have a new user added:

![](../.gitbook/assets/1_add_user.png)

Ensure that this user has "AmazonS3FullAccess" in the list of permissions.

Then, you'll need to create an access key for the newly created user. For that, go the user description, tab "Security credentials", press button "Create access key":

![](../.gitbook/assets/2_create_key.png)

Put these credentials to the local file in your project directory `./config/aws-credentials.txt`, for example:

```text
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

Set appropriate permissions to the secret file:

```bash
chmod 600 ./config/aws-credentials.txt
```

Set up Neuro Platform to use this file and check that Neuro project detects the file:

```bash
export AWS_SECRET_FILE=aws-credentials.txt
make aws-check-auth
```

This command should print something like: \`AWS will be authenticated via user account credentials file: '/project-path/config/aws-credentials.txt".

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

Run a development job and connect to the job's shell:

```bash
export PRESET=cpu-small  # to avoid consuming GPU for this test
make develop
make connect-develop
```

In your job's shell, try to use's 3\` to access your bucket:

```bash
aws s3 cp s3://my-neuro-bucket-42/hello.txt -
```

To close the remote terminal session, press `^D` or type `exit`.

Please don't forget to terminate your job when you don't need it anymore:

```bash
make kill-develop
```

