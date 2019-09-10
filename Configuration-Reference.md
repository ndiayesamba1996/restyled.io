The restyling process can be configured by committing a YAML file named `.restyled.yaml` at the root of your repository.

The (well-commented) default configuration is available [here](https://github.com/restyled-io/restyler/blob/master/config/default.yaml).

## `Configuration`

The top-level YAML document must be either:

- A *Configuration* object

  ```yaml
  ---
  enabled: true
  auto: false
  remote_files: []
  pull_requests: true
  comments: false
  statuses: true
  request_review: none
  labels: []
  restylers:
    - stylish-haskell
    - prettier
    - ...
  restylers_version: "..."
  ```

- Or just a list of *Restyler* objects

  ```yaml
  ---
  - stylish-haskell
  - prettier
  - ...
  ```

  In this case, you are accepting the defaults for all other keys.

Valid keys in a *Configuration* object are:

- `enabled`: if `false`, Restyled will do nothing
- `auto`: if `true`, Restyled will not open a new Pull Request, it will commit the style fixes directly to your original Pull Request (does not apply to Forks)
- `remote_files`: any files to download into the project directory before restyling, see below
- `pull_requests`: whether to open Restyle PRs, disable if statuses are enough
- `comments`: whether to leave comments on Pull Requests linking to the Restyle PR
- `statuses`: whether to set Commit statuses, see below
- `request_review`: specify if and from whom to request review on the Restyle PRs
- `labels`: a list of labels to add to created Restyle PRs
- `restylers`: the list of *Restyler*s to run
- `restylers_version`: see [here](https://github.com/restyled-io/restyled.io/wiki/Restyler-Versions)

All keys are optional.

## `RemoteFile`

The `remote_files` key can be used to pull down centralized auto-formatter configuration (e.g. a `brittany.yaml`) from some remote source. That way, you don't have to commit the same versions of these files into all your repositories and keep them up to date. The only limitation here is that the files be available on the public internet.

```yaml
---
remote_files:
  - url: https://raw.githubusercontent.com/pbrisbin/dotfiles/master/config/brittany/config.yaml
    path: brittany.yaml
```

`RemoteFile` values require both a `url` and `path`.

## `RequestReviewConfig`

The `request_review` key can be either:

- An object

  ```yaml
  request_review:
    origin: none|author|owner  # default: author
    forked: none|author|owner  # default: owner
  ```

- Or a value, to indicate the same for `origin` or `forked`

  ```yaml
  request_review: none|author|owner  # default: none
  ```

The values, specify from whom to request review on the created Restyle PRs: the author or owner of the original PR (or none).

The default for (same-)origin PRs is `author`, because it's most likely that the author of the original PR, when forks are not involved, is the one who should see and possibly incorporate the style fixes. In the case of forked PRs, the author's original PR gets re-created with the original contribution along with style fixes. In this case, the `owner` should be reviewing and possibly merging that PR -- making them the better default reviewer.

**NOTE**: since both keys are optional, the simplest configuration `request_review: {}` can be used to enable the feature with default behavior for both cases.

## `StatusesConfig`

The `statuses` key can point to either:

- A simple boolean `true|false`

  ```yaml
  ---
  statuses: false
  ```

  In this case, you are disabling all statuses we send.

- Or an object

  ```yaml
  ---
  statuses:
    differences: false
    no_differences: true
    error: true
  ```

  In this case, you are disabling sending a status when we find differences.

  Omitted keys will default to `true`.

## `Restyler`

A *Restyler* can be either:

- A name:

  ```yaml
  ---
  - stylish-haskell
  ```

  In this case, you are accepting the defaults for this Restyler.

- Or a key into a configuration object for that Restyler:

  ```yaml
  ---
  - stylish-haskell:
      arguments:
        - --verbose
      include:
        - "**/*.lhs"
        - "!test/**/*"
  ```

Valid keys in a *Restyler* object are:

- `arguments`: The arguments to pass (in addition to the files to restyle)
- `include`: *Patterns* for targeting which files to restyle
- `interpreters`: If specified, also restyle files with these interpreters as their [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))

All keys are optional.

## `Pattern`

The `include` option accepts patterns similar to a `.gitignore`. You can use it to work around problematic files:

```yaml
---
- stylish-haskell:
    include:
      - "**/*.hs"
      - "!src/MyBadFile.hs"
      - "!docs/**/*"
```

It can be used to include-then-exclude, like above, but also to exclude-then-include:

```yaml
---
- stylish-haskell:
    include:
      - "**/*.hs"
      - "!test/**/*"
      - "test/specifics/**/*.hs"
```

**NOTE**: Order matters! Beware of sorting this list, such that you end up with something like

```yaml
- "!something.hs"
- "*.hs"
```

This configuration says to "exclude `something.hs` then include `*.hs`", which effectively cancels out the exclude.