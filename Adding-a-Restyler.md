Restylers can be added by anyone through a Pull Request on the `restylers` repository. There is no burden of popularity or usefulness. Most Restylers can even be configured to run by default, provided they don't conflict with other Restylers that operate on the same file-types.

## Prerequisites

To create and build a Restyler image that will pass CI in a PR:

1. `make`
1. `git`
1. A working Docker setup, and some general Docker knowledge

To run the tests this tutorial also has you write:

1. `bash`
1. [`cram`](https://bitheap.org/cram/)

To get started, check out the `restyled-io/restylers`, repository:

```console
git clone https://github.com/restyled-io/restylers
cd restylers
```

## 0. The Auto-formatter

Normally, you would download, compile, or otherwise install an auto-formatter from some external source as a `RUN` step (or steps) in the Docker image build. For example:

```dockerfile
FROM alpine
RUN apt add --update py-pip
RUN pip install black
```

**BUT**, for this tutorial, we will fabricate a simple auto-formatter to wrap. It will be a small shell script that lives right in our local `restylers` checkout. It's a simple tool that "auto-formats" all occurrences of the word "apple" into "banana". I know, it's bananas.

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

All of our working files should live under `./<name>`, in this case `./bananas`. You can organize these files however you want, but it can be convenient to create a `files` sub-directory with the same structure as you would want in the image. Therefore, I recommend putting this script at **./bananas/files/usr/bin/bananas**, and don't forget to make it executable!

## 1. `info.yaml`

Create **./bananas/info.yaml**.

This is just a bit of metadata about how the Restyler works:

```yaml
---
name: bananas

# We'll build to a conventionally-named image and tag it with a made-up version
image: restyled/restyler-bananas:v0.0.1

# The command to run is our banana script
command:
- bananas

# It requires no arguments to make sure it works in-place
arguments: []

# We can (apparently) fix up any kind of file!
include:
- "**/*"

# We don't support "--" between arguments and paths
supports_arg_sep: false

# But we do support multiple paths in one invocation
supports_multiple_paths: true

# Don't worry about what these mean, just add them.
interpreters: []
documentation: []
metadata:
  languages: []
```

## 2. Docker Image

Create **./bananas/Dockerfile**:

```dockerfile
FROM alpine
RUN mkdir -p /code
WORKDIR /code
COPY files /
CMD ["bananas", "--help"]
```

And build your image, using our `make` target.

```console
make bananas/Dockerfile.built
```

## 3. Tests

Create **./test/fixtures/apples.txt**, as a file with "bad style":

```
Hi, here are some apples.
```

And **./test/bananas.t**, which runs your newly built image on your example file:

```cram
  $ source "$TESTDIR/helper.sh"
  If you don't see this, setup failed

bananas

  $ run_restyler bananas apples.txt
  TEMPORARY
```

Cram works by asserting on expected output. This test is currently asserting that there is the string `TEMPORARY` as the `git diff` output after restyling, which is clearly not right. But we can run it anyway, and have `cram` automatically update the file with an assertion of whatever output you do get:

```console
cram -i --shell=$(which bash) test/bananas.t
```

Assuming everything is working, you should see and be able to accept the following change that test:

```
  $ run_restyler bananas apples.txt
  diff --git i/apples.txt w/apples.txt
  index e4654af..65d084f 100644
  --- i/apples.txt
  +++ w/apples.txt
  @@ -1,1 +1,1 @@
  -Hi, here are some apples.
  +Hi, here are some bananas.
```

The above manually ran `cram` so we could use the `-i` option to automatically add the expectation. Now that that's done, run them the "right" way one last time to make sure they pass:

```console
make bananas/Dockerfile.tested
```

And that's it! Open a Pull Request and we'll take it from there.