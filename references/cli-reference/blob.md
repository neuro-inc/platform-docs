# blob

Blob storage operations

## Usage

```bash
neuro blob [OPTIONS] COMMAND [ARGS]...
```

Blob storage operations.

## Commands

* [neuro blob cp](blob.md#cp): Simple utility to copy files and directories...
* [neuro blob ls](blob.md#ls): List buckets or bucket contents
* [neuro blob glob](blob.md#glob): List resources that match PATTERNS

### cp

Simple utility to copy files and directories...

#### Usage

```bash
neuro blob cp [OPTIONS] [SOURCES]... [DESTINATION]
```

Simple utility to copy files and directories into and from Blob Storage.
Either `SOURCES` or `DESTINATION` should have `blob://` scheme.
If scheme is
omitted, file:// scheme is assumed. It is currently not possible to
copy files
between Blob Storage (`blob://`) destination, nor with `storage://`
scheme
paths.
Use `/dev/stdin` and `/dev/stdout` file names to upload a file from
standard input
or output to stdout.
File permissions, modification times and
other attributes will not be passed to
Blob Storage metadata during upload.

#### Options

| Name                                       | Description                                                                                                                                                                                 |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-r`, `--recursive`                        | Recursive copy, off by default                                                                                                                                                              |
| `--glob` / `--no-glob`                     | Expand glob patterns in SOURCES with explicit scheme.  _[default: True]_                                                                                                                    |
| `-t`, `--target-directory DIRECTORY`       | Copy all SOURCES into DIRECTORY.                                                                                                                                                            |
| `-T`, `--no-target-directory`              | Treat DESTINATION as a normal file.                                                                                                                                                         |
| `--exclude`                                | Exclude files and directories that match the specified pattern. The default can be changed using the storage.cp-exclude configuration variable documented in "neuro help user-config"       |
| `--include`                                | Don't exclude files and directories that match the specified pattern. The default can be changed using the storage.cp-exclude configuration variable documented in "neuro help user-config" |
| `-p`, `--progress` / `-P`, `--no-progress` | Show progress, on by default.                                                                                                                                                               |
| `--help`                                   | Show this message and exit.                                                                                                                                                                 |

### ls

List buckets or bucket contents

#### Usage

```bash
neuro blob ls [OPTIONS] [PATHS]...
```

List buckets or bucket contents.

#### Options

| Name                      | Description                                                         |
| ------------------------- | ------------------------------------------------------------------- |
| `-h`, `--human-readable`  | with -l print human readable sizes (e.g., 2K, 540M).                |
| `-l`                      | use a long listing format.                                          |
| `--sort [name|size|time]` | sort by given field, default is name.                               |
| `-r`, `--recursive`       | List all keys under the URL path provided, not just 1 level depths. |
| `--help`                  | Show this message and exit.                                         |

### glob

List resources that match PATTERNS

#### Usage

```bash
neuro blob glob [OPTIONS] [PATTERNS]...
```

List resources that match `PATTERNS`.

#### Options

| Name     | Description                 |
| -------- | --------------------------- |
| `--help` | Show this message and exit. |
