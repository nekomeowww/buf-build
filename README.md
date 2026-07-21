# buf-build

Build selected upstream [`bufbuild/plugins`](https://github.com/bufbuild/plugins)
Dockerfiles and publish them to GitHub Container Registry.

This repository only builds and publishes plugin images. Downstream repositories decide how
to consume those images.

## Tags

Use:

```text
<family>-<plugin>/<version>
```

Examples:

```text
protocolbuffers-go/v1.36.10
protocolbuffers-python/v35.1
protocolbuffers-pyi/v35.1
grpc-go/v1.5.1
grpc-python/v1.75.1
community-nipunn1313-mypy-grpc/v3.6.0
```

The tag maps directly to the upstream submodule path:

```text
upstream/bufbuild-plugins/plugins/<family>/<plugin>/<version>/Dockerfile
```

And publishes:

```text
ghcr.io/nekomeowww/buf-build/<family>-<plugin>:<version>
```

## Publish

Create and push a tag:

```sh
git tag protocolbuffers-go/v1.36.10
git push origin protocolbuffers-go/v1.36.10
```

GitHub Actions will build the upstream Dockerfile and push a multi-platform image to GHCR.
