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

This is an open bug `rustfmt` bug: https://github.com/rust-lang/rustfmt/issues/2916.

Once `rustfmt` has been fixed, Restyled can update. For now, unfortunately, you have to either edit the code in a way that doesn't cause this bug, or exclude it from Restyled via `.restyled.yaml`.