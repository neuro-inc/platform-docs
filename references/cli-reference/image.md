# image

Container image operations

## Usage

```bash
neuro image [OPTIONS] COMMAND [ARGS]...
```

Container image operations.

## Commands

* [neuro image ls](image.md#ls): List images
* [neuro image push](image.md#push): Push an image to platform registry
* [neuro image pull](image.md#pull): Pull an image from platform registry
* [neuro image tags](image.md#tags): List tags for image in platform registry

### ls

List images

#### Usage

```bash
neuro image ls [OPTIONS]
```

List images.

#### Options

| Name         | Description                 |
| ------------ | --------------------------- |
| `-l`         | List in long format.        |
| `--full-uri` | Output full image URI.      |
| `--help`     | Show this message and exit. |

### push

Push an image to platform registry

#### Usage

```bash
neuro image push [OPTIONS] LOCAL_IMAGE [REMOTE_IMAGE]
```

Push an image to platform registry.

Remote image must be `URL` with image://
scheme.
Image names can contain tag. If tags not specified 'latest' will
be
used as value.

#### Examples

```bash

$ neuro push myimage
$ neuro push alpine:latest image:my-alpine:production
$ neuro push alpine image://myfriend/alpine:shared
```

#### Options

| Name            | Description                            |
| --------------- | -------------------------------------- |
| `-q`, `--quiet` | Run command in quiet mode (DEPRECATED) |
| `--help`        | Show this message and exit.            |

### pull

Pull an image from platform registry

#### Usage

```bash
neuro image pull [OPTIONS] REMOTE_IMAGE [LOCAL_IMAGE]
```

Pull an image from platform registry.

Remote image name must be `URL` with
image:// scheme.
Image names can contain tag.

#### Examples

```bash

$ neuro pull image:myimage
$ neuro pull image://myfriend/alpine:shared
$ neuro pull image://username/my-alpine:production alpine:from-registry
```

#### Options

| Name            | Description                            |
| --------------- | -------------------------------------- |
| `-q`, `--quiet` | Run command in quiet mode (DEPRECATED) |
| `--help`        | Show this message and exit.            |

### tags

List tags for image in platform registry

#### Usage

```bash
neuro image tags [OPTIONS] IMAGE
```

List tags for image in platform registry.

Image name must be `URL` with
image:// scheme.

#### Examples

```bash

$ neuro image tags image://myfriend/alpine
$ neuro image tags image:myimage
```

#### Options

| Name     | Description                 |
| -------- | --------------------------- |
| `--help` | Show this message and exit. |
