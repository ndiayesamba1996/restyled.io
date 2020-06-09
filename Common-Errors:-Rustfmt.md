## `rustfmt may have failed to format`

Restyled can fail with this error:

```
error: left behind trailing whitespace
    --> /code/src/lib.rs:1058
     |
1058 |         // 
     |           ^

warning: rustfmt may have failed to format. See previous 1 errors.
```

This is an open `rustfmt` bug: https://github.com/rust-lang/rustfmt/issues/2916.

Once `rustfmt` has been fixed, Restyled can update. For now, unfortunately, you have to either edit the code in a way that doesn't cause this bug, or exclude it from Restyled via `.restyled.yaml`.

## ``expected one of `!`, `)`, `,`, `.`, `::`, `?`, `{`, or an operator, found keyword `move` ``

The `rustfmt` Restyler is built to run the 2015 "edition"*, and constructions only available in the 2018 edition may cause such syntax errors.

The fix is to specify your edition via `arguments`:

```yaml
restylers:
  - rustfmt:
      arguments:
        - "--edition"
        - "2018"
```

Or by creating a `rustfmt.toml` for the project:

```toml
edition = "2018"
```

**NOTE**: Unfortunately, specifying edition in `Cargo.toml` only effects invocations of `cargo fmt`, and not direct invocations of `rustfmt` (as the Restyler must do in order to only restyle changed files.

\*PRs welcome :)