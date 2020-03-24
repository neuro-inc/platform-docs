# admin

Cluster administration commands

## Usage

```bash
neuro admin [OPTIONS] COMMAND [ARGS]...
```

Cluster administration commands.

## Commands

- [neuro admin get-clusters](admin.md#get-clusters): Print the list of available clusters
- [neuro admin generate-cluster-config](admin.md#generate-cluster-config): Create a cluster configuration file
- [neuro admin add-cluster](admin.md#add-cluster): Create a new cluster and start its...
- [neuro admin get-cluster-users](admin.md#get-cluster-users): Print the list of all users in the cluster...
- [neuro admin add-cluster-user](admin.md#add-cluster-user): Add user access to specified cluster
- [neuro admin remove-cluster-user](admin.md#remove-cluster-user): Remove user access from the cluster
- [neuro admin set-user-quota](admin.md#set-user-quota): Set user quota to given values
- [neuro admin add-user-quota](admin.md#add-user-quota): Add given values to user quota

### get-clusters

Print the list of available clusters

#### Usage

```bash
neuro admin get-clusters [OPTIONS]
```

Print the list of available clusters.

#### Options

| Name     | Description                 |
| -------- | --------------------------- |
| `--help` | Show this message and exit. |

### generate-cluster-config

Create a cluster configuration file

#### Usage

```bash
neuro admin generate-cluster-config [OPTIONS] [CONFIG]
```

Create a cluster configuration file.

#### Options

| Name               | Description                 |
| ------------------ | --------------------------- |
| `--type [aws|gcp]` |                             |
| `--help`           | Show this message and exit. |

### add-cluster

Create a new cluster and start its...

#### Usage

```bash
neuro admin add-cluster [OPTIONS] CLUSTER_NAME CONFIG
```

Create a new cluster and start its provisioning.

#### Options

| Name     | Description                 |
| -------- | --------------------------- |
| `--help` | Show this message and exit. |

### get-cluster-users

Print the list of all users in the cluster...

#### Usage

```bash
neuro admin get-cluster-users [OPTIONS] [CLUSTER_NAME]
```

Print the list of all users in the cluster with their assigned role.

#### Options

| Name     | Description                 |
| -------- | --------------------------- |
| `--help` | Show this message and exit. |

### add-cluster-user

Add user access to specified cluster

#### Usage

```bash
neuro admin add-cluster-user [OPTIONS] CLUSTER_NAME USER_NAME [ROLE]
```

Add user access to specified cluster.

The command supports one of 3 user
roles: admin, manager or user.

#### Options

| Name     | Description                 |
| -------- | --------------------------- |
| `--help` | Show this message and exit. |

### remove-cluster-user

Remove user access from the cluster

#### Usage

```bash
neuro admin remove-cluster-user [OPTIONS] CLUSTER_NAME USER_NAME
```

Remove user access from the cluster.

#### Options

| Name     | Description                 |
| -------- | --------------------------- |
| `--help` | Show this message and exit. |

### set-user-quota

Set user quota to given values

#### Usage

```bash
neuro admin set-user-quota [OPTIONS] CLUSTER_NAME USER_NAME
```

Set user quota to given values

#### Options

| Name                     | Description                                      |
| ------------------------ | ------------------------------------------------ |
| `-g`, `--gpu AMOUNT`     | GPU quota value in hours (h) or minutes (m).     |
| `-n`, `--non-gpu AMOUNT` | Non-GPU quota value in hours (h) or minutes (m). |
| `--help`                 | Show this message and exit.                      |

### add-user-quota

Add given values to user quota

#### Usage

```bash
neuro admin add-user-quota [OPTIONS] CLUSTER_NAME USER_NAME
```

Add given values to user quota

#### Options

| Name                     | Description                                                 |
| ------------------------ | ----------------------------------------------------------- |
| `-g`, `--gpu AMOUNT`     | Additional GPU quota value in hours (h) or minutes (m).     |
| `-n`, `--non-gpu AMOUNT` | Additional non-GPU quota value in hours (h) or minutes (m). |
| `--help`                 | Show this message and exit.                                 |
