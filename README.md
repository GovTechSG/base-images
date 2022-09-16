# Base Containers

[![Build Status](https://github.com/govtechsg/base-images/actions/workflows/build.yaml/badge.svg)](https://github.com/govtechsg/base-images/actions/workflows/build.yaml/)

This repository is a collection of Docker images we use internally for testing out runtimes/frameworks.

Daily builds are run against these images and automatically sent to our DockerHub repository at:

https://gallery.ecr.aws/govtechsg/base-images

## Methodology
All runtimes are built from official sources using the methods documented in the runtimes' official documentation.

## Catalog (Alphabetical Order)

- Node.js (`node*`)

### Release Notes
The images are found in the [Public ECR registry](https://gallery.ecr.aws/govtechsg/base-images), and the names of the different types of images are added as a tag. For example given a type of image called `xyz`, it will be available under the repository URL `public.ecr.aws/govtechsg/base-images:xyz-latest`.

### Usage/Description

#### Node (`node*`)
Canonical Tag: `node*-<NODE_VERSION>`
Latest URL: `public.ecr.aws/govtechsg/base-images:node-latest`

The `*` is available for versions of Node which satisfy the following criteria:

1. is an LTS release
2. is the latest non-LTS release

##### Notes
Generally compiled Node.js binary that comes with Yarn.

General compilation allows for debugging and performance profiling.

Available runtime commands:

- `npm`
- `node`
- `yarn`

## Other Uses
Images specified here can be uploaded to other repositories if you so wish. The commands are:

### For Building
The build script creates the build for the specified image:

```bash
REPO=__URL_TO_REPO__
IMAGE_NAME=__DIRECTORY_NAME__
./.scripts/.build.sh "${REPO}" "${IMAGE_NAME}"
```

An example to upload to a ECR at `public.ecr.aws/yourusername/yourimage:dind-latest`:

```bash
REPO="public.ecr.aws/yourusername/yourimage"
IMAGE_NAME="dind"
./.scripts/.build.sh "${REPO}" "${IMAGE_NAME}"
```

### For Publishing
The publish script in each directory sends your built image to DockerHub and relies on the `./.scripts/.build.sh` script being run prior to it. The general format of usage:

```bash
REPO=__URL_TO_REPO__
IMAGE_NAME=__DIRECTORY_NAME__
./${IMAGE_NAME}/.publish.sh "${REPO}:${IMAGE_NAME}"
```

A corresponding example to upload to a DockerHub repository at `public.ecr.aws/yourusername/yourimage:dind-latest`:

```bash
REPO="public.ecr.aws/yourusername/yourimage"
IMAGE_NAME="dind"
./${IMAGE_NAME}/.publish.sh "${REPO}:${IMAGE_NAME}"
```

Each directory and type of image has its own publish script because of the different ways they are versioned.
