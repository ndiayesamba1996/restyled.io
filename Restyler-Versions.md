Restyled wraps off-the-shelf auto-formatting tools as transparently as possible. Therefore, the version of the underlying tool has a large impact on the behavior you'll experience using our service. To help this, we offer a few configuration points in your `.restyled.yaml` that affect what versions of things will run on your PRs.

## How It Works

When the Restyler process runs, it fetches a manifest of all known Restylers, we call this a "Restylers set". These sets are themselves versioned and specified by the `restylers_version` key in your `.restyled.yaml`. 

As you are unlikely to be specifying this value yourself today, you can see what is being run by looking at [the defaults][default-restylers-version]. The value corresponds to a release (really a git ref) on the [`restyled-io/restylers`][restylers-releases] repository.

[default-restylers-version]: https://github.com/restyled-io/restyler/blob/master/config/default.yaml
[restylers-releases]: https://github.com/restyled-io/restylers/releases

In the set, each Restyler has [an `image` key][brittany-image] that points to the specific, tagged Docker image to run for that Restyler. These tags are named after the version of the underlying auto-formatter that image will run, so while we may add our own prefixes or suffixes, you should always be able to infer an auto-formatter version from these tags directly.

[brittany-image]: https://github.com/restyled-io/restylers/blob/628cd0cf7a8fd80fe1116c84ea7aceb64c6b904a/restylers.yaml#L32

We are happy to build and push images in all sorts of configurations, and we will (as much as possible) never remove a pushed tag. This means there may be many tags available not present in any versioned set. You can explore what is available for a specific Restyler by viewing [its Docker Hub page][brittany-tags] directly.

[brittany-tags]: https://hub.docker.com/r/restyled/restyler-brittany/tags

**TL;DR**:

- Use `restylers_version` to specify the overall set of Restyler versions
- Optionally, use `restylers[$name].image` to specify a specific Restyler version to use
- See [Available Restylers][available-restylers] for appropriate values

[available-restylers]: https://github.com/restyled-io/restyled.io/wiki/Available-Restylers

## Common Use Cases

The following are some common reasons to make use of these configuration points:

### Avoid surprises in general

We reserve the right to update Restylers fairly regularly and prefer to stay more up to date than to shoot for absolute behavior stability for a default configuration. If you're more conservative than that, you can re-specify the `restylers_version` as the current default in your own configuration:

```yaml
restylers_version: "20190627"
```

From then on, the Restylers set that is used won't change unless you change it.

**NOTE**: there are a few cases where the format of `restylers.yaml` needed to change in a breaking way. If you specify a `restylers_version` from before such a change, this will cause an error indicating you must update.

### Avoid specific surprises

In a specific Restyler, you can re-specify its default image:

```yaml
restylers:
  - brittany:
      image: restyled/restyler-brittany:v0.11.0.0
```

From then on, regardless of how the default Restylers set changes, this Restyler will always run this image, until you change it.

### Opt in to latest and greatest in general

Since the `restylers_version` is a git reference on the `restyled-io/restylers` repository, you can move faster than the default by setting it to any other reference:

```yaml
restylers_version: master
```

Note that the downloaded sets are cached on the filesystem of whatever Docker Machine you happen to have your Job allocated to, so referencing a mutable ref like `master` could mean seeing updated versions in a non-deterministic way.

### Opt in to specific latest and greatest

When/if we're making major updates, we will likely release a Restyler image before making it a part of any released set, and certainly before making it the default. You can opt in to trying it out ahead of time through its `image` key:

```yaml
restylers:
  - brittany:
      image: restyled/restyler-brittany:vX.Y.Z-beta
```

### Use your own image(s)

One consequence of this system is that whatever you set as a `restylers[$name].image` will be pulled and run as the Restyler. This means you can maintain and use your own images:

```yaml
restylers:
  - brittany:
      image: pbrisbin/restyler-brittany:latest
```

As long as the image is publicly available (and functions properly as a Restyler), this will work. This is a great way to test fixes yourself before submitting PRs.

**NOTE**: Restylers have direct access to the source code they're restyling. While they are run in an isolated working directory and with `--net=none` as safe guards (among other things), you should still fully trust the image you are specifying here.