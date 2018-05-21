The restyling process can be configured by committing a YAML file named `.restyled.yaml` at the root of your repository.

## `Configuration`

The top-level YAML document must be either:

- A *Configuration* object

  ```yaml
  ---
  enabled: true
  auto: false
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
- `restylers`: The list of *Restyler*s to run

All keys are optional.

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