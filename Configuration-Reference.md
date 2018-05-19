Restylers can be enabled, disabled, or configured by committing YAML configuration in a file named `.restyled.yaml` at the root of your repository.

The top-level YAML document must be either:

- A *Configuration* object

  ```yaml
  ---
  enabled: true
  restylers:
    - stylish-haskell
    - prettier
  ``

- Or just a list of *Restyler* objects

  ```yaml
  ---
  - stylish-haskell
  - prettier
  ```

  In this case, you are accepting the defaults for all other keys.

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