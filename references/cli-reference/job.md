# job

Job operations

## Usage

```bash
neuro job [OPTIONS] COMMAND [ARGS]...
```

Job operations.

## Commands

* [neuro job run](job.md#run): Run a job with predefined resources...
* [neuro job submit](job.md#submit): Submit an image to run on the cluster
* [neuro job ls](job.md#ls): List all jobs
* [neuro job status](job.md#status): Display status of a job
* [neuro job tags](job.md#tags): List all tags submitted by the user
* [neuro job exec](job.md#exec): Execute command in a running job
* [neuro job port-forward](job.md#port-forward): Forward port\(s\) of a running job to local...
* [neuro job logs](job.md#logs): Print the logs for a container
* [neuro job kill](job.md#kill): Kill job\(s\)
* [neuro job top](job.md#top): Display GPU/CPU/Memory usage
* [neuro job save](job.md#save): Save job's state to an image
* [neuro job browse](job.md#browse): Opens a job's URL in a web browser

### run

Run a job with predefined resources...

#### Usage

```bash
neuro job run [OPTIONS] IMAGE [CMD]...
```

Run a job with predefined resources configuration.

`IMAGE` container image name.

`CMD` list will be passed as commands to model container.

#### Examples

```bash
# Starts a container pytorch:latest on a machine with smaller GPU resources
# (see exact values in `neuro config show`) and with two volumes mounted:
#   storage://<home-directory>   --> /var/storage/home (in read-write mode),
#   storage://neuromation/public --> /var/storage/neuromation (in read-only mode).
$ neuro run --preset=gpu-small --volume=HOME pytorch:latest

# Starts a container using the custom image my-ubuntu:latest stored in neuromation
# registry, run /script.sh and pass arg1 and arg2 as its arguments:
$ neuro run -s cpu-small image:my-ubuntu:latest --entrypoint=/script.sh arg1 arg2
```

#### Options

| Name | Description |
| :--- | :--- |
| `-s`, `--preset PRESET` | Predefined resource configuration \(to see available values, run `neuro config show`\) |
| `-x`, `--extshm` / `-X`, `--no-extshm` | Request extended '/dev/shm' space  _\[default: True\]_ |
| `--http PORT` | Enable HTTP port forwarding to container  _\[default: 80\]_ |
| `--http-auth` / `--no-http-auth` | Enable HTTP authentication for forwarded HTTP port  _\[default: True\]_ |
| `-n`, `--name NAME` | Optional job name |
| `--tag TAG` | Optional job tag, multiple values allowed |
| `-d`, `--description DESC` | Optional job description in free format |
| `-q`, `--quiet` | Run command in quiet mode \(DEPRECATED\) |
| `-v`, `--volume MOUNT` | Mounts directory from vault into container. Use multiple options to mount more than one volume. --volume=HOME is an alias for storage::/var/storage/home:rw and storage://neuromation/public:/var/storage/neuromation:ro |
| `--entrypoint TEXT` | Executable entrypoint in the container \(note that it overwrites `ENTRYPOINT` and `CMD` instructions of the docker image\) |
| `-e`, `--env VAR=VAL` | Set environment variable in container Use multiple options to define more than one variable |
| `--env-file PATH` | File with environment variables to pass |
| `--life-span TIMEDELTA` | Optional job run-time limit in the format '1d2h3m4s' \(some parts may be missing\). Set '0' to disable. Default value '1d' can be changed in the user config. |
| `--wait-start` / `--no-wait-start` | Wait for a job start or failure  _\[default: True\]_ |
| `--pass-config` / `--no-pass-config` | Upload neuro config to the job  _\[default: False\]_ |
| `--browse` | Open a job's URL in a web browser |
| `--detach` | Don't attach to job logs and don't wait for exit code |
| `-t`, `--tty` | Allocate a TTY |
| `--help` | Show this message and exit. |

### submit

Submit an image to run on the cluster

#### Usage

```bash
neuro job submit [OPTIONS] IMAGE [CMD]...
```

Submit an image to run on the cluster.

`IMAGE` container image name.

`CMD` list will be passed as commands to model container.

#### Examples

```bash
# Starts a container pytorch:latest with two paths mounted. Directory /q1/
# is mounted in read only mode to /qm directory within container.
# Directory /mod mounted to /mod directory in read-write mode.
$ neuro submit --volume storage:/q1:/qm:ro --volume storage:/mod:/mod:rw pytorch:latest

# Starts a container using the custom image my-ubuntu:latest stored in neuromation
# registry, run /script.sh and pass arg1 arg2 arg3 as its arguments:
$ neuro submit image:my-ubuntu:latest --entrypoint=/script.sh arg1 arg2 arg3
```

#### Options

| Name | Description |
| :--- | :--- |
| `-g`, `--gpu NUMBER` | Number of GPUs to request  _\[default: 0\]_ |
| `--gpu-model MODEL` | GPU to use  _\[default: nvidia-tesla-k80\]_ |
| `--tpu-type TYPE` | TPU type to use |
| `--tpu-sw-version VERSION` | Requested TPU software version |
| `-c`, `--cpu NUMBER` | Number of CPUs to request  _\[default: 0.1\]_ |
| `-m`, `--memory AMOUNT` | Memory amount to request  _\[default: 1G\]_ |
| `-x`, `--extshm` / `-X`, `--no-extshm` | Request extended '/dev/shm' space  _\[default: True\]_ |
| `--http PORT` | Enable HTTP port forwarding to container |
| `--http-auth` / `--no-http-auth` | Enable HTTP authentication for forwarded HTTP port  _\[default: True\]_ |
| `-p`, `--preemptible` / `-P`, `--non-preemptible` | Run job on a lower-cost preemptible instance  _\[default: False\]_ |
| `-n`, `--name NAME` | Optional job name |
| `--tag TAG` | Optional job tag, multiple values allowed |
| `-d`, `--description DESC` | Optional job description in free format |
| `-q`, `--quiet` | Run command in quiet mode \(DEPRECATED\) |
| `-v`, `--volume MOUNT` | Mounts directory from vault into container. Use multiple options to mount more than one volume. --volume=HOME is an alias for storage::/var/storage/home:rw and storage://neuromation/public:/var/storage/neuromation:ro |
| `--entrypoint TEXT` | Executable entrypoint in the container \(note that it overwrites `ENTRYPOINT` and `CMD` instructions of the docker image\) |
| `-e`, `--env VAR=VAL` | Set environment variable in container Use multiple options to define more than one variable |
| `--env-file PATH` | File with environment variables to pass |
| `--life-span TIMEDELTA` | Optional job run-time limit in the format '1d2h3m4s' \(some parts may be missing\). Set '0' to disable. Default value '1d' can be changed in the user config. |
| `--wait-start` / `--no-wait-start` | Wait for a job start or failure  _\[default: True\]_ |
| `--pass-config` / `--no-pass-config` | Upload neuro config to the job  _\[default: False\]_ |
| `--browse` | Open a job's URL in a web browser |
| `--detach` | Don't attach to job logs and don't wait for exit code |
| `-t`, `--tty` | Allocate a TTY |
| `--help` | Show this message and exit. |

### ls

List all jobs

#### Usage

```bash
neuro job ls [OPTIONS]
```

List all jobs.

#### Examples

```bash
$ neuro ps -a
$ neuro ps -a --owner=user-1 --owner=user-2
$ neuro ps --name my-experiments-v1 -s failed -s succeeded
$ neuro ps --description=my favourite job
$ neuro ps -s failed -s succeeded -q
$ neuro ps -t tag1 -t tag2
```

#### Options

| Name | Description |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `-s`, \`--status \[pending | running | succeeded | failed | all\]\` | Filter out jobs by status \(multiple option\). Note: option `all` is deprecated, use `neuro ps -a` instead. |
| `-o`, `--owner TEXT` | Filter out jobs by owner \(multiple option\). |  |  |  |  |
| `-a`, `--all` | Show all jobs regardless the status \(equivalent to `-s pending -s running -s succeeded -s failed`\). |  |  |  |  |
| `-n`, `--name NAME` | Filter out jobs by name. |  |  |  |  |
| `-t`, `--tag TAG` | Filter out jobs by tag \(multiple option\) |  |  |  |  |
| `-d`, `--description DESCRIPTION` | Filter out jobs by description \(exact match\). |  |  |  |  |
| `-q`, `--quiet` | Run command in quiet mode \(DEPRECATED\) |  |  |  |  |
| `-w`, `--wide` | Do not cut long lines for terminal width. |  |  |  |  |
| `--format COLUMNS` | Output table format, see "neuro help ps-format" for more info about the format specification. The default can be changed using the job.ps-format configuration variable documented in "neuro help user-config" |  |  |  |  |
| `--help` | Show this message and exit. |  |  |  |  |

### status

Display status of a job

#### Usage

```bash
neuro job status [OPTIONS] JOB
```

Display status of a job.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### tags

List all tags submitted by the user

#### Usage

```bash
neuro job tags [OPTIONS]
```

List all tags submitted by the user.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### exec

Execute command in a running job

#### Usage

```bash
neuro job exec [OPTIONS] JOB CMD...
```

Execute command in a running job.

#### Examples

```bash
# Provides a shell to the container:
$ neuro exec my-job /bin/bash

# Executes a single command in the container and returns the control:
$ neuro exec --no-tty my-job ls -l
```

#### Options

| Name | Description |
| :--- | :--- |
| `-t`, `--tty` / `-T`, `--no-tty` | Allocate virtual tty. Useful for interactive jobs. |
| `--no-key-check` | Disable host key checks. Should be used with caution. |
| `--timeout FLOAT` | Maximum allowed time for executing the command, 0 for no timeout  _\[default: 0\]_ |
| `--help` | Show this message and exit. |

### port-forward

Forward port\(s\) of a running job to local...

#### Usage

```bash
neuro job port-forward [OPTIONS] JOB LOCAL_REMOTE_PORT...
```

Forward port\(s\) of a running job to local port\(s\).

#### Examples

```bash
# Forward local port 2080 to port 80 of job's container.
# You can use http://localhost:2080 in browser to access job's served http
$ neuro job port-forward my-fastai-job 2080:80

# Forward local port 2222 to job's port 22
# Then copy all data from container's folder '/data' to current folder
# (please run second command in other terminal)
$ neuro job port-forward my-job-with-ssh-server 2222:22
$ rsync -avxzhe ssh -p 2222 root@localhost:/data .

# Forward few ports at once
$ neuro job port-forward my-job- 2080:80 2222:22 2000:100
```

#### Options

| Name | Description |
| :--- | :--- |
| `--no-key-check` | Disable host key checks. Should be used with caution. |
| `--help` | Show this message and exit. |

### logs

Print the logs for a container

#### Usage

```bash
neuro job logs [OPTIONS] JOB
```

Print the logs for a container.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### kill

Kill job\(s\)

#### Usage

```bash
neuro job kill [OPTIONS] JOBS...
```

Kill job\(s\).

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### top

Display GPU/CPU/Memory usage

#### Usage

```bash
neuro job top [OPTIONS] JOB
```

Display `GPU`/`CPU`/Memory usage.

#### Options

| Name | Description |
| :--- | :--- |
| `--timeout FLOAT` | Maximum allowed time for executing the command, 0 for no timeout  _\[default: 0\]_ |
| `--help` | Show this message and exit. |

### save

Save job's state to an image

#### Usage

```bash
neuro job save [OPTIONS] JOB IMAGE
```

Save job's state to an image.

#### Examples

```bash
$ neuro job save job-id image:ubuntu-patched
$ neuro job save my-favourite-job image:ubuntu-patched:v1
$ neuro job save my-favourite-job image://bob/ubuntu-patched
```

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### browse

Opens a job's URL in a web browser

#### Usage

```bash
neuro job browse [OPTIONS] JOB
```

Opens a job's `URL` in a web browser.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |
