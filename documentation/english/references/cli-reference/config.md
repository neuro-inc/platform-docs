# config

Client configuration

## Usage

```bash
neuro config [OPTIONS] COMMAND [ARGS]...
```

Client configuration.

## Commands

* [neuro config login](config.md#login): Log into Neuro Platform
* [neuro config login-with-token](config.md#login-with-token): Log into Neuro Platform with token
* [neuro config login-headless](config.md#login-headless): Log into Neuro Platform from non-GUI server...
* [neuro config show](config.md#show): Print current settings
* [neuro config show-token](config.md#show-token): Print current authorization token
* [neuro config show-quota](config.md#show-quota): Print quota and remaining computation time...
* [neuro config aliases](config.md#aliases): List available command aliases
* [neuro config get-clusters](config.md#get-clusters): Fetch and display the list of available...
* [neuro config switch-cluster](config.md#switch-cluster): Switch the active cluster
* [neuro config docker](config.md#docker): Configure docker client to fit the Neuro...
* [neuro config logout](config.md#logout): Log out

### login

Log into Neuro Platform

#### Usage

```bash
neuro config login [OPTIONS] [URL]
```

Log into Neuro Platform.

`URL` is a platform entrypoint `URL`.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### login-with-token

Log into Neuro Platform with token

#### Usage

```bash
neuro config login-with-token [OPTIONS] TOKEN [URL]
```

Log into Neuro Platform with token.

`TOKEN` is authentication token provided by administration team. `URL` is a platform entrypoint `URL`.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### login-headless

Log into Neuro Platform from non-GUI server...

#### Usage

```bash
neuro config login-headless [OPTIONS] [URL]
```

Log into Neuro Platform from non`-GUI` server environment.

`URL` is a platform entrypoint `URL`.

The command works similar to "neuro login" but instead of opening a browser for performing OAuth registration prints an `URL` that should be open on guest host.

Then user inputs a code displayed in a browser after successful login back in neuro command to finish the login process.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### show

Print current settings

#### Usage

```bash
neuro config show [OPTIONS]
```

Print current settings.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### show-token

Print current authorization token

#### Usage

```bash
neuro config show-token [OPTIONS]
```

Print current authorization token.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### show-quota

Print quota and remaining computation time...

#### Usage

```bash
neuro config show-quota [OPTIONS] [USER]
```

Print quota and remaining computation time for active cluster.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### aliases

List available command aliases

#### Usage

```bash
neuro config aliases [OPTIONS]
```

List available command aliases.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### get-clusters

Fetch and display the list of available...

#### Usage

```bash
neuro config get-clusters [OPTIONS]
```

Fetch and display the list of available clusters.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### switch-cluster

Switch the active cluster

#### Usage

```bash
neuro config switch-cluster [OPTIONS] [CLUSTER_NAME]
```

Switch the active cluster.

`CLUSTER`\_`NAME` is the cluster name to select. The interactive prompt is used if the name is omitted \(default\).

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### docker

Configure docker client to fit the Neuro...

#### Usage

```bash
neuro config docker [OPTIONS]
```

Configure docker client to fit the Neuro Platform.

#### Options

| Name | Description |
| :--- | :--- |
| `--docker-config PATH` | Specifies the location of the Docker client configuration files |
| `--help` | Show this message and exit. |

### logout

Log out

#### Usage

```bash
neuro config logout [OPTIONS]
```

Log out.

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

