# buf-build

Rebuild selected Buf plugin images from the upstream
[`bufbuild/plugins`](https://github.com/bufbuild/plugins) Dockerfiles and publish them to
GitHub Container Registry.

This repository does not keep a plugin manifest. Releases are selected by Git tags.

## Tag Format

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

## Local Build

```sh
scripts/build-plugin protocolbuffers-go/v1.36.10
```

By default this builds `linux/amd64` and loads it into the local Docker image store.
Set `PLATFORM` to build a different single platform:

```sh
PLATFORM=linux/arm64 scripts/build-plugin protocolbuffers-go/v1.36.10
```

## Extract a Linux Plugin Binary

After building or pulling one of the images:

```sh
scripts/extract-plugin ghcr.io/nekomeowww/buf-build/protocolbuffers-go:v1.36.10 .tools/protoc-plugins
```

Then use the extracted executable from `buf.gen.yaml`:

```yaml
plugins:
  - local: .tools/protoc-plugins/protoc-gen-go
    out: apis/sdk/go
    opt: paths=source_relative
```

The extracted binary is a Linux binary. It can be used directly in Linux CI, or inside a
Linux container that runs `buf generate`. It will not run directly on macOS.
