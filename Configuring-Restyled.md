The restyling process can be configured by committing a YAML file named `.restyled.yaml` at the root of your repository. The contents of that file are documented here. The current default configuration is available [here](https://github.com/restyled-io/restyler/blob/master/config/default.yaml). Any differences or additional notes in that source file take precedence over what's described in this wiki page.

The `.restyled.yaml` **in the branch being Restyled** is what is used. If you make a configuration change on another branch (e.g. `master`), you will need to bring that change into any open Pull Requests (e.g. rebase) before Restyled will see it there.

## General notes

- All keys are optional and default values begin each section below
- All list values can also be given a single value, to indicate a single-element list.

## Core Configuration

### Enabled

```yaml
enabled: true
```

Do anything at all?

### Exclude

```yaml
exclude:
  - "**/*.patch"
  - "**/node_modules/**/*"
  - "**/vendor/**/*"
  - ".github/workflows/**/*"
```

Patterns to exclude from all Restylers.

By default, we ignore directories that are often checked-in but rarely
represent project code. Some globs are slightly complicated to match paths
within directories of names appearing at any depth.

This behavior can be disabled in your project with:

```yaml
exclude: []
```

### Remote files

```yaml
remote_files: []
```

Files to download before restyling.

Example:

```yaml
remote_files:
  - url: https://raw.github.com/.../hlint.yaml
    path: .hlint.yaml
```

If omitted, `path` is the basename of `url`.

## Restyling Outcomes

### Auto

```yaml
auto: false
```

Push the style fixes directly to the original PR?

This setting implies `pull_requests: false` for origin PRs, and has no effect on
forked PRs (since we can't push to those).

:hocho: This feature has seen low adoption and is slated for deprecation. If you make
use of this functionality, please [let us know](mailto:support@restyled.io).

### Pull Requests

```yaml
pull_requests: true
```

Open Restyle PRs?

### Comments

```yaml
comments: false
```

Leave comments on the original PR linking to the Restyle PR?


:hocho: This feature has seen low adoption and is slated for deprecation. If you make
use of this functionality, please [let us know](mailto:support@restyled.io).

### Pull Request Statuses

```yaml
statuses:
  differences: true     # Red when style differences are found
  no_differences: true  # Green when no differences are found
  error: true           # Red if we encounter errors restyling
```

A single value can be used to disable/enable all:

```yaml
statuses: false
```

Or separate values as shown in the defaults.

### Review requests

```yaml
request_review: none
```

Possible values:

- `author`: From the author of the original PR
- `owner`: From the owner of the base repository
- `none`: Don't

One value will apply to both origin and forked PRs:

```yaml
request_review: author
```

Or you can also specify separate values:

```yaml
request_review:
  origin: author
  forked: owner
```

:bug: Be aware of [Issue 188](https://github.com/restyled-io/restyled.io/issues/188).

### Labels

```yaml
labels: []
```

Labels to add to any created Restyle PRs.

These can be used to tell other automation to avoid our PRs.

Example:

```yaml
labels:
  - pullassigner-ignore
```

:warning: Unfortunately, PRs must be created and labels added in distinct
steps. So, some automation may still process our PRs if they are using
the PR details from before the labels were added.

### Ignore labels

```yaml
ignore_labels:
  - restyled-ignore
```

Labels to ignore.

PRs with any of these labels will be ignored by Restyled.

## Restylers

### Restylers version

:warning: default value changes with every deploy. See project source for current default.

```yaml
restylers_version: "..."
```

Version of the set of Restylers to run.

This should correspond to a ref on the restyled-io/restylers repository,
usually it's a [tag that is a date of when that set was released][refs]. You could
re-specify the default in your own config if you prefer to avoid update
surprises.

[refs]: https://github.com/restyled-io/restylers/releases

### Overrides

```yaml
overrides: null
```

Modify how any Restyler runs.

For example,

- Don't run at all
- Run a different Docker Image
- Use a different `command` or `arguments`
- etc

Anything not overridden uses the values defined for your `restylers_version`. Currently
released values can be seen in [Available Restylers](https://github.com/restyled-io/restyled.io/wiki/Available-Restylers).

Overrides can be specified in three forms:

1. A string, which means to run that Restyler with all its defaults

   ```yaml
   overrides:
     - prettier
   ```

1. A single key, that is a name, and the rest of the override object as the value:

   ```yaml
   overrides:
     - prettier:
         include:
           - "**/*.js"
   ```

1. A full override object with a name key

   ```yaml
   override:
     - name: prettier:
       include:
         - "**/*.js"
   ```

Under any of these constructions, the `name` value is matched as a _glob_. And the
**first** override to match is used. For example, configuring Restyled to run
**only** the `astyle` Restyler would look like this:

```yaml
overrides:
  - astyle  # don't change anything, just ensure we match
  - "*":    # for all others, disable
      enabled: false
```

#### Restyler Override

Valid keys in the _override object_ are:

- `enabled`: true|false

  Run the Restyler or not.

- `arguments`: string or array of string

  Any additional argument(s) to pass to the restyling command.

- `include`: pattern or array of pattern

  Pattern(s) to match files that should be Restyled.

  **NOTE**: these are processed in order, so be careful you don't accidentally do
  something like:

  ```yaml
  - "!/bad-file.hs"
  - "**/*.hs"
  ```

  which says to exclude `bad-file.hs`, then immediately re-includes it via the
  wildcard.

- `interpreters`: interpreter or array of interpreters

  Extension-less files will be Restyled if they match interpreter(s) given
  here. Valid values are `sh`, `bash`, `python`, and `ruby`.

Less-commonly needed:

- `image`: string

  The Docker image to run. Can be anything publicly pull-able.

- `command`: string or array of string

  The command (and any required argument(s)) to perform the Restyling.