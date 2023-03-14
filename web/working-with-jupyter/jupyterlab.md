# JupyterLab

Jupyter Notebooks is a powerful computational notebook tool that lets you create and share your code. JupyterLab uses Jupyter Notebook and takes it much further. JupyterLab is an interactive web-based development environment for working with Jupyter Notebooks, text editors, terminals, and custom components.

You can arrange multiple Notebooks, documents, and activities in a single interface using tabs and splitters. Also, JupyterLab has a modular extensible architecture that lets you customize your development environment. A Notebook is just one of the components that you can work on in JupyterLab.

**To create a new JupyterLab instance:**

* Log in to your Neu.ro dashboard.
* Click **RUN A JOB** in the IDEs widget.

![](<../../.gitbook/assets/image (34).png>)

* Select Jupyterlab in the top drop-down menu, a preset in the bottom drop-down menu, and click **RUN**.

![](<../../.gitbook/assets/image (39).png>)

When you start JupyterLab, the platform storage is attached as /var/storage. All data created during a lab session in this folder persists and can be used later.&#x20;

All JupyterLab instances are jobs that run on GPU-small preset. You must kill the JupyterLab session whenever you are done; else, it consumes GPU hours.&#x20;

![JupyterLab instance](../../.gitbook/assets/JL\_Overview.jpg)

The Jupyter UI has the following areas:

* Tools area: The tools area lists the most commonly used commands.
* Work area: The work area hosts the launcher and the tabs that you are currently working on.
* Menu: The menu bar includes all available options for JupyterLab.

The tools area lists a host of options available for JupyterLab, such as:

* File Browser for managing files in the storage.
* Sessions for managing a list of active sessions.
* Commands for viewing the commands available for the current state.
* Properties for managing the properties for the current selection.
* Tabs to view the list of tabs open.
* Extension Manager.

For more information about JupyterLab, see [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) documentation.

## Manage your JupyterLab Instances

You can manage your JupyterLab instances from the Neu.ro dashboard. The JupyterLab instances are listed in the Running Jobs section, and their names start with _**jupyter-lab-**_.

You can view the details of your JupyterLab instance by clicking on its job ID.

![](<../../.gitbook/assets/image (136).png>)

There are a few ways you can open an instance of JupyterLab:

* Click **HTTP URL** in the **Running Jobs** section on your Dashboard.

![](<../../.gitbook/assets/image (44).png>)

* Clicking **OPEN RUNNING** in the **IDEs** widget on your Dashboard.

![](<../../.gitbook/assets/image (191).png>)

* Clicking **HTTP URL** in the drop-down menu for the corresponding job in the job list.

![](<../../.gitbook/assets/image (152).png>)

You can kill the JupyterLab instance using the same drop-down menu. Note that closing the tab does not kill the instance.

Alternatively, you can kill the instance from the JupyterLab instance using the File > Shut Down option.

![Killing a JupyterLab instance](../../.gitbook/assets/JL\_shutdown.gif)

An instance is automatically terminated 24 hours after initiation.
