# GitHub Action to install and setup the Buildpacks [`pack` CLI](https://buildpacks.io/docs/tools/pack/)

[![Build](https://github.com/imjasonh/setup-pack/actions/workflows/use-action.yaml/badge.svg)](https://github.com/imjasonh/setup-pack/actions/workflows/use-action.yaml)

## Example usage

```yaml
name: Publish

on:
  push:
    branches: ['main']

jobs:
  publish:
    name: Publish
    runs-on: ubuntu-latest
    steps:
      - uses: actions/setup-go@v2
        with:
          go-version: 1.15
      - uses: actions/checkout@v2

      - uses: imjasonh/setup-pack@main
      - run: pack build ...
```

Learn more about [`pack build`](https://buildpacks.io/docs/tools/pack/cli/pack_build/) or other available [CLI commands](https://buildpacks.io/docs/tools/pack/cli/).

This action installs the `pack` CLI and configures credentials to authorize pushes to the [GitHub Container Registry](https://ghcr.io).

### Select `pack` version to install

By default, `imjasonh/setup-pack` installs the latest released version of `pack`.

You can select a version with the `version` parameter:

```yaml
- uses: imjasonh/setup-pack@main
  with:
    version: v0.17.0
```

To build and install `pack` from source using `go get`, specify `version: tip`.

### Pushing to other registries

By default, `imjasonh/setup-pack` configures authorization so `pack` can push images to [GitHub Container Registry](https://ghcr.io), but you can configure it to push to other registries as well.

To do this, you need to provide credentials to authorize the push.
You can use [encrypted secrets](https://docs.github.com/en/actions/reference/encrypted-secrets) to store the authorization token, and pass it to `docker login` before pushing:

```
- uses: imjasonh/setup-pack@main
- env:
    auth_token: ${{ secrets.auth_token }}
  run: |
    echo "${auth_token}" | docker login https://my.registry --username my-username --password-stdin
    pack build ...
```
