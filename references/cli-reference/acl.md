# acl

Access Control List management

## Usage

```bash
neuro acl [OPTIONS] COMMAND [ARGS]...
```

Access Control List management.

## Commands

* [neuro acl grant](acl.md#grant): Shares resource with another user
* [neuro acl revoke](acl.md#revoke): Revoke user access from another user
* [neuro acl list](acl.md#list): List shared resources

### grant

Shares resource with another user

#### Usage

```bash
neuro acl grant [OPTIONS] URI USER [read|write|manage]
```

Shares resource with another user.

`URI` shared resource.

`USER` username to share resource with.

`PERMISSION` sharing access right: read, write, or manage.

#### Examples

```bash
$ neuro acl grant storage:///sample_data/ alice manage
$ neuro acl grant image:resnet50 bob read
$ neuro acl grant job:///my_job_id alice write
```

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### revoke

Revoke user access from another user

#### Usage

```bash
neuro acl revoke [OPTIONS] URI USER
```

Revoke user access from another user.

`URI` previously shared resource to revoke.

`USER` to revoke `URI` resource from.

#### Examples

```bash
$ neuro acl revoke storage:///sample_data/ alice
$ neuro acl revoke image:resnet50 bob
$ neuro acl revoke job:///my_job_id alice
```

#### Options

| Name | Description |
| :--- | :--- |
| `--help` | Show this message and exit. |

### list

List shared resources

#### Usage

```bash
neuro acl list [OPTIONS]
```

List shared resources.

The command displays a list of resources shared BY current user \(default\).

To display a list of resources shared `WITH` current user apply --shared option.

#### Examples

```bash
$ neuro acl list
$ neuro acl list --scheme storage
$ neuro acl list --shared
$ neuro acl list --shared --scheme image
```

#### Options

| Name | Description |
| :--- | :--- |
| `-u TEXT` | Use specified user or role. |
| `-s`, `--scheme TEXT` | Filter resources by scheme, e.g. job, storage, image or user. |
| `--shared` | Output the resources shared by the user. |
| `--help` | Show this message and exit. |

