# Projects

## What are projects?

Projects provide a way to quickly create the basic necessary folder structure and set up integrations with several recommended tools for your model development environment.&#x20;

So, once you start working with the Neu.ro platform, we highly recommend that you create a new project to ensure that the most important functionality will be accessible right away.

Every project is based on a template that contains the `.neuro/live.yaml` configuration file for [Neu.ro Flow](https://neu-ro.gitbook.io/neuro-flow/) - a tool that simplifies your work process with Neu.ro. This file guarantees a proper connection between the project structure, the base environment that we provide, and actions with storage and jobs.

## Creating projects

As our project templates are based on the [**cookiecutter** package](https://github.com/cookiecutter/cookiecutter), you will first need to install it via **pip** or **pipx**:

```
$ pipx install cookiecutter
```

Now, to initialize a new [Neuro cookiecutter project](https://github.com/neuro-inc/cookiecutter-neuro-project/blob/master/cookiecutter.json), run:

```
$ cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
```

This command will prompt you to enter some info about your new project:

```
project_name [Neuro Project]: New Cookiecutter Project
project_dir [new-cookiecutter-project]:
project_id [new_cookiecutter_project]:
code_directory [modules]:
preserve Neuro Flow template hints [yes]:
```

{% hint style="info" %}
Default values are indicated by square brackets **\[ ].** You can use them by pressing **Enter**.
{% endhint %}

When you create a new project, its structure will be as follows:

```
new-cookiecutter-project
├── .github/            <- Github workflows and a dependabot.yml file
├── .neuro/             <- neuro and neuro-flow CLI configuration files
├── config/             <- configuration files for various integrations
├── data/               <- training and testing datasets (we don't keep it under source control)
├── notebooks/          <- Jupyter notebooks
├── modules/            <- models' source code
├── results/            <- training artifacts
├── .gitignore          <- default .gitignore file for a Python ML project
├── .neuro.toml         <- autogenerated config file
├── .neuroignore        <- a file telling Neuro to ignore the results/ folder
├── HELP.md             <- autogenerated template reference
├── README.md           <- autogenerated informational file
├── Dockerfile          <- description of the base image used for your project
├── apt.txt             <- list of system packages to be installed in the training environment
├── requirements.txt    <- list of Python dependencies to be installed in the training environment
├── setup.cfg           <- linter settings (Python code quality checking)
└── update_actions.py   <- instructions on update actions
```

The internal ID of a project used by Neu.ro Flow is generated automatically based on the name of the root folder containing the `.neuro` folder. This can be changed by placing a `project.yml` file with the desired project ID attribute into the `.neuro` folder. Feel free to refer to the [Neu.ro Flow reference](https://neu-ro.gitbook.io/neuro-flow/reference/project-configuration-syntax) for more information.

## Project modes

Neu.ro Flow provides two modes of automating and simplifying your work process while working in a project - **live** mode and **batch** mode.&#x20;

### Live mode

Live mode allows to execute sets of job definitions that create independent jobs in the Neu.ro cloud.&#x20;

A job or set of jobs executed in live mode is called a **live job**.

You can learn more about live jobs in the [Neu.ro Flow reference](https://neu-ro.gitbook.io/neuro-flow/reference/live-workflow-syntax#live-workflow).

### Batch mode

Batch mode serves to orchestrate sets of remote tasks that depend on each other. This is achieved by having a main job that manages the overall workflow by spawning all required sub-jobs, waiting for their results, and starting dependent tasks when all of their requirements are satisfied.&#x20;

A single execution of a set of jobs in batch mode is called a **bake**.

You can learn more about the batch mode in the [Neu.ro Flow reference](https://neu-ro.gitbook.io/neuro-flow/reference/batch-workflow-syntax).

## Monitoring projects

You can monitor your projects both via the CLI and in the Web UI.

{% tabs %}
{% tab title="CLI" %}
### Monitoring live jobs

To see a list of live jobs on the current project, their IDs, statuses, and starting times, run the following command:

```
$ neuro-flow ps

  JOB          │ STATUS    │ RAW ID                                   │ WHEN
╶──────────────┼───────────┼──────────────────────────────────────────┼────────╴
  filebrowser  │ unknown   │ N/A                                      │ N/A
  jupyter      │ unknown   │ N/A                                      │ N/A
  multitrain   │ unknown   │ N/A                                      │ N/A
  remote_debug │ cancelled │ job-5938269e-4a57-408c-af39-109970ac5c59 │ Dec 22
  tensorboard  │ unknown   │ N/A                                      │ N/A
  train        │ unknown   │ N/A                                      │ N/A
               ╵           ╵                                       
```

### Monitoring bakes

Use the following command to monitor your project's bakes:

```
$ neuro-flow bakes
```

This command will show the the ID, creation date, and the status of each bake on your project.

To see detailed info about a specific bake, use:

```
$ neuro-flow inspect <bake-id>
```

This will show which tasks were executed in this bake, their statuses, IDs, starting and completion time. You can also export a bake's graph by adding the `--output-graph` option and choosing the desired graph format (DOT or PDF).\
\
You can learn more about bake inspection [here](https://neu-ro.gitbook.io/neuro-flow/reference/cli#neuro-flow-inspect).
{% endtab %}

{% tab title="Web UI" %}
You can view the list of your projects in the **Projects** tab:

![](<../../.gitbook/assets/image (142).png>)

Click on a project's name to view the list of its live jobs and batch bakes:

![](<../../.gitbook/assets/image (141).png>)

You can view each job's and bake's details by clicking on their name.
{% endtab %}
{% endtabs %}

## Sharing projects

You can share a project (i.e., access to resources created under a project) by creating a custom role for this project in it's `project.yml` file.

This can be done in two ways:

* Specifying an `<owner>` value. \
  In this case, the project's role will be of the following form: \
  `role://<owner>/projects/<projectname>`
* Specifying a `<role>` value.\
  In this case, the project's role will be of the following form:\
  `role://<role>`

After this role is created, you can share it with other users iva the following command:

```
$ neuro share project-role another-user read (share ~ acl grant)
```

The access level you decide to choose here only affects the ability of the target user to share this role further. With `read` or `write`, the user will not be able to share the role, while `manage` will provide them the ability to do so.

{% hint style="info" %}
You can find more information about the owner and role values in the [corresponding section of our Neu.ro Flow reference](https://neu-ro.gitbook.io/neuro-flow/reference/project-configuration-syntax#owner).
{% endhint %}

After this, the target user will receive all access rights of this project role, and, accordingly, access to all the resources in the project. \
Moreover, when this user runs new jobs and creates new objects, the rights to these jobs and objects will also be added to the project role, and everyone with this role will gain access to them from this point on.

The only objects that will not be automatically shared via this process are secrets, buckets, and disks. For objects like these, access rights should be shared manually through corresponding commands.
