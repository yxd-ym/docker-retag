[![Go Report Card][go-report-card-badge]][go-report-card-link]
[![License][license-badge]][license-link]

# Docker Retag

🐳 Retag an existing Docker image without the overhead of pulling and pushing

This is a fork of https://github.com/joshdk/docker-retag . Enhanced with docker integration.

## Motivation

There are certain situation where it is desirable to give an existing Docker image an additional tag. This is usually acomplished by a `docker pull`, followed by a `docker tag` and a `docker push`.

That approach has the downside of downloading the contents of every layer from Docker Hub, which has bandwidth and performance implications, especially in a CI environment.

This tool uses the [Docker Hub API](https://docs.docker.com/registry/spec/api/) to pull and push only a tiny [manifest](https://docs.docker.com/registry/spec/manifest-v2-2/) of the layers, bypassing the download overhead. Using this approach, an image of any size can be retagged in approximately 2 seconds.

## Installing

### From source

You can use `go get` to install this tool by running:

```bash
$ go get -u github.com/yxd-ym/docker-retag
```

## Usage

### Setup

Since `docker-retag` communicates with the [Docker Hub](https://hub.docker.com/) API, it will use the credential of your docker config.
Use docker login to login to dockerhub first.

```bash
$ docker login
# Use your login credentials to login
```

The credentials must have both pull and push access for the Docker repository you are retagging.

### Examples

This tool can be used in a few simple ways. The simplest of which is using a
source image reference (similar to anything you could pass to `docker tag`) and
a target tag.

##### Referencing a source image by tag name.

```bash
$ docker-retag joshdk/hello-world:1.0.0 1.0.1
  Retagged joshdk/hello-world:1.0.0 as joshdk/hello-world:1.0.1
```

##### Referencing a source image by `sha256` digest.

```bash
$ docker-retag joshdk/hello-world@sha256:933f...3e90 1.0.1
  Retagged joshdk/hello-world@sha256:933f...3e90 as joshdk/hello-world:1.0.1
```

##### Referencing an image only by name will default to using `latest`.

```bash
$ docker-retag joshdk/hello-world 1.0.1
  Retagged joshdk/hello-world:latest as joshdk/hello-world:1.0.1
```

Additionally, you can pass the image name, source reference, and target tag as seperate arguments.

```bash
$ docker-retag joshdk/hello-world 1.0.0 1.0.1
  Retagged joshdk/hello-world:1.0.0 as joshdk/hello-world:1.0.1
```

```bash
$ docker-retag joshdk/hello-world @sha256:933f...3e90 1.0.1
  Retagged joshdk/hello-world@sha256:933f...3e90 as joshdk/hello-world:1.0.1
```

In all cases, the image and source reference **must** already exist in Docker Hub.

## License

This library is distributed under the [MIT License][license-link], see [LICENSE.txt][license-file] for more information.

[go-report-card-badge]:   https://goreportcard.com/badge/github.com/yxd-ym/docker-retag
[go-report-card-link]:    https://goreportcard.com/report/github.com/yxd-ym/docker-retag
[license-badge]:          https://img.shields.io/github/license/yxd-ym/docker-retag.svg
[license-file]:           https://github.com/yxd-ym/docker-retag/blob/master/LICENSE.txt
[license-link]:           https://opensource.org/licenses/MIT
