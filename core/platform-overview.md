# Platform Overview

### Understanding the Basics

Neu.ro puts data manipulation, model management, and training at your fingertips. The platform allows you to focus on model development tasks at hand by managing critical aspects of infrastructure and system integration, including resource allocation and storage, image management and sharing, secure web and terminal access to running jobs.

The key components of Neu.ro are:

* \*\*\*\*[**Environment**](working-with-the-platform/environments-docker-images.md). A Docker image that can be launched on the platform. For your convenience, we provide a base image that is built on the `deepo` Docker image and contains the most popular ML tools \(Tensorflow, Keras, PyTorch, TensorBoard, Jupyter Notebooks, and JupyterLab\).
* \*\*\*\*[**Storage**](platform-storage/storage.md). One or more volumes that can be mounted to containers running on the platform. These volumes may contain datasets or be used to store output.
* \*\*\*\*[**Job**](working-with-the-platform/jobs.md). A running container with a certain amount of allocated GPU/CPU/RAM resources and  certain storage volumes mounted to its filesystem.

Environments, storage volumes, and jobs can be published and shared among users.

Each time you start a job, Neu.ro will:

* Wait for requested resources to become available.
* Pull an environment Docker image and launch it.
* Attach storage volumes to the container's local mount points.

All your interactions with the platform are happening either through the command-line interface \(CLI\) tool or through the web interface.

