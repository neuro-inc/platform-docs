# Remote Debugging with VS Code

### Introduction

In this tutorial, we will show how to set up remote debugging with VS Code on the Neuro Platform using the Neu.ro project template.

### Initializing a new project

First, make sure that you have the `neuro` client installed and configured:

```bash
pip install -U neuro-cli neuro-extras neuro-flow
neuro login
```

Then, initialize an empty project:

```bash
neuro project init
```

Make sure to re-download the `\.cookiecutters\cookiecutter-neuro-project` project if prompted.

The project initialization command asks several questions about your project:

```text
project_name [Name of the project]: Neuro VSCode
project_slug [neuro-vscode]: 
code_directory [modules]:
```

### Configuring the project

Add `debugpy` to your project's `requirements.txt` file \(located in the project's root folder\): 

```bash
neuro-flow

flake8
mypy
isort
black
debugpy
```

Add the following lines to the beginning of your code file. In this example, it's `modules/train.py`.

```bash
import debugpy
debugpy.listen(("0.0.0.0", 5678))
debugpy.wait_for_client()
```

Next, configure the project's environment on the Neu.ro Platform:

```bash
neuro-flow build myimage
```

When the image is built, you can upload your code from a local file to the platform storage:

```text
neuro-flow upload code
```

By default, this will upload everything from your project's `modules` folder to the `storage:<your_project_id>/modules` storage folder. To configure the source and the target for this command, go to your project's `.neuro/live.yml` file and find the `code` section under `volumes`:

```text
volumes:
  data:
    remote: storage:$[[ flow.project_id ]]/data
    mount: /project/data
    local: data
  code:
    remote: storage:$[[ flow.project_id ]]/modules
    mount: /project/modules
    local: modules
  config:
    remote: storage:$[[ flow.project_id ]]/config
    mount: /project/config
    local: config
    read_only: True
```

Here, you can specify your local code folder and the storage folder you want to upload this code to.

### Running your code

In this example, we will be running a training job based on the code contained in the `train.py` file we just uploaded to the platform storage. To do this, run:

```text
neuro-flow run train
```

Once the job is running, detach from it by pressing **Ctrl+P, Ctrl+Q** and run the following command:

```text
neuro job port-forward <job-id> 5678:5678
```

This will allow you to access the job by the `5678` port.

### Debugging 

Open your code file in VS Code and navigate to **Run &gt; Start Debugging** or press **F5**:

![](../.gitbook/assets/image%20%2889%29.png)

Select **Remote Attach**:

![](../.gitbook/assets/image%20%2887%29.png)

Enter **localhost** as the host name and the job's port number \(in this case, it's **5687**\):

![](../.gitbook/assets/image%20%2886%29.png)

![](../.gitbook/assets/image%20%2888%29.png)

When this is done, you can set the breakpoint and start debugging.

