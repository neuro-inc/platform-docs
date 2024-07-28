# Secrets

Secrets provide a way to store confidential data on Apolo, be it passwords, access keys, tokens, etc. You can use secrets in jobs to securely access some external services. Apolo secrets are based [Kubernetes secrets](https://kubernetes.io/docs/concepts/configuration/secret/). You can manage secrets both through Apolo CLI and the Web UI.

### Creating secrets

#### Creating secrets through the CLI

To create a secret from the Apolo CLI, use the `apolo secret add` command:

```
> apolo secret add secret-password p@$$w0rd123
```

This will create a secret with a key _secret-password_ and a value of _p@\$$w0rd123_.

You can also point to an existing file with the required value when creating a secret by using the `@` notation:

```
> apolo secret add secret-password @path/to/secret/file.txt
```

#### Creating secrets through the Web UI

To create a secret from the Apolo Web UI:

* Log in to Apolo console&#x20;
* Go to the **Secrets** tab:

![](<../../.gitbook/assets/image (234) (1).png>)

* Click **Add**:

![](<../../.gitbook/assets/image (74).png>)

* Enter the secret's name, value, and click **Save**:

![](<../../.gitbook/assets/image (63).png>)

The new secret will be added to the list of your secrets:

![](<../../.gitbook/assets/image (75).png>)

### Using secrets

There are two ways to use secrets in jobs - as a file and as an environment variable. Let's look how to do this in the CLI and in the Web UI.

#### Using secrets through the CLI

To use a secret as a file in your job, provide its location in a `--volume` parameter when running a job. For example:

```
--volume secret:secret-password:/var/secrets/secret-password.txt"
```

To use a secret as an environment variable, declare it through the `--env` parameter when running a job:

```
--env mypass=secret:secret-password
```

Now, depending on which method you chose, you can access the secret from within your job by either referring to its location `/var/secrets/secret-password.txt` or reading the value of the `mypass` environment variable.

#### Using secrets through the Web UI

To use a secret through the Web UI:

* Log in to platform console&#x20;
* On your dashboard, click **RUN A JOB** on a widget you want to work with. We'll use **Terminal** in this example:

![](<../../.gitbook/assets/image (256).png>)

* In the newly-opened window, click **ADD NEW SECRET**:

![](<../../.gitbook/assets/image (235) (1).png>)

* Select the type of secret you want to use and the secret's key from the drop-down list. Then, depending on the type of secret you selected, enter the name of the environment variable or the path to the secret's file:

![](<../../.gitbook/assets/image (232).png>)

* When ready, click **RUN.** You will be able to use the secret within a job run in this way.

### Sharing secrets

You can share secrets through the CLI by using the `apolo acl grant` command. The syntax is `apolo acl grant secret:<key> <username> <access-level>`. For example:

```
> apolo acl grant secret:secret-password bob read
```

This will give Bob the access to use the `secret-password` secret in their jobs.

You can also use the `apolo share` command as an alias for `apolo acl grant`:

```
> apolo share secret:secret-password bob
```

Keep in mind that, at this point in time, secret sharing is implemented in such a way that Bob won't be able to see this secret in their list of secrets when running `apolo secret ls`. To check if they have access rights to use this secret, Bob will need to run `apolo acl list` and search the output for the corresponding secret's URI. For example, if the secret was created by Alice, the URI will look like this:

```
secret://default/alice/secret-password
```

### Deleting secrets

#### Deleting secrets through the CLI

To delete a secret, run the `apolo secret rm` command:

```
> apolo secret rm secret-password
```

To check that the secret was removed, run the `apolo secret ls` command to list all your current secrets:

```
> apolo secret ls

  KEY
╶─────────────╴
  aws-key
  gcp-key
  wandb-token
```

#### Deleting secrets through the Web UI

* Go to the **Secrets** tab.
* Click the **trash bin** icon to the right of the secret you want to delete:

![](<../../.gitbook/assets/image (66).png>)

* Click the **check mark** icon to confirm the changes:

![](<../../.gitbook/assets/image (72).png>)
