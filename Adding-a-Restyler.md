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

Normally, you would download, compile, or otherwise install an auto-formatter
from some external source as a `RUN` step (or steps) in the Docker image build.
For example:

```dockerfile
FROM alpine
RUN apt add --update py-pip
RUN pip install black
```

**BUT**, for this tutorial, we will fabricate a simple auto-formatter to wrap.
It will be a small shell script that lives right in our local `restylers`
checkout. It's a simple tool that "auto-formats" all occurrences of the word
"apple" into "banana". I know, it's bananas.

```sh
#!/bin/sh
if [ "$1" = "--help" ]; then
  echo "usage: bananas [path, ...]" >&2
  exit 64
fi

for path; do
  sed -i 's/apple/banana/g' "$path"
done
```

And we'll use a simple `COPY` to "install" it in our Docker image.

All of our working files should live under `./<name>`, in this case `./bananas`.
You can organize these files however you want, but it can be convenient to
create a `files` sub-directory with the same structure as you would want in the
image. Therefore, I recommend putting this script at
**./bananas/files/usr/bin/bananas**, and don't forget to make it executable!

## 1. Create the Restyler

You need only two files, described below.

**./bananas/info.yaml**:

```yaml
---
name: bananas

# We'll build to a conventionally-named image and tag it with a made-up version
image: restyled/restyler-bananas:v0.0.1

# The command to run is our banana script. If there were arguments that must
# always be passed for things to function (e.g. --inplace), they would be
# included here (that's why it's an Array).
command:
- bananas

# We won't default any additional arguments. This is more for end-users, but
# the "schema" is the same as what they'll configure in this regard, so we
# have to do something here.
arguments: []

# We can (apparently) fix up any kind of file!
include:
- "**/*"

# We don't support "--" between arguments and paths
supports_arg_sep: false

# But we do support multiple paths in one invocation
supports_multiple_paths: true

# If you wanted to run on extension-less files based on their
# shebang, you could list interpreter executables (sh, ruby,
# python2, etc) here.
interpreters: []

# A list of URLs that will be displayed with any exceptions this Restyler
# generates.
documentation: []

# This key contains any data that we need in the build process, but that users
# would not interact with within their configuration.
metadata:
  # Again, we run on any kind of file.
  languages:
  - Any

  # Tests declare how the Restyler should impact mis-styled files. You can make
  # as many as you like. There is more test-related functionality available, but
  # this is the bare minimum:
  tests:
  - contents: |
      Hi, here are some apples.
    restyled: |
      Hi, here are some bananas.
```

**./bananas/Dockerfile**:

```dockerfile
FROM alpine:3.10.3
LABEL maintainer="You <you@example.com>"
RUN mkdir -p /code
WORKDIR /code
COPY files /
CMD ["bananas", "--help"]
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
