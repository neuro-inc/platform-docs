# Installing CLI

To start working with CLI, you have two options:

* Use [Web Terminal](https://apps.neu.ro/shell)
* Install Neu.ro CLI on your machine and run `neuro login`

The first option is recommended for the [Meeting Neu.ro](meeting-neu.ro.md) section. The second option is better when working with projects \(see [Developing on GPU](developing-on-gpu.md) section\).

### Linux and Mac OS instructions

Neu.ro CLI requires Python 3 installed \(recommended: 3.7, required: &gt;=3.6\). We suggest using [Anaconda Python 3.7 Distribution](https://www.anaconda.com/distribution/).

```text
pip install -U neuromation
neuro login
```

### Windows instructions

While there are several options to make Neu.ro CLI work on Windows, we highly recommend using [Anaconda Python 3.7 Distribution](https://www.anaconda.com/distribution/) with default installation settings.

When you have it up and running, run the following commands in Conda Prompt:

```text
conda install -c conda-forge make
conda install -c conda-forge git    
pip install -U neuromation
pip install -U certifi
neuro login
```

