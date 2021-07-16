# Running Elastic Horovod Training

[Horovod](https://github.com/horovod/horovod) is a distributed deep learning training framework that helps make the learning process faster and easier.

Horovod is great when it comes to running training in a job on preemptible or spot instances, as there's a dedicated elastic mode for cases like this. Elastic training enables Horovod to scale the number of workers up and down dynamically at runtime.

## Running Horovod with TensorFlow

We created a script for running Horovod with TensorFlow based on the official [TensorFlow tutorial](https://www.tensorflow.org/tutorials/quickstart/beginner) and extended it with the help of this [Horovod tutorial](https://horovod.readthedocs.io/en/stable/elastic.html).

Here's how you can use it with Neu.ro.

### Preparation

You can clone the [repository containing this script](https://github.com/neuro-inc/mlops-horovod-example) by running:

```text
$ git clone https://github.com/neuro-inc/mlops-horovod-example.git
```

Once it's cloned, switch to its local root folder:

```text
$ cd <local-repo-path>
```

Then, generate a SSH key to allow SSH-based communication between jobs:

```text
$ ssh-keygen -t rsa -b 4096 -f ssh-keys/id_rsa -q -N ""
```

Now, store the private part of the SSH key. It will be used by the main node to coordinate secondary nodes:

```text
$ neuro secret add horovod-id-rsa @ssh-keys/id_rsa
```

Store the public part of the SSH key which will be used by the secondary nodes to validate the main node:

```text
$ neuro secret add horovod-id-rsa-pub @ssh-keys/id_rsa.pub
```

### Launching the main training node

To run the main training node, execute the following command:

```text
$ neuro-flow run main
```

 this job will additionally wait for 600 seconds for the secondary nodes to appear \(see "--start-timeout 600" in the `main` job's `bash` section\).

### Launching secondary nodes

Execute this command a few times to spawn worker nodes:

```text
$ neuro-flow run secondary
```

For this example, at least two workers are needed \(see "--num-proc 2" in the `main` job's `bash` description\). All of these worker nodes will be connected to the Horovod instance. 

You could also update the number of secondary nodes during the training process. Each of them will be connected and synchronized with the main training process - this is the main feature of Horovod in elastic mode.



