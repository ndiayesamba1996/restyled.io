The general syntax of a configuration error is:

```
We had trouble with your .restyled.yaml:
Error in <location>: <message>.
```

## Location

The `location` string is a (perhaps cryptic) attempt at telling you where the error was encountered.

Working backwards,

- `<location>[<index>]` means the element at index `<index>` of an array located at `<location>`
- `<location>.<name>` means the `<name>` key of an object located at `<location>`
- `$` means the top-level document

So `$.restylers[0]` would mean the **0th element** of the **`restylers` key** in the **top-level document**. For example:

```yaml
                      # <-- top-level document
restylers:            # <-- restylers key
  - stylish_haskell:  # <-- 0th element
      include:
        - "**/*.hs"
  - brittany
```

## Messages

The following is a non-exhaustive list of error message you may see, as well as potential causes and solutions.

### Unknown restyler name

```
Error in $.restylers[0]: Unknown restyler name: stylish-haskell include: - "**/*.hs".
```

In this case, I had forgotten the `:` after the restyler name.

```yaml
restylers:
  - stylish-haskell
      include:
        - "**/*.hs"
  - brittany
```

Without the colon, `stylish-haskell` is not used as a key into the configuration object. Instead, the entire value is a single String. So we were seeing the value at `$.restylers[0]` as `stylish-haskell include: "**/*.hs"`, which is not a valid Restyler name.

The fix was easy:

```yaml
restylers:
  - stylish-haskell:
      include:
        - "**/*.hs"
  - brittany
```

(Notice `brittany` is still lacking the `:`. This is valid because in that case we do mean to specify just the name of that restyler, not a key into a configuration object.)

### expected Name with override object, encountered Object

```
Error in $: expected Name with override object, encountered Object
```

This usually means you didn't indent something far enough. For example:

```yaml
- brittany:
  arguments:
    - --inplace
    - --verbose
```

This defines an `Object` with the keys `brittany` and `arguments`, but that's not valid. What we need is a `Name` (`brittany`), that points to an override `Object` (`{ arguments: ... }`).

In other words, it's the difference between `{ brittany: ..., arguments: ... }` and `{ brittany: { arguments: ... } }`.

The fix is to indent the override object another level, so `brittany` becomes the key to it.

```yaml
- brittany:
    arguments:
      - --inplace
      - --verbose
```

---

For more details see the [Configuration Reference](https://github.com/restyled-io/restyled.io/wiki/Configuration-Reference#restyler).