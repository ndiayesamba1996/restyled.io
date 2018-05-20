Restylers can be added by anyone through the usual Pull Request process.

There is no burden of popularity or usefulness. Most Restylers can even be configured to run by default, provided they don't conflict with other Restylers that operate on the same file-types.

## The Restyler Docker Image

**TL;DR**: Open a Pull Request in [`restyled-io/restylers`](https://github.com/restyled-io/restylers) to add `./{name}/Dockerfile`.

Restylers are implemented as Docker images. The Docker image should not implement the restyling process, it only wraps an existing auto-formatting tool into a consistent interface.

A typical `Dockerfile` will:

- Download, compile, or otherwise install the auto-formatting tool (tool-dependent)
- Make the `/code` directory and set it as `WORKDIR` (required)
- Set up a `--help` invocation as default `CMD` (nice-to-have)

One simple example is [elm-format](https://github.com/restyled-io/restylers/blob/master/elm-format/Dockerfile).

Usage for invoking the Docker image must be `[command] [arguments] [--] [path, ...]` and should restyle the (repository-relative) `path`s **in place**. Most tools (such as elm-format) work this way already, so nothing else needed. Some tools might require an argument (e.g. `--in-place`), and you will get a chance to declare that fact in a later step. If the tool does not support this interface (e.g. it can't do in-place editing, or handle multiple `path`s at once), you should write a wrapper script. See [hindent](https://github.com/restyled-io/restylers/tree/master/hindent) as an example.

Support for `--` is not strictly required, but is strongly recommended. If the tool does not support this (and you don't add support for it via a wrapper script), your restyler may fail on repositories with file-names that begin with `-`.

## "Installing" in `restyler`

**TL;DR**: open a Pull Request in [`restyled-io/restyler`](https://github.com/restyled-io/restyler) configuring your Restyler in `allRestylers`, adjusting `defaultConfig` (if desired), and adding a test-case.

Available restylers are added to the [`allRestylers`](https://github.com/restyled-io/restyler/blob/85eb1c50ed6f8fa25c20bcd21f7318fd9494fc7f/src/Restyler/Config.hs#L64) function. Here, you define:

- The `command`
- Any `arguments` required
- The `include` patterns this Restyler should run on
- Any `interpreters` this Restyler should run on
- If the tool supports the `--` argument-path separator

**NOTE**: through their `.restyled.yaml`, users can override any of these values except `command` and arg-separator support.

### Testing

Testing is accomplished by running Restylers on a "fixture" of bad style, and asserting the expected `git diff` afterward. You should add an [example file](https://github.com/restyled-io/restyler/tree/master/test/core/fixtures) with bad styling and a [test case](https://github.com/restyled-io/restyler/blob/85eb1c50ed6f8fa25c20bcd21f7318fd9494fc7f/test/core/main.t#L246) that exercises your Restyler.

Assuming you've built the `restyler-core` executable, tests can be run through `make test.core`. This process will `docker run restyled/restyler-{name}`, which will auto-`pull` the `:latest` images from Docker Hub if they don't already exist locally. If the *do* exist locally, those images will be run. In this way, you can iterate on your local Docker image through this test-suite.

### Versioning

We are not doing any complex versioning of Restyler images themselves. The restyling process will always run the `:latest` image versions from Docker Hub. When building your Docker image, it's suggested (though no required) that you install a specific version of the auto-formatting tool. This will make it explicit to update through another Pull Request.

Exact current versions can always be found by a command like this:

```console
docker run --rm restyled/restyler-rubocop --version
```

## Publish

Once I accept this Pull Request (and master auto-deploys), your Restyler will be available. The last step is to update [this wiki](https://github.com/restyled-io/restyled.io/wiki/Available-Restylers).