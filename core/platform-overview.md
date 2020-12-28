---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Platform Overview

### Understanding the Basics

Neu.ro puts data, models, training, and tuning at your fingertips. The platform allows you to focus on model development tasks at hand by managing critical aspects of underlying infrastructure and system integration, including resource allocation, storage and image management, sharing, secure web and terminal access to running jobs.

The key components of Neu.ro are:

* \*\*\*\*[**Environment**](environments-docker-images.md). A Docker image that can be launched on the platform. For your convenience, we provide a base image which is based on `deepo` Docker image and contains the most popular ML tools \(Tensorflow, Keras, PyTorch, TensorBoard, Jupyter Notebooks, and JupyterLab\).
* \*\*\*\*[**Storage**](storage.md). One or more volumes that can be mounted to containers running on the platform. These volumes may contain datasets or be used to store output.
* \*\*\*\*[**Job**](jobs.md). A running container with a certain amount of GPU/CPU/RAM resources allocated, and with certain storage volumes mounted to its filesystem.

Environments, storage volumes, and jobs can be published and shared among users.

Each time you start a job, Neu.ro will:

* Wait for requested resources to become available.
* Pull an environment Docker image and launch it.
* Attach storage volumes to the container's local mount points.

All your interactions with the platform are happening using neuro, the command-line interface \(CLI\) tool, in conjunction with web interface.

