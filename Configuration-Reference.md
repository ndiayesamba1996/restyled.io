The restyling process can be configured by committing a YAML file named `.restyled.yaml` at the root of your repository.

## `Configuration`

The top-level YAML document must be either:

- A *Configuration* object

  ```yaml
  ---
  enabled: true
  auto: false
  statuses: true
  restylers:
    - stylish-haskell
    - prettier
  ```

- Or just a list of *Restyler* objects

  ```yaml
  ---
  - stylish-haskell
  - prettier
  ```

  In this case, you are accepting the defaults for all other keys.

Valid keys in a *Configuration* object are:

- `enabled`: If `false`, Restyled will do nothing
- `auto`: If `true`, Restyled will not open a new Pull Request, it will commit the style fixes directly to your original Pull Request (does not apply to Forks)
- `statuses`: whether to send Pull Request statuses in addition leaving comments, see the next section
- `restylers`: The list of *Restyler*s to run

All keys are optional.

## `StatusesConfig`

The `statuses` key can point to either:

- A simple boolean `true|false`

  ```yaml
  ---
  statuses: false
  ```

  In this case, you are disabling all statuses we send.

- An object

  ```yaml
  ---
  statuses:
    differences: false
    no-differences: true
    error: true
  ```

  In this case, you are disabling sending a status when we find differences.

  Omitted keys will default `true`.

## `Restyler`

A *Restyler* can be either:

- A name:

  ```yaml
  ---
  - stylish-haskell
  ```

  In this case, you are accepting the defaults for this restyler.

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