---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Jupyter Notebooks

Jupyter Notebooks is a powerful computational notebook tool that lets you create and share your code. Neu.ro lets you manage all aspects of your Jupyter Notebooks from an intuitive web-based dashboard.

## Introduction

This tutorial demonstrates how to run a Jupyter Notebook instance in the Neu.ro Web UI. You will start a new instance of Jupyter Notebook and create a Python 3 notebook. You will also know how to view or kill the currently running instances.

## Create a new Jupyter Notebook instance

Neu.ro lets you create multiple instances of Jupyter Notebooks.

**To create a new Jupyter Notebook instance:**

1. Login in to your Neu.ro dashboard.
2. Click **Start** in the Jupyter Notebooks section.

![](../.gitbook/assets/Jupyter_Dashboard.jpg)

A new Jupyter Notebook instance is launched in a new tab.

![](../.gitbook/assets/Jupyter_Notebok.jpg)

The instance is not killed when you close the tab. You can always open the instance from the Neu.ro dashboard. However, the instance is terminated automatically after 24 hours of initiation.

The new instance is a powerful event-driven Jupyter notebook that saves every change that you make. All new instances run on [NVIDIA](https://www.nvidia.com/en-gb/data-center/tesla-k80/)[Tesla K80](https://www.nvidia.com/en-gb/data-center/tesla-k80/) video card that guarantees great performance. The notebook kernel includes a list of installed pip packages such as [TensorFlow](https://www.tensorflow.org/) 2.0 and [PyTorch](https://pytorch.org/) 1.2. You can view the complete list of installed packages by running the pip list command in the terminal.

The notebook has three tabs:

* **Files:** Lets you manage the files in the Jupyter instance. The entire Neu.ro platform storage is available for you to host and manage files on the notebook.

![](../.gitbook/assets/Jupyter_Files.JPG)

You can select a file to do context-based actions on the file, such as Shutdown or Duplicate for code and Rename or Download for text files.

* **Running:** Lists the list of terminals and Python notebooks updated with the latest status. You can also shut down a terminal or notebook from the tab.

![](../.gitbook/assets/Jupyter_Running_Tab.JPG)

* **Clusters:** Lets you manage the notebook clusters that you have access to. Currently, clusters are provided by [IPython parallel](https://github.com/ipython/ipyparallel).

![](../.gitbook/assets/Jupyter_Clusters.jpg)

## Manage your Jupyter Notebook Instances

You can manage your Jupyter Notebook instances from the Neu.ro dashboard. The Jupyter Notebook instances are listed in the Running Jobs section, and their URLs start with [http://jupyter-](http://jupyter-). You can click on the URL to open the instance.

You can view the details of your Jupyter Notebook instances by clicking on the job ID.

![](../.gitbook/assets/Jupyter_URLs_1.jpg)

You can open an instance by:

* Clicking the Open button from the Jupyter Notebooks section.

![](../.gitbook/assets/Jupyter_Open.jpg)

* Clicking on the URL for the instance.

![](../.gitbook/assets/Jupyter_Running_Jobs_IDs.jpg)

The instance is listed as a job in your Neu.ro dashboard. You can kill an instance from the Running Jobs section of your dashboard. Note that closing the tab does not kill the instance.

![](../.gitbook/assets/Jupyter_Running_AllJobs.JPG)

Alternatively, you can kill the instance by clicking on the Quit button from within the instance.

![](../.gitbook/assets/Jupyter_Quit.jpg)

An instance is automatically terminated 24 hours after initiation.

