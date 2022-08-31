Restylers can be added by anyone through a Pull Request on the `restylers`
repository. There is no burden of popularity or usefulness. Most Restylers can
even be configured to run by default, provided they don't conflict with other
Restylers that operate on the same file-types.

:hand: *If the Restyler you're planning to add is just a modified version of an existing one, **don't follow these instructions**. Instead, you can do something simpler, called an *override* Restyler. See [`prettier-markdown`](https://github.com/restyled-io/restylers/blob/main/prettier-markdown/info.yaml) and [`prettier-yaml`](https://github.com/restyled-io/restylers/blob/main/prettier-yaml/info.yaml) as examples that override the `prettier` Restyler.*

## Prerequisites

1. `git`
1. Docker
1. The [Restyled SDK](https://github.com/restyled-io/sdk#installation)

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
version_cmd: |
  echo "v0.0.1"
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

:heavy_check_mark:  It's very important to add `tests`. Since I'm not always familiar with the language, conventions, or other
aspects of the auto-formatters we run, we need to have these assertions that it's working as intended when
you add it and as it's maintained and upgraded over time.

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

Build (and lint) the Docker image and run the tests:

```console
restyled restylers bananas
```

**NOTE**: if this doesn't work, and you can't make it work, please still submit
the PR and we'll help you out through its review.

## Prepare CI

To add the new Restyler to CI, make a small change to `.github/workflows/ci.yml`:

```diff
  restyler:
    - astyle
    - autopep8
+   - bananas
    - black
    - brittany
    - clang-format
    - dfmt
```

That's it! Open a Pull Request and we'll go from there.

## When will you see your changes?

When your change lands in `main`, we will promote them to the `dev` channel.
This should make it easy for you to use yourself and impact any other
users who run `dev`.  See [here](https://github.com/restyled-io/restyled.io/wiki/Restyler-Versions)
for how channels work generally.

On the 1st and 15th of every month, `dev` is promoted to `stable`, releasing your changes to
our entire user base.
