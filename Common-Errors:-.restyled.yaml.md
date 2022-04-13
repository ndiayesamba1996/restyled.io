The general syntax of a configuration error is:

```
We had trouble with your configuration:
Error in <location>: <message>.
```

## Location

The `location` string is a (perhaps cryptic) attempt at telling you where the error was encountered:

- `<location>[<index>]` means the element at index `<index>` of an array located at `<location>`
- `<location>[<name>]` means the `<name>` key of an object located at `<location>`
- Depending on the error, the above may also appear as `<location>.<name>`
- `$` means the top-level document

Working backwards, `$.restylers[0]` would mean the **0th element** of the **`restylers` key** in the **top-level document**. For example:

```yaml
                      # <-- top-level document
restylers:            # <-- restylers key
  - stylish_haskell:  # <-- 0th element
      include:
        - "**/*.hs"
  - brittany
```

And a similar error may use `$['restylers'][0]` for that location.

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

### Did you intend to specify a full Restyler object...

**TL;DR**: you may have incorrect indentation.

`restylers` values are expected to be a "named override":

```yaml
# Exhibit A
restylers:
  - prettier:           # <-- key: the name of a known Restyler
      include:          # <-- value: an object overriding some of its properties
        - "**/*.jsx"
```

But we also support fully specifying a `Restyler` directly:

```yaml
# Exhibit B
restylers:
  - name: prettier                       # <-- element: flat, complete Restyler object
    image: restyled/restyler-prettier
    command: [prettier]
    arguments: [--inplace]
    include: [...]
    # more keys...
```

Normally, this feature is only known to Restyled contributors who may use it to try out custom Restylers or non-default features not directly supported in the named-override syntax. The problem is it's **super easy** to accidentally do this:

```yaml
restylers:
  - prettier:
    include:
      - "**/*.jsx"
```

Notice how you *meant* to do _Exhibit A_, but this will parse like _Exhibit B_. The error that results could be confusing because it will talk about `prettier` being an invalid key for a `Restyler` (it is), when the real problem is that `include` needs to be shifted over two characters.

The solution is to make sure your indentation is like _Exhibit A_, assuming that's what you meant.

---

For more details see the [Configuration Reference](https://github.com/restyled-io/restyled.io/wiki/Configuring-Restyled).