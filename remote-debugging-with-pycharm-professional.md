# Remote Debugging with PyCharm Professional

### Introduction

In this tutorial, we show how to set up remote debugging with PyCharm Professional on Neuro Platform using the Neuro project template.

### Initializing a new project

First, ensure that you have `neuro` client installed and configured:

```bash
pip install -U neuromation
neuro login
```

Then, initialize an empty project:

```bash
neuro project init
```

This command asks several questions about your project:

```text
project_name [Name of the project]: Neuro PyCharm
project_slug [neuro-pycharm]: 
code_directory [modules]:
```

Next, configure the project's environment on Neuro Platform:

```bash
make setup
```

### Setting up PyCharm

Open the project created on the previous step in Pycharm Professional and add sample code to debug \(in this example, we use code snipped taken from [JetBrains documentation](https://www.jetbrains.com/help/pycharm/remote-debugging-with-product.html)\).

Then, _exclude all directories that don't contain Python code_ \(in an empty Neuro project, only `modules/` is supposed to contain code\). PyCharm does not synchronize excluded directories. For that, select all directories to exclude, mouse right-click, "Mark Directory as" -&gt; "Excluded". As a result, you will see a configured project:

![](.gitbook/assets/0_empty.png)

Now, we are ready to start a development GPU-powered job on Neuro Platform. In the shell, run:

```bash
make develop
```

This command starts a `develop` job on Neuro Platform, which uses `gpu-small` preset. All running jobs consume your quota, so please _don't forget to terminate your jobs_ when they are no longer needed \(for that, you can run `make kill-all`\).

Now, forward the job's SSH port to the localhost:

```bash
make port-forward-develop
```

Note, if default port `2211` is not available, you can specify another port via `make port-forward-develop LOCAL_PORT=2212`.

Then go back to PyCharm project, go to "File" -&gt; "Settings", "Project" -&gt; "Project interpreter" \(you can use search by the word "interpreter"\). Press on the gear sign to see project interpreter options, select "Add...". In the new window, select "SSH Interpreter", and set up the following configuration:

```bash
Host: localhost
Port: 2211
Username: root
```

![](.gitbook/assets/1_add_py_interpreter.png)

Press the Next button.

In the new window, specify paths:

```bash
Interpreter: /usr/local/bin/python
Sync folders: <Project root> -> /neuro-pycharm
```

Note, within the job, your project root is available at the root of the filesystem: `/{project_name}`

Press the Finish button, and your configuration is ready:

![](.gitbook/assets/2_mapping.png)

Press the OK button.

Once you applied remote interpreter configuration, PyCharm starts files synchronization.

Congratulations! Your PyCharm project is now configured to work with remote Python interpreter running on Neuro job:

![](.gitbook/assets/3_debugging.png)

Note: if your project mapping was not configured and the remote interpreter attempts to execute a file with a local path on the remote environment, you might need to specify mapping again: "Run" -&gt; "Edit Configurations..." -&gt; "Path mappings":

![](.gitbook/assets/4_after_mapping.png)

