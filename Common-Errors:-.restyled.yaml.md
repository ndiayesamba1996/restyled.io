```
We had trouble with your .restyled.yaml:
Error in <location>: <message>.
```

Example:

```
We had trouble with your .restyled.yaml:
Error in $.restylers[0]: Unknown restyler name: stylish-haskell include: - "**/*.hs".
```

## Location

The `location` string is a (perhaps cryptic) attempt at telling you where the error was encountered.

Working backwards,

- `<location>[<index>]` means the element at index `<index>` of an array located at `<location>`
- `<location>.<name>` means the `<name>` key of an object located at `<location>`
- `$` means the top-level document

So `$.restylers[0]` means the 0th element of the `restylers` key in the top-level document

```yaml
                     # <-- top-level document
restylers:           # <-- restylers key
  - stylish_haskell  # <-- 0th element
      include:
        - "**/*.hs"
  - brittany
```

## Message

```
Unknown restyler name: <what we saw>.
```

In this case, I had forgotten the `:` after the restyler name.

```yaml
---
restylers:
  - stylish-haskell
      include:
        - "**/*.hs"
  - brittany
```

Without the colon, `stylish-haskell` is not used as a key into the configuration object. Instead, the entire value is a single String. So we were seeing the value at `$.restylers[0]` as `stylish-haskell include: "**/*.hs"`, which is not a valid Restyler name.

The fix was easy:

```yaml
---
restylers:
  - stylish-haskell:
      include:
        - "**/*.hs"
  - brittany
```

(Notice `brittany` is still lacking the `:`. This is valid because in that case we do mean to specify just the name of that restyler, not a key into a configuration object.)

For more details see the [Configuration Reference](https://github.com/restyled-io/restyled.io/wiki/Configuration-Reference#restyler).