# On-premise Installation Guide

### Requirements:

* A Linux VM :
  * OS: Ubuntu \(version:  at least 18\);
  * This VM can be accessed by the Kubernetes cluster \(the **neuro** docker registry will be installed on this VM – it contains all docker images needed for the neu.ro platform\);
  * This VM has to be able to access the Kubernetes cluster \(_**setup\_k8s.sh**_ script needs a connection to cluster\);
  * It must have these utilities installed: _**docker, helm2, kubectl, htpasswd, openssl**._
* Storage:
  * Must be able to use **OpenEBS Cstor** – the disks have to be attached to the Kubernetes nodes. These disks have to be unmounted and unformatted, and be visible as block devices in the Kubernetes cluster in order to be used by **OpenEBS Cstor**.
* Kubernetes cluster:
  * The **busybox:1.32.0** docker image should be pre-pulled on each node.
* SSL certificate for **Traefik** \(optional\).

### Neuro.tar archive and its structure

To get the latest version of the archive containing all files necessary for the installation of the platform, contact our team at [team@neu.ro](mailto:team@neu.ro)

The structure of this archive is as follows:

#### _**/chartmuseum**_

A directory with all needed helm charts. It has to be mounted as a volume for the **chartmuseum** helm repository.

#### _**/registry**_

A directory with all needed docker images. It has to be mounted as a volume for the **registry** docker repository.    

#### _**registry.tar**_

**registry:2** docker image in **.tar** format.

#### _**chartmuseum.tar**_

**chartmuseum/chartmuseum:latest** docker image in **.tar** format.

#### _**neuro\_config.sh**_

A config file with all needed data for the install script. It has to be adjusted with regards to required values in each case.

#### _**install.sh**_

A bash script that will be executed on the dedicated registry machine. It does the following:

* Creates a self-signed SSL certificate;
* Starts the docker registry;
* Starts the **chartmuseum** helm repository;

#### _**setup\_k8s.sh**_

A bash script that will be executed on the dedicated registry machine. It does the following:

* Prepares the Kubernetes cluster for the **neu.ro** platform:
  * Creates necessary additional namespaces;
  * Creates secrets with self-signed SSL certificates and credentials to access the **neuro** docker registry VM;
  * Patches Kubernetes service accounts;
* Installs **PostgreSQL**, **redis**, and the storage solution in Kubernetes;
* Installs the **neu.ro** platform.

### How to Install

1. Connect to the VM;

2. Mount the USB or the external storage device;

3. Make a directory to which **neuro.tar** will be extracted and extract the archive:

```text
INSTALL_DIR=”/var/neuro”
mkdir –p $INSTALL_DIR
tar -xvf neuro.tar --directory $INSTALL_DIR
```

4. Adjust the values in _**neuro.config.sh**._

* Mandatory values: 
  * `DOCKER_REGISTRY_VM_LOCAL_IP` \(the private IP of the VM on which the **neuro** registry will be installed\).

5. Run _**install.sh**:_

```text
cd $INSTALL_DIR
chmod +x neuro_config.sh
chmod +x install.sh
./install.sh
```

6. Connect to the Kubernetes cluster;

7. Run the script that prepares the Kubernetes cluster for the **neu.ro** platform:

```text
chmod +x setup_k8s.sh
./setup_k8s.sh
```

