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

### Changed paths

```yaml
changed_paths:
  maximum: 1000
  outcome: error
```

Pull Requests with too many changed paths will not be restyled. The limit is controlled by the `maximum` key and `outcome` can either be `error` or `skip`.

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

### Commit Template

```yaml
commit_template: |
  Restyled by ${restyler.name}
```

Control the commit messages used when Restyler makes fixes. Supports limited
interpolation, currently just `${restyler.name}`.

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
- `{any text}`: From this GitHub user or team name

One value will apply to both origin and forked PRs:

```yaml
request_review: author
```

Or you can specify separate values. If you specify only one, the other is defaulted as shown:

```yaml
request_review:
  origin: author
  forked: none
```

Therefore,

```yaml
request_review: {}
```

Is a method to opt into review-requests but with default choices about from who.

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


```yaml
restylers_version: stable
```

Version of the set of Restylers to run.

This name corresponds to a manifest at (e.g.)
https://docs.restyled.io/data-files/restylers/manifests/stable/restylers.yaml. Feel free
to specify `dev` to get new versions more quickly, but `stable` does not lag far behind.

### Restylers

```yaml
restylers:
  - "*"
```

Restylers to run, and how

Elements in this list can be specified in one of three forms:

1. A string, which means to run that Restyler with all defaults

   ```yaml
   restylers:
     - prettier
   ```

1. A single key, that is a name, and _override object_ as the value:

   ```yaml
   restylers:
     - prettier:
         include:
           - "**/*.js"
   ```

1. An object with a name key

   ```yaml
   restylers:
     - name: prettier
       include:
         - "**/*.js"
   ```

All three of the above are equivalent. The latter two are useful if you want
to run the same Restyler multiple ways:

```yaml
restylers:
  - name: prettier
    arguments: ["--one-thing"]
    include: ["needs-one-thing/**/*.js"]

  - name: prettier
    arguments: ["--another"]
    include: ["needs-another/**/*.js"]
```

Omitted keys inherit defaults for the Restyler of that name, which can be seen
in [Available Restylers](https://docs.restyled.io/available-restylers/).

Note that the `enabled` key is not inherited. Adding an item to this list, without specifying
`enabled: false`, automatically enables that Restyler.

#### Wildcard

The special value `*` (wildcard) means _all Restylers not configured_. One wildcard
may be placed anywhere in the `restylers` list and remaining Restylers will be run,
with their default values, at that point.

Note that the Restylers added by the `*` entry will not run if they're default configuration
includes `enabled: false`. You must explicitly add such Restylers for them to run.

Examples:

- Just run all Restylers with default values, i.e. the default configuration value

  ```yaml
  restylers:
    - "*"
  ```

- Enable `jdt`, and run all others after

  ```yaml
  restylers:
    - jdt
    - "*"
  ```

- Enable `jdt`, and run it after all others

  ```yaml
  restylers:
    - "*"
    - jdt
  ```

- Ensure `stylish-haskell` runs before `brittany`, and before all others

  ```yaml
  restylers:
    - stylish-haskell
    - brittany
    - "*"
  ```

- Run *only* `clang-format`

  ```yaml
  restylers:
    - clang-format
  ```

- Run `clang-format`, `astyle`, everything else, then `clang-format` again with
  different options

  ```yaml
  restylers:
    - clang-format
    - astyle
    - "*"
    - clang-format:
        arguments: ["--special"]
        include:
          - "special/**/*.cs"
  ```

- Disable the `astyle` Restyler, maintaining all other defaults

  ```yaml
  restylers:
    - astyle:
        enabled: false
    - "*"
  ```

#### Restyler Override

Valid keys in the _override object_ are:

- `enabled`: true|false

  Restylers present in the list are considered enabled and those not in the
  list are considered not enabled, however this key is an explicit way to
  disable a Restyler without removing it from the list (e.g. temporarily).

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
