Restylers can be added by anyone through a Pull Request on the `restylers`
repository. There is no burden of popularity or usefulness. Most Restylers can
even be configured to run by default, provided they don't conflict with other
Restylers that operate on the same file-types.

## Prerequisites

1. Docker
1. `git`
1. `make`
1. `bash`
1. `ruby` and `rspec`

To get started, check out the `restyled-io/restylers`, repository:

```console
git clone https://github.com/restyled-io/restylers
cd restylers
```

## 0. The Auto-formatter

For this tutorial, we will fabricate our own simple auto-formatter to wrap:

```sh
#!/bin/sh
for path; do
  sed -i 's/apple/banana/g' "$path"
done
```

Place this script at `./bananas/files/usr/bin/bananas`, and make it executable.

## 1. Create the Restyler

You need only two files, described below.

**./bananas/info.yaml**:

```yaml
---
name: bananas
version: v0.0.1
include:
  - "**/*"
supports_arg_sep: false
metadata:
  languages:
    - Any
  tests:
    - contents: |
        Hi, here are some apples.
      restyled: |
        Hi, here are some bananas.
```

See [here](https://github.com/restyled-io/restyled.io/wiki/Restyler-Info-Yaml) for documentation on this file.

**./bananas/Dockerfile**:

```dockerfile
FROM alpine:3.10.3
LABEL maintainer="You <you@example.com>"
RUN mkdir -p /code
WORKDIR /code
COPY files /
CMD ["bananas"]
```

## 2. Test locally

Build the Docker image and run the tests:

```console
make bananas/Dockerfile.tested
```

**NOTE**: if this doesn't work, and you can't make it work, please still submit
the PR and we'll help you out through its review.

## Prepare CI

To have this test run on CI, make a small change to `.travis.yaml`:

```diff
  include:
    - env: RESTYLER=astyle
    - env: RESTYLER=autopep8
+   - env: RESTYLER=bananas
    - env: RESTYLER=black
    - env: RESTYLER=brittany
    - env: RESTYLER=clang-format
    - env: RESTYLER=dfmt
```

That's it!
